# 黑杰克代码解释

## 表 10-2 代码解释（续）

`return true;`

如果是，返回 `true`，表示可以再拿一张牌。

`}`

结束该子句。

`else {`

开始 else 子句。

`return false;`

返回 `false`，表示庄家不再拿牌。

`}`

结束 else 子句。

`}`

结束该函数。

---

`function dealFromDeck(who)` 从牌堆发牌的函数头。

`{`

`var card;`

存放牌。

`var ch = 0;`

存放下一张未发牌的索引。

`while ((deck[ch].`

询问这张牌是否已被发出。

`dealt>0)&&(ch<51)) {`

`ch++;`

增加 `ch` 以进入下一张牌。

`}`

结束 while 循环。

`if (ch>=51) {`

询问是否没有未发的牌了。

`ctx.fillText("NO MORE`

直接在画布上显示一条消息。

`CARDS IN DECK. Reload. ", 200, 250);`

`ch = 51;`

将 `ch` 设置为 51 以使该函数正常工作。

`gameStart = false;`

阻止响应任何玩家要新牌的请求。

`}`

结束 if 真子句。

`deck[ch].dealt = who;`

存储 `who`（一个非零值），以此标记该牌已被发出。

`card = deck[ch];`

设置一张牌。

`return card;`

返回一张牌。

---

## 表 10-2 代码解释（续）

**代码** | **解释**

--- | ---

`}` | 结束该函数。

`function buildDeck() {` | 构建 `MCard` 对象的函数头。

`var n;` | 用于内部迭代的变量。

`var si;` | 用于外部迭代（遍历花色）的变量。

`var suitnames= ["clubs", "hearts","spades", "diamonds"];` | 花色的名称。

`var i;` | 追踪放入牌堆数组的元素。

`i=0;` | 将数组初始化为 0。

`var pickName;` | 简化编码。

`var nums=["a","2","3","4", "5","6","7","8","9","10", "j","q","k"];` | 所有牌的名称。

`for (si=0;si<4;si++) {` | 遍历花色。

`for (n=0;n<13;n++) {` | 遍历同花色中的牌。

`pickName=suitNames[si]+"-"+nums[n]+"-75.png";` | 构造文件名。

`deck[i]=new MCard(n+1, suitNames[si],pickName);` | 用指定的值构造一个 `MCard` 对象。

`i++;` | 递增 `i`。

`}` | 结束内部 for 循环。

`}` | 结束外部 for 循环。

`}` | 结束该函数。

---

## 表 10-2 代码解释（续）

**代码** | **解释**

--- | ---

`function MCard(n, s, pickName){` | 用于创建对象的构造函数头。

`this.num = n;` | 设置 `num` 值。

`if (n>10) n = 10;` | 对花牌进行调整。

`this.value = n;` | 设置 `value`。

`this.suit = s;` | 设置 `suit`。

`this.picture = new Image();` | 创建一个新的 `Image` 对象并将其赋值为一个属性。

`this.picture.src = pickName;` | 将此 `Image` 对象的 `src` 属性设置为图片文件名。

`this.dealt = 0;` | 将 `dealt` 属性初始化为 0。

`}` | 结束该函数。

---

## 表 10-2 代码解释（续）

**代码** | **解释**

--- | ---

`function add_up_player() {` | 确定玩家手牌点数的函数头。

`var ac = 0;` | 存放 A 牌的数量。

`var i;` | 用于迭代。

`var sumUp = 0;` | 初始化总和。

`for (i=0;i<pi;i++) {` | 遍历玩家手中的牌。

`sumUp += playerHand[i].value;` | 累加玩家手牌的点数。

`if (playerHand[i].value==1) {ac++;}` | 询问牌是否为 A 牌。增加 A 牌计数。

`}` | 结束 for 循环。

`if (ac>0) {` | 询问是否有任何 A 牌。

`if ((sumUp+10)<=21) {` | 如果这不会导致总和超过 21。

`sumUp +=10;` | 将一张 A 牌改为 11 点。

`}` | 结束内部 if。

`}` | 结束外部 if。

`return sumUp;` | 返回总和。

`}` | 结束该函数。

---

## 表 10-2 代码解释（续）

**代码** | **解释**

--- | ---

`function playerDone() {` | 当玩家表示停牌时调用的函数头。

`If (gameStart) {` | 检查游戏是否已开始。

`while(more_to_house()) {` | `more_to_house()` 函数指示庄家应该再拿一张牌。

`houseHand[hi] = dealFromDeck(2);` | 给庄家发一张牌。

`ctx.drawImage(back,houseXp, houseYp,cardw,cardh);` | 在画布上绘制这张牌。

`houseXp = houseXp+20;` | 调整水平指针。

`hi++;` | 增加庄家手牌的索引。

`}` | 结束 while 循环。

`showHouse();` | 亮出庄家的手牌。

`playerTotal = add_up_player();` | 确定玩家的总点数。

`if (playerTotal>21){` | 询问玩家是否爆牌。

`if (houseTotal>21) {` | 询问庄家是否爆牌。

`ctx.fillText("You and house both went over.",30,100);` | 显示一条消息。

`}`