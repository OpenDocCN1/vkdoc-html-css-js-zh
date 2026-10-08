# 1. 初学者的路径

在踏上任何一条路径之前，基础工作必须到位。在本开篇章节中，这正是我们要做的——打好基础。我们将介绍 SVG，解释是什么使它成为一种独具优势的图像格式，并展示如何将其与 JavaScript 结合来创建生成式艺术。在此过程中，我们将配置好我们的工具，并建立一个可用于后续草图的模板。

## 为什么选择 JavaScript 和 SvJs？

大多数关于生成式艺术的书籍都使用一种基于 Java 的语言，称为 Processing，或者它的 JavaScript 移植版本 `p5.js`。Processing 是专门为刚接触编程的艺术家和设计师创建的，拥有庞大且活跃的社区。那么，为什么这本书不使用它呢？

我最初接触生成式艺术正是从 Processing 开始的，所以我当然认可它的价值。当 `p5.js` 库在 2013 年首次发布时，我很快就转向了它，这允许直接用 JavaScript（Web 的语言）编写生成式草图。但是，当我想将自己的一些草图集成到现实的 Web 开发项目中时，它的局限性很快就显现出来了。它是一个庞大的库，根据最近一次检查，大小接近一兆字节。虽然这听起来可能不算什么，但按照 Web 开发的标准来看，这已经很多了。

`p5.js` 库构建在 HTML Canvas API 之上，而我很快发现这个 API 实际上使用起来相当直接。直接使用这个 API，我能够实现与 `p5.js` 大体相同的结果，所以它成了我的首选。然而，我所有草图（无论是 `p5.js` 还是 Canvas）的输出仍然是分辨率依赖的位图图形，缺乏任何语义内容。这意味着什么？为什么这很重要（至少对我而言）？让我来解释一下。

### 介绍可缩放矢量图形

早在 Web 的早期时代，当拨号调制解调器占据主导地位，连接互联网绝非瞬间完成时，带宽是极其宝贵的。如果你希望页面能在你有生之年加载完成，文件大小就必须非常小，而位图图像（如 JPG 和 PNG）是主要的带宽瓶颈。

位图图像——也称为栅格图像——由大量数据（或比特）组成。一般来说，如果你想要更大的图像，就需要更多的比特，这意味着更大的文件大小。另一方面，SVG 是一种矢量格式，这是根本性的不同。SVG 可以缩放到任意尺寸，而文件大小不会相应增加，并且它始终极其清晰锐利。这是可能的，因为它不关心将图像绘制到屏幕所需的比特（即像素），而是在更抽象、更具语义的层面上描述要渲染的图像。它描述图像的方式，与 HTML 描述网页结构和内容的方式非常相似。正如 Mozilla 开发者网络所说：

> 本质上，SVG 之于图形，就如同 HTML 之于文本。

SVG 格式不仅是解决实际带宽问题的强大方案；它还是由万维网联盟正式标准化的非专有格式（这在 Web 技术世界中几乎等同于面向未来）。其被广泛采用的条件早在 1999 年就已具备，但不幸的是，浏览器厂商行动迟缓，支持引入得极其缓慢。因此，在很长一段时间里（至少按互联网的时间尺度来看），SVG 默默无闻，不为人知。直到 Flash（Adobe 的专有格式，需要插件才能运行，且经常充斥着安全漏洞）的衰落，以及响应式设计和视网膜（或高 PPI）屏幕的兴起，其可缩放性和锐利度才真正大放异彩，SVG 才终于获得了它应得的支持。

如今，我们可以毫无顾虑地自由使用这种格式，但仅用于制作一些图标和徽标是一回事；挖掘其全部潜力则是另一回事。

### 原生 SVG

由于 SVG 与 HTML 一样是一种声明式语言，它具有极佳的可读性，并且很容易上手。就像 HTML 元素一样，SVG 元素使用开闭尖括号编写，并包含带有值的属性。例如，下面展示了如何创建一个圆形：

```
<circle r="10" cx="20" cy="20" fill="red" />
```

上述示例中的属性 `r`、`cx`、`cy` 和 `fill` 分别指代圆的半径、圆心在 x 轴和 y 轴上的坐标，以及填充颜色。这些属性命名合理，易于理解。

某些 SVG 元素会包含嵌套在其中的其他元素（称为其子节点），因此需要有开闭标签。一个显著的例子是父级 `<svg>` 元素本身，它包含了所有其他 SVG 元素。

为了展示如何手写 SVG，以下是一个以西尔玛·阿夫·克林特（Hilma af Klint）——可以说是西方艺术史上第一位抽象艺术家——风格创作的极简构图的底层标记语言。

```
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100">
  <circle cx="25" cy="25" r="20" fill="pink" />
  <circle cx="75" cy="25" r="20" fill="orange" />
  <circle cx="25" cy="75" r="20" fill="lightblue" />
  <circle cx="75" cy="75" r="20" fill="lightgreen" />
</svg>
```

一个受西尔玛·阿夫·克林特启发的简单仿制品

如你所见，SVG 的编写方式保留了代码的语义。搜索引擎非常喜欢这一点；它们不再面对难以穿透的像素墙，而是能清晰地在标记语言中看到其*意图*。在这个例子中，四个圆按照书写顺序显示，并附带一个标题以增强可访问性。图 1-1 展示了这个标记语言渲染后的效果。

![](img/538894_1_En_1_Fig1_HTML.png)

一幅抽象画，呈现一个深色方形结构，中心有 4 个不同色调的同心圆。

**图 1-1** – 仿西尔玛·阿夫·克林特《天鹅》的简单构图

声明式格式易于阅读，但缺点是编写起来可能很繁琐。如果我们想要 100 个圆而不是四个呢？使用原生 SVG，我们得逐个手写全部 100 个圆。而且，如果我们想让圆显示在随机的 `cx` 和 `cy` 位置上，每个圆使用随机选择的 `fill` 颜色，这根本不可能做到。变量、循环、函数以及所有命令式编程中有趣的东西，我们都无法使用。声明式格式更关注程序*是什么*，而不是*如何做*。然而，*如何做*对我们至关重要。没有*如何做*，就不会有算法，而算法是生成式艺术的核心。

算法就像食谱；它们包含了实现特定结果所需的步骤。如果你在烤蛋糕，你不会只是大喊一声“蛋糕！”就期望它凭空出现，无论你的声明多么具体（“加草莓糖霜和焦糖夹心！”）。这将是声明式方法，在实践中，听起来有点古怪。要烤蛋糕，你需要遵循一系列定义明确的步骤或指令，并准备好原料和烤箱。这类似于编程中的命令式方法；它更亲力亲为，有时事情会变得一团糟。但最终它能给你更多的创意控制权。

### 生成 SVG

那么，如果原生 SVG 不允许使用算法，我们如何用命令式方法编写 SVG 呢？答案是*编写脚本*来生成 SVG，这就是 JavaScript 和 SvJs 发挥作用的地方。

我们可以在没有库的情况下直接用 JavaScript 编写 SVG 脚本，但这也有其挑战，主要在于需要编写冗长的样板代码。SvJs 库省去了我们的麻烦，让编写 SVG 更加直观和有趣。以下是一个用原生 JavaScript 编写的简单 SVG（图 1-2）示例：

```javascript
const svg = document.createElementNS('http://www.w3.org/2000/svg', 'svg');
svg.setAttribute('width', '150px');
svg.setAttribute('height', '150px');

const div = document.getElementById('container');
div.appendChild(svg);

const rect = document.createElementNS('http://www.w3.org/2000/svg', 'rect');
rect.setAttribute('x', '0');
rect.setAttribute('y', '0');
rect.setAttribute('width', '150');
rect.setAttribute('height', '150');
rect.setAttribute('fill', 'cornflowerblue');
svg.appendChild(rect);
```

而以下片段是用 SvJs 编写的等效 SVG（暂时不用关心细节；你只需要注意它与原生 JavaScript 相比有多简洁）。这段代码的输出见图 1-2。

```javascript
const div = document.getElementById('stage');
const svg = new SvJs().set({ width: '150px', height: '150px' }).addTo(div);
svg.create('rect').set({
  x: 0, y: 0, width: 150, height: 150, fill: 'cornflowerblue'
});
```

![](img/538894_1_En_1_Fig2_HTML.png)

一幅抽象画，呈现一个亮色调的矩形。

**图 1-2** – 一个矢车菊蓝色的矩形，尽显风采

现在，你`可以`编写一些函数来减少编写原生 JavaScript 的痛苦，但这会导致你编写一大堆其他实用函数来减轻基础工作的负担。使用 `SvJs`，我尽力为你省去这个麻烦。它本质上是对真实 `SVG` 规范的一个非常轻量的封装，并加入了一些有用的生成式函数。这使其保持极小的体积，同时保持对 `SVG` 规范的忠实。

`SvJs` 的灵感来自 `gySVG` 库，这是一个类似轻量但更通用的 JavaScript 库，附带完整的插件 API 和现代框架支持。我最初打算使用 `gySVG` 并通过插件扩展其功能，但这个插件很快发展到我认为更有意义的是编写一个专注于生成式艺术的自己的库。`SvJs` 就这样诞生了。

### 准备工作

前言说得够多了。让我们开始动手编写代码吧。

#### 代码编辑器

如果你的机器上还没有运行 `VS Code`，请前往 [code.visualstudio.com](https://code.visualstudio.com) 下载并安装适合你操作系统的版本。

如果你无法在机器上安装代码编辑器，或者更愿意在线学习，你可以改用 `CodePen`。前往 [codepen.io](https://codepen.io) 并注册一个账户，如果你还没有注册的话。

本书中的示例将使用 `VS Code` 编辑器并基于本地文件编写；相同的代码可以在线找到 `CodePen` 版本，仅有微小差异（例如我们如何导入 `SvJs` 库，以及缺少 `CodePen` 自动提供的 `HTML` 样板文件）。

#### Node.js 和 NPM

如果你只打算使用 `CodePen`，可以跳过这部分。但如果你有任何进入 Web 开发领域的计划，我仍然建议你熟悉 `Node.js`。

`Node.js` 是一个开源 JavaScript 运行时环境，允许你设置自己的 JavaScript 驱动的服务器。它还允许你通过内置的包管理器 `NPM` 管理依赖项。

前往 [nodejs.org](https://nodejs.org) 下载并安装最新的 `LTS`（长期支持）版本。

#### 初始化并安装 SvJs

创建一个基础文件夹，用于保存与本书相关的所有作品，并将其命名为类似 `generative-svg` 的名称。

打开 `VS Code`，选择 `文件 ➤ 打开文件`，然后导航到该文件夹。接下来，选择 `终端 ➤ 新建终端` 命令，你应该会看到屏幕底部出现一个新窗口，引用了当前文件夹的位置。它应该看起来像这样：

```
your-name@computer-name ~/Documents/generative-svg
```

重要的是路径要指向你在上一步创建的文件夹（即 `generative-svg`）。确认无误后，就可以初始化项目了。为此，在终端中输入以下内容：

```
npm init -y
```

## 运行包管理器并安装 SvJs

此操作将运行 Node 的包管理器 `npm`，而 `init` 命令则用于初始化项目（或称“包”）。`-y` 标志告诉 `npm` 我们接受所有默认设置选项。您会注意到已创建一个名为 `package.json` 的新文件。这是您在 Web 开发中会经常遇到的文件，但目前无需担心其内容。只需知道它的作用是管理我们的依赖项，其中最主要的依赖项就是 SvJs 库。

要安装 SvJs，请运行以下命令：

```bash
npm install svjs
```

这会在 `package.json` 文件中新增一行，引用了您刚刚安装的 SvJs 版本。同时还会创建一个 `package-lock.json` 文件（一个您永远无需查看的文件）和一个 `node_modules` 文件夹，SvJs 及其所有依赖项都存储在此文件夹中（它没有任何依赖项，因此您不会在这里看到其他文件夹）。

## 搭建草图框架

在开始处理 JavaScript 之前，我们的第一个草图需要一些 HTML 和 CSS。我们会尽量精简这些标记和样式，并在全书范围内保持基本一致，因为真正的重点将放在 JavaScript 上。

现在，在项目根文件夹内创建一个名为 `sketches` 的新文件夹，然后在 `sketches` 文件夹内创建一个名为 `00-template` 的新文件夹。我们对文件夹进行编号，以便于排序。`00-template` 文件夹将包含我们从一个草图复制到另一个草图的极简基础内容。在 `00-template` 文件夹内创建两个文件：`index.html` 和 `sketch.js`。完成这些操作后，您应该会得到如下所示的文件夹结构：

```
generative-svg
|-- node_modules
|-- sketches
|-- 00-template
|-- index.html
|-- sketch.js
|-- package-lock.json
|-- package.json
```

打开 `index.html` 文件。值得注意的是，如果您键入 `!` 并按 `tab` 键，VS Code 可以为您生成一些样板标记，但我会在此处完整提供我们的样板代码，以便您复制粘贴。一般来说，我建议您自己动手编写示例代码，并尽可能限制“复制粘贴”操作，因为这种方式学习效果较差。不过，在这里这样做是可以的，因为 HTML 和 CSS 并非我们的重点。

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>SvJs 模板</title>
  <style>
    body {
      margin: 0;
      background-color: #202020;
    }
    #container {
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
    }
  </style>
</head>
<body>
  <div id="container"></div>
  <script type="module" src="sketch.js"></script>
</body>
</html>
```

关于上述代码，有几点值得指出：我们的 `<div id="container">` 将是生成艺术实际发生的地方。可以这么说，它将成为我们的画布。我将其放置在屏幕中央，移除了所有默认边距，并为模板赋予了整体的“深色模式”感觉（这是我个人的偏好）。在 `<div>` 下方，您会看到一个 `<script>` 标签。这是我们引入所有代码的地方，因此后续无需再对 HTML 进行任何修改（可能除了更新每个草图的 `<title>` 标签之外，但我会把这个留给您决定）。脚本设置为 `type="module"`。这使我们能够处理作为模块打包（或导出）的其他文件和库的导入。这是一个值得养成的好习惯，也是我们导入 SvJs 库的方式。

打开 `sketch.js` 文件，并添加以下内容作为前两行代码：

```javascript
// 导入 SvJs 库。
import { SvJs } from '../../node_modules/svjs/src/index.js';
```

现在我们的模板已经完成，可以复制用于后续的草图了。干得好！

## 提供草图服务

如果此时导航到 `00-template` 文件夹并双击 `index.html` 在 Web 浏览器中打开它，您会在开发者控制台（在大多数浏览器上按 `F12` 键可访问）中看到一些错误。这是因为我们需要使用 `http://` 协议而不是 `file://` 协议来提供我们的 HTML 文件（如果您查看地址栏中的完整 URL，就会看到协议前缀）。

要解决这个问题，我们需要安装一个 http 服务器。理想情况下，需要一款能检测文件更改并立即重新加载页面的服务器，这样我们就不必每次都手动刷新浏览器（时间长了会变得很烦人）。有一个简洁的小包叫做 `live-server`，它可以为我们处理这些。要安装它，请在终端中运行以下命令：

```bash
npm install live-server -g
```

`-g` 标志告诉 `npm` 将此包全局安装到我们的机器上，而不是本地安装到当前项目中。因为它更像是一个通用工具，而不是项目特定的依赖项，这正是我们想要的。要运行 `live-server`，您只需在终端中输入 `live-server` 即可。然后它会自动打开一个新的浏览器窗口，您将在其中看到您的项目文件（假设您仍在 VS Code 的项目根文件夹中）。导航到 `sketches` ➤ `00-template`，您应该会看到页面加载成功，并且控制台没有任何错误。不过，页面上也没有任何内容，所以让我们写一些代码来解决这个问题。

## 我们的第一个生成式草图

正如我之前提到的，我们的第一个草图将作为解释我们在第 2 章中要介绍的基本编程概念的基础，因此，出于这个原因，接下来的内容（除了代码本身的一些注释外）解释会比较简略。

复制 `00-template` 文件夹，并将其命名为 `01-our-first-generative-sketch`，确保复制到相同位置（即父文件夹为 `sketches`）。

然后，在您的 `sketch.js` 文件中，在 `import` 语句下方，编写（推荐）或复制以下代码。在输入过程中请阅读注释（以 `//` 开头的行），我有意让注释较为详尽，以便您更好地理解正在发生的事情。

```javascript
// 导入 SvJs 库。
import { SvJs } from '../../node_modules/svjs/src/index.js';

// 创建一些全局变量。
const svgSize = window.innerWidth > window.innerHeight ? window.innerHeight : window.innerWidth;
const bgColor = '#181818';

// 创建一个对象来存储一些随机化参数。
const randomised = {
  hue: random(0, 360),
  rotation: random(-180, 180),
  iterations: random(10, 100)
}

// 创建我们的父级 SVG，并将其附加到 id 为 'container' 的元素上。
const svg = new SvJs();
svg.addTo(document.getElementById('container'));

// 设置 viewBox 的宽度和高度以及 SVG 的显示尺寸。
svg.set({ viewBox: '0 0 1000 1000', width: svgSize, height: svgSize });

// 创建一个背景层——一个覆盖 viewBox 全尺寸的矩形。
const rect = svg.create('rect');
rect.set({ x: 0, y: 0, width: 1000, height: 1000, fill: bgColor });

// 运行一个随机次数的循环来创建我们的椭圆。
for (let i = 0; i < randomised.iterations; i += 1) {
  // 设置椭圆的中心点、x 轴和 y 轴半径以及旋转角度。
  let center = 500;
  let radiusX = 100 + (i * 3);
  let radiusY = 300 + (i * 2);
  let rotation = randomised.rotation + (i * 2);

  // 如果随机色相小于 180，则递增它。否则递减它。
  let hue;
  if (randomised.hue < 180) {
    hue = randomised.hue + (i * 3);
  } else {
    hue = randomised.hue - (i * 3);
  }

  // 创建我们的椭圆。
  let ellipse = svg.create('ellipse');
  ellipse.set({
    cx: center,
    cy: center,
    rx: radiusX,
    ry: radiusY,
    fill: 'none',
    stroke: `hsl(${hue} 80% 80% / 0.6)`,
    transform: `rotate(${rotation} ${center} ${center})`
  });
}

/**
* 获取介于最小值和最大值之间的随机数。
*/
function random(min, max, integer = true) {
  let random = Math.random() * (max - min) + min;
  let number = integer ? Math.floor(random) : random;
  return number;
}
```

好的，代码量相当大！这无疑会让任何编程新手感到不知所措，所以如果您属于这一类并且发现对上述内容感到畏惧，请耐心等待；所有内容将在后续章节中解释。这个第一个生成式草图的目的就是让您快速上手，并向您展示一些快速成果。

当你保存上述代码后，应该会看到类似图 1-3 所示的效果。每次刷新浏览器都会生成一个独特的版本，因此你看到的内容无疑会在某些方面有所不同。但亲爱的读者，这正是生成艺术的魅力所在。

![一幅抽象画，在深色背景下呈现出一个由椭圆簇构成的旋转图案，形成方形结构。](img/538894_1_En_1_Fig3_HTML.png)

**图 1-3** 我们的第一个生成式草图（众多变体之一）

## 总结

回顾一下，我们在第一章中涵盖了以下内容：

- 为何使用 JavaScript 和 `SvJs`，而非 Processing 或 `p5.js`

- SVG 与 PNG 和 JPG 等光栅格式的区别

- 脚本化 SVG 与直接在标记语言中编写有何不同

- `SvJs` 如何让 SVG 脚本编写更直观、更简洁

- 安装与导入 `SvJs` 库，并设置基础模板

- 如何在本地开发服务器上预览我们的草图

下一章：编程入门。
