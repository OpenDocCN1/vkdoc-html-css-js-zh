## 第 11 章 文本样式

在本章中，我将演示可用于格式化 HTML 文档文本部分的 CSS 属性。我将从选择字体和各种字体特性开始，然后介绍你可以使用的文本格式化功能。我将介绍间距和对齐以及控制换行和分页的方法。我还会解释一些杂项主题，例如颜色、不透明度和光标，这些并非文本格式化所特有。

### 字体

格式化文本时最基本的因素是选择字体。可用的字体似乎无穷无尽，简单的字体更改就能对网页产生显著效果。字体是一组符号，在排版学中称为字形，这些符号映射到一个字符集。例如，浏览器将根据当前字体中字母“A”映射到的字形来显示它。

#### 获取字体

浏览器可以通过三种方式获取字体定义以便渲染。

#### 网页安全字体

# 文本样式

## 网络安全字体

第一种被称为*网络安全字体*，这个说法其实有些名不副实。所有操作系统都内置了字体；为了在屏幕上显示文本，需要字体将每个字符代码映射到字形。浏览器可以使用这些内置字体中的任何一种。但问题是，并非所有操作系统都支持同一套字体。网络安全字体是指那些普遍被大多数设备支持的字体。这与其说是科学，不如说是艺术，没有哪种字体能在所有操作系统上获得 100%的支持。不过，有一些字体，如 `Arial`、`Verdana`、`Times New Roman` 和 `Courier New`，是比较稳妥的选择。在 [`www.cssfontstack.com/`](http://www.cssfontstack.com/) 上有篇文章展示了这些字体在 Windows 和 Mac 上的可用性（遗憾的是，它没有显示 Android 或 iOS 上的覆盖情况）。

## Web 字体

你可能已经意识到，使用网络安全字体，你的选择范围是有限的。第二种方法能显著增加你的选项，那就是使用*Web 字体*。Web 字体由浏览器下载，就像 CSS 和图片文件等其他资源一样。这种方法有两个缺点：

-   较旧的浏览器不支持 Web 字体；不过，这只会影响一小部分且日益减少的用户群体。
-   额外的下载可能会减慢页面的加载速度，特别是当你使用多种 Web 字体时。这些字体通常会被浏览器缓存，所以通常只影响首次加载。

如果你有兴趣使用 Web 字体，请访问 [`fonts.google.com/`](https://fonts.google.com/)。截至撰写本文时，该网站提供了 808 个字体族，并拥有一个出色的用户界面用于预览和选择字体。选择字体后，该网站会提供如何将其包含到 HTML 文档中的说明。要包含一个 Web 字体，你可以像这样使用 `link` 元素：

```html
<link href="https://fonts.googleapis.com/css?family=Cabin" rel="stylesheet">
```

或者，你也可以使用 `@import` 规则将字体包含在 CSS 文件中，如下所示：

```css
@import url(//fonts.googleapis.com/css?family=Cabin);
```

两种方式达到的效果相同。使用 `link` 元素将其放在标记中，而 `@import` 规则则在 CSS 中完成。我建议使用 `@import`，这样所有样式更改都集中在 CSS 文件中。

如果你之后决定更改字体，应该无需修改 HTML 标记就能做到。

前面提到的 CSS 字体栈网站也提供了一个很好的用户界面，用于选择字体、配置 CSS 以及预览字体定义。它同样引用了相同的 `fonts.googleapis.com` 网站来获取实际的字体资源。

## 自定义字体

第三种方法是将你自己的字体定义嵌入到 CSS 文件中。就像图片一样，定义字体也有多种格式。TrueType 字体（TTF）已经存在大约 30 年了，它和 OpenType 字体（OTF）一起，是在台式电脑上安装字体的标准格式。主流浏览器也支持这些格式。但是，Web 开放字体格式（WOFF）是 W3C 推荐的标准，并且也被大多数浏览器支持。此外，WOFF 2.0 的受欢迎程度和支持度也在不断提升。

创建字体定义超出了本书的范围。但是，一旦你创建或购买了字体定义文件，就可以将其复制到你的 Web 服务器上。然后，你需要使用 `@font-face` 规则在 CSS 文件中引用它。就像音频和视频文件一样，你可以提供多种文件格式，以便浏览器根据其支持的类型进行选择。例如，为了支持 TTF、WOFF 和 WOFF2.0，你需要包含如下规则。当然，你需要在服务器上提供这些文件格式的每一种。

```css
@font-face {
  font-family: 'CustomFont';
  src: url('custom.ttf') format('truetype'),
       url('custom.woff') format('woff'),
       url('custom.woff2') format('woff2');
}
```

还有其他提供字体的网站，例如 [`www.fontsquirrel.com/`](https://www.fontsquirrel.com/) 和 [`www.fontex.org/`](http://www.fontex.org/)。这两个网站都提供字体文件下载，你需要将这些文件托管到你的 Web 服务器上，并作为自定义字体进行引用。

### 字体族

字体通过术语*族*来标识。`Arial`、`Courier New` 和 `Times New Roman` 这些术语就是字体族。在前面的自定义字体示例中，我们定义了 `font-family` 属性，CSS 后续就是通过这个属性来选择它的。此外还有一组通用字体族，图 11-1 展示了每种字体族的示例：

-   **cursive（手写体）** - 该字体族的字体模仿手写体而非印刷体。有时字符之间会相互连接，给人一种手写的感觉。
-   **fantasy（幻想体）** - 这些字体更具装饰性，带有装饰或非标准的字符表现。
-   **monospace（等宽体）** - 等宽字体的所有字符都具有相同的宽度。
-   **sans-serif（无衬线体）** - sans 意为“没有”，serif 是一个额外的笔画，通常是渐变的末端。所以无衬线字体没有这些额外的标记，通常看起来更朴素。
-   **serif（衬线体）** - 这些是更传统的字体，尤其在印刷文本中。你现在读到的就是这种字体。

***图 11-1.** 示例通用字体族*

因此，浏览器拥有一组可供使用的字体，无论是操作系统原生字体、链接的 Web 字体还是嵌入的自定义字体。你可以像这样使用 `font-family` 属性来选择使用哪一种：

```css
p {
  font-family: Cabin;
}
```

但是，由于你无法保证每个浏览器都支持这种字体，你应该提供一些额外的选项作为后备方案。这被称为*字体栈*。你可以通过逗号分隔来指定多个字体族。例如：

```css
p {
  font-family: Cabin, Verdana, sans-serif;
}
```

浏览器会按顺序处理这些字体，所以把你的首选放在第一位。你应该总是以一个通用字体结尾，这样至少能得到一个与你预期相似的字体。前面的示例使用了一种典型方法：先放自定义字体或 Web 字体，接着放一个类似的网络安全字体，最后放一个通用字体族。你可以通过将 `Cabin` 改为 `Cabin1`，然后将 `Verdana` 改为 `Verdana1` 来测试这一点。每次修改后，你都应该注意到字体变了，但仍然与首选相似。

**注意：** 如果字体族名称包含标点符号或数字，例如 `Modern No. 20`，你需要将名称用引号括起来。

### 字体设置

然而，选择字体族只是第一步。还有其他属性，如字号、字重和字体样式，可以更精确地配置字体。还有一些高级功能可以让你更有力地处理特殊情况。

#### 字体样式

`font-style` 属性用于选择斜体字体。有三个可能的值：`normal`（默认）、`italic` 和 `oblique`。Oblique 意为倾斜或偏斜。`oblique` 关键字支持度不太好，通常会产生与 `italic` 相同的效果。

#### 字号

历史上，字号是以字母“M”的宽度来衡量的；但现在，它代表了大写字母的高度。当然，在非西方字符集中，这个定义可能并不完全适用。

在设置字体大小时，有数量惊人的选项。其值是一个距离单位，我在第二章中解释了所有可用于定义距离的单位。附录 C 的参考资料中也总结了这些单位。

# 字体大小与样式

设计师应避免使用绝对尺寸（如`12px`）。若用户因无障碍需求调整字体大小，绝对尺寸将无法随之改变。使用相对尺寸还有助于页面设计，尤其在处理响应式布局时。因此，除了标准距离单位外，还有多种设置字体大小的方法。

浏览器定义了一组关键字，它们会根据用户的字体设置映射到物理单位：

- `xx-small`
- `x-small`
- `small`
- `medium`
- `large`
- `x-large`
- `xx-large`

这些关键字允许你设置字体大小，同时仍允许用户根据需要增大或减小字体。

还有两个关键字可相对于父元素的字体大小设置字体：

- `smaller`
- `larger`

它们会按与上述关键字之间大致相同的幅度调整字体大小。例如，若父元素使用`small`，则使用`larger`大致相当于选择`medium`。

你也可以将字体大小表示为父元素字体大小的百分比。设置`font-size: 120%;`将使字体比父元素的字体大 20%。

我在第 2 章中描述的一些距离单位是相对单位。例如，`em`单位基于当前字体大小，而`rem`基于根元素的字体大小。还有视口相对单位，如`vh`，它代表视口高度的 1%。

> **提示**：有一篇好文章在[`css-tricks.com/almanac/properties/f/font-size/`](https://css-tricks.com/almanac/properties/f/font-size/)，其中提供了各种大小调整方法的示例。

## 字重

`font-weight`属性用于设置粗体字体，它支持两个值：`normal`（默认）和`bold`。不过，你也可以将其设置为具体值：`100`、`200`、`300`、`400`、`500`、`600`、`700`、`800`或`900`。

大多数字体仅支持常规和粗体字形，可能不支持全部 9 种字重。对于仅支持常规和粗体的字体，`100`-`500`将映射为常规，`600`-`900`将映射为粗体。对于支持更精确值的字体，`normal`关键字映射为`400`，`bold`映射为`700`。

还有相对关键字，可相对于父元素的字体设置字重。使用`lighter`将使字重比父元素更轻，`bolder`则相反。为了演示大小和字重的相对值，请考虑以下标记：

```html
<p class="relative">
  This is going to be big, <strong>big, <strong>big</strong></strong>.
</p>
```

此文本包含一个嵌套在另一个`strong`元素内部的`strong`元素。我在第 5 章中解释过，这具有语义含义，表示这段文本非常重要。为了设置样式，以下 CSS 规则考虑了这种含义。

```css
.relative {
  font-family: Arial;
  font-size: medium;
}
strong {
  font-size: larger;
  font-weight: bolder;
}
```

第一条规则仅选择字体并将字体大小设置为`medium`。下一条规则使用了大小和字重的相对值：`larger`和`bolder`。按此方式实现后，嵌套的`strong`元素将两次应用`larger`和`bolder`值。结果如图 11-2 所示。

**图 11-2.** 演示相对大小和字重

## 颜色

字体颜色通过`color`属性设置。我在第 2 章中解释了各种颜色单位，它们在附录 C 的参考资料中也有总结。你还可以设置`opacity`属性，指定一个从 0 到 1 的数字，其中 1 表示完全不透明，0 表示完全透明。你也可以使用`rgba()`或`hsla()`函数同时设置`color`和`opacity`属性。

## 字距调整

一般情况下，每个字符由一个矩形定义的空间所占据。在使用比例字体时，每个字符的宽度都不相同。然而，有些字符更像三角形，例如大写的`A`和`V`是最典型的例子。如果这两个字符连在一起（比如`VA`），使用矩形模型时，它们之间会显得有额外的空间。另一个例子是句点或逗号紧挨着像`T`或`Y`这样顶部沉重的字符。

字距调整（*Kerning*）是一种消除这种空间的功能，使字符之间的间距更一致。

这由`font-kerning`属性控制。该属性支持以下值：

- `auto` - 这是默认值，允许浏览器决定何时应使用该功能。在多数情况下，字距调整会启用；但对于较小的字体大小，浏览器通常会将其关闭。
- `normal` - 这将强制字距调整始终开启。
- `none` - 这将关闭字距调整。

解释字距调整效果的最佳方式是通过演示。以下标记包含两个段落，它们的字符相同；第一个段落包含`kerning`类，第二个段落包含`noKerning`类。

```html
<p class="kerning">T,VAY. - with kerning</p>
<p class="noKerning">T,VAY. - w/o kerning</p>
```

然后，以下 `CSS` 规则将为两个段落分配一个大号衬线字体，并根据`class`属性设置`font-kerning`属性。渲染结果如图 11-3 所示。

```css
.kerning, .noKerning {
    font-family: serif;
    font-size: 48px;
}
```

![](img/index-225_1.png)

**图 11-3** 演示字距调整

```css
.kerning {
    font-kerning: normal;
}
.noKerning {
    font-kerning: none;
}
```

注意当字距调整开启时，`V`、`A`和`Y`字符的矩形会重叠。此外，句点和逗号也更靠近`T`和`Y`字符。

### 拉伸（Stretch）

字体大小描述的是字体的高度，而非宽度。某些字体提供了多种具有不同字符宽度的版本。较宽的字体称为*拉伸（stretched）*，较窄的字体称为*压缩（condensed）*。

您可以使用`font-stretch`属性来控制宽度。以下是定义的值：

- `ultra-condensed`
- `extra-condensed`
- `condensed`
- `semi-condensed`
- `normal`
- `semi-expanded`
- `expanded`
- `extra-expanded`
- `ultra-expanded`

特定的字体可能不支持所有这些选项，浏览器会选择最接近请求值的可用宽度。此属性对仅提供一种宽度的字体没有影响。作为演示，以下标记有两个段落：一个带有`stretched`类，另一个带有`condensed`类。后续的 `CSS` 选择了 `Arial` 字体，并对这两个类应用了`font-stretch`属性。结果如图 11-4 所示。

```html
<p class="stretched">stretched</p>
<p class="condensed">condensed</p>
```

```css
.stretched, .condensed {
    font-family: arial;
    font-size: 48px;
}
.stretched {
    font-stretch: extra-expanded;
}
.condensed {
    font-stretch: extra-condensed;
}
```

**图 11-4** 演示 `font-stretch`

### 变体大写（Variant Capitals）

`font-variant-caps`属性允许您为小写字符使用大写字形。此功能不受 `IE` 或 `Edge` 浏览器支持，并且也受限于字体的功能。该属性支持以下值：

- `all-petite-caps` - 如果支持，小写和大写字符都将使用特小号大写字形。如果不支持，则两者都将使用小型大写字形。
- `all-small-caps` - 小写和大写字符都将使用小型大写字形。
- `normal` - 这是默认值，将禁用大写字形的使用。
- `petite-caps` - 仅小写字符将使用特小号字形；如果不支持，则改用小型大写字形。
- `small-caps` - 小型大写字形将用于小写字符；大写字符将使用正常字形。
- `titling-caps` - 为大写和小写字符使用特殊字形，这些字形被设计为在长段文本使用大写字符时不会显得过于粗重。如果不支持这些字形，则此属性会被忽略。
- `unicase` - 仅对大写字符使用小型大写字形。这会导致大写字符与小写字符尺寸相同。

![](img/index-227_1.png)

![](img/index-227_2.png)

## 第 11 章 ■ 文本样式

作为一个快速演示，下面的标记包含一个带有`smallCaps`类的段落，并且 CSS 将`font-variant-caps`属性设置为`small-caps`。在大多数浏览器中的渲染效果如图 11-5 所示。

```markdown
```html
<p class="smallCaps">使用小型大写字母</p>
```

```css
.smallCaps {
  font-size: x-large;
  font-variant-caps: small-caps;
}
```

***图 11-5.** 演示小型大写字母*

### 数字

`font-variant-numeric`属性提供了控制某些数字内容呈现方式的能力。例如，像 1st、2nd 和 3rd 这样的序数可以显示为上标。同样，分数可以渲染为对角线形式（½）或堆叠形式。IE、Edge 或 Safari 目前不支持此属性。此外，这要求字体支持这些特殊字形。我发现对角线分数似乎相当一致地工作，但其他特性大部分时间都不起作用。如果你想进一步研究，Mozilla 对 W3C 推荐标准定义的选项有一个很好的解释，可以在[`developer.mozilla.org/en-US/docs/Web/CSS/font-variant-numeric`](https://developer.mozilla.org/en-US/docs/Web/CSS/font-variant-numeric)找到。

可以通过以下标记和 CSS 来演示对角线分数。其渲染效果如图 11-6 所示。

```html
<p class="diagonal">1/2 2/3 3/4 4/5</p>
```

```css
.diagonal {
  font-family: Verdana;
  font-size: xx-large;
  font-variant-numeric: diagonal-fractions;
}
```

***图 11-6.** 演示对角线分数*

### 特性设置

`font-feature-settings`属性提供了低级访问权限，以控制特定字体的高级特性。它可以通过设置字体特定的配置值来修改 OpenType 字体特性。只有在无法通过其他属性实现所需效果时才应使用此属性。在[`css-tricks.com/almanac/properties/f/font-feature-settings/`](https://css-tricks.com/almanac/properties/f/font-feature-settings/)有一个关于此属性的好文章。但是，请检查你使用的字体支持哪些特性。

## 第 11 章 ■ 文本样式

### 简写符号

我在第[2 章](http://dx.doi.org/10.1007/978-1-4842-2463-2_2)中解释过，通常可以使用简写符号通过单个声明设置多个属性。`font`属性就是一个很好的例子。使用单个声明，你可以设置以下属性：

* `font-family`

* `font-size`

* `font-weight`

* `font-style`

* `font-stretch`

* `font-variant`

* `line-height`（本章稍后解释）

但是，有一些限制。`font-family`和`font-size`是必需的；其他的是可选的。`font-family`属性必须列在最后。如果指定了`font-weight`、`font-style`或`font-variant`属性，它们必须放在`font-size`之前。如果违反了这些规则中的任何一条，整个声明将被忽略。如果指定了`line-height`属性，它必须紧跟在`font-size`属性之后，并且两者之间应该用斜杠（`/`）字符分隔。

> **警告**：`font-stretch`属性相对较新，可能不被某些浏览器支持。如果不支持且它被包含在简写符号中，整个声明将被忽略。因此，你不应该包含`font-stretch`，而应单独指定它。

使用简写符号时，有几个预定义的关键字可以用来设置所有字体属性，以匹配操作系统使用的字体。这些是支持的字体值：

* `caption`

* `icon`

* `menu`

* `message-box`

* `small-caption`

* `status-bar`

例如，设置`font: icon;`将使用与桌面图标匹配的字体。

> **提示**：谷歌在这篇文章中提供了对字体的很好的概述：[`developers.google.com/fonts/docs/getting_started`](https://developers.google.com/fonts/docs/getting_started)。这篇文章还演示了一些高级技术，例如字体效果。其中许多效果仅受 Chrome 浏览器支持，因此要注意这一点；但它仍然是很有用的信息。

## 第 11 章 ■ 文本样式

### 文本格式化

还有一些 CSS 属性可以用来控制文本的格式化方式，例如对齐和缩进。你还可以使用阴影和其他效果来装饰文本。

#### 水平对齐

`text-align`属性定义了文本在其块内如何水平对齐。主要的值有`left`、`right`、`center`和`justify`，它们的工作原理正如你所预期的那样。有两个新值，`start`和`end`，Edge 目前还不支持它们。`start`值对于从左到右的语言等同于`left`，但在从右到左的语言中则是右对齐。如果使用了`justify`，则`text-justify`属性指定了两端对齐的方法。默认方法是在单词之间添加空格。目前对`text-justify`属性的支持非常少。

#### 缩进

`text-indent`属性指定了块的第一个行应该如何缩进（或悬挂缩进）。它以绝对或相对单位输入一个距离。负值表示第一行是悬挂缩进的。也可以使用百分比，它将占包含块宽度的百分比。

#### 溢出

如果文本无法换行到下一行，它可能会溢出其水平空间。我稍后会在本章中解释换行属性。当这种情况发生时，并且`overflow`属性没有设置为`visible`，无法容纳的文本部分将被隐藏。`text-overflow`指定了浏览器应如何处理这种情况。有两个关键字可以使用：`clip`和`ellipsis`。`clip`值只是裁剪文本，但`ellipsis`值会在块内包含一个省略号（…），表明还有更多文本无法显示。这也会减少可以显示的文本量。

> **注意**：此属性的 W3C 规范提供了一个或两个参数。如果指定了单个参数，它表示在块的右侧要做什么。如果指定了两个参数，它们分别指定左侧和右侧。然而，目前只有 Firefox 支持双值版本。

作为演示，以下标记包含一个相当长的单词。CSS 规则随后设置了一个固定宽度并将`overflow`设置为`hidden`。最后，使用`text-overflow: ellipsis;`来指示应该显示省略号。其渲染效果如图 11-7 所示。

```html
<p class="overflow">Supercalifragilisticexpialidocious</p>
```

```css
.overflow {
  font-size: large;
  border: 1px solid black;
  width: 150px;
  overflow: hidden;
  text-overflow: ellipsis;
}
```

![](img/index-230_1.png)

![](img/index-230_2.png)

## 第 11 章 ■ 文本样式

***图 11-7.** 设置`text-overflow`属性*

#### 引号

引号在几种情况下由浏览器自动创建。最常见的是当使用`quote`（`q`）元素时。浏览器还会检测是否存在嵌套的引用元素，例如：
```

# 排版后的文本

**<q>**你什么意思<q>我不敢相信他说了<q>不</q></q>？**</q>** 在此情况下，外层引号会使用双引号，内层引号会使用单引号。显示效果为：“你什么意思‘我不敢相信他说了’不“？””。不过，你可以使用 `quotes` 属性来控制引号所使用的字符。例如，以下 CSS 规则将告知浏览器对外层引号使用花括号，对内层引号使用方括号。`quotes` 属性提供了四个字符串值；前两个用于外层引号，后两个用于内层引号。

```css
* {
  quotes: "{" "}" "[" "]"
}
```

另一种需要输入引号的情况是使用 `::before` 和 `::after` 伪元素时。为了演示这一点，下面的标记包含一个带有内嵌引用的段落：

```html
<p class="quote">他说，<q>找到了！</q></p>
```

接着，这些 CSS 规则使用伪元素在文本前后添加引号。正如我在第 [9](http://dx.doi.org/10.1007/978-1-4842-2463-2_9) 章中解释的那样，`content` 属性用于指定应向所选元素追加或前置的内容。在此情况下，它被设置为 `open-quote` 和 `close-quote` 关键字。

```css
.quote::before {
  content: open-quote;
}

.quote::after {
  content: close-quote;
}
```

应用这些 CSS 规则后，两个示例的渲染结果如图 11-8 所示。

***图 11-8.** 演示 quotes 属性*

![](img/index-231_1.jpg)

请注意，`找到了！` 周围的引号使用的是内层引号。因为引号是由伪元素添加的；浏览器将 `<q>` 元素视为嵌套在外层引号之内。如果在伪元素中改用 `no-open-quote` 和 `no-close-quote` 关键字，则不会渲染外层引号，但 `<q>` 元素仍会像嵌套使用内层引号一样被处理。

### 阴影

为了增添有趣的效果，你可以使用 `text-shadow` 属性为文本添加阴影。该属性接受一个颜色值和三个距离值。颜色值可以作为第一个或最后一个参数。从技术上讲它是可选的；但为了获得最佳兼容性，你应始终包含它。某些浏览器（如 Safari）在未指定颜色时将不会显示阴影。大部分浏览器会默认将颜色设为文本颜色。

三个距离值分别为 x 偏移量、y 偏移量和模糊半径，且必须按此顺序提供。模糊半径定义了原始文本图像的扩散范围。模糊半径越大，阴影就越宽越淡。该参数可选，默认为 0。x 和 y 偏移量为必填项，表示阴影相对于文本的位置。正值将向右和向下偏移；负值则向左和向上偏移。

为了演示文本阴影的不同效果，下面的标记包含三个拥有相同文本和不同类值的段落。为每个类定义了一条不同的 CSS 规则，其 `text-shadow` 属性值各不相同。渲染结果如图 11-9 所示。

```html
<p class="shadow">这段文本有阴影</p>
<p class="background">这段文本有阴影</p>
<p class="blurred">这段文本有阴影</p>
```

```css
.shadow {
  font-size: xx-large;
  text-shadow: 10px 5px 1px gray;
}

.background {
  font-size: xx-large;
  text-shadow: 0px 0px 5px gray;
}

.blurred {
  font-size: xx-large;
  text-shadow: 10px 5px 10px gray;
}
```

***图 11-9.** 演示文本阴影*

## 大小写

`text-transform` 属性可用于强制改变文本的大小写。它仅影响字符在浏览器中的渲染方式；在 DOM 中，文本保留其原始值。`text-transform` 属性支持以下值：

# 文本样式与排版

## 大小写转换

- `capitalize` - 每个单词的首字母转换为大写。但请注意，这不支持标题大小写规则，即标题中间诸如“a”或“the”之类的单词不会被大写。
- `uppercase` - 所有字符转换为大写。
- `lowercase` - 所有字符转换为小写。
- `none` - 不进行任何更改（用于覆盖之前的规则）。

**注意** 某些语言具有独特的大小写规则，浏览器对此的支持程度各不相同。有些情况下这些规则会被正确处理，而有些则不会。

### 间距与对齐

您可以使用`CSS`属性控制 HTML 文档中文本的间距。您还可以定义浏览器应如何处理内容中的空白字符。内联元素的垂直对齐可以通过`vertical-align`属性实现。

#### 基本间距

有三个简单的`CSS`属性允许您控制字母、单词和行之间的间距。这些属性无需过多解释，我将简要描述它们：

- `letter-spacing` - 正值会在字符之间创建额外间距，负值会减少间距并可能导致重叠。允许除百分比外的所有距离单位，包括像素等绝对值和`em`、`rem`、`vw`等相对单位。默认值为`normal`，允许字体执行标准间距（包括两端对齐）。
- `word-spacing` - 影响单词和内联元素之间的间距。允许使用任何距离单位；但只有`Firefox`支持百分比。值可以是绝对值或相对值，但不支持负值。默认值为`normal`。
- `line-height` - 指定每行文本使用的垂直空间。默认值为`normal`，大约比字体大小多 20%。`line-height`可以指定为数字（如`1.2`）或百分比（如`120%`），这些值会乘以字体大小；或者指定为距离单位（如`20px`或`3em`）。避免使用绝对距离单位，因为这可能会产生意外结果。`line-height`属性也可以使用我之前解释过的`font`简写符号进行设置。

![图 11-10 预览](img/index-233_1.png)

第 11 章 ■ 文本样式

### 处理空白字符

在[第 1 章](http://dx.doi.org/10.1007/978-1-4842-2463-2_1)中，我提到空白字符通常被忽略，除非内容位于`pre`元素内部，这是默认行为。但是，您可以通过`white-space`属性更改空白字符的处理方式。它支持以下值：

- `normal` - 默认值。空白字符序列被压缩为单个空格，换行符被视为空格。文本仅在需要填充块时进行换行。
- `nowrap` - 空白字符的压缩方式与`normal`相同，但文本从不换行。
- `pre` - 功能类似于`pre`元素；保留空白字符，文本仅在换行符处换行。
- `pre-wrap` - 功能类似于`pre`值，但文本也会在必要时换行以填充块。
- `pre-line` - 空白字符的压缩方式与`normal`值相同，但文本会在换行符处换行，并且在必要时也会换行以填充块。

为了演示这一点，我将使用我在[第 4 章](http://dx.doi.org/10.1007/978-1-4842-2463-2_4)中介绍的亨利·沃兹沃斯·朗费罗的诗。这次，它将位于一个普通的段落元素内，而不是预格式化元素中，但我可以使用`CSS`实现相同的效果。

```html
<p class="whitespace">I heard the bells on Christmas Day

Their old, familiar carols play,

And wild and sweet

The words repeat

Of peace on earth, good-will to men!</p>
```

仅应用默认样式时，此文本将位于单行上，或根据窗口宽度在必要时换行。但是，以下`CSS`规则将把它视为预格式化文本，如图 11-10 所示。

```css
.whitespace {
  font-family: monospace;
  white-space: pre;
}
```

### **图 11-10.** 在 CSS 中处理预格式化文本

## 第 11 章 ■ 文本样式

#### 垂直对齐

垂直对齐可能比较棘手。`vertical-align` 属性用于此目的，但其行为并非总是直观易懂。它对于表格单元格效果很好，这将在第 [13](http://dx.doi.org/10.1007/978-1-4842-2463-2_13) 章中介绍。对于其他内容，它仅适用于内联元素和内联块元素。对于内联内容（包括内联块），定义了以下值：

- `baseline` - 元素的基线与父元素的基线对齐。
- `bottom` - 元素的底部与当前行的底部对齐。
- `middle` - 这个值可能与你预期的略有不同；元素的中间与父元素的基线加上父元素小写字母`x`高度的一半对齐。
- `sub` - 基线与父元素的下标基线对齐。
- `super` - 基线与父元素的上标基线对齐。
- `text-bottom` - 元素的底部与父元素的底部对齐。
- `text-top` - 元素的顶部与父元素的顶部对齐。
- `top` - 元素的顶部与当前行的顶部对齐。

你也可以使用绝对或相对单位输入一个距离值，这将调整此元素的基线相对于父元素基线的位置。也可以使用百分比，它会乘以行高。也允许使用负值。

除了`top`和`bottom`值之外，对齐都是基于父元素的。为了演示这一点，我将使用以下标记，在一个段落元素内放置两个`span`元素。

```html
<p id="Align">
  X
  <span class="large">Large </span>
  <span class="small">Small</span>
  X
</p>
```

段落是父元素，我在`span`元素前后都包含了一个“X”，以便你可以直观地看到`span`元素所要对齐的父元素位置。然后，我将创建三条 CSS 规则。第一条规则简单地定义了父元素的字体。

```css
#Align {
  font-family: Verdana;
  font-size: 36px;
}
```

```css
.large {
  font-size: 48px;
  vertical-align: text-bottom;
}
```

```css
.small {
  font-size: 24px;
  vertical-align: text-top;
}
```

![图 11-11 预览](img/index-235_1.png)

接下来的两条规则设置了`span`元素的字体大小，并设置了`vertical-align`属性。大号字体对齐到底部，小号字体对齐到顶部。结果如图 11-11 所示。

### **图 11-11.** 演示垂直对齐

你可能已经注意到，文本并没有像你预期的那样完美对齐。这是因为浏览器计算基线以及用于执行对齐的其他因素的方式。如果你想进一步研究这个问题，可以看看 [`christopheraue.net/2014/03/05/vertical-align/`](http://christopheraue.net/2014/03/05/vertical-align/) 上的一篇好文章。

作为第二个示例，我将把一个固定大小的`div`元素与一个文本元素对齐。以下是标记；有两个带有`align`类的`div`元素。每个里面都有一个带有`box`类的空`div`元素和一个包含一些文本的`span`元素。

```html
<div class="align">
  <div class="solid box"></div>
  <span>Solid Box</span>
</div>
<br />
<div class="align">
  <div class="outline box"></div>
  <span>Outline Box</span>
</div>
```

为了设置样式，以下 CSS 规则将使第一个内部`div`显示为一个实心方块，第二个显示为一个轮廓方块。`box`这个`div`和`span`元素都将`vertical-align`属性设置为`middle`。渲染结果如图 11-12 所示。

```css
.align {
  display: inline-block;
  font-size: 48px;
}

.box {
  display: inline-block;
  height: 10px;
  width: 10px;
  border: 1px solid black;
  vertical-align: middle;
}

.solid {
  background-color: black;
}

.align>span {
  vertical-align: middle;
}
```

![图 11-12 预览](img/index-236_1.png)

### **图 11-12.** 将方块与文本对齐

### 断开

有一些 CSS 属性可以用来控制何时应发生换行和分页。分页主要用在打印文档时。

#### 单词换行

# 文本换行与分页

## 文本换行

`overflow-wrap` 属性决定了可以在何处插入换行符以换行文本，使其适应其容器的宽度。`normal` 值表示只能在单词之间、在断字位置（`wbr`）元素处或在硬连字符或软连字符处插入换行符。这些已在第 [5](http://dx.doi.org/10.1007/978-1-4842-2463-2_5) 章中解释过。`break-word` 值表示可以在任意点断开单词，以适应允许的空间。

> **注意：** 大多数浏览器支持 `word-wrap` 属性，但它已被 `overflow-wrap` 取代。`word-wrap` 属性可能会在很长一段时间内继续得到支持，但你应该改用 `overflow-wrap`。

还有一个额外的 `word-break` 属性，它与 `overflow-wrap` 类似。它支持 `normal` 和 `break-all` 值，这些值大致相当于 `overflow-wrap` 的 `normal` 和 `break-word` 属性。`word-break` 属性还支持 `keep-all` 值，主要适用于中文、日文和韩文。`overflow-wrap: break-word;` 和 `word-break: break-all;` 在不同浏览器和上下文中的行为存在一些细微差别。除非你正在处理这些特定语言，否则我建议坚持使用 `overflow-wrap` 属性。

当换行符插入到单词中间时，你可能还想在行尾添加一个连字符，以表明单词还在继续。`hyphens` 属性控制这种情况的发生时间，并支持以下值：

- `none` - 断词时从不使用连字符。
- `manual` - 仅当在单词断开处存在硬连字符或软连字符字符时才使用连字符。
- `auto` - 默认值。它的工作方式类似于 `manual`，区别在于，当语言特定的资源确定适合使用连字符时，也可能会添加连字符。当然，这依赖于语言特定的支持，而这种支持目前非常有限。在大多数情况下，它的功能类似于 `manual`。

## 分页

在渲染要打印的 HTML 时，你可以控制如何生成分页。有三个属性可以应用：

- `page-break-after` - 定义在元素末尾应用的规则。
- `page-break-before` - 指定应在元素之前应用的分页规则。
- `page-break-inside` - 指示是否允许在元素内部进行分页。

对于这些属性中的每一个，默认值都是 `auto`，它不定义任何规则；浏览器通常会在页面的底部换到新的一页。`page-break-inside` 属性支持另一个值 `avoid`。使用此值可以防止在特定元素中间分页。例如，如果你不想在标题元素中间分页，创建以下规则：

```
header {
  page-break-inside: avoid;
}
```

另外两个属性 `page-break-before` 和 `page-break-after`，除了 `auto` 和 `avoid` 之外，还支持以下值：

- `always` - 应始终在元素之前（或之后）进行分页。
- `left` - 工作方式类似于 `always`，但它可能生成一个或两个分页符，以强制下一页为左手页。
- `right` - 与 `left` 类似，只是下一页将是右手页。

例如，你可以使用 `page-break-before` 属性为每个 `h1` 元素强制分页。你也可以根据文档的结构，对每个 `section` 或 `article` 元素执行相同操作。

> **注意：** 这些分页属性正在被通用的 `break` 属性取代。例如，`page-break-after` 将被 `break-after` 取代。但是，在撰写本文时，尚不支持这些新属性。

# 光标形状

鼠标在网页上移动时，光标的形状能提供重要的反馈信息。例如，当悬停在链接上时，光标通常会变成手指指向的手掌形状。通过使用`cursor`属性并选择预定义的关键字，可以轻松设置光标形状。例如：

```css
a {
  cursor: pointer;
}
```

与其在此列出所有选项，不如参考以下 Mozilla 文章，该文章列出了所有选项并附有示例图片：<https://developer.mozilla.org/en-US/docs/Web/CSS/cursor>。您也可以将鼠标悬停在选项上，在您的操作系统中查看它们的具体外观。光标由操作系统控制，实际形状会根据您使用的操作系统而有所不同。

您还可以通过指定图像文件的 URL 来定义自己的光标。使用自定义光标时，应同时包含一个预定义关键字作为后备方案，以防图像文件不被支持。可以像这样定义多个光标：

```css
a {
  cursor: url(custom.png), url(fallback.cur), crosshair;
}
```

## 总结

在本章中，我解释了如何使用网页安全字体和网络字体，以及如何配置通过 CSS 控制的各种文本样式属性。CSS 提供了许多属性，让您可以出色地控制文本的排列方式。在下一章中，我将演示如何使用边框和背景来增强 HTML 文档中的内容。

---
