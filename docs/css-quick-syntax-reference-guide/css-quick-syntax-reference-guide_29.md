# 20. 尺寸

尺寸属性控制元素的大小及其最小和最大尺寸。这些属性不具有继承性，且只能应用于块级元素和可替换内联元素。

## width 和 height

元素内容区域的宽度和高度可以通过 `width` 和 `height` 属性来设置。这两个属性可以赋予长度值或百分比值，其中百分比是相对于父元素的尺寸而言的。

`width | height : auto | <length> | <percentage>`

块级元素通常会伸展至其容器的边缘。相比之下，元素的高度会收缩以适应其内容。通过使用 `width` 和 `height` 属性，可以改变这些默认行为。在下面的示例中，应用了该类的元素具有 100 像素的宽度和 50 像素的高度。像这样设置尺寸后，无论页面如何调整大小，该元素都会保持该尺寸。

```
.mybox {
  width: 100px;
  height: 50px;
}
```

## min-width 和 min-height

`min-width` 和 `min-height` 属性设置元素的最小尺寸。元素的宽度和高度仍然会扩展以适应内容，但元素不会收缩到低于指定的最小尺寸，这最小尺寸不包括内边距、边框或外边距。

`min-width | min-height : <length> | <percentage>`

这些属性的有效值是长度和百分比，其中百分比是相对于包含块的尺寸而言的。例如，以下类使元素至少占据可用宽度和高度的一半：

```
.half {
  min-width: 50%;
  min-height: 50%;
}
```

## max-width 和 max-height

元素内容区域的最大尺寸通过 `max-width` 和 `max-height` 属性来设置。它们可以设置为长度值或百分比值，以及关键字 `none` 来清除之前设置的值。

`max-width | max-height : none | <length> | <percentage>`

通过同时设置最大和最小宽度，你可以定义一个元素宽度可变动的区间。使用以下类的块级元素在空间允许时会扩展到 500 像素。当水平空间受限时，该元素允许收缩到不低于 200 像素。

```
.mybox {
  max-width: 500px;
  min-width: 200px;
}
```

`max-width` 属性优先级高于 `min-width`。然而，对于高度属性则相反，因为 `min-height` 优先级高于 `max-height`。因此，使用以下类的元素高度为 5 em，除非其内容需要更多高度。在这种情况下，元素会垂直扩展，直到其允许的最大值 20 em。

```
.mybox {
  max-height: 20em;
  min-height: 5em;
}
```

请记住，固定的 `width` 和 `height` 属性不应与 `min-` 和 `max-` 属性一起使用。所有主流浏览器都支持这四个 `min-` 和 `max-` 属性，包括 Chrome 1+、Firefox 1+、IE7+、Safari 1+ 和 Opera 8+。它们常与媒体规则一起使用，以创建在不同屏幕尺寸下都能良好运行的流式布局。

## box-sizing

尺寸属性通常指的是内容区域，而不是内边距或边框区域。因此，要知道一个元素在盒模型中实际占据的宽度或高度，必须考虑其周围的内边距和边框。

```
/* 100 像素宽的元素 */
.mybox {
  padding: 3px;
  border: 2px solid red;
  width: 90px;
}
```

CSS 3 引入了 `box-sizing` 属性，允许网页开发者选择如何计算宽度和高度。默认值是 `content-box`，这意味着尺寸属性仅指内容区域。另一种 `border-box` 值则在测量中包含内边距和边框。

`box-sizing : content-box | border-box`

通过将盒模型尺寸计算方式更改为 `border-box`，你可以更轻松地创建网格布局，因为不再需要考虑内边距和边框的大小。

```
/* 100 像素宽的元素 */
.mybox {
  box-sizing: border-box;
  padding: 3px;
  border: 2px solid red;
  width: 100px;
}
```

`border-box` 属性不具有继承性，但可以使用通配选择器将其应用于网站上的所有元素。为了增加浏览器兼容性，应使用 `-webkit` 和 `-moz` 前缀。

```
/* 对所有元素使用 border-box */
* {
  /* Chrome 1-8, Safari 3-5 */
  -webkit-box-sizing: border-box;
  /* Firefox 1-28*/
  -moz-box-sizing: border-box;
  /* Chrome 9+, Firefox 29+, IE8+, Safari 5.1+, Opera 9.5+ */
  box-sizing: border-box;
}
```

`box-sizing` 属性的浏览器支持已经非常好，所有主流浏览器现在都支持它。因此，许多新网站已开始采用此属性来简化其布局的网格计算。

