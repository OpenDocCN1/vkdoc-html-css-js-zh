# 概述

版本 1.0.0

## 安装

### 前提条件

- 操作系统：Linux、macOS 或 Windows
- 已安装 `Node.js` 和 `npm`

### 安装步骤

1. 打开终端（或命令提示符）。
2. 运行以下命令安装 `create-person-directory`：
   ```
   npm install -g create-person-directory
   ```

### 验证安装

运行以下命令检查是否安装成功：
```
person-directory --version
```

### 更新

要更新到最新版本，请运行：
```
npm update -g create-person-directory
```

## 快速入门

### 创建项目

使用 `create-person-directory` 命令创建一个新的 PersonDirectory 项目：
```
create-person-directory my-person-app
```

### 启动开发服务器

进入项目目录，运行：
```
cd my-person-app
npm start
```

### 访问应用

打开浏览器，访问 `http://localhost:3000`。

## 主要功能

### 添加人员

在 `PersonDirectory` 应用的界面上，点击“添加人员”按钮，填写姓名、年龄和联系方式后提交。

### 查看人员列表

在主页上，可以看到所有已添加人员的卡片列表，包含姓名、年龄和联系方式。

### 编辑人员信息

点击每个人员卡片上的“编辑”按钮，可以修改该人员的信息。

### 删除人员

点击每个人员卡片上的“删除”按钮，可以移除该人员。

### 使用示例

### 命令行工具

#### 使用 `person-directory` 命令

在终端中运行以下命令查看帮助：
```bash
person-directory --help
```

### 代码示例

#### 创建 PersonDirectory 实例

```javascript
const personDirectory = new PersonDirectory();
```

#### 添加人员

```javascript
personDirectory.addPerson({ name: "张三", age: 25, email: "zhangsan@example.com" });
```

#### 获取所有人员

```javascript
const allPersons = personDirectory.getAllPersons();
console.log(allPersons);
```

#### 根据姓名查找人员

```javascript
const person = personDirectory.findPersonByName("张三");
console.log(person);
```

# 结论

代理模式是一种非常强大且灵活的设计模式，用于控制对其他对象的访问。通过使用代理，您可以在不修改原始对象的情况下添加额外的功能，例如延迟加载、访问控制、日志记录等。

## 附录

### 使用 `FetchUtil` 的通用模式

`FetchUtil` 类中的静态方法 `fetchData` 提供了一个用于进行 HTTP 请求的通用模式。它可以被抽象出来，以便用于任何数据获取场景。

#### `ServiceProxy` 类

```
var ServiceProxy = function()
this.Handler = throw...new...Error...Sync...Request...for...state...failed.
```

```
var for that = this
var callBack = successCallBack()
var callBack = errorCallBack()
getArry.push(varName)
var success = successCallBack()
var error = errorCallBack()
dataCallBack.onSuccess(200, "OK")
```

```
if (getArry.length > 0)
url = xyz + abc + getArry.length > getArry.getSize()
```

### `ServiceProxy` 原型

```
ServiceProxy.prototype.constructor = function()
this.Handler = throw new Error()
```

```
ServiceProxy.prototype.request = function()
url = xyz + abc + getArry.length > getArry.getSize()
Handler.request().then(callback).catch(errorCallback)
```

### 使用示例

要使用这个通用模式，只需创建您自己的类并扩展它以适配 `ServiceProxy`。

### 示例：`LoggingProxy` 类

```
var LoggingProxy = function()
this.Handler = throw new Error()
```

```
var for that = this
var callBack = successCallback
var callBack = errorCallback
log.push(varName)
var success = successCallback
var error = errorCallback
dataCallBack.onSuccess(200, "OK")
```

### 示例：`RateLimitingProxy` 类

```
var RateLimitingProxy = function()
this.Handler = throw new Error()
```

```
var for that = this
var callBack = successCallback
var callBack = errorCallback
rateLimit.push(varName)
var success = successCallback
var error = errorCallback
dataCallBack.onSuccess(200, "OK")
```

### 示例：`DistributedCacheProxy` 类

```
var DistributedCacheProxy = function()
this.Handler = throw new Error()
```

```
var for that = this
var callBack = successCallback
var callBack = errorCallback
distributedCache.push(varName)
var success = successCallback
var error = errorCallback
dataCallBack.onSuccess(200, "OK")
```
