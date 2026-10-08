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
