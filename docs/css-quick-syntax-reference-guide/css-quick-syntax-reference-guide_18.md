# 11. 文本

文本属性用于格式化文本内容的视觉外观。

## color

`color` 属性通过使用某种颜色表示法来设置文本的颜色。默认情况下，其值设置为 `inherit`，意味着它继承父元素的颜色。

```
color : inherit | <color>
```

所有主流浏览器的初始值都是黑色。在下面的示例规则中，段落将被设置为蓝色：

```
p { color: #00f; }
```

## text-transform

`text-transform` 控制文本的大小写。可能的取值如下所列，其中 `none` 为初始值：

```
text-transform : inherit | none | uppercase | lowercase | capitalize
```

此属性可以将文本转换为大写或小写字母。`capitalize` 值会将每个单词的首字母大写。此属性默认继承，因此可以使用 `none` 值来移除继承的 `text-transform` 效果。

```
text-transform: none; /* 移除效果 */
```

## text-decoration

可以使用 `text-decoration` 属性为文本添加一个或多个视觉效果。

```
text-decoration : none | underline + overline + line-through + blink
```

若要添加多个装饰效果，请用空格分隔这些值（如之前使用“`+`”号所示）。以下规则为应用此类的文本内容添加了上划线和下划线：

```
.highlight { text-decoration: underline overline; }
```

此属性不会继承，但它的效果会以类似于继承的方式渲染到后代行内元素上。

## text-indent

块级元素中第一行文本可以使用 `text-indent` 属性进行缩进。它可以设置为一个度量单位或父元素宽度的百分比。也可以通过使用负值来实现反向缩进。

```
text-indent (块级) : inherit | <length> | <percentage>
```

以下示例将段落元素的第一行缩进一个 em 的宽度：

```
p { text-indent: 1em; }
```

## text-align

块级元素的文本内容可以使用 `text-align` 属性进行对齐。此属性可以替代 HTML 中已废弃的 `align` 属性的用法。

```
text-align (块级) : inherit | left | center | right | justify
```

文本和行内元素可以左对齐、右对齐或居中对齐。`justify` 值还会拉伸每一行，以使左右边距看起来都整齐。

```
p { text-align: justify; }
```

`text-align` 属性会继承，因此需要在子元素中显式更改它，以恢复默认的左对齐。

## direction

文本的书写方向可以使用 `direction` 属性进行切换。

```
direction (块级) : inherit | ltr | rtl
```

默认值是 `ltr`，表示从左到右。可以将其更改为 `rtl`，以使块级元素内的文本内容从右向左排列。这表明文本应从右向左阅读，例如希伯来语或阿拉伯语文本。

```
<p style="direction: rtl">
  从右到左对齐
</p>
```

此属性会继承，因此可以在 `<body>` 元素上设置一次，使其应用于整个网页。



## `text-shadow`

可以使用 `text-shadow` 属性为文本添加阴影效果。

`text-shadow : inherit | none | <offset-x> <offset-y>` `[<blur-radius>] [<color>]`

阴影通过两个偏移值来定义，后跟两个可选值：模糊半径和颜色。X 和 Y 偏移量指定为相对于文本的长度值。正值会使阴影向右和向下移动；负值则使其向左和向上移动。

通过设置模糊半径可以添加模糊效果，这会使阴影边缘拉伸并淡出。该属性的最后一个可选值是阴影的颜色。如果未指定颜色值，大多数浏览器会以与文本相同的颜色渲染阴影。以下示例样式会在 `<h1>` 元素的右上角显示一个略微模糊的灰色阴影：

`h1 { text-shadow: 1px -1px 1px gray; }`

`text-shadow` 是一个 CSS 3 属性，受大多数主流浏览器支持，包括 Chrome 2+、Firefox 3.5+、IE10+、Safari 1.2+ 和 Opera 9.5+。

## `box-shadow`

除了文本，还可以使用 `box-shadow` 属性为块级元素添加阴影效果。

`box-shadow (block) : inherit | none | [inset]` `<offset-x> <offset-y>`

`[<blur-radius>] [<spread-radius> [<color>]]`

盒阴影的值与 `text-shadow` 的值相同——有两个例外。可以指定第四个长度值 `spread-radius` 来放大或缩小阴影。此值是可选的，若未指定则默认为 0，阴影与元素大小相同。例如，以下类规则会在任何使用此类的块级元素的右下角显示一个模糊的灰色阴影：

`.drop-shadow { box-shadow: 3px 3px 3px 6px #ccc; }`

`box-shadow` 属性独有的第二个值是 `inset` 关键字。如果存在，阴影将显示在框内部，而不是作为外部投影。

`.inset-shadow { box-shadow: inset 3px 3px 3px 6px #ccc; }`

`box-shadow` 是一个 CSS 3 属性，已在 Chrome 10+、Firefox 4+、IE9+、Safari 5.1+ 和 Opera 10.5+ 中实现。可以使用 `-webkit` 和 `-moz` 前缀来扩展支持，如下所示：

```
.drop-shadow
{
  /* Chrome 1-5, Safari 2-5.1+ */
  -webkit-box-shadow: 3px 3px 5px 6px #ccc;
  /* Firefox 3.5-3.6 */
  -moz-box-shadow: 3px 3px 5px 6px #ccc;
  box-shadow: 3px 3px 5px 6px #ccc;
}
```

