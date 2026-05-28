# 二、JavaScript 函数的基础

在前一章中，我们看到了函数式编程是怎么一回事。我们看到了软件世界中的函数只不过是数学函数。我们花了很多时间讨论纯函数如何给我们带来巨大的优势，比如并行代码执行、可缓存等等。我们现在确信函数式编程完全是关于函数的。

在这一章中，我们将会看到 JavaScript 中的函数是如何使用的。我们将会看到最新的 JavaScript 版本 ES7/8。本章回顾了如何创建函数、调用函数以及传递 ES6 和更高版本中定义的参数。然而，这不是本书的目标。我们强烈建议您尝试书中的所有代码片段，以获得如何使用函数的要点（更准确地说，我们将研究`箭头函数`）。

一旦我们对如何使用函数有了坚实的理解，我们将把注意力转向如何在我们的系统中运行 ES8 代码。截至目前，浏览器并不支持 ES8 的所有功能。为了解决这个问题，我们将使用一个叫做`Babel`的工具。在本章的最后，我们将开始创建一个函数库的基础工作。为此，我们将使用一个节点项目，该项目将使用`Babel-Node`工具进行设置，以便在您的系统中运行我们的代码。

### 注意

章节示例和库源代码在第二章分支中。回购的网址是[`github.com/antsmartian/functional-es8.git`](https://github.com/antsmartian/functional-es8.git)

一旦你检查出代码，请检查分支第二章:

```
git checkout -b 第二章来源/第二章
```

对于运行代码，与之前一样，运行:

```
npm 跑步游乐场
```

## ECMAScript:一点历史

ECMAScript is a specification of JavaScript, which is maintained by ECMA International in ECMA-262 and ISO/IEC 16262\. Here are the versions of ECMAScript:

1.  ECMAScript 1 是 JavaScript 语言的第一个版本，发布于 1997 年。
2.  ECMAScript 2 是 JavaScript 语言的第二个版本，相对于前一个版本，它包含了非常小的变化。这个版本发布于 1998 年。
3.  ECMAScript 3 引入了几个特性，并于 1999 年发布。
4.  ECMAScript 5 现在几乎被所有的浏览器所支持。这是将`严格`模式引入语言的版本。它于 2009 年发布。ECMAScript 5.1 于 2011 年 6 月发布，并做了一些小的修改。
5.  ECMAScript 6 引入了许多变化，包括类、符号、箭头函数、生成器等等。
6.  ECMAScript 7 和 8 有一些新概念，如 async await、SharedArrayBuffer、尾随逗号、Object.entries 等等。

在本书中，我们将 ECMAScript 称为 ES7，因此这些术语可以互换。

## 创建和执行函数

在这一节中，我们将看到如何在 JavaScript 中以多种方式创建和执行函数。这一部分将会很长，也很有趣。因为许多浏览器还不支持 ES6 或更高版本，所以我们想找到一种方法来流畅地运行我们的代码。认识一下`Babel`，一个`transpiler`，它可以将最新的代码转换成有效的 ES5 代码（注意在我们的历史部分，我们提到了 ES5 代码可以在今天所有的浏览器中运行）。将代码转换成 ES5 让开发人员可以毫无问题地查看和使用最新版本 ECMAScript 的特性。使用 Babel，我们可以运行本书中介绍的所有代码示例。

在你安装了 Babel 之后，我们可以通过看到我们的第一个简单函数来弄脏我们的手。

### 第一功能

We define our first simple function here. The simplest function one can write in ES6 or higher versions is given in Listing 2-1.

```
() => "Simple Function"
```

Listing 2-1 简单的功能

If you try to run this function in babel-repl, you will see this result: `[Function]`

### 注意

没有必要在巴别塔世界中运行代码示例。如果您使用的是最新的浏览器，并且您确定它支持最新版本的 ECMAScript，那么您可以使用浏览器控制台来运行代码片段。毕竟这是一个选择的问题。如果您正在运行代码，比如说在 Chrome 中，清单 2-1 应该会给出这样的结果:

```
function () = > "简单函数"
```

这里要注意的一点是，根据您运行代码片段的位置，显示函数`表示`的结果可能会有所不同。

That’s it: We have a function. Take a moment to analyze this function. Let’s split them:

```
() => "Simple Function"
//where () represents function arguments
//=> starts the function body/definition
//content after => are the function body/definition.
```

我们可以跳过`function`关键字来定义函数。您可以看到，我们使用了`=>`操作符来定义函数体。这样创建的函数被称为`箭头函数`。我们在整本书中都使用了箭头函数。

现在函数已经定义好了，我们可以执行它来查看结果。哦，等等，我们创建的函数没有名字。我们怎么称呼它？

### 注意

没有名字的函数叫做匿名函数。当在第三章中看到高阶函数时，我们将理解匿名函数在函数式编程范例中的用法。

Let’s assign a name for it as shown in Listing 2-2.

```
var simpleFn = () => "Simple Function"
```

Listing 2-2 一个有名字的简单函数

Because we now have access to the function `simpleFn` we can use this reference to execute the function:

```
simpleFn() //returns "Simple Function" in the console
```

Now we have created a function and also executed it. We can see how the same function looks alike in ES5\. We can use babel to convert our code into ES5, using the following command:

```
babel simpleFn.js --presets babel-preset-es2015 --out-file script-compiled.js
```

This will generate the file called `script-compiled.js` in your current directory. Now open the generated file in your favorite editor:

```
"use strict";
var simpleFn = function simpleFn() {
  return "Simple Function";
};
```

这是我们在 ES5 中的等价代码。您可以感觉到在最新版本中编写函数变得更加容易和简洁。在转换后的代码片段中有两点需要注意。我们一个接一个地讨论它们。

### 严格模式

在这一节中，我们将讨论 JavaScript 中的严格模式。我们将看到它的好处以及为什么应该选择严格模式。

You can see that the converted code runs in strict mode, as shown here:

```
"use strict";
var simpleFn = function simpleFn() {
  return "Simple Function";
};
```

严格模式与最新版本无关，但在此讨论是合适的。正如我们已经讨论过的，ES5 将严格模式引入了 JavaScript 语言。

简单地说，严格模式是 JavaScript 的受限变体。在严格模式下运行的相同 JavaScript 代码在语义上可能与不使用严格模式的代码不同。所有在 JavaScript 文件中不使用`strict`的代码片段都将处于非`strict`模式。

# 为什么应该使用严格模式？有什么优势？

在 JavaScript 的世界里，使用严格模式有许多优势。一个简单的优势是，如果你在全局状态下定义变量（即不使用 `var` 命令），像这样：`"use strict"; globalVar = "evil"`，在严格模式下这将是一个错误！这对我们的开发人员来说是个好消息，因为在 JavaScript 中全局变量是非常有害的。然而，如果相同的代码在非严格模式下运行，那么它就不会报错。

现在您可以猜到，无论您是在严格模式还是非严格模式下运行，JavaScript 中的相同代码都会产生不同的结果。因为严格模式对我们很有帮助，我们将让 Babel 在传输我们的 ES8 代码时使用严格模式。

## 注意

我们可以将 `"use strict";` 放在 JavaScript 文件的开头，在这种情况下，它将对特定文件中定义的所有函数进行检查。否则，只能对特定函数使用严格模式。在这种情况下，严格模式将仅应用于该特定函数，而其他函数行为则处于非严格模式。有关这方面的更多信息，请参见 [`developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode)。

## Return 语句是可选的

在 ES5 转换后的代码片段中，我们看到 Babel 在我们的 `simpleFn` 中添加了 `return` 语句。

```javascript
"use strict";
var simpleFn = function simpleFn() {
    return "Simple Function";
};
```

然而，在我们的实际代码中，我们并没有指定任何 `return` 语句：`var simpleFn = () => "Simple Function"`。

因此，在这里，如果你有一个只有一个语句的函数，那么它隐含地意味着它返回值。多语句函数呢？我们将如何创造它们？

## 多语句函数

现在我们将看看如何编写多语句函数。让我们把我们的 `simpleFn` 变得稍微复杂一些，如清单 2-3 所示。

```javascript
var simpleFn = () => {
    let value = "Simple Function";
    return value;
};
//for multiple statement wrap with { }
```

**清单 2-3 多语句函数**

运行这个函数，你会得到和以前一样的结果。不过，在这里，我们使用了多个语句来实现相同的行为。除此之外，请注意我们使用了 `let` 关键字来定义 `value` 变量。`let` 关键字是 JavaScript 关键字家族中的新成员。它允许您声明限制在特定块范围内的变量，这与 `var` 关键字不同，`var` 关键字将变量全局定义到一个函数中，而不管它是在哪个块中定义的。

为了具体说明这一点，我们可以在一个 `if` 块内使用 `var` 和 `let` 关键字编写相同的函数，如清单 2-4 所示。

```javascript
var simpleFn = () => { //function scope
    if(true) {
        let a = 1;
        var b = 2;
        console.log(a);
        console.log(b);
    } //if block scope
    console.log(b); //function scope
    console.log(a); //function scope
};
```

**清单 2-4 带有 `var` 和 `let` 关键字的 `simpleFn`**

运行此函数将产生以下输出：

```
1
2
2
Uncaught ReferenceError: a is not defined(...)
```

从输出中可以看出，通过 `let` 关键字声明的变量只能在 `if` 块内访问，而不能在块外访问。当我们访问块外的变量时，JavaScript 抛出错误，而用 `var` 声明的变量不会那样做。相反，它声明了整个函数的变量范围。这就是变量 `b` 可以在 `if` 块之外被访问的原因。

因为块范围是非常需要的，所以我们将在整本书中使用 `let` 关键字来定义变量。现在让我们看看如何创建一个带有参数的函数作为最后一节。

## 函数参数

创建带参数的函数与 ES5 中相同。请看下面的快速示例（清单 2-5）。

```javascript
let identity = (value) => value;
```

**清单 2-5 带自变量的函数**

这里我们创建一个名为 `identity` 的函数，它以 `value` 作为参数并返回相同的值。如你所见，创建带参数的函数和在 ES5 中是一样的；只有创建函数的语法发生了变化。

## ES5 函数在 ES6 和更高版本中有效

在我们结束这一部分之前，我们需要把重要的一点说清楚。ES5 中编写的函数在最新版本中仍然有效。新版本引入了箭头函数只是一件小事，但这并不能取代旧的函数语法或其他任何东西。然而，我们将在本书中使用箭头函数来展示函数式编程方法。

# 设置我们的项目

现在我们已经了解了如何创建箭头函数，我们将把重点转移到本节的项目设置。我们将把我们的项目设置为一个节点项目，在本节的最后，我们将编写我们的第一个函数。

## 初始设置

在本节中，我们遵循一个简单的分步指南来设置我们的环境。步骤如下：

1.  第一步是创建一个存放源代码的目录。创建一个目录，并随意命名。
2.  进入该特定目录，从终端运行以下命令：`npm init`。
3.  运行步骤 2 后，将会询问您一组问题；你可以提供你想要的值。一旦完成，它将在您的当前目录中创建一个名为 `package.json` 的文件。

我们创建的项目 `package.json` 看起来像清单 2-6。

```json
{
  "name": "learning-functional",
  "version": "1.0.0",
  "description": "Functional lib and examples in ES8",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "Anto Aravinth @antoaravinth",
  "license": "ISC"
}
```

**清单 2-6 `package.json` 内容**

现在我们需要添加一些库，这些库将允许我们编写 ES8 代码并执行它们。在当前目录中运行以下命令：`npm install --save-dev babel-preset-es2017-node7`。

## 注意

本书使用 Babel 版本 `babel-preset-es2017-node7`。当你读到这篇文章的时候，这个版本可能已经过时了。你可以自由安装最新的版本，一切都应该很顺利。然而，在本书的上下文中，我们将使用指定的版本。

该命令下载名为 `es2017` 的 Babel 预设；这个包的主要目的是允许最新的 ECMAScript 代码在 Node.js 平台上运行。原因是 Node.js，在写这本书的时候，还没有完全兼容最新的特性。

一旦运行这个命令，您将能够看到在目录中创建了一个名为 `node_modules` 的文件夹，其中包含 `babel-preset-es2017` 文件夹。

因为我们在安装时使用了 `--save-dev`，`npm` 确实将相应的 `babel` 依赖项添加到了我们的 `package.json` 中。现在，如果你打开 `package.json`，它看起来像清单 2-7。

```json
{
  "name": "learning-functional",
  "version": "1.0.0",
  "description": "Functional lib and examples",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "Anto Aravinth @antoaravinth>",
  "license": "ISC",
  "devDependencies": {
    "babel-preset-es2017-node7": "⁰.5.2",
    "babel-cli": "⁶.23.0"
  }
}
```

清单 2-7 添加 `devDependencies` 后

现在这已经就位，我们可以继续创建两个名为 `lib` 和 `functional-playground` 的目录。所以现在你的目录结构如下：

```
learning-functional
  - functional-playground
  - lib
  - node_modules
    - babel-preset-es2017-node7/*
  - package.json
```

现在我们将把所有的函数库代码放入 `lib`，并使用 `functional-playground` 来探索和理解我们的函数技术。

### 我们解决循环问题的第一个函数方法

想象我们需要遍历数组并将数据打印到控制台。我们如何在 JavaScript 中实现这一点？

```javascript
var array = [1,2,3]
for(i=0;i<array.length;i++)
    console.log(array[i])
```

清单 2-8 循环数组

正如我们在第一章已经讨论过的，将操作抽象成函数是函数式编程的支柱之一。让我们将这个操作抽象成函数，这样我们就可以在任何需要的时候重用它，而不是重复告诉它如何迭代循环。

在 `lib` 目录中创建一个名为 `es8-functional.js` 的文件。我们的目录结构现在如下：

```
learning-functional
  - functional-playground
  - lib
    - es8-functional.js
  - node_modules
    - babel-preset-es2017-node7/*
  - package.json
```

现在有了这个文件，继续将清单 2-9 的内容放入该文件。

```javascript
const forEach = (array,fn) => {
   let i;
   for(i=0;i<array.length;i++)
      fn(array[i])
}
```

清单 2-9 `forEach` 函数

### 注意

现在不要担心这个函数是如何工作的。我们将在下一章看到高阶函数如何在 JavaScript 中工作，并提供大量的例子。

你可能注意到我们使用了关键字 `const` 来定义函数。这个关键字是最新版本的一部分，它使声明成为常量。例如，如果有人试图用同名变量重新赋值，像这样：

```javascript
forEach = "" //making your function as string!
```

前面的代码将抛出如下错误：

```
TypeError: Assignment to constant variable.
```

这将防止它被意外重新赋值。现在我们将使用创建的函数将数组的所有数据打印到控制台。为此，在 `functional-playground` 目录中创建一个名为 `play.js` 的文件。所以现在当前的文件结构如下：

```
learning-functional
  - functional-playground
    - play.js
  - lib
    - es8-functional.js
  - node_modules
    - babel-preset-es2017-node7/*
  - package.json
```

我们将在 `play.js` 文件中调用 `forEach`。我们如何调用这个驻留在不同文件中的函数？

### 出口要点

ES6 还引入了称为 *模块* 的概念。ES6 模块存储在文件中。在我们的例子中，我们可以将 `es8-functional.js` 文件本身视为一个模块。伴随着模块的概念，出现了 `import` 和 `export` 语句。在我们的运行示例中，我们必须 *导出* `forEach` 函数，以便其他人可以使用它。我们可以将清单 2-10 所示的代码添加到我们的 `es8-functional.js` 文件中。

```javascript
const forEach = (array,fn) => {
   let i;
   for(i=0;i<array.length;i++)
      fn(array[i])
}
export default forEach
```

清单 2-10 导出 `forEach` 函数

### 进口要点

既然我们已经导出了函数，如清单 2-10 所示，让我们通过 `import` 来使用它。打开文件 `play.js` 并添加清单 2-11 所示的代码。

```javascript
import forEach from '../lib/es8-functional.js'
```

清单 2-11 导入 `forEach` 函数

这行代码告诉 JavaScript 从 `es8-functional.js` 导入名为 `forEach` 的函数。现在该函数在整个文件中都以名称 `forEach` 可用。现在将代码添加到 `play.js` 中，如清单 2-12 所示。

```javascript
import forEach from '../lib/es8-functional.js'
var array = [1,2,3]
forEach(array,(data) => console.log(data)) //refereing to imported forEach
```

清单 2-12 使用导入的 `forEach` 函数

### 使用巴别塔节点运行代码

让我们运行`play.js`文件。因为我们在文件中使用的是最新版本，所以我们必须使用`Babel-Node`来运行我们的代码。`Babel-Node`用于传输我们的代码并在节点js上运行。`Babel-Node`应该与`babel-cli`一起安装。

So, from our project root directory, we can call the `babel-node` like this: `babel-node functional-playground/play.js --presets es2017` This command tells us that our `play.js` file should be transpiled with `es2017` and run into node js. This should give the output as follows: `1 2 3` Hurray! Now we have abstracted out for logic into a function. Imagine you want to iterate and print the array contents with multiples of 2. How will we do it? Simply reuse our `forEach`, which will print the output as expected: `forEach(array,(data) => console.log(2 * data))`

### 注意

我们将在整本书中使用这种模式。我们用命令式方法讨论问题，然后继续实施我们的函数技术，并在`es8-functional.js`的函数中捕获它们。然后我们用它在`play.js`文件中播放！

### 在 Npm 中创建脚本

We have seen how to run our `play.js` file, but it’s a lot to type. Each time we need to run the following: `babel-node functional-playground/play.js --presets es2015-node5` Rather than entering this, we can bind the command shown in Listing 2-13 to our npm script. We will change the `package.json` accordingly:

```json
{
  "name": "learning-functional",
  "version": "1.0.0",
  "description": "Functional lib and examples",
  "main": "index.js",
  "scripts": {
    "playground": "babel-node functional-playground/play.js --presets es2017-node7"
  },
  "author": "Anto Aravinth @antoaravinth",
  "license": "ISC",
  "devDependencies": {
    "babel-preset-es2017-node7": "⁰.5.2"
  }
}
```

Listing 2-13 向`package.json`添加npm脚本

Now we have added the `babel-node` command to `scripts`, so we can run our playground file (`node functional-playground/play.js`) as follows: `npm run playground`

这将和以前一样运行。

### 从 Git 运行源代码

Whatever we are discussing in the chapter will go into a git repository ([`github.com/antoaravinth/functional-es8`](https://github.com/antoaravinth/functional-es8)). You can clone them into your system using git like this: `git clone https://github.com/antsmartian/functional-es8.git` Once you clone the repo, you can move into a specific chapter source code branch. Each chapter has its own branch in the repo. For example, to see the code samples used in Chapter 2, you need to enter this: `git checkout -b chap02 origin/chap02`

一旦您签出了分支，您就可以像以前一样运行操场文件。

## 摘要

在这一章中，我们花了很多时间学习如何使用函数。我们利用`Babel`工具在我们的节点平台上无缝运行我们的代码。我们还将项目创建为节点项目。在我们的节点项目中，我们看到了如何使用`Babel-node`来转换代码，并使用预置在节点环境中运行它们。我们还看到了如何下载该书的源代码并运行它。有了所有这些技术，在下一章，我们将集中讨论高阶函数的含义。我们将在后面的章节中解释ES7的异步/等待特性。