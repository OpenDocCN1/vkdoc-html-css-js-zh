# 基础元素

`base` 元素用于定义文档中所有其他引用应使用的基础 URL。这允许你在其他地方使用相对 URL。这可以节省一些输入，并且在需要更改基础地址时也很方便。`base` 元素支持两个特定属性：`href`，用于定义基础 URL；以及 `target`，用于指定选择链接时的默认行为。

一个 `base` 元素可能看起来像这样：

```html
<base href="www.thecreativepeople.com/html5" target="_self" />
```