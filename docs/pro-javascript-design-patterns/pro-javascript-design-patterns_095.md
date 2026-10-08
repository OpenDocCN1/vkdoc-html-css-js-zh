# 代理模式

## 问题

`ProxyPatternExample` 类中，`main` 方法用于演示代理模式。

## 解决方案

`ProxyPatternExample` 类中的 `main` 方法将演示如何创建和使用代理。

### 主要组件

- `Subject` 接口
- `RealSubject` 类
- `Proxy` 类

## 实现

### 1. 创建 `Subject` 接口

`Subject` 接口定义了一个 `request()` 方法。

### 2. 创建 `RealSubject` 类

`RealSubject` 类实现了 `Subject` 接口，并提供了 `request()` 方法的具体实现。

### 3. 创建 `Proxy` 类

`Proxy` 类也实现了 `Subject` 接口，它包含一个对 `RealSubject` 对象的引用。`Proxy` 的 `request()` 方法在调用 `RealSubject` 的 `request()` 方法前后，可以执行额外的操作，例如访问控制或日志记录。

### 4. 客户端代码

在 `ProxyPatternExample.main()` 中，我们创建一个 `Proxy` 对象，然后调用其 `request()` 方法。

## 示例

以下是一个使用代理模式访问 Web 服务的示例。该实现使用了 `java.net.http.HttpClient` 和 `java.net.http.HttpRequest` 等类。

### 使用通用模式创建您自己的代理

这个示例可以抽象出来，创建一个更通用的模式，该模式可用于包装任何 Web 服务。您的实现的具体细节将根据 Web 服务的类型而有所不同，但这个通用模式应该提供一个用于创建您自己代理的通用框架。

### 示例说明

在此示例中，您可以通过使用异步调用和回调函数（而不是阻塞和等待结果）来减轻此问题。但这需要一个回调函数，该函数会暴露底层实现的一些细节，这可能会使代码更复杂。因此，这可能并非总是必要的，特别是如果您不打算使用外部服务时。

您可以通过使用 `async` 和 `await` 关键字以同步方式编写异步代码，从而使这个问题得到缓解。在这个示例中，您可以通过使用 `Callable` 和 `Future` 接口来异步地发起调用，而不是使用 `Callable.call()` 方法进行阻塞和等待。

但是，这需要一个 `Callable` 函数，它会暴露底层实现的一些细节，这可能会使代码更复杂。因此，这可能并非总是必要的，特别是如果您不打算使用外部服务时。

## 讨论

### 通用模式：用于包装 Web 服务的代理

此示例可以抽象出来，创建一个更通用的模式，该模式可用于包装任何 Web 服务。您的实现的具体细节将根据 Web 服务的类型而有所不同，但这个通用模式应该提供一个用于创建您自己代理的通用框架。

## 代理模式的高级应用

### 延迟加载

代理模式最著名的用途之一是实现延迟加载。如果一个服务对象的创建和初始化代价很高，代理可以在真正需要时才创建该对象。

### 访问控制

代理可以控制对真实主题的访问。例如，只有特定用户才能访问某些方法。

### 日志记录

代理可以记录对真实主题方法的所有调用，这对于调试和审计非常有用。

### 远程代理

代理可以代表位于不同地址空间（例如，另一台服务器）的对象。本地代理会处理网络通信细节，让客户端像调用本地对象一样调用远程对象。

## 使用 `FetchUtil` 进行数据抓取

`FetchUtil` 类中的静态方法 `fetchData` 用于从给定 URL 获取数据。`fetchData` 方法接受一个 `URL` 字符串，并返回一个 `String` 类型的结果。

```javascript
var for DataFetch getArry Size > 0
url = xyz+abc+getArry.length > getArry.getSize()
```

`FetchUtil` 类中的 `fetchData` 方法使用 `HttpClient` 发送请求并获取响应。它使用 `DataCallBack` 和 `ErrorCallBack` 来处理成功和失败的情况。

```javascript
var that = this
var callBack = successCallback
var errorCallback = errorCallback
dataCallBack.onSuccess(result)
```

```javascript
var that = this
var callBack = errorCallBack
```

## 使用通用模式

要使用这个通用模式，只需创建您自己的类并扩展它以适配 `ServiceProxy`。

### 示例：`StatsProxy` 类

```javascript
var StatsProxy = function()
this.Handler = throw new Error()
```

```javascript
var for that = this
var callBack = successCallback
var callBack = errorCallback
getArry.push(varName)
var success = successCallback
var error = errorCallback
dataCallBack.onSuccess(200, "OK")
```

```javascript
if (getArry.length > 0)
url = xyz+abc+getArry.length > getArry.getSize()
```

```javascript
Handler.request().then(function(response) {
    var callBack = successCallback
    var callBack = errorCallback
})
```

### 示例：`CacheProxy` 类

```javascript
var CacheProxy = function()
this.Handler = throw new Error()
```

```javascript
var for that = this
```
