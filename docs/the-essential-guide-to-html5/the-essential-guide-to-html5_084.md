# 当猜中字母时，迭代过程并不会停止，而是会继续推进。这意味着任何一个字母的所有实例都会被找到并揭示出来。每次匹配时，变量 `not` 都会被设置为 `false`。如果同一个字母出现两次或更多次，这个变量就会被多次设置，但这并不是问题。我加入了单词 `kerfuffle` 来确保重复字母能被正确处理（此外，我也很喜欢这个词）。你可以在下一节中查看所有代码。

#### 构建应用并使其为己所用

猜单词应用使用了 CSS 样式、由 JavaScript 创建的 HTML 标记以及 JavaScript 代码。它包含两个初始化和设置函数（`init` 和 `setupGame`）、一个承担大部分工作的函数（`pickElement`），以及另外两个用于提供反馈的函数。这些函数在表 9-1 中进行了描述。

***表 9-1.** 被调用或主动调用的函数*

| **函数** | **被谁调用/主动调用** | **调用内容** |
| --- | --- | --- |
| `init` | 由 `<body>` 标签中的 `onLoad` 动作触发调用 | `setupGame` |
| `setupGame` | `init` | 设置字母表并选取秘密单词 |
| `pickElement` | 由 `setupGame` 中 `addEventListener` 调用的动作触发调用 | `showProgress`, `pickElement`, `drawRemain` |
| `drawRemain` | `showProgress` | — |

表 9-2 展示了猜单词游戏的完整实现。

***表 9-2.** 猜单词程序的完整实现：代码与解释*

| **代码** | **解释** |
| --- | --- |
| `<html>` | 开始 html 标签。 |
| `<head>` | 开始 head 标签。 |
| `<title>Word Guess</title>` | 完成标题元素。 |
| `<style>` | 开始 style 元素。 |
| `.letters {position:absolute;left: 0px; top: 0px; border: 2px; border-style: double;margin: 5px; padding: 5px; color:#F00;background-color:#0FC; font-family:"Courier New", Courier, monospace; }` | 为指定了 `letters` 类的元素指定样式，包括边框、颜色和字体。 |
| `.blanks {position:absolute;left: 0px; top: 0px; border:none; margin: 5px; padding: 5px; color:#006; background-color: white; font-family:"Courier New", Courier, monospace; text-decoration:underline; color: black; }` | 为指定了 `blanks` 类的元素指定样式，包括边框、间距、颜色和字体，并添加下划线。 |
| `</style>` | 结束 style 元素。 |
| `<script src="words1.js" defer></script>` | 引用外部文件 `words1.js` 中的单词列表的元素，其中 `defer` 指令指示在加载本文档其余部分的同时加载该文件。 |
| `<script >` | script 元素的开始标签。 |
| `var ctx;` | 用于所有绘图的变量。 |
| `var thingelem;` | 用于创建的元素的变量。 |
| `var alphabet = "abcdefghijklmnopqrstuvwxyz";` | 定义字母表的字母，用于字母按钮。 |
| `var alphabety = 300;` | 所有字母按钮的垂直位置。 |
| `var alphabetx = 20;` | 字母表的起始水平位置。 |
| `var alphabetWidth = 25;` | 分配给字母元素宽度。 |
| `var secret;` | 将保存秘密单词。 |
| `var lettersGuessed = 0;` | 记录已猜字母的数量。 |
| `var secretx = 160;` | 秘密单词的起始水平位置。 |
| `var secrety = 50;` | 秘密单词的垂直位置。 |
| `var secretwidth = 50;` | 显示秘密单词时每个字母的分配宽度。 |
| `var cur = 0;` | 初始化 `cur`。 |
| `var guessLimit = 7;` | 如果你想改变允许的错误猜测次数，可以修改此值。 |
| `var msgx = 100;` | 消息的水平坐标。 |
| `var msgy = 120;` | 消息的垂直坐标。 |
| `var clearX = 0;` | 画布左上角的水平坐标。 |
| `var clearY = 0;` | 画布左上角的垂直坐标。 |
| `var clearW = 600;` | 画布的宽度。 |
| `var clearH = 400;` | 画布的高度。 |
| `var startRx = 10;` | 矩形堆叠的起始 x 坐标。 |
| `var startRy = alphabety - 150;` | 计算起始 y 坐标，使其位于字母表上方。 |
| `var unity = 140 / guessLimit;` | — |



#### 计算矩形高度

`var unitX = 40;`  
设置存储矩形宽度的变量。

```
function init(){
```
文档加载时调用的函数头。

`ctx = document.`  
为画布上的所有绘图设置变量。

`getElementById('canvas').getContext('2d');`

`setupGame();`  
调用设置游戏的函数。

`ctx.font="bold 20pt Ariel";`  
设置字体。

```
}
```
函数结束。

```
function setupGame() {
```
设置字母按钮和秘密单词的函数头。

`var i;`  
创建迭代变量。

`var x;`  
创建位置变量。

`var y;`  
创建位置变量。

`var uniqueid;`  
为每组创建的 HTML 元素创建变量。

`var an = alphabet.length;`  
值为 26。

```
for(i=0;i<an;i++) {
```
循环创建字母按钮。

`uniqueid = "a"+String(i);`  
创建唯一标识符。

`d = document.`  
创建一个字母类型的元素。

`createElement('alphabet');`

`d.innerHTML = (`  
按下一行指定定义内容。

```
"<div class="letters" id='"+uniqueid
+"'>"+alphabet[i]+"</div>");
```
指定一个类为 letters 的 div，具有唯一标识符和文本内容，即字母表中的第 i 个字母。

`document.body.appendChild(d);`  
添加到 body 中。

`thingelem = document.getElementById(uniqueid);`  
获取具有该 Id 的元素。

`x = alphabetx + alphabetWidth*i;`  
计算其水平位置。

`y = alphabety;`  
设置垂直位置。

`thingelem.style.top = String(y)+"px";`  
使用 style.top 设置垂直位置。

`thingelem.style.left = String(x)+"px";`  
使用 style.left 设置水平位置。

`thingelem.addEventListener('click', pickElement,false);`  
设置鼠标点击事件的处理程序。

```
}
```
字母循环结束。

`var ch = Math.floor(Math.random()*words.length);`  
随机选择一个单词的索引。

`secret = words[ch];`  
将全局变量 secret 设置为此单词。

```
for (i=0;i<secret.length;i++) {
```
根据秘密单词的长度进行循环。

`uniqueid = "s"+String(i);`  
为单词创建唯一标识符。

`d = document.createElement('secret');`  
为单词创建一个元素。

`d.innerHTML = ("<div class="blanks" id='" +uniqueid+"'> __ </div>");`  
设置内容为一个类为 blanks 的 div，其 Id 为刚创建的 uniqueid。文本内容为下划线。

`document.body.appendChild(d);`  
将创建的元素作为 body 的子元素添加。

`thingelem = document.getElementById(uniqueid);`  
获取创建的元素。

`x = secretx + secretwidth*i;`  
计算元素的水平位置。

`y = secrety;`  
设置其垂直位置。

`thingelem.style.top = String(y)+"px";`  
使用 style.top 设置垂直位置。

`thingelem.style.left = String(x)+"px";`  
使用 style.left 设置水平位置。

```
}
```
循环结束。

`return false;`  
返回 false 以防止 HTML 页面刷新。

```
}
```
函数结束。

```
function pickElement(ev) {
```
因点击而调用的函数头。

`var not = true;`  
将 not 设置为 true，可能会被改变。

`var picked = this.textContent;`  
从 this 引用的对象中提取文本内容，即字母。

`var i;`  
用于迭代。

`var j;`  
用于迭代。

`var uniqueid;`  
用于为元素创建唯一标识符。

`var thingelem;`  
保存元素。

`var out;`  
显示消息。

```
for (i=0;i<secret.length;i++) {
```
遍历秘密单词中的字母。

```
if (picked==secret[i]) {
```
表示：“如果玩家猜中的字母与秘密单词中的这个字母相等……”

`id = "s"+String(i);`  
构建该字母的标识符。

`document.getElementById(id).textContent = picked;`  
将文本内容更改为该字母。

`not = false;`  
将 not 设置为 false。

`lettersGuessed++;`  
正确识别的字母数加一。

```
if (lettersGuessed==secret.length) {
```
表示：“如果整个秘密单词已被猜中……”



`ctx.fillStyle = gallowsColor;`

设置颜色，这里使用了绞刑架的棕色，但也可以是其他任意颜色。

`out = "You won!";`

设置消息。

`ctx.fillText(out, 200, 80);`

显示消息。

`ctx.fillText("Re-load the page to try again.", 200, 120);`

显示另一条消息。

```
for (j = 0; j < alphabet.length; j++) {
```

遍历整个字母表。

```
uniqueid = "a" + String(j);
```

构造标识符。

```
thingelem = document.getElementById(uniqueid);
```

获取元素。

```
thingelem.removeEventListener('click', pickElement, false);
```

移除事件处理。

```
}
```

结束`j`的`for`循环迭代。

```
}
```

结束`if (lettersGuessed...)`，即全部猜中的测试。

```
}
```

结束`if (picked == secret[i])`为真的子句。

```
}
```

结束对秘密单词中字母的`for`循环迭代。

(*续*)

