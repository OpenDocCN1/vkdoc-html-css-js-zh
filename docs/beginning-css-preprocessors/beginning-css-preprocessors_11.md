# 排版后的内容

参考代码清单 1-6 中的示例。现代浏览器支持 `RGBA`（红绿蓝透明度）和 `HSLA`（色相饱和度亮度透明度）颜色值。不过，你需要为旧版浏览器提供降级方案。常见的做法是先声明一个十六进制颜色值，再声明 `RGBA` 或 `HSLA` 颜色值。

***代码清单 1-6***。使用 CSS 在所有浏览器中支持新的颜色格式

```
.container {
  background-color: #000;
  background-color: rgba(0,0,0, 0.9);
}
```

使用预处理器可以轻松完成这项工作。如果使用 `Sass` 预编译器完成同样的任务，则可按照代码清单 1-7 所示进行操作。

***代码清单 1-7***。代码清单 1-6 的 `Sass` 版本

```
$black:#000;
.container {
        background-color: $black;
        background-color: rgba($black, 0.9);
}
```

预处理器会为你完成所有与 `RGBA` 值相关的计算。

随着现代浏览器对 `CSS3` 的支持，人们不再大量使用图片，转而采用纯 `CSS`。然而，每种浏览器对这些功能的实现方式各有不同，因此需要添加浏览器厂商前缀并使用不同的语法。你可以在代码清单 1-8 中看到一个简单的示例。

***代码清单 1-8***。针对不同浏览器的 `CSS` 特性实现

```
.container {
        -webkit-border-radius: 10px;
        -moz-border-radius: 10px;
        -ms-border-radius: 10px;
        -o-border-radius: 10px;
        border-radius: 10px;
}
```

预处理器通过允许你将这段代码声明为一个可重用的组件（称为 *mixin*，将在后续章节中介绍），然后直接在代码中使用它，从而让这项工作变得更加轻松。

此示例假设这个可重用的 mixin 名为 `rounded-corners`。在 `Sass` 预处理器中，相应的代码如代码清单 1-9 所示。

***代码清单 1-9***。使用 `Sass` 预处理器实现代码清单 1-8 的代码

```
.container {
  @include rounded-corners(4px);
}
```

到目前为止，你已经了解了预处理器如何帮助编写 `CSS` 代码。现在，我们来考虑一下预处理器如何在后编码阶段提供帮助。压缩 `CSS` 可以减小其原始大小，从而缩短加载时间。这可以通过在线工具或编辑器插件来实现。

然而，预处理器可以通过多种方式编译成 `CSS`，其中一种方式就是压缩。预处理器可以配置为：每当保存预处理器文件时，自动编译为 `CSS` 并压缩。这使得它可以直接用于生产环境。

## 关于 CSS 预处理器的常见误解

本节将讨论一些常见的误解，这些误解阻碍了开发者在日常工作中采用预处理器。

### 你必须成为命令行专家

许多开发者害怕使用命令行。使用预处理器几乎不需要多少命令行知识。事实上，你只需要学习一条命令。此外，还有一些应用程序可以在不接触命令行的前提下完成同样的任务。

### 你必须改变编写 CSS 的方式

开发者有自己编写和组织样式表的方式。由于 `SCSS` 语法与 `CSS` 相同，开发者无需改变他们一直以来编写 `CSS` 的方式。

### 为什么不直接编写 CSS？

许多开发者初次接触 `CSS` 预处理器时的反应是：“直接编写 `CSS` 有什么问题？我们日常都在用 `CSS`，可以解决所有常见的布局问题，并开发出在所有设备上都能正常运行的响应式网站。”

其实，预处理器本身并不能生成更好的 `CSS`。例如，如果你缺乏如何正确使用 `CSS` 的知识，那么预处理器也帮不上什么忙。预处理器通过遵循 **DRY**（不要重复自己）原则，帮助你以更快速、更易于维护的方式编写 `CSS`。




## 已知的预处理器有哪些？

让我们来看一下当今一些已知的预处理器，如图 1-2 所示。

![9781484213483_Fig01-02.jpg](img/9781484213483_Fig01-02.jpg)

图 1-2。已知的预处理框架

### Sass

`Sass`（Syntactically Awesome Stylesheets，语法上令人惊艳的样式表）是最著名的预处理器，已经存在了八年之久。`Sass` 是一种预处理器语言，最初由 Hampton Catlin 和 Natalie Weizenbaum 设计和开发。

当 `SassScript` 被编译时，它会根据 `Sass` 文件中指定的规则，为各个选择器生成相应的 CSS 规则。`Sass` 可以监视 `.sass` 或 `.scss` 文件，并在这些文件被保存时自动生成输出的 `.css` 文件。

`Sass` 是开源的，并使用 Ruby 编写。

### Less

与 `Sass` 类似，`Less` 是另一种流行的 CSS 预处理器。它通过提供混入（mixins）、变量和嵌套样式规则来增强默认的 CSS 语法。它很容易设置。你也可以在 HTML 文档中引用它的 JavaScript 文件并开始使用。

`Less` 因其简洁性吸引了非常庞大的用户群。`Less` 是开源的。其初始版本是用 Ruby 编写的，但后续版本是用 JavaScript 编写的。有效的 CSS 代码也是有效的 `Less` 代码，因为 `Less` 遵循相同的语义。

`Less` 与其他 CSS 预处理器的主要区别在于，`Less` 允许通过 `less.js` 在浏览器中进行实时编译。

### Compass

`Compass` 是一个开源的 CSS 编写框架。`Compass` 基于 `Sass`，因此可以使用 `Sass` 的所有特性。它非常流行，并且正处于活跃的开发阶段。使用 `Compass`，开发者可以编写更简洁的标记，而无需使用表示性的类。

`Compass` 包含了大量开发者最常使用的可复用模式。

与 `Compass` 齐名的是 `Bourbon`，它由 Thoughtbot 公司使用 `Sass` 为 `Sass` 构建。`Bourbon` 是一个为 `Sass` 提供的简单轻量级混入库。

接下来讨论 `Compass` 提供的一些关键特性。

### 混入（Mixins）

`Compass` 和 `Bourbon` 都为各种 CSS3 特性提供了大量的混入集合，你将在后续章节中详细了解。这意味着开发者无需担心浏览器厂商前缀或 CSS 技巧。清单 1-10 演示了关于盒子大小调整的混入使用。

***清单 1-10***。展示使用 Compass 提供的混入的示例

```
el{
        @include box-sizing(border-box);
}
```

### 排版

`Compass` 和 `Bourbon` 都包含与排版相关的混入、变量和函数。`Compass` 带有大量变量和几个混入。

在这一领域，`Compass` 的一个特性是它还能使用带有 `px` 回退方案的 `rem` 单位。

***清单 1-11***。展示使用 Compass 排版功能的示例

```
el{
        @include adjust-font-size-to(42px);
}
```

### 辅助工具

`Compass` 提供的一个功能是辅助工具。辅助工具可以节省时间，因为它们是预定义的 CSS 代码片段，你可以直接在样式表中使用。

例如，`Compass` 提供了一个用于清除浮动的辅助工具、一个重置工具（带有多种选项）、一些图像替换技术等。

`Bourbon` 将这些辅助工具称为附加组件，但其数量比 `Compass` 少。

### 精灵图（Sprites）

由于 `Compass` 部分是用 Ruby 构建的，因此它可以与文件系统交互。它能够做到的事情之一就是根据一个图像文件夹构建精灵图。

***清单 1-12***。展示 Compass 构建精灵图的简单方法的示例

```
@import "icon/*.png";
@include all-icon-sprites;
```

它还提供了 `image-width()`、`image-height()` 和 `inline-image()` 等函数，用于将图像文件编码为 Base64 格式。

## 预处理器的特性

本节将介绍预处理器的一些常见特性，与直接使用 CSS 相比，这些特性使它们成为开发者出色的工具。你将看到一些上下文示例。这些示例使用了两个著名的预处理器——`Sass` 和 `Less`——你将在后续章节中详细学习它们。

### 变量



### 变量

变量帮助你存储在样式表中需要复用的信息。像颜色、字体样式或任何 CSS 样式都可以被存储和复用。例如，在 Sass 中，使用`$`符号来声明一个变量。

***列表 1-13***：使用 SASS 实现变量的示例

```
$text-color:#333;
body {
        color:$text-color;
}
```

当这段代码被编译时，它会获取为`$text-color`定义的变量，并输出普通的 CSS，其中变量的值会被填入 CSS 中。这在处理适用于整个站点的属性并保持其一致时非常有用。

***列表 1-14***：上述代码片段的输出

```
body {
        color:#333;
}
```

类似地，在 Less CSS 中，使用`@`符号来声明一个变量。

***列表 1-15***：使用 Less 实现变量的示例

```
@text-color:#333;
body{
        color:$text-color;
}
```

### 嵌套

页面在 CSS 中的视觉层次不如在 HTML 中那么清晰。预处理器允许你像在 HTML 中那样嵌套 CSS 选择器。

***列表 1-16***：在 Sass 或 Less 中的嵌套示例

```
nav {
  ul {
    margin: 0 auto;
    padding: 10px 15px;
    list-height: 1em;
  }
}
```

生成的 CSS 将如列表 1-17 所示。

***列表 1-17***：上述代码片段的输出

```
nav ul {
  margin: 0 auto;
  padding: 10px 15px;
  list-height: 1em;
}
```

### 导入

预处理器的`@import`实现与 CSS 中的略有不同。Sass 不会从服务器获取文件，而是获取指定的文件并将其合并到你正在导入的文件中。这会向 web 浏览器提供一个单一的 CSS 文件。

假设你有两个 SCSS 文件，名为`reset.scss`和`style.scss`，你想将`reset.scss`导入到`style.scss`中。这可以通过 Sass 实现；见列表 1-18。

***列表 1-18***：将一个文件的内容导入另一个文件的示例

```
@import 'reset';
body {
        background-color: #efefef;
}
```

![Image](img/sq.jpg)**注意** 在 Sass 中导入文件时，无需指定文件扩展名。Sass 会自动识别合适的文件并将其导入。

然而，在 Less 中，导入的行为因文件扩展名而异。如果导入的文件具有`.css`扩展名，它将被视为普通的 CSS 导入，编译时不会改变。如果它有其他扩展名，它将被视为 Less 文件并会被导入和合并。

### 混合宏

混合宏允许你创建可在样式表中复用的 CSS 声明片段。通过传递参数以及为这些参数设置默认值，可以进一步配置混合宏。当你想要重复使用厂商前缀语法时，混合宏最为有用。它们减少了繁琐的代码重写（见列表 1-19 和 1-20）。

***列表 1-19***：在 Sass 中带参数和默认值的圆角混合宏示例

```
@mixin rounded-corners($radius:5px) {
  -webkit-border-radius: $radius;
     -moz-border-radius: $radius;
      -ms-border-radius: $radius;
          border-radius: $radius;
}

.container { @include rounded-corners(10px); }
```

***列表 1-20***：在 Less 中带参数和默认值的圆角混合宏示例

```
.rounded-corners(@radius:5px) {
  -webkit-border-radius: @radius;
     -moz-border-radius: @radius;
      -ms-border-radius: @radius;
          border-radius: @radius;
}

.container { .rounded-corners(10px); }
```

### 扩展/继承

预处理器允许你在一个或多个选择器之间共享一组 CSS 属性。这有助于你按照 DRY 原则编写预处理器代码。见列表 1-21 和 1-22。

***列表 1-21***：在 Sass 中用于扩展的示例

```
.messagebox {
        font-size:1em;
        line-height:40px;
}

.successBox {
        @extend .messagebox;
        color:#0F0;
}
```

***列表 1-22***：在 Less 中用于扩展的示例

```
.messagebox {
        font-size:1em;
        line-height:40px;
}

.successBox {
        &:extend(.messagebox);
        color:#0F0;
}
```

### 运算符

预处理器提供标准的数学运算符，以便你可以动态进行计算。见列表 1-23。

***列表 1-23***：在 Sass 和 Less 中展示数学运算的示例

```
.mainContent {
  float: left;
  width: 750px / 960px * 100%;
}
```

## CSS 预处理器的问题

本节讨论被不当使用的预处理器特性。你将会看到因误用或过度使用预处理器特性而导致的最常见问题。

### 混合宏的问题

混合宏是可复用的代码块，可以在需要时被调用。这避免了多次重写这些代码块。

考虑一个用 Sass 预处理器编写的代码片段，如列表 1-24 所示。

***列表 1-24***：一个使用 Sass 的混合宏示例

```
@mixin notification {
    color: #fff;
    border-radius: 50%;
}

.errorNotification {
    @include notification;
    background:#F00;
}

.successNotification {
    @include notification;
    background: #0F0;
}
```

这段代码编译成列表 1-25。

***列表 1-25***：列表 1-24 的输出

```
.errorNotification {
  color: #fff;
  border-radius: 50%;
  background: #F00;
}

.successNotification {
  color: #fff;
  border-radius: 50%;
  background: #0F0;
}
```

注意在列表 1-25 的输出中出现的代码重复。如果这个混合宏在整个样式代码中被广泛使用，将导致大量的代码冗余，从而增加文件大小。

Sass 有一种其他预处理器不支持的方法——*扩展*。通过以逗号分隔的格式指定选择器，扩展代码可以同时设置多个选择器的相同属性。

这种方法避免了重复；然而，如果扩展了太多选择器，可能会导致性能问题。生成的 CSS 将有许多逗号分隔格式的选择器，这可能导致性能问题。

### 扩展选择器或使用混合宏可能妨碍维护

扩展类或使用混合宏可能会导致维护问题。由于属性在不同的地方声明和使用，在没有完全预料影响的情况下修改属性的可能性很高。这可能导致布局在任何时候被破坏。

## 小结

本章介绍了预处理器。你了解了它们如何影响你的工作流程，使其更高效，并在未来更易于维护。你还了解了目前市面上一些知名的预处理器。

下一章将深入探讨 Sass，并讨论其各种特性。

