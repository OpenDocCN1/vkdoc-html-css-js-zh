# 表 10-2. (续)

| 代码 | 说明 |
|------|------|
| `}` | 关闭内部的 `if` 语句。 |
| `else {` | 开始 `else` 子句。 |
| *(续)* | |
| `ctx.fillText("You went over and lost.",30,100);` | 显示一条消息。 |
| `}` | 关闭 `else` 子句。 |
| `}` | 关闭外部子句（玩家点数爆了）。 |
| `else` | `else` 玩家点数未爆。 |
| `if (houseTotal>21) {` | 询问庄家点数是否爆了。 |
| `ctx.fillText("You won. House went over.",30,100);` | 显示一条消息。 |
| `}` | 关闭该子句。 |
| `else` | `else`。 |
| `if (playerTotal>=houseTotal) {` | 比较两个点数。 |
| `if (playerTotal>houseTotal) {` | 执行更具体的比较。 |
| `ctx.fillText("You won.",30,100);` | 显示赢家消息。 |
| `}` | 关闭内部子句。 |
| `else {` | 开始 `else` 子句。 |
| `ctx.fillText("TIE!",30,100);` | 显示一条消息。 |
| `}` | 关闭 `else` 子句。 |
| `}` | 关闭外部子句。 |
| `else` | `else`。 |
| `if (houseTotal<=21) {` | 检查庄家是否未爆。 |
| `ctx.fillText("You lost.",30,100);` | 显示一条消息。 |
| `}` | 关闭该子句。 |
| *(续)* | |
| `else {` | 开始 `else` 子句。 |
| `ctx.fillText("You won because house went over.");` | 显示一条消息（玩家未爆，庄家爆了）。 |
| `}` | 关闭该子句。 |
| `gameStart = false;` | 重置 `gameStart`。 |
| `}` | 关闭 `if(gamestart)` 的真值分支。 |
| `else {` | 向玩家显示消息。 |
| `alert("Press n to start a new game with the same deck.\n Reload for a new deck and then press n to start a game.");` | |
| `}` | |
| `}` | 关闭函数。 |
| `function newGame() {` | 新游戏函数的头部。 |
| `ctx.clearRect(0,0,cwidth,cheight);` | 清除画布。 |
| `pi=0;` | 重置玩家的索引。 |
| `hi=0;` | 重置庄家的索引。 |
| `playerXp = 100;` | 重置玩家手牌第一张牌的水平位置。 |
| `houseXp= 500;` | 重置庄家手牌的水平位置。 |
| `dealStart();` | 调用函数以初始发牌。 |
| `}` | 关闭函数。 |
| `function showHouse() {` | 揭示庄家手牌函数的头部。 |
| `var i;` | 用于迭代。 |
| `houseXp = 500;` | 重置水平位置。 |
| *(续)* | |
| `for (i=0;i<hi;i++) {` | 遍历手牌的 `for` 循环。 |
| `ctx.drawImage(househand[i].picture,houseXp,houseYp,cardw,cardh);` | 绘制牌。 |
| `houseXp = houseXp+20;` | 调整指针。 |
| `}` | 关闭 `for` 循环。 |
| `}` | 关闭函数。 |
| `function shuffle() {` | 洗牌函数的头部。 |
| `var i = deck.length - 1;` | 设置 `i` 变量的初始值，指向最后一张牌。 |
| `var s;` | 用于随机选择的变量。 |
| `while (i>0) {` | 只要 `i` 大于零。 |
| `s = Math.floor(Math.random()*(i+1));` | 随机挑选。 |
| `swapindeck(s,i);` | 与 `i` 位置的牌交换。 |
| `i--;` | 递减。 |
| `}` | 关闭 `while` 循环。 |
| `}` | 关闭函数。 |
| `function swapInDeck(j,k) {` | 交换辅助函数的头部。 |
| `var hold = new MCard(deck[j].num,deck[j].suit,deck[j].picture.src);` | 保存位置 `j` 的牌。 |
| `deck[j] = deck[k];` | 将位置 `k` 的牌赋值给位置 `j`。 |
| `deck[k] = hold;` | 将 `hold` 赋值给位置 `k` 的牌。 |
| `}` | 关闭函数。 |
| `</script>` | 关闭 script 元素。 |
| *(续)* | |
| `</head>` | 关闭 `head` 元素。 |
| `<body onLoad="init();">` | 设置调用 `init` 的起始标签。 |
| `<header>` | 包含说明的 `header` 元素。 |
| `Press <b>n</b> for a new game (same deck), <b>d</b> for deal 1 more card, <b>h</b> for hold. Reload for a new deck and then press n for a new game.<br></header>` | |
| `<canvas id="canvas" width="800" height="500">` | Canvas 起始标签。 |
| `Your browser doesn't support the HTML5 element canvas.` | 对不兼容浏览器的警告。 |
| `</canvas>` | 关闭该标签。 |
| `<footer>Card images obtained courtesy of the American Contract Bridge Association, <a href="http://acbl.mybigcommerce.com/52-playing-cards/">52 playing cards </a><br/> Fisher-Yates shuffle explained at [`eli.thegreenplace.net/2010/05/28/the-intuition-behind-fisher-yates-shuffling`](http://eli.thegreenplace.net/2010/05/28/the-intuition-behind-fisher-yates-shuffling)</footer>` | 打开 `footer` 元素，该元素提供扑克牌图片的致谢和链接，并添加洗牌算法文章的致谢。 |



