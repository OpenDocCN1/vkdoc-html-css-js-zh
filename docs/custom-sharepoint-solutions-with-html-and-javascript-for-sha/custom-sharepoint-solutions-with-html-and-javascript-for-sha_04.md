# 1. 设置你的开发环境

正确设置开发环境将为你提供构建自定义解决方案所需的所有工具。许多你需要的功能已内置在 SharePoint 中，可直接使用。不过，你可能还需要下载并安装一些额外的工具来简化工作流程，这些工具包括：

*   `SharePoint Online` 开发人员网站
*   `SharePoint Designer`
*   `Visual Studio Express`
*   各类浏览器及开发者工具
*   `jQuery`
*   在 `SharePoint` 环境中部署 `jQuery`

如果这份清单让你感到棘手，不必担心。本章将详细探讨每一项内容，帮助你熟悉它们及其用途。此外，我们还会介绍如何创建自己的 `SharePoint Online` 环境作为开发区域，并演示如何在该环境中部署 `jQuery`。

如果你已经在开发 `SharePoint` 解决方案，甚至只是从事 Web 开发工作，那么这些工具很可能已经安装好了。本章将更侧重于面向业务用户或前端开发新手。如果你是一名经验丰富的开发者，可以快速浏览本章，确认是否已具备所需工具。无论你是谁，本章结束时你都将做好充分准备，开启在 `SharePoint` 中构建自定义解决方案的旅程！

### `SharePoint Online` 开发人员网站

毋庸置疑，如果没有一个可以施展的环境，能够构建自定义 `SharePoint` 解决方案的技能也毫无用武之地！有些读者可能已经拥有可用于开发的环境。这可能是你公司用于内部网的 `SharePoint Online` 环境，也可能是 `SharePoint 2013` 本地部署环境。也许你正初次尝试学习 `SharePoint` 开发，但目前还没有可操作的环境。无论何种情况，你都有多种选择，我们将在本节中逐一探讨。

> **注意**
> 本节讨论的选项将重点围绕 `SharePoint Online` 和 `Office 365`。如果你已在本地方安装了 `SharePoint`，可以轻松创建一个新网站进行开发。即使你工作中已能使用 `SharePoint`，你仍可能希望拥有自己的独立工作环境，因此本节提供的选项同样适用。

微软已大大简化了在最新版 `SharePoint` 上进行前端开发的入门流程。你可以通过以下网址注册 `Office 365` 开发人员网站：[`http://msdn.microsoft.com/en-us/library/office/fp179924(v=office.15).aspx`](http://msdn.microsoft.com/en-us/library/office/fp179924(v=office.15).aspx)。图 1-1 展示了该页面及各种注册选项。

![A978-1-4842-0544-0_1_Fig1_HTML.jpg](img/A978-1-4842-0544-0_1_Fig1_HTML.jpg)

图 1-1. 微软网站上的 `Office 365` 开发人员网站注册选项

如图所示，创建开发人员网站有三种方式：

*   `MSDN` 订阅用户 - 如果你或你所在公司订阅了 `MSDN`，你可以作为该权益的一部分免费创建一个开发人员网站。很多拥有专职开发人员的公司都订阅了 `MSDN`，或者你个人可能也拥有订阅。无论哪种情况，只需点击“立即兑换您的权益”链接即可开始。如果你是业务用户，也值得考虑这个选项。如果你的公司有 `MSDN` 订阅，你可以申请一个许可证，或让 IT 部门为你创建网站并授予访问权限。
*   `Office 365` 订阅用户 - 已订阅 `E3` 或 `E4` `Office 365` 计划的企业可以直接从 `Office 365` 管理中心预配开发人员网站。这些订阅级别允许使用针对开发人员的特殊网站模板创建新的专用网站集。有关创建开发人员网站的完整详细信息和步骤指南，请访问以下网址：[`http://msdn.microsoft.com/en-us/library/office/jj692554(v=office.15).aspx`](http://msdn.microsoft.com/en-us/library/office/jj692554(v=office.15).aspx)。
*   30 天试用版或开发者订阅 - 如果你希望或需要一个自己的环境，微软还提供 30 天的 `Office 365` 试用版。这是一个很好的选择，因为它为你提供了充足的时间来探索 `Office 365` 和 `SharePoint Online`，并在完全属于你自己的环境中测试开发技能。你还可以随时将该试用升级为每年 99.00 美元的年订阅。即使你在公司拥有 `SharePoint`，这也是一个绝佳选择。它能为你提供一个完全独立于生产环境的理想空间，用于构建自定义解决方案。每年 99 美元的价格，让你能以低成本构建解决方案，并可轻松迁移到生产环境中。

无论你的情况如何，你都可以轻松获得一个可用于开发自定义 `SharePoint` 解决方案的环境。我们已经讨论过如何获取 `SharePoint` 环境，接下来让我们探索一下本书中将使用的一些工具。

> **注意**
> 本书中的所有示例和截图均来自 `SharePoint Online` 环境，这是目前可用的最新版 `SharePoint`。

### `SharePoint Designer`

如果你以前用过 `SharePoint`，那么你可能也用过 `SharePoint Designer`。它是微软提供的免费工具，也是处理 `SharePoint` 相关任务的主力工具。它允许你执行广泛的任务——从品牌定制、连接外部数据库、构建列表等等。在本书中，`Designer` 并非核心工具，但它在帮助我们实现 `jQuery` 与 `SharePoint` 的集成方面将发挥关键作用。这部分内容将在本章末尾介绍。现在，让我们先下载 `Designer`。

你可以通过以下网址下载 `SharePoint Designer`：[`http://www.microsoft.com/en-us/download/details.aspx?id=35491`](http://www.microsoft.com/en-us/download/details.aspx?id=35491)。图 1-2 展示了微软下载中心网站上可供下载 `Designer` 的页面。

![A978-1-4842-0544-0_1_Fig2_HTML.jpg](img/A978-1-4842-0544-0_1_Fig2_HTML.jpg)

图 1-2. 微软网站上的 `SharePoint Designer` 下载页面

现在，你只需下载并安装 `Designer`；我们稍后会使用它。尽管本书未对 `Designer` 进行深入探讨，但当你在使用 `SharePoint` 时，它仍会是一个非常有用的工具。如前所述，它功能广泛，能协助你完成许多任务。`Designer` 同时适用于 `SharePoint Online` 和本地部署环境。

### `Visual Studio Express`

如果你不是开发者，可能没听说过 `Visual Studio Express`；即便听说过，它对你来说可能也像是为代码高手准备的高深程序。从某种程度上说，确实如此，但它是一个强大的工具，我们将用它来编写 `HTML` 和 `JavaScript` 代码。而且它还是免费的，由微软提供。

`Visual Studio` 是一个集成开发环境，简称 `IDE`。简单来说，就是“一个允许你编写代码的程序”。在本书中，我们所有的示例都将使用 `HTML` 和 `JavaScript`。`HTML` 和 `JavaScript` 的一个优点是，它们只是带有特殊扩展名（`.html` 和 `.js`）的文本文件。因此，创建和编辑这些文件真正需要的只是`记事本`。然而，尽管`记事本`看起来友好且不具威胁性，但在编写代码时它绝不是你的朋友。`Visual Studio Express` 提供了你所需的所有工具——甚至在检测到问题时还会提醒你。`记事本`可做不到这一点！

根据技术需求的复杂程度，`Visual Studio` 有多种不同的版本。`Express` 版是一个功能齐全的版本，完全能满足本书的需求。你可以通过以下网址下载 `Visual Studio Express`：[`http://www.visualstudio.com/en-us/products/visual-studio-express-vs.aspx`](http://www.visualstudio.com/en-us/products/visual-studio-express-vs.aspx)。图 1-3 展示了 `Visual Studio` 网站的下载页面。

![A978-1-4842-0544-0_1_Fig3_HTML.jpg](img/A978-1-4842-0544-0_1_Fig3_HTML.jpg)

图 1-3. 微软网站上的 `Visual Studio Express` 下载页面

`Visual Studio Express` 有多个版本，包括 Web、Windows 等。`Express for Web` 是使用 `HTML` 和 `JavaScript` 构建解决方案的理想版本。

### 浏览器与开发者工具

当最终用户使用`SharePoint`时，显然首选浏览器是`Internet Explorer`。虽然`SharePoint`与其他浏览器（如`Firefox`和`Chrome`）配合良好，但`Internet Explorer`无疑是微软推荐的浏览器，也是`SharePoint`最可能友好兼容的。话虽如此，本书主要介绍如何使用`HTML`和`JavaScript`构建自定义解决方案，你会发现大多数现代浏览器的操作和表现都是一致的。在讨论浏览器之前，我们先来谈谈开发者工具。

开发者工具是一个通用术语，指的是用来帮助开发、调试，以及了解浏览器中`HTML`、`JavaScript`和`CSS`运行状况的工具。它们也可能被称为开发者工具栏、浏览器工具，或者完全不同的名称，比如`Firebug`。无论是什么工具，它们的功能都是一样的：帮助你开发解决方案。在几乎所有的浏览器中，你只需按下键盘上的`F12`键即可启动开发者工具，如图 1-4 在`Internet Explorer`中所示。

![A978-1-4842-0544-0_1_Fig4_HTML.jpg](img/A978-1-4842-0544-0_1_Fig4_HTML.jpg)

图 1-4. 在 Internet Explorer 中按下`F12`后启动的开发者工具

如图 1-4 所示，这些工具提供了大量关于浏览器内部运行情况的洞察。你可以立即看到生成页面的`HTML`，以及应用的`CSS`样式。使用工具左侧的菜单，你还可以查看页面上加载了哪些`JavaScript`，甚至查看其中的代码。你甚至可以更进一步，在`JavaScript`中设置“断点”来暂停其执行，以便观察这些脚本运行时发生了什么。在前端开发工作中，这是一个非常宝贵的工具，我们会经常用到它。

图 1-5 展示了在`Google Chrome`中打开的开发者工具。同样，只需按下`F12`键即可打开。与`Internet Explorer`一样，你可以立即看到页面上应用的`HTML`和样式。

![A978-1-4842-0544-0_1_Fig5_HTML.jpg](img/A978-1-4842-0544-0_1_Fig5_HTML.jpg)

图 1-5. 在 Google Chrome 中按下`F12`后启动的开发者工具

在`Chrome`中，菜单选项横跨工具屏幕顶部，提供类似的功能。你可以通过再次按下`F12`键来关闭开发者工具，就像打开它们一样。你应该下载几种浏览器，熟悉一下它们的工具。每个开发者对浏览器和工具都有自己的偏好；我偏好的是`Firefox`和`Firebug`，本书将使用它们。

我要明确地说，我认为`Internet Explorer`是一款出色的浏览器，至少最新版本是这样。这些年来它取得了长足的进步，终于能与一些后来居上的新秀浏览器相媲美。`Internet Explorer`中的开发者工具非常好用，但我最偏好的还是`Firefox`中的`Firebug`。`Firebug`是`Firefox`的一个附加组件，其功能与其他浏览器中的工具类似，但在我看来，它组织内容的方式更友好、更易用。你绝对应该找到自己的偏好，使用最适合你的浏览器和工具。本书中的所有示例都将使用`Firebug`，但你并不局限于这些工具。

要安装`Firebug`，请通过以下网址下载`Firefox`浏览器：[`https://www.mozilla.org/firefox/`](https://www.mozilla.org/firefox/)。安装完成后，点击右上角的菜单图标。图 1-6 展示了打开的菜单，其中高亮了“附加组件”选项。

![A978-1-4842-0544-0_1_Fig6_HTML.jpg](img/A978-1-4842-0544-0_1_Fig6_HTML.jpg)

图 1-6. Firefox 菜单中的“附加组件”菜单选项

点击“附加组件”选项，将在新标签页中打开“附加组件管理器”页面。该页面允许你管理所有附加组件并搜索新的组件。在屏幕右上角的搜索框中输入`firebug`，然后点击放大镜图标。图 1-7 显示了输入后的搜索框。

![A978-1-4842-0544-0_1_Fig7_HTML.jpg](img/A978-1-4842-0544-0_1_Fig7_HTML.jpg)

图 1-7. 搜索前的附加组件管理器页面上的搜索框

搜索结果会返回所有匹配的附加组件，`Firebug`应该在列表顶部。点击图 1-8 中所示的“安装”按钮，将`Firebug`添加到`Firefox`中。

![A978-1-4842-0544-0_1_Fig8_HTML.jpg](img/A978-1-4842-0544-0_1_Fig8_HTML.jpg)

图 1-8. 点击“安装”按钮将在 Firefox 中安装 Firebug

下载和安装完成后，`Firefox`会在右上角菜单旁边显示一个“bug”图标。图 1-9 高亮了`Firebug`图标。

![A978-1-4842-0544-0_1_Fig9_HTML.jpg](img/A978-1-4842-0544-0_1_Fig9_HTML.jpg)

图 1-9. 安装完成后显示 Firebug 图标

现在`Firebug`已安装，你可以像在其他浏览器中打开开发者工具一样打开它——按下键盘上的`F12`键。你也可以点击`Firebug`图标来打开它，如图 1-10 所示。

![A978-1-4842-0544-0_1_Fig10_HTML.jpg](img/A978-1-4842-0544-0_1_Fig10_HTML.jpg)

图 1-10. 通过点击`F12`切换打开的 Firebug

如图 1-10 所示，`Firebug`的外观和功能与其他浏览器中的开发者工具非常相似。再次强调，最终选择使用哪些工具是你的个人偏好。本书中的所有示例都将使用`Firefox`和`Firebug`。

### jQuery

如果你过去做过任何 JavaScript 开发工作，你可能会时常感到有些沮丧。JavaScript 是一个出色的前端开发框架，它速度快，能让你迅速构建有用的代码。但有时它难以驾驭、难以调试，而且让新手望而生畏。jQuery 的出现改变了这一切。

jQuery 是一个与 JavaScript 协同工作的框架，旨在让前端代码的编写更简单、更快速。jQuery 自称是“写得更少，做得更多”的框架——你会发现事实确实如此。它很快成为了 JavaScript 开发的标准，本书中的许多示例都会以某种方式用到 jQuery。如果你从未使用过它，也别担心：我们会从基础开始，让你逐步掌握。

**注**

如果你是 jQuery 新手，jQuery 网站上有一个很棒的“入门”指南，网址是 [`http://learn.jquery.com/about-jquery/how-jquery-works/`](http://learn.jquery.com/about-jquery/how-jquery-works/)。此外，还有杰克·富兰克林（Apress，2013 年）编写的《jQuery 入门》一书，对于刚起步的读者来说是一个很好的资源（参见 [`www.apress.com/9781430249320`](http://www.apress.com/9781430249320)）。

现在，我们来做一个快速的 jQuery 练习，让你熟悉它的工作原理。jQuery 的主要优点之一是能够轻松选择页面上的 HTML 元素。实现这一点的两种主要方法是使用元素的 ID 或其 CSS 类。图 1-11 显示了在打开 Firebug 的 Firefox 浏览器中的 jQuery 主页（[`jquery.com`](http://jquery.com)）。我们将在 jQuery 的网站上做这个练习，因为该网站肯定已经加载了 jQuery！

![A978-1-4842-0544-0_1_Fig11_HTML.jpg](img/A978-1-4842-0544-0_1_Fig11_HTML.jpg)

*图 1-11. 在打开 Firebug 的 Firefox 浏览器中显示 jQuery 主页。将鼠标悬停在某个 HTML 部分上，页面会高亮显示它*

我们在 Firebug 中展开了 HTML 结构，并定位到页面左上角的徽标。当鼠标悬停在 HTML 上时，Firebug 会在浏览器中高亮显示相应的部分，让你能轻松看到它。在此页面上，徽标位于一个 ID 为 `"logo-events"` 的 `DIV` 元素内部。由于该元素有 ID，因此可以使用 jQuery 轻松定位。在 Firebug 中，单击标题为“Script”（脚本）的选项卡将其激活，如图 1-12 所示。

![A978-1-4842-0544-0_1_Fig12_HTML.jpg](img/A978-1-4842-0544-0_1_Fig12_HTML.jpg)

*图 1-12. 在 Firebug 中激活的“Scripts”（脚本）选项卡*

在“Scripts”（脚本）选项卡中，有两个主要区域可用于处理前端代码。左侧面板显示当前正在检查的脚本。它上方有一个下拉菜单，你可以查看并选择当前页面加载的所有 JavaScript 文件。我们稍后会详细探讨这一点。右侧面板有更多选项卡，但最主要的是“Watch”（监视）选项卡。在此选项卡中，我们可以使用“New watch expression”（新建监视表达式）文本框直接在浏览器中按需运行 JavaScript 命令。在编写前端代码时，这个区域将非常有用。

单击“New watch expression”（新建监视表达式）文本框内部，输入以下代码行，然后按回车键：

```
$("#logo-events")
```

图 1-13 显示了运行这行代码的输出结果。

![A978-1-4842-0544-0_1_Fig13_HTML.jpg](img/A978-1-4842-0544-0_1_Fig13_HTML.jpg)

*图 1-13. 在“Watch”（监视）选项卡中运行 jQuery 命令的输出*

如你所见，结果是一个 `Object`（对象），在方括号内你可以看到 `"div#logo-events"`，这意味着 jQuery 成功找到了你正在查找的元素。让我们花点时间看看这到底是怎样发生的，以及 jQuery 是如何工作的。我们可以将 jQuery 命令的结构分解如下：

- `$` - 这是调用 jQuery 的简写符号。所有 jQuery 命令都以这个符号开头。你可以把它理解为让 jQuery 去为你做一些事情。在这个例子中，我们是在说：“jQuery，去找出具有这个 ID 的元素。”`$` 后面总是跟着 `()`，括号内是你正在查找的元素。

- `#` - 这是一个简写符号，用于告诉 jQuery 你正在通过元素的 ID 来查找。`#` 后面的任何文本都应该是 HTML 元素上的有效 ID，并且只会返回一个结果，因为 HTML 元素不能共享 ID。

- `.` - 这是一个简写符号，用于告诉 jQuery 你正在通过元素的 CSS 类来查找。因此，如果你的 HTML 中有 10 个共享同一个 CSS 类 `"red-box"` 的 `DIV` 元素，你可以使用以下命令找到它们：`$(".red-box")`。它的结构与 ID 命令完全相同，只是将 `$` 换成了 `.`。`.` 后面总是跟着 `()`，括号内是你正在查找的元素。

既然我们已经找到了包含徽标的 `DIV` 元素，就可以开始对其使用 jQuery 并操作页面上的 HTML 了。单击“New watch expression”（新建监视表达式）文本框内部，输入以下代码行，然后按回车键：

```
$("#logo-events").hide("slow")
```

你会看到徽标区域从浏览器窗口中慢慢消失。你可以使用以下命令将其显示回来：

```
$("#logo-events").show("slow")
```

这些都是非常简单的示例，但应该能展示 jQuery 能轻松为你的代码带来的强大功能。本书将通篇使用 jQuery，因此随着学习的深入，你会看到更多示例，并对它的使用越来越熟悉。现在，让我们在 SharePoint 环境中实现 jQuery，以便随时使用。

### 使用 SharePoint Designer 在 SharePoint 环境中实现 jQuery

对于部分人来说，jQuery 已经在 SharePoint 中实现了。这可能是为了支持之前的开发而完成的。但是，如果你是刚开始使用，或者刚刚注册 SharePoint Online 的试用版，那么你需要实现 jQuery 才能学习本书中的示例。实际上，实现 jQuery 非常简单直接，在本节中，我们将介绍如何使用 SharePoint Designer 来实现。

**注**

在你的环境中实现 jQuery 有多种不同的方法。这些方法包括通过自定义代码部署将其注入到所有页面，在特定页面上放置内容编辑器 Web 部件等等。我们将使用 SharePoint Designer，因为它提供了一种快速简便的方法将 jQuery 引入 SharePoint。

#### 获取 jQuery

在实现 jQuery 之前，我们首先需要引用 jQuery 代码，或者下载一份副本并将其物理上传到 SharePoint。在本节中，我们将使用内容分发网络（CDN）来引用 jQuery。CDN 基本上是一个公共可用的服务器，上面的内容你可以用于自己的网站。jQuery 运营着一个 CDN，你可以在 [`https://code.jquery.com`](https://code.jquery.com/) 上引用它。包括微软和谷歌在内的许多其他公司也运营着托管 jQuery 的 CDN。

使用 CDN 的好处很多。最大的好处是速度快，因为在 Web 上有许多网站使用相同的 CDN。如果你的用户已经访问过其中一个站点，那么 jQuery 可能已经加载到他们的浏览器中了，因此你的页面加载速度会更快。另一个大好处是你无需担心直接下载和存储代码。无论哪种情况，在生产站点上使用 CDN 的决定都应咨询你的 IT 部门。在我们的案例中，这完全没问题，你应该放心在开发环境中使用它。

要从 CDN 引用 jQuery，我们将使用以下代码行：

```
<script src="//code.jquery.com/jquery-1.11.0.min.js"></script>
```

这是一个基本的 HTML 脚本标签，它将从 jQuery 的 CDN 加载版本 1.11 的 jQuery。你会注意到 URL 中没有 `HTTP` 或 `HTTPS`。这样移除后，脚本可以在非安全或安全 URL 下同样工作。现在让我们看看如何将此代码放入 SharePoint 中。

#### 团队网站模板

现在我们已经知道如何实现 `jQuery`，接下来就要实际动手了！我们将探讨两种选项：团队网站和发布网站。两者略有不同，所以我们先来看团队网站。启动 SharePoint Designer，点击“打开网站”图标，如图 1-14 所示。

![A978-1-4842-0544-0_1_Fig14_HTML.jpg](img/A978-1-4842-0544-0_1_Fig14_HTML.jpg)

图 1-14. SharePoint Designer 中显示“打开网站”图标的主屏幕

点击此按钮将弹出“打开网站”对话框，你需要在其中指定 SharePoint 网站集的 URL。你可以打开特定的网站集，也可以通过输入 SharePoint 环境的基础 URL 来打开根网站。如果你使用的是 SharePoint Online，根网站的 URL 格式类似 `https://YOURSITENAME.sharepoint.com`。

图 1-15 展示了“打开网站”对话框。你需要在标有“网站名称”的文本框中输入 URL，然后点击“打开”。在 Designer 打开网站之前，系统会弹出另一个对话框要求你输入凭据。

![A978-1-4842-0544-0_1_Fig15_HTML.jpg](img/A978-1-4842-0544-0_1_Fig15_HTML.jpg)

图 1-15. “打开网站”对话框，你需要在此提供 SharePoint 网站的 URL

一旦提供凭据并通过身份验证，SharePoint Designer 将打开并显示你的网站视图。如果你从未使用过 Designer，不妨花点时间探索一下；你可能会对其提供的信息丰富程度印象深刻。Designer 让你可以深入了解所有列表和库、工作流、外部内容类型以及更多内容。它是一个非常有用的工具——尤其是在无法访问服务器的 SharePoint Online 环境中。

**注意：** SharePoint Designer 是一个非常强大的工具，能轻易改变你的 SharePoint 环境。如果你是新手，在导航时要格外小心，只进行那些你知道不会对环境造成危害的更改。

熟悉 Designer 后，点击左侧导航中的“母版页”链接，如图 1-16 所示。此区域不仅显示 SharePoint 中的所有母版页，还显示其他布局，包括搜索结果、显示模板等。我们将在本书后面再次探讨此区域，但现在，请选中 `seattle.master` 文件。选中该文件后，点击功能区中的“编辑文件”。

![A978-1-4842-0544-0_1_Fig16_HTML.jpg](img/A978-1-4842-0544-0_1_Fig16_HTML.jpg)

图 1-16. SharePoint Designer 中的“母版页”选项卡。选中 `seattle.master` 文件后，点击功能区中的“编辑文件”

如果你是开发新手，下一个界面可能看起来有点吓人，但其实不然。编辑 `seattle.master` 文件时，你会看到所有呈现 SharePoint 页面的 ASP.NET 代码。嗯，几乎全部。此文件负责 SharePoint 页面的公共部分，例如徽标、功能区、导航等。虽然内容很多，但我们只需要添加 `jQuery` 引用——然后退出！

我们将代码行放置在页面顶部附近，就在闭合的 `</head>` 标签之前，如图 1-17 所示。

![A978-1-4842-0544-0_1_Fig17_HTML.jpg](img/A978-1-4842-0544-0_1_Fig17_HTML.jpg)

图 1-17. 添加到 Seattle 母版页的 `jQuery` 引用

如图 1-17 所示，我们将 `jQuery` 引用放置在闭合的 `</head>` 标签之前，该标签正好位于开始的 `<body>` 标签上方。它基本上将成为 Head 区域中母版页引用的最后一个脚本。放置完成后，点击屏幕左上角的“保存”图标保存更改。这将自动将更改发布到你的 SharePoint 环境。

保存更改后，启动浏览器并导航到你的网站。图 1-18 展示了打开 Firebug 的 SharePoint 环境。检查 HTML，我们可以看到 `jQuery` 引用现在已存在于母版页中，并且可供 SharePoint 中的所有页面使用。

![A978-1-4842-0544-0_1_Fig18_HTML.jpg](img/A978-1-4842-0544-0_1_Fig18_HTML.jpg)

图 1-18. 使用 Firebug 检查 SharePoint 环境，以确保 `jQuery` 引用已就位

**注意：** 一般来说，不建议修改 SharePoint 的现成母版页。最主要的原因是，由于它们是现成的，如果 SharePoint Online 发布了更新或新版本并进行升级，你的更改极有可能会丢失。最佳实践是复制一份要修改的母版页，然后修改副本。这样，更新就不会覆盖你的更改。

## 发布网站模板

发布网站在修改母版页时表现得截然不同。从 SharePoint 2013 开始引入了设计管理器，它允许你上传一个 HTML 文件，该文件将自动转换为母版页。这对于想要上传自定义设计并彻底改变网站外观的用户来说非常有用。然而，我们只需要注入 jQuery，因此需要多完成几个步骤。

打开 SharePoint Designer，点击左侧导航中的 `Master Pages` 链接。你首先会注意到文件数量比团队网站多得多。所有额外的文件都是发布网站自带的，目前你基本上可以忽略它们。找到 `seattle.master` 文件，你会注意到它附带了一个对应的 HTML 文件，如图 1-19 所示。

![A978-1-4842-0544-0_1_Fig19_HTML.jpg](img/A978-1-4842-0544-0_1_Fig19_HTML.jpg)

图 1-19 发布网站模板中的 SharePoint Designer 母版页

在这种模式下，你只能编辑 HTML 文件。当你对 HTML 文件进行编辑并保存后，更改会被转换并保存到母版页文件中。我们首先需要做的是复制一份 `seattle.html` 文件，因为这是我们可以实际编辑的文件。选中 `seattle.html` 文件，从功能区的“剪贴板”部分点击 `Copy` 按钮。随后 `Paste` 按钮会变为可用状态。点击 `Paste`。将创建一份 `seattle.html` 的副本，如图 1-20 所示。

![A978-1-4842-0544-0_1_Fig20_HTML.jpg](img/A978-1-4842-0544-0_1_Fig20_HTML.jpg)

图 1-20 功能区中的“复制”和“粘贴”按钮

复制文件后，它会被粘贴进来，默认名称为 `seattle_copy(1).html`。对大多数人来说，这显然不是一个可接受的名称！你可以右键单击新文件，然后从上下文菜单中选择 `Rename`，如图 1-21 所示。

![A978-1-4842-0544-0_1_Fig21_HTML.jpg](img/A978-1-4842-0544-0_1_Fig21_HTML.jpg)

图 1-21 上下文菜单中的“重命名”选项

我们将文件重命名为 `seattle_jquery.html`，然后按回车键。由于 HTML 文件通过同名链接到母版页，因此在你重命名新文件之前，Designer 会提示你。图 1-22 显示了重命名时你会看到的提示。

![A978-1-4842-0544-0_1_Fig22_HTML.jpg](img/A978-1-4842-0544-0_1_Fig22_HTML.jpg)

图 1-22 重命名复制的 HTML 文件时的重命名提示

重命名后，你现在将拥有一个 `seattle_jquery.html` 文件和一个 `seattle_jquery.master` 文件。选中新的 `seattle_jquery.html` 文件，点击功能区中的 `Edit File` 按钮。与团队网站母版页类似，我们将插入以下代码行来引用 jQuery：

```
<script src="//code.jquery.com/jquery-1.11.0.min.js"></script>
```

这次我们将把它放在一个不同的位置，如图 1-23 所示。它将被添加在 `META` 标签下方，紧跟在一些 SharePoint 开箱即用的标签之后。

![A978-1-4842-0544-0_1_Fig23_HTML.jpg](img/A978-1-4842-0544-0_1_Fig23_HTML.jpg)

图 1-23 向新的母版页文件添加 jQuery 代码行

添加完 jQuery 引用后，点击 `Save`，这将保存并发布文件。它还会生成一个包含修改的新 `seattle_jquery.master` 文件。现在我们已经添加了引用并更新了所有文件，需要将此新母版页设置为默认，以便我们的网站使用它。选中 `seattle_jquery.master`，点击功能区中的 `Set as Default`，如图 1-24 所示。

![A978-1-4842-0544-0_1_Fig24_HTML.jpg](img/A978-1-4842-0544-0_1_Fig24_HTML.jpg)

图 1-24 将新母版页设置为默认

现在新的母版页已创建并设置为默认，我们需要检查确保 jQuery 引用已正确设置。导航到使用发布网站模板的站点。图 1-25 展示了打开了 Firebug 的 SharePoint 环境。检查 HTML，我们可以看到 jQuery 引用现在位于母版页中，并且对 SharePoint 中的所有页面都可用。

![A978-1-4842-0544-0_1_Fig25_HTML.jpg](img/A978-1-4842-0544-0_1_Fig25_HTML.jpg)

图 1-25 新母版页中的 jQuery 引用

任何基于使用发布模板的父站点创建的子站点将自动继承此新母版页。这是在母版页中添加 jQuery 引用的首选方法。复制文件可确保在 SharePoint 应用更新时，你的更改保持不变。

### 总结

在本章中，我们介绍了本书中做前端开发所需的各种组件。如果你已经在开发解决方案，那么在拿起这本书之前，你可能已经安装了大部分（如果不是全部）这些组件。如果你是前端开发新手，你可能安装了一些新程序！现在，你应该已经拥有开始创建自己自定义解决方案所需的一切。这些组件的一大优点是，除了 SharePoint Online 和 Designer 之外，它们都可以用于许多其他项目。如果你发现自己喜欢前端开发，你可以将这些工具用于各种项目！

在下一章中，我们将探讨服务器端开发和前端开发之间的区别。我们还将研究 SharePoint 提供的用于支持此类开发的一些组件。你将看到一些代码，并为后续章节做好准备！
