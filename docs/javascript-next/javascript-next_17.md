# 14. 看似命令式的 `async` 与 `await`

在处理异步操作时，Promise 比使用回调函数向前迈进了一大步。然而，Promise 为我们提供了一个特定的领域特定语言（DSL）来处理，包括但不限于 `then`、`catch` 和 `finally`。这种 DSL 虽然使用起来流畅且简单，但它与我们通常编写 JavaScript 代码的方式（即命令式代码）并不完全契合。以错误捕获为例——JavaScript 提供了像 `try`/`catch` 这样的语言结构来捕获错误并提供有意义的堆栈跟踪。而 Promise 则通过 `catch` 和 `finally` API 来捕获错误。这种与传统代码编写模式的背离，会在我们在代码库的不同部分之间切换时引入心智负担。

在本章中，我们将了解 `async` 和 `await` 如何帮助我们重构所有代码，使我们能够像处理同步代码一样处理异步代码。我们将能够更清晰地表达意图，不会迷失在回调的迷宫中，并使用我们熟悉的概念来处理状态管理和错误处理。在本章结束时，我们将流下喜悦的泪水，为我们代码的优美、简洁，以及最重要的一致性而欢欣鼓舞。

## `Promise` DSL 的不足

在 JavaScript 中，当尝试执行任何长时间运行的任务时，异步编程是我们唯一的选择。Promise 作为 ES6 的原生 API，通过简化状态管理和错误传播，试图驯服回调函数带来的复杂性。Promise 本质上是回调的封装，虽然确实让代码更容易理解，但同时也带来了一系列妥协。

然而，JavaScript 中命令式（或阻塞式）代码与异步代码的编写方式截然不同。请看下面这段尝试以命令式执行相当简单操作的代码片段：

- ① 命令式函数
- ② 对结果执行一些简单操作

```js
const firstSyncOp = () => 10; ①
const secondSyncOp = arg => arg + 20;
const thirdSyncOp = arg => arg + 30;
const firstSyncOpResult = firstSyncOp(); ②
const secondSyncOpResult = secondSyncOp(firstSyncOpResult);
const result = thirdSyncOp(secondSyncOpResult);
// 输出 '使用同步操作 60'
console.log('Using synchronous operations', result);
```

虽然这个例子略显刻意，但它很好地服务于我们的用例。我们从几个同步或阻塞函数开始，然后对各个结果执行一些操作，这里是指简单的算术运算。这个例子除了有些冗长外，理解起来相当简单——我们可以把每个函数的结果当作普通值一样对待，等待使用它们，然后计算最终结果。

接下来，我们将使用 Promise 把阻塞操作转换为非阻塞或异步操作，并尝试获得同样的结果。

- ① 一系列异步操作
- ② Promise 链

```js
const firstAsyncOp = () => new Promise(res => setTimeout(res, 10, 10)); ①
const secondAsyncOp = arg => new Promise(res => setTimeout(res, 10, arg + 20));
const thirdAsyncOp = arg => Promise.resolve(arg + 30);
const result = firstAsyncOp()
.then(secondAsyncOp) ②
.then(thirdAsyncOp);
result.then((r) => {
console.log('Using promises', r);
return r;
});
```

这次，我们假设操作数是异步获取的——前两个在十分之一秒的延迟后获取，而第三个则立即解析。但由于每个操作数都被包裹在 Promise 中，我们必须使用 `then` API 来链式调用这些操作，最终得到与前一个例子相同的结果。

如果比较这两种尝试，虽然最终结果相同，但两种方法看起来却大相径庭。虽然 Promise 减少了回调带来的仪式性和冗长性，但在使我们能以处理同步操作的方式来处理异步操作方面，它们几乎毫无建树。两者毫无相似之处！而这只是冰山一角。

我们习惯于将中间操作的结果保存在变量中，这些变量又可用于条件判断或 `for` 循环等循环结构。请看下面这个对比命令式代码和 Promise 链中使用条件判断的示例：

- ① 命令式条件判断
- ② 我们被迫在处理器内部进行条件判断
- ③ 我们还需要处理条件不满足的情况

```js
let syncConditionalResult = firstSyncOp();
if (syncConditionalResult > 20) { ①
syncConditionalResult = secondSyncOp(syncConditionalResult);
} else {
syncConditionalResult = thirdSyncOp(syncConditionalResult);
}
// 输出 '命令式条件判断结果: 40'
console.log(`Imperative conditional result: ${syncConditionalResult}`);
const asyncConditionalResult = firstAsyncOp()
.then((val) => {
if (val > 20) { ②
return secondAsyncOp(val);
}
return val; ③
})
.then(thirdAsyncOp);
asyncConditionalResult.then((r) => {
// 输出 '链式条件判断结果: 40'
console.log(`Chained conditional result: ${r}`);
return r;
});
```

使用命令式方式编写代码时，很容易理解其逻辑——如果为真则执行这个，否则执行那个。然而，当使用 Promise 链时，事情就变得棘手了——条件判断被嵌入了处理器内部，我们还必须处理条件判断失败的情况。

问题的核心在于，处理异步代码时，我们被迫采用一种与同步代码不同的编程模型。我们无法以一种*看起来*是命令式的方式来处理异步代码，因此，我们失去了利用语言提供的绝大多数设施的能力。

于是 `async` 和 `await` 应运而生。ES8 引入了这两个新关键字，并提供了相关机制，以减少 JavaScript 中同步与异步编程模型之间的差异。

## `async/await`

`async`，在 ES8 中引入，旨在用作函数修饰符。`async`函数向外界声明该函数执行的工作可能是异步的。由于我们在 ES6 中表示异步操作的标记是 Promise，因此前一句可以归结为：`async`函数**总是**返回一个 Promise，如下所示：

- ① 一个返回值的异步函数表达式
- ② 一个显式返回 Promise 的异步函数

```js
const implicitPromise = async () =>
  ['Implicitly', 'wrapped', 'in', 'a', 'promise']; ①
implicitPromise().then(console.log);
const explicitPromise = async () =>
  Promise.resolve(['This', 'just', 'returns', 'the', 'promise']); ②
explicitPromise().then(console.log);
```

`async`尝试通过保证返回一个 Promise 来减少样板代码——对于仅仅`return`一个值的函数，`async`修饰符会自动将该返回值包装在 Promise 中。另一方面，如果函数已经返回了一个 Promise，那么它就直接返回。

ES8 同时引入了`await`关键字，它**只能**在`async`函数内部使用。`await`的作用是修改一个恰好返回 Promise 的函数的调用（与修改函数**定义**的`async`关键字相反）。由于`await`应用于返回 Promise 的函数（这些函数可能是其他`async`函数），因此`await`的作用是等待被返回的 Promise 状态稳定，然后继续解包该值。也许下面的示例会帮助你理解：

- ① 一个返回 Promise 的简单函数
- ② 定义一个匿名的`async`箭头函数
- ③ 在`async`函数内部使用`await`解析为一个值

```js
const firstAsyncOp = () => new Promise(res => setTimeout(res, 10, 10)); ①
(async () => { ②
  const result = await firstAsyncOp(); ③
  // 输出 '10'
  console.log(result);
  return result;
})();
```

我们从一个异步执行工作并因此返回 Promise 的函数开始。然后在`async`函数内部使用`await`关键字来“等待”被返回的 Promise 状态稳定，并打印其结果。

这里的关键好处是，使用`await`使我们无需向`then`提供回调，而由于`firstAsyncOp`的结果是一个 Promise，我们本需要这样做。从概念上讲，`await`会暂停它所在的`async`函数，直到它等待的函数产生结果。听起来熟悉吗？这与我们讨论过的生成器函数和`yield`关键字是相同的机制！

注意`await`关键字的使用如何将一个或多个异步调用转换为命令式或阻塞式代码。让我们使用`async/await`重写之前的示例，以便比较这两种方法：

- ① 定义一个匿名的`async`箭头函数
- ② 在`async`函数内部使用`await`解析第一个操作的值
- ③ 然后继续使用`await`调用后续操作，提供顺序调用的概念
- ④ 最终返回结果
- ⑤ 由于`async`操作总是返回 Promise，我们使用`then`来消费已解析的值

```javascript
const result = (async () => { ①
  const firstAsyncOpResult = await firstAsyncOp(); ②
  const secondAsyncOpResult = await secondAsyncOp(firstAsyncOpResult); ③
  const res = await thirdAsyncOp(secondAsyncOpResult);
  return res; ④
})();
result.then((r) => { ⑤
  console.log('Using async/await', r);
  return r;
});
```

我们从第一个操作开始，挂起执行直到 Promise 解析，然后将该值作为参数传递给后续函数调用，最终返回完全计算出的值。然而，之前使用 Promise 时，我们被迫使用`then`来排序操作。`await`消除了所有这些繁琐的步骤——事实上，这个实现看起来与我们的同步示例没有太大区别。这个示例与同步实现不同的地方在于，如果我们想使用来自立即执行函数表达式的返回值。回想一下，`async`函数的返回值总是被包装在 Promise 中——因此，如果我们想使用`result`，我们就必须使用`then` API。

利用条件语句和循环结构与预期一样有效。让我们回到之前使用条件语句的示例，只不过这次使用`async/await`：

- ① 一个命令式的条件检查

```javascript
const result = (async () => {
  let firstAsyncOpResult = await firstAsyncOp();
  if (firstAsyncOpResult > 20) { ①
    firstAsyncOpResult = await secondAsyncOp(firstAsyncOpResult);
  } else {
    firstAsyncOpResult = await thirdAsyncOp(firstAsyncOpResult);
  }
  return firstAsyncOpResult;
})();
result.then((r) => {
  // 输出 'async/await conditional result: 40'
  console.log(`async/await conditional result: ${r}`);
  return r;
});
```

`firstAsyncOpResult`是*异步*操作的结果这一事实，丝毫不会影响我们的代码。去掉`async/await`关键字，我们发现这正是我们处理同步操作结果的方式。

让我们检查一个更实际的用例来突出`async`和`await`的用途。在这个示例中，我们将尝试使用 Fetch API 进行一个 Ajax 调用，以`GET`一个资源并解包响应。请记住，`fetch`返回一个 Promise，该 Promise 产生一个`Response`对象，而该对象又拥有`json`方法，该方法也返回一个 Promise。我们将编写两个实现——一个使用原生 Promise，一个使用`async`和`await`：

- ① 使用`fetch`暴露的原生 API
- ② 定义一个`async`函数
- ③ 等待 fetch 操作完成
- ④ 然后等待 JSON 负载被解析
- ⑤ 调用我们基于 Promise 的实现
- ⑥ 调用我们基于`async/await`的实现
- ⑦ 使用`async/await`消费`fetch`操作的结果

```javascript
const getWithPromises = url => fetch(url).then(resp => resp.json()); ①
const getWithAsyncAwait = async (url) => { ②
  const resp = await fetch(url); ③
  return await resp.json(); ④
};
const url = 'https://my-json-server.typicode.com/typicode/demo/comments';
getWithPromises(url) ⑤
  .then((json) => {
    console.log(json);
    return json;
  });
getWithAsyncAwait(url) ⑥
  .then((json) => {
    console.log(json);
    return json;
  });
// 或者
(async () => {
  // 两种方式效果相同
  // const json = await getWithPromises(url);
  const json = await getWithAsyncAwait(url); ⑦
  console.log(json);
})();
```

这个示例突出了两个微妙但不同的问题。当尝试使用一个返回 Promise 的 API（如`fetch`）时，使用`async/await`可以使实现更清晰、更易于理解。另一方面，我们如何实现获取功能，对我们如何消费它没有任何影响。鉴于两种情况下我们都总是返回一个 Promise，我们可以使用传统的`then`调用，或者使用另一个（匿名的）`async`函数。也就是说，我们可以开始考虑在任何使用 Promise 链的地方使用`async/await`。

如果 Promise 被拒绝了呢？好吧，`async/await`的目的是让异步代码看起来像是命令式的。因此，我们像通常一样捕获错误（和拒绝）——使用我们钟爱的`try/catch`。观察：

- ① 一系列异步操作
- ② 这个操作被拒绝
- ③ 将我们的代码包裹在 try-catch 中
- ④ 像捕获同步错误一样捕获错误

```javascript
const firstAsyncOp = () =>
  new Promise(res => setTimeout(res, 10, 10));
const secondAsyncOp = arg =>
  new Promise(res => setTimeout(res, 10, arg + 20));
const thirdAsyncOp = () =>
  Promise.reject('Oops!');
const result = (async () => {
  try {
    const firstAsyncOpResult = await firstAsyncOp();
    const secondAsyncOpResult = await secondAsyncOp(firstAsyncOpResult);
    const res = await thirdAsyncOp(secondAsyncOpResult);
    return res;
  } catch (e) {
    console.error(e);
    return 0;
  }
})();
result.then((r) => {
  // 输出 '0'
  console.log('Using async/await', r);
  return r;
});
```

我们的示例与之前的尝试差别不大，唯一区别在于这次有一个 Promise 被拒绝（rejected）。为了应对此类情况，我们可以将代码包裹在传统的 `try`/`catch` 块中，就像处理阻塞代码一样捕获错误。在我们的示例中，IIFE 最终会返回一个结果，因此我们简单地返回 `0`（或任何适合你业务场景的默认值）。

正如我们可以推断的那样，`await` 执行两个任务：它暂停 `async` 函数的执行，直到其等待的 promise 落定（settled），然后解包 promise resolve 的值，使 `async` 函数继续执行。如果 promise 被拒绝，`await` 会适当地抛出一个 `Error`，我们可以用传统的 `try`/`catch` 块来捕获。两者的组合实质上将使用 promise 时的编程范式从函数式风格转变为命令式风格。我们来讨论更多细节，特别是关于 `await` 的细节，这将有助于阐明其最佳使用场景。

## 细节与注意事项

首先，很容易忘记应用 `await` 关键字。这不会造成语法错误——但请记住，我们只能对返回 promise 的操作使用 `await`。如果我们忘记应用 `await` 关键字，那么最终会得到返回的 promise 的引用，这并非我们想要的结果。

相反，我们常常会过度使用 `await`，而实际上直接使用 promise API 会更好。回顾一下，`await` 会暂停 `async` 函数的执行，这在需要先解决某个 promise 才能进行后续操作时非常适用。然而，在可以并发运行操作的情况下，更好的做法是降级使用 promise API，例如 `all` 或 `race`，然后使用 `await` 来消费结果。观察以下示例：

- ① 以命令式风格依次等待每个操作

- ② 简单的辅助函数

- ③ 并发运行多个操作

```javascript
const redundantWaits = (async () => {
  const url = 'https://my-json-server.typicode.com/typicode/demo';
  const fetchPosts = await fetch(`${url}/posts`);
  const posts = await fetchPosts.json();
  const fetchComments = await fetch(`${url}/comments`);
  const comments = await fetchComments.json();
  return {
    posts,
    comments,
  };
})();
redundantWaits
  .then(obj => console.log('From redundantWaits', obj));

const synchronousCalls = (async () => {
  const getJson = url => fetch(url).then(resp => resp.json());
  const url = 'https://my-json-server.typicode.com/typicode/demo';
  const [posts, comments] = await Promise.all([
    getJson(`${url}/posts`),
    getJson(`${url}/comments`),
  ]);
  return {
    posts,
    comments,
  };
})();
synchronousCalls
  .then(obj => console.log('From synchronousCalls', obj));
```

这次我们尝试完成同样的任务，即用多个 Ajax 调用的响应来填充一个对象。但在这个例子中，这些操作之间没有依赖关系。这是一个典型的场景，我们应该使用 promise API 来并发运行这些任务。当然，就像其他返回 promise 的函数一样，我们总可以使用 `await` 来收集 `Promise.all` 的结果，并回到命令式模式。

另一个潜在的 bug 来源是，如果我们的 `async` 函数依赖于某种全局状态。^((101)) `await` 会暂停函数的执行；在不阻塞主线程的情况下，运行时可以继续工作。因此，谨慎起见，我们应该假设全局状态在函数被挂起前后已经发生了变化。

坏消息就说到这里。从积极方面看，`await` 可以消费任何“thenable”对象——因此如果我们使用某种第三方库来支持 promise，`async` 可以像处理原生 promise 一样处理它们。放心大胆地去用 `await` 吧！

# 总结

`async`/`await` 与 promises 一起，让我们能够以最适合用例和编程风格的范式来编写 JavaScript 代码——前者更偏向命令式，而后者则更倡导函数式思维。虽然命令式风格对许多人来说可能更自然，但我们必须记住，promises 提供了诸如 `all` 和 `race` 之类的机制，这些机制可能使我们的代码运行得更快，同时也能与 `await` 配合使用，从而允许我们根据需要为手头的问题提升或降低抽象层次。promises 的使用仍然有其价值，但 `await` 确实有助于减少或消除使用 promises 所带来的一些负担。

在下一章中，我们将看到另一个扩展，其中迭代器和生成器可以像处理同步操作一样轻松地处理异步操作。

脚注 1   2   3   4   5
