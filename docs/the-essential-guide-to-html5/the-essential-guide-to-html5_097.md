# 箭头实现

箭头被设计为绘制在从一个圆的圆心到另一个圆圆心的连线上。但线条起始于圆的边界（圆周），尤其箭头尖端终止于第二个圆的边界。每个箭头被构建为一个三角形，其底边与连线垂直。这需要进行一系列计算，包括对垂直和水平线的特殊处理。

##### 实现细节

**注意** 附录中的示例遵循当前推荐的做法，即每个文档以`<!DOCTYPE html>`开头。我还使用`meta`标签声明字符编码。这些内容需要您在阅读中自行探究，目前先接受它们即可。`<!... >`结构是 HTML 中编写注释的方式。JavaScript 中的注释通过`//`表示单行注释，`/* ... 和 */`表示多行注释。

如前所述，文档的`body`部分包含一个`canvas`元素。`canvas`元素中的单行文本会在浏览器不支持`canvas`时显示。这能让旧版浏览器更友好地显示异常信息，被视为良好实践。`canvas`的尺寸通过将`width`和`height`设置为`100%`来指定占满整个窗口。

```
<body onload="init();" onresize="init();">
<canvas id="canvas" width="100%" height="100%">
  您的浏览器不支持 canvas
</canvas>
</body>
```

您已在本书中见过类似内容。同样，您也见过动态创建 HTML 元素的例子（如第 6 章）。在页面加载和窗口调整大小时调用的`init`函数，会调用`buildCircles`函数为每个圆创建新的`canvas`元素，然后调用`drawLinks`函数绘制箭头（连线）。圆和连线的规格定义在`script`元素开头的变量声明中。

```
var circles=[
  [200,300,20,"red"],
  [400,300,40,"blue"],
  [200,500,80,"purple"],
  [100,100,40,"pink"]
];

var links =[
  [0,1],
  [2,0],
  [1,2],
  [0,3],
  [3,1]
];
```

`circles`和`links`数组代表平行结构。`circles`数组中的每个项目本身是一个数组，包含位置（水平和垂直坐标）、半径和颜色。`links`数组中的每个项目是一个包含对`circles`数组引用的数组。因此，`links[0]`表示从第 0 个圆到第 1 个圆的箭头。

请记住，数组索引从 0 开始。当圆被移动时，`circles`数组中保存的水平和垂直坐标将发生变化。探索此程序的一个步骤是向`circles`和`links`数组添加内容。您需要使用标准颜色之一来指定圆的颜色。

回到`init`函数，它设置某些变量，确保`canvas`尺寸与窗口匹配，然后调用`buildCircles`和`drawLinks`函数。`buildCircles`函数如其名所示。通过`for`循环，它为`circles`数组中描述的每个圆创建一个`canvas`元素，并将其追加到`body`元素中以成为文档的一部分。它设置其为可见状态，并在该`canvas`元素上绘制指定大小的圆，并用指定颜色填充。每个`canvas`元素被推入`canvases`数组，每个圆对应一个。每个新创建的`canvas`元素都会添加一个名为`aindex`的新属性，以引用`circles`数组中的项目。该属性用于改变位置。该函数还为`mousedown`事件调用`addEventListener`，事件处理程序指定为`startDragging`函数。也就是说，它为玩家在特定对象上按下鼠标按钮时的事件处理进行了设置。

`startDragging`函数会针对该对象两次调用`addEventListener`。



### 排版后的文本

事件已被触发：在某个 `canvas` 元素上。其中一次事件是 `mousemove`，事件处理程序是 `moving()` 函数。另一次事件是 `mouseup`，事件处理程序是 `stopmove()` 函数。`moving()` 函数用于移动对象，即包含圆形的 `canvas`。请记住，正如我之前所写的，对象的位置由 `left` 和 `top` 属性指定。这意味着需要使用 `String()` 函数将数字转换为字符串，使用 `+` 运算符将这些字符串与 `px` 拼接，并使用 `parseInt()` 函数提取数值。由于底层的 JavaScript 程序会响应事件并多次调用 `moving()` 函数，因此 `drawLinks()` 函数也会被多次调用，用于重新绘制链接（箭头）。移动过程看起来十分流畅。我认为，理论上有可能让事件处理程序承担过多工作，但我并没有遇到过这种情况。

我将链接的绘制拆分成两个函数：`drawLinks()` 和 `drawAdjustedLink()`，以便单独处理垂直或水平线的特殊情况。将一个大型任务分解成若干较小的任务，始终是一个好做法。

表 A-1 列出了每个函数，并说明了它们是如何被调用的，以及它们调用了哪些函数（如果有的话），或者设置了哪些事件处理程序来进行调用。

## 附录 关于绘图的更多技巧

#### **表 A-1.** 圆形与箭头函数调用关系表

| **函数** | **被谁调用** | **由事件调用** | **调用谁** | **设置事件处理** |
| :--- | :--- | :--- | :--- | :--- |
| `init` | `onLoad`, `buildCircles`, `onResize` | | `drawLinks` | |
| `buildCircles` | `init` | `mouseDown` 事件处理程序 | `startDragging` | |
| `drawLinks` | `init`, `drawAdjustedLink` | | `moving`, `stopMove` | |
| `startDragging` | 在 `buildCircles` 中通过 `mouseDown` 设置 | `MouseMove` 事件处理程序 `moving`, `mouseUp` 事件处理程序 `stopMove` | | |
| `moving` | 在 `startDragging` 中通过 `mouseMove` 设置 | | | `drawLinks` |
| `stopMove` | 在 `startDragging` 中通过 `mouseUp` 设置 | | | `drawLinks` |
| `drawAdjustedLink` | `drawLinks` | | | |

请注意（并赞赏这一点）：我们无需编写代码来确定鼠标下方是哪个圆形！对象的事件处理机制已经完成了这项工作。

包含解释的完整代码见表 A-2。

#### **表 A-2.** 圆形与箭头代码

| **代码语句** | **解释** |
| :--- | :--- |
| `<!DOCTYPE html>` | DOCTYPE 注释。 |
| `<html>` | 开始标签 `<html>`。 |
| `<head>` | 开始标签 `<head>`。 |
| `<meta charset="UTF-8">` | 指定标准字符集。 |
| `<meta name="viewport" content="initial-scale=1">` | 标准 `<meta>` 标签。 |
| `<title>Circles and arrows</title>` | 设置标题，该标题将显示在浏览器的标签页上。 |
| `<style>` | 开始标签 `<style>`。 |
| `body {font-family:Garamond,serif; font-size: 24px; position:absolute;}` | 开始标签 `<body>` 的样式，指定字体……字号……定位方式。 |
| `}` | 关闭 body 样式定义。 |
| `canvas {position:absolute;}` | canvas 元素的样式规定。 |
| `</style>` | 关闭 style 标签。 |
| `<script>` | 开始标签 `<script>`。 |
| `var circles=[ [200,300,20,"red"], [400,300,40,"blue"], [200,500,80,"purple"], [100,100,40,"pink"] ];` | circles 数组声明和赋值的开始：一个包含多个数组的数组。红色圆形。蓝色圆形。紫色圆形。粉色圆形。 |
| `var links =[ [0,1], [2,0], [1,2], [0,3], [3,1] ];` | links 数组声明和赋值的开始：一个包含多个数组的数组。 |
| `var canvases = new Array();` | canvases 数组的声明和初始化。该数组将由所创建的、包含圆形的 canvas 元素填充。 |
| `var canvas;` | 将保存对原始 canvas 的引用。 |
| `var ctx;` | 将保存对原始 canvas 上下文的引用。 |
| `var cWidth;` | 将保存 canvas 宽度。 |
| `var cHeight;` | 将保存 canvas 高度。 |
| `var movingObject;` | 将保存对移动对象的引用。该值会发生变化。 |
| `var movingObjectIndex;` | 将保存移动对象在 canvases 数组中的索引。 |
| `var oldx;` | 将保存先前的 x 坐标。在 `moving` 函数中使用。在 `startDragging` 中设置，在 `moving` 中重置。 |
| `var oldy;` | 将保存先前的 y 坐标。在 `moving` 函数中使用。在 `startDragging` 中设置，在 `moving` 中重置。 |

（*续*）



