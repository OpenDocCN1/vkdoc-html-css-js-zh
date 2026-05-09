# 这是某州中学官方拼写比赛单词表。我确实需要在 Excel 中进行一些操作才能生成 JavaScript 代码。一个增强版程序可以包含多个文件，让玩家在不同关卡或语言之间进行选择。

##### 生成并定位 HTML 标记，然后将标记转换为按钮，最后禁用按钮

字母按钮和秘密单词破折号的创建是通过 JavaScript 和 CSS 的组合完成的。

我们将编写代码为程序的两个部分创建 HTML 标记：字母图标和秘密单词的空白位。（你可以参考[第 6 章](http://dx.doi.org/10.1007/978-1-4842-4155-4_6)的猜谜游戏，了解更多关于创建 HTML 标记以及附录的内容。）每种情况下，都使用以下内置方法创建 HTML 标记：

- `document.createElement(x)`: 为新元素类型`x`创建 HTML 标记
- `document.body.appendChild(d)`: 将`d`元素作为 body 元素的另一个子元素添加
- `document.getElementById(id)`: 提取 ID 值为`id`的元素

创建的 HTML 包含每个元素的唯一 ID。代码涉及设置某些属性：

- `d.innerHTML` 用于保存 HTML 内容
- `thingelem.style.top` 用于设置垂直位置
- `thingelem.style.left` 用于设置水平位置

## 第九章 猜单词

有了这些背景知识，下面是设置字母按钮的编码。我们首先声明一个全局变量`alphabet`：

```
var alphabet = "abcdefghijklmnopqrstuvwxyz";
```

`setupGame`函数中包含用于创建字母按钮的代码：

```
var i;
var x;
var y;
var uniqueid;
var an = alphabet.length;
for(i=0;i<an;i++) {
    uniqueid = "a"+String(i);
    d = document.createElement('alphabet');
    d.innerHTML = (
      "<div class="letters" id='"+uniqueid+"'>"+alphabet[i]+"</div>");
    document.body.appendChild(d);
    thingelem = document.getElementById(uniqueid);
    x = alphabetx + alphabetWidth*i;
    y = alphabety;
    thingelem.style.top = String(y)+"px";
    thingelem.style.left = String(x)+"px";
    thingelem.addEventListener('click',pickElement,false);
}
```

变量`i`用于遍历字母字符串。唯一 ID 是`a`与索引值的拼接，索引值从 0 到 25。插入到创建元素中的 HTML 是一个包含字母文本的`div`。字符串用双引号括起来，而字符串内的属性用单引号括起来。元素在屏幕上水平排列，起始位置为`alphabetx`、`alphabety`（每个全局变量在文档中更早声明），并按`alphabetWidth`水平递增。`top`和`left`属性需要设置为字符串，并以`"px"`结尾以表示像素。最后一步是设置事件处理，使这些元素充当按钮。

## 第九章 猜单词

秘密单词元素的创建与此类似。不同之处在于每个元素的文本内容都是两个下划线。在屏幕上，这两个下划线看起来像一个长下划线。`ch`（代表选择）的赋值是程序选择秘密单词的方式。注意`length`是`String`数据类型对象以及数组的属性。在本例中，我将`length`用于单词列表。如果我的列表超过四个元素，这段代码仍然有效。

```
var ch = Math.floor(Math.random()* words.length);
secret = words[ch];
for (i=0;i<secret.length;i++) {
    uniqueid = "s"+String(i);
    d = document.createElement('secret');
    d.innerHTML = (
      "<div class="blanks" id='"+uniqueid+"'> __ </div>");
    document.body.appendChild(d);
    thingelem = document.getElementById(uniqueid);
    x = secretx + secretwidth*i;
    y = secrety;
    thingelem.style.top = String(y)+"px";
    thingelem.style.left = String(x)+"px";
}
```

看到这里，你可能会问：字母图标是如何变成带边框的块状字母的？答案是使用了 CSS。CSS 的用途远不止字体和颜色。样式决定了游戏关键部分的外观和感觉。注意，字母`div`元素的类设置为`'letters'`，而秘密单词字母`div`元素的类设置为`'blanks'`。样式部分包含以下两种样式，为了便于阅读我进行了分组。换行对浏览器没有影响。

```
<style>
.letters {
  position:absolute;
  left: 0px; top: 0px;
  border: 2px; border-style: double;
  margin: 5px; padding: 5px;
  font-size: 24px;
  color:#F00; background-color:#0FC;
  font-family:"Courier New", Courier, monospace;
}
.blanks {
  position:absolute;
  left: 0px; top: 0px;
  border:none; margin: 5px; padding: 5px;
  color:#006; background-color:white;
  font-family:"Courier New", Courier, monospace;
  text-decoration:underline;
  color: black; font-size:24px;
}
</style>
```

点号后跟名称的指定方式意味着此样式适用于该类的所有元素。这与仅使用名称（如上一章中的`form`，其中样式应用于所有`form`元素）或使用`#`后跟名称（指向文档中 ID 为该名称的唯一元素）不同。注意`letters`的样式包含边框、颜色和背景色。指定字体族是一种为任务选择首选字体，并在该字体不可用时指定备用字体。CSS 的这一功能为设计师提供了广阔的自由度。我在这里的选择是`"Courier New"`，第二选择是`Courier`，第三选择是可用的任何等宽字体（在*等宽字体*中，所有字母的宽度相同）。我决定使用等宽字体，以便于制作大小相同、在屏幕上整齐排列的图标。`margin`属性设置边框外部的间距，而`padding`指文本与边框之间的间距。

我们希望代表字母的按钮在被点击后消失。`pickElement`函数中的代码可以使用`this`来引用被点击的对象。以下两条语句（可以合并为一条）通过设置`display`属性来实现此效果：

```
var id = this.id;
document.getElementById(id).style.display = "none";
```

## 第九章 猜单词

当游戏结束时，无论是赢还是输，我们通过遍历所有元素来移除所有字母的点击事件处理：

```
for (j=0;j<alphabet.length;j++) {
    uniqueid = "a"+String(j);
    thingelem = document.getElementById(uniqueid);
    thingelem.removeEventListener('click',pickElement,false);
}
```

`removeEventListener`事件如其名所示：它移除事件处理。

##### 创建剩余错误次数的反馈

我决定通过文本和图片两种方式向玩家提供反馈。文本可以被屏幕阅读器读取，而图片则能带来强烈的视觉冲击。我选择用一叠矩形来表示剩余允许猜错的字母数量。文本和矩形堆叠都位于画布上。定位、显示和擦除需要一些坐标值的调整，但由于字母按钮和秘密单词未写在画布上，这一过程大为简化。

绘制矩形堆叠的代码如下：

```
function drawRemain(remain){
    ctx.rect(startRx,startRy,unitX+10,5);
    var ypos=startRy-unitY;
    for (i=0;i<remain;i++){
        ctx.strokeRect(startRx,ypos,unitX,unitY);
        ypos=ypos-unitY;
    }
}
```

**提示：** 如果你还没有尝试过绘图（即使尝试过了也请再试一次），可以尝试用另一种方式来传达剩余猜错次数。

##### 维护游戏状态并判定胜负



