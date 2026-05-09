# JavaScript 基础

#### 命名空间

`helpers.logger("some more text");`

> **提示** 实现命名空间概念还有其他方式。如需了解这些变体的示例，请参阅 [`addyosmani.com/blog/essential-js-namespacing`](https://addyosmani.com/blog/essential-js-namespacing) 上的文章。同时，这还假设不存在名为 `mySample` 或 `helpers` 的其他全局对象。你可能需要在这些名称前加上公司名称或缩写，以确保唯一性。

#### 异常

JavaScript 支持 `try/catch/finally` 模式，这与你在 C# 等其他语言中看到的类似。

可能失败的代码应放入 `try` 块中。如果在 `try` 块内引发了异常，则将执行 `catch` 块。无论是否发生错误，`finally` 块始终会执行。

你还可以使用 `throw()` 函数在代码中生成异常。这使你可以将错误处理放在 `catch` 块中，从而使主代码更清晰。以下是这些结构的一个简单示例：

```javascript
try {
  var x = 5;
  var y = 0;
  if (y == 0) {
    throw("不能除以零")
  }
  console.log(x/y);
}
catch(e) {
  console.log("错误: " + e);
}
finally {
  console.log("Finally 块已执行");
}
```

`finally` 块是放置任何必要的清理代码的好地方，这些代码即使在发生错误时也需要运行。`catch` 和 `finally` 块都是可选的，但你至少需要其中一块。

#### Promise

*Promise* 是处理异步函数调用的一种标准化方式。实际上，它只是一个带有两个参数的回调函数：一个函数在成功时被调用，另一个在失败时被调用。Promise 可以处于以下三种状态之一：

- **待定（Pending）** - 操作尚未完成
- **已完成（Fulfilled）** - 操作成功完成
- **已拒绝（Rejected）** - 处理过程中发生错误

有时，已完成和已拒绝被统称为已决。让我们看一个使用 Promise 的简单示例。`getNumber()` 函数等待 2 秒，然后生成一个介于 0 和 100 之间的随机数。如果你请求一个偶数且随机数为偶数，则该函数返回成功。

```javascript
function getNumber(bEven) {
  return new Promise(function (fulfill, reject) {
    // 执行某些长时间运行的任务
    window.setTimeout(
      function () {
        var i = Math.round((Math.random() * 100), 0);
        if ((i % 2 != 0 && bEven) ||
            (i % 2 == 0 && !bEven)) {
          reject(i);
        }
        else {
          fulfill(i);
        }
      }, 2000);
  });
}
```

`getNumber()` 函数返回一个 `Promise` 对象，该对象是一个带有两个参数的函数。工作在这个内部函数中完成。由于代码位于 `getNumber()` 函数内部，因此它可以访问其变量，例如 `bEven` 参数。`fulfill()` 和 `reject()` 函数的引用被传入内部函数，当内部函数完成时，会调用其中一个。

要调用此代码，请添加以下内容：

```javascript
var p = getNumber(true);
p.then(
  function (i) { console.log("Promise 已完成, i = " + i); },
  function (i) { console.log("Promise 已拒绝, i = " + i); }
);
console.log("Promise 已创建...");
```

这将调用 `getNumber()` 函数请求一个偶数。该函数立即返回一个 `Promise` 对象。然后调用 `Promise` 对象的 `then()` 成员，传入两个匿名函数。第一个将在成功时调用，第二个在失败时调用。此时，`getNumber()` 函数尚未完成；它处于待定状态。然后，`Promise` 对象会执行其中一个匿名回调函数。

你也可以捕获错误回调，而不是将其传递给 `Promise` 对象。例如，以下方式可以实现相同的效果：

```javascript
p.then(
  function (i) { console.log("Promise 已完成, i = " + i); }
)
.catch(
  function (i) { console.log("Promise 已拒绝, i = " + i); }
);
```

#### 数组方法

处理数组时有相当多的内置功能。我将在此解释更常见的方法，并在附录 C 的参考部分提供完整列表。

### 访问元素

正如我之前提到的，填充数组的最简单方法是使用字面量表示法。例如，这将创建一个包含六个字符串元素的数组：

```javascript
var arr = ["red", "green", "blue", "yellow", "orange", "purple"];
```

第一个元素的索引为 0，其余元素的索引按顺序分配。

元素通过其索引进行访问。要获取第二个元素的值，请使用 `arr[1]`，它将返回字符串 `"green"`。你可以通过直接设置来更新值，例如 `arr[2] = "azure"`。这将用 `"azure"` 替换 `"blue"` 元素。你还可以使用 `delete` 关键字删除元素：`delete arr[3]`。这将删除索引 3 处的值 (`"yellow"`)，但在数组中留下一个空元素。

JavaScript 在处理越界错误方面非常宽容。例如，在上述数组中，有六个元素，每个元素通过索引 0-5 引用。调用 `arr[8]` 不会导致错误，而是返回 `undefined`。同样，你可以调用 `arr[8] = "brown"`，数组将自动增长以容纳九个元素，第九个元素的值为 `"brown"`。这会在数组中留下一些空洞；`arr[6]` 和 `arr[7]` 将是 `undefined`，但 `arr[8]` 将返回 `"brown"`。

有两种添加新元素的方法：`unshift()` 将元素添加到数组的开头，`push()` 将元素添加到数组的末尾：

```javascript
arr.unshift("white");
arr.push("black");
```

执行此操作后，`"white"` 将位于列表的开头，`"black"` 位于末尾。原始六个元素将按原始顺序位于两者之间。这两种方法都返回添加元素后数组的新大小。你还可以通过 `length` 属性获取大小：

```javascript
var numberOfElements = arr.length;
```

还有相应的方法来移除第一个和最后一个元素。`pop()` 方法返回最后一个元素并将其从数组中移除。同样，`shift()` 方法返回第一个元素并将其从数组中移除。

`shift()` 和 `unshift()` 方法因其需要调整元素索引而得名。数组中的第一个元素的索引为 0。当在开头添加新元素时，它将位于索引 0，这意味着每个现有元素的索引都需要递增或上移。同样，移除第一个元素需要索引下移。

> **提示** 你可以通过使用 `push()` 和 `pop()` 方法来实现栈的功能。栈使用后进先出（LIFO），意味着最后添加的元素最先被移除。使用 `push()` 将元素添加到末尾，然后 `pop()` 将移除最后添加的元素。要实现队列（先进先出，FIFO），请使用 `unshift()` 添加新元素，并使用 `pop()` 移除队列中最旧的元素。

### 输出数组

`valueOf()` 方法将返回数组作为值集合。这是数组对象的默认方法；如果你访问数组而不调用其任何方法，则会调用此方法。

此外，你可以通过调用 `toString()` 方法轻松地将数组的内容输出为一组逗号分隔的值。如果你需要使用不同的分隔符字符串输出元素，请使用 `join()` 方法。该方法接受一个参数，指定将放置在每个元素之间的字符串。

如果你不向 `join()` 方法传递任何参数，它将使用逗号字符。例如，如果你有一个名为 `arr` 的数组，以下四个语句将输出其内容：前两个作为字符串集合，后两个作为包含逗号分隔值的单个字符串。

```javascript
console.log(arr);
console.log(arr.valueOf());
console.log(arr.toString());
console.log(arr.join());
```


好的，作为一名高级文档工程师和翻译员，我将遵循您提供的注意事项和示例，对以下英文文本进行翻译。


