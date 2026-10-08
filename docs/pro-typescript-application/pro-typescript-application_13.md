# 附录 1：JavaScript 快速参考

如果你还不熟悉 JavaScript，本快速参考旨在提供你将在 TypeScript 程序中使用的 JavaScript 核心功能的概述。在 TypeScript 语言旁边已经描述过的功能，此处不再赘述。

## 变量

变量用于在 JavaScript 中存储应用程序的状态，可以包含从字符串、数字到对象、函数等任何类型的数据。

列表 A1-1 展示了一些包含简单类型的变量。变量可以在声明的同时赋值，也可以分开声明和赋值。变量的类型会与其最近一次赋值的值相对应。如果未被赋值，变量的值将为 `undefined`。

声明变量时有两个关键字可供选择：`let` 和 `const`。这两个声明都是块级作用域的，就像大多数使用花括号的语言一样。使用 `let` 关键字声明变量时，你可以在后续代码中重新给该变量赋值。而使用 `const` 关键字时，则无法重新赋值。对 `const` 变量赋值的限制并不意味着它不可变，只是该变量本身不能被覆盖。在两者之间选择时，建议优先使用 `const`，当确定需要重新赋值时再升级为 `let`。`let` 和 `const` 都优于 `var` 关键字，因为 `var` 是函数级作用域，而非块级作用域。

```javascript
// 变量声明
let variable;

// 变量赋值
variable = 10;
```


# 变量

```javascript
variable = '一个字符串被赋给变量';

// 动态赋值（将变量类型改为数值）

variable = 10;
```

## 列表 A1-1. 变量

你可以在变量中存储数组。可以使用空字面量 `[]` 创建一个新的空数组，并使用 `array.push` 添加元素。也可以通过将值放入数组字面量中，一步完成数组的创建和填充，如列表 A1-2 所示。

```javascript
// 创建空数组并添加值

const myArray = [];

myArray.push(1);

myArray.push(3);

myArray.push(5);

// 使用数组字面量添加值

const myLiteralArray = [1, 3, 5];
```

## 列表 A1-2. 数组

对象可用于表示复杂结构的数据。对象可以包含属性，每个属性都像一个变量，可以包含字符串、数值、数组、函数以及其他对象。与数组类似，你可以使用空对象字面量 `{}` 创建空对象，也可以通过将属性放入对象字面量中，一步创建并填充对象。列表 A1-3 展示了这两种方式。

```javascript
// 对象

const myObject = {};

myObject.title = '示例对象';

myObject.value = 5;

// 对象字面量

const myLiteralObject = {

title: '示例对象',

value: 5

};
```

## 列表 A1-3. 对象

在所有示例中，我们都使用了字面量赋值，而非通过 `new` 关键字实例化值。虽然可以使用 `new Array()` 创建数组、`new Object()` 创建对象，甚至用 `new String()` 创建字符串，但在 JavaScript 中，字面量语法更受推荐。

# 函数

函数可用于在程序中创建可复用的代码块。你可以通过函数声明或函数表达式来创建函数，如列表 A1-4 所示。

```javascript
// 函数声明

function myFunction(name) {

return '你好 ' + name;

}

// '你好 Steve'

let greeting = myFunction('Steve');

// 函数表达式

var myFunctionExpression = function (name) {

return '嗨 ' + name;

};

// '嗨 Steve'

greeting = myFunctionExpression('Steve');
```

## 列表 A1-4. 函数

使用函数声明创建函数时，该函数在解析阶段被创建，这意味着它在整个程序中，只要作用域可访问的地方都能使用。而使用函数表达式时，函数在运行时才被求值，且只能在作用域可访问且调用代码位于函数表达式之后的位置进行调用。

# 条件语句

条件语句可用于在程序中实现逻辑分支。你可以使用条件语句仅在满足特定条件时执行代码。

`if` 语句允许根据自定义条件（计算结果为 `true` 或 `false`）进行代码分支。例如，列表 A1-5 中的 `if` 语句会根据 `age` 变量的值执行不同的代码。

```javascript
// If 语句

const age = 21;

if (age > 18) {

// 当 age 大于 18 时执行的代码

}

if (age > 40) {

// 当 age 大于 40 时执行的代码

} else if (age > 18) {

// 当 age 大于 18 但小于 41 时执行的代码

} else {

// 其他所有情况下执行的代码

}
```

## 列表 A1-5. If 语句

`switch` 语句适用于基于单个变量控制多个分支，且仅在特定值才会触发分支的场景。列表 A1-6 展示了一个典型的 `switch` 语句，它可以根据 `style` 变量的值执行不同的代码。如果没有其他条件被执行，则会执行 `default` 条件。

```javascript
// Switch 语句

const styles = {

传统: 1,

现代: 2,

后现代: 3,

未来主义: 4

};

const style = styles.传统;

switch (style) {

case styles.传统:

// 为传统风格执行的代码

break;

case styles.现代:

// 为现代风格执行的代码

break;

case styles.后现代:

// 为后现代风格执行的代码

break;

case styles.未来主义:

// 为未来主义风格执行的代码

break;

default:

throw new Error('未知风格: ' + style);

}
```

## 列表 A1-6. Switch 语句

> **注意**
> `switch` 语句与 TypeScript 枚举配合使用效果更佳，枚举将在第 1 章中介绍。

# 循环

循环用于在程序中重复执行一段代码。JavaScript 中最常见的循环是 `for` 循环，可用于遍历数组中的每一项并重复执行操作，如列表 A1-7 所示。

```javascript
const names = ['莉莉', '丽贝卡', '黛比', '安'];

for (let i = 0; i < names.length; i++) {

console.log(names[i]);

}
```

## 列表 A1-7. For 循环

你可以使用 `for-of` 循环简化列表 A1-7 中的代码。`for-of` 循环无需用变量跟踪索引值，直接将每次迭代的值赋给一个变量供你使用。列表 A1-8 展示了与第一个示例等价的 `for-of` 循环；其结果更具表现力且更易读。

```javascript
const names = ['莉莉', '丽贝卡', '黛比', '安'];

for (let name of names) {

console.log(name);

}
```

## 列表 A1-8. For-of 循环

`while` 循环允许重复执行一段代码，直到某个条件满足为止，如列表 A1-9 所示。这种循环常见的用途是向字符串中添加字符，并重复该过程直到字符串达到指定长度。

```javascript
let counter = 10;

while (counter > 0) {

counter--;

console.log(counter);

}
```

## 列表 A1-9. While 循环

`do-while` 循环与 `while` 循环几乎相同，区别在于它至少会执行一次代码，即使条件不满足。

```javascript
let counter = 0;

do {

counter--;

console.log(counter);

} while (counter > 0);
```

## 列表 A1-10. Do-while 循环

列表 A1-10 类似于前面展示的 `while` 循环，只是 `counter` 变量已经满足了退出条件（因为它不大于零）。尽管如此，代码仍会在条件评估前执行一次，从而导致循环退出。

# 字符串

虽然大量的字符串操作可能暗示着设计上的错误，但 JavaScript 中处理字符串的方式有一些值得了解的显著发展。列表 A1-11 展示了处理字符串的几种机制。

```javascript
const name = 'Jamie';

// 传统字符串

const classicString = 'Line One\n' +

'Line Two\n' +

'Line Three. Hello ' + name + '!';

// 反引号字符串

const backtickedString = `Line One

Line Two

Line Three. Hello ${name}!`;

// 'Five plus seven is 12.'

const expressionString = `Five plus seven is ${5 + 7}.`;
```

## 列表 A1-11. 字符串

传统字符串可以使用单引号或双引号。反引号字符串接受换行符，并允许使用 `${}` 包围的字符串插值。字符串插值允许插入变量和表达式。

# 期约（Promise）

期约正迅速成为 JavaScript 应用程序的核心特性，并为协调异步操作提供了一种简单的机制。列表 A1-12 展示了如何使用 `fetch` API 从 URL 检索数据，来通过期约进行编程。

```javascript
fetch('/Your/URL/').then(function (response) {

// 收到响应

if (response.status >= 200 && response.status < 300) {

// 我们得到了一个成功状态码

}

}).catch(function (error) {

// 请求失败

});
```

## 列表 A1-12. 使用期约

一个期约包含三个部分：一个调用返回期约的函数（例如 `fetch` 函数）、一个用于处理 `then` 结果（成功时调用）的处理程序，以及一个用于处理 `catch` 结果（发生错误时调用）的处理程序。

let promise = new Promise(function (resolve, reject) {
    window.setTimeout(function () {
        if (true) {
            resolve('Success');
        } else {
            reject('Failed');
        }
    }, 1000);
});

promise.then(function (message) {
    alert(message);
}).catch(function (error) {
    alert(error);
});

## 列表 A1-13. 使用自定义期约

当你使用`new Promise`实现自己的期约时，你的函数应接受一些处理程序作为参数，这些处理程序将在成功或失败时被调用。这些处理程序是一种松散的约定；传递给`resolve`和`reject`函数的参数可以是任何类型，尽管按照惯例，`reject`函数应传入一条错误消息。

## 总结

虽然本附录快速介绍了 JavaScript 的一些非常基础的功能，但它们是你开始学习 TypeScript 所需掌握的核心部分。如果某个功能与 TypeScript 语言有重要关联，我会将其包含在 TypeScript 的相关信息中，而不是放在本附录里。JavaScript 及其在不同执行环境中的内容远比我在此处能介绍的要多得多。

阅读完这个快速入门后，你应该能够毫无障碍地阅读本书，因为所有 TypeScript 语言特性都已详细描述。
