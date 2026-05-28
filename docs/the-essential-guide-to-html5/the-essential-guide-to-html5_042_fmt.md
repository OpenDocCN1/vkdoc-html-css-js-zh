# 应用构建于更简单的基础之上，这将帮助你创建自己的作品。

创建弹弓应用涉及设计弹弓、实现鼠标事件来移动球和弹弓部件，然后发射球。由于玩家的动作只是鼠标操作，因此没有表单。此外，我在处理目标被击中时采用了稍微不同的方法。我检查球是否与目标内 40 像素的区域相交。也就是说，我要求球击中鸡的中间！当击中时，我将`target.src`值更改为另一个`Image`元素，从鸡的图片变为羽毛的图片。而且，我没有停止动画，因此球只在击中地面时才会停止。正如我之前提到的，我没有让弹弓臂恢复到原始位置，因为我想观察位置以计划下一次尝试。

表 4-5 显示了在弹弓应用中调用和被调用的函数。此表与炮弹应用的表格非常相似。

## 第 4 章 炮弹与弹弓

**表 4-5.** 弹弓应用中的函数

| 函数 | 被谁调用/调用者 | 调用谁 |
| :--- | :--- | :--- |
| `init` | `body`标签中`onLoad`的动作 | `drawall` |
| `drawall` | 由`init`直接调用 | 调用`everything`数组中所有对象的`draw`方法；这些是函数`drawBall`、`drawRects`、`drawSling`和`drawAnImage` |
| `findball` | 由`drawall`和`distsq`中`addEventListener`的动作调用 | `init`用于`mousedown`事件 |
| `distsq` | 由`findBall`调用 | |
| `moveit` | 由`drawAll`中`addEventListener`的动作调用 | `init`用于`mouseMove`事件 |
| `finish` | 由`drawAll`和`distsq`中`addEventListener`的动作调用 | `init`用于`mouseup`事件 |
| `change` | 由`setInterval`的动作调用 | `drawAll`，调用`cball`的方法`moveit` |
| 在`finish`中调用的函数 | | `cball`的`moveBall` |
| `Ball` | 由`var`语句中的代码直接调用 | |
| `Myrectangle` | 由`var`语句中的代码直接调用 | |
| `drawball` | 通过调用一个`Ball`对象的`draw`方法来调用 | |
| `drawrects` | 通过调用目标对象的`draw`方法来调用 | |
| `moveball` | 通过调用一个`Ball`对象的`moveit`方法来调用 | |
| `Picture` | 由`var`语句中的代码直接调用 | |
| `drawAnImage` | 通过调用一个`Picture`对象的`draw`方法来调用 | |
| `Sling` | 由`var`语句中的代码直接调用 | |
| `drawsling` | 通过调用`mysling`的`draw`方法来调用 | |

表 4-6 显示了弹弓应用的代码，其中大多数行都带有注释，即使是早期程序中相同的行也是如此。请注意，`body`元素中没有表单。在查看代码之前，请尝试确定哪些部分与炮弹应用相同，哪些部分会不同。

**表 4-6.** 弹弓应用

| 代码 | 解释 |
| :--- | :--- |
| `<html>` | |
| `<head>` | |
| `<title>Slingshot pulling back</title>` | |
| `<script type="text/javascript">` | |
| `var cwidth = 1200;` | |
| `var cheight = 600;` | |
| `var ctx;` | |
| `var canvas1;` | |
| `var everything = [];` | |
| `var tid;` | |
| `var startrockx = 100;` | 起始位置 x。 |
| `var startrocky = 240;` | 起始位置 y。 |
| `var ballx = startrockx;` | 设置`ballx`。 |
| `var bally = startrocky;` | 设置`bally`。 |
| `var ballrad = 10;` | |
| `var ballradsq = ballrad*ballrad;` | 保存此值。 |
| `var inmotion = false;` | 用于检查石头是否在移动的标志变量。 |
| `var horvelocity;` | 用于水平速度。 |
| `var verticalvel1;` | 用于一个时间间隔开始时的垂直速度。 |
| ( *续*) | |

**表 4-6.** ( *续*)

| 代码 | 解释 |
| :--- | :--- |
| `var verticalvel2;` | 用于一个时间间隔结束时的垂直速度。 |
| `var gravity = 2;` | 重力值。请参见我的评论。 |
| `var chicken = new Image();` | 原始目标的名称。 |
| `chicken.src = "chicken.jpg";` | 设置图像文件。 |
| `var feathers = new Image();` | 被击中目标的名称。 |
| `feathers.src = "feathers.gif";` | 设置图像文件。 |
| `function Sling(bx,by,s1x,s1y,s2x,s2y,` | 基于四个点加一种颜色定义弹弓的函数。 |
| `s3x,s3y,stylestring) {` | |
| `this.bx = bx;` | 设置`bx`属性。 |
| `this.by = by;` | ...`by`。 |
| `this.s1x = s1x;` | |
```javascript
this.s1x = s1x;
this.s1y = s1y;
this.s2x = s2x;
this.s2y = s2y;
this.s3x = s3x;
this.s3y = s3y;
this.strokeStyle = stylestring;
this.draw = drawsling;
this.moveit = movesling;
```
设置`draw`方法，设置`move`方法（未使用）。
```javascript
}
```
关闭函数。
```javascript
function drawSling() {
```
函数`drawSling()`头。
```javascript
ctx.strokeStyle = this.strokeStyle;
```
设置样式。
```javascript
ctx.lineWidth = 4;
```
设置线宽。
```javascript
ctx.beginPath();
```
开始路径。
（续）

**第 4 章 炮弹与弹弓**

**表 4-6.**（续）

| 代码 | 说明 |
|------|------|
| `ctx.moveTo(this.bx,this.by);` | 移动到`bx,by` |
| `ctx.lineTo(this.s1x,this.s1y);` | 设置绘制到`s1x,s1y` |
| `ctx.moveTo(this.bx,this.by);` | 移动到`bx,by` |
| `ctx.lineTo(this.s2x,this.s2y);` | 设置绘制到`s2x,s2y` |
| `ctx.moveTo(this.s1x,this.s1y);` | 移动到`s1x,s1y` |
| `ctx.lineTo(this.s2x,this.s2y);` | 设置绘制到`s2x,s2y` |
| `ctx.lineTo(this.s3x,this.s3y);` | 绘制到`s3x,s3y` |
| `ctx.stroke();` | 现在绘制路径 |
```javascript
}
```
关闭函数。
```javascript
function moveSling(dx,dy) {
```
函数`moveSling()`头。
```javascript
this.bx += dx;
```
将`dx`加到`bx`。
```javascript
this.by += dy;
```
将`dy`加到`by`。
```javascript
this.s1x += dx;
```
将`dx`加到`s1x`。
```javascript
this.s1y += dy;
```
将`dy`加到`s1y`。
```javascript
this.s2x += dx;
```
将`dx`加到`s2x`。
```javascript
this.s2y += dy;
```

## 第 4 章 炮弹与弹弓

### 表 4-6.（续）

将`dy`加到`s2y`。

```javascript
this.s3x += dx;
```

将`dx`加到`s3x`。

```javascript
this.s3y += dy;
```

将`dy`加到`s3y`。

```javascript
}
```

关闭函数。

```javascript
var mySling = new Sling(startrockx,startrocky,
                        startrockx+80,startrocky-10,
                        startrockx+80,
                        startrocky+10,
                        startrockx+70,
                        startrocky+180,
                        "rgb(120,20,10)");
```

构建新的`Sling`对象并赋值给`mySling`变量。

```javascript
function Ball(sx,sy,rad,stylestring) {
```

函数`Ball()`头。

```javascript
this.sx = sx;
```

设置属性`sx`。

```javascript
this.sy = sy;
```

设置属性`sy`。

（续）

| 代码 | 说明 |
|------|------|
| `this.rad = rad;` | 设置属性`rad` |
| `this.draw = drawball;` | 设置属性`draw` |
| `this.moveit = moveball;` | 设置属性`moveit` |
| `this.fillstyle = stylestring;` | 设置属性`fillstyle` |

```javascript
}
```

关闭`Ball`函数。

```javascript
function drawBall() {
```

函数`drawBall()`头。

```javascript
ctx.fillStyle = this.fillstyle;
```

从属性设置`fillStyle`。

```javascript
ctx.beginPath();
```

开始路径。

```javascript
ctx.arc(this.sx,this.sy,this.rad,0,Math.PI*2,true);
```

绘制圆弧。

```javascript
ctx.fill();
```

填充。

```javascript
}
```

关闭`drawBall`。

```javascript
function moveBall(dx,dy) {
```

函数`moveBall()`头，参数为位置变化量。

```javascript
this.sx += dx;
```

增加`sx`。

```javascript
this.sy += dy;
```

增加`sy`。

```javascript
}
```

关闭`moveit`。

```javascript
var cball = new Ball(startrockx,startrocky,ballrad,"rgb(250,0,0)");
```

设置`cBall`为新的`Ball`对象。

```javascript
function myRectangle(sx,sy,swidth,sheight,stylestring) {
```

函数`myRectangle()`头。

```javascript
this.sx = sx;
```

设置属性`sx`。

```javascript
this.sy = sy;
```

设置属性`sy`。

```javascript
this.swidth = swidth;
```

设置属性`swidth`。

```javascript
this.sheight = sheight;
```

设置属性`sheight`。

```javascript
this.fillstyle = stylestring;
```

设置属性`fillStyle`。

（续）

### 表 4-6.（续）

| 代码 | 说明 |
|------|------|
| `this.draw = drawrects;` | 设置属性`draw` |
| `this.moveit = moveball;` | 设置属性`moveit` |

```javascript
}
```

关闭`myRectangle`。

```javascript
function drawRects() {
```

函数`drawRects()`头。

```javascript
ctx.fillStyle = this.fillstyle;
```

从属性设置`fillStyle`。

```javascript
ctx.fillRect(this.sx,this.sy,this.swidth,this.sheight);
```

绘制矩形。

```javascript
}
```

关闭`drawRects`。

```javascript
function Picture(sx,sy,swidth,sheight,imga) {
```

函数`Picture()`头。

```javascript
this.sx = sx;
```

设置属性`sx`。

```javascript
this.sy = sy;
```

设置属性`sy`。

```javascript
this.img = imga;
```

设置属性`img`。

```javascript
this.swidth = swidth;
```

设置属性`swidth`。

```javascript
this.sheight = sheight;
```

设置属性`sheight`。

```javascript
this.draw = drawAnImage;
```

设置属性`drawAnImage`。

```javascript
this.moveit = moveball;
```

设置属性`moveit`。

```javascript
}
```

关闭`Picture`。

```javascript
function drawAnImage() {
```

函数`drawAnImage()`头。

```javascript
ctx.drawImage(this.img,this.sx,this.sy,this.swidth,this.sheight);
```

使用`drawImage`。

```javascript
}
```

关闭`drawAnImage`。

```javascript
var target = new Picture(700,210,209,179,chicken);
```

构建新的`Picture`对象并赋值给`target`。注意`chicken`是`Image`数据类型的变量。

```javascript
var ground = new myRectangle(0,370,1200,30,"rgb(10,250,0)");
```

创建作为地面的矩形。

```javascript
function init() {
```

函数`init()`头。

```javascript
ctx = document.getElementById('canvas').getContext('2d');
```

为画布上下文设置`ctx`。

```javascript
canvas1 = document.getElementById('canvas');
```

将`canvas1`设为保存画布元素的变量。

```javascript
canvas1.addEventListener('mousedown',findball,false);
```

为`mousedown`事件设置事件处理。

```javascript
canvas1.addEventListener('mousemove',
```