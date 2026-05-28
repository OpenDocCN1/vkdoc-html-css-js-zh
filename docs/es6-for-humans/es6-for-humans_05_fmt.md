# ES6 中的新语法

`ECMAScript 6` 引入的新特性代表了未来 JavaScript 应用程序构建的基础。在本章中，我们将探讨`ES6`语法的变化和新增内容，这些内容有助于修复之前 JavaScript 版本中存在的许多问题。我们将详细讨论声明变量和定义作用域的新方式。我们还将介绍新概念，如箭头函数、默认函数参数、展开与剩余运算符以及对象字面量扩展。

## 变量声明：let、const 与块级作用域

过去，JavaScript 中的所有变量都使用关键字`var`声明。这些变量是函数作用域的，意味着它们的作用域受限于包含它们的函数，这对于来自其他语言的开发者来说有时会造成混淆。因此，如果需要创建一个具有自身作用域的新块，你必须将所需代码包裹在一个常规函数或立即调用函数表达式中。

以下是常规函数级作用域的示例：

```
var price = 10; // 全局声明
function showPrice() {
var price = 12; // 使用 var 的局部声明
console.log(price); // 12
}
showPrice();
console.log(price); // 10
```

以下是使用 IIFE 的函数级作用域示例：

```
var price = 10; // 全局声明
(function () {
var price = 12; // 使用 var 的局部声明
console.log(price); // 12
})();
console.log(price); // 10
```

上述代码表明，变量 `price` 现在的作用域限定在包含它的函数内，并且变更不会泄露到父作用域（在本例中为全局作用域）。新值 `price = 12` 仅在包含它的函数作用域内可用。

如果我们用块作用域（‘if’块）替换函数作用域，代码如下：

```
var price = 10;
if (price) {
price = 12;
console.log(price); // 12
}
console.log(price); // 12
```

上述代码清楚地表明，‘if’块内部的变更泄露到了父作用域，这告诉我们`var`声明绑定的是函数作用域，并不会创建块级作用域。

在`ES6`之前，JavaScript 使用函数作用域，但在大多数编程语言中，块级作用域比函数作用域更常见。随着`ES6`的到来，我们现在有了两种额外的变量声明方式：`let`和`const`，它们声明的变量都是块级作用域的。

### 使用 `let` 和 `const` 进行块级作用域

简单来说，块级作用域是指在一对 `{ }` 之间创建了一个新的作用域。使用关键字 `let` 和 `const` 声明的变量只存在于包裹它们的最内层块中。

在下面的示例中，你认为执行这段代码片段时控制台会打印什么？

```
let nbr = 42;
{
let nbr = 1000;
}
console.log(nbr);
```

控制台会打印值 `42`，因为第二个 `nbr` 变量的作用域限定在其声明的块内部，不会影响块外部的 `nbr` 变量，外部变量仍为 `42`。通常，你不会这样使用一个块，除非它位于像 `if` 条件或循环这样的控制流语句中，但这解释了变量如何实现块级作用域。

我们现在知道，`var` 受函数作用域约束，而 `let` 和 `const` 则是块级作用域。这意味着如果你有一组花括号（一个代码块），你就有一个块级作用域。但需要注意的是，在每个块级作用域中，你只能声明一次变量。

另一方面，与 `let` 不同，`const` 创建的是不可变的变量。使用 `const` 创建的变量必须在声明时赋值，并且之后在程序中不能更改。

请看下面的例子：

```
const value = 42;
console.log(value);   // 42
value = 1000;         // TypeError
```

**注意**

尝试更改 `const` 变量的值会抛出 `TypeError`。在严格模式下更改不可变绑定只会引发异常 `SetMutableBinding()`。

请确保始终为使用 `const` 声明的变量初始化一个值；否则会抛出错误。请看下面的例子：

```
const item;  // SyntaxError: Missing initializer in const declaration
```

如果你需要一个值为 `undefined` 的常量，你仍然需要这样做：

```
const value = undefined;
```

如果你知道变量的值在整个代码中不会改变，你应该使用 `const`；否则使用 `let` 来声明变量。我们建议摒弃使用 `var` 声明变量的做法，因为如果你使用块级作用域，代码会更清晰、更高效，并且易于调试。`let` 和 `const` 避免了误解的来源，尤其是对于习惯了具有块级作用域语言的程序员来说。如果你试图在声明变量的块外部访问由 `let` 和 `const` 声明的变量，它们会抛出异常，并且不会发生提升，这有助于你限定代码片段的影响范围。

请参考以下示例，理解提升如何影响使用关键字 `var` 声明的变量：

```
console.log(nbr);  // undefined
var nbr = 42;
```

由于提升，上述例子会在控制台打印 `undefined`。当我们使用 `var` 声明变量 `nbr` 时，由于提升，这等同于在作用域的开头声明该变量，并将其值设置为 `undefined`。但是，如果你将 `console.log` 语句移动到声明之后，它会打印数字 `42`，这是因为进入使用 `var` 声明的变量的作用域（即其外围函数）会创建一个绑定。然后通过将值设置为 `undefined` 来初始化该变量。当执行到达声明处时，该变量会被设置为赋值语句中指定的值。

让我们看一个使用 `let` 的类似例子：

```
console.log(nbr); // Reference Error: nbr is not defined
let nbr = 42;
```

这实际上会给你一个 `Reference Error: nbr is not defined`，因为这里没有发生提升。这有助于调试由变量在声明之前被使用引起的各种类型的错误。这种 `Reference Error` 在技术上被称为暂时性死区（TDZ）错误，因为你正在访问一个已声明但尚未初始化的变量。

### 关于暂时性死区的更多内容

本讨论的主要收获是，`let` 和 `const` 声明的变量具有暂时性死区（TDZ），这与使用 `var` 时发生的提升形成对比。暂时性死区中的变量尚未用任何值初始化。当变量被声明时，会创建一个内存绑定，但保持未初始化状态。此时尝试获取或设置该变量会引发 `Reference Error`。当程序流到达声明处时，该变量会被设置为语句中指定的值；如果语句中没有赋值，则设置为 `undefined`。

`const` 的工作方式类似，唯一的区别是它需要在声明时提供一个初始值，该值之后不能更改。

```
let data = true;
if (true) { // 进入新作用域，TDZ 开始
// 为 "data" 创建未初始化的绑定
console.log(data); // ReferenceError
let data; // TDZ 结束，"data" 被初始化为 "undefined"
}
console.log(data); // true
```

一旦通过给变量赋值完成初始化，TDZ 就结束了。

暂时性死区的主要目的是捕获错误。你不应该能在声明变量之前访问它，即使你意外访问了，也应该得到警告。TDZ 是让 `const` 按现有方式工作的最佳解决方案，而让 `let` 也拥有 TDZ 使得在两者之间切换非常容易。TDZ 帮助我们确保变量在运行时始终具有正确的值。

```
if (true) {
console.log(typeof anUndeclaredVariable); // 'undefined'
console.log(typeof random); // ReferenceError (TDZ)
let random;
}
```

即使你尝试使用 `typeof` 访问暂时性死区中的变量，也会得到异常。`typeof` 是检查变量是否存在的安全方法。但如果一个变量稍后使用 `let` 在代码中声明，那么当你调用 `typeof` 时，它会抛出一个 TDZ 错误，因为该变量正处于暂时性死区中。因此，一个良好的实践是始终将变量声明放在作用域的顶部。这项检查对于使用 `var` 有条件地创建全局变量也很有用。你可以通过以下方式检查全局变量是否存在：

```
if (typeof globalVariable === 'undefined') {
var globalVariable = { ··· };
}
```

**注意**

`const` 用于声明一个不可变的变量，但这并不会使变量中包含的值变成不可变的。

在以下代码片段中，`obj` 是一个常量，但它所指向的值是可变的；因此你可以向其添加属性，但无法为 `obj` 赋另一个值。

### 对象冻结

```javascript
const obj = {};
obj.key = 42;
console.log(obj.key); // 42
obj = {}; // TypeError
```

如果你愿意，可以通过冻结来使值本身变成不可变的。

```javascript
const obj = Object.freeze({});
obj.key = 42;
console.log(obj);   // {}
```

请记住，`Object.freeze()` 是浅层冻结。它只会冻结传递给它的对象的直接属性。只有对象的第一层属性变得不可变，而存储在其属性中的对象则不会。

### 循环中的变量声明

`for` 循环（`for`、`for-in`、`for-of`）允许你在循环头部声明变量。但使用 `var`、`let` 或 `const` 声明这些变量的方式会改变一些事情。我们来逐一分析这些情况。

在基本的 `for` 循环中，使用 `var` 会为该变量创建一个单一绑定：

```javascript
let arr = [];
for (var i=0; i < 3; i++) {
arr.push(function () { return i });
}
let value = arr[0]();
console.log(value); // 3
```

你可能期望输出结果是 0，但实际输出是 3，因为循环结束时，变量 `i` 上形成了一个闭包。`i` 被设置为 3，并且函数体中的每个 `i` 都指向同一个绑定。因此，该函数始终返回 3。现在，我们来看看使用 `let` 声明 `i` 的情况：

```javascript
let arr = [];
for (let i=0; i < 3; i++) {
arr.push(function () { return i });
}
let value = arr[0]();
console.log(value); // 0
```

当我们在 `for` 循环中使用 `let` 时，循环的每次迭代都会获得自己的 `i` 变量，并且任何闭包都会封闭其自身的 `i` 值。

对于 `const` 来说，它的工作方式类似于 `var`，因为赋给 `const` 变量的初始值不会再次改变。

```javascript
for (const i=0; i<3; i++) {
console.log(i);
}
// TypeError: Assignment to constant variable (due to i++)
```

### 函数参数与全局作用域中的变量声明

如果你在函数内部使用 `let` 声明一个与参数同名的变量，ES6 将会抛出一个静态（加载时）错误。

```javascript
function fn(param) {
let param; // SyntaxError: Identifier 'param' has already been declared
}
```

使用 `var` 做同样的事情则不会产生任何效果，因为这等同于重新声明一个变量。解决这个问题的另一种方法是在块内使用 `let`，但新声明的变量只会隐藏（shadow）参数：

```javascript
function fn(param) {
{
let param;
}
}
```

JavaScript 中的全局对象（浏览器中的 `window` 和 Node.js 中的 `global`）一直令人困惑。这就是为什么在 ES6 中引入了一个区分。请考虑以下示例：

```javascript
let notGlobal = "hello";
var isGlobal = "what up";
{ console.log(notGlobal); } // hello
{ console.log(isGlobal); } // what up
global.isGlobal //'what up'
global.notGlobal // undefined
```

全局对象的所有属性都是全局变量。因此，在全局作用域中，`var` 声明和函数声明会创建此类属性。但是，使用 `let` 和 `const` 声明的全局变量不会成为全局对象的属性。

### 箭头函数

箭头函数是 ES6 中另一项主要的语法更新。箭头函数是使用一种新语法（即“胖”箭头 `=>`）定义的函数。该箭头基于 CoffeeScript 中的类似概念。通过省略 `function` 和 `return` 关键字，它们有助于使代码更易读，并以与函数执行相同的方式解读。在本节中，我们将详细讨论如何使用箭头函数以及何时使用它们是有意义的。

箭头函数的基本语法如下：

```javascript
var fn = data => data;
```

赋值语句左侧的第一部分是提供给函数的参数。如果只有一个参数，则不需要任何额外的语法。接下来的部分是箭头，然后是即将返回的表达式。在这里，它只是返回参数。该函数实际上等同于：

```javascript
var fn = function(data) {
return data;
};
```

考虑另一个示例：

```javascript
let getNumber = () => 42;
console.log(typeof getNumber); // function
console.log(getNumber()); // 42
```

在上面的代码中，我们声明了一个名为 `getNumber` 的新函数，并使用箭头函数进行赋值。空括号 `()` 表示该函数没有参数，最后，函数返回值 42。在箭头函数中，我们可以省略 `return` 关键字。只要箭头后指定的表达式没有用 `{}` 包裹，它就会被返回。

```javascript
var getPrice = (quantity, tax) => (quantity * 5) * (1 + tax);
console.log(getPrice(2, .095)); //  10.95
```

请注意，如果恰好只有一个参数，可以省略括号 `()`，但当参数为零个或多个时，则始终需要使用括号。

如果要指定一个包含多个表达式的更传统的函数块，则需要将函数体包裹在花括号中。但是你需要特别使用 `return` 关键字来指定返回值。请查看以下代码片段以获取示例：

```javascript
var getPrice = (quantity, tax) => {
let price = (quantity * 5)
price *= (1 + tax);
return price;
}
console.log(getPrice(2, .095)); //  10.95
```

花括号表示函数体。如果你希望箭头函数在函数体外返回一个对象字面量，则必须将该字面量包裹在括号中。例如：

```javascript
var getNumber = data => ({ data: "check", number: 42 });
// 实际上等同于：
var getNumber = function(data) {
return {
data: "check",
number: 42
};
};
```

用括号包裹的对象字面量表明这些花括号是一个对象字面量，而不是函数体。

#### 使用箭头函数创建 IIFE

JavaScript 中的函数可用于创建立即调用函数表达式（IIFE）。你可以定义一个匿名函数并立即调用它，而不保留对它的任何引用。这是一种将表达式与程序其他部分隔离开的有效模式。

```javascript
var fn = function(number) {
return {
getNumber: function() {
return number;
}
};
}(42);
console.log(fn.getNumber());      // 42
```

在上面的代码中，使用了一个 IIFE 来创建 `getNumber()` 方法，该方法使用 `number` 参数作为返回值，确保 `number` 属性实际上是返回对象的一个私有成员。使用箭头函数也可以实现非常类似的效果，只需将其包裹在括号中即可。

```javascript
var fn = ((number) => {
return {
getNumber: function() {
return number;
}
};
})(42);
console.log(fn.getNumber());      // 42
```

需要注意的一个区别是，箭头函数是函数表达式，而不是函数声明。它们是匿名的函数表达式，为了递归、事件绑定或解绑的目的，没有命名的引用。

箭头函数的优点不仅仅是漂亮和更清晰的语法。箭头函数确实可以为我们节省几行代码和几个字符，但箭头函数的真正目的是处理函数内部的 `this` 关键字。`this` 在箭头函数内部有不同的行为。但在我们讨论这一点之前，在下一节中，让我们先看看 `this` 关键字通常是如何工作的。

## 关于 this 的故事

在 JavaScript 中，`this` 是函数的当前执行上下文。让我们看看它在以下场景中的含义：

### 1. 函数调用

在 Chrome 中调用 `getContext()` 函数将在控制台中打印 `Window` 对象。这是因为 `getContext()` 的上下文是 `Window/Global 对象`。在调用 `getContext()` 时，JavaScript 会自动将 `this` 设置为 `global` 对象，在浏览器中即 `Window`。

# JavaScript 中的 `this` 关键字

当 `this` 在任何函数作用域之外使用时，它同样指的是全局作用域。请检查以下代码片段：

```
if (this === window) {
    console.log("this 指的是全局上下文");
}
```

**注意：** 如果你处于严格模式（`"use strict"`）下，`this` 将为 `undefined`。

## 方法调用

方法调用是指调用对象的方法，在这种情况下，`this` 指向方法调用中拥有该方法的对象。

```
let myObj = {
    name: 'fancy',
    operation: function() {
        console.log(this);
    }
}
myObj.operation(); // { name: 'fancy', operation: [Function: operation]}
```

方法调用，即调用 `myObj.operation()` 时，控制台会打印出 `[myObj object]`。

现在，让我们尝试以下代码：

```
let x = myObj.operation;
x(); // Window
```

此处，`x` 引用了 `[myObj object]` 中的 `operation()` 方法。调用 `x()` 意味着我们进行的是函数调用而非方法调用，因此，`this` 将指向 `Global`（或 `Window`）对象。

如果我们想使用 `[myObj object]` 来调用 `x`（进行方法调用），就必须使用 `.call()` 方法：

```
let x = myObj.operation;
x(); // Window
x.call(myObj); // { name: 'fancy', operation: [function]}
```

## 构造函数调用

构造函数调用发生在使用 `new` 关键字创建对象时。请考虑以下示例：

```
function Employee(name, department, salary) {
    this.name = name;
    this.department = department;
    this.salary = salary;
    console.log("Welcome " + this.name + "!");
}
let john = new Employee('John', 'Sales', 4000);
// Welcome John!
```

`new Employee('John', 'Sales', 4000)` 是对 `Employee` 函数的构造函数调用。执行结果是一个新对象，并且 `this` 指向这个新创建的对象。

构造函数 `Employee()` 可以用 ES6 中的 class 来编写，我们将在本书后面讨论。

## 箭头函数中的 `this`

让我们看看 `this` 在箭头函数内部是如何工作的。箭头函数旨在词法上绑定上下文，这意味着 `this` 指向定义箭头函数时的封闭上下文。与普通函数不同，箭头函数不会创建自己的执行上下文，而是从定义它的外部函数中获取 `this`。请考虑以下代码：

```
function Employee(firstName, department, salary) {
    this.firstName = firstName;
    this.department = department;
    this.salary = salary;
    this.getInfo = function() {
        // 外部函数上下文 = Employee 对象
        return function () {
            // 内部函数上下文 = Global 对象
            console.log(this.firstName + " from " +
                this.department + " earns " + this.salary);
        };
    }
}
let jim = new Employee('Jim', 'Finance', 5200);
```

在上面的示例中，我们创建了一个名为 `Employee` 的构造函数，并使用该构造函数和 `new` 关键字创建了一个名为 `jim` 的新员工对象。为了打印员工信息，我们需要使用 `jim.getInfo()` 返回的函数。请查看以下代码片段：

```
let printInfo = jim.getInfo();
printInfo();  // undefined from undefined earns undefined
```

这里，`printInfo` 指向内部函数。由于我们只是进行函数调用，`this` 指向 `Global` 对象，该对象没有任何 `Employee` 属性，因此每当使用 `this` 上的属性时都会产生 `undefined`。

让我们看看如果用箭头函数替换内部函数，`this` 的行为会有什么不同：

```
function Employee(firstName, department, salary) {
    this.firstName = firstName;
    this.department = department;
    this.salary = salary;
    this.getInfo = function() {
        // 外部函数上下文 = Employee 对象
        return () => {
            // 内部函数上下文 = 包围它的上下文 = Employee 对象
            console.log(this.firstName + " from " +
                this.department + " earns " + this.salary);
        };
    }
}
let jim = new Employee('Jim', 'Finance', 5200);
let printInfo = jim.getInfo();
printInfo();  // Jim from Finance earns 5200
```

在这种情况下，`this` 关键字指向的是包围箭头函数的函数的上下文，而不是之前指向 `Global` 对象的情况。这里需要注意的是，箭头函数不会在调用时改变它们的上下文。请考虑以下示例：

```
function Employee() {
    this.firstName = 'Mike',
    this.department = 'HR',
    this.salary = 4500,
    this.getContext = () => {
        console.log(this);
    }
}
let mark = new Employee();
mark.getContext(); // [Employee object]
let context = mark.getContext;
context(); // [Employee object] (无论是否是函数调用)
```

在上面的示例中，箭头函数的上下文在声明时就被设置，并且无法更改。这里需要注意的是，你无法“重新绑定”一个箭头函数。上下文始终是固定的。请查看以下代码片段以获取示例：

```
var details = {
    number: 42,
    operation: function () {
        return () => console.log(this.number);
    }
};
var details2 = {
    number: 84
};
details.operation().bind(details2)(); // 42
```

在上面的示例中，我们将 `details2` 的 number 设置为 84。但知道我们无法将新对象绑定到箭头函数。引擎不会抛出任何错误，它只是完全忽略了 `bind`。因此，即使我们用 `details2` 对象调用 `operation` 方法，也会打印出 42。这也适用于 `call` 和 `apply`。因此，对于箭头函数，调用 `bind`、`call` 或 `apply` 将无法改变 `this` 的值。

从这个例子中可以清楚地看出，函数内部 `this` 关键字的值是无法更改的。函数内部 `this` 的值将保持不变，但你仍然可以在箭头函数上使用 `bind`、`call` 和 `apply`。

```
var product = (x, y) => x * y;
console.log(product.call(null, 2, 3));           // 6
console.log(product.apply(null, [2, 3]));        // 6
var multiply = product.bind(null, 2, 3);
console.log(multiply ());                        // 6
```

除了词法上的 `this`，箭头函数还具有词法上的 `arguments`。它们没有自己的 `arguments` 数组，而是从父级继承。箭头函数没有 `this`、`super`、`arguments` 和 `new.target` 绑定，因此函数内部的 `this`、`super`、`arguments` 和 `new.target` 的值是最近的包含非箭头函数里的相应值。

**注意**

通常情况下，在 JavaScript 中，代码之间的额外换行符没有任何意义，代码仍然可以正常工作，但对于箭头函数来说，这是一个问题。我们不能将箭头符号放在新行上。ES6 禁止在箭头函数的参数定义和箭头之间出现换行。

箭头函数的另一个重要方面是，它们不能与 `new` 一起调用，因为它们没有构造方法，也不能用作构造函数。当与 `new` 一起使用时，它们会产生错误。请查看下面的示例：

```javascript
var newFn = () => {},
object = new newFn();  // 错误 - 你不能对箭头函数使用 'new'
```

通常，在 ES5 中声明函数时，它有一个用于构造函数的 `prototype` 属性。但由于你不能对箭头函数使用 `new`，因此也就不需要 `prototype`。因此，使用箭头函数声明的函数无法访问 `prototype` 字段。

```javascript
var details = () => 42;
console.log(details.hasOwnProperty("prototype")); // false
```

与严格模式下不允许重复命名参数的非箭头函数类似，箭头函数无论在严格模式还是非严格模式下都不能有重复的命名参数。

好的，作为高级文档工程师和翻译员，我将严格遵循您提供的格式和注意事项，对给定的英文文本进行翻译。

## 使用箭头函数

当你有一个简短的单行内联函数表达式，且有计算后的返回值，并且该函数没有使用 `this` 引用或自引用时，你就可以用箭头函数替换它。

如果你有函数使用 `var self = this` 这种技巧来处理 `this` 问题，或者调用 `.bind(this)` 来实现正确的 `this` 绑定，那么箭头函数正是为解决此问题而生的。例如，考虑以下代码片段，我们在点击 `btn` 后等待 1 秒钟来切换 `btn-active` 类。

```javascript
$('.btn').on('click', function() {
var self = this;
setTimeout(function() {
$(self).toggleClass('btn-active');
}, 1000);
});
```

如果没有箭头函数，我们需要将上下文存储在一个变量中，以便能够在 `setTimeout` 函数内部访问它。使用箭头函数可以简单地重写为：

```javascript
$('.btn').on('click', function() {
setTimeout(() => {
$(this).toggleClass('btn-active');
}, 1000);
});
```

因此，除了更短、更简洁的语法外，箭头函数对 `this` 关键字的处理方式也略有不同，使得在许多情况下代码更易于管理。

## 默认函数参数

与其他语言中常见的模式不同，JavaScript 中的函数很独特，它允许你传入任意数量的参数来调用它们，而不受函数定义中声明的参数数量的限制。这为开发人员提供了一个机会，可以在未指定参数时为参数使用任意值。在 ES5 中，如果未指定参数，其值将被设置为 `undefined`。为未指定参数设置默认值的常见模式如下：

```javascript
function getSum(a,b) {
a = (a !== undefined) ? a : 1;
b = (b !== undefined) ? b : 41;
console.log( a + b );
}
getSum();            // 42
getSum(1, 2);        // 3
getSum(10);          // 51
getSum(null, 6);     // 6
```

ES6 试图通过允许我们在函数声明语句中为参数设置默认值来简化此过程。

```javascript
var getSum2 = function(a = 1, b = 41) {
console.log(a + b);
}
getSum2();            // 42
getSum2(1, 2);        // 3
getSum2(10);          // 51
getSum2(null, 6);     // 6
```

当我们调用函数时，如果未指定任何参数，则会使用参数的默认值。指定默认值的语法是参数后跟等号 `=` 和一个表达式。

```javascript
var getAnswer = function(number = 42, item = "universe") {
console.log(number + " is the answer to " + item);
}
getAnswer(undefined, "life"); // 42 is the answer to life
```

在这里，我们将 `undefined` 作为第一个参数传递，ES6 会使用默认值 42 代替。

函数默认值可以不仅仅是像 42 这样的简单值；它们可以是任何有效的表达式，甚至是函数调用。你可以使用复杂的表达式作为参数的默认值。默认值表达式是惰性求值的，这意味着它们仅在参数的参数不存在或是 `undefined` 时才运行。

```javascript
var getName = function(firstName = "John", lastName = "Doe") {
console.log(firstName + " " + lastName);
}
getName("Jane"); // Jane Doe
```

你还可以在用作默认值的表达式中访问其他变量。

```javascript
var defaultName = "John";
var getName = function(firstName = defaultName, lastName = "Doe") {
console.log(firstName + " " + lastName);
}
getName(); // John Doe
```

在给参数指定默认值时，你也可以调用一个函数。

```javascript
var getFirstName = () => "John";
var getName = function(firstName = getFirstName(), lastName = "Doe") {
console.log(firstName + " " + lastName);
}
getName(); // John Doe
```

在上面几个例子中，如果我们尝试检查参数个数：

```javascript
var getName = function(firstName, lastName = "Doe") {
console.log(arguments.length);
}
getName("John"); // 1
```

即使第二个参数有默认值，`arguments.length` 也只返回实际传递给函数的参数个数。

让我们看一个稍微复杂一点的例子：

```javascript
var getPrice = function(quantity = price, price = 5) {
console.log(quantity + ", " + price);
}
getPrice(); // ReferenceError: price is not defined
```

把函数声明和参数看作一个作用域。函数声明的参数在括号 `(...)` 之间有它们自己的作用域。不要将其与函数体作用域混淆。在求值 `quantity` 时，JavaScript 尚未执行到 `price` 的声明，因此它不知道 `price` 的值。在默认值表达式中引用一个标识符时，会先在形式参数的作用域内匹配，然后才查找外部作用域。

就像我们在关于 `let` 声明的部分中讨论的那样，ES6 有一个暂时性死区（TDZ），不允许在变量初始化之前访问它。因此，在上面的例子中，当你尝试在声明之前使用参数时，会发生暂时性死区引用错误。

ES6 默认参数的另一个很棒的特性是，即使在创建动态函数时它们也能正常工作：

```javascript
var getNumber = new Function("number = 42", "return number;");
console.log(getNumber()); // 42
```