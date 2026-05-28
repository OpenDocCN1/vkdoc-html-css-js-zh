# 六、柯里化和局部应用

在这一章中，我们将了解术语`curry`的含义。一旦我们理解了这意味着什么以及它可以用在什么地方，我们将转向函数式编程中的另一个概念，叫做`局部应用`。当我们在功能组合中使用 currying 和 partial application 时，理解它们非常重要。和前几章一样，我们将看一个样例问题，并解释如何应用像 currying 和局部应用这样的函数式编程技术。

### 注意

章节示例和库源代码在第六章。回购的网址是[`github.com/antoaravinth/functional-es8.git`](https://github.com/antoaravinth/functional-es8.git)。

一旦你检查出代码，请检查分支第六章:

```
git checkout -b 第六章来源/第六章
```

对于运行代码，与之前一样，运行:

```
npm 跑步游乐场
```

## 关于术语的几点说明

在解释 currying 和 partial application 的意思之前，我们需要理解本章将要用到的几个术语。

### 一元函数

A function is called *unary* if it takes a single function argument. For example, the identity function, shown in Listing 6-1, is a unary function.

```
const identity = (x) => x;
```

Listing 6-1 一元恒等函数

这个函数只有一个参数 `x`，所以我们可以称它为一元函数。

### 二元函数

A function is called *binary* if it takes two arguments. For example, in Listing 6-2, the add function is a binary function.

```
const add = (x,y) => x + y;
```

Listing 6-2 二进制加法函数

`add` 函数有两个参数，`x`，`y`；因此我们称它为二元函数。

你可以猜到，有三个参数的三元函数，等等。JavaScript 还允许一种特殊类型的函数，我们称之为*可变*函数，它接受可变数量的参数。

### 可变函数

A variadic function is a function that takes a variable number of arguments. Remember that we had `arguments` in older versions of JavaScript, which we can use to capture the variable number of arguments.

```
function variadic(a){
    console.log(a);
    console.log(arguments)
}
```

Listing 6-3 可变函数

We call the variadic function like this:

```
variadic(1,2,3) => 1 => [1,2,3]
```

### 注意

正如您在输出中看到的，`arguments` 确实捕获了传递给函数的所有参数。

As you can see in Listing 6-3, using `arguments` we are able to capture the additional arguments one could call on a function. Using this technique, we used to achieve the variadic functions in ES5 versions. However, starting with ES6, we have an operator called *Spread Operator* that we can use to achieve the same result.

```
const variadic = (a,...variadic) => {
    console.log(a)
    console.log(variadic)
}
```

Listing 6-4 使用扩展算子的变量函数

Now if we call this function we get exactly what we would expect:

```
variadic(1,2,3) => 1 => [2,3]
```

正如您在结果中看到的，我们被指向第一个传递的参数 `1` 和所有其他由我们的变量捕获的参数，该变量使用 `...` 休息论点！ES6 风格更简洁，因为它清楚地提到函数确实接受变量参数进行处理。

现在我们已经记住了一些关于函数的常用术语，是时候把注意力转向这个有趣的术语 curry 了。

## 携带

你是否已经从博客上听过无数次“阿谀奉承”这个词，仍然想知道它是什么意思？不用担心；我们将把 currying 定义分解成更小的定义，这对你来说更有意义。

我们从一个简单的问题开始:什么是 currying？这个问题的一个简单答案是:curry 是一个将带有 *n* 个参数的函数转换成一个*嵌套*一元函数的过程。如果你还不明白，不要担心。让我们用一个简单的例子来看看这意味着什么。

Imagine we have a function called `add`:

```
const add = (x,y) => x + y;
```

It’s a simple function. We can call this function like `add(1,1)`, which is going to give the result `2`. Nothing fancy there. Now here is the curried version of the `add` function:

```
const addCurried = x => y => x + y;
```

The `addCurried` function is now a curried version of `add`. If we call `addCurried` with a single argument like this:

```
addCurried(4)
```

it returns a function where `x` value is captured via the closure concept as we saw in earlier chapters:

```
=> fn = y => 4 + y
```

We can call the `addCurried` function like this to get the proper result:

```
addCurried(4)(4) => 8
```

Here we have manually converted the `add` function, which takes the two arguments into an `addCurried` function, which has nested unary functions. The process of converting a function from two arguments to a function that takes one argument (unary function) is called currying, as shown in Listing 6-5.

```
const curry = (binaryFn) => {
  return function (firstArg) {
    return function (secondArg) {
      return binaryFn(firstArg, secondArg);
    };
  };
};
```

Listing 6-5 库里函数定义

### 注意

我们已经用 ES5 格式编写了 `curry` 函数，这样我们可以可视化返回一个*嵌套的*一元函数的过程。

Now we can use our `curry` function to convert the `add` function to a curried version like this:

```
let autoCurriedAdd = curry(add)
autoCurriedAdd(2)(2) => 4
```

输出正是我们想要的。现在是时候修改 currying 的定义了:Currying 是将一个有 *n* 个参数的函数转换成一个嵌套的一元函数的过程。

正如你在我们的 `curry` 函数定义中看到的，我们正在将二元函数转换成嵌套函数，每个函数只需要一个参数；也就是说，我们返回的是嵌套的一元函数。现在我们已经在你的头脑中澄清了奉承这个术语，但是你仍然有一些明显的问题:我们为什么需要奉承？它有什么用？

### 携带用例

We'll start simple. Imagine we have to create a function for creating tables. For example, we need to create `tableOf2`, `tableOf3`, `tableOf4`, and so on. We can achieve this via Listing 6-6.

```javascript
const tableOf2 = (y) => 2 * y
const tableOf3 = (y) => 3 * y
const tableOf4 = (y) => 4 * y
```

**Listing 6-6: 表格功能无需奉承**

With that in place, the functions can be called this:

```javascript
tableOf2(4) // => 8
tableOf3(4) // => 12
tableOf4(4) // => 16
```

Now you see that you can generalize the tables concept into a single function like this:

```javascript
const genericTable = (x,y) => x * y
```

and then you can use `genericTable` to get `tableOf2` like the following:

```javascript
genericTable(2,2)
genericTable(2,3)
genericTable(2,4)
```

and the same for `tableOf3` and `tableOf4`. If you notice the pattern, we are filling up `2` in the first argument for `tableOf2`, `3` for `tableOf3`, and so on!

Perhaps you are thinking that we can solve this problem via curry? Let’s build tables from `genericTable` using curry:

```javascript
const tableOf2 = curry(genericTable)(2)
const tableOf3 = curry(genericTable)(3)
const tableOf4 = curry(genericTable)(4)
```

**Listing 6-7: 使用 Currying 的表格功能**

Now you can do your testing with these curried versions of the tables:

```javascript
console.log("Tables via currying")
console.log("2 * 2 =",tableOf2(2))
console.log("2 * 3 =",tableOf2(3))
console.log("2 * 4 =",tableOf2(4))
console.log("3 * 2 =",tableOf3(2))
console.log("3 * 3 =",tableOf3(3))
console.log("3 * 4 =",tableOf3(4))
console.log("4 * 2 =",tableOf4(2))
console.log("4 * 3 =",tableOf4(3))
console.log("4 * 4 =",tableOf4(4))
```

This is going to print the value we expect:

```
Tables via currying
2 * 2 = 4
2 * 3 = 6
2 * 4 = 8
3 * 2 = 6
3 * 3 = 9
3 * 4 = 12
4 * 2 = 8
4 * 3 = 12
4 * 4 = 16
```

## 一个日志功能:使用 Currying

The example in the previous section helped us understand what currying does, but let’s use a more complicated example in this section. As developers when we write code, we do a lot of logging at several stages of the application. We could write a helper logger function that looks like Listing 6-8.

```javascript
const loggerHelper = (mode,initialMessage,errorMessage,lineNo) => {
    if(mode === "DEBUG")
        console.debug(initialMessage,errorMessage + "at line: " + lineNo)
    else if(mode === "ERROR")
        console.error(initialMessage,errorMessage + "at line: " + lineNo)
    else if(mode === "WARN")
        console.warn(initialMessage,errorMessage + "at line: " + lineNo)
    else
        throw "Wrong mode"
}
```

**Listing 6-8: 简单的 loggerHelper 函数**

When any developer needs to print an error to the console from the `Stats.js` file, he or she can use the function like the following:

```javascript
loggerHelper("ERROR","Error At Stats.js","Invalid argument passed",23)
loggerHelper("ERROR","Error At Stats.js","undefined argument",223)
loggerHelper("ERROR","Error At Stats.js","curry function is not defined",3)
loggerHelper("ERROR","Error At Stats.js","slice is not defined",31)
```

类似地，我们可以将 `loggerHelper` 函数用于调试和警告消息。正如你所看到的，我们在重复所有调用的参数，主要是 `mode` 和 `initialMessage`。我们能做得更好吗？是的，通过奉承，我们可以更好地打这些电话。我们可以使用前面定义的 `curry` 函数吗？不幸的是，不能，因为我们设计的 `curry` 函数只能处理二进制函数，而不能像 `loggerHelper` 那样处理四个参数的函数。

让我们来解决这个问题，实现全功能的 `curry` 函数，它可以处理任何带有 *n* 个参数的函数。

## 重温咖喱

We all know that we can curry (Listing 6-5) only a function. How about many functions? It’s simple but important to have it in our implementation of `curry`. Let’s add the rule first, as shown in Listing 6-9.

```javascript
let curry = (fn) => {
    if(typeof fn !== 'function'){
        throw Error('No function provided');
    }
};
```

**Listing 6-9: 修改 curry 函数定义**

With that check in place, if others call our `curry` function with an integer like `2`, and so on, they get back the error. That’s perfect! The next requirement to our curried function is that if anyone provided all arguments to a curried function, we need to execute the real function by passing the arguments. Let’s add that using Listing 6-10.

### Listing 6-10 处理参数的 curry 函数

```javascript
let curry = (fn) => {
    if (typeof fn !== 'function') {
        throw Error('No function provided');
    }
    return function curriedFn(...args) {
        return fn.apply(null, args);
    };
};
```

Now if we have a function called `multiply`:

```javascript
const multiply = (x, y, z) => x * y * z;
```

We can use our new `curry` function like the following:

`curry(multiply)(1,2,3)` => `6`
`curry(multiply)(1,2,0)` => `0`

Let’s look at how it really works. We have added the logic in our `curry` function like this:

```javascript
return function curriedFn(...args) {
    return fn.apply(null, args);
};
```

The returned function is a variadic function, which returns the function result by calling the function via `apply` along by passing the `args`:

```javascript
fn.apply(null, args);
```

With our `curry(multiply)(1,2,3)` example, `args` will be pointing to `[1,2,3]` and because we are calling `apply` on `fn`, it’s equivalent to:

`multiply(1,2,3)`

这正是我们想要的！因此，我们从函数中得到预期的结果。

Now let us get back to the problem of converting the *n* argument function into a nested unary function (that’s the definition of curry itself)!

### Listing 6-11 将 n 参数函数转换为一元函数的 curry 函数

```javascript
let curry = (fn) => {
    if (typeof fn !== 'function') {
        throw Error('No function provided');
    }
    return function curriedFn(...args) {
        if (args.length < fn.length) {
            return function () {
                return curriedFn.apply(null, args.concat([].slice.call(arguments)));
            };
        }
        return fn.apply(null, args);
    };
};
```

We have added the part:

```javascript
if (args.length < fn.length) {
    return function () {
        return curriedFn.apply(null, args.concat([].slice.call(arguments)));
    };
}
```

Let’s understand what’s happening in this piece of code, one element at a time.

`args.length < fn.length`

这一行检查通过`...args`长度和函数参数列表长度小于或等于。如果是这样，我们就进入`if`块，否则我们就像以前一样调用完整的函数。

Once we enter the `if` block, we use the `apply` function to call `curriedFn` recursively like this:

```javascript
curriedFn.apply(null, args.concat([].slice.call(arguments)));
```

The snippet `args.concat([].slice.call(arguments))` is important. Using the `concat` function, we are concatenating the arguments that are passed one at a time and calling the `curriedFn` recursively. Because we are combining all the passed arguments and calling it recursively, we will meet a point in which the line `if (args.length < fn.length)` condition fails. The argument list length (`args`) and function argument length (`fn.length`) will be equal, thus skipping the `if` block and calling:

```javascript
return fn.apply(null, args);
```

这将产生函数的完整结果！

With that understanding in place, we can use our `curry` function to invoke the `multiply` function:

`curry(multiply)(3)(2)(1)` => `6`

完美！我们创建了自己的咖喱功能。

### 注意

您也可以像下面这样调用前面的代码片段:

```javascript
let curriedMul3 = curry(multiply)(3);
let curriedMul2 = curriedMul3(2);
let curriedMul1 = curriedMul2(1);
```

其中 `curriedMul1` 将等于 `6`。不过，我们使用 `curry(multiply)(3)(2)(1)`，因为它可读性更好。

需要注意的重要一点是，我们的 `curry` 函数现在将一个有 *n* 个参数的函数转换为一个可以作为一元函数调用的函数，如示例所示。

### 返回记录器功能

Now let’s solve our logger function using the defined curry function. Bringing up the function here for easy reference (Listing 6-8):

```javascript
const loggerHelper = (mode,initialMessage,errorMessage,lineNo) => {
    if(mode === "DEBUG")
        console.debug(initialMessage,errorMessage + "at line: " + lineNo)
    else if(mode === "ERROR")
        console.error(initialMessage,errorMessage + "at line: " + lineNo)
    else if(mode === "WARN")
        console.warn(initialMessage,errorMessage + "at line: " + lineNo)
    else
        throw "Wrong mode"
}
```

The developer used to call the function:

```javascript
loggerHelper("ERROR","Error At Stats.js","Invalid argument passed",23)
```

Now let’s solve the repeating first two arguments problem via curry:

```javascript
let errorLogger = curry(loggerHelper)("ERROR")("Error At Stats.js");
let debugLogger = curry(loggerHelper)("DEBUG")("Debug At Stats.js");
let warnLogger = curry(loggerHelper)("WARN")("Warn At Stats.js");
```

Now we can easily refer to the earlier curried functions and use them under the respective context:

```javascript
//for error
errorLogger("Error message",21) => Error At Stats.js Error messageat line: 21
//for debug
debugLogger("Debug message",233) => Debug At Stats.js Debug messageat line: 233
//for warn
warnLogger("Warn message",34) => Warn At Stats.js Warn messageat line: 34
```

太棒了！我们已经看到了 `curry` 函数如何在现实世界中帮助删除函数调用中的大量样板文件。不要忘记感谢闭包概念，它支持了 `curry` 函数。节点的调试模块在其 API 中使用了库里概念(见[`github.com/visionmedia/debug`](https://github.com/visionmedia/debug))。

## 进行中

在上一节中，我们创建了自己的 `curry` 函数。我们还看到了一个使用这个 `curry` 函数的简单例子。

在这一节中，我们将看到一些小而紧凑的例子，其中使用了涂抹技巧。本节展示的例子将帮助你更好地理解如何在日常活动中使用奉承。

### 在数组内容中查找数字

Imagine we want to find the array content that has a number. We can solve the problem via the following code snippet:

```javascript
let match = curry(function(expr, str) {
    return str.match(expr);
});
```

The returned `match` function is a curried function. We can give the first argument `expr` a regular expression `/[0-9]+/` that will indicate whether the content has a number in it.

```javascript
let hasNumber = match(/[0-9]+/)
```

Now we will create a curried `filter` function :

```javascript
let filter = curry(function(f, ary) {
    return ary.filter(f);
});
```

With `hasNumber` and `filter` in place, we can create a new function called `findNumbersInArray`:

```javascript
let findNumbersInArray = filter(hasNumber)
```

Now you can test it:

```javascript
findNumbersInArray(["js","number1"]) => ["number1"]
```

### 对数组求平方

We know how to square contents of an array. We have also seen the same problem in previous chapters. We use the `map` function and pass on the square function to achieve the solution to our problem. Here we can use the `curry` function to solve the same problem in another way:

```javascript
let map = curry(function(f, ary) {
    return ary.map(f);
});
let squareAll = map((x) => x * x)
squareAll([1,2,3]) => [1,4,9]
```

正如你在这个例子中看到的，我们已经创建了一个新的函数 `squareAll`，现在我们可以在代码库中的其他地方使用它。类似地，您也可以对 `findEvenOfArray`、`findPrimeOfArray` 等执行此操作。

## 数据流

在使用 currying 的两节中，我们已经设计了 curried 函数，使得它们总是在最后使用数组。这是一种有意创建定制函数的方式。如前几章所讨论的，我们作为程序员经常处理像 `array` 这样的数据结构，所以把 `array` 作为最后一个参数允许我们创建许多可重用的函数，比如 `squareAll` 和 `findNumbersInArray`，我们可以在整个代码库中使用它们。

### 注意

在我们的源代码中，我们调用了 `curry` 函数 `curryN`。这只是为了保持原来的 `curry`，它应该对二元函数进行 `curry`。

### 部分应用

在这一节中，我们将看到另一个名为 `partial` 的函数，它允许开发人员部分地应用函数参数。

# 柯里化与部分应用

## 使用 `setTimeout` 的问题

想象一下，我们想每 10 毫秒执行一组操作。使用 `setTimeout` 函数，我们可以这样做：

```javascript
setTimeout(() => console.log("Do X task"), 10);
setTimeout(() => console.log("Do Y task"), 10);
```

如你所见，我们在每次 `setTimeout` 函数调用中都传递了 `10`。我们能否在代码中隐藏这个值？能否使用柯里化函数来解决这个问题？答案是否定的，因为柯里化函数从左到右应用参数。由于我们希望按需传递函数，并将 `10` 作为常量（它是参数列表的大部分），我们不能直接使用柯里化。

一种解决方法是包装我们的 `setTimeout` 函数，使函数参数成为最右边的参数：

```javascript
const setTimeoutWrapper = (time, fn) => {
  setTimeout(fn, time);
};
```

然后我们可以使用柯里化函数将 `setTimeout` 包装成一个 10 毫秒的延迟：

```javascript
const delayTenMs = curry(setTimeoutWrapper)(10);
delayTenMs(() => console.log("Do X task"));
delayTenMs(() => console.log("Do Y task"));
```

它会在我们需要的时候工作。但问题是，我们必须创建像 `setTimeoutWrapper` 这样的包装器，这会带来开销。这就是我们可以使用*部分应用*技术的地方。

## 实现部分功能

为了充分理解部分应用技术是如何工作的，我们将在这一部分创建我们自己的部分函数。一旦实现完成，我们将通过一个简单的例子来学习如何使用我们的部分函数。

部分函数的实现如清单 6-12 所示。

```javascript
const partial = function (fn, ...partialArgs) {
  let args = partialArgs;
  return function (...fullArguments) {
    let arg = 0;
    for (let i = 0; i < args.length && arg < fullArguments.length; i++) {
      if (args[i] === undefined) {
        args[i] = fullArguments[arg++];
      }
    }
    return fn.apply(null, args);
  };
};
```

**清单 6-12 部分函数定义**

让我们快速地将 `partial` 函数用于我们当前的问题：

```javascript
let delayTenMs = partial(setTimeout, undefined, 10);
delayTenMs(() => console.log("Do Y task"));
```

这将如你所愿地在控制台打印输出。现在让我们逐步了解 `partial` 函数的实现细节。使用闭包，我们捕获了第一次传递给函数的参数：

```javascript
partial(setTimeout, undefined, 10)
// 将导致
let args = partialArgs => args = [undefined, 10]
```

我们返回一个函数，该函数将记住 `args` 的值（是的，我们再次使用闭包）。返回的函数非常简单。它接受一个名为 `fullArguments` 的参数，因此我们通过传递这个参数来调用像 `delayTenMs` 这样的函数：

```javascript
delayTenMs(() => console.log("Do Y task"))
// fullArguments 指向
// [() => console.log("Do Y task")]
// 使用闭包的 args 将有
// args = [undefined, 10]
```

现在在 `for` 循环中，我们迭代并为函数创建必要的参数数组：

```javascript
if (args[i] === undefined) {
  args[i] = fullArguments[arg++];
}
```

现在让我们从值 `i` 为 0 开始：

```javascript
// args = [undefined, 10]
// fullArguments = [() => console.log("Do Y task")]
args[0] => undefined === undefined // true
// 在 if 循环内
args[0] = fullArguments[0] => args[0] = () => console.log("Do Y task")
// 因此 args 将变成 => [() => console.log("Do Y task"), 10]
```

正如您在这些代码片段示例中看到的，我们的参数指向数组，正如我们对 `setTimeout` 函数调用的预期。一旦我们在 `args` 中有了必要的参数，我们就通过 `fn.apply(null, args)` 调用函数。

请记住，我们可以对任何具有 *n* 个参数的函数应用 `partial`。为了具体说明这一点，让我们看一个例子。在 JavaScript 中，我们使用以下函数调用来进行 JSON 美化打印：

```javascript
let obj = {foo: "bar", bar: "foo"};
JSON.stringify(obj, null, 2);
```

如你所见，调用 `stringify` 函数的最后两个参数总是相同的：`null, 2`。我们可以使用 `partial` 来消除样板代码：

```javascript
let prettyPrintJson = partial(JSON.stringify, undefined, null, 2);
```

然后你可以使用 `prettyPrintJson` 来打印 JSON：

```javascript
prettyPrintJson({foo: "bar", bar: "foo"});
```

这将给你以下输出：

```json
"{
  "foo": "bar",
  "bar": "foo"
}"
```

### 注意

在部分函数的实现中有一个小错误。如果用不同的参数再次调用 `prettyPrintJson` 呢？有用吗？

它总是给出第一个调用的参数的结果，但是为什么呢？你能看出我们哪里出错了吗？

提示：请记住，我们是通过用我们的参数替换未定义的值来修改参数的，数组用于引用。

### 柯里化与部分应用

我们已经看到了这两种技术，所以问题是何时使用哪一种。答案取决于你的 API 是如何定义的。如果你的 API 被定义为 `map`，`filter`，那么我们可以很容易地使用 `curry` 函数来解决我们的问题。正如上一节所讨论的，生活并不总是一帆风顺的。可能有些函数不是为 `curry` 设计的，比如我们例子中的 `setTimeout`。在这些情况下，最好的选择是使用部分函数。毕竟，我们使用 `curry` 或 `partial` 是为了让函数参数和函数设置变得简单和更强大。

还需要注意的是，`currying` 将返回嵌套的一元函数；我们已经实现了 `curry`，为了方便起见，它需要 `n` 个参数。开发人员需要 `curry` 或 `partial`，但不是两者都需要，这也是一个被证明的事实。

## 摘要

`Currying` 和局部应用始终是函数式编程中的一个工具。我们从解释 `currying` 的定义开始这一章，`currying` 只不过是将一个有 *n* 个参数的函数转换成嵌套的一元函数。我们已经看到了 `currying` 的例子以及它非常有用的地方，但是有些情况下，您希望填充函数的前两个参数和最后一个参数，而让中间的参数在一段时间内未知。这就是局部应用发挥作用的地方。为了充分理解这两个概念，我们实现了自己的 `curry` 和部分函数。我们已经取得了很大的进步，但我们还没有完成。

函数式编程就是组合函数，即组合几个小函数来构建一个新函数。构成和管道是下一章的主题。