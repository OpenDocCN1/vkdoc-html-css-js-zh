# 表 7-1：完整的日期应用程序

表 7-1 展示了完整的日期应用程序。请记住，你可能需要将其上传到服务器才能进行测试。

## 表 7-1. 日期应用程序的完整代码

| 代码 | 说明 |
|------|------|
| `<html>` | 开始 `html` 标签。 |
| `<head>` | 开始 `head` 标签。 |
| `<title>Local Storage test</title>` | 完整的标题。 |
| `<script>` | 开始脚本。 |
| `function store() {` | `store` 函数头。 |
| `if (typeof(localStorage) ==` | 检查 `localStorage` 是否被识别。 |
| `"undefined") {` | |
| `alert("Browser does not recognize HTML Display alert message.");` | |
| `local storage.");` | |
| `}` | 关闭 `if` 子句。 |
| `else {` | `else` 分支。 |
| `try {` | 设置 `try` 子句。 |
| `oldDate = new Date();` | 定义一个新的 `Date` 对象。 |
| (*续*) | |
| `localStorage.` | 使用键存储在本地存储中 |
| `setItem("lastdate",oldDate);` | `"lastdate"`。 |
| `alert("Stored: "+oldDate);` | 显示消息，说明已存储的内容。 |
| `}` | 关闭 `try` 子句。 |
| `catch(e) {` | 开始 `catch` 子句：如果出现问题。 |
| `alert("Error with use of local` | 显示一条消息。 |
| `storage: "+e);}` | |
| `}` | 关闭 `try` 子句。 |
| `return false;` | `return false` 以防止任何页面刷新。 |
| `}` | 关闭函数。 |
| `function remove() {` | `remove` 函数头。 |
| `if (typeof(localStorage) ==` | 检查 `localStorage` 是否被识别。 |
| `"undefined") {` | |
| `alert("Browser does not recognize` | 显示警告消息。 |
| `HTML local storage.");` | |
| `}` | 关闭 `if` 子句。 |
| `else {` | `else` 分支。 |
| `localStorage.removeItem('lastdate');` | 移除使用键 `'lastdate'` 存储的项。 |
| `alert("Removed date stored.");` | 显示指示已执行操作的消息。 |
| `}` | 关闭子句。 |
| `return false;` | `return false` 以防止页面刷新。 |
| `}` | 关闭函数。 |
| (*续*) | |
| `function fetch() {` | 读取函数头。 |
| `if (typeof(localStorage) ==` | 检查 `localStorage` 是否被识别。 |
| `"undefined") {` | |
| `alert("Browser does not recognize` | 显示一条警告消息。 |
| `HTML local storage.");` | |
| `}` | 关闭 `if` 子句。 |
| `else {` | `else` 分支。 |
| `alert("Stored "+localStorage.` | 读取存储在键 |
| `getItem('lastdate'));` | `'lastdate'` 下的项并显示它。 |
| `}` | 关闭子句。 |
| `return false;` | `return false` 以防止页面刷新。 |
| `}` | 关闭函数。 |
| `</script>` | 关闭 `script` 元素。 |
| `</head>` | 关闭 `head` 元素。 |
| `<body>` | 开始 `body` 标签。 |
| `<button onClick="javascript:store();">` | 用于存储的按钮。 |
| `Store date info </button>` | |
| `<button onClick="javascript:fetch();">` | 用于检索（即读取存储数据）的按钮。 |
| `Retrieve date info </button>` | |
| `<button onClick="javascript:remove();">` | 用于移除的按钮。 |
| `Remove date info </button>` | |
| `</body>` | 关闭 `body` 标签。 |
| `</html>` | 关闭 `html` 标签。 |

将 `Date` 函数与 `localStorage` 结合，可以让你实现许多功能。例如，你可以计算玩家当前使用应用程序与上次使用之间的经过时间，或者玩家连续赢得两场比赛之间的时间。在第五章中，我们使用 `Date` 和 `getTime` 方法计算了经过的时间。回想一下，`getTime` 存储的是自 1970 年 1 月 1 日以来的毫秒数。你可以将该值转换为字符串并进行存储，然后在读取回来时，进行算术运算以计算经过的时间。

与可以设置持续时间的 JavaScript Cookie 不同，`localStorage` 的键值对会一直存在，直到它们被移除。

### 为本地存储编码数据

为简单起见，第一个应用程序仅包含一个 HTML 文档。你可以使用这个版本来创建迷宫、存储和检索迷宫，并让标记在迷宫中移动。该应用程序的第二个版本涉及两个 HTML 文档。第一个脚本与第一个应用程序相同，可用于构建、遍历和保存迷宫，以及在每个迷宫中穿行。第二个脚本仅用于遍历固定列表中的某个已保存迷宫。一组单选按钮允许玩家选择简单、中等或困难选项，前提是有人已经创建并保存了名为 `easymaze`、`moderatemaze` 和 `hardmaze` 的迷宫。你可以将这些名称更改为你想要的任何名称，并且/或者添加任意多个名称。你只需要确保在构建程序中创建、命名和保存的内容与在游览程序中引用的内容保持一致即可。

现在，让我们解决 `localStorage` 仅存储字符串的问题。这里描述的应用程序必须存储足够的关于墙壁的信息，以便这些墙壁可以添加到画布上。在单文档版本中，旧墙壁实际上被添加到画布上已有的任何内容之上。双文档版本会擦除任何旧的迷宫，并加载请求的迷宫。我使用了两个表单，每个表单都有一个用于输入名称的字段和一个提交按钮。玩家为保存迷宫选择一个名称，并且必须记住它以便检索。

要存储的数据是一个字符串，即一段文本。我们将通过为每堵墙执行以下操作，来创建包含一组墙壁信息的文本：

- 将 `sx`、`sy`、`fx`、`fy` 组合成一个名为 `w` 的数组，用于表示单堵墙。
- 使用 `join` 方法，利用 `w` 数组生成一个由 `+` 号分隔的字符串。
- 将每个这样的字符串添加到一个名为 `allw` 的数组中，用于表示所有墙壁。
- 再次使用 `join` 方法，利用 `allw` 数组生成一个名为 `sw` 的字符串。

`sw` 字符串变量将保存所有墙壁的所有坐标（每堵墙四个数字）。下一步是使用 `localStorage.setItem` 方法，将 `sw` 存储在玩家提供的名称下。我们使用上一节中解释过的 `try` 和 `catch` 结构来执行此操作。

```
try {
   localStorage.setItem(lsname,sw);
}
catch (e) {
   alert("data not saved, error given: "+e);
}
```

这是一种通用技术，它会尝试某件事，抑制任何错误消息，如果出现错误，则会调用 `catch` 块中的代码。

**注意** 这并不总是按你的意图工作。例如，当直接在计算机上通过 Firefox 执行此应用程序时（而不是从服务器下载的文件），`localStorage` 语句不会引发错误，但不会存储任何内容。当 HTML 文件通过 Firefox 从服务器下载时，此代码可以工作；并且创建脚本在作为本地文件以及通过 Chrome 下载时都能工作。双脚本版本必须在服务器上针对每个浏览器进行测试。

检索信息的工作方式类似。代码提取玩家提供的名称来设置变量 `lsname`，然后使用

```
swalls = localStorage.getItem(lsname);
```

# 第 7 章 迷宫

来设置变量 `swalls`。如果这不是 `null`，我们使用字符串方法 `split` 来执行与 `join` 相反的操作：在给定的符号处拆分字符串（我们在每个分号处拆分），并将值分配给数组的连续元素。相关的代码行是

```
wallstgs = swalls.split(";");
```

和

```
sw = wallstgs[i].split("+");
```

接下来，代码使用刚刚检索到的信息以及固定的墙壁宽度和墙壁样式信息，来创建一个新的 `Wall` 对象：

```
curWall = new Wall(sx,sy,fx,fy,wallWidth,wallStyle);
```

最后，还有将 `curWall` 添加到 `everything` 数组和 `walls` 数组的代码。

## 单选按钮

单选按钮是一组按钮，在该组中只能选择一个成员。如果玩家做出新选择，旧选择将被取消选中。对于此应用程序的困难/中等/简单选择来说，它们是合适的选择。以下是 `<body>` 部分中的 HTML 标记：

```
<form name="gf" onSubmit="return getWalls()" >
   <br/>
   <input type="radio" value="hard" name="level" />困难 <br/>
   <input type="radio" value="moderate" name="level" />中等 <br/>
   <input type="radio" value="easy" name="level" />简单<br/>
   <input type="submit" value="获取迷宫"/><br/>
</form>
```

注意所有三个输入元素都具有相同的 `name` 属性。这是定义一个单选按钮组的关键，该组中一次只能选中一个按钮。在此例中，标记创建了一个名为 `level` 的数组。`getWalls` 函数将在下一节中完整展示。它类似于一体化脚本中的函数。但在此例中，`localStorage` 项的名称由单选按钮决定。代码如下：

```
for (i=0; i<document.gf.level.length; i++) {
  if (document.gf.level[i].checked) {
    lsname = document.gf.level[i].value + "maze";
    break;
  }
}
```

`for` 循环遍历所有输入项。`if` 测试基于 `checked` 属性。当检测到 `true` 条件时，变量 `lsname` 由该项的 `value` 属性构成，并且 `break;` 语句将使执行流程退出 `for` 循环。如果你希望单选按钮在初始状态下有一项被选中，可以使用如下代码：

```
<input type="radio" value="easy" name="level" checked />
```

或这样：

```
<input type="radio" value="easy" name="level" checked="true" />
```

### 构建应用并使其成为你自己的作品

现在让我们来看看迷宫应用的编码，首先是一体化脚本，然后是双脚本版本中的第二个脚本。

表 7-2 展示了用于创建、保存、检索和穿梭迷宫的脚本中的函数。请注意，许多函数的调用是通过事件处理完成的：`onLoad`、`onSubmit` 和 `addEventListener` 调用。这些调用并非直接或立即执行函数，而是设置在指定事件发生时进行调用。

**表 7-2.** 迷宫应用中的函数

| **函数** | **调用方/被谁调用** | **调用哪些函数或方法** |
| --- | --- | --- |
| `init` | 由 `body` 标签中的 `onLoad` 动作调用 | `drawAll` |
| `drawAll` | `init`, `startWall`, `stretchWall`, `getkeyAndMove`, `getWalls` | `Wall` 和 `token` 的 `draw` 方法：`drawToken` 和 `drawAline` |
| `Token` | `var` 语句声明 `mypent` | `Wall` |
| `startWall`, `getWalls` | `drawAll` | 使用 `everything` 数组中 `token` 对象的 `draw` 方法 |
| `drawToken` | `drawAll` | 使用 `myPent` 的 `moveit` 方法 |
| `moveToken` | `getkeyAndMove` | `drawAline` |
| `intersect` | `drawAll` | 使用 `everything` 数组中 `Wall` 对象的 `draw` 方法 |
| `startWall` | 由 `init` 中的 `addEventListener` 调用触发 | `drawAll`, `Wall` |
| `stretchWall` | 由 `init` 中的 `addEventListener` 调用触发 | `drawAll` |
| `finish` | 由 `init` 中的 `addEventListener` 调用触发 | |
| `getkeyAndMove` | 由 `init` 中的 `addEventListener` 调用触发 | `moveToken`，使用 `myPent` 的 `moveit` 方法 |
| `saveWalls` | 由 `sf` 表单的 `onSubmit` 调用 | |
| `getWalls` | 由 `gf` 表单的 `onSubmit` 调用 | `drawAll`, `Wall` |

表 7-3 展示了迷宫应用的完整代码及解释。

**表 7-3.** 一体化迷宫应用的完整代码

| **代码** | **解释** |
| --- | --- |
| `<html>` | 开头的 `html` 标签。 |
| `<head>` | 开头的 `head` 标签。 |
| `<title>Build maze & travel maze</title>` | 完整的 `title` 元素。 |
| `<script type="text/javascript">` | 开头的 `script` 标签。 |
| `var cwidth = 900;` | 用于清除画布。 |
| `var cheight = 350;` | 用于清除画布。 |
| `var ctx;` | 用于保存画布上下文。 |
| `var everything = [];` | 用于保存所有对象。 |
| `var curWall;` | 用于表示正在构建的墙壁。 |
| `var wallWidth = 5;` | 固定的墙壁宽度。 |
| `var wallStyle = "rgb(200,0,200)";` | 固定的墙壁颜色。 |
| `var walls = [];` | 保存所有墙壁。 |
| `var inMotion = false;` | 当通过拖拽构建墙壁时，该标志为真。 |
| `var unit = 10;` | 令牌的移动单位。 |
| `function Token(sx, sy, rad, styleString, n) {` | 构建令牌的函数头。 |
| `this.sx = sx;` | 设置 `sx` 属性。 |
| `this.sy = sy;` | 设置 `sy` 属性。 |
| `this.rad = rad;` | 设置 `rad` 属性（半径）。 |
| `this.draw = drawToken;` | 设置 `draw` 方法。 |
| `this.n = n;` | 设置 `n` 边数。 |
| `this.angle = (2 * Math.PI) / n;` | 计算并设置角度。 |
| （*续*） | |
| `this.moveit = moveToken;` | 设置 `moveit` 方法。 |
| `this.fillstyle = styleString;` | 设置颜色。 |
| `}` | 结束函数。 |
| `function drawToken() {` | `drawToken` 函数头。 |
| `ctx.fillStyle = this.fillstyle;` | 设置颜色。 |
| `var i;` | 索引。 |
| `var rad = this.rad;` | 设置 `rad`。 |
| `ctx.beginPath();` | 开始路径。 |
| `ctx.moveTo(this.sx+rad*Math.cos(-.5*this.angle),this.sy+rad*Math.sin(-.5*this.angle));` | 移动到令牌多边形（五边形）的第一个顶点。 |
| `for (i=1;i<this.n;i++) {` | |

for 循环绘制令牌的`n`条边：此处为五边形。

```
ctx.lineTo(this.sx+rad*Math.cos((i-.5)*this.angle),this.sy+rad*Math.sin((i-.5)*this.angle));
```

指定到下一个顶点的线条，设置五边形一条边的绘制。

```
}
```

关闭 for 循环。

```
ctx.fill();
```

绘制令牌。

```
}
```

关闭函数。

```
function moveToken(dx,dy) {
```

函数头。

```
this.sx += dx;
```

增加 x 值。

```
this.sy += dy;
```

增加 y 值。

```
var i;
```

索引。

```
var wall;
```

用于每个墙。

```
for(i=0;i<walls.length;i++) {
```

循环遍历所有墙。

```
wall = walls[i];
```

提取第 i 个墙。

```
if (intersect(wall.sx,wall.sy,wall.fx,wall.fy,this.sx,this.sy,this.rad)) {
```

检查相交。如果令牌的新位置与该特定墙之间存在交集。

```
this.sx -= dx;
```

将 x 改回——不执行本次移动。

```
this.sy -= dy;
```

将 y 改回——不执行本次移动。

```
break;
```

离开 for 循环，因为如果与一面墙发生碰撞，则无需进行更多检查。

```
}
```

关闭 if true 子句。

```
}
```

关闭 for 循环。

```
}
```

关闭函数。

```
function Wall(sx,sy,fx,fy,width,styleString) {
```

创建`Wall`的函数头。

```
this.sx = sx;
```

设置`sx`属性。

```
this.sy = sy;
```

设置`sy`。

```
this.fx = fx;
```

设置`fx`。

```
this.fy = fy;
```

设置`fy`。

```
this.width = width;
```

设置`width`。

```
this.draw = drawAline;
```

设置`draw`方法。

```
this.strokestyle = styleString;
```

设置`strokestyle`。

```
}
```

关闭函数。

```
function drawAline() {
```

`drawAline`的函数头。

```
ctx.lineWidth = this.width;
```

设置线宽。

```
ctx.strokeStyle = this.strokestyle;
```

设置`strokestyle`。

```
ctx.beginPath();
```

开始路径。

```
ctx.moveTo(this.sx,this.sy);
```

移动到线条起点。

```
ctx.lineTo(this.fx,this.fy);
```

设置线条到终点。

```
ctx.stroke();
```

绘制线条。

```
}
```

关闭函数。

```
var mypent = new Token(100,100,20,"rgb(0,0,250)",5);
```

将`mypent`设置为五边形形状作为游戏棋子。

```
everything.push(mypent);
```

添加到`everything`。

```
function init(){
```

`init`的函数头。

```
ctx = document.getElementById('canvas').getContext('2d');
```

为所有绘制定义`ctx`（上下文）。

```
canvas1 = document.getElementById('canvas');
```

定义`canvas1`，用于事件。

```
canvas1.addEventListener('mousedown',startWall,false);
```

设置`mousedown`的处理。

```
canvas1.addEventListener('mousemove',stretchWall,false);
```

设置`mousemove`的处理。

```
canvas1.addEventListener('mouseup',finish,false);
```

设置`mouseup`的处理。

```
window.addEventListener('keydown', getkeyAndMove,false);
```

设置箭头键使用的处理。

```
drawAll();
```

绘制所有内容。

```
}
```

关闭函数。

```
function startWall(ev) {
```

`startWall`的函数头。

```
var mx;
```

保存鼠标 x 坐标。

```
var my;
```

保存鼠标 y 坐标。

```
mx = ev.pageX;
```

设置`mx`。

```
my = ev.pageY;
```

设置`my`。

```
curWall = new Wall(mx,my,mx+1,my+1,wallWidth,wallStyle);
```

创建一个新的墙。此时它很小。

```
inMotion = true;
```

将`inMotion`设置为`true`。

```
everything.push(curWall);
```

将`curWall`添加到`everything`。

```
drawAll();
```

绘制所有内容。

```
}
```

关闭函数。

```
function stretchWall(ev) {
```

`stretchWall`的函数头，用于在鼠标拖拽时拉伸墙。

```
if (inMotion) {
```

检查是否处于运动状态。

```
var mx;
```

保存鼠标 x 坐标。

```
var my;
```

保存鼠标 y 坐标。

```
mx = ev.pageX;
```

设置`mx`。

```
my = ev.pageY;
```

设置`my`。

```
curWall.fx = mx;
```

将`curWall.fx`更改为`mx`。

```
curWall.fy = my;
```

将`curWall.fy`更改为`my`。

```
drawAll();
```

绘制所有内容（将显示正在增长的墙）。

```
}
```

关闭`if inMotion`。

```
}
```

关闭函数。

```
function finish(ev) {
```

`finish`的函数头。

```
inMotion = false;
```

将`inMotion`设置为`false`。

```
walls.push(curWall);
```

将`curWall`添加到`walls`。

```
}
```

关闭函数。

```
function drawAll() {
```

`drawAll`的函数头。

```
ctx.clearRect(0,0,cwidth,cheight);
```

擦除整个画布。

```
var i;
```

索引。

```
for (i=0;i<everything.length;i++) {
```

循环遍历`everything`。