# 第 4 章 炮弹与弹弓

`sx)&&(bx<=(target.sx+target.swidth))&&(by>=target.sy)&&(by<=(target.sy+target.sheight))) {`

`clearInterval(tid);`

……那么垂直方向呢？

若如此，则停止运动。

`}`

关闭 if 真条件子句。

`if (by>=ground.sy) {`

球是否超出地面？

`clearInterval(tid);`

若如此，则停止运动。

（*续*）

#### 表 4-2.（*续*）

| 代码 | 说明 |
|------|------|
| `}` | 关闭 if 真条件子句。 |
| `drawAll();` | 绘制所有内容。 |
| `}` | 关闭 `change` 函数。 |
| `</script>` | 关闭 `script` 元素。 |
| `</head>` | 关闭 `head` 元素。 |
| `<body onLoad="init();">` | 打开 `body` 并设置对 `init` 的调用。 |
| `<canvas id="canvas" width="600" height="400">` | 定义 `canvas` 元素。 |
| 您的浏览器不支持 HTML5 的 canvas 元素。 | 向不兼容的浏览器用户发出警告。 |
| `</canvas>` | 关闭 `canvas` 元素。 |
| `<br/>` | 换行。 |
| `<form name="f" id="f" onSubmit="return fire();">` | 起始 `form` 标签，包含 `name` 和 `id`。设置对 `fire` 的调用。 |
| 设置速度并发射炮弹。 | 标签与换行。 |
| `<br/>` | 换行。 |
| 水平位移 `<input name="hv" id="hv" value="10" type="number" min="-100" max="100" />` | 输入字段的标签与规格。 |
| `<br>` | 换行。 |
| 初始垂直位移 `<input name="vv" id="vv" value="-25" type="number" min="-100" max="100" />` | 输入字段的标签与规格。 |
| `<input type="submit" value="发射"/>` | 提交输入元素。 |
| `</form>` | 关闭 `form` 元素。 |
| `</body>` | 关闭 `body` 元素。 |
| `</html>` | 关闭 `html` 元素。 |

---

你当然可以改进这个应用程序，但更合理的做法是首先确保你按原样理解了它，然后再继续下一步。

## 炮弹：带大炮、角度和速度

我们下一个应用程序添加了一个矩形来表示大炮，用一张图片表示原始目标（取代第一个应用程序中使用的简单矩形），以及另一张图片表示被击中的目标。大炮会根据表单输入进行旋转。我将 `everything` 数组改成了数组的数组，因为我需要一种方法来添加旋转和平移信息。我还决定让炮弹击中目标时的效果更加戏剧化。这意味着 `change` 函数中用于检测碰撞的代码是相同的，但 if 真子句中的代码会移除旧目标，放入被击中的目标，并移除球。综上所述，大部分代码是相同的。表 4-3 展示了这些函数，其中增加了 `Picture` 和 `drawAnImage` 两行。

#### 表 4-3. 第二个炮弹应用程序中的函数

| 函数 | 调用方 | 调用对象 |
|----------|----------------------|-------|
| `init` | `body` 标签中 `onLoad` 的动作 | `drawall` |
| `drawall` | 由 `init`、`fire`、`change` 直接调用 | 调用 `everything` 数组中所有对象的 `draw` 方法；这些对象是函数 `drawball` 和 `drawrects` |
| `fire` | 由表单中 `onSubmit` 属性的动作调用 | `drawall` |
| `change` | 由 `fire` 中调用的 `setInterval` 函数的动作调用 | `drawall`，调用 `cball` 的 `moveit` 方法，即 `moveBall` |
| `Ball` | 由 `var` 语句中的代码直接调用 | |
| `myRectangle` | 由 `var` 语句中的代码直接调用 | |
| `drawBall` | 通过调用 `Ball` 对象的 `draw` 方法调用 | |
| `drawRects` | 通过调用目标对象的 `draw` 方法调用 | |
| `moveBall` | 通过调用 `Ball` 对象的 `moveit` 方法调用 | |
| `Picture` | 由 `var` 语句中的代码直接调用 | |
| `drawAnImage` | 通过调用 `Picture` 对象的 `draw` 方法调用 | |

表 4-4 展示了第二个应用程序的完整代码，但仅在修改过的行添加了注释。

#### 表 4-4. 第二个炮弹应用程序

| 代码 | 说明 |
|------|------|
| `<html>` | |
| `<head>` | |
| `<title>炮弹</title>` | |
| `<style>` | |
| `form { width:330px; margin:20px; background-color:brown; padding:20px; }` | |
| `</style>` | |
| `<script type="text/javascript">` | |
| `var cwidth = 600;` | |
| `var cheight = 400;` | |
| `var ctx;` | |
| `var everything = [];` | |
| `var tid;` | |
| `var horvelocity;` | |
| `var verticalvel1;` | |
| `var verticalvel2;` | |
| `var gravity = 2;` | |
| `var cannonx = 10;` | 大炮的 x 坐标。 |
| `var cannony = 280;` | 大炮的 y 坐标。 |

#### 代码与解释

`var cannonLength = 200;`

炮管长度（即宽度）。

`var cannonht = 20;`

炮管高度。

（*续*）

## 第 4 章 炮弹与弹弓

***表 4-4.*** （*续*）

**代码**

**解释**

`var ballrad = 10;`

`var targetx = 500;`

目标的 x 坐标。

`var targety = 50;`

目标的 y 坐标。

`var targetw = 85;`

目标的宽度。

`var targeth = 280;`

目标的高度。

`var htargetx = 450;`

已击中目标的 x 坐标。

`var htargety = 220;`

已击中目标的 y 坐标。

`var htargetw = 355;`

已击中目标的宽度。

`var htargeth = 96;`

已击中目标的高度。

```
function Ball(sx,sy,rad,styleString) {
	this.sx = sx;
	this.sy = sy;
	this.rad = rad;
	this.draw = drawBall;
	this.moveit = moveBall;
	this.fillstyle = styleString;
}
```

```
function drawBall() {
	ctx.fillStyle=this.fillStyle;
	ctx.beginPath();
	//ctx.fillStyle= rgb(0,0,0);
	ctx.arc(this.sx,this.sy,this.rad,0,Math.
PI*2,true);
	ctx.fill();
}
```

（*续*）

## 第 4 章 炮弹与弹弓

***表 4-4.*** （*续*）

**代码**

**解释**

```
function moveBall(dx,dy) {
	this.sx +=dx;
	this.sy +=dy;
}
```

`var cball = new Ball(cannonx+cannonLength, cannony+cannonht*.5, ballrad, "rgb(250,0,0)");`

# 第 4 章 炮弹与弹弓

```javascript
function myRectangle(sx,sy,swidth,sheight,stylestring) {
	this.sx = sx;
	this.sy = sy;
	this.swidth = swidth;
	this.sheight = sheight;
	this.fillstyle = stylestring;
	this.draw = drawrects;
	this.moveit = moveball;
}
```

```javascript
function drawRects() {
	ctx.fillStyle = this.fillStyle;
	ctx.fillRect(this.sx,this.sy,this.
swidth,this.sheight);
}
```

```javascript
function Picture (sx,sy,swidth,sheight,filen) {
```

用于设置`Picture`对象的函数头。

`var imga = new Image();`

创建一个`Image`对象。

`imga.src=filen;`

设置文件名。

`this.sx = sx;`

设置`sx`属性。

（*续*）

## 表 4-4. （*续*）

**代码**

**解释**

`this.sy = sy;`

设置`sy`属性。

`this.img = imga;`

将`img`属性设置为`imga`。

`this.swidth = swidth;`

设置`swidth`属性。

`this.sheight = sheight;`

设置`sheight`属性。

`this.draw = drawAnImage;`

设置`draw`属性。这将是此类对象的绘制方法。

`this.moveit = moveBall;`

这将是`moveit`方法。未被使用。

`}`

关闭`Picture`函数。

```javascript
function drawAnImage() {
```

`drawAnImage`函数的头部。

```javascript
ctx.drawImage(this.img,this.sx,this.sy,this.swidth,this.sheight);
```

使用当前对象的属性绘制图像。

`}`

关闭该函数。

`var target = new Picture(targetx,targety, targetw,targeth,"hill.jpg");`

创建一个新的`Picture`对象，并将其赋值给变量`target`。

`var htarget = new Picture(htargetx, htargety, htargetw, htargeth, "plateau.jpg");`

创建一个新的`Picture`对象，并将其赋值给变量`htarget`。

`var ground = new myRectangle(0,300, 600,30,"rgb(10,250,0)");`

创建一个新的`myRectangle`对象，并将其赋值给`ground`。

`var cannon = new myRectangle(cannonx, cannony, cannonlength,cannonht,"rgb(40,40,0)");`

创建一个新的`myRectangle`对象，并将其赋值给`cannon`。

`var targetindex = everything.length;`

保存`target`将要使用的索引。

`everything.push([target,false]);`

将`target`添加到`everything`。

`everything.push([ground,false]);`

将`ground`添加到`everything`。

（*续*）

## 表 4-4. （*续*）

**代码**

**解释**

`var ballindex = everything.length;`

保存`cball`将要使用的索引。

`everything.push([cball,false]);`

将`cball`添加到`everything`。

`var cannonIndex = everything.length;`

保存`cannon`将要使用的索引。

`everything.push([cannon,true,0, cannonx, cannony+cannonht*.5]);`

将`cannon`添加到`everything`；为旋转预留空间。

```javascript
function init(){
	ctx = document.getElementById
('canvas').getContext('2d');
	drawall();
}
```

```javascript
function fire() {
	var angle = Number(document.f.ang.value);
```

从表单中提取角度；转换为数字。

```javascript
	var outOfCannon = Number(document.f.vo.value);
```

从表单中提取出膛速度；转换为数字。

`var angleRadians = angle*Math.PI/180;`

转换为弧度。

`horvelocity = outOfCannon*Math.cos(angleradians);`

计算水平速度。

`verticalvel1 = - outOfCannon*Math.sin(angleradians);`

计算初始垂直速度。

`everything[cannonIndex][2]= - angleRadians;`

设置旋转炮管所需的信息。