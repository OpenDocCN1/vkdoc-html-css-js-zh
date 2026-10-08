# 第 3 章 ■ JavaScript 要点

`map()` 方法会创建一个新数组，其中包含原始数组中每个元素对应的一个元素。它遍历数组中的元素，调用指定的回调函数。该函数会返回一个新元素，该元素是原始元素经过某种转换后的结果。例如，你可能希望将一个字符串数组转换为大写形式。

为了使用之前的 `numbers` 数组进行测试，我们可以实现 `isOdd()` 函数：

```javascript
function isOdd(element, index, array) {
  if (isNaN(element)) { return false; }
  if (element % 2 != 0) { return true; }
  return false;
}
```

该函数返回一个布尔值：如果数字是奇数则返回 `true`，否则返回 `false`。当将此函数传递给 `map()` 方法时，你将获得一个由布尔值组成的新数组：

```javascript
var boolArray = numbers.map(isOdd);
console.log(boolArray);
```

`every()` 和 `some()` 方法与 `find()` 和 `filter()` 方法类似，都需要你提供一个返回布尔值的回调函数，该函数会对数组中的每个元素进行调用。`find()` 方法返回第一个使函数返回 `true` 的元素，而 `filter()` 方法返回所有使函数返回 `true` 的元素。

`every()` 方法在函数对数组中的每个元素都返回 `true` 时返回 `true`。`some()` 方法则在函数对数组中的任意（部分）元素返回 `true` 时返回 `true`。可以尝试以下代码进行测试：

```javascript
console.log(numbers.some(isOdd)); // 返回 true
console.log(numbers.every(isOdd)); // 返回 false
```

## 处理

有一些有用的方法可以对数组中的元素执行某些处理操作。我们从最简单的 `forEach()` 方法开始。

`forEach()` 方法会遍历数组中的所有元素，并调用指定的函数。你必须定义将要执行的函数，并将该函数作为参数传递给 `forEach()` 方法。与我们之前看到的其他回调函数类似，该函数有一个必需参数，即当前正在执行操作的特定元素。此外，还有两个可选参数：元素的索引以及正在被处理的数组本身。该函数可能如下所示：

```javascript
function process(item, index, array) {
  console.log("[" + index + "]: " + item + ", in array ", array.toString());
}
```

然后，调用 `forEach()` 方法，添加以下代码：

```javascript
arr.forEach(process);
```

控制台输出应该如下所示：

```
[0]: white, in array white,red,green,blue,yellow,orange,purple,black
[1]: red, in array white,red,green,blue,yellow,orange,purple,black
[2]: green, in array white,red,green,blue,yellow,orange,purple,black
[3]: blue, in array white,red,green,blue,yellow,orange,purple,black
[4]: yellow, in array white,red,green,blue,yellow,orange,purple,black
[5]: orange, in array white,red,green,blue,yellow,orange,purple,black
[6]: purple, in array white,red,green,blue,yellow,orange,purple,black
[7]: black, in array white,red,green,blue,yellow,orange,purple,black
```

`reduce()` 方法及其对应的 `reduceRight()` 方法将数组转换为单个值。解释该方法工作原理的最佳方式是描述一个具体的使用场景。例如，你有一个包含数值的数组，并希望计算这些数值的总和。为此，你需要遍历所有元素并累加元素值。

与 `forEach()` 方法类似，你需要向 `reduce()` 方法提供一个函数。该函数有两个必需参数。第一个参数是一个累加器，它保存了上一个元素处理后的中间结果，或者如果是第一个元素，则保存一个初始值。第二个参数是当前正在调用该函数的元素。该函数还有两个可选参数，与 `forEach()` 方法相同，即索引和数组引用。该函数将当前元素与累加器合并，并返回结果。以下是实现求和用例的方法：

```javascript
function sumArray(total, item, index, array) {
    return total + item;
}

var arr = [1, 3, 5, 2, 8, 1];
console.log(numbers);
var sum = arr.reduce(sumArray, 0);
console.log(sum);
```

注意，`reduce()` 方法包含第二个参数，它是累加器的初始值。因此，在首次调用 `sumArray()` 函数时，`total` 参数的值为 `0`。

`reduceRight()` 方法与 `reduce()` 的功能相同，不同之处在于它从数组末尾开始，按逆序迭代元素。当顺序无关紧要时（例如求和操作），`reduce()` 或 `reduceRight()` 会产生相同的结果。

## 本章小结

我在本章中介绍了大量内容。虽然这远未涵盖所有特性，但它为你提供了所需基础知识的良好入门。如果你对 JavaScript 还比较陌生，我建议你仔细阅读下载代码中的 `SampleScript.js` 文件，并确保理解代码的用途。

在第 [16](http://dx.doi.org/10.1007/978-1-4842-2463-2_16) 章至第 [20](http://dx.doi.org/10.1007/978-1-4842-2463-2_20) 章中，你将广泛使用 JavaScript 来增强你的 HTML5 应用程序。第 21 至 26 章的高级内容也会大量运用这些 JavaScript 特性。

在下一章中，我将演示用于为 HTML 文档提供结构的 HTML 元素。
