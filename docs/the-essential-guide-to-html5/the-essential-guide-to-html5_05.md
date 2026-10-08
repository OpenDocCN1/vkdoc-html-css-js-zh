# 第三章 弹跳球

## 使用匿名函数启动、定位和重新播放视频

视频会略微移过底部和右侧边缘。要精确实现弹跳并不容易。物体在空间中并非连续运动！

正如我之前所述，我的程序使用一个按钮来启动视频。这需要用户交互。该按钮会调用 `startV` 函数。请参考以下代码。

视频开始播放。显示设置为 `block`（之前为 `none`）。容纳视频的 `div` 元素（我将其命名为 `c`，即容器）被定位在变量 `ballx` 和 `bally` 的任意值处。

视频的重新播放（而非屏幕上视频的移动）是通过为 `ended` 事件设置事件处理程序来实现的。我决定使用所谓的*匿名函数*来演示这一点。整个函数包含在 `v.addEventListener` 的调用中。

```javascript
function startV(){
    v.play();
    v.style.display = "block";
    c.style.top = bally + "px";
    c.style.left = ballx + "px";
    v.addEventListener('ended', function() {
        v.currentTime = 0;
        v.play();
    });
}
```

不使用匿名函数的一个原因是调试工具没有可跟踪的函数。但它也有触手可及的好处。我在此使用了空格和换行。你也可以将其压缩为一行。

## 验证

表单，即从用户/玩家/客户端获取输入的方式，是原始 HTML 的一部分。表单元素以 `<form>` 标签开头，该标签提供了一种指定提交表单时执行操作的方式，并包含输入元素。HTML5 提供了验证表单输入的新功能。表单创建者可以指定输入字段为 `number` 类型（而非 `text` 类型），HTML5 将立即检查用户/玩家是否输入了数字。类似地，我们可以指定 `max` 和 `min` 值。表单的代码如下：

```html
<form name="f" id="f" onSubmit="return change();">
    水平速度
    <input name="hv" id="hv" value="4" type="number" min="-10" max="10" />
    <br>
    垂直速度
    <input name="vv" id="vv" value="8" type="number" min="-10" max="10" />
    <input type="submit" value="更改" />
</form>
```

输入仍然是文本，即一串字符，但这些值应该是可以解释为指定范围内数字的文本。

其他输入类型包括 `"email"` 和 `"URL"`，让 HTML5 检查这些内容非常方便。当然，你可以使用 `isNumber` 和更复杂的编码（包括*正则表达式*，即可匹配的字符模式）来检查任何字符串是否为数字，以验证有效的电子邮件地址和 URL。

一种常见的电子邮件地址检查策略是让用户输入两次，以便比较两者，确保用户没有犯任何错误。

我们希望利用 HTML5 为我们做的工作，但我们也希望让用户/玩家知道是否出了问题。你可以使用 HTML5 和 CSS 来实现这一点，即为有效和无效的输入指定样式。

```css
input:valid {background:green;}
input:invalid {background:red;}
```

HTML5 验证至少在计算机上的最新版本浏览器中是可操作的，但你需要根据自己的需求决定如何处理旧版浏览器和设备。如果你使用的是符合规范的浏览器（例如 Chrome），可以测试下一节给出的示例。请注意，即使输入了无效值（例如在指定数字的地方输入 `abc`），球也会继续弹跳，因为程序会继续使用当前设置。

**提示**：在任何应用中，验证输入并向用户生成适当的反馈都非常重要。HTML5 提供的新功能之一是 `input` 元素中的 `pattern` 属性，其中可以使用一种称为*正则表达式*的特殊语言来指定有效输入。在搜索字段中输入 **HTML5 正则表达式**，可以找到最新信息。

## 由按钮触发的动画停止与恢复

当我决定添加停止和恢复功能时，我认为一个重要教训是这可以仅仅作为一个新增功能，而无需更改程序的其余部分。

这里涉及的术语是*事件驱动编程*。我们这些构建者或多或少地独立考虑不同的事件。我还决定使用 `button` 元素，这是 HTML5 引入的一项功能。`button` 元素提供了一种指定事件（本例中为 `onClick`）以及处理该事件的函数的方法。`<button>` 标签和 `</button>` 标签之间的文本将显示在菱形按钮中。

旧的方法是使用表单，对于我的示例来说，这意味着要使用多个表单。

以下代码生成了两个按钮。下一节将描述 `return` 语句的重要性。`&nbsp;` 被称为*实体*，它产生一个空格，但不会强制换行。

```html
<button onClick="return stopcc();">停止</button> &nbsp; &nbsp;
<button onClick="return resume();">恢复</button>
```

现在我需要向你介绍 `stopcc` 函数和 `resume` 函数的定义。

`stopcc` 函数的任务是停止棉花糖图像在背景上的移动。你知道该怎么做：调用 `clearInterval`。我的代码还需要做一些其他事情。因为我想恢复弹跳，我编写代码来保存 `ballvx` 和 `ballvy` 的值。这可能是多余的，但某些情况似乎需要这么做。该代码还调用了 `moveBall` 来生成多一幅图像。`return` 的使用将在下一节解释。代码如下：

```javascript
function stopcc() {
    clearInterval(tid);
    stoppedx = ballvx;
    stoppedy = ballvy;
    moveBall();
    return false;
}
```

`resume` 函数确实包含了对 `setInterval` 的调用，但我还需要做其他事情来保护玩家不误操作。如果玩家在未停止动画的情况下单击了恢复按钮，或者只是想看看会发生什么，那么调用多个 `setInterval` 会产生多个定时事件。这进而会导致弹跳看起来越来越快。为了防止这种情况，我插入了一个对 `clearInterval` 的调用。如果不存在定时事件，则不会发生任何事情。我的代码使用之前保存的值重置了 `ballvx` 和 `ballvy`。这可能是多余的，但算是一种预防措施。

```javascript
function resume() {
    clearInterval(tid);
    ballvx = stoppedx;
    ballvy = stoppedy;
    tid = setInterval(moveball, 100);
    return false;
}
```

## HTML 页面重新加载

在继续之前，我想提到一些可能导致意外问题的情况。

浏览器带有重新加载/刷新按钮。单击该按钮时，文档会重新加载。我们在第 2 章的简单掷骰子应用中使用了这个功能。

然而，有时你可能希望阻止重新加载，在这种情况下，你可以在没有返回值以免页面重新加载的函数中放置一个 `return(false);`。

当文档包含表单时，重新加载并不总是会重新初始化表单输入。你可能需要离开页面，然后使用完整 URL 重新加载它。

最后，浏览器会尝试使用先前下载到客户端（用户）计算机的文件，而不是根据日期和时间检查后从服务器请求文件。客户端计算机上的文件存储在所谓的*缓存*中。如果你认为自己做了更改，但浏览器没有显示最新版本，你可能需要采取清除缓存等步骤。

## 预加载图像

计算机速度非常快，通常我们的感知足够慢，以至于我们期望所做的一切都没有延迟。然而，网站上的图像必须从服务器下载到我们的本地计算机，而大图像显然是大型文件。实际上，我还想说明另一点。我们的现代相机产生的图像由大量像素组成。

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

# 第 3 章 弹跳球

**表 3-2.** (*续*)

| **代码** | **说明** |
|---|---|
| `ctx.beginPath();` | 开始路径。 |
| `ctx.arc(ballx, bally, ballrad, 0, Math.PI*2, true);` | 设置在当前球的位置绘制圆形。 |
| `ctx.fill();` | 填充路径；即绘制一个实心圆。 |
| `ctx.strokeRect(boxx, boxy, boxWidth, boxHeight);` | 绘制矩形轮廓。 |
| `}` | 关闭 `moveBall`。 |

```javascript
function moveAndCheck() {
    // 开始 moveAndCheck。
    var nballx = ballx + ballvx;
    // 设置试探性的下一个 x 位置。
    var nbally = bally + ballvy;
    // 设置试探性的下一个 y 位置。
    if (nballx > boxBoundx) {
        // 这个 x 值是否超出了右边界？
        ballvx = -ballvx;
        // 如果是，则改变水平位移方向。
        nballx = boxBoundx;
        // 将下一个 x 值准确地设为该边界值。
    }
    // 关闭该子句。
    if (nballx < inboxBoundx) {
        // 这个 x 值是否小于左边界？
        nballx = inboxBoundx;
        // 如果是，则将 x 值准确地设为边界值。
        ballvx = -ballvx;
        // 改变水平位移方向。
    }
    // 关闭该子句。
    if (nbally > boxBoundy) {
        // y 值是否超出了下边界？
        nbally = boxBoundy;
        // 如果是，则将 y 值准确地设为边界值。
        ballvy = -ballvy;
        // 改变垂直位移方向。
    }
    // 关闭该子句。
    if (nbally < inboxBoundy) {
        // y 值是否小于上边界？
        nbally = inboxBoundy;
        // 如果是，则将 y 值准确地设为边界值。
    }
}
```

(*续*)

# 第 3 章 弹跳球

**表 3-2.** (*续*)

**代码** | **说明**
---|---
`ballvy = -ballvy;` | 改变垂直位移方向。
`}` | 关闭该子句。
`ballx = nballx;` | 将 x 位置设置为 `nballx`。
`bally = nbally;` | 将 y 位置设置为 `nbally`。
`}` | 关闭 `moveAndCheck` 函数。

```javascript
function change() {
    // 开始 change 函数。
    ballvx = Number(document.f.hv.value);
    // 将输入转换为数字并赋值给 `ballvx`。
    ballvy = Number(document.f.vv.value);
    // 将输入转换为数字并赋值给 `ballvy`。
    return false;
    // 返回 false 以确保页面不会重新加载。
}
// 关闭该函数。
</script>
// 关闭 script 标签。
</head>
// 关闭 head 标签。
<body onLoad="init();">
// 开始 body 元素。设置对 `init` 函数的调用。
<canvas id="canvas" width="400" height="300">
// 开始 canvas 元素。
Your browser doesn't support the HTML5 element canvas.
</canvas>
// 关闭 canvas 元素。
<br />
// 换行。
<form name="f" id="f" onSubmit="return change();">
// 开始表单。指定名称和 ID（某些浏览器可能需要）。设置提交按钮的动作。
```

(*续*)

# 第 3 章 弹跳球

**表 3-2.** (续)

| **代码** | **说明** |
| --- | --- |
| `Horizontal velocity <input name="hv" id="hv" value="4" type="number" min="-10" max="10" />` | 标注用于输入水平速度的字段。 |
| `<br>` | 换行。 |
| `Vertical velocity <input name="vv" id="vv" value="8" type="number" min="-10" max="10" />` | 标注用于输入垂直速度的字段。 |
| `<input type="submit" value="CHANGE" />` | 提交按钮。 |
| `</form>` | 关闭表单。 |
| `</body>` | 关闭 body。 |
| `</html>` | 关闭 html。 |

---

使用图像作为球的应用程序与使用渐变填充墙壁的应用程序类似。表 3-3 展示了所有代码——但我只注释了不同的代码部分。我并非偷懒；目的是让你看到每个应用程序是如何在前一个的基础上构建的。

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

# 第 3 章 弹跳球

## 表 3-4（续）

### 代码

### 说明

`var ballx = 50;`
`var bally = 60;`
`var ctx;`
`var ballvx = 4;`
`var ballvy = 8;`

```javascript
function init(){
    ctx = document.getElementById('canvas').getContext('2d');
    ctx.lineWidth = ballrad;
    moveBall();
    setInterval(moveBall,100);
}
```

```javascript
function moveBall(){
    ctx.clearRect(boxx,boxy,boxwidth,boxheight);
    moveAndCheck();
    ctx.beginPath();
    ctx.fillStyle ="rgb(200,0,50)";
    ctx.arc(ballx, bally, ballrad,0,Math.PI*2,true) ;
    ctx.fill();
    ctx.strokeRect(boxx,boxy,boxWidth,boxHeight);
}
```

```javascript
function moveAndCheck() {
    var nballx = ballx + ballvx;
    var nbally = bally +ballvy;
```

## 表 3-4（续）

### 代码

### 说明

```javascript
    if (nballx > boxBoundx) {
        ballvx =-ballvx;
        nballx = boxBoundx;
    }
    if (nballx < inboxBoundx) {
        nballx = inboxBoundx;
        ballvx = -ballvx;
    }
    if (nbally > boxBoundy) {
        nbally = boxBoundy;
        ballvy =-ballvy;
    }
    if (nbally < inboxBoundy) {
        nbally = inboxBoundy;
        ballvy = -ballvy;
    }
    ballx = nballx;
    bally = nbally;
}
```

```javascript
function change() {
    ballvx = Number(document.f.hv.value);
    ballvy = Number(document.f.vv.value);
    return false;
}
```

`</script>`
`</head>`

## 表 3-4（续）

### 代码

### 说明

```html
<body onLoad="init();">
<canvas id="canvas" width="400" height="300">
您的浏览器不支持 HTML5 的 canvas 元素。
</canvas>
<br/>
<form name="f" id="f" onSubmit="return change();">
水平速度 <input name="hv" id="hv" value="4" type="number" min="-10" max="10" />
<br>
垂直速度 <input name="vv" id="vv" value="8" type="number" min="-10" max="10"/>
<input type="submit" value="更改"/>
</form>
</body>
</html>
```

第四个应用是弹跳棉花糖游戏。我做的第一件事超出了 HTML/JavaScript/CSS 编程的范围。我使用了在线工具 `pixlr` 来提取棉花糖原图的一部分，并用另一张图片填补了缺失的部分。

我不会在这里给出棉花糖游戏的完整代码，仅指出新增部分。请参见表 3-5。

# 第 3 章 弹跳球

## 表 3-5 棉花糖游戏代码

```html
<style>
...
img {visibility: hidden;}
```

将任何 `img` 元素设置为不可见。这两个 `img` 元素将不会被显示。不过，加载的图像文件将通过 `drawImage` 绘制在画布上。

```html
</style>
```

```javascript
<script type="text/javascript">
...
var bkg = new Image();
```

`bkg` 是一个全局变量，用于保存一个 `Image` 对象。

```javascript
var stoppedx = ballvx;
```

该变量将被 `stopcc` 修改。

```javascript
var stoppedy = ballvy;
```

该变量将被 `stopcc` 修改。

```javascript
function init(){
...
bkg.src = "reunion.jpg";
```

设置这两个 `Image` 对象的 `src` 属性值。

```javascript
ball.src = "candy.png";
...
}
...
```

```javascript
function stopcc() {
```

`stopcc` 函数的头部。

```javascript
clearInterval(tid);
```

停止定时器事件。

```javascript
stoppedx = ballvx;
```

保存当前的 `ballvx` 值。

```javascript
stoppedy = ballvy;
```

保存当前的 `ballvy` 值。

```javascript
moveBall();
```

调用 `moveBall` 来显示当前场景。这一步有时是多余的。

```javascript
return false;
```

返回 `false` 以防止页面重新加载。

```javascript
}
```

关闭 `stopcc` 函数。

## 表 3-5（续）

```javascript
function resume(){
```

`resume` 函数的头部。

```javascript
clearInterval(tid);
```

停止定时器事件。

```javascript
ballvx = stoppedx;
```

将 `ballvx` 设置为 `stoppedx` 的值。在大多数情况下，这将是 `stopcc` 中设置的值。

```javascript
ballvy = stoppedy;
```

将 `ballvy` 设置为 `stoppedy` 的值。在大多数情况下，这将是 `stopcc` 中设置的值。

```javascript
tid = setInterval
(moveBall,100);
```

启动定时器事件。

```javascript
return false;
```

返回 `false` 以防止页面重新加载。

```javascript
}
```

关闭 `resume` 函数。

```html
</script>
</head>
```

```html
<body onload="init();">
```

`body` 标签。注意：当所有元素（包括 `<img>` 标签中提到的图像文件）加载完成时，会调用 `init` 函数。

```html
...
<form name="f" id="f" onSubmit="return change();">
...
<button onClick="return stopcc();">停止 </button>
```

用于调用 `stopcc` 的按钮。注意使用 `&nbsp;` 来定位下一个按钮。

```html
&nbsp; &nbsp;
<button onClick="return resume();">继续 </button>
```

用于调用 `resume` 的按钮。

```html
</form>
```

# 第 3 章 弹跳球

**表 3-5.** (续)

`<img src="candy.png" />`

一个 `img` 标签，用于在发生任何操作之前完全加载 `candy.png` 文件。

`<img src="reunion.jpg" />`

一个 `img` 标签，用于在发生任何操作之前完全加载 `reunion.jpg` 文件。

```
</body>
</html>
```

最后，表 3-6 描述了弹跳视频中的函数。

**表 3-6.** 弹跳视频的函数

| **函数** | **被谁调用** | **调用谁** |
|---|---|---|
| `init` | `body` 标签中 `onLoad` 的动作 | `startV` |
| `startV` | “点击此处开始”按钮的事件处理调用 | `moveball` |
| `moveball` | 由 `startV` 直接调用，并由 `setInterval` 动作调用 | `moveandcheck` |
| `moveAndCheck` | 由 `moveball` 调用 | – |

请理解，`ended` 事件的事件处理会调用匿名（未命名）函数来重置视频的 `currentTime` 并播放视频。另请注意，我将 `moveBall` 和 `moveAndCheck` 保持为两个不同的函数，是为了遵循其他程序的示例，在这些程序中，`moveBall` 确实有其他任务要完成。

弹跳视频的代码如表 3-7. 所示。我将包含几乎所有代码，尽管其中大部分你应该已经很熟悉。

# 第 3 章 弹跳球

## 表 3-7. 弹跳视频的代码

| 代码 | 说明 |
| :--- | :--- |
| `<html>` | |
| `<head>` | |
| `<title>Bouncing Video</title>` | |
| `<style>` | |
| `#videoE {position: absolute; display: none; z-index: 1;}` | 设置视频位于图片之上。初始状态不显示。 |
| `#con {position: absolute;}` | 设置 `div` 容器的定位。 |
| `</style>` | |
| `<script type="text/javascript">` | |
| `var rightEdge;` | 虚拟盒子的右边缘。 |
| `var leftEdge;` | 左边缘。 |
| `var topEdge;` | 上边缘。 |
| `var botEdge;` | 下边缘。 |
| `var ballx = 250;` | 弹跳容器/视频的初始 x 坐标。 |
| `var bally = 260;` | 初始 y 坐标。 |
| `var v;` | 将保存对视频的引用。 |
| `var c;` | 将保存对 `div`（我称之为容器）的引用。 |
| `var img;` | 将保存对图片的引用。 |
| `var iWidth;` | 将保存图片的宽度。 |
| `var iHeight;` | 将保存图片的高度。 |
| `var vWidth;` | 将保存视频的宽度。 |
| `var vHeight;` | 将保存视频的高度。 |
| `var ballvx = 14;` | 水平坐标的初始变化量。 |
| `var ballvy = 18;` | 垂直坐标的初始变化量。 |
| `function init(){` | `init` 函数的头部。 |
| `v = document.getElementById("videoE");` | 获取视频对象的指针/引用。 |
| `c = document.getElementById("con");` | 作为视频容器的 `div` 对象。 |
| `img = document.getElementById("AandF");` | 填充 `div` 对象的图片。 |
| `iwidth = img.clientWidth;` | 设置图片的宽度。 |
| `iheight = img.clientHeight;` | 设置图片的高度。 |
| `vwidth = v.videoWidth;` | 设置视频的宽度。 |
| `vheight = v.videoHeight;` | 设置视频的高度。 |
| `leftEdge = 5;` | 将 `leftEdge` 设置为离实际边缘稍远一点的位置。 |
| `rightEdge = leftEdge+iwidth-.6*vwidth;` | 设置 `rightEdge` 以使弹跳迅速发生。 |
| `topEdge = 5;` | 将 `topEdge` 设置为离实际边缘稍远一点的位置。 |
| `botEdge = topEdge+iheight-.6*vheight;` | 设置 `botEdge` 以使弹跳迅速发生。 |
| `}` | 关闭 `init`。 |
| `function startV(){` | `startV` 的头部。 |
| `v.play();` | 开始播放视频。 |
| `v.style.display= "block";` | 使视频可见。 |
| `c.style.top = bally +"px";` | 设置初始 x 坐标。 |
| `c.style.left = ballx + "px";` | 设置 y 坐标。 |
| `v.addEventListener('ended', function(){` | 为视频结束设置事件处理，使用匿名函数。 |
| `v.currentTime = 0;` | 将 `currentTime` 设置为 0，即起点。 |
| `v.play();` | 开始播放视频。 |
| `});` | 关闭匿名函数的定义，并关闭 `addEventListener` 调用。 |
| `moveball();` | 调用 `moveball`。 |
| `setInterval(moveball,100);` | 使用 `setInterval` 函数重复调用 `moveball`。 |
| `}` | 关闭 `startV`。 |
| `function moveBall(){` | `moveBall` 的头部。 |
| `moveAndCheck();` | 调用 `moveAndCheck`。 |
| `}` | 关闭 `moveBall`。 |
| `function moveAndCheck() {` | `moveAndCheck` 的头部。 |
| `var nballx = ballx + ballvx;` | 计算移动对象的可能下一个 x 值。 |
| `var nbally = bally + ballvy;` | 计算 y 值。 |
| 现在开始对每个边缘进行检测。 | |
| `if (nballx < leftEdge) {` | 如果对象位于左边缘的左侧。 |
| `ballvx = -ballvx;` | 反转 `ballvx` 的符号。 |
| `nballx = leftEdge;` | 将下一个 x 位置设置为 `leftEdge`。 |
| `}` | 关闭 if 语句。 |
| `if (nballx > rightEdge) {` | 如果对象位于右边缘的右侧。 |
| `nballx = rightEdge;` | 将下一个 x 设置为 `rightEdge`。 |
| `ballvx = -ballvx;` | 反转 `ballvx` 的符号。 |
| `}` | 关闭 if 语句。 |
| `if (nbally > botEdge) {` | 如果对象位于底边缘的下方。 |
| `nbally = botEdge;` | 将下一个 y 位置设置为 `botEdge`。 |
| `ballvy = -ballvy;` | 反转 `ballvy` 的符号。 |
| `}` | 关闭 if 语句。 |
| `if (nbally < topEdge) {` | 如果对象位于顶边缘的上方。 |
| `nbally = topEdge;` | 将下一个位置设置为 `topEdge`。 |
| `ballvy = -ballvy;` | 反转 `ballvy` 的符号。 |
| `}` | 关闭 if 语句。 |
| `ballx = nballx;` | 现在设置 `ballx`。 |
| `bally = nbally;` | 设置 `bally`。 |
| `c.style.top = bally + "px";` | 使用 px 设置 top 属性。 |
| `c.style.left = ballx + "px";` | 使用 px 设置 left 属性。 |
| `}` | 关闭 `moveAndCheck` 函数。 |
| `</script>` | 关闭 script 元素。 |
| `</head>` | 关闭 head 元素。 |
| `<body onLoad="init();">` | 开始 body。在 body 标签中设置对 `init` 的调用。 |
| `<img id="AandF" src="readers.jpg" width="auto" height="100%"/>` | 设置图像，指定 ID 和源。将宽度设为自动，高度设为 100%，使其在无失真的情况下适应屏幕。右侧可能会留有空白空间。 |
| `<div id="con" width="300">` | 定义一个 div 作为容器。将其宽度设置为与视频匹配。 |
| `<video controls width="300" id="videoE">` | 设置视频并指定 ID。注意：控件存在但难以使用。 |
| `<source src="talk.theora.ogv" type="video/ogg" />` | 提供了三个视频剪辑。它们建议用于不同的浏览器。 |
| `<source src="talk.mp4video.mp4" type="video/mp4" />` | 第二个视频源。 |
| `<source src="talk.webvmp8.webm" type="video/webm" />` | 第三个视频源。 |


`Sorry, your browser doesn't support embedded videos.` | 将根据需要显示错误消息。

`</video>` | 关闭 `video` 元素。

`</div>` | 关闭 `div` 元素。

`<button onclick="startV()">Click to start</button>` | 提供用户交互的按钮。这将调用`startV()`。

`</body>` | 关闭 `body` 元素。

`</html>` | 关闭 `html` 元素。

有很多方法可以制作类似的应用程序。你可以为球选择自己的图像，并尝试使用墙壁的颜色（带或不带渐变）。你可以更改每面墙壁的位置和尺寸。你可以向页面添加文本和 HTML 标记。你可以更改表单的外观。你可以将第一个弹跳应用程序中的表单和其他功能添加到弹跳视频中。当然，你可以添加自己的视频和图像作为背景。一个有用的补充是提供一种访问控件的方式，而不是让它们在屏幕上四处移动。

你可以包含多个球，并跟踪每个球的位置。如果决定使用两个球，则需要两组变量，并且之前的一行代码现在需要两行。一种系统化的方法是使用编辑器中的搜索功能查找所有`ball`的实例，对于每一行，替换为两行。因此，在`ballx`的位置，你将使用`ball1x`和`ball2x`；在`var ballx = 50;`的位置，使用以下代码：

```
var ball1x = 50;
var ball2x = 250;
```

这会将第二个球放置在画布上向右 200 像素的位置。

你还需要一组用于墙壁的所有比较逻辑。

### 第 3 章 弹跳球

如果你想使用两个以上的球，建议考虑使用数组。后续章节将向你展示如何处理对象集合。

你也可以尝试编写代码，让球每次撞墙时速度都减慢。这是一种很酷的效果，且能模拟真实的物理结果。在代码中所有通过改变变量符号来改变方向的位置，添加一个因子来减小绝对值。例如，如果选择将值减小 10%，可以编写如下代码：

```
if (nballx > boxBoundx) {
  ballvx =-ballvx *.9;
  nballx = boxBoundx;
}
```

这意味着垂直方向的增量变化将降至原来的 90%。

你可以通过使用自己的照片并让游戏更具“游戏感”，在棉花糖游戏的基础上进行扩展或从中汲取灵感。思考一下如何判定“落点位置”是否足够好。限制暂停和继续操作的次数。研究本部分其余内容（以及继续阅读 *《HTML5 与 JavaScript 项目》* 一书）中的示例，学习其他操作，例如鼠标或触摸的使用。

#### 测试并上传应用程序

第一个和第三个应用程序可以在 HTML 文档中完整实现。第二个和第四个应用程序需要将图像文件放在同一文件夹中，第五个则需要图像和视频文件。你可以访问网络上的任何文件，但必须确保包含正确的地址。例如，如果将 HTML 文档上传到名为`myGames`的文件夹，并将`pearl.jpg`上传到`myGames`的子文件夹`images`中，则相应的代码行必须为：

`img.src = "images/pearl.jpg";`

同时，你必须使用准确的文件扩展名，例如`.JPG`，以指明正确的文件类型。有些浏览器会容错，但很多不会。你可以尝试提交错误数据，查看不同浏览器的响应。但无论如何，你都应尊重知识产权，不得使用未经许可的图像或视频。


# 第 3 章 弹跳球

## 本章小结

在本章中，你学习了如何创建一个基于用户输入而变化的动画应用程序。我们涵盖了许多编程和 HTML5 特性，包括以下内容：

- 使用`setInterval`设置动画的定时事件，并使用`clearInterval`停止该事件
- 验证表单输入
- 使用程序员定义的函数来水平和垂直地重新定位圆形或图像，以模拟弹跳球
- 测试虚拟碰撞
- 绘制矩形、图像和圆形，包括渐变色填充
- 使用按钮元素
- 确保图像文件的下载
- 移动视频元素
- 启动视频以满足用户交互的要求
- 重新启动视频

下一章将描述炮弹和弹弓游戏，玩家在这些游戏中需要尝试击中目标。这些应用使用了与我们制作动画相同的编程和 HTML5 特性，但将它们向前推进了一步。你还可以在第[8](http://dx.doi.org/10.1007/978-1-4842-4155-4_8)章的石头剪刀布实现中看到一个动画示例。
