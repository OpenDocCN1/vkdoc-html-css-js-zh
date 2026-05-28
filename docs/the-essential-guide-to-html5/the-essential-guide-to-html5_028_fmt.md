# 第 3 章 弹跳球

**表 3-3.** 第二个应用程序，使用图像作为球并采用渐变填充墙壁

| **代码** | **说明** |
|---|---|
| `<html>` | |
| `<head>` | |
| `<title>Bouncing Ball with inputs</title>` | |
| `<style>` | |
| `form {` | |
| `width:330px;` | |
| `margin:20px;` | |
| `background-color:#b10515;` | |
| `padding:20px;` | |
| `}` | |
| `</style>` | |
| `<script type="text/javascript">` | |
| `var boxx = 20;` | |
| `var boxy = 30;` | |
| `var boxWidth = 350;` | |
| `var boxHeight = 250;` | |
| `var ballrad = 20;` | 这不是一个实质性的变化，但图片需要更大的半径。 |
| `var boxBoundx = boxWidth+boxx-ballrad;` | |
| `var boxBoundy = boxHeight+boxy-ballrad;` | |

# 第 3 章 弹跳球

## 表 3-3. （*续*）

| 代码 | 说明 |
| :--- | :--- |
| `var inboxBoundx = boxx + ballrad;` | |
| `var inboxBoundy = boxy + ballrad;` | |
| `var ballx = 50;` | |
| `var bally = 60;` | |
| `var ballvx = 4;` | |
| `var ballvy = 8;` | |
| `var img = new Image();` | 将 `img` 变量定义为一个 `Image` 对象。`new` 运算符和 `Image` 函数的调用实现了这一点。 |
| `img.src = "pearl.jpg";` | 将此图像的 `src` 设置为 "pearl.jpg" 文件。 |
| `var ctx;` | |
| `var grad;` | 将 `grad` 设为一个变量。它将在 `init` 函数中被赋值。 |
| `var color;` | 用于设置渐变 `grad`。 |
| `var hue = [` | 用于设置渐变 `grad`。这是一个数组的数组，每个内部数组提供 RGB 值。 |
| `[255, 0, 0],` | 红色。 |
| `[255, 255, 0],` | 黄色。 |
| `[0, 255, 0],` | 绿色。 |
| `[0, 255, 255],` | 青色。 |
| `[0, 0, 255],` | 蓝色。 |
| `[255, 0, 255]` | 紫色（品红）。 |
| `];` | 关闭数组。 |
| `function init() {` | 用于设置渐变。 |
| `var h;` | |
| `ctx = document.getElementById('canvas').getContext('2d');` | |
| `grad = ctx.createLinearGradient(boxx, boxy, boxx + boxWidth, boxy + boxHeight);` | 创建并赋值一个渐变值。 |
| `for (h = 0; h < hue.length; h++) {` | `for` 循环开始。 |
| `color = 'rgb(' + hue[h][0] + ',' + hue[h][1] + ',' + hue[h][2] + ')';` | 将 `color` 设置为一个表示 RGB 值的字符串。 |
| `grad.addColorStop(h * 1 / hue.length, color);` | 设置颜色停止点以定义渐变。 |
| `}` | 关闭 `for` 循环。 |
| `ctx.fillStyle = grad;` | 将填充设置为 `grad`。 |
| `ctx.lineWidth = ballrad;` | |
| `moveball();` | |
| `setInterval(moveBall, 100);` | |
| `}` | |
| `function moveBall() {` | |
| `ctx.clearRect(boxx, boxy, boxwidth, boxheight);` | |
| `moveAndCheck();` | |
| `ctx.drawImage(img, ballx - ballrad, bally - ballrad, 2 * ballrad, 2 * ballrad);` | 绘制图像。 |
| `ctx.fillRect(boxx, boxy, ballrad, boxheight);` | 绘制左墙。 |
| `ctx.fillRect(boxx + boxWidth - ballrad, boxy, ballrad, boxHeight);` | 绘制右墙。 |
| `ctx.fillRect(boxx, boxy, boxWidth, ballrad);` | 绘制顶墙。 |
| `ctx.fillRect(boxx, boxy + boxHeight - ballrad, boxWidth, ballrad);` | 绘制底墙。 |
| `}` | |
| `function moveAndCheck() {` | |
| `var nballx = ballx + ballvx;` | |
| `var nbally = bally + ballvy;` | |
| `if (nballx > boxBoundx) {` | |
| `ballvx = -ballvx;` | |
| `nballx = boxBoundx;` | |
| `}` | |
| `if (nballx < inboxBoundx) {` | |
| `nballx = inboxBoundx;` | |
| `ballvx = -ballvx;` | |
| `}` | |
| `if (nbally > boxBoundy) {` | |
| `nbally = boxBoundy;` | |
| `ballvy = -ballvy;` | |
| `}` | |
| `if (nbally < inboxBoundy) {` | |
| `nbally = inboxBoundy;` | |
| `ballvy = -ballvy;` | |
| `}` | |
| `ballx = nballx;` | |
| `bally = nbally;` | |
| `}` | |
| `function change() {` | |
| `ballvx = Number(document.f.hv.value);` | |
| `ballvy = Number(document.f.vv.value);` | |
| `return false;` | |
| `}` | |
| `</script>` | |
| `</head>` | |
| `<body onLoad="init();">` | |
| `<canvas id="canvas" width="400" height="300">` | 此浏览器不支持 HTML5 canvas 元素。 |
| `</canvas>` | |
| `<br/>` | |
| `<form name="f" id="f" onSubmit="return change();">` | |
| `水平速度 <input name="hv" id="hv" value="4" type="number" min="-10" max="10" />` | |
| `<br>` | |
| `垂直速度 <input name="vv" id="vv" value="8" type="number" min="-10" max="10"/>` | |
| `<input type="submit" value="更改"/>` | |
| `</form>` | |
| `</body>` | |
| `</html>` | |

我选择在第一个应用程序的基础上对样式信息进行适度的修改。表 3-4 展示了第三个弹跳球应用程序，并带有表单验证。同样，我只注释了新代码，但为了完整性，我包含了所有代码。

## 表 3-4. 第三个弹跳球应用程序，带表单验证

| 代码 | 说明 |
| :--- | :--- |
| `<html>` | |
| `<head>` | |
| `<title>带输入的弹跳球</title>` | |
| `<style>` | |
| `form {` | |
| `width:330px;` | |
| `margin:20px;` | |
| `background-color:brown;` | |
| `padding:20px;` | |
| `}` | |
| `input:valid {background:green;}` | 为有效输入设置反馈。 |
| `input:invalid {background:red;}` | 为无效输入设置反馈。 |
| `</style>` | |
| `<script type="text/javascript">` | |
| `var cWidth = 400;` | |
| `var cHeight = 300;` | |
| `var ballrad = 10;` | |
| `var boxx = 20;` | |
| `var boxy = 30;` | |
| `var boxWidth = 350;` | |
| `var boxHeight = 250;` | |
| `var boxBoundx = boxWidth + boxx - ballrad;` | |
| `var boxBoundy = boxHeight + boxy - ballrad;` | |
| `var inboxBoundx = boxx + ballrad;` | |