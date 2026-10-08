# 10. 项目：Ajax 入门

如果您尝试在没有网络服务器的情况下运行此项目，那可能不太走运。除非其他域名明确允许，否则 JavaScript 无法向不同域名发出 Ajax 请求。这被称为**同源策略**。现代浏览器倾向于将本地打开的（通过“文件：打开...”）不同文档视为来自不同域，*即使它们位于同一个文件夹中*。因此，Ajax 请求将被拒绝。使用`http://`协议从同一位置获取的文件被视为来自同一源，这就是为什么本章节需要一个网络服务器。此项目中唯一能正常工作的部分是最后一节，因为它专门访问了不同的网络服务器。运行此项目不需要功能强大的网络服务器。您可以从[`https://github.com/manngo/micro-web-server/releases/latest`](https://github.com/manngo/micro-web-server/releases/latest)使用一个轻量级的服务器。

在迄今为止的一些项目中，包含了大量静态内容。幻灯片使用了硬编码的图片列表。关于显示和隐藏内容的项目操作了页面上已有的内容。而灯箱项目则需要一个缩略图图片和链接的集合。

传统上，当你从网络服务器加载一个包含图片、CSS 文件和其他相关资源的页面时，连接会关闭，浏览器自行处理已获取的内容。但是，借助 JavaScript，你可以通过一种称为 **Ajax** 的过程来获取额外的资源。Ajax 这个词据说是由 James Garrett 创造的，用来描述一系列允许浏览器无需从头重新加载所有内容即可与服务器交互的技术。在我们的例子中，我们将使用 JavaScript 与相同或不同的网络服务器建立另一个连接，允许它请求额外的资源或将信息发送回服务器。

在幻灯片项目中，你已经看到了这个想法的一个简单形式。每次更改图片的`.src`属性时，你都会与服务器建立一个新的连接以获取另一张图片。以原始的方式，你也在与服务器通信，因为每个图片的请求都可以被处理，并可能被记录。

现代 JavaScript 现在能够以更复杂的方式做到这一点，同时在服务器端和浏览器端进行实时更新。现代 Ajax 最初是使用一个特殊的`XMLHTTPRequest`对象实现的，后来经过改进增加了更多功能。该对象仍然可用，但实现起来可能有些棘手。如今，所有现代浏览器都实现了 Fetch API，它允许你使用`fetch`函数来完成大部分繁重的工作。它比旧的`XMLHTTPRequest`对象简单得多。

在本章中，我们将通过几个小项目来探索 Ajax。每个项目都将使用 Ajax 从服务器获取额外数据。

-   第一部分使用被劫持的链接来获取额外的 HTML 内容。
-   第二部分通过从服务器获取图片列表，并利用你已经编写的代码来运行幻灯片，实现了一个幻灯片功能。
-   第三部分对灯箱画廊进行了类似的操作。此外，我们将创建包含缩略图的元素。
-   第四部分从服务器获取国家列表以生成一个菜单。然后，我们使用该菜单刷新一个表单，表单中的数据是从服务器获取的更多数据。

通过完成这些，我们将培养以下技能：

-   理解异步处理
-   使用`fetch()`函数从服务器获取数据
-   处理 JSON 字符串数据格式以传输复杂数据
-   在网页上创建元素
-   使用 Ajax API 访问数据服务

我们需要打开`ajax.html`来查看发生了什么，并打开`ajax.js`来编写代码。我们还将使用`library.js`文件中的现有代码。

## 时序问题

在接下来的讨论中，请确保你已打开了一个页面，例如`ajax.html`。如果没有打开页面，你很可能会遇到内容安全策略报错。

Ajax 编程中最棘手的问题始终在于时序。与网络服务器的所有通信都需要时间，而 JavaScript 又不喜欢等待。

以下面这段代码为例：

```
var image = new Image;

image.src = 'https://pure-javascript.net/random.jpg';

console.log(image.width);
```

`Image`对象是一个虚拟图像，它具备图像的属性，但不会实际显示出来。如果你执行一次，很可能会得到结果`0`。再执行一次，有可能得到更有意义的值，但也未必。

问题在于请求新图像需要一点时间。JavaScript 不想干等着，就会跳到下一行，试图在图像可用之前读取其宽度，从而得到结果`0`。我们称图像是**异步**加载的，也就是说，加载发生在后台。为了让你免于继续等待，图像可能会被存储在浏览器缓存中，这样下次加载就会更快——快得足以让 JavaScript 能够读取它的宽度，这就是为什么你再次尝试时可能会得到一个非零的结果。

让 JavaScript 等待图像的一种方法是使用**回调**函数：

```
var image = new Image;

image.onload = function(event) {

    console.log(image.width);

};

image.src = 'https://pure-javascript.net/random.jpg';
```

图像的 `onload` 属性被赋予一个函数，该函数将在图像加载完成后运行。通过推迟对图像的操作，现在代码就能按预期工作了。

使用 Ajax 时，你会不断遇到类似的时序问题：你试图获取一个资源（如文本），但 JavaScript 急于继续执行，因此数据总是到达得太晚。处理异步 Ajax 调用的经典方法是同样使用一个 `onload` 事件监听器，将后续处理推迟到回调函数中。然而，对于 Ajax，回调函数要复杂得多，尤其是当你需要在此之后再发起另一个 Ajax 调用时。

现代 JavaScript 通过 `Promise`（期约）解决了这个问题。`Promise` 允许你包含一段在条件就绪时运行的代码，而无需费力处理回调函数。

## 使用 Fetch

`fetch()` 函数向服务器请求一些数据，并返回一个 `Promise`。`Promise` 是一种特殊对象，允许你添加接下来要执行的操作，类似于添加一系列回调函数。

典型的模式是：

```javascript
//  假设示例
fetch(url)                  //  第 1 步：请求某些内容
.then(processResponse)      //  第 2 步
.then(processData);         //  第 3 步
```

`url` 是你期望找到数据的位置。每个后续步骤都是一个传递给 `.then()` 方法的函数。该函数的数据取决于前一步骤中发生的情况。这里我们使用了名称 `processResponse` 和 `processData`，不过你通常会更常使用函数表达式。

对于 `fetch()` 函数，下一步骤将得到一个 `response`（响应）对象作为其参数，通常（尽管不必然）被称为 `response`。响应对象包含从服务器返回的各种详细信息，但我们通常只对提取数据感兴趣。

你可以像这样定义 `processResponse()` 函数：

```javascript
function processResponse(response) {
    return response.text();
}
```

`.text()` 方法从响应中提取文本。结果实际上不仅仅是文本：它是另一个包含文本的 `Promise` 对象。在后续章节中，一旦我们弄清楚了 `json()` 方法的含义，我们也会用到它。

当然，我们也可以使用箭头函数表达式：

```javascript
response => {
    return response.text();
}
```

如果你只是要返回一个值，箭头函数表达式还有一种更简短的形式：

```javascript
response => response.text();
```

现在 `fetch()` 函数可以是这样的：

```javascript
//  仍然是一个假设示例
fetch(url)
.then(response => response.text())
.then(processData);
```

下一步骤将涉及实际数据。`processData` 函数将接收一个参数，其中包含来自前一步骤的文本。同样，如果处理逻辑足够简单，我们可以使用箭头函数表达式：

```javascript
//  仍然仍然是一个假设示例
fetch(url)
.then(response => response.text())
.then(text => {
    console.log(text);
});
```

这个箭头函数所做的不仅仅是返回一个值，这就是代码放在花括号内的原因。

我们可以在控制台中尝试一下：

```javascript
fetch('https://pure-javascript.net/quote.txt')
.then(response => response.text())
.then(text => {
    console.log(text);
});
```

短暂停顿后，你会看到一句来自格劳乔·马克斯的随机名言，例如：

*我一直在寻找一个像你这样的女孩——不是你，而是像你这样的女孩。*

这就是基本原理。现在我们将把它付诸实践。对于本项目，我们需要处理 `ajax.html` 和 `ajax.js` 这两个文件。

HTML 文件包含四个区域，我们将用它们来使用 Ajax。`ajax.js` 文件包含以下代码：

```javascript
init();

function init() {
    ajaxContent();
    ajaxSlideshow();
    ajaxLightbox();
    ajaxCountries();
}

function ajaxContent() {
}

function ajaxSlideshow() {
}

function ajaxLightbox() {
}

function ajaxCountries() {
}
```

每个函数分别处理 HTML 中的一个区域。

## 获取所选内容

在本项目的第一部分，我们将使用 Ajax 来获取点击链接时所需的文本内容。如果你打开 `ajax.html` 文件，会看到一个列表内的链接集合：

```
斑点土豚
星夜
幸运之国
澳大利亚
```

如果你点击其中一个链接，浏览器会加载新文本，但我们希望劫持该链接，让浏览器将内容加载到一个容器 `div` 中：

> 点击链接应使用新内容填充 `div`。内容本身是一个文本文件，但实际上包含 HTML 代码。

我们在 `ajaxContent()` 函数中设置初步的 JavaScript 代码：

```javascript
function ajaxContent() {
    let links = document.querySelector('ul#links');
    let content = document.querySelector('div#content');
}
```

`links` 变量引用链接列表，`content` 变量引用将被填充内容的 `div`。

### 劫持链接

我们之前在灯箱项目中已经经历过劫持链接的过程。在这里，我们将采用不同的方法，使用委托事件监听器——也就是说，我们将事件监听器放在容器上。

我们首先将事件监听器挂载到 `links` 容器的 `onclick` 属性上：

```javascript
function ajaxContent() {
    let links = document.querySelector('ul#links');
    let content = document.querySelector('div#content');
    links.onclick = event => {
        event.preventDefault();
    }
}
```

我们还添加了 `event.preventDefault()` 来阻止链接加载新页面。

### 获取内容

接下来，我们可以直接使用 `fetch()` 函数加载内容，方式与之前的 `fetch` 练习类似。由于锚点已经包含了内容的引用，我们可以在 URL 参数中使用它：

```javascript
function ajaxContent() {
    let links = document.querySelector('ul#links');
    let content = document.querySelector('div#content');
    links.onclick = event => {
        event.preventDefault();
        fetch(event.target.href)
        .then(response => response.text())
        .then(text => {
            content.innerHTML = text;
        });
    }
}
```

这里务必记住使用 `event.target`。尽管事件监听器位于包含元素的列表上，但 `event.target` 引用的却是你实际点击的元素：即锚点（anchor）。通过锚点，我们可以利用 `.href` 属性获取内容的 URL。

与前一个练习相同，我们首先从响应中获取文本，并在下一步对其进行处理。在这个案例中，只需使用 `innerHTML` 填充 `content` 这个 `div` 即可。

现在，我们可以通过刷新页面并点击四个链接来测试此功能。这种技术的好处之一在于，你可以修改待加载的内容，而无需改动 HTML 文件本身。

## Ajax 幻灯片

项目的下一部分将重新探讨幻灯片。在幻灯片项目中，待加载的图片是硬编码的：首先是一个图片文件名数组，然后是一个包含图片文件名和标题的对象数组。我们现在要做的是，使用 Ajax 从外部源加载这个集合。

在 `ajax.html` 文件中，幻灯片部分的代码只有寥寥几行：

`slideshow.html` 文件中的内容确实也就这么多。大部分繁重的工作都在库文件中的 `doSlides()` 函数里完成。我们在这里要做的，是使用 Ajax 获取幻灯片对象的数组。但首先，我们需要了解一种名为 JSON 的数据格式。

### JSON 简介

传统上，简单的数据可以以文本文件的形式存储和传输。但对于更复杂的数据结构，这种方式就行不通了。一种替代方案是使用 XML 来结构化文本文件，XML 是一种类似于 HTML 的标记语言。这就是 Ajax 中 **X** 的由来：异步 JavaScript 和 XML（Asynchronous JavaScript and XML）。XML 非常流行，但在 JavaScript 应用中已显得过于繁琐。因此，一种更简单、更直接的文件格式应运而生。

你已经见过使用如下 JavaScript 代码创建对象的方式：

```
var apple = {
    name: 'Granny Smith',
    shape: 'Round',
    colour: 'Green'
};
```

你也可以使用如下方式创建对象数组：

```
var apples = [
    { name: 'Granny Smith', shape: 'Round', colour: 'Green' },
    { name: 'Fuji', shape: 'Round', colour: 'Red' },
];
```

当然，空格和缩进是可选的，它们的存在仅仅是为了便于人类阅读。注意最后一项后面的逗号。这可能略显粗心，但 JavaScript 会宽容地忽略它。

上述代码是纯粹的 JavaScript。JSON 则将这种表示法扩展到了字符串。JSON 这个词读作“j-son”，意为 `JavaScript 对象表示法`。字符串形式与对象字面量非常相似，但它有一些非常严格的要求。第二个例子的 JSON 版本如下：

```
var apples = `[
    {"name":"Granny Smith", "shape":"Round", "colour":"Green"},
    {"name":"Fuji", "shape":"Round", "colour":"Red"}
]`;
```

注意，字符串被包裹在反引号中。当然，这在 JSON 中并非必需，但它允许我们将字符串写成多行。

这个字符串与之前的对象数组非常相似，但请注意以下标注差异：

-   每个属性名都用双引号括起来。
-   所有字符串都用双引号括起来，而不是单引号。
-   最后一项之后不能有尾随逗号。

这对于普通的 JavaScript 对象字面量来说也是有效的，但却是 JSON 的绝对要求。

一旦你有了 JSON 文本，通常可以使用 `JSON.parse(...)` 将其转换为合法的 JavaScript 对象：

```
var apples = `[
    {"name":"Granny Smith", "shape":"Round", "colour":"Green"},
    {"name":"Fuji", "shape":"Round", "colour":"Red"}
]`;
apples = JSON.parse(apples);
```

`JSON` 这个内置对象只有两个方法：`JSON.parse()` 将字符串转换为对象，`JSON.stringify()` 则执行相反的操作。如果你想知道为什么要费心在字符串和对象数据之间进行转换，那是因为字符串是一种简单、安全的数据形式——它们易于通过互联网连接传输，也易于保存在文本文件中。通过设计一种将对象编码为字符串的方法，你可以存储或传输更复杂的数据。JSON 已变得非常流行，以至于在那些甚至没有 JavaScript 的其他各种平台上都得到了广泛支持。例如，PHP、Python 以及一些数据库也支持 JSON 格式。

### 加载幻灯片文件

在 `images/slides/` 目录下有一个名为 `slides.json` 的文件。我们将采用与上一节获取文本内容相同的方式来获取这个文件。以下代码最终会被添加到 `ajaxSlides()` 函数中，但我们会先在控制台中试验一下。

我们可以获取数据并使用 `JSON.parse()` 函数来解析它。请记住，在尝试此操作时，确保 `ajax.html` 页面是打开的，以避免跨域错误：

```
fetch('images/slides/slides.json')
    .then(response => response.text())
    .then(text => {
        let data = JSON.parse(text);
        console.log(data);
    });
```

这样能行，但响应对象还有一个名为 `.json()` 的便捷快捷方法，可以为我们完成这项工作：

```
fetch('images/slides/slides.json')
    .then(response => response.json())
    .then(data => {
        console.log(data);
    });
```

现在，我们可以将其构建到我们的代码中。

用于管理幻灯片的函数叫做 `ajaxSlides()`：

```
function ajaxSlideshow() {
}
```

在函数中，我们可以获取 JSON 文件并处理它：

```
function ajaxSlideshow() {
    fetch('images/slides/slides.json')
        .then(response => response.json())
        .then(data => {
        });
}
```

只剩最后一步了。

### 运行幻灯片

实际上，所有繁重的工作都已经完成了。回想一下，我们在库文件中保存了一个名为 `doSlides()` 的函数：

```
doSlides(images, containerSelector, delay)
```

在这种情况下，`images` 对象将来自 `fetch()` 函数，而 `containerSelector` 只是对 HTML 中 `div` 的引用：

```
function ajaxSlideshow() {
    fetch('images/slides/slides.json')
        .then(response => response.json())
        .then(data => {
            doSlides(data, 'div#slides', 3000);
        });
}
```

延迟设置为默认值 `3000`，但你可以根据自己的喜好更改它，特别是在测试时。

### Ajax 灯箱画廊

对于 Ajax 幻灯片来说，唯一显著不同的是图片集合的来源。以前，集合是在 JavaScript 代码中硬编码的。使用 Ajax，你可以从加载的文件中获取集合。

然而，对于灯箱画廊来说，情况要稍微复杂一些。画廊最初使用的是 HTML 链接中的图片集合，然后我们劫持这些链接以用于灯箱。在当前的文档中，你只会找到以下内容：

```
你肯定会同意，这并不多。我们将结合使用 Ajax 和少量 DOM 操作来填充目录，然后让我们的库函数 `doLightbox()` 完成其余工作。
```

JavaScript 代码将位于 `ajaxLightbox()` 函数中。

#### 获取画廊数据

灯箱画廊中的图片与幻灯片中的图片相同。这意味着我们可以采用与幻灯片相同的方式来开始：

```
function ajaxLightbox() {
    fetch('images/slides/slides.json')
        .then(response => response.json())
        .then(data => {
        });
}
```

从这里开始，任务就是使用这些数据来生成画廊。

#### 创建画廊元素

计划是将画廊元素添加到容器中。首先，我们将设置一个变量来引用容器：

```
function ajaxLightbox() {
    let catalogue = document.querySelector('div#catalogue');
    ...
}
```

# Ajax 与 API

请记住，`fetch()` 函数返回的数据将是一个对象数组。我们可以遍历这个数组：

```javascript
function ajaxLightbox() {
    let catalogue = document.querySelector('div#catalogue');
    fetch('images/slides/slides.json')
        .then(response => response.json())
        .then(data => {
            data.forEach(item => {
                //  create gallery element
            });
        });
}
```

每个画廊元素都是一个包含在锚点中的图像：我们将创建这些元素并将它们添加到画廊中。

在前面的代码中，我们大多使用 `document.createElement()` 来创建元素并添加到容器中。不过，也可以使用 HTML 代码创建元素，并通过 `.insertAdjacentHTML()` 添加：

```javascript
//  Not Yet
data.forEach(item => {
    catalogue.insertAdjacentHTML(
        'beforeend',
        ``
    );
});
```

新元素被添加在 `beforeend` 位置，这表示它被添加到容器的末尾。我们将 HTML 字符串放在反引号中，因为稍后需要用到模板字面量。

对于实际的元素，有很多值需要替换。不过，我们可以将 HTML 字符串拆分成几行：

```javascript
data.forEach(item => {
    catalogue.insertAdjacentHTML(
        'beforeend',
        `
        `
    );
});
```

当然，你也可以把它写在一行里，但在这个书中显然空间不够。

一旦画廊填充完成，你就可以将其传递给 `doLightbox()` 函数：

```javascript
function ajaxLightbox() {
    let catalogue = document.querySelector('div#catalogue');
    //  fetch ...
    doLightbox('div#catalogue');
}
```

完成后的函数应该如下所示：

```javascript
function ajaxLightbox() {
    let catalogue = document.querySelector('div#catalogue');
    fetch('images/slides/slides.json')
        .then(response => response.json())
        .then(data => {
            data.forEach(item => {
                catalogue.insertAdjacentHTML(
                    'beforeend',
                    `
                    `
                );
            });
            doLightbox('div#catalogue');
        });
}
```

你可能会注意到，画廊中的图片比原始版本更多。这正是此技术的一个优势：画廊变得更加灵活。

## 访问数据库

Ajax 最强大的应用之一在于数据源是动态的情况。这类数据通常来自服务器端的数据库。

你不能直接使用 JavaScript 访问数据库。你可以做的是通过 Web 服务器向数据库服务器发起请求。其思路是，Web 服务器应当运行着某个监听此类请求的应用程序。

如果你在轻量级 Web 服务器（例如 Micro Web Server）上运行示例网站，则无法尝试此操作。如果你碰巧通过更强大的工具（例如 XAMPP）甚至完整的 Web 服务器来运行它，那么有一个 PHP 脚本可以完成这项工作。

在本节中，我们假设你并未运行此类 Web 服务器。相反，我们将依赖在线网站上的一个应用程序。

用于支持 Ajax 访问的服务通常被称为 API，这是一个非常古老的术语。它的意思是应用程序编程接口。实际上，它仅意味着该服务会接收格式正确的请求，并返回某种结果。

在本示例中，我们将使用一个提供世界各国信息的 API。

我们使用的服务将返回 JSON 格式的数据。但并非总是如此。另一种常见的格式是 XML，JavaScript 处理 XML 需要额外做很多工作。

并非所有 API 都可以免费使用。有些需要注册，有些则需要付费。

无论如何，访问其他服务器上的数据并不总是被允许的。服务器需要设置为允许来自其他来源的请求。那些提供 API 的服务器自然已经完成了这样的设置。

## 使用 API

API 通常以带有查询字符串的 URL 形式存在。查询字符串是问号后面的部分，用于提供额外信息。

当 API 开放使用时，你通常会看到相关使用说明。在本例中：

* 单独的 URL 会返回国家列表。
* 查询字符串 `id=...` 会返回特定国家的详细信息。

在许多情况下，你还需要提供某种密钥来验证用户身份。

在实时示例中，URL 是：

```
https://pure-javascript.net/resources/countries.php
```

你可以在浏览器中尝试这个链接。

你会发现该 URL 直接返回一个 JSON 字符串，其中包含国家 ID 和名称的列表。

```json
[
    { "id": "af", "name": "Afghanistan" },
    { "id": "al", "name": "Albania" },
    { "id": "dz", "name": "Algeria" },
    ...
    { "id": "zm", "name": "Zambia" },
    { "id": "zw", "name": "Zimbabwe" }
]
```

你的浏览器可能会解析 JSON，并以更偏向数据视图的方式呈现。你可能需要查看原始数据才能看到实际的 JSON 字符串。

你可以通过在 URL 后添加查询字符串来进行测试：

```
https://pure-javascript.net/resources/countries.php?id=au
```

现在你将得到一个国家的详细信息：

```json
{
    "id":"au",
    "name":"Australia",
    "local_name":"Australia",
    "capital":"Canberra",
    "alpha3":"AUS",
    "tld":"au",
    "numeric":"036",
    "phone":"61",
    "continent":"Australia",
    "population":"22751014",
    "area":"7741220",
    "coastline":"25760",
    "currency":"AUD"
}
```

我们不会使用所有这些详细信息。

## 使用 Ajax 调用 API

在示例页面底部附近有一个表单，用于显示某些国家详细信息。以下是该表单的简化版本：

```html
选择国家
本地名称
顶级域名
大洲
货币
```

表单元素大部分是用于接收数据的输出元素。然而，表单中包含一个 `select` 元素，它将显示为下拉菜单。目前 `select` 元素是空的：我们需要用 JavaScript 来填充它。

对于普通的 `select` 元素，其选项应为 `option` 元素：

```html
<option>Apple</option>
<option>Banana</option>
<option>Cherry</option>
```

如果提交表单，`select` 项的值将是所选选项的 value。

### 准备 `select` 元素

我们将把所有代码放在 `ajaxCountries()` 函数中。

首先，我们可以找到这个表单：

```javascript
function ajaxCountries() {
    let countriesForm
        = document.querySelector('form#country-details');
}
```

下拉菜单的问题在于总会有一个选项被选中。通常，比较好的做法是先提供一个实际上不选任何内容的选项，类似这样：

```html
<option value="">选择一个国家 ...</option>
```

你可以使用 `document.createElement()` 来创建 `option` 元素，但有一个专门的函数可以简化这个过程：

```javascript
new Option(text, value)
```

严格来说，`Option()` 被称为构造函数，用于生成对象；构造函数通常与 `new` 关键字一起使用。在这种情况下，生成的对象将是一个 `option` 元素，这正是我们所需要的。

我们可以直接将新的 `option` 元素追加到 `select` 元素中：

```javascript
function ajaxCountries() {
    let countriesForm
        = document.querySelector('form#country-details');
    countriesForm.elements['id'].append(
        new Option('选择一个国家 ...', '')
    );
}
```

记住，`select`元素是表单的一部分，因此我们可以通过`countriesForm.elements`来引用它。这里，文本是`选择...`，值将是一个空字符串。下一步是从 API 中获取国家列表，并用其填充`select`元素的其余部分。

首先，我们定义获取数据的 URL：

```javascript
function ajaxCountries() {
    let url = 'https://pure-javascript.net/resources/countries.php';
    ...
}
```

这样，在后续使用时会简化代码。

接下来，我们可以从该 URL 获取国家列表：

```javascript
function ajaxCountries() {
    let url = 'https://pure-javascript.net/resources/countries.php';
    let countriesForm = document.querySelector('form#country-details');
    countriesForm.elements['id'].append(
        new Option('选择一个国家 ...', '')
    );
    fetch(url)
        .then(response => response.json())
        .then(data => {
            //  添加选项
        });
}
```

`fetch()`函数将获取一个包含国家 ID 和名称列表的 JSON 对象，我们将把它转换成`option`元素。

我们可以遍历数据数组，并使用`new Option()`来添加`option`元素。文本将是每个项的`name`属性，而值将是`id`属性：

```javascript
function ajaxCountries() {
    ...
    fetch(url)
        .then(response => response.json())
        .then(data => {
            data.forEach(item=> {
                countriesForm.elements['id'].append(
                    new Option(item.name, item.id)
                );
            });
        });
}
```

如果你现在尝试，会看到下拉菜单中填充了国家列表。下一步，当从菜单中选择一个国家时，将填充表单的其余部分。

## 响应选择操作

表单元素的值可能会发生变化，当发生这种变化时，会触发一个`change`事件。这也是为什么菜单应该从空值开始的原因：这样可以确保即使选择了第一个真实值，也总会触发 change 事件。

你可以通过简单地赋值`.onchange`事件属性来添加事件监听器：

```javascript
function ajaxCountries() {
    ...
    countriesForm.onchange = event => {
    };
}
```

当选项发生变化时，将使用下拉菜单的当前值再次发起 fetch 请求：

```javascript
function ajaxCountries() {
    ...
    countriesForm.onchange = event => {
        fetch(`${url}?id=${event.target.value}`)
            .then(response => response.json())
            .then(data => {
                //  处理结果
            });
    };
}
```

请注意，我们使用了模板字面量来拼接字符串，以便同时使用`url`变量和`event.target.value`。查询字符串由字符串`?id=`和所选国家项的当前值组成。

当我们从`fetch()`函数获得结果后，可以填充表单的其余部分：

```javascript
function ajaxCountries() {
    ...
    countriesForm.onchange = event => {
        fetch(`${url}?id=${event.target.value}`)
            .then(response => response.json())
            .then(data => {
                countriesForm.elements['local-name'].value = data['local_name'];
                countriesForm.elements['tld'].value = data['tld'];
                countriesForm.elements['continent'].value = data['continent'];
                countriesForm.elements['currency'].value = data['currency'];
            });
    }
}
```

我们再次使用`response.json()`将接收到的 JSON 字符串转换为对象。转换后，我们填充四个输出元素的值。要完成这项工作，还有一项任务。在某些时候，用户可能会选择第一个选项`选择...`。作为开发者，你需要意识到潜在的错误，并尽量赶在用户之前进行处理。

请记住，第一个选项的值是一个空字符串。当我们从服务器返回结果时，不会有任何数据。我们可以使用空值合并运算符（`??`）来检测这种情况：

```javascript
function ajaxCountries() {
    ...
    countriesForm.onchange = event => {
        ...
        .then(data => {
            countriesForm.elements['local-name'].value = data['local_name'] ?? '';
            countriesForm.elements['tld'].value = data['tld'] ?? '';
            countriesForm.elements['continent'].value = data['continent'] ?? '';
            countriesForm.elements['currency'].value = data['currency'] ?? '';
        });
    }
}
```

缺失的`data`值会是`undefined`。使用空值合并运算符，我们可以将其替换为空字符串`''`。

完成后的代码如下所示：

```javascript
function ajaxCountries() {
    let url = 'https://pure-javascript.net/resources/countries.php';
    let countriesForm = document.querySelector('form#country-details');
    countriesForm.elements['id'].append(new Option('选择...',''));

    fetch(url)
        .then(response => response.json())
        .then(data => {
            data.forEach(item=> {
                countriesForm.elements['id'].append(
                    new Option(item.name,item.id)
                );
            });
        });

    countriesForm.onchange = event => {
        fetch(`${url}?id=${event.target.value}`)
            .then(response => response.json())
            .then(data => {
                countriesForm.elements['local-name'].value = data['local_name'] ?? '';
                countriesForm.elements['tld'].value = data['tld'] ?? '';
                countriesForm.elements['continent'].value = data['continent'] ?? '';
                countriesForm.elements['currency'].value = data['currency'] ?? '';
            });
    }
}
```

在 Ajax 及其相关函数成为 JavaScript 标准组成部分之前，从服务器获取新数据需要重新加载整个页面，这会给用户带来糟糕的体验。

## 总结
本章我们学习了如何使用 Ajax 从 web 服务器获取额外数据。在幻灯片和灯箱图库中，我们之前已经以图像形式获取过额外数据。而本章，我们着重探讨了如何获取文本以及更复杂的数据。

### 使用 `fetch()` 函数
需要克服的主要问题是**时机**。由于 web 请求（包括 Ajax 请求）需要一定的时间，代码的编写必须确保对数据的处理推迟到数据到达之后。`fetch()` 函数允许你向服务器发送 Ajax 请求，并返回一个 `Promise` 对象。该 `Promise` 对象可以附加代码，以便在数据到达后对其进行处理。一个简单的 `fetch()` 应用在请求数据后可以分两步操作：第一步是从响应中提取数据，第二步是处理提取到的数据。如果数据是简单的文本，则可以立即使用，例如用于填充其他元素的内容。

### 使用 JSON
JavaScript 支持一种对象字面量格式，允许直接在代码中定义一个对象或一个对象数组。JSON 字符串格式是一种类似于对象字面量的字符串，可以使用内置的 JavaScript 函数将其转换为真实的对象或对象数组。JSON 的真正优势在于能够将复杂数据表示为单个字符串。字符串可以轻松地保存在文本文件中或通过互联网传输，而复杂数据则不那么容易管理。JSON 数据可以来自服务器上保存的文件，也可以像 PHP 和数据库那样，通过服务器端的某些处理动态生成。JavaScript 有一个内置函数 `JSON.parse()`，用于转换 JSON 字符串。但 `fetch()` 的响应对象本身有一个 `.json()` 方法，它可以对接收到的 JSON 字符串执行相同的转换。

### 与 DOM 交互
之前的项目通过更改或添加元素来与 DOM 交互。在这个项目中，我们对此进行了扩展：使用 `.innerHTML` 填充元素，使用 `.insertAdjacentHTML()` 为灯箱图库创建锚点和图像，以及使用 `new Option()` 填充 `select` 元素。我们还学习了如何使用 `change` 事件响应表单，以触发另一个 `fetch()` 并填充表单的其余部分。

### Ajax API
有许多第三方以 Ajax API 的形式提供数据服务。这些服务通常返回 JSON 字符串，尽管有些使用其他形式的数据，例如 XML。访问第三方 API 需要对方的服务器接受连接。有些服务会限制只有注册用户（可能还需要付费）才能访问。
