# 5. 伪选择器

伪类和伪元素是可以附加到选择器上使其更具体的关键字。它们很容易识别，因为前面总是带有一个冒号。

## 伪元素

伪元素可以对元素的部分进行样式设置。CSS 中有四个伪元素，如下述各节所述。

### first-letter 和 first-line

伪元素 `:first-letter` 和 `:first-line` 可以将样式应用于元素的第一个字母和第一行。它们仅适用于段落等块级元素。

```css
p:first-letter { font-size: 120%; }
p:first-line   { font-weight: bold; }
```

上述第一个规则使段落中的首字母比其他文本大 20%。第二个规则使段落中的第一行文本加粗。

### before 和 after

顾名思义，`:before` 和 `:after` 伪元素可以定位元素之前和之后的位置。它们与 `content` 属性一起使用，可在元素之前或之后插入内容。

```css
p:before { content: "之前"; }
p:after  { content: "之后"; }
```

这些规则使以下段落显示为“之前中间之后”：

```html
<p>中间</p>
```

`content` 属性很特殊，因为它只能与这两个伪元素一起使用。它也是唯一打破内容（HTML）和设计（CSS）之间界限的属性。请记住，只有当一段内容的存在归结于设计决策时，才应打破这条界限。例如，`content` 属性可用于在元素之前添加图标，这可以通过使用 `url` 函数来实现。

```css
p.bullet:before { content: url(my-bullet.png); }
```

## 伪类

伪类允许根据元素之间的关系以及 HTML 文档中未包含的信息来设置样式。大多数伪类可分为三类：动态伪类、结构伪类和用户界面伪类。

### 动态伪类

第一类伪类用于在链接或其他交互元素的状态发生改变时为其应用样式。这类伪类共有五个，全部在 CSS 2 中引入。

#### `:link` 和 `:visited`

动态伪类 `:link` 和 `:visited` 仅能应用于锚点元素（`<a>`）。`:link` 伪类匹配指向尚未访问页面的链接，而 `:visited` 匹配已访问过的链接。

```css
a:link    {} /* 未访问的链接 */
a:visited {} /* 已访问的链接 */
```

#### `:active` 和 `:hover`

另一个伪类是 `:active`，它匹配正在被激活的元素，例如通过鼠标点击激活。这对于为锚点元素设置样式最有用，但也可以应用于任何元素。

```css
a:active {} /* 激活的链接 */
```

当用户将鼠标等指向设备移动到所选元素上时，会应用带有 `:hover` 伪类的选择器。它常用于创建链接悬停效果。

```css
a:hover {} /* 悬停的链接 */
```

当这四种伪类应用于同一个选择器时，需要按正确的顺序出现。具体来说，`:hover` 伪类必须位于 `:link` 和 `:visited` 之后，并且要使 `:active` 生效，它必须出现在 `:hover` 之后。可以使用短语 "love and hate" 来帮助记忆构成正确顺序的首字母。

```css
a:link    {} /* L */
a:visited {} /* V */
a:hover   {} /* H */
a:active  {} /* A */
```

#### `:focus`

最后一个动态伪类是 `:focus`，它可用于能够获得焦点的元素，例如表单中的 `<input>` 元素。`:active` 和 `:focus` 的区别在于，`:active` 仅在点击过程中持续，而 `:focus` 会一直持续到元素失去焦点。

```css
input:focus {}
```

IE 浏览器直到版本 8 才支持 `:focus` 伪类。

### 结构伪类

结构伪类根据元素与其他元素的关系来定位元素。CSS 2 在此类别中仅包含一个结构伪类 `:first-child`，而 CSS 3 引入了大量新的结构伪类。所有主流浏览器（IE7 及更低版本除外）都支持这些 CSS 3 结构伪类。

#### `:first-child`

`:first-child` 伪类匹配所选元素的第一个子元素。

```css
p:first-child {} /* 第一个段落子元素 */
```

在以下示例中，此规则适用于第一个锚点元素：

```html
<p>
  <span>第一个子元素</span>
  <span>文本</span>
</p>
```

#### `:last-child`

`:last-child` 伪类表示所选元素的最后一个子元素。

```css
p:last-child {} /* 最后一个段落子元素 */
```

此规则针对以下段落元素的最后一个子元素。

```html
<p>
  <em>文本</em>
  <em>最后一个子元素</em>
</p>
```

#### `:only-child`

`:only-child` 伪类匹配没有兄弟元素的元素。

```css
p:only-child {} /* 无兄弟元素的子元素 */
```

此规则应用于以下第一个 `<strong>` 元素，因为它是该段落的唯一子元素。第二个段落元素有两个子元素，因此其中没有一个被此规则选中。

```html
<p>
  <strong>唯一子元素</strong>
</p>
<p>
  <strong>文本</strong>
  <em>文本</em>
</p>
```

#### `:only-of-type`

顾名思义，`:only-of-type` 选择器仅在选择元素没有相同类型的兄弟元素时才匹配该元素。

```css
p:only-of-type {} /* 唯一的 <p> 元素 */
```

以下段落被此规则选中，因为它是其父元素中唯一的段落元素。

```html
<div>
  <h1>文本</h1>
  <p>唯一类型</p>
</div>
```

#### `:first-of-type`

`:first-of-type` 伪类匹配属于所选类型的第一个子元素。

```css
p:first-of-type {} /* 第一个 <p> 元素 */
```

它匹配以下标记中的第一个段落元素：

```html
<div>
  <h1>文本</h1>
  <p>首个类型</p>
  <p>文本</p>
</div>
```

#### `:last-of-type`

特定类型的最后一个兄弟元素可以使用 `:last-of-type` 伪类来选择。

```css
strong:last-of-type {} /* 最后一个 <strong> 元素 */
```

此规则适用于其兄弟元素中的最后一个 `<strong>` 元素。

```html
<div>
  <strong>文本</strong>
  <strong>最后类型</strong>
</div>
```

#### `:nth-child`

`:nth-child(an + b)` 伪类在子元素被分成每 `a` 个元素一组后，匹配每组的第 `b` 个子元素。

```css
p:nth-child(1)    {} /* 第一个子元素 */
p:nth-child(2n)   {} /* 偶数子元素 */
p:nth-child(2n+1) {} /* 奇数子元素 */
```

这些规则适用于以下 HTML 标记：

```html
<p>
  <span>第一个且奇数</span>
  <span>偶数</span>
  <span>奇数</span>
</p>
```

匹配奇数与偶数子元素是一种常见操作，因此也可以使用关键字 `odd` 和 `even` 来匹配表格中的每隔一行。

```css
tr:nth-child(even) {} /* 偶数表格行 */
tr:nth-child(odd)  {} /* 奇数表格行 */
```

如上所示，`:nth-child()` 的参数可以是一个整数、关键字 `even` 或 `odd`，或者形如 `an+b` 的表达式。在表达式形式中，关键字 `n` 是一个计数器，会遍历所有子元素。计数器可以为负数；在这种情况下，迭代会反向进行。它可用于选择特定数量的前几个子元素。

```css
p:nth-child(-n+3) {} /* 前三个子元素 */
```

与 `:nth-child()` 一起使用的数学逻辑和参数同样适用于接下来的三个伪类，它们都以 `:nth` 开头。

#### `:nth-of-type`

`:nth-of-type(an + b)` 伪类在相同类型的兄弟元素被分成每 `a` 个元素一组后，匹配所选类型的第 `b` 个元素。

```css
p:nth-of-type(2)    {} /* 第二个段落兄弟元素 */
p:nth-of-type(2n)   {} /* 偶数段落兄弟元素 */
p:nth-of-type(2n+1) {} /* 奇数段落兄弟元素 */
```

此伪类的行为类似于 `:nth-child`，但它是匹配指定元素的相同类型的兄弟元素，而不是匹配指定元素的子元素。

```html
<div>
  <em>文本</em>
  <p>奇数</p>
  <p>第二个且偶数</p>
  <p>奇数</p>
</div>
```

与其他 `:nth` 伪类类似，可以使用关键字 `odd` 和 `even` 来匹配索引为奇数或偶数的相同类型兄弟元素。

```css
p:nth-of-type(even) {} /* 偶数段落兄弟元素 */
p:nth-of-type(odd)  {} /* 奇数段落兄弟元素 */
```

#### `:nth-last-of-type`

`:nth-last-of-type(an + b)` 伪类匹配所选类型的元素，该元素之后还有 `an+b` 个相同类型的元素。其行为等同于 `:nth-of-type` 伪类，区别在于元素计数是从底部开始而不是从顶部开始。

```css
p:nth-last-of-type(3)    {} /* 倒数第三个段落 */
p:nth-last-of-type(-n+2) {} /* 最后两个段落 */
```

这两个规则适用于以下示例。由于 `<em>` 元素不是指定类型（本例中为段落），因此不计算在内。

```html
<div>
  <p>倒数第三</p>
  <p>最后两个</p>
  <p>最后两个</p>
  <em>文本</em>
</div>
```

#### `:nth-last-child`

`:nth-last-child(an + b)` 伪类表示在其之后有 `an+b` 个兄弟元素的任何元素。其行为与 `:nth-child` 相同，不同之处在于它是从底部元素开始而不是从顶部元素开始。

```css
p:nth-last-child(3)    {} /* 倒数第三个子元素 */
p:nth-last-child(-n+2) {} /* 最后两个子元素 */
```

这两个规则适用于以下示例中的子元素：

```html
<div>
  <p>倒数第三</p>
  <p>最后两个</p>
  <p>最后两个</p>
</div>
```