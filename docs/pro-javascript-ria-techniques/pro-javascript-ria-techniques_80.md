# IE 浏览器使用的语法与其他浏览器不同。为了在网页应用中使用矢量图形，你需要一种简单的方法来指定能被两种不同模型支持的矢量图形。我们通常需要使用 JavaScript 库来消除跨浏览器的不一致性，如第 2 章所述。在这种情况下，我们可以使用由开发者 **Dmitry Baranovskiy** 构建的可重用 JavaScript 库 **Raphaël**。

`Raphaël` 通过一个简单统一的 JavaScript API，简化了以跨浏览器方式创建自定义矢量图形的过程。该库会根据浏览器选择合适的矢量图形绘制技术：SVG 或 VML。你可以从项目主页 [`raphaeljs.com/`](http://raphaeljs.com/) 下载该库，该网站还提供了许多组件的运行示例。该库组件压缩后的代码体积为 53KB。

正如你所见，使用 SVG 和 VML 创建的每个图形对象也都是 DOM 对象，这意味着你可以像处理任何其他页面元素一样，为它们添加事件处理器或动态修改现有的图形元素。

举个例子，假设你想用折线图向网页应用的最终用户展示一组数据。这些数据是不断变化的——也许你正使用 Ajax 每 30 秒从服务器获取一次新数据。你希望在数据到达时，图表能反映出这些变化。图 11-2 展示了用户可能看到的界面。点击“获取新数据”链接，模拟从服务器接收新数据并更新图表的过程。将鼠标悬停在图表上的每个点时，该点会高亮显示并显示其精确数值。

![Image 35](img/index-388_1.png)

**364**

## 第 11 章 ■ 在浏览器中绘图

**图 11-2.** *动态更新图表*

`清单 11-5` 展示了如何使用 `Raphaël` JavaScript 库在 HTML 页面中动态构建图 11-2 所示的图表，以确保跨浏览器兼容性。

首先，创建一个空白画布，然后绘制网格并添加正确的 x 轴和 y 轴标签。接着，标记数据点并在点之间绘制线条。当点击“获取新数据”链接时，会随机生成新数据，现有线条和点将从画布上移除，并在其位置添加新的数据点和线条。

**清单 11-5.** *使用 Raphaël JavaScript 库绘制动态图表*

```html
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN"
    "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
<html lang="en" xml:lang="en" xmlns="http://www.w3.org/1999/xhtml">
<head>
  <title>动态图表</title>
  <meta http-equiv="content-type" content="text/html;charset=utf-8" />
</head>
<body>
  <h1>动态图表</h1>

  <!-- 稍后我们将通过 JavaScript 在该元素中填充一个按钮 -->
  <p id="button"></p>

  <!-- 稍后我们将使用 JavaScript 在该元素中填充图表 -->
  <div id="canvas"></div>

  <!-- 引用 $ JavaScript 库 -->
  <script type="text/javascript" src="$.js"></script>

  <!-- 引用 Raphaël JavaScript 库 -->
  <script type="text/javascript" src="raphael.js"></script>

  <!-- 为简单起见，我将生成图表的 JavaScript 代码包含在页面内。
       在你的网页应用中，你应该从外部文件引用它 -->
  <script type="text/javascript">
    // 声明一个新的构造函数，它将绘制并代表我们的图表
    var Graph = function(input) {
        // 期望的输入包括：x 轴数据标签（数组形式）、数据值本身（数组形式）、
        // 画布的宽度和高度，以及用于填充画布的元素本身
        this.labels = input.labels || [];
        this.data = input.data || [];
        this.width = input.width || 600;
        this.height = input.height || 300;
        this.element = input.element || $.Elements.create("div");

        // 使用 Raphaël 创建一个新的跨浏览器画布。这个 paper
  </script>
</body>
</html>
```

**365**



// `property` 表示一个对象，我们后续可以对其执行方法来修改和影响新画布
`this.paper = Raphael(this.element, this.width, this.height);`

// 确定数据数组中的最大值，并四舍五入到最近的 100。该值将作为
// 图表 y 轴上的最大值
`this.maximumDataValue = Math.ceil(Math.max.apply(Math, this.data) / 100) * 100;`

// `buildGrid` 方法在画布上绘制网格，包括
// 坐标轴标签，并返回实际网格位置的 `x` 和 `y` 坐标
// （假设为标签预留了空间），以及网格本身的宽度和高度（不含标签）
`this.buildGrid = function() {`

  // 定义分配给坐标轴标签的高度和宽度
  `var xLabelHeight = 20, yLabelWidth = 20;`

  // 确定网格本身的 x 和 y 坐标，以及
  // 网格的宽度和高度（不含坐标轴标签）
  `var x = yLabelWidth, y = 20;`
  `var width = this.width - yLabelWidth;`
  `var height = this.height - xLabelHeight - y;`

  // 计算网格每个方向上要绘制多少条线
  `var horizLines = this.data.length * 2;`
  `var vertLines = (this.maximumDataValue / 10);`

  // 使用 Raphaël 以浅灰色（十六进制颜色 #ccc）绘制网格
  `this.paper.drawGrid(x, y, width, height, horizLines, vertLines, "#ccc");`

  // `drawXLabels` 函数创建并定位图表 x 轴上的标签
  `var drawXLabels = function() {`
    `for (var index = 0, length = this.data.length; index < length; index++) {`
      `var x = yLabelWidth + ((index / this.data.length) * width) + (width/(2 * this.data.length));`
      `var y = this.height - (xLabelHeight/2);`

      // 使用 Raphaël 将标签文本绘制到画布上
      `this.paper.text(x, y, this.labels[index]).attr({`
        `"font": '10px "Arial"',`
        `stroke: "none",`
        `fill: "#000"`
      `});`
    `}`
  `}.call(this);`

  // `drawYLabels` 函数基于我们之前计算并四舍五入到最近 100 的
  // 最大数据值，创建并定位 y 轴上的标签。这使我们能够
  // 使用整数作为坐标轴标签
  `var drawYLabels = function() {`
    `for (var index = 0, length = vertLines; index <= length; index++) {`
      `var labelText = (index * this.maximumDataValue) / vertLines;`
      `var labelPosition = height - (vertLines * index * height / this.maximumDataValue) + y;`

      // 使用 Raphaël 将标签文本绘制到画布上
      `this.paper.text(yLabelWidth / 2, labelPosition, labelText).attr({`
        `"font": '10px "Arial"',`
        `stroke: "none",`
        `fill: "#000"`
      `});`
    `}`
  `}.call(this);`

  // 现在返回画布中网格起始点的 x 和 y 坐标，
  // 以及网格的宽度和高度
  `return {`
    `x: x,`
    `y: y,`
    `width: width,`
    `height: height`
  `}`
`}`

// 执行 `buildGrid` 方法，将返回的值存储到
// `grid` 属性中
`this.grid = this.buildGrid();`

// `drawPath` 方法绘制数据点并在
// 现有网格上绘制连线
`this.drawPath = function() {`

  // 要使用 Raphaël 绘制一条线，需要创建一个路径，并为其
  // 设置一个 `stroke-width` 值，该值等于该线的宽度
  `var pathAttributes = {`
    `stroke: "#333",`
    `"stroke-width": 4,`
    `"stroke-linejoin": "round"`
  `};`

  `this.path = this.paper.path(pathAttributes);`

  // 创建数组，用于存储对将要绘制到网格上的数据点、文本和
  // 形状的引用
  `this.points = [];`
  `this.text = [];`
  `this.rects = [];`

  // 遍历数据数组中的每一项
  `for (var index = 0, length = this.data.length; index < length; index++) {`

    // 计算网格上数据点的 x 和 y 坐标位置，
    // 该点将代表当前数据项
    `var x = this.grid.x + (index * (this.grid.width / this.data.length)) + (this.grid.width / (2 * this.data.length));`
    `var y = this.grid.y + this.grid.height - (this.data[index] * this.grid.height / this.maximumDataValue);`

    // 第一个数据项将由一个点表示，并且
    // 不会绘制连线到其位置。所有其他
`}`



```javascript
// 数据项将通过线条连接到它们

if (index == 0) {
    this.path.moveTo(x, y, 10);
} else {
    this.path.lineTo(x, y, 10);
}

// drawPoints 函数负责在网格上渲染数据点，
// 并在每个数据点上方显示一个文本标签，该标签默认隐藏，
// 只有当用户将鼠标悬停在一个覆盖了该点和其周围大范围区域的
// 不可见矩形上时，文本标签才会显示。
// 这为鼠标交互提供了更大的操作区域，
// 无需用户精确定位网格上较小的数据点即可查看与之关联的文本标签

var drawPoints = function() {
    // 绘制一个从网格顶部到底部的不可见矩形，该矩形覆盖代表数据值的数据点。
    // 透明度为 0 使矩形不可见，但仍存在于网格上
    var rect = this.paper.rect(this.grid.x + (this.grid.width * index / this.data.length), this.grid.y, (this.grid.width * (index + 1) / this.data.length), this.grid.height).attr({ stroke: "none", fill: "#fff", opacity: 0 });

    // 将代表该矩形的对象添加到一个数组中
    this.rects.push(rect);

    // 在网格上绘制一个代表数据值的数据点，并将其添加到所有数据点的数组中
    var point = this.paper.circle(x, y, 5).attr({
        'fill': "#333"
    });
    this.points.push(point);

    // 在数据点上方绘制一个文本标签，并将其添加到标签数组中
    var text = this.paper.text(x, y - 15, this.data[index]).attr({
        "font": '10px "Arial"',
        stroke: "none",
        fill: "#000"
    });
    this.text.push(text);

    // 由于代表画布中每个形状或对象的 XML 元素可以使用标准 DOM 进行操作，
    // 因此我们可以像使用 HTML 元素一样，使用诸如 insertAfter 之类的 DOM 方法
    text.insertAfter(dot);

    // 默认情况下隐藏文本标签
    text.hide();

    // 由于画布上创建的元素的行为与标准 HTML 元素类似，
    // 我们可以为它们添加事件。
    // 此处，我们使用 Raphaël 在用户将鼠标悬停在数据点周围的不可见矩形元素上时，
    // 动态改变先前创建的数据点的颜色和大小，并显示文本标签
    rect.mouseover(function() {
        point.attr({
            "fill": "#999",
            "r": 7
        });
        text.show();
    });

    // 当用户将鼠标移出数据点周围的矩形时，
    // 文本标签再次隐藏，数据点恢复为其原始大小和颜色
    rect.mouseout(function() {
        point.attr({
            "fill": "#333",
            "r": 5
        });
        text.hide();
    });

}.call(this)
}

// 现在执行刚刚描述的 drawPath 方法
this.drawPath();

// replaceData 方法用于移除网格上的矢量图形，但保持网格本身不变，
// 以便绘制新的数据
this.replaceData = function(data) {
    // 新数据作为方法的输入参数传入，并用新数据替换现有数据
    this.data = data;

    // 从网格上移除线条
    this.path.remove();

    // 逐个从网格上移除数据点
    for (var index = 0, length = this.points.length; index < length; index++) {
        this.points[index].remove();
    }

    // 从网格上移除文本标签
    for (var index = 0, length = this.text.length; index < length; index++) {
        this.text[index].remove();
    }

    // 从网格上移除不可见的矩形形状
    for (var index = 0, length = this.rects.length; index < length; index++) {
        this.rects[index].remove();
    }

    // 执行 drawPath 方法，该方法将在网格上绘制线条、数据点和文本标签
    // 本次将使用新数据
    this.drawPath();
}

// 实例化 Graph 构造函数，传入数据、标签以及用于放置新图表的 DOM 元素
var myGraph = new Graph({
    labels: [2004, 2005, 2006, 2007, 2008, 2009],
    data: [0, 47, 32, 100, 78, 89],
    element: document.getElementById("canvas")
});

// 在页面上放置一个'获取新数据'按钮，点击该按钮将随机生成一组新的数据值。
// 此操作旨在模拟......
```



// 在真实的 RIA 中，这可以是一个 Ajax 调用，用于从服务器获取新的未知数据

```javascript
var getNewData = $.Elements.create("a");
getNewData.innerHTML = "获取新数据";
document.getElementById("button").appendChild(getNewData);

$.Events.add(getNewData, "click", function(e) {
    e.preventDefault();
    // 随机生成一组新数据，每个值在
    // 0 到 100 的范围内
    var data = [];
    for (var index = 0, length = 5; index < length; index++) {
        data.push(Math.round(Math.random() * 100));
    }
    // 执行 Graph 实例的 replaceData 方法，将
    // 新数据集渲染到网格上
    myGraph.replaceData(data);
});
```

你应该熟悉这个强大的 JavaScript 库所支持的全部功能。当你需要在你的富互联网应用程序（RIA）中动态显示矢量图形时，都可以使用它。

### 使用 HTML 5 的 `<canvas>` 标签

Mac OS X Dashboard 功能使用 HTML、CSS 和 JavaScript 来生成小工具，例如时钟、日历、天气和股票报告。在构建此功能时，Apple 开发人员引入了一个新的 HTML 标签，以支持在这些小工具中动态创建图形组件。该标签 `<canvas>` 被 WHATWG 推荐，并且 Safari 2、Firefox 1.5、Google Chrome 和 Opera 9.5 都增加了对该标签的支持。IE 在直到第 8 版的任何版本中都不支持此标签（但人们一直期望开发人员会在下一版本中包含它）。如本节末尾所述，已经有一些工作利用 Flash Player 插件在 IE 中提供了 `<canvas>` 标签的实现。

与 SVG 不同，在页面的 `<canvas>` 标签内不会创建任何标签；相反，图形完全通过使用原生的 JavaScript API 生成。因此，标签内的图形组件无法使用 CSS 设置样式，也无法通过 DOM 进行操作或绑定事件。如果你希望将 DOM 事件连接到图形以提供用户交互，请改用 SVG。

### 在浏览器中绘图

该标签内只允许使用两种基本形状：矩形和线条，这两种形状可以通过弯曲和操作来创建所有其他所需的形状。图形本身不是基于矢量的，因此如果你需要对标签内的图形组件进行任何大小调整，你必须根据添加到标签中的图形，自己编写代码来执行此操作。

`<canvas>` 标签的一个优点是它很好地支持嵌入和操作图像文件中的图片，这是使用 SVG 难以实现的。`<canvas>` 标签的性能优于 SVG，尤其是在处理大型复杂图形时。这很可能是因为 SVG 必须将对每个图形组件的引用作为 DOM 元素来维护；而 `<canvas>` 没有这个要求。

清单 11-6 展示了如何在 HTML 页面中使用 `<canvas>` 标签和 JavaScript 绘制一个带有渐变填充背景的矩形。

**清单 11-6.** *一个简单的 Canvas 标签图像*

```html
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN"
    "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
<html xml:lang="en" lang="en" xmlns="http://www.w3.org/1999/xhtml">
<head>
    <title>Canvas 演示</title>
</head>
<body>
    <h1>Canvas 演示</h1>
    <!-- <canvas> 标签采用可选的 width 和 height 属性。如果省略这些属性，画布通常是 300 像素宽，150 像素高 -->
    <canvas id="canvas" width="300" height="300"></canvas>
    <!-- 以下脚本为简单起见包含在此处。理想情况下，你应该
    从单独的文件中引用代码 -->
    <script type="text/javascript">
        // 获取 <canvas> 标签的 DOM 引用
        var canvas = document.getElementById("canvas");
        // <canvas> 标签内实际的绘图表面称为
        // *渲染上下文*。绘图方法与上下文相关联，
        // 因此我们必须首先获取对此上下文的引用。目前，
        // <canvas> 标签仅支持 2D 形状渲染上下文。在
        // 未来，预计将能够进行 3D 形状渲染
        var context = canvas.getContext("2d");
        // 定义一个渐变，从 #000000 到 #cccccc 在 200 像素的
        // 水平空间上渐变
        var gradient = context.createLinearGradient(0, 0, 200, 0);
        gradient.addColorStop(0, "#000000");
        gradient.addColorStop(1, "#cccccc");
        // 要在矩形中使用渐变，我们将渐变色定义为当前
        // 填充样式，然后执行 fillRect 方法
        context.fillStyle = gradient;
        // 从位置 (20, 20) 开始绘制渐变填充矩形，从该点开始
        // 延伸 200 像素宽和高
        context.fillRect(20, 20, 200, 200);
    </script>
</body>
</html>
```

Mozilla 在 `https://developer.mozilla.org/en/Canvas_tutorial/` 上提供了一个关于 `<canvas>` 标签及其相关 API 的有用教程。你还可以在 Canvas Demos 网站（`http://www.canvasdemos.com/`）上看到其他开发人员使用 `<canvas>` 标签的演示。

为了解决 IE 不支持 `<canvas>` 标签的问题，开发人员 Grant Jones 尝试构建一个可重用的 Flash 组件，以提供对此标签的实现。他的代码通过在 IE 中的 JavaScript 中复制 `<canvas>` 标签 API，并将代码发送到此浏览器中的 Flash 组件来代替。他的任务是确定这种方法是否能够在不使用其他库（如 Raphaël）中基于 VML 的渲染的情况下，在 IE 中构建响应更快的图形绘制引擎。他在 `http://www.azarask.in/blog/post/flash-canvas/` 的一篇博客文章中相当详细地描述了他的经验。他的结论表明，这种方法并不能比任何原生实现提供更快合适的图形渲染能力。JavaScript 到 Flash 的通信相当慢，而且跨通信越多，整体结果就越慢。在一个特定的测试中，他注意到使用 Flash 技术比在其他浏览器中使用原生 `<canvas>` 渲染慢 40 倍。目前，建议你使用 Raphaël，而不是依赖不能带来性能提升的第三方插件。

### 总结

在本章中，你了解了在现代浏览器中实现矢量图形绘制的不同技术。你了解了一个第三方 JavaScript 库，它允许你在自己的 Web 应用程序中轻松创建自定义的动态矢量图形。你还了解了 HTML 5 中一个用于在页面上动态渲染图形的新标签。在本书的最后一章中，我将回到可访问性的主题，揭示如何使用当前技术和新兴的浏览器标准使你的 RIA 尽可能易于访问。

