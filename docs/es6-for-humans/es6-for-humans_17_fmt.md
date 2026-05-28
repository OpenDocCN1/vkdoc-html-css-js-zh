# 7. 数组与集合

到目前为止，我们已经涵盖了 ES6 提供的许多新的和改进的特性和功能。在本章中，我们将介绍数组和集合等数据类型的更多增强功能。ES6 为 JavaScript 语言带来了许多新的静态属性和方法，扩展了内置原生对象和对象的功能。我们将探讨 `Array` 类型的扩展特性，以及一些现在已成为语言原生组件的新数据结构抽象。

## 数组与新方法

多年来，许多流行的 JavaScript 库（如 Underscore 和 Lodash）都随着数组数据结构的发展而发展，数组是最常被扩展的特性之一。因此，随着时间的推移，语言对这些库中常见的数组功能提供内置支持变得越来越必要。在本节中，我们将介绍所有新的 ES6 数组功能，并研究一些它们表现出色的用例。

我们现在有了语言提供的几个新的静态方法和原型方法，可帮助你创建、操作、填充和过滤数组。首先，让我们看看静态方法：`Array.from()` 和 `Array.of()`，然后我们将探索 ES6 中为数组引入的新原型方法。

### `Array.from()`

在 ES6 之前，为了将类数组对象（例如 `arguments` 和 `NodeList`）转换为真正的数组，我们需要使用一些技巧，比如在 `arguments` 或 `NodeList` 上调用 `Array.prototype.slice.call()` 来将对象转换为数组。考虑以下示例，其中 DOM 中存在一个元素列表：

```
Apple
Mango
Orange
Litchi
Banana

var listItems = document.querySelectorAll('li');
// 将 NodeList 转换为数组
listItems = Array.prototype.slice.call(listItems);
// 在新转换的数组上使用数组方法
listItems.map(item => { console.log(item.innerText) });
// Apple
// Mango
// Orange
// Litchi
// Banana
```

有了 ES6，我们现在有了一个更直接的解决方案，用于将像上例中描述的类数组对象转换为数组。新的 `Array.from` 静态方法允许我们从任何类数组或可迭代对象创建数组。不过，在继续之前，让我们清楚地理解我们所说的类数组对象具体是什么意思。这些就是具有 `length` 属性或索引元素的对象。例如，考虑以下对象：

```
const customers = {
'0': 'matt',
'1': 'ian',
'2': 'mikhail',
'3': 'akia',
'4': 'vincent',
length: 5
};
```

现在，如果我们想把这个对象转换为数组，我们可以使用 `Array.from()` 语法像这样简单地实现：

```
Array.from(customers).forEach(customer => {
console.log(customer);
});
```

在这个例子中，我们只是将客户对象传递给了函数，而它就返回了一个由各个元素组成的数组。让我们看看具体的 `Array.from` 语法：

```
Array.from(arrayLike[, mapFn[, thisArg]])
```

在语法 `arrayLike` 中，`arrayLike` 是一个类数组或可迭代对象，`mapFn`（可选参数）是一个映射函数，可应用于数组的每个元素。`thisArg` 是另一个可选参数，其值在执行 `mapFn` 时用作 `this`。

`Array.from()` 会尝试检查其第一个参数是否可迭代，如果是，则使用迭代器生成值并复制到返回的数组中。但如果传入一个类数组对象，其行为与 `slice()` 或 `apply()` 相同，即简单地循环访问从 0 到对象长度的数值命名属性值。

如果现在将上述示例与可选参数 `mapFn` 结合使用，代码如下所示：

```
Array.from(customers, customer => {
console.log(customer);
});
```

注意，我们移除了 `forEach` 方法，并使用第二个参数作为映射函数来遍历结果数组。

#### `Array.of()`

ES6 引入的另一个非常实用的方法是 `Array.of()`，它允许你轻松创建元素数组。`Array.of()` 接受一个项目列表作为参数，并将其作为数组返回。请看以下示例：

```
let arr = Array.of(10, 20, 30, 40);
console.log(arr); // [10, 20, 30, 40]
```

你可能会疑惑这与传统的 `Array(...)` 构造函数有何不同。你也可以使用 `Array()` 语法通过数组构造方法来创建数组。但 `Array.of()` 与 `Array` 构造函数的区别在于它们处理单个数字作为参数的方式。`Array` 构造函数有一个非常奇怪的行为：如果只传入一个数字，它不会创建包含该数字值的单元素数组，而是创建一个空数组，并以该数字作为其长度，所有元素都被设置为 `undefined`。

`Array.of()` 静态方法解决了这个问题，现在是首选的函数形式数组构造函数。

请看以下示例：

```
const arr1 = Array.of(10);
console.log(arr1); // [10]
console.log(arr1.length);      // 1
const arr2 = Array(10);
console.log(arr2); // [,,,,,,,,,]
console.log(arr2.length);      // 10
```

从上面的示例可以看出，`Array.of(10)` 创建了一个包含单个数字 `[10]` 的数组，而 `new Array(10)` 方法创建了一个长度为 10 的空数组。说到这里，我们需要理解为什么在第一种情况下要使用构造函数，而不是像 `a = [1, 2, 3]` 这样的字面量语法。例如，当你有一个回调函数需要将传入的参数包装成数组时，或者当你需要子类化 `Array` 并希望在子类实例中创建或初始化元素时，就会使用构造函数。

现在我们已经了解了 ES6 中为数组引入的两个新静态方法，接下来深入探讨 ES6 中为 `Array` 原型引入的新原型方法。

### 新的 `Array.prototype` 方法

在讨论了创建数组的新方法之后，让我们看看 ES6 中引入的新数组方法如何帮助我们轻松访问和操作存储在数组中的数据。在本节中，我们将介绍新的 `Array.prototype` 方法，包括 `entries()`、`keys()`、`values()`、`find()`、`findIndex()`、`fill()` 和 `copyWithin()`，并通过一些实际示例来理解这些方法如何帮助我们轻松访问和操作数据。

#### `Array.prototype.entries()`

`entries()` 方法返回一系列值，但通过迭代器逐个暴露它们，而不是一次性作为一个数组返回。你将在后续章节中更详细地学习迭代器，但这里我们先对迭代器做一个快速介绍。

迭代器一次从集合中访问一个项目，同时跟踪其在序列中的当前位置。它提供了一个 `next()` 方法，该方法返回序列中的下一个项目。`next()` 方法返回一个包含两个属性的对象：`done` 和 `value`，其中 `value` 表示集合中的项目，`done` 是一个布尔值，当可迭代对象的所有元素都被迭代完毕时，其值为 `true`。

每个可迭代对象（例如数组）都必须实现可迭代协议，这意味着该对象（或其原型链上的某个对象）必须有一个属性，其键为 `Symbol.iterator`。`Symbol.iterator` 是 ES6 的另一个新增特性，我们将在本书后面讨论，但现在可以将其视为一种定义永远不会与常规对象键冲突的特殊键的方式。

让我们来看一个数组，它是一个可迭代对象，以及它可以生成的用于消费其值的迭代器：

```
const arr = [11,12,13];
const itr = arr[Symbol.iterator]();
itr.next(); // { value: 11, done: false }
itr.next(); // { value: 12, done: false }
itr.next(); // { value: 13, done: false }
itr.next(); // { value: undefined, done: true }
```

现在我们对迭代器和可迭代对象有了基本了解，让我们仔细看看 `Array.prototype.entries()` 方法如何帮助我们遍历数组。此方法返回一个新的 `Array Iterator` 对象，该对象包含数组中每个索引的键/值对。例如，考虑以下代码片段：

### `Array.prototype.entries()`

```javascript
const breakfast = ['apples', 'bananas', 'oranges'];
const eBreakfast = breakfast.entries();
console.log(eBreakfast.next().value); // [0, 'apples']
console.log(eBreakfast.next().value); // [1, 'bananas']
console.log(eBreakfast.next().value); // [2, 'oranges']
```

你也可以使用`for-of`循环来遍历从`breakfast.entries()`调用返回的迭代器：

```javascript
for (let entry of eBreakfast) {
  console.log(entry);
}
// [0, 'apples']
// [1, 'bananas']
// [2, 'oranges']
```

类似地，还有另外两个数组方法可以分别访问键和值。让我们来看看它们。

### `Array.prototype.keys()`

此方法返回一个新的`Array Iterator`，其中包含数组中每个索引的键：

```javascript
const breakfast = ['apples', 'bananas', 'oranges'];
const kBreakfast = breakfast.keys();
console.log(kBreakfast.next().value); // 0
console.log(kBreakfast.next().value); // 1
console.log(kBreakfast.next().value); // 2
```

### `Array.prototype.values()`

此方法返回一个新的`Array Iterator`对象，其中包含数组中每个索引的值：

```javascript
const breakfast = ['apples', 'bananas', 'oranges'];
const vBreakfast = breakfast.values();
console.log(vBreakfast.next().value); // apples
console.log(vBreakfast.next().value); // bananas
console.log(vBreakfast.next().value); // oranges
```

### `Array.prototype.find()`

`find()`方法允许你遍历数组，并返回第一个与数组的`callback(element, index, array)`匹配的项目。此方法还允许你选择性地为`this`传递上下文绑定。换句话说，`find()`方法返回数组中第一个使测试函数返回`true`的值；否则返回`undefined`。`find()`方法的语法如下：

```javascript
arr.find(callback[, thisArg])
```

其中`callback`是测试函数，它对每个值执行，并接受三个参数：

```javascript
element, index and array.
function callback(element, index, array) {
  // 基于某些条件返回 true 或 false
}
```

考虑以下代码片段：

```javascript
const inventory = [
  {name: 'apples', quantity: 2},
  {name: 'bananas', quantity: 0},
  {name: 'oranges', quantity: 5}
];
let result = inventory.find((fruit) => fruit.name === 'apples');
console.log(result);             // {name: 'apples', quantity: 2}
```

上述示例展示了如何轻松地在给定数组中查找与`fruit.name`匹配`'apples'`的`第一个`元素。你也可以使用`find()`方法获取特定索引位置的元素，例如：

```javascript
result = inventory.find((fruit, i) => i > 2);
console.log(result);             // {name: 'oranges', quantity: 5}
```

如上所述，如果在数组中未找到元素，`find()`方法返回`undefined`：

```javascript
result = inventory.find((fruit, i) => i > 10);
console.log(result);             // undefined
```

下一个参数`thisArg`允许你为`this`可选地传递一个上下文绑定；例如，考虑以下代码片段：

```javascript
function eligibleToVote(age) {
  return age >= this.legalAge;
}
function foo() {
  this.legalAge = 18;
  const result = [10, 12, 23, 26, 32].find(eligibleToVote, this);
  console.log(result);
}
foo();     // 23
```

在这个例子中，`foo`的上下文作为第二个参数传递给`find()`方法，因此`eligibleToVote()`中的`this`指向`foo`的上下文，而`this.legalAge`的值为`18`。

你可能会认为这个方法类似于 ES5 中的`filter()`方法，但请注意`filter()`总是返回一个匹配项的数组（并且可能返回多个匹配项），而`find()`只返回一个元素（数组中第一个满足条件的元素）。

### `Array.prototype.findIndex()`

此方法是`find()`的等价方法。它不返回元素本身，而是返回索引位置。如果集合中没有元素满足`callback (element, index, array)`的条件，则返回值为 -1。

例如，考虑以下代码片段：

```javascript
result = inventory.findIndex((fruit) => fruit.name === 'apples');
console.log(result);             // 0
result = inventory.findIndex((fruit) => fruit.name === 'grapes');
console.log(result);             // -1
```

此方法类似于 ES5 中的`indexOf()`方法，它仅定位数组中的元素。使用`indexOf()`方法，你只能搜索数组中的元素；它不支持回调函数。而使用`findIndex()`方法，我们可以应用一个条件，返回的索引位置上的元素必须满足该条件。

### `Array.prototype.fill()`

这是一个非常简单的方法，允许我们用静态值填充数组的所有元素。它还可以接受可选的起始和结束索引值。

考虑以下代码片段：

```javascript
[1, 2, 3].fill(4);         // [4, 4, 4]
[1, 2, 3].fill(4, 1);      // [1, 4, 4]
[1, 2, 3].fill(4, 1, 2);   // [1, 4, 3]
```

输入值可以是任意值，不一定是数字、字符或任何其他原始类型。它可以是一个对象，例如：

```javascript
new Array(4).fill({});     // [{}, {}, {}]
```

### `Array.prototype.copyWithin()`

`copyWithin()`方法将数组中的元素序列复制到以`target`为目标起始位置。要复制的元素取自`(start, end)`范围。`start`参数是可选的，默认为 0。同样，`end`参数也是可选的，默认为数组的长度。

`copyWithin()`方法的语法如下：

```javascript
Array.prototype.copyWithin(target, start = 0, end = this.length)
```

我们先从一个简单的例子开始。考虑以下代码片段：

```javascript
let fruits = ["apples", "bananas", "oranges", "grapes", "guava", "watermelon"];
fruits.copyWithin(4);
console.log(fruits);
// ["apples", "bananas", "oranges", "grapes", "apples", "bananas"]
```

`fruits.copyWithin(4)`将`target`索引设为 4，即"guava"。然后，它将确定要复制的元素`start`为 0（默认值），`end`为 6（数组长度）。当然，你可以根据需要指定起始和结束值：

```
fruits = ["apples", "bananas", "oranges", "grapes", "guava", "watermelon"];
fruits.copyWithin(4, 1, 3);
console.log(fruits);
// ["apples", "bananas", "oranges", "grapes", "bananas", "oranges"]
```

`copyWithin()`方法也接受负值的`start`索引、负值的`end`索引和负值的`target`索引。让我们看一个使用负值的例子：

```
[1, 2, 3, 4, 5].copyWithin(-3, -4, -1);    // [1, 2, 2, 3, 4]
```

在这个例子中，`target`索引为`-3`，确定了起始位置为数组倒数第三个元素（在输入数组中是`3`）。然后确定`start`和`end`值分别为`2`和`5`（分别是数组的第四个和最后一个元素，但这里`-4`对应的索引是 2，`-1`对应的索引是 5，表述需准确：-4 对应数组第四个元素，即 2；-1 对应数组最后一个元素，即 5）。

### 类型化数组

ES6 中的类型化数组提供了一种高效访问和操作二进制数据的方法。在 ES6 之前，JavaScript 不太擅长处理二进制数据。在处理二进制数据时，类型化数组有助于提高性能。

使用类型化数组，JavaScript 引擎不必推断数组的类型。如果你在 JavaScript 中创建一个普通数组，并且只存储浮点数，你的 JavaScript 引擎可能会乐观地认为它是一个浮点数数组，并为其优化代码。此时性能可以与类型化数组相当。但是，假设你有一个浮点数数组，并且代码中某个时刻，你在一个元素中存储了一个对象。在这种情况下，你的 JavaScript 引擎必须取消优化并重新生成代码，使数组恢复为通用类型。因此，使用类型化数组，事情要简单得多，因为它们保证是单一类型，并且你不能在其中存储对象等其他内容。它们更简单这一事实意味着不那么复杂的 JavaScript 引擎可以轻松实现它们，并且你可以获得一致的性能。

类型化数组的架构由缓冲区和视图组成。缓冲区是一个表示数据块的对象。我们不能直接操作缓冲区的内容。为了访问缓冲区中包含的内存，我们需要使用视图。视图使我们能够读写缓冲区的数据内容。

因此，类型化数组有两个独立的类：`ArrayBuffer`和`DataView`。`ArrayBuffer`包含我们的数据，而`DataView`提供了对此数据的自定义视图，该视图以特定格式表示缓冲区，并支持数据访问和操作。

### 使用类型化数组的基础知识

查看以下代码片段；这里我们创建一个`ArrayBuffer`，然后创建一个特定的`DataView`来处理该缓冲区内的数据：

```
const buffer = new ArrayBuffer(16);
const int32View = new Int32Array(buffer);
for (let i = 0; i < int32View.length; i++) {
int32View[i] = i * 2;
}
console.log(int32View); // [0, 2, 4, 6]
```

类型化数组视图为所有常用的数值类型提供了视图，如 Int8、Uint32、Float64 等。你可以在表 7-1 中查看所有类型及其各自的大小和描述。

**表 7-1.** 类型化数组视图

| 类型 | 大小（字节） | 描述 |
| --- | --- | --- |
| Int8Array | 1 | 8 位有符号整数 |
| Uint8Array | 1 | 8 位无符号整数 |
| Uint8ClampedArray | 1 | 8 位无符号整数（钳制） |
| Int16Array | 2 | 16 位有符号整数 |
| Uint16Array | 2 | 16 位无符号整数 |
| Int32Array | 4 | 32 位有符号整数 |
| Uint32Array | 4 | 32 位无符号整数 |
| Float32Array | 4 | 32 位浮点数 |
| Float64Array | 8 | 64 位浮点数 |

请注意，在钳位数组（clamped arrays）的情况下，所有超出范围的值都会被设置为范围内最近的值，即第一个或最后一个元素。`Uint8ClampedArray`类型化数组表示一个 8 位无符号整数的数组，值被钳位在 0-255 之间，这意味着如果你指定一个超出`[0,255]`范围的值，则会设置 0 或 255。如果你指定一个非整数，则会设置最近的整数。

### 类型化数组与普通数组

类型化数组和普通数组在某些方面相似：它们都有`length`属性，并且在两种情况下，都可以使用`[]`运算符访问数组中的元素。类型化数组也支持所有标准数组方法。但它们在多方面存在差异。

JavaScript 中的类型化数组类似于 C、C++等其他编程语言中的数组，其中数组的所有元素类型相同，这与 JavaScript 中的数组不同，后者元素可以是任何类型。类型化数组对所有元素都有严格的类型要求。

类型化数组不能有空元素。它们总是用 0 值进行初始化，而普通数组中的值可以是空的（`undefined`）。请查看以下代码片段：

```
const buffer = new ArrayBuffer(16);
const float32View = new Float32Array(buffer);
let arr = new Array(10);
console.log(float32View); // Float32Array [0, 0, 0, 0]
console.log(arr); // [,,,,,,,,,]
```

类型化数组受到多种浏览器 API 的支持：`File API`、`XMLHttpRequest`、`Fetch API`、`Canvas`、`WebSockets`、`WebGL`、`Web Audio API`、`Media elements`等。目前，类型化数组的某些功能可能并非所有浏览器和 JavaScript 引擎都支持，因此你可以在`https://kangax.github.io/compat-table/es6/#typed_arrays`检查目标 JavaScript 引擎的可用性。

## Map 和 WeakMap

ES6 引入了一组新的数据结构，称为`Map`和`WeakMap`，它们本质上是一个哈希表或字典，类似于 Python 或 C#中的概念。ES6 Maps 提供了一个简单的 API，用于通过任意键存储对象，这是许多 JavaScript 程序中必不可少的核心功能。我们将在本节中详细介绍它们。

### Map

实际上，我们一直在 JavaScript 中使用 Map。事实上，每个对象都可以被视为一个`Map`。对象由键（始终是字符串）和值组成，而在`Map`中，任何值（包括对象和原始值）都可以用作键或值。

在`Map`中，键可以是任何类型：`string`、`Boolean`、`number`、`object`或`function`。请看这段代码：

```
let myMap = new Map();
const keyString = "a string",
keyObj = {},
keyFunc = () => {};
// 设置值
myMap.set(keyString, "value associated with 'a string'");
myMap.set(keyObj, "value associated with keyObj");
myMap.set(keyFunc, "value associated with keyFunc");
```

请注意，`set()`方法是可链式调用的，因此你也可以这样做：

```
myMap.set(keyString, "value associated with 'a string'")
.set(keyObj, "value associated with keyObj")
.set(keyFunc, "value associated with keyFunc");
myMap.size; // 3
// 获取值
myMap.get(keyString);    // "value associated with 'a string'"
myMap.get(keyObj);       // "value associated with keyObj"
myMap.get(keyFunc);      // "value associated with keyFunc"
```

### 检查键的相等性

`Map()`中的键相等性基于“same-value”算法，该算法与`===`运算符类似，但将`NaN`视为等于自身（尽管通常`NaN !== NaN`），这意味着当使用`NaN`作为键时，它将返回之前使用`NaN`作为键设置的 map 对象。请看以下代码片段：

# JavaScript 中的集合：Map、WeakMap、Set 和 WeakSet

```javascript
let myMap = new Map();
myMap.set(NaN, "not a number");
myMap.get(NaN); // "not a number"
```

请注意，你也可以通过一个包含键/值对的可迭代对象来创建 map：

```javascript
let myMap = new Map([
[ 1, 'apple' ],
[ 2, 'banana' ],
[ 3, 'orange' ]
]);
```

此外，maps 可以使用`for...of`或`forEach()`进行迭代：

```javascript
for (let [key, value] of myMap) {
console.log(key + " = " + value);
}
// 1 = apple
// 2 = banana
// 3 = orange
```

Maps 还支持`keys()`、`values()`和`entries()`方法：

```javascript
for (let key of myMap.keys()) {
console.log(key);
}
// 1
// 2
// 3
for (let value of myMap.values()) {
console.log(value);
}
// apple
// banana
// orange
for (let [key, value] of myMap.entries()) {
console.log(key + " = " + value);
}
// 1 = apple
// 2 = banana
// 3 = orange
```

如果你是一位经验丰富的 JS 开发者，你会知道对象是创建无序键/值对作为数据结构的主要机制。然而，使用对象作为 maps 并不能让我们使用非字符串值作为键。

此时，你一定会想，何时该使用 maps，何时不该使用 maps 而使用对象。当你需要一个键/值集合时，你应该始终使用`Map`，因为对象本质上并不是设计用来作为集合的，因此没有有效的方法来确定对象拥有的属性数量。当你遍历对象的属性时，你也会得到其原型属性。当然，有一些解决方法，但当你遍历对象的属性时，属性不一定会按照插入的顺序被检索到。这就是为什么在 JavaScript 程序中需要键/值集合时，推荐使用 maps。

另一个好的判断指标是，当你事先不知道集合的键时，即这些键是从数据库读取或由用户输入的，这意味着它们不一定是字符串或数字。相反，当你在编写代码时就知道对象拥有哪些属性以及有多少属性时，即当它们的结构是静态的时，你应该使用对象。

### WeakMap

`WeakMaps`类似于普通的`Maps`，但方法更少，并且在垃圾回收方面存在一些差异。`WeakMap`是一种键被弱引用的`Map`，这意味着如果对键的所有引用都丢失了，并且没有更多对值的引用，那么`WeakMap`不会阻止其键被垃圾回收。因此，你无需担心`WeakMaps`的内存泄漏。通常，当你需要存储与 DOM 节点等相关的元数据时，你会希望有这样的行为，这样当 DOM 元素不再被关注时，它们可以被从内存中释放。

`WeakMap`最大的限制是它是不可迭代的，这与`Map`不同——这意味着它不支持`entries()`、`keys()`、`values()`、`forEach()`和`clear()`方法。这里需要注意的另一件事是，与`Map`不同，在`WeakMap`中，每个键都必须是`object`。

尽管它拥有与`Map`几乎相同的 API，但我们无法迭代`WeakMap`集合。我们甚至无法确定集合的长度，因为这里我们没有`size`属性。`WeakMap`只有四个方法：

*   `delete(key)`

*   `has(key)`

*   `get(key)`

*   `set(key, value)`

```javascript
let wMap = new WeakMap();
wMap.set('a', 'b');
// Uncaught TypeError: Invalid value used as weak map key
const o1 = {},
o2 = () => {},
o3 = window;
wMap.set(o1, 42);
wMap.set(o2, "hello");
wMap.set(o3, undefined);
wMap.get(o3); // undefined, 因为这就是设置的值
wMap.has(o1); // true
wMap.delete(o1);
wMap.has(o1); // false
```

## Set 和 WeakSet

`Sets`和`WeakSets`是 ES6 中引入的另一种集合类型。如果你来自像 Python 这样的编程语言，你会知道集合是多么有用。让我们在本节中看看它们。

### Set

`Set`对象是唯一值的集合。重复值将被忽略，因为集合必须包含所有唯一的值。`Sets`速度很快，值可以是原始类型或对象引用。

查看以下代码片段以了解有关集合及其方法的更多信息：

```javascript
let mySet = new Set([1, 1, 2, 2, 3, 3]);
mySet.size; // 3
mySet.has(1); // true
mySet.add('strings');
mySet.delete('strings'); // true
mySet.has('strings'); // false
mySet.add({ a: 1, b:2 });
mySet.size; // 4
mySet.clear(); // 清空集合
mySet.size; // 0
```

请注意，与映射（Maps）类似，在 `Set` 中 `NaN` 也等于 `NaN`。你可以使用 `forEach` 方法或 `for...of` 循环按照插入顺序遍历集合：

```javascript
mySet = new Set([1, 1, 2, 2, 3, 3, { a: 1, b:2 }]);
mySet.forEach((item) => {
console.log(item);
});
// 1
// 2
// 3
// Object { a: 1, b: 2 }
for (let value of mySet) {
console.log(value);
}
// 1
// 2
// 3
// Object { a: 1, b: 2 }
```

如果你需要一个包含唯一元素的动态列表，用于在下拉菜单中渲染或作为输入框的自动建议，集合（Sets）会非常有用。使用 `Set` 可以确保你的建议列表永远不会出现重复值，并省去了检查重复的麻烦。

### WeakSet

与 `WeakMap` 类似，`WeakSet` 对象允许你在集合中存储对象的弱引用，这意味着 `WeakSet` 不会阻止其元素被垃圾回收。`WeakSet` 中的对象只出现一次；它在集合中是唯一的。

`WeakMap` 中的值必须是唯一的 `对象` 引用。如果没有其他内容引用 `WeakSet` 中存在的对象，它将被垃圾回收。

与 `WeakMap` 非常相似，`WeakSet` 是不可遍历的，也没有 `size` 属性。`WeakSet` 仅支持三种方法：

- `add(value)`

- `has(value)`

- `delete(value)`

```javascript
let ws = new WeakSet();
const obj = {};
const foo = {};
ws.add(window);
ws.add(obj);
ws.has(window); // true
ws.has(foo);    // false，foo 尚未添加到集合中
ws.delete(window); // 从集合中移除 window
ws.has(window);    // false，window 已被移除
```

当你需要在集合中利用垃圾回收能力时，应优先使用 `WeakSet` 而非 `Set`。另一个典型用例是标记某个对象具有某些属性。请参考以下示例：

```javascript
const fruits = new WeakSet();
class Fruit {
constructor() {
fruits.add(this);
}
getName() {
if (!fruits.has(this)) {
throw new TypeError("getName() 在不相容的对象上被调用！");
} else {
// 返回名称
}
}
}
```

这是一种很好的方式，可以防止在非类构造函数创建的对象上使用类方法。

## 总结

在本章中，我们学习了 ES6 中引入的、用于使用数组和集合处理及操作数据的新有效方法。这些新特性为 ES6 之前处理起来很棘手的常见用例提供了内置解决方案。借助新的数组方法，你可以在 JavaScript 程序中轻松创建和管理数组。我们还学习了 ES6 中引入的新数组原型方法，这些方法帮助我们高效地遍历和访问存储在数组中的元素。我们还讨论了在处理 JavaScript 中的二进制数据时，类型数组如何帮助你提升性能。在本章后半部分，我们学习了 ES6 中引入的新集合类型：映射（Maps）、弱映射（WeakMaps）、集合（Sets）和弱集合（WeakSets），它们具有可靠的功能集和有效的用例。

至此，你应该对在 JavaScript 应用程序中使用这些新数据类型充满信心。在下一章中，我们将深入探讨之前简要介绍过的可迭代对象和迭代器，并学习 ES6 中引入的生成器概念。