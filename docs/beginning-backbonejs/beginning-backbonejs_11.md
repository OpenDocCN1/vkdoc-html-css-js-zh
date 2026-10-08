# 5. 路由器和事件

到目前为止，我们构建的所有内容都非常简单，由单个页面组成，页面中包含一个在视图中呈现的模型或集合。如果你考虑使用 Backbone，不太可能将应用程序范围局限于如此基础的结构。相反，你的应用程序将由多个视图组成，这些视图需要使用事件相互通信。同样常见的是，你可能希望通过某种 URL 方案来管理导航。

本章将探讨路由器和事件，它们将为任何大型 Backbone 应用程序提供粘合剂。

## Backbone 路由器

路由是 Web 应用程序中一个可添加书签的部分，通常由末尾的哈希标识符或片段表示。与此同时，路由器是管理应用程序中定义的路由的代码。遵循此路由应能将你带到整体文档的特定部分。另一个有用的类比是将路由器视为书籍的目录。通过查阅目录，你就知道在哪里可以找到特定的信息。

路由器通过两种不同的方案观察应用程序 URL 的变化来检测路由变化。第一种方案使用哈希标签，例如 `http://apress.com/#myapp`，这适用于所有浏览器。对于支持 HTML5 标准的浏览器，第二种方案是利用 History API 并使用更“标准”的 URL，例如 `http://apress.com/myapp`。无论采用哪种方式，你都无需担心目标浏览器，因为 Backbone 会为你透明地回退到哈希/片段解决方案。

### 创建路由

在 Backbone 中创建路由遵循与库中所有对象相同的模式，即使用 `.extend` 方法来定义自定义路由。

```
MyRouter = Backbone.Router.extend({

});
```

接下来，你需要将特定的 URL 模式与当导航到该 URL 片段时会被调用的函数相匹配。这通过向 `extend` 函数传递一个 `routes` 哈希对象来实现。

```
MyRouter = Backbone.Router.extend({

        routes: {'hello' : 'sayHello'},

});
```

上述代码设置了一个路由，因此如果 URL 以 `#hello` 结尾，将调用名为 `sayHello` 的函数。该函数应像这样与路由一同定义：

```
MyRouter = Backbone.Router.extend({

        routes: {'hello' : 'sayHello'},

        sayHello: function(){

                console.log('Saying hello');

        }

});
```

为了让路由开始监听路由变化，需要创建一个新的路由实例。

```
var router = new MyRouter();
```

最后，为了监控哈希片段的变化并调用相应的事件，你需要初始化 `Backbone.history`。

```
Backbone.history.start();
```

现在，当访问 `http://localhost/index.html#hello` 时，你将在控制台中看到 “Saying hello” 字样。

由于大多数 Backbone 应用被设计为单页 Web 应用，你通常只需要创建一个路由。如果你想出于代码分离或结构考虑而创建独立的路由，主要需注意避免路由冲突。

现在我们已经了解了如何创建一个简单的路由，接下来让我们更仔细地研究其组成部分以及如何构建更复杂的路由。

### Backbone.History

`Backbone.history` 是库提供的一个抽象，用于处理 `hashchange` 事件或 HTML5 的 `pushState`。当监测到 URL 变化时，相应的路由会与你自定义路由中定义的回调函数进行匹配。

正如你在本节开头的示例中所见，需要调用 `Backbone.history.start()` 来开始监听变化事件。如果没有这一步，你的路由将无法成功运行。值得注意 `start()` 函数返回的布尔值；如果没有路由匹配当前 URL，它将返回 `false`。

### 使用 pushState

如果你决定在应用中使用 HTML5 的 `pushState` 来提供不带哈希片段的 URL，可以在启动历史记录时进行指定。

```
Backbone.history.start({pushState:true});
```

不过，你可能会注意到，直接粘贴示例路由 `http://localhost/hello` 会导致 404 错误。这是因为你需要配置服务器，将所有 URL 重定向到 `index.html` 页面，以允许 Backbone 处理路由。

你可以通过修改 Apache 服务器的 `httpd.conf` 文件来使用 `mode_rewrite.c` 模块来实现这一点。配置如下所示：

```
RewriteEngine On

RewriteBase /

RewriteCond %{REQUEST_FILENAME} !-f  RewriteCond %{REQUEST_FILENAME} !-d

RewriteRule . index.html [L]
```

或者，你可以使用 Node.js 托管页面，并让它提供一些智能逻辑来处理此类路由。以下是一个简单的 Node.js 服务器示例，用于服务你的 Backbone 应用：

```
var express = require('express');

var app = express();

app.use(express.static(__dirname));

app.use('/chapter5/hello', express.static(__dirname + "/chapter5"));

app.listen(3010);
```

假设你已经安装了优秀的 `express` 模块，上述代码可以使用 `node server.js` 命令运行。将你所有的客户端源代码放在与 `server.js` 同级的 `chapter5` 目录中，第二个 `app.use` 规则便会生效。该规则规定，如果导航到 `/chapter5/hello`，则由 `chapter5` 目录提供服务；这将强制应用跳转到 `index.html`。

由于应用并非从域的根目录 `/` 提供服务，请使用 `root` 参数来指定 Backbone 应用的提供位置。就前面的示例而言，我们使用 `chapter5` 作为 Backbone 应用的根目录：

```
Backbone.history.start({pushState:true, root:"/chapter5/"});
```

通过访问 `http://localhost:3010/chapter5/hello`，你现在会看到 `sayHello` 路由被调用。对于如此简单的操作来说，这似乎需要大量工作，但它确实能为你的应用带来更简洁的 URL 方案。

请记住，你需要在创建完所有应用路由以处理初始 URL 后，调用 `Backbone.history.start()` 或 `Backbone.history.start({pushState: true})`。

### 所有应用通用的路由

有两个路由值得添加到所有路由中。一个是初始路由，即空字符串。

```
MyRouter = Backbone.Router.extend({

        routes: {'' : 'start'},

        ....

        start: function(){

                console.log('Initial route invoked');

        },

        ....

});
```

你还应提供一个默认路由；当使用了路由未配置处理的 URL 时，会调用此路由。该路由使用 `*default` 模式创建。

```
MyRouter = Backbone.Router.extend({

        routes: { '' : 'start',

                             '*default': 'defaultRoute' },

                //其他路由定义代码

        start: function(){

                        console.log('Initial route invoked');

                },

        defaultRoute: function(){

                console.log('Router does not handle this route');

                },

                //其他路由定义代码

});
```

### 为路由添加参数

定义路由时传入的 `routes` 哈希表可将应用内的 URL 与函数关联起来。你还可以为这些映射的函数添加参数，从而在 Backbone 应用中提供更有用的书签。

你可以使用 `:param` 片段来设置路由，使其能够处理特定模式的参数。该参数将匹配两个斜杠之间的单个 URL 组成部分。例如，要使用之前创建的 `hello` 路由，在类似 `http://localhost:3010/chapter5/#hello/james` 的 URL 中接收一个 `name` 参数，你需要按如下方式指定路由。请确保你定义的路由始终列在默认路由之前。

`routes : {'hello/:name': 'sayHello'},`

现在函数定义改为接受 `name` 参数。

```
sayHello: function(name){
        console.log('Saying hello to ' + name);
}
```

可以在路由中指定任意数量的此类参数。你可以通过添加第二个参数来处理诸如 `#hello/james/backbone` 这样的 URL。

`routes : {'hello/:name/:skill': 'sayHello'},`

现在可以更新函数，使其处理额外的参数。

```
sayHello: function(name, skill){
        console.log('Saying hello to ' + name + ' ' + skill + ' author');
}
```

然而，前一个路由的定义要求必须指定两个参数才能进入该路由。你也可以通过将参数放入方括号中来指定可选参数。

`routes : {'hello/:name(/:skill)': 'sayHello'},`

前一个路由要求使用 `#hello/james` 或 `#hello/james/backbone` URL 格式。路由映射到的函数应当检查是否有参数为 `null`。

```
sayHello: function(name, skill){
        if(skill !== null){
                console.log('Saying hello to ' + name + ' ' + skill + ' author');
        }
        else{
                console.log('Saying hello to ' + name );
        }
}
```

请注意，你可以在任何路由参数前加上纯文本。例如，要在参数前加上 `chapter`，以形成类似 `#hello/james/chapter5` 的模式，你可以像这样使用路由：

`routes : {'hello/:name/chapter:chapterNumber': 'sayHello'},`

这将产生两个变量：`name`（其值将设为 `james`）和 `chapterNumber`（其值将设为 `5`）。

最后，你可以使用 `*` 传递一个通配符（splat），它将匹配任意数量的 URL 组成部分。如果你有一个想要处理的路径，这会非常有用。例如，此处定义的路由允许使用类似 `#hello/james/chapter5/section1` 的 URL：

`routes : {'hello/:name/*path': 'sayHello'},`

在上述代码的情况下，`path` 变量的值将是 `chapter5/section1`。

## 路由事件

每当导航到已定义路由的 URL，或在浏览器中使用后退按钮时，都会触发一个事件，其名称为映射的路由器函数名加上前缀 `route:`。在本例中，事件名将是 `router:sayHello`。

```
router.on("route:sayHello", function(page) {
});
```

如果其他对象需要监听应用内导航的变化，这会非常有用。

## 手动创建路由

除了在创建路由器时以哈希表形式传递路由外，你还可以手动创建路由。通常，这会在你创建的 `initialize` 函数中完成，该函数在创建路由器的新实例时会被调用。

`route` 函数接受三个参数：路由、名称，以及路由创建时将被调用的函数。要实现与之前路由示例相同的结果，你可以像这样调用简单的 `hello` 路由：

```
initialize: function(options){
        this.route('hello', sayHello);
},
```

不过，你可以用这个函数做更多事情。`route` 参数可以像之前的例子那样是一个路由字符串，但你也可以使用正则表达式通过 `this.route()` 来定义路由。

第二个参数是路由的名称。通常这将是你想要调用的函数名称。但是，你可以为路由添加第三个回调参数。这意味着为路由事件触发的 `router:name` 将匹配第二个 `name` 参数，而不是函数名。实际执行的函数将是回调函数，而不是匹配路由名的函数。

```
this.route('hello', 'sayHello',
        function(){ console.log('callback invoked');
});
```

在这个例子中，当路由匹配时，控制台将输出“callback invoked”，而不是执行 `sayHello` 函数。

## 编程式导航

在应用中，可能存在你想要保存为 URL 的节点，以便用户可以书签该节点以便稍后导航。为此，你可以使用 `navigate` 函数，前提是你已经调用了 `Backbone.history.start()`。

以下示例展示了当调用 `showBook` 函数时，`navigate` 如何在浏览器地址栏中创建一个新的 URL：

```
function showBook(name){
        console.log('Show book ' + name);
        router.navigate('book/'+name);
}

showBook('Backbone');
```

现在 URL 将显示模式 `#book/Backbone`。即使已经创建了一个处理此模式的路由，它也不会被上述代码调用。如果你确实希望触发路由器函数运行，你只需要在导航时传递一个额外的 `trigger` 参数即可。

```
MyRouter = Backbone.Router.extend({
        routes: {'book(/:name)' : 'displayBook' },
        ....
        displayBook: function(name){
                console.log('Displaying ' + name);
        },
        .....
});
```

在设置了上述路由的情况下，以下代码将触发 `displayBook` 路由器函数：

`router.navigate('book/'+name, {trigger: true});`

最后，如果你想要更改浏览器地址栏的 URL，但阻止程序在浏览器历史记录中添加一条记录，请确保将 `replace` 选项设置为 `true`。

`router.navigate('book/'+name, {trigger: true, replace:true});`

## 作为控制器的路由器

在探讨模型-视图-*模式如何应用于 Backbone 时，我们提到路由器可以承担部分控制器的职责。毫无疑问，路由器很好地完成了其主要任务：处理应用中 URL 的变化，并将 URL 映射到应用中的函数。

通过将路由器绑定到应用模型，它可以被用作应用级别的控制器。由于 URL 的变化隐含了应用状态的改变，模型可以随每个应用处理程序一同更新。假设我们有一个简单的模型用于追踪应用状态，本例中是当前书籍。

```
AppModel = Backbone.Model.extend({

    defaults: {
        currentBook: 'Beginning Backbone',
    }

});
```

可以添加变更监听器，以便任何被修改的属性都能调用其他操作或重新渲染视图。

```
initialize: function(){
    this.on("change", function(){
        if(this.hasChanged('currentBook')){
            alert('当前图书已更改');
        }
    });
},
```

现在，这个模型可以在创建时作为参数提供给路由器。

```
var appModel = new AppModel();
var router = new MyRouter({model: appModel});
```

路由器的 `initialize` 函数会接收这个模型参数。这样，由于路由是在路由器中被调用的，模型就能获知这种应用状态的变化。

# Backbone路由器

```javascript
MyRouter = Backbone.Router.extend({
    model : null,
    routes: {'book(/:name)' : 'displayBook',
        '' : 'start',
        '*default': 'defaultRoute' },
    initialize: function(options){
        this.model = options.model;
    },
    ....
    displayBook: function(name){
        console.log('正在显示 ' + name);
        this.model.set('currentBook', name);
    },
    ...
});
```

这为我们的应用提供了一种模型-视图-控制器架构。一个共享的应用模型被同时传递给路由器和应用视图。路由器无需了解应用视图的任何信息，反之亦然。

当然，你可以应用多种变体。正如本章下一节所见，将应用路由与事件相结合会催生多种架构可能性。

## Backbone 事件

在介绍模型、集合、视图和路由器时，我们遇到了许多不同的事件，因为 `Backbone` 允许将事件添加到任何对象上。`Backbone` 事件模块使我们能够绑定和触发内置或自定义事件。

所有 `Backbone` 对象（`View`、`Router`、`Collection`、`Model`）都包含了 `Backbone` 事件模块，但你也可以像下面这样，将 `Backbone` 事件功能扩展到任何基础对象上：

```javascript
var object = {};
_.extend(object, Backbone.Events);
```

下一节将在列举库内置事件之前，深入探讨事件在 `Backbone` 中的工作原理。

### 绑定事件

绑定是指设置一个对象来监听特定事件的发生。这是通过 `.on` 函数完成的。该函数接受三个参数：事件名称、事件触发时要调用的函数，以及一个可选的执行上下文。

例如，在我们的模型对象中，如果希望某个属性发生变化时收到通知，需要监听的事件就是 `change`。

```javascript
var model = new Book();
model.on("change", function(){
    console.log('某个属性已被更改');
});
```

回想一下，变更监听器包含一种形式，可以根据特定属性的变化来触发事件。例如，如果只想监听 `author` 属性的变化，事件名称应为 `change:author`。

```javascript
model.on("change:author", function(){
    console.log('作者属性已被更改');
});
```

你可以用空格分隔不同的事件名称，让 `.on` 事件处理程序处理多个不同事件。例如，要监听 `Book` 对象的作者或标题的变化，可以这样写：

```javascript
model.on("change:author change:title", function(){....});
```

你可以使用 `all` 事件名称添加一个特殊的监听器，它会监控所有事件。这允许你提供一个对象，将事件委托给适当的处理对象。

```javascript
monitor.on('all', function(eventName){
    delegateObject.trigger(eventName);
});
```

最后，还可以像路由器中定义路由那样，通过提供 `事件名称: 处理程序` 的哈希表来设置事件处理。你甚至可以将事件委托给其他对象。例如，如果我们创建了一个简单的视图来渲染书籍模型，那么可以添加一个函数，允许外部对象（如路由器）更新该视图：

```javascript
BookView = Backbone.View.extend({
    updateTitleView: function(){
        console.log('正在更新标题视图');
    }
});

var bookView = new BookView();
```

现在，当创建 `Book` 对象的实例时，我们可以向 `.on` 函数传递一个事件哈希表，指示当 `name` 属性发生变化时调用 `updateTitleView` 函数。

```javascript
var thisBook = new Book({name : 'Beginning Backbone',
    author: 'James Sugrue'});
thisBook.on({ 'change:name': bookView.updateTitleView});
```

更改书的 `name` 属性将触发事件，进而调用 `bookView` 对象中的 `updateTitleView`。

```javascript
thisBook.set('name', "Beginning Backbone.js")
```

这种事件哈希表可以作为一种更简洁的方式，将所有对象的事件定义在一个地方，而无需将回调函数与事件定义内联在一起。

### 解除事件绑定

当应用状态发生变化时，你可能不再需要监听某些事件。这种关闭监听器的行为称为解除绑定，通过 `.off` 函数实现。

可以解除对特定事件的所有处理程序。例如，如果不再希望监听任何对象中书名的变化，以下代码将解除所有监听器：

```javascript
thisBook.off('change:name');
```

你也可以解除特定的处理程序。例如，如果之前使用的 `bookView` 不再可见，可以仅解除它对名称变化事件的处理程序，而保持其他处理程序仍然有效。

```javascript
thisBook.off('change:name', bookView.updateTitleView);
```

如果一个处理程序被用于多个事件且不再需要，可以使用 `null` 作为事件名称，将其与所有事件解除关联。

```javascript
thisBook.off(null, bookView.updateTitleView);
```

你还可以通过不带任何参数调用 `.off` 来移除特定对象上的所有监听器。

```javascript
thisBook.off();
```

这将移除此对象上的所有事件处理，包括 `Backbone` 内部事件。

### 只监听一次事件

有时，只需监听并响应一次事件，之后便不再关心，无需在事件处理完毕后费心解除绑定。

`Backbone` 提供了 `.once` 函数来实现此功能。它接受与 `.on` 处理程序相同的参数。

```javascript
thisBook.once({ 'change:name': bookView.updateTitleView});
```

在控制台中，如果你每次都使用不同的名称执行以下操作，你会看到它仍然只会被处理一次：

```javascript
thisBook.set('name', 'Beginning Backbone.js')
```

你会发现这在初始化期间检查事件或等待事件“下一次”触发时非常有用。

### 监听其他对象的事件

我们已了解模型对象中的事件处理器如何调用独立视图对象中的函数。使用 `listenTo` 函数，一个对象（视图）也可以监听另一个对象（模型）上的所有事件。

理论上，以下两行代码是等效的：

`thisBook.on({ 'change:name': bookView.updateTitleView });`

`bookView.listenTo(thisBook, 'change:name', bookView.updateTitleView);`

然而，使用 `listenTo` 来追踪另一个对象上的事件更具优势，因为 `stopListening` 函数允许对象通过一次简单的函数调用完全断开所有事件连接。

`bookView.stopListening(model);`

如果该对象正在监听多个不同对象的事件，只需在 `stopListening` 函数中不传入任何参数，即可一次性注销所有监听。

`bookView.stopListening()`

为了保持一致性，`Backbone` 提供了与 `.once` 函数使用方式相同的 `listenTo` 方法，即使用 `listenToOnce` 来处理事件：事件只处理一次，然后自动断开连接。

`bookView.listenToOnce(thisBook, 'change:name', bookView.updateTitleView);`

### 触发事件

到目前为止，我们研究的所有事件都是由 `Backbone` 内部触发的。我们也可以在不执行本应触发事件的动作的情况下，直接触发事件。

`.trigger` 函数允许将一个事件名称及任意数量的参数传递给事件处理器。你可以通过以下代码模拟一个 `change:name` 事件：

`thisBook.trigger('change:name', {});`

当然，你也可以触发自定义事件，而不是模仿内置事件。我们将在后面的“创建自定义事件”部分深入探讨。

### 内置事件

如前所述，`Backbone.Event` 被包含在每个对象中。这些对象包括：

-   `Backbone`

-   `Backbone.Model`

### Backbone 事件

`Backbone` 提供了许多内置事件，如下一节所述。当事件被触发时，回调函数中可以获取多个参数，这些参数在以下表格中列出。

#### 集合事件

表 5-1 描述了用于处理集合的 `Backbone` 事件。

**表 5-1.** `Backbone.Collection` 的事件

| 事件 | 回调参数 | 触发时机... |
| --- | --- | --- |
| `add` | `model`, `collection`, `options` | 模型被添加到集合中时 |
| `remove` | `model`, `collection`, `options` | 模型从集合中移除时 |
| `reset` | `collection`, `options` | 集合的全部内容被替换时 |
| `sort` | `collection`, `options` | 集合被重新排序时 |

#### 模型事件

表 5-2 描述了处理 `Backbone.Model` 时可使用的事件。

**表 5-2.** `Backbone.Model` 的事件

| 事件 | 回调参数 | 触发时机... |
| --- | --- | --- |
| `change` | `model`, `options` | 模型中的任何属性发生改变时 |
| `change:[attribute]` | `model`, `value`, `options` | 模型中的特定属性发生改变时 |
| `destroy` | `model`, `collection`, `options` | 模型被销毁时 |
| `request` | `model`, `xhr`, `options` | 模型开始向服务器发送请求时 |
| `sync` | `model`, `xhr`, `options` | 模型与服务器成功同步时 |
| `error` | `model`, `xhr`, `options` | 模型的保存请求在服务器端失败时 |
| `invalid` | `model`, `error`, `options` | 客户端的模型验证失败时。当定义了 `validate` 函数且调用模型的 `save` 函数时，通常会捕获此事件。 |

#### 路由器和历史事件

表 5-3 描述了与 `Backbone.Router` 类相关的事件。

**表 5-3.** `Backbone.Router` 的事件

| 事件 | 回调参数 | 触发时机... |
| --- | --- | --- |
| `route:[name]` | `params` | 特定的路由被匹配到时 |
| `route` | `router`, `route`, `params` | 任何路由被匹配到时（由 `history` 或 `router` 触发） |

#### 全局事件

表 5-4 描述了在 `Backbone` 应用中全局使用的事件。

**表 5-4.** `Backbone` 应用中的全局事件

| 事件 | 回调参数 | 触发时机... |
| --- | --- | --- |
| `all` | `eventName` | 整个应用中任何事件被触发时 |

### 创建自定义事件

除了使用 `Backbone` 提供的任何事件外，你还可以创建自己的事件，以便在应用中的任何对象之间实现解耦通信。例如，当一个模型更新时，你可能希望另一个模型或视图对此事件作出响应。

想象一个简单的例子：如果 `Book` 模型的 `name` 和 `author` 属性都为 `null`，你想要触发一个特定事件。事件命名的惯例是使用冒号分隔的标识符，例如 `model:useless`。

由于事件在全局 `Backbone` 对象上可用，这意味着你可以在代码中的任何位置设置全局监听器。

```js
Backbone.on('model:useless', function(){
    console.log('模型无用的全局监听器被调用');
    // 在此处进行更具体的处理
});
```

触发这个全局事件可以在模型的变更监听器中处理，该监听器用于处理这两个属性中的任何一个发生改变的情况。

```js
thisBook.on('change:name change:author', function(model, value, options){
    if(model.get('name') === null && model.get('author') === null){
        console.log('现在触发事件');
        Backbone.trigger('model:useless', {});
    }
});
```

现在，如果你在控制台中执行以下两条命令，该事件就会被触发：

`thisBook.set('name', null);`

`thisBook.set('author', null);`

请注意，你也可以通过应用的事件总线传递参数。例如，可以传递触发该事件的模型。

`Backbone.trigger('model:useless', {model:model});`

然后，处理回调函数就可以处理任何可用的参数。

```js
Backbone.on('model:useless', function(data){
    console.log(data.model.get('name'));
    console.log('模型无用的全局监听器');
});
```

### 监听 DOM 事件

正如我们在第 4 章中所述，我们可以监听 `DOM` 上的事件，并将这些事件绑定到回调函数。这些事件键总是以 `jQuery` 选择器的形式定义，例如 `<event> #<elementid>` 或 `<event> .<elementclass>`，其中 `event` 可以是任何交互，如 `click`、`mouseover`、`blur` 或 `keypress`。

如果你在 `Backbone` 中使用 `jQuery`，可以查阅 `jQuery` 事件列表来了解视图可以响应的所有事件：[`api.jquery.com/category/events/`](http://api.jquery.com/category/events/)。

通常，这些事件绑定在定义视图时的 `events` 哈希中提供。

```js
LibraryView = Backbone.View.extend({
    // ...
    events: {
        'click' : 'alertBook'
    },
    // ...
    alertBook: function(e){
        alert('书本被点击');
    },
    // ...
});
```

## 摘要

本章我们探讨了路由器和事件。通过使用 `Backbone.Router` 配合 `Backbone.History`，我们学习了如何在应用中提供可添加书签的 URL，以便稍后重新访问应用的特定部分。

我们还了解了 `Backbone.Events` 如何跨所有对象工作，在一个位置处理事件，并在其他位置触发它们。尽管 `Backbone` 已经提供了丰富的内置事件集，你仍可添加自己的事件类型，以实现应用内部的解耦通信。我们在回顾 `Backbone.View` 如何与 `DOM` 触发的事件无缝交互后，完成了事件部分的学习。

在下一章中，我们将开始构建一个真实的 `Backbone` 应用，应用前三章涵盖的所有概念。
