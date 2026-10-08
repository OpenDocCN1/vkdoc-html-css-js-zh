## 附录 A

## AJAX

在传统 Web 应用（有时称为旧式方法）中，页面渲染后用户进行交互（如登录、输入搜索条件或选择选项），信息随后回传至服务器，新页面返回并在浏览器中渲染。这必然需要服务器往返通信以及下载和渲染新页面。但还有另一种响应更迅速的方式：使用第[18](http://dx.doi.org/10.1007/978-1-4842-2463-2_18)章介绍的 DOM 操作技术直接更新当前页面。为此需要从服务器获取数据，这便是 Ajax 的用武之地。Ajax 的另一个常见用途是仅下载部分页面，在用户阅读已下载内容时，Ajax 在后台加载剩余页面。

*Ajax*是异步 JavaScript 与 XML（Asynchronous JavaScript And XML）的缩写。Ajax 与其说是一项技术，不如说是一种使用多种技术的设计模式。简而言之，Ajax 就是客户端运行的 JavaScript 代码通过服务器调用来获取数据。调用时会注册回调函数处理响应。最初开发此技术时，XML 是预期的响应格式，因此得名。但响应并非必须为 XML，如今多数情况下会采用 JSON 或 HTML 格式。

该概念最早出现在 Internet Explorer 5 中，使用名为`XMLHTTP`的 ActiveX 对象。其他浏览器随后推出了提供类似功能的`XMLHttpRequest`对象。从 IE7 开始，微软也支持`XMLHttpRequest`对象。

> **注意**：Ajax 用于与 RESTful Web 服务通信。要使用 Ajax，您需要了解 RESTful Web 服务的基础知识，例如 HTTP 动词以及请求与响应标头。如果您不熟悉这些内容，建议阅读此文：[`www.drdobbs.com/web-development/restful-web-services-a-tutorial/240169069`](http://www.drdobbs.com/web-development/restful-web-services-a-tutorial/240169069)

## 发起请求

要发起请求，您需要创建`XMLHttpRequest`对象的实例。然后执行以下操作：

- 指定回调函数处理`readystatechange`事件（稍后详述）。
- 打开连接，指定 HTTP 动词（如`GET`、`POST`、`PUT`或`DELETE`）以及要调用的 Web 服务 URL。
- 设置所需的请求标头。
- 发送请求。

© Mark J. Collins 2017

M. J. Collins，《精通 HTML5：CSS、JavaScript 与多媒体》，DOI 10.1007/978-1-4842-2463-2_27

一个简单的 GET 请求，代码可能如下所示：

```javascript
var myRequest = new XMLHttpRequest();
myRequest.onreadystatechange = displayResults;
myRequest.open("GET", url);
myRequest.setRequestHeader("Content-Type", "application/json");
myRequest.send();
```

如果使用 POST 或 PUT 动词，你很可能需要在请求正文中传递数据。在这种情况下，数据通过 `send()` 方法传递。例如：

```javascript
myRequest.send(body);
```

> **警告**：通常情况下，Ajax 调用只能发送到与调用页面同域的服务。例如，如果你的页面来源是 `http://someDomain.com`，那么你只能调用 URL 以 `http://someDomain.com` 开头的服务。存在绕过这些限制的方法，但这超出了本书的讨论范围。

更多信息，请参阅以下文章：[`developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)

## 处理响应

你可能已经注意到，有一个单独的回调函数会在请求的就绪状态（或简称状态）每次发生变化时被调用。当收到响应或发生错误时，状态会发生变化，因此你的回调函数需要处理这两种情况。共有五种可能的状态值，当状态变为其中任何一种时，该函数都会被调用：

- 0 - UNSENT（未发送）
- 1 - OPENED（已打开）
- 2 - HEADERS_RECEIVED（已收到响应头）
- 3 - LOADING（加载中）
- 4 - DONE（完成）

在大多数情况下，你不会关心中间状态，只需要知道请求何时完成。你可以使用常量 `XMLHttpRequest.DONE`，其值为 4。回调函数通常会以如下代码开始，以忽略所有其他状态转换：

```javascript
if (myRequest.readyState === XMLHttpRequest.DONE) {
    // ... 执行某些操作
}
```

你还应该将此代码包装在 try/catch 块中，因为某些错误（例如网络错误）会生成异常。接下来你需要做的是检查请求的状态。就绪状态只告诉你请求已完成；你仍然不知道它是否成功。这通过检查 `status` 属性来完成，如下所示：

```javascript
switch(myRequest.status) {
    case 500:  // 服务器错误
        // ...
        break;
    case 404:  // 未找到
        // ...
        break;
    case 200:  // 成功
        // ...
        break;
}
```

> **提示**：返回的状态值是 HTTP 规范（http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html）定义的标准 HTTP 状态码。这篇 Mozilla 文章提供了一个更简化的版本：[`developer.mozilla.org/en-US/docs/Web/HTTP/Status`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status)。

假设你获得了成功的状态码 200，那么你需要从响应消息中提取数据。数据将位于 `responseText` 属性中，你需要根据预期的格式对其进行转换。例如，如果返回的是 JSON，你可以像这样获取实际对象：`var data = JSON.parse(myRequest.responseText);`

#### 总结

你通常会使用一个函数来发送请求，并使用另一个不同的函数来处理响应。`XMLHttpRequest` 对象需要被两个函数共享，因此你需要在函数作用域之外声明它。综合起来，一个简单的解决方案如清单 A-1 所示。

*清单 A-1. 一个简单的 Ajax 示例*

```javascript
var myRequest;
var responseData;

function getData() {
    myRequest = new XMLHttpRequest();
    myRequest.onreadystatechange = getResponse;
    myRequest.open("GET", "http://someDomain.com/resource");
    myRequest.setRequestHeader("Content-Type", "application/json");
```
