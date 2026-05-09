# HTML 文件上传控制

`HTML`文件上传控件适用于上传小型的单个文件。正如您所料，选中的文件直到表单提交时才会与表单中的其他数据一起发送到服务器。如果用户选择了一个大文件，而服务器可能因文件过大或文件格式不符合预期而拒绝该文件，这可能会成为一个问题。此外，对于较大的文件，表单提交时没有进度指示器——无法让用户知道他们需要等待多长时间才能发现表单提交是否成功。一个更优的解决方案应具备以下功能：

- 允许用户选择一个或多个文件
- 如果页面开发者指定，这些文件应在表单其余部分完成之前就开始发送到服务器
- 显示进度指示器，让用户了解上传文件需要等待多长时间

幸运的是，这样的解决方案是存在的。

`Adobe`的`Flash Player`是一个功能极其丰富的浏览器插件，在网络上拥有非常强大的存在，拥有庞大的安装基础。除了本书这一部分讨论的许多其他`Flash Player`用途外，该插件还支持将文件上传到服务器的方法。不仅如此，它还能报告上传进度，并将多个文件排队，一次性发送。

许多聪明的`Flash`开发者编写了可重用的组件，供`JavaScript`开发者在他们的页面上使用这些功能。快速搜索`Google`将会发现针对几种流行的`JavaScript`库编码的版本，例如`jQuery`（`jQuery Transmit`，位于`http://code.google.com/p/jquery-transmit/`）和`MooTools`（`FancyUpload`，位于`http://digitarald.de/project/fancyupload/`）。这些组件中的大多数具有几乎相同的`JavaScript API`，这使得它们之间的迁移成为一项相当简单的任务。

■**警告** 您应确保您选择的任何文件上传组件已更新，以适用于`Flash Player`版本 10。此版本的`Flash`引入了一项重要的安全更改。该版本要求“浏览”文件选择按钮必须包含在`Flash`影片中，而不能在页面本身中。

在本节中，我们将研究一个可以独立运行的文件上传组件，无需依赖其他`JavaScript`库。该组件名为`SWFUpload`，您可以找到该项目的首页，地址为`http://www.swfupload.org/`。下载最新版本，让我们开始构建一个简单的示例表单来演示其功能。

## `SWFUpload`示例用法

清单 8-6 展示了一个简单的`HTML`页面，其中包含支持`SWFUpload`组件所需的所有表单字段和元素。

**清单 8-6.** *演示`SWFUpload`组件的`HTML`页面*

```
<!DOCTYPE html PUBLIC "- //W3C//DTD XHTML 1.0 Strict//EN" ➥
"http://www.w3.org/TR/xhtml1/DTD/xhtml1- strict.dtd">
<html lang="en" xml:lang="en">
<head>
<meta http-equiv="Content- Type" content="text/html; charset=utf- 8" />
<title>多文件上传示例</title>
<link rel="stylesheet" href="upload.css" type="text/css" />
</head>
<body>
<h1>多文件上传示例</h1>
<!- - 带有文件上传字段的表单在将数据发送到服务器进行处理时，必须使用 multipart/form-data 编码类型 -- >
<form method="post" action="/" id="form" enctype="multipart/form- data">
<fieldset>
<legend>要上传的文件</legend>
<label for="make- multiple">选择文件</label>
<!- - 包含一个真正的文件上传控件，以防 JavaScript 被禁用 -- >
<input type="file" id="make- multiple" />
<!- - 创建一个元素来容纳 Flash 组件中的“浏览”按钮。用于启动文件选择对话框的按钮本身必须位于 Flash 中。否则 Adobe 会认为其存在安全风险 -- >
```



