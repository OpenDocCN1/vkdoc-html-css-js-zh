# 第 5 章：记忆（又称配对）游戏

`ctx.fillText("Number of matches so far: " + String(count), 10, 360);`

```
if (count >= 0.5 * deck.length) {
    // 检查是否完成所有配对。
    finished = true;
    // 防止在结束前可能的多余擦除操作。
    var now = new Date();
    // 获取一个 Date 对象。
}
```

**表 5-4.** *（续）*

| 代码 | 说明 |
|------|------|
| `var nt = Number(now.getTime());` | 提取当前时间。 |
| `var seconds = Math.floor(0.5 + (nt - startTime) / 1000);` | 计算已用时间。 |
| `ctx.fillStyle = tableColor;` | 准备擦除。 |
| `ctx.fillRect(0, 0, 900, 400);` | 擦除整个画布。 |
| `ctx.fillStyle = backColor;` | 设置绘图颜色。 |
| `out = "You finished in " + String(seconds) + " secs.";` | 准备文字内容。 |
| `ctx.fillText(out, 10, 100);` | 写入文字。 |
| `ctx.fillText("Reload the page to try again.", 10, 300);` | 写入文字。 |
| `}` | 关闭游戏是否完成的检查。 |
| `}` | 关闭配对检查。 |
| `else {` | 否则。 |
| `matched = false;` | 未配对成功。 |
| `}` | 关闭 else 分支。 |
| `firstPick = true;` | 为下一轮选择做准备。 |
| `setTimeout(flipBack, 1000);` | 设置定时回调以翻转牌面，让玩家看到他们所选的牌。 |
| `}` | 在第二次选择内。 |
| `}` | 关闭第二次选择的 else 分支。 |
| `}` | 关闭 choose 函数。 |

```
function flipBack() {
    // flipback 函数头部。由 setTimeout 动作调用。
    var card;
}
```

**表 5-4.** *（续）*

| 代码 | 说明 |
|------|------|
| `if (finished) return;` | 防止擦除部分最终消息。 |
| `if (!matched) {` | 如果配对不成功，则… |
| `deck[firstCard].draw();` | …绘制第一张牌，即牌背面。 |
| `deck[secondCard].draw();` | …绘制第二张牌。 |
| `}` | 关闭配对不成功的情况。 |
| `else {` | |
| `ctx.fillStyle = tableColor;` | 设置颜色，准备擦除这些牌。 |
| `ctx.fillRect(deck[secondCard].sx, deck[secondCard].sy, deck[secondCard].sWidth, deck[secondCard].sHeight);` | 覆盖第二张牌。 |
| `ctx.fillRect(deck[firstCard].sx, deck[firstCard].sy, deck[firstCard].sWidth, deck[firstCard].sHeight);` | 覆盖第一张牌。 |
| `deck[secondCard].sx = -1;` | 设置此值，防止该牌再次被选取。 |
| `deck[firstCard].sx = -1;` | 同上。 |
| `}` | 关闭 else 分支；表示配对成功。 |
| `}` | 关闭 flipback 函数。 |

```
function init() {
    // init 函数头部。
    ctx = document.getElementById('canvas').getContext('2d');
    canvas1 = document.getElementById('canvas');
    // 设置 canvas 变量以引用画布。用于设置事件处理。
    canvas1.addEventListener('click', choose, false);
}
```

**表 5-4.** *（续）*

| 代码 | 说明 |
|------|------|
| `makeDeck();` | 创建牌组。 |
| `shuffle();` | 洗牌（粗略洗牌）。 |
| `ctx.font = "bold 20pt sans-serif";` | 设置字体。 |
| `ctx.fillText("Click on two cards to make a match.", 10, 20);` | 在画布上显示操作说明文字。 |
| `ctx.fillText("Number of matches so far: 0", 10, 360);` | 显示已配对数量。 |
| `startTime = new Date();` | 获取一个 Date 对象。 |
| `startTime = Number(startTime.getTime());` | 提取时间作为开始时间。 |
| `}` | 关闭 init 函数。 |
| `</script>` | 关闭 script 元素 |
| `</head>` | 关闭 head。 |
| `<body onLoad="init();">` | 开始 body 标签。设置调用 init 函数。 |
| `<canvas id="canvas" width="900" height="400">` | canvas 标签。 |
| `Your browser doesn't support the HTML5 element canvas.` | 针对旧浏览器的标准提示信息。 |
| `</canvas>` | 关闭 canvas 标签。 |
| `</body>` | 关闭 body。 |
| `</html>` | 关闭 html。 |

## 第 5 章：记忆（又称配对）游戏

尽管这两个程序是可玩的游戏，但仍有改进空间。例如，玩家不可能输掉游戏。在回顾本章内容后，尝试想出一种强制玩家失败的方法，比如限制移动次数或施加时间限制。

这些应用程序在加载时便开始计时。有些游戏会等到玩家执行第一次操作后才开始计时。如果你想采用这种更友好的方式，你需要设置一个初始化为 `false` 的逻辑变量，并在 `choose` 函数中创建一个检查该变量是否已设为 `true` 的机制。由于可能尚未设置，你还需要添加用于设置 `startTime` 变量的代码。



