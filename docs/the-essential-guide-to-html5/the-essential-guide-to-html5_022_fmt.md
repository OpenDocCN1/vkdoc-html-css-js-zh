# 第 3 章 弹跳球

## 设置定时事件

在 `HTML5` 中设置定时事件实际上与旧版 `HTML` 中的方式类似。有两个内置函数：`setInterval` 和 `setTimeout`。我们将在本章介绍 `setInterval`，而在第 5 章的记忆游戏中使用 `setTimeout`。这两个函数都接受两个参数。请记住，参数是包含在函数或方法调用中的额外信息。在第 1 章中，我们看到 `document.write` 将需要输出到屏幕的内容作为其唯一参数。

我先描述第二个参数。第二个参数指定时间量，单位是毫秒。1 秒等于 1000 毫秒。这看起来是一个非常短的时间单位，但事实证明这正是我们游戏开发所需要的。对于计算机游戏来说，一秒（1000 毫秒）相当长。

第一个参数指定在第二个参数指定的时间间隔内要执行的操作。第一个参数可以是一个函数的名称。在本应用程序中，`init` 函数定义包含以下代码行：

```javascript
setInterval(moveBall,100);
```

这告诉 `JavaScript` 引擎每 100 毫秒（每秒 10 次）调用一次 `moveBall` 函数。`moveBall` 是一个函数的名称，该函数将在此 `HTML` 文档中定义；它是`定时间隔事件`的`事件处理程序`。

如果您在编写函数定义代码之前就编写了这行代码，请不要担心。重要的是在应用程序运行时存在的内容。

`JavaScript` 还提供了除函数名称之外的方式来指定事件处理程序。您也可以编写以下代码，效果相同：

```javascript
setInterval("moveBall();",100);
```

换句话说，对于操作是无参数函数调用的情况，使用函数名称即可。对于更复杂的情况，您可以编写字符串来指定代码。假设我有一个名为 `slide` 的函数，它本身接受一个参数，我希望该参数是变量 `d` 的 10 倍，并且我希望每 1.5 秒执行一次，那么我会这样编写代码：

```javascript
setInterval("slide(10*d);",1500);
```

我注意到 `moveBall` 不需要参数的原因在于，我们使用了全局变量来表示位置和位移。

## 屏幕上的时间显示示例

通常，您希望在屏幕上指示时间的流逝。以下示例将显示 0、1……等数字，并且每秒更改一次。

```html
<html>
<head>
<title>elapsed</title>
<script>
function init() {
    setInterval(increase,1000);
}
function increase() {
    document.f.secs.value = String(1+Number(document.f.secs.value));
}
</script>
</head>
<body onLoad="init();">
<form name="f">
    <input type="text" name="secs" value="0"/>
</form>
</body>
</html>
```

这是一个很好的示例，值得您花时间编写并运行，既因为它展示了定时事件，也因为它会让您体会到一秒有多长。代码从名为 `f` 的表单中获取 `secs` 输入字段的值，将该值转换为数字，加 1，然后再将其转换回字符串，赋值为 `secs` 元素的值。尝试将 `increase` 函数中的单个语句替换为以下语句，看看会发生什么：

```javascript
document.f.secs.value = 1+document.f.secs.value;
```

这是数字与字符串之间区别的一课。请随意尝试这个小例子。如果您希望数字以更小的增量递增，可以将 `1000` 改为 `250`，将 `1` 改为 `.25`。这会使脚本显示四分之一秒的变化。

## 停止定时事件

如果您希望代码能够停止特定事件，可以设置一个全局变量（位于任何函数外部）。我使用名为 `tev` 的变量，它是定时事件的简写。

```javascript
var tev;
```

然后，您可以将 `setInterval` 调用修改为如下所示：

```javascript
tev = setInterval(moveBall,100);
```

当您想要停止此事件时，请包含以下代码：

```javascript
clearInterval(tev);
```

顺便提一下，如果我的代码在未调用 `clearInterval` 的情况下再次执行包含 `setInterval` 函数的语句，这相当于设置了另一个闹钟。其效果是增加执行速度。在描述棉花糖游戏时，您会注意到我的代码包含了多个 `clearInterval` 语句。

重申一下，`setInterval` 函数设置了一个持续发生直到被清除的定时事件。如果您知道希望事件只发生一次，`setTimeout` 方法正好设置一个事件。您可以使用任何一种方法产生相同的结果，但 `JavaScript` 提供这两种方法是为了方便。

对于弹跳球应用程序，`moveBall` 函数计算球的新位置，进行碰撞检测计算，并在碰撞发生时重定向球并绘制新画面。这反复进行——因为我们使用了 `setInterval`，所以对 `moveBall` 的调用持续进行。

## 计算新位置与碰撞检测

现在我们已经知道如何绘制、如何清除并重绘，以及如何以固定间隔执行操作，挑战在于如何计算新位置以及如何进行碰撞检测。我们将通过声明以下变量来实现：`ballx` 和 `bally` 用于保存球心的 x 和 y 坐标；`ballvx` 和 `ballvy` 用于保存球位置要改变的量；`boxBoundx`、`inboxBoundx`、`boxBoundy` 和 `inboxBoundy` 用于指示一个比实际箱子略小的边界，用于碰撞计算。球位置要改变的量初始化为 4 和 8（完全任意选择），并在玩家做出有效更改（请参阅下一节）并点击更改按钮时改变。这些量被称为`位移`或`增量`，通俗地说，也称为`速度`。

在这种情况下，方向改变非常简单。如果球“撞到”垂直墙，水平位移必须改变符号；即，如果球向右移动四个单位并且撞到墙，我们希望开始向它的位置添加 `-4`，这将使其向左移动。垂直位移保持不变。通过将*下一个*水平值与边界进行比较来确定是否撞墙。类似地，如果通过将垂直位置与适当的边界进行比较确定球“撞到”水平墙，则垂直位移改变符号，而水平位移保持不变。此改变用于下一次迭代。碰撞检查执行四次，即对四面墙各检查一次。计算包括将建议的新 `x` 或 `y` 值（视情况而定）与特定墙壁的边界条件进行比较。如果球中心越过四面墙中的一面，则调整暂定新位置，使其恰好位于边界上。这会产生使球稍微穿过每面墙或看起来被每面墙挤压的效果。边界值被设置为刚好位于盒子内部，盒子的左上角位于 `boxx`、`boxy`，宽度为 `boxWidth`，高度为 `boxHeight`。我可以使用更复杂的计算来比较圆上的任何点与墙上的任何点。然而，这里涉及一个更基本的原理。没有墙，也没有球。这是一个基于计算的模拟。

计算按时间间隔进行。如果球移动得足够快，并且墙壁足够薄，薄于这里指定的 `ballrad`，球可能会逃出盒子。这就是我根据下一步移动和一个稍小的盒子进行计算的原因。

```javascript
var boxBoundx = boxWidth+boxx-ballrad;
var boxBoundy = boxHeight+boxy-ballrad;
var inboxBoundx = boxx+ballrad;
var inboxBoundy = boxy+ballrad;
```

以下是 `moveAndCheck` 函数的代码，该函数用于检查碰撞并重新定位球：

```javascript
function moveAndCheck() {
    var nballx = ballx + ballvx;
    var nbally = bally + ballvy;
    if (nballx > boxBoundx) {
        ballvx =-ballvx;
        nballx = boxBoundx;
    }
    if (nballx < inboxBoundx) {
        nballx = inboxBoundx
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

你可能会说这里实际上没发生什么，你说得对。变量 `ballx` 和 `bally` 被修改以供稍后在绘制到画布时使用。

从这段代码中并不明显，但请记住，垂直值（`y` 值）在屏幕上向下递增，水平值（`x` 值）从左向右递增。

`moveAndCheck` 函数在弹跳视频中略有不同。我是“从头开始”编写的，它为读者提供了一种略微——非常略微——不同的方法。它确实提供了理由，告诉您如何确定特定类型对象的宽度和高度。以下是 `init` 函数的代码：

```
function init(){
    v = document.getElementById("videoE");
    c = document.getElementById("con");
    img = document.getElementById("AandF");
    iwidth = img.clientWidth;
    iheight = img.clientHeight;
    vwidth = v.videoWidth;
    vheight = v.videoHeight;
    leftEdge = 5; //arbitrary margin
    rightEdge = leftEdge+iwidth-.6*vwidth;
    topEdge = 5; //arbitrary margin
    botEdge = topEdge+iheight-.6*vheight;
}
```

使用“Edge”变量，`moveAndCheck` 函数如下所示：

```
function moveAndCheck() {
    var nballx = ballx + ballvx;
    var nbally = bally + ballvy;
    if (nballx < leftEdge) {
        ballvx =-ballvx;
        nballx = leftEdge;
    }
    if (nballx> rightEdge) {
        nballx = rightEdge;
        ballvx = -ballvx;
    }
    if (nbally > botEdge) {
        nbally = botEdge;
        ballvy =-ballvy;
    }
    if (nbally < topEdge) {
        nbally = topEdge;
        ballvy = -ballvy;
    }
    ballx = nballx;
    bally = nbally;
    c.style.top=bally+"px";
    c.style.left=ballx+"px";
}
```