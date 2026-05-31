# 2. 走向面向对象

## 摘要

在详细讨论 Backbone.js 之前，我们将讨论一些 JavaScript 基础知识，重点是如何在 JavaScript 中支持面向对象的概念。如果你已经熟悉 JavaScript，可以跳过本章。然而，如果你是创建 JavaScript 应用程序的新手，那么这将是充分利用本书的重要一步。如果你来自其他编程语言，你会对 JavaScript 所具备的能力感到惊讶。JavaScript 不再只是一种用于简单 Web 表单处理或爱好者使用的语言，它已成为软件开发生态系统中最流行的语言之一。

## JavaScript 的崛起

JavaScript 最初于 1995 年以 `LiveScript` 的名义出现，它一直是当今我们所知的 Web 发展的重要组成部分。通过允许客户端脚本，JavaScript 实现了更动态的网站和应用程序。

JavaScript 是一种解释型语言，这意味着它不需要像 Java 和 C# 那样在执行前经过编译过程。相反，JavaScript 代码由浏览器逐条语句执行。由于 JavaScript 是一种解释型语言，大多数人认为在 JavaScript 中创建真正的对象几乎没有什么结构可言。尽管它最初专注于在浏览器上为网站创建客户端脚本，但它现在更常用于创建大规模应用程序。这催生了大量框架，这些框架有助于创建“单页 Web 应用程序”——即可以与后端服务异步通信的应用程序，而无需为每个操作重新加载整个页面。

Backbone 是这类框架中的佼佼者之一。在我们了解 Backbone 应用程序如何工作之前，让我们先看看幕后，探究该语言的根基。

### JavaScript 版本

为了努力在浏览器间标准化 JavaScript，ECMAScript 于 1997 年发布。自那时起，它经历了一系列版本发布，最近的一次是 2011 年 6 月发布的 ECMAScript 5.1。不同浏览器对最新规范版本中功能的支持差异很大；要了解你的目标浏览器支持哪些功能，可以查看网上众多兼容性表格之一，例如 Juriy Zaytsev 在 [`http://kangax.github.io/es5-compat-table/`](http://kangax.github.io/es5-compat-table/) 上的概览。

此版本中引入的特性包括改进的对象创建、对象属性定义、原生 JSON 支持以及有用的数组查询方法。这些改进在该语言持续主导 Web 的过程中发挥了巨大作用。

未来的 ECMAScript 版本，称为 ES.next，将包含模块定义、用于检测对象状态变化的可观察对象，以及真正的 `Map` 和 `Set` 实现。

纵观该语言的路线图，毫无疑问 JavaScript 将长期存在。ECMAScript 的每一次迭代都在不断加强该语言，添加那些被证明有用但又不会使语言变得过于复杂的功能。

### 执行 JavaScript 片段

在本章中，你会遇到许多你可能想亲自尝试的代码片段。执行这些小段代码的最简单方法是使用 Chrome 开发者工具，它包含在你的标准 Chrome 发行版中。它提供的控制台允许你输入 JavaScript 并看到它立即执行。我们将在下一章更详细地介绍 Chrome 开发者工具。现在，你可以通过 Chrome 的 `工具/开发者工具` 菜单选项来访问这些工具。

要向控制台输出内容，只需使用 `console.log` 语句，它可以接受任何类型的输出。Chrome 控制台甚至会为你提供 JavaScript 的代码补全功能。如果你更喜欢使用 Firefox，Firebug 扩展程序可以提供类似的功能。

你还可以在网上找到许多允许你执行 JavaScript 并分享你创建的片段的在线服务，例如 `jsfiddle.net` 和 `codepen.io`。

## 对象到底是什么？

对象是你问题领域中某个事物的表示，它包含许多属性。对象的经典例子是“人”。在你的代码中，你会想引用人的不同属性，例如他们的姓名、年龄和性别。

除了这些属性，一个对象还包含方法，这样你就可以更改任何属性的状态。继续以我们的人为例，你可能有一个 `setAge` 方法，允许你更改人的年龄。

综合起来，对象是状态（属性）和行为（方法）的结合。在你的代码中使用对象的好处包括：

- **模块化**：每个对象独立于其他任何对象，因此你可以对你的对象进行修改而不会影响其他对象。
- **封装**：每个对象中存储的属性可以对其他对象隐藏，因此你只暴露基本信息，并隐藏内部状态。
- **复用**：如果已经存在一个对象来表示你的数据或完成特定任务，你可以简单地复用它，或者更好的是，扩展它的功能。

我们将在本章后面看到这里列出的每个实践如何应用到你的 JavaScript 代码中。但首先，我们将回到基础，简要介绍 JavaScript 的构建块。

## JavaScript 基础

JavaScript 并不是一种臃肿的语言，它为你提供了刚好够用的功能。但不要因此就误以为它是一种缺乏能力和特性的语言。让我们花些时间来了解 JavaScript 提供的各种数据类型和对象，并复习一下基本语法。

### 原始数据类型

JavaScript 包含五种原始数据类型：字符串（String）、数字（Number）、布尔值（Boolean）、未定义（Undefined）和空值（Null）。对于前三种类型，它们实际上是封装了真正原始类型的对象，因此它们提供了额外的方法来处理它们所代表的数据。

由于 JavaScript 是一种动态语言，一个变量在其生命周期内可以改变类型。等号左侧不需要像在静态类型语言中那样指定变量的类型，而是采用以下形式：

```
var <变量名> = <值>
```

现在让我们逐一看看这些数据类型。



#### 字符串

`String`是一个简单的字符序列。你可以使用单引号或双引号来创建你的`String`，前提是你以开始时使用的相同引号结束。以下是两个有效的`String`声明：

```
var doubleQuoteString = "A String in double quotes";
```

如果你想在`String`中包含引号，可以使用`\`进行转义，如下所示：

```
var escapedQuote = 'He said \'hello\'';
```

如前所述，`String`是简单类型的对象包装器，因此有许多可用于`String`操作的属性和方法（参见表 2-1）。

表 2-1. `String`函数列表

| 方法/属性 | 用途 |
| --- | --- |
| `.length` | 返回`String`中的字符数。 |
| `charAt(index)` | 返回指定索引处的字符。 |
| `charCodeAt(index)` | 返回指定索引处字符的 Unicode。 |
| `concat(String1, ..., StringN)` | 连接`String`并返回结果的副本。 |
| `fromCharCode()` | 将 Unicode 转换为字符。 |
| `indexOf(value)` | 返回`String`中首次出现`value`的位置。如果不存在则返回-1。 |
| `lastIndexOf(value)` | 返回`String`中最后一次出现`value`的位置。如果不存在则返回-1。 |
| `match(regExp)` | 在`String`内查找正则表达式的匹配项，将匹配项作为数组返回。 |
| `replace(regExp, value)` | 在`String`内查找正则表达式的匹配项，并将生成的子字符串替换为新值。 |
| `search(regExp)` | 在`String`内查找正则表达式的匹配项，并返回匹配项的位置。 |
| `slice(startPos, endPos)` | 提取指定开始和结束位置之间的`String`部分。 |
| `split(token)` | 使用指定的`token`作为分隔符，将`String`拆分为子字符串数组。 |
| `substr(startPos, numChars)` | 从`String`中提取子字符串，从开始位置开始，包含指定数量的字符。 |
| `subString(startPos, endPos)` | 从`String`中提取指定开始位置和结束位置之间的子字符串。 |
| `toLowerCase()` | 返回所有小写字符的`String`。 |
| `toUpperCase()` | 返回所有大写字符的`String`。 |
| `trim()` | 返回删除两端空白字符后的`String`。 |
| `valueOf()` | 返回`String`的原始值。 |

`String`对象是不可变的，这意味着所有这些方法都不会改变原始`String`，而是返回一个新的`String`。

#### 数值

与大多数其他编程语言不同，JavaScript 对数字使用浮点表示，仅此而已！根据你想如何表示数字，你可以创建整数、双精度数，并为更大的数字添加科学记数法。

```
var count      = 10;    //没有小数位的数字
var cost       = 2.99;  //有两位小数的数字
var pi         = 123e5; //12300000
```

当未定义小数位时，`Number`的范围是从-2⁵³ 到 2⁵³。对于浮点数，范围从-2e31 到 2e31。

数字有两个特殊值：当发生数学溢出异常时为`Infinity`，以及当数字小于支持的最小值时`-Infinity`。

#### 布尔值

`Boolean`是用于条件语句测试的简单值，可以是两个值之一：`true`或`false`。值得注意的是，`true`被视为`String`而不是`Boolean`。

```
var lightsOn = true;
```

#### undefined 和 null

很多人对`undefined`和`null`之间的区别感到困惑，主要是因为他们在其他编程语言中的经验。实际上，理解其工作原理相当简单。`Undefined`是分配给任何变量的默认值。因此，如果我如下创建一个变量并打印其值，它将是`undefined`。

```
var initialVariable;
console.log(initialVariable);
```

`Null`必须由程序员赋值以清空变量，它不是默认值。

```
var initialVariable = 'hello';
initialVariable = null;
```

这两种类型在 JavaScript 世界中被视为原始数据类型。

## JavaScript 中的核心对象

JavaScript 语言还为你提供了一些更有用的对象，以帮助处理日期、简单的数学运算、正则表达式和数组。

### 日期

虽然存在许多库可以帮助你处理日期和时间，但 JavaScript 的`Date`对象本身相当强大。`Date`对象可以使用四个构造函数中的任何一个来构造，最有用的是不带参数的形式，它返回当前日期和时间。你也可以通过提供毫秒值来构建特定时间的`Date`对象。

```
var now = new Date();
```

你可以使用完整的`getX()`方法列表提取日期的各个部分，例如`getMinute()`、`getDay()`和`getMonth()`。

当然，还有更优雅的方法来获取日期表示，例如使用实用方法返回可读的日期：

```
var calendarString  = now.toDateString();
var time            = now.toTimeString();
```

### 数学

JavaScript 有一个内置的`Math`对象，允许你执行简单的数学运算。该对象包含许多有用的常量，例如`Math.PI`、`Math.E`和`Math.LOG2E`。

尽管它是一个对象，但你永远不需要创建`Math`的新实例来使用它。可以将其视为一个包含许多便捷方法的工具对象。

例如，要将数字向上舍入到最接近的整数，请使用以下代码：

```
var doubleValue = 2.1;
var intValue = Math.ceil(doubleValue); //intValue 现在为 3
```

表 2-2 列举了一些你可能觉得有用的可用方法。

表 2-2. `Math`函数列表

| 方法 | 用途 |
| --- | --- |
| `abs(x)` | 返回`x`的绝对值。 |
| `ceil(x)` | 返回`x`向上舍入到最接近的整数后的值。`ceil(2.1)`的结果为 3。 |
| `floor(x)` | 返回`x`向下舍入到最接近的整数后的值。`floor(2.1)`的结果为 2。 |
| `log(x)` | 返回`x`的自然对数。 |
| `pow(x,y)` | 返回`x`的`y`次幂的结果。 |
| `random()` | 返回一个介于 0 和 1 之间的随机数。 |
| `round(x)` | 将`x`四舍五入到最接近的整数。`round(2.1)`的结果为 2。 |
| `sqrt(x)` | 返回`x`的平方根。 |

请注意，上表并非完整列表。`Math`对象还允许你对数字执行正弦、余弦和正切计算。



### RegExp

正则表达式在需要对字符串执行模式匹配操作时非常有用。创建新表达式时，你将模式和若干标志作为参数传入。`RegExp`的创建有两种形式。

```
var pattern = new RegExp('javascript','i');
```

或者

```
var pattern = /javascript/i;
```

这两种形式都会搜索单词"javascript"，并因`i`修饰符而忽略大小写。你会记得`String`对象有一个`match`方法，它接受一个正则表达式作为参数，所以让我们在一个简短的代码示例中把两者结合起来。

```
var myString = 'I love Javascript';
var pattern = new RegExp('javascript', 'i');
var match = myString.match(pattern);
```

在前一个例子中，`match`的结果是一个包含一个字符串的数组：`'Javascript'`。

注意

正则表达式的内容远不止于此；你可以在 Mozilla 开发者网络上找到关于此对象的完整解释：[`https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/RegExp`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/RegExp)。

## 基本语法

你需要了解每种语言的一些基础知识。JavaScript 相当简单，规则很少，所以这部分不会花很长时间。

### 声明变量

正如你已经看到的，变量可以简单地使用`var`关键字声明。

```
var greeting = 'Hello';
```

如果你使用的是对象而非原始类型，你可能需要使用`new`关键字来构造对象。

```
var now = new Date();
```

变量的作用域（或可用性）取决于它在何处声明。在任何函数外部声明的变量是全局的，而在函数块内部声明的变量仅在该块内可见。

### 循环与条件语句

编写 JavaScript 程序时，不可避免地需要在某个阶段使用条件语句。在本节中，我们将介绍`if`/`else`条件语句，以及`for`和`while`循环。

JavaScript 中的`if`条件采用以下形式：

```
if (<condition>){
   <action>
}
else{
  <alternate action>
}
```

在讨论条件时，注意`==`和`===`之间的区别很重要。双等号只检查值是否相等，而`===`提供了更高的精度，确保值和类型都相等。在大多数情况下，你都会希望使用这种精确相等操作。

```
if (userInput === 'run'){
   console.log('running application');
}
```

你会在一些程序中看到上述示例的简短形式，条件及两种结果都在一行内表达。

```
var outputString = (price < 10 ) ? 'Reasonable ' : 'Too Expensive';
```

你可能还需要使用`for`或`while`循环来遍历数组或执行重复操作。同样，语法很简单。`for`循环类似于 Java。

```
for(var i = 99; i >= 0; i--){
   console.log(i + 'bottles of beer on the wall.' + i + ' bottles of beer');
}
```

`while`循环仅包含一个条件测试，将变量初始化和变量状态的改变放在循环结构之外。

```
while(<condition>){
   <actions>
}
```

还有一种`while`循环的变体可用，即在测试条件之前先执行一段代码。

```
do{
<actions>
}
while(<condition>)
```

### 数组

数组是任何编程语言的基础部分，提供了创建列表和集合的能力。JavaScript 提供了有效处理数组所需的一切。

创建新数组有两种方式；第二种更受欢迎，如下所示：

```
var myArray = [];
var myArray = new Array();
```

然后可以将项目添加到数组的特定索引中。数组从零开始索引，这意味着位置 0 是数组中的第一个元素。

```
myArray[0] = 'first item';
```

项目也可以推入数组，占据下一个可用位置。

```
myArray.push('first item');
```

如果你已经知道数组中需要哪些项目，可以使用更简洁的语法来创建。

```
var myArray = new Array('first item', 'second item', 'third item');
```

表 2-3 列举了数组中所有可能的操作。

**表 2-3. JavaScript 数组函数列表**

| 方法/属性 | 用途 |
| --- | --- |
| `.length` | 返回数组中的项目数，也可用于设置数组的大小。 |
| `indexOf()` | 在数组中查找元素并返回其位置。如果找不到元素则返回-1。 |
| `join()` | 连接数组的所有元素以创建一个字符串。 |
| `lastIndexOf()` | 查找数组中元素的最后出现位置并返回其位置，如果找不到元素则返回-1。 |
| `pop()` | 移除数组中的最后一个元素并返回该元素。 |
| `push()` | 将元素添加到数组末尾并返回数组的新长度。 |
| `reverse()` | 反转数组中元素的顺序。 |
| `shift()` | 移除数组中的第一个元素并返回该元素。 |
| `slice()` | 选取数组的一部分并将其作为一个新数组返回。 |
| `sort()` | 对数组元素进行排序。可以提供一个排序函数作为参数以强制执行特定的排序策略。 |
| `splice()` | 从数组中添加或移除元素。该方法接受两个元素列表：要移除的元素和要添加的元素。 |
| `toString()` | 返回数组的字符串表示形式。 |
| `unshift()` | 将元素添加到数组的开头并返回新长度。 |
| `valueOf()` | 返回数组的原始值。 |

注意

一个数组可以包含混合的对象类型。你也可以通过将数组放入数组中来创建 n 维数组。

### 闭包

对于某些人来说，闭包可能是一个稍微令人困惑的话题，但很可能你会在编写 JavaScript 代码的过程中不知不觉地使用它们。jQuery 大量使用了闭包。

闭包是一个内部函数，它可以访问外部函数的变量。正如我们本章前面提到的，变量具有全局作用域或函数作用域。闭包位于函数内部，因此既可以访问这两个作用域，也可以访问其自身的内部作用域。此外，闭包还可以访问外部函数的参数。

让我们通过一个简单的代码示例来研究这一点，其中两个闭包提供了计算圆形或矩形面积的功能。

```
function calculateArea(height, width, shape){
   var pi  = Math.PI;
   //第一个闭包
   function getCircleArea(){
      var circleArea = pi * (height*height);
      return circleArea;
   }
   //第二个闭包
   function getRectangleArea(){
      var area = width * height;
      return area;
   }
   if(shape === 'Circle'){
      return getCircleArea();
   }
   else{
      return getRectangleArea();
   }
}
```

因此，只要你在一个函数内部定义另一个函数，就是在创建一个闭包。这可以被认为是一个只包含一个方法的对象，并且该方法是私有的，因为它无法从该函数外部执行。

## 让 JavaScript 面向对象

如果你使用过传统的面向对象编程语言，你会熟悉诸如构造函数、属性和方法等术语，以及封装、继承和多态等附加概念。以下部分将介绍这些概念如何在 JavaScript 中应用。



### 在 JavaScript 中创建简单对象

在 JavaScript 中创建对象非常简单。只需创建一个新的 `Object` 实例，然后根据需要添加属性即可。

```
var myObject = new Object();
myObject.id = 100;
```

另一种常见的创建对象方式是采用对象字面量模式，即在创建时将所有属性作为参数列在构造器内。

```
var myObject = {id: 100};
```

前面的两个例子都会生成相同的对象，你可以通过以下方式访问 `id` 属性：

```
var objectId = myObject.id;
```

### 定义构造器

上一节只是浅尝辄止地介绍了如何创建对象。通常情况下，你会希望使用构造器来创建对象。构造器允许你传入一些初始变量并设置对象的初始状态。

在其他语言中，这是通过类来实现的，但 JavaScript 没有类；它只有函数。这正是该语言施展其能力的地方。

假设你想创建一个表示消息的对象。你需要定义主题、收件人和消息内容。使用前面任何一种方法都会显得冗长且容易出错。为了创建我们的表示，我们为 `Message` 创建一个新函数，并列出参数，如下所示：

```
function Message(subject, recipient, content){
    this.subject = subject;
    this.recipient = recipient;
    this.content = content;
}
```

现在，如果我们想创建这个 `Message` 对象的新实例，我们可以调用构造器函数，同时使用 `new` 关键字。在调用旨在充当构造器的函数时，使用 `new` 是必不可少的。

```
var myEmail = new Message('Javascript is cool', 'you@gmail.com', 'Creating objects is simple');
```

这让我们更进一步：我们能够像以前一样访问属性，但我们现在有了对象及其属性的定义。

### 创建方法

我们可以对前面的例子进行扩展，为对象添加方法。方法允许我们访问对象内部的变量、改变对象的状态，或对对象执行操作。通过在“构造器”函数内部创建一个函数来添加方法。

```
function Message(subject, recipient, content){
    this.subject = subject;
    this.recipient = recipient;
    this.content = content;

    //暴露方法
    this.showMessage = showMessage;

    function showMessage(){
        console.log('To: ' + recipient + ' Subject: ' + subject + ' Message: ' + content);
    }
}
```

现在我们可以轻松调用这个方法，它会将消息结构打印到控制台。

```
var myEmail = new Message('Javascript is cool', 'you@gmail.com', 'Creating objects is simple');
myEmail.showMessage();
```

请注意，在添加函数之前，我们有一行代码将 `showMessage` 方法暴露为这个对象的一部分。没有这行代码，执行上述代码将导致以下错误：

```
TypeError: Object #<Message> has no method 'showMessage'
```

不过，你可能希望这样做来保持方法的私有性，使其仅在对象内部使用，而不对对象的调用者开放。

### JavaScript 原型

你经常会听到 JavaScript 是基于原型的，而不是基于类的。这仅仅意味着，虽然不存在类，但你仍然可以通过克隆作为原型的对象来实现代码复用和继承。

每一个 JavaScript 函数都有一个 `prototype` 属性，默认情况下它没有被赋予任何内容。当属性和方法被附加到这个属性上时，它们将对该函数的所有实例可用。让我们看看如何利用这一点，通过 `prototype` 属性为我们之前创建的 `Message` 对象添加 `show` 方法。

```
function Message(subject, recipient, content){
    this.subject = subject;
    this.recipient = recipient;
    this.content = content;
}

Message.prototype.show = function(){
    console.log('To: ' + this.recipient + ' Subject: ' + this.subject + ' Message: ' + this.content);
};

var myEmail = new Message('Javascript is cool', 'you@gmail.com', 'Prototype is useful');
myEmail.show();
```

可以使用 `prototype` 属性添加任意数量的函数和属性。如果你检查 `Message.prototype` 的内容，你会看到 `Message` 有一个构造器、一个 `show` 方法，以及 `__proto__` 属性（目前可以忽略它）。

```
> Message.prototype
Message {show: function}
    constructor: function Message(subject, recipient, content){
    show: function (){
    __proto__: Object
```

值得注意的是我们刚刚讨论过的 `prototype` 属性与 `prototype` 属性之间的区别。后者适用于已创建的对象实例，其值会指定对象的父类或类型。

在我们之前的例子中，`myEmail` 的 `prototype` 属性的值是 `Message.prototype`。任何由构造器函数创建的对象都将继承该构造器。但是，当使用对象字面量模式创建对象时，你会注意到其原型将是 `Object.prototype`。

### 封装

当你构建一个对象模型时，会有一些通用功能需要暴露给所创建的特定对象的每个实例。例如，你期望能够发送和展示任何消息。所以，让我们基于最初的 `Message` 示例进行构建，来说明如何通过组合使用构造器/原型模式来实现这一点。

为方便起见，以下是 `Message` 对象的构造器：

```
function Message(subject, recipient, content){
    this.subject = subject;
    this.recipient = recipient;
    this.content = content;
}
```

现在，我们不必单独定义原型的每个部分，而是可以使用以下简写方式将构造器和多个方法赋给 `Message` 对象：

```
Message.prototype = {
    constructor: Message,
    sendMessage: function(){
        console.log('Sending message to ' + this.recipient);
    },
    show: function(){
        console.log('To: ' + this.recipient + ' Subject: ' + this.subject + ' Message: ' + this.content);
    }
};
```

`Message` 对象的完整定义现在集中在一个地方，并且相同的方法对我们创建的所有 `Message` 实例都是可用的。每个创建的消息都可以拥有自己的实例属性，这些属性通过构造器传入。

```
var workMessage = new Message('Work complete', 'boss@mycorp.com', 'My work is done here');
var socialMessage = new Message('Time to go out', 'friend@gmail.com', 'Finished work now.');
workMessage.send();
socialMessage.send();
```



### 使用原型实现继承

继承是面向对象代码库的重要优势之一，它允许你在一个对象中提供基础行为，供其他对象使用和扩展。JavaScript 通过 `prototype` 属性提供了继承机制。

为了简单说明这种继承机制的工作原理，我们先看看如何创建一个 `Animal` 的基础定义，并为其添加 `talk` 方法：

```
function Animal(name){
    this.name = name;
}

Animal.prototype.talk = function(){
    console.log(this.phrase);
}
```

现在，我们创建一个 `Dog` 对象，该对象带有 `phrase` 属性。

```
function Dog(phrase){
    this.phrase = phrase;
}
```

接下来，我们将 `Dog` 的原型设置为 `Animal`。这样它就能继承 `Animal` 对象提供的所有属性和方法。

```
Dog.prototype = new Animal();
```

创建一个 `Dog` 对象的新实例，并调用由 `Animal` 对象提供的 `talk` 方法，控制台将输出 `bark`。

```
var myDog = new Dog('bark');
myDog.talk();
```

我们也可以通过添加下面这条语句，确保 `Dog` 的实例使用正确的构造函数，达到与之前相同的效果：

```
Dog.prototype.constructor = Dog;
```

使用继承时，你通常还需要能够调用父类构造函数。这可以通过始终指向父类的 `constructor` 属性来实现。这样就能确保 `name` 属性被正确设置。

```
function Dog(phrase){
    this.phrase = phrase;
    this.constructor('Dog');
}
```

### 重写方法

当子类需要重写父类方法时，也需要应用类似的模式。这通常发生在你需要在继承的基础上提供额外功能时。借助任意 `Function` 对象自带的 `.call` 方法，在 JavaScript 中实现这一点非常直接。下面我们通过扩展 `talk` 方法的功能来演示。

```
Dog.prototype.talk = function(){
    console.log('The dog says');
    Animal.prototype.talk.call(this);
}
```

调用父类方法时，你可以传入另一个对象作为其上下文（context），这里我们将 `Dog` 对象作为上下文。你还可以在 `this` 声明之后，传递该方法所需的任何参数。

在上面的父类方法调用中，我们需要知道父类是 `Animal.prototype`。通过记录父类对象，我们可以让代码更优雅。

```
Dog.prototype = new Animal();
Dog.prototype.constructor = Dog;

//定义父类属性
Dog.prototype.parent = Animal.prototype;

Dog.prototype.talk = function(){
    console.log('The dog says');
    //使用父类属性调用方法
    this.parent.talk.call(this);
}
```

### 原型链

当像这样的“子类”调用任何方法或属性时，JavaScript 会沿着原型链向上查找，直到找到对应的值。如果当前对象没有定义该方法，则会检查其父对象，以此类推，直到到达所有对象的根：`Object`。

运行以下代码将执行 `Object.toString()` 方法，因为原型链中没有其他定义，但 `Object` 中存在该定义。

```
myDog.toString();
```

### 寄生组合式继承模式

到目前为止，我们采用的继承方式相当直接。不过，我们可以在 JavaScript 代码库中创建一个 `inheritPrototype` 函数，以提供更优雅的继承方式。在讨论这个函数之前，我们首先需要了解 JavaScript 已有的 `Object.create()` 方法。

#### Object.create

`Object.create` 最初由 Douglas Crockford 编写，后来成为了 JavaScript 语言的一部分。该方法允许你根据作为参数传入的原型创建一个新对象，同时还可以传入可选的属性描述符。

创建一个简单的 `Object` 来继承 `Animal` 对象的所有定义，非常简单：

```
var dog = Object.create(Animal.prototype);
```

之后可以给 `dog` 对象添加额外的属性。或者，你也可以使用属性描述符参数。属性描述符包含属性的名称、其值的定义，以及该属性是否可以被修改。

```
var dog = Object.create(Animal.prototype, {
    'legs': {value: 4, writable: true},
    'barks': {value: true, writeable: false}
});
```

总之，使用 `Object.create` 实质上是传入你想要继承的对象。请注意，`Object.create` 是在 ECMAScript 5.0/JavaScript 1.8.5 中引入的。

#### inheritPrototype

`inheritPrototype` 函数的核心思想是，传入父对象和子对象，该函数将让子对象继承父对象的所有内容。

```
function inheritPrototype(childObject, parentObject){
    var parentCopy = Object.create(parentObject.prototype);
    parentCopy.constructor = childObject;
    childObject.prototype = parentCopy;
}
```

上面的代码看起来可能有些复杂，但逐行阅读还是很容易理解的。

首先，创建父对象的一个副本，该副本包含了 `parentObject` 的所有内容。这个副本在我们复制过程中用作临时对象；最终，这个 `parentCopy` 实际上会作为子对象的原型使用。

第二行明确表示，这个临时对象的构造函数将是子对象的构造函数。最后，将 `childObject` 的原型设置为 `parentCopy`。至此，`childObject` 就继承了 `parentObject` 的所有内容。

现在，我们可以创建一个 `Dog` 对象，首先定义其构造函数，然后使用我们的 `inheritPrototype` 函数。

```
//定义 Dog 构造函数
function Dog(phrase){
    this.phrase = phrase;
}

//设置 Dog 对象继承自 Animal
inheritPrototype(Dog, Animal);
```

在此之后，可以给 `Dog` 对象添加新的属性和方法。如果在使用 `inheritPrototype` 函数之前定义了这些内容，由于复制过程的存在，这些额外的定义将会被覆盖。

### 控制方法和属性的访问权限

使用原型时，所有添加的属性和方法对任何调用者来说都是可访问的。但是，有时你可能希望隐藏某些属性，只在对象内部使用特定的方法，而不是对外暴露。

私有成员可以通过不将其包含在原型上或不使用 `this` 暴露它们来创建。

以 `Message` 对象为例。假设我们希望发送安全消息，并且拥有一个用于发送消息的私钥。

```
function Message(subject, recipient, content){
    //私有属性
    var privateKey = '11111';
    
    //私有方法
    function encryptMessage(content){
        return content || privateKey;
    }
    
    this.subject = subject;
    this.recipient = recipient;
    this.content = content;
    
    //暴露方法
    this.showMessage = showMessage;
    function showMessage(){
        console.log('To:' + recipient + 'Subject: ' + subject + 'Message:' + content);
    }
    
    this.sendMessage = sendMessage;
    //使用私有方法的公有方法
    function sendMessage(){
        console.log(encryptMessage(this.content));
    }
}
```

虽然这个例子说明了要点，但请记住 JavaScript 是一种解释型语言，任何人都可以通过查看源代码看到这个密匙。在实际应用中，编译后的服务器端代码才会处理这类机密数据。



### 提供命名空间

在创建任何稍具规模的代码库时，命名空间对于避免对象名称冲突以及使代码导航更加便捷都至关重要。

`Message` 对象就是一个很容易发生对象冲突的例子。`Message` 并未明确地与电子邮件、短信或推特相关联。因此，加入一个文档欠佳项目的开发人员可能会为了不同目的创建自己的 `Message` 定义。

为了实际演示这一点，让我们为 `Message` 创建一个命名空间，其形式为 `com.apress.chapterone`。命名空间的每个部分都必须按顺序定义。

```
var com = com || {};
```

```
com.apress = com.apress || {};
```

```
com.apress.chapterone = com.apress.chapterone || {};
```

`Message` 的定义会随之改变，以利用命名空间的各个部分。

```
com.apress.chapterone.Message = function Message(
```

现在，使用 `Message` 对象时需要遵循如下命名空间：

```
var myMessage = new com.apress.chapterone.Message(
```

这可能是 JavaScript 现有的最简单的命名空间管理方法，但它也是最有效且最容易理解的方法。

## JavaScript 面向对象框架

虽然前面的内容让你领略了 JavaScript 面向对象编程的复杂性，但包括 Backbone 在内的许多库都提供了实用工具和模式，使程序员能更轻松地在 JavaScript 中实现面向对象概念。在此，我们探讨一下主要的几个框架，不过，在大多数情况下，Backbone 应该就够用了。

### PrototypeJS

Prototype.js ([`http://prototypejs.org/`](http://prototypejs.org/)) 目前版本为 1.7.1，它提供了更自然的方式来定义类和处理继承。不要将此与原型语言特性混淆，该库还提供了许多其他有用的功能，比如编程快捷方式和 Ajax 抽象，以确保跨浏览器兼容性。

要在代码中使用 PrototypeJS，只需下载该库，并在你自己的源代码之前用 script 标签将其引入即可。

创建类非常简单，通过 `Class.create` 方法传入属性即可。

```
var Animal = Class.create({
    talk: function(phrase){
        console.log(phrase);
    },
    move : function(){
        console.log('Animal moving');
    }
});
```

PrototypeJS 的真正强大之处在于其实现子类化的方式。`create` 方法允许你将想要继承的类作为第一个参数指定。此外，每个函数都可以通过使用 `$super` 参数进行细化，该参数指向父类中该函数的实现。

```
var Dog = Class.create(Animal,{
    talk: function($super, message){
        console.log('Dog says');
        $super();
    }
});
```

仅这些小小的补充就为 JavaScript 中的继承提供了一种更简洁的方法。

### MooTools

MooTools ([`www.mootools.net`](http://www.mootools.net/)) 是另一个紧凑型 JavaScript 框架，它提供了一些面向对象机制以及类似 Prototype.js 的 DOM 和 Ajax 相关功能。一些开发者可能会觉得 MooTools 更熟悉，因为它引入了类似于 Java 编程语言的 `Extends` 和 `Implements` 概念。

创建类的方式也更接近 Java，因为 MooTools 使用 `new` 关键字来创建。

```
var Animal = new Class({
    initialize: function(name){
        this.name = name;
    },
    talk : function(phrase){
        console.log(phrase);
    }
});
```

`initialize` 是一个特殊函数，充当类的构造函数，因此创建新的 `Animal` 可以使用以下方式实现：

```
var animal = new Animal('Cow');
```

扩展基类并调用父类构造函数也非常简单。

```
var Dog = new Class({
    //定义继承关系
    Extends: Animal,
    initialize: function(name, phrase){
        //调用父类构造函数
        this.parent(name);
    }
});
```

定义 implements 关系遵循类似的模式，其中 `initialize` 函数可以省略。

### Backbone.js

幸运的是，Backbone.js 提供了足够的面向对象概念，使你无需再使用 Prototype.js 或 MooTools 等任何额外库。

在本书后续章节中，我们将看到如何轻松地扩展任何对象，以及如何在创建新的对象实例时利用 `initialize` 函数充当构造函数。

