# 7. 使用 Node.js 实现实时协作

到目前为止，在本书中，我们编写的所有代码都是客户端的。虽然如今的客户端 JavaScript 非常强大，但通常还需要一个服务器，以便数据可以存储在线、将敏感逻辑保留在客户端之外，或者让用户能够在线互动。你可能熟悉其他服务器端语言，如 PHP 或 Ruby（甚至可能是 Perl！），但 Node.js 的优势之一是你使用 JavaScript 编写服务器端代码，因此由于你拥有客户端 JS 的经验，应该很容易上手。

## 制作聊天室

在本章中，我们使用 Node.js 和 Socket.IO 来创建一个实时的聊天室 Web 应用，允许多个用户互相交谈。我指导你在本地机器上设置 Node.js，因为最常用的服务器（共享主机）通常不支持 Node.js。但请注意，像 Heroku 和 Amazon Web Services 这样的服务提供免费套餐。

### 安装 Node.js

显然，要使用 Node.js 编写任何代码，你需要先安装它。安装方法有很多。你可以使用包管理器，如 `apt-get`、`yum` 或 `Homebrew`（取决于你使用的操作系统），但这些包管理器并不总是最新的。在 Node.js 网站 [`www.nodejs.org`](http://www.nodejs.org/) 上，你可以下载最新版本的可执行二进制文件或从源代码构建。或者，你也可以使用来自 [`www.github.com/creationix/nvm`](http://www.github.com/creationix/nvm) 的 Node 版本管理器（`nvm`）。如果你正在处理许多并非都使用最新版本的代码库，这尤其有用。Node 自带一个名为 `npm` 的内置包管理器，它使用名为 `package.json` 的文件来处理依赖关系。因此，你无需自己安装 `Express` 和 `Socket.IO`，只需将它们写入该文件即可。当你准备安装这些包时，在包含 `package.json` 的文件夹中运行 `npm install`。

```
{
    "name": "javascript-creativity-chatroom",
    "description": "A chatroom made in chapter 7 of the JavaScript Creativity book.",
    "version": "0.0.1",
    "private": true,
    "dependencies": {
        "express": "3.x",
        "socket.io": "0.9.x"
    }
}
```

与传统的 JavaScript 不同，Node.js 有一个模块系统，允许你在运行时加载 JavaScript 文件。每个模块都在一个文件中定义，通过 `module.exports` 对象导出变量和函数。在另一个文件中，你可以使用 `require` 函数来加载该模块并获取其 `module.exports` 的内容。加载模块有三种不同的方式。

*   `require()` 一个相对文件，例如 `require('./lib/utils.js')`。这会加载单个文件并返回该文件中 `module.exports` 的值。如果你的项目很大且包含多个文件，你会经常使用这种方法。
*   `require()` 一个核心模块，这些模块是一些特殊的关键词。例如，使用 `require('http')` 来加载核心的 `http` 模块。Node.js 预装了一系列标准核心模块，但它们默认不包含在内，这意味着当你需要它们时必须显式地引入。你可以在 Node.js 文档（[`www.nodejs.org/api/index.html`](http://nodejs.org/api/index.html)）中查看核心模块的完整列表。
*   `require()` 一个来自 `node_modules` 文件夹的独立模块，例如 `require('express')`。这是模块系统最强大的功能。你可以通过引用模块名称来加载整个独立的模块。其工作原理是，Node.js 对 `node_modules` 文件夹有特殊认知。它会从当前脚本所在的目录向上遍历目录树，查找 `node_modules/<name>/package.json` 或 `node_modules/<name>/index.js`。它会首先查看 `package.json`，检查它是否有一个 `"main"` 属性，该属性引用了一个名称与 `index.js` 不同的 `.js` 文件来加载。假设你有一个文件 `/www/myapp/index.js`，其中包含 `require('express')`。Node.js 会在以下目录中查找该模块：
    *   `/www/myapp/node_modules/express/`（`package.json` 或 `index.js`）
    *   `/www/node_modules/express/`（`package.json` 或 `index.js`）
    *   `/node_modules/express/`（`package.json` 或 `index.js`）
    *   `$NODE_PATH/express/`（`package.json` 或 `index.js`）
    *   `$HOME/.node_modules/express/`（`package.json` 或 `index.js`）
    *   `$HOME/.node_libraries/express/`（`package.json` 或 `index.js`）
    *   `$PREFIX/lib/node/express/`（`package.json` 或 `index.js`）
    *   这是 Node.js 查找的文件夹的详尽列表。强烈建议你将模块放在本地的 `node_modules` 文件夹中，以确保速度和可靠性。其他文件夹主要用于 `node_modules` 文件夹内的 Node.js 模块需要它们自己的模块时，这样你就可以在项目和你自己的模块之间共享模块。



这就是 `npm` 发挥作用，让事情变得更好的地方。由于 Node.js 中 `node_modules` 文件夹的特性，`npm` 会查看你的 `package.json` 中的 `dependencies` 和 `devDependencies` 对象，并将每个依赖项下载到你的 `node_modules` 文件夹中。它还会下载你所有模块的依赖项，以及这些模块的模块的依赖项，依此类推。`dependencies` 和 `devDependencies` 的区别在于，`npm` 不会安装其他模块的 `devDependencies`，只会安装 `dependencies`。默认情况下，`npm` 会将所有内容安装到本地的 `node_modules` 文件夹中。但这并不总是你想要的，所以如果你使用 `npm install -g <module>` 安装模块，那么它会将它们全局安装（通常在 `$HOME/.node_modules/` 中）。这非常有用，因为你可以用它来安装用 Node.js 编写的命令行程序。例如，我在开发项目时使用 Remy Sharp 的 `nodemon` 作为运行 node 服务器的替代方式，因为默认的运行服务器的方式 `node [filename]` 不会监视服务器运行时已更新的文件。要安装 `nodemon`，你可以运行 `npm –g install nodemon`，然后运行 `nodemon [filename]` 通过 `nodemon` 运行服务器。更多关于 `nodemon` 的信息可以在 [`www.remy.github.io/nodemon/`](http://remy.github.io/nodemon/) 找到。

当然，Node.js 最简单的例子就是 Hello World。要提供网页服务，我们需要引入一个名为 `http` 的模块，它可以让我们处理 `HTTP` 的底层细节。这个模块是一个核心模块，所以我们使用列表中的第一个定义。一旦引入了 `HTTP`，我们将其存储在一个名为 `http` 的变量中。然后我们使用 `http` 上的一个名为 `createServer` 的方法；这个方法有一个回调函数，允许你发送响应（也就是网页）。要创建被提供的网页，我们首先使用 `response.writeHead` 编写头部信息。需要提供的第一个参数是 `HTTP` 状态码 `200`（表示成功）。第二个参数是实际的头部信息，在这个例子中我们只写入 "Hello World"，所以 `Content-Type` 可以是 `text/plain`，如果你想向页面写入一些 HTML，你可以选择使用 `text/html`。要写入实际内容，只需使用 `response.write`。一旦你写完了响应，使用 `response.end()` 结束它。现在你已经有了一个服务器，但要让页面真正被提供，你需要告诉它在一个端口上监听活动（在这个例子中，端口是 8080）。

```
var http = require("http");

http.createServer(function(request, response) {
  response.writeHead(200, {"Content-Type": "text/plain"});
  response.write("Hello World");
  response.end();
}).listen(8080);
```

现在你已经知道如何引入一个模块并用它来做一些事情了，让我们来创建我们自己简单的模块，这个模块会用到我提到的 `module.exports`。这个模块将把我们的 Hello World 代码封装到一个名为 `helloworld.js` 的文件中，然后我们在 `server.js` 中将其作为名为 `hello` 的变量引入，并调用 `server` 方法。

`helloworld.js`

```
var http = require("http");

function hello() {
  http.createServer(function(request, response) {
    response.writeHead(200, {"Content-Type": "text/html"});
    response.write("Hello World");
    response.end();
  }).listen(8080);
}

module.exports.server = hello;
```

`server.js`

```
var hello = require('./helloworld.js');
hello.server();
```

当然，这是一个极其简单的例子，但它很好地展示了如何引入一个自定义模块，并根据它导出的名称（而不是函数本身的名称）来调用一个方法。在这里指出方法和函数之间的区别可能会有所帮助，因为很多人会交替使用这两个词（这是可以的，但我更喜欢更精确一些）。方法是用于对象的，而函数则独立于任何对象。

## Socket.IO 和 Express

我之前提到过 Socket.IO，它是我们通过 `package.json` 安装的包之一，但我没有解释它的作用。基本上，它是 web sockets 的一个封装，使得所有浏览器都能支持实时通信。

> Socket.IO 旨在让每个浏览器和移动设备都能实现实时应用，模糊了不同传输机制之间的差异。它是无忧无虑的、100% 基于 JavaScript 的实时通信。 —— Socket.IO 主页

Socket.IO 也非常易于使用。一旦安装完成，它需要在服务器端文件中被引入（类似于在 PHP 中引入文件，但 node 会自动找到它被安装的位置），然后将其设置为监听服务器。

当然，我们还没有创建服务器。像 PHP 这样的语言每次执行一个文件，并将文件的内容转换为输出；Node.js 则不同，它是一个长期运行的过程，会多次为多个文件提供服务。Node.js 内置了一个 HTTP 模块（如果你愿意，也可以使用 apache 或 nginx），它让你有能力自己创建一个简单的服务器——但在大多数情况下，你并不想处理底层服务器代码。

Express.js 让你可以专注于开发网站时实际编写代码的部分，而不用担心每个文件如何访问其余的文件结构等问题。然而，它的观点并不强烈；使用 Express.js，你可以使用任何你喜欢的范式（或工作方式）。你可以使用 Express 提供的工具编写简单的网站、博客、高级 web 应用或 REST API。如果你确实喜欢强观点的做法，Express 有一个名为 `express-generator` 的官方结构工具，可以使用 `npm` 全局安装，以帮助你开始一个新项目，尽管它不会强迫你一直使用相同的结构。

作为使用 Express 管理底层代码的简要示例，请看下面的两个代码片段。如果你在 `localhost:8080/hello`，它们都会向页面写入 'Hello World!'，如果你在其他任何页面上，它们都会写入 'Goodbye cruel world!'。区别在于，第一个片段直接使用了 `http`，而第二个片段使用了 Express。当然，这仅仅触及了 Express 能力的皮毛，但它应该很好地展示了 Express 如何不需要像 HTTP 头部这样的底层代码。

**不使用 Express**

```
var http = require('http');

http.createServer(function(request, response) {
  var url = request.url;
  response.writeHead(200, {"Content-Type": "text/html"});

  if (url == '/hello')  {
    response.write("Hello World!");
  }
  else  {
    response.write("Goodbye cruel world!");
  }
  response.end();
}).listen(8080);
```

**使用 Express**

```
var express = require('express');
var app = express();

app.get('/hello', function(request, response) {
  response.send("Hello World!");
});

app.get('/*', function(request, response) {
  response.send("Goodbye cruel world!");
});

app.listen(8080);
```

现在，让我们使用 Express 来开始为我们的聊天室编写服务器。在本章的开头，有一个用于安装 Express 和 Socket.IO 的 `package.json`，如果你还没有安装这些包，那么你现在需要安装它们。单独使用 Express 与使用 Socket.IO 略有不同，因为需要将一个 `http.createServer` 实例传递给 Socket.IO 的监听器。这在 Express 的第 3 版中发生了变化，在此之前，`express()` 函数返回的是 `http.Server` 的一个实例。

```
var app = require('express')(),
    server = require('http').createServer(app),
    io = require('socket.io').listen(server);

server.listen(8080);
```



## 构建基础聊天室

目前没什么可看的，因为我们这次连“Hello World”都没写在页面上，所以先用纯 HTML 搭建一个基础页面。对于最简单的聊天室，我们只需要两个文本框来输入你的昵称和消息（根据 UI 设计不同，昵称的输入方式可以有所不同），以及一个发送消息的按钮和一个用于显示消息的 `div`（也可以使用列表来改善语义）。这个文件通常可以（但不是必须）保存为 `index.html`，放在与服务器代码相同的文件夹中。

```html
<!doctype html>
<html>
<head>
<title>聊天室</title>
</head>
<body>
<label for="nickname">昵称:</label><input type="text" id="nickname"></input>
<div id="messages"></div>
<textarea id="message"></textarea>
<a id="send">发送</a>
</body>
</html>
```

现在你需要使用路由器（就像我们在 Hello World 示例中那样）来告诉浏览器在哪里找到内容。对于这个聊天室，我们希望多个房间可访问，一个很好的方法是将聊天室的名称（或一个随机标识符）放在房间的 URL 中。`Express` 提供了一种简单的方法来检查这个名称，作为路径名片段（也称为命名参数），但由于这个聊天室不需要多个视图，我们可以直接将每个页面重定向到 `index.html`。我们可以使用通配符来代替路径名片段，但介绍路径名作为参数使用的概念是个好方法。这也是强制包含聊天室 ID 的一种方式，因为如果没有 ID，它将不会提供页面；这意味着如果你想要添加一个主页，就需要一个单独的路由。

```javascript
app.get('/:id?', function (req, res) {
  res.sendfile(__dirname + '/index.html');
});
```

当然，这只是一个相当无用的聊天室。让我们编写处理消息的服务器端代码。使用 `Socket.IO`，我们需要通过 `io.sockets.on('connection', function (socket) {});` 作为连接事件处理器来检查连接，然后加入路径名片段中命名的房间。与 `Express` 不同，为了获取标识符，我们使用 `var room = socket.handshake.headers.referer`，因为一旦路由完成，`Express` 的 `.get` 调用就不可用了。获取标识符的其他方法包括：从客户端发送路径名片段，或者最初使用 `res.local`（作用域限于请求/响应周期的变量）提供它，并在客户端将其暴露为全局变量。使用 `socket.handshake` 虽然路径较长，但在这种情况下是避免从客户端发送的好方法。

将标识符存储在名为 `room` 的变量中后，我们现在可以使用 `socket.join(room)` 来加入一个与 ID 同名的 `Socket.IO` 房间。这些房间本质上是一种将正确的消息发送给正确的人的方式，只有房间的成员才能收到该特定房间的消息。

为了检查消息或用户离开房间的情况，我们在 `socket` 上使用事件处理器。`Socket.IO` 上的每一次通信都被归类为一条消息，不要与聊天室中写入的消息混淆。发送消息被称为发射（emitting）。要让聊天室中的消息相互发送，我们首先在客户端（我稍后会讲到）向服务器发射消息，然后服务器将消息发送给连接到该房间的其他客户端。要从服务器向房间发射消息，我们使用 `io.sockets.in(room).emit(key, value)`。对于聊天室消息，我们使用字符串 `'message'` 作为键，数据则通过事件监听器上的 `data` 参数作为值传递。对于离开房间，键是 `'leave'`。

```javascript
io.sockets.on('connection', function (socket) {
  var room = socket.handshake.headers.referer;
  socket.join(room);
  socket.on('message', function (data) {
    io.sockets.in(room).emit('message', data);
  });
  socket.on('leave', function (room) {
    socket.leave(room);
  });
});
```

就基础聊天室的服务器端而言，这就是全部了，你可以看到代码在清单 7-1 中是多么简洁。我们现在需要做的就是在客户端为事件处理器设置代码，以便在发送消息时发射消息。这样，借助 `Socket.IO` 房间和我们刚刚编写的代码，它会自动将消息发送给同一房间中的任何人——而且是实时的！不需要担心 AJAX 或长轮询，`Socket.IO` 会使用最适合的方法（大多数现代浏览器使用 Web Socket，其他浏览器则会回退到合适的方法）。不使用 AJAX 是我选择在本章中不使用 jQuery 或任何类似库的最大因素之一，不过在大型项目中你可能需要使用它们。

**清单 7-1**

```html
<script>
  var socket = io.connect('http://localhost');

  socket.on('message', function(evt) {
    document.querySelector("#messages").innerHTML += '<p><span class="nickname">'+evt.nickname+': </span><span class="message">' + evt.message +'</span></p>';
  });

  sendButton = document.querySelector("#send");
  var messages = document.querySelector("#message");

  sendButton.addEventListener('click', function() {
    socket.emit('message', {
      "message": messages.value,
      "nickname": document.querySelector("#nickname").value || '访客'
    });
    messages.value = "";
  });
</script>
```

> **注意**  
> 要在客户端使用 `Socket.IO`，你必须包含 `<script src="/socket.io/socket.io.js"></script>`，不过你不需要担心文件的位置，因为 Node.js 会自动处理。

## 总结

许多 Node.js 初学者在接触到一种与自己习惯的技术截然不同的技术时，总是感到有些畏惧。我希望本章能够让你轻松地进入 Node.js 的世界，让你觉得制作一个实时的多用户应用简直简单得可笑。聊天室可以非常多样化，所以这个练习应该为你学习更多语言知识提供了一个很好的起点。你可以通过使用你感兴趣的语言特性来增强聊天室，从而更进一步。在下一章中，我们将为聊天室添加视频和音频流功能，作为一种会议风格的功能，这将把你的 Node.js 专业技能提升到新的水平。

---

