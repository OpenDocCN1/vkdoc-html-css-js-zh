# REST API 与代理模式

## REST API

这些调用可以封装在一个 `REST` 接口中，这样可以避免代码重复，并防止在调用接口时返回多个回调函数。

这样，每个调用都可以使用自己的回调函数，同时采用相同的模式来获取数据，并且可以使用其他代码来处理返回的结果，例如为返回结果进行 `XML` 调用，并显示在页面上。

成功封装后，您只需要返回：

```javascript
var callBack = function(responseText) {
    var stats = eval(responseText); // 解析 JSON 数据
    displayPageviews(stats.getPageviews); // 显示页面访问量
};

var urls = getUniques();
var success = function(){
    var stats = eval(responseText); // 解析 JSON 数据
    displayPageviews(stats.getBrowsers); // 显示浏览器统计
};
```

### 使用 REST 请求的优势

一个标准的 `REST` 格式请求，能让您方便地使用不同类型的请求资源。

1.  **定义接口**：使用一个通用的 `HTTP` 请求来处理 HTML 格式，并与现有数据匹配，这样能让请求更加符合逻辑，并且只需要一个请求接口来获取数据。
2.  **简化操作**：`REST` 将复杂的代码逻辑封装到一个简单的调用中，让您能够快速完成调用。
3.  **提高性能**：使用 `REST` 可以避免多次请求相同数据，并能缓存数据，从而提升应用性能。

HTTP 请求示例：

```javascript
var callback = function(responseText) {
    var stats = JSON.parse(responseText);
    DisplayPageviews(stats.getPageviews);
};
```

REST 的调用非常简单，只要定义好回调函数，就可以通过 `GET` 请求来获取数据。

```javascript
var url = "http://www.mydomain.com/stats/getPageviews";
xmlHttp.open("GET", url, true);
xmlHttp.send();
```

### 总结

`REST` API 是一种简单、高效的 Web 服务接口，它能够帮助您快速实现统计数据的获取和处理。通过使用 `REST`，您可以：
- 简化代码
- 提高性能
- 降低复杂性
- 提升开发效率

---
