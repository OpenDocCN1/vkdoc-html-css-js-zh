# 在浏览器中导航到网页时

当您在浏览器中导航到网页时，页面的 URL 会通过 HTTP 请求（如果使用安全传输则为 HTTPS）发送到相应的 Web 服务器。Web 服务器会返回一个由浏览器渲染的 HTML 文档。如图 8-1 所示。

***图 8-1.** 客户端/服务器架构*

如果网页包含表单，则在提交表单时，会向服务器发送另一个 HTTP 请求。该请求包含的 URL 与第一个请求相同，但也包含了来自输入字段的数据。这些数据可以包含在请求正文中，也可以作为查询字符串参数包含在 URL 本身中。我会在本章后面解释这一点。响应要么是一个新的网页，要么是对现有网页的更新。

© Mark J. Collins 2017

M. J. Collins，*精通 HTML5、CSS、JavaScript 与多媒体*，DOI 10.1007/978-1-4842-2463-2_8

---

## 表单元素

通过服务器请求提交输入数据是网站和 HTML 的基本功能。`form` 元素用于将一个或多个输入元素分组，以便将这些信息发送到 Web 服务器。您还可以在 `form` 元素中配置数据提交的位置（以及方式）。

一个 `form` 元素将包含一个或多个输入元素，这些是用户输入数据的字段。表单通常还会包含 `label` 元素，它们是描述输入元素的简单文本块。一个简单的表单定义如下：

```html
<form action="" method="get">
  <label for="iFirstName">名字:</label>
  <input id="iFirstName" type="text" />
  <label for="iLastName">姓氏:</label>
  <input id="iLastName" type="text" />
  <input type="submit" value="提交" />
</form>
```

此表单包含两个输入字段（名字和姓氏），以及每个字段对应的一个 `label` 元素。最后一个输入元素的 `type="submit"`，默认情况下会将其渲染为一个按钮。更重要的是，当单击此按钮时，表单将提交到服务器。此表单的渲染效果如图 8-2 所示。

***图 8-2.** 一个简单的表单渲染*

> **提示：** 如我在示例表单中所示，通过将输入元素放置在 `form` 元素内部，可以将它们与表单关联。但是，您也可以通过使用 `form` 属性来引用某个 `form` 元素，从而将输入元素包含在该表单中。这允许您将输入元素放置在文档的任何位置，并将其 `form` 属性设置为某个 `form` 元素的 `id` 属性。当提交该表单时，该输入值也会被提交。

### 表单操作

如果您在这些字段中输入数据并点击提交按钮，将会发送类似以下的 HTML 请求：`http://localhost:5266/?FirstName=Mark&LastName=Collins`

您可能还会注意到页面刷新时出现闪烁。Web 服务器会用一个新 HTML 文档来响应此请求。`action` 属性指定了请求的 URL。由于此处留空，因此使用了当前页面的地址。在我的例子中，它是 `http://localhost:5266`。所以浏览器会返回并重新渲染同一页面。

在许多情况下，`action` 属性会指定一个不同的页面。例如，假设初始页面提供了一个表单用于输入搜索条件。提交这些数据后，您会希望根据用户输入返回一个结果页面。此情况下的 `action` 属性就是结果页面的地址。

当提交初始表单时，您实际上是在请求渲染结果页面，并将搜索详情作为请求的一部分提供。

在其他场景中，您希望渲染现有页面。更准确地说，您希望根据刚刚输入的信息来*重新渲染*它。这被称为`回发`。这是指网页提交一个请求，让服务器刷新页面自身，通常会将数据传递给服务器。例如，如果您在一个页面上呈现一系列问题，但某个问题的答案可能会使其他一些问题不再适用。一旦该问题得到回答，一个简单的回发就可以仅用适用的问题重新渲染页面。

> **注意：** 在当前的网页设计中，回发通常不被看好，因为往返服务器和重新渲染整个页面可能会很慢。在这个例子中，最好在客户端用 JavaScript 来处理。

#### 表单方法

到目前为止，您可能已经正确地推断出，`form` 元素完全是关于定义提交表单时的 HTTP 请求。我已经解释了 `action` 属性，它指定了请求发送到的 URL。`method` 属性指示应该使用哪个 HTTP 动词。目前仅支持两个：`GET` 和 `POST`。

`GET` 动词不支持消息正文，因此所有表单数据必须像我之前演示的那样在 URL 中传递。如果您将 `method` 属性设置为 `post`，则表单数据将位于请求正文中。

为了演示这一点，我使用 Fiddler 拦截了请求，原始请求如下所示：

```http
POST http://localhost:5266/ HTTP/1.1
Host: localhost:5266
Connection: keep-alive
Content-Length: 31
Cache-Control: max-age=0
Origin: http://localhost:5266
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko)
Chrome/51.0.2704.103 Safari/537.36
Content-Type: application/x-www-form-urlencoded
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8
Referer: http://localhost:5266/
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.8

FirstName=Mark&LastName=Collins
```

第一行指示被请求的 URL，随后的行列出了 HTTP 请求中包含的所有标头。其中一些标头可以使用其他属性进行调整，我稍后会解释。最后一行是消息正文，包含表单数据。

> **提示：** 表述性状态转移（REST）是一种用于与网站和 Web 服务通信的架构风格。在 RESTful 应用中，HTTP 动词是请求的重要组成部分。您可以对完全相同的 URL 使用不同的动词，并获得截然不同的结果。在我看来，HTML5 不支持其他动词如 `PUT` 和 `DELETE` 非常遗憾。虽然有一些增加这些动词的动向，但似乎缺乏动力。如果您感兴趣，可以查看这篇文章：
>
> [`programmers.stackexchange.com/questions/114156/why-are-there-are-no-put-and-delete-methods-on-html-forms`](http://programmers.stackexchange.com/questions/114156/why-are-there-are-no-put-and-delete-methods-on-html-forms)

## 其他属性

`enctype` 属性可用于控制数据的格式化方式；但是，这里的选项并不多。默认值是 `application/x-www-form-urlencoded`，这基本上意味着输入数据经过 URL 编码。本质上，空格和特殊字符会被编码以满足 URL 格式规则。如果 `method` 属性是 `get`，这是您可以使用的唯一选项。实际上，`enctype` 属性仅在 `method` 属性设置为 `post` 时才允许使用。