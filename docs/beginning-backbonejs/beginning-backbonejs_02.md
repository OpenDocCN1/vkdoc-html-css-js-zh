# 目录

1. 第 1 章：Backbone.js 简介 1
   - 什么是 Backbone.js? 1
   - 使命宣言 1
   - Web 应用的设计模式 3
   - 模型-视图-控制器 4
   - 模型-视图-* 5
   - Backbone 如何支持模型-视图-* 6
   - 其他 JavaScript 模型-视图-* 框架 7
   - Backbone 在现实世界中的应用 11
   - 使用 Backbone 的公司 11
   - 在项目中使用 Backbone 的五个理由 14
   - Backbone 可能不适合你的三个理由 15
   - 配置 Backbone 16
   - 下载 Backbone 16
   - 测试你的配置：Backbone 项目结构 18
   - 开发环境的一些提示 19
   - 小结 20

2. 第 2 章：面向对象编程入门 21
   - JavaScript 的崛起 21
   - JavaScript 版本 21
   - 执行 JavaScript 代码片段 22
   - 究竟什么是对象？ 22
   - JavaScript 基础 22
   - 原始数据类型 22
   - JavaScript 中的核心对象 24
   - 基本语法 26
   - 声明变量 26
   - 循环和条件语句 27
   - 数组 27
   - 闭包 29
   - 实现 JavaScript 的面向对象特性 29
   - 在 JavaScript 中创建简单对象 30
   - 定义构造函数 30
   - 创建方法 30
   - JavaScript 原型 31
   - 封装 32
   - 使用原型的继承 33
   - 控制方法和属性的访问 36
   - 提供命名空间 36
   - JavaScript 的面向对象框架 37
   - PrototypeJS 37
   - MooTools 38
   - Backbone.js 38
   - 小结 38

3. 第 3 章：Backbone 模型与集合 39
   - 系统设置 39
   - Backbone 模型 39
   - 构造函数 40
   - 模型属性 41
   - 为你的模型添加函数 43
   - 模型事件 43
   - 模型验证 45
   - 与服务器交换数据 47
   - 扩展你自己的模型 52
   - Backbone 集合 53
   - 构造函数 54
   - 操作集合 54
   - 遍历集合 56
   - 其他实用方法 57
   - 与服务器交换数据 59
   - 集合快速参考 61
   - 小结 62

4. 第 4 章：Backbone 视图与模板库 63
   - 代码设置 63
   - Backbone 视图 64
   - 创建 Backbone 视图 64
   - 绑定到物理视图 65
   - 渲染内容 66
   - 视图事件 68
   - 处理 self 和 this 69
   - Backbone 视图快速参考 69
   - Backbone 中的模板化 70
   - 使用 Underscore 进行模板化 70
   - 使用 Handlebars 进行模板化 72
   - 在 Backbone 中使用 Handlebars 73
   - Handlebars 快速指南 74
   - 提供辅助函数 76
   - 预编译模板 77
   - 使用 Mustache 进行模板化 77
   - 在 Backbone 中使用 Mustache 77
   - Mustache 快速指南 78
   - 该选哪个模板库？ 79
   - 小结 79

5. 第 5 章：路由器和事件 81
   - Backbone 路由器 81
   - 创建路由器 81
   - Backbone.History 82
   - 使用 pushState 82
   - 为所有应用使用路由 83
   - 为路由添加参数 84
   - 路由事件 85
   - 手动创建路由 85
   - 通过编程进行导航 86
   - 将路由器用作控制器 87
   - Backbone 事件 88
   - 绑定事件 88
   - 解绑事件 89
   - 监听一次事件 90
   - 监听其他对象上的事件 90
   - 触发事件 91
   - 内置事件 91
   - 创建自定义事件 93
   - 监听 DOM 事件 94
   - 小结 94

6. 第 6 章：从零到一：一个完整的应用示例 95
   - 设计你的应用 95
   - CSS 样式 96
   - 应用领域 96
   - 应用代码结构 97
   - 外部依赖 97
   - 整合所有内容 98
   - 第一步：处理认证 99
   - 在 Twitter 上设置 99
   - 在服务器上保护密钥 100
   - 显示 Twitter 时间线 102
   - 添加个人资料视图 110
   - 个人资料数据层 110
   - 个人资料视图 111
   - 整合所有内容 113
   - 重用个人资料视图 114
   - 使个人资料模型可重用 114
   - 更改时间线上的行为 115
   - 为时间线视图添加事件 116
   - 为个人资料创建弹出对话框 116
   - 添加搜索功能 118
   - 数据层 118
   - 添加搜索视图 119
   - 在 Backbone 对象间共享模型 121
   - 错误处理 124
   - 小结 125

7. 第 7 章：Backbone 生态系统 127
   - 代码示例 127
   - 用户界面组件 127
   - LayoutManager 127
   - Backgrid 130
   - Backbone UI 132
   - Backbone.stickit 134
   - Backbone.Notifications 135
   - 模型和集合扩展 137
     - Backbone.trackit 137
     - Backbone.memento 138
     - Backbone.localStorage 139
     - Backbone.dualStorage 141
     - Backbone.ViewModel 142
     - Backbone-Validator 144
     - Query Engine 145
     - ModelAttrs 147
   - 开发者工具和实用程序 148
   - 快速参考 149
   - 小结 149

8. 第 8 章：测试你的 Backbone.js 应用 151
   - 测试驱动开发的优势 151
   - 为测试驱动开发提出论据 152
   - 处理遗留代码库 154
   - 测试自动化 154
   - 使用 QUnit 进行测试 154
   - QUnit 入门 155
   - 一个简单的 Backbone 模型测试 164
   - Sinon.JS 167
     - 测试间谍 167
     - 测试桩和模拟对象 168
     - 使用虚假服务器 169
   - 使用 Jasmine 进行测试 170
   - Jasmine 入门 171
   - 期望 172
   - 小结 178

9. 第 9 章：在构建过程中使用 Grunt 179
   - Grunt 简介 179
   - 选择 Grunt 的几个理由 180
   - 安装 Grunt 180
   - 将 Grunt 添加到现有项目 181
   - 任务配置 184
   - 处理文件 185
   - 为你的项目使用 Grunt 187
   - 添加静态分析工具 187
   - 压缩 JavaScript 源代码 190
   - 压缩 CSS 194
   - 自动化测试套件 195
   - 创建不同的任务集 198
   - 项目脚手架 199
   - 一些有用的 Grunt 插件 201
   - 小结 201

10. 第 10 章：使用 Marionette 和 Thorax 扩展 Backbone 203
    - Marionette 203
    - Marionette 入门 204
    - Marionette 功能集 204
    - 一个简单的 Marionette 应用 205
    - Thorax 218
    - Thorax 功能集 219
    - Thorax 入门 220
    - Thorax 类型简介 221
    - 在 Twitter 示例中使用 Thorax 225
    - 小结 229

11. 第 11 章：Backbone 最佳实践 231
    - 通用最佳实践 231
    - 保持清晰的责任分离 231
    - 为你的应用添加错误处理模式 231
    - 避免内存泄漏 233
    - 关闭视图时解绑事件 233
    - 使用 listenTo 进行事件处理 234
    - 使用一种模式来正确关闭视图 234
    - 渲染视图 236
    - 只渲染你需要的内容 236
    - 使用部分模板减少模板膨胀 238
    - 在 Handlebars 中预编译模板 240
    - 视图管理 242
    - 在视图间共享公共代码 242
    - 从子视图更新父视图 242
    - 网络性能 243
    - 最小化页面加载时的请求 244
    - 感知性能 244
    - 乐观的网络调用 244
    - 使用 Document Fragment 渲染集合 245
    - 使用模型存储额外数据 246
    - 尽可能缓存对象 247
    - 小结 248

12. 第 12 章：创建可维护的 JavaScript 代码库 249
    - 使用 RequireJS 创建模块化应用 249
    - 什么是 RequireJS? 249
    - 使用 RequireJS 250
    - 定义和使用模块 252
    - 在 Backbone 应用中引入 RequireJS 254
    - 使用 Yeoman 快速入门 261
    - 安装 Yeoman 262
    - 使用 Backbone 生成器 263
    - Backbone 应用的设计模式 265
    - 利用门面模式 265
    - 利用中介者模式 267
    - 小结 269

