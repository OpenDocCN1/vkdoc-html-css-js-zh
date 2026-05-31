# 处理后的文本

```javascript
// TODO: 插入实际删除消息的代码
});

// 这种自我实例化使得 app 变量等于
// this 关键字的值，而 this 正是 WebMail 本身
}(this);

// 定义名为 UserInterface 的代码块，用于存放所有与用户界面
// 相关的属性和方法。此代码块与其他代码块无关，
// 仅了解应用中触发的事件
var UserInterface = new function(app) {

// TODO: 在浏览器内构建用户界面组件
$.Events.add(document.getElementById("delete-button"), "click",
function(e) {
// 阻止 HTML 内删除按钮的默认行为
e.preventDefault();

// TODO: 确定被删除消息的真实 ID
var messageId = 0;

// 通知任何监听 DELETE_MESSAGE 事件的代码块，
// 需要删除具有给定消息 ID 的消息
app.fire(app.EVENT.DELETE_MESSAGE, messageId);
});

app.listen(app.EVENT.MESSAGES_READY, function (messages) {
// TODO: 显示从事件中传递过来的消息
});

// 通知任何监听中的代码块，UI 已构建完成
// 并可供使用
app.fire(app.EVENT.UI_READY);

}(this); // 自我实例化，正如之前对 Data 所做的那样
}

// 继承观察者模式，采用其 listen 和 fire 事件
WebMail.prototype = new $.Observer();

// 创建 WebMail 应用作为单例，执行其中的代码
WebMail = new WebMail();
```

上面的代码创建了一个单例，代表了浏览器中一个 Web 邮件客户端的雏形。其数据专属代码和用户界面专属代码彼此互不了解。然而，它们都知道为自身使用而创建的预定义事件列表，以及每个事件需要传递的变量。随后，Web 邮件应用便由其内部存在的事件列表所定义。这些事件几乎无一例外地以系统内的高级操作命名，例如删除消息或发送邮件，这使得代码相当易读。

这两个代码块被称为*松散耦合*，因为它们都对彼此的数据执行操作，但并未以任何方式与对方的代码绑定或依赖。

任何一个代码块都可以从系统中移除，而不会在浏览器中引发任何错误。这对于 MVC 模式来说是做不到的，因为其控制器需要模型和视图始终可用才能无误运行。

你还可以在应用代码之外触发和监听事件。例如，页面中的任何代码都可以监听 `WebMail.EVENT.MESSAGE_DELETE` 事件，并添加自己的代码，以便在 Web 邮件应用中触发该事件时执行。因此，你可以使用观察者模式创建更小的模块，这些模块组合成更大的应用，而应用本身也可以使用观察者模式。这使得该模式极为强大、通用且易于维护。

### 测试与测试驱动开发

你应该在编写代码的同时对其进行测试，最好是逐行或逐个函数地进行测试。

不要整天只写代码而不测试。始终进行测试，并在尽可能多的不同浏览器上进行测试，这样你才能对自己的代码和编码技能充满信心。

*测试驱动开发*是一个深受程序员欢迎的概念。其总体原则是，当你准备编写一个新函数时，首先创建该函数的基本骨架，定义其输入和输出。然后立即为该函数编写一系列测试用例，预测一组输入对应的输出。测试用例编写完毕后，你再返回函数并编写能够通过这些测试用例的代码。这是一种合理的开发方法，因为它确保测试成为首要任务，并保证每个函数健壮且不易出错。



测试框架的存在是为了确保无论您向代码库中编写的函数提供何种合理输入集，都能获得合理的输出。通过了解函数在一系列预定条件下的工作方式，您可以确信在代码中使用该函数时，它会按预期工作。

Yahoo! 发布了一个非常有用的 JavaScript 测试框架，名为 YUI Test（[`developer.yahoo.com/yui/yuitest/`](http://developer.yahoo.com/yui/yuitest/)）。YUI Test 是 Yahoo! UI JavaScript 库的一部分，但如果您使用其他 JavaScript 库，它也能很好地工作。它允许您编写和运行有效的测试用例，并在错误发生时检测并记录它们。测试用例可以分组到更大的测试套件中，以提供一次运行多个测试的简单方法。在我使用过的其他测试框架中，这个框架的独特之处在于它支持测试异步方法调用，这使您能够测试 Ajax 回调方法。它还在大多数常用浏览器中提供了 DOM 事件模拟。

要使用 YUI Test，首先创建一个引用 YUI Test JavaScript 文件和您要测试的 JavaScript 代码的测试页面。然后，在页面的`<script>`标签内，多次调用您代码中的一个函数，并传入不同的输入。您告诉测试框架每个输入对应的预期输出，框架会记录函数调用的实际输出是否与您的预测匹配。任何与预测不匹配的输出都会被标记为错误。清单 2-23 展示了一个为与 YUI Test 框架一起运行而编写的简单测试套件。

■**注意** 编写测试用例时，请确保测试“边缘”情况——那些在典型使用中可能不会出现，但在特殊情况下可能发生的情况，例如当需要输入数据但数据缺失时。

**清单 2-23.** *使用 YUI Test 的基本测试套件 HTML 页面*

```html
<!DOCTYPE html PUBLIC "- //W3C//DTD XHTML 1.0 Strict//EN"
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
<title>Test suite example with YUI Test</title>
<!-- Include Yahoo! CSS and JS for the Test framework -->
<link rel="stylesheet" type="text/css" href="http://yui.yahooapis.com/2.6.0/build/logger/assets/skins/sam/logger.css" />
<link rel="stylesheet" type="text/css" href="http://yui.yahooapis.com/2.6.0/build/fonts/fonts-min.css" />
<link rel="stylesheet" type="text/css" href="http://yui.yahooapis.com/2.6.0/build/yuitest/assets/skins/sam/yuitest.css" />
<script type="text/javascript" src="http://yui.yahooapis.com/2.6.0/build/yahoo-dom-event/yahoo-dom-event.js"></script>
<script type="text/javascript" src="http://yui.yahooapis.com/2.6.0/build/logger/logger-min.js"></script>
<script type="text/javascript" src="http://yui.yahooapis.com/2.6.0/build/yuitest/yuitest-min.js"></script>
</head>
<body>
<h1>Test suite example with YUI Test</h1>
<div id="testLogger"><!-- The logging console will go here --></div>
<script type="text/javascript">
// This will be the function we will write our test cases for
var squareAndRound = function(number) {
return Math.round(number * number);
}
// Now, create the test case code
YAHOO.namespace("example.yuitest");
// Create a new test case
YAHOO.example.yuitest.MyTestCase = new YAHOO.tool.TestCase({
name: "My test case", // Give a name to label the test case with
// All test case functions must begin with the test prefix
testTen: function() {
var Assert = YAHOO.util.Assert; // Allows you to test the value
// Execute the function we're testing
var result = squareAndRound(10);
// Assert that you expect the result of the function to be 100.
// If it is, the test passes, if not an error is logged
Assert.areEqual(100, result);
}
// Insert a multitude of other test cases here to push the
// function to the limit
});
// Create a new test suite and add the test case to it
YAHOO.example.yuitest.ExampleSuite = new YAHOO.tool.TestSuite("Example Suite");
YAHOO.example.yuitest.ExampleSuite.add(YAHOO.example.yuitest.MyTestCase);
// When the DOM is ready...
YAHOO.util.Event.onDOMReady(function (){
// Create the logging console on the page
var logger = new YAHOO.tool.TestLogger("testLogger");
// Add the test suite to the test runner's list of tasks
YAHOO.tool.TestRunner.add(YAHOO.example.yuitest.ExampleSuite);
// Run the test runner, which executes the tests and outputs the
// results in the logging console
YAHOO.tool.TestRunner.run();
});
</script>
</body>
</html>
```

图 2-3 展示了一个日志控制台的示例，它指示了代码针对测试用例的运行情况。

**图 2-3.** *Yahoo! UI Test 框架日志控制台*

### 使用第三方脚本

重用已经编写好的代码来解决您作为 RIA 开发者面临的问题是非常明智的。您在 JavaScript 库中已经看到了这一点。同样，允许他人重用您编写的代码也是对 Web 开发社区的回馈。

以下是在应用程序中使用第三方脚本的一些指南：

- 检查代码是否已使用测试框架（例如我们在上一节中介绍的 YUI Test 框架）进行了彻底测试。
- 确保脚本支持您应用程序所支持的所有浏览器，这样您就不必在之后用其他脚本来替换它，以提供所需的所有支持。
- 将所有第三方脚本放在一个单独的文件夹中，与您自己的脚本分开。将它们视为您应用程序的插件，而不是核心代码本身。

在本书的其余部分，我将向您介绍几个第三方脚本，当您尝试为 RIA 添加额外功能时，它们应该会对您有所帮助。

### 总结

在本章中，您学习了如何使用面向对象编程原则和软件架构设计模式在 JavaScript 中构建 RIA。本章涵盖了如何应用清晰且可读的编码风格，以及如何以稳健的方式使用 JavaScript 语言。您已经看到第三方库和框架如何让您将代码构建在坚实的跨浏览器基础之上。您还学习了如何全面测试代码，以确保应用程序中不存在错误。



