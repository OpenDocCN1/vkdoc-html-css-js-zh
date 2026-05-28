# 4. Backbone 视图与模板库

**摘要**

前一章重点介绍了 Backbone 为支持 Web 应用模型层所提供的类和功能。现在，焦点转向视图层，`Backbone.View` 是其背后的核心类。我们还将探讨模板库如何工作，以简化 HTML 页面的更新过程。除了了解 Underscore，我们还将花时间介绍 Handlebars 和 Mustache，这是两种用于 JavaScript 应用的主流模板库。

## 代码设置

为了执行示例中的代码，我们将继续使用上一章的 `index.html`。由于我们将使用之前定义的模型和集合，任何视图相关的 JavaScript 代码片段都应放置在其定义和实例化之后，如下所示：

```html
<!DOCTYPE HTML>
<html>
<head>
    <meta charset="UTF-8">
    <title>我的应用</title>
</head>
<body>
<script src="js/external/jquery-1.10.2.js"></script>
<script src="js/external/underscore.js"></script>
<script src="js/external/backbone.js"></script>
<script>
        Book = Backbone.Model.extend({
          // 第 3 章中所有 Book 模型定义代码。
        });
        /**
         * 基于 book 定义一个集合
         **/
         var Library = Backbone.Collection.extend({model: Book,
           // 第 3 章中所有 Library 集合定义代码。
         });
        //实例化书籍
        var bookOne = new Book({name: 'Beginning Backbone', author: 'James Sugrue', year: 2013});
         var bookTwo = new Book({name: 'Pro Javascript Design Patterns', author:'Dustin Diaz, Ross Harmes', year: 2012});
         //创建图书馆
         var myLibrary = new Library([bookOne, bookTwo]);
        //此处将放置所有 Backbone 视图代码
</script>
</body>
</html>
```

## Backbone 视图

当提及 `Backbone.View` 时，我们指的是在 Backbone 应用中创建的、用于将代码组织成逻辑视图的 JavaScript 对象。这意味着，尽管视图可以更新页面中存在的 HTML（通常通过模板库实现），但视图会监听模型的变化，并在 HTML 页面的指定区域渲染这些变化。

### 创建 Backbone 视图

通过 `Backbone.View.extend()` 函数来创建自定义的视图定义。与 `Backbone.Model` 和 `Backbone.Collection` 类似，您可以传入一个 `initialize` 函数，该函数会在创建新实例时被调用。

```javascript
//定义图书馆视图
LibraryView = Backbone.View.extend({
        initialize: function(){
                console.log('视图已创建');
        },
});
```

要创建视图的新实例，只需使用 `new` 运算符。

```javascript
//创建视图的一个实例
var myView = new LibraryView();
```

您可以在创建视图时传入额外的选项，例如视图所关联的模型以及视图所应附加到的元素。在以下代码片段中，一个 `Book` 模型的实例被传递给了 `LibraryView`，该模型可以在视图的 `initialize` 函数中访问：

```javascript
var thisBook = new Book({name : 'Beginning Backbone',
                         author: 'James Sugrue'});
//创建视图的一个实例
var myView = new LibraryView({model: thisBook});
```

我们将在本章后续部分了解如何在创建视图时传入其他属性，例如 `collection`、`el`、`id`、`className`、`tagName` 和 `attributes`。

### 绑定到物理视图

要将你的 `View` 类绑定到 HTML 页面，你需要使用 `el` 属性，它是任何视图的核心。`el` 是对 HTML 页面上与该视图关联的 DOM 元素的引用。

无论视图是否已被渲染，该 DOM 元素始终可用。因此，整个视图可以在渲染之前创建好，然后一次性将元素插入到 DOM 中，从而尽可能减少重绘的次数。

使用 `el` 有两种方式：引用一个已有的 DOM 元素，或者创建一个新的 DOM 元素。

通过向构造函数传递一个 `el` 属性，你就是在告知视图它应该附加到哪个 DOM 元素上。例如，如果 HTML 页面有一个用于包含库视图内容的 `div`，你应该在构造视图时传递对该元素的引用。

```html
<body>
<div id="myLibraryViewSection"></div>
```

这个引用是使用标准的 CSS 选择器来完成的，这些选择器使用 `#<element_id>` 或 `.<element_class>`。

```javascript
// 创建一个带有模型和元素的视图实例
var myView = new LibraryView({
  model: thisBook,
  el: '#myLibraryViewSection'
});
```

你也可以通过在构造视图时传递一些属性，来动态创建视图的 `el`。

-   `tagName`：用于视图的 HTML 元素的名称。如果未指定，`tagName` 的值默认为 `div`。
-   `className`：用于渲染此元素的 CSS 类。此属性是可选的。你可以为元素指定多个类，通过空格分隔的值传递它们。
-   `id`：分配给元素的 ID。此属性是可选的。
-   `attributes`：分配给元素的其他属性，例如以键值对形式存在的 `data-` 属性。

```javascript
// 创建一个将构建其自身 DOM 元素的视图
var myNewView = new LibraryView({model: thisBook,
  tagName: 'ul',
  className: 'libraryview',
  id: 'library',
  attributes: {'data-date': new Date()}
});

console.log(myNewView.el);
```

`myNewView.el` 的结果现在将如下所示：

```html
<ul id='library' class='libraryview' data-date='Mon Aug 05 2013 13:54:20 GMT+0100 (IST)'/>
```

如果这些属性都没有传递，那么 `el` 将被创建为一个空的 `div` 元素。

无论 `el` 是新创建的还是作为引用传递的，所有这些属性都可以通过视图的对象访问。

> **注意**
>
> 当为视图构造 `el` 时，它不会自动附加到 DOM 中。

### 渲染内容

要将你的视图内容放到页面上，你需要重写 `render` 函数。如果不实现它，视图的内容将永远不会被渲染。

要让视图在构造时自动渲染，你可以从视图的 `initialize` 函数中调用 `render` 函数。

```javascript
// 定义库视图
LibraryView = Backbone.View.extend({
  initialize: function(){
    this.render();
  },
  render: function(){
    this.$el.html('Hello Library');
    return this;
  }
});
```

当然，一个更有用的 `render` 函数会利用视图中存在的模型或集合。

```javascript
render: function(){
  this.$el.html('Book Name: ' + this.model.get('name'));
  return this;
}
```

注意`render`函数末尾有一个`return this`语句。这是 Backbone 提倡的一种约定，用于允许链式调用。通过让函数返回对视图的引用，你可以执行需要该引用的进一步操作。这在处理嵌套视图时特别有用，因为它提供了对子视图`el`属性的访问。以下代码展示了由于可以轻松访问`el`属性，如何将所有子视图附加到父视图上：

```javascript
render: function(){
  for (var i = 0; i < this.subviews.length; i++){
    this.$el.append(this.subviews[i].render().el);
  }
  return this;
}
```

从前面的代码片段可以看出，由于你可以访问`$el`属性，HTML 可以直接设置在元素上。然而，这种方法容易出错，并导致 JavaScript 源码中出现大量杂乱的 HTML 代码。最好使用众多模板库之一来提供更清晰、更高效的渲染实现。我们将在本章稍后部分讨论可用的模板选项。

注意，与其在我们的渲染中使用单个模型，我们也可以在创建新实例时向视图传递一个集合。

```javascript
var myLibraryView = new LibraryView({
  collection: myLibrary,
  el: '#myLibraryViewSection'
});
```

现在，`render`方法可以处理整个模型集合了。

```javascript
render: function(){
  for(var i =0; i < this.collection.size(); i++){
    this.$el.append('<li>Book Name: ' + this.collection.at(i).get('name') + '</li>');
  }
  return this;
}
```

给你渲染的每个元素分配一个唯一的 ID 是个好主意，这样在后续阶段如果需要操作或隐藏它，可以很容易地找到它。在这种情况下，我们可以将书名视为唯一标识符，并按如下方式标记每个`<li>`：

```javascript
render: function(){
  for(var i =0; i < this.collection.size(); i++){
    var bookname = this.collection.at(i).get('name');
    this.$el.append('<li id=\'' + bookname + '\'>Book Name: ' + bookname) + '</li>');
  }
  return this;
}
```

是否需要使用模型或集合，取决于你的视图需求。

最后，可以完全从 DOM 中移除视图。这也会导致所有事件监听器被释放。

```javascript
myLibraryView.remove();
```

### 在视图中查找元素

经常需要在视图中查找嵌套的元素，并在视图的范围内运行其他 jQuery 函数。Backbone 提供了一个额外的`$el`属性，作为`$(view.el)`的简写。

例如，要在视图中查找一个子元素，你可以使用以下代码。如果视图内存在 ID 为`Beginning Backbone`的元素，这段代码将找到它。回想一下，在前面的章节中，我们为视图中的每个`<li>`标签分配了唯一的标识符。

```javascript
$el.find('#Beginning Backbone');
```

类似地，Backbone 提供了一个`$(<selector>)`函数，作为`$(view.el).find(<selector>)`的简写，这使得任何处理子元素的代码都更加优雅和易读。

```javascript
$('#Beginning Backbone');
```

前面两段代码都达到了相同的结果：在当前视图的范围内找到一个 ID 为`myelement`的元素。

### 更改 DOM 元素

在某些情况下，你可能希望在整个应用生命周期内切换视图所应用的 DOM 元素。这可以通过`.setElement()`函数实现，该函数接受一个对新元素的引用。以下代码假设你的 HTML 中创建了一个 ID 为`anotherViewSection`的`div`：

```javascript
myView.setElement('#anotherViewSection');
```

此函数还会创建`$el`引用的新版本，并确保所有在旧元素上注册的事件都会被转移到新元素上。

### 视图事件

既然视图已经渲染完成，你可能想要添加一些事件来使其具有交互性。在 Backbone 中，通过在视图定义中指定一个事件哈希表，同样可以轻松实现这一点。

事件哈希表中列出的事件将使用 jQuery 的`on`函数来为视图的 DOM 事件提供回调函数。使用`{'事件 选择器': '回调函数'}`的格式来定义将要处理的事件。以下示例演示了如何为 ID 为`book`的元素添加点击监听器：

```javascript
//定义图书馆视图
LibraryView = Backbone.View.extend({

        events: {
            'click #book' : 'alertBook'
        },

        render: function(){
             for(var i =0; i < this.collection.size(); i++){
                this.$el.append('<li id="book">图书名称: ' + this.collection.at(i).get('name') + '</li>');
                                 }
                             return this;
                },
                alertBook: function(e){
                          alert('图书被点击');
               }

});
```

如果未定义选择器，事件将绑定到`this.el`，即视图的根元素。

例如，要为整个视图添加点击处理程序，你可以简单地使用以下代码：

```javascript
events: {
     'click ' : 'alertBook'
},
```

如果你需要为视图的每个实例单独定义事件，或者视图拥有不同的操作模式，你可以使用`delegateEvents()`。此函数会自动从`View`构造函数中调用，因此当你的`events`哈希表已定义时，你无需再调用它。调用`delegateEvents`会移除所有现有的回调函数，并使用提供的事件哈希表创建一组新的事件处理程序。

以下代码会将警示监听器从点击事件更改为鼠标悬停事件：

```javascript
myLibraryView.delegateEvents( {
      'mouseover #book ' : 'alertBook'
});
```

你可以通过向`delegateEvents`传入空的事件哈希表，或者调用`undelegateEvents`函数来移除所有事件。通过在视图中添加如下一行代码，你可以移除`myLibaryView`中处理的所有事件。在 Chrome 开发者工具控制台中输入以下代码行，你将注意到视图不再响应你所创建处理程序的任何事件。

```javascript
myLibraryView.undelegateEvents();
```

### 处理 self 与 this

在 Backbone 应用中，你将会看到大量代码中的函数会将原始`this`对象的引用保存为`self`。

```javascript
var self = this;
```

这样做是为了在上下文发生变化时，仍能保持对原始`this`的引用。你会经常在代码的闭包中看到这种情况，尤其是在你创建的任何事件处理程序中。

使用这种方法，`render`函数将改用`self`引用来替代`this`。

```javascript
render: function(){
        var self = this;
        for(var i =0; i < this.collection.size(); i++){
             self.$el.append('<li id="book">图书名称: ' + self.collection.at(i).get('name') + '</li>');
         }
                        return self;
},
```

### Backbone 视图快速参考

表 4-1 描述了`Backbone.View`中可用的属性和函数。

表 4-1. `Backbone.View`中最重要的属性和函数

| 函数/属性 | 描述 |
| --- | --- |
| `el` | 指定用于包含此视图的 DOM 元素的引用。 |
| `$el` | 指定视图元素的缓存 jQuery 对象。 |

| `$(<选择器>)` | 当包含 jQuery 时，`$`函数将在视图元素范围内执行查询。 |
| --- | --- |
| `model` | 指定此视图应渲染的模型。 |
| `collection` | 指定视图应渲染的集合。 |
| `id` | 指定分配给视图根元素的 ID。 |
| `className` | 指定视图根元素的 CSS 类。 |
| `tagName` | 指定此视图根元素的 HTML 标签。 |
| `attributes` | 指定要用于该元素的额外属性。此属性也可用于访问可用的属性。 |
| `setElement(<选择器>)` | 更改此视图的根元素。 |
| `remove()` | 从 DOM 中完全移除该视图。 |
| `render()` | 负责处理视图所有渲染的函数。 |
| `events` | 一个事件哈希表及其处理函数，用于此视图。 |
| `delegateEvents(<事件>)` | 为视图设置一组新的事件和处理程序，移除之前的事件。 |
| `undelegateEvents(<事件>)` | 移除视图的所有事件。 |

## Backbone 中的模板化

模板库使得逻辑与表现的分离更加明显。通过在 JavaScript 源码之外定义模板，你可以避免在视图的`render`方法中混杂大量的 HTML 字符串。

模板库的思想是从你的 JavaScript 应用中获取 JSON 对象，并将它们插入到 HTML 模板中，该模板包含条件判断和其他逻辑表达式，以及对象值的占位符。

Backbone 并不强制你使用任何特定的模板库，如前一节所示，你甚至可以直接使用纯 HTML。本章我们将探讨三种模板库。首先处理 Underscore，你的 Backbone 应用已经依赖于它。然后我们将讨论 Handlebars 和 Mustache，这是两个最流行的模板库。

### 使用 Underscore 进行模板化

除了为你的 Backbone 应用提供大量实用函数外，Underscore 还包含用于处理模板的功能。

模板作为 `script` 标签放置在 HTML 页面中，其类型设置为 `text/template`，这样浏览器在渲染 HTML 页面时会忽略我们创建的模板。模板还应带有一个 ID，以便你的 JavaScript 能够找到它。

```
<script type="text/template" id="library-template">
</script>
```

Underscore 允许你在模板中使用 JavaScript 表达式来循环遍历数据并执行其他条件操作。首先，回顾一下上一节中 `render` 方法内的数据结构。我们有一个包含多本书的图书馆。由于需要渲染的是一个集合而非单个模型，因此在模板中需要使用循环。

```
<script type="text/template" id="library-template">
  <ul>
    <% for (var i = 0; i < library.length; i++) { %>
      <% var book = library[i]; %>
      <li>
        <b><%= book.name %></b> 由 <%= book.author %> 编著
      </li>
    <% } %>
  </ul>
</script>
```

此模板要求传入渲染的 JSON 结构被命名为 `library`，并且它是一个包含一个或多个项目的集合，这些项目具有前面列出的属性（`name` 和 `author`）。

请注意，任何 JavaScript 代码都可以放在 `<% ... %>` 标记内，例如 `for` 循环，而所有 HTML 代码都出现在这些标记之外。然后可以使用 `<%= variable %>` 标记来显示数据。

在 HTML 中定义好模板后，视图需要在 `render` 方法中感知到它的存在。关键在于 Underscore 提供的 `template()` 函数。

该函数将 JavaScript 模板编译成一个可评估并用于渲染的函数。模板的参数包括代表模板的字符串、要渲染的数据，以及一个可选的模板设置参数。

通过查找页面中具有相应 ID 的元素并获取其 HTML 内容，来填充模板字符串的值。

`$("#library-template").html()`

由于模板期望数据采用特定结构且名称为 `library`，因此将集合的 JSON 表示形式作为第二个参数传入，并附带预期的标识符。

`{'library': self.collection.toJSON()}`

最后，可以通过将输出追加到视图的 `$el` 属性来渲染模板。

`self.$el.append(output);`

我们启用了 Underscore 的模板函数的完整代码如下：

```
render: function(){
  var self = this;
  var output = _.template($("#library-template").html(), {'library': self.collection.toJSON()});
  self.$el.append(output);
  return self;
}
```

请注意，在整个代码中，Underscore 库通过 `_` 命名空间来引用。上面定义的 `render` 函数很直观，但它有一个缺陷：每次调用 `render` 时，模板都会被重新编译。更简洁的做法是将编译后的模板存储为视图的一个属性。

```
LibraryView = Backbone.View.extend({
  // 其他视图相关代码
  template: _.template($("#library-template").html()),
  //....
});
```

现在，当视图被创建时，模板就被编译并随时可用。这使得 `render` 函数更加简洁。

```
render: function(){
  var self = this;
  var output = self.template({'library': self.collection.toJSON()});
  self.$el.append(output);
  return self;
},
```

`templateSettings` 参数可用于更改定界符样式，方法是在 `interpolate` 参数中使用不同的正则表达式。例如，要使用 `{{ }}` 而不是 `<% %>`，你可以使用以下设置：

```
var templateSettings = {
  interpolate : /\{\{(.+?)\}\}/g
};
```

通过在 `_.` 级别定义这些设置，可以使其对所有 Underscore 模板全局生效。

```
_.templateSettings = {
  interpolate : /\{\{(.+?)\}\}/g
};
```

这一行代码将应用于整个应用程序，因此需要在你的 JavaScript 源代码中创建任何视图之前声明它。

如你所见，使用 Underscore 进行模板化相当简单。通过在模板内部使用纯 JavaScript 来处理逻辑和条件，代码非常易于理解。但还有更出色的选择，我们将在接下来的章节中介绍。

### 使用 Handlebars 进行模板化

Handlebars（[`www.handlebarsjs.org`](http://www.handlebarsjs.org/)）是 JavaScript 社区中最流行的模板库之一。该库遵循 Mustache.js 风格，其理念提倡无逻辑模板。请注意，这与 Underscore 是不同的方法。因此，Handlebars 模板不接受模板内的任意 JavaScript，迫使你使用内置构造。不过，它具有添加自定义辅助函数的能力，我们稍后会介绍。

与所有模板库一样，Handlebars 接受模板结构并将其编译成一个 JavaScript 函数，该函数接受 JSON 数据进行呈现。Handlebars 的优势之一在于，它遵循与 Mustache 模板相同的风格，因此这些库是可互换的，同时你可以保留相同的模板。此外，凭借其模板预编译选项，Handlebars 被认为是目前最先进的库之一。

#### 在 Backbone 中使用 Handlebars

在 Backbone 项目中集成 Handlebars 出奇地简单。首先，你需要从 [`www.handlebarsjs.org`](http://www.handlebarsjs.org/) 下载该库的最新版本。为了与本书迄今为止使用的代码结构保持一致，你可以将其下载到 `js/external` 文件夹，并按如下方式在 `index.html` 中引入：

```
<script src="js/external/handlebars.js"></script>
```

现在，让我们将之前为 Underscore 创建的模板替换为 Handlebars 兼容的版本。与之前一样，模板包含在一个带有 ID 的 `<script>` 标签中，但模板的类型现在是 `text/x-handlebars-template`。

`<script type="text/x-handlebars-template" id="library-template">`

在循环的处理方式上，最能清晰地看出 Handlebars 与 Underscore 的区别。Handlebars 使用 `{{#<表达式名称>}}` 这种形式来整合表达式。

`<script type="text/x-handlebars-template" id="library-template">`

```
<ul>
    {{#each library}}
    <li>
        <em>{{name}}</em> by {{author}}
    </li>
    {{/each}}
</ul>
```

从前面的代码可以看出，Handlebars 模板看起来比 Underscore 的对应版本优雅得多。它没有使用 `for` 语句，而是使用 `#each` 表达式来遍历集合，该集合通过标识符 `library` 传递。

与 `{{#each}}` 用于启动循环相对应，循环体使用 `{{/each}}` 结束。Handlebars 中的所有块都遵循此模式。变量只需将其名称放在花括号内即可呈现。

如果 `library` 对象内部还包含另一个对象，则可以使用点表示法访问其原始值。例如，假设每本书都有以下结构，其中包含一个表示出版日期的对象：

```
var bookOne = new Book({
    name: 'Beginning Backbone',
    author: 'James Sugrue',
    publishDate: {
        year: 2013,
        month: 'December'
    }
});
```

那么，就可以在循环中使用 `{{publishDate.year}}` 访问出版年份。

```
<script type="text/x-handlebars-template" id="library-template">
<ul>
    {{#each library}}
    <li>
        <em>{{name}}</em> by {{author}} published in {{publishDate.year}}
    </li>
    {{/each}}
</ul>
</script>
```

引入 Handlebars 模板只需将视图的 `template` 属性改为使用 Handlebars 而不是 Underscore 进行编译即可。

```
LibraryView = Backbone.View.extend({
    //....
    template: Handlebars.compile($("#library-template").html()),
    ...
});
```

请注意，唯一的变化就是将 `_.template` 替换为 `Handlebars.compile`。

实际上，`render` 方法与之前相同，因为唯一与模板库相关的代码就是 `template` 变量的声明。

```
render: function(){
    var self = this;
    var output = self.template({'library':self.collection.toJSON()});
    self.$el.append(output);
    return self;
},
```

如你所见，Handlebars 提供了一种简洁的创建模板的方式。其表达式语言直观明了，但功能强大。下一节将让你更深入地了解 Handlebars 的各种可能性。

## Handlebars 快速指南

由于 Handlebars 模板的语法不仅仅是纯 JavaScript，本节将介绍一些基本表达式，以帮助你入门。

### 显示变量

可以使用 `{{name}}` 这种形式将变量值输出到模板。这会打印出当前上下文中的值 `name`，该上下文可以通过遍历集合的块表达式来改变。

```
<h1>{{name}}</h1>
```

如果你正在处理嵌套变量，可以使用点分隔的路径来确定值。例如，如果模板处理的是一个 `person` 对象，你可以像下面这样显示详细信息：

```
<h1>{{person.name}}</h1>
<p>Birthday: {{person.birthday}}</p>
```

### 注释

可以使用 `{{! comment }}` 或 `{{!-- comment --}}` 将注释插入到模板中。

```
{{! Name of the person }}
<h1>{{person.name}}</h1>
```

当你需要注释掉 Handlebars 表达式时，应使用 `{{!-- }}` 风格。

```
{{!-- unused expression for now {{name}} --}}
```

### 块表达式

Handlebars 中的块使用 `{{#expression}}` 语句开始，并以 `{{/expression}}` 语句结束。

#### each

正如我们在 library 示例中已经看到的，`each` 助手允许遍历一个列表。

```
{{#each library}}
    <li>
        <em>{{name}}</em> by {{author}} published in {{publishDate.year}}
    </li>
{{/each}}
```

请注意，你可以在块内使用 `{{this}}` 来引用当前正在遍历的元素。

你也可以使用路径来访问当前上下文之外的对象和属性。例如，可以使用 `{{../<属性名称>}}` 来访问与你当前正在遍历的列表位于同一层的属性。

### 条件判断

模板定义中可以使用多个条件语句。以下部分列出了一些。

#### if else

当模板中有需要根据条件渲染的部分时，Handlebars 包含一个 `if else` 结构。参数会被传递给 `{{#if }}` 表达式进行评估。

```
{{#if book.published}}
    <p>Book now available</p>
{{else}}
    <p>Book coming soon : {{book.pubishDate}}</p>
{{/if}}
```

#### unless

`unless` 助手的作用与前面介绍的 `if` 语句相反。它接收一个参数，并确保在该参数评估为 `false` 时才会进入模板部分。

```
{{unless book.published}}
<p>This book is not yet published</p>
{{/unless}}
```

### 内置助手

Handlebars 自带了一些内置助手，以便于使用。

#### with

`with` 助手允许在模板内更改上下文，从而使用特定的对象。这个助手允许你指定在块内处理某个特定对象，形式如下，而不是使用 `{{object.property}}`：

```
{{#with object}}
    {{property}}
{{/with}}
```

为了直观地理解这一点，让我们考虑以下 `Person` 对象：

```
{{#with person}}
    <h1>{{name}}</h1>
    <p>Birthday: {{birthday}}</p>
{{/with}}
```

#### log

`log` 助手允许在模板内进行简单的日志记录，这对于调试非常有用。

该助手委托给 `Handlebars.logger.log`，如果需要自定义日志记录，可以覆盖该方法。

```
{{#with person}}
    <h1>{{log name}}</h1>
{{/with}}
```

## 提供自定义助手

Handlebars 的真正强大之处在于，当内置表达式和助手不够用时，你可以编写自己的助手。你可以在 JavaScript 代码中创建自己的助手，这些助手随后会在任何模板的编译过程中被使用。

让我们创建一个简单的加法助手。你需要使用 Handlebars 的 `registerHelper` 函数，并提供助手名称，后跟要执行的操作。

```
Handlebars.registerHelper('add', function(param1, param2){
    return param1 + param2
});
```

现在，你可以像这样在模板中使用它：

```
<p>In 20 years time {{name}} will be {{add person.age 20}} years old </p>
```

别忘了，Handlebars 有一个庞大的用户社区，他们会分享自己的助手。

## 预编译模板

Handlebars 提供了强大的预编译能力，可以节省客户端执行时间，并允许你使用不包含运行时编译功能的 Handlebars 运行时库 `handlebars.runtime.js`。在撰写本文时，未压缩的运行时库大小为 11KB，而完整的 Handlebars 库则为 73KB。当针对移动设备时，这种节省效果会非常显著。

你需要在开发或构建机器上安装 Node.js 才能运行预编译。在第 3 章中，我们创建了一个 Node.js 服务器作为应用程序的后端。如果尚未安装 Node.js，可以从 [`http://nodejs.org`](http://nodejs.org/) 下载。安装 Node.js 后，即可使用 Node 包管理器 (NPM) 下载 `handlebars` 包。

```
npm install handlebars -g
```

所有模板都需要存储在一个外部文件中，该文件会通过命令行传递给预编译器。

```
handlebars <inputfile> -f <outputfile>
```

这将生成一个包含已编译模板的 JavaScript 文件，随后可将该文件包含在你的应用程序中。

## 使用 Mustache 进行模板化

`Mustache.js` 是另一个无逻辑的模板库，可从 [`http://mustache.github.io`](http://mustache.github.io/) 获取。Mustache 在多种语言中广受欢迎，包括 Ruby、Java、Objective-C 等。

其模板格式与 Handlebars 使用的格式非常相似，只是没有帮助函数。

### 在 Backbone 中使用 Mustache

首先，你需要从 [`http://mustache.github.io`](http://mustache.github.io/) 下载最新版本的 `Mustache.js`。同样，为符合本书中使用的惯例，应将其保存在 `js/external` 文件夹中，并按如下方式包含在 `index.html` 中：

```
<script src="js/external/mustache.js"></script>
```

该模板看起来与 Handlebars 版本相似。

```
<script type="text/x-mustache-template" id="library-template">
<ul>
    {{#library}}
   <li>
    <em>{{name}}</em> by {{author}}
 </li>
    {{/library}}
</ul>
</script>
```

请注意，遍历列表使用的是 `{{#<listname>}}` 语法，而不是 Handlebars 中的 `{{#each}}` 块辅助函数。循环结束使用 `{{/<listname>}}`。除此之外，模板中的其他内容基本相同。

同样，从 Backbone 视图更改模板库只需更改模板属性即可。

```
template: Mustache.compile($("#library-template").html()),
```

`render` 方法保持不变。

```
render: function(){
        var self = this;
        var output = self.template({'library': self.collection.toJSON()});
        self.$el.append(output);
        return self;
},
```

从上述示例可以清楚地看出，在 Handlebars 和 Mustache 之间切换是一个相对直接的过程。

### Mustache 快速指南

本节简要概述如何在 Mustache 模板中完成一些常用任务。

#### 显示变量

变量使用双花括号语法显示，与 Handlebars 相同。

```
{{name}}
```

可以使用点号表示法来访问对象的属性。

```
{{person.name}}
```

#### 遍历列表

可以通过使用 `{{#list}}` 语法初始化一个块，并使用 `{{/list}}` 结束该块来创建遍历列表的块。

```
{{#library}}
   <li>
    <em>{{name}}</em> by {{author}}
</li>
{{/library}}
```

可以使用单个点号来访问当前在迭代列表中聚焦的对象。

```
{{#library}}
      <p>This object {{.}}</p>
{{/library}}
```

#### 注释

Mustache 模板可以包含使用 `{{! comment }}` 格式的注释部分。

```
{{! 显示人物名称 }}
{{person.name}}
```

## 使用哪个模板库？

现在你已经了解了三个主要选项，决定使用哪个模板库可能有些困难。这实际上取决于你项目的需求。如果你使用有限或非常简单的模板，请使用 Underscore。因为你已经存在依赖关系，使用 Underscore 应该是你的最低要求。利用任何模板库都将带来更清晰的代码库，使 HTML 与视图中的所有 JavaScript 代码完美分离。

但是，如果你的应用程序可能包含许多不同的视图，则值得考虑使用 Handlebars。模板的语法更自然，而且你始终可以添加更多辅助函数来满足项目需求。Handlebars 提供的预编译功能使其成为关注性能（尤其是针对移动设备的项目）的项目的理想选择。