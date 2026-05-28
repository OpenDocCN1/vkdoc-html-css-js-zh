# 9. ES6 中的 Promise

众所周知，JavaScript 在设计上是单线程的。在浏览器中，它使用事件处理来并发管理许多任务。JavaScript 引擎管理一个事件队列，当事件发生时，会调用注册的处理函数。如果你有传统 JavaScript 的经验，那么你很清楚理解异步编程是正确使用 JavaScript 的必备条件。使用异步的主要模式是通过回调函数。ES6 为标准 JavaScript 规范带来了一个新特性 —— Promise（一些新的和最新的 JS 库和框架中已有的特性），它解决了回调（唯一的异步方法）中的许多重大问题。

## Promise 概述

在某些需要等待异步操作完成然后执行任务的情况下，JavaScript 严重依赖回调函数，允许代码执行绕过长时间运行的任务继续进行。您一定已经知道`setTimeout`、`XMLHttpRequest`以及所有基于浏览器的异步函数都是基于回调的。虽然这个概念在理论上简单易懂，但它可能导致一些非常令人困惑且难以理解的代码，尤其是在需要在回调之后进行回调（嵌套回调）的情况下，这通常被称为“回调地狱”。例如，考虑以下代码片段，其中必须先发生 X，然后发生 Y，最后发生 Z：

```
x = getData();
y = getMoreData(x);
z = getMoreData(y);
```

在 ES6 之前，您可以异步获取`x`，然后将其作为参数传递给获取`y`的函数，对`z`也是如此，使用回调的方式如下：

```
getData(function(x){
    getMoreData(x, function(y){
        getMoreData(y, function(z){
            ...
        });
    });
});
```

在实际应用中，当您有大量回调函数时，这可能会变得越来越复杂，从而形成回调金字塔。这些回调金字塔无处不在——在处理 HTTP 请求、数据库操作、动画、进程间通信以及其他各种场景中都会出现。通常，使用回调函数的代码会变得更难理解、重构和测试。

这就是 Promise 的用武之地。Promise 为我们提供了一种更可控的模式来处理异步处理。它们代表了一个可以在未来某个时间点处理的值，并提供了一种摆脱回调地狱的方法。与基于回调的方法相比，Promise 为执行、组合和管理异步操作提供了一种更简单的替代方案。它们还使得代码的逻辑流程更容易理解。

ES6 原生支持 Promise。一个 Promise 是一个等待异步操作完成的对象，当该操作完成时，该 Promise 要么被兑现（fulfilled），要么被拒绝（rejected）。一个 Promise 对象可以处于以下三种状态之一：

*   `fulfilled` – 当 Promise 成功时
*   `rejected` – 当 Promise 失败时
*   `pending` – 当它既未兑现也未拒绝时

一个待定（pending）的 Promise 可能会转换为兑现（fulfilled）或拒绝（rejected）状态，并且当 Promise 被兑现或拒绝时，它被认为已敲定（settled）。需要注意的是，一个已敲定的 Promise 是不可变的，这意味着一旦 Promise 被敲定，就不能重新敲定。

### 创建 Promise

Promise 是使用`new Promise()`构造函数创建的，该构造函数接受一个执行器（一个函数），该函数有两个参数：

*   第一个参数（通常命名为`resolve`）是一个函数，当未来值就绪时，即 Promise 被兑现时，会调用该函数；
*   第二个参数（通常命名为`reject`）是一个函数，如果它无法解决未来值，则调用该函数来拒绝 Promise。

执行器启动一些异步工作，然后一旦完成，就调用`resolve`或`reject`函数来兑现 Promise，或者如果发生错误则拒绝它。

一个简单的 Promise 如下所示：

```
const p = new Promise((resolve, reject) => {
    if (/* some condition */) {
        resolve(/* some value */);  // fulfilled successfully
    } else {
        reject(/* some reason */);  // error, rejected
    }
});
```

第二个方法（`reject`）是可选的，您可以只使用`resolve`方法来创建 Promise，如下面的代码片段所示，其中 Promise 分别被兑现和拒绝：

```
new Promise(resolve => resolve()) // promise is fulfilled
new Promise((resolve, reject) => reject()) // promise is rejected
```

您也可以使用以下方式创建一个立即兑现的 Promise：

```
const sayHello = Promise.resolve("hello!");
```

类似地，您也可以使用`Promise.reject(value)`来拒绝一个 Promise。不带值地兑现或拒绝 Promise 并不十分有用。通常，一个 Promise 会兑现为某个值，该值可能是来自 HTTP 请求、动画或其他异步操作的结果。

一旦创建，一个 Promise 只能被敲定，这意味着它要么被兑现，要么被拒绝。并且如上所述，由于已敲定的 Promise 是不可变的，因此已敲定的值或失败原因不能被更改（该值也可以是`undefined`）。然而，一个对象可以用作已兑现的值，并且对象的属性可能会发生改变。

### 使用`then()`和`catch()`消费 Promise

一旦创建了 Promise，它可以作为一个值传递，本质上表示未来值的占位符。当 Promise 被兑现时，可以使用`.then()`方法来消费这个值。此方法接受一个函数，该函数将在 Promise 被兑现时被传递已兑现的值。例如，考虑以下代码片段：

```
const p = new Promise((resolve, reject) => resolve(42));
p.then((val) => console.log(val)); // 42
```

每个 Promise 都必须有一个`.then()`方法，它实际上接受两个可能的参数。第一个参数是 Promise 被兑现时要调用的函数，第二个参数是 Promise 被拒绝时要调用的函数，如图 9-1 所示。

![A431489_1_En_9_Fig1_HTML.gif](img/A431489_1_En_9_Fig1_HTML.gif)

图 9-1. 一个待定的 Promise 可以被兑现或拒绝，并使用`.then()`方法进行处理

```
p.then((value) => { console.log("Promise Fulfilled:", value) },
       (error) => { console.log("Promise Rejected: ", error) });
```

如果给定的 Promise 总是被解决，我们可以为了简单起见省略第二个参数。查看以下示例，其中顾客在订单下达五秒后收到他的披萨：

```
const pizza = new Promise((resolve) => {
    console.log("Getting your pizza in 5 seconds...");
    setTimeout(() => {
        resolve("Onion Pizza");
    }, 5000);
});
pizza.then(
    (item) => { console.log(`Order Received: ${item}`) },
    (error) => { console.log("Something went wrong with your pizza") }
);
// Getting your pizza in 5 seconds...
// Order Received: Onion Pizza
```

此示例演示了两件事：

*   首先，我们附加到 Promise 上的处理程序是在所有其他代码异步运行之后才被调用的。
*   其次，兑现处理程序仅在 Promise 被兑现时被调用，并传入它被解决时的值（在我们的例子中，是洋葱披萨）。拒绝处理程序也是如此。

这是另一个消费 Promise 的典型示例，其中`getRentalList(location)`是异步的，并使用返回 Promise 的 Web 服务返回给定位置当前可用的租赁房产列表：

```
const housingPromise = getRentalList("London");
housingPromise.then(
(res) => {
if (res.properties && res.properties.length > 0) {
console.log(`We found the houses for you: ${res.properties}`);
} else {
console.log(`Sorry, no housing is available in ${res.location}`);
}
},
(error) => {
console.log(`Something went wrong: ${error}`);
}
);
```

如果您只对拒绝 Promise 感兴趣，可以省略第一个参数并将其传递为`null`。例如，查看以下代码片段：

```
const networkReq = new Promise((resolve, reject) => {
reject("No Server Found");
});
networkReq.then(
null,
(error) => { console.log(error); }
);
```

一方面，如果处理程序在`.then()`调用中返回一个值，它会在返回时自动包装在一个 Promise 中。这些`.then()`调用可以被链式调用。我们将在下一节详细讨论 Promise 链式调用的工作原理。另一方面，您也可以使用`catch()`方法以更紧凑的方式处理被拒绝的 Promise。例如，您可以重写上面的示例如下：

```
networkReq.catch((error) => { console.log(error); });
```