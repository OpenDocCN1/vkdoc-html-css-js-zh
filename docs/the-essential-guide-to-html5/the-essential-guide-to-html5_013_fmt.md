# 排版后的内容

如果你希望形状既有填充色又有清晰的轮廓线，就需要同时使用 `fill` 和 `stroke` 命令，并通过 `fillStyle` 和 `strokeStyle` 属性指定不同的颜色。配色方案基于第 [1](http://dx.doi.org/10.1007/978-1-4842-4155-4_1) 章中介绍的相同红/绿/蓝编码。你可以自行尝试，或使用 Photoshop 等工具以及在线照片编辑器 [pixlr.com](http://pixlr.com) 来获取所需的颜色。以下是完整代码：

```html
<html>
<head>
<title>Smile</title>
<script type="text/javascript">
function init() {
    var ctx = document.getElementById("canvas").getContext('2d');
    ctx.beginPath();
    ctx.strokeStyle = "rgb(200,0,0)";
    ctx.arc(200, 200, 50, 0, Math.PI, false);
    ctx.fillStyle = "rgb(200,0,200)";
    ctx.closePath();
    ctx.fill();
    ctx.strokeStyle = "rgb(255,0,0)";
    ctx.lineWidth = 5;
    ctx.stroke();
}
</script>
</head>
<body onLoad="init();">
<canvas id="canvas" width="400" height="300"> Your browser doesn't support the HTML5 element canvas.
</canvas>
</body>
</html>
```

![](img/index-66_1.png)

这段代码绘制了一个用紫色（红色和蓝色的组合）填充的半圆形，并带有一条纯红色的轮廓线（即描边），如图 2-10 所示。编码过程是先指定路径，然后以填充方式绘制路径，再以描边方式绘制路径。

*图 2-10. 使用不同颜色进行填充和描边*

通过多种不同的命令可以绘制出完整的圆形，包括以下命令：

```
ctx.arc(200,200,50,0, 2*Math.PI, true);
ctx.arc(200,200,50, 0, 2*Math.PI, false);
ctx.arc(200,200,50, .5*Math.PI, 2.5*Math.PI, false);
```

你完全可以坚持使用第一个命令——它和其他命令一样好用。请注意，我仍然使用了 `closePath` 命令。从几何学角度讲，圆形是一个闭合图形，但在 JavaScript 中这一点并不重要。

如果你将 `canvas` 元素想象成一块画布，在上面涂抹墨水或颜料，那么要绘制新内容时，就需要擦除画布或画布上的相应部分。为此，HTML5 提供了以下命令：

```
ctx.clearRect(x,y,width,height);
```

后续示例将展示如何绘制弹弓（第 [4](http://dx.doi.org/10.1007/978-1-4842-4155-4_4) 章）、记忆/配对游戏的多边形（第 [5](http://dx.doi.org/10.1007/978-1-4842-4155-4_5) 章）、迷宫墙壁（第 [7](http://dx.doi.org/10.1007/978-1-4842-4155-4_7) 章）以及吊死鬼游戏中的火柴人（第 [9](http://dx.doi.org/10.1007/978-1-4842-4155-4_9) 章）。现在，让我们回到掷骰子游戏所需的内容上。