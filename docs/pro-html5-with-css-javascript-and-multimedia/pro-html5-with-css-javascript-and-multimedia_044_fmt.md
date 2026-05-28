# 连字符

`<wbr>`元素标识了单词在必要时可以断开并换行到下一行的逻辑位置。你也可以使用连字符或*软连字*来实现这一点。

连字符（`-`）即使单词不需要换行也会显示出来。然而，软连字是一个插入到单词中的不可见字符。它使用 `&shy;` 输入。连字符和软连字都会告诉浏览器换行点可以出现在哪里。软连字仅在单词实际在该位置换行时才显示出来。

■ **提示** 你也可以使用 Unicode 表示法 `&#173;` 来指定软连字。

为了尝试这一点，让我们修改示例 HTML，使用软连字来包含第三个 `Supercalifragilisticexpialidocious` 实例。

```html
<p style="width: 70px; word-wrap: break-word">
  Supercalifragilisticexpialidocious<br />
  --<br />
  Super<wbr />cali<wbr />fragilistic<wbr />expiali<wbr />docious<br />
  --<br />
  Super&shy;cali&shy;fragilistic&shy;expiali&shy;docious
</p>
```

呈现结果将如图 5-9 所示。

***图 5-9.** 使用软连字*

当包装 URL 时，你不希望包含连字符，因为当行重新组合在一起时，你会得到多余的字符，导致 URL 不正确。在这种情况下，请使用`<wbr>`元素。然而，对于像 `Supercalifragilisticexpialidocious` 这样的单词，你应该使用软连字（`&shy;`），因为你希望显示一个连字符来表明单词在下一行继续。

### 双向文本

在某些语言中，例如希伯来语和阿拉伯语，文本是从右到左书写的。浏览器通常能很好地处理从右到左的语言。只需在 `<html>` 元素上将 `dir` 属性设置为 `rtl`，在大多数情况下就能正确渲染。当然，也会有一些你需要处理的情况，为此，你首先需要理解*Unicode 双向算法*，下面我将进行解释。

## 文本方向