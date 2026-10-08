# 7. 使用增强的对象字面量实现简洁明了的对象

JavaScript 中的字面量对象语法 (`{}`) 是一种强大的机制，允许我们*直接在代码中*描述对象。这种 `{}` 语法相当优雅且简洁；然而，仍存在一些小问题——定义方法可能略显冗长，添加计算属性也颇麻烦。ES6 极大地增强了此语法，使我们能够*更加*简洁地初始化属性和方法，同时将定义计算属性的能力提升为头等公民。

在本章中，我们将看到这些增强如何使在 JavaScript 中构建对象变得远不那么冗长。我们还将探讨在对象上添加计算属性所允许的新语法，这使得 JavaScript 中的元编程变得更容易处理。最后，我们将了解使用尾逗号的好处，以及 ES7 中新增的功能，该功能将其使用范围扩展到了函数定义和调用。在本章结束时，我们的工具包中将增添更多工具，使我们的代码更加一致，且不那么冗长。

## 对象字面量的局限性

在惯用的 JavaScript 代码中，对象字面量语法 `{}` 的使用非常普遍。然而，我们将特别讨论三个缺点：

1. getter/setter 与对象方法之间缺乏对称性
2. 无法在字面量语法中列出计算属性
3. 使用变量定义对象属性时通常伴随的冗长

让我们分别审视这些问题。

### Getter/Setter 与方法

JavaScript 中的对象除了可以定义方法，还允许定义 `get` 和 `set` 访问器。两者的区别在于调用方式——Getter 和 Setter 虽然是函数，但并非作为函数被调用，而是遵循属性的工作方式。请看以下示例，该示例旨在突出两者在使用模式上的差异：

1.  ① 引入属性
2.  ② 创建 `get` 方法
3.  ③ 一个 `set` 方法
4.  ④ 验证输入
5.  ⑤ 以不同方式存储
6.  ⑥ 传统方法
7.  ⑦ 属性查找
8.  ⑧ 这将产生一个验证错误
9.  ⑨ 调用 `set` 方法
10. ⑩ 调用 `get` 方法
11. ⑪ 调用普通方法

```javascript
const song = {
name: 'Yellow Submarine', ①
get duration() { ②
console.log('Getter is invoked');
return this._duration ? (this._duration / 60) : 0;
},
set duration(inMin) { ③
if (inMin <= 0) { ④
throw new Error('duration of song cannot be less or equal to 0 min');
}
console.log(`Setter is invoked with ${inMin}`);
this._duration = inMin * 60; ⑤
},
getName: function () { ⑥
return this.name;
},
};
console.assert(song.name === 'Yellow Submarine'); ⑦
// song.duration = -1; ⑧
song.duration = 3.00; ⑨
console.assert(song.duration === 3); ⑩
console.assert(song.getName() === 'Yellow Submarine'); ⑪
```

可以推测，Getter 和 Setter（通常称为“虚拟属性”）允许我们在看似是属性查找的过程中实现钩子。这些钩子可用于多种场景，包括验证和清理输入，或像之前演示的那样在输出时修改属性。当我们比较读取（`get`）和写入 `duration` 与调用 `getName` 方法时，这种差异就显而易见了。

关键在于，尽管它们都被定义为对象的方法，但定义 Getter/Setter 与定义方法的方式缺乏对称性。前者采用了优雅甚至对于其他语言背景的开发者来说可能熟悉的语法，而后者则继续沿用传统的键值语法。

### 在对象字面量上定义计算属性

JavaScript 中的对象还允许在运行时“计算”键名，这为 JavaScript 提供了一定程度的元编程能力。为了在对象上添加一个计算属性，我们需要对现有对象使用“方括号”（或下标）表示法，如下所示：

1.  ① 一个从属性生成方法名的函数
2.  ② 将给定属性的名称首字母大写
3.  ③ 使用下标语法将计算出的方法名添加到对象上

```javascript
const getterFor = (prop) => { ①
const capitalized = prop.charAt(0).toUpperCase() + prop.slice(1); ②
return `get${capitalized}`;
};
const song = {
name: 'All You Need Is Love',
album: 'Yellow Submarine',
};
song[getterFor('name')] = function() { ③
return song['name'];
};
song[getterFor('album')] = function() {
return song['album'];
};
console.assert(song.getName() === song.name);
console.assert(song.getAlbum() === song.album);
```

在这个例子中，我们从一个简单的辅助函数开始，该函数根据给定的属性名称计算出 Getter 的名称。对于属性 `name`，`getterFor('name')` 会直接返回 `getName`。

正如我们所看到的，方括号表示法提供了一种计算即将成为对象键名的*表达式*的方法，并将其附加到对象上。然而，我们必须在 `{}` 对象表示法之外执行此操作，因为该语法不允许我们像定义其他属性那样内联操作。这迫使我们不得不在多个不同的地方分散定义“完整”的对象。

### 使用变量定义对象属性

地道的 JavaScript *无处不在*地使用对象字面量，从使用“揭示模块模式”封装私有成员并暴露公共 API，到 Node.js 的 `exports`。这种使用对象字面量的方式常常会导致大量代码重复。请看：

1.  ① 一个私有成员变量
2.  ② 一个私有函数
3.  ③ 在对象字面量上暴露特定的公共 API
4.  ④ 将对象创建包装在 IEFE（立即执行函数表达式）中，以将私有成员封装在函数作用域内

```javascript
const greeter = (function () {
let greeting = 'Hello'; ①
const exclaim = msg => `${msg}!`; ②
const greet = name => exclaim(`${greeting} ${name}`);
const salutation = (newGreeting) => {
greeting = newGreeting;
};
return {
greet: greet,
salutation: salutation,
}; ③
}()); ④
console.assert(greeter.greet('Mason') === 'Hello Mason!');
greeter.salutation('Hola');
console.assert(greeter.greet('Micah') === 'Hola Micah!');
```

揭示模块模式利用了函数会引入作用域这一特性。我们从一个“立即执行函数表达式”（IEFE）开始。在 IEFE 内部，我们可以自由定义任意数量的“私有”成员和函数，它们都被限定在封闭函数的作用域内。在 IEFE 的末尾，我们通过字面量语法创建一个新对象，并将其属性指向需要作为公共 API 一部分的成员。正如我们所观察到的，这种实现方式存在相当多的重复，例如 `greet: greet`。如果这是一个更复杂的实现，那么它不仅会显得冗长，而且也容易出错。

现在，我们已经了解了对象字面量的一些用例，接下来让我们探讨 ES6 如何尝试通过“增强的”对象字面量语法来克服这些问题。

## 增强的对象字面量语法

ES6 中对对象字面量语法的增强，旨在简化我们在字面量对象定义中定义属性和方法的方式。

### 方法定义简写

对象字面量语法为字面量内的方**法定义**引入了一种简写形式，使其与 Getter/Setter 的定义保持一致。我们重新回顾前面的示例，这次使用简写形式：

1.  ① 使用方法简写定义一个普通函数
2.  ② 定义一个接收参数的方法

# JavaScript 对象字面量新特性

## 方法定义简写

```javascript
const song = {
name: 'Yellow Submarine', ①
get duration() { ②
console.log('Getter is invoked');
return this._duration ? (this._duration / 60) : 0;
},
set duration(inMin) { ③
if (inMin <= 0) { ④
throw new Error('duration of song cannot be less or equal to 0 min');
}
console.log(`Setter is invoked with ${inMin}`);
this._duration = inMin * 60; ⑤
},
getName() { ①
return this.name;
},
setName(name) { ②
this.name = name;
},
};
console.assert(song.getName() === 'Yellow Submarine');
song.setName('All You Need Is Love');
console.assert(song.getName() === 'All You Need Is Love');
```

这个例子与我们之前尝试的版本并无不同——但是，`getName`（和 `setName`）的定义显著地不那么冗长了。`:` 和 `function` 关键字已经消失，剩下的只是方法实现。现在，我们发现字面量上的任何方法定义之间（无论是 Getter/Setter 还是普通方法）都达到了对称。

## 计算属性值

计算属性值的增强特性让我们能够在字面量语法内部定义计算属性，从而避免将字面量的定义分散在代码库的不同位置。我们回到之前的示例，不过这次改用内联计算属性：

- ① 一个根据属性创建名称的函数
- ② 拆分名称以获取第一个字符
- ③ 内联计算属性的定义

```javascript
const getterFor = (prop) => { ①
const capitalized = prop.charAt(0).toUpperCase() + prop.slice(1); ②
return `get${capitalized}`;
};
const song = {
name: 'All You Need Is Love',
album: 'Yellow Submarine',
[getterFor('name')]() { ③
return song.name;
},
[getterFor('album')]() {
return song.album;
},
};
console.assert(song.getName() === song.name);
console.assert(song.getAlbum() === song.album);
```

这一增强特性让我们可以像往常一样定义计算属性，即使用方括号语法，只不过方括号现在出现在字面量内部，使它们与其他属性定义一样成为一等公民。新的语法还沿用了方法定义简写引入的对称性，让我们可以省略多余的 `function` 关键字。同样地，如果我们要定义的是属性而非方法，熟悉的 `key:value` 语法在这里同样适用，与传统键名不同的是，计算键名仍需要用方括号括起来。因此，计算属性的等价写法是 `[computedKey]:value`。

## 属性值简写

属性值简写并不是定义属性（和方法）的新方式，而是提供了一种减少“揭示模块模式”或 node `exports` 中常见冗余代码的方法。它允许我们仅在键名与作用域中现有变量名匹配时，压缩对象字面量中键值对的定义。

我们重新审视揭示模块模式的定义，这次使用属性值简写：

- ① 使用属性值简写

```javascript
const greeter = (function() {
let  greeting = 'Hello';
const exclaim = msg => `${msg}!`;
const greet = name => exclaim(`${greeting} ${name}`);
const salutation = (newGreeting) => {
greeting = newGreeting;
};
return {
greet, ①
salutation,
};
// 等价于
/*
return {
greet: greet,
salutation: salutation
}
*/
}());
console.assert(greeter.greet('Mason') === 'Hello Mason!');
greeter.salutation('Hola');
console.assert(greeter.greet('Micah') === 'Hola Micah!');
```

在这个例子中，（返回的）模块中引入的属性（键）与其值同名——现在我们就可以使用简写语法来消除这种重复。当然，如果需要，我们也可以将属性值简写与熟悉的 `key:value` 记号混合使用。当返回对象中某些键的名称与作用域中任何变量的名称**不**匹配时，这种方式会非常方便。

这看似微小的优化，但如果将其推广到大型模块，或许我们就能看出可以避免多少重复，从而遵循 DRY 原则，并避免因拼写错误而引入潜在 bug。

## 尾逗号的情况

JavaScript 已经允许在数组和对象字面量中使用“尾逗号”：

- ① 数组字面量中的尾逗号
- ② 对象字面量中的尾逗号

```javascript
const friedmanBooks = [
'The Little Schemer',
'The Seasoned Schemer',
'The Reasoned Schemer', ①
];
const hoyteBook = {
name: 'Let Over Lambda',
author: 'Doug Hoyte',
isbn10: '1435712757', ②
};
```

尾逗号这个特性看似微不足道，但它具有强大的功效。它使我们的代码保持一致性，让数组或对象定义中的每一行看起来都一样。这使得字面量中项目的重新排序变得容易，并且在版本控制系统中添加或删除项目（尤其是在字面量的末尾）时，会产生更清晰的差异对比。

ES7 将尾逗号引入到函数参数列表和函数调用中：

- ① 函数参数列表中的尾逗号
- ② 函数调用中的尾逗号

```javascript
const rate = (
book,
starCount, ①
) => `You rated '${book}' ${starCount} stars`;
const rating = rate(
'Practical Common Lisp',
5, ②
);
// 输出 'You rated 'Practical Common Lisp' 5 stars'
console.log(rating);
```

这一添加旨在实现一致性，从而使函数定义和调用也能享受尾逗号的所有好处。

表面上看，尾逗号只有在元素单独成行时才有价值。因此，建议仅在元素（或对象属性）独占一行时强制执行尾逗号。大多数像 `ESLint` 这样的代码检查工具都可以配置为仅在多行声明中标记缺失的尾逗号。

# 总结

ES6 引入了一系列新的语法和 API，使得创建新对象（无论是通过字面量还是基于现有对象）变得更加简单、简洁且不易出错。

在下一章中，我们将探索 `Symbol`——ES6 引入的一种新的原始类型，它通常与增强的字面量语法结合使用，既可作为在对象中引入“唯一”键的机制，也可通过元编程来修改对象的行为。
