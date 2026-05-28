# 每行加斜杠（//）并包含空行

你不必对每一行都加注释，但做好注释工作能在日后改进代码时带来便利。比注释更重要的是变量和函数的命名。

你可以通过更改表单的字体、字号、颜色和背景色来修改这款游戏。本章稍后会给出更多让应用程序个性化的方法。

使用图片的记忆游戏版本与多边形版本结构大致相同，它不需要单独的函数来绘制图片。表 5-3 列出了该版本游戏的函数。

## 第 5 章 记忆游戏（又称连连看）

**表 5-3.** 图片版记忆游戏的函数列表

| **函数** | **调用者/被调用者** | **调用内容** |
|---|---|---|
| `init` | 响应`body`标签中的`onLoad`调用 | `makeDeck`, `shuffle` |
| `choose` | 响应`init`中的`addEventListener`调用 | `flipBack`（响应`choose`中的`setTimeout`调用）, `Card`对象的`draw`方法 |
| `drawBack` | 在`makeDeck`和`flipBack`中作为卡片的`draw`方法被调用 | |
| `shuffle` | 在`init`中被调用 | |
| `makeDeck` | 在`init`中被调用 | |
| `Card` | 由`makeDeck`调用 | |

图片版记忆游戏的代码与多边形版本相似，大部分逻辑相同。但由于此示例演示了在画布上书写文字，因此 HTML 文档中没有`form`元素。代码详见表 5-4，并在有差异的行上添加了注释。我还指出了应在何处放入照片的图像文件名。在查看第二个记忆游戏版本前，请思考哪些部分可能相同，哪些可能不同。

## 第 5 章 记忆游戏（又称连连看）

**表 5-4.** 照片版记忆游戏的完整代码

| **代码** | **说明** |
|---|---|
| `<html>` | |
| `<head>` | |
| `<title>Memory game using pictures</title>` | 完整的`title`元素。 |
| `<script type="text/javascript">` | `script`元素的头部。 |
| `var ctx;` | 将保存画布的上下文。 |
| `var firstPick = true;` | 布尔值（即标记），初始值为`true`。 |
| `var firstCard = -1;` | 将保存首张卡片在牌组中的索引。`-1`是无效数字。 |
| `var secondCard;` | 将保存第二张卡片在牌组中的索引。 |
| `var backColor = "rgb(128,0,128)";` | 卡片背面颜色。 |
| `var tableColor = "rgb(255,255,255)";` | 用于擦除卡片。 |
| `var deck = [];` | `deck`数组将在`makeDeck`中填充。 |
| `var firstsx = 30;` | 第一行卡片的水平坐标。 |
| `var firstsy = 50;` | 垂直坐标。 |
| `var margin = 30;` | 卡片间距。 |
| `var cardWidth = 100;` | 如果你希望图片有不同的宽度，可能需要更改此值…… |
| `var cardHeight = 100;` | ……和/或高度。 |
| `var matched;` | |
| `var startTime;` | |
| `var finished = false;` | 用于在结尾停止多余擦除。 |
| `var count = 0;` | 用于内部计数。 |
| ( *续*) | |

## 第 5 章 记忆游戏（又称连连看）

**表 5-4.** ( *续*)

| **代码** | **说明** |
|---|---|
| `var pairs = [` | 五个人对应的图片文件配对数组。此数组包含数组，关联每个任务的两张图片。 |
| `[ "anneGorge.jpg" , "anneNow.jpg" ],` | 在此处放入你的图片文件名。 |
| `[ "esther.jpg" , "pigtailEsther.jpg" ],` | |
| `[ "pigtailJeanine.jpg" , "jeanineGorge.jpg" ],` | |
| `[ "pigtailAviva.jpg" , "avivacuba.jpg" ],` | |
| `[ "pigtailAnnika.jpg" , "annikaTooth.jpg" ]` | 你可以使用任意数量的成对图片，但请注意最后一对的数组在右括号后没有逗号。 |
| `];` | 关闭牌组数组。 |
| `function Card(sx,sy,sWidth,sHeight, img, info) {` | `Card`构造函数的头部。 |
| `this.sx = sx;` | 使用参数设置水平位置…… |
| `this.sy = sy;` | ……垂直位置。 |
| `this.sWidth = sWidth;` | ……宽度。 |
| `this.sHeight = sHeight;` | ……高度。 |
| `this.info = info;` | 指示匹配项。 |
| `this.img = img;` | 图片引用。 |
| `this.draw = drawBack;` | 设置绘制卡片背面的函数。 |
| `}` | |