# 第 8 章 ■ HTML 表单元素

## 自动填充详情令牌

然而，仅仅开启自动填充功能还不够。例如，如果我正在输入电子邮件地址，我不希望在自动填充列表中看到姓名、地址或电话号码。你不应将`autocomplete`属性设置为`on`，而应将其设置为特定的自动填充详情令牌，在本例中为`email`。这样，自动填充列表将只包含在其他具有相同自动填充详情令牌的字段中输入的值。

目前已经定义了一个相当长的自动填充详情令牌列表，包括电话号码、地址或支付信息的各个部分，以及许多其他类型的信息。与其在这里重复所有信息，最好直接查阅规范，你可以在[`html.spec.whatwg.org/multipage/forms.html#autofill`](https://html.spec.whatwg.org/multipage/forms.html#autofill)找到它。这也是获取最新信息的最佳途径。

## 使用 `datalist` 元素

你还可以使用`datalist`元素提供自定义的自动填充列表。`datalist`元素包含一组`option`元素。然后，你可以使用`list`属性将`datalist`分配给`input`元素。

你在`datalist`中包含的任何值现在都会出现在自动填充建议中。以下是一个简单的示例：

```
<datalist id="sports">
  <option value="Baseball" />
  <option value="Basketball" />
  <option value="Hockey" />
  <option value="Football" />
</datalist>

<label for="iSport">最喜欢的运动:</label>
<input type="text" id="iSport" name="Sport" list="sports" />
```

### 示例说明

`datalist`元素包含四个值，它们是四种热门运动的名称。`datalist`通过其`id`属性被引用，我将其`id`指定为`sports`。然后，在`input`元素中，我添加了`list`属性：`list="sports"`。

带有预填充`datalist`的文本框与你使用`select`元素创建的下拉列表不同，这一点我稍后会解释。用户不仅限于这些选项；他们可以输入任何想要的内容。这只是在需要一些常见值时让输入稍微方便一些。建议列表会随着你的输入自动过滤。例如，输入字母“b”后，只有 Baseball 和 Basketball 会保留下来供选择。此外，如果自动完成功能已启用，基于之前的输入，建议列表还会添加额外的条目。例如，我之前输入过`tennis`，现在文本框看起来就像图 8-3 所示。

**图 8-3.** 使用预填充的`datalist`