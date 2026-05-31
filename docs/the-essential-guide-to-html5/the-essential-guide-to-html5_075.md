# 第八章 石头、剪刀、布

**提示** 编写程序时，如果你遇到了像`this.draw = drawThrow;`这样的代码，而你还未编写`drawThrow`函数，不必担心。你迟早会写的。有时，在创建函数或变量之前引用它们是不可避免的。关键在于，所有这些代码都要在你尝试执行程序之前编写完成。

以下是`drawThrow`方法：

```
function drawThrow() {
   ctx.strokeStyle = "rgb(0,0,0)";
   ctx.strokeRect(this.sx,this.sy,this.bWidth,this.bHeight);
   ctx.fillStyle = this.fillstyle;
   ctx.fillRect(this.sx,this.sy,this.bWidth,this.bHeight);
   ctx.drawImage(this.img,this.sx+this.sMargin,this.sy+this.sMargin,
         this.sWidth,this.sHeight);
}
```

正如所承诺的，它会使用黑色`rgb(0,0,0)`绘制一个矩形的轮廓。回想一下，`ctx`是设置好的变量，用于获取 canvas 元素的绘图属性。黑色实际上是默认颜色，因此这一行可以省略。不过，我们还是加上它，以防你在之前颜色已被更改的应用程序中重用了这段代码。接下来，该函数使用传递给该特定对象的`rectColor`绘制一个填充矩形。最后，代码在矩形顶部绘制一张图片，在水平和垂直方向上都偏移了边距量。`bWidth`和`bHeight`分别比`sWidth`和`sHeight`大两倍的`sMargin`值。这样实际上就将图片居中显示在了矩形内部。

三个按钮通过`var`语句创建为`Throw`对象，其中变量使用`new`运算符并调用`Throw`构造函数来初始化。为此，我们需要石头、剪刀、布的图片，我通过多种方式获得了它们。这三个图片文件与 HTML 文件位于同一文件夹中。

```
var rockb = new Throw(rockbx,rockby,8,50,50,"rgb(250,0,0)","rock.jpg");
var paperb = new Throw(paperbx,paperby,8,50,50,"rgb(0,200,200)","paper.gif");
var scib = new Throw(scissorsbx,scissorsby,8,50,50,"rgb(0,0,200)",
      "scissors.jpg");
```

