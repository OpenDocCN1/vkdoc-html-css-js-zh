# 架构概览

## 关于弹窗脚本

**注意：** `Popup`脚本也代表扩展运行时。但它们不像事件脚本那样享有那么多特权（例如，持久性），因为弹窗脚本仅在相应弹窗打开时才会执行。因此，弹窗脚本只能成功监听少数事件。

## 事件脚本的必要性

到目前为止讨论的每个组件都有其特定用途。例如，`BrowserAction`和`PageAction`组件作为按钮输入；快捷键提供了一种替代的输入方式；弹窗组件提供视图，而弹窗脚本提供应用程序逻辑。现在你可能已经对事件脚本如何对扩展有用有了大致的了解。以下几点明确说明了其用途：

- 最重要的用途是监听来自输入组件的事件，例如来自`BrowserAction`或`PageAction`组件的点击事件、调用快捷键时的命令事件、以及来自上下文菜单项选择的上下文菜单事件。此外，事件脚本还可以监听来自多功能框输入的事件。以下是输入组件触发的事件的完整列表及相应的输入组件 API（这里列出了除`ContentUI`之外的所有输入组件）：
  - `chrome.browserAction` — `onClicked`
  - `chrome.pageAction` — `onClicked`
  - `chrome.commands` — `onCommand`
  - `chrome.contextMenus` — `onClicked`
  - `chrome.omnibox` — `onInputStarted`，`onInputChanged`，`onInputEntered`，`onInputCancelled`

**注意：** 如果在`manifest`中为`BrowserAction`或`PageAction`组件定义了`default_popup`，则`onClicked`事件将不会触发。

- 另一个重要用途是监听来自扩展本身的事件。这包括仅能从`chrome.runtime`对象访问的事件，例如`onMessage`，`onConnect`，`onInstalled`，`onUpdateAvailable`等。这些事件大多数是 Chrome 扩展框架提供的消息 API 的一部分。你将在下一章中了解这些事件。

**注意：** 内容脚本（一种注入到访问网页中的脚本组件）可用于在宿主网页中创建 HTML 元素。这些元素在本书中称为 Content UI。Content UI 只能触发标准的 JavaScript 事件。此外，内容脚本对 Chrome 扩展 API 的访问非常有限，因为它们不代表扩展运行时。它们只能访问以下 API：`chrome.runtime`，`chrome.extension`和`chrome.storage`。内容脚本仍然可以使用 Chrome 扩展框架提供的消息 API（可通过`chrome.runtime`对象访问）与扩展运行时通信。

- 正如你将在第 3 章中看到的，使用`onMessage`（或`onConnect`）事件，事件脚本可以监听来自扩展注入到网页中的内容脚本的消息。此外，使用`chrome.runtime.onMessageExternal`事件，事件脚本还可以直接监听来自网页的消息。

- 除了所有这些事件之外，事件脚本还可以监听与各种 API 功能关联的事件，例如`tabs`，`alarms`，`storage`，`bookmarks`，`history`等。这些事件（及其相应的 API）的示例如下：
  - `chrome.tabs` — `onCreated`，`onUpdated`，`onRemoved`等
  - `chrome.alarms` — `onAlarm`
  - `chrome.storage` — `onChanged`
  - `chrome.bookmarks` — `onCreated`，`onRemoved`，`onChanged`，`onImportBegan`，`onImportEnded`等
  - `chrome.history` — `onVisited`，`onVisitRemoved`

- 最重要的是事件脚本的**持久性**，它赋予了所有这些功能。事件脚本能够以可靠的方式监听事件，因为它是一个长期运行的脚本（与弹窗脚本不同，后者仅在弹窗打开时执行）。事件脚本会在需要时（即它们监听的事件被触发时）自动加载，并在再次空闲时卸载。

- 最后，事件脚本还可用于包含由扩展中多个组件共享的应用程序逻辑。例如，考虑一个允许在任何页面上记笔记的笔记扩展。最初，如果只使用`BrowserAction`，整个应用程序逻辑可以放在弹窗脚本中。但如果添加了其他输入组件，弹窗脚本中的代码将无法被这些组件访问（因为弹窗脚本仅在弹窗打开时执行）。

为了允许这种情况，共享代码（反映通用逻辑）可以放在事件脚本中。这使得各种组件可以不受限制地访问通用应用程序逻辑。显然，在这种情况下，弹窗脚本需要使用消息 API 与事件脚本来回通信（以便访问通用应用程序逻辑）。不要被这个例子搞糊涂，当你在本章后面完成`BrowserActionNotes`扩展时，你会更清楚地理解它。

## 此组件的清单角色

要在你的扩展中使用事件脚本，你需要在`manifest`中定义`background`属性。该属性是一个由`scripts`和`persistent`键组成的对象。

`scripts`键接受一个字符串数组作为其值，其中每个字符串是事件脚本的（相对）路径。而`persistent`键将布尔值`false`作为其值（如清单 2-4 所示）。允许多个事件脚本以模块化代码（图 2-10 和 2-11），如下所示：

```
"background" : {
  "scripts" : ["event_script.js", "another_event_script.js"],
  "persistent" : false
}
```

**图 2-10.** `HelloShortcutKey`：后台页面

**图 2-11.** `HelloShortcutKey`：后台页面

此外，一旦在`manifest`中定义了这些脚本，在扩展成功加载到浏览器后，它们会自动被引用到一个自动生成的 HTML 页面中，该页面称为**后台页面**，如图 2-12 和 2-13 所示。

**图 2-12.** 扩展管理页面：`HelloShortcutKey`

**图 2-13.** `HelloShortcutKey`：后台页面

实际的 HTML 文件命名不同（见图 2-14）。

**图 2-14.** `HelloShortcutKey`：后台页面

## 后台脚本

事件脚本仅在被监听的事件被触发时持久存在。当事件未被触发时，Chrome 浏览器会使事件脚本处于非活动状态（请注意，这不会立即发生），如图 2-15 所示。

**图 2-15.** 扩展管理页面：`HelloShortcutKey`

要使此脚本在浏览器打开期间持续存在，你需要通过在`manifest`中将`persistent`设置为`true`来将其标记为持久。此类事件脚本称为**后台脚本**。通常，你不需要在扩展中使用后台脚本。扩展框架中只有少数 API 调用需要后台脚本才能正确执行。本书中使用的大多数示例除非另有说明，否则仅使用事件脚本。

**注意：** **不推荐**使用后台脚本。这是出于性能原因——它们会占用更多内存和其他系统资源。

## 事件对象