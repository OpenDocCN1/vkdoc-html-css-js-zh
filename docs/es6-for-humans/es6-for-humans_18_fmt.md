# 8. 迭代器和生成器

在本章中，我们将全面学习新引入的迭代器和生成器，它们为 JavaScript 的遍历机制带来了简洁性和可定制性。许多编程语言已经摒弃了繁琐的陈旧的 `for` 循环，这种循环在遍历集合中的项目时需要额外跟踪当前索引位置。让我们看看如何利用迭代器和生成器在我们的应用程序中高效地处理数据。

## 可迭代对象和迭代器

在最新发布的 ECMAScript 标准中，我们投入了大量精力来改进代码的组织方式。ES6 引入了一套全新的遍历机制，使得实现可迭代对象和迭代器变得非常简单和容易。这一版本的 JavaScript 引入了一组称为迭代器协议和可迭代对象协议的协议，任何 JavaScript 对象都应实现这些协议才能成为迭代器或可迭代对象。在继续讨论这些协议之前，让我们先了解一下 JavaScript 中的可迭代对象和迭代器：

简而言之，迭代器模式是一种结构化的模式，用于按特定顺序逐个从元素集合中获取信息。它是一个具有特定接口、专为迭代设计的对象。这种模式一直存在，并通过自定义对象在 JavaScript 中使用，但通过 ES6，我们迎来了迭代器模式的隐性标准化。JavaScript 中的许多现有数据结构现在都公开了迭代器，你也可以使用相同的标准构建自己的迭代器，从而获得最大的互操作性。

可迭代对象是一系列可以迭代的元素的简单表示。它没有任何迭代状态，例如“当前元素”。相反，它有一个生成迭代器的方法。

迭代器是带有迭代状态的对象。所有迭代器对象都有一个 `next()` 方法，用于返回集合中的下一个对象。`next()` 方法返回的对象具有两个属性，即：

(i) `value`，表示集合中的下一个值；

(ii) `done`，是一个布尔值，用于指示序列是否结束。也就是说，只要序列中还有值要返回，它就是 `false`；只有当集合中的值已全部返回时，它才返回 `true`。

每次调用 `next()` 方法都会生成集合中的下一个值。遍历完集合的所有值后，再次调用 `next()` 方法将返回 `value` 为 `undefined` 且 `done` 为 `true`。

综上所述，让我们看看如何在 ES6 中手动实现一个迭代器：

```javascript
function myIterator(data) {
let currentIndex = 0;
return {
next: () => {
const done = (currentIndex >= data.length);
const value = !done ? data[currentIndex] : undefined;
currentIndex += 1;
return {
done,
value
};
}
};
}
const itrObj = myIterator([41, 42, 43]);
itrObj.next();  // { value: 41, done: false }
itrObj.next();  // { value: 42, done: false }
itrObj.next();  // { value: 43, done: false }
itrObj.next();  // { value: undefined, done: true }
// 此后所有调用
console.log(itrObj.next());    // { value: undefined, done: true }
```

在上述代码片段中，`myIterator()` 函数返回一个含有 `next()` 方法的对象，每次调用该方法都会返回集合中的下一个值。在最后一个元素之后，返回对象的 `done` 值变为 `true`，且返回的 `value` 始终为 `undefined`。

ES6 通过引入 `[Symbol.iterator]` 简化了迭代器的实现过程，该属性用于指定对象的默认迭代器。这个键持有 `@@iterator` 方法，它是本章后续将讨论的可迭代协议的基础。每当需要对对象进行迭代时（例如在 `for..of` 循环开始时），其 `@@iterator` 方法会被无参数调用。该方法返回对象的默认迭代器。在 ES6 中，`@@` 表示一个众所周知的符号。此处，`@@iterator` 指定了存储在对象键 `[Symbol.iterator]` 上的迭代器函数。因此，对于上述示例，`@@iterator` 方法将返回 `next()` 函数：

```javascript
myIterator[Symbol.iterator] = function () {
  return {
    next: function () {}
  }
}
```

使用符号作为键来持有迭代器方法，可确保它永远不会与常规对象键冲突。现在，让我们来看一下上述示例在 ES6 中替换为 `Symbol.iterator` 后的相同场景：

```javascript
const arr = [41, 42, 43];
const itrObj = arr[Symbol.iterator]();
itrObj.next(); // { value: 41, done: false }
itrObj.next(); // { value: 42, done: false }
itrObj.next(); // { value: 43, done: false }
itrObj.next(); // { value: undefined, done: true }
```

当为给定数组 `arr` 调用 `Symbol.iterator` 处的 `@@iterator` 方法时，会创建一个新的迭代器。请注意，每次调用时都会创建一个全新的迭代器。可迭代的 ES6 内置数据结构均具有此行为，允许通过 `next()` 方法对值进行迭代。

让我们以原始字符串值为例：

```javascript
const message = "Ian is an awesome student";
const itrObj = message[Symbol.iterator]();
itrObj.next();      // { value: "I", done: false }
itrObj.next();      // { value: "a", done: false }
..
```

在上述示例中，我们能够使用 `next()` 方法对字符串 `message` 的每个字符进行迭代。ES6 还允许我们对新的数据结构（如 `Map` 和 `Set` 集合）进行迭代。让我们以 `Map` 为例：

```javascript
const permissionMap = new Map();
permissionMap.set("admin", {read: true, write: true, del: true});
permissionMap.set("student", {read: true, write: false, del: false});
permissionMap.set("faculty", {read: true, write: true, del: false});
permissionMap.set("staff", {read: true, write: false, del: true});
const permissions= permissionMap[Symbol.iterator]();
console.log(permissions.next());
// {value: ['admin', { read: true, write: true, del: true }], done: false}
console.log(permissions.next());
// {value: ['student', { read: true, write: false, del: false }], done: false}
console.log(permissions.next());
// {value: ['faculty', { read: true, write: true, del: false }], done: false}
console.log(permissions.next());
// {value: ['staff', { read: true, write: false, del: true }], done: false}
console.log(permissions.next());
// {value: undefined, done: true}
```

在上例中，我们为不同用户类型的权限创建了一个映射，并以用户类型作为键。我们可以通过调用 `@@iterator` 方法简单地创建一个新的迭代器，并使用 `next()` 对值进行迭代。此处值得注意的一点是，这些集合还提供了用于生成迭代器的 API 方法，例如，你可以使用 `entries()` 方法获取映射的迭代器：

```
const permissionEntries = permissionMap.entries();
console.log(permissionEntries.next());
// {value: ['admin', { read: true, write: true, del: true }], done: false}
console.log(permissionEntries.next());
// {value: ['student', { read: true, write: false, del: false }], done: false}
```

## 迭代协议

现在我们已对迭代器和可迭代对象有了基本了解，接下来谈谈本章开头简要讨论过的那些协议。迭代协议共有两种：可迭代协议和迭代器协议。任何遵守特定约定的对象都可以实现这些协议。

### 可迭代协议

可迭代协议允许你自定义 JavaScript 对象的迭代行为。该协议规定，所有可迭代对象都必须通过 `[Symbol.iterator]` 属性实现 `@@iterator` 方法。每当需要迭代某个对象时，就会调用此方法。该方法不接受参数，并返回默认的迭代器，可用于从可迭代对象中获取值。内置的可迭代对象，例如 `Array`、`Map`、`Set` 等，都有默认的 `@@iterator` 方法，允许我们遍历相应的集合。

### 迭代器协议

迭代器协议定义了一种从可迭代对象获取值序列的标准方式。该协议规定，迭代器对象必须定义一个 `next()` 方法，该方法不接受参数，并返回一个包含两个属性的对象：`done` 和 `value`。

### 作为可迭代对象的迭代器

如果一个迭代器本身也是可迭代对象，那么它可以直接与 `for...of` 循环一起使用。你可以通过为迭代器提供 `Symbol.iterator` 属性（该属性返回迭代器自身）来使其成为可迭代对象。请看下面的代码片段，我们将一个迭代器变成了可迭代对象，然后使用 `for...of` 循环进行遍历：

```javascript
const infiniteSequenceGenerator = {
currentNumber: 0,
// 使 "infiniteSequenceGenerator" 这个迭代器成为可迭代对象
[Symbol.iterator]() {
return this;
},
next() {
return {
value: this.currentNumber++,
done: false
}
}
};
const iter = infiniteSequenceGenerator[Symbol.iterator]();
console.log(iter === infiniteSequenceGenerator);   // true
console.log(iter.next().value);   // 0
console.log(iter.next().value);   // 1
console.log(iter.next().value);   // 2
console.log(iter.next().value);   // 3
for (let item of iter) {
if (item > 20) break;
console.log( item );
}
// 4
// 5
// 6
// 7
// ... 20
```

在上面的示例中，请注意我们是如何直接对可迭代对象使用 `for...of` 循环的，但内部实际上使用了 `next()` 方法，因为在 `for...of` 循环中我们得到的数字列表是从 `4` 开始的，我们已经在循环开始前手动使用 `next()` 方法调用了迭代器四次。

## 迭代器中的 `return()` 和 `throw()`

迭代器还有两个可选的方法：`return()` 和 `throw()`。大多数内置迭代器并未实现这两个方法，但它们在生成器的上下文中确实具有重要作用，我们将在本章后面部分进行探讨。

在某些情况下，你可能希望最后一个 `{done: true, value: undefined}` 项的 `value` 不是 `undefined`，而是某个其他值。这就是 `return()` 方法的用武之地。`return()` 方法用于向迭代器发送信号，表明使用该迭代器的代码将不再从迭代器中请求更多的值。当迭代器已消耗完所有值，或遇到异常终止时，可以使用此方法，以便执行清理操作，如关闭数据库连接、保存文件或关闭文件。如果 `return()` 方法存在，当集合被耗尽时，它将自动被调用；当然，也可以手动调用。`return()` 方法接受一个可选参数，该参数通常作为返回对象中的 `value` 值传回。

另一方面，`throw()` 用于向迭代器发出关于可能发生的异常或错误的信号。迭代器对 `throw()` 信号的处理方式可以与 `return()` 不同，因为它不意味着像 `return()` 那样完全停止。我们将在生成器章节中更详细地研究这些方法。

### 生成器

简单来说，生成器是可以暂停的函数。在 ES6 之前，JavaScript 的函数一旦开始运行，就必须运行到完成，期间无法中断任何执行。随着 ES6 的到来，通过生成器，我们引入了一种不同类型的函数，它不像普通函数那样总是运行到完成，而是可以在执行过程中暂停和恢复循环。

生成器是一种允许我们创建特殊类型迭代器的函数，其执行可以被挂起并保留上下文。如果一个函数包含一个或多个 `yield` 表达式，并且使用了 `function *` 语法，那么它就是一个生成器：

```javascript
function *gen() {
yield 42;
}
```

在这种语法中，`*` 的位置并不重要。生成器可以通过以下几种方式编写：

```javascript
function *gen()  { .. }
function* gen()  { .. }
function * gen() { .. }
function*gen()   { .. }
```

尽管如此，在代码中使用生成器时，最好保持风格一致。在本书的其余部分，我们将使用第一种格式。

#### 生成器函数

生成器函数通过执行函数体中的 `yield` 关键字来暂停，`yield` 可以在函数体中使用任意多次。你可以通过 `yield` 表达式返回值。请看下面的例子，我们从一个生成器中 yield 了值 `42`：

```javascript
function *generator () {
yield 42;
}
```

生成器执行起来就像任何其他函数一样，你也可以向它传递参数。唯一的区别在于，执行生成器并不会真正运行其内部的代码。它只是生成了一个迭代器，可以用来执行其内部的代码。

```javascript
function *gen() {
yield "Hello";
yield "from";
yield "generator";
}
```

现在，如果我们调用这个生成器函数，它不会被执行；相反，它会返回一个迭代器，用于执行其内部的代码。

```javascript
let obj = gen();
```

在上面这个生成器中，开头的操作会先运行，然后 `yield` 语句会暂停生成器的执行，直到调用 `next()` 方法。方法 `obj.next()` 会继续执行 `gen`，直到遇到下一个 `yield` 表达式：

```javascript
console.log(obj.next());   // { value: "Hello", done: false}
console.log(obj.next());   // { value: "from", done: false}
console.log(obj.next());   // { value: "generator", done: false}
console.log(obj.next());   // { value: undefined, done: true}
```

`yield` 可以在生成器内部使用任意多次。它也可以是循环的一部分，用于表示重复的暂停点。请注意，`yield` 不仅仅是一个暂停点，它在暂停生成器时还会发出一个值。

```javascript
function *infiniteNumbers() {
var n = 1;
while (true) {
yield n++;
}
}
var numbers = infiniteNumbers(); // 返回一个可迭代对象
console.log(numbers.next()); // { value: 1, done: false }
console.log(numbers.next()); // { value: 2, done: false }
console.log(numbers.next()); // { value: 3, done: false }
```

每次调用 `yield` 时，yield 出来的值就成为序列中的下一个值。没有提供值的 `yield` 语句仅表示该值为 `undefined`。

#### 与生成器通信
```

# 生成器与迭代器的高级用法

## 通过 `yield` 向生成器传递值

在上一节中，我们看到了生成器如何使用 `yield` 语句向我们的代码通信。我们也可以通过 `yield` 向生成器函数传递一个值，并在执行恢复时在生成器函数内部使用这些值。请看下面的代码片段：

```javascript
function *calculator() {
const num1 = yield "I am a calculator";
const num2 = yield "I add numbers";
console.log(`Sum is: ${num1 + num2}`);
}
const myGenerator = calculator();
console.log(myGenerator.next());
// { value: 'I am a calculator', done: false }
console.log(myGenerator.next(2));
// { value: 'I add numbers', done: false }
console.log(myGenerator.next(3));
// Sum is: 5
// { value: undefined, done: true }
```

在上面的例子中，第一次调用`next()`时，生成器 yield 出`"I am a calculator"`字符串然后暂停。第二次调用`next(2)`时，传入的值会被赋值给`num1`，因为在生成器内部，我们指定了在`yield "I am a calculator"`处 yield 出的任何值都会赋值给`num1`，`num2`同理。因此，第三次调用`next()`时，`Sum is: 5`被打印到控制台，并且由于没有更多的`yield`语句，`undefined`被作为值返回。

## 生成器与异步编程

生成器与代码良好通信的这种行为在异步编程场景中非常有用。生成器可以使你的异步代码看起来像同步代码。考虑另一个例子，演示生成器和异步代码如何良好协作：

```javascript
function getFlightDurations() {
  setTimeout(() => {
    flightIterator.next({
      Qatar: "39h 0m",
      Emirates: "40h 20m"
    });
  }, 1200);
}
function getFlightPrices() {
  setTimeout(function(){
    flightIterator.next({
      Qatar: "$2010",
      Emirates: "$1904"
    })
  }, 1000);
}
function *getFlights() {
  const allFlights = ["Qatar", "Emirates"];
  const flightDurations = yield getFlightDurations();
  const flightPrices = yield getFlightPrices();
  for (let flight of allFlights) {
    console.log(`New York to Auckland takes ${flightDurations[flight]} in ${flight} airlines for around ${flightPrices[flight]}`);
  }
}
const flightIterator = getFlights();
flightIterator.next();
// New York to Auckland takes 39h 0m in Qatar airlines for around $2010
// New York to Auckland takes 40h 20m in Emirates airlines for around $1904
```

在上面的例子中，`genFlights()`是一个生成器函数，它返回一个迭代器。在调用返回的迭代器的`next()`方法时，生成器执行到第一个`yield`语句，该语句调用了`getFlightDurations()`。`getFlightDurations()`是一个异步函数，它使用`setTimeout`将执行延迟 1.2 秒，并在迭代器上调用`next()`方法，确保`flightDurations`能被正确赋值。`flightPrices`同理，当`getFlightPrices()`中再次调用`next()`方法时，输出结果会被打印到控制台。如果我们在这里没有使用`yield`，`flightDurations`和`flightPrices`只会被赋值为`undefined`，因为在赋值时函数本身返回了`undefined`。使用`yield`语句可以防止在`setTimeout`函数执行之前赋值。这就是生成器如何帮助我们编写阻塞式异步代码，使代码看起来更同步。

## 提前完成

在本章前面，我们讨论了迭代器有两个可选方法：`return(...)`和`throw(...)`，它们在生成器上下文中具有很大相关性。一旦调用这些方法，它们会立即结束暂停生成器中的序列。例如，请看以下代码片段：

```javascript
function *getFruits() {
  yield "apple";
  yield "orange";
  yield "banana";
}
const fruitIterator = getFruits();
console.log(fruitIterator.next());           // {value: 'apple', done: false}
console.log(fruitIterator.return("kiwi"));  // {value: 'kiwi', done: true}
console.log(fruitIterator.next());           // {value: undefined, done: true}
```

在上面的例子中，`return("kiwi")`返回了传入的参数值`"kiwi"`并结束序列，因此`done`为`true`。这等同于在生成器函数中有一个返回值，不过你应该始终牢记，遍历从生成器获得的序列时，不包括`{ done: true }`所表示的值。例如，检查以下代码片段，我们正在遍历集合`fruitIterator`，它只包含在源生成器中调用`return()`方法之前 yield 出的元素：

```javascript
function *getFruits() {
  yield "apple";
  yield "orange";
  yield "banana";
  return "kiwi";
  yield "watermelon";
}
const fruitIterator = getFruits();
for (let fruit of fruitIterator) {
  console.log(fruit);
}
// apple
// orange
// banana
```

你也可以通过将代码包装在`try-finally`块中来避免序列立即终止，确保无论序列是提前完成还是`yield`语句耗尽，`finally`子句中的代码都会执行。这在序列结束后执行清理或关闭操作时非常有用。例如，请看以下代码片段：

```javascript
function *getFruits() {
  try {
    yield "apple";
    yield "orange";
    yield "banana";
  }
  finally {
    console.log("You must always eat a big watermelon");
    yield "watermelon";
  }
}
const fruitIterator = getFruits();
console.log(fruitIterator.next());
// { value: 'apple', done: false }
console.log(fruitIterator.return("kiwi"));
// You must always eat a big watermelon
// { value: 'watermelon', done: false }
console.log(fruitIterator.next());
// { value: 'kiwi', done: true }
console.log(fruitIterator.next());
// { value: undefined, done: true }
```

在上面的例子中，一旦`finally`块中的表达式执行完毕，序列将以`value`为`'kiwi'`且`done`为`true`终止，其中`'kiwi'`是作为参数传递给`return(...)`方法的。

类似地，`throw(...)`也会停止生成器的执行，但它永远不会被自动调用。当您可能想要警告用户一些意外错误时，这个方法会很有用，利用`try-catch`块在生成器中启用错误处理。例如，请看以下代码片段：

```javascript
function *getFruits() {
  yield "apple";
  yield "orange";
  yield "spinach";
  yield "watermelon"
}
const fruitIterator = getFruits();
for (let fruit of fruitIterator) {
  try {
    console.log(fruit);
    if (fruit === "spinach") {
      fruitIterator.throw("Vegetable Found");
    }
  }
  catch (err) {
    console.log(`Exception: ${err}`);
  }
}
// apple
// orange
// spinach
// Exception: Vegetable Found
```

注意`"watermelon"`从未从生成器 yield 出来，因为当`"spinach"`被返回时调用了`throw()`，从而停止了序列的执行。

## 总结

ES6 引入的迭代器和生成器提供了对集合中存储值的顺序访问，并允许我们自定义遍历这些值的方式。ES6 还引入了一组新的协议来定义迭代器和可迭代对象，称为迭代协议。在本章中，我们学习了如何使用独特的`Symbol.iterator`属性定义对象的默认迭代器。当在对象上提供`Symbol.iterator`时，该对象被视为可迭代对象。

在本章的后半部分，我们还学习了生成器函数，它由星号（*）字符和函数体中的一个或多个`yield`语句表示。它们是特殊类型的函数，可以在执行过程中暂停和恢复。