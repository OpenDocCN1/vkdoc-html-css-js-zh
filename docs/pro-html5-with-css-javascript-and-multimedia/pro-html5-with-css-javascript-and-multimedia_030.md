# JavaScript 基础

要访问数组中的某个元素，需要指定它的索引。例如，`colors[0]` 将返回字符串 `"red"`。

`length` 属性表示数组中有多少个元素。由于索引是从 0 开始的，因此 `length` 属性总是指向最后一个元素之后的位置。所以你也可以通过以下方式在数组末尾添加元素：

```js
colors[colors.length] = "blue";
```

**提示：** 数组中的元素通过从 0 开始的数字索引来访问。数组不支持命名索引（或键）。如果你需要通过名称访问某个项，应将其创建为命名属性或变量，而不是数组元素。

一个数组可以包含不同类型的元素。在同一个数组中包含数字、字符串和对象引用是完全允许的。根据你使用数组的方式，这在逻辑上可能并不正确，但 JavaScript 并不会阻止你这样做。

你可以使用 `for` 循环遍历数组中的元素：

```js
for (var i=0; i < colors.length; i++) {
  console.log(colors[i]);
}
```

#### 属性

所有属性都有一组可被读取和更新的特性（attribute）。这些特性如下：

- `value` — 属性的值；这是默认属性。
- `writable` — 如果属性可以被更新，则设置为 `true`。
- `enumerable` — 如果枚举对象成员时应包含此属性，则设置为 `true`。
- `configurable` — 如果属性可以被删除，并且其特性可以被修改，则设置为 `true`。

你可以使用 `getOwnPropertyDescriptor()` 方法来访问*属性描述符*。属性描述符是一组命名值，用于指定这些特性的值。例如，考虑以下代码：

```js
function Item(color, count) {
  this.color = color;
  this.count = count;
}

var redObject = new Item("Red", 5);
console.log(Object.getOwnPropertyDescriptor(redObject, "color"));
```

这将输出 `redObject` 的 `color` 属性的特性。控制台会显示如下内容：`Object {value: "Red", writable: true, enumerable: true, configurable: true}`

`getOwnPropertyDescriptor()` 方法接收两个参数：第一个参数是包含该属性的对象；第二个参数是属性的名称。它返回一个属性描述符。

你可以使用 `Object.defineProperty()` 方法更新这些特性。该方法的头两个参数分别是一个对象和一个属性名称，与 `getOwnPropertyDescriptor()` 方法类似。然后，你传入一个属性描述符作为第三个参数。你只需要指定想要更改的值。例如，如果你希望 `color` 属性为只读，可以这样调用：

```js
Object.defineProperty(redObject, "color", { writable: false });
```

你也可以使用 `defineProperty()` 方法创建新属性，这样你就可以在属性首次创建时设置这些特性。如果你传入一个不存在的属性名称，则会创建一个新属性。以下代码在 `Item()` 构造函数中添加了 `size` 属性：

```js
function Item(color, count) {
  this.color = color;
  this.count = count;
  Object.defineProperty(this, "size",
    {value: 7, writable: true, enumerable: false, configurable: true});
}
```

你还可以通过调用属性的 `propertyIsEnumerable()` 方法来确定它是否可枚举。

#### 特殊类型

除了三种简单类型之外，还有两种特殊的数据类型：`null` 和 `undefined`。`null` 数据类型通常用于属性是一个对象引用，但该对象尚未创建（或引用尚未设置）的情况。你可以使用 `null` 关键字将属性设置为 `null`（`test = null;`）。

`undefined` 数据类型表示该值尚未被定义。考虑以下代码：

```js
var x = null;
var y;

console.log(typeof x);
console.log(typeof y);
```

变量 `x` 已被设置为 `null`，`typeof` 运算符将返回 `"object"`。相比之下，变量 `y` 未被定义，`typeof` 运算符将返回 `"undefined"`。

你可以使用数字作为属性名称；但是，你需要像访问数组索引一样使用方括号来访问该属性。例如：

```js
var myEnum = {1: "Red", 2: "Green", 3: "Blue"};
console.log(myEnum[2]);
```

#### 其他主题

JavaScript 有一些特殊的行为，或者至少对 JavaScript 新手来说不太容易理解。以下我将简要介绍它们。

#### 比较运算符

JavaScript 支持典型的比较运算符，例如小于、大于等。我不打算详细说明，因为它们大多不言自明。如果你想进一步探索，Mozilla 有一篇关于这个主题的好文章（<https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Comparison_Operators>）。

不过，有一个地方我需要详细说明。JavaScript 区分等同性（equivalence）和相等性（equality）。例如，数字 `12` 和字符串 `"12"` 被视为等同，但并不相等。在这里，等同性意味着如果两个对象被转换为相同的数据类型，它们将具有相同的值。相等性则意味着两个对象具有相同的类型和值。这有时被称为*严格相等*。

标准运算符，如等于（`==`）、小于（`<`）、大于或等于（`>=`），使用的是等同性概念。为了测试严格相等性，JavaScript 提供了 `===` 和 `!==` 运算符。例如，`a === b` 将使用严格相等性比较对象 `a` 和 `b`。同样，你可以使用 `!==` 来测试不严格相等。

**提示：** 我使用“等同性”和“相等性”这两个术语，因为它们能更准确地描述这些概念。然而，你经常会看到用“相等性”来指代等同性，而用“严格相等性”来表示相等性。`===` 和 `!==` 运算符是 JavaScript 中相对较新的补充。在此之前，所有的比较运算符都是基于等同性的，我们使用等于运算符（`==`）并称之为等于。现在我们有了一个真正表示相等的运算符（`===`）。

#### 变量作用域

变量的*作用域*决定了它可以在何处被访问。如果变量在函数体外部声明，它将具有*全局作用域*，这简单意味着所有函数都可以访问它。此外，单个实例由所有函数共享；如果一个函数修改了它的值，那么在它被使用的任何地方，它都会具有这个新值。并且，该变量在浏览器关闭之前一直可用。

在函数内部声明的变量被视为具有*局部作用域*。该变量仅对函数内部的代码可用。一旦函数返回，该变量就不再可用。每次调用函数时，都会创建该变量的一个新实例，因此调用之间不会共享值。函数参数始终具有局部作用域。

最后一种作用域类型是*块级作用域*，它将变量的访问限制在其被声明的*块*内。一个块由围绕它的花括号 `{}` 定义，例如在 `if` 或 `for` 语句中。块级作用域是 JavaScript 中相对较新的功能（在 ECMAScript 第 6 版中引入）。由于 JavaScript 最初不支持块级作用域，例如在 `if` 语句中声明的变量，整个函数都可以访问。如果 JavaScript 突然开始强制执行块级作用域，现有的应用程序将会失败。

相反，块级作用域是通过使用 `let` 关键字而不是 `var` 来选择性地应用的。考虑以下函数定义：

```js
function testScope() {
  var localScope = 5;
  if (localScope > 3) {
    var localScope2 = 7;
    let blockScope = 4;
  }
}
```


```markdown

`console.log(localScope2 + blockScope);` // 输出 11

}

`console.log(localScope2);` // 输出 7

`console.log(blockScope);` // 这将失败

}

`testScope();`

`localScope` 变量是使用传统的 `var` 关键字声明的，它在 `if` 块外部是可用的。然而，`blockScope` 变量是使用 `let` 关键字声明的，它在 `if` 块外部不可用。无论如何，在 `testScope()` 函数外部，局部作用域或块作用域的变量都是不可访问的。

变量作用域中一个棘手的问题是多个变量使用了相同的名称声明。

这不会产生错误，但你可能无法得到预期的结果。例如，请看以下代码：

`var x = 1;` // 全局作用域

```
function testX(x1){
    var x = 2; // 局部作用域
    console.log("传入的原始 x 值: " + x1);
    console.log("局部作用域的 x: " + x);
}

testX(x);
console.log("全局作用域的 x: " + x);
```

一个名为 `x` 的变量在全局作用域和局部作用域中都被创建了。它们实际上是两个不同的对象，各自拥有自己的值。全局作用域对象的值被传入函数，并在函数内作为参数 `x1` 可用。

函数声明了一个名为 `x` 的变量；这是一个新对象，与第一个对象无关，只是它们名字相同。任何引用 `x` 的代码都会使用局部对象；函数不可能访问全局作用域的变量。这被称为*隐藏*对象。在函数内设置 `x` 的值只会改变局部变量。当这段代码执行时，控制台会输出以下内容：

```
original x value passed in: 1
local-scoped x: 2
global-scoped x: 1
```

**注意：** 如果你在函数内使用变量但没有用 `var` 或 `let` 关键字声明，它将被创建为全局作用域变量。例如，如果在函数中添加语句 `x=5;`，变量 `x` 将是全局变量。你应该始终使用 `var` 或 `let` 声明变量，以使你的意图清晰。

#### 严格模式

当使用*严格模式*时，未声明的变量是不被允许的，并且会导致错误。否则，如果不处于严格模式，JavaScript 会为你声明变量。虽然这看起来是个好主意，但可能导致意外结果。例如，考虑以下代码：

```
var myIntervalCounter = 0;

for (var i=0; i < 10; i++)
{
    if (i % 2 == 0) { myInterbalCounter = i * 2; }
}

console.log(myIntervalCounter);
```

注意在 `for` 循环中 `myIntervalCounter` 变量拼写错了。在非严格模式下，会创建一个新变量并设置其值，而不是更新原变量。本应被更新的变量从未被设置，其值仍为 0。

![](img/index-69_1.jpg)

你可以通过在脚本顶部添加以下内容来开启严格模式：

```
"use strict";
```

这将为整个文件开启严格模式。或者，你也可以通过在函数定义的第一行添加它，将其应用于特定函数。在严格模式下，之前的代码会生成一个错误，如图 3-4 所示。

***图 3-4.** 严格模式错误*

**提示：** 严格模式还有其他一些细微差别，例如不允许你使用保留关键字命名变量。Mozilla 有一篇很好的文章详细解释了严格模式的功能，网址为 [`developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode)。

#### 函数

我们之前已经看过函数，使用函数创建对象。函数也是一个对象，可以像属性一样作为对象成员添加。这满足了面向对象封装的要求。

函数也可以像其他任何类型的变量一样被定义。因此，函数可以在全局作用域（函数体外）中定义，但也可以在其他函数内部定义。考虑以下代码：

```
var functionA = function(){
    var x = 1;
    var functionB = function(){
        var y = 2;
        var functionC = function(){
            var z = 3;
            console.log(x+y+z);
        }
    }
}
```

在这里，`functionA()` 在局部作用域中声明了 `functionB()`，后者又在局部作用域中创建了 `functionC()`。这被称为*词法作用域*，即在一个函数内部定义另一个函数。

每个函数定义了一个新的作用域。`functionA()` 在全局作用域中。在花括号之间定义了一个新的局部作用域；我们称其为作用域 A，它包含变量 `x` 和 `functionB()`。在 `functionB()` 的花括号内部，定义了另一个作用域；我们称其为作用域 B。它包含变量 `y` 和 `functionC()`。类似地，在 `functionC()` 内部有第三个局部作用域，包含变量 `z`。

此时，没有任何函数被调用；你只是在定义它们。只有 `functionA()` 具有全局作用域，并且它是你唯一可以调用的函数。让我们重写这段代码，加入一些函数调用，以便观察实际效果。内部函数 `functionB()` 可以调用 `functionC()`，因为它位于 `functionB()` 的作用域内。

类似地，`functionA()` 可以调用 `functionB()`。由于 `functionA()` 是全局作用域，你可以直接调用它。

```
var functionA = function(){
    var x = 1;
    var functionB = function(){
        var y = 2;
        var functionC = function(){
            var z = 3;
            console.log(x+y+z);
        }
        functionC();
    }
    functionB();
}

functionA();
```

注意，`functionC()` 可以访问 `functionA()` 和 `functionB()` 中的局部作用域变量。

这被称为*闭包*。闭包发生在内部函数访问其外部（包含）函数中的变量时。内部函数也可以访问外部函数的局部作用域中的函数和变量。所以 `functionC()` 可以调用 `functionB()`。它也可以调用 `functionA()`，因为这是全局作用域的。但如果它这样做（在当前的例子中），你会陷入无限循环。

**注意：** 请记住，`functionA()` 不能调用 `functionC()`。一个函数可以访问其局部作用域以及作用域链中往前的任何东西；但它不能访问作用域链中更靠后的任何东西。

所以你不能直接调用 `functionC()`。真的不能吗？实际上，如果返回了对它的引用，你就可以调用它；这是闭包的一种常见用法。只有 `functionB()` 可以访问 `functionC()`，但函数与其他任何变量一样都是对象，可以作为返回值提供给调用者。我稍微重写一下代码来演示这一点。

```
var functionA = function(){
    var x = 1;
    var functionB = function(){
        var y = 2;
        var functionC = function(){
            var z = 3;
            console.log(x+y+z);
        }
        return functionC;
    }
    return functionB();
}

var closure = functionA();
closure();
```

在 `functionB()` 内部，不是调用 `functionC()`，而是返回该函数。注意 `return` 语句中没有括号。这仅仅是返回名为 `functionC` 的变量（它恰好是一个函数），而不实际执行它。在 `functionA()` 内部，它确实调用了 `functionB()` 并返回了 `functionB()` 返回的值，即对 `functionC()` 的引用。

调用 `functionA()` 只是返回一个函数引用，该引用存储在 `closure` 变量中。`closure` 变量现在有了对 `functionC()` 的引用。任何时候你想调用 `functionC()`，只需调用 `closure()`。

#### 上下文

`this` 关键字指的是当前的执行上下文，但有时并不清楚 *this* 是什么。这就像去一家餐厅，告诉你的服务员“你想再来一些*这个*。” 希望你已经指明了*这个*是什么，否则你可能得不到你想要的。JavaScript 有时就是这样。

```


`this` 关键字引用单个对象。例如，在函数内部，`this` 指向调用该函数的对象。在包含属性和方法的典型对象中，`this` 通常指向其所属对象。例如：

```
var myObject = {
    color: "Red",
    count: 5,
    log: function () {
        console.log("Quantity: " + this.count + ", Color: " + this.color);
    }
};
```

这里，`log()` 函数使用 `this.count` 来访问其所属对象中的属性。然而，有一个非常重要的点：`this` 的值直到函数被调用时才确定……并且该值通常被设置为调用该函数的对象，尽管这一点可以被覆盖。所以，在这个例子中，我们期望 `log()` 函数通过类似 `myObject.log();` 的方式被调用，在这种情况下，`myObject` 将是调用对象。

如果将一个函数用作回调，例如事件处理器，则调用对象很可能不是其所属对象。例如，如果该函数处理按钮的 `onclick` 事件，那么调用对象将是该按钮。

`call()` 函数允许你调用一个函数并指定 `this` 的值。我将用以下代码来演示：

```
function Vehicle(weight, cost) {
    this.weight = weight;
    this.cost = cost;
}

function Truck(weight, cost, axles, length) {
    Vehicle.call(this, weight, cost);
    this.axles = axles;
    this.length = length;
}

var tonka = new Truck(5, 25, 3, 15);
console.log(tonka);
```

你可能认识 `Vehicle()` 函数；这是一种创建对象的典型方式，通常通过如下语句调用：

```
var v = new Vehicle(5, 25);
```

这将创建一个具有两个属性 (`weight` 和 `cost`) 的对象，并用提供的值进行初始化。`Truck()` 函数类似，初始化了 `axles` 和 `length` 属性。但请注意该函数的第一行：

```
Vehicle.call(this, weight, cost);
```

这使用了 `call()` 函数来调用 `Vehicle()`，并向其传递了当前的 `this` 值，该值是对 `Truck` 对象的引用。当 `Vehicle()` 函数以这种方式被调用时，`weight` 和 `cost` 属性会被添加到 `Truck` 对象上，而不是 `Vehicle` 对象上。将新的 `tonka` 变量发送到控制台日志即可验证这一点。你应该会看到以下输出：

```
Truck {weight: 5, cost: 25, axles: 3, length: 15}
```

注意，`call()` 函数传入了三个参数。第一个参数被用作函数内部的 `this` 值。其余参数则被传递给被调用的函数。`apply()` 函数的工作方式与 `call()` 函数完全相同，唯一的区别是额外的参数以数组形式传递。以下两行代码实现相同的功能：

```
Vehicle.call(this, weight, cost);
// 或者
Vehicle.apply(this, [weight, cost]);
```

你可以任选其一使用，这只是一个偏好问题。然而，`apply()` 可以用于参数数量可变的列表。你可以动态构建数组，然后将其作为第二个参数传递给 `apply()`。

`call()` 和 `apply()` 函数都会立即调用它们所作用的函数。因此，当你执行 `Vehicle.call(...)` 时，`Vehicle()` 函数会被立即调用。如果你只想设置好调用但实际并不立即执行呢？例如，当你正在设置一个在点击事件发生时执行的回调函数。这可能会像这样：

```
myButton.click(myHandler);
```

正如我前面提到的，当 `myHandler()` 被调用时，`this` 将是按钮对象。如果你想要一个不同的 `this` 值，你不能使用 `call()` 或 `apply()`，因为你尚未调用该处理器；你只是在为事件触发时的调用做准备。要解决这个问题，可以像这样使用 `bind()` 函数：

```
myButton.click(myHandler.bind(newThis));
```

现在，当事件发生时，`myHandler()` 函数将被调用，并且其 `this` 值将是传入的值 `newThis`。你可以通过在 `this` 引用之后列出它们来向函数传递额外的参数，就像 `call()` 函数那样。



