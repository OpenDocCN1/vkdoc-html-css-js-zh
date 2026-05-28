# 不透明度（opacity）

`opacity`属性可以使元素及其内容透明。

`opacity : <number>`

使用 0.0 到 1.0 之间的十进制值来设置透明度。值为 1 时，元素不透明；值为 0 时，元素完全透明（不可见）。

```css
.half-transparent { opacity: 0.5; }
```

Chrome 1+、Firefox 1+、IE9+、Safari 1.2+ 和 Opera 9+ 支持此 CSS3 属性。使用以下滤镜可以极大地增强 IE 支持：

```css
.half-transparent {
    filter: alpha(opacity=50); /* IE5-8 */
    opacity: 0.5;
}
```