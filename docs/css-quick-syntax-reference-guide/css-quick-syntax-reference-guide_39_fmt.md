# `list-style`

`list-style` 是用于设置所有列表属性的简写属性。值可以按任意顺序设置，因为它们之间没有歧义。任何一个值也可以省略，此时将使用该属性的默认值。

`list-style : <list-style-type> + <list-style-image> + <list-style-position>`

在以下示例中，设置了 `list-style` 的 `type` 和 `position` 值，这些值会继承给列表项。

```
<ul style="list-style: inside square;">
  <li>苹果</li>
  <li>橙子</li>
  <li>梨</li>
</ul>
```

请记住，列表属性不仅可以样式化列表容器 `<ul>` 和 `<ol>`，还可以样式化单个列表项 `<li>`。