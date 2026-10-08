# 8. 隐藏在 Symbol 背后

## 简介

JavaScript 对象中的属性命名存在一个主要障碍，尤其是在使用第三方库或框架时，那就是属性名冲突。我们不得不接受这样的限制：在变量和属性的命名方式上会受到约束。诸如“不要使用 `$` 前缀作为变量、属性和方法的名称。此前缀为 `<插入框架/库名称>` 所保留”这样的警告屡见不鲜，而我们也只能坦然接受。

好了，这种情况不会再发生了！

在本章中，我们将看到 ES6 引入的一种新数据类型——`Symbol`，如何帮助我们消除这一障碍，摆脱这种约束，让我们能够随心所欲地设计对象，并且知道在任何我们想要使用的环境中，都不会受到任何限制。我们还将看到如何使用 `Symbol` 来修改内置操作（如 `String.match` 和 `String.split`）的行为。在本章结束时，我们将从强制命名约定的专制中解放出来，与所有库和框架，更不用说 JavaScript 本身，和谐共处，如同一个快乐的大家庭。

## 无法强制执行契约

假设我们在代码中使用了某个第三方库，它暴露了一个函数，要求你提供的对象必须包含一个`特定`名称的属性。来看一个简单的例子：

* ① 方便的 `const` 让事情更简单
* ② 查找该实现期望在传入对象上找到的属性
* ③ 在简单对象上添加一个属性，以便 `simpleLogger` 能使用它
* ④ 另一个恰好拥有 `log` 属性的对象
* ⑤ 现在我们该怎么办？

```javascript
const LOG_LEVEL = 'log'; ①
const DEBUG = 'DEBUG';
const INFO = 'INFO';
const ERROR = 'ERROR';
const simpleLogger = (obj, msg) => {
  let returnMsg;
  switch (obj[LOG_LEVEL]) { ②
    case DEBUG:
      returnMsg = `DEBUG: ${msg}`;
      break;
    case INFO:
      returnMsg = `INFO: ${msg}`;
      break;
    case ERROR:
      returnMsg = `ERROR: ${msg}`;
      break;
    default:
      throw new Error('无效的日志级别');
  }
  console.log(returnMsg);
  return returnMsg;
};
const loggableObj = {
  [LOG_LEVEL]: INFO, ③
};
simpleLogger(loggableObj, 'this is a test');
const someOtherObject = {
  log: '这只是一个普通属性', ④
};
// simpleLogger(someOtherObject, 'some message') ⑤
```

在这个例子中，这个库函数期望传入的任何对象都必须有一个名为 `log` 的属性。库很贴心地提供了一个 `const`，我们可以结合增强的字面量语法，将合适的属性添加到我们的对象上。在内部，`simpleLogger` 函数会检查 `log` 属性以确定日志级别，然后打印出相应的消息。

但是，如果我们自己的对象已经有了一个名为 `log` 的属性，而它用于完全不同的目的，该怎么办？也就是说，我们对 `log` 属性的预期用途与函数期望的使用方式发生了冲突。于是，我们只能选择要么替换掉自己的属性来满足库的需求，要么就不用这个库。这两种选择似乎都不太理想。

这里的问题在于，虽然我们有 `const`，可以安全地引用一个“名称”，从而总能得到正确的`值`，但字符串以及由此衍生的对象键（无论是属性还是方法）`确实可能`发生冲突！这就是为什么很多在其它对象上引入和/或期望“特殊”属性的库会采用命名惯例——比如给注入的属性加上 `_` 或 `$` 前缀，并规定使用该库的开发者`不`应在自己的代码中使用相同的前缀。看看 AngularJS 风格指南，它明确指出了以下几点：

> *不要对变量、属性和方法的名称使用 $ 前缀。此前缀保留给 AngularJS 使用。*

这里的两难困境在于，为了满足我们的需求，我们需要一种方法来定义一个`不可能`发生冲突的名称，换句话说，就是一个真正独一无二的名称，它不会、也不能以任何方式与任何现有的名称或属性发生冲突。此外，我们希望做到这一点，而无需强制推行编码策略，或者更糟的是，使用某种晦涩的命名策略（比如使用 UUID）来避免冲突！

## Symbol 来救场

`Symbol` 是 ES6 中引入的一种`新`的原始类型。与其他原始类型类似，`Symbol` 有一个包装类型，名为 `Symbol`，它提供了一个工厂函数来初始化新的 Symbol。与 `String` 等其他包装对象不同，我们`不能`使用 `new` 来创建 Symbol——也就是说，使用 `new Symbol()` 会导致错误。让我们看一段代码片段，了解如何初始化 Symbol：

* ① 使用工厂函数创建一个新的 Symbol
* ② 两个 Symbol 永远不会相等
* ③ 出于调试目的，可以为 Symbol 提供一个描述
* ④ Symbol 有一个 `toString` 实现，如果提供了描述，它会显示该描述

```javascript
const symbol1 = Symbol();
const symbol2 = Symbol(); ①
console.assert(symbol1 !== symbol2); ②
const symbol3 = Symbol('unique'); ③
// 打印 Symbol(unique)
console.log(symbol3); ④
```

`Symbol` 初始化器可以接受一个描述；然而，这个描述对创建的 Symbol 本身没有影响，只对调试有用。

两个 Symbol `永远`不会相等——每一次调用 `Symbol` 初始化器都会返回一个新的、唯一的 Symbol 实例。即使提供的描述相同也是如此（因为正如我们讨论过的，提供给初始化函数的字符串仅仅是为了调试）。

所以，现在我们有了创建唯一“事物”的方法。这如何帮助我们解决命名冲突呢？

原来，我们可以将 Symbol 用作对象中的键！让我们先看看如何使用 Symbol 和字符串在对象上定义属性：

* ① 定义一个 Symbol
* ② 一个只有 `name` 属性的普通 JavaScript 对象
* ③ 无冲突地添加上 `iterable` 属性
* ④ 这个对象已经有一个名为 `iterator` 的属性
* ⑤ 我们仍然可以无冲突地添加一个描述为 `iterator` 的 Symbol

```javascript
const ITERATOR = Symbol('iterator'); ①
const iterableObject = {
  name: 'iterable',
}; ②
iterableObject[ITERATOR] = function() {
  return '我是可迭代的';
}; ③
const objWithExistingKey = {
  iterator: '一些值',
}; ④
objWithExistingKey[ITERATOR] = () => '成功了！'; ⑤
```

注意，我们首先创建了一个 Symbol——然后我们使用那个`引用`作为对象属性的名称，以及用于查找。如示例所示，我们可以使用下标语法在现有对象上添加新属性，就像我们对任何表达式所做的那样。最后，一个对象可以同时拥有字符串和 Symbol 作为其属性，即使属性的名称与 Symbol 的描述相匹配。

利用我们新学的知识，让我们重新审视 `simpleLogger`，看看如何使用 Symbol 将行为“洒”到对象上，而在此之前我们只能屈从于命名冲突的后果。

* ① 将 `LOG_LEVEL` 定义为一个 Symbol
* ② 在 switch 中查找所需的属性
* ③ 使用 Symbol 引用引入一个属性
* ④ 现在它能工作了！

```javascript
const LOG_LEVEL = Symbol('log'); ①
const DEBUG = 'DEBUG';
const INFO = 'INFO';
const ERROR = 'ERROR';
const simpleLogger = (obj, msg) => {
  let returnMsg;
  switch (obj[LOG_LEVEL]) { ②
    case DEBUG:
      returnMsg = `DEBUG: ${msg}`;
      break;
    case INFO:
      returnMsg = `INFO: ${msg}`;
      break;
    case ERROR:
      returnMsg = `ERROR: ${msg}`;
      break;
    default:
      throw new Error('无效的日志级别');
  }
  console.log(returnMsg);
  return returnMsg;
};
const loggableObj = {
  log: '这只是一个普通属性',
  [LOG_LEVEL]: INFO, ③
};
simpleLogger(loggableObj, 'this is a test'); ④
```

这个例子强调了如何利用 `Symbol` 在新对象或现有对象上引入无冲突的属性。

使用 `Symbol` 来定义“接口”或“契约”属性，使得库和函数的编写者无需强制规定编码规范，同时允许使用这些库的客户端按照自己的意愿设计对象，并且能够使用这些库而无需担心冲突。

双赢！

## 符号（Symbol）与 Switch-case 语句

我们可以考虑另一种使用场景，其中使用唯一值（符号）有助于减少潜在的代码错误。我们经常使用字符串来标识“唯一性”，而 `switch-case` 等语言结构则利用这一点来辅助程序流程。请看以下示例：

- ① 定义辅助常量
- ② 对字符串属性执行 `switch case` 判断
- ③ 引入一个误导性常量
- ④ 将该误导性常量用作新对象中的日志级别
- ⑤ 输出结果与预期不符

```javascript
const LOG_LEVEL = Symbol('log');
const DEBUG = 'DEBUG'; // ①
const INFO = 'INFO';
const ERROR = 'ERROR';
const simpleLogger = (obj, msg) => {
  let returnMsg;
  switch (obj[LOG_LEVEL]) { // ②
    case DEBUG:
      returnMsg = `DEBUG: ${msg}`;
      break;
    case INFO:
      returnMsg = `INFO: ${msg}`;
      break;
    case ERROR:
      returnMsg = `ERROR: ${msg}`;
      break;
    default:
      throw new Error('Invalid LOG_LEVEL');
  }
  console.log(returnMsg);
  return returnMsg;
};
const MY_LEVEL = 'ERROR'; // ③
const loggableObj = {
  log: 'this is just an ordinary property',
  [LOG_LEVEL]: MY_LEVEL, // ④
};
// 输出 'ERROR: this is a test'
simpleLogger(loggableObj, 'this is a test'); // ⑤
```

只有敏锐的读者才能意识到这里发生了什么。我们引入了一个误导性常量，即值为 `"ERROR"` 的 `MY_LEVEL`，并将其作为 `LOG_LEVEL` 赋值给一个对象。这样做之所以有效，是因为在 `simpleLogger` 函数内部，`switch-case` 语句判断的是`相等性`，由于传入的 `LOG_LEVEL` 恰好是我们已知的一个值，因此匹配到了 `ERROR` 分支。然而，我们没有抛出错误，反而成功记录了一条错误信息。这里的问题在于，虽然字符串`确实是`值，但两个独立构造的字符串如果碰巧拥有相同大小写的字符集，它们就可以是相等的！

理想情况下，我们希望能够创建真正唯一的、可供我们进行切换判断的值。实际上，JavaScript 中的 `switch` 语句仅仅是使用 `===` 相等运算符来查找匹配的 `case` 分支！再次，符号（Symbol）前来救场。

- ① 将辅助常量定义为符号而非字符串
- ② 注意 `switch case` 语句**没有**变化
- ③ 其工作方式与使用字符串时完全相同

```javascript
const LOG_LEVEL = Symbol('log');
const DEBUG = Symbol('DEBUG'); // ①
const INFO = Symbol('INFO');
const ERROR = Symbol('ERROR');
const simpleLogger = (obj, msg) => {
  let returnMsg;
  switch (obj[LOG_LEVEL]) { // ②
    case DEBUG:
      returnMsg = `DEBUG: ${msg}`;
      break;
    case INFO:
      returnMsg = `INFO: ${msg}`;
      break;
    case ERROR:
      returnMsg = `ERROR: ${msg}`;
      break;
    default:
      throw new Error('Invalid LOG_LEVEL');
  }
  console.log(returnMsg);
  return returnMsg;
};
const loggableObj = {
  log: 'this is just an ordinary property',
  [LOG_LEVEL]: ERROR,
};
// 输出 'ERROR: this is a test'
simpleLogger(loggableObj, 'this is a test'); // ③
```

回想一下，符号是唯一的，每次调用初始化函数 `Symbol` 都会创建一个新的、唯一的符号对象，它不等于任何其他符号（即使是那些拥有相同描述字符串的符号）。在我们最新的迭代中，我们使用符号作为键来唯一标识 `LOG_LEVEL`，同时也用作其值，现在我们就能保证其唯一性了。

## 全局注册表

关于 `Symbol`，还有另一个方面我们需要讨论，那就是全局符号注册表。我们可以将符号注册表视为一个符号池——你可以在池中创建并放入一个符号，并在之后检索它。

要从池中创建或获取一个符号，我们使用 `Symbol.for`。`Symbol.for` 需要提供一个“键”，如果注册表中**不**存在具有该键的符号，则会创建一个新符号。否则，将从注册表中检索具有相同键的符号，具体如下：

- ① 使用 `for` 定义一个符号——在这种情况下，会创建一个新符号
- ② 使用相同的“键”查找一个符号——这只是一个“获取”操作
- ③ 它们是相等的

```javascript
const global = Symbol.for('globally visible'); // ①
const otherGlobal = Symbol.for('globally visible'); // ②
// 输出 'true'
console.log(global === otherGlobal); // ③
```

事实证明，对于全局注册表中的符号，`正是`这个键来标识符号——如果两个引用都使用相同的键通过 `for` 查找符号，它们将指向同一个符号。

我们也可以反向操作——给定一个符号，我们可以使用 `Symbol` 上的 `keyFor` 方法获取其键。

- ① 使用 `for` 定义一个符号——在这种情况下，会创建一个新符号
- ② 查找特定符号的键

```javascript
const global = Symbol.for('a unique key'); // ①
const key = Symbol.keyFor(global); // ②
// 输出 'true'
console.log(key === 'a unique key');
```

你可能会问——这有什么意义？如果我们想要跨“领域”共享符号（符号是引用类型，不同于字符串、数字和布尔值这些原始类型），那么这个池就是必需的。^(⁵¹) 领域是一个全局上下文，例如存在于 iframe 中的上下文。因此，在网页中运行的 JavaScript 代码与在嵌入的 iframe 中运行的代码，或在 Service Worker 中运行的代码，处于不同的领域。如果我们想要将符号用作跨领域的全局标识符或属性名，那么全局符号注册表就是你的得力助手。

当然，要标识全局注册表中的符号，我们需要一个键。总有可能出现两段不同的代码（甚至运行在不同领域中的代码）使用相同的键（出于不同目的）来创建符号。因此，如果我们打算使用全局注册表，就有必要适当地对这些标识符进行“命名空间”化处理。

## 众所周知的符号

JavaScript 提供了一组有用的符号，作为“静态”属性附加在 `Symbol` 类型上。这些“众所周知的符号”的主要目的是允许我们开发者自定义某些算法如何与我们的对象协作。

让我们退一步，重新审视一下我们之前学到的知识。第一，符号允许我们在对象上引入无冲突的属性；第二，库函数的作者现在可以自由地组合他们定义的一系列符号，并且`要求`我们，如果希望使用该库，就在我们的对象中使用这些符号。

那么，通过众所周知的符号，JavaScript 这门语言`本身`就是一个库！

让我们通过一个具体例子来阐明这一点。JavaScript 字符串有一个叫 `split` 的方法，它接收一个分隔符（以及一个可选的限制参数），会在分隔符处将字符串拆分，并返回一个由各个子字符串组成的数组。

- ① 定义一个包含用户名和密码编码的 URL
- ② 使用正则表达式提取用户名和密码组合

```javascript
const url = 'https://admin:supersecret@gitlab.com/looselytyped/sample-repo.git'; ①
const split = url.match(/\w+:\w+/); ②
// 输出
// [
//   'admin:supersecret',
//   index: 8,
//   input: 'https://admin:supersecret@gitlab.com/looselytyped/sample-repo.git',
//   groups: undefined
// ]
console.log(split);
```

我们使用一个简单的正则表达式来提取 URL 字符串中编码的用户名和密码。但问题是，密码是以明文形式显示的！如果我们想在将每个匹配项放入数组之前更改或增强它们，该怎么办？

显而易见的解决方案是，在使用正则表达式匹配到密码之后，再对其进行混淆处理。然而，这将提取用户名和密码的操作与密码混淆的操作分离开了。

或许我们可以在 `match` 阶段同时完成密码混淆。事实证明，`String.match` 方法可以接收一个对象作为参数（而不是正则表达式），并且如果传入的对象实现了一个方法，其名称计算结果为 `Symbol.match`，那么 `String.match` 就会简单地将调用转发给那个方法。请看示例：

- ① 定义一个自定义对象
- ② 使用 `Symbol.match` 在该对象上定义一个方法
- ③ 从匹配结果中提取用户名
- ④ 返回一个新数组，包含用户名并混淆密码
- ⑤ 将自定义匹配器提供给 `match` 方法

```javascript
const passwordObfuscator = { ①
  regex: /\w+:\w+/,
  Symbol.match { ②
    const creds = str.match(this.regex);
    if (creds) {
      const [username] = creds[0].split(':'); ③
      return [
        `${username}:***********`, ④
      ];
    }
    return creds;
  },
};
const url = 'https://admin:supersecret@gitlab.com/looselytyped/sample-repo.git';
const credentials = url.match(passwordObfuscator); ⑤
// 输出 'obfuscated credentials [ 'admin:***********' ]'
console.log('obfuscated credentials', credentials);
```

我们定义了一个对象，并使用增强的对象字面量语法，添加了一个名称计算结果为 `Symbol.match` 的方法。通过实现 `Symbol.match`，我们的对象遵守了 `String.match` 期望的契约。现在 `match` 可以将匹配操作委托给我们的对象——它不再自己执行匹配，而是简单地调用 `passwordObfuscator[Symbol.match]`。在我们的实现内部，我们首先从字符串中提取用户名（碰巧又使用了 `match`，但这次提供给它的是一个正则表达式），混淆密码，然后返回结果。

诚然，这个例子远未达到生产级标准，但它确实有助于说明我们如何利用这些众所周知的符号来修改 JavaScript 中现有 API 的行为。

`Symbol` 对象还有许多其他静态成员，用于改变 JavaScript 处理我们对象的方式。比如 `Symbol.iterator`，我们很快会在讨论生成器和迭代器时看到它。还有 `Symbol.primitive`，它允许我们定义 JavaScript 如何将一个对象强制转换为原始值。你可以在 MDN Web 文档的 Symbol 参考[页面](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Symbol)上找到所有这些符号的列表。

## 并非私有

如果我们取出任何一个对象，并使用 `Object.keys`（列出对象所有可枚举的键）或 `Object.getOwnPropertyNames`（列出所有键，包括不可枚举的）来遍历其成员，我们会发现使用符号定义的成员不会被列出。让我们通过一个简单的演示来测试这一点：

- ① 定义一个带有字符串键的对象
- ② 使用符号定义另一个键
- ③ 定义第三个不可枚举的键
- ④ 检查键

```javascript
const obj = {
  a: 'a simple property', ①
  [Symbol()]: 'a symbol property', ②
};
Object.defineProperty(obj, 'nonenumerable', { ③
  value: 'an unenumerable property',
  enumerable: false,
});
// 只列出 'a'
console.log(Object.keys(obj));
// 列出 'a' 和 'nonenumerable'
console.log(Object.getOwnPropertyNames(obj));
// 也只列出 'a'
console.log(JSON.stringify(obj)); ④
```

我们定义了一个具有多个键的对象——一个传统的键值对，另一个碰巧是符号，最后第三个是使用 `Object.defineProperty` 并将 `enumerable` 标志设置为 false 来定义的。运行这个例子表明，我们无法在对象上看到符号属性。

虽然这似乎暗示符号键是私有的，并且就我们使用传统的自省机制（如 `JSON.stringify`）而言，它们确实是“私有的”。

然而，这并不意味着它们就是私有的，而且允许对象拥有私有键也不是符号属性的目的。ES6 引入了另一个 API，即 `Object.getOwnPropertySymbols`，它可以列出对象上存在的所有符号属性。继续，在我们的例子上尝试一下，你会发现它确实列出了那一个符号属性。

总之，通过组合使用 `Object` 上的方法，我们可以梳理出任何对象上存在的*所有*属性。

# 总结

在本章中，我们深入探讨了一种新的原始类型，即符号。我们研究了使用传统键的一些限制，即存在冲突的风险，以及如何使用符号来定义真正的唯一标识符。我们还看到，全局符号注册表允许我们定义和查询符号，甚至跨越 JavaScript 领域。我们简要了解了如何修改 JavaScript（或任何其他 API）与我们的对象交互的方式，使用符号作为在调用者和被调用者之间实现“契约”的手段。最后，我们以符号虽然看似私有但实际上并非真正私有这一结论收尾。

在下一章中，我们将了解一种遍历集合的机制，以及如何生成和使用惰性计算与序列。

脚注 1 2 3 4 5
