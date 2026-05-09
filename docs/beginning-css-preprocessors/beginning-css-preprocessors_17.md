# 使用 Less 进行开发

在上一章中，你了解了可以用 Sass 做的各种事情，包括 mixins 等各种技术。在本章中，你将了解另一个同样著名的预处理器框架，名为 *Less*。

本章将探讨 Less 的以下方面：

*   Less 的客户端和服务端变体
*   语言特性
*   嵌套规则
*   导入指令
*   Mixin 守卫
*   引用父级选择器
*   将 `&` 与守卫一起使用
*   分离规则集
*   合并属性
*   循环 mixins
*   函数

## Less 简介

Less 是一种预处理器语言，会被编译成 CSS。它由 Alexis Sellier 创建。Less 遵循 Sass 的“SCSS”语法风格，而后者又类似于 CSS 语法。Less 是开源的。Less 最初是用 Ruby 编写的；然而，在后续版本中，它被用 JavaScript 重写。请注意，有效的 CSS 也是有效的 Less 代码。Less 提供了与你在上一章中看到的 Sass 类似的功能，例如变量、嵌套、运算符、mixins 和函数；然而，使 Less 区别于其他 CSS 预编译器的是它能够通过浏览器中的 `less.js` 进行实时编译。

与 Sass 不同，Less 有两种变体。Less 有一个客户端变体，它在浏览器中编译 Less 代码；还有一个服务端变体，它在服务端编译文件，然后将其提供给浏览器。开发人员可以使用客户端变体来开发和测试代码。服务端变体可用于编译代码、压缩代码，然后使其可用于生产环境。

需要记住的一点是，客户端变体不会生成输出文件。它会在每次浏览器重新加载时编译代码。

### Less 的客户端变体

要使用 Less 的客户端变体，你需要下载名为 `less.js` 的客户端编译器，该编译器可在 [`lesscss.org/#download-options`](http://lesscss.org/#download-options) 获取。下载完成后，将其复制到你的工作目录中。现在，让我们尝试使用这个 Less 编译器编写一些 Less 代码。

***列表 4-1***. 示例的 HTML 标记

```
<html>
        <head>
                <title>列表 4-1</title>
                <link type="text/css" rel="stylesheet/less" href="styles1.1.less" />
                <script src="//cdnjs.cloudflare.com/ajax/libs/less.js/2.2.0/less.min.js"></script>
        </head>
        <body>
                <p class="impElement">这是我的第一个 Less 代码</p>
        </body>
</html>
```

***列表 4-2***. `styles1.1.less` 中的 Less 代码

```
.impElement{
        background:#666;
        padding:10px;
        border-radius:10px;
}
```

当你运行此页面时，输出将如图 4-1 所示。

![9781484213483_Fig04-01.jpg](img/9781484213483_Fig04-01.jpg)

图 4-1. 列表 4-1 和 4-2 的输出

![Image](img/sq.jpg) **注意** 在列表 4-1 提供的示例中，我们通过内容分发网络（CDN）链接了 `less.js`。要运行此代码，你需要一个 Web 服务器，否则 `less.js` 将无法加载。如果你希望在不运行 Web 服务器的情况下运行此示例，请下载 `less.js` 并在本地链接它。

### 服务端变体

客户端变体适用于开发和调试。但是，当部署到生产环境时，建议使用服务端变体——即将 Less 代码编译成浏览器友好的 CSS。为此，你需要安装 `lessc` 编译器。可以使用 node 包管理器进行安装。

如果你没有安装 node，则需要先安装它。安装 Node.js 的步骤在此处有详细说明：[`howtonode.org/how-to-install-nodejs`](http://howtonode.org/how-to-install-nodejs)。

完成 Node.js 的安装后，你就可以开始安装 `lessc` 编译器了。在终端中运行列表 4-3 中的命令。

***列表 4-3***. 安装 `lessc` 编译器的命令

```
npm install -g less
```

你可以通过运行列表 4-4 中所示的命令来测试 `lessc` 是否安装成功。

***列表 4-4***. 检查 `lessc` 命令是否可用的命令

```
lessc -v
```

此命令将输出已安装的 `lessc` 版本。既然 `lessc` 编译器已经安装，让我们来看一下编译 Less 文件的过程。

#### 编译 Less 文件

你可以使用列表 4-5 中所示的命令来编译一个 Less 文件。

***列表 4-5***. 调用 `lessc` 编译器的命令

```
lessc style.less
```

此命令将直接在终端中为你提供输出。如果你希望将输出保存到 `.css` 文件中，则需要修改命令，添加一个由空格分隔的第二个参数，如列表 4-6 所示。

***列表 4-6***. 调用 `lessc` 编译器并将输出保存到另一个文件的命令

```
lessc style.less style.css
```

可以使用各种选项来调用 `lessc` 编译器。列表 4-7 到 4-9 展示了一些支持的选项。

***列表 4-7***. `-h` 选项用于获取 `lessc` 编译器的帮助

```
lessc -h
```

***列表 4-8***. `--include` 用于包含 Less 查找导入文件的路径

```
lessc --include-path=framework/;lib/
```

可用的导入路径可以用 `:` 或 `;` 分隔的列表指定。

***列表 4-9***. 使用 `-l` 选项通过 `lessc` 编译器检查 Less 代码

```
lessc -l style.less
```

除了这些之外，在调用 `lessc` 编译器时还可以使用许多其他选项。可以在 Less 文档中找到它们：[`lesscss.org/usage/index.html#command-line-usage-command-line-usage`](http://lesscss.org/usage/index.html#command-line-usage-command-line-usage)。

在下一节中，你将了解该语言提供的特性。

## 语言特性

本节涵盖 Less 提供的所有特性。许多特性与你之前章节中学到的类似；因此，它们应该很容易理解。

### 变量



