# 排版后的内容

成千上万的像素，这被称为*高分辨率*。这使得文件变得很大。为了确保图像准备好使用，一个技巧是在`body`元素中创建包含图像的`img`元素。对于这个例子，这包括背景照片和棉花糖照片。在`body`标签的`onLoad`属性的作用下，这些文件将在`init`函数被调用之前加载。完全加载的背景图像将在棉花糖图像绘制在其上之前就可以被绘制。挑战在于如何阻止这两张图像被显示。答案是，在`style`元素中包含以下指令：

```css
img {visibility: hidden;}
```

这条 CSS 指令阻止任何`img`文件被显示。在我的例子中，`img`元素从未被显示。被显示的是由代码创建和操作的`Image`元素。

## 构建应用并使其成为你自己的

我现在将解释基础弹跳球应用的代码，使用图像作为球并使用渐变色作为墙壁的应用，以及验证输入和弹跳棉花糖的应用。`表 3-1`显示了所有函数调用以及它们调用了什么。该表包含所有四个应用的函数。`stopcc`和`resume`函数仅在第四个应用中出现。

**表 3-1.** 弹跳球应用中的函数

| 函数 | 被谁调用/由谁调用 | 调用了谁 |
|---|---|---|
| `init` | `body`标签中`onLoad`属性的行为 | `moveBall` |
| `moveBall` | 由`init`直接调用以及由`setInterval`的行为调用 | `moveAndCheck` |
| `moveAndCheck` | 由`moveBall`调用 | |
| `change` | `form`标签中`onSubmit`属性的行为 | |
| `stopcc` | 按钮标签中`onClick`属性的行为 | `moveBall` |
| `resume` | 按钮标签中`onClick`属性的行为 | |

## 第 3 章 弹跳球

`moveAndCheck`代码本可以是`moveBall`函数的一部分。我选择将其分离，因为定义一个执行特定操作的函数是一种好的实践。通常，在开发应用时，更多、更小的函数比更少、更大的函数要好。顺便说一句，当你自己编程时，不要忘记按照第 2 章所述在代码中添加注释并添加空白行以使代码更易读。`表 3-2`展示了基础弹跳球应用的代码并解释了每一行的作用。

**表 3-2.** 弹跳球应用

| 代码 | 解释 |
|---|---|
| `<html>` | 开始`html`。 |
| `<head>` | 开始`head`。 |
| `<title>Bouncing Ball with inputs` | 完成`title`元素。 |
| `</title>` | |
| `<style>` | 开始`style`。 |
| `form {` | 开始`form`样式。 |
| `width:330px;` | 设置宽度。 |
| `margin:20px;` | 设置外边距。 |
| `background-color:brown;` | 设置背景颜色。 |
| `padding:20px;` | 设置内边距。 |
| `}` | 关闭此样式。 |
| `</style>` | 关闭`style`元素。 |
| `<script type="text/javascript">` | 开始`script`元素。（`type`不是必需的。我在这里显示它只是为了让你知道你在许多在线示例中会看到什么。） |
| `var boxx = 20;` | 盒子左上角的 x 坐标。 |
| `var boxy = 30;` | 盒子左上角的 y 坐标。 |
| `var boxWidth = 350;` | 盒子宽度。 |
| `var boxHeight = 250;` | 盒子高度。 |
| `var ballrad = 10;` | 球的半径。 |
| `var boxBoundx = boxWidth+boxx- ballrad;` | 右边界。 |
| `var boxBoundy = boxHeight+boxy- ballrad;` | 底部边界。 |
| `var inboxBoundx = boxx+ballrad;` | 左边界。 |
| `var inboxBoundy = boxy+ballrad;` | 顶部边界。 |
| `var ballx = 50;` | 球的初始 x 坐标位置。 |
| `var bally = 60;` | 球的初始 y 坐标位置。 |
| `var ctx;` | 保存画布上下文的变量。 |
| `var ballvx = 4;` | 初始水平位移。 |
| `var ballvy = 8;` | 初始垂直位移。 |
| `function init() {` | 开始`init`函数。 |
| `ctx = document. getElementById ('canvas'). getContext('2d');` | 设置`ctx`变量。 |
| `ctx.linewidth = ballrad;` | 设置线宽。 |
| `ctx.fillStyle ="rgb(200,0,50)";` | 设置填充样式。 |
| `moveBall();` | 第一次调用`moveBall`函数以移动、检查和显示球。 |
| `setInterval(moveBall,100);` | 设置定时事件。 |
| `}` | 关闭`init`函数。 |
| `function moveBall(){` | 开始`moveBall`函数。 |
| `ctx.clearRect(boxx,boxy,boxWidth,` | 清除（擦除）这个方块（包括来自球的任何颜料）。 |
| `boxheight);` | |
| `moveAndCheck();` | 执行检查，然后移动球。 |