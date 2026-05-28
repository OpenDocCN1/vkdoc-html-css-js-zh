# 清除（clear）

`clear`属性用于清除元素左侧、右侧或两侧的浮动元素。

`clear (block) : none | left | right | both`

此属性通常被赋予一个与属性同名的独立类。

`.clear { clear: both; }`

一个带有`clear`类的空`div`容器通常放置在浮动元素之后。这个清除元素会移动到浮动元素的下方，而不是出现在它们旁边。

`<div class="clear"></div>`

由于浮动布局往往复杂且脆弱，它们通常已被其他布局方法所取代，例如使用定位属性。

在前面给出的盒子对齐示例中，一个更好的替代方案是将盒子改为`inline-block`元素。这可以完成相同的任务，同时无需清除浮动元素。

```
.box {
    display: inline-block;
    width: 100px;
    height: 100px;
    margin: 0 1em;
    background: #ccc;
    border-radius: 10px;
}
```