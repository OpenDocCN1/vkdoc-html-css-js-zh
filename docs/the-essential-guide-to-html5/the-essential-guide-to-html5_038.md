# 第四章 炮弹与弹弓

**构建应用，让其成为你的专属之作**

以下代码提供了检测鼠标光标是否位于球体上方的相同测试方法。对于另一项任务，我决定使用距离的平方来计算初始速度是可以接受的。我尝试了一些数值，正如我之前提到的，700 这个值似乎效果不错。

现在，让我们来看看基于水平初速度和垂直初速度发射炮弹（无炮管）的基础代码；基于炮管角度和初速度发射炮弹的代码；以及基于鼠标位置确定角度和初速度的弹弓代码。与之前章节类似，我将为每个应用展示其函数以及它们调用或被调用的关系。在本例中，三个应用的表格虽然不完全相同，但结构相似。与之前的示例相比，调用的方式更加多样化，有些情况下函数被调用是因为它们被命名为程序员定义对象的方法，或是作为`var`声明语句的一部分。这是**面向对象、事件驱动编程**的一个特点。我还会将每个应用的完整代码放在其对应的表格中，并附上每行代码的说明。表 4-1 展示了基础炮弹应用的函数。

***表 4-1：最简炮弹应用中的函数***

| **函数** | **被谁调用/由谁调用** | **调用谁** |
|---|---|---|
| `init` | body 标签中 `onLoad` 的动作 | `drawall` |
| `drawall` | 由 `init`、`fire`、`change` 直接调用 | 调用 `everything` 数组中所有对象的 `draw` 方法；这些对象对应于函数 `drawBall`、`drawRects` |
| `fire` | 表单中 `onSubmit` 属性的动作触发 | `drawAll` |
| `change` | 由 `fire` 中调用的 `setInterval` 函数触发 | `drawall`，调用 `cBall` 的 `moveit` 方法，即 `moveBall` |
| `Ball` | 由 `var` 语句中的代码直接调用 | |
| `MyRectangle` | 由 `var` 语句中的代码直接调用 | |
| `drawBall` | 通过调用唯一 `Ball` 对象的 `draw` 方法触发 | |
| `drawRects` | 通过调用 `target` 对象的 `draw` 方法触发 | |
| `moveBall` | 通过调用唯一 `Ball` 对象的 `moveit` 方法触发 | |

## 第四章 炮弹与弹弓

表 4-2 展示了最简应用的完整代码，其中球体沿弧线运动，且没有实际的炮管。

***表 4-2：第一个炮弹应用***

| **代码** | **说明** |
|---|---|
| `<html>` | 开始 html 标签。 |
| `<head>` | 开始 head 标签。 |
| `<title>Cannonball</title>` | 完整的 title 元素。 |
| `<style>` | 开始 style 标签。 |
| `form {` | 表单的样式。 |
| `width:330px;` | 宽度。 |
| `margin:20px;` | 外边距。 |
| `background-color:brown;` | 设置表单的背景颜色。 |
| `padding:20px;` | 内边距。 |
| `}` | 关闭此样式。 |
| `</style>` | 关闭 style 元素。 |
| `<script>` | 开始 script 标签。 |
| `var cwidth = 600;` | 设置画布的宽度值；用于清除操作。 |
| `var cheight = 400;` | 设置画布的高度值；用于清除操作。 |
| `var ctx;` | 用于存储画布上下文的变量。 |
| `var everything = [];` | 用于存储所有待绘制对象的数组。初始化为空数组。 |
| `var tid;` | 用于存储定时事件标识符的变量。 |
| `var horVelocity;` | 用于存储水平速度（即水平位移）的变量。 |
| `var verticalVel1;` | 用于存储间隔开始时垂直位移的变量。 |
| `var verticalVel2;` | 用于存储间隔结束时，受重力影响后垂直位移的变量。 |
| `var gravity = 2;` | 垂直位移的变化量。该值为随意设定，能形成一条不错的弧线。 |
| `var iballx = 20;` | 球的初始水平坐标。 |
| `var ibally = 300;` | 球的初始垂直坐标。 |
| `function Ball(sx,sy,rad, styleString) {` | 定义 `Ball` 对象的函数开始。使用参数设置属性。 |
| `this.sx = sx;` | 设置 `this` 对象的 `sx` 属性。 |
| `this.sy = sy;` | 设置 `this` 对象的 `sy` 属性。 |
| `this.rad = rad;` | 设置 `this` 对象的 `rad` 属性。 |
| `this.draw = drawBall;` | |



好的，这是按照您的要求排版后的 Markdown 文档。

---

设置 `this` 对象的 `draw` 属性。由于 `drawball` 是一个函数名，这使得 `draw` 成为一个可以被调用的方法。

```
this.moveit = moveBall;
```

将 `moveit` 属性设置为 `moveBall` 函数。

```
this.fillStyle = styleString;
```

将 `fillStyle` 设置为 `styleString` 的值。

```
}
```

关闭 `Ball` 函数。

```
function drawBall() {
```

`drawBall` 函数的头部。

```
ctx.fillStyle=this.fillstyle;
```

使用 `this` 对象的 `fillStyle` 属性来设置 `fillStyle`。

```
ctx.beginPath();
```

开始一条路径。

```
ctx.arc(this.sx,this.sy,this.rad,0,Math.PI*2,true);
```

设置绘制一个圆形。

```
ctx.fill();
```

将路径绘制为填充路径。

```
}
```

关闭函数。

```
function moveBall(dx,dy) {
```

`moveBall` 函数的头部。

```
this.sx +=dx;
```

将 `sx` 属性增加 `dx`。

```
this.sy +=dy;
```

将 `sy` 属性增加 `dy`。

```
}
```

关闭函数。

```
var cball = new Ball(iballx,ibally,10,"rgb(250,0,0)");
```

在指定的位置、半径和颜色下创建一个新的 `Ball` 对象。将其赋值给变量 `cball`。注意，此时并没有绘制任何东西，这些信息只是为后续使用而设置。

```
function myRectangle(sx,sy,swidth,sheight,stylestring) {
```

用于构造 `myRectangle` 对象的函数头部。

```
this.sx = sx;
```

设置 `this` 对象的 `sx` 属性。

```
this.sy = sy;
```

设置 `sy` 属性。

```
this.swidth = swidth;
```

设置 `swidth` 属性。

```
this.sheight = sheight;
```

设置 `sheight` 属性。

```
this.fillstyle = styleString;
```

设置 `fillStyle` 属性（原文笔误，应为 `fillstyle`）。

```
this.draw = drawRects;
```

设置 `draw` 属性。这创建了一个可以被调用的方法。

```
this.moveit = moveBall;
```

设置 `moveit` 属性。这创建了一个可以被调用的方法。该程序未使用它。

```
}
```

关闭 `myRectangle` 函数。

```
function drawRects() {
```

`drawRects` 函数的头部。

```
ctx.fillStyle = this.fillStyle;
```

设置 `fillStyle`。

```
ctx.fillRect(this.sx,this.sy,this.swidth,this.sheight);
```

使用对象的属性绘制矩形。

```
}
```

关闭函数。

```
var target = new myRectangle(300,100,80,200,"rgb(0,5,90)");
```

构建一个 `myRectangle` 对象并将其赋值给 `target`。

```
var ground = new myRectangle(0,300,600,30,"rgb(10,250,0)");
```

构建一个 `myRectangle` 对象并将其赋值给 `ground`。

```
everything.push(target);
```

这些语句位于任何函数之外，但可以正常工作。将 `target` 添加到 `everything` 数组中。

```
everything.push(ground);
```

添加 `ground`。

```
everything.push(cball);
```

添加 `cball`（它将被最后绘制，因此位于所有其他对象之上）。

```
function init(){
```

`init` 函数的头部。

```
ctx = document.getElementById('canvas').getContext('2d');
```

设置 `ctx` 以在画布上绘制。

```
drawall();
```

绘制所有内容。

```
}
```

关闭 `init` 函数。

```
function fire() {
```

`fire` 函数的头部。

```
cball.sx = iballx;
```

重置 `cball` 的 x 坐标。

```
cball.sy = ibally;
```

重置 `cball` 的 y 坐标。

```
horvelocity = Number(document.f.hv.value);
```

从表单中设置水平速度，并将其转换为数字。

```
verticalvel1 = Number(document.f.vv.value);
```

从表单中设置初始垂直速度。

```
drawall();
```

绘制所有内容。

```
tid = setInterval(change,100);
```

启动定时事件。

```
return false;
```

返回 `false` 以阻止 HTML 页面刷新。

```
}
```

关闭函数。

```
function drawall() {
```

`drawall` 函数的头部。

```
ctx.clearRect(0,0,cwidth,cheight);
```

擦除画布。

```
var i;
```

为 `for` 循环声明变量 `i`。

```
for (i=0;i<everything.length;i++) { everything[i].draw();}
```

对于 `everything` 数组中的每一项，调用该对象的 `draw` 方法。关闭 `for` 循环。

```
}
```

关闭函数。

```
function change() {
```

`change` 函数的头部。

```
var dx = horvelocity;
```

将 `dx` 设置为 `horvelocity`。

```
verticalvel2 = verticalvel1 + gravity;
```

计算新的垂直速度（增加重力）。

```
var dy = (verticalvel1 + verticalvel2)*.5;
```

计算时间间隔内的平均速度。

```
verticalvel1 = verticalvel2;
```

将旧值设置为新值。

```
cball.moveit(dx,dy);
```

移动 `cball` 指定的量。

```
var bx = cball.sx;
```

设置 `bx` 以简化 `if` 语句。

```
var by = cball.sy;
```

设置 `by`。

```
if ((bx>=target.
```

球是否在目标的水平范围内……



