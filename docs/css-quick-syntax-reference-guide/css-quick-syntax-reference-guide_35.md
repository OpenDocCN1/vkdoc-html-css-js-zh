# 浮动（float）

`float`属性将一个元素从其包含元素中分离出来，使其浮动在其上方，可以向左或向右浮动。它旨在使文本环绕图像，但也常用于制作布局。浮动一个内联元素会自动将其更改为块级元素。

`float : none | left | right`

要使文本和其他内联内容环绕图像，您可以将其向左或向右浮动。

`<img style="float: left;" src="myimage.png" alt="">`

对于布局，浮动允许块级元素水平排列。例如，可以使用以下类创建盒子网格：

```
.box {
    float: left;
    width: 100px;
    height: 100px;
    margin: 0 1em;
    background: #ccc;
    border-radius: 10px;
}
```

这个类使盒子水平堆叠，而不是垂直堆叠（块级元素的正常行为）（参见图 22-2）。

```
<div class="box"></div>
<div class="box"></div>
<div class="box"></div>
```

![A978-1-4302-6491-0_22_Fig2_HTML.jpg](img/A978-1-4302-6491-0_22_Fig2_HTML.jpg)

图 22-2. 浮动的盒子

使用浮动的一个副作用是，任何跟随这些浮动盒子的元素也会水平排列。`clear`属性旨在停止这种行为。

