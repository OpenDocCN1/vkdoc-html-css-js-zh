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