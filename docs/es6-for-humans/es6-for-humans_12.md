# 11. 超越 ES6

正如第一章所述，ES6 的发布版本非常庞大，是对现有 JavaScript 版本（即 ES5）的重要更新。我们在本书的前几章中已经全面介绍了所有主要变化。TC39 花费了六年多的时间才推出新版本的语言，并引入了大量全新的概念和功能。委员会在版本发布末期做出的一项重大决定是：他们不会再像以前那样花费太长时间才得出结论并发布新功能。他们决定正式将 ES6 命名为 ES2015，并在此后以逐年递增的方式发布更小规模的增量更新，并以发布年份命名。在本章中，我们将探讨 ES2016 和 ES2017 中的变化和新特性。

## ES2016

ES2016 是对现有 ES2015（ES6）版本的一次小型增量更新，引入了`Array.prototype.includes`方法和求幂运算符`**`。你还可以查看 KangaX 兼容性表格，了解支持 JavaScript 的不同浏览器和平台对 ES2016 新特性的支持程度：[`https://kangax.github.io/compat-table/es2016plus/`](https://kangax.github.io/compat-table/es2016plus/)。

### 数组的`includes`方法

ES2016 为 JavaScript 中的数组原型引入了`includes`方法。你可以使用此方法检查数组中是否存在某个元素。它会根据传递给它的元素是否属于调用该方法的数组的一部分，返回一个布尔值。请参考以下示例：

```
['apple', 'banana', 'carrot'].includes('apple');    //true
['apple', 'banana', 'carrot'].includes('orange');   //false
```

如上例所示，在水果数组上使用`includes`方法，如果传递给它的元素是数组的一部分，则返回`true`，否则返回`false`。在 JavaScript 中，传统上这种检查是通过`indexOf`方法完成的，该方法如果存在则返回元素的索引，否则返回-1。所以，上面的示例实际上等同于：

```
['apple', 'banana', 'carrot'].indexOf('apple') >= 0;    //true
['apple', 'banana', 'carrot'].indexOf('orange') >= 0;   //false
```

上述两个语句的工作方式与之前使用`includes`的示例完全一致。但`includes()`和`indexOf()`的主要区别在于它们与值`NaN`（非数字）的交互方式，这有助于识别该值。

```
const arr = [NaN];
arr.includes(NaN);              // true
arr.indexOf(NaN);               // -1
```

从这个示例可以明显看出，`indexOf`不能用于检查数组中的`NaN`值，但`includes`方法允许我们检查数组中是否存在`NaN`值。

### 求幂运算符 `**`

求幂运算符是 ES2016 中引入的一个新的算术运算符，它与 Python 等语言中的对应运算符非常相似，使用双星号来计算数字的幂值。它等同于 Math 库中传统的`pow()`函数。

```
let a = 2;
let b = 3;
a ** b               //8
Math.pow(a, b)       // 8
```

使用求幂运算符是一种快速计算数值的方法，当数字被提升到相应幂次时，例如本例中 2 的 3 次方等于 8。

## ES2017

ES2017 为该语言带来了两个主要的新特性，即异步函数和带共享内存的 Atomics。它还包括其他更简单的特性，如`Object.entries()`、`Object.values()`、`Object.getOwnPropertyDescriptors()`、`padStart`、`padEnd`，以及在函数参数列表和调用中使用尾后逗号的能力。让我们更详细地了解每一个特性。

### 异步函数

ES2017 引入了一种处理异步代码的令人兴奋的新方式，而异步代码是 JavaScript 的生存之道。我们已经了解了一种使用 Promise 处理异步操作的流行方法。ES2017 在此基础上进一步构建，通过使用`async`和`await`关键字，使其以同步方式工作。为了正确理解这一点，请考虑以下示例，我们只是简单地创建了一个返回 Promise 的函数：

```
function getData(site) {
return fetch(site)
.then(request => request.json())
}
```

对于上面的示例，调用`getData()`函数将返回一个 Promise，该 Promise 将在我们的执行上下文中异步地得到解决。现在使用`async`/`await`关键字，我们可以利用基于 Promise 的方法来发挥生成器模式的优势，使我们的代码表现得像同步代码。`async`始终返回一个可以被解析为值的 Promise，而`await`会暂停执行，直到 Promise 被解决。我们可以使用`async`/`await`关键字重写上面的示例，如下所示：

```
async function getData(site) {
let request = await fetch(url);
let text = await request.text();
return JSON.parse(text);
}
```

请注意，`await`关键字仅适用于 Promise，如果表达式不是 Promise，它会将其转换为 Promise。你还可以在函数表达式、方法定义和箭头函数中使用`async`，如下例所示：

```
// 使用函数表达式：
const getData = async function () {};
// 使用方法定义：
const item = { async getData() {} }
// 使用箭头函数：
const item = async () => {};
```

你可能对上一个示例中使用的`await`关键字感到好奇：`await`运算符会等待操作数（即 Promise）被解决，如果 Promise 被履行，其结果就是兑现值；如果 Promise 被拒绝，它会抛出拒绝值。因此，我们可以使用传统的`try`/`catch`块配合`async`/`await`，以更好的方式处理 Promise。

### 原子操作与共享内存

任何熟悉 `WebWorker` 的人都已经了解创建工作进程的过程。通过分配 `Worker` 对象并将需要运行的脚本传入其中来创建它们。工作进程通过消息通道进行通信，多种类型的数据都可以通过此通道发送，并且当数据到达目的地时，其类型和结构会保持不变。在 ES2017 中，要分配共享内存，你可以直接使用 `SharedArrayBuffer` 构造函数。它就像一个普通的 `ArrayBuffer`，但其内存是共享的。

此外，ES2017 中的这个新规范还为我们带来了新的底层 `Atomics` 命名空间对象的概念，它与 `SharedArrayBuffer` 构造函数一起，为我们提供了用于构建更高级别并发抽象的基本构建块。你可以使用这些特性在多个 Web Worker 和主线程之间共享 `SharedArrayBuffer` 对象中的数据。这使你能受益于轻松地在 Worker 之间共享数据，从而更好地协调它们之间工作。关于此主题的更详细教程和规范（由共享内存和 `Atomics` 的原始提案作者 Lars Hansen 编写）可以在以下链接中找到：[`https://github.com/tc39/ecmascript_sharedmem/blob/master/TUTORIAL.md`](https://github.com/tc39/ecmascript_sharedmem/blob/master/TUTORIAL.md)，但这超出了本书的范围。

# Object.entries() 和 Object.values()

`Object.entries()` 和 `Object.values()` 是 `ES2017` 中引入的两个新方法。`Object.entries()` 方法在对象上运行时，会以 `for-in` 循环提供的相同顺序返回该对象自身的可枚举属性 `[key, value]` 对。但与 `for-in` 循环不同，它不会枚举原型链上的属性。类似地，`Object.values()` 在传入对象时，会返回一个包含其自身可枚举属性值的数组：

```javascript
const myObj = { a: 1, b: 42 };
Object.entries(myObj);     // [['a', 1], ['b', 42]]
Object.values(myObj);      // [1, 42]
```

正如你在上面的示例中所看到的，当传入 `Object.entries()` 方法时，该对象会返回一个包含其 `键/值对` 的数组，而 `Object.values()` 方法则返回属性值。

# padStart 和 padEnd

`padStart()` 和 `padEnd()` 方法是 ES2017 中引入的两个新的字符串方法，它们有助于向字符串添加填充，以便结果字符串的长度等于作为第一个参数传入的值。你还可以传入第二个可选参数（另一个字符串）来代替默认的空格进行填充。思考以下示例：

```javascript
'string'.padStart(10);           // "    string"
'string'.padStart(10, "abc");    // "abcastring"
'string'.padStart(10,"123465");  // "1234string"
'string'.padStart(8, "0");       // "00string"
'string'.padStart(3);            // "string"
'string'.padEnd(10);             // "string    "
'string'.padEnd(10, "abc");      // "stringabca"
'string'.padEnd(8, "123456");    // "string12"
'string'.padEnd(1);              // "string"
```

正如你在上面的示例中所看到的，添加到给定字符串的填充使得最终结果字符串的长度要么大于第一个参数的长度，具体取决于原始字符串的长度；如果给定的长度更大，则会根据你使用的方法在字符串的开头或结尾添加适当数量的填充。你也可以使用第二个参数来使用不同的字符串代替空格进行填充。

# Object.getOwnPropertyDescriptors()

顾名思义，`Object.getOwnPropertyDescriptors()` 是 ES2017 中的一个新方法，它允许你精确检查给定对象所有自身属性的描述。JavaScript 对象中的属性通常由一个字符串名称和一个属性描述符组成。描述符是属性值、布尔值 `writable`、`get` 函数、`set` 函数、布尔值 `configurable` 和布尔值 `enumerable` 的记录。思考以下示例：

```javascript
const myObj = {
[Symbol('mySymbol')]: 42,
get random() { return 'test' },
};
console.log(Object.getOwnPropertyDescriptors(myObj));
// Object {random: Object, Symbol(mySymbol): Object}
// {random:{
//     configurable: true,
//     enumerable: true,
//     get: function random(),
//     set: undefined}
//[Symbol('mySymbol')]:{
//     configurable: true,
//     enumerable: true,
//     value: 42,
//     writable: true}
//}
```

在像 `myObj` 这样的对象上使用 `Object.getOwnPropertyDescriptors()` 的输出将大致如上所示。请注意，上述所有特性都相当新且是实验性的，因此你可能不总是能得到完全相同的结果。话虽如此，它们现在正变得越来越标准化。

## 函数参数列表与调用中的尾逗号

ES2017 中的这个特性是一个简单的语法更新。在引入此特性之前，即使规范的其他部分（如数组和对象字面量的情况）中已普遍允许，但在 JavaScript 中是不允许在最后一个函数参数后面有逗号的。通过 ES2017 的这一更新，你在整个 JavaScript 中拥有了更统一的尾逗号语法。所以你可以这样做：

```javascript
const trailFunct = function(x, y, z,) {
// 函数体
}
```

这纯粹是语言的一个语法更新，对该语言的行为或功能没有重大改变。

# 总结

本章我们探讨了在编写本书时已发布的 `JavaScript` 各种微小增量变更以及基于 `ES6` 的更新。`TC39` 的目标是每年发布一次新规范，这些规范从提案阶段进入最终规范阶段，然后被纳入到语言中。它们应当是小型功能更新，不像 `ES6`（`ES2015`）那样是巨大的变革。

索引 A、B、`Array.from()`、`Array.of()`、`Array.prototype.copyWithin()`、`Array.prototype.entries()`、`Array.prototype.fill()`、`Array.prototype.find()`、`Array.prototype.findIndex()`、`Array.prototype.keys()`、`Array.prototype.values()`、异步函数、异步模块定义（AMD）、C、CommonJS 风格模块、`Constructor()` 方法、D、解构赋值、默认值、定义、JSON 数据格式中的 `JavaScript`、嵌套值、对象和数组、参数值、剩余语法、E、F、ECMA 国际组织、ECMAScript 2015、ECMAScript 3.1、ECMAScript 4、ECMAScript 6（ES6）、添加加载器、箭头函数、项目中的 `Babel` 和块级作用域、样板代码、类与访问器属性、内置类、基于类的开发模式、经典类导向开发、计算属性名、`constructor()` 方法、构造函数环境、声明、表达式、继承与 `super` 关键字、继承静态属性、语言、可选构造函数方法、属性与隐私、静态方法与属性、构造函数调用、默认函数参数的核心概念、ECMA、ECMAScript 与 `JavaScript`、ES6/ES2015 知识、函数调用、函数参数与全局作用域、循环结构、对象字面量、原型方法、剩余与展开运算符、“稻草人提案”、带标签的模板字面量、TDZ、模板字面量与分隔符、转译、Web 浏览器与 `Node.js`、`webpack` 与 `webpack-dev-server`、数组上的 ES2016、原子操作与共享内存、ES2017、指数运算符 `**`、`object.entries()`、`object.getOwnPropertyDescriptors()`、`object.values()`、`padStart` 与 `padEnd`、尾后逗号、ES2017 表达式插值、G、H、生成器、I、J、K、L、立即执行函数表达式（IIFE）、拦截、内省、可迭代协议、迭代器生成器、可迭代对象、可选方法、协议、`return()` 与 `throw()`、`Symbol.iterator`、`yield` 语句、M、`Map`、元编程、ES5 与 ES6 概述、未来 ECMAScript 版本、内部语言特性、ES6 中代理处理器的代理、可撤销代理、浏览器中的模块系统、ES6 中的解构、`eval()`、导出、导入、`JavaScript` 应用、模块化语义代码库、只读绑定、N、新的 `Array.prototype` 方法、O、`Object.defineProperty()`、对象解构语法、`Object.entries()`、`Object.getOwnPropertyDescriptors()`、`Object.getOwnPropertySymbols()`、`Object.getOwnSymbolProperties()`、`Object.values()`、`OnResolved()` 函数、P、Q、`padEnd()` 方法、`padStart()` 方法、待定状态的 `Promise`、`Promise()` 构造函数、`Promise`、异步操作、链式调用、创建、错误处理、嵌套回调、对象、带 `then()` 和 `catch()` 的 `setTimeout`、`XMLHttpRequest`、R、`Reflect.get()`、`Reflect.getPrototypeOf()`、`Reflect.has()`、`Reflect.set()`、`Revoke()` 函数、S、自修改、字符串替换、`Symbol.for(key)` 方法、`Symbol.for(myCar)`、`Symbol.iterator`、`Symbol`、内置符号的应用、创建、`symbol.for(key)`、`Symbol.keyFor(symbol)`、`.toString()` 方法、T、暂时性死区（TDZ）、类型化数组、U、V、通用模块定义（UMD）、W、X、Y、Z、`WeakMap`、`WeakSet`