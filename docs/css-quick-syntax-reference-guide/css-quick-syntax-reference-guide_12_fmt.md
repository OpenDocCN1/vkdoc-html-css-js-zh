# root

`:root` 伪类匹配文档树中的顶层元素。在 HTML 文档中，它始终是 `<html>` 元素。

```css
:root {} /* 根元素 */
```

当 CSS 与其他语言（如 XML）一起使用时，该伪类尤其有用，因为在这些语言中，根元素可能会发生变化。除 IE8 及更低版本外，所有主流浏览器均支持 `:root` 伪类。

## 用户界面伪类

CSS 3 引入了一些用户界面伪类，用于根据交互元素的当前状态为其设置样式。

### enabled 和 disabled

`:enabled` 和 `:disabled` 伪类匹配选定类型中处于启用或禁用状态的任何元素。它们仅适用于可处于启用或禁用状态的交互元素，例如表单元素。

```css
input:enabled  { background: green; }
input:disabled { background: red; }
```

以下表单包含一个启用和一个禁用的输入元素，它们受这两条规则影响：

```html
<form>
  <input type="text" name="enabled">
  <input type="text" name="disabled" disabled>
</form>
```

除 IE8 及更低版本外，所有主流浏览器均支持这两个伪类。

### checked

`:checked` 伪类匹配处于选中状态的元素。它仅可用于复选框、单选按钮和 `<option>` 元素。

```css
input[type="checkbox"]:checked {}
```

此规则匹配网页上任何选中的复选框。

```html
<form>
  <input type="checkbox">
</form>
```

除 IE8 及更早版本外，所有主流浏览器均支持 `:checked` 伪类。

### valid 和 invalid

`:valid` 和 `:invalid` 伪类用于在用户填写表单时向其提供反馈。现代浏览器可以根据表单元素的输入类型执行基本字段验证，并且结合这些伪类，结果可用于设置输入元素的样式。

```css
input:valid   { background: green; }
input:invalid { background: red; }
```

这里给出了两个字段，一个是必填项，一个是可选项。第一个字段在输入电子邮件地址之前一直保持无效状态。第二个字段是可选的，因此如果留空则视为有效。

```html
<form>
  <input type="email" required>
  <input type="email">
</form>
```

请注意，这些伪类绝不能替代使用 JavaScript 或 PHP 等进行的正确表单验证。这两个伪类的浏览器支持情况为 Chrome 10+、Firefox 4+、IE10+、Safari 5+ 和 Opera 10+。

### required 和 optional

设置了 `required` 属性的表单字段由 `:required` 伪类匹配。相关的 `:optional` 伪类则相反：它匹配未设置 `required` 属性的输入元素。

```css
input:required { color: red; }
input:optional { color: gray; }
```

以下表单包含一个必填和一个可选的输入元素，它们受前述样式影响：

```html
<form>
  <input type="email" required>
  <input type="url">
</form>
```

与 `:valid` 和 `:invalid` 伪类类似，对 `:required` 和 `:optional` 的支持仅限于 Chrome 10+、Firefox 4+、IE10+、Safari 5+ 和 Opera 10+。

## 其他伪类

有些伪类不属于上述任何类别，即 `:target`、`:lang` 和 `:not` 伪类。

### target

`:target` 伪类可以为通过 `id` 链接定位到的元素设置样式。它对于突出显示文档中的目标部分很有用。

```css
:target { font-weight: bold; } /* 目标元素 */
```

当点击以下页面内部链接时，此规则将应用于该锚点元素。浏览器还会滚动到该元素所在位置。

```html
<a href="#my-target" id="my-target">页面内部链接</a>
```

除 IE8 及更早版本外，所有主流浏览器均支持此伪类。

### lang

`:lang()` 伪类匹配被确定为使用参数指定语言编写的元素。

```css
p:lang(en) {}
```

此伪类适用于面向英语受众的段落元素，例如以下段落：

```html
<p lang="en">English</p>
```

请注意，此伪类的行为与语言属性选择器类似。不同之处在于，`:lang` 伪类还会在语言设置于祖先元素上，或以其他方式（例如通过页面 HTTP 头部或 `<meta>` 标签）设置时匹配元素。

```html
<body lang="fr">
  <p>French</p>
</body>
```

### not

否定伪类 `:not` 匹配未由指定选择器定位到的元素。

```css
p:not(.first) { font-weight: bold; }
```

此示例规则选择未使用 `first` 类的段落。

```html
<p class="first">不加粗</p>
<p>加粗</p>
```

除 IE8 及更早版本外，所有主流浏览器均支持 `:not` 伪类。