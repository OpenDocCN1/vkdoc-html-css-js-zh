# 目录

## 第 1 章：Backbone.js 简介

- 什么是 `Backbone.js`?
- 使命宣言
- Web 应用的设计模式
- 模型-视图-控制器
- 模型-视图-*
- `Backbone` 如何支持模型-视图-*
- 其他 `JavaScript` 模型-视图-* 框架
- `Backbone` 在现实世界中的应用
- 使用 `Backbone` 的公司
- 在项目中使用 `Backbone` 的五个理由
- `Backbone` 可能不适合你的三个理由
- 配置 `Backbone`
- 下载 `Backbone`
- 测试你的配置：`Backbone` 项目结构
- 开发环境的一些提示
- 小结

## 第 2 章：面向对象编程入门

- `JavaScript` 的崛起
- `JavaScript` 版本
- 执行 `JavaScript` 代码片段
- 究竟什么是对象？
- `JavaScript` 基础
- 原始数据类型
- `JavaScript` 中的核心对象
- 基本语法
- 声明变量
- 循环和条件语句
- 数组
- 闭包
- 实现 `JavaScript` 的面向对象特性
- 在 `JavaScript` 中创建简单对象
- 定义构造函数
- 创建方法
- `JavaScript` 原型
- 封装
- 使用原型的继承
- 控制方法和属性的访问
- 提供命名空间
- `JavaScript` 的面向对象框架
- `PrototypeJS`
- `MooTools`
- `Backbone.js`
- 小结

## 第 3 章：`Backbone` 模型与集合

- 系统设置
- `Backbone` 模型
- 构造函数
- 模型属性
- 为你的模型添加函数
- 模型事件
- 模型验证
- 与服务器交换数据
- 扩展你自己的模型
- `Backbone` 集合
- 构造函数
- 操作集合
- 遍历集合
- 其他实用方法
- 与服务器交换数据
- 集合快速参考
- 小结

## 第 4 章：`Backbone` 视图与模板库

- 代码设置
- `Backbone` 视图
- 创建 `Backbone` 视图
- 绑定到物理视图
- 渲染内容
- 视图事件
- 处理 `self` 和 `this`
- `Backbone` 视图快速参考
- `Backbone` 中的模板化
- 使用 `Underscore` 进行模板化
- 使用 `Handlebars` 进行模板化
- 在 `Backbone` 中使用 `Handlebars`
- `Handlebars` 快速指南
- 提供辅助函数
- 预编译模板
- 使用 `Mustache` 进行模板化
- 在 `Backbone` 中使用 `Mustache`
- `Mustache` 快速指南
- 该选哪个模板库？
- 小结

## 第 5 章：路由器和事件

- `Backbone` 路由器
- 创建路由器
- `Backbone.History`
- 使用 `pushState`
- 为所有应用使用路由
- 为路由添加参数
- 路由事件
- 手动创建路由
- 通过编程进行导航
- 将路由器用作控制器
- `Backbone` 事件
- 绑定事件
- 解绑事件
- 监听一次事件
- 监听其他对象上的事件
- 触发事件
- 内置事件
- 创建自定义事件
- 监听 `DOM` 事件
- 小结

## 第 6 章：从零到一：一个完整的应用示例

- 设计你的应用
- `CSS` 样式
- 应用领域
- 应用代码结构
- 外部依赖
- 整合所有内容
- 第一步：处理认证
- 在 `Twitter` 上设置
- 在服务器上保护密钥
- 显示 `Twitter` 时间线
- 添加个人资料视图
- 个人资料数据层
- 个人资料视图
- 整合所有内容
- 重用个人资料视图
- 使个人资料模型可重用
- 更改时间线上的行为
- 为时间线视图添加事件
- 为个人资料创建弹出对话框
- 添加搜索功能
- 数据层
- 添加搜索视图
- 在 `Backbone` 对象间共享模型
- 错误处理
- 小结

## 第 7 章：`Backbone` 生态系统

- 代码示例
- 用户界面组件
- `LayoutManager`
- `Backgrid`
- `Backbone UI`
- `Backbone.stickit`
- `Backbone.Notifications`
- 模型和集合扩展
  - `Backbone.trackit`
  - `Backbone.memento`
  - `Backbone.localStorage`
  - `Backbone.dualStorage`
  - `Backbone.ViewModel`
  - `Backbone-Validator`
  - `Query Engine`
  - `ModelAttrs`
- 开发者工具和实用程序
- 快速参考
- 小结

## 第 8 章：测试你的 `Backbone.js` 应用

- 测试驱动开发的优势
- 为测试驱动开发提出论据
- 处理遗留代码库
- 测试自动化
- 使用 `QUnit` 进行测试
- `QUnit` 入门
- 一个简单的 `Backbone` 模型测试
- `Sinon.JS`
  - 测试间谍
  - 测试桩和模拟对象
  - 使用虚假服务器
- 使用 `Jasmine` 进行测试
- `Jasmine` 入门
- 期望
- 小结

## 第 9 章：在构建过程中使用 `Grunt`

- `Grunt` 简介
- 选择 `Grunt` 的几个理由
- 安装 `Grunt`
- 将 `Grunt` 添加到现有项目
- 任务配置
- 处理文件
- 为你的项目使用 `Grunt`
- 添加静态分析工具
- 压缩 `JavaScript` 源代码
- 压缩 `CSS`
- 自动化测试套件
- 创建不同的任务集
- 项目脚手架
- 一些有用的 `Grunt` 插件
- 小结

## 第 10 章：使用 `Marionette` 和 `Thorax` 扩展 `Backbone`

- `Marionette`
- `Marionette` 入门
- `Marionette` 功能集
- 一个简单的 `Marionette` 应用
- `Thorax`
- `Thorax` 功能集
- `Thorax` 入门
- `Thorax` 类型简介
- 在 `Twitter` 示例中使用 `Thorax`
- 小结

# 第 11 章：Backbone 最佳实践

## 通用最佳实践

### 保持清晰的责任分离

### 为你的应用添加错误处理模式

### 避免内存泄漏

#### 关闭视图时解绑事件

#### 使用 `listenTo` 进行事件处理

#### 使用一种模式来正确关闭视图

### 渲染视图

#### 只渲染你需要的内容

#### 使用部分模板减少模板膨胀

#### 在 `Handlebars` 中预编译模板

### 视图管理

#### 在视图间共享公共代码

#### 从子视图更新父视图

### 网络性能

#### 最小化页面加载时的请求

#### 感知性能

#### 乐观的网络调用

#### 使用 `Document Fragment` 渲染集合

#### 使用模型存储额外数据

#### 尽可能缓存对象

### 小结

# 第 12 章：创建可维护的 JavaScript 代码库

## 使用 `RequireJS` 创建模块化应用

### 什么是 `RequireJS`?

### 使用 `RequireJS`

### 定义和使用模块

### 在 `Backbone` 应用中引入 `RequireJS`

## 使用 `Yeoman` 快速入门

### 安装 `Yeoman`

### 使用 `Backbone` 生成器

## `Backbone` 应用的设计模式

### 利用门面模式

### 利用中介者模式

### 小结