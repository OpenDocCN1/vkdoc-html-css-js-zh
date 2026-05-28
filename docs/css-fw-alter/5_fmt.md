# 五、材质设计简要说明

## 安装 MDL

MDL can be downloaded in several ways. One of the easiest ways is to go to [`getmdl.io/started/index.html#download`](https://getmdl.io/started/index.html%23download) . You will see a Download MDL button, as shown in Figure 5-1.

![A450102_1_En_5_Fig1_HTML.jpg](img/A450102_1_En_5_Fig1_HTML.jpg)

Figure 5-1Download link for MDL Click the button to download a zipped file containing the various CSS and JavaScript files. Figure 5-2 shows the tree structure of the unzipped components.

![A450102_1_En_5_Fig2_HTML.jpg](img/A450102_1_En_5_Fig2_HTML.jpg)

Figure 5-2Structure of files in MDL The preferred way of including MDL in your document is to use the CDN links for the icons, CSS, and JavaScript files, as shown here:

```
<link rel="stylesheet" href="https://fonts.googleapis.com/icon?family=Material+Icons">
<link rel="stylesheet" href="https://code.getmdl.io/1.3.0/material.indigo-pink.min.css">
<script defer src="https://code.getmdl.io/1.3.0/material.min.js"></script>
```

Just include the three lines of the preceding code in your HTML page and you are good to go. We already defined the advantages of using CDN links in the previous chapters, and the creators of MDL also recommend using the CDN links. Alternatively, you can download and build MDL from the GitHub portal or by using Node or Bower . In this chapter, we will stick to showing the preferred way (in other words, using CDN-hosted files in your markup file) to demonstrate various examples.

## MDL 布局

# MDL 布局组件

在本节中，您将了解 MDL 的一些布局组件，以便理解 MDL 网格以及其他重要属性，例如页脚和标签页。首先，您将通过一些简单的示例了解 MDL 中的头部和抽屉概念。

## 清单 5-1：MDL 中的固定头部与普通抽屉

清单 5-1 展示了 MDL 中的一个固定头部和一个普通抽屉。

```html
<html>
<head>
    <link rel="stylesheet" href="https://fonts.googleapis.com/icon?family=Material+Icons">
    <link rel="stylesheet" href="https://code.getmdl.io/1.3.0/material.indigo-pink.min.css">
    <script defer src="https://code.getmdl.io/1.3.0/material.min.js"></script>
</head>
<body>
    <div class="mdl-layout mdl-js-layout mdl-layout--fixed-header">
        <header class="mdl-layout__header">
            <div class="mdl-layout__header-row">
                <!-- Title -->
                <span class="mdl-layout-title">SUPERMAIL007</span>
                <!-- Add spacer, to align navigation to the right -->
                <div class="mdl-layout-spacer"></div>
                <!-- Navigation -->
                <nav class="mdl-navigation">
                    <a class="mdl-navigation__link" href="" style="color:white">INBOX</a>
                    <a class="mdl-navigation__link" href="" style="color:white">SPAM</a>
                    <a class="mdl-navigation__link" href="" style="color:white">TRASH</a>
                </nav>
            </div>
        </header>
        <div class="mdl-layout__drawer">
            <span class="mdl-layout-title">SUPERMAIL007</span>
            <nav class="mdl-navigation">
                <a class="mdl-navigation__link" href="">INBOX</a>
                <a class="mdl-navigation__link" href="">SPAM</a>
                <a class="mdl-navigation__link" href="">TRASH</a>
            </nav>
        </div>
        <main class="mdl-layout__content">
            <div class="page-content">Come Undone</div>
        </main>
    </div>
</body>
</html>
```

在清单 5-1 中，您可以看到固定头部和普通抽屉的代码。让我们逐行查看代码以了解其工作原理。

在 `<head>` 部分，您包含了 MDL 框架的所有 CDN 文件。然后，您创建了一个 `<body>` 标签，将在其中定义固定头部示例的功能标记。

接着，您编写了一个 `<div>` 并为其分配了 `mdl-layout`、`mdl-js-layout` 和 `mdl-layout--fixed-header` 类。`mdl-layout` 类将容器标识为 MDL 组件，是外部容器元素的一部分。`mdl-js-layout` 类为布局添加 MDL 行为，也是外部容器元素的一部分。`mdl-layout--fixed-header` 类使头部始终可见，即使在小屏幕上也是如此。

然后，您定义了 HTML `<header>` 标签，并为其分配了 `mdl-layout__header` 类。分配的类将容器标识为 MDL 组件。在 `<header>` 标签内，您编写了一个 `<div>` 并为其分配了 `mdl-layout__header-row` 类。`mdl-layout__header-row` 类将容器标识为 MDL 头部行，是头部内容容器所必需的。

在 `<div>` 内部，您创建了一个 `<span>` 标签并为其分配了 `mdl-layout-title` 类，该类标识布局标题文本，是布局标题容器所需的。您使用 `SUPERMAIL007` 作为头部的标题内容。

然后，您编写了另一个 `<div>`，为其分配了 `mdl-layout-spacer` 类，这会填充剩余空间，通常用于将元素右对齐。

接着，您使用 `<nav>` 标签创建导航元素，并为其分配了 `mdl-navigation` 类，该类将容器标识为 MDL 导航组。您使用 `<a>` 标签创建了三个锚点链接，并分配了 `mdl-navigation__link` 类，该类将锚点标识为 MDL 导航链接。然后，您使用单词 `INBOX`、`SPAM` 和 `TRASH` 作为锚点标签的内容。您使用 `</header>` 闭合标签完成了此头部部分。

接下来，您创建了一个 `<div>` 元素并为其分配了 `mdl-layout__drawer` 类，该类将容器标识为 MDL 抽屉。然后，您在该 `<div>` 内创建了一个 `<span>` 元素并为其分配了 `mdl-layout-title` 类，该类标识容器的标题文本。您使用的内容 `SUPERMAIL007` 与 `<header>` 部分中头部标题的内容相同。

然后，您使用 `<nav>` 标签创建导航元素，并为其分配了 `mdl-navigation` 类，该类将容器标识为 MDL 导航组。您使用 `<a>` 标签创建了三个锚点链接，并为其分配了 `mdl-navigation__link` 类，该类将锚点标识为 MDL 导航链接。然后，您使用单词 `INBOX`、`SPAM` 和 `TRASH` 作为锚点标签的内容。

接下来，您创建了 `<main>` 标签来定义布局的主要内容，并为其分配了 `mdl-layout__content` 类。`mdl-layout__content` 类对于将容器定义为 MDL 布局内容是必需的。然后，您使用了闭合标签。

图 5-3 展示了代码的输出。

![A450102_1_En_5_Fig3_HTML.jpg](img/A450102_1_En_5_Fig3_HTML.jpg)

**图 5-3** 固定头部和普通抽屉的输出

如您所见，您创建了一个在较小屏幕上也可见的固定头部。此外，当您点击导航图标（即菜单图标）时，抽屉会滑出，如图 5-4 所示。

![A450102_1_En_5_Fig4_HTML.jpg](img/A450102_1_En_5_Fig4_HTML.jpg)

**图 5-4** 显示内容的抽屉滑出

对于固定抽屉，您只需在定义整个功能标记的第一个 `<div>` 中引入 `mdl-layout--fixed-drawer` 类。该 `<div>` 的代码行如下所示：

```html
<div class="mdl-layout mdl-js-layout mdl-layout--fixed-drawer mdl-layout--fixed-header">
```

图 5-5 展示了代码的输出。

![A450102_1_En_5_Fig5_HTML.jpg](img/A450102_1_En_5_Fig5_HTML.jpg)

**图 5-5** 固定抽屉

假设您想要一个随内容滚动的可滚动头部。在这种情况下，您可以从第一个父级 `<div>` 中移除 `mdl-layout--fixed-header` 类。在 `<header>` 标签中，您需要引入 `mdl-layout__header—scroll` 类。其余代码相同，只是您需要在布局内容 `<main>` 标签中放入大量内容。输出将类似，但当您滚动时，头部将不会固定，而是随内容一起滚动。请参考代码包获取完整代码和输出。

## 清单 5-2：带可滚动标签页的固定头部

在清单 5-2 中，您将查看带可滚动标签页的固定头部的代码。

```html
<div class="mdl-layout mdl-js-layout mdl-layout--fixed-header">
    <header class="mdl-layout__header">
        <!-- Top row, always visible -->
        <div class="mdl-layout__header-row">
            <!-- Title -->
            <span class="mdl-layout-title">SUPERMAIL007</span>
        </div>
        <!-- Tabs -->
        <div class="mdl-layout__tab-bar mdl-js-ripple-effect">
            <a href="#scroll-tab-1" class="mdl-layout__tab is-active">INBOX</a>
            <a href="#scroll-tab-2" class="mdl-layout__tab">SPAM</a>
            <a href="#scroll-tab-3" class="mdl-layout__tab">TRASH</a>
        </div>
    </header>
    <div class="mdl-layout__drawer">
        <span class="mdl-layout-title">SUPERMAIL007</span>
        <nav class="mdl-navigation">
            <a class="mdl-navigation__link" href="">INBOX</a>
            <a class="mdl-navigation__link" href="">SPAM</a>
            <a class="mdl-navigation__link" href="">TRASH</a>
        </nav>
    </div>
    <main class="mdl-layout__content">
        <section class="mdl-layout__tab-panel is-active" id="scroll-tab-1">
            <div class="page-content">Lorem ipsum dolor sit amet... (content)</div>
        </section>
        <section class="mdl-layout__tab-panel" id="scroll-tab-2">
            <div class="page-content">Lorem ipsum dolor sit amet, ...(content)</div>
        </section>
        <section class="mdl-layout__tab-panel" id="scroll-tab-3">
            <div class="page-content">Lorem ipsum dolor sit amet....(content)</div>
        </section>
    </main>
</div>
```

在清单 5-2 中，您定义了父级 `<div>` 并为其分配了 `mdl-layout`、`mdl-js-layout` 和 `mdl-layout--fixed-header` 类。然后，您创建了一个 `<header>` 标签并为其分配了 `mdl-layout__header` 类。接着，在 `<header>` 标签内创建了另一个 `<div>`，并为其分配了 `mdl-layout__header-row` 类。您继续编写了一个 `<span>` 标签并为其分配了 `mdl-layout-title` 类。您将标题内容定义为 `SUPERMAIL007`。

接下来，您创建了标签页。您创建了一个 `<div>` 元素并为其分配了 `mdl-layout__tab-bar` 和 `mdl-js-ripple-effect` 类。`mdl-layout__tab-bar` 类将容器标识为 MDL 标签栏，而 `mdl-js-ripple-effect` 类用于沉浸式波纹效果。然后，您创建了三个锚点链接，分别为其分配了 `#scroll-tab-1`、`#scroll-tab-2` 和 `#scroll-tab-3` href 属性。您还为每个锚点链接分配了 `mdl-layout__tab` 类。在第一个锚点链接中，您分配了 `is-active` 类，因为它应该默认处于活动状态。使用 `</header>` 闭合标签来结束此部分代码。

然后，您继续创建一个类似于清单 5-1 示例中的抽屉。接下来，您在 `<main>` 标签内定义布局内容，并为 `<main>` 标签分配了 `mdl-layout__content` 类。然后，您创建了第一个 `<section>` 标签并为其分配了 `mdl-layout__tab-panel` 类。仅对于此 `<section>` 标签，您引入了 `is-active` 类。然后，您分配了一个 ID，其值是在 `<header>` 标签中创建的第一个锚点标签的 href 属性（即 `scroll-tab-1`）。

接下来，您创建了一个 `<div>` 并为其分配了页面内容类，然后定义了内容。类似地，您创建了另外两个部分，唯一的区别是分配给它们的 ID。您分别为第二和第三部分分配了 `scroll-tab-2` 和 `scroll-tab-3` 作为 ID 的值。

继续前进，您使用必要的闭合标签完成了代码。图 5-6 展示了代码的输出。

![A450102_1_En_5_Fig6_HTML.jpg](img/A450102_1_En_5_Fig6_HTML.jpg)

**图 5-6** 可滚动标签页的输出

（我们在代码中使用了随机的 Lorem Ipsum 内容来演示此示例，有关包含大量内容的完整代码，请参考本章的代码包。）

接下来，您将了解 MDL 中的网格系统。MDL 中的网格系统非常简单，有助于根据不同的屏幕尺寸为多种设备布局内容。默认情况下，MDL 中的网格在桌面屏幕上有 12 列，平板电脑有 8 列，手机尺寸有 4 列，单元格按行顺序排列。正如 MDL 网站上所写，MDL 网格系统有两个例外。

*   如果一个单元格不适合某个屏幕大小的行，它将进入下一行。
*   如果某个单元格的指定列大小等于或大于当前屏幕大小的列数，则它会占据整行。

## Listing 5-3 Grid Layout

Listing 5-3 shows the code sample for a grid layout.

```html
<div class="mdl-grid">
  <div style="text-align:center; border: 1px solid black;" class="mdl-cell mdl-cell--1-col">Cloud</div>
  <div style="text-align:center; border: 1px solid black;" class="mdl-cell mdl-cell--1-col">Cloud</div>
  <div style="text-align:center; border: 1px solid black;" class="mdl-cell mdl-cell--1-col">Cloud</div>
  <div style="text-align:center; border: 1px solid black;" class="mdl-cell mdl-cell--1-col">Cloud</div>
  <div style="text-align:center; border: 1px solid black;" class="mdl-cell mdl-cell--1-col">Cloud</div>
  <div style="text-align:center; border: 1px solid black;" class="mdl-cell mdl-cell--1-col">Cloud</div>
  <div style="text-align:center; border: 1px solid black;" class="mdl-cell mdl-cell--1-col">Cloud</div>
  <div style="text-align:center; border: 1px solid black;" class="mdl-cell mdl-cell--1-col">Cloud</div>
  <div style="text-align:center; border: 1px solid black;" class="mdl-cell mdl-cell--1-col">Cloud</div>
  <div style="text-align:center; border: 1px solid black;" class="mdl-cell mdl-cell--1-col">Cloud</div>
  <div style="text-align:center; border: 1px solid black;" class="mdl-cell mdl-cell--1-col">Cloud</div>
  <div style="text-align:center; border: 1px solid black;" class="mdl-cell mdl-cell--1-col">Cloud</div>
</div>
<div class="mdl-grid">
  <div style="text-align:center; border: 1px solid black;" class="mdl-cell mdl-cell--4-col">Cloud</div>
  <div style="text-align:center; border: 1px solid black;" class="mdl-cell mdl-cell--4-col">Cloud</div>
  <div style="text-align:center; border: 1px solid black;" class="mdl-cell mdl-cell--4-col">Cloud</div>
</div>
<div class="mdl-grid ">
  <div style="text-align:center; border: 1px solid black;" class="mdl-cell mdl-cell--6-col mdl-cell--8-col-tablet">6 (8 tablet)</div>
  <div style="text-align:center; border: 1px solid black;" class="mdl-cell mdl-cell--4-col mdl-cell--6-col-tablet">4 (6 tablet)</div>
  <div style="text-align:center; border: 1px solid black;" class="mdl-cell mdl-cell--2-col mdl-cell--4-col-phone">2 (4 phone)</div>
</div>
```

Listing 5-3 uses the following classes:

*   `mdl-grid`: 将标识为 mdl 网格组件
*   `mdl-cell`: 将标识为 mdl 单元
*   `mdl-cell--1-col`: 将单元格的列大小设置为桌面屏幕上 12 个单元格中的 1 个单元格
*   `mdl-cell--4-col`: 将单元格的列大小设置为桌面屏幕上 12 个单元格中的 4 个
*   `mdl-cell--8-col-tablet`: 在 tablet 屏幕上将单元格的列大小设置为 8 个单元格
*   `mdl-cell--6-col-tablet`: 在 tablet 屏幕上将单元格的列大小设置为 6 个单元格
*   `mdl-cell--4-col-phone`: 在电话屏幕上将单元格的列大小设置为 4 个单元格

If you see the code, you have used the `mdl-grid` class for the first parent `<div>`. You move on to create 12 child `<div>`s for 12 cells, and you use inline CSS styles for the borders of each cell. You assign `mdl-cell` and `mdl-cell--1-col` to each cell in the code, defining a total of 12 cells. Next, you create another parent `<div>`, and similar to the first `<div>`, you assign the `mdl-grid` class to it. Then, you create three child `<div>` cells and assign `mdl-cell` and `mdl-cell--4-col` classes for it. In the next parent `<div>`, you create a grid with three cells and define screen sizes for each child `<div>` cell by customizing the size of each cell on the default desktop, tablet, and phone. Figure 5-7 shows the output of the code.

![A450102_1_En_5_Fig7_HTML.jpg](img/A450102_1_En_5_Fig7_HTML.jpg)

Figure 5-7 Grid system sample in MDL

Now that you have gained insight into some of the components of MDL, let’s look at an example: how to build a web page with MDL.

## 使用 MDL 构建直观的网页

In this section, you will take a look at the process of building a web page for Anirudh Prabhu, co-author of this book. It is a simple example. We will divide it into several steps and then apply the finishing touches to create an aesthetic page.

### 步骤 1:创建部分

Listing 5-4: <head> Section with All the JavaScript and CSS Files

Listing 5-4 shows step 1, wherein you create the `<head>` section of the web page and include all the JavaScript and CSS files.

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Website Using Material Design Lite</title>
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <link rel='stylesheet prefetch' href='https://fonts.googleapis.com/css?family=Roboto:400,100,500,300italic,500italic,700italic,900,300'>
    <link rel="stylesheet" href="https://code.getmdl.io/1.3.0/material.brown-orange.min.css" />
    <link rel='stylesheet prefetch' href='https://fonts.googleapis.com/icon?family=Material+Icons'>
    <link rel="stylesheet" href="style.css">
</head>
<body>
<script src='https://storage.googleapis.com/code.getmdl.io/1.0.6/material.min.js'></script>
<script src='http://cdnjs.cloudflare.com/ajax/libs/jquery/2.1.3/jquery.min.js'></script>
</body>
</html>
```

In Listing 5-4, you include the `viewport` attribute and the MDL files. You also add a custom style sheet, i.e., `style.css`.

## 步骤 2: 创建带抽屉的固定页眉

## Listing 5-5: 定义固定页眉、页眉标题和抽屉

在本节中，你将在 `<body>` 标签之间添加一个代码片段，用于定义固定页眉、页眉标题以及抽屉。参见清单 5-5。

```html
<div class="mdl-layout mdl-js-layout mdl-layout--fixed-header mdl-layout--fixed-tabs">
  <header class="mdl-layout__header">
    <div class="mdl-layout__header-row ">
      <!-- Title -->
      <span class="mdl-layout-title">Anirudh Prabhu</span>
    </div>
    <!-- Tabs -->
    <div class="mdl-layout__tab-bar mdl-js-ripple-effect">
      <a href="#fixed-tab-1" class="mdl-layout__tab is-active">About</a>
      <a href="#fixed-tab-2" class="mdl-layout__tab">Moments</a>
    </div>
  </header>
  <div class="mdl-layout__drawer">
    <span class="mdl-layout-title">Anirudh Prabhu</span>
    <div class="avatar">
      <img src="https://s3-us-west-2.amazonaws.com/s.cdpn.io/234228/cat.jpg" alt="Kaptain Kitty" class="avatar-img">
    </div>
    <!-- /.avatar -->
    <div class="drawer-text">
      Lorem ipsum dolor sit amet, consectetur adipisicing elit. Aspernatur officiis animi, soluta ab deserunt dolore fugit voluptatem laboriosam, magni. Eligendi quia quasi qui cupiditate optio fugit vel, suscipit harum illum.
    </div>
    <!-- /.drawer-text -->
  </div>
  <!-- /.mdl-layout__drawer -->
```

在清单 5-5 中，首先定义了 `<div>` 元素，并为其分配了 `mdl-layout`、`mdl-js-layout`、`mdl-layout--fixed-header` 和 `mdl-layout--fixed-tabs` 类。然后，定义了 `<header>` 标签，并为其分配了 `mdl-layout__header` 类。在 `<header>` 标签内，创建了另一个 `<div>`，并为其分配了 `mdl-layout__header-row` 类，接着创建了一个 `<span>` 元素，使用 `mdl-layout-title` 类定义布局标题。关闭 `<div>` 标签后，为固定选项卡编写另一个 `<div>`，并为其分配 `mdl-layout__tab-bar` 和 `mdl-js-ripple-effect` 类。定义完选项卡名称并关闭 `</header>` 标签后，开始编写抽屉的代码。编写一个 `<div>` 并为其分配 `mdl-layout__drawer` 类。为抽屉创建布局标题，然后编写另一个 `<div>`，并为其分配一个自定义的 `avatar` 类。接着，使用 `<img>` 标签引入一张图片。之后编写另一个 `<div>`，并定义抽屉文本的内容。

接下来，在 `style.css` 文件中定义自定义 CSS 样式，如清单 5-6 所示。

```css
.mdl-layout__drawer-button, .mdl-layout__drawer-button i {
  color: white;
}

@media (max-width: 900px) {
  .mdl-layout__drawer-button {
    width: 100%;
    margin: 0;
    background-color: transparent;
  }
}

img {
  max-width: 100%;
  height: auto;
  display: block;
}

.avatar {
  height: 200px;
  width: 200px;
  margin: 0 auto 2em;
}

.avatar-img {
  height: 200px;
  width: 200px;
  margin: 0 auto;
  border-radius: 50%;
}

.drawer-text {
  padding: 1em;
  text-align: center;
}
```

## Listing 5-6: 定义自定义 CSS 样式

在清单 5-6 中，将抽屉按钮的颜色定义为白色，并为其定义了背景颜色。然后，定义了图片的样式，即最大宽度以及高度和显示属性。定义了 `avatar` 类和 `avatar-img` 类的样式，换句话说，定义了高度、宽度和边距（以及图片的圆角）。最后，使用自定义样式使抽屉文本居中。

### Figure 5-8: 页眉和抽屉的输出

图 5-8 显示了代码的输出。

![A450102_1_En_5_Fig8_HTML.jpg](img/A450102_1_En_5_Fig8_HTML.jpg)

### Figure 5-9: 滑动抽屉内容

如果单击导航图标以显示滑动抽屉，可以看到抽屉图片和内容，如图 5-9 所示。

![A450102_1_En_5_Fig9_HTML.jpg](img/A450102_1_En_5_Fig9_HTML.jpg)

### 步骤 3: 创建“关于”部分

You will now define the content for the About section, as shown in Listing 5-7.

```html
<main class="mdl-layout__content">
    <div class="mdl-layout__tab-panel is-active" id="fixed-tab-1">
        <div class="page-content">
            <!-- Your content goes here -->
            <!-- Hero section -->
            <div class="hero-section">
                <div class="hero-text mdl-typography--text-center">
                    <h1 class="mdl-typography--display-2">I'm Anirudh Prabhu</h1>
                    <p class="mdl-typography--display-1">
                        I'm a passionate mobile photographer
                    </p>
                    <a class="mdl-button mdl-js-button mdl-button--fab mdl-js-ripple-effect mdl-button--accent kitty-hero__text-button" href="#intro">
                        <i class="material-icons">keyboard_arrow_down</i>
                    </a>
                </div>
                <!-- /.hero-text -->
            </div>
            <!-- /.hero-section -->
            <!-- INTRO -->
            <div id="intro" class="mdl-grid intro-section">
                <div class="about-kitty mdl-cell mdl-cell--12-col">
                    <p class="mdl-typography--headline">
                        Welcome to my web page! I wish to display my mobile photography thru this web page.
                    </p>
                </div>
                <!-- /.about-kitty -->
                <div class="about-kitty mdl-cell mdl-cell--12-col">
                    <p>
                        Various mobiles and gadgets with which i have performed photography.
                    </p>
                </div>
                <!-- /.about-kitty -->
                <div class="about-kitty mdl-cell mdl-cell--5-col mdl-cell--1-col-tablet mdl-cell--hide-phone">
                    <div class="circle-container">
                        <div class="circle"></div>
                        <div class="circle"></div>
                        <div class="circle"></div>
                    </div>
                    <!-- /.circle-container -->
                </div>
                <!-- /.about-kitty -->
                <div class="about-kitty mdl-cell mdl-cell--7-col mdl-cell--6-col-tablet mdl-cell--4-col-phone">
                    <div class="topics-container">
                        <div class="topic">Xiaomi MI3</div>
                        <div class="topic">OnePlus 2</div>
                        <div class="topic">Sony DSC QX100</div>
                    </div>
                    <!-- /.topics-container -->
                </div>
            </div>
            <!--/.mdl-grid -->
        </div>
        <!-- /.page-content -->
    </div>
</main>
<!-- /.mdl-layout__content -->
```

**Listing 5-7** About Section Code

In Listing 5-7, you code a `<main>` tag to which you assign the `mdl-layout__content` class. You create a `<div>` and assign the `mdl-layout__tab-panel` and `is-active` classes to it. You also assign an ID of `fixed-tab-1` to it, which is the `href` attribute for the anchor tag for the About section content. Within this, you code another `<div>` to define the page content. You create another section within and assign the custom class `hero-section` to it. Within this, you create another `<div>` to which you assign the custom `hero-text` class along with the `mdl-typography--text-center` class. This centers the text. You then define the content using different typography classes such as `mdl-typography--display-2` and `mdl-typography--display-1`, which decides the font weight of the content. Next, you create a button, a circular one also called the fab button, by defining the `mdl-button`, `mdl-js-button`, `mdl-button--fab`, `mdl-js-ripple-effect`, and `mdl-button--accent` classes to define the look of the button. You used a drop-down MDL arrow and embed it in the button. After the hero section, you define a new parent `<div>` and assign the grid functionality to it. You then define the content for this introduction section. You define the `mdl-cell mdl-cell--12-col` class to it so that the content occupies 12 columns on a desktop. After you jot down the content, you then create three circles by using `mdl-cell`, `mdl-cell--5-col`, `mdl-cell--1-col-tablet`, and `mdl-cell--hide-phone`. This defines the cells based on the screen size such as tablets and phone, especially `mdl-cell--hide-phone`, which hides the content on a small phone. Next, you create the topic container section wherein you define the content that will eventually be placed next to the circles. Now you create custom styles for the section, as shown in Listing 5-8.

```css
.hero-section {
    height: 100vh;
    /* IE11 doesn't like min-height */
    width: 100%;
    margin: 0;
    padding: 0;
    background-color: rgba(121,85,72, 0.6);
    background-image: -webkit-linear-gradient(rgba(121,85,72, 0.3), rgba(121,85,72, 0.3)), url(https://pacdn.500px.org/2185509/e9a80e8a5bb01d46da6830d55a34c6c61146d27d/cover_2048.jpg?2);
    background-image: linear-gradient(rgba(121,85,72, 0.3), rgba(121,85,72, 0.3)), url(https://pacdn.500px.org/2185509/e9a80e8a5bb01d46da6830d55a34c6c61146d27d/cover_2048.jpg?2);
    background-position: center center;
    background-repeat: no-repeat;
    background-size: cover;
    position: relative;
    display: -webkit-box;
    display: -ms-flexbox;
    display: flex;
    -webkit-box-orient: vertical;
    -webkit-box-direction: normal;
        -ms-flex-direction: column;
            flex-direction: column;
    margin: auto;
}

.hero-text {
    color: white;
    margin: auto ;
}

@media screen and (max-width: 580px) {
    .hero-text p {
        white-space: pre-line;
    }
}

.kitty-hero__text-button, .mdl-button--fab.kitty-hero__text-button {
    position: absolute;
    bottom: -28px;
    left: 50%;
    -webkit-transform: translateX(-50%);
            transform: translateX(-50%);
}

/* ABOUT KITTY INTRO + CARDS */
.intro-section, .cards-section {
    max-width: 960px;
}

/* ABOUT KITTY INTRO */
.intro-section, .mdl-grid.intro-section {
    padding: 5em 2em 5em;
}

.about-kitty p {
    max-width: 640px;
    margin: auto ;
}

.circle-container {
    width: 100%;
    min-height: 100px;
    padding: 2em 0;
    display: -webkit-box;
    display: -ms-flexbox;
    display: flex;
    -webkit-box-orient: vertical;
    -webkit-box-direction: normal;
        -ms-flex-direction: column;
            flex-direction: column;
    -webkit-box-align: end;
        -ms-flex-align: end;
            align-items: flex-end;
}

.circle-container .circle {
    height: 16px;
    width: 16px;
    background-color: #c51162;
    border-radius: 50%;
    margin: 0 3px 9px;
}

.topics-container {
    padding: 2em 0;
}

.topics-container .topic {
    font-size: 20px;
    margin: 0 2px 5px;
}

@media screen and (max-width: 480px) {
    .topics-container .topic {
        margin-bottom: 0.5em;
    }
}

.embedded-img {
    max-width: 150px;
    max-height: 150px;
    margin: 0.5em;
    border-radius: 50%;
}
```

**Listing 5-8** Custom Styles

In the custom style sheet, you are essentially defining the background image, height, width, color, and flex characteristics for the hero section. You thereon define the text for the content with the custom `hero-text` class. You then define the maximum width of the `intro-section` followed by assigning the margin and padding for the paragraphs as well as for the element defined with the `about-kitty` class (which incidentally defines the circles and their respective topics). Next, you define the styles for the circle container and the subsequent circles. Moving forward, you define the styles for the topic container and topics along with the media query. Finally, you define the maximum width and height along with the border radius for the embedded image. Figure 5-10 shows the output of the code.

![A450102_1_En_5_Fig10_HTML.jpg](img/A450102_1_En_5_Fig10_HTML.jpg)

**Figure 5-10** Output of the About section

### 步骤 4: 插入包含内容的图像

Now you will develop both the About and Moments tabs, as shown in Listing 5-9.

```html
<!-- Testimonial -->
<div class="mdl-grid mdl-grid--no-spacing fullwidth-panel">
  <div class="mdl-cell mdl-cell--12-col mdl-typography--text-center quote-panel">
    <blockquote>
      <p>
        Taking an image, freezing a moment, reveals how rich reality truly is.
      </p>
      <footer>
        — <cite>Anonymous</cite>
      </footer>
    </blockquote>
  </div>
  <!-- /.mdl-cell -->
</div>
<!--/.mdl-grid -->
</div>
<!-- /.page-content -->
</div>
<!-- /.tab1 -->
```

**Listing 5-9** Code for About and Moments Tabs

In Listing 5-9, you code a `<div>` and assign the `mdl-grid`, `mdl-grid--no-spacing`, and `fullwidth-panel` classes. While the `mdl-grid--no-spacing` class modifies the grid cells to have no margin between them, the `fullwidth-panel` class creates a panel that has a size of the entire grid. Within that `<div>`, you create another `<div>` and allocate a space of 12 columns using the `mdl-cell--12-col` class. You assign the typography class to the content and center the text. You also use the `quote-panel` styling for the content using the `quote-panel` class. You then create a quote using the HTML `<blockquote>` tags.

Moving forward, you assign custom styles for the panel and insert a background image in the custom CSS style sheet, i.e., `style.css`, as shown in Listing 5-10.

```css
/* FULLWIDTH BACKGROUND SECTION */
.fullwidth-panel {
  color: white;
  background-color: rgba(156, 39, 176, 0.6);
}

.fullwidth-panel p {
  max-width: 640px;
  margin: auto;
}

.quote-panel {
  background-image: -webkit-linear-gradient(rgba(63, 81, 181, 0.5), rgba(63, 81, 181, 0.5)), url('https://udemy-images.udemy.com/course/750x422/394968_538b_7.jpg');
  background-image: linear-gradient(rgba(63, 81, 181, 0.5), rgba(63, 81, 181, 0.5)), url('https://udemy-images.udemy.com/course/750x422/394968_538b_7.jpg');
  background-position: center 5%;
  background-repeat: no-repeat;
  background-size: cover;
  padding: 4em 2em 2em;
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  -ms-flex-line-pack: start;
  align-content: flex-start;
}

@media screen and (min-width: 800px) {
  .quote-panel {
    background-position: center 0;
    padding: 6em 2em;
  }
}

@media screen and (min-width: 1200px) {
  .quote-panel {
    background-position: center 8%;
    padding: 10em 2em 8em;
  }
}
```

**Listing 5-10** Assigning Custom Styles

In Listing 5-10, you assign the white color and define the background color to the section containing the `fullwidth-panel` class. You also define the maximum width and set an auto margin to it. Then, for the section pertaining to the `quote-panel` class, you insert a background image and define its position and size along with the padding. Using media queries, you assign the background position and padding for both the 800px and 1200px screen sizes.

Figure 5-11 shows the output of the code.

![A450102_1_En_5_Fig11_HTML.jpg](img/A450102_1_En_5_Fig11_HTML.jpg)

**Figure 5-11** Image with text

### 步骤 5: 为“时刻”选项卡开发内容

## 创建第二个固定标签页内容

接下来，你将为第二个固定标签页（称为 Moments）创建内容，它位于 About 固定标签页旁边，如清单 5-11 所示。

```html
<div class="mdl-layout__tab-panel" id="fixed-tab-2">
  <div class="page-content">
    <!-- Your content goes here -->
    <!-- CARDS  -->
    <div class="mdl-grid cards-section">
      <div class="mdl-cell mdl-cell--6-col mdl-cell--12-col-tablet mdl-card mdl-shadow--2dp home-bringing-card">
        <div class="mdl-card__title">
          <h2 class="mdl-card__title-text">Roses everywhere</h2>
        </div>
        <div class="mdl-card__supporting-text">
          Roses everywhere in flower market
        </div>
      </div>
      <!-- /.mdl-card -->
      <div class="mdl-cell mdl-cell--4-col mdl-cell--4-col-tablet mdl-cell--4-col-phone mdl-card mdl-shadow--2dp play-card">
        <div class="mdl-card__title">
          <h2 class="mdl-card__title-text">Random flower</h2>
        </div>
        <div class="mdl-card__supporting-text">
          Random flower
        </div>
      </div>
      <div class="mdl-cell mdl-cell--6-col mdl-cell--8-col-tablet mdl-cell--4-col-phone mdl-card mdl-shadow--2dp litter-card">
        <div class="mdl-card__title">
          <h2 class="mdl-card__title-text">Lilac</h2>
        </div>
        <div class="mdl-card__supporting-text">
          Lilacs are a beloved, fragrant shrub that produce clusters of light-purple flowers.
        </div>
      </div>
      <!--/.mdl-card -->
      <div class="mdl-cell mdl-cell--6-col mdl-cell--8-col-tablet mdl-cell--4-col-phone mdl-card mdl-shadow--2dp diet-card">
        <div class="mdl-card__title">
          <h2 class="mdl-card__title-text">Beautiful sunset at aguada beach</h2>
        </div>
        <div class="mdl-card__supporting-text">
          Beautiful sunset at aguada beach in Goa
        </div>
      </div>
      <!--/.mdl-card -->
      <!--/.mdl-card -->
    </div>
    <!--/.mdl-grid -->
  </div>
  <!-- /.page-content -->
</div>
```

### 清单 5-11：第二个固定标签页的代码

在清单 5-11 中，你定义了第二个固定标签页（称为 Moments）的内容。首先，你编写了一个 `<div>` 并为其分配了 `grid` 类。然后，你编写了四张卡片的代码。对于第一张卡片，你使用了 `mdl-card` 类，并使用 `mdl-cell`、`mdl-cell--6-col` 和 `mdl-cell--12-col-tablet` 类为其分配了桌面端 6 列和平板端 12 列的空间。接着，你使用 `mdl-shadow--2dp` 类定义了阴影以增加美观性。之后，你编写了一个 `<div>` 并使用 `mdl-card__title` 类为卡片分配了标题。然后，你使用 `mdl-card__title-text` 类定义了标题文本。接着，你使用 `mdl-card__supporting-text` 类为标题分配了支持内容。类似地，你使用不同的内容名称创建了另外三张卡片。

创建完卡片后，你在自定义 `style.css` 样式表中定义了自定义样式，如清单 5-12 所示。

```css
/* CARDS SECTION */
.cards-section {
  padding: 5em 0;
}

.mdl-card__title {
  min-height: 300px;
}

.mdl-card__title > .mdl-card__title-text {
  color: white;
}

.home-bringing-card .mdl-card__title {
  background: -webkit-linear-gradient(rgba(0, 0, 0, 0.1), rgba(0, 0, 0, 0.8)), url('https://drscdn.500px.org/photo/210599845/q%3D80_h%3D300/v2?webp=true&sig=94f8683780d7d009224f477342bf4c34740920b5b75576cb8793ff52e7229b1a') center / cover;
  background: linear-gradient(rgba(0, 0, 0, 0.1), rgba(0, 0, 0, 0.8)), url('https://drscdn.500px.org/photo/210599845/q%3D80_h%3D300/v2?webp=true&sig=94f8683780d7d009224f477342bf4c34740920b5b75576cb8793ff52e7229b1a') center / cover;
}

.play-card .mdl-card__title {
  background: url('https://drscdn.500px.org/photo/225478901/q%3D80_h%3D450/v2?webp=true&sig=ddd21866e9502c5f56aef387adf4cc0553513de4582ed30a5bc57ba817f43b06') center / cover;
}

.image-card {
  background: url('https://s3-us-west-2.amazonaws.com/s.cdpn.io/234228/image-card.jpg') center / cover;
}

.image-card > .mdl-card__actions {
  height: 52px;
  padding: 16px;
  background: rgba(0, 0, 0, 0.6);
}

.image-card__title {
  color: #fff;
  font-size: 14px;
  font-weight: 500;
}

.litter-card .mdl-card__title {
  background: -webkit-linear-gradient(rgba(0, 0, 0, 0.1), rgba(0, 0, 0, 0.8)), url('https://drscdn.500px.org/photo/187345183/q%3D80_h%3D450/v2?webp=true&sig=883a5a5734775d4b4084bd4f5fe7cd7ac9728bf0b6fc5d4ee91a522444023e6e') center / cover;
  background: linear-gradient(rgba(0, 0, 0, 0.1), rgba(0, 0, 0, 0.8)), url('https://drscdn.500px.org/photo/187345183/q%3D80_h%3D450/v2?webp=true&sig=883a5a5734775d4b4084bd4f5fe7cd7ac9728bf0b6fc5d4ee91a522444023e6e') center / cover;
}

.diet-card .mdl-card__title {
  background: -webkit-linear-gradient(rgba(0, 0, 0, 0.1), rgba(0, 0, 0, 0.8)), url('https://drscdn.500px.org/photo/109883725/q%3D80_h%3D450/v2?webp=true&sig=29611a8077b1b73ce190f28e138ed7147973317e15ba8c9ed418a4f797683df8') center / cover;
  background: linear-gradient(rgba(0, 0, 0, 0.1), rgba(0, 0, 0, 0.8)), url('https://drscdn.500px.org/photo/109883725/q%3D80_h%3D450/v2?webp=true&sig=29611a8077b1b73ce190f28e138ed7147973317e15ba8c9ed418a4f797683df8') center / cover;
}

.card-small {
  min-height: auto;
}

.card-small > .mdl-card__title {
  color: rgba(0, 0, 0, 0.87);
  height: auto;
  min-height: auto;
}

.card-small .mdl-card__title-text {
  font-size: 16px;
}

.card-small .mdl-card__title-text:before {
  content: "";
  display: inline-block;
  margin-right: 0.5em;
  width: 18px;
  height: 18px;
  background-color: #c51162;
  border-radius: 50%;
}
```

### 清单 5-12：在自定义 `style.css` 样式表中定义自定义样式

在清单 5-12 中，你为包含 `cards-section` 类的部分分配了内边距。然后，你为卡片标题部分定义了最小高度和白色。接着，你定义了自定义样式，并为四张卡片中的每一张插入了背景图片。之后，你定义了小屏幕设备上卡片的最小高度，此外还定义了小屏幕设备上卡片以及包含卡片标题部分的颜色、高度和最小高度。你还定义了小屏幕设备上卡片以及标题文本的字体大小、边距、高度、背景颜色和边框半径。

图 5-12 显示了代码的输出。

![A450102_1_En_5_Fig12_HTML.jpg](img/A450102_1_En_5_Fig12_HTML.jpg)

### 图 5-12：Moments 标签页内容

### 步骤 6:设计页脚部分

Finally, you will design a form and the footer section, as shown in Listing 5-13.

```html
<!-- Contact -->
<div class="mdl-grid mdl-grid--no-spacing">
  <!--/.contact-intro  -->
  <div class="mdl-cell mdl-cell--6-col mdl-cell--8-col-tablet mdl-cell--4-col-phone contact-panel form-panel mdl-color--brown-50">
    <form action="#">
      <div class="mdl-textfield mdl-js-textfield mdl-textfield--floating-label">
        <input class="mdl-textfield__input" type="text" id="name">
        <label class="mdl-textfield__label" for="name">Your name</label>
      </div>
      <div class="mdl-textfield mdl-js-textfield mdl-textfield--floating-label">
        <input class="mdl-textfield__input" type="email" id="email">
        <label class="mdl-textfield__label" for="email">Your email</label>
      </div>
      <div class="button-container clearfix">
        <button class="mdl-button mdl-js-button mdl-button--raised mdl-js-ripple-effect mdl-button--accent subscribe-button">
          Join my fans
        </button>
      </div>
      <!--/.button-container -->
    </form>
  </div>
  <!--/.contact-panel -->
  <div class="mdl-cell mdl-cell--6-col mdl-cell--8-col-tablet mdl-cell--4-col-phone contact-panel address-panel mdl-typography--text-center mdl-color--brown-100">
    <p class="mdl-typography--title-color-contrast mdl-typography--text-nowrap mdl-typography--font-thin">
      <i class="material-icons">email</i> <a href="mailto:info@amp.com">info@amp.com</a>
    </p>
    <p class="mdl-typography--title-color-contrast mdl-typography--text-nowrap mdl-typography--font-thin">
      <a class="mdl-button mdl-js-button mdl-button--raised mdl-js-ripple-effect" href="twitter.com">twitter</a>
      <a class="mdl-button mdl-js-button mdl-button--raised mdl-js-ripple-effect" href="plus.google.com">Google+</a>
      <a class="mdl-button mdl-js-button mdl-button--raised mdl-js-ripple-effect" href="facebook.com">Facebook</a>
    </p>
  </div>
  <!-- /.contact-panel -->
</div>
<!-- /.mdl-grid -->
<!-- FOOTER -->
<footer class="mdl-mini-footer mdl-color--brown-200">
  <div class="mdl-mini-footer__left-section">
    <ul class="mdl-mini-footer__link-list">
      <li><a href="#">Help</a></li>
      <li><a href="#">Privacy & Terms</a></li>
    </ul>
  </div>
  <!-- /.mdl-mini-footer__left-section -->
</footer>
```

## Listing 5-13: Form and the Footer Section

In Listing 5-13, you code a `<div>` and assign the `mdl-grid` class along with the `mdl-grid--no-spacing` class. You then define the space occupied by the grid columns on a tablet, phone, and desktop screen sizes. You assign custom classes to it so that you can use custom styles in the `style.css` sheet. You also assign the brown shade to this section using the `mdl-color--brown-50` class.

Moving forward, you define the form within the `<form>` tags. Inside the `<form>` tags, you create a `<div>` and assign `mdl-textfield`, `mdl-js-textfield`, and `mdl-textfield--floating-label` to design the text fields and use the MDL behavior for those fields. You create the Name and Email fields by adding the `mdl-textfield__input` to the input tag and `mdl-textfield__label` to the `<label>` tags apart from defining the type of the text fields.

You then create a button. First, you create a container for the button by using the `button-container` class. Then, you define another `<div>` element within that `<div>` and assign the `mdl-button`, `mdl-js-button`, `mdl-button--raised`, `mdl-js-ripple-effect`, and `mdl-button--accent` classes. This creates the button, assigns the MDL behavior, and creates the required effects and color to the button.

Then, you create a form. Once you are done with the form, you create the contact panel section to the right side of the form. You define another `<div>` and assign the column space for the cells depending on the screen size. You then define the typography, color contrast, and font using the `mdl-typography--title-color-contrast`, `mdl-typography--text-nowrap`, and `mdl-typography--font-thin` classes. Thereon, you define the e-mail icon using the `material-icons` class to the enclosed `<i>` tags. Then, you create another paragraph tags and create three anchor tags and define the button classes for the Twitter, Google+, and Facebook buttons using the `mdl-button`, `mdl-js-button`, `mdl-button--raised`, and `mdl-js-ripple-effect` classes.

Finally, you define the footer by using the `<footer>` tags to which you assign the `mdl-mini-footer` and `mdl-color--brown-200` classes for the footer type due to which it will inherit the footer type and brown color. Within this section, you define the position of the footer using the `mdl-mini-footer__left-section`, which will align it to the left. You then define the link list using the `<ul>` and `<li>` tags.

Moving on, you define the custom styles for the preceding code, as shown in Listing 5-14.

```css
.contact-intro {
  color: rgba(255, 255, 255, 0.87);
}
.contact-panel {
  padding: 6em 4em;
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  -webkit-box-orient: vertical;
  -webkit-box-direction: normal;
      -ms-flex-direction: column;
          flex-direction: column;
  margin: auto;
}
.mdl-textfield {
  display: block;
  width: 100%;
  padding: 20px 0;
}
@media screen and (min-width: 800px) {
  .subscribe-button {
    float: right;
  }
}
.address-panel {
  background-color: #dbdef1;
  color: rgba(255, 255, 255, 0.87);
}
.address-panel .material-icons {
  position: relative;
  top: 0.2em;
  display: inline-block;
  height: 30px;
  width: 30px;
  line-height: 30px;
  background-color: #ff4081;
  padding: 0.5em;
  border-radius: 50%;
}
.mdl-mini-footer, .mdl-mini-footer .mdl-logo, .mdl-mini-footer--link-list a, .mdl-mini-footer__link-list a {
  color: rgba(0, 0, 0, 0.54);
}
ul {
  list-style-type: none;
}
/* UTILITIES */
.clearfix:after {
  content: "";
  display: table;
  clear: both;
}
.float-right {
  float: right;
}
.float-left {
  float: left;
}
```

## Listing 5-14: Custom Styles for Listing 5-13

In Listing 5-14, you define the color for the contact and assign the padding and display properties for the `contact-panel` section. You also define the display type, width, and padding for the text fields in the form. You shift the submit button of the form to the right of the form section. For the same button, you use the `clearfix` and `float` properties to automatically clear the child elements without using any additional markup.

You move on to define the background color of the panel along with the panel color. You also define the position, height, inline block display, padding, and border radius for the material icons. You set the color to the logo and link list of the footer. You remove the listing bullets from the list using the `list-style-type: none;` property.

## Figure 5-13: Complete web page with the footer section

![A450102_1_En_5_Fig13_HTML.jpg](img/A450102_1_En_5_Fig13_HTML.jpg)

## 摘要

In this chapter, you learned about the subtle nuances of the intuitive `MDL` framework. It is a lightweight framework and can help you design an interactive web site with ease. It is quite resourceful and is a vital cog in the wheel for small projects compared to heavyweight frameworks because it helps you build remarkable, immersive web pages. In the next chapter, you will look at the last light framework/utility covered in this book, called `Susy`.