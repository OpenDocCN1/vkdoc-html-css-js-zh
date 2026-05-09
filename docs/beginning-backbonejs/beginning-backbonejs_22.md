# 11. Backbone 最佳实践

## 摘要

在前面的章节中，我们了解了如何使用 Backbone 以及一些有用的扩展来创建高质量的 Web 应用程序。然而，随着你的应用规模扩大并开始吸引真正的用户，你需要自信地使用底层技术。虽然像 Marionette 和 Thorax 这样的框架在视图管理方面减少了出错的可能性，但理解那些能确保你充分利用 Backbone 的技巧仍然很有价值。本章汇总了开发者在 Backbone 实践中采用的最佳实践。你将学习如何有效管理模板、避免内存泄漏，并确保你的应用通过以下章节列出的提示获得最佳性能。

## 通用最佳实践

在深入细节实践之前，有一些简单的事情在构建 Backbone 应用时你需要牢记。

### 保持清晰的关注点分离

人们常强调 Backbone 的非限制性本质，即你可以自由地使用这个库。如果没有适当的约束，代码很容易失控。值得牢记“关注点分离”原则。最需要坚持这一原则的主要例子是在模型内部引用视图。尽管在某些情况下这很诱人，但你应该尽可能抵制这种做法。

虽然视图为了填充数据会引用模型或集合，但模型永远不需要知道它在哪个视图中被渲染。强制实施这种分离有许多好处：

*   可以在不需要渲染视图的情况下测试模型/集合。
*   与视图相关的逻辑将被排除在模型之外，保留在视图中（因为模型对视图一无所知）。
*   如果为移动和桌面浏览器分别构建了视图，模型也不会增加额外的复杂性。无论何种情况，模型都保持相同。

因此，你所有的数据类将更加模块化和可复用。

### 为你的应用添加错误处理模式

对于任何涉及数据输入的应用，你都需要建立一个一致的错误处理和通知系统。在构建应用时，这应该是你首先着手处理的事项之一。它能确保你团队中的所有其他开发者遵循相同的模式，并减少重复代码的数量。同时，由于错误能以一致且有用的方式显示，也能确保更好的用户体验。

#### 处理验证错误

一种常见的模式是从 `validate` 函数返回一个错误对象。当视图通过 `invalid` 事件收到模型验证失败的通知时，它可以检查这个错误对象，并相应地更新视图。

下面的 `validate` 方法返回一个错误数组，其中包含验证过程中遇到的各个错误。注意，如果没有错误，函数将不返回任何内容，表示模型有效。

```
validate: function(attrs) {

   var errors = [];

   if(attrs.email.indexOf('@') == -1) {

       errors.push({

           'message': 'Invalid email address',

           'attribute': 'email'

       });

   }

   if(attrs.name.length < 1) {

       errors.push({

           'message': 'Name must be specified',

           'attribute': 'name'

       });

   }

   //if there are no errors and no return, then there is no invalid event.

   if(errors.length) {

       return errors;

   }

}
```

视图监听模型触发的 `invalid` 事件，检查第二个参数（`validate` 函数的返回值）中的错误，并相应地更新视图。

```
this.model.on('invalid', function(model, errors) {

       for(var i =0 ; i < errors.length; i++){

           if(errors[i].attribute === 'email'){

               //highlight the email field with errors[i].message

           }

           if(errors[i].attribute === 'name'){

               //highlight the name field with errors[i].message

           }

       }

});
```

Philip Whisenhunt 和 Derick Bailey 建议对这种模式进行更高级的变体，即为 `validate` 函数中可能发生的每个错误触发单独的事件。其优点是视图可以单独处理每个错误，而不是因为触发了 `invalid` 事件而编写一个大型函数。`validate` 函数非常相似，只是触发事件而不是添加错误。

```
validate: function(attrs) {

   var errors = [];

   if(attrs.email.indexOf('@') == -1) {

      this.trigger('invalid:email', 'Invalid email address', this);

   }

   if(attrs.name.length < 1) {

        this.trigger('invalid:name', 'Name must be specified', this);

   }

}
```

在视图中，每个错误事件都被监听。

```
this.model.on('invalid:email', function(error) {

       //highlight the email field with the error

});

this.model.on('invalid:name', function(error) {

       //highlight the name field with the error

});
```

如你所见，这提供了一种更优雅、解耦的错误处理方法。

#### 添加通用错误提示

对于自定义用户界面，值得添加一个可以在所有视图中使用的通用提示对话框。这可以通过扩展 `Backbone.View` 创建你自己的超类来实现，或者使用本章后面“在视图间共享公共代码”部分详述的 mixin 方法。



## 避免内存泄漏

无论使用何种编程语言或框架，内存泄漏是所有应用程序中最常见的错误之一。当本不应再被使用的对象或视图在系统中仍然“存活”时，就会发生内存泄漏。这通常是对象未被正确释放的结果。在 Backbone 中，这是一个常见问题，但一旦你了解原因，就很容易解决。

### 关闭视图时解绑事件

Backbone 中出现内存泄漏最常见的原因之一是，当前未使用的视图（因导航到新视图导致）仍然绑定着事件。我们在本书中之前将这些视图称为僵尸视图。

正如我们已经看到的，事件是 Backbone 应用程序基础设施的重要组成部分，它实现了视图、模型和集合之间真正的解耦。事件绑定处理不当的一个症状是，单个事件被意外地多次触发。

让我们首先回顾一下在视图中处理事件的不同方式。视图通过 `on` 函数将自身绑定到模型的 `change` 事件。

```javascript
MyView = Backbone.View.extend({

  initialize: function(){
    this.model.on('change', this.render, this);
  },

});
```

使用上述代码，模型中的任何更改都会导致视图中的 `render` 函数被调用。请注意，在 Backbone 的早期版本中，此函数名为 `bind` 而非 `on`；旧名称仍然支持，但最好使用 `on`。

视图也使用 `events` 哈希将 DOM 事件绑定到处理函数。

```javascript
MyView = Backbone.View.extend({

  events: {
    'click #search': 'runSearch'
  },

  runSearch: function(){
      // 搜索代码
  },

});
```

当应用程序使用完视图并渲染新视图时，这些事件仍然绑定着，因此该视图会一直保留在内存中。Backbone 新手可能会认为 `remove` 函数（它将视图从 DOM 中移除）会处理所有事件，但事实并非如此。

你可以在应用程序代码中采取几种不同的方法来处理此问题。

### 使用 `listenTo` 处理事件

使用 `listenTo` 处理事件的一个优势是，当调用视图的 `remove` 函数时，`stopListening` 函数会依次被调用，从而解绑所有通过 `listenTo` 绑定的事件。以下示例展示了如何使用此函数绑定模型的所有 `change` 事件：

```javascript
MyView = Backbone.View.extend({

  initialize: function(){
    this.listenTo(model, 'change', this.render);
  },

});
```

在处理事件时，使用 `listenTo` 替代 `bind` 或 `on` 函数可以作为一个不错的原则。

### 使用模式正确关闭视图

另一种选择是为视图编写一些额外的代码，以确保它们被正确释放。在路由器中切换视图时，你可以确保在显示新视图的同时，关闭之前的任何视图。Derick Bailey 在他的博客 `http://lostechies.com/derickbailey/2011/09/15/zombies-run-managing-page-transitions-in-backbone-apps/` 上为此提出了一种有用的模式，该模式被用于他的 Marionette 库，并且也适用于他的 EventBinder 插件。

此模式始于为 `Backbone.View` 添加一个 `close` 函数。这可以通过多种方式实现，但最有效的是扩展 `Backbone.Views` 的原型，添加一个 `close` 函数。`close` 函数将封装从 DOM 中移除视图和解绑所有事件的功能。

```javascript
Backbone.View.prototype.close = function(){
        this.remove();
        this.unbind();
        if (this.onClose){
                this.onClose();
        }
}
```

请注意，该函数的最后几行引用了 `onClose()`。这是另一个可以添加到每个独立视图中的额外函数，用于解绑视图在其整个生命周期中添加的所有事件。

```javascript
var MyView = Backbone.View.extend({

   initialize: function() {
     this.model.on('change', this.render, this);
  },

   render: function() {
       // 渲染模板
   },

   onClose: function(){
        // 解绑在视图生命周期内绑定的事件
        this.model.off('change');
   }

});
```

正如你在前面的代码示例中所看到的，视图在初始化时注册监听模型上的 `change` 事件，并在调用 `onClose` 函数时注销此监听器。

为了调用关闭视图的操作，需要一个能够跟踪当前焦点视图的对象。为此，Derek Bailey 建议添加一个单一的 `AppView` 控制器，以便在切换视图时供路由器使用。

```javascript
function AppView(){
  this.showView(view) {
   if (this.currentView){
     this.currentView.close();
   }
   this.currentView = view;
   this.currentView.render();
   $("#mainContent").html(this.currentView.el);
 }
}
```

`AppView` 对象暴露了一个函数 `showView`。此函数在渲染新视图并将其添加到页面之前，会先关闭 `currentView`（如果存在）。

现在，当需要根据触发的不同路由显示视图时，`Router` 可以使用这个 `AppView` 对象。首先创建视图，然后使用 `showView` 函数显示视图并确保之前的视图被正确释放。

```javascript
MyRouter = Backbone.Router.extend({

  routes: {
    "": "home",
    "search/:query": "showResults"
  },

  initialize: function(options){
    this.appView = options.appView;
  },

  home: function(){
    var homeView = new HomeView();
    this.appView.showView(homeView);
  },

  showPost: function(query){
    // 获取搜索结果的代码...
    var searchResultsView = new SearchResultsView({model: results});
    this.appView.showView(searchResultsView);
  }

});
```

再次重申，值得注意利用像 Marionette 这样的框架可以帮助你实现与上述类似的效果，但更深入地理解视图被移除时的实际运作机制总是有益的。

## 渲染视图

在处理视图时，你可以使用多种实践方法。这些方法涵盖从提高 `render` 函数的粒度到改进模板的使用方式等各个方面。



### 仅渲染所需部分

本书迄今为止的所有示例中，每个视图都只使用了单一的渲染函数。尽管这很合理，但用不了多久，你就会为更复杂的视图编写出复杂的渲染函数。更重要的是，在某些情况下，你需要确保视图保持最新。以 Twitter 应用为例：当获取新数据时，会执行一个单一的渲染函数来渲染时间线中的所有推文。这些推文会显示类似“一分钟前”的时间戳。理想情况下，这些时间应该能自动更新以避免变得过时，但每次都重新渲染整个集合，甚至只是单条推文的部分，都显得大费周章。如果能够将渲染拆分开来，让时间戳可以独立更新，那将会更加实用。

一种处理方式是将 `render` 函数拆分为不同的区域。通过拆分模板，可以让主 `render` 函数调用多个负责特定功能的小函数。

以单条推文为例，`render` 函数可以拆分为两部分：一部分负责主内容，另一部分负责时间戳。

```
render: function(){
  this.renderContent().
  renderTimestamp();
  return this;
},
```

这种方法的好处在于，后续你可以只更新视图的某一部分，而无需重新运行整个 `render` 函数。下面的代码片段展示了如何构建这样的视图：

```
var com = com || {};
com.apress = com.apress || {};
com.apress.view = com.apress.view || {};

com.apress.view.TweetView = Backbone.View.extend({
  el: '#tweet',
  $timestamp: null,
  $content : null,
  tweetTemplate: Handlebars.compile($("#tweet-template").html()),
  timestampTemplate: Handlebars.compile($("#timestamp-template").html()),
  tweet : null,
  initialize: function(options){
    this.tweet = options.tweet;
    this.render();
  },
  render: function(){
    this.renderContent().
    renderTimestamp();
    return this;
  },
  renderContent: function(){
    //如果内容已经渲染，则移除
    if(this.$content){
      this.$content.remove();
    }
    //处理主内容模板
    $content = this.tweetTemplate({
      tweet: this.tweet.toJSON()
    });
    this.$el.append($content);
    return this;
  },
  renderTimestamp: function(){
    //如果时间戳已经渲染，则移除
    if(this.$timestamp){
      this.$timestamp.remove();
    }
    //处理时间戳模板
    $timestamp = this.timestampTemplate({
      time: this.tweet.getTimestamp()
    });
    this.$('#timestamp').append($timestamp);
    return this;
  }
});
```

注意这里使用了两个独立的模板，并且当每个低层级的 `render` 函数被调用时，它们会首先清除之前已渲染过的元素。每个函数还返回一个指向 `this` 的指针，使得 `render` 方法能够以更简洁的方式链式调用。

视图所使用的模板如下。主内容模板包含一个用于放置时间戳的占位符。

```
<script type="text/x-handlebars-template" id="tweet-template">
  <p>{{tweet.text}}</p>
  <div id="timestamp"></div>
</script>

<script type="text/x-handlebars-template" id="timestamp-template">
  <p><i>{{time}}</i></p>
</script>
```

为完整起见，以下是填充并显示该视图的代码。`Tweet` 模型是一个简单的 `Backbone.Model`，仅包含一个简单的 `text` 属性，以及一种通过 `moment.js` 以可读方式呈现时间戳的方式。

```
Tweet = Backbone.Model.extend({
  timestamp: null,
  initialize: function(){
    timestamp = new Date();
  },
  getTimestamp: function(){
    var friendly = moment(timestamp).fromNow();
    return friendly;
  }
});

var tweet = new Tweet({text: 'James'});
var tweetView = new com.apress.view.TweetView({tweet: tweet});
```

即使你不需要在特定时间间隔更新视图的某些部分，拆分 `render` 函数也能让你的代码更易于他人阅读，并降低视图代码变得难以维护的风险。

### 使用部分模板减少模板臃肿

随着应用的增长，在定义模板时难免会出现一些重复，因为某些模板的部分内容会被复制到新模板中。这可能导致维护问题，并违背了软件开发中备受推崇的“不要重复自己（DRY）”原则。本节将探讨 Handlebars 和 Underscore 中可用的解决方案。

#### Handlebars 中的部分模板

Handlebars 通过 `registerPartial` 函数为部分模板提供了开箱即用的支持。当一个模板在其定义中包含另一个部分模板时，会使用 `{{> partialname}}` 辅助函数将其放置在正确的位置。下面的时间线模板示例展示了如何实现这一点：

```
<script id='timeline-template' type='text/x-handlebars-template'>
  {{#each tweetlist}}
    {{> tweet}}
  {{/each}}
</script>
```

该模板期望注册另一个名为 `tweet` 的部分模板。如前所述，这是通过 `Handlebars.registerPartial` 函数完成的。

```
Handlebars.registerPartial('tweet', $('#tweet-partial').html());
```

部分模板本身的定义方式与任何 Handlebars 模板相同。为了便于快速识别，一个有用的约定是在模板 ID 的末尾添加 `-partial`。

```
<script id='tweet-partial' type='text/x-handlebars-template'>
  <div class='tweet'>
    <h2>{{username}}</h2>
    <div class='text'>{{text}}</div>
  </div>
</script>
```

一旦部分模板被及时注册，你的 Backbone 视图就可以像平常一样编译主模板。

#### Underscore 中的部分模板

在 Underscore 模板中也可以使用部分模板。尽管不像 Handlebars 那样有辅助函数，但由于 Underscore 的 JavaScript 语法特性，实现起来仍然相当简单。

首先，定义部分模板用于显示单个项目，这里以一本书为例。

```
<script type='text/template' id='book-template'>
  <li>
    <b><%=name %></b> by <%=author %>
  </li>
</script>
```

要使用这个部分模板，主模板可以像调用 JavaScript 函数一样引用它，并将项目作为参数传递。

```
<script type='text/template' id='library-template'>
  <ul>
    <% for (var i = 0; i < library.length; i++) { %>
      <% itemTemplate(library[i]);  %>
    <% } %>
  </ul>
</script>
```

在 Backbone 视图中，`render` 函数需要能够访问这两个模板。

```
LibraryView = Backbone.View.extend({
  template: _.template($('#library-template').html()),
  itemTemplate: _.template($('#book-template').html()),
  render: function() {
    var html = this.template({
      library: library /* a collection */,
      itemTemplate: this.bookTemplate
    });
    $(this.el).append(html);
  }
});
```

这种方法可以在你的应用中所有可复用的 Underscore 模板中使用。



### Handlebars 中的模板预编译

模板预编译是指在构建时准备模板，以减少渲染视图时所需的编译时间。它还可以帮助减小模板库的大小，因为生产环境中运行的应用程序不再需要编译引擎。如第 4 章所述，Handlebars 已经提供了完整版和运行时版之间的分离。

使用预编译模板带来的性能提升不容小觑，尤其是在移动设备上。

在第 4 章中，我们创建了一个简单的 Handlebars 示例，为图书馆藏集合提供了一个模板。为了预编译此模板以便在 Backbone 应用程序中使用，您首先需要安装 Handlebars 的 Node 包。

```
npm install handlebars -g
```

接下来，您需要将模板从 HTML 页面中取出，并将其放入一个单独的文件中。通常，每个模板会被移动到一个 `.handlebars` 文件中。对于这个图书馆藏模板，将其移到 `library.handlebars` 文件中是合理的。

```
<script type="text/x-handlebars-template" id="library-template">
<ul>
  {{#each library}}
    <li>
    {{#log name 4}}{{/log}}
      <em>{{name}}</em> by {{author}}
    </li>
  {{/each}}
</ul>
</script>
```

要预编译此模板，`handlebars` 命令采用以下形式：

```
handlebars <input> -f <output>
```

对于 `library.handlebars` 文件，命令如下：

```
handlebars library.handlebars -f templates.js
```

这将生成一个 `templates.js` 文件，可用于 HTML 中。在这种情况下，模板将被标识为 `library`，因为编译器使用输入文件的名称作为模板名称。您可以通过打开 `templates.js` 来验证这一点，您会在文件开头看到 `templates` 数组包含一个 `library` 条目。

```
(function() {
  var template = Handlebars.template, templates = Handlebars.templates = Handlebars.templates || {};
templates['library'] = template(function (Handlebars,depth0,helpers,partials,data) {
```

> **注意**  
> 稍后您将使用此数组 (`Handlebars.templates[]`) 来访问预编译模板。

`templates.js` 文件需要包含在 HTML 页面中，并且只要您的 JavaScript 代码中没有调用编译模板的操作，就可以改用 Handlebars 运行时版。

```
<script src="js/external/handlebars-runtime.js"></script>
<script src="js/templates/templates.js"></script>
```

最后，在 `Backbone.View` 中，您不再需要使用 `Handlebars.compile` 来处理模板，而是可以通过 `Handlebars.templates` 数组来访问模板。

```
template: Handlebars.templates['library'],
```

重构后的 `LibraryView` 其余部分保持不变。

```
LibraryView = Backbone.View.extend({
    initialize: function(){
        console.log('View created');
    },
    events: {
        'click ' : 'alertBook'
    },
    template: Handlebars.templates['library'],
    render: function(){
        var self = this;
        var output = self.template({'library': self.collection.toJSON()});
        self.$el.append(output);
        return self;
    },
    alertBook: function(e){
        alert('Book clicked');
    }
});
```

## 视图管理

当你的应用程序变得更大时，视图周围可能会有很多复杂的问题。以下部分将讨论 Backbone 开发者在这个领域遇到的一些常见问题，包括如何在视图之间共享公共代码以及如何在子视图及其父视图之间进行通信。

### 在视图间共享公共代码

在应用程序中编写视图时，你很可能会发现有些代码需要在它们之间共享。有几种方法可以处理这个问题；一种是使用继承。但我发现的最有用的共享代码技术是提供混入（mixins），将公共代码放在一个单独的 JavaScript 文件中，可以根据需要引入。

例如，假设您想要在选定的几个视图中共享通用的警告功能。所有与警告相关的、可以共享的函数都将放在一个文件中。

```
var Mixins = Mixins || {};
Mixins.Alerts = {
    showSuccessMessage: function(message){
        console.log('Show success message');
    },
    showFailureMessage: function(message){
        console.log('Show failure message');
    }
};
```

然后，在定义视图时，可以使用 `_.extend` 函数将混入合并进来。

```
var com = com || {};
com.apress = com.apress || {};
com.apress.view = com.apress.view || {};
com.apress.view.TweetView = Backbone.View.extend(
_.extend({}, Mixins.Alerts, {
    //standard Backbone view code
}));
```

因此，混入中定义的所有函数现在都可以在 `TweetView` 对象中使用。

### 从子视图更新父视图

在你作为 Backbone 开发者的职业生涯中，某个时刻你会遇到这样一种情况：由于子视图中的更改，需要更新父视图。有两种方法可以解决这个问题：一种是将对父视图的引用传递给子视图，另一种依赖于事件机制。

传递引用是最容易阅读和理解的方法，但会导致父视图和子视图之间的耦合。以下代码展示了如何实现这一点：

```
var MainView = Backbone.View.extend({
  initialize: function() {
    var self = this;
    this._views = [];
    // create a sub view for each model
    this.collection.each(function(model) {
      self._views.push(new SubView({
        model: model,
        parentView: self
      }));
    });
  }
});
```

现在，每个 `SubView` 都有一个对父视图的引用，可以通过以下方式从子视图调用以重新渲染父视图：

```
this.parentView.render();
```

更简洁的基于事件的解决方案无需传递引用。相反，父视图只需监听来自子视图的事件即可。

```
var MainView = Backbone.View.extend({
initialize: function() {
   var self = this;
   this._views = [];
   // create a sub view for each model
   this.collection.each(function(model) {
       var subview = new SubView({
       model: model,
       parentView: self
     });
     self._views.push(subview);
     self.listenTo(subview, 'event', self.render);
   });
}
});
```

子视图只需触发事件，即可让父视图进入其 `render` 函数。

```
this.trigger('event');
```

## 网络性能

一旦你证明了应用程序的功能性，该应用程序的性能将受到审查。有大量工具可以帮助你诊断哪些地方可以进行潜在改进，并且许多文章也给出了很好的建议。本书前面已经介绍过其中一些技巧。

例如，惯例是所有 CSS 样式应位于 HTML 页面的头部，而脚本应全部加载在底部。这确保了页面最初能正确渲染，因为等待 CSS 脚本加载会阻塞渲染。同时，下载 JavaScript 资源会阻塞页面上的其他下载，因此这些操作应该放在最后。

然而，在构建 Backbone 应用程序时，还有一些其他技巧你应该牢记在心。



### 最小化页面加载时的请求数量

关于网络性能的首要秘诀之一就是减少请求次数。页面需要的每一项资源都会增加一次额外的 HTTP 请求。与其在应用中急切地填充每一个 `Backbone.Model` 和 `Backbone.Collection` 对象，不如花点时间考虑一下：当应用展示给用户时，哪些数据是立即需要的。

一种选择是使用延迟加载，即在用户实际需要数据时才从服务器加载对象。这种方法虽然有助于缩短页面加载时间，但当用户真正请求信息时，可能会导致延迟。

如果你的应用使用服务器端技术（如 Grails、JSP 或 .NET）构建，那么有可能用 JSON 数据填充部分初始模型和集合。这项技术已经变得越来越流行，尤其是 Airbnb 将其 Rendr 库作为 GitHub 上的开源项目（`https://github.com/airbnb/rendr`）提供给所有人使用。该项目允许 Backbone 代码在客户端和服务器端都能渲染。更重要的是，它使得在浏览器渲染页面时，能够轻松地将 Backbone 模型的内容传递给页面，从而让服务器执行大部分网络操作。

最后，根据对应用性能分析的结果，你可能会决定重构那些从客户端发往服务器端的 API 调用。构建能够减少 API 间数据重复的端点（end points）是一项很有用的实践。随着项目的推进，重新审视并重构 API 总是值得的。

## 感知性能

你可以运用一些技巧，让应用的响应速度看起来快得多。总的来说，你应该始终对自己应用的响应速度持批判态度。那些日复一日使用相同代码的开发者，往往会习惯应用的响应时间，而用户可能就没那么宽容了。

### 乐观的网络调用

在某些情况下，可以安全地假设对后端的调用会成功完成，尤其是当所有验证都在应用客户端执行时。如果你有一个视图，需要将一些模型属性保存到服务器，通常你会等待成功处理程序被调用后，才显示这些更改或向用户显示成功消息。

```
var MyView = Backbone.View.extend({

  events: {

    'click .save': 'save'

  },

  save: function(e) {

    // 保存模型

    this.model.save(model.attributes, {

      success: function(model, response, options) {

        // 渲染更改

      },

      error: function(model, xhr, options) {

        // 渲染错误

      }

    });

  }

});
```

如果你确信调用会成功，并且所有必要的验证已在客户端完成，那么你无需等待即可显示成功交易的结果。如果真的出了差错，错误处理器可以捕获变更并运行一些代码来撤销更改，并告知用户操作失败。

在某些情况下，这种优化可能风险太大，因此需要权衡这种更改带来的风险与收益。

### 使用文档片段渲染集合

当一个单独的视图对应整个集合时，集合发生变化后更新视图看起来可以相当流畅地进行，因为只调用了一个渲染函数，并且只对视图的 `el` 进行一次追加操作。但是，当你为集合中的每个模型对象都设置了一个单独的视图时，每次集合发生变化，重新渲染每个子视图都会影响性能，并导致用户界面看起来不稳定。

考虑以下代码，它采用了这种针对集合的方法：

```
var MainView = Backbone.View.extend({

      initialize: function() {

    var self = this;

    this._views = [];

    // 为每个模型创建一个子视图

    this.collection.each(function(model) {

      self._views.push(new SubView({

        model: model

      }));

    });

  },

  render: function() {

    var self = this;

    this.$el.empty();

    // 渲染每个子视图，然后追加

      for(var i=0; i < this.views.length; i++){

      self.$el.append(views[i].render().el);

    });

}

});
```

每次发生追加操作时，DOM 都需要重新计算 DOM 树中所有元素的位置和大小，当集合中包含大量模型时，这是一项开销巨大的操作。更简洁的做法是，先构建好元素内容，然后只进行一次 DOM 追加操作。这正是 DOM 的 `createDocumentFragment` 函数可以帮助你实现的。所有主流浏览器都支持该函数，它在后台创建一个 `Node` 对象，让你在追加到真实文档之前准备好内容。

这使得前面代码示例中的 `render` 函数变为以下形式：

```
  render: function() {

    var self = this;

    self.$el.empty();

    var fragment = document.createDocumentFragment();

     // 渲染每个子视图，然后追加

      for(var i=0; i < this.views.length; i++)

       fragment.$el.append(views[i].render().el);

     });

     this.$el.append(fragment);

}
```

注意，首先创建了一个片段，然后将每个子视图添加到其中。当循环完成且片段已包含所有视图后，整个片段就可以追加到视图的根元素 `el` 上。

### 使用模型存储额外数据

大多数 Backbone 模型的默认实现是仅表示持久化到服务器的数据。虽然这在原则上听起来不错，但在某些情况下会引起更多麻烦。考虑一个例子：我们想要跟踪视图中的某个项何时被选中。在视图类中，只需添加一个点击事件监听器来跟踪哪个项被选中。

```
com.apress.view.TweetView = Backbone.View.extend({

     // 其他代码

     tweet : null,

     events: {

         'click': 'markSelected'

     },

     markSelected: function(options){

         console.log('marking..');

         var self = this,

             $target = $(options.currentTarget);

         $target.addClass('selected');

     },

     // 其他代码

});
```

如果你想检查当前选中的模型对象是哪一个，就需要遍历 DOM 并找到哪个元素拥有 `selected` 类。

在诸如此类的情况下，在模型对象中跟踪状态是很有意义的。这样，你就可以遍历集合，找出 `selected` 属性等于 `true` 的模型对象。这会将 `markSelected` 函数改为以下形式：

```
markSelected: function(options){

         console.log('marking..');

         var self = this,

             $target = $(options.currentTarget);

         self.tweet.set('selected', true);

         $target.addClass('selected');

},
```

注意，这确实增加了一些管理变量和决定哪些变量应该持久化到服务器的开销。然而，在应用中发现项的状态，其开销远小于发现哪些类被应用于相应的 DOM 元素。同时，由于选定状态有效地与视图解耦，这也有助于提高模型对象的可测试性。



### 尽可能缓存对象

与其反复获取应用中反复使用的视图或模型的新实例，更佳的做法是缓存这些对象。用户对象就是一个很好的例子，该对象在应用的许多视图中共享。如果每次跳转到新页面时都创建用户对象的新实例，这将造成浪费，尤其是当该对象不频繁变化时。

路由器是这种模式最有用的典型场景之一。初始化时，用户模型被设置为 `undefined`。

```
initialize: function() {
   this.cached = {
      userModel: undefined
   }
},
```

如果路由器内的某个路由被触发，若模型为 `undefined` 则进行获取，否则可以返回缓存的版本。

```
mainView: function(parameter) {
   this.cached.model = this.cached.model || new UserModel({});
   new View({
       userModel: this.cached.userModel
   });
},
```

如果另一个视图（如个人资料视图）使用该模型，则无需从服务器重新获取。

```
profileView: function(parameter) {
   this.cached.model = this.cached.model || new UserModel({});
   new ProfileView({
       userModel: this.cached.userModel
   });
},
```

在某些情况下，你可能需要强制重新获取模型。此时，可以在模型自身添加一个变更监听器来触发一次数据获取。以下示例在密码被更改时强制进行数据获取：

```
initialize: function() {
   this.on("change:password", this.fetch, this);
}
```

缓存对象非常有用，能够提升应用性能，代价是略微增加内存开销，因为对象会持续存活。你还需要注意，由于缓存的存在，模型对象可能无法按预期进行更新。最好的建议是谨慎使用此模式。

