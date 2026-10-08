# 10. 用 Promise 避免回调

处理异步操作是一件繁琐的事情。在很长一段时间里，我们求助于使用回调函数，但事实证明，这种模式很难驾驭，尤其是在状态管理和错误传播方面。随后，我们提升了抽象层次，利用了 Promise。然而，它们本身也带来了一系列问题——不同的库有不同的实现方式，这让我们感到沮丧，并寻求一种合理的方式来处理异步操作。

这种情况现在改变了！JavaScript 在 ES6 中引入了 Promise 作为原生 API，将 Promise 提升为该语言的一等公民。在本章中，我们将探讨 Promise 提供的 API，以便我们可以在代码中开始使用 Promise，从而弃用回调。我们还将学习如何封装那些本质上依赖回调的操作，以便我们能够始终一致地使用 Promise。基于本章所学的内容，我们将一劳永逸地摆脱回调，同时使我们的代码更易于编写和理解。

## Promise 的“破碎承诺”

JavaScript，按设计来说，是一种强制采用异步编程范式的语言。作为 JavaScript 开发者，我们都清楚代码是在单线程中运行的。因此，“阻塞”操作——如 I/O、网络请求，甚至周期性调用（使用`setInterval`）——都需要异步执行。也就是说，它们需要以一种不阻塞主线程的方式执行。

通常，这是通过回调函数来实现的。异步 API 期望一个或多个“处理”函数作为参数，这些处理程序会在异步调用返回结果（或错误）时被调用。处理程序包含了当异步调用完成时要执行的操作。

回调函数虽然常见，但可能难以处理。尝试对异步调用进行排序（其中一个调用的执行依赖于前一个调用的响应）可能会导致回调嵌套（被戏称为[厄运金字塔](https://en.wikipedia.org/wiki/Pyramid_of_doom_%28programming%29)^(⁵⁸)）。这种嵌套的代码风格导致更高的耦合度，使其更难测试。由于处理程序在与你代码其余部分不同的“上下文”中运行，错误的报告和传播需要仔细的处理和管理。

回调强制了一种“急切”的编程风格，也就是说，任何期望回调作为参数的异步函数都强制调用方在操作完成之前确切知道需要做什么。由于回调必须在调用异步操作之前定义，因此你无法将如何消费调用的结果推迟到开始或完成工作之后。从概念上讲，要执行的（异步）任务与任务完成后要执行的工作是绑定在一起的。

但最重要的是，阅读包含回调的代码需要一些脑力劳动，并且难于理解代码逻辑，尤其是当回调彼此嵌套时。诚然，我们可以将回调重构为独立的函数，并使用它们，但这似乎只是“把问题踢到更远的地方”。在处理嵌套回调时，有一些常用的策略，其中许多策略已在诸如 [Callback Hell](http://callbackhell.com/)^(⁵⁹) 这样的网站中有所记载。

于是，Promise 应运而生。顾名思义，Promise 代表了某些最终会发生的工作的结果。使用 Promise 时，异步调用不再接受处理函数，而是立即返回一个 Promise 对象（或将其包裹在一个 Promise 中）。这个 Promise 代表了未来潜在成功/失败的结果，它可以被传递到应用程序的其他部分，从而将异步工作的概念提升为像任何其他对象一样的一等公民。

这个 Promise “令牌”暴露了一个`then`方法，它期望另一个处理程序，并反过来返回一个新的 Promise，该 Promise 将处理程序的结果包裹起来。提供给`then`的处理程序是在其所链接的 Promise 完成后要执行的工作。这允许我们推迟对异步操作结果的处理，直到稍后，从而有效地将启动工作与消费结果的地点解耦。

鉴于`then` API 本身返回一个 Promise，这允许我们“链式调用”Promise，从而为我们提供了一种优雅表达排序的机制——实际上就是“执行这个，完成后，然后再做那个。”

最后，Promise 对象暴露了一种捕获任何错误的机制，通常使用`catch`方法，这允许捕获并处理 Promise 链中任何位置可能发生的任何错误。

Promise 是对回调的一种简单抽象——它们只是提供了一种包装回调的机制，减轻了我们开发者处理状态和错误管理的需要。

然而，长期以来，Promise 是作为库来实现的。流行的独立实现包括 Kris Kowal 的 `q`^(⁶⁰)，而像 jQuery 这样的库也自带其内部实现。希望使用 Promise 的开发者要么被迫引入（显式或隐式地）一个第三方库，因为语言本身不支持它。

这就引出了第二点——虽然存在一个社区驱动的规范^(⁶¹)来规定 JavaScript Promise 应如何工作，但无法保证某个实现确实遵循了该规范，或者该实现（对规范）的遵循程度如何。换句话说，没有一个规范的 API 来处理 Promise，使得调用方可以不依赖于具体的实现细节。

再加上许多库和框架选择遵循原生的语言结构（而这种结构只有一个——回调），从而造成了分裂，往往在一个应用程序中，代码同时使用了回调和 Promise，进一步加剧了混乱。

为了解决这些问题，ES6 将 Promise API 引入到了 JavaScript。接下来，让我们深入探讨这个 API 的细节。

### 使用 Promise

Promise 通过 `Promise` 构造函数创建，该构造函数只接受一个函数作为参数。这个函数本身又接收两个参数，而这两个参数恰好也都是函数。第一个参数用于发出 Promise 成功兑现的信号，第二个参数则用于发出失败兑现的信号。我们来看一个简单的示例，以便将这个理念具体化：

- ① 使用构造函数创建一个 promise

- ② 有条件地将其 `resolve`（兑现）

- ③ 否则将其 `reject`（拒绝）

```javascript
const shouldResolve = true;
const p = new Promise((resolve, reject) => { // ①
  if (shouldResolve) {
    resolve('I transitioned successfully'); // ②
  } else {
    reject(new Error('I failed to resolve')); // ③
  }
});
console.log(p);
```

我们引入某个条件变量，作为模拟现实场景中 promise 可能成功兑现或失败兑现的机制。为了先走“顺利路径”，我们将条件变量设置为 `true`。然后，我们使用构造函数创建一个 promise，通过调用回调处理函数的第一个参数（恰当地命名为 `resolve`），有条件地“兑现”并附带一条消息。如果在浏览器控制台中运行这段代码，会看到以下输出：

```
Promise { <state>: "fulfilled", <value>: "I transitioned successfully" }
```

由于我们的条件变量允许 promise 被“兑现”，因此该 promise 现在处于“已兑现”状态，并报告了我们的成功消息。

如果我们尝试运行相同的代码片段，但将条件变量设置为 `false`，我们会注意到该 promise 被“拒绝”了。

```
Promise { <state>: "rejected" }
```

Promise 构造函数提供了一种机制，用于包装任何执行异步任务的现有函数或操作。许多遗留 API 都期望一个回调函数；然而，如果我们希望在任何地方都使用 promise（我们也应该这样做），那么可以轻松地对使用者隐藏这一点，如下所示：

- ① 将`XMLHttpRequest` 调用包装在一个函数中

- ② 如果请求成功，则用 `XMLHttpRequest` 对象的响应来 `resolve` 该 promise

- ③ 否则，适当地包装响应并将其 `reject`

```javascript
const asyncRequest = (method, url) => // ①
  new Promise((resolve, reject) => {
    const xhr = new XMLHttpRequest();
    xhr.open(method, url, true);
    xhr.onload = () => resolve(xhr.response); // ②
    xhr.onerror = () => reject(new Error(xhr.statusText)); // ③
    xhr.send();
  });
const req1 = asyncRequest('GET', 'https://my-json-server.typicode.com/typicode/demo/posts');
```

我们尝试（我必须承认，这种方式相当幼稚^(⁶²)）使用 promise API 来包装基于 `XMLHttpRequest` 的请求。现在，任何对 `asyncRequest` 的调用都会简单地返回一个 promise，也就是说，在这个例子中 `req1` 是一个 `Promise` 对象。

正如人们可能推断的那样，`promise` 就像一个状态机。一旦创建，`promise` 就处于“待定”状态，最终会“转换”到“已兑现”（如果被 `resolve`）或“已拒绝”（如果被 `reject`）状态，从而使 `promise` 的状态变为“已敲定”。一旦敲定，一个 `promise` **不能** 恢复到其先前的任何状态，也不能再转换到任何其他状态。

## 接下来介绍 Promise API！

### 使用 Promise

Promises/A+ 规范提到了一个 `thenable` 对象，它是一个行为类似 `promise` 的对象，并且恰好有一个 `then` 方法。ES6 的 `promise` 符合 Promises/A+ 规范，并相应地公开了一个 `then` 方法，该方法允许客户端访问一个已兑现 `promise` 的*最终*结果，或者其被拒绝的原因。

任何相关方都可以*在任何时候*（甚至在 `promise` 转换之后）通过向 `then` 方法附加一个处理函数来表达对 `promise` 值的兴趣。如果 `promise` 成功兑现，这个回调函数将被调用。

`then` 方法也可以接受第二个可选参数，即错误回调函数；如果 `promise` 被拒绝，这个回调函数将被调用。然而，这被认为是一种[反模式](https://www.datchley.name/promise-patterns-anti-patterns/)^(⁶³)。为了捕获 `promise` 链中的错误，`promise` 对象还公开了一个 `catch` 方法，该方法将捕获所有被拒绝的情况。`finally` 是 `promise` 公开的另一个方法，它允许我们在 `promise` 敲定后进行任何清理工作；换句话说，无论 `promise` 是被兑现还是被拒绝，提供给 `finally` 的处理函数都会被调用。

`then`、`catch` 和 `finally` 都保证返回 `promise` 对象。也就是说，我们不需要将这些回调的返回值包装在 `promise` 中——我们可以简单地返回任何值或引用，底层机制会自动为我们将该返回值包装在一个 `promise` 中。

因此，按照惯例，`resolve` 和 `then` 方法*应该*分别捕获并 `return` 值，允许下游消费者继续链式调用，而 `reject` 则应该使用 `Error` 对象来完成，这会捕获堆栈跟踪，从而大大简化调试工作。

这就引出了一个问题——`catch` 的返回值应该是什么？回想一下，`then` 和 `catch` 都返回 `promise`——所以如果有一种方法可以从错误中恢复并允许 `promise` 链继续执行，那么 `catch` 处理函数可以返回一个值，同时允许产生副作用（例如记录错误日志）！这样，下游消费者就可以保持对错误的无知，并且可以继续链式调用，确信他们一定会得到一个值（尽管可能是某种默认值）。

`finally` 稍微有些特殊——它不接受任何参数，仅仅是一种机制，允许在 `promise` 兑现后进行任何所需的清理。无论 `promise` 是被兑现还是被拒绝，`finally` 块**都**会被调用，很像你可能在其他语言中习惯使用的任何 `try-catch-finally` 块。

我们来做个快速演示。我们将基于我们 `promise` 化的 `XMLHttpRequest` 示例进行构建，为结果注册适当的监听器，并捕获任何错误。

- ① 使用 `then` 方法附加多个处理函数
- ② 确保使用 `catch` 捕获任何错误

```javascript
const req1 = asyncRequest('GET', 'https://my-json-server.typicode.com/typicode/demo/posts');
req1
  .then(json => JSON.parse(json)) // ①
  .then(resp => resp.map(item => item.id))
  .then((ids) => {
    console.log(`Found ${ids.length} posts`);
    return ids;
  })
  .catch(err => console.log(err)); // ②
```

Promise API 除了具有流畅的接口之外，还提供了一种非常优雅的机制，用于将*启动*异步操作与该操作结果的消费解耦。不再需要嵌套的回调函数；取而代之的是，多个 `then` 允许我们（更好地）使代码与我们思考和推理程序的方式保持一致。此外，由于链式调用能正确传播错误，我们可以安全地捕获 `promise` 链中可能发生的*任何*错误。

### 全有或全无

有些场景下，我们希望并行启动大量（异步）操作，要么将它们视为一个 "整体"，关心所有已决议的值，要么只关心其中一个。这与我们尝试序列化操作不同，后者会使用 `Promise` 上的 `then` 方法。

Promise API 提供了两个类级别的方法，即 `all` 和 `race`，旨在满足此需求。

假设有一个场景，你需要从多个文件（磁盘上）读取数据，或从一系列不同的端点收集信息，并且只有在所有请求完成后，才整理结果。我们可以使用 `Promise.all` 来启动多个并发请求，并等待它们全部完成，如下所示：

*   ① 开始查找某个用户的所有文章
*   ② 同样地，查找该用户的所有评论
*   ③ 使用 `all` 并行启动多个操作
*   ④ 接收一个包含所有已决议值的数组，顺序与传入的 `Promise` 顺序相同

```javascript
const p = asyncRequest('GET', 'https://my-json-server.typicode.com/typicode/demo/posts'); // ①
const c = asyncRequest('GET', 'https://my-json-server.typicode.com/typicode/demo/comments'); // ②
Promise.all([p, c]) // ③
.then(([posts, comments]) => ({ // ④
  posts: JSON.parse(posts),
  comments: JSON.parse(comments),
}))
.then(obj => console.log(obj))
.catch(err => console.log(err));
```

我们关注的是获取某个特定用户的所有文章和评论，这是两个独立的请求。`Promise.all` 接收一个 `Promise` 数组，然后返回一个 `Promise`。当且仅当传递给 `all` 的所有 `Promise` 都成功决议时，它会将所有兑现的值收集到一个数组中，按原始 `Promise` 的顺序排序，然后调用我们的 `then` 处理器。

然而，如果一个或多个 `Promise` 失败（由于拒绝或错误），那么 `all` 返回的 `Promise` 也会被拒绝。

此外，还有 `Promise.race`。顾名思义，给定一个 `Promise` 数组，第一个成功决议的 `Promise` 赢得比赛（或者第一个被拒绝的），而 "获胜" `Promise` 的决议值就是提供给处理器的唯一参数（从而丢弃之后任何其他 `Promise` 的决议结果）。

`Promise.race` 的一个有趣用例是当我们希望为异步操作设置超时，如下所示：

*   ① 开始查找某个用户的所有文章
*   ② 将 `setTimeout` 包装在一个 `Promise` 中，该 `Promise` 在设定时间间隔后被拒绝
*   ③ 使用 `race` 并行启动多个操作
*   ④ 如果我们的请求在超时前完成，则接收文章数组
*   ⑤ 否则，捕获拒绝，为后续消费者组合一个默认返回值

```javascript
const p = asyncRequest('GET', 'https://my-json-server.typicode.com/typicode/demo/posts'); // ①
const timer = new Promise((_, reject) => setTimeout(reject, 1000)); // ②
Promise.race([p, timer]) // ③
.then(posts => ({ // ④
  posts: JSON.parse(posts),
}))
.catch(() => ({ // ⑤
  posts: [],
}))
.then(obj => console.log(obj));
```

我们在这里利用了多种技术——（简单地）将像 `setTimeout` 这样的异步操作包装在 `Promise` 中，将我们的实际请求与一个会在超时时被拒绝的 `Promise` 进行 `race`，并从 `catch` 方法返回默认值，以便后续消费者可以继续链式调用。^(⁶⁴) 如果你想看看如果在 Ajax 请求被决议之前发生了超时，`race` 会如何表现，只需将超时限制降低，例如 `ten`。

最后，Promise API 还提供了两个额外的（静态）方法，即 `Promise.resolve` 和 `Promise.reject`，它们作为方便的快捷方式，尤其在测试时。

第一个限制是 Promise 是热切的，这意味着一旦我们使用 Promise 启动一个操作，这项工作`将会`被执行，无论是否有感兴趣的消费者。也就是说，Promise 会尝试执行其分配的任务，而不会等待客户端附加一个 `then` 处理器。

假设有一个场景，你尝试（通过 Ajax 调用）解析一些需要在小部件中显示的数据，然后再将小部件显示在屏幕上。现在，假设某个用户交互导致不再需要这个小部件。由于 Promise 是热切的，无论是否有小部件来消费这些数据，获取数据的过程都会发生。`如果` Promise 能等到小部件附加了 `then` 处理器再执行，那么我们的担忧就会消失。但可惜！

除了 Promise 是热切的（或`不`懒惰）之外，它们也是不可取消的。也就是说，Promise 对象没有提供 API 来取消其包装的底层进程。考虑一个搜索界面，它试图在我们输入查询时刷新显示的结果（很像 Google 或 Netflix）。这要求我们每输入一个字符，就向后端发起一个 Ajax 调用。然而，很可能我们根据搜索输入框中的字符启动了一个后端调用，而根据用户输入的速度，在前一个查询完成之前，我们可能不得不启动另一个查询。如果能在开始新查询之前简单地取消之前的查询就好了。

所有这些都归结为一个简单的经验法则——除非你确定会使用这个 Promise，否则不要创建它！

## 原生 API 的优势

Promise API 现在已成为语言的一部分，这比使用库带来了`巨大`的优势：我们现在有了一个标准 API，它可以统一浏览器厂商、库和框架包装异步操作的方式。例如，`window.fetch`^(⁶⁵) 现在可以返回一个 Promise，因为语言现在原生支持 Promise。其一个副作用是，我们现在有了一个统一的词汇来处理所有异步操作，这使我们这些开发者能够开发出一套更丰富的模式和反模式，这些模式都围绕着一个核心结构，即 Promise。相应地，其他语言结构，如我们很快会讨论的 `async`/`await`，也可以安全地使用 Promise。

更重要的是，在 JavaScript 中引入 Promise API 与我们看到的其他特性（如 iterables）是一致的，这些特性试图为库/框架作者和消费者提供一个共同的基础，同时又不偏离既有的社区标准（如 Promises/A+ 规范）。

展望未来，作为 JavaScript 开发者，如果我们都围绕（原生）Promise，无论是在我们的代码中，还是在我们对所使用的库和框架的期望中，我们都会受益。当然，就算没有其他好处，我们或许最终也能摆脱对又一个第三方库的依赖。:)

# 总结

`Promise` 作为 `JavaScript` 中的原生 `API` 提供，相较于使用第三方库具有显著优势，它允许所有异步操作（无论原生的还是其他的）都返回一个 `Promise`。诚然，`Promise` 本身也有其问题，但与我们之前所处的世界（处理异步操作时语义不一致）相比，它已是一个巨大的飞跃。

在下一章中，我们将探讨一个备受期待的语言特性，即在 `JavaScript` 中声明类以构建继承树的能力。

脚注 1 2 3 4 5 6 7 8
