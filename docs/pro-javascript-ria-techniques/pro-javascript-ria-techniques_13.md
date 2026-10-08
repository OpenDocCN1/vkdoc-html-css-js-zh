# 第 4 章：性能调优

**清单 4-1.** `在不阻塞浏览器的情况下按需加载 JavaScript 文件`

```javascript
$.prototype.Remote.loadScript = function(fileName, callback) {

  var scriptTag = document.createElement("script");

  scriptTag.src = fileName;

  if (callback) {

    scriptTag.onload = callback;

    scriptTag.onreadystatechange = function() {

      if (scriptTag.readyState == 4) {

        callback();

      }

    }

  }

  document.getElementsByTagName("head")[0].appendChild(scriptTag);

}

// 示例用法
// 假设页面上已存在 $ 库的实例
// 加载 my-script.js，完成后输出 "script loaded and available"
$.Remote.loadScript("my-script.js", function() {

  alert("script loaded and available!");

});
```

在第 2 章中，我介绍了在页面的整个 DOM 初始化完毕并可访问后立即执行 JavaScript 代码的能力。这发生在整个页面加载完成之前，因为它只考虑 HTML 已加载，而不考虑任何引用的外部资源（如图片）。通过编写代码使其在此事件触发时执行，如清单 4-2 所示，您可以让代码更早开始工作，从而减少最终用户的额外等待时间。您可以使用此事件并行加载多个 JavaScript 文件，采用清单 4-1 中概述的技术。

**清单 4-2.** `当 DOM 就绪时立即执行脚本`

```javascript
$.onDomReady(function() {
    // 此函数在 DOM 可访问时立即执行
    // 同时加载两个 JavaScript 文件：my-script.js 和 my-other-script.js
    $.Remote.loadScript("my-script.js", function() {
        // my-script.js 已加载
    });
    $.Remote.loadScript("my-other-script.js", function() {
        // my-other-script.js 已加载
    });
});
```

## 提升核心 JavaScript 性能

我所指的`核心`JavaScript，是指语言中专注于纯数据处理、执行、循环和逻辑的部分，而不是通过 DOM 或 Ajax 技术进行页面操作的部分。在此，我将提出一些改进措施，您可以将其应用到自己的核心 JavaScript 代码中，以加快执行速度。

### 使用记忆化器

`记忆化器`是一种兼作存储机制的函数。它保存该函数例程先前执行的结果，以便将来使用相同输入调用该函数时，能从存储的结果列表中返回输出，而无需重新计算结果。这提高了在代码中被多次调用的函数的性能。

只有那些基于相同输入集始终产生相同输出的函数才能被“记忆化”。

清单 4-3 展示了如何构建一个实用函数，允许您为任何函数添加存储结果值的能力，在可能的情况下自动从其内部存储属性返回函数结果，以提升性能。和之前一样，将此代码添加到`$`JavaScript 库中。

**清单 4-3.** `使用记忆化器加速重复函数的执行时间`

```javascript
// memoize 接受一个函数作为输入，并返回具有存储能力的同一函数
$.prototype.Utils.memoize = function(func) {
    return function() {
        // 如果不存在，则为该函数添加一个 memory 对象属性
        func.memory = func.memory || {};
        
        // 创建一个键，用于在 memory 对象属性中存储和检索函数结果。
        // 该键应基于传递给函数的所有参数的组合，以确保基于所有输入组合的唯一性
        arguments.join = Array.prototype.join;
        var key = arguments.join("|");
        
        // 键是否存在于 memory 对象中？
        if (key in func.memory) {
            // 如果存在，则返回关联的值以避免重新计算
            return func.memory[key];
        } else {
            // 如果不存在，则执行关联的函数，然后将结果保存到 memory 对象中
            func.memory[key] = func.apply(this, arguments);
            // 返回新保存的值，即函数执行的结果
            return func.memory[key];
        }
    }
};

// 示例用法
// 假设页面上存在一个$库的实例
// 编写一个计算给定数字阶乘的函数
// - 执行它 99999 次以夸大慢速运行函数的效果
var computeFactorial = function(input) {
    var result;
    for (var count = 0; count < 99999; count++) {
        result = 1;
        for (var num = 2; num <= input; num++) {
            result *= num;
        }
    }
    return result;
}

// 为阶乘函数添加记忆化能力
computeFactorial = $.Utils.memoize(computeFactorial);
```

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

# 性能调优

## 循环控制：`continue` 与 `break`

你可以使用 `continue` 语句跳过当前循环迭代，直接进入下一次迭代。`break` 语句则会完全终止循环。这两条命令能帮助你找到所需的值，或者跳过与当前循环迭代无关的某些代码块。下面展示了一个同时使用这两条命令的示例。

```
var myArray = [10, 20, 30, 40, 50, 60, 70, 80, 90, 100];

for (var index = 0, length = myArray.length; index < length; index++) {
    if (myArray[index] < 50) {
        // 忽略数组中所有低于 50 的值
        // continue 会立即执行下一次迭代，跳过循环内的任何其他代码
        continue;
    }
    if (myArray[index] == 90) {
        // 忽略数组中所有高于 90 的值
        // break 会立即停止循环迭代，跳过循环内的任何其他代码
        // 循环中不会再执行任何其他迭代
        break;
    }
}
```

## 即将推出：后台 Web Worker 进程

Web 超文本应用程序技术工作组（WHATWG）是一个由开发者和公司组成的社区，致力于推动 Web 新技术的采纳和推荐。目前，该工作组正忙于制定一项名为 Web Workers 的规范，它将允许开发者将 JavaScript 代码分配到后台线程中执行，而不会妨碍用户与浏览器的交互，也不会阻塞其他代码的运行。

这项推荐规范尚处于早期阶段，但已在 Mozilla Firefox 3.5 和基于 WebKit 的浏览器中以初步形态构建。它发布于 `www.whatwg.org/specs/web-workers/current-work/`。待规范完成后，你将能够在后台运行耗时较长的代码，而不会影响页面其他部分的性能。（我个人已迫不及待地希望这项规范能完成并实现在浏览器中——它将使核心 JavaScript 性能问题成为历史！）

---

# 提升 Ajax 性能

如第 2 章所述，Ajax 是大多数富互联网应用程序（RIA）的主要组成部分，它让开发者能够在浏览器中保持对用户界面的控制，同时在后台向 Web 服务器发送和接收数据。我们已经确认，RIA 的一个主要性能瓶颈在于浏览器与 Web 服务器之间的网络连接，因此你需要尽可能地进行所有调整，以提升代码的性能和效率，从而最大限度地减少这种影响。

## 使用 JSON 格式处理响应

你可以使用 Ajax 技术从服务器请求任何基于文本的数据，尽管该技术及其底层代码确实为 XML 格式提供了内置支持，能在可能的情况下将服务器响应转换为 JavaScript 中的 XML 数据结构。但从性能角度来看，问题在于在大多数浏览器中通过 JavaScript 解析 XML 是一项极其缓慢的操作，因此应尽可能排除这一选项。

相反，应使用第 2 章介绍的 JSON 格式来编码数据。这种格式与 XML 一样具有层次结构，并以名称/值对的形式表示数据，就像 JavaScript 对象字面量一样。然而，它的优势在于极其轻量，因为它不依赖开放标签、闭合标签和嵌套标签来表示层次结构数据。你可以用更少的空间有效地表示相同的数据，这意味着你的 Web 应用程序能更快地获取数据。

由于接收到的数据是文本格式，不能直接作为 JavaScript 对象使用，因此你需要将 JSON 格式的文本响应解析为你可以在 RIA 中访问的对象字面量。JSON 格式的发明者道格拉斯·克洛克福特提供了一个开源的 JSON 解析器。该解析器可从 JSON 网站（`www.json.org/json2.js`）获取，你可以使用它将文本转换为原生对象。某些浏览器，包括 IE 8 和 Firefox 3.5，在浏览器本身内部提供了 JSON 解析器的原生实现，从而在这些浏览器中加速了解析过程。

# 使用 `<script>` 标签实现 JSON-P

早在 2005 年，Bob Ippolito 就在他的博客 [`bob.pythonmac.org/`](http://bob.pythonmac.org/) 上首次提出了另一种从服务器获取 JSON 数据的方法。这种方法被称为 JSON with Padding（简称 JSON-P），其技术原理是使用 JavaScript 在当前页面中动态创建一个 `<script>` 标签，指向一个返回 JSON 格式数据的 URL。这使得浏览器可以像处理任何其他脚本一样执行返回的数据。当然，JSON 数据与对象字面量表示法几乎相同，只是它仅仅是原始数据——它没有被赋值给一个变量，也没有被包裹在函数调用中。因此，当通过 `<script>` 标签执行 JSON 数据时，浏览器会解析它，但数据会立即消失。如果你能以某种方式将这些数据赋值给一个变量，或者将其包裹起来作为函数调用的输入，那么你就可以随心所欲地读取和操作这些数据了。

JSON-P 技术正是采用了这种方法。通过向请求的脚本传递一个已知的查询字符串参数，其中包含请求页面中某个函数的名称，服务器就应该将 JSON 数据包裹在对此名称函数的调用中返回。下面展示了请求带有或不带有此可选参数的 URL 如何影响返回的 JSON 数据。

<!-- 请求一个返回 JSON 格式数据的 PHP 脚本 -->

```
<script type="text/javascript" src="/my-json.php"></script>
```

<!-- 实际产生如下代码，这些代码会被执行。由于数据没有被赋值给变量或在函数调用中使用，它被视为对象字面量，但会立即消失 -->

```
<script type="text/javascript">
{
    to: "den@denodell.com",
    from: "me@denodell.com",
    subject: "Dinner tonight?",
    body: "Do you feel like having dinner tonight? Call me."
}
</script>
```

<!-- 请求相同的 URL，但带上一个可选的查询字符串参数 -->

```
<script type="text/javascript" src="/my-json.php?jsonp=myFunction"></script>
```

<!-- 假设 PHP 脚本知道如何处理该查询字符串参数，返回的 JSON 数据会被包裹在对指定函数名称的调用中。myFunction 函数会被执行，其输入数据是 JSON 数据，且已经是一个对象字面量，因此无需执行任何解析 -->

```
<script type="text/javascript">
myFunction({
    to: "den@denodell.com",
    from: "me@denodell.com",
    subject: "Dinner tonight?",
    body: "Do you feel like having dinner tonight? Call me."
});
</script>
```

当代码执行时，查询字符串参数中指定的函数会被调用，其输入就是所请求的 JSON 数据。这会利用浏览器内置的 JavaScript 解析器，将数据视为对象字面量；当浏览器接收到数据时，该函数会立即执行。

**列表 4-4** 展示了如何使用 JSON-P 技术从指定 URL 加载 JSON 数据，并将其原生地提供给一个函数，该函数在数据接收完毕后执行。将此代码添加到你从第 2 章开始编写的 `$` JavaScript 库中。

**列表 4-4.** *使用 JSON-P*

```
// loadJSONP 方法模仿了现有的 loadScript 方法，但允许
// 将返回的 JSON 数据作为输入提供给回调方法。
// 将此方法添加到我们的 $ JavaScript 库代码中。
$.prototype.Remote.loadJSONP = function(url, callback){

    // 回调函数需要存在于全局 window 对象中
    window.tempFunction = callback;

    // 将 jsonp=tempFunction 查询字符串参数附加到 URL 上。
    // 服务器应将以该参数中指定的函数名来包裹返回的 JSON 数据，这样在数据返回时它就会被执行
    url = url.contains("?") ? url + "&jsonp=tempFunction" : "?jsonp=tempFunction";

    // 调用现有的 loadScript 方法将 <script> 标签放置在页面中
    this.loadScript(url);
}

// 使用示例
// 假设页面上存在 $ 库的一个实例
// 请求文件 my-script.php?jsonp=tempFunction，该请求
// 返回包裹在 tempFunction 方法调用中的 JSON 数据
$.Remote.loadJSONP("my-script.php", function(data) {
    // 输出 "object"，表示返回了一个对象字面量
    alert(typeof data);
});
```

> **提示：** 与 Ajax 技术不同，JSON-P 不限于请求与发起请求的页面处于同一域名的数据，因为 `<script>` 标签可以从任何域名请求 JavaScript 文件。唯一的条件是返回 JSON 数据的外部脚本支持额外的查询字符串参数，以便将对指定函数名称的调用包裹在返回的数据上。

## 整合服务器请求与响应

不要短时间内多次发送数据请求，应考虑如何修改代码，以更低频率向服务器发送单个请求，并让服务器返回包含所有请求数据合并响应的单一数据块。这将减少浏览器与 Web 服务器之间的通信量，从而提升 RIA 的性能。

此外，如果你的 RIA 需要来自服务器的多次更新，请考虑使用 Ajax 将当前状态的数据发送到服务器，并配置你的服务器端脚本仅返回需要更改的数据的差异部分。相比正常响应，这种响应会小得多，更容易解析，并且能减轻 Web 服务器和网络的负担。

## 提升 DOM 性能

RIA 需要基于某种输入（通常来自用户或服务器）对页面的用户界面进行更新。需要使用 JavaScript DOM 来修改用户界面中的 HTML 元素或 CSS 属性。众所周知，这非常耗时，尤其是在较旧的浏览器中，正如你在第 3 章的性能基准测试结果中所看到的那样。本节提供了一些指南和代码片段，旨在帮助你通过 JavaScript 提升页面交互的性能。

### 最小化 DOM 访问

使用 JavaScript 访问 HTML 元素是一项耗时的操作，因此应尽可能少地执行。如果你需要访问一个 DOM 元素，请将其赋值给一个变量，并在代码的其余部分使用该变量引用，如下例所示：

```
// 在此例程中只定位一次 DOM 引用，并通过变量在其他地方引用它
var menu = document.getElementById('menu');

if ($.CSS.hasClass(menu, "navigation")) {
    $.CSS.addClass(menu, "menu");
    // 输出菜单元素的当前宽度
    alert($.CSS.getAppliedStyle(menu, "width"));
}
```

如果你需要动态创建 DOM 元素并将其添加到页面，请在将新元素添加到页面之前，应用所有属性并设置所有必要的属性。这样，浏览器就不需要持续访问活跃的 DOM 来进行修改。

当通过 DOM 定位页面元素时，尽可能过滤 DOM 树——遍历越少，访问速度越快。如果你在查找具有特定类名的元素，并且知道它存在于某个标签内，请先按此标签过滤节点树以更快地返回结果，如下例所示：

```
// 定位一个已知元素，它包裹着你想要遍历的节点树
var filterElement = document.getElementById("menu");

// 在 filterElement 内查找所有类名为 selected 的元素。
// 这比遍历页面的整个 DOM 节点树要快
var selected = $.Elements.getElementsByClassName("selected", filterElement);
```

### 使用 DocumentFragment 对象

DOM（文档对象模型）规范中一个常被忽视的部分是，可以在 JavaScript 中使用 `DocumentFragment` 对象作为某种“迷你 DOM”，在代码中创建、追加和操作元素，然后将整个迷你 DOM 节点树一次性地添加到页面中。与传统的逐个将 DOM 元素直接添加到页面相比，这种方法能显著提升性能。下面展示了如何使用该技术向页面添加一组元素。

# DocumentFragment 与内存优化

```
// 创建一个 DocumentFragment 对象作为离线的迷你 DOM，
// 它不与页面的活跃 DOM 连接。
var miniDOM = document.createDocumentFragment();

// 创建一些新的 DOM 元素
var p = document.createElement("p");
var hr = document.createElement("hr");
var h2 = document.createElement("h2");

// 将每个元素添加到 DocumentFragment 对象中
miniDOM.appendChild(p);
miniDOM.appendChild(hr);
miniDOM.appendChild(h2);

// 将这个 DocumentFragment 的副本添加到页面中。
// 只对活跃 DOM 执行一次操作意味着性能比多次操作有显著提升
document.body.appendChild(miniDOM.cloneNode(true));
```

## 解决内存泄漏

在大多数浏览器中，当创建 JavaScript 对象时，运行代码的机器或设备会为其分配内存。当这些对象被销毁或不再使用时，这些内存会被回收，供后续代码使用。

IE 6 和 7 存在一个内存管理问题，因为它们有两个内存管理器：一个用于核心 JavaScript 代码，另一个用于 DOM 对象。两者各自都能完美工作，但当它们之间出现循环引用时（如下例所示），两个管理器就会冲突，导致内存泄漏。

```
<div id="leaking"></div>

<script type="text/javascript">
// 一个 DOM 元素被赋值给一个变量，然后该变量又被赋值给原始 DOM 元素的一个属性，
// 从而创建了循环引用。
// 这会导致 Internet Explorer 6 和 7 中发生内存泄漏。
var leakyDiv = document.getElementById('leaking');
document.getElementById('leaking').newProperty = leakyDiv;
</script>
```

如果不加处理，这些内存泄漏最终会耗尽可用内存，导致浏览器崩溃。即使在页面刷新时，这些内存也不会被回收，因此可能会给最终用户带来严重问题。

IE 7 引入了一种变通方法：当用户导航到另一个页面或网站时，之前页面分配的内存会被释放。不幸的是，它只释放用户导航离开时页面上存在的 DOM 对象所占用的内存。如果你使用 JavaScript 从页面移除了一些 DOM 元素，那么因这些特定元素泄漏的内存将永远无法恢复。由于在 Web 应用程序中动态添加和移除 DOM 元素非常常见，因此在这些浏览器中，对于富互联网应用程序（RIA）来说，内存泄漏仍然是一个大问题。幸运的是，有一种方法可以在页面刷新时解决泄漏问题，即使用页面的`unload`事件来释放内存，如清单 4-5 所示。

**清单 4-5.** 解决内存泄漏

```
<div id="leaking"></div>

<script type="text/javascript">
var leakyDiv = document.getElementById('leaking');
document.getElementById('leaking').newProperty = leakyDiv;

$.Events.add(window, "unload", function() {
    // 在页面卸载前移除所有循环引用以解决泄漏
    document.getElementById('leaking').newProperty = null;
});
</script>
```

微软已经为 IE 7 发布了一款名为“JavaScript 内存泄漏检测器”的软件测试版，它可以帮助你检测内存泄漏。在构建 Web 应用程序时运行这个工具非常有用。它将帮助你确保捕获并解决所有内存泄漏——你肯定不希望最终用户因访问你的网站而导致浏览器崩溃！你可以从`http://blogs.msdn.com/gpde/pages/javascript-memory-leak-detector.aspx`下载该工具。

## 使用事件委托

从本质上讲，富互联网应用程序（RIA）通常是事件驱动的。它们等待事件发生，例如按钮点击、鼠标移动或按键，并在事件发生时执行特定的代码函数。对于 DOM 事件，即使事件发生在特定元素上，它也会从该元素向上冒泡，并发生在其周围的所有元素上。下面的示例演示了这种行为：

```
<body>
<div>
    <!-- 锚点标签上的事件也会在 <div> 和 <body> 标签上触发 -->
    <a href="/">返回首页</a>
</div>
</body>
```

在这里，对锚点`<a>`标签的点击事件也会在其周围的元素上触发，从最内层的元素开始，然后向外冒泡到页面结构的顶层。

你可能会觉得这种行为有点奇怪，但你可以利用它来提升编写事件关联代码时的性能。不要为每个元素创建单独的事件处理器，而是编写单个事件处理器来监听整个页面中发生的所有特定类型的事件。然后，你可以根据事件发生的元素的属性来分配要执行的操作，如清单 4-6 所示。这些被称为事件*委托*，因为它们成为了根据事件和页面元素的属性来委托操作的逻辑块。

**清单 4-6.** 事件委托

```
<a href="/">返回首页</a>

<script type="text/javascript">
// 监听整个文档中发生的点击事件
$.Events.add(document.body, "click", function(e) {
    // e.target 包含对事件实际发生的元素的引用
    if (e.target.tagName.toLowerCase() == "a") {
        alert("你点击了锚点标签");
    } else {
        alert("你点击了锚点标签以外的其他地方");
    }
});
</script>
```

通过只在顶层的`document.body`元素上监听每种事件类型一次，然后识别事件实际发生的元素，你可以确保在页面加载时只需要注册少量的事件。这有助于实现缩短最终用户能够与页面交互所需时间的总体目标。

## 更新 CSS 时，修改类名而非样式

通过 DOM，你可以直接操作 CSS 样式属性。任何通过 JavaScript 对 DOM 元素执行的操作都需要时间才能完成。当你需要将样式属性更新为已知的新值时，更好的解决方案是利用浏览器的渲染引擎能力，只需添加、移除或替换要修改元素上的`class`值。新的样式将包含在样式表文件中，而布局和设计代码本应放在那里。这样，只需一次 DOM 操作即可添加或移除大量的样式属性。清单 4-7 演示了这种技术。

**清单 4-7.** 更新应用的类名而非单个样式

```
<div id="menu"></div>

<script type="text/javascript">
$.Events.add(document.body, "click", function(e) {
    // 如果用户点击了 id 为 "menu" 的元素
    if (e.target.id == "menu") {
        // 切换该元素上的 "menu-highlight" 类。该类在样式表文件中被定义为具有不同的设计
        if ($.CSS.hasClass("menu-highlight")) {
            $.CSS.removeClass("menu-highlight");
        } else {
            $.CSS.addClass("menu-highlight");
        }
    };
});
</script>
```

## 复制现有节点而非创建新节点

# 高效创建 DOM 元素

在富互联网应用中，动态创建 DOM 元素是相当常见的需求。然而，使用标准的`document.createElement()`方法每次创建元素时都会带来性能损耗。根据经验，我发现复制现有节点比从头创建新节点更快。如果你需要创建多个具有相似属性的元素，可以先创建一个实例，然后使用`cloneNode()`方法复制该元素及其关联属性，从而提升性能。

基于这一认识，你可以在`$` JavaScript 库中添加一个新方法，以便在某些情况下加速 DOM 元素的创建，如清单 4-8 所示。

## 清单 4-8. 高效创建 DOM 元素

```javascript
$.prototype.Elements.create = function(tagName) {
  // 该方法利用了记忆化技术
  this.memory = this.memory || {};
  if (tagName in this.memory) {
    // 如果已存储了该标签名的元素，则复制它
    return this.memory[tagName].cloneNode(true);
  } else {
    // 创建该标签名的新元素并存储它
    this.memory[tagName] = document.createElement(tagName);
    return this.memory[tagName].cloneNode(true);
  }
};

// 使用示例
// 假设页面上存在一个 $ 库的实例
// 从头创建两个元素
var newH2Tag = $.Elements.create("h2");
var newPTag = $.Elements.create("p");

// 创建另一个与已创建元素类型相同的元素。
// 复制已存储的元素，比再次从头创建元素性能更高
var anotherH2Tag = $.Elements.create("h2"); // 使用复制的元素
```

# 以 HTML 字符串形式将元素追加到页面

向页面添加新 DOM 元素的正确方法是对现有页面元素使用 `appendChild()` DOM 方法。该方法的问题在于，在某些旧版浏览器中，它的运行速度相当慢，尤其当需要向页面添加大量新元素时。有一种更快但风险也更高的方法：将新元素写成 HTML 字符串，然后将该字符串赋值给页面元素的 `innerHTML` 属性。**清单 4-9** 展示了这种技术。

**清单 4-9.** *以 HTML 字符串形式动态添加页面元素*

```
<div id="container"></div>

<script type="text/javascript">
  var pageElement = document.getElementById("container");
  var newElementString = "<p>你好，世界！</p>";

  // 设置 DOM 元素的 innerHTML 属性比任何其他动态添加元素的方法都要快
  pageElement.innerHTML = newElementString;
</script>
```

如果你的富互联网应用需要向页面添加大量元素，而前面建议的 `DocumentFragment` 技术仍不奏效，你可以考虑将此方法作为最后的手段。这种方法存在风险，因为你有可能会向页面添加错误或无效的 HTML，导致页面布局错乱。想象一下，如果你在 HTML 字符串中打开了一个标签却从未关闭它。因此，如果必须使用此技术，请务必非常小心！

### 本章小结

本章涵盖了与性能相关的广泛主题。它提供了许多建议，帮助你通过调整 Web 服务器、HTML 标记、样式表、图片和 JavaScript 代码来提升富互联网应用的速度。

请记住，最终用户不会注意到良好的性能；他们只会感受到糟糕性能带来的痛苦。不要给他们任何抱怨的理由。善用本章中的丰富信息，为你的网站访问者提供他们应得的体验。

下一章将讨论一个不同但相关的主题——感知性能——即尽管后台可能正在发生什么，但要给用户一种应用响应迅速的感觉。
