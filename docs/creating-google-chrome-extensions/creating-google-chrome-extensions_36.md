# 与 API 的权限类似，*匹配模式*提供对一个或多个主机的访问权限（用于 XHR API）。为避免对扩展程序或用户安全造成损害，您应*仅在扩展程序中允许列入白名单的主机*，或者实际上，仅允许扩展程序所需的确切白名单远程服务。

#### 外部可连接

如前文“网页脚本和事件脚本”部分（第 3 章）所述，要使扩展程序能够接收来自外部网页（或其他扩展程序）的消息，清单文件中需要包含`externally_connectable`属性。

对于`ids`和`matches`键，您应分别指定需要向扩展程序发送消息的外部网页的扩展程序 ID 和 URL 模式。这样，您可以避免扩展程序收到未经请求的消息，并*专门选择您想要通信的源*。

#### 存储

`storage` API 是扩展程序框架提供的优化 API，用于满足扩展程序特定的存储需求。然而，需要注意的重要一点是，所提供的存储空间并未加密。因此，切勿使用此 API 存储机密的用户信息。

要为扩展程序存储用户数据，您可以使用`storage.sync`或`storage.local`。

使用`storage.sync`时，只要用户启用了同步功能，存储的数据将自动同步到用户登录的任何 Chrome 浏览器。

当 Chrome 处于离线状态时，Chrome 会将数据存储在本地。下次浏览器在线时，Chrome 会同步这些数据。即使**用户禁用了同步，`storage.sync`仍然有效**。在这种情况下，它的行为将与`storage.local`相同。

要使用`storage.sync` API，需要进行以下调用。要了解更多关于此 API 的信息，您可以回看上一章中“Storage API”主题。

```
chrome.storage.sync.get(string or array of string or object keys, function callback)
chrome.storage.sync.set(object items, function callback)
```

#### 内容脚本

在编写内容脚本时，您应注意两个安全问题。首先，注意不要将安全漏洞引入到您注入内容脚本的网页中。例如，如果您的内容脚本从远程服务器（例如，通过 XHR API）接收数据，在将其注入当前网页之前，请务必过滤该数据，以防跨站脚本攻击。例如，最好通过`innerText`而不是`innerHTML`来注入数据。

> **注意：** 内容脚本在一个称为*隔离世界*的特殊环境中执行。它们可以访问所注入网页的 DOM，但无法访问该网页创建的任何 JavaScript 变量或函数。在每个内容脚本看来，其运行的网页上没有其他 JavaScript 在执行。反之亦然：网页上运行的 JavaScript 无法调用内容脚本定义的任何函数或访问其任何变量。隔离世界允许每个内容脚本对其 JavaScript 环境进行更改，而无需担心与网页或其他内容脚本发生冲突。

值得注意的是网页和扩展程序共享的 JavaScript 对象会发生什么——例如`window.onload`事件。每个隔离世界都看到自己版本的对象。对该对象的赋值会影响您的独立副本。例如，网页和扩展程序都可以对`window.onload`进行赋值，但两者都无法读取对方的事件处理程序。事件处理程序将按照它们被赋值的顺序被调用。

其次，尽管在隔离世界中运行内容脚本可以提供一定的保护，使其免受网页攻击，但如果您不加选择地使用来自网页（或服务）的数据，恶意网页（或通过 XHR API 访问的远程服务）仍可能攻击您的内容脚本。例如，使用`eval`方法解析此数据会使其得以执行，从而使恶意代码能够在您的扩展程序内部执行。为避免这种情况，请*选择不执行代码的更安全（解析）API*，例如`JSON.parse`方法。

### 总结

本章讨论了谷歌 Chrome 扩展程序的其余功能，例如选项页面、覆盖页面和主题。本章描述的`OverridePages`扩展程序覆盖了“新建标签页”。该扩展程序还演示了选项页面的使用。此页面提供了一个单一选项来保存用户对`OverridePages`扩展程序的偏好设置。

`Themes`扩展程序为浏览器显示了一个简单的主题——为窗口提供彩色边框，为“新建标签页”提供背景图片，并为隐身窗口和非活动窗口提供色彩色调。最后，描述了一些安全问题，并讨论了相应的补救措施。所描述的问题包括 API 权限、*匹配模式*权限、`externally_connectable`属性、存储和内容脚本注入。

本章结束了本书。您了解了什么是谷歌 Chrome 扩展程序，学习了如何创建它们，了解了扩展程序的组件和消息传递，以及在 Chrome 网上应用店（前身为谷歌 Chrome 扩展程序画廊）发布扩展程序。所以，现在是时候利用谷歌 Chrome 浏览器的强大功能，创建您自己出色的扩展程序了！

## 索引

### A
- `chrome.runtime.sendMessage` 方法，105
- Alarms API，121–124
- Chrome 扩展程序架构
  - 组件，40–41
  - 输入组件，36–39
  - 清单文件，40–41
  - 弹出窗口，37–39
  - 脚本，36–39
  - 用户视角，35
- `chrome.runtime.connect` 方法，101
- 内容脚本
  - Chrome 扩展程序 API，62
  - 内容 UI，62
  - DOM API，62
  - HelloContentScript 扩展程序，65–68
  - 清单文件，63–64

### B
- 浏览器操作按钮，9, 12
- BrowserActionNotes 扩展程序
  - 浏览器操作组件，69
  - 控制台面板，弹出窗口，73
  - `loadNoteForActiveURL` 函数，72
  - 弹出窗口组件，69–70
  - `popup_script.js`，71–72，74
  - 资源面板，70
  - 设置徽章文本，73
  - `textarea` 元素，72
- 浏览器操作弹出窗口，37

### C
- Chrome 网上应用店，11
- 内容脚本和事件脚本
  - `chrome.runtime.sendMessage` 方法，103
  - `connect` 和 `sendMessage` 方法，103
  - 控制台面板，103，107
  - `content_script.js`，104
  - `event_script.js`，105
  - `extensionID` 参数，105
  - `manifest.json`，106–107
  - 检查弹出窗口，106
  - `runtime.onMessage`，103
  - `runtime.onMessageExternal` 和 `runtime.onConnectExternal` 事件，103
- 内容脚本和网页脚本
  - 书签 API，113–114
  - `button` 元素，112
  - `content_script.js`，113
  - `content_scripts` 属性，111
  - `manifest.json`，112
  - `postMessage` 方法—`window.postMessage`，112
  - `runtime.onMessageExternal` 和 `runtime.onConnectExternal` 事件，111
  - `webpage_script.js`，114
  - `window.addEventListener("message", function(me){/**/})`，112
  - `window.postMessage` 方法，111
  - `window.postMessage(message,"*")`，112
- 上下文菜单项
  - `contextMenus` 权限，85
  - 项目创建
    - `addEventListener` 方法，93
    - `browser_action` 和 `page_action` 上下文，87
    - `chrome.contextMenus.create` 方法，89
    - `chrome.runtime.lastError` 对象，89
    - `documentUrlPatterns` 属性，88
    - `event_script.js`，87–89，93
    - HelloContextMenuItem 扩展程序，88
    - `ID_CONTEXT_MENU_ITEM_HELLO` 变量，87
    - `info` 对象，90
    - `info.menuItemId` 属性，90
    - `onClicked` 事件，89

### E, F
- 事件脚本
  - API 特性，52
  - 后台脚本，55–56
  - 浏览器操作和页面操作组件，51
  - BrowserActionNotes，53
  - `chrome.runtime` 对象，51
  - `chrome.runtime.onMessageExternal` 事件，52
  - 内容脚本，52
  - `declarativeContent` API `addRules`/`removeRules`，60



