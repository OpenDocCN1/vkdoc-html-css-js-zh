# 第 7 章 MazeS（迷宫）

**表 7-3.**（续）

| 代码 | 说明 |
|------|------|
| `length;i++) {` | |
| `everything[i].draw();` | 绘制所有内容。 |
| `}` | 关闭循环。 |
| `}` | 关闭函数。 |

```javascript
function getKeyAndMove(event) {
```
函数头`getKeyAndMove`。

| 代码 | 说明 |
|------|------|
| `var keyCode;` | 存放按键码`keyCode`。 |
| `if(event == null) {` | 如果事件为空。 |
| `keyCode = window.event.keyCode;` | 使用`window.event`获取`keyCode`。 |
| `window.event.preventDefault();` | 阻止默认行为。 |
| `}` | 关闭子句。 |
| `else {` | 否则。 |
| `keyCode = event.keyCode;` | 从事件中获取`keyCode`。 |
| `event.preventDefault();` | 阻止默认行为。 |
| `}` | 关闭子句。 |
| `switch(keyCode) {` | 根据`keyCode`进行切换。 |
| `case 37:` | 如果为左箭头。 |
| `mypent.moveit(-unit,0);` | 水平向后移动。 |
| `break;` | 离开`switch`。 |
| `case 38:` | 如果为上箭头。 |
| `mypent.moveit(0,-unit);` | 向屏幕上方移动。 |
| `break;` | 离开`switch`。 |

**表 7-3.**（续）

| 代码 | 说明 |
|------|------|
| `case 39:` | 如果为右箭头。 |
| `mypent.moveit(unit,0);` | 向左移动。 |
| `break;` | 离开`switch`。 |
| `case 40:` | 如果为下箭头。 |
| `mypent.moveit(0,unit);` | 向屏幕下方移动。 |
| `break;` | 离开`switch`。 |
| `default:` | 其他情况。 |
| `window.removeEventListener('keydown',getkeyAndMove,false);` | 停止监听按键。假设玩家试图保存到本地存储或从本地存储中读取。 |
| `}` | 关闭`switch`。 |
| `drawAll();` | 绘制所有内容。 |
| `}` | 关闭函数。 |

```javascript
function intersect(sx,sy,fx,fy,
```
函数头`intersect`。

| 代码 | 说明 |
|------|------|
| `cx,cy,rad) {` | |
| `var dx;` | 用于存放中间值。 |
| `var dy;` | 用于存放中间值。 |
| `var t;` | 用于表示`t`中的表达式。 |
| `var rt;` | 用于存放距离的平方。 |
| `dx = fx-sx;` | 设置 x 的差值。 |
| `dy = fy-sy;` | 设置 y 的差值。 |
| `t =0.0-((sx-cx)*dx+(sy-cy)*dy)/((dx*dx)+(dy*dy));` | 这一行来源于计算每一点到`cx,cy`距离平方的公式，然后求导并令其等于 0。 |
| `if (t<0.0) {` | 如果最近点位于`t < 0`处。 |
| `t=0.0; }` | 在 0 处检查（这将更远）。 |
| `else if (t>1.0) {` | 如果最近点位于`t > 1`处。 |

**表 7-3.**（续）

| 代码 | 说明 |
|------|------|
| `t = 1.0;` | 在 1 处检查（这将更远）。 |
| `}` | 关闭子句。 |
| `dx = (sx+t*(fx-sx))-cx;` | 计算在此`t`值下的差值。 |
| `dy = (sy +t*(fy-sy))-cy;` | 计算在此`t`值下的差值。 |
| `rt = (dx*dx) +(dy*dy);` | 计算距离的平方。 |
| `if (rt<(rad*rad)) {` | 与`rad`的平方进行比较。 |
| `return true; }` | 返回`true`。 |
| `else {` | 否则。 |
| `return false;}` | 返回`false`。 |
| `}` | 关闭函数。 |

```javascript
function saveWalls() {
```
函数头`saveWalls`。

| 代码 | 说明 |
|------|------|
| `var w = [];` | 临时数组。 |
| `var allw=[];` | 临时数组。 |
| `var sw;` | 存放最终字符串。 |
| `var oneWall;` | 存放中间字符串。 |
| `var i;` | 索引。 |
| `var lsname = document.sf.slname.value;` | 提取玩家的名称用于本地存储。 |
| `for (i=0;i<walls.length;i++) {` | 遍历所有墙壁。 |
| `w.push(walls[i].sx);` | 将`sx`添加到`w`数组中。 |
| `w.push(walls[i].sy);` | 将`sy`添加到`w`数组中。 |
| `w.push(walls[i].fx);` | 将`fx`添加到`w`数组中。 |
| `w.push(walls[i].fy);` | 将`fy`添加到`w`数组中。 |
| `onewall = w.join("+");` | 生成一个字符串。 |
| `allw.push(onewall);` | 添加到`allw`数组中。 |

**表 7-3.**（续）

| 代码 | 说明 |
|------|------|
| `w = [];` | 将`w`重置为空数组。 |
| `}` | 关闭循环。 |
| `sw = allw.join(";");` | 现在将`allw`转换为一个字符串。 |
| `try {` | 尝试。 |
| `localStorage.setItem(lsname,sw);` | 保存到`localStorage`。 |
| `}` | 结束`try`。 |
| `catch (e) {` | 如果出现可捕获的错误。 |
| `alert("data not saved, error given: "+e);` | 显示消息。 |
| `}` | 结束`catch`子句。 |
| `return false;` | 返回`false`以避免刷新。 |
| `}` | 关闭函数。 |

```javascript
function getWalls() {
```
函数头`getWalls`。

| 代码 | 说明 |
|------|------|
| `var swalls;` | 临时存储。 |
| `var sw;` | 临时存储。 |
| `var i;` | 索引。 |
| `var sx;` | 存放`sw`值。 |
| `var sy;` | 存放`sy`值。 |
| `var fx;` | 存放`fx`值。 |
| `var fy;` | 存放`fy`值。 |
| `var curWall;` | 存放正在创建的墙壁。 |
| `var lsname = document.gf.glname.value;` | 提取玩家的名称以便从存储中检索。 |
| `swalls=localStorage.getItem(lsname);` | 获取存储内容。 |

**表 7-3.**（续）

| 代码 | 说明 |
|------|------|
| `if (swalls!=null) {` | 如果取到了内容。 |
| `wallstgs = swalls.split(";");` | 分割以生成数组。 |
| `for (i=0;i<wallstgs.length;i++) {` | 遍历此数组。 |
| `sw = wallstgs[i].split("+");` | 分割单个项目。 |
| `sx = Number(sw[0]);` | 提取第 0 个值并转换为数字。 |
| `sy = Number(sw[1]);` | 提取第 1 个值并转换为数字。 |
| `fx = Number(sw[2]);` | 提取第 2 个值并转换为数字。 |
| `fy = Number(sw[3]);` | 提取第 3 个值并转换为数字。 |



```html
# 第 7 章 迷宫

#### 表 7-3（续）

| 代码 | 说明 |
|------|------|
| `curWall = new Wall(sx,sy,fx,fy, wallWidth, wallStyle);` | 使用提取并固定的值创建新墙壁。 |
| `walls.push(curWall);` | 添加到墙壁数组。 |
| `everything.push(curWall);` | 添加到全部对象数组。 |
| `}` | 关闭循环。 |
| `drawAll();` | 绘制所有内容。 |
| `}` | 关闭非空判断。 |
| `else {` | 值为空。 |
| `alert("No data retrieved.");` | 未检索到数据。 |
| `}` | 关闭子句。 |
| `window.addEventListener('keydown', getkeyAndMove, false);` | 设置按键动作。 |
| `return false;` | 返回 false 以防止刷新。 |
| `}` | 关闭函数。 |
| `</script>` | |
| `</head>` | 结束 head 元素。 |

*(续)*

**第 7 章 迷宫**

**表 7-3（续）**

| **代码** | **说明** |
|----------|----------|
| `<body onLoad="init();" >` | 开始 body；设置对`init`的调用。 |
| `<canvas id="canvas" width="900" height="350">` | Canvas 标签。 |
| `Your browser doesn't support the HTML5 element canvas.` | 针对某些浏览器的警告。 |
| `</canvas>` | 关闭 canvas。 |
| `<br/>` | 换行。 |
| 按下鼠标按钮，拖动并释放以创建墙壁。使用箭头键移动令牌。 | 操作说明。 |
| `<br/>` | 操作说明与换行。 |
| 按下任何其他键将停止按键捕捉，并允许您在本地保存迷宫。 | 操作说明。 |
| `<form name="sf" onSubmit="return saveWalls()" >` | 表单标签；设置对`saveWalls`的调用。 |
| 要保存您的迷宫，请输入名称并点击“保存墙壁”按钮。<br/>使用名称 `<em>easymaze</em>`、`<em>moderatemaze</em>` 和 `<em>hardmaze</em>` 以便在 travelmaze 程序中使用。 | 操作说明。这些名称必须与`travelmaze`中使用的名称相匹配。 |
| `<br/>` | |
| 名称：`<input name="slname" value="maze_name" type="text">` | 标签和输入字段。 |
| `<input type="submit" value="SAVE WALLS"/>` | 提交按钮。 |

*(续)*

**第 7 章 迷宫**

**表 7-3（续）**

| **代码** | **说明** |
|----------|----------|
| `</form>` | 关闭表单。 |
| `<form name="gf" onSubmit="return getWalls()" >` | 表单标签；设置对`getWalls`的调用。 |
| 要添加旧墙壁，请输入名称并点击“获取已保存墙壁”按钮。<br/> | 操作说明。 |
| 名称：`<input name="glname" value="maze_name" type="text">` | 标签和输入字段。 |
| `<input type="submit" value="GET SAVED WALLS"/>` | 提交按钮。 |
| `</form>` | 关闭表单。 |
| `</body>` | 关闭 body。 |
| `</html>` | 关闭 HTML。 |

#### 创建旅行迷宫应用程序

只要位于同一服务器上，`localStorage`数据可以被与创建数据的 HTML 文档不同的另一个 HTML 文档访问。如前所述，这是一项安全功能，将本地存储的读取权限限制在同一服务器上的脚本。

第二个脚本正是基于此功能。表 7-4 显示了相互调用或调用的函数；它是前一个表的一个子集。

**第 7 章 迷宫**

**表 7-4.** 旅行迷宫脚本中的函数

| **函数** | **由谁调用/调用者** | **调用谁** |
|----------|---------------------|------------|
| `init` | 由 body 标签中的`onLoad`动作调用 | `drawAll` |
| `drawAll` | `init` | `startWall`、`stretchWall`、`getkeyAndMove`、`getWalls`、墙壁的 draw 方法和令牌的 draw 方法：`drawToken`和`drawAline` |
| `Token` | 声明`mypent`的变量语句 | `Wall` |
| `Wall` | `startWall`、`getWalls` | |
| `drawToken` | 通过`everything`数组中令牌对象的 draw 方法由`drawAll`调用 | |
| `moveToken` | 通过`mypent`的`moveit`方法由`getKeyAndMove`调用 | `intersect` |
| `drawAline` | 通过`everything`数组中 Wall 对象的 draw 方法由`drawAll`调用 | |
| `getkeyAndMove` | 由`init`中`addEventListener`调用的动作调用 | 通过`mypent`的`moveit`方法调用`moveToken` |
| `getWalls` | 由`gf`表单的`onSubmit`动作调用 | `drawAll`、`Wall` |
| `intersect` | `moveToken` | |

这些函数与一体化脚本中的相同，但有一个例外，即`getWalls`函数，因此我仅对新增或修改的代码进行了注释。此应用程序还用单选按钮替代了表单输入字段。表 7-5 显示了`travelmaze`应用程序的完整代码。

**第 7 章 迷宫**

**表 7-5.** 旅行迷宫脚本的完整代码

| **代码** | **说明** |
|----------|----------|
| `<html>` | |
| `<head>` | |
| `<title>Travel maze</title>` | 旅行迷宫。 |
| `<script type="text/javascript">` | |
| `var cwidth = 900;` | |
| `var cheight = 700;` | |
| `var ctx;` | |
| `var everything = [];` | |
| `var curWall;` | |
| `var wallWidth = 5;` | |
| `var wallStyle = "rgb(200,0,200)";` | |
| `var walls = [];` | |
| `var inMotion = false;` | |
| `var unit = 10 ;` | |
| `function Token(sx,sy,rad,styleString,n)` | |
| `{` | |
| `this.sx = sx;` | |
| `this.sy = sy;` | |
| `this.rad = rad;` | |
| `this.styleString = styleString;` | |
| `this.n = n;` | |
| `}` | |
```


好的，作为高级文档工程师，我已按照您的要求对文本进行排版处理。

---

`this.draw = drawToken;`

`this.n = n;`

`this.angle = (2*Math.PI)/n`

`this.moveit = moveToken;`

`this.fillStyle = styleString;`

}

(*续*)

## 第 7 章 迷宫 S

***表 7-5.*** (*续*)

| 代码 | 说明 |
| :--- | :--- |
| `function drawToken() {` | |
| `ctx.fillStyle=this.fillStyle;` | |
| `ctx.beginPath();` | |
| `var i;` | |
| `var rad = this.rad ;` | |
| `ctx.beginPath();` | |
| `ctx.moveTo(this.sx+rad*Math.cos(-.5*this.angle),this.sy+rad*Math.sin(-.5*this.angle));` | |
| `for (i=1;i<this.n;i++) {` | |
| `ctx.lineTo(this.sx+rad*Math.cos((i-.5)*this.angle),this.sy+rad*Math.sin((i-.5)*this.angle));` | |
| `}` | |
| `ctx.fill();` | |
| `}` | |
| `function moveToken(dx,dy) {` | |
| `this.sx +=dx;` | |
| `this.sy +=dy;` | |
| `var i;` | |
| `var wall;` | |
| `for(i=0;i<walls.length;i++) {` | |
| `wall = walls[i];` | |
| `if (intersect(wall.sx,wall.sy,wall.fx,wall.fy,this.sx,this.sy,this.rad)) {` | |
| `this.sx -=dx;` | |
| `this.sy -=dy ;` | |
| `break;` | |
| `}` | |
| `}` | |
| `}` | |

(*续*)

## 第 7 章 迷宫 S

***表 7-5.*** (*续*)

| 代码 | 说明 |
| :--- | :--- |
| `function Wall(sx,sy,fx,fy,width,styleString) {` | |
| `this.sx = sx;` | |
| `this.sy = sy;` | |
| `this.fx = fx;` | |
| `this.fy = fy;` | |
| `this.width = width;` | |
| `this.draw = drawAline;` | |
| `this.strokestyle = styleString;` | |
| `}` | |
| `function drawAline() {` | |
| `ctx.lineWidth = this.width;` | |
| `ctx.strokeStyle = this.strokestyle;` | |
| `ctx.beginPath();` | |
| `ctx.moveTo(this.sx,this.sy);` | |
| `ctx.lineTo(this.fx,this.fy);` | |
| `ctx.stroke() ;` | |
| `}` | |

(*续*)

## 第 7 章 迷宫 S

***表 7-5.*** (*续*)

| 代码 | 说明 |
| :--- | :--- |
| `var mypent = new Token(100,100,20,"rgb(0,0,250)",5);` | |
| `everything.push(mypent);` | |
| `function init(){` | |
| `ctx = document.getElementById('canvas').getContext('2d');` | |
| `window.addEventListener('keydown',getkeyAndMove,false);` | |
| `drawAll();` | |
| `}` | |
| `function drawAll() {` | |
| `ctx.clearRect(0,0,cWidth,cHeight);` | |
| `var i;` | |
| `for (i=0;i<everything.length;i++) {` | |
| `everything[i].draw() ;` | |
| `}` | |
| `}` | |
| `function getKeyAndMove(event) {` | |
| `var keyCode;` | |
| `if(event == null)` | |
| `{` | |
| `keyCode = window.event.keyCode;` | |
| `window.event.preventDefault();` | |
| `}` | |

(*续*)

## 第 7 章 迷宫 S

***表 7-5.*** (*续*)

| 代码 | 说明 |
| :--- | :--- |
| `else` | |
| `{` | |
| `keyCode = event.keyCode;` | |
| `event.preventDefault();` | |
| `}` | |
| `switch(keyCode)` | |
| `{` | |
| `case 37: //left arrow` | |
| `mypent.moveit(-unit,0);` | |
| `break ;` | |
| `case 38: //up arrow` | |
| `mypent.moveit(0,-unit);` | |
| `break;` | |
| `case 39: //right arrow` | |
| `mypent.moveit(unit,0);` | |
| `break;` | |
| `case 40: //down arrow` | |
| `mypent.moveit(0,unit);` | |
| `break;` | |
| `default:` | |
| `window.removeEventListener('keydown',getkeyAndMove,false);` | |
| `}` | |
| `drawAll();` | |
| `}` | |

(*续*)

## 第 7 章 迷宫 S

***表 7-5.*** (*续*)

| 代码 | 说明 |
| :--- | :--- |
| `function intersect(sx,sy,fx,fy,cx,cy,rad) {` | |
| `var dx;` | |
| `var dy;` | |
| `var t ;` | |
| `var rt;` | |
| `dx = fx-sx;` | |
| `dy = fy-sy;` | |
| `t =0.0-((sx-cx)*dx+(sy-cy)*dy)/((dx*dx)+(dy*dy));` | |
| `if (t<0.0) {` | |
| `t=0.0; }` | |
| `else if (t>1.0) {` | |
| `t = 1.0;` | |
| `}` | |
| `dx = (sx+t*(fx-sx))-cx;` | |
| `dy = (sy +t*(fy-sy))-cy;` | |
| `rt = (dx*dx) +(dy*dy);` | |
| `if (rt<(rad*rad)) {` | |
| `return true; }` | |
| `else {` | |
| `return false;}` | |
| `}` | |
| `function getWalls() {` | |
| `var swalls ;` | |

(*续*)

## 第 7 章 迷宫 S

***表 7-5.*** (*续*)

| 代码 | 说明 |
| :--- | :--- |
| `var sw;` | |
| `var i;` | |
| `var sx;` | |
| `var sy;` | |
| `var fx;` | |
| `var fy;` | |
| `var curWall;` | |
| `var lsname;` | |
| `for (i=0;i<document.gf.level.length;i++) {` | 遍历`gf`表单中名为`level`的单选按钮组。 |
| `if (document.gf.level[i].checked) {` | 这个单选按钮是否被选中？ |
| `lsname= document.gf.level[i].value+"maze";` | 如果是，则使用该单选按钮元素的`value`属性构建本地存储名称。 |
| `break;` | 退出`for`循环。 |
| `}` | 关闭`if`。 |
| `}` | 关闭`for`。 |
| `swalls=localStorage.getItem(lsname);` | 从本地存储中获取此项。 |
| `if (swalls!=null) {` | 如果不为空，则是有效数据。 |
| `wallstgs = swalls.split(";");` | 提取每个墙壁的字符串。 |
| `walls = [];` | 从`walls`数组中移除所有旧墙壁。 |
| `everything = [];` | 从`everything`数组中移除所有旧墙壁。 |
| `everything.push(mypent);` | 将名为`mypent`的五边形标记添加到`everything`。 |

(*续*)

## 第 7 章 迷宫 S

***表 7-5.*** (*续*)

| 代码 | 说明 |
| :--- | :--- |
| `for (i=0;i<wallstgs.length;i++) {` | 开始解码每面墙壁。其余代码与全功能应用相同。 |
| `sw = wallstgs[i].split("+");` | |
| `sx = Number(sw[0]);` | |
| `sy = Number(sw[1]);` | |
| `fx = Number(sw[2]);` | |
| `fy = Number(sw[3]);` | |
| `curWall = new Wall(sx,sy,fx,fy,wallWidth,wallStyle);` | |
| `walls.push(curWall);` | |
| `everything.push(curWall);` | |
| `}` | |
| `drawAll();` | |
| `}` | |
| `else {` | |
| `alert("No data retrieved.");` | |
| `}` | |
| `window.addEventListener('keydown',getkeyAndMove,false);` | |
| `return false ;` | |
| `}` | |
| `</script>` | |
| `</head>` | |

(*续*)


