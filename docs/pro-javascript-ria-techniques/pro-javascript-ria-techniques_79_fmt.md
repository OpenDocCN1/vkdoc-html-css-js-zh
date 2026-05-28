# 图 11-1. 一个简单的 SVG 图像文件

## 在 HTML 中指定 SVG

使用 SVG 创建独立的图像文件是可行的，但这种方法无法提供动态渲染功能。通过直接在 HTML 页面中定义图像，你可以利用 DOM 与画布中的各个元素进行动态交互。

`清单 11-2` 展示了如何使用 XHTML 描述图 11-1 的图像，无需任何外部图像文件引用。

你必须以 `.xhtml` 扩展名保存`清单 11-2` 中所示的文件，这样才能强制浏览器同时使用其 XML 和 HTML 解析器来渲染页面，而不仅仅是使用其 HTML 解析器。XML 解析器包含渲染 SVG 所需的代码，并且浏览器只有在绝对确定页面内容是 XML 时，才会使用其 XML 解析器。这可以通过指定文件扩展名为 `.xhtml`，或确保 Web 服务器发送 MIME 类型为 `application/xhtml+xml` 来描述文件内容来实现。实际上，应该使用这种 MIME 类型来发送所有 XHTML 文件内容，而不是像普通 HTML 文档那样使用常见的 `text/html` MIME 类型。不使用它的唯一原因是 IE 不识别此 MIME 类型，并拒绝渲染页面。

**清单 11-2.** *在 XHTML 页面中描述的简单 SVG 图像*

```
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN"
["http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">](http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd)
[<html lang="en" xml:lang="en">](http://www.w3.org/1999/xhtml)
<head>
<title>SVG Demo</title>
</head>
<body>
<h1>SVG Demo</h1>
<svg width="1024" height="768" version="1.1"
[>](http://www.w3.org/2000/svg)
<linearGradient id="gradient" gradientUnits="userSpaceOnUse"
x1="0" y1="0" x2="200" y2="0">
<stop offset="0" stop-color="#000000" />
<stop offset="1" stop-color="#cccccc" />
</linearGradient>
<rect x="20" y="20" width="200" height="200" fill="url(#gradient)" />
<rect x="220" y="220" rx="20" ry="20" width="250" height="100"
style="fill**:**#330000**;** "/>
<circle cx="350" cy="120" r="50" stroke="#336699"
stroke-width="25" fill="#003300" />
<line x1="20" y1="20" x2="320" y2="320"
style="stroke**:**#cccccc**;** stroke-width**:**3"/>
<g font-size="50" font-weight="bold" fill="#333333">
<text x="200" y="460">Text Example</text>
</g>
</svg>
</body>
</html>
```

这种方法并不是特别跨浏览器友好。IE 不识别 `.xhtml` 文件扩展名或 `application/xhtml+xml` MIME 类型。尽管 IE 不支持 SVG，你仍然希望你的 XHTML 页面能在该浏览器中加载。因此需要另一种解决方案。

## 通过 JavaScript 指定 SVG

值得庆幸的是，有一种解决方案允许你编写标准的 HTML 页面，同时仍然在你的 Web 应用程序中创建 SVG 图像，那就是使用 JavaScript 动态地将 SVG 元素添加到页面中，确保使用浏览器的 XML 解析器来创建这些元素。`清单 11-3` 展示了使用 JavaScript 动态创建 SVG 元素来呈现图 11-1 图像的初步实现。

**清单 11-3.** *通过 JavaScript 描述的简单 SVG 图像*

```
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN"
["http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">](http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd)
[<html lang="en" xml:lang="en">](http://www.w3.org/1999/xhtml)
<head>
<title>SVG Demo</title>
</head>
<body>
<h1>SVG Demo</h1>
<!-- 在页面上包含脚本块用于演示 -
这通常存在于一个单独的 JavaScript 文件中 -->
<script type="text/javascript">
// document.createElementNS 使用定义的命名空间创建一个新元素
// - 这里使用 URL 引用的 SVG 命名空间。
// 根据第二个参数，创建的标签是 <svg>。由于命名空间是 XHTML 和 XML 的一部分，
// 该元素将使用浏览器的 XML 解析器进行渲染——这正是我们在页面上表示 SVG 所需要的
var svg = document.createElementNS(
['http://www.w3.org/2000/svg', 'svg');](http://www.w3.org/2000/svg)
svg.setAttribute('width', '1024');
svg.setAttribute('height', '768');
// 动态创建 <circle> 元素并将其添加到之前创建的 <svg> 画布中
var circle = document.createElementNS(
['http://www.w3.org/2000/svg', 'circle');](http://www.w3.org/2000/svg)
circle.setAttribute('cx', '350');
circle.setAttribute('cy', '120');
circle.setAttribute('r', '50');
circle.setAttribute('stroke', '#336699');
circle.setAttribute('stroke-width', '25');
circle.setAttribute('fill', '#003300');
svg.appendChild(circle);
// TODO: 在此处以类似方式表示所有其他矢量元素
// 将 <svg> 标签添加到页面中，从而渲染图像
document.body.appendChild(svg);
</script>
</body>
</html>
```

## 使用向量标记语言绘图

早在 1998 年，微软向 W3C 提交了自己的基于 XML 的矢量图形格式提案，称为向量标记语言（VML），以供考虑作为推荐标准。由于 Adobe、Sun 和其他公司也在提交他们自己的类似格式，W3C 决定开发一种统一的格式供所有人使用，这最终在 2002 年成为了 SVG 推荐标准，正如前一节所讨论的那样。不幸的是，对于 Web 开发者来说，微软的开发者选择不采用 SVG——并且在 IE 8 中仍然没有采用——而是继续开发和完善他们的 VML 格式。

从版本 5 开始，所有 IE 版本都支持 VML，但其他任何浏览器都不支持。一个跨浏览器的矢量图形库需要同时支持 SVG 和 VML，才能成为开发者的可行解决方案。

VML 与 SVG 有着显著的相似之处，并且支持几乎相同的功能。然而，VML 更简洁的格式使其文件大小比使用 SVG 表示相同图像的等效文件更小。有趣的是，VML 元素使用 CSS 进行定位，并且不需要一个单一的元素作为画布来包围它们。标签可以直接添加到页面的任何位置。`清单 11-4` 展示了在图 11-1 中的图像如何在 HTML 页面中使用 VML 表示。

**清单 11-4.** *在 HTML 页面中描述的简单 VML 图像*

```
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN"
["http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">](http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd)
[<html lang="en" xml:lang="en">](http://www.w3.org/1999/xhtml)
<head>
<title>SVG Demo</title>
<!-- 要在页面中使用 VML，请使用 <style> 元素来启用其行为 -->
<style>v\: * {behavior:url(#default#VML); display: inline-block;}</style>
<!-- 接下来，声明 VML 命名空间以匹配该行为 -->
<xml:namespace ns="urn:schemas-microsoft-com:vml" prefix="v" />
</head>
<body>
<h1>SVG Demo</h1>
<v:rect style='position: absolute; top: 20px; left: 20px; width: 200px;
height: 200px' fillcolor="#000000">
<v:fill method="linear sigma" color2="#cccccc" angle="90"
type="gradient" />
</v:rect>
<v:oval style="top: 120px; left: 350px; width: 50px; height: 50px;"
fillcolor="#003300" strokecolor="#336699" strokeweight="25px" />
<v:line from="20px,20px" to="320px,320px" strokecolor="#cccccc"
strokewidth="3px" />
<!-- VML 文本不能直接写入页面；它必须被另一个形状包围。这里，我们创建一个矩形来容纳文本 -->
<v:rect style="top=460px; left=200px; width=500; height=100">
<v:textbox ><span style="font-size: 50px; font-weight: bold;
color: #333333;">Text Example</span></v:textbox>
</v:rect>
</body>
</html>
```

### 使用可重用绘图库构建动态图形

到目前为止，你已经了解了现代浏览器如何支持基于 XML 的矢量图形，但这