# 1. `let` 和 `const` —— 声明变量的新方式

与许多其他语言一样，JavaScript 中变量的使用非常普遍。赋值、函数调用的参数、计算结果；变量无处不在。简单来说，它们让我们能够存储程序的“状态”。

JavaScript 之前只提供了一种声明变量的机制，即无处不在的 `var`。然而，正如我们将在本章中看到的，`var` 给我们带来了几个语义上的陷阱。我们还将看到，引入两个新关键字 `let` 和 `const` 来声明变量，是如何帮助我们避免细微的 bug 和常常是意料之外的不良副作用，同时让代码的意图更加清晰。

在本章结束时，我们将了解为什么 `let` 和 `const` 不仅是声明和初始化变量的首选方式，而且还为弃用 `var` 提供了强有力的论据。我们还将学习如何重构现有代码，以充分利用 `let` 和 `const` 带来的好处，并识别它们各自的最佳使用场景。

## 变量作用域的危险性

JavaScript 同时支持语句（statements）和表达式（expressions），通常，为了捕获表达式求值的结果，我们会使用变量。但有时，一个变量虽然名为“变量”，实际上却被指定为常量。也就是说，一旦赋值，我们意图让这个变量的值在后续不再改变。

然而，到目前为止，JavaScript 只提供了一个关键字 `var` 来初始化变量，无论这些变量是值会变化的（比如 `for` 循环中的索引），还是应当保持不变的常量。对于后者，开发者只能采用命名约定，通常是大写下划线分隔的格式，比如 `SOUTH_WEST`，以此暗示它应被当作常量对待，而语言层面并没有提供运行时支持。

因此，`var` 的用法不仅被过度泛化，雪上加霜的是，它还给我们带来了几个绊脚石。使用 `var` 声明的变量会自动提升（hoist）到当前作用域的顶部。此外，在同一作用域内使用相同的名称重新声明两个 `var` 变量并不会抛出错误。将这两点结合起来，其结果可能相当隐蔽，如下例所示：

*   ① 一个顶层变量声明
*   ② 看似“遮蔽”了外层的声明
*   ③ 结果发现，这两个变量其实是同一个！

```
function insidious() {
  var functionScoped = '外层声明'; ①
  if (true) {
    var functionScoped = '内层声明'; ②
  }
  console.log(functionScoped); ③
  return functionScoped;
}
// 输出 'Inner declaration'（内层声明）
insidious();
```

有人可能会认为内层嵌套的 `functionScoped` 作用于 `if` 块内部，但这不正确！事实上，第二次声明 `functionScoped` 是一个空操作（no-op）。然而，这并不妨碍 JavaScript 先（在顶部）声明这个变量，然后在后续代码中重新赋值，正如输出结果所体现的那样。为了避免混淆，JavaScript 开发者倾向于在函数开头就声明好所有将在该函数内使用的变量。

这种 `var` 不遵循块级作用域的观念确实令人惊讶，因为许多语言都是采用词法作用域来声明变量的。也就是说，在这些语言中，变量在文本上被限定在封闭的括号内，并且在由块创建的作用域外部是不可见的。这可能会让人误以为 `var` 也是有作用域的，但实际上并非如此，从而可能导致无法解释的结果。考虑下面的例子：

*   ① 使用三个按钮模拟 DOM
*   ② 为每个按钮添加一个 `click` 处理器
*   ③ 所有按钮都输出 "Item 4 is clicked"

```
function simulateDom() {
  var pseudoDom = {
    button1: {},
    button2: {},
    button3: {},
  }; ①
  for (var i = 1; i <= 3; i++) {
    var element = pseudoDom['button' + i];
    element.click = function () {
      return 'Item ' + i + ' is clicked.'; ②
    };
  }
  console.log(pseudoDom.button1.click());
  console.log(pseudoDom.button2.click());
  console.log(pseudoDom.button3.click()); ③
  return pseudoDom;
}
// 输出 3 次 'Item 4 is clicked.'
simulateDom();
```

由于变量 `i` 被提升到了 `for` 循环外部，并且不是 `for` 循环的局部变量，每个点击处理器“看到”的都是同一个 `i`。在循环结束时，`i` 的值可预见地变为 4，因此任何按钮点击都报告这个值。

总而言之，在函数内声明的任何 `var` 变量，其作用域都是该函数，即使它是在某个块内部定义的也是如此。

### 那 `严格模式` 呢？

ES5 引入了 `strict mode`（严格模式）作为一个可选功能，用以限制 JavaScript 的某些行为。这部分目的是通过改变代码的语义，特别是禁止某些行为，来简化向未来 JavaScript 版本的过渡。虽然 `strict mode` 确实有助于防止一些相当恶劣的错误发生，例如无意中声明了“全局”变量（在函数内部不使用 `var` 关键字声明的变量），但它并没有改变我们迄今为止讨论的作用域规则。

我们强烈建议在 ES5 代码中使用 `strict mode`。而且，在我们开始理解和采用 ES6+ 的旅程中，逐步在我们现有的脚本中引入 `strict mode` 是明智之举。这无疑会使将它们转换为 ES6 变得更加容易。`strict mode` 的文档可在 [`https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode) 找到。

总而言之，如果以上所有内容让你头晕目眩，那么你并不孤单。理清 JavaScript 的作用域规则本身就很难；再结合 `var` 的变量提升，更是雪上加霜。为了解决围绕 `var` 的一些问题，ES6 启用了^(¹²) `let`。正如我们将在以下章节中看到的，`let` 解决了 `var` 带来的许多作用域缺陷。

你可能会问，为什么不直接修复 `var` 而是引入 `let`？嗯，我们可不想去搞垮整个互联网，对吧？

### `let`

正如你可能已经猜到的，围绕 `var` 的大部分混乱之所以存在，是因为 `var` 不遵守“块级”作用域。在 JavaScript 中，块是一种收集一个或多个语句的方式；换句话说，它们允许我们创建一个复合语句。块由一对花括号界定，如果你在想：“等等。循环结构（比如 `for` 和 `while`）和条件语句（比如 `if` 和 `else`）也使用花括号。那些是块吗？”那么，你是对的。

块本身不引入作用域。然而，ES6 为使用 `let` 关键字定义的变量引入了块级作用域。使用 `let` 关键字定义的变量将被限定在最近的封闭块内。换句话说，使用 `let` 关键字定义的变量在词法上被限定在最近的封闭块内：

*   ① 在块内部声明的变量不能在块外部被引用
*   ② 使用 `let` 语法声明的变量
*   ③ 和之前一样，该变量在 `if` 块定义的作用域外部不可见

```
function letFunction() {
  // console.log('在定义块之前', scopedLet); ①
  if (true) {
    let scopedLet = '我被限定在块内！'; ②
  }
  // console.log('在定义块之后', scopedVar); ③
}
// 调用它
letFunction();
```

我们可以松一口气了！使用 `let` 关键字声明的变量似乎遵循了你在其他语言中熟悉的作用域规则，并且它们确实减少了与 `var` 相关的上下文开销。

许多由 `var` 的宽泛作用域引起的问题（以及潜在的 bug）都可以通过改用 `let` 来避免。让我们重新审视之前模拟 DOM 的例子，看看使用 `let` 的效果：

*   ① `i` 是 `for` 循环的局部变量
*   ② 由于 `i` 是局部变量，每个处理器都能看到正确的 `i` 值
*   ③ 可预见地，所有点击处理器都报告正确的消息

```
function simulateDom() {
let pseudoDom = {
button1: {},
button2: {},
button3: {},
};
for (let i = 1; i <= 3; i++) { ①
let element = pseudoDom['button' + i];
element.click = function() {
return 'Item ' + i + ' is clicked.'; ②
};
}
console.log(pseudoDom.button1.click());
console.log(pseudoDom.button2.click());
console.log(pseudoDom.button3.click()); ③
return pseudoDom;
}
// 输出 'Item 1 is clicked.', 'Item 2 is clicked.', 'Item 3 is clicked.'
simulateDom();
```

正如我们所见，使用 `let` 声明的变量确实尊重封闭块的作用域，从而使代码更易于推理。

尽管 `let` 确实简化了变量的作用域规则，但它本身也有一些注意事项，我们将在下一节中看到。

### `let` 变量会被提升

事实证明，使用 `let` 声明的变量确实会被提升到其所在块的顶部。然而，在变量定义之前尝试引用该变量会导致错误。这与 `var` 的行为不同，对于 `var` 我们确实可以在其定义之前“看到”该变量（只是它求值为 `undefined`）。

请看下面的示例：

-   ① 引入一个人工块
-   ② `foo` 在声明之前引用了 `bar`
-   ③ 我们定义 `bar`
-   ④ 然后调用 `foo`

```
var foo;
{ ①
foo = function() {
console.log('I am', bar); ②
};
let bar = 'Bar'; ③
// invoke it
// foo(); ④
}
```

我们可以推断出这里发生了什么。`bar` 的声明被移动到了所在块的顶部，从而允许它在声明之前被使用。

就其本身而言，这个特性本身可能看起来并不麻烦。然而，`let` 还带来了另一个限制：你**不**允许在同一作用域内定义两个同名的变量（这再次与 `var` 的行为不同）。在这种情况下，JavaScript 会抛出一个 `SyntaxError`。

也许下面的例子可以说明问题：

-   ① 引入一个变量
-   ② 这会抛出一个错误！
-   ③ 这也会抛出一个错误

```
function determineFavoriteDrink(profession) {
switch (profession) {
case 'Programmer':
let drink = 'coffee'; ①
break
case 'Yoga instructor':
let drink = 'tea'; ②
break;
default:
let drink = 'water'; ③
break;
}
return drink;
}
// results in a 'SyntaxError'
console.log(determineFavoriteDrink('Programmer'));
```

调用该函数会导致 `SyntaxError`！原因是 `let` 同时执行提升操作**并且**阻止同一变量的重新定义。这个例子警示我们，当开始将代码从使用 `var` 迁移到 `let` 时，我们不能简单地将 `var` 替换为 `let`。`var` 可能允许我们侥幸写出草率的代码；然而，`let` 正确地收紧了规则，我们必须小心谨慎，确保重构后一切正常。不过，这对我们也有好处——`let` 展示了合理的语义，仅凭这一点就足以让我们再也不使用 `var`。

### 不再需要 IIFE

JavaScript 对 `var` 缺乏作用域的限制，常常导致人们使用创造性的解决方案来创建私有变量。其中一种解决方案是使用“立即执行函数表达式”（IIFE）。让我们来看看同一个功能的前后版本，一个使用带 IIFE 的 `var`，另一个使用带块的 `let`：

-   ① 通过创建一个匿名函数来引入人工块
-   ② 这个 `var` 将被限定在声明的函数作用域内
-   ③ 通过 `return` 一个对象来暴露公共 API
-   ④ 这将导致一个 `ReferenceError`
-   ⑤ 你可以使用公共 API
-   ⑥ 调用公共 getter

```
const counter = (function createCounter() { ①
var steps = 0; ②
function increment() {
steps++;
}
function getCount() {
return steps;
}
return {
increment: increment,
getCount: getCount,
}; ③
}());
// use it
// console.log(steps); ④
counter.increment(); ⑤
console.assert(counter.getCount() === 1); ⑥
```

有了 `let`，我们不再需要使用 IIFE，因为我们可以简单地用花括号将代码包围起来，从而创建一个封闭的作用域！

-   ① 使用花括号引入人工块
-   ② 这个变量自动限定在块作用域内
-   ③ 将函数声明转换为函数表达式
-   ④ 简单地将一个变量赋值，使其在块外部可用
-   ⑤ 我们仍然无法“看到”这个用 `let` 声明的变量
-   ⑥ 但你可以使用公共 API
-   ⑦ 调用公共 getter

```
let counter;
{ ①
let steps = 0; ②
let increment = function () { ③
console.log('increment');
steps++;
};
let getCount = function () {
return steps;
};
counter = {
increment: increment,
getCount: getCount,
}; ④
}
// use it
// console.log(steps); ⑤
counter.increment(); ⑥
console.assert(counter.getCount() === 1); ⑦
```

这两个示例都试图限制 `increment` 和 `getCount` 函数的可见性。然而，利用 `let` 可以让我们消除使用 `var` 所涉及的所有仪式性代码。

### `const`

ES6 引入了另一个关键字，即 `const`。顾名思义，它将定义一个常量。换句话说，它定义一个一旦声明其值就无法更改的变量。

除了 `const` 不能被重新赋值之外，我们目前讨论的关于 `let` 的所有内容也适用于 `const`——它们具有块作用域，会被提升到所在块的顶部，并且不能在同一作用域内声明两个同名的 `const`。

重要的是，我们要记住，不能更改的是 `const` 变量的*绑定*。这一点很重要，因为 JavaScript 对象（例如 `Object` 和 `Array`）本质上是可变的。也就是说，如果一个 `const` 被赋值为一个对象的引用，那么无法更改的是那个*引用*；对象本身可以改变，而 `const` 将看到那个改变。也许通过代码更容易理解这一点。

-   ① 一个值引用
-   ② 一个对象引用
-   ③ 重新赋值会失败并抛出 `TypeError`
-   ④ 但是我们可以修改被引用的对象

```
const VALUE_REFERENCE = 'This cannot be re-assigned'; ①
const ARRAY_REFERENCE = [ 'I', 'am', 'mutable' ]; ②
// attempt re-assignment
// VALUE_REFERENCE = false ③
// ARRAY_REFERENCE = {}
// Mutate the array object
ARRAY_REFERENCE.push('!'); ④
// prints [ 'I', 'am', 'mutable', '!' ]
console.log(ARRAY_REFERENCE);
```

#### 不可变对象怎么办？

`const` 限制自身仅使变量的绑定不可变。如果我们想要使 JavaScript 中的对象类型不可变，我们应该考虑使用例如 `Object.freeze` 来“冻结”它，或者考虑使用像 Immutable.js^((13)) 这样的库。

另一点需要注意的是，声明一个没有初始值的 `const` 也会导致 `SyntaxError`。基本上，声明这样一个 `const` 会将该常量永久地赋值为 `undefined`，考虑到 JavaScript 已经为这种情况提供了 `undefined` 关键字，这样做是多余的。

最后，JavaScript 社区已经达成共识，将常量变量命名为全大写加下划线的形式。这使得在代码库中更容易识别常量。

## 在 ES6 及以后版本中使用 `var` 的理由

我们现在知道，我们可以在任何使用 `var` 的地方使用 `let`，并获得一些额外的好处，比如更清晰的作用域规则。`const` 允许我们声明式地定义不可变的绑定。考虑到我们有了块作用域，我们可以摆脱像 `IIFE` 这样的仪式性代码来实现封装。所有这一切都说明，对于使用 ES6+ 编写的任何代码，我们`不应该`再使用 `var`。

我建议我们更进一步——我们应该寻找一种方式来编写代码，使其更多地使用 `const` 而不是 `let`。重新赋值变量会使代码难以阅读、理解和推理。在 `let` 和 `const` 之间做选择时，我强烈鼓励选择后者，你会很快发现结果会更加清晰和干净。

像 `ESLint` 这样的工具可以配置^((14)) 为在检查 ES6+ 代码时将其视为错误，强烈建议我们这样做。`var` 将永远是 JavaScript 历史的一部分；然而，现在是时候让我们转向在代码库中只使用 `let` 和 `const` 了。