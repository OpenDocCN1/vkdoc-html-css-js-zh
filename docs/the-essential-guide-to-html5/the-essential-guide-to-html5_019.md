# 第 2 章 骰子游戏

***表 2-5.*** （*续*）

**代码**

**说明**

```javascript
draw1();
break;
case 6:
draw4();
draw2mid();
break;
}

function draw1() {
var dotx;
var doty;
ctx.beginPath();
dotx = dx + .5*dicewidth;
doty = dy + .5*diceheight;
ctx.arc(dotx,doty,dotrad,0,
Math.PI*2,true);
ctx.closePath();
ctx.fill();
}

function draw2() {
var dotx ;
var doty;
ctx.beginPath();
dotx = dx + 3*dotrad;
doty = dy + 3*dotrad;
ctx.arc(dotx,doty,dotrad,0,Math.
PI*2,true);
dotx = dx+dicewidth-3*dotrad;
doty = dy+diceheight-3*dotrad ;
ctx.arc(dotx,doty,dotrad,0,Math.
PI*2,true);
ctx.closePath();
ctx.fill();
}

function draw4() {
draw2();
var dotx;
var doty;
ctx.beginPath();
dotx = dx + 3*dotrad;
doty = dy + diceheight-3*dotrad;
ctx.arc(dotx,doty,dotrad,0,Math.
PI*2,true);
dotx = dx+dicewidth-3*dotrad;
doty = dy+ 3*dotrad;
ctx.arc(dotx,doty,dotrad,0,Math.
PI*2,true);
ctx.closePath();
ctx.fill() ;
}

function draw2mid() {
var dotx;
var doty ;
ctx.beginPath();
dotx = dx + 3*dotrad;
doty = dy + .5*diceheight;
ctx.arc(dotx,doty,dotrad,0,Math.
PI*2,true);
dotx = dx+dicewidth-3*dotrad;
doty = dy + .5*diceheight;
ctx.arc(dotx,doty,dotrad,0,Math.
PI*2,true);
ctx.closePath();
ctx.fill();
}
```

```html
<script>
</head>
<body>
<canvas id="canvas" width="400" height="300">
Your browser doesn't support the HTML5 element canvas.
</canvas>
<br/>
<button onClick="throwdice();">Throw dice</button>
<form name="f">
Start a form named f.
Stage: <input name="stage" value="First Throw"/>
With the text Stage: right before it, set up an input field named stage.
Point: <input name="pv" value=" "/>
With the text Point: right before it, set up an input field named pv.
Outcome: <input name="outcome" value=" "/>
With the text Outcome: right before it, set up an input field named outcome.
</form>
Close the form.
</body>
Close body.
</html>
Close html.
```

#### 让应用程序个性化

让这个应用程序个性化并不像收藏站点应用程序那样直接，因为花旗骰的规则就是规则本身。如果你不想改变规则，仍然有很多事情可以做。使用`fillRect`并设置`fillStyle`为不同颜色，改变骰子面的大小和颜色。改变整个画布的颜色和大小。将结果文本改为更生动的描述。你也可以使用标准或特制的骰子实现其他游戏。

你可以提前阅读下一章，学习如何在画布上绘制图像，而不是使用弧线和矩形来绘制每个骰子面。HTML5 提供了一种引入外部图像文件的方法。这种方法的缺点是你必须管理这些单独的文件。

你可以开发计分代码。对于赌博游戏，你可以让玩家以固定金额开始，比如 100 个货币单位，每玩一局扣除一些金额，比如 10 个，并且仅在玩家获胜时增加一些金额，比如 20 个。你可以将此赌注信息作为`<form>`元素的一部分添加到`<body>`中。

## 第 2 章 骰子游戏

```html
<form name="f" id="f">
Stage: <input name="stage" value="First Throw"/>
Point: <input name="pv" value=" "/>
Outcome: <input name="outcome" value=" "/>
Bank roll: <input name="bank" value="100"/>
</form>
```

JavaScript（以及其他编程语言）区分数字和表示数字的字符串。也就是说，值`"100"`是一个字符串，由字符 1、0 和 0 组成；而值`100`是一个数字。无论哪种情况，变量的值都存储为 1 和 0 的序列。对于数字，这将是表示为二进制数的数字。对于字符串，每个字符将使用标准编码系统（如 ASCII 或 Unicode）表示。在某些情况下，JavaScript 会自动从一种数据类型转换为另一种，但不要依赖这种行为。我建议的编码方式使用内置函数`String`和`Number`来进行这些转换。

在`throwDice`函数中，在`if(firstTurn)`语句之前，添加表 2-6（或类似内容）中的代码。

***表 2-6.** 为玩家添加赌注*

**代码**

**说明**

```javascript
var bank = Number(document.f.bank.value);
```
设置一个新变量`bank`，其值为`bank`输入字段中表示的数字。

```javascript
if (bank<10) {
```
将`bank`与 10 进行比较。

```javascript
alert("You ran out of money! Add some more and try again.");
```
如果`bank`小于 10，则弹出一个警告。

```javascript
return;
```
退出函数，不执行任何操作。

```javascript
}
```
关闭`if`真分支。

```javascript
bank = bank – 10;
```
将`bank`减少 10。仅当`bank`大于 10 时才会执行这一行。

```javascript
document.f.bank.value = String(bank);
```
将该值的字符串表示放入`bank`字段。

## 第 2 章 骰子游戏

然后，在玩家获胜的每个位置（首次投掷中 7 点和 11 点之后的`switch`语句，或者后续投掷中，点数情况之后的`switch`语句），添加表 2-7 中的代码。

***表 2-7.** 增加赌注的值*

**代码**

**说明**

```javascript
bank = Number(document.f.bank.value);
```
将`bank`设置为`bank`输入字段中表示的数字。再次设置`bank`允许玩家在游戏过程中重置赌注金额。

```javascript
bank +=20;
```
使用`+=`运算符将`bank`的值增加 20。

```javascript
document.f.bank.value = String(bank);
```
将赌注金额的字符串表示放入`bank`字段。

当玩家输掉游戏或处于后续轮次时，不需要添加任何代码。每次新游戏开始前，赌注值会减少。

#### 测试和上传应用程序

这些应用程序完整地包含在 HTML 文件中。没有使用其他文件（如图像文件）。相反，骰子面是在画布上绘制的。（作为参考，我使用较旧 HTML 编写的骰子游戏版本使用了一个或两个`<img>`元素。为了使这些固定的`<img>`元素显示不同的图像，我编写了更改`src`属性为不同外部图像文件的代码。当我上传应用程序时，我必须上传所有图像文件。）

在浏览器中打开 HTML 文件。第一个应用程序需要重新加载才能获得新的（单个）骰子。第二个和第三个应用程序（第三个是花旗骰游戏）使用一个按钮来掷骰子。

我重复我之前写的内容。要测试这个程序，你确实需要检查多种情况。当你（作为玩家）获胜时，并不代表测试完成。典型的问题包括：

- 缺失或不匹配的开始和结束标签。
- 不匹配的开花括号和闭花括号（`{`和`}`），它们包围函数、`switch`语句和`if`子句。

## 第 2 章 骰子游戏

- 缺少引号。使用 TextPad 和其他一些编辑器时，可用的颜色编码可以帮助解决这个问题，因为它会高亮识别出的关键字。
- 变量和函数的命名和使用不一致。这些名称可以是任意选择的，但必须保持一致。函数`draw2mid`不会被`drawmid2()`调用。

除了最后一点，这些都是语法错误，类似于语法和标点错误。语义错误（即含义错误）可能更难检测。如果你编写第二个`switch`语句，使其在掷出 7 点时获胜，而在掷出点数时失败，你可能编写了正确的 JavaScript 代码，但这不再是花旗骰游戏了。



#### 总结

在本章中，你学习到了如何

- 声明变量并使用全局变量来表示应用程序状态
- 编写代码执行算术运算
- 定义并使用程序员自定义函数
- 使用 JavaScript 的几个内置功能，包括`Math.random`和`Math.floor`方法
- 使用`if`和`switch`语句
- 使用 HTML 元素创建画布
- 绘制矩形和圆形

### 第 2 章：骰子游戏

本章介绍了 HTML5 的一个关键特性——画布，以及随机性和交互性的概念。同时还介绍了许多你将在本书后续示例中使用的编程特性。特别是**分阶段构建应用程序**的技术非常实用。下一章将展示一个在盒子中弹跳的球的动画——为第[4](http://dx.doi.org/10.1007/978-1-4842-4155-4_4)章真正的游戏做准备——包括名为“炮弹”和“弹弓”的弹道模拟。

