# 第 2 章 ■ 架构概述

**表 2-2.** 浏览器动作：设置标题、图标和弹出窗口

| 方法 | 描述 |
| :--- | :--- |
| `chrome.browserAction.setTitle(object details)` | `Details`对象接受键`tabId`和`title`，其中`tabId`（整数）是要限制此修改的标签页 ID，`title`是工具提示字符串。当此标签页关闭时，工具提示被重置。 |
| `chrome.browserAction.setIcon(object details, function callback)` | `Details`对象接受键`tabId`和`path`，其中`path`是相对图像路径。（可选的）`callback`参数应为一个形如`function() {...}`的函数。当此标签页关闭时，图标被重置。 |
| `chrome.browserAction.setPopup(object details)` | `Details`对象接受键`tabId`和`popup`，其中`popup`是弹出窗口中要显示的 HTML 文件。如果将其设置为空字符串（`""`），则不显示弹出窗口。当此标签页关闭时，弹出窗口被重置。 |

### 组件用法

现在您已经了解了这些组件是什么，让我们讨论如何在“组件示例”主题中使用它们——该主题包含供学习用的演示扩展。如前所述，浏览器动作组件用于需要对每个访问页面实现公共功能的扩展中。此类扩展的一个示例是用于在不同页面上记笔记的笔记记录扩展。

您将在“BrowserActionNotes”主题中操作此笔记记录扩展（以使用浏览器动作）。对于页面动作演示，您将使用修改版的 BrowserActionNotes 扩展，该扩展仅允许在属于`stackoverflow.com`主机的网页上记笔记。

此外，在“HelloPageAction 扩展”主题中演示了使用事件脚本来显示页面动作。

### 快捷键或命令

快捷键（或命令）是另一个有用的输入组件。顾名思义，快捷键是可以注册为扩展输入的键盘快捷方式。

每个这样的快捷键必须作为`commands`清单属性的属性列在清单中，如清单 2-4 所示。一个扩展可以有多个这样的快捷键属性，但最多只能有四个键盘快捷方式映射到这些属性。