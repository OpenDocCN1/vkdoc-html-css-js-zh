# 第 3 章：弹跳的球

本章涵盖以下内容：

- 创建程序员自定义对象
- 使用`setInterval`实现动画
- 绘制图像
- 接受并验证表单输入
- 使用按钮
- 使用`for`循环
- 使用渐变色绘图
- 预加载图像

## 引言

动画，无论是在电影中、翻页书中，还是电脑游戏中生成的，都涉及快速显示一系列静态图像，使我们将所见解读为动态的生命场景。在本章中，我将演示如何通过模拟一个在二维盒子中弹跳的球来制作动画场景，球的水平和垂直速度可由玩家更改。程序的第一版会以固定时间间隔计算球的新位置并显示结果，同时还能判断球与墙壁何时发生虚拟碰撞以及球如何反弹。之后，我们将看到如何用图像替换球，以及如何使用渐变色绘制矩形。我们还将探讨用于验证表单输入的 HTML5 特性。接着，我会展示一个交互式示例，为玩家提供停止和重新开始弹跳的方法。最后一个示例是一个矩形框，里面播放的视频会在一张照片上弹跳。以下是五个示例：

- 在二维盒子中让球弹跳（见图 3-1）
- 用图像替换球，并为盒子墙壁使用渐变色（见图 3-2）
- 验证输入（见图 3-3）
- 在背景图像上弹跳图像，并提供停止和恢复操作的方法（见图 3-4）
- 在背景图像上弹跳视频

**注意** 我们要制作的这种动画称为*计算动画*，即由计算机程序重新计算对象的位置并重新显示该对象。这与赛璐珞（或逐帧）动画形成对比，后者使用预先绘制的独立静态图像。动画 GIF 就是赛璐珞动画的实例，可以在许多图形程序中制作。你需要发挥想象，将这些静态图片所代表的动画呈现出来。在图 3-1 中，注意表单中包含用于设置水平速度和垂直速度的输入字段。

***图 3-1.** 弹跳的球*

在图 3-2 中，球已被替换为图像，墙壁使用渐变色填充。

***图 3-2.** 球现在来自外部文件的图像*

HTML5 允许你指定输入的类型。在此示例中，我指定输入必须为数字，并设置了最小值和最大值。我使用 CSS 指定：如果用户进行了无效输入，字段颜色将变为红色。如图 3-3 所示。

***图 3-3.** 显示错误输入的表单*

这组应用程序展示了相当复杂的编程，但这还不是真正的游戏，尽管人们喜欢看到头像或其他图像在盒子中弹跳。受最近一张家庭照片的启发，我决定制作一个带有弹跳图片的程序，并增加了停止和恢复动画的功能。我还加入了显示背景图片的功能。图 3-4 展示了一个屏幕截图。游戏目标是将移动物体（一张棉花糖的照片）停在靠近面部彩绘成熊猫的孩子安尼卡附近。如图 3-4 所示。这为我提供了一个展示所谓事件驱动编程优势的示例。

***图 3-4.** 弹跳棉花糖游戏*

最新的示例包含了视频。我的女儿阿维娃正在操作一个折纸模型——由小弗里茨·雅凯设计的“接吻鱼”。背景是我在亚特兰大植物园拍摄的照片，照片中阿维娃坐在长椅上，旁边是一座青蛙雕像，如图 3-5 所示。为此，你需要想象承载视频的矩形在移动，*同时*矩形内鱼的嘴巴在张合。

***图 3-5.** 在盒子中弹跳的视频*

弹跳视频程序比其他示例更简单，你可能希望添加玩家互动方式。然而，“点击开始”按钮起到了重要作用。为了防止网站创建者强制用户观看未经请求的视频，需要一些用户交互。该按钮正是为此目的而设。

## 关键需求

对于这个应用程序——实际上对于所有编程而言——在编写任何代码之前定义需求至关重要。该应用程序需要用到我在前面章节演示过的内容：在画布元素上绘制形状，以及使用表单画布元素。在这个示例中，我们将实际使用表单字段进行输入。在第 2 章描述的骰子游戏中，这些字段严格用于输出。

在第 1 章中，HTML 文档使用了外部图像文件。在第 2 章中，我们完全通过代码绘制了骰子的面。本章中，我将同时演示两种方式：用代码绘制的弹跳圆形，以及来自图像文件的弹跳图像。

为了实现这一点，我们需要一些代码，能够以固定的时间间隔执行某些操作（目前具体是什么无关紧要）。时间间隔必须足够短，使结果看起来像运动。

在这种情况下，要执行的操作是重新定位球（或替代球的物体）。此外，代码需要判断球是否会碰到任何墙壁。当然，并没有真实的球，也没有真实的墙壁。这一切都是虚拟的，所以完全是代码层面的工作。我们将编写代码，对球的虚拟位置与每面墙壁的虚拟位置进行计算。如果发生虚拟碰撞，代码会调整水平或垂直位移值，使球从墙壁上反弹。更精确地说（尽管有些学究气），代码会设置某些值，以便在下一个迭代中，球对象沿不同方向运动。

## HTML5、CSS 和 JavaScript 特性

让我们来看看实现弹球应用所需的 HTML5、CSS 和 JavaScript 的具体特性。我们将基于前几章介绍的内容进行构建，特别是 HTML 文档的通用结构、`canvas` 元素的使用、程序员自定义函数和内置函数，以及 `form` 元素的使用。

为了计算重新定位，我们使用初始值或表单输入字段中键入的任何新值。然而，目标是构建一个健壮的系统，不会对玩家的错误输入做出响应。错误输入是指不是数字的条目，或超出指定范围的数字。我们可以直接忽略错误输入，但也要向玩家反馈输入有误，因此我们将让输入框改变颜色，如图 3-3 所示。

为了给用户（现在称为*玩家*）提供与应用程序交互的方式，我添加了代码，在玩家界面上显示一个停止按钮和一个恢复按钮。响应点击停止按钮的函数会停止时间间隔事件。响应点击恢复按钮的函数会启动时间间隔事件。

为了让弹跳视频循环播放，我添加了代码，在“ended”事件发生后重新开始播放。这是因为我了解到 `video` 元素中的 `loop` 属性可能并非在所有浏览器中都有效。加入重启代码后，程序在 Chrome 和 Safari 中均可正常运行。

## 绘制一个球体或一个/多个图像

如第 [2](http://dx.doi.org/10.1007/978-1-4842-4155-4_2) 章所述，在画布上绘制任何内容（例如代表球体的圆形）都需要在 HTML 文档的 body 部分包含 `canvas` 元素。接下来，我们需要定义一个变量 `ctx`，并添加设置该变量值的代码，以便使用 JavaScript。以下是实现此功能的语句：

```
ctx = document.getElementById('canvas').getContext('2d');
```

正如我们在第 [2](http://dx.doi.org/10.1007/978-1-4842-4155-4_2) 章中所见，圆形是通过绘制路径的一部分（弧线）来创建的。以下几行代码启动路径、设置填充颜色、指定弧线，然后使用 `fill` 方法绘制一个闭合的、填充的路径。请注意，`arc` 方法使用变量来指定圆心的坐标和半径。参数 `0` 和 `Math.PI*2` 表示角度，此处为 0 到 `Math.PI*2`，形成一个完整的圆。`true` 参数表示逆时针方向，尽管在此特定情况下，`false` 也会产生相同的效果。

```
ctx.beginPath();
ctx.fillStyle = "rgb(200,0,50)";
ctx.arc(ballx, bally, ballrad, 0, Math.PI*2, true);
ctx.fill();
```

对于第一个版本的弹球，盒子被绘制成一个矩形轮廓。轮廓的宽度，称为*描边*，通过以下方式设置：

```
ctx.lineWidth = ballrad;
```

您可以尝试调整线宽。请注意，如果线宽太小且球速设置得很快，球可能会在一次移动中越过墙壁。

绘制矩形的语句是：

```
ctx.strokeRect(boxx, boxy, boxwidth, boxheight);
```

我将球体的代码放在矩形代码之前，这样矩形会覆盖在球体之上。我认为对于弹跳效果来说，这样看起来更好。

程序的第二个版本用图像来显示球体。这需要代码通过 `new` 运算符调用 `Image()` 来设置一个 `img` 对象，将其赋值给一个变量，并为 `src` 属性赋值。在应用中，我们在一条语句中完成所有操作，但让我们先看看各个部分。

您在第 [2](http://dx.doi.org/10.1007/978-1-4842-4155-4_2) 章中了解了 `var` 语句。此类语句定义或*声明*一个变量。在这里使用名称 `img` 作为我们的 `var` 是可以的；它与 HTML 的 `img` 元素没有冲突。`new` 运算符名副其实：它创建一个新对象，在此例中是内置类型 `Image` 的对象。`Image` 函数被称为*构造函数*：它构造一个 `Image` 类型的对象。`Image` 函数不接受任何参数，因此只有开括号和闭括号。

`Image` 对象具有属性，就像 `img` 这样的 HTML 元素一样。所使用的具体图像由 `src` 属性的值指示。此处，`pearl.jpg` 是一个图像文件的名称，该文件与 HTML 文档位于同一文件夹中。以下两条语句设置了 `img` 变量，并将其 `src`（源）设置为图像文件的地址（URL）。

```
var img = new Image();
img.src = "pearl.jpg";
```

对于您的应用，请使用您所选图像文件的名称。它可以是 JPG、PNG 或 GIF 格式，并确保将其放在与 HTML 文档相同的文件夹中，或包含正确的路径。注意文件名和扩展名的大小写匹配。

要在画布上绘制此图像，我们需要一行代码来指定图像对象、图像左上角的位置以及显示图像时要使用的宽度和高度。与矩形的情况一样，此代码是调用上下文对象的方法，因此我使用在 `init` 函数中定义的变量 `ctx`。我需要调整先前用于圆心的 `ballx` 和 `bally` 值，以表示左上角。我将宽度和高度都设置为球半径的两倍。语句如下：

```
ctx.drawImage(img, ballx - ballrad, bally - ballrad, 2 * ballrad, 2 * ballrad);
```

现在让我们稍作休息。亲爱的读者，轮到您来做些工作了。请考虑以下 HTML 文档：

```
<html>
<head>
<title>The Origami Frog</title>
<script>
var img = new Image();
img.src = "frogface.gif";
var ctx;

function init() {
    ctx = document.getElementById("canvas").getContext('2d');
    ctx.drawImage(img, 10, 20, 100, 100);
}
</script>
</head>
<body onLoad="init();">
<canvas id="canvas" width="400" height="300">
    Your browser doesn't support the HTML5 element canvas.
</canvas>
</body>
</html>
```

找到您自己的图像文件，并用其名称替换 `frogface.gif`。将标题改为适当的内容。尝试修改这行代码：

```
ctx.drawImage(img, 10, 20, 100, 100);
```

也就是说，更改 `10` 和 `20` 来重新定位图像，更改 `100` 和 `100` 来改变宽度和高度。进行修改，看看程序是否按您的预期响应。请记住，在指定宽度和高度时，您可能会改变图片的形状——即*宽高比*。

这里需要重点指出的是，由于代码是在画布上绘制图像，为了产生球体移动的效果，我们还需要代码来擦除所有内容，然后在新的位置重新绘制所有内容，包括球体。擦除所有内容的语句是：

```
ctx.clearRect(boxx, boxy, boxwidth, boxheight);
```

或许可以只擦除（清除）画布的部分区域，但我选择擦除所有内容然后重新绘制。在每个具体情况下，您需要决定哪种方式更合理。

考虑在画布上绘制两个图像。您需要两个不同的变量来代替 `img`。请为这些任务变量赋予独特的名称。如果您在模仿苏斯博士（Dr. Seuss），可以使用 `thing1` 和 `thing2`；否则，请选择对您有意义的名称！

对于绘制背景图像，然后绘制移动的棉花糖，我的代码简单地先绘制背景图像（位置固定），然后在计算出的位置绘制棉花糖。完整的代码如下。您将在后面的章节中了解到 `moveAndCheck`。

```
function moveBall() {
    ctx.clearRect(boxx, boxy, boxWidth, boxHeight);
    moveAndCheck();
    ctx.drawImage(bkg, 0, 0, 4000, 3000, 0, 0, 400, 300);
}
```

`ctx.drawImage(ball,0,0,388,435,ballx-ballrad,bally-ballrad,388/10,435/10);`

`ctx.strokeRect(0,0,400,300);`

}

你可能会问为什么需要重绘背景。答案是一旦在画布上绘制了某些内容，它就相当于设置了特定颜色的“颜料点”——这个术语是*像素*（图像元素）。每次迭代都会发生一些变化（请等待下一节关于时间间隔的内容），尽管画布的大部分保持不变，但生成新图片的最佳方法是清除画布、绘制背景，然后再绘制小球。

## 渐变——顺便解释一下数组

让我们看看如何为弹跳程序使用渐变，这是一种类似彩虹的颜色组合。你可以使用渐变来设置 `fillStyle` 属性。我不想让小球位于一个实心矩形之上，因此我需要弄清楚如何分别绘制四面墙壁。

渐变是 HTML5 中的一种对象类型。有线性渐变和径向渐变。在这个应用中，我们使用线性渐变。代码定义了一个变量（我将其命名为 `grad`）作为渐变对象，使用了我们之前用变量 `ctx` 定义的画布上下文的一个方法。渐变代码如下所示：

```
var grad;
grad=ctx.createLinearGradient(boxx,boxy,boxx+boxwidth,boxy+boxheight);
```

渐变覆盖在一个矩形形状上。

渐变涉及颜色集合。一种典型做法是编写代码来设置所谓的*色标*，例如让渐变呈现彩虹效果。为此，我在一个名为 `hue` 的变量中设置了一个数组的数组。

你可以将数组视为一个用于存放值集合的容器。变量只能保存一个值，而数组可以保存多个值。在下一章中，你将了解一个名为 `everything` 的数组，它将保存所有要绘制在屏幕上的对象。

# 第 3 章 弹跳小球

在描述猜词游戏的[第 9 章](http://dx.doi.org/10.1007/978-1-4842-4155-4_9)中，单词列表是一个单词数组。你会在本书中读到数组的许多应用。这里有一个具体的例子。

下面的 `var` 语句将一个变量设置为特定的数组：

```
var family = ["Daniel","Aviva", "Annika"];
```

变量 `family` 是一个数组。它的数据类型是数组。它包含我家庭成员的一份列表。要访问或设置该数组的第一个元素，你会使用 `family[0]`。

用于指定数组中特定成员的值称为*索引值*或*下标*。数组索引从零开始。表达式 `family[0]` 会得到 `"Daniel"`。表达式 `family[1]` 会得到 `"Aviva"`。表达式 `family[2]` 会得到 `"Annika"`。如果变量 `relative` 的值是 `1`，那么 `family[relative]` 会得到 `"Aviva"`。要确定数组中的元素数量，你会使用 `family.length`。在这个例子中，长度是 `3`。注意长度是 `3`；索引范围从 `0` 到 `2`。

数组中的各个项可以是任何类型，包括数组。例如，我可以修改 `family` 数组以提供更多信息：

```
var family = [["Daniel","son"],
              ["Aviva", "daughter"],
              ["Annika","granddaughter"]
            ];
```

这种带有换行和缩进的格式不是必需的，但这是一个好习惯。JavaScript 不会对其解析。我们必须确保括号和逗号正确无误！

表达式 `family[2][1]` 会得到 `"grandDaughter"`。请记住，数组索引从 `0` 开始，因此对于该数组（在此类示例中有时称为*外部数组*），索引值 `2` 产生 `["Annika", "grandDaughter"]`，而对于该数组，索引 `1` 产生 `"granddaughter"`。这些内部数组不必具有相同的长度。

考虑下面的例子：

```
var family = [["Daniel","teacher"],
              ["Aviva", "government staff"],
              ["Annika"]
            ];
```

代码会检查数组的长度，如果长度是 `2` 而不是 `1`，则第二个项是此人的职业。如果内部数组的长度是 `1`，则假定此人没有职业。

数组的数组对于产品名称和价格非常有用。以下语句指定了商店的有限库存：

```
var inventory = [
  ["toaster",25.99],
  ["blender",74.99],
  ["dish",10.50],
  ["rug",599.99]
];
```

这家商店有四种商品，最便宜的是盘子，位于索引 `2` 的位置，最贵的是地毯，位于索引 `3` 的位置。

现在，让我们看看如何使用这些概念来定义渐变。我们将使用一个数组，其各个元素也是数组。每个内部数组保存一种颜色的 RGB 值，即红色、黄色、绿色、青色、蓝色、品红色。

```
var hue = [
  [255,   0,   0 ],
  [255, 255,   0 ],
  [  0, 255,   0 ],
  [  0, 255, 255 ],
  [  0,   0, 255 ],
  [255,   0, 255 ]
] ;
```

# 第 3 章 弹跳小球

这些值表示从红色（RGB 255,0,0）到品红色（RGB 255,0,255）的颜色范围，中间指定了四种颜色。JavaScript 中的渐变功能会填充颜色以生成如图 3-2 所示的彩虹图案。渐变是通过指定从 0 到 1 区间内的点来定义的。你可以指定非彩虹的其他渐变。例如，你可以使用图形程序选择一组 RGB 值作为所谓的停止点，然后 JavaScript 会填充值以从一个点渐变到下一个点。

数组中的数值并不完全是我们要的，因此我们需要对它们进行处理，以生成 JavaScript 所需的内容。

对数组的操作通常需要对数组的每个成员执行某些操作。许多编程语言中都有一种用于此目的的构造，即 `for` 循环，它使用一个称为索引变量的变量。`for` 循环的结构是：

```
for (索引变量的初始值; 继续条件; 索引变量的变化) {
  每次要执行的代码。该代码通常引用索引变量
}
```

这句话的意思是：从该初始值开始，只要该条件成立就继续执行循环，并以这种指定的方式更改索引值。用于变化的典型表达式会使用像 `++` 这样的运算符。`++` 运算符将指定的变量加 `1`。一个典型的 `for` 开头语句是：

```
for (n=0;n<10;n++)
```

这个 `for` 循环使用名为 `n` 的变量，其中 `n` 初始化为 `0`。如果 `n` 的值小于 `10`，则执行循环内的语句。每次迭代后，`n` 的值增加 `1`。在这种情况下，循环代码将执行 10 次，`n` 的值依次为 `0`、`1`、`2`，一直增加到 `9`。

下面是另一个例子，一个演示数组的常见例子。设置 `grades` 变量以保存学生的分数集合：

```
var grades = [4.0, 3.7, 3, 2.3, 3];
```

根据不同的机构，这可能表示成绩为 A、A-、B、C+ 和 B。以下代码片段计算平均绩点并将其存储在名为 `gpa` 的变量中。请注意，我们需要将名为 `sum` 的变量初始化为值 `0`。`+=` 运算符将 `grades` 数组在索引值 `g` 处的值累加到 `sum` 中保存的值上。

```
var sum = 0;
for (g=0;g<grades.length;g++) {
  sum += grades[g];
}
var gpa;
gpa = sum/grades.length;
```

为了生成构建渐变所需的内容，代码从 `hue` 数组中提取值，并使用它们生成指示 RGB 值的字符串。我们使用 `hue` 数组以及一个名为 `color` 的变量来设置色标以定义渐变。色标设置在 `0` 到 `1` 之间的任意点，使用一个 `for` 循环将 `color` 设置为所需格式的字符串，即以 `rgb(` 开头，并包含这三个值。

```
for (h=0;h<hue.length;h++) {
  color = 'rgb('+hue[h][0]+','+hue[h][1]+','+hue[h][2]+')';
  grad.addColorStop(h*1/hue.length,color);
}
```

设置 `color` 的赋值语句可能对你来说很奇怪：这里有很

# JavaScript 字符串拼接与定时事件

## 字符串拼接与数组操作

`going on`——那些加号（`+`）在做什么？记住，我们的任务是生成表示特定 RGB 值的字符串。这里的加号**不是**表示数字的加法，而是表示字符串的拼接。这意味着这些值被粘合在一起，而不是数学上的相加，所以 `5+5` 的结果是 `10`，而 `'5'+'5'` 的结果是 `55`。第二个例子中的 `5` 被引号括起来，因此它们是字符串而不是数字。方括号用于提取数组中的元素。

`JavaScript` 将数字转换为对应的字符串，然后将其组合。请记住，我们处理的是数组中的数组，因此方括号内的第一个数字（在本例中由变量 `h` 提供）给出第一个数组，而方括号内的第二个数字则给出该数组中的数字。让我们看一个简单的例子。循环第一次运行时，`h` 的值将为 `0`，这会给出 `hue` 数组中的第一个条目。然后我们查找该条目的各个部分，以构建最终的颜色。

## 绘制渐变矩形

经过以上所有步骤，我们的代码设置了变量 `grad`，用于指示填充模式。代码没有将 `fillStyle` 设置为颜色，而是将其设置为变量 `grad`。

```javascript
ctx.fillStyle = grad;
```

绘制矩形的方式与之前相同，但现在使用了指定的填充。这些是原始矩形左侧、右侧、顶部和底部的四条窄墙。我让这些墙的厚度等于球的半径。对于垂直墙，厚度就是宽度；对于水平墙，厚度就是高度。

```javascript
ctx.fillRect(boxx,boxy,ballrad,boxheight);
ctx.fillRect(boxx+boxwidth-ballrad,boxy,ballrad,boxheight);
ctx.fillRect(boxx,boxy,boxwidth,ballrad);
ctx.fillRect(boxx,boxy+boxheight-ballrad,boxwidth,ballrad);
```

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
