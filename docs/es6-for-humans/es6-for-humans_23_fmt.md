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