# 附录 A：技巧与建议

这个简短的附录提供了一些关于 Node.js 开发的一般性技巧与建议。

## 测试

通过完善的单元测试和集成测试，可以确保新功能或错误修复不会引入回归问题。许多优秀的服务提供商都提供托管的持续集成（CI）环境。它们可以在每个拉取请求合并之前进行测试，这大大简化了代码审查工作。一个流行的托管 CI 服务是 Travis CI。Travis CI 对开源项目是免费的。

## 使用 SemVer 进行语义化版本控制

我建议遵循 SemVer（[`http://semver.org/`](http://semver.org/)）的语义化版本控制规范。SemVer 将版本的版本号分为三个部分：MAJOR.MINOR.PATCH。版本 3.5.8 中，3 是主版本号，5 是次版本号，8 是补丁号。如果新版本包含不兼容的 API 修改，则主版本号递增。新增功能只需递增次版本号，而错误修复则只需递增补丁号。

举个例子：我的包版本是 3.5.7。我添加了一个不影响向后兼容性的新功能。我的下一个版本就会是 3.6.0。

通过这种方式，你的用户就能大致了解某个版本是否会破坏他们的生产代码（主版本）、是否包含新功能（次版本），或者是否包含错误修复（补丁）。semantic-release（[`www.npmjs.com/package/semantic-release`](https://www.npmjs.com/package/semantic-release)）是一个能帮助你为下一个版本号递增做出正确决定的优秀工具。

## Greenkeeper

跟踪项目中哪些依赖项有新版本需要更新可能是一件乏味的事情。更新本身（在 `package.json` 文件中修改版本号）也并非世上最有趣的任务。一项令人兴奋的新服务是 [`http://greenkeeper.io`](http://greenkeeper.io)。一旦你为项目注册了它，它就会向你发送包含依赖项更新版本的拉取请求。如果你的测试套件已就绪并且所有状态都是“绿灯”，那么你只需合并来自 Greenkeeper 机器人的拉取请求即可。

测试、自动运行测试的持续集成服务、SemVer 以及 Greenkeeper 结合起来才能真正发挥它们的优势。

## 索引

A, B
命令行客户端中的 API

C
命令行界面（CLI）
CLI 客户端命令行工具配置
`config-chain` 配置对象创建
编辑 ini 格式文件
文档（参见 `文档`, CLI）
错误处理
`bugs` 属性
命令行客户端
`isonline` 命令
`lib/isonline.js`
不存在的函数
`package.json`
堆栈跟踪
使用错误
验证
Git 项目
辅助函数
JSON 支持与简写
命令列表 `$ lounger`
手册页
`npm publish`
`opener` 模块
原则
`spawn` 命令
持续集成（CI）

D
数据库管理工具
启动
命令行客户端
命令行工具
CouchDB 和 PouchDB
数据库服务器 `curl`
文件监视器错误
PouchDB 数据库服务器
设置
故障排除
生态系统
`isonline` 命令
`isonline.js` 文件
Node.js `package.json` 文件

E
文档
`buildMan()`
`build` 脚本
详细说明
`doc/cli/lounger-isonline.md`
文件夹结构
`fs` 模块
`fs.writeFileSync`
`getSources` 函数
HTML 输出
JavaScript
`lounger.commands`
手册页
`man-pages` 目录
Markdown 文件
Node.js 平台
`npm script`
可选命令
`scripts` 部分
`sources` 对象

F, G
双工流

H
错误处理
`bugs` 属性
命令行客户端
`isonline` 命令
`lib/isonline.js`
不存在的函数
`package.json`
堆栈跟踪
使用错误
验证

I, J, K, L
Greenkeeper

M, N, O
`MyTransformStream`

P, Q, R
直通流

S
高级用户
命令行客户端
配置
退出码
JSON 输出
编写脚本
快捷方式

T
语义化版本

U
流
`cat` 命令
`console.log`
CouchDB/PouchDB 自定义流
`_flush` 方法
`fs` 和 `util` 模块
`import` 命令
`CouchBulkImporter` 创建，数据库
设计 `TransformToBulkDocs`
`MyTransformStream`
`node streams-bulk-example.js`
`node streams-example.js`
`test/fixtures/test.csv`
`transform` 方法
转换流 `TransformToBulkDocs`
可写流

V, W, X, Y, Z
测试驱动开发（TDD）
转换流