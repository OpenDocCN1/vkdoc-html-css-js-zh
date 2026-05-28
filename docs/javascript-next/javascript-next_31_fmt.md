# 15. 异步迭代器和生成器——思想的碰撞

生成器的概念很诱人，而且往往会让我们变得挑剔。我们不再害怕处理大型或无限序列以及数据流。然而，尽管生成器在处理值方面表现出色，但它们从来就不是为处理 promises 而设计的。

在本章中，我们将看到另一种融合——它将生成器的语法与 `async` 和 `await` 结合起来，为我们带来了异步生成器和迭代器。

## 生成器/迭代器与异步操作之间的阻抗不匹配

ES6 为我们引入了可迭代对象、众所周知的符号 `Symbol.iterator`（它使对象可迭代），以及新的 `for-of` 循环（它知道如何迭代可迭代对象）。迭代器与其消费者之间的交互协议，恰好是一个采用 `{ value: ..., done: ...}` 形式的对象。如果迭代器尚未 `done`，则消费者可以使用 `value`。一旦迭代器的返回值将 `done` 设置为 `true`，迭代就会停止。

ES6 还引入了另一个特性：生成器函数。生成器函数是可以使用 `yield`（和 `yield*`）运算符暂停的函数。它们返回恰好是可迭代的生成器对象，这意味着它们有一个响应 `Symbol.iterator` 的方法。换句话说，我们可以使用 `for-of` 循环来遍历生成器对象。

如果我们在同步操作的结果上进行操作，那么无论我们采用何种机制来生成或消费可迭代对象，它们都能很好地工作。但是，如果这些值恰好是异步操作的结果（或 Promise），那么事情就会变得有些棘手。请看：

- ① 一个用于创建随机超时值的辅助函数
- ② 返回一个 Promise，它会解析为一个值和一个索引
- ③ 一个包含 Promise 的可迭代对象
- ④ 我们需要等待 Promise 被解析

```javascript
const randomTimeout = async (val, index) => {
const timeout = Math.floor(Math.random() * Math.floor(42)); ①
return new Promise(res =>
setTimeout(res, timeout, `My value is ${val} at index: ${index}`)); ②
};
const promiseList = [
randomTimeout(10, 1),
randomTimeout(20, 2),
randomTimeout(30, 3),
]; ③
// 输出如下（你的顺序可能不同）
// My value is 20 at index: 2
// My value is 10 at index: 1
// My value is 30 at index: 3
for (const p of promiseList) {
p.then(console.log); ④
}
```

我们从一个简单的辅助函数开始，它接受一个值（用于解析）和一个索引，这样我们就可以看到调用它的顺序。该函数实现生成一个随机超时值，并利用它返回一个最终解析为某个值的 Promise。然后，我们使用这个辅助函数生成一个异步操作数组。

暂时让我们忽略我们正在使用一个 Promise 数组这一事实，并假设我们处理的可迭代对象长度未知，就像使用 I/O 读取文件中的所有行一样。毕竟，如果我们有一个（固定长度的）Promise 列表，我们可以简单地使用 `Promise.all` 让它们并发运行，并收集结果。

考虑到这一点，请注意，虽然 `for-of` 循环成功地遍历了 Promise 列表，但我们仍然需要等待每个 Promise 被解析。然而，敏锐的观察者会注意到，这并不能满足我们的需求。虽然 `for-of` 循环按顺序尽职尽责地返回 Promise，但它们在不同时间点解析。因此，我们看到的输出与 Promise 的初始顺序不一致。这是因为 `for-of` 循环不知道迭代器产生的值是异步的。

但是等等。ES8 为我们引入了 `async`/`await`。我们不能在这里使用它吗？当然可以！

- ① 在继续循环之前，`await` 返回的 Promise

```javascript
const promiseList = [
randomTimeout(10, 1),
randomTimeout(20, 2),
randomTimeout(30, 3),
];
(async () => {
for (const p of promiseList) {
const res = await p; ①
console.log(res);
}
})();
```

这次，我们使用 `await` 来暂停 `for-of` 循环，直到我们的 Promise 解析完成。`await` 会暂停 `for-of` 的执行，直到每个 Promise 依次解析完成，从而给出我们期望的输出。我们付出了一点样板代码的代价，因为我们必须将对 `await` 的调用包装在一个匿名的 `async` 函数中，但它确实有效。

现在让我们考虑生产端。我们知道生成器函数会生成生成器对象，这些对象是可迭代的。同样，如果 `yield` 出的值是同步的，这可以完成任务。但是 `yield` 异步数据源会产生相当令人惊讶的结果：

- ① 定义一个生成器
- ② `yield` Promise
- ③ 开始消费

```javascript
function* asyncGenerator() { ①
yield randomTimeout(10, 1); ②
console.log('第一次');
yield randomTimeout(20, 2);
console.log('第二次');
}
const generator = asyncGenerator(); ③
// 打印 '{ value: Promise {  }, done: false }'
console.log(generator.next());
// 打印 '{ value: Promise {  }, done: false }'
console.log(generator.next());
// 打印 '{ value: undefined, done: true }'
console.log(generator.next());
for (const i of asyncGenerator()) {
console.log(i);
}
(async () => {
for (const p of asyncGenerator()) {
const res = await p;
console.log(res);
}
})();
```

我们定义了一个简单的生成器，它 `yield` 出 Promise，并尝试通过调用 `next`、使用传统的 `for-of` 循环，以及最终使用 `async`/`await` 来获取值。注意，在每种情况下，我们都会得到预期的 `{ value: ..., done: ... }` 元组，只不过在这个例子中，`value` 恰好是一个 Promise。

这个例子揭示了真正的问题所在——也就是说，当迭代器将结果交给我们时，它试图告诉我们 `value` 是什么以及它是否 `done`。在这种情况下，我们看到 `value` 恰好是一个 Promise，它可能已被解析也可能尚未被解析——然而，生成器会继续 `yield` 值，直到它完成为止！

换句话说，虽然 `value` 的解析是异步的，但判断生成器是否 `done` 的计算*是*同步的，因此是不正确的。在 `value` 被解析（或拒绝）之前，我们**不能**将迭代器声明为 `done`。

这一切都雪上加霜——也就是说，迭代一个可迭代对象的全部意义在于*按顺序*获取值！

总结一下，在消费端，我们希望 `for-of` 循环按顺序给我们期望的值，在继续下一次迭代之前暂停，等待异步数据源被解析。

在生成端，我们希望在进行 `yield` 之前先 `await`。但可惜的是！我们只能*在* `async` 函数内部使用 `await`。

ES9 为我们引入了一个新的符号、一个异步的 `for-of` 循环，以及 `async` 生成器的概念，使我们能够迭代和生成异步可迭代对象。让我们开始吧。

## 异步迭代器

我们已了解 `Symbol.iterator` 的作用及其在 `for-of` 循环中的运作方式。ES9 引入了一个新符号 `Symbol.asyncIterator`，旨在用于执行异步操作的可迭代对象。任何希望实现异步迭代协议的对象，其约定与 `Symbol.iterator` 类似：调用时必须返回一个包含 `next` 方法的对象。

但在具体实现上，它与 `Symbol.iterator` 有所不同。`Symbol.asyncIterator` 返回的不是包含 `done` 和 `value` 键的对象，而是返回一个 Promise，该 Promise 解析为这样的对象。或许通过代码比文字解释更容易理解。

- ① 一个模拟后端调用的简单函数
- ② 相应地 `return` 带有随机超时的 Promise
- ③ 一个简单的类
- ④ 实现 `Symbol.asyncIterator`

### 资源清理

- ⑤ `Symbol.asyncIterator` 返回一个简单对象，即迭代器
- ⑥ 迭代器具有 `next` 方法
- ⑦ 如果有值需要返回，则将 `value` 包装在 `Promise` 中返回
- ⑧ 否则，如果迭代完成，则返回包含 `done: true` 的 `Promise`

```javascript
const fetchFriends = (index) => { ①
  const friends = [
    'Matt',
    'Neal',
    'Ken',
  ];
  const timeout = Math.floor(Math.random() * Math.floor(42));
  if (index < friends.length) {
    return new Promise(res => setTimeout(res, timeout, friends[index])); ②
  }
  return Promise.reject(new Error('No more records'));
};

class FriendsService { ③
  [Symbol.asyncIterator]() { ④
    let index = 0;
    return { ⑤
      next: async () => { ⑥
        // 调用后端异步请求以获取当前值
        try {
          const friend = await fetchFriends(index);
          index += 1;
          return { value: friend, done: false }; ⑦
        } catch (e) {
          index = 0;
          return { done: true }; ⑧
        }
      },
    };
  }
}
```

首先，我们定义一个模拟数据库游标，它使用索引从好友列表中检索好友。为了模拟真实场景，我们通过一个生成随机超时值的函数来模拟不同的检索时间。

接着，我们创建一个简单的类，作为异步调用的服务封装。为了符合异步可迭代协议，它实现了 `Symbol.asyncIterator` 方法，该方法返回迭代器。这个迭代器具有 `next` 方法——但它不直接返回值，而是将值包装在 `Promise` 中返回。

实现 `Symbol.asyncIterator` 与 `Symbol.iterator` 的区别在于，前者中 `value` 和 `done` 标志都是异步解析的。

`ES9` 还引入了另一种 `for-of` 循环，即 `for-await-of` 循环，它知道如何遍历异步可迭代对象。与 `for-of` 循环不同，这种循环在进入下一次迭代之前，会 `await` `next` 调用返回的 `Promise`。顾名思义，此循环使用了 `await` 关键字，而我们知道，使用 `await` 时必须在 `async` 函数内。让我们看看如何将其与我们的可迭代对象一起使用：

- ① 使用 `for-await-of` 循环

```javascript
(async () => {
  for await (const friend of new FriendsService()) { ①
    console.log(friend);
  }
})();
```

再次强调，我们需要一些样板代码来将调用包装在匿名 `async` 函数中。但如果运行此代码，我们会发现好友按照请求顺序依次显示。`for-await-of` 循环与其同步版本类似，通过调用 `Symbol.asyncIterator` 来获取可迭代对象的迭代器，然后依次调用 `next` 方法，`await` 返回的 `Promise` 被解析，最后提取 `value`。请注意，如果 `Promise` 解析为 `done: true`，迭代就会停止。

与同步迭代器一样，我们也可以选择自行调用 `next` 方法；但由于 `next` 调用返回的是 `Promise`，我们需要附加一个 `then` 调用。

异步可迭代对象可以在其 `next` 方法中，在通过 `{ done: true }` 发出迭代结束信号之前，执行任何资源清理。由于迭代器自身知道何时到达终点，因此这种方式效果良好。

但是，如果 `for-await-of` 循环提前终止迭代会发生什么？与同步可迭代对象一样，异步可迭代对象也可以（可选地）实现 `return` 方法。当客户端终止迭代时，该方法会被调用，示例如下：

- ① 同时实现必需的 `next` 方法和可选的 `return` 方法
- ② 在 `for-of` 循环中提前跳出

```javascript
class SimpleIterable {
  [Symbol.asyncIterator]() {
    let counter = 0;
    return {
      next: async () => {
        if (counter < 3) {
          counter++;
          return { value: counter, done: false };
        }
        return { done: true };
      },
      return: async () => {
        console.log('Cleanup called!');
        return { done: true };
      }, // ①
    };
  }
}

(async () => {
  try {
    for await (const n of new SimpleIterable()) { ①
      console.log(n);
      return; ②
      // 或者 break，或者 throw new Error()
    }
  } catch (e) {
    console.log('Error', e);
  }
})();
```

我们将之前的示例简化，改为 `return` 一个解析为递增值的 `Promise`。但我们也实现了 `return` 方法，该示例中只是简单打印日志以确认被调用。请注意，我们应该在 `return` 方法的返回值中表明可迭代对象已完成（`done`）。

这里我们再次发现 `for-await-of` 循环与其同伴 `for-of` 循环的相似性：调用可迭代对象的 `return` 方法是底层机制的副作用。也就是说，如果我们提前终止迭代，`for-await-of` 循环会自动调用可迭代对象的 `return` 方法。其他会触发自动调用 `return` 方法的机制包括：在 `for-await-of` 循环中的 `throw` 错误，以及简单地使用 `break` 退出循环。

### 命令式风格

`for-await-of` 循环名副其实，它使异步代码看起来像命令式代码。当出现问题时，我们使用的处理机制进一步凸显了这一特点。如果正在遍历的异步可迭代对象被 `reject` 了怎么办？很简单。我们可以将 `for-await-of` 循环包裹在传统的 `try-catch` 块中。请看：

- ① 拒绝 `Promise`
- ② 使用传统 `try-catch` 块捕获错误

```javascript
class SimpleRejectingIterable {
  [Symbol.asyncIterator]() {
    return {
      next: () => Promise.reject('Error!'), ①
    };
  }
}

(async () => {
  try {
    for await (const n of new SimpleRejectingIterable()) {
      // 我们永远不会执行到这里
      console.log(n);
    }
  } catch (e) {
    console.log('Error', e); ②
  }
})();
```

此示例中的迭代器直接 `reject` 了 `Promise`。如果运行此代码，我们会看到 `await` 解包了错误，并正确地传播错误，使得 `catch` 块能够捕获它。瞧！

### 与同步可迭代对象混合使用

事实证明，`for-await-of` 循环可以同时处理异步和同步迭代器。这非常有用，因为我们甚至可以用它来处理普通的可迭代对象，如数组、`Map` 和 `Set`。让我们重新审视使用 `for-await-of` 遍历 `Promise` 列表的例子：

- ① 一个 `Promise` 列表
- ② 对同步可迭代对象使用 `for-await-of` 循环

```javascript
const randomTimeout = async (val, index) => {
  const timeout = Math.floor(Math.random() * Math.floor(42));
  return new Promise(res =>
    setTimeout(res, timeout, `My value is ${val} at index: ${index}`));
};

const promiseList = [
  randomTimeout(10, 1),
  randomTimeout(20, 2),
  randomTimeout(30, 3),
]; ①

(async () => {
  for await (const p of promiseList) { ②
    console.log(p);
  }
})();
```

请注意，数组是一个同步可迭代对象。然而，正如所宣称的那样，`for-await-of` 循环可以遍历它。如果每次迭代返回的值恰好是 Promise，循环会暂停，直到该值被解析、解包，然后交给你。

## 生成器

我们对 ES6 中引入的生成器已非常熟悉。正如前面讨论的，ES6 生成器是同步的。然而，在处理异步操作时，我们希望在 `yield` 之前能够 `await`——除非我们身处 `async` 函数中，否则无法 `await`。

ES9 引入了将 `async` 关键字应用于生成器函数的能力，实际上就是引入了异步生成器。我们可以在生成器内部 `await` 异步操作（回想一下，我们只在 `async` 函数内部使用 `await`），就像在常规生成器中一样，一旦解析完成，就 `yield` 这些值：

- ① 定义一个 `async` 生成器
- ② `await` 一个异步操作
- ③ 使用 `for-await-of` 循环遍历生成器迭代器

```javascript
const randomTimeout = async (val, index) => {
  const timeout = Math.floor(Math.random() * Math.floor(42));
  return new Promise(res =>
    setTimeout(res, timeout, `My value is ${val} at index: ${index}`));
};
async function* asyncGenerator() { ①
  yield await randomTimeout(10, 1); ②
  console.log('First time');
  yield await randomTimeout(20, 2);
  console.log('Second time');
}
console.log(typeof asyncGenerator()[Symbol.asyncIterator] === 'function');
(async () => {
  const gen = asyncGenerator();
  for await (const item of gen) { ③
    console.log('item', item);
  }
})();
```

异步生成器，与常规生成器函数非常相似，会产生一个迭代器对象，不同的是返回的生成器对象实现了 `Symbol.asyncIterator` 方法（与同步生成器产生的同步可迭代对象相比）。因此，我们不能对生成的迭代对象使用 `for-of` 循环。但是，我们可以使用 `for-await-of` 循环，因为它确实适用于异步可迭代对象。

异步生成器的机制与同步生成器并无不同。它们都是允许我们创建可迭代对象的机制，支持惰性代码，从而可以创建无限序列。区别在于它们返回的内容——异步迭代器返回给我们实现了 `Symbol.asyncIterator` 方法的可迭代对象，允许使用 `for-await-of` 循环来消费它们。

## 与同步迭代器和生成器的对等性

我们详细讨论了同步迭代器和生成器，特别是迭代器实现 `return()` 方法的能力，如果我们提前终止迭代，该方法将被调用。我们还看到生成器可以作为“消费者”，我们可以通过向 `next()` 调用传递参数来影响生成器的下一个 `yield`。所有这些同样适用于异步生成器和迭代器。最后，回想一下 `async` 和 `await` 的目的是让异步操作看起来像是命令式的。这使我们能够将异步生成器中的代码包装在 `try-catch` 或 `try-catch-finally` 块中，以捕获在生成器执行期间可能被拒绝的任何 promise。

## 优势

异步迭代器和生成器在我们希望消费（惰性地或以其他方式）异步操作的结果时非常有用，同时还能享受 `async`/`await` 提供的看似命令式代码的好处。一个很好的例子是逐行读取大文件（例如使用 Node.js）。另一个用例是消费返回给我们的数据是分页的 API。考虑 GitHub 的 API，请求仓库的提交记录时——GitHub（默认情况下）每次响应返回 100 条提交，并在响应头中提供指向接下来 100 条提交的链接。消费这样的 API 很自然地适合异步生成器提供的机制，我们在其中 `await` 每一个 `fetch()` 并进行处理，如果需要，再继续处理接下来的 100 条提交。