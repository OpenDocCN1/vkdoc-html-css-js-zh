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