# 第 1 章 基础

## 表 1-2. 最爱站点代码

| 代码 | 解释 |
|------|------|
| `<html>` | 开启 `html` 标签。 |
| `<head>` | 开启 `head` 标签。 |
| `<title>Annotated links</title>` | 完整的 `title` 元素：包含开启和闭合标签，以及中间的“Annotated links”。 |
| `<style>` | 开启 `style` 标签，这意味着我们将开始使用 CSS。 |
| `article {` | 样式开始。被修饰的元素是所有的 `article` 元素。样式随后有一个花括号 `{`。开启和闭合花括号包围着我们创建的样式规则，类似于 HTML 中的开启和闭合标签。 |
| `width:60%;` | 宽度设置为包含元素宽度的 60%。请注意，每条指令都以分号 `;` 结束。 |
| `text-align:left;` | 文本左对齐。 |
| `margin:10px;` | 外边距为 10 像素。 |
| `border:2px green double;` | 边框为 2 像素宽的绿色双实线。 |
| `padding:2px;` | 文本与边框之间的内边距为 2 像素。 |
| `display:block;` | `article` 是一个块级元素，意味着其前后会有换行。 |
| `}` | 闭合 `article` 的样式。 |

```html
img {display:block;}
```

`style` 将 `img` 元素设置为块级样式：前后换行。

```html
</style>
```

闭合 `style` 标签。

```html
<script>
```

打开 `script` 标签。我们现在编写 JavaScript 代码。

```javascript
document.write(Date());
```

一行代码：输出 `Date()` 调用产生的结果。

（*续*）

## 表 1-2. （*续*）

**代码**

**说明**

```html
</script>
```

闭合 `script` 标签。

```html
</head>
```

```html
<body>
```

打开 `body` 标签。

### 最喜欢的网站

被 `h3` 和 `/h3` 标签包围的 `text`。这使得文本看起来比常规稍微大一些。

```html
<article>
```

打开 `article` 标签。

```html
<a href="http://www.purchase.edu"> The website for Purchase</a>
```

此文本将应用指定的样式。它包含一个 `a` 元素。纽约州立大学帕切斯学院。

```html
</article>
```

闭合 `article` 标签。

```html
<article>
```

打开 `article` 标签。

```html
<a href="https://avivameyer.smugmug.com/">Aviva Meyer's photographs</a>
```

此文章与前一个类似，包含一个 `a` 元素和一些文本。网站是 Aviva 照片的集合，存储在一个名为 `smugmug` 的网站上。分类包括音乐、冒险和家庭（需要密码）。

```html
<img src="avivasmugmug.png" width="300"/>
```

一个 `img` 标签。图片来源是文件 `avivasmugmug.jpeg`。如果文件扩展名是 `.jpg`，则无法生效。宽度设置为 300 像素。由于 `style` 区域中的样式指令，前后有换行。

```html
</article>
```

闭合 `article` 标签。

（*续*）

## 表 1-2. （*续*）

**代码**

**说明**

```html
<article>
```

打开 `article` 标签。

```html
<a href="http://apress.com">Apress publishers</a>
```

这与前一个文章类似：一个 `a` 元素是本书出版商的网站。

```html
<br/>
```

```html
<img src="apressshot.png" width="300"/>
```

一个 `img` 元素。图片来源是 `apressshot.jpeg`。宽度设置为 300 像素。

```html
</article>
```

闭合 `article` 标签。

```html
</body>
```

闭合 `body` 标签。

```html
</html>
```

闭合 `html` 标签。

让这个应用为你所用是非常直接的：使用你自己最喜欢的网站！在大多数浏览器中，如果你希望使用网站徽标作为超链接，可以下载并保存图像文件，或者你也可以包含其他图片。据我所知，列出网站并添加评论，以及包含徽标等图片，属于“合理使用”的范畴，但我不是律师。通常来说，人们喜欢别人链接到他们的网站。这并不影响法律问题，不过你也可以选择将 `img` 标签中的 `src` 设置为图片所在网站的网址，如果你不想先将某个图片文件下载到本地再上传到你的网站。

你也可以通过更改格式来个性化这个应用。样式可用于指定字体，包括特定的字体、字体系列和字号。这样你可以选择一种喜欢的字体，并指定如果用户计算机上没有首选字体时使用何种字体。你可以指定外边距和内边距，或者分别独立设置 `margin-top`、`margin-left`、`padding-top` 等。

#### 测试和上传应用

你需要将所有文件（本例中是一个 HTML 文件加上所有图片文件）放在同一个文件夹中，除非你使用的是完整网址。要使链接生效，你需要为所有 `href` 属性提供正确的地址。我的示例展示了如何为同一文件夹中的 HTML 文件或 Web 上其他位置的 HTML 文件进行此操作。

即使应用尚未完全完成，你也可以开始测试你的工作。例如，你可以先放入一个 `img` 元素或一个 `a` 元素。打开浏览器，如 Firefox、Chrome 或 Safari。在 Firefox 中，点击“文件”，然后选择“打开文件”，浏览到你的 HTML 文件。在 Chrome 中，在 PC 上按 Ctrl（Mac 上按 Cmd），然后浏览到文件并点击“确定”以打开它。你应该能看到类似我示例中的内容。