# 第 2 章 骰子游戏

#### 表 2-4（续）

| 代码 | 说明 |
|------|------|
| `Call the draw4 function and then.` | 调用`draw4`函数，然后执行。 |
| `draw2mid();` | 调用`draw2mid`函数。 |
| `break;` | 跳出`switch`语句（并非严格必要）。 |
| `}` | 关闭`switch`语句。 |
| `}` | 关闭`drawface`函数。 |
| `function draw1() {` | 开始定义`draw1`。 |
| *(续)* | |
| `var dotx;` | 用于绘制单个圆点的水平位置的变量。 |
| `var doty;` | 用于绘制单个圆点的垂直位置的变量。 |
| `ctx.beginPath();` | 开始一条路径。 |
| `dotx = dx + .5*diceWidth;` | 将该圆点中心设在骰子面的水平中心（使用`dx`）... |
| `doty = dy + .5*diceHeight;` | ……以及垂直中心（使用`dy`）。 |
| `ctx.arc(dotx, doty, dotrad, 0, Math.PI*2, true);` | 构造一个圆形（通过`fill`命令绘制）。 |
| `ctx.closePath();` | 关闭路径。 |
| `ctx.fill();` | 绘制路径，即圆形。 |
| `}` | 关闭`draw1`。 |
| `function draw2() {` | 开始定义`draw2`函数。 |
| `var dotx;` | 用于绘制两个圆点的水平位置的变量。 |
| `var doty;` | 用于绘制两个圆点的垂直位置的变量。 |
| `ctx.beginPath();` | 开始一条路径。 |
| `dotx = dx + 3*dotrad;` | 在骰子面左上角向内水平方向，将圆点圆心设在 3 倍半径处…… |
| `doty = dy + 3*dotrad;` | ……以及垂直方向。 |
| `ctx.arc(dotx, doty, dotrad, 0, Math.PI*2, true);` | 构造第一个圆点。 |
| *(续)* | |
| `dotx = dx+diceWidth-3*dotrad;` | 在骰子面右下角向内水平方向，将圆点圆心设在 3 倍半径处…… |
| `doty = dy+diceHeight-3*dotrad;` | ……以及垂直方向。 |
| `ctx.arc(dotx, doty, dotrad, 0, Math.PI*2, true);` | 构造第二个圆点。 |
| `ctx.closePath();` | 关闭路径。 |
| `ctx.fill();` | 绘制两个圆点。 |
| `}` | 关闭`draw2`。 |
| `function draw4() {` | 开始定义`draw4`函数。 |
| `draw2();` | 调用`draw2`函数。 |
| `var dotx;` | 用于绘制圆点的水平位置的变量。 |
| `var doty;` | 用于绘制圆点的垂直位置的变量。 |
| `ctx.beginPath();` | 开始路径。 |
| `dotx = dx + 3*dotrad;` | 将该圆点置于左下角内侧，水平方向…… |
| `doty = dy + diceHeight-3*dotrad;` | ……以及垂直方向。 |
| `ctx.arc(dotx, doty, dotrad, 0, Math.PI*2, true);` | 构造圆形。 |
| `dotx = dx+diceWidth-3*dotrad;` | 将该圆点置于右上角内侧，水平方向…… |
| `doty = dy+3*dotrad;` | ……以及垂直方向。 |
| *(续)* | |
| `ctx.arc(dotx, doty, dotrad, 0, Math.PI*2, true);` | 构造圆形。 |
| `ctx.closePath();` | 关闭路径。 |
| `ctx.fill();` | 绘制两个圆点。 |
| `}` | 关闭`draw4`函数。 |
| `function draw2mid() {` | 开始定义`draw2mid`函数。 |
| `var dotx;` | 用于绘制两个圆点的水平位置的变量。 |
| `var doty;` | 用于绘制两个圆点的垂直位置的变量。 |
| `ctx.beginPath();` | 开始路径。 |
| `dotx = dx + 3*dotrad;` | 将圆点置于水平方向内侧…… |
| `doty = dy + .5*diceHeight;` | ……以及垂直方向中点。 |
| `ctx.arc(dotx, doty, dotrad, 0, Math.PI*2, true);` | 构造圆形。 |
| `dotx = dx+diceWidth-3*dotrad;` | 将该圆点置于右边界内侧。 |
| `doty = dy + .5*diceHeight;` | y 坐标设在中点（不变）。 |
| `ctx.arc(dotx, doty, dotrad, 0, Math.PI*2, true);` | 构造一个圆形。 |
| `ctx.closePath();` | 关闭路径。 |
| `ctx.fill();` | 绘制圆点。 |
| `}` | 关闭`draw2mid`函数。 |
| `</script>` | 关闭`script`元素。 |
| `</head>` | 关闭`head`元素。 |
| `<body>` | body 标签开始。 |
| *(续)* | |
| `<canvas id="canvas" width="400" height="300">` | Canvas 标签开始。 |
| `Your browser doesn't support the HTML5 element canvas.` | 设置一个画布，若浏览器不支持`canvas`元素则提供提示信息。 |
| `</canvas>` | 关闭`canvas`标签。 |
| `<br/>` | 换行。 |
| `<button onClick="throwDice();">Throw dice</button>` | 按钮元素（注意`onClick`属性设置为调用`throwDice`）。 |
| `</body>` | 关闭`body`标签。 |
| `</html>` | 关闭`html`标签。 |

#### 完整的 Craps 游戏



