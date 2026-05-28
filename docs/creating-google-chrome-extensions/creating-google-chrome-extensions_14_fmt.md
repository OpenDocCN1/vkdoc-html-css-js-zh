# 注意

*大多数*这些输入组件可以组合在一个扩展程序中。例如，Google Dictionary（Google 出品）扩展程序使用 `Browser-Action`、`Content-UI`（事件附加到文档内容）和 `Shortcut-Key` 作为输入组件。

那么，现在您已经理解了输入是什么。让我们进一步了解它们，包括如何使用它们，以及如何在清单中声明和定义它们。让我再次重申，清单是您声明扩展程序将要使用的功能并定义其对应值的地方。

但在继续之前，请记住，*无需*任何输入组件也可以创建功能完整的扩展程序。例如，一个自动将访问网页的详细信息发送到 HTTP 服务器的扩展程序。除了这个例子外，为 Chrome 浏览器创建的*主题*也是在没有任何输入组件的情况下创建的。事实上，主题完全使用清单组件（和一些图像）创建。此外，主题也是一种扩展程序！您已经在上一章学习了创建主题。

### `Browser-Action` 和 `Page-Action` 组件

`Browser-Action` 和 `Page-Action` 组件是 Google Chrome 工具栏中的按钮，如图 2-1 所示。`Browser-Action` 始终位于地址栏的右侧（地址栏外部），而 `Page-Action` 始终位于地址栏内部（与地址栏右侧对齐）。在图 2-1 中，`Browser-Action` 是位于多功能框右侧的圆形紫色按钮。`Page-Action` 是位于多功能框内部的浅紫色三角形按钮。请注意，一个扩展**只能**使用其中之一——要么是 `Browser-Action`，要么是 `Page-Action` 组件。

`Browser-Action` 和 `Page-Action` 都有一个与之关联的 `popup` 组件（以及图标和标题）。当点击对应 `Browser-Action` 或 `Page-Action` 按钮的图标时，会显示此弹出窗口。在清单文件中为这些操作分配的标题会被设置为 `Browser-Action` 或 `Page-Action` 按钮的 `tooltip`（取决于您使用的是哪一个）。`Browser-Action` 和 `Page-Action` 的 API 可以从所有脚本组件中访问，但内容脚本除外。`chrome.browserAction` 对象用于访问 `Browser-Action` 的 API。类似地，`chrome.pageAction` 对象用于访问 `Page-Action` 的 API。

**第 2 章 ■ 架构概述**

> **注** 在 Google Chrome 浏览器中，地址栏也称为*多功能框*。在本书中，这两个词可互换使用。

#### 需要单独操作的原因

`Browser-Action` 表示对所有网页通用的操作，而 `Page-Action` 则并非如此。`Page-Action` 是针对特定网页设计的（例如，属于 `facebook.com` 的网页、包含密码字段的网页等）。这就是 `Page-Action` 名称的由来。这是 Chrome 的建议，但并没有阻止您违反它。您可以为所有网页使用 `Page-Action`，而不是（建议的）仅用于特定网页。但在 API 和架构方面，`Page-Action` 有一个限制，这清楚地表明了其实际用途。

一旦在清单文件中声明和定义，`Browser-Action` 始终可见。`Page-Action` 则并非如此。为了在浏览器中显示 `Page-Action` 按钮，需要从任何脚本组件（内容脚本除外）调用 `chrome.pageAction.show(tabId)` 方法（参见清单 2-7 和 2-8）。此外，`chrome.declarativeContent` API 也可用于显示 `Page-Action`（不必担心，您将在后续主题中了解更多关于这些 API 的知识）。

> **注** 在扩展开发的上下文中，每个标签页都有一个唯一的 ID。此外，在 Chrome 扩展框架中，每个标签页都使用 `Tab` 类型表示。此类型具有许多相关属性，其中重要的包括 `id`、`active` 和 `url`。完整的属性列表可在 URL [`developer.chrome.com/extensions/tabs#type`](https://developer.chrome.com/extensions/tabs#type) 找到。`chrome.tabs` API 用于操作标签页。您将在本章后续主题中了解更多相关内容。

请注意，`popup` 组件可用于 `Browser-Action` 和 `Page-Action` 输入组件。由于弹出脚本始终附加到弹出窗口，因此该脚本始终可用于这些输入组件（显然，只要用户点击这些输入组件后弹出窗口可见即可）。`popup` 组件的唯一目的是包含额外的视图，例如表单字段。

但要显示弹出窗口，需要点击对应 `Browser-Action` 或 `Page-Action` 组件的按钮。

#### 此组件的清单文件作用

`Browser-Action` 和 `Page-Action` 组件需要在清单文件中声明。除了声明之外，您还需要定义这些组件（无论您在扩展中使用哪一个）。`Browser-Action` 在清单文件中按以下方式声明（和定义）（清单 2-2）。

**第 2 章 ■ 架构概述**

***清单 2-2.*** 第 2 章/`HelloBrowserAction/manifest.json`

```json
"browser_action" : {
  "default_title" : "HelloBrowserAction",
  "default_icon" : "icon.png",
  "default_popup" : "popup.html"
}
```

类似地，清单 2-3 演示了如何在清单文件中声明（和定义）`Page-Action`。再次提醒您，**一个扩展只能使用其中之一**——要么是 `Browser-Action` 组件，要么是 `Page-Action` 组件，但不能同时使用。

***清单 2-3.*** 第 2 章/`HelloPageAction/manifest.json`

```json
"page_action" : {
  "default_title" : "HelloPageAction",
  "default_icon" : "icon.png",
  "default_popup" : "popup.html"
}
```

如您所见，这两个输入组件的声明和定义是同时进行的。如果清单中仅包含属性（`browser_action` 或 `page_action`）而没有对应的值，当尝试将此包上传到 Chrome 浏览器进行测试时，将会产生错误。因此，除了属性外，您还需要提供相应的值。对于 `Browser-Action` 和 `Page-Action` 组件，这些值是键值对，包含以下键：

*   `default_title` — 设置为扩展的提示工具
*   `default_icon` — 设置此路径下的 PNG 图像资源作为扩展的图标
*   `default_popup` — 设置此路径下的 HTML 文件作为扩展的弹出窗口

> **注** 清单文件中提供的所有资源路径都是相对于扩展文件夹根目录的。例如，如果您有一个名为 `HelloWorldExtension` 的文件夹，其中包含清单文件，那么 `default_icon "icon.png"` 是相对于根目录的，其完整路径将是 `HelloWorldExtension/icon.png`。因此，请确保只提供相对路径。

#### 定义组件

对于 `browser_action` 或 `page_action` 属性，`null`、空字符串、`bool` 等都是无效值。唯一允许的值是一个对象，即 `{}`。此对象可以为空。

这通常是为了快速部署扩展以进行测试。此外，有时您可能出于其他原因（如下文所述）而有意这样做。

无论情况如何，只要 `browser_action` 或 `page_action` 属性的值为空对象，扩展就可以成功加载。

您可能会疑惑，不提供值的情况下这是如何工作的。嗯，它能工作是因为 Chrome 会提供一个默认的扩展图标（如图 2-5 所示）和一个默认的标题（使用您在清单文件中为 `name` 属性分配的扩展名称）。但不会为此操作设置默认的弹出窗口。

***图 2-5.** Google Chrome 浏览器中的默认扩展图标*

请注意，所有这些属性键都带有 `default` 前缀，因为它们的值可以由扩展运行时使用某些 API 调用（由 Chrome 扩展框架提供）覆盖。这些调用分别列于表 2-1（`Page-Action` 组件）和表 2-2（`Browser-Action` 组件）。

***表 2-1.** `Page-Action`：设置标题、图标和弹出窗口*

| 方法 | 描述 |
| :--- | :--- |
| `chrome.pageAction.setTitle(object details)` | `details` 对象接受键 `tabId` 和 `title`，其中（整数）`tabId` 是您要修改 `Page-Action` 的标签页 ID，`title` 是提示工具字符串。 |

| `chrome.pageAction.setIcon(object details, function callback)` | `details`对象接受键`tabId`和`path`，其中`path`是相对图像路径。（可选的）`callback`参数应是一个看起来像`function() {...}`的函数。 |