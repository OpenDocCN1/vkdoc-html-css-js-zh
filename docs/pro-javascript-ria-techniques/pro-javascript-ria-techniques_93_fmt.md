# 前言

- 目录
- 内容速览
- 详细目录
- 关于作者
- 关于技术审校
- 致谢
- 引言

# 构建坚实基础

## 最佳实践概述

### 谁定义了“最佳”实践？

### 最佳实践惠及谁？

- 网页开发者
- 搜索引擎及其他自动化系统
- 最终用户

## 通用最佳实践

### 明确项目目标

### 掌握基本规则

- 遵循成熟、开放且得到良好支持的 Web 标准
- 处理跨浏览器问题
- 默认仅支持 HTML
- 统一文件夹和文件的命名与分组方式
- 保持代码库整洁
- 为性能而设计代码
- 不要为技术而技术

## 标记最佳实践：语义化 HTML

### 学习 HTML 标签

### 从文档类型定义开始

- DOCTYPE 切换
- DTD 选择
- DTD 验证

### 如何在 XHTML 中融入 X？

- 结构良好的文档
- 元素禁用

### 将最佳实践付诸行动

- 编写整洁有序的代码
- 减少注释数量
- 为 IE 使用条件注释
- 正确设置`<html>`标签
- 指定内容类型
- 设置页面标题
- 分离表现层、内容与行为
- 为整个文档添加包装元素
- 帮助 CSS 和 JavaScript 定位特定页面
- 统一命名 ID 和 class 属性
- 正确排列内容顺序
- 区分前景图像与背景图像
- 合理使用表格
- 优化表单
- 避免使用框架

### Web 内容的无障碍指南

- 不要被访问键误导
- 不要被 Tab 索引误导
- 不要依赖插件
- 尽可能添加额外语义

## 格式最佳实践：CSS

### 关于像素级精确还原设计稿

### W3C CSS 标准

# 样式表指南

- 分离通用样式规则
- 理解层叠与特异性
- 考虑打印样式
- 格式化样式规则
- 为单个页面元素应用多个类名
- 重置浏览器默认样式
- 掌握简写样式规则

## 样式的无障碍指南

- 对支持 CSS 的浏览器隐藏内容
- 移动内容块以保持正确的标记源顺序
- 使用相对字体大小

## 注释块

## 浏览器兼容处理

## 本地化注意事项

## 组织文件夹、文件与资源

### 可读的 URL

### 文件与文件夹命名

### 文件编码

### 资源管理

- 图像指南
- 多媒体指南

## 搭建开发环境

### 编写文件：集成开发环境

### 存储文件：版本控制系统

### 测试页面：浏览器与开发工具

## 总结

# 面向富互联网应用的 JavaScript

## 编码风格指南

- 使用一致的格式
- 使用花括号和方括号
- 通过大小写增加语义
- 使用描述性的变量和函数名称
- 保持函数块简短
- 使用 ScriptDoc 将注释作为文档
- 用 TODO 标记未完成的任务

## 专业 JavaScript 编程

- 避免解决不存在的问题
- 使用文档对象模型
    - 动态修改 CSS 属性
    - 操作元素及其内容
- 不要混用 JavaScript 和 HTML
- 将样式与代码分离
- 链式调用函数
- 编写健壮的代码
    - 优雅处理致命异常错误
    - 定义自定义异常
    - 以及 finally...
- 编码时考虑本地化

## 面向对象的 JavaScript

### 对象、类与构造函数

- 什么是对象？
- 什么是类？
- 对象作为类的实例
- 构造函数
    - `properties`——属性
    - `Methods`——方法
    - `the prototype` Keyword——`prototype`关键字
    - `Singletons`——单例
- `Inheritance`: Creating New Classes from Existing Ones——`继承`：从现有类创建新类
    - `encapsulation`: Each Class Doing What It Does Best——`封装`：各司其职
    - `polymorphism`: Redefining Inherited Properties and Methods——`多态`：重定义继承而来的属性和方法
- `The this` Keyword——`this`关键字
- `Access to Properties and Methods`——访问属性和方法
- `Object Literals and JavaScript Object Notation`——对象字面量与 JavaScript 对象表示法
    - `Using Object Literals When Creating Classes`——创建类时使用对象字面量
    - `Using Object Literals as Inputs to Functions`——将对象字面量用作函数的输入
- `Creating Namespaces and Hierarchies`——创建命名空间与层级结构
- `Libraries and Frameworks`——库与框架
    - `Selecting a Library`——选择库
    - `Building a JavaScript Library`——构建 JavaScript 库
        - `Detecting When the DOM Is Available`——检测 DOM 何时可用
        - `Handling Events in the Browser`——处理浏览器中的事件
        - `Loading Content on Demand with Ajax`——使用 Ajax 按需加载内容
        - `Using Utility Functions`——使用实用函数
        - `Handling CSS and Styles`——处理 CSS 与样式
        - `Locating Elements Within the Page`——定位页面内的元素
        - `Completing the Library`——完成库的构建
- `Building RIAs`——构建 RIA
    - `Structuring the Application`——应用程序的结构化
    - `Managing Two Sets of HTML`——管理两套 HTML
    - `Using Design Patterns`——使用设计模式
        - `The Model-View-Controller Pattern`——模型-视图-控制器模式
        - `The Observer Pattern`——观察者模式
- `Testing and Test-Driven Development`——测试与测试驱动开发
- `Using Third-Party Scripts`——使用第三方脚本
- `Summary`——总结
- `Understanding the Web Browser`——理解 Web 浏览器
    - `Engines: The Browser’s Powerhouse`——引擎：浏览器的心脏
        - `The Rendering and JavaScript Engines`——渲染引擎与 JavaScript 引擎
        - `JavaScript Engine Performance Benchmarking`——JavaScript 引擎性能基准测试
    - `Anatomy of a Web Page Request`——网页请求剖析
        - `HTTP: The Communication Standard Behind the Web`——HTTP：Web 背后的通信标准
            - `An HTTP Request Message`——HTTP 请求消息
            - `An HTTP Response Message`——HTTP 响应消息
        - `HTTP Status Codes`——HTTP 状态码
            - `200+ (Success)`——200+（成功）
            - `300+ (Redirection)`——300+（重定向）
            - `400+ (Client Error)`——400+（客户端错误）
            - `500+ (Server Error)`——500+（服务器错误）
        - `How Messages Are Transmitted`——消息如何传输

# Page Performance

## Viewing the Performance of a Page

## Identifying Potential Bottlenecks in Performance

### `Engine Horsepower`

### `User’s Upload Speeds`

## `Parallel Connection Limits`

### `Choice of Image Format`

### `Unnecessary Content`

### `Cache Settings`

### `Uncompressed Text Files and Extra Whitespace`

## `Summary`

## `Performance Tweaking`

### `Is Performance Really an Issue?`

### `Tweaking Your Web Server for Performance`

#### `Use Separate Domain Names for External Assets`

#### `Use a Content Delivery Network`

#### `Send HTML to the Browser in Chunks`

##### `Flushing the Buffer with PHP`

##### `Flushing the Buffer with ASP.Net/C#`

##### `Flushing the Buffer with JSP`

#### `Customize HTTP Headers to Force Browser Caching`

#### `Compress the Output from the Server`

##### `Enabling Compression in Apache Web Server`

##### `Enabling Compression in IIS`

##### `Enabling Compression in Tomcat`

### `Tweaking HTML for Performance`

#### `Shrink Your HTML File Size with HTML Tidy`

#### `Reference JavaScript Files at the End of Your HTML`

#### `Reduce the Number of HTTP Requests`

##### `Good Division and Structure`

##### `Combine Files`

#### `Don’t Load Every Asset from Your Home Page`

#### `Reduce Domain Name Lookups`

#### `Split Components Across Domains`

#### `Avoid Linking to Redirects`

#### `Reduce the Number of HTML Elements`

#### `Don’t Link to Nonexistent Files`

#### `Reduce the Size of HTTP Cookies`

### `Tweaking Your Style Sheets for Performance`

#### `Shrink Your CSS File Size with CSSTidy`

#### `Don’t Use the @import Command`

#### `Speed Up Table Layouts`

#### `Avoid CSS Filters and Expressions in IE`

#### `Use Shorthand Values`

##### `Colors`

##### `Margins and Padding`

##### `Borders`

##### `Backgrounds`

##### `Fonts`

##### `Lists`

#### `Use the CSS Sprite Technique`

#### `Avoid Inefficient CSS Selectors`

### `Tweaking Your Images for Performance`

#### `Understand Image File Formats`

##### `GIF Format`

##### `JPEG Format`

##### `PNG Format`

#### `Optimize PNG Images`

#### `Don’t Forget the Favicon`

### `Tweaking Your JavaScript for Performance`

#### `Shrink Your JavaScript File Using Dojo ShrinkSafe`