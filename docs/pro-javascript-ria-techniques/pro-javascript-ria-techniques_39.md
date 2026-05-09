# 性能调优

这项技术的妙处在于，在服务器上启用它非常简单。几乎所有现代网页浏览器都支持它。不支持压缩技术的浏览器将从服务器接收未压缩的数据，因此数据绝不会丢失。

如果您的网络服务器默认未启用此功能，应尽快启用它。您会立即注意到它的效果，最终用户也会有所感受。

在服务器上压缩数据，与直接返回未压缩文件相比，确实会增加服务器的 CPU 负载。这种增加通常不足以引起任何问题，但如果您的服务器硬件资源有限，则值得注意。

**注意：** 某些非常古老的浏览器会向服务器宣称它们支持压缩内容，但实际上并不支持。一些压缩服务器组件足够智能，不会向这些浏览器发送压缩数据。在实践中，这并不足以构成值得真正担忧的问题。

让我们来看看如何在三种主流服务器上启用压缩：Apache、微软 Internet 信息服务 (IIS) 和 Tomcat。

## 在 Apache 网络服务器中启用压缩

如果您运行的是 Apache 网络服务器 1.3 版本，则应安装并启用 `mod_gzip` 模块，该模块可在 [`sourceforge.net/projects/mod-gzip/`](http://sourceforge.net/projects/mod-gzip/) 在线获取。如果您运行的是 Apache 2.*x* 版本，则应配置 `mod_deflate` 模块，该模块已包含在 Apache 安装包中。关于 `mod_deflate` 的完整说明可以在线查阅 [`httpd.apache.org/docs/2.0/mod/mod_deflate.html`](http://httpd.apache.org/docs/2.0/mod/mod_deflate.html)。基本配置非常简单，只需在您的 `apache.confd` 文件中输入以下内容：

```
AddOutputFilterByType DEFLATE text/html text/plain text/xml text/javascript text/json
```

## 在 IIS 中启用压缩

如果您运行的是微软 IIS 7 版本，某些文件会默认压缩。但是，您应该通过执行以下命令（从服务器命令行）为所有文件启用压缩：

```
appcmd set config -section:urlCompression /doDynamicCompression:true
```

不幸的是，在微软 IIS 6 版本中启用 gzip 压缩并非如此简单。这里不列出所有详尽步骤，而是请您参考以下说明来启用这个出色的性能特性：[`www.wwwcoder.com/main/parentid/170/site/3669/68/default.aspx`](http://www.wwwcoder.com/main/parentid/170/site/3669/68/default.aspx)。

## 在 Tomcat 中启用压缩

在 Tomcat 中启用同类型的压缩与其他技术一样简单。以下是将要添加到您的 `server.xml` 文件中的一行代码：

```
compression="on"
```

现在，我们已经了解了一些重要的网络服务器更改，用以提高内容进入浏览器时的速度。接下来，让我们看看在前端（浏览器本身内部）可以做些什么，以确保您的页面尽可能快地加载。

