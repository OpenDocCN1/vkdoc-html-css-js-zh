# JavaScript 中的数组操作方法

JavaScript 中有几种可用的数组操作方法，我将在此简要说明。其中一些方法会修改现有数组，而另一些则不会，而是返回一个新数组。请密切注意这一细节。

## `concat()` 方法

`concat()` 方法用于合并两个或多个数组，并将元素返回到一个新数组中。现有数组不会被修改。`concat()` 方法在第一个数组上调用，而其他数组作为参数传入。例如：

```javascript
var arr1 = ["a", "b", "c"];
var arr2 = ["x", "y", "z"];
var combined = arr1.concat(arr2);
```

生成的数组 `combined` 将包含元素 `a`、`b`、`c`、`x`、`y` 和 `z`。如果你需要合并超过两个数组，只需将其他数组作为附加参数传入即可。

## `slice()` 方法

使用 `slice()` 方法可以创建一个包含现有数组子集的新数组，而不会改变现有数组。传入起始和结束索引以指定要返回哪些元素。请注意，起始索引是包含的，但结束索引是不包含的。因此，调用 `arr.slice(2, 4);` 将返回索引 2 和 3 的元素。结束参数是可选的；如果省略，则返回从起始索引开始的所有元素。

你还可以在调用 `slice` 时使用负值，这将从右侧开始计数。例如，`arr.slice(-3, -1);` 将返回倒数第二个元素以及它前面的那个元素。这种语法可能看起来不太直观；然而，如果你将数组长度加到这些参数上，就更容易理解了。

例如，以下两条语句将返回完全相同的结果：

```javascript
arr.slice(-3, -1);
arr.slice(arr.length-3, arr.length-1);
```

**提示：** 如果你想获取最后 `n` 个元素，请将 `-n` 用作第一个参数，将 `length` 用作第二个参数。例如，要获取最后 5 个元素，请使用 `arr.slice(-5, length);`。

## `splice()` 方法

`splice()` 方法可以同时执行删除数组中的元素以及在指定位置插入新元素的操作。第一个参数指定删除开始的位置，同时也是插入新元素的位置。第二个参数指示应删除多少个元素。如果你不想删除任何元素，请输入 `0`。要插入元素，请将它们作为附加参数传入。

你可以使用 `splice()` 方法仅删除元素，也可以仅插入元素，还可以同时执行这两项操作。例如：

```javascript
arr.splice(2, 1); // 删除索引 2 处的一个元素
arr.splice(2, 0, "teal", "pink"); // 在索引 2 处添加两个元素
arr.splice(2, 1, "teal", "pink"); // 删除 1 个元素并插入 2 个
```

**注意：** `splice()` 方法会返回一个包含被删除元素的数组。在这方面，它的工作方式与 `slice()` 方法类似。事实上，`arr.splice(2,1);` 和 `arr.slice(2,1);` 将返回相同的结果。然而，与 `slice()` 不同，`splice()` 方法会修改调用它的数组。调用 `slice()` 会保持原数组不变，但调用 `splice()` 会从数组中移除元素。当然，如果你使用 `splice()` 来插入元素，数组也会被更新。

## `sort()` 方法

`sort()` 方法会修改现有数组。它接受一个可选参数，用于指定在排序过程中使用的回调函数。如果未提供，`sort()` 方法会在必要时将每个元素转换为字符串，然后按字母顺序对字符串进行排序。考虑以下代码：

```javascript
var numbers = [1, 33, 7, 12, 5];
numbers.sort();
```

执行 `sort()` 方法后，元素将变为 `[1, 12, 33, 5, 7]`，这可能不是你想要的。要纠正这一点，你需要提供一个比较函数。此函数接收两个参数，它们将是数组中的两个元素。如果元素相等（就它们应如何排序而言），它应返回 `0`；如果第一个元素应排在第二个元素之前，则返回 `-1`；如果第一个元素应排在第二个元素之后，则返回 `1`。

以下代码定义了一个 `numericSort()` 函数，并将其传递给 `sort()` 方法。调用此方法后，元素将按数字顺序 `[1, 5, 7, 12, 33]` 排序。

```javascript
function numericSort(a, b){
    if(isNaN(a) || isNaN(b)) return 0; // 如果不是数字，则无法比较
    if(a == b) return 0;
    if(a < b) return -1;
    if(a > b) return 1;
}

var numbers = [1, 33, 7, 12, 5];
numbers.sort(numericSort);
```

## `reverse()` 方法

`reverse()` 方法通过反转其元素的顺序来修改现有数组。它不接受任何参数，并返回修改后的数组。以下代码将输出元素 `[3, 2, 1]`。

```javascript
var numbers = [1, 2, 3];
numbers.reverse();
console.log(numbers);
```

### 搜索

数组对象提供了几种在数组中查找元素的方法。`indexOf()` 方法返回指定元素首次出现的索引。例如，`arr.indexOf("red");` 将返回第一个值为 `"red"` 的元素。类似地，`arr.lastIndexOf("red");` 将返回最后一次出现的位置。

这两种方法都使用严格相等性来执行搜索，这意味着如果你要查找 `"12"` 而数组包含数字 `12`，则无法找到它。`indexOf()` 方法从数组开头开始向前搜索，直到找到第一个匹配项。`lastIndexOf()` 方法从数组末尾开始向后扫描。

两种方法都支持一个 `fromIndex` 参数，该参数指定搜索应从何处开始。如果未指定，则搜索从头开始（对于 `lastIndexOf()` 则是从末尾开始）。也支持负值，表示搜索应从距离数组末尾这么多个元素的位置开始。如果使用负数来定义搜索的起始位置，`indexOf()` 的搜索仍然是向前的，而 `lastIndexOf()` 的搜索仍然是向后的。

```javascript
var arr = ["red", "green", "blue", "yellow", "blue", "purple"]; 
arr.indexOf("blue"); // 返回 2
arr.lastIndexOf("blue"); // 返回 4
arr.indexOf("blue", 3); // 返回 4
arr.lastIndexOf("blue", -3); // 返回 2
```

`find()` 和 `findIndex()` 方法将使用你定义的回调函数来搜索数组中的元素。这使你可以完全控制搜索逻辑。`find()` 方法返回元素，而 `findIndex()` 返回元素的索引。这两种方法都会遍历数组中的元素，执行指定的回调函数。当回调函数返回 `true` 时，搜索停止，并返回当前元素或其索引。

回调函数可以有三个输入参数，但只有第一个是必需的：

- `element` - 当前正在评估的数组元素

- `index` - 当前元素的索引

- `array` - 正在其中搜索的数组对象

例如，如果你想从数组中查找第一种原色，可以将回调函数实现为：

```javascript
function isPrimary(color, index, array) {
    if (color == "red" || color == "blue" || color == "yellow"){
        return true;
    }
    return false;
}
```

然后调用 `find()` 方法，并传入 `isPrimary()` 函数：

```javascript
console.log(arr.find(isPrimary));
```

`find()` 和 `findIndex()` 方法都支持第二个可选参数。这允许调用者在调用回调函数时定义 `this` 的值。

### 创建子集

`filter()` 函数不会修改数组，而是返回一个新数组，该数组是原数组的一个子集。要实现过滤逻辑，你必须指定一个回调函数，该函数将为数组中的每个元素调用。如果元素应包含在子集中，则该函数返回 `true`，否则返回 `false`。

此回调函数的工作方式与 `find()` 方法使用的回调函数相同。实际上，你可以对两者使用相同的函数：

```javascript
var subset = arr.filter(isPrimary);
console.log(subset);
```

这不会影响现有数组，但会返回一个包含 `["red", "blue", "yellow", "blue"]` 的新数组。

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