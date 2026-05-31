# 9. 使用生成器与迭代器的可迭代序列

在 JavaScript 中存储和操作集合数据不仅容易，而且优雅。然而，迭代则是另一回事。以数组为例，为了遍历并获取数组元素，我们使用诸如 `for (let i = 0; i < array.length; i++)` 循环这样矫揉造作的语法，甚至更糟的是 `for-in` 循环。但这还不是全部！我们时常会遇到需要迭代的自定义对象。我们只能认输，然后构建（或使用）一个自定义 API（比如 `array.forEach`），仅仅为了能够按照特定顺序获取每个元素。

那些日子已经一去不复返了。

ES6 引入了一种新的迭代机制，即 `for-of` 循环，它使集合的迭代变得简单且一致。ES6 还引入了迭代器的概念，允许任何对象通过 `for-of` 循环进行迭代。最后，我们得到了一种新型函数——生成器，它允许我们借助一些甜美的语法糖来生成可迭代对象。

在本章中，我们将探讨 `for-of` 循环的优雅之处及其优点，并探索它与迭代器之间的共生关系。我们将了解如何让自定义对象变得可迭代，从而使其能够与 `for-of` 循环一起使用。

到本章结束时，我们将理解 `Symbol.iterator` 的必要性和用法，并了解它如何使集合和对象变得可迭代。我们还将看到如何使用生成器创建有限长度和无限长度的可迭代对象，以及如何编写惰性求值代码。

## 缺乏迭代协议的弊端

JavaScript 在 ES5 之前只提供了两种数据结构：对象和数组。对象主要用作键值存储，数组则作为索引集合。ES6 又为我们引入了另外两种：映射和集合。JavaScript 提供了一些通过循环或迭代来消费这些数据结构元素的机制；然而，它缺少许多其他现代语言提供的便利功能，尤其是标准化的消费 API。

以数组为例。数组提供了几种迭代方式——命令式的 `for(const i = 0; i < array.length)` 版本、数组原型上的 `forEach` 方法，以及 `for-in` 循环。让我们逐一简要分析。

传统的 `for` 循环虽然应用广泛，并且可能是大多数 JavaScript 程序员最熟悉的，但它既冗长又容易出错。正如 Venkat Subramaniam 常说的（我同意）：“我永远记不清是 `<` 还是 `<=`！”（感谢 IDE 的自动补全功能，对吧？）然而，它的确允许我们控制要迭代数组的多少部分，甚至可以在需要时 `break` 退出循环。

另一方面，数组提供的 `forEach` 方法并非语言本身的语法结构，而是 `Array` 对象上的一个方法，因此无法用于其他索引集合类型。`forEach` 循环旨在遍历整个数组长度，且无法“中断”循环（除非我们抛出一个 `Error`，但这显然不是一个好办法）。

最后是 `for-in` 循环，其作用是遍历对象的可枚举属性。将 `for-in` 循环与数组一起使用会产生相当令人意外的结果：

- ① 定义一个单元素数组
- ② 查看键的类型
- ③ 使用该键获取实际的索引值

```
const arr = ['a']; ①
for (const k in arr) {
console.assert(k === '0'); ②
console.log(arr[k]); ③
}
```

我们得到的“键”不是一个数字（正如人们可能期望的索引类型），而是一个字符串。此外，由于循环只提供键，我们被迫使用下标或方括号表示法来获取数组中该位置的元素。

当然，这些都无法解决当前的实际问题，即 JavaScript 本质上缺乏让任何集合遵守可迭代契约的能力。在很多场景下我们会得到集合项——比如 `document.querySelectorAll` 返回一个 `NodeList`，或者 `Immutable.js`^((55)) 包含一个 `Immutable.List` 对象。目前我们还没有一个规范的 API 来遍历这些集合中的所有项。

谈到迭代，JavaScript 另一个明显的缺失是缺乏编写惰性求值代码的能力，从而无法生成无限序列。

假设我们在编写一个数学库，并且希望编写一个生成斐波那契数列的函数。基于我们在 ES5 中已有的知识以及到目前为止对 ES6 的探索，我们的第一次尝试可能如下所示：

*斐波那契数列生成器的朴素实现*

- ① 定义一个期望一个参数 `n` 的函数

```
const fibonacci = (n) => { ①
const result = [];
let start = 0;
let next = 1;
switch (n) {
case 0:
break;
case 1:
result.push(start);
break;
case 2:
result.push(start);
result.push(next);
break;
default:
result.push(start);
result.push(next);
for (let i = 2; i < n; i++) {
const val = start + next;
start = next;
next = val;
result.push(val);
}
}
return result;
};
// 打印 []
console.log(fibonacci(0));
// 打印 [ 0 ]
console.log(fibonacci(1));
// 打印 [ 0, 1 ]
console.log(fibonacci(2));
// 打印 [ 0, 1, 1, 2, 3, 5, 8, 13, 21, 34 ]
console.log(fibonacci(10));
```

我们编写了一个简单的函数，它接受一个参数 `n`，用于告知需要生成序列中的多少个数字，然后使用一个相当简单的算法生成所需的斐波那契数列，每次将项推入结果数组。我承认这是一个相当朴素的实现，但它足以服务于我们的讨论，其核心围绕着一个相当微妙的问题：函数的参数。

像 Haskell 这样的语言天生就是惰性的——换句话说，Haskell 中的代码除非绝对必要，否则永远不会被求值。像 Clojure 以及现在的 Java（通过 `Stream` API）这样的语言，为我们提供了惰性的数据结构——实际上允许我们处理惰性计算的集合，进而处理无限集合。

回到当前的讨论，这里的问题是，任何使用我们库的客户端代码都必须*事先*知道他们需要多少个斐波那契数列数字。^((56)) 这将对函数和库的责任转移到了用户身上！它没有提供优雅的机制，让客户端在运行时决定是否需要更多数字，或者只是在遍历序列时停止消费。

然而，如果我们*能够*在客户端要求时*才*惰性求值斐波那契数列的“下一个”项，那么我们就无需强制要求斐波那契数列生成器带有一个参数。相反，我们只需返回一个数据结构（甚至可能是一个数组），然后耐心等待他们请求序列中的下一个项，进行计算并返回！

惰性求值和序列提供了诸多好处，从使代码（可能）更高效（我们永远不会计算超出需要的部分），到简化 API（无需事先询问“需要多少个”），同时为一系列算法问题提供了非常优雅的解决方案。



## for-of 循环

ES6 引入了一种全新的循环机制，即 `for-of` 循环，其立竿见影的好处可通过以下步骤轻松展示：

- ① 定义一个数组
- ② 使用 `for-of` 循环
- ③ 打印数组中的每一项

```
const arr = ['a', 'b', 'c']; ①
for (const k of arr) { ②
// 依次打印 'a', 'b', 'c'
console.log(k); ③
}
```

让我们暂停片刻，欣赏一下 `for-of` 循环的精妙之处。命令式带索引的 `for` 循环不见了，也无需再使用 `for-in` 循环。剩下的只有代码的真实意图——遍历数组以按顺序获取每个*元素*！

这之所以行得通，是因为 `for-of` 循环可以遍历任何碰巧是“可迭代”的对象，而数组在 ES6 中恰好是可迭代的。也就是说，它们提供了一套规范的迭代 API，使得 `for-of` 循环能够按正确顺序获取数组中的每个元素。

不仅如此——映射（Map）、集合（Set）和字符串也都是可迭代的，因此我们可以对它们全部使用 `for-of` 循环：

- ① 考虑到每次迭代会返回一个键值对，我们可以使用数组解构

```
const array = [10, 20, 30, 40, 50];
for (const i of array) {
console.log(i);
}
const set = new Set(['a', 'set', true, false]);
for (const i of set) {
console.log(i);
}
const map = new Map([
['s', 'is a string'],
[true, 'is a boolean'],
]);
for (const [k, v] of map) { ①
console.log(`${k}->${v}`);
}
const string = 'ES6 is awesome!';
for (const i of string) {
console.log(i);
}
```

事实证明，任何对象或集合只要符合可迭代协议，就能参与 `for-of` 循环。让我们来看看在 JavaScript 中“可迭代”意味着什么。

## 可迭代对象与迭代器

像 Java 这样的静态类型语言为我们提供了名为 `Iterator` 的接口。接口是一种契约，要求对象公开特定的 API，并且这种契约由运行时强制实施。在 Java 中，任何“实现” `Iterator` 接口的对象实质上都是向外界声明它可被迭代。类似地，像 Ruby 这样的动态类型语言也有契约的*概念*，本质上是通过约定实现的。

无论语言类型是静态还是动态，它们都提供了一种将对象标记为可迭代的方式，使得消费者能够以标准方式访问该对象的各个元素，而无需对象暴露其任何内部实现细节。此外，由于访问元素是通过某种契约进行的，语言本身可以提供额外的结构（如 Java 增强的 `for` 循环），利用相同的机制进行循环。

通常，为了成为可迭代对象，该对象（或集合）**必须**实现一个声明自身可迭代的 API（例如在 Java 中，对象必须实现 `Iterator` 接口），从而能够回答两个问题：

- 我还有更多元素吗？
- 如果有，那个元素是什么？

JavaScript 在 ES6 中宣布了对对象和集合实现可迭代的类似支持。然而，JavaScript 作为一种解释型语言，并没有运行时强制实施契约，而是通过约定来施加行为。这个约定如下——任何对象要成为可迭代对象，它必须具有一个名称为 `Symbol.iterator`（另一个众所周知的符号）的方法。该方法的返回值**必须**是一个具有零参数（即无参数）`next` 方法的对象，而该方法又返回一个包含两个属性的对象：

- `done`——一个布尔值，用于回答是否还有更多元素可供迭代
- `value`——当前项

我们可以通过一个简单的示例让事情更清晰：

- ① 定义一个简单对象
- ② 实现 `Symbol.iterator` 方法
- ③ 迭代器方法返回可迭代对象
- ④ 可迭代对象实现 `next` 方法
- ⑤ 如果迭代结束，将 `done` 标记为 `true`
- ⑥ 如果迭代未结束，返回 `value` 值
- ⑦ 获取迭代器的引用
- ⑧ 开始迭代

```
const arrayList = { ①
data: [1, 2, 3, 4, 5],
[Symbol.iterator]() { ②
let index = 0;
return { ③
next: () => { ④
if (index === this.data.length) {
return {
done: true, ⑤
};
}
const ret = this.data[index];
index++;
return {
done: false,
value: ret,
}; ⑥
},
};
},
};
const iterator = arrayList[Symbol.iterator](); ⑦
// 打印 '{ done: false, value: 1 }'
console.log(iterator.next()); ⑧
```

我们在这里尝试实现一个可迭代的数组列表。由于要遵守可迭代协议，我们实现了一个名为 `Symbol.iterator` 的方法。当调用这个迭代器方法时，它会返回一个新对象，由该对象为我们执行迭代。

请注意，对 `next` 的任何调用返回值都是一个对象，该对象适当地设置了 `done` 标志，并且如果需要，还包含了 `value`。

既然我们已经了解了实现的大致样子，接下来让我们梳理一下各个活动部件。JavaScript 中的迭代由两个（或三个，取决于你怎么看）不同的部分组成：

- “可迭代”的对象，即具有 `Symbol.iterator` 方法的对象
- 调用迭代器方法时*返回*的对象，即“迭代器”，它本身具有 `next` 方法

最后，还有 `next` 调用的结果，我们可以将其视为迭代器契约的一部分，或者简单地称为迭代器结果。大功告成！我们的 `arrayList` 成功实现了使其可迭代所需的所有部分。现在，我们可以像使用数组一样，使用 `for-of` 循环来遍历它。请看：

```
// 使用前面示例中的 arrayList 实现
for (const v of arrayList) {
// 依次打印 1, 2, 3, 4, 5
console.log(v);
}
```

这一切归结为：JavaScript 现在拥有了一套规范化的 API，任何客户端都可以安全地使用它来遍历我们的对象！

### 清理工作

我们知道迭代器需要实现 `next` 方法。当该方法到达可迭代对象的末尾时，它可以在通过 `{ done: true }` 发出结束信号之前，执行任何必要的清理工作。

但是，如果消费者提前停止迭代会发生什么？实际上，迭代器可以（可选地）实现一个 `return` 方法，当消费者终止迭代时将调用该方法。考虑以下情况：

- ① 实现必需的 `next` 方法和可选的 `return` 方法
- ② 在 `for-of` 循环中提前退出

```
const incrementor = {
[Symbol.iterator]() {
let start = 0;
return {
next() {
return {
value: start++,
done: false,
};
},
return: function () { ①
console.log('正在清理...');
return {
done: true,
};
},
};
},
};
for (const v of incrementor) {
if (v > 10) {
break; ②
}
console.log(v);
}
```

这里我们有一个简单的递增可迭代对象，它同时实现了 `next` 和 `return` 方法。我们使用新朋友 `for-of` 循环开始遍历该对象，并有条件地 `break` 跳出循环。如果运行这个示例，我们将在控制台中看到清理消息。请注意，我们返回了 `done: true` 来表示迭代器现在已完成。

这种机制是 `for-of` 循环了解迭代器机制的一个副作用——此外，不仅是 `break`，在 `for-of` 循环*内部*抛出错误（`throw`）以及从 `for-of` 循环中返回（`return`）都会调用迭代器的 `return` 方法。

如果我们显式地调用 `next` 方法，那么当迭代完成后，我们需要调用 `return`。



### 其他优势

采用标准的可迭代方法，使得 JavaScript 能够在 API 之上构建额外的结构，同时允许其他库设计自己的对象以利用相同的机制。在第 4 章中，我们讨论了`spread`运算符，并了解到任何可迭代的对象也都是可展开的。重申一下，ES6 中引入的扩展运算符与`for-of`循环使用了完全相同的机制。

此外，展望未来，任何对象——包括我们可能在应用程序中引入的对象、诸如`Immutable.js`以及`NodeList`（由`document.querySelectorAll`返回的集合）等库所提供的集合——都可以安全且正确地被循环遍历和“展开”！^(⁵⁷)

最后，这一点较为微妙，我们引入了一种实现代码惰性求值的机制！可迭代对象为消费者返回一个迭代器，并静待迭代器的`next`方法被调用。当`next`方法被调用时（如果有必要），对象可以计算序列中的下一个项并将其返回。更进一步，我们的可迭代对象也可以是无限的（当然，在溢出的范围内）。

在本节中，我们讨论了可迭代对象和迭代协议。接下来，我们将探讨“可暂停”函数及其一些用例。

## 生成器

生成器函数是 ES6 中的新增功能，引入了新的语法和关键字，以及函数的新的语义。让我们先看一个简单的示例，然后继续了解其工作原理：

*定义简单的生成器*

```
function* generatorDemo() {
yield 10;
console.log('第一次');
yield 20;
console.log('第二次');
}
const generator = generatorDemo(); // 控制台无输出
console.assert(generator !== undefined);
// 每行对应控制台输出
console.log(generator.next());
// { value: 10, done: false }
console.log(generator.next());
// 第一次
// { value: 20, done: false }
console.log(generator.next());
// 第二次
// { value: undefined, done: true }
// 或者，我们可以直接使用 for-of 循环
for (const i of generatorDemo()) {
console.log(i);
}
```

让我们聚焦于这里的语法层面——我们接触到了新的`function*`（注意：*并非*笔误），以及一个新的`yield`关键字。

当我们调用生成器函数时，看到两件有趣的事情发生：控制台中没有输出（如果是普通函数，我们预期会看到`console.log`语句打印出消息），并且我们从返回值中*得到了一些东西*！

你看，当生成器函数被调用时，会立即返回一个生成器对象，**而不会**执行其函数体。此外，如果我们观察示例中的后续代码行，会发现返回的对象上有一个`next`方法，而该方法又返回一个包含`value`和`done`键的对象。换句话说，生成器对象是可迭代的！

`yield`的作用是什么？如果我们仔细查看示例代码的输出，会发现每次在迭代器上调用`next`时，生成器函数体仅执行到遇到第一个`yield`语句为止，此时，“yield”出的任何值都会返回给调用代码（包装在迭代器结果对象中）。实际上，`yield`语句*暂停*了生成器函数的执行，将控制权返回给调用上下文。

一个恰当的类比是（两位玩家之间的）乒乓球或网球。一旦我们开始比赛（即我们手中有了生成器对象），调用`next`会将控制权发送到生成器函数的“球场”。当遇到`yield`时，生成器会通过一个值将控制权返回给调用者。作为“消费者”，我们可以选择不再调用`next`来停止比赛，此时生成器函数（如果尚未“完成”，则保持暂停状态）；如果生成器到达其函数体定义的末尾，比赛将结束（此时返回的值为`{done: true}`）。

本质上，生成器函数是一个返回可迭代对象的函数，而该可迭代对象又实现了迭代器协议。生成器函数会暂停（使用`yield`），并在调用者调用`next`时恢复执行。

让我们将之前的斐波那契数列示例转换为使用生成器函数：

```
function* fibonacci() {
let start = 0;
let next = 1;
yield start;
yield next;
while (true) {
const result = start + next;
start = next;
next = result;
yield result;
}
}
const f = fibonacci();
console.log(f.next().value); // 0
console.log(f.next().value); // 1
console.log(f.next().value); // 1
console.log(f.next().value); // 2
console.log(f.next().value); // 3
console.log(f.next().value); // 5
console.log(f.next().value); // 8
```

这个实现与我们之前使用`Symbol.iterator`的实现差别不大，只是我们不再需要实现那个方法——生成器函数为我们完成了所有工作。

### 作为消费者的生成器

到目前为止，我们对生成器的探索一直将其视为数据的“生产者”——它们在每次迭代中产生值。尽管名称如此，生成器也可以充当“消费者”，即我们可以将值“推入”其中，也许作为种子，或作为一种向它们发送数据以供其使用的方式。

让我们修改简单的示例，看看这种机制如何运作：

*   ① 通过`next`启动迭代

*   ② 将一个值作为参数推入`next`方法，从而设置`first`的值

*   ③ 再次执行相同操作，不过这次设置`second`的值

```
function* generatorWithPushDemo() {
const first = yield 10;
console.log('第一次', first);
const second = yield 20;
console.log('第二次', second);
}
const generator = generatorWithPushDemo(); // 控制台无输出
console.assert(generator !== undefined);
// 每行对应控制台输出
console.log(generator.next()); ①
// { value: 10, done: false }
console.log(generator.next('传入一个值')); ②
// 第一次 传入一个值
// { value: 20, done: false }
console.log(generator.next('传入另一个值')); ③
// 第二次 传入另一个值
// { value: undefined, done: true }
```

我们从获取生成器对象的引用开始，然后调用`next`。控制权转入生成器，它`yield`出一个值返回给我们。此时，生成器处于暂停状态。

有趣的地方来了——我们再次调用`next`，不过这次提供了一个参数。这个值会被*赋值*给`first`变量，生成器现在可以在其实现中使用这个值。一种理解方式是：我们传入生成器的任何值实际上都会替换生成器中的`yield`。当然，如果生成器内部没有捕获`yield`语句，则向生成器传入值的结果是空操作——传入的值会被简单忽略。




### 清理操作

自定义迭代器可以实现一个 `return` 方法，该方法可用于在消费者提前终止迭代（使用 `break`、`throw` 或 `return`）时进行清理。事实证明，生成器函数返回的生成器对象也能做到这一点。

但是，等等，你可能会说，我们并没有实现 `next` 方法！如果调用者调用了 `return`，我们这些生成器函数的作者该如何进行清理呢？

实际上，JavaScript 已经提供了一种清理机制——那就是 `try-catch-finally` 块！其工作原理如下：如果我们的生成器实现包含一个 `try` 块，并且调用者在生成的生成器上调用了 `return`，那么 JavaScript 会为我们执行生成器函数内的 `finally` 块：

- ① 将生成器的主体包裹在一个 `try` 块内
- ② 在生成器上调用 `return` 方法
- ③ 使用原生的 `for-of` 循环驱动生成器直至完成
- ④ 使用 `throw` 方法使生成器报错并停止

```
function* generatorFunctionWithTryCatchFinally() {
  try { ①
    console.log('Started');
    yield 'some value';
  } catch (error) {
    console.log('Caught: ' + error);
  } finally {
    console.log('Finally block');
  }
}

const callee = generatorFunctionWithTryCatchFinally();
console.log(callee.next());
// Started
// { value: 'some value', done: false }

console.log(callee.return()); ②
// Finally block
// { value: undefined, done: true }

console.log('----------');

for (const looped of generatorFunctionWithTryCatchFinally()) { ③
  console.log(looped);
  // Started
  // some value
  // Finally block
}

console.log('----------');

const c = generatorFunctionWithTryCatchFinally();
console.log(c.next());
// Started
// { value: 'some value', done: false }

console.log(c.throw(new Error('Stop now!'))); ④
// Caught: Error: Stop now!
// Finally block
// { value: undefined, done: true }
```

我们编写了一个简单的生成器，其主体包裹在 `try-catch-finally` 块中。我们获取生成器对象的引用，并对其调用 `return`。该示例列举了我们在控制台中看到的输出，我们看到此时 `finally` 块为我们执行了。

接下来，我们使用 `for-of` 循环来获取生成器的引用并驱动其完成。同样，如果我们观察输出，会发现在最后调用了生成器函数的 `finally` 块。换句话说，`for-of` 循环会自动调用生成器的 `return` 方法，就像我们在自定义迭代器中看到的那样。

最后，生成器对象还实现了另一种方法，即 `throw`。回到乒乓球的比喻，不是将球（一个值）发送到生成器中，而是向它抛出一个错误。类似于发送一个值（使用 `next(someValue)`）会替换生成器*内部*的 `yield` 语句，`throw` 一个错误会导致生成器在其暂停的位置抛出该错误。如果该行周围没有 `try-catch` 块，那么生成器函数就会简单地在该行报告错误。然而，在我们的示例中，我们*确实*有一个 `try-catch`，因此，`catch` 块会被适当地执行，随后是 `finally` 块。

正如我们在本例中所看到的，生成器函数以及由此产生的生成器对象，为我们提供了与迭代器相同的能力，保持了自定义迭代器和自动生成的迭代器之间的对称性。唯一的例外是生成器对象还实现了 `throw` 方法，而其他可迭代对象则没有。

### 其他优点

生成器允许我们编写惰性求值的代码，并且能够生成无限序列，这两点都在我们的斐波那契数列生成器中得到了体现。撇开生成器的所有机制不谈，我们必须始终记住，它们产生的是迭代器。我们编写的许多函数倾向于返回序列（如数组），但通常我们必须预先填充数组，无论客户端需要用它做什么。生成器为客户主动请求元素提供了一种机制，从而潜在地优化了需要完成的工作量。

我们必须始终牢记，生成器函数具有挂起和恢复的概念，非常适用于任何涉及控制权从一个上下文转移到另一个上下文再返回的场景。一个明显的候选是异步操作，其中控制权在主执行上下文和回调之间来回切换。

### 额外的语法考量

考虑这样一种场景：一个生成器函数（我们称之为“外部”生成器）在其实现中调用另一个生成器函数（称为“内部”生成器）。鉴于我们的意图是消费外部和内部生成器函数所有 `yield` 产生的值，我们需要一种额外的机制来挂起外部生成器，遍历内部生成器产生的所有值，然后将控制权返回给外部生成器，从而恢复它。这就是新关键字 `yield*` 发挥作用的地方。考虑以下情况：

- ① 我们所谓的内部生成器函数
- ② 我们的主生成器或“外部”生成器函数
- ③ 使用 `yield*` 表达式来调用内部生成器并驱动其完成

```
function* inner() { ①
  yield 'b';
  yield 'c';
  yield 'd';
}

function* outer() { ②
  yield 'a';
  console.log('after the first outer yield');
  yield* inner(); ③
  console.log('after finishing the inner yield*');
  yield 'e';
}

for (const v of outer()) {
  console.log(v);
}
// a
// after the first outer yield
// b
// c
// d
// after finishing the inner yield*
// e
```

我们从两个生成器函数开始，外部生成器使用 `yield*` 将工作委托给内部生成器。从输出中我们看到，外部生成器首先产生一个值，然后在一次 `next` 调用（通过 `for 循环`）时，将控制权移交给内部生成器，并在其完成时（如果完成的话）恢复外部生成器。

关于 `yield*` 的一个微妙之处在于，它接收的是调用内部生成器（通过 `inner()`）所得到的*结果*，**而不是**对该生成器的引用（即 `inner`）。换句话说，它接收到的是一个可以对其进行循环的生成器对象。事实证明，`yield*` 可以接收*任何*可迭代对象——生成器对象、数组、映射、集合、字符串，甚至是我们手工制作的自定义可迭代对象。不过请记住，虽然*所有*这些可迭代对象都实现了 `next` 和 `return` 方法，但只有生成器对象实现了 `throw` 方法。

