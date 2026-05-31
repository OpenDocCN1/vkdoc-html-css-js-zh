# 文档大纲

- `Page-Action` 组件，94
- `chrome.declarativeContent` 及其属性，90
- `chrome.declarativeWeb`
- `runtime.sendMessage` 方法，93
- `Request`，59
- `TYPES_CONTEXT` 变量，87
- `chrome.declarativeContent`
- `manifest.json`，85
- `ShowPageAction`，60
- `ShowPageAction`，61–62

### D

- 事件处理器，声明式的，57
- 事件对象，56
- 调试 Chrome 扩展程序
- `HelloPageAction` 扩展，58–59
- Chrome DevTools 窗口，18
- 输入组件 API，51
- 控制台面板，21–22
- 清单文件
- 元素面板，19
- `background` 属性，53
- 检查弹出窗口，18
- 扩展程序管理
- 源代码和资源面板，19–21
- 页面，54
- `Disposition`，84
- `HelloShortcutKey`，后台
- 分发扩展程序，Chrome 网上应用店
- 页面，53，55
- 控制台，24
- 脚本和持久化键，53
- 开发者注册费，24
- 多个组件，52
- 草稿
- `onClicked` 事件，51
- 编辑，32
- `onCommand` 事件，57
- 保存，32
- 脚本和持久化
- Google 钱包，27
- 属性，57
- 添加图标，29
- 共享代码，53
- 商品列表，33
- 扩展程序运行时，40
- 日志记录，Google 账户，23
- 扩展程序
- 添加新扩展，28
- 创建
- 浏览器操作，12
- 支付网关，26
- `console.log` 方法，14
- 支付方式，25
- 扩展按钮，11
- 完成购买，27
- 扩展按钮，11
- 审查购买，26
- `getMonth` 方法，13
- 48px 图标，29
- JavaScript 代码，当前时间，13
- `ShowTime` 扩展，22
- JSON 文件验证，11
- 上传
- 清单文件，12
- `ShowTime` 压缩包，30–31
- 弹出窗口，10
- 压缩包，28
- `setTimeAndDate` 函数，13–14
- 文档对象模型（DOM），9，117
- `ShowTime`，9–10
- `DOMContentLoaded` 事件，14
- `ShowTime/popup.html`，14–15

## 索引

#### 调试（参见 调试 Chrome 扩展程序）

- HTML 和 CSS，8
- JavaScript 和 JSON，8
- 加载，扩展文件夹，15–17

#### 书签 API

- 关联事件，128
- `bookmarks.BookmarkTreeNode` 对象，124
- `chrome.bookmarks.create` 方法，125
- `chrome.bookmarks.getTree` 方法，127
- `chrome.bookmarks.search` 方法，127
- 创建，125–126
- 层级结构，127–128
- 通知 API，124
- 更新，126

#### 覆盖页面

- 书签管理器，149
- `children.length` 属性，153
- CSS 和 JavaScript 代码，149
- 历史记录，149
- 清单文件，151
- `myNewTab_1.js`，151–154
- `myNewTab.html`，150
- 新标签页，书签，149，155
- 选项页面，154
- 声明权限，118

#### 下载 API

- 关联事件，131
- 取消/恢复，131
- `chrome.downloads.download` 方法，129
- `chrome.downloads.removeFile` 方法，131
- `chrome.runtime.lastError`，131
- `event_script.js`，128–129
- 文件下载，129–130
- `open` 方法，131

#### 扩展程序 API

- 8–9
- 扩展程序开发，安全性
- API 权限，160
- 内容脚本，161
- `externally_connectable` 属性，160
- 匹配模式权限，160
- 存储，160

#### 扩展程序管理页面

- 3，17

### G

- Google Chrome 应用，2
- Google Chrome 扩展程序
- 浏览器扩展，支持，1
- 浏览器插件，2
- Chrome 网上应用店，5
- 定义，1
- 从商店添加扩展，5–7
- 扩展程序管理页面，3
- 固定，`chrome://extensions` 标签页，3
- 插件，3–4
- 沙盒机制，4
- 技术，8
- URLs，7

#### Google Chrome 扩展程序 API

- `alarms` 权限，119，121
- alarms API
- `alarmName` 和 `alarmInfo`，122
- `chrome.alarms.clearAll` 方法，124
- `chrome.alarms.create` 方法，122
- `event_script.js`，123
- 通知 API，121–122
- 可选权限，121
- `periodInMinutes`，122
- 必需权限，119–120

#### 历史记录 API

- 关联事件，134
- 回调函数，133
- `chrome.history.search` 方法，133
- `event_script.js`，132
- 查询对象
- 属性，133
- URL，添加和移除，133

#### 输入组件

- `chrome.commands`，117
- `chrome.contextMenus`，117
- `chrome.omnibox`，117
- `chrome.pageAction` 和 `chrome.browserAction`，117

#### 通知 API

- 关联事件，137
- `chrome.notifications.create` 方法，136
- `chrome.notifications.update` 方法，137
- `event_script.js`，134–136

### Google Chrome 扩展程序 API（续）

- 多功能框输入（参见 多功能框输入）
- `onCommand` 事件，49
- 单独操作，43
- 快捷键/命令
- 更改扩展图标，47
- `_execute_browser_action` 和 `_execute_page_action`，48
- `HelloBrowserAction` 扩展，48
- `HelloShortcutKey` 扩展，49

#### 存储 API

- `chrome.storage.onChanged` 事件，139
- `chrome.storage.sync.remove` 方法，139
- `chrome.storage.sync.set` 和 `chrome.storage.sync.get`，139
- `event_script.js`，138
- `localStorage` API，137
- 设置和获取数据项，139
- 同步存储 vs. 本地存储，137–138

#### 标签页 API

- 关联事件，142
- 捕获，142
- `chrome.tabs.captureVisibleTab` 方法，142
- `chrome.tabs.create` 方法，141
- `chrome.tabs.update` 方法，141
- 创建和移除，141
- `event_script.js`，140–141
- `insertCSS` 方法，139
- `manifest.json`，141–142
- XHR API，118，142–143

### H

- 色相-饱和度-明度（HSL），159

### I，J，K，L

- 输入组件
- 浏览器操作和页面操作，42
- `BrowserActionNotes`，46
- 右键菜单项（参见 右键菜单项）
- `HelloContentUI` 扩展
- `activeTab` 权限，91
- `content_script.js`，92–93
- 注入的内容脚本组件，92
- `manifest.json`，91
- 交互功能，42
- 清单文件
- 浏览器操作，43–44，46
- 默认扩展图标，45
- 页面操作，43–45
- 设置标题、图标和弹出窗口，45–46

#### 消息传递 API

- 和事件，95–96
- CS 和 ES 扩展（参见 内容脚本和事件脚本）
- CS 和 WS 扩展（参见 内容脚本和网页脚本）
- 扩展框架，95
- 弹出脚本和事件脚本，115–116
- PS 和 CS 扩展（参见 弹出脚本和内容脚本）
- 网页脚本和事件脚本
- `chrome.runtime.sendMessage` 方法，96
- 控制台面板，97–98
- `event_script.js`，99
- 监听事件，98，100
- 长连接，101–102
- 清单文件的作用，100
- 参数，96
- `responseCallback` 参数，98
- `send_message`，96
- `webpage_script.js`，97–98

### M，N

- `manifest_version` 属性，12
- 匹配模式权限，118，144，160

### O

#### 多功能框输入

- `chrome.omnibox.onInputChanged`，82
- `chrome.omnibox.onInputEntered`，82
- 事件脚本组件，79，81–82，84
- 扩展程序管理页面，80
- `HelloOmniboxInput`
- 后台页面，84
- `event_script.js`，81–82
- 交互，多功能框，81，83
- `manifest.json`，81
- 设置默认建议，84
- 建议结果，82
- `icons` 属性，80
- `keyword` 属性，80
- 16px 图标，80

#### `OnConnectExternal` 事件

- 102

#### 选项页面，扩展程序

- HTML 页面，146
- 清单文件，145–146
- `myOptionsPage_1.js`，148–149
- `myOptionsPage.html`，147–148
- `OverridePages` 扩展，146
- 覆盖提示，147

#### `OverridePages` 扩展

- 151–153

### P，Q

- `PageActionNotes` 扩展
- 后台页面，75
- 控制台面板，弹出窗口，76
- `declarativeContent` API，74
- 事件脚本组件，74
- 弹出窗口组件，75–76
- `stackoverflow.com` 主机，74，77
- 文本区域，77
- 页面操作弹出窗口，38

#### 弹出脚本和内容脚本

- `alarms` API，111
- `chrome.runtime.onMessage` 事件，110
- `chrome.tabs.query` 方法，108
- `chrome.tabs.sendMessage` 方法，108
- `content_script.js`，110
- 长连接，110
- `manifest.json`，108
- 参数，108
- `popup_script.js`，109
- PS 和 ES 扩展，109
- `responseCallback`，108

#### 弹出脚本和事件脚本

- `background` 属性，115
- `chrome.runtime.onMessage` 事件，117
- `chrome.runtime.sendMessage(message, responseCallback)`，116
- downloads API，116
- `event_script.js`，116
- `manifest.json`，115
- `popup_script.js`，115
- `sendResponse` 回调，117
- `port.onMessage` 事件，102

### R

- `Run HelloOmniboxInput` 命令，85

### S

- 沙盒机制，4
- `ShowTime`，9

### T, U, V, W

#### 主题，Google Chrome

- 颜色和色调元素，157
- 颜色色调，非活动窗口，159
- 图片，156
- 无痕模式，159
- 清单文件，155
- 新标签页，应用主题，158
- 属性，157

### X, Y, Z

- XHR API，118，142–143



*   概览
*   目录
*   关于作者
*   关于技术审校者
*   致谢
*   引言
*   第 1 章：Google Chrome 扩展程序简介
    *   什么是 Google Chrome 扩展程序？
        *   对浏览器扩展的支持
        *   扩展程序不是插件
        *   扩展程序与插件
    *   著名的扩展程序示例
    *   从商店添加扩展程序
    *   创建扩展程序的技术
        *   技术如何被使用？
    *   扩展程序 API
    *   创建你的第一个扩展程序
        *   验证 JSON 文件
        *   创建清单文件
        *   添加按钮：浏览器操作
    *   加载扩展程序文件夹
    *   调试扩展程序
        *   检查弹出窗口
        *   源代码与资源面板
        *   控制台面板
    *   在商店中分发
    *   总结
*   第 2 章：架构概述
    *   什么是用户视角？
    *   创建 Chrome 扩展程序所涉及的组件
    *   扩展运行时
        *   代表运行时的脚本
    *   输入组件：第一部分
        *   浏览器操作和页面操作组件
            *   需要独立操作的原因
            *   此组件中清单文件的作用
                *   定义组件
            *   组件的使用
        *   快捷键或命令
            *   `onCommand` 事件
    *   事件脚本
        *   事件脚本的必要性
        *   此组件中清单文件的作用
            *   后台脚本
        *   事件对象
        *   重新审视 `onCommand` 事件
        *   声明式事件处理器
            *   `HelloPageAction` 扩展程序
            *   `declarativeContent` API
                *   添加和移除规则
                *   使用 `ShowPageAction`
    *   内容脚本
        *   此组件中清单文件的作用
        *   `HelloContentScript` 扩展程序
    *   组件示例
        *   `BrowserActionNotes` 扩展程序
        *   `PageActionNotes` 扩展程序
    *   总结
*   第 3 章：API 可用性与消息传递
    *   输入组件：第二部分
        *   多功能框输入
    *   此组件中事件脚本的作用
        *   上下文菜单项
            *   创建菜单项
        *   重新审视内容界面
            *   `HelloContentUI` 扩展程序
    *   用于通信的消息传递
        *   API 和事件
        *   网页脚本与事件脚本
            *   监听事件
            *   此 API 中清单文件的作用
            *   使用长连接
        *   内容脚本与事件脚本
            *   弹出脚本和内容脚本
                *   使用长连接
            *   内容脚本与网页脚本
        *   弹出脚本与事件脚本
    *   Google Chrome 扩展程序 API
        *   声明权限
            *   权限示例
            *   需要权限的 API
            *   可选权限
        *   警报 API
        *   书签 API
            *   创建书签
            *   更新书签
            *   搜索书签
            *   使用书签层级结构
            *   相关事件
        *   下载 API
            *   下载文件
            *   取消或恢复下载
            *   打开下载项
            *   删除下载项
            *   相关事件
        *   历史记录 API
            *   添加和移除 URL
            *   相关事件
        *   通知 API
            *   创建和清除通知
            *   更新通知
            *   相关事件
        *   存储 API
            *   同步存储与本地存储
            *   设置和获取项目
            *   移除项目
            *   相关事件
        *   标签页 API
            *   创建和移除标签页
            *   更新标签页
            *   捕获标签页
            *   相关事件
        *   XHR API
    *   总结
*   第 4 章：更多关于扩展程序的内容
    *   提供选项页面
        *   此组件中清单文件的作用
        *   编写选项页面
    *   使用覆盖页面
        *   此组件中清单文件的作用




- `OverridePages 扩展`
  - `为谷歌浏览器创建主题`
  - `扩展开发：安全考量`
    - `API 权限`
    - `匹配模式权限`
    - `外部可连接性`
    - `存储`
    - `内容脚本`
  - `总结`
- `索引`

