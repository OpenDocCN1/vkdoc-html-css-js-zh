# 第 10 章 21 点

#### 表 10-2. 21 点游戏的注释代码

本示例中的函数展现出一种过程调用模式，只有 `init()` 和 `getKey()` 是通过事件触发的。请注意，编写应用程序有很多方式，包括函数的定义方式。通常，将代码拆分为小函数是一种良好的实践，但这并非必须。代码中有许多重复的相似行，因此有机会定义更多函数。注释文档见下文表 10-2。

**代码** | **解释**
--- | ---
`more_to_house`, `dealFromDeck`, `showHouse`, 和 `add_up_player` | 
`newGame` | 
`getKey` | 
`dealStart` | 
`showHouse` | 
`playerDone` | 
`shuffle` | 
`init` | 
`swapInDeck` | 
`swapInDeck` | 
`shuffle` | 
`MCard` | 

---

| **代码** | **解释** |
|---|---|
| `<html>` | 开始 html 标签。 |
| `<head>` | 开始 head 标签。 |
| `<title>Black Jack</title>` | 完成标题元素。 |
| `<style>` | 开始 style 标签。 |
| `body {` | 指定 body 元素的样式。 |
| `background-color:white;` | 设置背景颜色。 |
| `color: black;` | 设置文本颜色。 |
| `font-size:18px;` | 设置字体大小。 |
| `font-family:Verdana, Geneva, sans-serif;` | 设置字体系列。 |
| `}` | 结束样式。 |
| `footer {` | 指定 footer 元素的样式。 |
| `display:block;` | 将此元素视为块级元素。 |
| `font-family:Tahoma, Geneva, sans-serif;` | 设置字体系列。 |
| `text-align: center;` | 将文本居中对齐。 |
| `font-style:oblique;` | 使文本倾斜。 |
| `}` | 结束样式。 |
| `header {` | 指定 header 元素的样式。 |
| `width:100%;` | 使其占据整个窗口宽度。 |
| `display:block;` | 将其视为块级元素。 |
| `}` | 结束样式。 |
| `</style>` | 结束 style 元素。 |
| `<script>` | 开始 script 元素。 |
| `var cwidth = 800;` | 设置画布宽度；用于清除画布。 |
| `var cheight = 500;` | 设置画布高度；用于清除画布。 |
| `var cardw = 75;` | 设置每张牌的宽度。 |
| `var cardh = 107;` | 设置每张牌的高度。 |
| `var playerXp = 100;` | 设置玩家手牌起始水平位置。 |
| `var playerYp = 300;` | 设置玩家手牌的垂直位置。 |
| `var houseXp = 500;` | 设置庄家手牌起始水平位置。 |
| `var houseYp = 100;` | 设置庄家手牌的垂直位置。 |
| `var houseTotal;` | 用于存储庄家手牌的总点数。 |
| `var playerTotal;` | 用于存储玩家手牌的总点数。 |
| `var pi = 0;` | 玩家手牌下一张牌的索引。 |
| `var hi = 0;` | 庄家手牌下一张牌的索引。 |
| `var deck = [];` | 存储所有牌。 |
| `var playerHand = [];` | 存储玩家的手牌。 |
| `var houseHand = [];` | 存储庄家的手牌。 |
| `var back = new Image();` | 用于牌背图像。 |
| `var ctx;` | 用于存储画布上下文。 |
| `var gameStart = false;` | 用于检查游戏是否已开始。 |
| `function init() {` | 由 body 中的`onLoad`调用的函数，用于执行初始化任务。 |
| `ctx = document.getElementById('canvas').getContext('2d');` | 设置用于所有绘图的变量。 |
| `ctx.font="italic 20pt Georgia";` | 设置字体。 |
| `ctx.fillStyle = "blue";` | 设置颜色。 |
| `buildDeck();` | 调用构建牌组的函数。 |
| `back.src ="cardback.png";` | 指定牌背图像（注意：只出现一张牌背：庄家的暗牌）。 |
| `canvas1 = document.getElementById('canvas');` | 设置用于事件处理的变量。 |
| `window.addEventListener('keydown',getkey,false);` | 设置按键按下的事件处理。 |
| `shuffle();` | 调用洗牌函数。 |
| `dealStart();` | 调用发前四张牌的函数。 |
| `}` | 结束函数。 |
| `function getKey(event) {` | 响应按键事件的函数。 |
| `var keyCode;` | 存储按键代码。 |
| `if(event == null)` | 浏览器特定代码，判断事件是否为空。 |
| `{` | 开始子句。 |
| `keyCode = window.event.keyCode;` | 从`window.event.keyCode`获取按键代码。 |
| `window.event.preventDefault();` | 阻止其他按键响应。 |


```markdown
**代码**

**说明**

`}`

闭合该子句。

`else {`

子句。

`keyCode = event.keyCode;`

从 `event.keyCode` 获取键码。

`event.preventDefault();`

阻止其他按键响应。

`}`

闭合该子句。

`switch(keyCode) {`

基于 `keyCode` 的 switch 语句头部。

`case 68:`

已按下 `d` 键。

`deal();`

给玩家（以及可能给庄家）发一张新牌。

`break;`

退出 switch。

`case 72:`

已按下 `h` 键。

`playerDone();`

调用 `playerDone` 函数。

`break;`

退出 switch。

`case 78:`

已按下 `n` 键。

`newGame();`

调用 `newGame` 函数。

`break;`

退出 switch。

`default:`

默认选项，如果你觉得不需要在玩家按下未识别键时提供反馈，可以将其移除。

`alert("Press d, h, or n.");`

反馈消息。

`}`

闭合 switch。

`}`

闭合该函数。

`function dealStart() {`

用于初始发牌的函数头部。

`playerHand[pi] =`

获取玩家的第一张牌。

`dealFromDeck(1);`

（*续*）

**代码**

**说明**

`ctx.drawImage(playerhand[pi].picture,playerXp,playerYp,cardw,cardh);`

在画布上绘制。

`playerXp = playerXp+30;`

调整水平指针。

`pi++;`

增加玩家手中的牌数。

`houseHand[hi] =`

获取庄家的第一张牌。

`dealFromDeck(2);`

`ctx.drawImage(back,houseXp,houseYp,cardw,cardh);`

在画布上绘制牌背。

`houseXp = houseXp+20;`

调整水平指针。

`hi++;`

增加庄家手中的牌数。

`playerHand[pi] =`

给玩家发第二张牌。

`dealFromDeck(1);`

`ctx.drawImage(playerhand[pi].picture,playerxp,playeryp,cardw,cardh);`

在画布上绘制。

`playerXp = playerXp+30;`

调整水平指针。

`pi++;`

增加玩家手中的牌数。

`houseHand[hi] =`

给庄家发第二张牌。

`dealFromDeck(2);`

`ctx.drawImage(househand[hi].picture,houseXp,houseYp,cardw,cardh);`

在画布上绘制。

`houseXp = houseXp+20;`

调整水平指针。

（*续*）

**代码**

**说明**

`hi++;`

增加庄家手中的牌数。

`}`

闭合该函数。

`function deal() {`

用于游戏过程中发牌的函数头部。

`if (gameStart) {`

检查游戏是否已开始。

`playerHand[pi] =`

给玩家发一张牌。

`dealFromDeck(1);`

`ctx.drawImage(playerhand[pi].picture,playerxp,playeryp,cardw,cardh);`

在画布上绘制。

`playerXp = playerXp+30;`

调整水平指针。

`pi++;`

增加玩家手中的牌数。

`if (more_to_house()) {`

判断庄家是否应继续要牌的函数。

`houseHand[hi] =`

给庄家发一张牌。

`dealFromDeck(2);`

`ctx.drawImage(househand[hi].picture,houseXp,houseYp,cardw,cardh);`

在画布上绘制一张牌。

`houseXp = houseXp+20;`

调整水平指针。

`hi++;`

增加庄家手中的牌数。

`}`

闭合 if 真子句。

`}`

闭合 `if (gameStart)` 的真子句。

（*续*）

**代码**

**说明**

`else {`

`alert("Press n to start a new game with the same deck.\n Reload page to start a game with a new deck.");`

向玩家输出提示信息：开始新游戏（使用同一副牌）或重新加载页面（使用新牌组）。

`}`

闭合游戏*未*开始的 else 分支。

`}`

闭合该函数。

`function more_to_house() {`

用于决定庄家行动的函数头部。

`var ac = 0;`

用于记录 A 牌数量的变量。

`var i;`

用于迭代的变量。

`var sumUp = 0;`

初始化总和变量。

`for (i=0;i<hi;i++) {`

遍历所有牌。

`sumUp += houseHand[i].value;`

累加庄家手中牌的点数。

`if (houseHand[i].value==1) { ac++; }`

记录 A 牌的数量。

`}`

闭合 for 循环。

`if (ac>0) {`

判断是否存在 A 牌。

`if ((sumUp+10)<=21) {`

如果存在，判断将一张 A 牌视为 11 点后总数是否仍不超过 21。

`sumUp += 10;`

如果是，则将其视为 11 点。

`}`

闭合内层 if。

`}`

闭合外层 if。

`houseTotal = sumUp;`

将全局变量设为该总和。

`if (sumUp<17) {`

判断总和是否小于 17。

（*续*）

**代码**
```


