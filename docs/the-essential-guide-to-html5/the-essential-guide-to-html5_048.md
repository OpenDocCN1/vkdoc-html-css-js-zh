# 第 5 章 记忆（亦即专注力）游戏

**表 5-2.**（ *续*）

| **代码** | **说明** |
| --- | --- |
| `var cx = firstsx;` | 用于保存 x 坐标的变量。从第一个 x 位置开始。 |
| `var cy = firstsy;` | 用于保存 y 坐标的变量。从第一个 y 位置开始。 |
| `for(i=3;i<9;i++) {` | 循环生成从三角形到八边形的卡片。 |
| `aCard = new Card(cx,cy,cardWidth,cardHeight,i);` | 创建一张卡片并定位。 |
| `deck.push(aCard);` | 添加到牌堆中。 |
| `bCard = new Card(cx,cy+cardHeight+margin,cardWidth,cardHeight,i);` | 创建一张信息相同的卡片，但位于屏幕上上一张卡片之后。 |
| `deck.push(bCard);` | 添加到牌堆中。 |
| `cx = cx+cardWidth+ margin;` | 递增以留出卡片宽度及边距的空间。 |
| `aCard.draw();` | 在画布上绘制第一张卡片。 |
| `bCard.draw();` | 在画布上绘制第二张卡片。 |
| `}` | 关闭 for 循环。 |
| `}` | 关闭函数。 |
| `function shuffle() {` | 洗牌函数的头部。 |
| `var i;` | 用于保存对一张卡片的引用的变量。 |
| `var k;` | 用于保存对一张卡片的引用的变量。 |
| `var holder;` | 执行交换所需的变量。 |
| `var dl = deck.length;` | 用于保存牌堆中卡片数量的变量。 |
| `var nt;` | 交换次数的索引。 |
| `for (nt=0;nt<3*dl;nt++) {` | for 循环。 |
| `i = Math.floor(Math.random()*dl);` | 获取一张随机卡片。 |
| `k = Math.floor(Math.random()*dl);` | 获取一张随机卡片。 |
| `holder = deck[i].info;` | 存储卡片 i 的信息。 |
| `deck[i].info = deck[k].info;` | 将卡片 k 的信息放入卡片 i。 |
| `deck[k].info = holder;` | 将原本在 i 的信息放入卡片 k。 |
| `}` | 关闭 for 循环。 |
| `}` | 关闭函数。 |

**表 5-2.**（ *续*）

| **代码** | **说明** |
| --- | --- |
| `function Polycard(sx,sy,rad,n) {` | Polycard 函数的头部。 |
| `this.sx = sx;` | 设置 x 坐标。 |
| `this.sy = sy;` | 设置 y 坐标。 |
| `this.rad = rad;` | 设置多边形半径。 |
| `this.draw = drawPoly;` | 设置绘制方式。 |
| `this.n = n;` | 设置边数。 |
| `this.angle = (2*Math.PI)/n` | 计算并存储角度。 |
| `}` | 关闭函数。 |
| `function drawPoly() {` | 函数头部。 |
| `ctx.fillStyle= frontbgcolor;` | 设置正面背景色。 |
| `ctx.fillRect(this.sx-2*this.rad,this.sy-2*this.rad,4*this.rad,4*this.rad);` | 矩形的左上角位于多边形中心的左上方。 |
| `ctx.beginPath();` | 开始路径。 |
| `ctx.fillStyle=polyColor;` | 将颜色改为多边形颜色。 |
| `var i;` | 索引变量。 |
| `var rad = this.rad;` | 提取半径值。 |
| `ctx.moveTo(this.sx+rad*Math.cos(-.5*this.angle),this.sy+rad*Math.sin(-.5*this.angle));` | 移动到第一个点。 |
| `for (i=1;i<this.n;i++) {` | 用于后续各点的 for 循环。 |
| `ctx.lineTo(this.sx+rad*Math.cos((i-.5)*this.angle),this.sy+rad*Math.sin((i-.5)*this.angle));` | 设置线段的绘制。 |
| `}` | 关闭 for 循环。 |
| `ctx.fill();` | 填充路径。 |
| `}` | 关闭函数。 |
| `function drawBack() {` | 函数头部。 |
| `ctx.fillStyle = backColor;` | 设置卡片背面颜色。 |
| `ctx.fillRect(this.sx,this.sy,this.sWidth,this.sHeight);` | 绘制矩形。 |
| `}` | 关闭函数。 |
| `function choose(ev) {` | choose 函数（点击卡片）的头部。 |
| `var mx;` | 用于保存鼠标 x 坐标的变量。 |
| `var my;` | 用于保存鼠标 y 坐标的变量。 |
| `var pick1;` | 用于保存所创建多边形对象引用的变量。 |
| `var pick2;` | 用于保存所创建多边形对象引用的变量。 |
| `mx = ev.pageX;` | 设置 mx。 |
| `my = ev.pageY;` | 设置 my。 |
| `var i;` | 声明用于 for 循环索引的变量。 |
| `for (i=0;i<deck.length;i++){` | 遍历整个牌堆。 |
| `var card = deck[i];` | 提取一张卡片的引用以简化代码。 |
| `if (card.sx >=0)` | 检查卡片未被标记为已移除。 |
| `if ((mx>card.sx)&&(mx<card.sx+card.sWidth)&&(my>card.sy)&&(my<card.sy+card.sHeight)) {` | 然后检查鼠标是否位于此卡片之上。 |
| `if ((firstPick)||(i!=firstCard)) break;` | 如果是，检查玩家是否没有再次点击第一张卡片，若此条件为真，则退出 for 循环。 |
| `}` | 关闭 if 真值子句。 |
| `}` | 关闭 for 循环。 |
| `if (i<deck.length) {` | for 循环是否提前退出？ |
| `if (firstPick) {` | 如果是第一次选择…… |
| `firstCard = i;` | ……将 firstCard 设置为该引用的值 |



