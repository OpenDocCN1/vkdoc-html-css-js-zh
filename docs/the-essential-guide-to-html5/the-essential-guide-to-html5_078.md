# 第 8 章 石头、剪刀、布

**表 8-2.** （ *续*）

**代码** | **说明**
--------|-----------------
`messages.` | 
`["TIE: you both threw rock","You lose: computer threw rock"]`, | 电脑出石头时的一组消息。
`["You lose: computer threw paper","TIE: you both threw paper","You win: computer threw paper"]`, | 电脑出布时的一组消息。
`["You win: computer threw scissors","You lose: computer threw scissors","TIE: you both threw scissors"]];` | 电脑出剪刀时的一组消息。
`var points = [` | 声明数组起始，该数组保存分数增量：0 代表平局，1 代表玩家赢，-1 代表玩家输。
`[0,1,-1],` | 电脑出石头时的增量组。
`[-1,0,1],` | 电脑出布时的增量组。
`[1,-1,0]];` | 电脑出剪刀时的增量组。
`var inMotion = false;` | 用于防止在电脑动画进行过程中响应玩家的出招。
`function Throw(sx, sy, sMargin, sWidth, sHeight, rectColor, picture) {` | 用于三个游戏符号的构造函数头。参数包括 x 和 y 坐标、边距、内部宽度和高度、矩形颜色以及图片文件。
`this.sx = sx;` | 赋值 `sx` 属性。
`this.sy = sy;` | 赋值 `sy` 属性。
`this.sWidth = sWidth;` | 赋值 `sWidth` 属性。
`this.bWidth = sWidth + 2 * sMargin;` | 计算并赋值外部宽度。即内部宽度加上两倍的边距。
`this.bHeight = sHeight + 2 * sMargin;` | 计算并赋值外部高度。即内部高度加上两倍的边距。
`this.sHeight = sHeight;` | 赋值 `sHeight` 属性。
`this.fillStyle = rectColor;` | 赋值 `fillStyle` 属性。
`this.draw = drawThrow;` | 将绘制方法赋值为 `drawThrow`。
`this.img = new Image();` | 创建一个新的 `Image` 对象。
`this.img.src = picture;` | 将其 `src` 设置为图片文件。
`this.sMargin = sMargin;` | 赋值 `sMargin` 属性。绘制时仍需要它。
`}` | 关闭函数。
`function drawThrow() {` | 绘制符号的函数头。
`ctx.strokeStyle = "rgb(0,0,0)";` | 将矩形轮廓样式设置为黑色。
`ctx.strokeRect(this.sx, this.sy, this.bWidth, this.bHeight);` | 绘制矩形轮廓。
`ctx.fillStyle = this.fillStyle;` | 设置填充矩形的样式。
`ctx.fillRect(this.sx, this.sy, this.bWidth, this.bHeight);` | 绘制填充矩形。
`ctx.drawImage(this.img, this.sx + this.sMargin, this.sy + this.sMargin, this.sWidth, this.sHeight);` | 在矩形内部偏移绘制图像。
`}` | 关闭函数。
`function choose(ev) {` | 点击事件调用的函数头。
`if (!inMotion) {` | 仅在电脑动画未进行时响应。
`var compch = Math.floor(Math.random() * 3);` | 基于随机处理生成电脑的出招。
`var compchn = choices[compch];` | 挑选出图片文件。
`compimg.src = compchn;` | 设置已创建的 `Image` 对象的 `src`。
`var mx;` | 用于鼠标 x 坐标。
`var my;` | 用于鼠标 y 坐标。
`mx = ev.pageX;` | 设置 `mx`。
`my = ev.pageY;` | 设置 `my`。
`var i;` | 用于遍历不同符号的索引。
`for (i = 0; i < everything.length; i++) {` | 遍历 `everything` 数组（即三个符号）中元素的 for 头。
`var ch = everything[i];` | 获取第 i 个元素。
`if ((mx > ch.sx) && (mx < ch.sx + ch.bWidth) && (my > ch.sy) && (my < ch.sy + ch.bHeight)) {` | 检查 `mx`、`my` 位置是否在此符号的边界（外部矩形边界）内。
`drawAll();` | 如果是，则调用 `drawAll` 函数，该函数将清除所有内容，然后绘制 `everything` 数组中的所有内容。
`size = 15;` | 电脑出招图像的初始大小。
`tid = setInterval(flyin, 100);` | 设置定时事件。
`result = beats[compch][i];` | 设置结果消息。关于音频的补充内容请参见表格后的章节。
`newScore = Number(document.` |


```markdown
获取当前分数，并将其转换为数字。

`f.score.value)`;

`newScore += points[compch][i]`;

添加调整值并保存，以便稍后显示。

`break`;

离开`for`循环。

}

结束`if`子句。

}

结束`for`循环。

}

结束`inMotion`为 false 时的 true 分支。

}

结束函数。

`function flyin() {`

处理定时区间事件的函数头。

`inMotion = true`;

计算机移动出现。此处多次设置为 true。

`ctx.drawImage(compimg`,

在屏幕上的`70,100,size,size`位置绘制计算机移动图像；

指定的位置及指定的尺寸。

`size +=5`;

通过递增`size`来改变尺寸值。

`if (size>50) {`

使用`size`变量判断过程是否已持续足够长时间。

`clearInterval(tid)`;

停止定时事件。

`ctx.fillText(result`,

显示消息。

`200,100,250)`;

`document.f.score.value =`

显示新分数。关于音频的添加，请参见表格后的章节。

`String(newScore)`;

`inMotion = false`;

恢复为初始设置。

}

关闭`if`的 true 子句。

}

关闭函数。

( *续* )

### 第 8 章：石头、剪刀、布

***表 8-2.*** ( *续* )

| **代码** | **说明** |
|---|---|
| `var rockb = new Throw(rockbx,rockby,8,50,50,"rgb(250,0,0)","rock.jpg");` | 创建石头对象。 |
| `var paperb = new Throw(paperbx,paperby,8,50,50,"rgb(0,200,200)","paper.gif");` | 创建布对象。 |
| `var scib = new Throw(scissorsbx,scissorsby,8,50,50,"rgb(0,0,200)","scissors.jpg");` | 创建剪刀对象。 |
| `everything.push(rockb);` | 将石头对象添加到`everything`数组中。 |
| `everything.push(paperb);` | 将布对象添加到`everything`数组中。 |
| `everything.push(scib);` | 将剪刀对象添加到`everything`数组中。 |
| `function init(){` | 文档加载时调用的函数头。 |
| `document.f.score.value = "0";` | 将分数设置为零。我也可以使用`...= String(0);`（实际上这并非必要，因为 JavaScript 在此情况下会将数字转换为字符串）。 |
| `ctx = document.getElementById('canvas').getContext('2d');` | 设置用于所有绘图的变量。 |
| `canvas1 = document.getElementById('canvas');` | 设置用于鼠标点击事件处理的变量。 |
| `canvas1.addEventListener('click',choose,false);` | 设置点击事件处理。 |
| `drawAll();` | 绘制所有内容。 |

( *续* )

### 第 8 章：石头、剪刀、布

***表 8-2.*** ( *续* )

| **代码** | **说明** |
|---|---|
| `ctx.font="bold 16pt Georgia";` | 设置用于结果消息的字体。 |
| `ctx.fillStyle = "blue";` | 设置颜色。 |
| `}` | 关闭函数。 |
| `function drawAll() {` | 函数头。 |
| `ctx.clearRect(0,0,cWidth,cHeight);` | 清空画布。 |
| `var i;` | 用于索引的变量。 |
| `for (i=0;i<everything.length;i++) {` | 遍历`everything`数组。 |
| `everything[i].draw();` | 绘制各个元素。 |
| `}` | 关闭`for`循环。 |
| `}` | 关闭函数。 |
| `</script>` | 关闭脚本元素。 |
| `</head>` | 关闭 head 元素。 |
| `<body onLoad="init();">` | body 标签起始。设置对`init`函数的调用。 |
| `<canvas id="canvas" width="600" height= "400">` | canvas 标签起始。 |
| `Your browser doesn't support the HTML5 element canvas.` | 针对不兼容浏览器的消息。 |
| `</canvas>` | 关闭标签。 |
| `<br/>` | 换行。 |
| `<form name="f">` | 表单起始标签，赋予表单名称。 |
| `Score: <input name="score" value="0" size="3"/>` | 标签后接输入字段，包含初始值和尺寸。 |
| `</form>` | 表单关闭标签。 |
| `</body>` | body 关闭标签。 |
| `</html>` | HTML 文档关闭标签。 |

音频增强版本需要额外三个全局变量，并在`init`、`choose`和`flyin`函数中进行添加。新增的全局变量如下：

```javascript
var music = [
    [3,1,0],
    [1,3,2],
    [0,2,3]
];
var musicelements;
var musicch;
```

`init`函数需要以下语句：

`musicElements = document.getElementsByTagName("audio");`

文档方法`getElementsByTagName`会生成一个包含文档中所有音频元素的数组，这正是`musicelements`所需要的。

以下是`choose`函数中包含新突出显示行的子句：

```javascript
if ((mx>ch.sx)&&(mx<ch.sx+ch.bWidth)&&(my>ch.sy)&&(my<ch.sy+ch.bHeight)) {
    drawAll();
    size = 15;
    tid = setInterval(flyin,100);
    result = beats[compch][i];
```
```


