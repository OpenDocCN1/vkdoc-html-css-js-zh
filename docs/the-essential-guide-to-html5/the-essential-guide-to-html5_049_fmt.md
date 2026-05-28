# 记忆（或称专注力）游戏

## 表 5-2（续）

| 代码 | 说明 |
|------|-------------|
| `the card in the deck` | |
| `firstPick = false;` | 将 `firstPick` 设置为 false。 |
| `pick1 = new Polycard(card.` | 创建多边形，其 |
| `sx+cardWidth*.5,card.` | 坐标位于中心。 |
| `sy+cardHeight*.5,` | |
| `cardRad, card.info);` | |
| `pick1.draw();` | 绘制多边形。 |
| `}` | 关闭首次选择分支。 |
| `else {` | 否则…… |
| `secondCard = i;` | ……将 `secondCard` 设置为对牌堆中卡牌的引用。 |
| `pick2 = new Polycard(card.` | 创建一个多边形，其坐标位于中心。 |
| `sx+cardWidth*.5,card.` | |
| `sy+cardHeight*.5,` | |
| `cardRad, card.info);` | |
| `pick2.draw();` | 绘制多边形。 |
| `if (deck[i].` | 检查是否匹配。 |
| `info==deck[firstCard].info) {` | |
| `matched = true;` | 将 `matched` 设置为 true。 |
| (续) | |

---

## 表 5-2（续）

| 代码 | 说明 |
|------|-------------|
| `var nm =` | 增加匹配次数。 |
| `1+Number(document.f.` | |
| `count.value);` | |
| `document.f.count.value` | 显示新的计数值。 |
| `= String(nm);` | |
| `if (nm >= .5*deck.` | 检查游戏是否结束。 |
| `length) {` | |
| `var now = new` | 获取新的日期信息。 |
| `Date();` | |
| `var nt =` | 提取并转换时间。 |
| `Number(now.` | |
| `getTime());` | |
| `var seconds =` | 计算已用秒数。 |
| `Math.floor` | |
| `(.5+(nt-` | |
| `startTime)/1000);` | |
| `document.f.elapsed.value = String(seconds);` | 输出时间。 |
| `}` | 关闭游戏结束分支。 |
| `}` | 关闭匹配成功分支。 |
| `else {` | 否则…… |
| `matched = false;` | ……将 `matched` 设置为 false。 |
| `}` | 关闭 else 子句。 |
| `firstPick = true;` | 重置 `firstPick`。 |
| `setTimeOut(flipback,1000);` | 设置暂停。 |
| `}` | 关闭非首次选择分支。 |
| `}` | 关闭有效选择（点击卡牌——for 循环提前退出）。 |
| (续) | |

---

## 表 5-2（续）

| 代码 | 说明 |
|------|-------------|
| `}` | 关闭函数。 |
| `function flipBack() {` | 函数头——暂停后处理翻转回退。 |
| `if (!matched) {` | 如果没有匹配…… |
| `deck[firstcard].draw();` | ……绘制卡牌背面。 |
| `deck[secondCard].draw();` | ……绘制卡牌背面。 |
| `}` | ……关闭该子句。 |
| `else {` | 否则需要移除卡牌。 |
| `ctx.fillStyle = tableColor;` | 设置为桌面/面板颜色。 |
| `ctx.fillRect(deck` | 覆盖绘制卡牌。 |
| `[secondCard].sx,` | |
| `deck[secondCard].sy,` | |
| `deck[secondCard].` | |
| `sWidth,deck[secondCard].` | |
| `sHeight);` | |
| `ctx.fillRect(deck[firstCard].` | 覆盖绘制卡牌。 |
| `sx,deck[firstCard].` | |
| `sy,deck[firstCard].` | |
| `sWidth,deck[firstCard].` | |
| `sHeight);` | |
| `deck[secondCard].sx = -1;` | 设置此值，使该卡牌不再被检查。 |
| `deck[firstCard].sx = -1;` | 设置此值，使该卡牌不再被检查。 |
| `}` | 关闭无匹配分支。 |
| `}` | 关闭函数。 |
| (续) | |

---

## 表 5-2（续）

| 代码 | 说明 |
|------|-------------|
| `function init(){` | 函数头 `init()`。 |
| `ctx = document.getElementById('canvas').` | 将 `ctx` 设置为执行所有绘图操作。 |
| `getContext('2d');` | |
| `canvas1 = document.getElementById('canvas');` | 将 `canvas1` 用于事件处理。 |
| `canvas1.addEventListener('click',choose,` | 设置事件处理。 |
| `false);` | |
| `makeDeck();` | 创建牌堆。 |
| `document.f.count.value = "0";` | 初始化可见计数值。 |
| `document.f.elapsed.value = "";` | 清除旧值。 |
| `starttime = new Date();` | 设置起始时间的第一步。 |
| `starttime = Number(starttime.getTime());` | 重用该变量，设置从基准时间开始的毫秒数。 |
| `shuffle();` | 打乱卡牌信息值。 |
| `}` | 关闭函数。 |
| `</script>` | 关闭 script 元素。 |
| `</head>` | 关闭 head 元素。 |
| `<body onLoad="init();">` | Body 标签，设置 `init()`。 |
| `<canvas id="canvas" width="900" height="400">` | Canvas 起始标签。 |
| 您的浏览器不支持 HTML5 的 canvas 元素。 | 警告信息。 |
| `</canvas>` | 关闭 canvas 元素。 |
| `<br/>` | 说明前的换行。 |
| 点击两张卡牌，查看是否匹配。 | 操作说明。 |
| `<form name="f">` | 表单起始标签。 |
| (续) | |

---

## 表 5-2（续）

| 代码 | 说明 |
|------|-------------|
| 匹配数量：`<input type="text"` | 用于输出的标签和输入元素。 |
| `name="count" value="0" size="1"/>` | |
| `<p>` | 段落间隔。 |
| 完成谜题所用时间：`<input` | 用于输出的标签和输入元素。 |
| `type="text" name="elapsed" value=" " size="4"/>` | |
| 秒。 | |
| `</form>` | 关闭表单。 |
| `</body>` | 关闭 body。 |
| `</html>` | 关闭 html。 |

无论你做出何种编程选择，请务必在代码中添加注释（使用两个