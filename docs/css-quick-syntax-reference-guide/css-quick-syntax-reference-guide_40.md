# 计数器

在 CSS 中，可以使用 `counter-increment` 和 `counter-reset` 属性自动为元素编号。`counter-reset` 属性指定计数器的名称，后面可跟计数器的初始值，默认值为零。

```
/* 创建一个名为 chapter 的计数器 */
body { counter-reset: chapter; }
```

`counter-increment` 属性接受计数器的名称，后面可跟一个可选数字。该数字设定元素每次出现时计数器的增量，默认值为 1。

```
/* 在每个 <h1> 处递增计数器 */
h1:before { counter-increment: chapter; }
```

创建计数器的最后一步是使用 CSS 的 `counter()` 函数来显示它，该函数参数为计数器的名称。在此示例中，章节编号会显示在 `<h1>` 元素之前：

```
/* 递增并显示计数器 */
h1:before {
  content: "第 " counter(chapter) " 章 - ";
  counter-increment: chapter;
}
```

现在，计数器会在 `<h1>` 元素前添加章节编号。

```
<h1>第一章</h1>  <!-- 第 1 章 - 第一章 -->
<h1>第二章</h1> <!-- 第 2 章 - 第二章 -->
<h1>第三章</h1>  <!-- 第 3 章 - 第三章 -->
```

可以添加另一个计数器来为 `<h2>` 子标题编号。此计数器会在每个 `<h1>` 元素处重置为零：

```
h2:before {
  content: counter(chapter) "." counter(section) " ";
  counter-increment: section;
}
h1 { counter-reset: section; }
```

以下示例展示了计数器的显示方式：

```
<h1>标题</h1> <!-- 第 1 章 - 标题 -->
<h2>子标题</h2>  <!-- 1.1 子标题 -->
<h2>子标题</h2>  <!-- 1.2 子标题 -->
<h1>标题</h1> <!-- 第 2 章 - 标题 -->
<h2>子标题</h2>  <!-- 2.1 子标题 -->
```

### 嵌套计数器

CSS 计数器可以任意层级地嵌套。这些嵌套的计数器可以使用名为 `counters()` 的 CSS 函数进行组合和显示。该函数的第一个参数是计数器名称，第二个参数是分隔每个计数器的字符串。

```
ul { counter-reset: item; }
li:before {
  content: counters(item, ".") " ";
  counter-increment: item;
}
```

这些规则适用于以下无序列表（注意，每个嵌套列表都会自动创建一个新的计数器实例）：

```
<ul>
  <li>项目</li>   <!-- 1 项目 -->
  <li>项目</li>   <!-- 2 项目 -->
  <ul>
    <li>项目</li> <!-- 2.1 项目 -->
    <li>项目</li> <!-- 2.2 项目 -->
  </ul>
</ul>
```

所有主流浏览器均支持计数器，包括所有版本的 Chrome、Firefox、Safari、Opera 以及 IE8+。

