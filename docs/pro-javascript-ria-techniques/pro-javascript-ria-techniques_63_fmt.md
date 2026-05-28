# 排版后的文档

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