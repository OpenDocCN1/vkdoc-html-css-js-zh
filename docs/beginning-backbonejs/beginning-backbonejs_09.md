# 3. Backbone 模型与集合

## 摘要

任何 Backbone.js 应用的模型层都有两个主要部分：数据的表示（`Backbone.Model`）和将数据分组为集合（`Backbone.Collections`）。理解这两个部分各自的工作原理以及它们之间的交互方式，将使你能够为你的 Backbone 应用创建一个结构良好的数据层。

> 注意：在浏览本章节时，当你使用代码示例时，可以简单地加载第 1 章的 `index.html` 文件，并使用 Chrome 开发者控制台来尝试这些代码片段。由于你已经加载了 Backbone、Underscore 和 jQuery，所有这些代码片段都能正确执行。

## 系统设置

在进一步深入之前，你需要做一些事情来确保你的系统能正常工作。首先，所有代码都应通过 Apache Web 服务器来执行，而不能仅仅从文件系统运行。在我的示例中，在 XAMPP 安装目录的 `htdocs` 下有一个名为 `backbone` 的文件夹。当从此服务器提供 `index.html` 服务时，访问的 URL 是 `http://localhost/backbone/index.html`。

`index.html` 的结构应遵循前一章的样式。确保你已经包含了 Backbone、Underscore 和 jQuery 库，并将本章中展示的任何 JavaScript 代码放在文件末尾的 script 元素内。

你还会注意到，对于任何后端 API 调用，我都使用了 Node.js。Node.js 可以从 `http://nodejs.org` 下载，在设置我们将在“与服务器交换数据”部分创建的 `server.js` 文件之前，你应确保本地安装正常工作。

最后，当遇到问题时，别忘了使用 Chrome 开发者工具；它总是能提供缩小 JavaScript 代码中错误范围的最佳机会。

## Backbone 模型

每个应用程序都始于对模型的定义。模型是需要在应用中操作和呈现的数据。如前面章节所述，模型是数据驻留的层级，通常包含多个对象。通常，你应该将问题领域分解为单个元素，并将每个元素描述为一个模型对象。

要在 Backbone 中创建一个模型对象，你需要使用 `Backbone.Model`。正如你将看到的，对于你创建的所有 Backbone 对象，你将使用 `.extend` 方法来创建你自己的类版本，如以下代码所示：

```
MyModel = Backbone.Model.extend({

       //对象属性将放在这里

});
```

该库中的所有对象都有这个 `Backbone` 命名空间，因此在使用 Backbone 对象时，这一点总是非常清晰。

在实际应用中，你很可能永远不会以这种方式创建模型，而是会传递属性来正确构造你的对象。在前面的代码片段中，空括号（`{}`）表示一个对象字面量。这是你希望在模型中可用的所有属性和函数所在的位置。

> **注意**：对象字面量包含一系列以逗号分隔的定义，形式为 `key : value`，其中 `value` 可以是一个函数或另一个对象。

### 构造函数

在创建新的模型对象时，你可能希望在构造时执行某些行为，例如设置不同的处理程序或在模型中设置一些初始状态。要在 Backbone 中实现这一点，只需在创建模型对象时定义一个 `initialize` 函数。

为了演示这个 `initialize` 函数的作用，让我们创建一个简单的 `Book` 对象，在对象创建时向控制台输出一行信息。

```
Book = Backbone.Model.extend({

     initialize: function(){

           console.log('创建了一本新书');

     }

});
```

一旦我们创建了 `Model` 对象的新实例，就会调用这个 `initialize` 函数。在这个例子中，当创建新的 `Book` 对象时，我们将在控制台中看到一行输出：“创建了一本新书”。

```
var myBook = new Book();
```

模型的另一个常见需求是拥有一些可用的默认属性。你可能希望这样做，以便未在对象创建时传递的可选参数具有一些定义。在 Backbone 中，这是通过使用 `defaults` 对象字面量来完成的。

```
Book = Backbone.Model.extend({

    initialize: function(){

          console.log('创建了一本新书');

    },

defaults: {

               name: '书名',

               author: '佚名'

}

});
```

现在，当创建对象时，每个实例都会获得这些默认值。

### 模型属性

由于数据是拥有模型的主要原因，我们首先关注属性。本节介绍使用 `Backbone.Model` 时所有可用的与属性相关的操作。

#### 获取属性值

任何模型对象中的属性都可以通过使用 `.get` 函数并传入你想要访问的属性名称来轻松获取。因为我们使用的是简单的默认值，所以在你初始化 `Book` 对象后，下面这行代码将输出“书名”。

```
console.log(myBook.get('name'));
```

在对象创建期间，可以传入模型中属性的值，以便每个实例都可以是唯一的。

```
var thisBook = new Book({name : '开始使用 Backbone',

                                author: '詹姆斯·苏格鲁'});
```

现在，当你检索任一属性的值时，传入的值将替代默认值被使用。

```
console.log(thisBook.get('name') + ' 作者 ' +        thisBook.get('author'));
```

你还可以使用 `.attributes` 属性来获取表示所有模型数据的 JSON 对象。

```
console.log(thisBook.attributes); // 所有属性的 JSON 表示
```

#### 更改属性值

在构造函数之外更改属性值的方式类似，使用 `set('<变量名>', <值>)` 的函数调用结构。例如，要更改书名，请使用以下代码：

```
thisBook.set('name', '开始使用 Backbone.js');
```

你可以像前面演示的那样，以相同的方式为你的对象添加新属性。

```
thisBook.set('year', 2013);//创建一个名为 year 的新属性
```

#### 删除属性

你可能会发现需要从模型中删除你不再使用的属性。在以下示例中，添加了一个新属性，随后将其删除。第二个 `console.log` 语句返回 `undefined` 作为属性值。

```
thisBook.set('year', 2013);

console.log('书籍年份 ' + thisBook.get('year'));

thisBook.unset('year');

console.log('书籍年份 ' + thisBook.get('year'));
```

Backbone 提供了一种更简洁的方式来检查模型中是否存在某个属性，即使用 `.has` 函数，该函数返回一个布尔值。

```
//检查属性是否存在

var hasYear = thisBook.has('year'); //结果为 false

var hasName = thisBook.has('name'); //结果为 true

console.log('是否存在名为 year 的属性  : ' + hasYear);

console.log('是否存在名为 name 的属性  : ' + hasName);
```

你还可以使用 `clear` 函数删除模型中的所有属性。

```
//创建一个新对象

var newBook = new Book({name: '另一本书', author: '你'});

newBook.clear();//移除所有属性

console.log('是否存在名为 name 的属性  : ' + newBook.has('name'));//结果为 false
```

`unset` 和 `clear` 函数都接受一个额外的 `options` 对象，你可以在其中添加一个布尔值，指示操作完成后不应从模型触发更改事件。本章后面的“模型事件”部分将解释这些事件如何运作。

```
thisBook.unset('year', {silent: true});
```

#### 克隆模型

你可能想要创建 Backbone 模型的完整副本，并保留所有相同的属性。无需担心如何创建深层副本的细节，你可以简单地使用`.clone()`方法来创建一个克隆的模型实例。

```
var clonedBook = thisBook.clone();
```

#### 属性函数参考

表 3-1 描述了可以在 Backbone 模型上执行的最有用的与属性相关的操作。

**表 3-1.** Backbone 模型的属性函数

| 操作 | 描述 |
| --- | --- |
| `.get(<属性名>)` | 返回具有给定名称的属性值。如果不存在此属性，则返回 `undefined`。 |
| `.set(<属性名>, 属性值)` | 将给定属性的值设置为第二个参数提供的值。如果该属性尚不存在，则使用此值创建一个新属性。 |
| `.has(<属性名>)` | 检查模型对象中是否存在给定的属性。 |
| `unset(<属性名>)` | 如果存在，则从模型中移除一个属性。 |
| `clear()` | 移除模型对象中的所有属性。 |
| `.attributes` | 返回模型中所有属性的 JSON 表示。 |
| `.clone()` | 创建模型的一个新实例，包含所有相同的属性。 |

#### 为模型添加函数

到目前为止，我们的模型只涉及属性，但你也可以添加自己的函数来处理重复性任务。下面的例子说明了如何包含一个`printDetails`函数来替代目前使用的所有`console.log`语句：

```
Book = Backbone.Model.extend({

     initialize: function(){
           console.log('一本新书');
     },

     defaults: {
           name: '书名',
           author: '佚名'
     },

     printDetails: function(){
           console.log(this.get('name') + ' 作者：' + this.get('author'));
     }

});
```

现在，`Book`对象的所有实例都可以使用这个函数。

```
//使用 printDetails 函数
thisBook.printDetails();
```

#### 模型事件

虽然我们将在本书后面讨论事件，但有一种对模型至关重要的特定事件类型是`change`事件，当某个属性或一组属性的值被修改时，就会触发此事件。

##### 监听变化

使用 Backbone 的 `change` 处理器，最简单的方式是监听整个模型的变化。事件处理器通过`.on()`函数添加到 Backbone 对象上，该函数接受一个表示处理器类型的字符串，并接受一个函数引用，该函数在变化发生时被执行。

创建这个监听器的最佳时机是在模型的`initialize`函数中。通过修改你现有的代码，你会发现每当调用 `set` 时，这个函数都会被触发。

```
Book = Backbone.Model.extend({

     initialize: function(){
                 this.on("change", function(){
                         console.log('模型已改变');
                 });
     },

     defaults: {
        name: '书名',
        author: '佚名'
     },

printDetails: function(){
        console.log(this.get('name') + ' 作者：' + this.get('author'));
     }

});
```

你可以通过使用`change:<属性名>`格式而不是`change`来监听特定属性的变化。以下新增内容创建了另一个仅处理`name`属性变化的处理器：

```
initialize: function(){
          this.on("change", function(){
          console.log('模型已改变');
          });

this.on("change:name", function(){
          console.log('name 属性已改变');
          });

},
```

我们之前提到，`.set`函数允许一个可选参数用于静默更新。如果使用了这个参数，那么 `change` 处理器将不会被调用。

```
//设置变量（期望调用 change 处理器）
thisBook.set('name', '不同的书'); // change 处理器被调用
thisBook.set('name', '不同的书', {silent:true});// change 处理器未被调用
```

##### 查明哪些内容发生了变化

Backbone 包含许多属性，用于跟踪模型中的变化。如果你使用全局的 `change` 处理器，这将是查看当前状态的一个非常有用的方法。

你可以使用`hasChanged('<属性名>')`来检查某个单独的属性是否被修改。

```
this.on("change", function(){
   console.log('检测到模型变化：');
    if(this.hasChanged('name')){
             console.log('name 已改变');
    }

    if(this.hasChanged('year')){
            console.log('year 已改变')
    }

});
```

你可以使用`.changed`属性获取所有已更改属性的集合。

```
Book = Backbone.Model.extend({

     initialize: function(){
          this.on("change", function(){
        console.log('已更改的属性：' + JSON.stringify(this.changed));
        });

},
```

你还可以使用`.previousAttributes()`函数获取所有属性的先前状态集合。下面这行代码也可以添加到 `change` 处理器中：

```
console.log('先前的属性：' + JSON.stringify(this.previousAttributes()));
```

最后，你可以使用`.previous('<属性名>')`来检索特定属性的先前值。

```
if(this.hasChanged('name')){
      console.log('name 已从 ' + this.previous('name') + ' 变为 ' + this.get('name'));
}
```

##### 属性变更参考

表 3-2 描述了与模型中属性变更相关的最常用操作。

**表 3-2.** Backbone 模型中与属性变更相关的函数

| 操作 | 描述 |
| --- | --- |
| `.on('change', <函数>)` | 提供一个全局的 `change` 处理器，响应模型中任何属性的变化 |
| `.on('change:<属性名>', <函数>)` | 监听特定属性的变化 |
| `.hasChanged(<属性名>)` | 如果自上次 `change` 事件以来该属性已更改，则返回`true` |
| `.previous(<属性名>)` | 返回特定属性的先前值 |
| `.changed` | 返回模型中所有已更改属性的完整集合 |

### 模型验证

Backbone 为模型数据提供了一种验证机制，这意味着你可以将所有判断模型状态是否正确的逻辑都放在模型内部，而不是放在外部的 JavaScript 或表单处理代码中。

如果你提供了一个验证方法，那么每次调用`.save`函数时，以及在每次 `set`/`unset` 操作中（当提供了`{validate:true}`作为可选参数时），该方法都会运行。

假设我们的`Book`模型要求必须存在 `name`，并且 `year` 必须在 2000 年之后。针对这些规则的验证方法如下所示：

# Backbone 模型验证与服务器交互

`Book = Backbone.Model.extend({`

`initialize: function(){`

`},`

`defaults: {`

`},`

`printDetails: function(){`

`},`

`validate: function(attrs){`

`if(attrs.year < 2000){`

`return '年份必须在 2000 年之后';`

`}`

`if(!attrs.name){`

`return '必须提供书名';`

`}`

`}`

`});`

如果在操作模型时违反了这些规则，操作将无法更改属性值。

```
//尝试将 year 设置为 2000 年之前
thisBook.set('year', 1999, {validate: true});
console.log('检查 year 是否更改：' + thisBook.get('year'));

//尝试从模型中移除 name
thisBook.unset('name', {validate: true});
console.log('检查 name 是否被移除：' + thisBook.get('name'));
```

当检测到验证错误时，会触发一个事件。通过添加一个"invalid"事件处理器，你可以针对验证错误提供反馈。与所有事件处理器一样，这应该添加到你的`initialize`函数中。

```
Book = Backbone.Model.extend({
initialize: function(){
    this.on("invalid", function(model, error){
        console.log("**验证错误：** " + error + "**");
    });
```

如果没有验证标志，验证函数将不会在 `set` 时执行。但是，你可以随时使用`isValid()`函数检查模型是否处于有效状态。

```
//检查模型是否有效
console.log('模型是否有效：' + thisBook.isValid());

//不使用 validate 标志来违反验证规则
thisBook.set('year', 1998);

//检查模型是否有效
console.log('模型是否有效：' + thisBook.isValid());
```

## 与服务器交换数据

Backbone 模型可用的最后一项功能涉及如何从提供 REST API 的服务器存储数据以及向其发送数据。在我们讨论 Backbone 相关的机制之前，首先设置一个简单的后端来为我们的 API 调用提供响应。

**注意**

请记住，`Backbone.Collection`负责处理大部分 RESTful 操作，而不需要对每个模型对象都进行调用。为了说明底层机制，我们将首先查看模型操作。

### Node.js 服务端

既然我们都在使用 JavaScript，我将简要介绍一个使用 Node.js 的简单服务器，但你也可以自由选择其他 REST 服务器实现。本书的重点在于客户端，因此不会过多使用这个服务器。不过，它在演示 Backbone 应用的请求响应时会很有用。

首先，你需要从 [`http://nodejs.org`](http://nodejs.org/) 安装 Node.js。安装完成后，使用 NPM 包管理器安装 `express` 包，这是一个极简的 Node.js Web 应用框架，可以轻松搭建带有 REST API 的简单服务器。

```
npm install express
```

最后，将以下代码复制到 `server.js` 文件中。从代码中可以看出，它提供了托管在 [`http://localhost:8080/books`](http://nodejs.org/) 的一些简单端点。

# 托管在 `localhost:8080/books` 的简单 API

```javascript
var express = require('express');
var app = express();
var bookId = 100;

function findBook(id){
  for(var i =0; i < books.length; i++){
    if(books[i].id === id){
      return books[i];
    }
  }
  return null;
}

function removeBook(id){
  var bookIndex = 0;
  for(var i=0; i < books.length; i++){
    if(books[i].id === id){
      bookIndex = i;
    }
  }
  books.splice(bookIndex, 1);
}

app.configure(function () {
  // 解析请求体中给出的 JSON 对象
  app.use(express.bodyParser());
});

var books = [
  {id: 98, author: 'Stephen King', title: 'The Shining', year: 1977},
  {id: 99, author: 'George Orwell', title: 1949}
];

/**
 * HTTP GET /books
 * 应返回图书列表
 */
app.get('/books', function (request, response) {
  response.header('Access-Control-Allow-Origin', '*');
  console.log('In GET function ');
  response.json(books);
});

/**
 * HTTP GET /books/:id
 * id 是你要获取的图书的唯一标识符
 * 应返回指定 id 的任务，否则返回 404
 */
app.get('/books/:id', function (request, response) {
  response.header('Access-Control-Allow-Origin', '*');
  console.log('Getting a  book with id ' + request.params.id);
  var book = findBook(parseInt(request.params.id,10));
  if(book === null){
    response.send(404);
  }
  else{
    response.json(book);
  }
});

/**
 * HTTP POST /books/
 * 此请求的请求体包含你要创建的图书。
 * 成功时返回 200
 */
app.post('/books/', function (request, response) {
  response.header('Access-Control-Allow-Origin', '*');
  var book = request.body;
  console.log('Saving book with the following structure ' + JSON.stringify(book));
  book.id = bookId++;
  books.push(book);
  response.send(book);
});

/**
 * HTTP PUT /books/
 * id 是你要更新的图书的唯一标识符。
 * 如果该 id 的图书不存在，则返回 404。
 */
app.put('/books/:id', function (request, response) {
  response.header('Access-Control-Allow-Origin', '*');
  var book = request.body;
  console.log('Updating  Book ' + JSON.stringify(book));
  var currentBook = findBook(parseInt(request.params.id,10));
  if(currentBook === null){
    response.send(404);
  }
  else{
    //在本地保存图书
    currentBook.title = book.title;
    currentBook.year = book.year;
    currentBook.author = book.author;
    response.send(book);
  }
});

/**
 * HTTP DELETE /books/
 * id 是你要删除的图书的唯一标识符。
 * 如果该 id 的图书不存在，则返回 404。
 */
app.delete('/books/:id', function (request, response) {
  console.log('calling delete');
  response.header('Access-Control-Allow-Origin', '*');
  var book = findBook(parseInt(request.params.id,10));
  if(book === null){
    console.log('Could not find book');
    response.send(404);
  }
  else
  {
    console.log('Deleting ' + request.params.id);
    removeBook(parseInt(request.params.id, 10));
    response.send(200);
  }
  response.send(200);
});

// 额外的设置以允许 CORS 请求
var allowCrossDomain = function(req, response, next) {
  response.header('Access-Control-Allow-Origin', "http://localhost");
  response.header('Access-Control-Allow-Methods', 'OPTIONS, GET,PUT,POST,DELETE');
  response.header('Access-Control-Allow-Headers', 'Content-Type');
  if ('OPTIONS' == req.method) {
    response.send(200);
  }
  else {
    next();
  }
};

app.configure(function() {
  app.use(allowCrossDomain);
});

// 在 8080 端口启动应用
app.listen(8080);
```

要启动服务器，在命令行中使用 `node server.js`。这个服务器虽然非常简单，但足以让 Backbone 的持久化调用正确执行。

## 标识符

Backbone 模型有三个属性用于在与服务器进行数据交换时唯一标识它们：`id`、`cid` 和 `idAttribute`。

`id` 属性是一个唯一的字符串或整数值，类似于关系数据库中的主键。在从集合中检索模型时，`id` 属性非常有用，它也用于构建模型 URL 的一部分。

`cid` 属性是 Backbone 在模型首次创建时自动生成的；当模型尚未保存到服务器且没有其真实 ID 时，它可以作为唯一标识符使用。

有时，你从后端检索的模型会使用不同的唯一键。例如，服务器可能使用 ISBN 作为图书的唯一标识符，或者用户 ID 字段可能是保存用户模型时使用的标识符。`idAttribute` 属性允许你在该键与模型中的 ID 之间建立映射，这意味着服务器将使用该属性来填充 ID。

### 保存模型

`save` 函数调用操作将模型保存到服务器，并调用 `Backbone.sync` 函数。我们稍后会看到如何替换 `sync` 函数以提供保存模型的其他方式，但现在我们先遵循 Backbone 内置的默认行为。

现在我们已经有了一个可以连接的后端服务，可以设置 `Model` 对象的 `urlRoot` 属性。

```javascript
Book = Backbone.Model.extend({
    urlRoot: 'http://localhost:8080/books/',
```

这个 URL 告诉 Backbone 在执行任何需要后端服务响应的操作时要指向哪里。由于我们的 node.js 服务器运行在 8080 端口，并且我们设置了从 `/books/` 响应请求的端点，因此我们的 URL 构造方式如上代码清单所示。

当 `id` 属性尚未设置并且调用了 `save` 时，模型将在后端 REST 服务上触发一个创建操作（HTTP POST），而当指定了 ID 时，将使用更新（HTTP PUT）操作。这是一种简单的方法，确保无论您的模型是新创建的还是自上次从服务器检索后经过编辑的，都可以使用同一个 `save` 函数。

`save` 函数可以在不传参的情况下调用，也可以接受您想要持久化到服务器的一组属性，以及一个包含成功和错误情况处理程序的 `options` 哈希表。

```
thisBook.save(thisBook.attributes,
    {
        success: function(model, response, options){
            console.log('Model saved');
            console.log('Id: '  + thisBook.get('id'));
        },
        error: function(model, xhr, options){
            console.log('Failed to save model');
        }
    });
```

在调用远程 API 端点时，成功和错误处理程序非常重要，因为您无法确定保存模型的调用总会成功。一旦调用完成并返回，将调用相应的回调函数，无论是成功还是错误。不要忘记，调用是异步进行的，因此 `save` 方法之后的任何代码行都不会等待保存先完成。

`save` 方法将调用由 `Node.js` 服务器提供的以下代码片段：

```
app.post('/books/', function (request, response) {
    response.header('Access-Control-Allow-Origin', '*');
    var book = request.body;
    console.log('Saving book with the following structure ' + JSON.stringify(book));
    book.id = bookId++;
    response.send(book);
});
```

此函数最重要的部分在于它为图书提供了新的 ID，并以 JSON 格式返回了图书对象的新结构。由于此图书对象现在有了一个分配的 ID，任何后续的 `save` 方法都将在后端触发更新操作。

请记住，如果指定了验证函数，它将在 `save()` 执行期间被调用。如果验证失败，模型将不会发送到服务器。

### 检索模型

如果您想将模型对象的状态重置为与服务器端相同，可以调用 `fetch()` 函数。同样，此函数接受一个包含成功和错误回调的 `options` 哈希表。

```
thisBook.fetch({
    success: function(model, response, options){
        console.log('Fetch success');
    },
    error: function(model, response, options){
        console.log('Fetch error');
    }
});
```

如果 `fetch` 函数的执行检测到服务端和客户端的模型存在差异，则会触发一个 change 事件。当您想确保应用程序与后端服务同步，或者需要在应用启动时填充模型对象时，这非常有用。

### 删除模型

您可能希望执行的最后一个服务器操作是从后端删除模型的删除操作。

```
thisBook.destroy({
    success: function(model, response, options){
        console.log('Destroy success');
    },
    error: function(model, response, options){
        console.log('Destroy error');
    },
    wait: true
});
```

如果模型是新建的并且尚不存在于服务器上，`destroy` 操作将失败。添加 `wait: true` 选项可确保在模型从客户端上任何包含它的 `Backbone.Collection` 中移除之前，已成功将其从服务器上删除。

### 解析服务器响应

在调用 `save()` 或 `fetch()` 函数时，您可能希望通过添加额外的属性或移除不必要的属性来解析模型以丰富您的数据模型。您可以通过将 `parse()` 函数添加到模型定义中来做到这一点。以下示例展示了如何在保存或获取操作后向您的数据模型添加新属性：

```
Book = Backbone.Model.extend({
    parse: function(response, xhr) {
        response.bookType = 'ebook';
        return response;
    }
});
```

请注意，除了添加新属性，您还可以进行其他更改，以使 API 返回的数据适用于您的前端应用程序，例如更改属性的货币单位。

### 扩展您自己的模型

与任何面向对象语言一样，有时您会希望在模型层内创建层次结构。Backbone 模型的优点之一是其扩展机制，您可以使用它来扩展自己的模型。

您可能还记得，在创建新模型时，需要在创建过程中使用 `Backbone.Model.extend`。

```
Book = Backbone.Model.extend({
```

您可以使用相同的格式进一步扩展它。在下面的例子中，让我们创建一个 `EBook` 模型，通过一个额外的方法来扩展原始的 `Book` 模型：

```
EBook = Book.extend({
    getWebLink: function(){
        return 'http://www.apress.com/' + this.get('name');
    }
});

var ebook = new EBook({name: "Beginning Backbone", author: "James Sugrue"});
console.log(ebook.getWebLink());
```

要调用父类中的函数，您需要使用原型显式地调用它并传入 `this`，以便当前模型被用作上下文。

```
EBook = Book.extend({
    printDetails: function(){
        console.log('An ebook');
        Book.prototype.printDetails.call(this);
    }
});
```

上一段代码导致在显示 `EBook` 模型中定义的额外打印语句之后，执行来自 `Book` 模型的 `printDetails` 函数。如果此函数涉及任何参数，它们将在 `this` 引用之后传递。

稍后我们将看到一些可用的 Backbone 插件，它们会使这种调用变得更简单。

## Backbone 集合

在上一节中，我们重点讨论了单个模型，但通常 Backbone 应用程序会使用 `Backbone.Collection` 来提供有序的模型集合。这样做有一些有用的副作用，例如能够从后端服务器获取整个集合，并监听集合中任意模型的事件。

定义集合时，你总是需要传入包含的模型。延续 `Backbone.Model` 部分中使用的书籍示例，我们将把书籍集合定义为一个图书馆。

```
//Define a collection based on book
var Library = Backbone.Collection.extend({model: Book});
```

与 `Model` 一样，可以在集合的定义中提供一个 `initialize` 方法，该方法会在创建集合的新实例时被调用。这对于设置集合的事件监听器非常有用。

```
var Library = Backbone.Collection.extend({model: Book,
        initialize: function(){
                console.log('Creating a new library collection');
        }
});
```

当创建一个新的 `Library` 对象时，你会看到 `initialize` 函数中的 `console.log` 语句输出到控制台。

```
var myLibrary = new Library();
```

### 构造函数

创建集合实例时，你可以传入一个模型对象数组来用一些初始内容填充它。

# 操作集合

由于集合在应用程序的整个生命周期中都会使用，其内容必然会发生变化，因此你需要在构造之后操作集合。虽然可以使用 `.models` 属性来获取包含的原始模型数组，但还有更实用的工具方法来操作集合。

```javascript
var bookOne = new Book({name: 'Beginning Backbone', author: 'James Sugrue'});
var bookTwo = new Book({name: 'Pro Javascript Design Patterns', author:'Dustin Diaz, Ross Harmes'});
var myLibrary = new Library([bookOne, bookTwo]);
console.log('Library contains ' + myLibrary.length + ' books');
```

`.length` 属性允许你获取集合中当前包含的模型数量。注意，`.size()` 函数返回相同的数字。

## 添加模型

可以使用 `.add` 方法轻松地将新模型添加到集合中。

```javascript
var bookThree = new Book({name: 'Pro Node.js for Developers', author: 'Colin J. Ihrig'});
myLibrary.add(bookThree);
console.log('Library contains ' + myLibrary.length + ' books');
```

`.add` 方法也接受一个书籍数组。

```javascript
var bookFour = new Book({name: 'Pro jQuery', author: 'Adam Freeman'});
var bookFive = new Book({name : 'Pro Javascript Performance', author: 'Tom       Barker'});
myLibrary.add([bookFour, bookFive]);
console.log('Library has ' + myLibrary.length + ' books');
```

如果传入 `.add` 的模型已经存在于集合中，它将被忽略。但是，如果在调用中包含 `{merge: true}`，则属性将合并到重复的模型中。

```javascript
myLibrary.add(bookOne, {merge:true});
console.log('Library has ' + myLibrary.length + ' books');
```

注意，当模型被添加到集合时，会触发一个 `add` 事件。

你也可以使用 `.push()` 函数将模型添加到集合的末尾，可以传入一个数组或单个模型，就像 `.add` 函数一样。

```javascript
myLibrary.push(bookFive);
console.log('Library has ' + myLibrary.length + ' books');
```

要执行与 `.push()` 相反的操作，并将模型添加到集合的开头，请使用 `.unshift` 函数。

```javascript
myLibrary.unshift(bookFive);
console.log('Library has ' + myLibrary.length + ' books');
```

## 移除模型

如你所料，有一个 `.remove` 函数可用于移除单个模型或模型数组。

```javascript
myLibrary.remove(bookFive);
console.log('Library contains ' + myLibrary.length + ' books');

myLibrary.remove([bookThree, bookFour]);
console.log('Library contains ' + myLibrary.length + ' books');
```

当模型被移除时，会触发一个 `remove` 事件。在监听器中使用的 `options` 对象可以访问被移除元素的索引。

```javascript
var Library = Backbone.Collection.extend({model: Book,
        initialize: function(){
                this.on("remove", function(removedModel, models, options){
                        console.log('element removed  at ' + options.index);
                });
        }
});
```

`.pop()` 函数移除并返回集合中的最后一个模型。

```javascript
var lastModel = myLibrary.pop();
```

要移除集合中的第一个模型，请使用 `.shift()` 函数而不是 `.pop()`。这也会返回你正在移除的模型。

```javascript
var firstModel = myLibrary.shift();
```

## 重置集合

`reset` 函数用于在一次调用中替换集合中的模型集。

```javascript
myLibrary.reset([bookOne]);
console.log('Library contains ' + myLibrary.length + ' books');
```

你可以通过调用不带参数的 `reset` 方法来一次性清空集合。

```javascript
myLibrary.reset();
console.log('Library contains ' + myLibrary.length + ' books');
```

使用 `reset` 会触发一个单一的 `reset` 事件，而不是一系列的 `remove` 和 `add` 事件。这对性能有好处，因为你的应用程序只需要对重置操作做出一次响应。

## 智能更新集合

根据 Backbone 官方文档的描述，`set` 函数用于对集合执行智能更新。通过传入一个模型数组，`set` 遵循以下规则：

- 如果模型中尚不存在于集合中，它将被添加。如果提供了 `{add: false}`，则忽略此规则。
- 如果模型已存在于集合中，则属性将被合并。如果提供了 `{merge: false}`，则忽略此规则。
- 如果集合中存在一个不在数组中的模型，它将被移除。如果提供了 `{remove: false}`，则忽略此规则。

以下代码片段展示了如何忽略移除规则：

```javascript
myLibrary = new Library([bookOne, bookTwo]);
console.log('Library contains ' + myLibrary.length + ' books');

myLibrary.set([bookTwo], {remove: false});
console.log('Library contains ' + myLibrary.length + ' books');
```

如果没有 `{remove:false}`，第二次计算 `myLibrary.length` 的结果将是 1。

## 遍历集合

有许多函数可用于对集合进行操作，以便检索单个模型和遍历整个集合。以下部分将详细介绍这些函数。

### 检索模型

只要你知道了模型的 `id`，就可以使用 `.get` 函数从集合中检索模型。回想一下，在模型与后端服务同步之前，使用的是 `cid` 属性。

```javascript
var aBook = myLibrary.get('c5');
console.log('Retrieved book named ' + aBook.get('name'));
```

如果没有模型与用作参数的 `id` 或 `cid` 匹配，此函数返回 `undefined`。

如果你不想使用 ID，也可以使用 `.at` 函数，它接受模型在集合中的索引。如果集合未排序，`index` 参数将引用插入顺序。

```javascript
var anotherBook = myLibrary.at(1);
console.log('Retrieved book named ' + anotherBook.get('name'));
```

### 遍历集合

尽管你可以使用简单的 `for` 循环来遍历集合，如下所示：

```javascript
// 一个简单的循环
for(var i = 0; i < myLibrary.length; i++){
    var model = myLibrary.at(i);
    console.log('书 ' + i + ' 的名字是 ' + model.get('name'));
}
```

但 Underscore 提供了一个更优雅的工具函数来帮助遍历集合，即 `forEach` 函数。

# Backbone.js 集合实用指南

## 其他实用方法

充分利用 Backbone 对 Underscore 的依赖，`Backbone.Collection` 提供了许多实用方法。其中一些已经在前面章节列出。让我们来探索剩下的函数。

### 排序集合

使用 `sortBy` 函数，你可以选择一个属性作为依据来对集合进行排序。

```javascript
// 排序集合
var sortedByName = myLibrary.sortBy(function (book) {
   return book.get("name");
});
console.log("排序后的版本：");
sortedByName.forEach(function(model){
    console.log('书的名字是 ' + model.get('name'));
});
```

请注意 `sortBy` 返回的是集合中模型的有序数组表示，它并不会实际改变集合内模型的顺序。

通过使用比较器（comparators），你可以强制一个排序顺序，该顺序将始终用于你的集合。最常用的方法是在集合定义时定义比较器函数。以下示例演示了如何创建一个按书名排序的比较器：

```javascript
var Library = Backbone.Collection.extend({model: Book,
    initialize: function(){
        // 初始化函数内容...
    },
    comparator:  function(a, b) {
        return a.get('name') < b.get('name') ? -1 : 1;
    }
});
```

现在集合将始终按书名排序。但是，如果你改变了一个模型中某个属性的值，集合不会自动重新排列顺序。相反，可以通过调用集合的 `sort` 函数来应用顺序。

```javascript
myLibrary.at(0).set('name', 'Z');
myLibrary.forEach(function(model){
    console.log('书的名字是 ' + model.get('name'));
});
// 强制排序
myLibrary.sort();
myLibrary.forEach(function(model){
    console.log('书的名字是 ' + model.get('name'));
});
```

请注意，当向集合添加新模型时，排序顺序会重新应用，除非在添加时传入了 `{sort: false}` 选项。

### 打乱顺序（Shuffle）

如果你需要获取集合中模型的随机化版本，`shuffle()` 函数将返回一个经过打乱算法处理后的模型数组。

```javascript
var shuffled = myLibrary.shuffle();
```

现在遍历集合将会以不同的顺序展示书籍。

```javascript
myLibrary.forEach(function(model){
    console.log('书的名字是 ' + model.get('name'));
});
```

### 获取属性列表

有时你可能想要获取集合中某个特定属性的所有实例列表。使用 `.pluck()` 函数可以轻松实现。

```javascript
console.log('集合中作者列表：');
var authors = myLibrary.pluck('author');
authors.forEach(function(authorName){
    console.log(authorName)
});
```

### 搜索

基于向搜索函数传递一组键值对，可以使用多种有用的搜索机制。

例如，要获取匹配特定条件的模型对象数组，可以使用 `.where` 函数。

```javascript
var results = myLibrary.where({author:'James Sugrue'});
console.log('找到：' + results.length + ' 个匹配');
```

`findWhere` 函数可用于查找第一个匹配查询的模型，而不是返回匹配项的数组。

```javascript
var result = myLibrary.findWhere({author:'James Sugrue'});
console.log('结果：' + result.get('author'));
```

你还可以按公共属性值对模型对象进行分组。例如，如果每本书都有一个表示是否已出版的布尔值，我们可以如下进行分组：

```javascript
var byPublished = myLibrary.groupBy('published');
myLibrary.forEach(function(model){
    console.log('书的名字是 ' + model.get('name'));
});
```

`Collections` 还有更多可用的实用方法，你可以阅读 [`http://backbonejs.org`](http://backbonejs.org/) 来了解。不过，这里列出的方法应该足以让你入门了。

## 与服务器交换数据

为了结束我们对 `Collections` 的概述，我们将看看集合如何与服务器交互。在本书的第一部分，当我们讨论 `Backbone.Model` 时，我们看到了如何使用另一台服务器上运行的 REST API 来检索、保存、更新和获取单个模型。

集合可以通过管理所有模型并提供工具来更高效地组织这一切，使保存模型到服务器变得稍微容易一些。

### 设置

你需要做的第一件事是使用 `url` 属性告诉集合数据持久化的服务器地址。我们将指向运行在 8080 端口上，带有 `/books/` API 端点的 Node.js 服务器。

```javascript
var Library = Backbone.Collection.extend({model: Book,
    url: 'http://localhost:8080/books/',
    initialize: function(){
        ....
```

### 从服务器检索数据

与 `Backbone.Model` 一样，集合有一个 `.fetch` 方法，用于从服务器检索集合数据。

```javascript
var myOnlineLibrary = new Library();
myOnlineLibrary.fetch({
    success: function(e){
        console.log('获取到数据');
    },
    error: function(e){
        console.log('出现错误');
    }
});
```

与所有处理在线操作的函数一样，你可以提供成功和失败的回调来检测操作是否正确完成。

与 `Backbone.Model` 类似，可以在集合级别定义一个 `parse` 方法，允许你自定义从服务器返回的数据。`response` 参数包含执行 `.fetch` 后检索到的所有模型对象的数组。

```javascript
var Library = Backbone.Collection.extend({model: Book,
    // ......
    parse: function(response, xhr) {
        // 在此处进行自定义
        return response;
    },
});
```

### 保存数据到服务器

虽然检索数据是批量进行的，但保存模型到服务器仍然是逐个进行的，如同本章 `Backbone.Model` 部分所述。

```javascript
// 向集合添加模型并保存到服务器
myOnlineLibrary.add(bookOne);
bookOne.save({
    success: function(e){
        console.log('书已保存到服务器');
    },
    error: function(e){
        console.log('保存书时出错');
    }
});
```

然而，如果你使用 `Backbone.Collection.create` 函数而不是 `.add`，模型将同时被添加到集合并持久化到服务器。

```javascript
// 添加到集合同步保存到服务器
myOnlineLibrary.create(bookTwo);
```

### 从服务器删除数据

从服务器删除模型是在单个模型级别上使用 `.destroy` 方法完成的，如上一节所述。

```javascript
bookOne.destroy({
    success: function(e){
        console.log('书已删除')
    },
    error: function(e){
        console.log('删除书时出错');
    }
});
```

## 集合快速参考

# 本章小结

本章重点介绍了 Backbone 应用的数据层，前半部分描述了 `Model` 对象提供的功能，后半部分则讨论了集合。我们了解了集合如何优雅地封装 `Model` 对象；最重要的是，我们学习了运行在 Node.js 服务器上且暴露 REST API 的服务端与 Backbone 客户端应用之间的交互方式。

## 集合方法参考

如您所见，Backbone 提供了许多实用的函数用于处理集合。表 3-3 列出了这些方法，方便快速查阅。

**表 3-3.** 集合可用方法

| 函数/属性 | 描述 |
| --- | --- |
| `.length()` / `.size()` | 返回集合中包含的模型对象数量 |
| `.models` | 返回包含所有模型对象的原始数组 |
| `.add([models])` | 向集合中添加单个模型或模型数组 |
| `.push([models])` | 向集合末尾添加单个模型或模型数组 |
| `.unshift([models])` | 向集合开头添加单个模型或模型数组 |
| `.remove([models])` | 从集合中移除单个模型或模型数组 |
| `.pop()` | 移除并返回集合末尾的模型 |
| `.shift()` | 移除并返回集合开头的模型 |
| `.reset()` | 替换集合中的模型，或无参数调用时清空集合 |
| `.get(id)` | 使用 `id` 或 `cid` 作为参数从集合中检索模型 |
| `.at(index)` | 从集合中检索指定索引位置的模型 |
| `.forEach(function)` | 遍历集合中的每个模型 |
| `.sortBy()` | 根据特定属性对集合中的模型进行排序，并返回模型数组 |
| `.sort()` | 重新应用集合的排序顺序 |
| `.where({key:value})` | 查找集合中所有具有指定键值对的模型 |
| `.findWhere({key:value})` | 查找第一个具有指定键值对的模型实例 |
| `.url` | 指向 REST API 端点的位置 |
| `.fetch()` | 从服务器检索整个集合 |
| `.create(<model>)` | 向集合中添加新模型并保存到服务器 |

## 集合事件参考

与模型类似，集合在内容发生变化时会触发多个事件。表 3-4 列出了这些事件，方便快速查阅。

**表 3-4.** 与 Backbone 集合相关的事件

| 函数/属性 | 描述 |
| --- | --- |
| `add` | 检测到模型已被添加到集合中 |
| `remove` | 检测到模型已从集合中移除 |
| `reset` | 检测到集合已被重置 |
| `sort` | 检测到集合正在排序 |
| `change` | 检测到集合中的模型正在发生变化 |
| `change:<attribute name>` | 检测到集合中的 `<attribute name>` 模型属性正在发生变化 |
