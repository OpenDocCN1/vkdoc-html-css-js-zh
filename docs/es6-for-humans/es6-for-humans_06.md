# Rest 与 Spread 操作符

JavaScript 一直以来都允许向函数传递比函数声明中正式指定的参数更少或更多的参数，而不会出现任何问题。正如之前讨论的，默认参数可以帮助你接受更少的参数值，同时让其他参数仍有值可赋。

许多现代编程语言都提供了函数接受可变数量参数的能力。ES6 通过 Rest 参数（剩余参数）为 JavaScript 引入了这一迫切需要的特性。现在，你可以非常轻松地向函数传递动态数量的参数。如果你在 ES5 中想这样做，就必须将所有值放入一个像数组这样的数据容器数据类型中。Rest 参数简化了整个流程。

**注意：** 不要将此术语与 Web 服务中的 REST 概念混淆。这与 Web 服务中的 REST 毫无关系。此处的 Rest 指的是将参数收集起来，并将它们全部放入一个单一的数组中。Spread（展开）则指将数组（甚至字符串）的元素展开。

让我们看一个例子：

```
var showCollections = function (id, ...collection) {
console.log(collection instanceof Array);
};
showCollections(42, "movies", "music");  // true
```

`...` 符号就是 rest 符号。它位于一个命名参数之前。这个命名参数将成为一个数组，用来收集所有传递给函数的剩余参数。因此在这里，`collection` 被设置成了一个数组。为了更清楚地说明，如果我们按以下方式执行上述程序：

```
var showCollections = function (id, ...collection) {
console.log(collection);
};
showCollections(42, "movies", "music"); // ["movies", "music"]
```

Rest 参数收集了 `id` 参数之后的所有剩余参数，并将它们放入一个名为 `collection` 的数组中。除了第一个已定义的参数 `'id'` 之外，所有其他内容都将被放入该数组中。

如果我们只传递一个值（即 `id`）来调用同一个函数，它会输出一个空数组 `[]`。

让我们看一下函数的 `length` 属性。如果我们调用 `showCollections.length`，它将返回函数中参数的数量。

```
var showCollections = function (id, ...collection) {};
console.log(showCollections.length); // 1
```

`length` 属性会忽略 Rest 参数。在这个例子中，它是 1。函数的 `length` 属性只计算命名参数的数量，而不包括 rest 参数。现在，我们来看看在函数内部检查 `arguments.length` 的情况：

```
var showCollections = function (id, ...collection) {
console.log(arguments.length);
};
showCollections(123, "movies", "music"); // 3
```

我们之前已经有一个 `arguments` 对象，我们可以用它来检查传递给函数的所有参数，而无需具体定义每个参数。请记住，函数中既可以有命名参数，也可以有未命名参数。在这种情况下，尽管函数定义中只有两个参数，但 `arguments` 对象会引用原始的函数调用，而 3 是传递给函数的参数个数。

在 ES4 规范中，Rest 参数本意是要取代 `arguments`，并且 `arguments` 对象会被完全废除，但 ES4 从未成为现实。在 ES6 中，这一概念被重新引入，但这一次 `'arguments'` 并未从语言中移除。

我们可以在函数构造器中使用 Rest 操作符。请看下面的例子，我们创建了一个带有 rest 参数的新函数，该函数返回传递给它的第一个参数。

```
var getFirst = new Function("...args", "return args[0]");
console.log(getFirst(1, 2));   // 1
```

## Spread 操作符

Spread 操作符，同样用数组前的 `...` 表示，其作用本质上是 rest 操作符的逆操作。它将一个数组展开，并将值传递给指定的函数。请看下面的示例：

```
let values = [200, 300, 400];
let newSet = [100, ...values, 500]
console.log(newSet);  // [100, 200, 300, 400, 500]
```

Spread 操作符与 rest 参数密切相关。在这种特定的格式中，作为 spread 操作符，`...` 被用作一种拼接或插入机制，将 `values` 数组插入到现有值之间，从而将新形成的数组赋值给 `newSet`。

在 rest 参数的情况下，你可以将多个参数合并成一个单一的数组；而在 spread 操作符的情况下，你可以指定一个单一的数组，它会被拆分成单独的参数，然后传递给一个函数或方法。让我们再看一个使用 `Math.max()` 方法和 spread 操作符的例子：

```
let numbers = [-25, 100, 42, -1000];
console.log(Math.max(...numbers, 900));        // 900
```

在上述例子中，`Math.max` 接收了五个参数，前四个是 -25, 100, 42, 和 -1000，另外还有我们添加的第五个参数 900。结果是这些数中的最大值，即 900。因此，spread 操作符将数组中的值作为参数展开到函数调用中。

Spread 操作符使得将以数组形式传递给函数的参数处理起来更加容易。与 rest 操作符一样，你也可以在函数构造器中使用 spread 操作符。

如果我们尝试展开一个空数组，比如一个可能缺少两个值的数组 `[,,]`，最后一个逗号被视为尾部逗号而被忽略。展开的参数也将会是 `undefined`。请看下面的例子：

```
function printInput(...input) {
console.log(input);
}
let input = [,,];
printInput(...input); // [undefined, undefined]
```



