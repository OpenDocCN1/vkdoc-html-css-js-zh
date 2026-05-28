# 12. 间距

以下属性涉及元素之间的间距。它们默认都会被继承。

## `line-height`

`line-height` 设置行与行之间的距离。初始值为 `normal`，通常渲染为字体大小的 120%。行高也可以设置为一个长度值、当前字体大小的百分比，或一个与当前字体大小相乘的无量纲数字。

`line-height : inherit | normal | <length> | <percentage> | <number>`

`line-height` 属性可继承，因此设置 `line-height` 的首选方式是使用无量纲数字。将 `line-height` 设置为长度或百分比，可能会对使用不同字体大小的子元素产生意想不到的结果，因为继承的行高是固定的，而不是相对于子元素字体大小的。

```css
/* 行高是字体大小的 1.5 倍 */
line-height: 1.5;
```

行高对可替换行内元素（如 `<img>`）没有影响。当用于非替换行内元素时，它会按预期设置行高。对于块级元素，`line-height` 设置元素内部行框的最小高度。

## `word-spacing` 和 `letter-spacing`

`word-spacing` 设置单词之间的间距，`letter-spacing` 设置单个字符之间的间距。这两个属性都允许使用负值。

`word-spacing : inherit | normal | <length>`

`letter-spacing : inherit | normal | <length>`

以下规则在段落内的字母之间创建 3 像素的间距，在单词之间创建 5 像素的间距：

```css
p {
  letter-spacing: 3px;
  word-spacing: 5px;
}
```

## `white-space`

`white-space` 属性改变块级元素内空白字符的处理方式。

`white-space (block) : inherit | normal | nowrap | pre | pre-wrap | pre-line`

在 HTML 中，多个空白字符通常会被合并为一个字符，并且文本会根据需要换行以填满包含它的块级元素的宽度。

```css
/* 换行文本，并折叠换行符、空格和制表符 */
p { white-space: normal; }
```

将 `white-space` 设置为 `nowrap` 会阻止文本换行，除非遇到换行标签 `<br>`。`pre`（预格式化）值也会阻止换行，但它会保留所有空白字符。其行为与 HTML 中的 `<pre>` 元素相同。

`pre-wrap` 和 `pre-line` 值都允许文本正常换行，`pre-wrap` 会保留空白字符序列，而 `pre-line` 则会折叠它们。`pre-line` 与 `normal` 的区别在于，`pre-line` 会保留换行符。请注意，IE 浏览器直到版本 8 才支持最后这两个值。