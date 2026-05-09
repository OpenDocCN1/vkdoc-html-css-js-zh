# 附录 更多绘图技巧

| **代码语句** | **解释** |
|--------------------|-----------------|
| `<!DOCTYPE html>` | DOCTYPE 注释。 |
| `<html>` | html 开始标签。 |
| `<head>` | head 开始标签。 |
| `<title>Little guy Joe</title>` | 完整的标题。 |
| `<script>` | script 开始标签。 |
| `var svgNS = "http://www.w3.org/2000/svg";` | 设置用于保存命名空间引用。 |
| `var headx = 0;` | 用于移动头部（以及脸部和文本）；保存头部 x 坐标。 |
| `var heady = 0;` | 保存头部 y 坐标。 |
| `var cheadx = 60;` | 用于定位头发。头部的水平中心。 |
| `var cheady = 100 + 20;` | 头部的垂直中心。 |
| `var rx = 50;` | 用于绘制眼睛。这是一个椭圆，因此必须给出水平和垂直直径。它是一个圆。 |
| `var ry = 50;` | …垂直直径。 |
| `var hairy = cheady - 2.2 * ry;` | 单根头发的位置。 |
| `var scaleFactor = 1;` | 初始缩放因子。 |
| `var fillColor = "tan";` | 肤色。 |
| `var strokeColor = "none";` | 构成花生的圆没有描边。 |
| `var myUpper;` | 接下来的变量将保存对面部各部分的引用，这里是上半部分。 |
| `var myMiddle;` | 中间部分。 |
| `var myLower;` | 下半部分。 |
| `var myHead;` | 对头部的引用。 |
| `var myNose;` | 对鼻子的引用。 |
| `var myLeftEye;` | 对左眼的引用。 |

*（续）*

