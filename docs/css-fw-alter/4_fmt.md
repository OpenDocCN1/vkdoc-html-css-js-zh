# 四、UIkit 简介

## 安装 UIkit

访问官方网站 [`getuikit.com/`](https://getuikit.com/)。下载按钮位于屏幕右上角，如图 4-1 所示。
![A450102_1_En_4_Fig1_HTML.jpg](img/A450102_1_En_4_Fig1_HTML.jpg)
图 4-1 UIkit 下载页面

点击下载后，会下载一个压缩文件。解压后，文件夹的目录结构如图 4-2 所示。
![A450102_1_En_4_Fig2_HTML.jpg](img/A450102_1_En_4_Fig2_HTML.jpg)
图 4-2 UIkit 框架内容

你也可以通过 NPM 安装包含预构建 JavaScript、CSS 和 Less 源文件的 UIkit，或者克隆仓库以获取所有源文件（包括构建脚本）。要克隆仓库，你需要使用以下命令：
`git clone git://github.com/uikit/uikit.git`

另一种简单的方法是使用 Cloudflare 内容分发网络上的 CDN 文件来包含所有 UIkit 版本的编译文件。你需要在标记中包含所有必要的文件，如代码清单 4-1 所示。

```html
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/uikit/3.0.0-beta.28/css/uikit.min.css" />
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.2.1/jquery.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/uikit/3.0.0-beta.28/js/uikit.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/uikit/3.0.0-beta.28/js/uikit-icons.min.js"></script>
```

代码清单 4-1 在你的网页中包含 UIkit

## 网格、卡片、柔性和宽度

# UIkit 网格系统

UIkit 拥有灵活的网格系统。默认情况下，其网格项是堆叠的。要添加网格，需要在 `<div>` 元素上添加 `uk-grid` 属性。通常，我们使用卡片组件来演示网格功能；卡片元素包含卡片、卡片主体和一个可选的卡片标题。

## 基础卡片组件

清单 4-2 展示了普通卡片组件的代码。

```html
<html>
<head>
    <!-- UIkit CSS -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/uikit/3.0.0-beta.28/css/uikit.min.css" />
    <!-- jQuery is required -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.2.1/jquery.min.js"></script>
    <!-- UIkit JS -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/uikit/3.0.0-beta.28/js/uikit.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/uikit/3.0.0-beta.28/js/uikit-icons.min.js"></script>
</head>
<body style="padding:10px 10px 10px 10px;">
    <div class="uk-card uk-card-default uk-card-body uk-width-1-2">
        <h3 class="uk-card-title">Cloud Computing</h3>
        <p>Cloud Computing is a computing infrastructure and software model for enabling access to shared pools of configurabel resources such as computer networks, servers, and storage including services which can be rapidly provisioned with minimal management effort over the internet or intranet</p>
    </div>
</body>
</html>
```

**清单 4-2** UIkit 的卡片组件

在此代码中，你在 `<head>` 部分包含了 UIkit 框架的链接。你包含了 jQuery 代码、UIkit JavaScript 代码和 UIkit 图标文件的链接。然后，你创建一个 `<body>` 标签并为其分配四周 10px 的内边距。接下来，你创建一个 `<div>` 并分配 `uk-card`、`uk-card-default` 和 `uk-card-body` 类，此外还使用 `uk-width-1-2` 类为父容器分配一半的宽度。`uk-card` 类定义卡片，而 `uk-card-default` 类是卡片的默认样式。`uk-card-body` 类定义卡片的主体。然后，你使用 `<h3>` 元素为卡片标题定义三级标题，并为其添加 `uk-card-title` 类。接下来，你在 `<p>` 标签内定义随机内容。

图 4-3 展示了代码的输出。

![A450102_1_En_4_Fig3_HTML.jpg](img/A450102_1_En_4_Fig3_HTML.jpg)

**图 4-3** UIkit 卡片组件的输出

## 卡片颜色与效果

你可以拥有不同颜色的卡片。让我们看一个如何在 UIkit 中为卡片分配不同颜色的例子；参见清单 4-3。

```html
<div class="uk-child-width-1-3 uk-grid-small" uk-grid>
    <div>
        <div class="uk-card uk-card-default uk-card-body">
            <h3 class="uk-card-title">Default</h3>
            <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.</p>
        </div>
    </div>
    <div>
        <div class="uk-card uk-card-primary uk-card-body">
            <h3 class="uk-card-title">Primary color</h3>
            <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.</p>
        </div>
    </div>
    <div>
        <div class="uk-card uk-card-secondary uk-card-body">
            <h3 class="uk-card-title">Secondary</h3>
            <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.</p>
        </div>
    </div>
</div>
```

**清单 4-3** 为 UIkit 卡片组件添加颜色

在清单 4-3 的代码示例中，你创建了一个父 `<div>` 并为其分配了 `uk-grid` 属性。默认情况下，网格中的所有项目都会堆叠。因此，你为项目分配宽度类以使其彼此相邻对齐。在此示例中，要为父 `<div>` 内的所有项目分配相等的宽度，你需要为其添加一个公共的 `uk-child-width-1-3` 类。这个类的作用是为网格中的所有项目分配父容器三分之一的宽度。你还添加了 `uk-grid-small` 类，这会应用一个小的间距。通常，网格组件带有默认间距，但这个 `uk-grid-small` 类会应用一个小的间距。然后，你创建三个子 `<div>` 并分配 `uk-card`、`uk-card-default` 和 `uk-card-body` 类来创建一个普通卡片。对于第二个项目，你使用 `uk-card-primary` 类代替默认类。对于第三个项目，你使用 `uk-card-secondary` 类代替默认类。你还在所有三个项目的 `<h3>` 标签中使用 `uk-card-title` 类为它们分配相同的标题。

图 4-4 展示了代码的输出。

![A450102_1_En_4_Fig4_HTML.jpg](img/A450102_1_En_4_Fig4_HTML.jpg)

**图 4-4** UIkit 彩色卡片组件的输出

除了默认样式，你可以使用悬停类在卡片上创建悬停效果。你需要做的就是为每个项目的每个子 `<div>` 添加 `uk-card-hover` 类。该行将如清单 4-4 所示，其中你为具有默认卡片颜色的 `<div>` 元素使用了 `uk-card-hover` 类。

```html
<div class="uk-card uk-card-default uk-card-hover uk-card-body">
```

**清单 4-4** 为卡片组件添加悬停效果

同样，你将 `uk-card-hover` 类添加到主要和次要颜色的 `<div>` 项目中。悬停时，你可以看到悬停效果，这在锚点和网页设计的其他方面非常方便。

## 卡片尺寸调整

你可以使用不同的尺寸修饰符来增加卡片的内边距。清单 4-5 包含了展示如何使用较小和较大内边距的代码。

```html
<div class="uk-child-width-1-3@m uk-grid-small" uk-grid>
    <div>
        <div class="uk-card uk-card-default uk-card-small uk-card-hover uk-card-body">
            <h3 class="uk-card-title">Default</h3>
            <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.</p>
        </div>
    </div>
    <div>
        <div class="uk-card uk-card-primary uk-card-large uk-card-hover uk-card-body">
            <h3 class="uk-card-title">Primary color</h3>
            <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.</p>
        </div>
    </div>
    <div>
    </div>
</div>
```

**清单 4-5** 在卡片组件中使用尺寸修饰符

这里你定义了两个内容相同的卡片，但你为默认卡片的子 `<div>` 使用了 `uk-card-small`，而为主要颜色的子 `<div>` 使用了 `uk-card-large` 类。其余代码对于两个子 `<div>` 是相同的。

图 4-5 展示了代码的输出。

![A450102_1_En_4_Fig5_HTML.jpg](img/A450102_1_En_4_Fig5_HTML.jpg)

**图 4-5** 在卡片组件中使用尺寸修饰符的输出

## 宽度类的使用

宽度类与卡片组件结合使用，可以将内容分割成响应式列。现在让我们通过一些例子更好地理解宽度类；参见清单 4-6。

```html
<div class="uk-text-center" uk-grid>
    <div class="uk-width-1-3">
        <div class="uk-card uk-card-primary uk-card-default uk-card-body">One Third</div>
    </div>
    <div class="uk-width-1-3">
        <div class="uk-card uk-card-primary uk-card-default uk-card-body">One Third</div>
    </div>
    <div class="uk-width-1-3">
        <div class="uk-card uk-card-primary uk-card-default uk-card-body">One Third</div>
    </div>
</div>

<div class="uk-text-center" uk-grid>
    <div class="uk-width-1-2">
        <div class="uk-card uk-card-default uk-card-body">Half</div>
    </div>
    <div class="uk-width-1-2">
        <div class="uk-card uk-card-default uk-card-body">Half</div>
    </div>
</div>

<div class="uk-text-center" uk-grid>
    <div class="uk-width-1-4">
        <div class="uk-card uk-card-default uk-card-secondary uk-card-body">One-fourth</div>
    </div>
    <div class="uk-width-3-4">
        <div class="uk-card uk-card-default uk-card-secondary uk-card-body">Three-fourth</div>
    </div>
</div>
```

**清单 4-6** 将宽度类与卡片组件结合使用

在前面的代码示例中，你创建了一个父 `<div>`，并在其中使用 `uk-text-center` 类为所有项目居中文本。然后，你为父 `<div>` 添加了 `uk-grid` 属性。对于每个子 `<div>`，你在父容器（即父 `<div>`）中为每个项目使用三分之一的宽度。你在包含宽度类的每个项目的 `<div>` 内定义了卡片的代码。类似地，你创建了另一个父 `<div>`。这里你使用了相同的编码策略，但使用了两个子项目。你为该父 `<div>` 的每个项目分配了一半的宽度。同样在前面的父 `<div>` 中，你为所有项目使用了主要颜色。这里你使用了卡片的默认颜色。你继续创建另一个带有 `uk-grid` 属性的父 `<div>`，就像前面的父 `<div>` 一样。这里你创建了两个 `<div>`，但你为卡片分配了次要颜色，并使用 `uk-width-1-4` 和 `uk-width-3-4` 类为第一个项目分配四分之一的宽度，为第二个项目分配四分之三的宽度。

图 4-6 展示了代码的输出。

![A450102_1_En_4_Fig6_HTML.jpg](img/A450102_1_En_4_Fig6_HTML.jpg)

**图 4-6** 到目前为止代码的输出

## 使用 `uk-child-width-*` 类

如果不为每个项目分配宽度，而项目大小相同，则可以使用 `uk-child-width-*` 类，其中 `*` 代表每个项目的尺寸。例如，`uk-child-width-1-4` 将为所有项目应用父容器四分之一的宽度。让我们通过一个代码示例来看；参见清单 4-7。

```html
<div class="uk-child-width-1-4 uk-grid-small uk-text-center" uk-grid>
    <div>
        <div class="uk-card uk-card-default uk-card-primary uk-card-body">Cloud</div>
    </div>
    <div>
        <div class="uk-card uk-card-default uk-card-primary uk-card-body">Cloud</div>
    </div>
    <div>
        <div class="uk-card uk-card-default uk-card-primary uk-card-body">Cloud</div>
    </div>
    <div>
        <div class="uk-card uk-card-default uk-card-primary uk-card-body">Cloud</div>
    </div>
    <div>
        <div class="uk-card uk-card-default uk-card-primary uk-card-body">Cloud</div>
    </div>
</div>
```

**清单 4-7** 使用 `uk-child-width-*` 类调整内部组件的宽度

在代码中，你在父 `<div>` 的所有项目中使用了公共的 `uk-child-width-1-4`。然后你定义了包含卡片元素代码的子 `<div>`。

图 4-7 展示了代码的输出。

![A450102_1_En_4_Fig7_HTML.jpg](img/A450102_1_En_4_Fig7_HTML.jpg)

**图 4-7** 使用 `uk-child-width-*` 类调整内部组件的宽度

如图 4-7 所示，带有分数的宽度类如果超出其容器的宽度，将会换行到新行。然而，要将它们均匀地分割在同一行中，你可以使用 `expand` 类，如清单 4-8 所示。

```html
<div class="uk-child-width-expand uk-grid-small uk-text-center" uk-grid>
    <div>
        <div class="uk-card uk-card-default uk-card-primary uk-card-body">Cloud</div>
    </div>
    <div>
        <div class="uk-card uk-card-default uk-card-primary uk-card-body">Cloud</div>
    </div>
    <div>
        <div class="uk-card uk-card-default uk-card-primary uk-card-body">Cloud</div>
    </div>
    <div>
        <div class="uk-card uk-card-default uk-card-primary uk-card-body">Cloud</div>
    </div>
    <div>
        <div class="uk-card uk-card-default uk-card-primary uk-card-body">Cloud</div>
    </div>
</div>
```

**清单 4-8** 实现 `uk-child-width-expand` 以均匀调整内部元素的大小

如你所见，你可以使用 `uk-child-width-expand` 来均匀分割同一行中的所有项目，而不会进入下一行。

图 4-8 展示了代码的输出。

![A450102_1_En_4_Fig8_HTML.jpg](img/A450102_1_En_4_Fig8_HTML.jpg)

**图 4-8** 均匀调整内部元素的大小

你还可以在代码中为某些项目定义自定义宽度，并将它们与 `uk-child-width` 类结合使用。让我们看清单 4-9 中的代码来了解一个例子。

```html
<div class="uk-child-width-expand uk-grid-small uk-text-center" uk-grid>
    <div>
        <div class="uk-card uk-card-default uk-card-primary uk-card-body"> Spread</div>
    </div>
    <div class="uk-width-2-3">
        <div class="uk-card uk-card-default uk-card-primary uk-card-body">Custom </div>
    </div>
    <div>
        <div class="uk-card uk-card-default uk-card-primary uk-card-body">Spread</div>
    </div>
    <div>
        <div class="uk-card uk-card-default uk-card-primary uk-card-body">Spread</div>
    </div>
</div>
```

**清单 4-9** 为内部元素定义自定义宽度

在清单 4-9 中，你使用 `uk-child-width-expand` 作为父 `<div>` 的公共类。然而，在第二个子 `<div>` 中，你使用了 `uk-width-2-3` 的自定义宽度，并让其他子 `<div>` 保持不变。结果是，第二个项目将占据父容器三分之二的宽度，而其他项目将在同一行中均匀分割。

图 4-9 展示了代码的输出。

![A450102_1_En_4_Fig9_HTML.jpg](img/A450102_1_En_4_Fig9_HTML.jpg)

**图 4-9** 为内部元素定义自定义宽度

## FlexBox 组件

你可以在 UIkit 中使用 FlexBox 组件。它帮助你构建交互式网格布局。让我们看一个例子来更好地理解它；清单 4-10 展示了如何使用 FlexBox 的示例。

```html
<div class="uk-flex">
    <div class="uk-card uk-card-default uk-card-primary uk-card-body">First</div>
    <div class="uk-card uk-card-default uk-card-primary uk-card-body uk-margin-left">Second</div>
    <div class="uk-card uk-card-default uk-card-primary uk-card-body uk-margin-left">Third</div>
</div>
```

**清单 4-10** 在 UIkit 中使用 FlexBox

在代码中，你在父 `<div>` 上使用了 `uk-flex` 类。然后，你在子 `<div>` 中定义了三个项目，并为第二和第三个项目使用了 `uk-margin-left` 来在项目之间创建空间。默认情况下，所有项目将与内容和高度一起左对齐，如图 4-10 所示。

![A450102_1_En_4_Fig10_HTML.jpg](img/A450102_1_En_4_Fig10_HTML.jpg)

**图 4-10** 在 UIkit 中使用 FlexBox

你可以使用 `uk-flex-right`、`uk-flex-center` 和 `uk-flex-around` 类将弹性项目右对齐、居中对齐，甚至在项目之间添加相等的空间。清单 4-11 展示了所有项目居中的代码。

```html
<div class="uk-flex uk-flex-center">
    <div class="uk-card uk-card-default uk-card-primary uk-card-body">First</div>
    <div class="uk-card uk-card-default uk-card-primary uk-card-body uk-margin-left">Second</div>
    <div class="uk-card uk-card-default uk-card-primary uk-card-body uk-margin-left">Third</div>
</div>
```

**清单 4-11** 对齐弹性项目

图 4-11 展示了代码的输出。

![A450102_1_En_4_Fig11_HTML.jpg](img/A450102_1_En_4_Fig11_HTML.jpg)

**图 4-11** 对齐弹性项目

## 响应式设计

你可以将响应式类与弹性项目一起使用，其中 `@s` 添加到弹性类会影响设备宽度为 640px 或更高的情况，`@m` 影响设备宽度为 960px 或更高的情况，`@l` 影响设备宽度为 1200px 或更高的情况，`@xl` 影响设备宽度为 1600px 或更高的情况。参见清单 4-12。

```html
<div class="uk-flex uk-flex-left@m uk-flex-center@l">
    <div class="uk-card uk-card-default uk-card-primary uk-card-body">FIRST</div>
    <div class="uk-card uk-card-default uk-card-primary uk-card-body uk-margin-left">SECOND</div>
    <div class="uk-card uk-card-default uk-card-primary uk-card-body uk-margin-left">THIRD</div>
</div>
```

**清单 4-12** 在弹性项目中使用响应式类

在清单 4-12 的代码中，你为父 `<div>` 定义了 `uk-flex-left@m` 和 `uk-flex-center@l`。然后你像前面的例子一样定义了子 `<div>`。在大屏幕上，弹性项目将居中，而在小屏幕上，项目将左对齐。

## 垂直对齐与项目排序

垂直对齐项目在 Flexbox 中是可能的。让我们看清单 4-13 中的例子。你也可以如同一示例中所示定义项目顺序。

```html
<div class="uk-flex uk-flex-column uk-text-center uk-width-1-3">
   <div class="uk-card uk-card-default uk-card-primary uk-card-body uk-flex-last@m uk-flex-first@l uk-margin-top">Cloud 1</div>
   <div class="uk-card uk-card-default uk-card-primary uk-card-body uk-margin-top">Cloud 2</div>
   <div class="uk-card uk-card-default uk-card-primary uk-card-body uk-flex-first@m uk-flex-last@l uk-margin-top">Cloud 3</div>
</div>
```

**清单 4-13** 垂直对齐弹性项目

如你所见，你可以在父 `<div>` 中使用 `uk-flex-column`。因此，输出将如图 4-12 所示。

![A450102_1_En_4_Fig12_HTML.jpg](img/A450102_1_En_4_Fig12_HTML.jpg)

**图 4-12** 垂直对齐弹性项目

然而，在清单 4-13 的同一代码中，你为第一和第三个项目定义了响应式类。你为第一个项目使用了 `uk-flex-last@m` 和 `uk-flex-first@l`。你为第三个项目使用了 `uk-flex-first@m uk-flex-last@l`。因此，虽然前面的输出是针对大屏幕的，但在小和中等屏幕上，第一个项目 Cloud 1 将成为最后一个项目，而第三个项目 Cloud 3 将成为第一个项目，如图 4-13 所示。

![A450102_1_En_4_Fig13_HTML.jpg](img/A450102_1_En_4_Fig13_HTML.jpg)

**图 4-13** 垂直对齐弹性项目

## 嵌套网格

最后，回到网格，你也可以在 UIkit 中使用嵌套网格。让我们看清单 4-14 中的代码来更好地理解它。

```html
<div class="uk-child-width-1-2 uk-text-center" uk-grid>
    <div>
        <div class="uk-card uk-card-default uk-card-primary uk-card-body">FIrst 1-1</div>
    </div>
    <div>
        <div class="uk-child-width-1-2 uk-text-center" uk-grid>
            <div>
                <div class="uk-card uk-card-primary uk-card-body">Nested 2-1 </div>
            </div>
            <div>
                <div class="uk-card uk-card-primary uk-card-body">Nested 2-2</div>
            </div>
        </div>
    </div>
</div>
```

**清单 4-14** 在 UIkit 中使用嵌套网格

这里，你创建了一个父 `<div>` 并为其分配了一半的子宽度。然后，你创建了两个子 `<div>`。你在第一个子 `<div>` 中定义了一个普通卡片。然而，在第二个子 `<div>` 中，你为每个子子 `<div>` 分配了一半的宽度。

图 4-14 展示了代码的输出。

![A450102_1_En_4_Fig14_HTML.jpg](img/A450102_1_En_4_Fig14_HTML.jpg)

**图 4-14** 在 UIkit 中使用嵌套网格

## 匹配元素高度

你可以匹配所有 `<div>` 的高度，而不管所有项目的内容如何。清单 4-15 展示了此功能的代码。

```html
<div class="uk-grid-match uk-child-width-expand@s uk-text-center" uk-grid>
    <div>
        <div class="uk-card uk-card-primary uk-card-body">Lorem ipsum dolor sit amet, consectetur adipisicing elit</div>
     </div>
    <div>
        <div class="uk-card uk-card-primary uk-card-body">Lorem ipsum dolor sit amet, consectetur adipisicing elitLorem ipsum dolor sit amet, consectetur adipisicing elit</div>
    </div>
    <div>
        <div class="uk-card uk-card-primary uk-card-body">Lorem ipsum dolor sit amet, consectetur adipisicing elitLorem ipsum dolor sit amet, consectetur adipisicing elitLorem ipsum dolor sit amet, consectetur adipisicing elitLorem ipsum dolor sit amet, consectetur adipisicing elit</div>
    </div>
</div>
```

**清单 4-15** 匹配元素的高度

如你所见，你在父 `<div>` 中使用了 `uk-grid-match` 类。然后，你像前面的例子一样创建了三个项目。

图 4-15 展示了代码的输出。

![A450102_1_En_4_Fig15_HTML.jpg](img/A450102_1_En_4_Fig15_HTML.jpg)

**图 4-15** 到目前为止代码的输出

## 动画片

UIkit has a plethora of animations that impart a certain degree of immersive behavior to your web sites. Let’s look at the example in Listing 4-16 to understand the behavior of animations in a web page.

```html
<div class="uk-child-width-1-4 uk-grid-match" uk-grid>
    <div class="uk-animation-toggle">
        <div class="uk-card uk-card-primary uk-card-body uk-animation-fade">
            <p class="uk-text-center">Fade</p>
        </div>
    </div>
    <div class="uk-animation-toggle">
        <div class="uk-card uk-card-primary uk-card-body uk-animation-scale-up">
            <p class="uk-text-center">Scale Up</p>
        </div>
    </div>
    <div class="uk-animation-toggle">
        <div class="uk-card uk-card-primary uk-card-body uk-animation-scale-down">
            <p class="uk-text-center">Scale Down</p>
        </div>
    </div>
    <div class="uk-animation-toggle">
        <div class="uk-card uk-card-primary uk-card-body uk-animation-shake">
            <p class="uk-text-center">Shake</p>
        </div>
    </div>
    <div class="uk-animation-toggle">
        <div class="uk-card uk-card-primary uk-card-body uk-animation-slide-left">
            <p class="uk-text-center">Left</p>
        </div>
    </div>
    <div class="uk-animation-toggle">
        <div class="uk-card uk-card-primary uk-card-body uk-animation-slide-top">
            <p class="uk-text-center">Top</p>
        </div>
    </div>
    <div class="uk-animation-toggle">
        <div class="uk-card uk-card-primary uk-card-body uk-animation-slide-bottom">
            <p class="uk-text-center">Bottom</p>
        </div>
    </div>
    <div class="uk-animation-toggle ">
        <div class="uk-card uk-card-primary uk-card-body uk-animation-slide-right">
            <p class="uk-text-center">Right</p>
        </div>
    </div>
</div>
```

Listing 4-16 Adding Animation

In Listing 4-16, you create the parent `<div>` to which you assign the `uk-child-width-1-4` class. This aligns the items in a row with each item taking one-quarter of the space of the parent container. You assign the `uk-grid` attribute as in the previous examples. Then, you proceed to create eight child `<div>`s. To each child `<div>`, you assign the `uk-animation-toggle` class due to which there will be a hover effect that will trigger the animation. Then, you create a card in a sub `<div>` inside those child `<div>`s. Since there are eight items, you assign these eight animation classes to each child `<div>`:

*   `uk-animation-fade` class for the first child. This creates a fade effect for the item.
*   `uk-animation-scale-up` class for the second child. This creates a fade effect where the item scales up.
*   `uk-animation-scale-down` class for the third child. This creates a fade effect where the item scales down.
*   `uk-animation-shake` for the fourth child. This produces a shaking effect where the item appears to vibrate.
*   `uk-animation-slide-left` for the fifth child. The item slides in from the left.
*   `uk-animation-slide-top` for the sixth child. The item slides in from the top.
*   `uk-animation-slide-bottom` for the seventh child. The item slides in from the bottom.
*   `uk-animation-slide-right` for the eighth child. The item slides in from the right.

## 动画效果

图 4-16 展示了代码的输出。
![A450102_1_En_4_Fig16_HTML.jpg](img/A450102_1_En_4_Fig16_HTML.jpg)
图 4-16 添加动画

当鼠标悬停在项目上时，你可以看到代码中定义的动画效果。例如，当鼠标悬停在第一个项目上时，它会淡出。悬停在第五个项目上时，该项目会从左侧滑入。

你还可以定义动画的空间或距离。你只需要添加相应的 `top`、`right`、`bottom` 或 `left` 动画类。例如，`uk-animation-slide-left-small` 会创建一个从较短距离开始的左滑动效果，而 `uk-animation-slide-left-medium` 会创建一个从较长距离开始的左滑动效果。距离已经由固定的像素值预先定义。

让我们看看清单 4-17 中的代码示例，以便更好地理解。

```html
<div class=" uk-child-width-1-4 uk-grid-match" uk-grid>
    <div class="uk-animation-toggle">
        <div class="uk-card uk-card-primary uk-card-body uk-animation-slide-left-small">
            <p class="uk-text-center">Left Small</p>
        </div>
    </div>
    <div class="uk-animation-toggle">
        <div class="uk-card uk-card-primary uk-card-body uk-animation-slide-top-small">
            <p class="uk-text-center">Top Small</p>
        </div>
    </div>
    <div class="uk-animation-toggle">
        <div class="uk-card uk-card-primary uk-card-body uk-animation-slide-bottom-small">
            <p class="uk-text-center">Bottom Small</p>
        </div>
    </div>
    <div class="uk-animation-toggle">
        <div class="uk-card uk-card-primary uk-card-body uk-animation-slide-right-small">
            <p class="uk-text-center">Right Small</p>
        </div>
    </div>
    <div class="uk-animation-toggle">
        <div class="uk-card uk-card-primary uk-card-body uk-animation-slide-left-medium">
            <p class="uk-text-center">Left Medium</p>
        </div>
    </div>
    <div class="uk-animation-toggle ">
        <div class="uk-card uk-card-primary uk-card-body uk-animation-slide-top-medium">
            <p class="uk-text-center">Top Medium</p>
        </div>
    </div>
    <div class="uk-animation-toggle">
        <div class="uk-card uk-card-primary uk-card-body uk-animation-slide-bottom-medium">
            <p class="uk-text-center">Bottom Medium</p>
        </div>
    </div>
    <div class="uk-animation-toggle">
        <div class="uk-card uk-card-primary uk-card-body uk-animation-slide-right-medium">
            <p class="uk-text-center">Right Medium</p>
        </div>
    </div>
</div>
```

清单 4-17 添加带空间和距离的动画

在清单 4-17 中，你遵循了与清单 4-16 相同的代码，唯一的区别是在动画类中添加了距离。你定义了 `uk-animation-slide-left-small`、`uk-animation-slide-top-small`、`uk-animation-slide-bottom-small` 和 `uk-animation-slide-right-small` 类，以创建从左侧、顶部、底部和右侧开始的较短距离的滑动动画效果。类似地，你使用 `uk-animation-slide-left-medium`、`uk-animation-slide-top-medium`、`uk-animation-slide-bottom-medium` 和 `uk-animation-slide-right-medium` 类来定义从左侧、顶部、底部和右侧开始的中等距离的滑动效果动画。

图 4-17 展示了代码的输出。
![A450102_1_En_4_Fig17_HTML.jpg](img/A450102_1_En_4_Fig17_HTML.jpg)
图 4-17 添加带空间和距离的动画

当鼠标悬停在项目上时，它们将显示代码中定义和解释的行为。例如，当鼠标悬停在第六个项目上时，该项目将从顶部滑入，其距离比从顶部滑入的第二个项目更长。

如果你观察到目前为止的动画，它们都是进入动画；然而，要使它们成为退出动画，你可以使用反向功能。你只需要向元素添加一个 `uk-animation-reverse` 类，如清单 4-18 所示。

```html
<div class="uk-child-width-1-2 uk-grid-match" uk-grid>
    <div class="uk-animation-toggle">
        <div class="uk-card uk-card-primary uk-card-body uk-animation-fade uk-animation-reverse">
            <p class="uk-text-center">Fade</p>
        </div>
    </div>
    <div class="uk-animation-toggle">
        <div class="uk-card uk-card-primary uk-card-body uk-animation-slide-right uk-animation-reverse">
            <p class="uk-text-center">Right</p>
        </div>
    </div>
</div>
```

清单 4-18 添加反向动画

在清单 4-18 中，你在父 `<div>` 内创建了两个卡片项目，它们占据了父网格的一半。对于第一个子 `<div>`，你定义了淡入动画类，后跟 `uk-animation-reverse` 类。对于第二个子 `<div>`，你定义了从右侧滑入的动画效果。然而，你也像第一个子 `<div>` 一样，为它定义了 `uk-animation-reverse` 类。

图 4-18 展示了代码的输出。
![A450102_1_En_4_Fig18_HTML.jpg](img/A450102_1_En_4_Fig18_HTML.jpg)
图 4-18 添加反向动画

当你点击第一个项目时，它会以相反的方式淡出（即退出）。类似地，当你点击第二个项目时，它会从左向右滑动（即反向方向）并淡出。

## 带动画的 Scrollspy

Scrollspy helps you trigger events when you scroll your page. It can be used with the animation class extensively to create an awesome effect for your web pages. Let’s understand it by means of a coding example, as shown in Listing 4-19. (Inside the `<p>` tags, there is a load of content. For the entire content, refer to the code bundle.)

## Listing 4-19: Adding Scrollspy

```html
<body style="padding:10px 10px 10px 10px;">
    <p> Lorem ipsum ....(loads of text) </p>
    <p> Lorem ipsum ....(loads of text) </p>
    <div class="uk-child-width-1-2@m uk-grid-match" uk-grid>
        <div>
            <div class="uk-card uk-card-primary uk-card-body" uk-scrollspy="cls: uk-animation-slide-left; repeat: true">
                <h3 class="uk-card-title">Cloud</h3>
                <p>Cloud Computing is the new revolution</p>
            </div>
        </div>
        <div>
            <div class="uk-card uk-card-primary uk-card-body" uk-scrollspy="cls: uk-animation-slide-right; repeat: true">
                <h3 class="uk-card-title">Cloud</h3>
                <p>Cloud Computing is the new revolution</p>
            </div>
        </div>
    </div>
</body>
```

In Listing 4-19, you define two paragraph elements and fill them with loads of text. Then you define a `<div>` class, and to this parent `<div>` you assign a `uk-child-width-1-2@m` class to define the width of the two child `<div>` elements within the grid defined by the `uk-grid-match` and `uk-grid` classes. Then you create two child `<div>`s; you create basically two card items, and you assign the `uk-scrollspy="cls: uk-animation-slide-left; repeat: true"` attribute to the first child `<div>` and `uk-scrollspy="cls: uk-animation-slide-right; repeat: true"` to the second child `<div>`. This will make the cards slide from the left and right, respectively, when you scroll down to that section of the page. You use the `repeat: true` property to repeat the effects when you scroll to that section of the page.

The output of the code will display two paragraphs of content. When you scroll down the page, the cards will slide from the left and right automatically. Figure 4-19 displays the two cards, which slide in from the left and right, respectively, when you scroll to that section.

![A450102_1_En_4_Fig19_HTML.jpg](img/A450102_1_En_4_Fig19_HTML.jpg)

## Figure 4-19: Adding Scrollspy

If you want to create items with the same animation effect, you do not have to define the Scrollspy animation separately for each item. You can club them together by using the `target` property to the class in the scrollspy attribute. Listing 4-20 shows an example of this.

## Listing 4-20: Adding Scrollspy

```html
<p>.. Lorem Ipsum...(loads of text)... </p>
<p> Lorem ipsum (loads of text)... </p>
<div class="uk-child-width-1-3@m" uk-grid uk-scrollspy="cls: uk-animation-slide-top; target: > div > .uk-card; delay: 500; repeat: true">
    <div>
        <div class="uk-card uk-card-primary uk-card-body">
            <h3 class="uk-card-title">Top Animation</h3>
            <p>Animation effect: Slides from the Top</p>
        </div>
    </div>
    <div>
        <div class="uk-card uk-card-primary uk-card-body">
            <h3 class="uk-card-title">Top Animation</h3>
            <p>Animation effect: Slides from the Top</p>
        </div>
    </div>
    <div>
        <div class="uk-card uk-card-primary uk-card-body">
            <h3 class="uk-card-title">Top Animation</h3>
            <p>Animation effect: Slides from the Top</p>
        </div>
    </div>
</div>
```

In Listing 4-20, you define the grid and assign the `uk-child-width-1-3@m` class to it to assign a width of one-third of the parent container width. Then, you apply the following scroll spy property to the parent `<div>`: `uk-scrollspy="cls: uk-animation-slide-top; target: > div > .uk-card; delay: 500; repeat: true"`. This defines the animation for all the child items when you scroll down to that section. Here, you use `cls: uk-animation-slide-top` to create an animation effect wherein the element slides from the top. Then, you follow it up with `target: > div > .uk-card;` in the same line, which will target the card defined by the `<div>` with the `uk-card` class. Thereon, you create a delay of 500ms and use the `repeat` attribute here too. Next, you go on to create three card items.

The output of the code will show text, and on scrolling down below the text, the three card items will slide in from the top, as shown in Figure 4-20.

![A450102_1_En_4_Fig20_HTML.jpg](img/A450102_1_En_4_Fig20_HTML.jpg)

## Figure 4-20: Adding Scrollspy

## 手风琴

## 基本手风琴

手风琴（Accordion）有助于堆叠项目，点击时会显示信息。它有助于减少网页上的信息过载。清单 4-21 展示了 UIkit 中手风琴的一个示例。

```html
<ul uk-accordion>
    <li class="uk-open">
        <h3 class="uk-accordion-title"> Cloud</h3>
        <div class="uk-accordion-content">
            <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nam fermentum justo urna. Nam blandit diam ac erat congue, ullamcorper vulputate odio tempus. Quisque maximus dolor sit amet nisi lacinia euismod. Pellentesque laoreet, tortor malesuada volutpat luctus, augue diam venenatis risus</p>
        </div>
    </li>
    <li>
        <h3 class="uk-accordion-title"> Cloud</h3>
        <div class="uk-accordion-content">
            <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nam fermentum justo urna. Nam blandit diam ac erat congue, ullamcorper vulputate odio tempus. Quisque maximus dolor sit amet nisi lacinia euismod. Pellentesque laoreet, tortor malesuada volutpat luctus, augue diam venenatis risus</p>
        </div>
    </li>
    <li>
        <h3 class="uk-accordion-title"> Cloud</h3>
        <div class="uk-accordion-content">
            <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nam fermentum justo urna. Nam blandit diam ac erat congue, ullamcorper vulputate odio tempus. Quisque maximus dolor sit amet nisi lacinia euismod. Pellentesque laoreet, tortor malesuada volutpat luctus, augue diam venenatis risus</p>
        </div>
    </li>
</ul>
```

**清单 4-21 添加手风琴**

在清单 4-21 中，你使用 `<ul>` 标签创建了一个列表。你需要为它分配 `uk-accordion` 类。这将创建手风琴。然后，你使用 `<li>` 标签创建三个列表项。你为第一个列表项分配 `uk-open` 类，使其默认打开。在第一个项目内，你使用带有 `<h3>` 标签的 `uk-accordion-title` 类定义手风琴的标题。然后，你创建一个 `<div>` 并为其分配 `uk-accordion-content` 类。你在该 `<div>` 内使用段落 `<p>` 标签定义内容；类似地，对于其他两个列表项，你定义了手风琴的内容和标题。但是，请记住不要为这两个列表项使用 `uk-open` 类，因为 `uk-accordion` 类仅用于默认显示内容的手风琴。图 4-21 显示了代码的输出。

![A450102_1_En_4_Fig21_HTML.jpg](img/A450102_1_En_4_Fig21_HTML.jpg)

**图 4-21 添加手风琴**

## 多开手风琴

如果你希望所有手风琴都显示内容而其他手风琴不折叠，那么请在 `uk-accordion` 属性中使用 `multiple: true` 属性。清单 4-22 展示了一个多个手风琴同时打开并显示内容而不折叠的示例。

```html
<ul uk-accordion="multiple: true">
    <li class="uk-open">
        <h3 class="uk-accordion-title"> Cloud</h3>
        <div class="uk-accordion-content">
            <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nam fermentum justo urna. Nam blandit diam ac erat congue, ullamcorper vulputate odio tempus. Quisque maximus dolor sit amet nisi lacinia euismod. Pellentesque laoreet, tortor malesuada volutpat luctus, augue diam venenatis risus</p>
        </div>
    </li>
    <li>
        <h3 class="uk-accordion-title"> Cloud</h3>
        <div class="uk-accordion-content">
            <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nam fermentum justo urna. Nam blandit diam ac erat congue, ullamcorper vulputate odio tempus. Quisque maximus dolor sit amet nisi lacinia euismod. Pellentesque laoreet, tortor malesuada volutpat luctus, augue diam venenatis risus</p>
        </div>
    </li>
    <li>
        <h3 class="uk-accordion-title"> Cloud</h3>
        <div class="uk-accordion-content">
            <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nam fermentum justo urna. Nam blandit diam ac erat congue, ullamcorper vulputate odio tempus. Quisque maximus dolor sit amet nisi lacinia euismod. Pellentesque laoreet, tortor malesuada volutpat luctus, augue diam venenatis risus</p>
        </div>
    </li>
</ul>
```

**清单 4-22 添加多个手风琴**

该示例与前一个手风琴示例相同；不同之处在于，你在主 `<ul>` 标签中使用了 `uk-accordion="multiple: true"` 属性。代码的输出将显示第一个手风琴打开，其余关闭。但是，当你点击其他两个手风琴时，它将显示内容，而不会有任何手风琴折叠，如图 4-22 所示。

![A450102_1_En_4_Fig22_HTML.jpg](img/A450102_1_En_4_Fig22_HTML.jpg)

**图 4-22 添加多个手风琴**

## 核标准情报中心

Finally, let’s talk about icons in UIkit. If you remember the `<head>` section of the code examples, you use the following line of code: `<script src="https://cdnjs.cloudflare.com/ajax/libs/uikit/3.0.0-beta.28/js/uikit-icons.min.js"></script>` This line is for the Scalable Vector Graphic icons, which are baked-in to UIkit. The best part of SVG icons is that they can be colored and styled with CSS to give an aesthetic look. Listing 4-23 shows an example of a few icons used in UIkit.

```html
<a href="" class="uk-margin-small-right" uk-icon="icon: home"></a>
<a href="" class="uk-margin-small-right" uk-icon="icon: trash"></a>
<a href="" class="uk-margin-small-right" uk-icon="icon: users"></a>
<a href="" uk-icon="icon: phone"></a>
<br><br><br><br>
<a href="" class="uk-icon-button uk-margin-small-right" uk-icon="icon: twitter"></a>
<a href="" class="uk-icon-button uk-margin-small-right" uk-icon="icon: facebook"></a>
<a href="" class="uk-icon-button" uk-icon="icon: google-plus"></a>
```

**Listing 4-23** Implementing Icons

In Listing 4-23, you initially create the Home, Trash, Users, and Phone icons. For this, you use the `<a>` anchor tags with the `href` attribute. Then, you create the first icon (i.e., home) by using the property `uk-icon="icon: home"`. As you can see, you assign the `icon: home` value to the `uk-icon` property. Similarly, you assign the `trash` value for the `uk-icon` for the trash using the `uk-icon="icon: trash"` property. You go on to create icons for users and the phone. You also use the `uk-margin-small-right` class for the spacing between the icons.

For social media button icons, you use the additional class `uk-icon-button` with the `<a>` tags. For the first social media icon button (i.e., Twitter), you use the `uk-icon-button` class followed by the `uk-icon="icon: twitter"` property. As you can see, the `icon: twitter` value is assigned to the `uk-icon` attribute. Similarly, you create the Facebook and Google Plus icons.

Figure 4-23 shows the output of the code.

![A450102_1_En_4_Fig23_HTML.jpg](img/A450102_1_En_4_Fig23_HTML.jpg)

**Figure 4-23** Implementing icons

## 摘要

As you learned in this chapter, UIkit is an expansive but light framework compared to heavyweights such as Bootstrap and Foundation. In the next chapter, you will learn about Material Design Lite, which is another amazing and intuitive framework.