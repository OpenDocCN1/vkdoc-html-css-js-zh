# 性能调优

## 测量阶乘函数的执行速度

`computeFactorial(100)`; // 执行耗时约 945 毫秒
`computeFactorial(50)`; // 执行耗时约 506 毫秒
`computeFactorial(100)`; // 执行耗时 0–1 毫秒 – 使用缓存值
`computeFactorial(50)`; // 执行耗时 0–1 毫秒 – 使用缓存值

## 使用高效的字符串拼接

目前为止，将多个字符串拼接成更长的字符串最常用的方法是使用 `+` 运算符，如下所示：

```
var outputString = "start text " + inputString + " end text";
```

当你在执行大量字符串拼接操作时（例如在代码循环中），使用 `+` 运算符完成整个拼接系列所消耗的时间会累积成严重的性能损失，尤其在 IE 浏览器中。

你可以采用另一种字符串拼接技术，它不会遭受同样的性能损失。该方案涉及使用数组，如下所示：

```
var myArray = [];
myArray.push('Welcome, ');
myArray.push(userName);
myArray.push(', to the site.');
myArray.join('');
```

在某些情况下，使用这种数组方式在 IE 浏览器中能将字符串拼接性能提升高达 25 倍，相比于使用 `+` 运算符。

## 使用正则表达式

正则表达式提供了一种有用、强大且高效的方式来执行字符串操作和模式匹配——速度比其他任何方法都快。在 JavaScript 中，正则表达式可以通过两种不同的方式定义：字面量形式或使用对象构造函数，如下所示：

```
// 以下两个正则表达式都旨在查找被搜索字符串中的任何大写字母实例。
var expression = new RegExp("[A-Z]", "g"); // 慢 – 需要先处理
var expression = /[A-Z]/g; // 快 – 可直接使用

// 输出 "_est _tring"
alert("Test String".replace(expression, "_"));
```

`RegExp` 对象构造函数接受一个字符串并将其转换为正则表达式，而字面量形式无需任何额外处理即可直接使用。这使得正则表达式的字面量形式成为使用 JavaScript 搜索字符串的最快方式。

避免使用 `RegExp` 构造函数来创建正则表达式，除非你绝对需要动态生成这个表达式。你可以在 [`www.evolt.org/article/Regular_Expressions_in_JavaScript/17/36435/`](http://www.evolt.org/article/Regular_Expressions_in_JavaScript/17/36435/) 找到一份关于 JavaScript 正则表达式非常详尽的在线教程。

## 更快速、更高效地循环

大多数 JavaScript 代码都充满了循环。通常，你需要处理数组或遍历对象字面量，以对其中存储的数据进行计算或操作。在 JavaScript 中，循环遍历数据是一项众所周知的慢任务，尤其在 IE 6 和 7 中。下面展示了一个典型的 JavaScript `for` 循环，以及同一循环的一个略有不同但效率却高得惊人的版本。

```javascript
var myArray = [10, 20, 30, 40, 50, 60, 70, 80, 90, 100];

// 最常见的循环类型
for (var index = 0; index < myArray.length; index++) {
	// 每次循环迭代时，都必须重新计算 myArray.length 的值，以确保其自上次迭代以来未发生变化
	// —— 这很慢
}

// 同一循环的略有不同但速度更快的版本
for (var index = 0, length = myArray.length; index < length; index++) {
	// myArray.length 的值只计算一次并存储在一个变量中。
	// 每次迭代时从该变量中读取值，而不是重新计算 —— 快得多！
}
```

你可以使用两个 JavaScript 命令来管理循环：

- `break` – 停止当前循环的执行，继续执行循环后面的代码。
- `continue` – 停止当前循环的本次迭代，并进入下一次迭代。

如果已达到目标，你可以使用这些命令来有效地停止循环继续迭代。



