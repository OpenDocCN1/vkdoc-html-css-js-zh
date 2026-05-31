# 第 5 章：记忆（又名专注力）游戏

***表 5-4.***(续)

**代码**

**说明**

```javascript
function makeDeck() {
```
`makeDeck` 函数的头部。

```javascript
var i;
```
在循环中使用。

```javascript
var acard;
```
两张配对卡片中的第一张……

```javascript
var bcard;
```
……第二张。

```javascript
var pica;
```
将放入 `acard` 的图片……

```javascript
var picb;
```
…… `bcard` 的图片。

```javascript
var cx = firstsx;
```
水平位置。

```javascript
var cy = firstsy;
```
垂直位置。

```javascript
for(i=0;i<pairs.length;i++) {
```
循环遍历 `pairs` 数组以提取信息。

```javascript
pica = new Image();
```
创建 `Image` 对象。

```javascript
pica.src = pairs[i][0];
```
设置为第一个文件。

```javascript
acard = new Card(cx,cy,cardWidth,cardHeight,pica,i);
```
创建 `Card`。

```javascript
deck.push(acard);
```
将卡片添加到牌组。

```javascript
picb = new Image();
```
创建 `Image` 对象。

```javascript
picb.src = pairs[i][1];
```
设置为第二个图片文件。

```javascript
bcard = new Card(cx,cy+cardHeight+margin,cardWidth,cardHeight,picb,i);
```
创建 `Card`。请注意，`acard` 和 `bcard` 在将要存储在 `info` 属性中的参数上具有相同的值。还要注意，它们在垂直方向上一上一下。

```javascript
deck.push(bcard);
```
将卡片添加到牌组。

```javascript
cx = cx+cardWidth+ margin;
```
准备处理下一对。

```javascript
acard.draw();
```
绘制 `acard`。

(续)

***表 5-4.***(续)

**代码**

**说明**

```javascript
bcard.draw();
```
绘制 `bcard`。

```javascript
}
```
关闭循环。

```javascript
}
```
关闭 `makeDeck`。

```javascript
function shuffle() {
```
`shuffle` 函数的头部。

```javascript
var i;
var k;
var holderInfo;
```
用于交换的临时存储。

```javascript
var holderImg;
```
用于交换的临时存储。

```javascript
var dl = deck.length
```
卡片数量。

```javascript
var nt;
```
交换次数。

```javascript
for (nt=0;nt<3*dl;nt++) {
```
执行交换，次数为牌组长度的 3 倍。

```javascript
i = Math.floor(Math.random()*dl);
```
选择两个随机数。

```javascript
k = Math.floor(Math.random()*dl);
```
它们相同也没关系。

```javascript
holderInfo = deck[i].info;
```
保存 `info`。

```javascript
holderImg = deck[i].img;
```
保存 `img`。

```javascript
deck[i].info = deck[k].info;
```
将 `k` 的 `info` 放入 `i`。

```javascript
deck[i].img = deck[k].img;
```
将 `k` 的 `img` 放入 `i`。

```javascript
deck[k].info = holderInfo;
```
设置为原来的 `info`。

```javascript
deck[k].img = holderImg;
```
设置为原来的 `img`。

```javascript
}
```
关闭 `for` 循环。

```javascript
}
```
关闭 `shuffle`。

```javascript
function drawBack() {
```
`drawBack` 函数的头部。

```javascript
ctx.fillStyle = backColor;
```
设置为牌背的颜色。

```javascript
ctx.fillRect(this.sx,this.sy,this.sWidth,this.sHeight);
```
绘制一个矩形。

```javascript
}
```
关闭 `drawBack`。

```javascript
function choose(ev) {
```
`choose` 函数的头部。此函数由鼠标点击的事件处理程序调用。

```javascript
var out;
```
用于要显示的消息。

```javascript
var mx;
```
鼠标的 x 坐标。

```javascript
var my;
```
鼠标的 y 坐标。

```javascript
var pick1;
```
第一次选择。

```javascript
var pick2;
```
第二次选择。

```javascript
mx = ev.pageX;
```
从事件 `ev` 中提取 x 坐标。

```javascript
my = ev.pageY;
```
提取 y 坐标。

```javascript
var i;
```
用于循环。

```javascript
for (i=0;i<deck.length;i++){
```
`for` 循环遍历牌组，判断哪张卡片被点击了。

```javascript
var card = deck[i];
```
提取一张卡片。这简化了其余代码。

```javascript
if (card.sx >=0)
```
这是避免检查点击此空间的方法。

```javascript
if ((mx>card.sx)&&(mx<card.sx+card.sWidth)&&(my>card.sy)&&(my<card.sy+card.sHeight)) {
```
检查是否在给定的卡片上。

```javascript
if ((firstPick)|| (i==firstCard)) {
```
如果是 `firstCard` 或玩家两次选择了同一张卡片，则离开 `for` 循环。

```javascript
break;}
```
离开循环。

```javascript
}
```
关闭在卡片上的检查。

```javascript
}
```
关闭遍历牌组的循环。

```javascript
if (i<deck.length) {
```
卡片在牌组中。

```javascript
if (firstPick) {
```
如果是第一次选择。

```javascript
firstCard = i;
```
现在将 `firstCard` 设置为卡片索引。

```javascript
firstPick = false;
```
将 `firstPick` 设置为 `false`。

```javascript
ctx.drawImage(card.img,card.sx,card.sy,card.sWidth,card.sHeight);
```
绘制图片。

```javascript
}
```
关闭首次选择的判断。

```javascript
else {
secondCard = i;
```
这是第二次选择。

```javascript
ctx.drawImage(card.img,card.sx,card.sy,card.sWidth,card.sHeight);
```
绘制图片。

```javascript
if ([card.info](http://card.info) ==deck[firstCard].info) {
```
使用 `info` 字段检查是否匹配。

```javascript
matched = true;
```
将 `matched` 设置为 `true`。

```javascript
count++;
```
递增 `count`。

```javascript
ctx.fillStyle= tableColor;
```
这将擦除已显示的卡片。

```javascript
ctx.fillRect(10,340,900,100);
```
擦除文本将出现的区域。

```javascript
ctx.fillStyle=backColor;
```
重置为文本的颜色。



