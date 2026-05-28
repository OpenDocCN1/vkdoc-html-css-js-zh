# 第一章 ■ 超文本标记语言

**图 1-1.** 在浏览器标签页中显示标题

**图 1-2.** 在书签中使用标题

虽然这看起来显而易见，但你的文档中只能有一个 `title` 元素。如果忽略这条规则，浏览器通常会显示第一个 `title` 元素，并忽略其余内容。

## Meta 元素

`meta` 元素是元数据（metadata）的缩写，元数据是描述其他数据的数据。在本例中，`meta` 元素描述的是 HTML 文档的内容。`head` 元素可以包含任意数量的 `meta` 元素，每个元素通过名称/值对结构提供单一数据点。例如：

```
<meta name="author" content="Mark J Collins" />
```

```
<meta name="description" content="Sample HTML document" />
```

```
<meta http-equiv="refresh" content="45" />
```

名称/值对中的值部分通过 `content` 属性指定；但名称部分则根据所设置数据的类型，通过 `name` 属性或 `http-equiv` 属性来定义。`name` 属性用于描述 HTML 文档内容的元数据。`name` 属性最常见的取值包括 `application-name`、`author`、`description` 和 `keywords`。

这些属性含义都比较直观。对于 `keywords`，其 `content` 属性会包含一个用逗号分隔的关键词列表。

`http-equiv` 属性用于模拟 HTTP 响应头。更常见的取值包括：

- `content-type` —— `content` 属性用于指定内容类型，通常为 `text/html` 及字符集。例如：`content = "text/html; charset=UTF-8"`。

- `default-style` —— 用于指定默认样式表。

- `refresh` —— 可强制页面在指定时间间隔后自动刷新，该间隔以秒为单位，在 `content` 属性中指定。

如示例文档所示，字符集也可以使用简写形式指定：`<meta charset="utf-8" />`。

> **注意** 你还可以使用许多其他 meta 名称。如果感兴趣，请参阅[`www.html-5.com/metatags`](http://www.html-5.com/metatags)上的文章。该网站将这些名称按逻辑分组，例如搜索引擎优化（SEO）、移动设备等，并提供了每个名称的详细信息。

## Script 元素

`script` 元素用于在页面中加载 JavaScript。要使用某个函数，你必须在页面中定义该函数，或者加载包含该函数的外部脚本文件。这两种操作都可以通过 `script` 元素完成。

要直接定义 JavaScript，将代码放在起始和结束标签之间，如下所示：

```html
<script type="text/javascript">
function doSomething() {
  alert("Hello World!");
}
</script>
```

`type` 属性是可选的，如果未指定，则默认为 `text/javascript`。这个属性在 HTML4 中是必需的，因此你经常会看到它被包含在内。

通常，最佳实践是将 JavaScript 放在单独的外部文件中。这样做的一个显著优势是可以在多个页面之间共享相同的脚本。要引用外部文件，使用 `src` 属性，并且不要在起始和结束标签之间包含任何内容。如果需要加载多个文件，请将每个文件放在单独的 `script` 元素中。例如：

```html
<script src="../scripts/sample.js" type="text/javascript"></script>
<script src="../scripts/demo.js" type="text/javascript"></script>
```

通常，当浏览器解析 HTML 文档时，遇到 `script` 元素会先加载并执行脚本，然后才继续解析文档的其余部分。对于外部文件，你可以使用 `async` 或 `defer` 属性来改变这一行为。这两个都是布尔属性。如果指定了 `async` 属性，文件会在解析过程继续进行的同时，以并行方式加载并执行。

或者，如果使用了 `defer` 属性，脚本将在页面完全解析完成之后才执行。

```html
<script src="../scripts/demo.js" defer ></script>
```