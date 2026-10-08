## 第 6 章：精美的排版

```php
// 画布的内容：文本。我们已经有了字体文件的存放位置
// 以及文本的尺寸。在写入画布之前，我们还需要
// 指定其颜色。与之前的背景色一样，我们必须用十六进制
// 的各个部分来指定颜色，并调用
// ImageColorAllocate 来创建后续其他 PHP 方法所需的颜色引用。
$color_hex['red'] = 0x00;
$color_hex['green'] = 0x00;
$color_hex['blue'] = 0x00;
$color = ImageColorAllocate($image, $color_hex['red'], $color_hex['green'], $color_hex['blue']);
```

```php
// 在将文本渲染到画布之前，我们需要计算字体的基线
// 位置，这将是文本渲染的起始点，
// 正如我们在本章前面的图 6-2 中看到的那样。
$top = -$lowerLeftCornerX;
$left = abs($upperRightCornerY - $lowerRightCornerY) - $lowerLeftCornerY;
```

```php
// 将文本渲染到画布上。
ImageTTFText($image, $size, $angle, $left, $top, $color, $fontFilename, $text);
```

```php
// 最后，我们使用 ImagePNG 函数将画布以 PNG 图像文件的形式绘制到屏幕上，
// 首先向浏览器设置正确的响应头，
// 最后销毁创建的画布引用，以节省 PHP
// 应用程序的内存。
header('Content-type: image/png');
ImagePNG($image);
ImageDestroy($image);
```

假设你将代码清单 6-1 中的代码以文件名 `generate-image.php` 保存到 Web 服务器根目录，你可以在 HTML 和 CSS 中使用以下图像 URL 格式动态生成图像：

```
/generate-image.php?text=News
```

## 在 ASP.NET (C#) 中动态生成图像

正如你可能知道的，ASP.NET 平台支持多种编码语言。在下面的例子中，我选择使用 C# 语言，尽管将其转换为其他语言应该也相当容易，因为不同语言之间的 API 对象和方法几乎是相同的。

创建一个名为 `generate-image.aspx` 的新 ASPX Web 窗体，然后将代码清单 6-2 中的代码添加到自动生成的后台代码文件 `generate-image.aspx.cs` 中。

`代码清单 6-2.` `使用 C# 以自定义字体生成包含文本的图像`

# 动态生成图像的实现

```csharp
namespace CustomImage
{
    using System;
    using System.Drawing;
    using System.Drawing.Text;
    using System.Web;

    public class GenerateImage : System.Web.UI.Page
    {
        protected void Page_Load(object sender, EventArgs e)
        {
            // 从查询字符串参数 text 中提取我们要显示的文本。
            string text = Request.QueryString["text"];

            // 定义表示字体所需的其他信息的变量：
            // 字体文件位置、其大小、颜色以及我们希望显示文本的画布的
            // 背景颜色。
            string fontFileLocation = "font-file-location.ttf";
            int size = 24;
            Color color = Color.Black;
            Color backgroundColor = Color.White;

            // 接下来，我们创建一个新的 Bitmap 对象，它代表我们将放置文本的
            // 空白画布。然而，我们一开始就面临一个两难境地，
            // 因为我们想创建一个与要表示的文本大小相同的画布，
            // 但不幸的是，没有画布来放置文本，我们就无法测量
            // 文本的尺寸。
            // 克服这个障碍的方法是，从一个尺寸仅为 1 像素 x 1 像素的
            // 虚拟画布开始，将其作为我们文本测量的基础，
            // 然后用真正的画布替换它。
            Bitmap canvas = new Bitmap(1, 1);

            // 在 ASP.NET 中，Graphics 类包含我们绘制和
            // 测量画布上文本所需的方法。在使用这些方法之前的第一步是
            // 声明一个该对象的实例，并将其
            // 与我们的画布关联起来。
            Graphics graphics = Graphics.FromImage(canvas);

            // 在编写任何文本之前，我们需要定义字体。ASP.NET 定义了
            // 两种字体集合：系统上安装的字体和
            // 可以从文件中动态加载以供临时使用的私有字体集合。
            // 我们需要使用的是后者，因此我们
            // 创建一个新的私有字体集合，并使用我们之前声明的
            // TrueType 字体的文件位置将字体添加到其中。
            PrivateFontCollection myFonts = new PrivateFontCollection();
            myFonts.AddFontFile(Server.MapPath(fontFileLocation));

            // 单个字体总是与字体系列相关联，字体系列是
            // 一组彼此关联的字体集合。这些系列
            // 包含同一字体的变体，例如同一字体的粗体或斜体版本。
            // 现在我们已经将字体添加到私有字体集合中，
            // 它存储在其系列组中，该系列组已在
            // 字体文件中定义并被 ASP.NET 自动提取。
            // 要获得我们可以在代码中使用的字体引用，我们必须通过
            // 其字体系列来访问它。我们知道我们只添加了一种字体，
            // 它只能属于一个字体系列，因此我们可以在
            // 代码中获取这个字体系列。
            FontFamily myFontFamily = myFonts.Families(0);

            // 现在我们可以创建对该系列中特定字体的引用，
            // 使用所需的尺寸，我们选择以像素为单位进行测量。
            Font myFont = new Font(myFontFamily, size, FontStyle.Regular, GraphicsUnit.Pixel);

            // 现在我们有了文本和字体的定义，我们需要测量
            // 当文本写入画布时将占用的空间。我们
            // 使用 MeasureString 方法来实现这一点，向其传递要渲染的
            // 文本和字体定义，它会返回
            // 渲染后文本的宽度和高度尺寸。
            int width = Convert.ToInt32(graphics.MeasureString(text, myFont).Width);
            int height = Convert.ToInt32(graphics.MeasureString(text, myFont).Height);

            // 确定了渲染文本的宽度和高度后，我们现在
            // 需要使用这些尺寸重新创建画布，并为最终图像指定一个
            // 颜色深度（本例中为 24 位 RGB）。
            canvas = new Bitmap(width, height, PixelFormat.Format24bppRgb);
            graphics = Graphics.FromImage(canvas);

            // 我们已经创建了最终的画布，因此可以开始构建
            // 最终的图像了。让我们首先使用我们之前定义为白色的
            // 背景颜色填充画布。
            graphics.Clear(backgroundColor);

            // 现在，在将文本写入画布之前，我们需要
            // 生成一个 ASP.NET 画笔概念的实例，用于
            // 绘制文本。我们现在不想使用太花哨的东西，所以我们将
            // 实例化一个纯色画笔，使用我们之前分配的前景文本颜色。
            SolidBrush brush = new SolidBrush(color);

            // 好了，现在所有设置都已准备好，我们可以使用选定的字体
            // 将文本绘制到画布上。以下代码使用我们的画笔
            // 将文本绘制到画布上，从画布区域的左上角开始。
            int topLeftCornerX = 0;
            int topLeftCornerY = 0;
            graphics.DrawString(text, myFont, brush, topLeftCornerX, topLeftCornerY, StringFormat.GenericTypographic);

            // 我们的画布已经准备好显示，因此下一步是
            // 实际将其作为 PNG 格式的图像绘制到屏幕上。在此之前，
            // 我们首先向浏览器发送正确的内容类型头，
            // 以便它将数据显示为图像，最后我们释放
            // 画布占用的应用程序内存，供 Web 服务器的其余部分使用。
            Response.ContentType = "image/png";
            canvas.Save(Response.OutputStream, ImageFormat.Png);
            canvas.Dispose();
        }
    }
}
```

将 Web 窗体连接到新的后台代码文件后，你应该能够在 HTML 和 CSS 代码中使用以下图像 URL 格式，以你预定义的字体动态生成图像：

```
/generate-image.aspx?text=News
```

## 在 Java/JSP 中动态生成图像

# 生成包含自定义字体文本的 JSP 图像

你可能有一个支持 Java 的 Web 服务器，用于创建和运行 JSP 页面。在这种情况下，动态文本图像生成的过程与 PHP 和 ASP.NET 类似：创建一个空白画布，使用指定的字体将文本绘制到画布上，然后以正确的内容类型将该画布发送到浏览器。

将`清单 6-3`中的代码输入到一个新的 JSP 文件中，并将其保存为`generate-image.jsp`。

## 清单 6-3. 在 JSP 中生成包含自定义字体文本的图像

```
<%

// 首先，我们从查询字符串中获取 text 参数的值，该值将用作要在图像中渲染的文本。
String text = request.getParameter("text");

// 接下来，我们将声明一些变量供后续代码使用，用于存储字体文件位置、所需的文本尺寸，
// 以及文本和画布的前景色与背景色。
String fontFileLocation = "font-file-location.ttf";
float size = 24f;
Color color = Color.BLACK;
Color backgroundColor = Color.WHITE;

// 现在变量已定义完毕，让我们创建一个对象实例来表示我们想要使用的字体，
// 该字体将从服务器文件系统中获取，然后设置所需的尺寸。
Font font = Font.createFont(Font.TRUETYPE_FONT, ➥
new java.io.FileInputStream(fontFileLocation));
font = font.deriveFont(size);

// 在创建画布并将文本绘制到其上之前，我们首先通过使用一个临时容器来测量
// 以所选字体绘制的文本尺寸，从而确定画布所需的大小。
FontRenderContext fontRenderContext = new FontRenderContext();
TextLayout textLayout = new TextLayout(text, font, fontRenderContext);
Rectangle2D dimensions = textLayout.getBounds();
int width = (int)Math.ceil(dimensions.getWidth());
int height = (int)Math.ceil(dimensions.getHeight());

// 现在，我们已经确定了画布所需的宽度和高度，可以创建空白画布，并指定高色深。
BufferedImage canvas = new BufferedImage(width, height, ➥
BufferedImage.TYPE_INT_RGB);

// 在此空白画布上绘图所需的方法包含在 Java 的 Graphics2D 类中。
// 在使用这些方法之前，我们必须先将该对象的一个实例与我们的画布关联起来。
Graphics2D graphics = canvas.createGraphics();

// 下一步是用背景色填充画布，这通过设置要使用的颜色，然后创建一个实心矩形形状
// 以该颜色填充画布来实现。
graphics.setColor(backgroundColor);
int topLeftPositionX = 0;
int topLeftPositionY = 0;
graphics.fillRect(topLeftPositionX, topLeftPositionY, width, height);

// 画布填充好背景色后，我们现在可以准备在其上写入文本。
// 我们需要设置文本的颜色和字体，以及字体基线的顶部和左侧位置。
// 基线是一条假想的水平线，字体中的字符沿此线放置，如本章前面的图 6-2 所述。
graphics.setFont(font);
graphics.setColor(color);
float baseLinePositionX = (float)- dimensions.getX();
float baseLinePositionY = (float)- dimensions.getY();

// 颜色、尺寸和位置设置完毕后，我们终于可以将文本绘制到画布上。
graphics.drawString(text, baseLinePositionX, baseLinePositionY);

// 画布完成之后，剩下的工作就是将其输出到浏览器：首先正确设置响应的内容类型，
// 最后将画布使用的应用程序内存返回给服务器，供其他地方使用。
response.setContentType("image/png");
ServletOutputStream outputStream = response.getOutputStream();
PNGImageEncoder imageEncoder = ➥
PNGCodec.createPNGEncoder(outputStream);
imageEncoder.encode(canvas);
graphics.dispose();

%>
```

# 现在你应当能使用以下图片 URL 格式，在 HTML 和 CSS 中利用自定义字体动态生成图片：

`/generate-image.jsp?text=News`

## 扩展服务端脚本代码例程

清单 6-1、6-2 和 6-3 中的三个代码示例展示了使用自定义字体在图片中显示文本的基本例程结构。这些代码仍有很大的改进空间。以下是你在选择增强这些例程时可以考虑的一些建议：

- **防御性编码**：你几乎肯定需要为例程添加一些错误检查代码，以确保查询字符串参数确实被提供。通过消除代码中的假设，可以提升代码的稳定性。
- **缓存所有生成的图片**：为了提升性能，你应当考虑将每个生成的图片画布保存到磁盘上，或许可以使用与所渲染文本相似的文件名。这样一来，当未来再次请求相同的标题时，你可以检查该图片是否已经生成并存在于磁盘上。通过这种方式加载预计算的图片，可以节省大量的服务器处理能力和时间。
- **考虑文本换行**：某些需要替换的较长标题可能无法适应浏览器中 Web 应用的布局尺寸。在这种情况下，你需要具备在自定义的字符限制之前的最后一个单词边界处进行文本换行的能力。
- **添加文本平滑**：PHP、ASP.NET 和 Java 中的某些图形库允许你设置文本的平滑程度，即反锯齿效果。对字体进行平滑处理有助于避免文本边缘出现锯齿和低质量的效果。
- **添加其他查询字符串参数**：你可能希望某些标题以粗体、斜体、下划线形式显示，甚至使用不同的颜色或替代字体。为何不考虑通过额外查询字符串参数传递给例程，并根据这些参数值来改变文本属性？这比为了应对不同用途而重复代码要好得多。

### 使用 Flash 生成自定义字体文本

Adobe 的 Flash Player 是一款外部浏览器插件，几乎安装在所有最终用户的浏览器中。例外情况包括某些移动浏览器和较旧的浏览器。最新版本的安装统计数据可在 Adobe 网站 [`www.adobe.com/products/player_census/flashplayer/version_penetration.html`](http://www.adobe.com/products/player_census/flashplayer/version_penetration.html) 上获取。

在没有必要的情况下，你必须谨慎使用 Flash Player 或任何其他类型的插件。仅在需要提供浏览器本身不具备的功能时使用它，并且即便如此，也只有在为没有安装该插件的用户提供合适的备用内容时方可使用。使用 Flash Player 以内嵌字体显示标题正好符合这一要求。如果插件不存在，你可以回退到使用不同但合适的字体来显示相同的标题文本。

Flash 支持在电影文件中嵌入字体，甚至允许你指定要嵌入字体集中的哪些字符。这意味着你无需为每个电影嵌入所有字符，从而可以节省文件大小。例如，如果你仅向英语观众提供文本，你可以阻止所有扩展字符嵌入到 Flash 电影中。

最简单的方法是创建一个包含嵌入字体和动态文本图层的 Flash 电影。然后，该电影将期望从 HTML 接收一个包含待渲染文本的变量，通常通过电影 URL 自身的查询字符串传递给 Flash。Flash 电影会简单地接收传入的文本，并用提供的文本填充其文本图层，随后使用页面上的嵌入字体进行渲染。

### 使用矢量图形生成文本

许多主流浏览器都支持在浏览器窗口内绘制矢量图形。

# 矢量图形与可重用自定义字体组件

将矢量图形视为由线条和曲线构成视觉图像的普通图片。与其定义图像中的像素，不如从头描述如何通过这些形状来形成一张图片。

通过这种方式描述图像，可以使图像具有分辨率独立性和尺寸独立性。由于你始终知道如何根据描述图像构建方式的数据从头开始生成图像，因此只需改变一个矢量（例如宽度），图像就会自动适应新的空间而不会损失质量。实际上，大多数字体都是在其关联的字体文件中这样描述的，因此调整文本大小不会导致质量损失。在浏览器中绘制字体的方式与在字体文件中创建字体的方式相同，这为在`CSS`中嵌入实际所需字体提供了一种极佳的替代方案，如前所述。

以下浏览器支持各种形式的矢量图形绘制：

- Firefox 1.5 及以上版本
- IE 6 及以上版本
- Opera 9 及以上版本
- Safari 3.0 及以上版本（包括 iPhone）
- Google Chrome

如你所见，这涵盖了当今市场上的大多数浏览器。使用矢量图形是一种极具吸引力且可行的技术，用于以自定义字体呈现文本。我将在第 11 章中讨论浏览器内绘制这一主题。

## 使用可重用的自定义字体组件

到目前为止，你应该已经认识到可重用脚本的有用性。正如你所期望的，有一些现成的组件可以以跨浏览器友好的方式将自定义字体标题应用到你的页面中。那些经历过应用本章讨论的技术所带来的痛苦，并成功走出困境的开发者们，已经将他们编写的代码打包，并免费提供给我们在自己的 RIA（富互联网应用）中使用。

在本节中，我们将介绍一些较流行的预打包组件：`Text2PNG`、`Scalable Inman Flash Replacement`、`Facelift Image Replacement`和`Typeface.js`。我建议你研究每一个组件，找到适合你需求的组件，并在你自己的 Web 应用程序中尝试使用。

### Text2PNG

通过使用`Text2PNG`组件（[`text2png.com/`](http://text2png.com/)），你可以将页面上的文本替换为使用所选字体渲染的图像。你可以选择替换特定的文本实例、替换特定标签中的文本，或者替换所有文本。

`Text2PNG`组件通过 Web 服务器动态生成图像。你可以选择自行托管代码的服务器端部分，或者调用托管在开发者服务器上的代码。如果你自行托管代码，在运行 PHP 4.3 或更高版本的服务器上，你可以使用上传到服务器上特定文件夹的自定义字体。否则，你将仅限于使用开发者服务器上安装的约 200 种预定义字体。

#### Text2PNG 使用示例

在你的页面中设置该组件，只需包含预编写的`JavaScript`代码；编写你的`CSS`以分配你希望使用的字体、颜色和大小；并添加几行你自己的`JavaScript`代码来对你选择的文本执行图像替换。

为了表示图 6-1 示例中的标题，你可以编写以下`HTML`代码，其中`text_script.js`是提供的组件`JavaScript`文件：

```html
<!DOCTYPE html PUBLIC "- //W3C//DTD XHTML 1.0 Strict//EN"
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
<html lang="en" xml:lang="en">
<head>
    <title>Text2PNG 示例</title>
    <link rel="stylesheet" src="myExample.css" type="text/css" />
</head>
<body>
    <h1 id="title">新闻</h1>
    <h2 id="news-headline">新产品发布：人体传送器</h2>
    <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit...</p>
    <script src="text_script.js" type="text/javascript"></script>
    <script src="my-example.js" type="text/javascript"></script>
</body>
</html>
```

在引用的 CSS 文件`my-example.css`中，首先指定你要使用的自定义字体，然后列出大多数浏览器默认字体列表中的可接受后备字体：

```css
h1 {
  font-family: 'Sketchy Rockwell', Arial, Helvetica, sans-serif;
  font-size: 1.6em;
}

h2#news-headline {
  font-family: 'Silom', Times New Roman, serif;
  font-size: 1.3em;
}
```

在你的 JavaScript 文件`my-example.js`中，定义你希望用自定义字体文本图像替换的标题、文本或页面部分。有以下三种方法可用：

- `replace()`：允许你替换单个元素的内容。
- `replaceTags()`：允许你替换所有指定命名标签的内容。
- `replaceAll()`：允许你替换所有子元素内的内容。

让我们逐一了解每种方法。

> **注意**：请确保在通过 JavaScript 读取 DOM 之后（使用你在第 2 章开始的`$` JavaScript 库的`$.onDomReady()`事件）再调用任何 JavaScript 方法。如果你在页面生命周期中过早调用这些文本替换方法，将不会有文本被替换，甚至可能在浏览器中生成 JavaScript 错误，并向最终用户显示（这是不可接受的！）。

#### 替换单个元素

我们首先掌握的方法是`replace()`，它会从页面元素的全部文本内容创建一个 PNG 图像文件。指定标签内的任何子元素或 HTML 标签都将被移除，替换为文本的图像表示。使用示例 HTML，你可以通过以下方法调用来替换`<h2 id="news-headline">`标签内的标题文本：

```javascript
text2png.replace("news-headline");
```

此方法期望接收一个表示要替换元素`id`属性的字符串（如上例所示），或一个 DOM 元素，如下所示：

```javascript
text2png.replace(document.getElementById("news-headline"));
```

两种方式执行相同的功能，具体使用哪种取决于你。

替换多个特定页面元素与向`replace()`方法传递额外参数一样简单，每个要替换的元素对应一个参数。假设你想替换之前的`<h2 id="news-headline">`元素，以及另一个页面元素`<h1 id="title">`中的文本。这可以通过一次方法调用完成，如下所示：

```javascript
text2png.replace("news-headline", "title");
```

#### 替换特定命名标签内的所有内容

Text2PNG 内置的`replaceTags()`方法执行与`replace()`方法相同类型的替换，但它会替换所有具有给定标签名的元素。假设像前面的 HTML 示例一样，你想将页面中的所有`<h1>`和`<h2>`标题标签替换为其文本图像等效项。为此，只需使用`replaceTags()`方法，如下所示：

```javascript
text2png.replaceTags("h1", "h2");
```

此方法将替换当前页面上所有指定标签的文本内容，无论每种标签有多少个。这是一种快速便捷的执行替换的方式。传递给此函数的参数数量没有限制，因此你可以选择只替换单个标签的内容，或多个标签的内容。

#### 替换所有子元素的内容

`replaceAll()`方法提供了一种简单的方法来替换传递给方法调用的元素下方所有嵌套 HTML 元素的内容。举个例子，假设你的 HTML 文档中有一个项目符号列表，如下所示：

```html
<ul id="list">
  <li>Faster and more efficient</li>
  <li>Smaller and lighter</li>
  <li>Beautiful and elegant</li>
</ul>
```

要将其图像等效项替换每个`<li>`标签内的文本，请使用`replaceAll()`方法，并向其传递外围元素的`id`属性值。这样，所有子元素的文本将一次性被替换：

```javascript
text2png.replaceAll('list');
```

此方法也支持多个参数。例如，如果你有两个列表，`<ul id="list">`和`<ul id="other-list">`，你可以通过对`replaceAll()`方法的单个调用来替换两组子节点的文本，如下所示：

```javascript
text2png.replaceAll("list", "other-list");
```

#### 使用 Text2PNG 应用额外的文本格式

Text2PNG 的`replace()`、`replaceTags()`和`replaceAll()`方法每个都接受一个可选的最终参数，该参数指定为一个 JSON 对象，用于设置生成文本图像的某些格式属性。在可能的情况下，Text2PNG 将尝试从应用于原始文本的 CSS 中推导出图像的显示格式（在文本被替换之前）。如果没有应用 CSS 属性，它接着会尝试从传递给方法调用的`options`对象中建立格式（如果设置了的话）。如果这些都不适用，则内部设置默认值以在需要时提供后备格式。

以下示例展示了如何将页面上的所有`<h2>`标签替换为其文本图像等效项，并在此过程中应用一些额外的格式选项：

```javascript
text2png.replaceTags("h2", {
  opacity: 50,
  bold: 1
});
```

应用于生成文本的完整选项列表和 CSS 规则可以在项目网站（[`www.text2png.com/`](http://www.text2png.com/)）的文档中找到。

## Text2PNG 评估

Text2PNG 组件最初吸引我的地方之一是它能够从被替换文本的 CSS 中读取并评估应将哪些样式应用于生成的图像。对我来说，样式和视觉信息应存储在我的 CSS 样式定义中，无论我是否使用 JavaScript 来执行替换，这都完全合理。

通过在我元素的`font-family` CSS 属性中指定我想使用的自定义字体以及一系列替代字体，我以常规方式使用 CSS。极少数情况下，我的最终用户可能在其浏览器中禁用了 JavaScript，但恰好在其计算机上安装了我指定的自定义字体；在这种情况下，字体将按我预期显示，并且不会进行图像替换。

国际语言通过 UTF-8 字符集得到支持，前提是所使用的自定义字体中存在这些字符。目前存在的唯一限制是仅支持从左到右阅读的文本，因此排除了阿拉伯语和希伯来语等语言。

在使用第三方脚本时，性能始终是我主要关心的问题之一，我很高兴地报告，Text2PNG 的开发者在这方面已做到无微不至。开发者声称在使用其组件的过程中只会发生五个 HTTP 请求，这包括从最初加载脚本到将最终图像传输回浏览器的全过程。

无论发生了多少次替换，只有一张图像被发送回浏览器，因为脚本使用了 CSS 精灵技术（在第 4 章中描述）来将所有要使用自定义字体显示的文本合并到一个图像文件中。不幸的是，CSS 精灵技术目前不适用于 IE 6 或 Opera。然而，返回给所有浏览器的图像都在服务器和浏览器中进行了缓存，其过期日期设置为从图像首次生成之日起十年以后。

大多数浏览器都支持此组件，包括 IE 6 及以上版本、Firefox 2 及以上版本、Safari 3 及以上版本以及 Google Chrome。列表中缺少 Opera 浏览器。

当 JavaScript 尝试读取应用于页面元素的`font-family`样式时...

Opera 只会将实际应用的字体返回给脚本。由于自定义字体很可能不存在于用户的机器上，Opera 只会返回实际应用的后备字体名称。由于该组件需要能够读取自定义字体的名称，才能请求服务器用该字体在图像中渲染文本，因此在 Opera 中会出现问题。希望未来的 Opera 更新能解决这个问题，使其用户不再受到影响。话虽如此，由于这纯粹是一种视觉效果，只要存在合适的后备字体，用自定义字体显示相同的文本实际上并非必要，而是可以被视为“锦上添花”。

## 可伸缩的 Inman Flash 替换技术

可伸缩的 Inman Flash 替换技术（`sIFR`）组件最初由 Shaun Inman（[`shauninman.com/`](http://shauninman.com/)）设计，因其灵活性、可复用性以及应用于现有页面的速度而广受欢迎。

该组件依赖于本章前面讨论的 Adobe Flash 技术。要使用它，您必须根据提供的模板创建一个自定义 Flash 电影文件，并将您希望显示的自定义字体以及所需的任何特定扩展字符嵌入到该电影中。创建好 Flash 电影文件后，在您的页面中引用提供的 JavaScript 和 CSS 文件，然后通过开发者脚本提供的 JavaScript API 执行您需要的特定文本替换操作。

在撰写本文时，`sIFR` 组件的当前完整发布版本是 2.0.7，其输出端的 Flash 电影文件兼容 Flash Player 版本 6（该插件的当前发布版本是 10）。另一条独立的开发路线已由 Shaun Inman 授权给另一位开发者 Mark Wubben。`sIFR` 的此版本目前处于早期测试阶段，将成为该组件的第 3 版，兼容 Flash Player 版本 8 及以上，使 Web 开发人员能够利用更高版本 Flash Player 中提供的许多排版和文本样式改进。

`sIFR` 第 2 版的完整文档和下载详情可在 [`wiki.novemberborn.net/sifr/`](http://wiki.novemberborn.net/sifr/) 找到，第 3 版的信息则在 [`wiki.novemberborn.net/sifr3/`](http://wiki.novemberborn.net/sifr3/)。

### sIFR 使用示例

让我们以图 6-1 所示的页面为目标，使用 `sIFR` 第 3 版来演示一个示例。在 HTML 文件中链接必要的样式表和 JavaScript 文件，即可引入该组件，如下所示：

```html
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN"
 "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
<html lang="en" xml:lang="en" xmlns="http://www.w3.org/1999/xhtml">
<head>
<title>sIFR 3 示例</title>
<link rel="stylesheet" src="sifr.css" type="text/css" />
</head>
<body>
  <h1>新闻</h1>
  <h2 id="news-headline">新产品发布：人体传送器</h2>
  <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit...</p>
  <script src="sifr.js" type="text/javascript"></script>
  <script src="sifr-config.js" type="text/javascript"></script>
</body>
</html>
```

假设您已经下载了该组件，并按照说明将所需字体导出到自定义的 Flash 文件中，那么您现在必须在脚本中引用这些电影文件，以便在页面中使用它们。在组件提供的 `sifr-config.js` 文件中附加几行代码，以引用您嵌入字体的 Flash 文件：

```javascript
var silom = {
  src: 'silom.swf'
};

var sketchyRockwell = {
  src: 'sketchy-rockwell.swf'
};
```

请注意，`src` 属性包含 Flash 文件的 URL，该路径是相对于其所在页面的。

在执行任何文本替换之前，下一步是将您的 Flash 电影文件加载到浏览器中。这是为了避免每次替换时都重新加载电影文件。执行此操作，请在 `sifr-config.js` 中附加以下代码行：

```javascript
sIFR.activate(silom, sketchyRockwell);
```

现在，您就可以实际执行文本替换了。这通过调用 `sIFR.replace()` 函数来实现，将您的字体分配给您希望替换的页面标签或元素：

```javascript
sIFR.replace(sketchyRockwell, {
  selector: 'h1'
});

sIFR.replace(silom, {
  selector: 'h2'
});
```

内置在 `sIFR` 中的选择器引擎相当灵活。除了可以使用逗号分隔多个选择器来同时执行特定字体的多次替换外，您还可以使用以下任意基于 CSS 选择器的选择格式来选择特定的标签和元素：

- `h1`

- `h1 > em`

- `h1 em`

- `.foo`

- `h1.foo`

- `#bar`

- `h1#bar`

- `h1#bar.foo`

- `#bar.foo`

- `.foo.baz`

在完成所有必需的文本替换后，您可能需要调整大小，因为在某些情况下，`sIFR` 组件无法设置正确的字体大小。如有必要，编辑 `sifr.css` 文件，在文件的 `@screen` 部分末尾附加以下代码：

```css
.sIFR-active h1 {
  visibility: hidden;
  font-family: Verdana;
  line-height: 1em;
  font-size: 24px;
}
```

当 `sIFR` 脚本初始化时，`.sIFR-active` 类会被添加到页面的 `<body>` 元素中，因此该 CSS 规则只有在初始化后才能生效。将文本的 `visibility` 属性设置为 `hidden`，以便在文本替换发生后不再显示原始页面文本。在规则末尾设置显示所用的字体大小。请记住，只有在执行上一步替换时出现显示问题，才需要设置此样式以及除 `visibility` 之外的任何其他样式。

### 使用 sIFR 应用额外文本格式

Adobe Flash 支持 CSS 的一个子集，这使得您可以通过常规样式表来设置某些文本格式。CSS 类 `.sIFR-root` 应用于每个 Flash 电影，允许您自定义电影本身及其内容的样式。由于 `sIFR` 组件（作为 Flash 技术）会保留对被替换元素的任何子元素的引用，因此这些子元素也可以通过 CSS 在 Flash 电影内部直接设置样式。

例如，假设您正在替换一个列表项元素 `<li>`，并且该标签包含文本和一个嵌套的链接元素 `<a>`。该链接本身也会被传递到 Flash 电影中，使其继续保持可点击状态，并可以与其余文本设置不同的样式。仅凭这一能力，这种方法就优于许多其他文本替换方法。

默认情况下，自定义的 Flash 电影文件支持粗体、斜体、下划线和普通文本。字距和行距属性可以通过 JavaScript 和 CSS 配置进行必要的调整。还可以通过进一步自定义 Flash 电影文件以包含这些效果，或通过 Flash Player 8 及以上版本内置的一系列滤镜来实现额外的文本效果，`sIFR` 使这些滤镜可通过 JavaScript 进行配置。这些滤镜包括为文本添加阴影、模糊文本、在文本周围添加发光效果等等。请参阅完整的 `sIFR` 第 3 版文档，了解可用效果列表以及如何在页面中使用它们。

### sIFR 评估

`sIFR` 组件在替换页面上的少数几个标题时效果良好，并且如果您希望使用不同的大小和样式，它也非常灵活。但是，如果您需要在页面上替换超过五到六个标题，则不建议使用此组件。在这种情况下，性能会明显下降，因为浏览器内存被推向极限，导致页面变慢并显著影响页面加载时间。

此组件支持 IE 6 及以上版本、Firefox 2 及以上版本、Safari 2 及以上版本、Google Chrome 和 Opera 9.5 及以上版本。一个明显的需求是最终用户的浏览器必须安装 Flash Player 插件。如果检测不到该插件，则不会进行任何替换，并显示原始的浏览器文本，因此您无需担心 JavaScript 受支持但 Flash 不受支持的情况。

此组件的性能随着每个版本的发布而不断提升。众所周知，Flash 会消耗大量浏览器内存。开发人员已确保导出的每个 Flash 影片都具有较低的刷新率，以进一步改善这一状况。Flash 影片文件会被缓存在浏览器中，因此它们只需要下载一次，随后即可在后续页面中从缓存中自由重复使用。默认情况下，每个影片文件会消耗约 50KB 到 60KB 的磁盘空间，如果文件内嵌入了额外字符，空间占用会更大。经过压缩后，`sIFR` 版本 3 的 JavaScript 文件本身大小约为 32KB。JavaScript 和 Flash 的组合重量使其成为较大的第三方文字替换组件之一。

此组件已经存在了几年，因此可以认为它相当成熟、稳定且支持良好。未来的开发将重点放在，在其他方面，能够将该代码搭载到其他 JavaScript 库上，包括 jQuery 和 Prototype。这意味着，未来代码重复将减少，并且如果您已经在代码中使用了这些库之一，该组件的 JavaScript 体积将大大减小。

再次强调，如果您希望替换的不仅仅是少数几个标题，绝对不要使用此技术，因为对性能的影响可能会让您的最终用户感到相当困扰。

## 第 6 章：精美的排版

## Facelift Image Replacement

一个新的组件，Facelift Image Replacement（FLIR，发音为“fleer”），与之前讨论的 Text2PNG 组件有相似之处。两者都使用服务器端图像生成技术，依赖 PHP 服务器将图像传送到请求的网页。

一个显著的区别是，使用 Text2PNG，您可以免费使用开发者自己的服务器来执行生成操作，但使用 FLIR，这项服务需要付费（诚然，这个费用仅为每月 1 美元）。

要在您自己的服务器上启动并运行该组件，需要安装 PHP。此外，为了支持某些扩展效果，还需要安装 ImageMagick（`http://www.imagemagick.org/`）。

### FLIR 示例

从其在线主页 [`facelift.mawhorter.net/`](http://facelift.mawhorter.net/) 下载 FLIR 组件，并将文件安装到您的 Web 服务器上。接下来，将您的 TrueType 字体文件复制到服务器，并在 `config-flir.php` 文件中创建一个引用，将字体文件与 CSS `font-family` 值关联起来（文件中提供了一些示例以帮助您入门）。然后，设置您的 HTML 文件，引用 FLIR 组件的 JavaScript 文件，并包含您自己的 JavaScript 文件以及您自己的 CSS 文件：

```html
<!DOCTYPE html PUBLIC "- //W3C//DTD XHTML 1.0 Strict//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
<html lang="en" xml:lang="en">
<head>
<title>FLIR 示例</title>
<link rel="stylesheet" src="my-example.css" type="text/css" />
</head>
<body>
<h1>新闻</h1>
<h2 id="news-headline">新产品发布：人体传送器</h2>
<p>Lorem ipsum dolor sit amet, consectetur adipiscing elit...</p>
<script src="$.js" type="text/javascript"></script>
<script src="flir.js" type="text/javascript"></script>
<script src="my-example.js" type="text/javascript"></script>
</body>
</html>
```

现在，配置 FLIR 仅替换您选择的页面元素。在您自己的 JavaScript 文件 `my-example.js` 中使用以下代码开始：

```javascript
$.onDomReady(function() {
   FLIR.init({
      path: '/path-to-flir/'
   });
   FLIR.auto();
});
```

默认情况下，当调用 `FLIR.auto()` 方法时，FLIR 会替换所有标题标签 `<h1>` 到 `<h5>`。但是，可以通过传入一个与您希望替换的元素相对应的 CSS 选择器数组来覆盖此功能。在此示例中，您可以使用以下代码仅替换您希望使用自定义字体的两个标题：

```javascript
FLIR.auto(['h1', 'h2#news-headline']);
```

默认情况下，FLIR 使用与 Text2PNG 组件相同的技术来确定您希望应用于文本的字体：它会检查您应用于被替换元素的 `font-family` CSS 值。然后，使用您在 `config-flir.php` 中的配置，将此 `font-family` 值与服务器上的字体文件关联起来，并在替换后显示正确的字体。

要执行单个替换，或更好地控制显示的文本样式，您可以使用 `FLIR.replace()` 方法，它接受一个可选的参数来定义文本样式。以下是如何对 HTML 示例中的 `<h2 id="news-headline">` 标签执行简单的单个替换：

```javascript
FLIR.replace('h2#news-headline');
```

要为生成的文本图像设置更多样式选项，您可以将一个 `FLIRStyle` 对象作为附加参数添加到 `FLIR.replace()` 方法中——在此例中，用于指定所需的确切字体以及让文本多行换行：

```javascript
FLIR.replace( 'h2#news-headline', new FLIRStyle({
   cFont: 'silom',
   mode: 'wrap'
}));
```

有关 `FLIRStyle` 选项的完整列表，请参阅 FLIR 网站上的文档。

FLIR 还拥有一个支持插件的架构，目前有两个可用的 FLIR 插件：`FancyFonts` 和 `QuickEffects`。这些插件让您可以通过 JavaScript，对替换后的文本使用安装在 Web 服务器上的 ImageMagick PHP 组件。更多关于使用这些插件或添加您自己插件的详细信息，可以在该组件的网站上找到。

### FLIR 评估

如您所见，`FLIR` 组件与 `Text2PNG` 组件类似。由于两者都使用相同的 JavaScript 技术来读取应用于页面元素的当前样式，因此它们支持的浏览器也相同：IE 6 及以上版本、Firefox 2 及以上版本、Safari 3 及以上版本和 Google Chrome。与 `Text2PNG` 一样，目前不支持 Opera 浏览器。在其他浏览器中，将显示默认的浏览器文本，仅使用通过 CSS 确定的备用 `font-family` 值。

此组件的性能评分略低于 `Text2PNG`，因为 `FLIR` 不支持 CSS 精灵；每个被替换的标题图像都会单独下载。这意味着可能增加多个 HTTP 请求和响应，从而降低完成页面的显示速度。（显然，如果支持 CSS 精灵图像，此组件可以得到极大改进，这可能会在未来的版本中添加。）

`FLIR` 的易用性值得称赞。在自定义 CSS `font-family` 字体名称与服务器上的特定 TrueType 字体文件之间建立直接链接非常轻松。默认的替换机制会替换最标准的 HTML 标题标签，这捕获并简化了大多数开发人员对此类组件的期望。请记住，像这样的第三方组件设置和使用越容易，后续的维护任务通常也越简单。

为您的页面提供 `FLIR` 支持的 JavaScript 包含文件，经过压缩后大小约为 16KB。服务器生成的每个图像大小不一，但平均每个图像通常约为 2KB，这使得此组件相对轻量。

`FLIR` 是一个相当年轻的组件。这未必是一个问题，但它可能意味着该社区支持将较为有限，直到其存在时间更长一些，并解决更多漏洞为止。

总体而言，该组件运用了一些巧妙的想法来生成样式精美的标题。然而，其性能并非最佳。此外，它要求你的网络服务器运行 PHP。

## Typeface.js

免费的 `Typeface.js` 组件（`http://typeface.neocracy.org/`）是近期加入在线自定义字体文本渲染工具集的新成员。目前它仍应被视为一个进行中的项目，但基于本章前面介绍的矢量图形技术，它展示出作为一款简单组件的前景。

字体文件从 TrueType 格式转换为一个 JavaScript 文件，该文件描述了字体的各种字形和属性。随后，这些信息通过 JavaScript 程序转换为矢量，并绘制在页面上，以替代你想要替换的文本。你可以使用该项目网站上的工具将字体文件转换为 JavaScript 格式。

### Typeface.js 使用示例

让我们用图 6-1 中的标题来演练本章中一直在做的相同示例。从 HTML 开始：

```html
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN"
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
<html lang="en" xml:lang="en">
<head>
<title>Typeface.js 示例</title>
<link rel="stylesheet" src="my-example.css" type="text/css" />
</head>
<body>
<h1>新闻</h1>
<h2 id="news-headline">新产品发布：人类传送器</h2>
<p>Lorem ipsum dolor sit amet, consectetur adipiscing elit...</p>
<script src="typeface-0.11.js" type="text/javascript"></script>
<script src="sketchy-rockwell.typeface.js" type="text/javascript"></script>
<script src="silom.typeface.js" type="text/javascript"></script>
</body>
</html>
```

包含您自己的样式表文件 `my-example.css`，以定义您的字体和其他样式：

```css
h1 {
font-family: 'Sketchy Rockwell', Arial, Helvetica, sans-serif;
font-size: 1.6em;
}

h2#news-headline {
font-family: 'Silom', Times New Roman, serif;
font-size: 1.3em;
}
```

你可能想知道为什么你没有像处理本章讨论的其他第三方组件那样，包含一个单独的 JavaScript 文件来初始化 `Typeface.js` 脚本并执行特定的替换操作。默认情况下，`Typeface.js` 会替换所有标题标签（`<h1>` 到 `<h6>`），这些标签的文本已通过 CSS 指定使用通过 `Typeface.js` 网站生成的 JavaScript 字体描述文件加载的自定义字体。请记住，矢量与分辨率和尺寸无关，可以在不损失质量的情况下调整大小。这意味着通过单个 JavaScript 文件中的字体表示，所有尺寸的文本都能得到支持。如果字体是以这种方式描述的，`Typeface.js` 将用基于矢量的渲染来替换文本，以显示你的自定义字体。如果未指定自定义字体，则不会发生替换。

现在，假设你想要替换 HTML 中不在标题标签内的某些文本。谢天谢地，你仍然不需要编写任何 JavaScript 来实现这一点。假设你的页面上有一个段落标签内的文本想要替换：

```html
<p class="coming-soon">该产品即将推出！</p>
```

通过该元素的 CSS 属性 `font-family` 设置你想要显示的自定义字体名称，并确保包含对该字体 JavaScript 表示的引用。

要告诉 `Typeface.js` 执行替换，只需将 CSS 类名 `typeface-js` 添加到该元素，如下所示：

```html
<p class="coming-soon typeface-js">该产品即将推出！</p>
```

一旦页面加载完毕，替换操作就会执行。通过 CSS 样式启用功能，你避免了需要一个单独的 JavaScript 文件来包含执行此替换的代码。

### Typeface.js 评估

`Typeface.js` 是一个较新的组件，使用了相当先进的浏览器技术。矢量绘制技术非常适合表示 TrueType 字体文件，因为后者本身就是矢量定义。然而，浏览器性能是当前影响该技术采用的主要问题。

浏览器支持情况相当不错，包括 IE 6 及以上版本、Safari 2 及以上版本、Firefox 1.5 及以上版本以及 Google Chrome。请注意，Opera 再次从支持列表中缺席，原因与本章涵盖的其他组件（除了 `sIFR`）相同。

在大多数浏览器中，矢量绘制性能明显较差，尤其是在 IE 6 和 7 中。在矢量图形的浏览器性能全面改善之前，我建议使用 `Typeface.js` 进行文本替换时，每页限制在五到六次左右。超过这个数量，你就有可能激怒最终用户。

`Typeface.js` 的 JavaScript 文件压缩后大约为 10KB，每个字体定义文件预计约 60KB 到 70KB。由于该组件仅为最终用户带来微小的视觉变化却需要相当沉重的下载负担，请谨慎选择。

该组件以及另一个使用相同技术且非常相似的组件——`Cufón`（`http://wiki.github.com/sorccu/cufon/about/`），将是未来几个月或几年内值得关注的焦点。随着浏览器变得更加强大，Web 开发者可以利用的能力也随之增加，从而帮助我们改善最终用户的体验。

### 总结

本章介绍了几种不同的技术，并评估了一系列第三方组件，这些组件允许你在网页上使用浏览器本身不直接支持的自定义字体显示文本。现在，你应该能够自行决定哪种技术最适合你和你的 Web 应用程序。利用该技术为你的网页添加美观的排版文本，同时尽量减少对网站性能、可访问性和可维护性的影响。当然，如果这些技术或组件都不适合你，请随意尝试自己发明的其他技术。如果你创造出有效的东西，请与 Web 开发社区的其他成员分享你的经验，这样我们都能从中受益。

在下一章中，我们将研究用于在 Web 应用程序中显示和控制音频与视频播放的不同方法和组件。
