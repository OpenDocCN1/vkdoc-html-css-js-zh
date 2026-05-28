# 本节展示了该游戏两个版本的完整代码。由于应用程序包含多个函数，本节为每个游戏提供了一个表格，说明每个函数调用了哪些函数，以及被哪些函数调用。

表 5-1 是多边形版本记忆游戏的函数列表。请注意，部分函数的调用是基于事件触发的。

***表 5-1.** 多边形版本记忆游戏的函数*

**函数** | **被谁调用** | **调用谁**
---|---|---
`init` | 响应 `body` 标签中的 `onLoad` 事件被调用 | `makeDeck`
`shuffle` | – | –
`choose` | 响应 `init` 中的 `addEventListener` 事件被调用 | `Polycard`；`drawPoly`（作为多边形的 `draw` 方法被调用）；`flipBack`
`flipBack` | 响应 `choose` 中的 `setTimeout` 调用被调用 | `drawBack`
`drawBack` | 在 `makeDeck` 和 `flipBack` 中作为卡牌的 `draw` 方法被调用 | –
`Polycard` | 在 `choose` 中被调用 | –
`shuffle` | 在 `init` 中被调用 | –
`makeDeck` | 在 `init` 中被调用 | –
`Card` | 被 `makeDeck` 调用 | –
`drawPoly` | 在 `choose` 中作为 `Polygon` 的 `draw` 方法被调用 | –

## 第 5 章 记忆游戏（又称 Concentration 游戏）

表 5-2 展示了该应用程序多边形版本的完整带注释代码。在审阅时，请思考它与其他章节中描述的应用程序的相似之处。请记住，这只是为应用程序组件命名和编写程序的一种方式，其他方式同样可行。

***表 5-2.** 多边形版本记忆游戏的完整代码*

**代码** | **说明**
---|---
`<html>` | 起始 `html` 标签。
`<head>` | 起始 `head` 标签。
`<title>Memory game using polygons</title>` | 完整的 `title` 元素。
`<style>` | 起始 `style` 标签。
`form {` | 为表单指定样式。
`width:330px;` | 设置宽度。
`margin:20px;` | 设置外边距。
`background-color:pink;` | 设置颜色。
`Padding:20px;` | 设置内边距。
`}` | 关闭样式。
`input {` | 设置输入字段的样式。
`text-align:right;` | 设置右对齐——适用于数字。
`}` | 关闭样式。
`</style>` | 关闭 `style` 元素。
`<script type="text/javascript">` | 开始 `script` 元素。`type` 属性并非必需，但此处保留，因为你会看到它。
`var ctx;` | 存储画布上下文的变量。
`var firstPick = true;` | 声明并初始化 `firstPick`。
`var firstCard;` | 声明变量，用于存储定义第一次选牌的信息。
`var secondCard;` | 声明变量，用于存储定义第二次选牌的信息。
`var frontbgcolor = "rgb(251,215,73)";` | 设置牌面背景颜色的值。
`var polyColor = "rgb(254,11,0)";` | 设置多边形颜色的值。
`var backColor = "rgb(128,0,128)";` | 设置牌背颜色的值。
`var tableColor = "rgb(255,255,255)";` | 设置面板（桌面）颜色的值。
`var cardRad = 30;` | 设置多边形的半径。
`var deck = [];` | 声明牌组，初始为空数组。
`var firstsx = 30;` | 设置第一张牌的 `x` 坐标。
`var firstsy = 50;` | 设置第一张牌的 `y` 坐标。
`var margin = 30;` | 设置牌之间的间距。
`var cardWidth = 4*cardRad;` | 设置牌宽度为多边形半径的四倍。
`var cardHeight = 4*cardRad;` | 设置牌高度为多边形半径的四倍。
`var matched;` | 该变量在 `choose` 中设置，在 `flipBack` 中使用。
`var startTime;` | 该变量在 `init` 中设置，用于计算已过时间。
`function Card(sx,sy,sWidth,sHeight,info) {` | `Card` 函数的头部，用于设置卡牌对象。
`this.sx = sx;` | 设置水平坐标。
`this.sy = sy;` | 设置垂直坐标。
`this.sWidth = sWidth;` | 设置宽度。
`this.sHeight = sHeight;` | 设置高度。
`this.info = info;` | 设置信息（边数）。
`this.draw = drawBack;` | 指定绘制方式。
`}` | 关闭函数。
`function makeDeck() {` | 用于设置牌组的函数头部。
`var i;` | 用于 `for` 循环。
`var aCard;` | 用于存储一对牌中的第一张牌。
`var bCard;` | 用于存储一对牌中的第二张牌。