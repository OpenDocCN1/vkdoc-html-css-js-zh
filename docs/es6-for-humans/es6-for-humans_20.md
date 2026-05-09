# `catch()` 方法与 Promise 链式调用

`catch()` 方法在 Promise 组合中用于错误处理非常有用。与 `then()` 方法类似，它也会返回一个 Promise，但只处理被拒绝的情况。`catch()` 方法相当于 `then(null, onRejected)` 的简写形式。我们将在本章后面进一步学习如何处理错误。

## Promise 的链式调用

由于 `.then()` 和 `.catch()` 始终返回一个新的 Promise，因此可以轻松地链式调用 Promise，同时高度控制错误处理的方式和位置。链式调用允许将异步操作串联起来，确保它们按照正确且预期的顺序执行，从而使代码看起来几乎是同步的。请看以下示例：

```
const bond = new Promise((resolve, reject) => {
  resolve("Bond");
});
bond.then((str) => `${str}, James ${str}`)
  .then((str) => `Hello, I'm ${str}!`)
  .then((str) => console.log(str));
// Hello, I'm Bond, James Bond!
```

上面是一个简单的示例，展示了链式调用如何帮助按顺序执行不同任务并获得最终结果。让我们看看下面这段更实际的代码片段，它演示了链式调用在处理异步任务时的优势：

```
getPaymentFromUser
  .then(displayTransactionDetails)
  .then(queueTransactionEmail)
  .then(redirectToOrdersPage)
  .catch(logError)
```

这里，`getPaymentFromUser` 返回一个 Promise，链中的每个函数都会在前一个处理器完成后，被调用并传入其返回值。在这个例子中，`displayTransactionDetails()` 会等待 `getPaymentFromUser()` 完成后再开始；`queueTransactionEmail()` 会等待 `displayTransactionDetails()` 完成后再开始，依此类推。需要注意的是，`logError()` 仅在之前的任意一个 Promise 被拒绝时才会执行。这种方式将调用串行化，而不会阻塞主执行线程，并确保这些操作按顺序执行。

#### 错误处理

在本章前面，我们讨论了在消费 Promise 时如何处理错误，可以通过以下任意一种方式：

```
somePromise().then(onResolved, onRejected);
// 或者直接使用 catch()
somePromise()
  .then(onResolved)
  .catch(onRejected);
```

但你应该更倾向于哪一种方式，为什么呢？这个问题的答案取决于你的 `onResolved()` 函数是否会抛出错误。`then()` 方法返回的 Promise 可能会被拒绝，但如果使用第一种方式，这个拒绝无法被捕获——导致错误在你的应用中被忽略。而使用第二种方式，无论是来自 `somePromise()` 还是 `onResolved()` 的错误，都可以在 `catch()` 方法中得到处理。如果我们有多个 `then()` 调用，错误会一直传递下去，直到遇到错误处理器。因此，建议在所有 Promise 链的末尾都加上一个 `catch()` 方法。

## 使用 `Promise.all` 组合 Promise

`Promise.all` 接收一个 Promise 数组（或任何可迭代对象），并返回一个 Promise。当可迭代参数中的所有 Promise 都 resolve 时，该返回的 Promise 会 resolve；如果任何一个 Promise 拒绝，它就会以第一个拒绝的 Promise 的原因 reject。

如果传入的任意一个 Promise 被拒绝，那么所有 Promise 会立即以该拒绝的 Promise 的值 reject，并丢弃所有其他 Promise，无论它们是否已 resolve。注意，如果传入一个空数组，该方法会立即 resolve。

```
const p1 = Promise.resolve(3);
const p2 = 42;
const p3 = new Promise((resolve, reject) => {
  setTimeout(resolve, 100, "foo");
});
Promise.all([p1, p2, p3]).then(values => {
  console.log(values); // [3, 42, "foo"]
});
```

然而，在某些情况下，你可能不想等待数组中的所有 Promise 都 resolve，而只是想获取数组中第一个完成的 Promise 的结果。在这种情况下，可以使用 `Promise.race()`。

`Promise.race()` 也接收一个 Promise 数组，但与 `Promise.all()` 不同，它会在数组中第一个 Promise 完成时立即兑现其返回的 Promise。例如，假设我们想从 API 端点获取一些 JSON 数据，但不想无限等待响应。在这种情况下，我们只想使用一个默认值。我们可以使用 `Promise.race()` 来实现这一点：

```
// 一个在给定时间（t）后超时的 Promise
function delay(t) {
  return new Promise((resolve, reject) => {
    setTimeout(resolve, t);
  });
}
// 哪个 Promise 先完成，其结果就会被传递到我们的处理器中
Promise.race([
    fetchData(),
    delay(5000).then(() => { data: "test" })
  ])
  .then((res) => {
    // 如果 fetchData() 耗时超过 5 秒，这里将是 "test"
    console.log("data:", res.data);
  })
  .catch(function(err) {
    console.log("error:", err);
  });
```

在这个例子中，`delay()` 函数返回一个在 `t` 毫秒后 resolve 的新 Promise。然后我们为该返回的 Promise 附加一个处理器，返回默认的用户对象 `{user: "guest"}`。这确保了如果在 5 秒内从服务器获取到用户数据，我们将获得用户详情；否则将使用一个访客用户。

## 总结

Promise 已成为处理大量异步操作的标准方法。在本章中，我们学习了如何正确使用 Promise 来编写易于阅读的代码，这使得它们比传统回调函数更容易调试。它们允许我们组合异步 API，并让我们用真正的 Promise 封装不符合规范的 Promise API 或回调 API。与回调和事件不同，Promise 帮助我们避免竞态条件，并确保其表示的值的不可变性。Promise 让我们能够以同步风格编写异步代码，拥有平坦的缩进和单一的异常处理通道。

