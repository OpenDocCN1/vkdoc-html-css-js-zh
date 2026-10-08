## 第 2 章：面向富互联网应用的 JavaScript

在上一章中，我解释了如何为你的网站代码打下坚实的基础，包括让你的页面在除 HTML 之外无需任何前端技术的情况下也能完全可访问和可操作。我还说明了构成你网站的其他组成部分——样式表、图片、插件等——是构建在 HTML 基础之上的层次，用于为页面应用视觉布局和设计，并为网站访问者提供更自然的体验。

本章的重点是 JavaScript，我们在页面中使用它来简化最终用户的任务、对他们的操作做出反应，并努力让他们在网上的时光更顺畅、更友好。在本章中，你将基于已有的 JavaScript 知识，学习如何以可扩展、灵活且可维护的方式组织代码；运用面向对象编程原则；以及克服跨浏览器实现差异。接着，你将学习构建 RIA 的技术，最后，还将了解如何为代码编写自动化测试，使其更加健壮。

这将是一章理论性较强的章节，但其中充满了有用且重要的信息，所以让我们开始吧！

### 编码风格指南

作为 Web 开发者，你可以通过以一致的方式组织和构建你的 JavaScript 代码，编写代码来解决你的问题或达成目标，而不使代码过度复杂化，从而简化日常编码体验。遵循一些指导方针，可以使你的代码更干净、更清晰、更易于管理、更易于理解、文档更完善，并且更易于阅读。

#### 使用一致的格式

正如你在上一章中看到的，可读代码的标志——无论是 HTML、CSS 还是 JavaScript——都是统一的格式。例如，JavaScript 函数体内的内容应该相对于函数名和花括号缩进一个制表符空间。每条语句应以分号结束，并且每条语句应独占一行，以确保良好的可读性，如下例所示：

```javascript
```

function myFunction() {
    alert("Hello, world");
    return true;
}

```

类似地，逻辑代码块和循环体内的内容也应该缩进。一条经验法则是，一对花括号（`{` 和 `}`）内的内容应相对于这些花括号缩进，如下例所示：

```javascript
if (x == y) {
    alert("We have a match!");
} else {
    alert("We have a problem!");
}
```

然而，在能使代码更易于阅读的地方，你可能希望使用额外的缩进，如下面代码中的 `case` 语句：

```javascript
switch (prompt("What is your favorite fruit?", "banana")) {
    case "banana":
        alert("Well done. Bananas are full of potassium.");
        break;
    case "mango":
        alert("Exotic and tasty!");
        break;
    default:
        alert("That's not to my taste, but keep it up to get your 5-a-day.");
        break;
}
```

### 使用花括号和圆括号

花括号（`{}`）和圆括号（`()`）在 JavaScript 中广泛用于划分代码块，并减少长语句中的混乱。应尽可能使用它们，以最大限度地减少潜在缺陷并提高可读性，这两者都将有助于未来的代码维护。

下面的例子展示了一段移除花括号和圆括号以缩短代码的 JavaScript 例程。

```javascript
var result = 0;
if (confirm("Would you like to do some math?"))
    result = 10 / 2 * 100 % 30;
alert("OK, the result is: " + result);
```

这在语言语法上是完全有效的，但它使得逻辑稍微难以理解。

下面的形式则不那么令人困惑，因此是一种更稳定、更易读、更易于维护的代码例程。

```javascript
var result = 0;
if (confirm("Would you like to do some math?")) {
    result = ((10 / 2) * 100) % 30;
}
alert("OK, the result is: " + result);
```

注意，使用这种格式，你可以确定 `alert` 语句何时执行。并且你可以看到，当使用圆括号将操作分组时，数学计算例程变得多么易于理解。

### 用大小写增加含义

几条关于变量和函数名大小写的简单规则，能让你一眼就看出它们在代码中的用途。以下是几条你应该在自己的代码中遵循的常见大小写准则：

-   大多数变量和函数名以小写字母开头。
-   对象构造函数名以大写字母开头。（本章后面的“对象、类和构造函数”部分会讨论对象构造函数。）
-   使用`驼峰命名法`，其中每个新单词以大写字母开头，所有其他字符为小写，以标记新单词的开始。
-   使用全大写字符表示常量——即在代码生命周期内不应改变的值。

下面的代码展示了每种大小写类型的例子：

```javascript
// 对象构造函数 – 含义详见本章后面
var Calendar = function() {
    // 常量使用大写
    var DAYS_IN_ONE_WEEK = 7;
    // 变量或函数名以小写字符开头
    var count;
    function add() {
    }
    // 变量和函数名中的新单词使用驼峰命名
    var currentDate;
    function doSomethingWithCurrentDate() {
    }
}
```

### 使用描述性变量和函数名

为变量和函数选择恰当的名称是构建易于理解的代码库的基础之一。命名变量和函数时，要让它们体现其用途和目的。如果较长的名称能传达信息，不要害怕使用它们。

想象一下，你只看到代码中的变量名和函数名，而没有任何逻辑或值，你能一眼看出它们的用途吗？如果不能，那就是时候开始重命名了。

以下是一些描述性变量和函数名的例子：

```javascript
var currentDate;
function setCurrentDate() {
}
var userName;
function updateUserName() {
}
```

你还应避免使用所谓的`魔术数字`，即计算中用到的、看似凭空出现的数字。以下例子展示了魔术数字的使用：

`var daysInAYear = 52 * 7;` // 稍加思考，你就能算出来
`var seatsOnThePlane = 25 * 6;` // 但这又意味着什么呢？

这些数字从何而来，它们代表什么含义？你应该力求消除代码中所有的疑惑。将含义明确表达出来，这样开发者就不需要逐行审视代码才能理解。下面的版本为前一个示例中使用的数字常量添加了定义。

```
var WEEKS_IN_A_YEAR = 52;
var DAYS_IN_A_WEEK = 7;
var daysInAYear = WEEKS_IN_A_YEAR * DAYS_IN_A_WEEK; // 忽略闰年
var SEAT_ROWS_ON_PLANE = 25;
var SEATS_PER_ROW = 6;
var seatsOnThePlane = SEAT_ROWS_ON_PLANE * SEATS_PER_ROW; // 啊，清晰多了！
```

注意到一切突然变得多么清晰了吗？

#### 保持函数块简短

一个函数应该只做一件事，并且把它做好。尽量避免将不相关的代码合并到一个函数中，如下例所示：

```
function feedbackOnUsersFavoriteFruit() {
    var favoriteFruit = prompt("What is your favorite fruit?", "None");
    var score = 0;
    switch (favoriteFruit.toLowerCase()) {
        case "banana":
            score = 6;
            break;
        case "apple":
            score = 4;
            break;
        default:
            break;
    }
    if (score > 5) {
        alert("You picked one of my favorites too!");
    } else if (score > 0) {
        alert("Credit for choosing a fruit, at least!");
    } else {
        alert("Not sure about your choice of fruit!");
    }
}
```

每个函数越简单，测试和调试就越容易。如果你的函数需要连续执行多个逻辑上不同的操作，请将一个大函数拆分成多个小函数，分别执行每个任务，如下例所示：

```
function getFavoriteFruit() {
    return prompt("What is your favorite fruit?", "None");
}
function getFruitScore(fruit) {
    var score = 0;
    switch (favoriteFruit.toLowerCase()) {
        case "banana":
            score = 6;
            break;
        case "apple":
            score = 4;
            break;
        default:
            break;
    }
    return score;
}
function getMessageByScore(score) {
    var message = "";
    if (score > 5) {
        message = "You picked one of my favorites too!";
    } else if (score > 0) {
```

# 专业级 JavaScript 编程

现在我们已经回顾了 JavaScript 文件中关于格式、文档和清晰度的一些基本准则，接下来让我们了解一些 JavaScript 编码原则。专业人士遵循这些实践，使他们的 JavaScript 代码健壮、可扩展，并建立在对要解决问题的扎实理解之上。

## 避免解决不存在的问题

编写 JavaScript 代码时，只编写解决当前问题所需的代码。例如，假设你想编写一段 JavaScript 代码，允许网站用户登录个人资料页面，并在用户提供有效凭证时动态刷新页面部分内容。要解决这个问题，你不需要编写能在网站任何页面刷新任何内容的代码——至少在当前尚未成为需求或成为新问题解决方案之前不需要。遵循这一准则，你的代码可以用更短的时间完成，因为你只构建完成任务所需的确切代码，而不是试图构建通常在实践中可靠性较低的通用、过度复杂的代码。之后，当你需要解决不同的问题，并认为已有代码可以改写来完成任务时，那才是重写它的时机。

## 使用文档对象模型

作为 JavaScript 语言的扩展，W3C 推荐了文档对象模型（DOM），该模型标准化了 Web 开发者与渲染页面中 HTML 元素交互的方式。该语言中增加了通过元素的 `id` 属性或标签名称定位页面元素的方法，支持动态操作元素的属性、CSS 样式属性，甚至移除或添加整个元素。DOM 推荐标准将 HTML 页面中的所有内容定义为通用名称 `node`。页面中可以存在五种类型的节点，如下所示，它们都是页面组件的 JavaScript 表示形式：

- **文档节点**：代表 HTML 页面的最顶层节点，实际上对应于 `<html>` 元素。

# 节点类型与 DOM 操作

## 节点类型

- **元素节点**（`Element node`）：页面上的每个 HTML 标签都表示为一个元素节点。
- **属性节点**（`Attribute node`）：标签的每个属性，例如 `<a>` 标签相关的 `href` 属性，都表示为一个属性节点。每个属性始终与一个元素节点关联。
- **文本节点**（`Text node`）：开闭标签之间的文本，例如 `<p>` 和 `</p>` 之间的文本，表示为一个文本节点。每个文本节点与一个元素节点关联。
- **注释节点**（`Comment node`）：HTML 注释表示为一个注释节点。

每个元素节点内部可能包含其他元素节点，称为**子节点**。根据 XHTML 的结构，`<html>` 元素有两个子节点：`<head>` 和 `<body>`。

以下示例展示了带有两个子节点的 HTML `<body>` 元素：

```html
<body>
<h1>My Page Title</h1>
<img id="chart-image" src="my-chart.jpg" alt="My chart" />
</body>
```

第一个子节点（表示 `<h1>` 标签）下有一个文本节点，包含文本“My Page Title”。请注意，元素内的文本不被视为元素本身的一部分，而是它的子节点。

`<body>` 标签的第二个子节点是另一个元素节点（表示 `<img>` 标签），包含两个属性节点（分别表示 `src` 和 `alt` 中存储的值）。从 `<h1>` 标签的角度来看，`<body>` 标签被称为它的**父节点**，而 `<img>` 标签被称为它的**兄弟节点**，因为它们在文档层级中处于同一级别。

## 查找元素节点

可以通过两种方法直接在文档中查找元素节点，以下示例演示了这两种方法：

```javascript
// 返回表示 id 属性为 chart-image 的标签的元素节点
var img = document.getElementById("chart-image");

// 返回元素节点数组，所有节点均为 <body> 标签
var bodyTag = document.getElementByTagName("body");

// 返回第一个 <body> 标签内 id 属性为 chart-image 的元素
var img = bodyTags[0].getElementById("chart-image");
```

> **注意**：某些浏览器提供了两种新的元素定位方法，但尚未普及。这些方法是 `querySelectorAll()`（基于 CSS 3 选择器定位元素节点）和 `getElementsByClassName()`（基于 CSS 类名定位元素节点）。

## 操作元素与内容

一旦找到元素，你很可能想对其执行某些操作。可能是更改其文本或任何属性；也可能是完全从页面中移除该元素，或者在其前后或内部添加新元素。DOM 提供了处理所有这些情况的方法。

### 动态修改 CSS 属性

读取和修改 CSS 样式属性就像在找到的元素上使用 `style` 对象一样简单。以下演示了如何读取 `color` CSS 属性并修改 `margin-left` CSS 属性：

```javascript
var navigationEleemnt = document.getElementById("navigation");

if (navigationElement.style.color == "black") {
```

## 函数设计原则

尽可能让每个函数只执行一个逻辑动作，根据需要接收输入并产生输出。

## 使用 ScriptDoc 格式的注释作为文档

与其在代码中嵌入大量内联注释，不如确保变量和函数命名准确，并且代码块简短且不言自明。

在代码中布满大量内联注释，反而会降低它们被实际阅读的可能性。

你应该为你编写的每个函数提供文档，方法是在函数之前添加一个注释块，定义函数的功能、预期的输入参数、产生的输出类型，以及一些使用示例。这类注释块具有以下优势：

-   确保所有开发者都理解一个函数应该（和不应该）做什么。
-   帮助你判断一个函数何时执行了过多操作，正如上一节所讨论的那样。如果函数的描述特别复杂，就表明它应该被拆分成几个更小的函数。
-   通过注明函数应能接受的输入范围或类型，有助于定义你可以对该函数执行何种测试。
-   在你的代码中提供视觉分隔。

你应该使用开放的 ScriptDoc 格式（`http://www.scriptdoc.org/`）来记录你的代码。这种格式的设计使其可以跨编程语言使用。由于格式和结构是预定义的，你可以使用诸如 `Aptana` 之类的软件。

[Studio](http://www.aptana.com/) 能够根据代码中特殊格式的注释块，自动生成可读性强的文档。

以下示例展示了如何记录整个 JavaScript 文件的功能。该注释应作为 JavaScript 文件中的第一项内容，以便开发者理解文件中包含的函数的目的。

```javascript
/**
 * @projectDescription 本文件包含执行日期对象操作的代码
 *
 * @author Den Odell me@denodell.com
 * @version 1.0
 */
```

以下示例展示了如何采用 ScriptDoc 格式记录文件中的函数：

```javascript
/**
 * 对数字进行平方运算，并将结果四舍五入为最接近的整数
 *
 * 示例：
 * square(2.5); => 6
 * square(2); => 4
 *
 * @param {Float} number 需要被平方的数字
 * @return {Integer} 返回输入值平方后的四舍五入结果
 */
function square(number) {
    return Math.round(number * number);
}
```

我鼓励你熟悉 ScriptDoc 网站上的规范说明，以及用于正确描述和记录函数的具体选项。使用该规范将极大提升代码的可理解性，并能生成有用的文档，供将来可能要与你的代码协作的其他开发者参考。

## 用 TODO 标记剩余任务

编码时养成一个有用的习惯：使用包含 `TODO` 一词的注释来标记代码中未完成的部分，示例如下：

```javascript
function checkPasswordStrength(password) {
    // TODO: 完善此函数，检查所提供的密码强度
    return true;
}
```

这使你之后可以在代码库中搜索该关键词，快速定位任何需要完成的部分。

## 代码示例

```javascript
message = "Credit for choosing a fruit, at least!";
} else {
    message = "Not sure about your choice of fruit!";
}
return message;
}

function feedbackOnUsersFavoriteFruit() { // 保留原函数名
    var favoriteFruit = getFavoriteFruit();
    var score = getFruitScore(favoriteFruit);
    alert(getMessageByScore(score));
}
```

### 操作元素及其内容

通过 DOM 设置元素内容的最简单（并且在许多浏览器中最快速）的方法是使用元素的 `innerHTML` 属性。该属性将元素内部所有内容的 HTML 代码表示为一个字符串，可以读取或设置为你想要的任何内容。以下是如何读取和设置元素 `innerHTML` 值的示例：

```javascript
var pTag = document.getElementsByTagName("p")[0]; // 页面上的第一个 <p> 标签
alert(pTag.innerHTML); // 输出标签当前的 HTML 字符串内容
pTag.innerHTML = "<b>New HTML contents!</b>"; // 重写标签的 HTML
alert(pTag.innerHTML); // 输出 "<b>New HTML contents!</b>"，页面上标签的内容现在以粗体显示
```

尽管这种方法快速简便，但存在引入错误的风险，例如因设置未正确闭合的 HTML 而导致渲染错误。操作元素最稳健的方式是使用内置的 DOM 节点操作方法，这些方法会根据页面当前的 `DOCTYPE`，始终以正确的格式将 HTML 代码添加到页面中。

以下示例演示了如何使用 `createElement()` 方法创建新的元素节点。我们可以通过 `appendChild()` 方法将这个新元素插入到页面中，该方法会创建一个新的水平线元素，并将其附加到 `<body>` 标签所包含的节点列表末尾。它将在 HTML 页面的底部显示出来。

```javascript
var horizontalRule = document.createElement("hr"); // 目前仅存在于内存中
var bodyTag = document.getElementsByTagName("body")[0];
bodyTag.appendChild(horizontalRule); // 使 <hr> 标签显示在页面上
```

相反地，从页面中移除元素只需在要移除元素的父元素上调用 `removeChild()` 方法，如下所示：

```javascript
var horizontalRule = document.getElementsByTagName("hr")[0];
// 上述 <hr> 标签必须是 <body> 标签的直接子元素
var bodyTag = document.getElementsByTagName("body")[0];
bodyTag.removeChild(horizontalRule); // 该元素从页面消失
```

我仅向你展示了少数几个 DOM 方法。我建议你熟悉 DOM。它是通过 JavaScript 访问页面内容的统一方式。Mozilla 维护了一个实用的在线资源，其中包含大量示例和说明，网址为 [`developer.mozilla.org/en/Gecko_DOM_Reference`](https://developer.mozilla.org/en/Gecko_DOM_Reference)。

## 不要混淆 JavaScript 和 HTML

正如我在前一章中指出的，你绝不应该将 JavaScript 代码内联到 HTML 代码中。如果你发现自己正在 HTML 中使用像 `onclick`、`onmouseover` 这样的属性，请立即停止！使用可用的 DOM 方法来定位元素，并将你的代码应用于这些元素。这可以将你的行为代码与内容分离开来，并且使你能够在不严重影响另一方的情况下对各自进行修改。

某些浏览器不支持 JavaScript。一些用户倾向于关闭它。一些用户的工作环境默认禁用了 JavaScript。这些用户不应该需要下载他们所访问页面的所有 JavaScript 代码，因为这会使页面体积增大，却对他们没有任何益处。

你应该致力于将所有 JavaScript 代码放在外部文件中，并通过 HTML 代码中的 `<script>` 标签进行引用，这样不使用 JavaScript 的用户只需下载他们需要的内容，而其他浏览器则通过引用 URL 下载该文件。这也允许浏览器在本地缓存外部文件，这样如果多个页面引用了该文件，它只需下载一次，后续每次请求都从本地副本加载。

### 将样式与代码分离

你会发现，有时你想要通过 JavaScript 影响页面上元素的某些样式属性。也许你想显示或隐藏页面的一部分，或者当用户执行某个操作时对某些元素应用新的配色方案。请尽可能将样式规则分离到 CSS 中，然后使用 DOM 来切换你想要更改的元素上正在使用的 CSS 类名；浏览器随后会为你处理样式属性的更改。通过这种方式，你在显示样式和行为代码之间建立了更松散的联系。

通过让 CSS 文件负责设计和布局，JavaScript 负责交互和行为，你就知道应该去哪个文件中查找或进行修改。你还可以使用 CSS 创建全新的设计，而无需修改 HTML 或 JavaScript 代码。

### 动态属性修改的例外情况

当您需要动态修改某些无法预先知道具体数值的属性时，此规则存在例外情况，例如根据鼠标从初始位置移动的距离来改变组件的宽度。在这种情况下，可编码的数值太多，无法在 CSS 中穷尽所有选项。因此，应仅通过 JavaScript 修改 `width` 属性，而同一元素的其他所有样式则应在 CSS 文件中定义。

### 链式函数调用

*链式调用*指在其他方法的结果上执行方法，从而节省冗余代码行。以下示例通过依次执行多个方法（每个方法都作用于前一个调用的结果）来演示这一点。最终返回的值是链中最后一个方法的结果。

```javascript
// 获取 <body> 标签内 id 为 "horizontal-rule" 的元素的宽度
var hrTagWidth = document.getElementsByTagName("body")[0]
    .getElementById("horizontal-rule").style.width;
```

### 编写健壮代码

编写代码时，某些方法调用可能并不总能返回代码其余部分所依赖的值。如果出现这种情况，就会导致 JavaScript 错误，代码也会停止执行。专业的 JavaScript 开发人员会在对方法的返回值执行进一步操作之前，检查这些值是否为所需格式。

以下示例演示了这一实践，其中使用 DOM 查找了一个元素。

```javascript
var horizontalRule = document.getElementById("horizontal-rule");
if (horizontalRule) { // 如果元素存在，则...
    horizontalRule.parentNode.removeChild(horizontalRule); // ...移除该节点
}
```

此代码检查元素是否实际存在——即其值不为 `null` 或 `undefined`。如果对一个稍后从 HTML 中移除的元素执行操作，则会发生错误，代码执行将停止。

## 优雅处理致命异常错误

JavaScript 语言（与许多其他语言一样）的一个有用特性是，能够将您有理由怀疑可能引发异常（即强制代码停止执行的那类错误）的语句组包装在 `try/catch` 代码块中，从而以优雅的方式处理错误。`try` 部分中的代码会执行；`catch` 部分中的代码仅在 `try` 部分代码导致错误时执行。如果没有发生错误，则忽略 `catch` 部分中的代码。以下是一个 `try/catch` 结构的示例，该结构在代码执行出错时向用户发出警报，而不会因此异常导致 JavaScript 代码执行停止。

```javascript
try {
    // 以下代码无效，通常会导致异常
    document.getElementById("horizontal-rule").propertyName.toString();
} catch (error) {
    alert("发生了一个错误。");
}
```

请注意 `catch` 块如何暴露一个变量，我将其命名为 `error`。此变量包含有关所发生错误的信息。在以下代码中，我尝试找出所发生错误的类型，以便向最终用户或开发者提供更有用的消息。

```javascript
try {
    // varx 不是语言中的关键字，因此会抛出 SyntaxError 异常
    varx x = 0;
} catch (error) {
    // 有六种基本的异常类型，均在此描述
    if (error instanceof TypeError) {
        // 使用的变量类型不符合预期
    } else if (error instanceof SyntaxError) {
        alert(error.message); // 输出描述异常的消息
    } else if (error instanceof RangeError) {
```

# 定义自定义异常

## 处理 JavaScript 中的自定义异常

除了捕获和处理内置的 JavaScript 异常外，你还可以定义自己的异常类型，并随时在代码中强制触发这些异常。以下示例展示了如何在特定情况下定义并抛出自定义错误。

```javascript
// 定义一种新的错误类型
var domObjectNotFoundException = new Error("DOM 对象未找到");

function getDOMObjectById(id) {
  var domObject = document.getElementById(id);
  if (!domObject) {
    // 如果 DOM 对象不存在，则抛出自定义错误类型的异常
    throw domObjectNotFoundException;
  }
  return domObject;
}

try {
  // 假设我们试图对一个不存在的元素进行操作
  getDOMObjectById("this-id-is-not-on-the-page").style.width = "100px";
} catch (error) {
  if (error instanceof domObjectNotFoundException) {
    alert(error.message); // 输出 "DOM 对象未找到！"
  } else {
    // 捕获其他类型的错误
  }
}
```

## `finally` 块

作为常见的 `try/catch` 块结构的扩展，你可以添加一个由 `finally` 关键字标识的额外代码块。`finally` 关键字允许你添加一组额外的语句来执行，无论是否发生异常，以便将相关代码组织在一起。在以下示例中，无论 DOM 对象是否被找到，都会调用最后的 `alert` 语句。

```javascript
try {
  getDOMObjectById("this-id-is-not-on-the-page").style.width = "100px";
} catch (error) {
  alert(error.message);
} finally {
  alert("就这些了，各位！");
}
```

## 编写具备本地化意识的代码

本地化是已构建好的 RIA 应用中较为常见的需求之一。想象一下，你构建了一个大型动态 Web 应用，却发现自己需要重构所有代码，才能对其中使用的文本字符串进行本地化处理。

将文本字符串分离到变量中，并将它们集中放在代码中的某个位置。这样，后续如果需要，你就可以轻松地覆盖它们的值。采用这种方法，在后续需要向 JavaScript 代码中添加特定区域文本字符串时，能为你节省宝贵的时间。

你可以考虑将这些变量移到一个单独的文件中——或许每种语言一个文件——这样 HTML 页面就能引用它所需的特定语言文件，而应用的其他部分则正常使用这些变量。这将语言选择工作转移到了 HTML 页面，这似乎是个明智的选择，因为页面内容中已经包含了语言相关的文本。

## 面向对象的 JavaScript

自诞生之初，JavaScript 编码已经取得了长足进步，如今许多前端 Web 开发者发现自己以一种与传统软件工程师并无二致的方式编写复杂代码。随着时间的推移，开发者们通过应用一些传统程序员所使用的面向对象原则，力求减少错误、提高代码复用性并简化代码理解。

通过将相关代码分组到可复用的组件（即`对象`）中，每个组件拥有自己的变量和函数，你可以促进代码复用，并提供一层抽象，帮助你和你的同事更好地理解代码。曾经 JavaScript 文件是长长一串函数列表，彼此间关联甚少且难以导航的日子已经一去不复返了。

> **注意**：JavaScript 最初并非设计为面向对象的编程语言，若将其视为此类语言可能会导致一些困惑。部分面向对象代码设计原则确实适用，但并非全部。如果你熟悉这些原则在其他语言中的用法，请仔细阅读本节内容，以便理解其中的差异。

本节将帮助你养成使用面向对象原则和技术编写 JavaScript 代码的习惯，就像专业开发者所做的那样。

### 对象、类与构造函数

面向对象编程的两个重要概念是对象和类。接下来你将了解它们的含义，以及它们与 JavaScript 语言的关系。

#### 什么是对象？

`对象`是一种特殊类型的数据，其中包含函数和变量。JavaScript 内置了多个核心对象，包括 `Math`、`Array` 和 `String`。这些对象提供了一组函数，用于对特定类型的数据执行操作。以下展示了调用这些对象方法的一些示例。

```javascript
alert(Math.round(9.9)); // 输出 '10'
var cityList = new Array("New York", "London", "Stockholm");
var sortedCityList = cityList.sort(); // "London", "New York", "Stockholm"
alert(sortedCityList.join()); // 输出字符串 "London, New York, Stockholm"
```

请注意，你可以通过在对象名称和要调用的方法名称之间使用点号（`.`）来访问对象中的方法。

#### 什么是类？

`类`是对对象的定义。除了作为创建对象的基础之外，它本身永远不会在代码中使用。在 JavaScript 中定义类就像编写一个函数一样简单。以下示例展示了使用 `function` 关键字定义类的两种方法。

```javascript
function EmailMessage() { // 定义了一个名为 EmailMessage 的类
}

var EmailMessage = function() { // 定义同一个类的另一种方法
}
```

这两种定义类的方式都是有效的。你可以根据自己的喜好选择任何一种方式在代码中使用。

#### 作为类实例的对象

再看一下使用对象的示例：

```javascript
alert(Math.round(9.9));
var cityList = new Array("New York", "London", "Stockholm");
var sortedCityList = cityList.sort();
alert(sortedCityList.join());
```

观察 `Math` 对象和 `Array` 对象在用法上的区别。请注意，`Math` 对象可以直接使用，但 `Array` 对象前面需要使用 `new` 关键字。`Array` 对象定义了一个与数组列表相关函数的模板，根据我们之前的定义，这使它成为一个类。

要从一个类中创建可在代码中使用的内容，你需要使用 `new` 关键字，它会创建该类的一个`实例`。实际上，这允许你创建一个或多个对象，每个对象都包含类中定义的所有函数和变量，并且它们之间互不干扰。以下展示了几个将对象创建为类实例的示例。

```javascript
var oceans = new Array("Atlantic", "Pacific", "Indian", "Arctic", "Antarctic");
var myMessage = new EmailMessage(); // 使用之前定义的类
var anotherMessage = new EmailMessage(); // 独立于上面的 myMessage
```

#### 构造函数

`构造函数`是一段可选的代码，在使用 `new` 关键字从类创建对象的瞬间执行。在 JavaScript 中，只需向定义类的函数中添加代码即可实现，如下所示：

```javascript
var EmailMessage = function() {
  // 实例化类时应执行的操作
  alert("新消息已创建。");
}

var myMessage = new EmailMessage(); // 输出 "新消息已创建。"
var anotherMessage = new EmailMessage(); // 输出 "新消息已创建。"
```

就像 JavaScript 语言中的普通函数可以传递参数供函数内部使用一样，类的构造函数也可以传递参数。以下是一个接受一个参数的类构造函数：

```javascript
var EmailMessage = function(message) {
  alert(message);
}

// 输出 "Return to sender"
```

var myMessage = new EmailMessage("Return to sender");

## 属性

在面向对象术语中，`属性`是存储在对象内部的变量。属性在类中使用 `this` 关键字定义，然后可供从该类实例化的每个对象使用。以下示例展示了如何在类中定义属性，以及如何从对象实例访问这些属性。

```javascript
var EmailMessage = function() {
    this.subject = ""; // 使用 `this` 关键字为类分配属性
}

var myMessage = new EmailMessage();
myMessage.subject = "Check this out..."; // 为属性赋值

var anotherMessage = new EmailMessage();
anotherMessage.subject = "Have you seen this before?";

alert(myMessage.subject); // 输出 "Check this out..."
alert(anotherMessage.subject); // 输出 "Have you seen this before?"
```

在类中使用 `this` 关键字来引用实例化对象的作用域，稍后将对此进行更详细的讨论。

## 方法

属性定义了与类或对象关联的变量，而方法则定义了所有对象实例均可使用的可执行函数。下面的示例演示了如何向类添加方法，以及如何从该类实例化的对象中执行该方法。

```javascript
var EmailMessage = function(subject) {
    this.subject = subject;
    this.send = function() {
        alert("Message '" + this.subject + "' sent!");
    }
}

var myMessage = new EmailMessage("Dear John...");
myMessage.send(); // 输出 "Message 'Dear John...' sent!"
```

## `prototype` 关键字

在类定义之外，向类分配属性和方法的另一种方式是，在类定义后，针对类名使用 `prototype` 关键字。下面演示如何以这种方式向类添加属性和方法：

```javascript
var EmailMessage = function(subject) {
    this.subject = subject;
}

EmailMessage.prototype.from = ""; // 动态地向类分配新属性
EmailMessage.prototype.send = function() { // 动态地分配新方法
    alert("Message from " + this.from + " sent!"); // 'this' 的行为仍如常
}

var myMessage = new EmailMessage("My new website");
myMessage.from = "me@denodell.com";
myMessage.send(); // 输出 "Message from me@denodell.com sent!"
```

除了允许你在类定义后向其添加额外的属性和方法外，你还可以使用 `prototype` 关键字向已从这些类创建的对象分配新的属性和方法。这允许你动态地为已存在的对象实例添加新功能。下面的示例演示了这一原理。

```javascript
var EmailMessage = function(subject) {
    this.subject = subject;
}

// 从 EmailMessage 类实例化一个对象
var myMessage = new EmailMessage("Coming to visit...");

// 向类添加一个新方法
EmailMessage.prototype.send = function() {
    alert("Message sent!");
}

// 这个新方法存在于该方法添加之前创建的对象上！
myMessage.send(); // 输出 "Message sent!"
```

## 单例

单例是一种在你的代码中始终只有一个实例的类，例如 JavaScript 语言本身中的 `Math` 对象。你无法创建此对象的新实例，但它确实包含可以在代码中利用的属性和方法。

`代码清单 2-1` 演示了如何创建单例。

**代码清单 2-1.** *创建单例*

```javascript
var User = function() {
    this.username = "";
    this.password = "";
    this.login = function() {
        return true;
    }
}

// 创建 User 类的一个实例，并将其存储在与最初定义该类时相同的变量中。
// 原始类现已从代码中移除，只留下了它的单个对象实例
User = new User();

// 对 User 的单个实例进行方法调用示例
User.login();

// 自实例化类的示例
var Inbox = new function() {
    this.messageCount = 0;
    this.refresh = function() {
        return true;
    }
}();

// new 关键字和大括号强制该函数立即执行，
// 这意味着 Inbox 变量现在包含的是单个对象实例，而不是类
// 对 Inbox 的单个实例进行方法调用示例
Inbox.refresh();
```

现在你已经熟悉了如何创建具有属性和方法的类。接下来，让我们转向面向对象编程的一个重要方面：继承。

## 继承：从现有类创建新类

`继承`允许你创建从其他类派生的类，从而以某种方式对该类进行特化。原始类称为`父类`，从中派生出的新类称为`子类`。

JavaScript 中的继承同样是使用 `prototype` 关键字来执行的。在定义了子类之后，将父类赋值给子类的 `prototype`，这样父类的属性和方法就会立即对子类可用。

然后，子类可以根据需要覆盖或扩展父类的任何方法或属性。`清单 2-2` 演示了如何使用 JavaScript 执行基本继承。

**清单 2-2.** *JavaScript 中的基本继承*

```javascript
var EmailMessage = function(subject) {
    this.subject = subject;
    this.send = function() {
        alert("Message '" + this.subject + "' sent!");
    }
}

// 创建一个新的空类
var EventInvitation = function() {};

// 继承 EmailMessage 类的属性和方法
EventInvitation.prototype = new EmailMessage();

// EventInvitation 认为自己就是 EmailMessage 类，因此修正这一点...
EventInvitation.prototype.constructor = EventInvitation;

// 为 EventInvitation 类的所有实例定义主题
EventInvitation.prototype.subject = "You are cordially invited to...";

// 创建 EventInvitation 类的一个实例
var myEventInvitation = new EventInvitation();

// 输出 "Message 'You are cordially invited to...' sent!"
myEventInvitation.send();
```

## 封装：每个类各司其职

当使用继承来创建现有类的变体或特化时，父类的所有属性和方法都对子类可用。你不需要在子类中声明或定义任何额外的东西就能使用父类的属性和方法。这被称为`封装`。子类只需包含那些在父类属性方法基础之上新增的属性和方法的定义。

## 多态：重定义继承的属性和方法

你已经看到，在创建继承自父类的新子类时，子类拥有父类的所有属性和方法。现在假设你希望子类包含一个与父类同名的方法，并在子类中执行该方法内的代码，而不是使用父类的方法。这就是`多态`。`清单 2-3` 演示了多态，其中子类包含一个与其继承的父类同名的方法。

**清单 2-3.** *多态实战：替换父类的方法*

```javascript
var EmailMessage = function(subject) {
    this.subject = subject;
    this.send = function() {
```

alert("Email message sent!");
}

}

// 让 EventInvitation 类继承自 EmailMessage

var EventInvitation = function() {};

EventInvitation.prototype = new EmailMessage("You are cordially invited to...");

EventInvitation.prototype.constructor = EventInvitation;

// 重写继承来的 send 方法

EventInvitation.prototype.send = function() {
  alert("Event invitation sent!");
}

var myEmailMessage = new EmailMessage("A new email coming your way.");
var myEventInvitation = new EventInvitation();

myEmailMessage.send(); // 输出 "Email message sent!"
myEventInvitation.send(); // 输出 "Event invitation sent!"

```

现在假设你想重写子类中的一个方法，使其在父类方法的基础上执行额外的代码。也就是说，不是替换父类的方法，而是希望以某种方式扩展它，同时仍然使用父类提供的同名方法。清单 2-4 演示了这一原理。

> **注意** 任何你想要像清单 2-4 那样扩展的方法或属性，都必须在父类中使用 `prototype` 关键字声明；否则，它们将无法从子类内部找到。

**清单 2-4.** *多态实战：扩展父类的方法*

```
var EmailMessage = function(subject) {
  this.subject = subject;
}

// 我们希望以后能扩展这个方法，
// 因此它必须使用 prototype 关键字进行声明
EmailMessage.prototype.send = function() {
  alert("Email message sent!");
}

// 让 EventInvitation 类继承自 EmailMessage
var EventInvitation = function() {};
EventInvitation.prototype = new EmailMessage("You are cordially invited to...");
EventInvitation.constructor.prototype = EventInvitation;

// 重写继承来的 send 方法
EventInvitation.prototype.send = function() {
  // 为 EventInvitation 的 send 方法添加代码
  alert("Event invitation sent!");
  // 找到并执行 EmailMessage 类中的 send 方法
  // this.constructor.prototype 指向 EmailMessage 类
  this.constructor.prototype.send.call(this);
}

var myEmailMessage = new EmailMessage("A new email coming your way.");
var myEventInvitation = new EventInvitation();

// 输出 "Email message sent!"
myEmailMessage.send();
// 先输出 "Event invitation sent!"，然后输出 "Email message sent!"
myEventInvitation.send();
```

## `this` 关键字

构建任何基于对象的 RIA 的一个基本部分，是理解 JavaScript 中的 `this` 关键字。在本章的代码示例中，你已经在将属性和方法与类关联时，以及在子类内部调用父类方法时，看到了它的使用。了解如何正确使用这个强大的关键字将提升你的编码技能，并让你更好地掌握 JavaScript 语言本身。

`this` 关键字指向包含它的函数的**所有者**。如果它出现在类的方法中，则指向类本身，或者更准确地说，指向代码执行时创建的类的对象实例。在类的上下文之外，`this` 通常指向页面的全局 `window` 对象。请看清单 2-5，它展示了一些 `this` 关键字在不同上下文中基于同一个函数运行时的示例。

**清单 2-5.** *`this` 关键字实战*

```
var showSubject = function() {
  // 输出与这个函数拥有相同所有者的 subject 属性
  alert(this.subject);
}

// 输出 "undefined"，因为 this 关键字指向全局 window 对象，
// 而它没有名为 subject 的变量
showSubject();

// 设置一个全局的 subject 属性
this.subject = "Global subject";
// 现在属性已被设置，输出 "Global subject"
showSubject();

// 定义 EmailMessage 类
var EmailMessage = function(subject) {
  this.subject = subject;
}

// 复制并将 showSubject 函数赋给 EmailMessage 类，
// 使 EmailMessage 类成为该函数的所有者。
// 注意函数名后没有圆括号，这表示复制的是函数的代码而非立即执行它
EmailMessage.prototype.showSubject = showSubject;

// 创建该类的一个新实例
var myEmailMessage = new EmailMessage("I am the subject.");

// 输出 "I am the subject."，因为该函数的所有者是该类
myEmailMessage.showSubject();

// 再次输出 "Global subject" 以演示 this 没有被丢失
showSubject();

// 现在让我们向类中添加另一个方法，以不同方式调用 showSubject
EmailMessage.prototype.outputSubject = function() {
  showSubject();
}

// 输出 "Global subject"，因为此方法调用的函数
// 是与 window 对象关联的
myEmailMessage.outputSubject();
```

如果你希望调用一个函数或方法，并确保 `this` 关键字指向特定的对象，可以使用 `call()` 或 `apply()` 方法。

## 使用 `apply` 和 `call` 实现不同的对象引用

当你需要强制函数内部的 `this` 关键字指向一个与默认引用不同的对象时，有两个函数可以做到这一点：`apply()` 和 `call()`。两者在实现上的区别非常细微。`apply()` 期望你传递给函数的任何参数都以数组形式提供，而 `call()` 则不需要。清单 2-6 展示了这两个函数的实际应用，它们改变了一组可重用函数中 `this` 关键字的值。

**清单 2-6.** 使用 `apply` 和 `call` 改变 `this` 关键字的值

```
var showSubject = function() {
    alert(this.subject);
}

var setSubjectAndFrom = function(subject, from) {
    this.subject = subject;
    this.from = from;
}

// 针对全局 window 对象执行
showSubject(); // 输出 "undefined"
setSubjectAndFrom("Global subject", "me@denodell.com");
showSubject(); // 输出 "Global subject"

// 创建 EmailMessage 类
var EmailMessage = function() {
    this.subject = "";
    this.from = "";
};

// 实例化类
var myEmailMessage = new EmailMessage();

// 执行 setSubjectAndFrom 函数，强制函数内的 this 关键字
// 指向 myEmailMessage 而非 window 对象。参数

// 在作为拥有者的对象之后以序列形式传递，将 `subject` 设置为

// "New subject"，并将 `from` 属性设置为 "den@denodell.com"

`setSubjectAndFrom.call(myEmailMessage, "New subject", "den@denodell.com");`

// 作为演示，我们现在使用 `apply` 代替 `call` 来做同样的事情；

// 与 `call` 不同，`apply` 期望参数以数组形式提供。

// 你可以在代码中自由使用 `apply` 或 `call`

`setSubjectAndFrom.apply(myEmailMessage, [ "New subject", "den@denodell.com" ]);`

// 输出 "New subject"

`showSubject.call(myEmailMessage);`

```

## 属性和方法的访问权限

大多数编程语言允许你定义对象中特定属性或方法在整个应用程序范围内的访问级别。到目前为止，所有示例中创建的类的属性和方法都是公开可用的，也就是说，它们可以从代码库中的任何位置访问。创建类的新实例允许对该实例中的任何属性进行读写访问，并允许执行任何方法。

但是，如果你要为类创建一个只在类内部可用的属性或方法（称为`私有`）呢？或者，假设你想从类定义外部对私有属性进行只读访问；你可以称之为`特权`属性。

> **注意：** 不要在此处将 `public`、`private` 和 `privileged` 这些术语与其他语言中的类似名称混淆。在此上下文中，我们仅讨论从类外部对类内部属性和方法的访问。

清单 2-7 演示了类属性的不同类型访问方式。

**清单 2-7.** 限制对类属性和方法的访问

```

var EmailMessage = function(subject) {

    // 公开可访问的属性和方法

    this.subject = subject;

    this.send = function() {

        alert("Message sent!");

    }

    // 私有属性和方法。

    // 使用 `var` 而非 `this` 关键字来设置属性意味着该

    // 变量的作用域被限制在其所在的函数内部，并且

    // 无法从外部访问

    var messageHeaders = "";

    var addEncryption = function() {

        // TODO：添加加密方法

        return true;

    }

    // 保护属性，使其从类外部只读，

    // 通过创建私有属性和公共方法实现

    var messageSize = 1024;

    this.getMessageSize = function() {

        alert(messageSize);

    }

}

var myEmailMessage = new EmailMessage("Save these dates...");

alert(myEmailMessage.subject); // 输出 "Save these dates..."

myEmailMessage.send(); // 输出 "Message sent!"

```


```javascript

// 输出 "undefined"，因为 `messageHeaders` 不是一个公开可见的属性

alert(myEmailMessage.messageHeaders);

// 由于该方法不存在，会引发异常

try {

    myEmailMessage.addEncryption();

} catch (e) {

    alert("该方法在公开范围内不存在！");

}

```

```

// 输出 "undefined"，因为 `messageSize` 在类外部无法访问

alert(myEmailMessage.messageSize);

// 输出 "1024"，即私有变量 `messageSize` 的值

myEmailMessage.getMessageSize();

```

### 对象字面量与 JavaScript 对象表示法

JavaScript 语言中的**对象字面量**本质上是一个列表或集合，包含任意数量的属性名称及其对应的值。对象字面量的值可以是字符串、整数、浮点数、布尔值、数组、函数，甚至是其他对象字面量，并以特定格式表示。

对象字面量的格式通常被称为 JavaScript 对象表示法（JSON）。

对象字面量的开始和结束由花括号`{`和`}`表示。花括号内列表中的每个项用逗号分隔，每个名称与其值之间用冒号分隔。

在语言中，对象字面量用于将相关属性和方法分组在一起，类似于类的用法。然而，对象字面量只有一个实例，并且在创建时无法像类定义那样执行任何代码语句。清单 2-8 展示了一个包含多种类型值的对象字面量。

**清单 2-8.** *包含多种类型值的对象字面量*

```javascript

var earth = {

    name: "Terra Firma",      // 字符串

    planet: true,             // 布尔值

    moons: 1,                 // 整数

    diameter: 12756.36,       // 浮点数（小数）

    oceans: ["大西洋", "太平洋", "印度洋", "北冰洋", "南极海"], // 数组

    poles: {                  // 嵌套的对象字面量

        north: "北极",

        south: "南极"

    },

    setDiameter: function(diameter) {   // 函数

        this.diameter = diameter;       // `this` 关键字指向 `earth` 变量

    }

}

// 使用点号表示法访问对象的属性

alert(earth.diameter);          // 输出 "12756.36"

earth.setDiameter(12756.37);

alert(earth.diameter);          // 输出 "12756.37"

```

只要在实例化单例时不需要执行任何特定代码，您就可以使用对象字面量创建单例。

### 在创建类时使用对象字面量

对象字面量表示法可以与`prototype`关键字一起使用来设置类的定义，如下所示：

```javascript

var EmailMessage = function() {};

EmailMessage.prototype = {

    subject: "",

    from: "",

    send: function() {

        alert("消息已发送！");

    }

}

var myEmailMessage = new EmailMessage();

// 正常访问属性和方法

myEmailMessage.subject = "过来参加派对……";

```

这种格式的局限性在于，您可能需要编写额外的代码来初始化类中的属性。

### 将对象字面量用作函数的输入

当您编写一个需要多个参数的函数时，应考虑接受一个对象字面量作为该函数的输入。这样可以用一个参数替换所有参数；通过点号表示法从单个参数中提取值。以这种方式使用对象字面量通常会使代码更易于阅读和理解，因为对象字面量中的每个属性值都有名称，为提供给函数的参数提供了上下文。

清单 2-9 演示了两个行为相同的函数，但一个接受多个参数，另一个只接受一个参数。

**清单 2-9.** *对象字面量作为函数的输入*

```javascript

// 使用特定顺序的多个参数

var sendEmail = function(to, from, subject, body) {

    alert("消息 '" + subject + "' 来自 '" + from + "' 已发送至 '" + to + "'！");

}

// 调用函数时参数必须按正确顺序排列。

// 输出 "消息 'Dinner this week?' 来自 'me@denodell.com' 已发送至 'you@denodell.com'！"

sendEmail("you@denodell.com", "me@denodell.com", "Dinner this week?", "Do you want to come over for dinner this week? Let me know.");

// 同样的函数，但使用单个包含命名属性的对象字面量参数

var sendEmail = function(message) {

# JavaScript DOM 就绪与事件处理

```javascript
alert("消息 '" + message.subject + "' 来自 '" + message.from + "' 已发送至 '" + message.to + "'！");
}

// 一个包含命名属性值的对象字面量参数，无特定顺序

// 输出 "消息 'Dinner this week?' 来自 'me@denodell.com' 已发送至 'you@denodell.com'！"

sendEmail({
    from: 'me@denodell.com',
    to: 'you@denodell.com',
    subject: 'Dinner this week?',
    body: 'Do you want to come over for dinner this week? Let me know.'
});
```

### 创建命名空间和层次结构

在全局的`window`对象中创建过多的变量通常被称为`污染全局作用域`。这种做法很危险，因为您定义的变量可能与网站上运行的第三方脚本使用的变量重名。JavaScript 不会在变量被这样覆盖时发出警告，因此您可能依赖的值已被外部不可控因素覆盖。

解决这个问题的方法是在全局作用域中创建一个单独的变量，作为对象字面量或单例，包含应用程序的所有代码。这在许多编程语言中被称为`命名空间`。您可以在该全局对象内创建其他对象字面量或单例，以维护命名空间的层次结构，将相关代码分组在一起。以下是一个命名空间层次结构的示例：

```javascript
var MyCompany = new function(){
    this.MyClient = {                 // 对象字面量
        WebMail: function() {        // 构造函数
            alert("正在创建 WebMail 应用程序……");
        }
    };
}();    // 创建 MyCompany 作为单例

// 现在可以通过点号表示法访问层次结构。
// 输出 "正在创建 WebMail 应用程序……"
var myWebMail = new MyCompany.MyClient.WebMail();
```

### 库与框架

近年来，为了简化动态网页和富互联网应用程序（RIA）的开发，已经编写了几个开源的 JavaScript 库。它们主要的存在目的是平滑不同浏览器在 JavaScript 实现上的不一致性，并帮助开发者编写更少的代码来实现目标。

不幸的是，不同浏览器在支持 JavaScript 的某些方面存在差异——并非所有浏览器都支持使用相同的方法执行相同的操作。一些主要的差异点包括：

- 处理用户与页面交互时发生的事件
- 从 Web 服务器加载远程内容
- 检测 DOM 何时初始化完毕并可用
- 确定哪些 CSS 样式应用到了特定元素

JavaScript 库通过填补缺失的功能和修复不正确的实现来消除这些差异。

此外，许多 JavaScript 库还包含额外的实用方法，以简化常见任务，例如使用 CSS 3 选择器表示法定位页面元素。许多库也开始增加对吸引人的过渡效果的支持，使页面上一种样式过渡到另一种样式时更平滑。

> **提示：** 关于 CSS 3 选择器表示法的指南，请访问 [`www.w3.org/TR/css3-selectors/`](http://www.w3.org/TR/css3-selectors/#selectors)。请注意，选择器列表目前仍是尚未完成的工作。

在构建丰富互联网应用程序（RIA）时，你应该使用一个 JavaScript 库。没有跨浏览器兼容性处理的平滑过渡，你将难以生存，而额外的实用方法将帮助你更高效地编码。

#### 选择库

有多个开源 JavaScript 库可供选择。目前，一些最流行的库包括 jQuery（[`jquery.com/`](http://jquery.com/)）、Dojo（[`www.dojotoolkit.org/`](http://www.dojotoolkit.org/)）、Prototype（[`www.prototypejs.org/`](http://www.prototypejs.org/)）、MooTools（[`mootools.net/`](http://mootools.net/)）、Yahoo! 用户界面库（YUI）（[`developer.yahoo.com/yui/`](http://developer.yahoo.com/yui/)）以及 Ext JS（[`extjs.com/`](http://extjs.com/)）。这些库中有许多拥有大量支持额外功能的三方插件。

在调研和选择 JavaScript 库时，请考虑以下标准：

*不扩展 JavaScript 对象*：选择一个不严重依赖扩展原生 JavaScript 对象（如 `Function`、`String`、`Array` 等）的库。如果你发现某个库允许你在这些对象上调用不属于 JavaScript 语言的方法或更改属性，请务必谨慎。语言未来的新增内容可能会破坏这些方法和属性，其他第三方 JavaScript 库也可能以不同的方式覆盖相同的方法。

*文档完善*：选择一个文档完善，并且在发布新版本前运行了大量测试用例的库。

*社区活跃*：选择一个拥有活跃开发者和用户社区的库。你遇到的任何问题，更有可能之前已经被解决了。如果没有，你的问题更有可能得到处理，并且库也会因此做出必要的修改。

*没有内存泄漏*：选择一个不会泄漏内存的库。这又回到了前面提到的测试用例。

*小巧高效*：选择一个不大且不臃肿的库。小巧不一定意味着更好，但这确实意味着你代码的用户在能使用你的网站之前，需要下载更少的 JavaScript。

*拥有自己的命名空间*：选择一个不会污染全局作用域的库。确保该库位于其自己的命名空间内。

*允许标准 JavaScript 语法*：最后，选择一个允许你以预期方式编写 JavaScript 的库。警惕那些强迫你花费大量时间学习如何使用其模式（仿佛它是一种完全不同的语言）的库。请记住，你是一名 JavaScript 程序员，而不是库程序员。

并非每个库都满足所有这些标准。你应该仔细研究可用的库，以便在构建 RIA 时，对自己将要依赖的库做出明智的决定。

#### 构建一个 JavaScript 库

为了理解库所解决的问题类型，特别是关于跨浏览器兼容性的问题，并展示一个遵循上一节中大部分指南的库，让我们来构建一个基础 JavaScript 库。这个库将作为本书中许多代码示例的基础。

运用本章讨论的面向对象编程原则，我们创建一个类来描述我们的 JavaScript 库，并将其简称为 `$`：

```javascript
var $ = function() {};
```

这不仅是该语言中函数或变量名的有效名称，实际上，由于其长度短（意味着代码更短），它经常被用作代表 JavaScript 库全局命名空间的名称。它的使用最初源于 Ruby 和 PHP 编程语言中。

#### 检测 DOM 何时可用

JavaScript 程序员的一个主要困扰是，当执行的脚本需要访问 DOM 时，DOM 有时尚未可用。如果脚本被引用在 HTML 页面的 `<head>` 部分，它将在该页面的 `<body>` 加载之前执行，这意味着此时它将无法定位页面元素。以前，开发者会将整个代码添加到 `window.onload` 事件中，这意味着它将在整个页面（包括任何外部图片、CSS 和 JavaScript 文件）加载完成后才执行。页面上的图片越多，用户等待脚本执行的时间就越长。

最终，一些浏览器厂商与 W3C 共同引入了 `DOMContentLoaded` 事件，该事件在页面元素加载完成并可通过 JavaScript 访问时触发。此事件在 `window.onload` 事件之前触发，也在外部文件完成加载之前触发，这意味着开发者可以让脚本针对 DOM 执行，而无需等待整个页面内容加载完成。

不幸的是，`DOMContentLoaded` 事件尚未在所有常见浏览器中实现，尤其是 IE 6、7 和 8。为了平滑这种不一致性，并在支持该事件的浏览器中提供支持，在不支持的浏览器中提供变通方案，你需要编写自己的方法，在 DOM 准备好使用时执行你的代码。清单 2-10 展示了在你的 `$` 库中，在所有浏览器中实现此行为的一种方法。

**清单 2-10.** *在 DOM 准备好访问时执行代码*

```javascript
$.prototype.onDomReady = function(callback){ // callback 应为一个函数
    if (document.addEventListener) {
        // 如果浏览器支持 DOMContentLoaded 事件，
        // 则在该事件触发时将回调函数分配给其执行
        document.addEventListener("DOMContentLoaded", callback, false);
```

} else {

    if(document.body && document.body.lastChild){

        // 如果 DOM 已可访问，则执行回调函数

        callback();

    } else {

        // 重新执行当前函数（由 arguments.callee 表示），

        // 等待一个短暂的纳秒时间，以免锁定浏览器

        return setTimeout(arguments.callee, 0);

    }

}

}

// 使用示例

// 将 $ 库对象实例化为页面使用的单例

$ = new $();

// 当 DOM 可访问时，输出“DOM 已就绪！”

$.onDomReady(function() {

    alert("DOM 已就绪！");

});

```

## 浏览器中的事件处理

当某些操作（如超链接点击和表单提交）发生时，JavaScript 事件会在浏览器内触发，你可以编写代码在这些时刻执行。问题在于，在 IE 6 和 7 与其他浏览器之间，为这些事件分配函数的方式有所不同。不仅如此，确定事件发生的页面元素以及事件的其他属性（如鼠标位置）的方法，在这些浏览器之间也有所不同。

清单 2-11 展示了如何向你的 `$` 库添加方法，以平滑这些不一致性。

**清单 2-11.** *编写方法以标准化浏览器中的事件处理*

```javascript
// 向 $ 库添加新的命名空间，用于存放所有与事件相关的代码，
// 使用对象字面量表示法一次性添加多个方法
$.prototype.Events = {
    // add 方法允许我们为特定元素上发生指定类型的事件时分配要执行的函数
    add: function (element, eventType, callback) {
        // 存储 this 的当前值，以便在子函数中使用
        var self = this;
        eventType = eventType.toLowerCase();
        if (element.addEventListener) {
            // 如果 W3C 的事件监听器方法可用，则使用该方法
            element.addEventListener(eventType, function(e){
                // 执行回调函数，并向其传递一个标准化后的事件对象 e
                // standardize 方法将在稍后定义
                callback(self.standardize(e));
            }, false);
        } else if (element.attachEvent) {
            // 否则，使用 Internet Explorer 专有的事件处理器
            element.attachEvent("on" + eventType, function() {
                // IE 使用 window.event 来存储当前事件的属性
                callback(self.standardize(window.event));
            });
        }
    },

    // remove 方法允许我们移除先前为事件指定的代码
    remove: function (element, eventType, callback) {
        eventType = eventType.toLowerCase();
        if (element.removeEventListener) {
            // 如果 W3C 指定的方法可用，则使用该方法
            element.removeEventListener(element, eventType, callback);
        } else if (element.detachEvent) {
            // 否则，使用 Internet Explorer 特定的方法
            element.detachEvent("on" + eventType, callback);
        }
    },

    // standardize 方法用于生成一组统一的事件属性，无论浏览器是什么
    standardize: function(event) {
        // 稍后定义的这两个方法，返回鼠标指针的当前位置，
        // 一个相对于整个文档，另一个相对于事件发生的元素
        var page = this.getMousePositionRelativeToDocument(event);
        var offset = this.getMousePositionOffset(event);

        // 阻止事件在当前元素之上的元素节点上触发
        if (event.stopPropagation) {
            event.stopPropagation();
        } else {
            event.cancelBubble = true;
        }

        // 我们返回一个包含七个属性和一个方法的对象字面量
        return {
            // target 是事件发生的元素
            target: this.getTarget(event),
            // relatedTarget 是事件正在监听的那个元素，
            // 如果事件发生在 relatedTarget 元素 DOM 内部的某个元素上，则可能不同于 target
            relatedTarget: this.getRelatedTarget(event),
            // 如果事件与键盘相关，key 返回按下的字符
            key: this.getCharacterFromKey(event),
            // 返回鼠标指针的 x 和 y 坐标，相对于文档
            pageX: page.x,
            pageY: page.y,
            // 返回鼠标指针的 x 和 y 坐标，相对于当前事件发生的元素
            offsetX: offset.x,
            offsetY: offset.y,
            // preventDefault 方法用于阻止所操作元素的默认事件发生。
            // 例如，如果我们监听超链接上的点击事件，此方法将阻止链接被跳转
            preventDefault: function() {
                if (event.preventDefault) {
                    event.preventDefault(); // W3C 方法
                } else {
                    event.returnValue = false; // Internet Explorer 方法
                }
            }
        };
    },

    // getTarget 方法用于定位事件发生的元素
    getTarget: function(event) {
        // Internet Explorer 使用 srcElement，W3C 使用 target
        var target = event.srcElement || event.target;
        // 修复旧版 Safari 的一个 bug，该 bug 会将事件报告发生在文本节点上，
        // 而不是元素节点上
        if (target.nodeType == 3) { // 3 表示文本节点
            target = target.parentNode; // 获取文本节点的父节点
        }
        // 返回事件发生的元素节点
        return target;
    },

    // getCharacterFromKey 方法在键盘事件发生时返回按下的字符。
    // 你应该使用 keypress 事件，因为其他事件的可靠性各不相同
    getCharacterFromKey: function(event) {
        var character = "";
        if (event.keyCode) { // Internet Explorer
            character = String.fromCharCode(event.keyCode);
        } else if (event.which) { // W3C
```

# 第 2 章 ■ 面向富互联网应用的 JavaScript

## 84

```javascript
character = String.fromCharCode(event.which);
    }
    return character;
},
```

```javascript
// getMousePositionRelativeToDocument 方法返回当前
// 鼠标指针相对于当前页面左上角的位置
getMousePositionRelativeToDocument: function(event) {
    var x = 0, y = 0;
    if (event.pageX) {
        // pageX 获取指针相对于整个文档左侧的坐标
        x = event.pageX;
        y = event.pageY;
    } else if (event.clientX) {
        // clientX 获取指针相对于当前可视区域左侧的坐标
        // 因此我们必须将页面已滚动的距离加到该值上
        x = event.clientX + document.body.scrollLeft +
            document.documentElement.scrollLeft;
        y = event.clientY + document.body.scrollTop +
            document.documentElement.scrollTop;
    }
    // 返回一个包含鼠标 x 和 y 坐标的对象字面量
    return {
        x: x,
        y: y
    };
},
```

```javascript
// getMousePositionOffset 方法返回鼠标指针相对于事件发生元素左上角的距离
getMousePositionOffset: function(event) {
    var x = 0, y = 0;
    if (event.layerX) {
        x = event.layerX;
        y = event.layerY;
    } else if (event.offsetX) {
        // Internet Explorer 专有属性
        x = event.offsetX;
        y = event.offsetY;
    }
    // 返回一个包含鼠标相对于触发事件的元素的 x 和 y 坐标的对象字面量
    return {
        x: x,
        y: y
    };
},
```

```javascript
// getRelatedTarget 方法返回事件原本应触发的元素节点，可能与实际触发事件的元素不同
getRelatedTarget: function(event) {
    var relatedTarget = event.relatedTarget;
    if (event.type == "mouseover") {
        // 对于 mouseover 事件，relatedTarget 默认未设置
        relatedTarget = event.fromElement;
    } else if (event.type == "mouseout") {
        // 对于 mouseout 事件，relatedTarget 默认未设置
        relatedTarget = event.toElement;
    }
    return relatedTarget;
},
```

## 85

```javascript
// 使用示例
// 将库实例化为单例，以便在页面中使用
$ = new $();
// 点击页面任意位置将输出鼠标指针的当前坐标
$.Events.add(document.body, "click", function(e) {
    alert("鼠标点击的位置：'x'坐标为 " + e.pageX + "，'y'坐标为 "+ e.pageY);
});
```

## 使用 Ajax 按需加载内容

大多数 RIA 的一个基本功能是无需刷新页面即可直接与 Web 服务器通信。这使得 Web 应用程序的行为更像桌面应用程序，可以在后台加载和保存数据，并独立地更新页面的各个部分。

`异步 JavaScript 和 XML`（简称 Ajax）通常是指通过 JavaScript 从远程 Web 服务器下载或发送内容的实践。尽管名称中提到了 XML，但该技术实际上并不依赖 XML 格式的数据进行通信；实际上，任何类型的内容都可以传输或接收。术语*异步*意味着，在加载内容时，脚本的其余部分仍可继续执行——代码不会停下来等待内容加载完成后再继续。

> **注意** 作为一种安全措施，使用 Ajax 技术时，浏览器只允许你访问与当前执行代码的页面位于同一域名下的内容。许多开发者通过在其 Web 服务器上包含一个服务器端脚本来解决这个限制，当向该脚本传递一个 URL 作为查询字符串参数时，它会加载该 URL 的内容并将其传递回页面上的 Ajax 请求。

别忘了在编写代码时，如果你通过此技术提供了执行操作或加载某些内容的方法，那么在相关情况下，同样的操作必须能够仅通过 HTML 完成。例如，如果你正在创建一个 Web 邮件客户端，你应该先构建一个能够通过 HTML 正常运行的应用程序，然后再添加 JavaScript 和 Ajax 技术，将其转变为 RIA。

让我们在你的`$`库中添加一些 JavaScript 方法，用于处理浏览器与 Web 服务器之间的交互。如代码中的注释所述，有几个跨浏览器障碍需要克服。清单 2-12 演示了如何以跨浏览器的方式动态加载内容。

## 86

### 清单 2-12. 使用 Ajax 动态加载内容

```javascript
// 在$库中定义一个名为 Remote 的新命名空间，用于存储
// 我们的 Ajax 方法
$.prototype.Remote = {
    // getConnector 方法返回用于通过 JavaScript 执行
    // 动态浏览器-服务器通信的基础对象
    getConnector: function() {
        var connectionObject = null;
        if (window.XMLHttpRequest) {
            // 如果 W3C 支持的请求对象可用，则使用该对象
            connectionObject = new XMLHttpRequest();
        } else if (window.ActiveXObject) {
            // 否则，如果 IE 专有对象可用，则使用该对象
            connectionObject = new ActiveXObject('Microsoft.XMLHTTP');
        }
        // 两个对象包含几乎相同的属性和方法
        // 因此只需返回当前浏览器支持的正确对象即可
        return connectionObject;
    },
    // configureConnector 方法定义了请求过程中应执行的操作，
    // 并确保在服务器成功返回响应时执行回调函数
    configureConnector: function(connector, callback) {
        // readystatechange 事件在请求生命周期的不同阶段触发：
        // 加载开始时、加载进行中以及加载结束时
        connector.onreadystatechange = function() {
            // 如果请求的当前状态表明请求已完成
            if (connector.readyState == 4) {
                // 确保 HTTP 状态码指示内容成功下载
                if (connector.status == 200) {
                    // 执行回调函数，并传入一个包含两个属性的对象字面量：
                    // 下载内容的原始文本，以及如果请求的内容能够被解析为 XML 时的 XML 格式内容。
                    // 同时将回调函数的 owner 设为 connector，以防回调函数中需要该对象
                    callback.call(connector, {
                        text: connector.responseText,
                        xml: connector.responseXML
                    });
                }
            }
        };
    }
};
```

```markdown
## 第 2 章 ■ 面向富互联网应用的 JavaScript

```javascript
            }

        }

    }

},

// load 方法接受一个对象字面量作为参数，该对象包含要加载的 URL 以及从该 URL 下载内容后要执行的方法。

// 由于 Ajax 技术是异步的，其余代码不会等待内容下载完成再继续执行，

// 因此需要传入一个方法，在后台内容下载完成后执行。

load: function(request) {

    // 从输入的请求对象字面量中获取 url，如果不存在则使用空字符串

    var url = request.url || "";

    // 从输入的请求对象字面量中获取回调方法，如果未提供则使用空函数

    var callback = request.callback || function() {};

    // 获取跨浏览器兼容的连接对象

    var connector = this.getConnector();

    if (connector) {

        // 配置连接对象，使其在内容成功下载后执行回调方法

        this.configureConnector(connector, callback);

        // 现在真正发起对 URL 内容的请求

        connector.open("GET", url, true);

        connector.send("");

    }

},

// save 方法执行 HTTP POST 操作，实际发送内容（例如表单字段值）到服务器端脚本进行处理

save: function(request) {

    var url = request.url || "";

    var callback = request.callback || function() {};

    // data 变量是一个 URL 编码的名称-值对字符串，发送到服务器的格式如下：

    // "参数 1=值 1&参数 2=值 2&..."

    var data = request.data || "";

    var connector = this.getConnector();

    if (connector) {

        this.configureConnector(connector, callback);

        // 现在实际将数据发送到 URL 指定的脚本

        connector.open("POST", url, true);

        connector.setRequestHeader("Content- type", "application/x-www-form-urlencoded");

        connector.setRequestHeader("Content- length", data.length);

        connector.setRequestHeader("Connection", "close");

        connector.send(data);

    }

}

// 使用示例

// 将库实例化为单例

$ = new $();

// 从 Web 服务器的根目录加载 URL 为 index.html 的内容

$.Remote.load({

    url: "/index.html",

    callback: function(response) {

        // 获取文件的纯文本内容

        var text = response.text;

        // 如果 HTML 文件是以 XHTML 格式编写的，它就可以通过 `response.xml` 属性以 XML 格式获取

        var xml = response.xml;

        // 以纯文本形式输出 index.html 文件的内容

        alert(text);

    }

});

// 向位于 `process-form.php` 网址的服务端脚本发送一些数据

$.Remote.save({

    url: "/process.form.php",

    data: "name=Den&surname=Odell",

    callback: function(response) {

        // 输出服务端脚本对表单提交的响应内容

        alert(response.text);

    }

});
```

Ajax 技术与数据格式无关，因此发送和接收的数据应采用一种便于你在代码中实际操作的格式。根据经验，我们知道通过 JavaScript 解析 XML 相当缓慢，因此 JSON 提供了一种更轻量的替代方案，无论是在数据大小方面，还是在将其转换为 JavaScript 可用的有意义的原生格式的速度方面。由于 JSON 本质上只是一个 JavaScript 对象，因此将其解析为可用的对象极为简单。在大多数情况下，使用更少的字符就能以 JSON 格式表示 XML 数据。

在某些情况下，你可能希望服务器返回 HTML，这样你无需通过 JavaScript 进行解析，而是可以直接将该内容插入到 DOM 中。XHTML 为此提供了绝佳的格式。当通过 JavaScript 动态地向页面插入 HTML 时，页面上所有必要的 CSS 样式都会按预期应用到新的标记上。这意味着，当你使用 Ajax 向页面动态添加内容时，无需做任何额外工作即可实现页面所需的外观。

## 使用实用函数

许多 JavaScript 库都包含一些有用的方法，这些方法能让开发者在自己的代码中节省时间并编写更短、更高效的代码。清单 2-13 向你所在的 `$` JavaScript 库中新增了几个字符串操作方法，以及一个允许你将两个对象字面量合并的方法（该方法目前并非语言内置功能）。

**清单 2-13.** 向 `$` 库添加实用方法

```javascript
// 添加 Utils 命名空间以容纳一组有用的、可重复使用的方法

$.prototype.Utils = {

  // mergeObjects 方法将一个对象字面量的所有属性值复制到另一个对象中，

  // 替换已存在的任何属性，并添加不存在的属性

  mergeObjects: function(original, newObject) {

    for (var key in newObject) {

      // for ... in ... 循环会暴露不需要的属性，如 prototype

      // 和 constructor 等。使用 hasOwnProperty 原生方法

      // 可以确保只允许真正的属性通过

      if (newObject.hasOwnProperty(key)) {

        // 遍历新对象字面量中的每一个项，

        // 获取该项在新对象中的值，以及

        // 在原对象中的对应值（如果存在）

        var newPropertyValue = newObject[key];

        var originalPropertyValue = original[key];

      }

      // 将原对象中的值设置为新对象中的对应值，

      // 但如果属性的值是对象类型，则递归调用此方法，

      // 以便也复制该对象字面量内部的每一个值

      original[key] = (originalPropertyValue &&

        typeof newPropertyValue == 'object' &&

        typeof originalPropertyValue == 'object') ?

        this.mergeObjects(originalPropertyValue, newPropertyValue) :

        newPropertyValue;

    }

    // 返回原对象，其中所有属性都已从新对象复制过来

    return original;

  },

  // replaceText 方法接收一个包含占位符值的文本字符串，

  // 并将这些占位符替换为通过 values 对象字面量传入的实际值。

  // 例如："You have {count} messages in the {folderName} folder"

  // 每个用花括号 { } 标记的占位符都将被替换为

  // 来自 values 对象字面量的实际值，本例中会查找 count 和 folderName 属性

  replaceText: function(text, values) {

    for (var key in values) {

      if (values.hasOwnProperty(key)) {

        // 遍历值对象字面量中的所有属性

        // ... (后续代码)
```

# JavaScript 工具库与 CSS 样式处理

## JavaScript 工具库实现

```javascript
if (typeof values[key] == undefined) { // 防御性编程
  values[key] = "";
}

// 将文本字符串中用花括号包裹的属性名
// 替换为该属性的实际值。该正则表达式
// 确保替换多次出现的情况
text = text.replace(new RegExp("{" + key +"}", "g"), values[key]);
}

// 返回所有占位符值被替换为实际值后的文本
return text;
},

// toCamelCase 方法将连字符分隔的值转换为驼峰形式，
// 例如，margin-left 变为 marginLeft。连字符被移除，
// 并且第一个单词后的每个单词以大写字母开头
toCamelCase: function(hyphenatedValue) {
  var result = hyphenatedValue.replace(/- \D/g, function(character) {
    return character.charAt(1).toUpperCase();
  });
  return result;
},

// toHyphens 方法执行相反的转换，将驼峰
// 字符串转换为连字符分隔的字符串。
// 例如，marginLeft 变为 margin-left
toHyphens: function(camelCaseValue) {
  var result = camelCaseValue.replace(/[A- Z]/g, function(character) {
    return ('- ' + character.charAt(0).toLowerCase());
  });
  return result;
}
};
```

### 使用示例

```javascript
// 页面上的使用示例
// 将库实例化为单例
$ = new $();

// 合并两个对象字面量
var creature = {
  face: 1,
  arms: 2,
  legs: 2
};

var animal = {
  legs: 4,
  chicken: true
};

// 结果对象字面量变为...
// {
//   face: 1,
//   arms: 2,
//   legs: 4,
//   chicken: true
// }
creature = $.Utils.mergeObjects(creature, animal);

// 输出 "You have 3 messages waiting in your inbox."；
$.Utils.replaceText("You have {count} messages waiting in your {folder}.", {
  count: 3,
  folder: "inbox"
});

// 输出 "fontFamily"
alert($.Utils.toCamelCase("font-family"));

// 输出 "font-family"
alert($.Utils.toHyphens("fontFamily"));
```

## 处理 CSS 和样式

到目前为止，我们探讨了如何以跨浏览器的方式处理事件和动态加载内容。这两种技术对于构建 RIA（富互联网应用）至关重要。除此之外，大多数开发者需要能够发现应用于特定元素的样式，并能够动态添加和移除元素的 CSS 类名以改变样式。清单 2-14 展示了如何以跨浏览器的方式执行这些操作，并将它们添加到你的 `$` 库中。

**清单 2-14.** *向 $ 库添加 CSS 样式相关方法*

```javascript
// 在 $ 库中定义 CSS 命名空间以存储样式相关方法
$.prototype.CSS = {
  // getAppliedStyle 方法返回特定元素上特定 CSS 样式属性的当前值
  getAppliedStyle: function(element, styleName) {
    var style = "";
    if (window.getComputedStyle) {
      // W3C 特定方法。期望使用带连字符的样式属性
      style = element.ownerDocument.defaultView.getComputedStyle(
        element, null).getPropertyValue($.Utils.toHyphens(styleName));
    } else if (element.currentStyle) {
      // Internet Explorer 特定方法。期望使用驼峰形式的样式属性名
      style = element.currentStyle[$.Utils.toCamelCase(styleName)];
    }
    // 返回找到的样式属性的值
    return style;
  },

  // getArrayOfClassNames 方法是一个实用方法，返回分配给特定元素的所有
  // CSS 类名的数组。多个类名由空格字符分隔
  getArrayOfClassNames: function(element) {
    var classNames = [];
    if (element.className) {
      // 如果元素指定了 CSS 类，则创建一个数组
      classNames = element.className.split(' ');
    }
    return classNames;
  },

  // addClass 方法向特定元素添加给定名称的新 CSS 类
  addClass: function(element, className) {
    // 获取当前应用于元素的 CSS 类名列表
    var classNames = this.getArrayOfClassNames(element);
    // 将新的类名添加到列表中
    classNames.push(className);
    // 将列表转换为空格分隔的字符串，并赋值给元素
    element.className = classNames.join(' ');
  },

  // removeClass 方法用于移除指定元素的指定 CSS 类名
  removeClass: function(element, className) {
    var classNames = this.getArrayOfClassNames(element);
    // 创建一个新数组，用于存储最终的所有 CSS 类名
    var resultingClassNames = [];
    for (var index = 0; index < classNames.length; index++) {
      // 遍历列表中的每个类名
      if (className != classNames[index]) {
        // 如果当前类名不是要移除的类名，则将其添加到新列表中
        resultingClassNames.push(classNames[index]);
      }
    }
    // 将新列表转换为空格分隔的字符串，并赋值给元素
    element.className = resultingClassNames.join(" ");
  },

  // hasClass 方法若指定类名存在于特定元素上则返回 true，否则返回 false
  hasClass: function(element, className) {
    // 默认假定该类名未应用于该元素
    var isClassNamePresent = false;
    var classNames = this.getArrayOfClassNames(element);
    for (var index = 0; index < classNames.length; index++) {
      // 遍历该元素上的每个 CSS 类名
      if (className == classNames[index]) {
        // 如果找到指定类名，将返回值设为 true
        isClassNamePresent = true;
      }
    }
    // 根据是否找到指定类名，返回 true 或 false
    return isClassNamePresent;
  },

  // getPosition 方法返回页面元素在当前页面中左上角的 x 和 y 坐标，
  // 以及该元素的当前宽度和高度
  getPosition: function(element) {
    var x = 0, y = 0;
    var elementBackup = element;
    if (element.offsetParent) {
```

// `offsetLeft` 和 `offsetTop` 属性获取元素相对于其父节点的位置。

// 要获取相对于页面本身的位置，我们需要沿 DOM 树向上遍历，

// 每次累加偏移量，直到到达文档顶部的节点，

// 此时我们便获得了该元素在页面中的位置坐标

do {

x += element.offsetLeft;

y += element.offsetTop;

// 故意使用 `=` 来强制循环在页面层级中的下一个父节点上执行

} while (element = element.offsetParent)

}

// 返回一个对象字面量，包含元素的 `x` 和 `y` 坐标，

// 以及元素的实际宽度和高度

return {

x: x,

y: y,

height: elementBackup.offsetHeight,

width: elementBackup.offsetWidth

}

}

};

// 页面中的使用示例

// 将库实例化为单例

`$` = new `$`();

// 查找页面中的第一个 `<hr>` 元素

var horizontalRule = document.getElementsByTagName("hr")[0];

// 输出 `<hr>` 元素的当前宽度

alert(`$.CSS.getAppliedStyle`(horizontalRule, "width"));

// 为 `<hr>` 元素添加 `hide` CSS 类

`$.CSS.addClass`(horizontalRule, "hide");

// 从 `<hr>` 元素中移除 `hide` CSS 类

`$.CSS.removeClass`(horizontalRule, "hide");

// 如果 `<hr>` 元素上存在 `hide` CSS 类，则输出 `true`

alert(`$.CSS.hasClass`(horizontalRule, "hide"));

// 输出 `<hr>` 元素的 `x` 和 `y` 坐标

var position = `$.CSS.getPosition`(horizontalRule)

alert("该元素的'x'位置为 '" + position.x + "'，'y'位置为 '" + position.y +

"'。它的宽度为 '" + position.width + "'，高度为 '" + position.height + "'");

```

## 在页面中定位元素

你已经知道如何使用 `getElementById()` 和 `getElementsByTagName()` 方法来定位 DOM 中的元素，但如果你想要定位一组共享同一个 CSS 类名的标签（例如），该怎么办？一个能够提供此类 DOM 元素定位功能并可重用的方法会非常有用。清单 2-15 展示了如何执行这种元素定位，并通过将其添加到你的 `$` 库中来使其可重用。

**清单 2-15.** *向 `$` 库添加用于在 DOM 中定位元素的方法*

```javascript

// 向 `$` 库中添加一个新的 `Elements` 命名空间

`$.prototype.Elements` = {

// `getElementsByClassName` 方法返回一个 DOM 元素数组，

// 这些元素都应用了同一个给定的 CSS 类名。为提高方法速度，

// 可以提供一个可选的 `contextElement` 参数，将搜索范围限制在

// 节点层级中该元素内的子节点

`getElementsByClassName`: function(className, contextElement){

var allElements = null;

if (contextElement) {

// 获取 `contextElement` 内所有元素的数组

// `*` 通配符值表示返回所有标签

allElements = contextElement.getElementsByTagName("*");

} else {

// 如果未提供 `contextElement`，则获取所有元素的数组

allElements = document.getElementsByTagName("*");

}

var results = [];

for (var elementIndex = 0; elementIndex < allElements.length;

elementIndex++) {

// 遍历找到的每个元素

var element = allElements[elementIndex];

// 如果元素包含指定类名，则将该元素添加到

// 输出数组中

if (`$.CSS.hasClass`(element, className)) {

results.push(element);

}

}

// 返回包含指定 CSS 类名的元素列表

return results;

}

}

```

## 完成库的构建

在实际使用你的 `$` 库类之前，它必须被实例化为一个单例对象。让我们将所有代码整合到一个名为 `$.js` 的文件中，并将清单 2-16 所示的代码添加到该文件的最末尾。然后，你就可以在 HTML 页面中从外部引用此文件，从而使用其方法确保代码在不同浏览器中均可正常工作。

**清单 2-16.** *实例化 `$` 库*

```javascript

// 在文件末尾将 `$` 库实例化为单例，

// 准备在引用了 `$.js` 文件的页面上使用

`$` = new `$`();

```

在本章的剩余部分，`$` 库将作为许多代码示例的基础。你会发现，几乎所有其他 JavaScript 库都具有相同的功能，因此请随意在你的代码中使用你偏好的任何库。请务必使用本节开头解释的标准来选择能够提供最佳效果和支持的库。

### 构建 RIA

在本章中，你已经了解了将协同工作以帮助你构建自己的 RIA 的各种构建模块。

在开始 RIA 项目时，你应该定义表示应用程序中主要概念的类和对象。任何可重复的概念都应创建为类，而系统中仅出现一次的概念则应创建为对象字面量或单例。使用命名空间的概念将逻辑相关的代码分组，就像你在上一节中构建 `$` JavaScript 库时所做的那样。

### 构建应用程序结构

让我们以 Web 邮件客户端为例，提出代表整个系统的高级概念。在任何电子邮件客户端中，主要概念都是邮件消息。一条邮件消息至少应表示邮件的发送者和接收者、主题、正文以及任何文件附件。可以对邮件消息执行的操作主要包括撰写、发送和删除。

在代码中创建新电子邮件时，如果你代表用户创建消息，你可能希望能够预填充消息的值。

或者，你也可以让用户输入信息来构成消息。

你可以利用这些信息构建该类的骨架结构，如清单 2-17 所示。注意，我们提供了默认值，以防某些输入值缺失；并且，如果在实例化时没有向类传递任何输入值，则会强制触发 `compose()` 方法。

**清单 2-17.** *电子邮件消息类的骨架结构*

```javascript

var EmailMessage = function(input) {

    this.from = input.from || "";

    this.to = input.to || []; // Array of recipients

    this.subject = input.subject || "";

    this.body = input.body || "";

    this.attachments = input.attachments || []; // Array of attachment URLs

    this.compose = function() {

        // TODO: Bring up an email composition form, populating the object

        // instance with the data entered

    };

    this.send = function() {

        // TODO: Send the message data to a server-side mail sender

        // script using Ajax

    };

    this.remove = function() {

        // TODO: Delete the message

    };

    // If the input attribute is not supplied, force the compose method to fire

    if (!input) {

        this.compose();

    }

}

```

```javascript

// Example usage on a page

// Create an object representing a populated email message

var myEmailMessage = new EmailMessage({

    from: "me@denodell.com",

```

# 排版后的内容

现在你已经有了代表应用程序主要数据结构的基类。接下来，我们再引入一个类结构来表示文件夹，它可以包含一个或多个消息，以及一个或多个子文件夹。每个文件夹必须能够向自身结构中添加新消息。

## 清单 2-18. 文件夹类结构

```javascript
var Folder = function(input) {
    this.name = input.name || "";
    this.folders = input.folders || []; // Array of instances of the Folder class
    this.messages = input.messages || []; // Array of EmailMessage instances
    this.addMessage = function(message) {
        this.messages.push(message);
    };
    this.removeMessage = function(message) {
        // TODO: Remove the message from the folder
    };
    this.listMessages = function() {
        // TODO: Display a list of the messages in this folder
    };
}
```

```javascript
// Example usage on a page
// Create a new Inbox folder
var inbox = new Folder({
    name: "Inbox"
});

// Create a new message
var myEmailMessage = new EmailMessage({
    from: "me@denodell.com",
    to: ["test@denodell.com"],
    subject: "Test message.",
    body: "This is a test message. Please ignore."
});

// Add the new message to the inbox folder
inbox.addMessage(myEmailMessage);
```

此外，你需要一个单例来表示整个 Web 邮件客户端应用。这个单例应包含顶级文件夹列表，能够加载存储在 Web 服务器上的文件夹和消息，并能够使用这些数据初始化应用。

## 清单 2-19. 表示 Web 邮件客户端应用的单例

```javascript
var WebMail = new function() {
    this.folders = [];
    this.loadFolders = function() {
        // TODO: Load list of folders via Ajax from the server and populate Folder
        // object instances from this information
    };
    this.loadMessagesIntoFolders = function() {
        // TODO: Load list of messages via Ajax from the server, create EmailMessage
        // object instances and populate the folder instances with these messages
    };
    // Initialize the data, loading messages into folders, ready for use
    this.folders = this.loadFolders();
    this.loadMessagesIntoFolders();
    // Initialize the user interface. When the user clicks the create-mail-button
};
```

```javascript
// element, a new EmailMessage is created, which launches the new mail
// composition form automatically
$.Events.add(document.getElementById("create-mail-button"), "click", function() {
    new EmailMessage();
});
```

# 第 2 章 ■ 用于丰富互联网应用程序的 JavaScript

到目前为止，你已经定义了在网页邮件系统中存储文件夹和邮件的结构，这些本质上是应用程序中与数据相关的组件。但网页邮件客户端需要允许用户通过直观的界面，在浏览器中创建和管理这些结构。你需要使用 HTML 和 CSS 构建该界面，然后利用 JavaScript 监听用户与页面组件的交互，并根据执行的操作运行相应的代码。请记住，当浏览器禁用 JavaScript 时，首先要确保应用程序能够正常运行，因此在添加任何 JavaScript 代码之前，务必将这一部分落实到位。

例如，如果用户点击“创建邮件”按钮，你可以执行代码来创建 `EmailMessage` 类的一个新实例，该实例会在页面上显示表单，以便用户创建新邮件。你的代码应按如下方式进行管理：

- 所有与电子邮件特定数据的数据存储和用户界面相关的代码，应归入 `EmailMessage` 类中处理。
- 所有与文件夹特定数据的数据和用户界面相关的代码，应归入 `Folder` 类中处理。
- 所有与应用程序本身的用户界面相关的代码，应存储在代表整个应用程序的单例中。

通过这种分离方式，你会发现随着代码库的增长，维护起来会更容易。当问题出现时，你也能快速定位其根源。

## 管理两套 HTML

你需要构建在 JavaScript 不可用的情况下仍能正常运行的 RIA（丰富互联网应用程序）HTML。那么，你的用户界面如何动态呈现存储在单独文件中的表单及其他 HTML 代码呢？有两种方法可供你实现这一目标。

- **维护两套 HTML 代码**：一套用于纯 HTML 应用程序，另一套用于 JavaScript 应用程序。你可以将 JavaScript 应用程序的 HTML 代码直接存储在代码中，既可以用纯文本形式，也可以利用 DOM 动态创建。

- **使用服务器端脚本**：利用服务器端脚本，你可以维护一个包含每个页面或组件 HTML 代码的单一包含文件。在纯 HTML 应用程序中，你引用一个包含页眉、页脚、导航以及围绕包含文件 HTML 的其他页面元素的页面。而在 JavaScript 代码中，你则使用 Ajax 技术仅动态加载该包含文件，从而获得你所需的精确 HTML 代码。然后，你可以利用 DOM 将这些代码插入到页面的指定位置。

**101**

到目前为止，我们探讨了如何遵循一种特定的软件架构设计模式（即代码组织技术）来构建 RIA：使用单例模式将对象和类整合在一起，以创建一个功能极其强大且可扩展的用户界面。现在，让我们看看其他几种模式。

## 使用设计模式

除了单例模式，还有两种模式可用于构建和整合你的代码：模型-视图-控制器模式和观察者模式。

### 模型-视图-控制器模式

模型-视图-控制器（MVC）软件架构模式将应用程序划分为三个逻辑上独立的组：

- **模型**，包含与数据存储和操作相关的代码
- **视图**，包含与构建和管理用户界面相关的代码
- **控制器**，充当桥梁，包含通过事件驱动机制将模型和视图连接起来的代码

控制器将模型和视图连接在一起，而模型和视图彼此无关——完全独立。图 2-1 展示了 MVC 软件模式的逻辑划分。

图 2-1. MVC 代码分离模式

`代码清单 2-20` 展示了采用 MVC 模式的邮件客户端应用示例。

**代码清单 2-20.** 使用 MVC 软件模式的网页邮件客户端应用

```javascript
var WebMail = new function() {

// 模型仅包含存储和处理应用原始数据所需的代码。
// 因此，我们的 EmailMessage 和 Folder 类，以及整个应用的其他数据存储和
// 处理代码都在此定义。

var Model = function() {
    this.EmailMessage = function(input) {
        // TODO: 在此处插入 EmailMessage 类的定义代码
    };
    this.Folder = function(input) {
        // TODO: 在此处插入 Folder 类的定义代码
    };
    this.folders = [];
    this.getMessages = function(folderName) {
        // TODO: 返回指定文件夹内的消息列表
    }
    this.composeMessage = function() {}
    this.sendMessage = function(messageId) {}
    this.deleteMessage = function(messageId) {
        // TODO: 插入删除指定 messageId 的消息的代码
    }
};

// 视图仅包含构建用户界面和绑定按钮事件所需的代码——
// 但它不指定事件发生时执行哪些代码，这由控制器处理
var View = function() {
    // 定义特定事件发生时要执行的方法——方法代码从控制器传入
    this.onComposeButtonClick = function() {};
    this.onSendButtonClick = function() {};
    this.onDeleteMessageButtonClick = function() {};

    var self = this;
    // 绑定页面上的 HTML 按钮事件
    $.Events.add(document.getElementById("compose-button"), "click",
        this.onComposeButtonClick);
    $.Events.add(document.getElementById("delete-message-2"), "click",
        function(e) {
            // 传递消息标识参数，默认值为 2
            self.onDeleteMessageButtonClick(e, 2);
        });

    // 添加用户界面特有方法
    this.showComposeMailForm = function() {
        // TODO: 显示撰写新邮件的表单
        // 绑定一个新按钮，用于发送新邮件
        $.Events.add(document.getElementById("send-button"), "click",
            function(e) {
                // 传递额外的消息标识参数，默认值为 2
                self.onSendButtonClick(e, 2);
            });
    };
    this.updateMessageList = function(messages) {
        // TODO: 添加代码从页面移除撰写邮件的表单
    }
    this.updateMessageList(messages) {
        // TODO: 显示消息列表
    }
}

// 控制器包含基于事件的高级行为代码，
// 连接模型和视图以构建完整的应用
var Controller = function(model, view) {
    var composeEmail = function(e) {
        // 阻止默认的按钮点击事件发生——即针对 HTML 版本的站点，我们在此覆盖
        e.preventDefault();
        view.showComposeMailForm();
    };
    var sendEmail = function(e, message) {
        e.preventDefault();
        var messageId = new model.EmailMessage(message);
        model.sendMessage(messageId);
        view.hideComposeMailForm();
    };
    var deleteMessage = function(e, messageId) {
        e.preventDefault();
        model.deleteMessage();
        view.updateMessageList(model.getMessages("Inbox"));
    };

    // 将视图中实例化的事件连接到实际代码
    view.onComposeButtonClick = composeEmail;
    view.onSendButtonClick = sendEmail;
    view.onDeleteMessageButtonClick = deleteMessage;
}

// 将整个 MVC 结构组装在一起
new Controller(new Model(), new View());
}();
```

**104**

不必被这段代码吓到。起初它可能看起来很复杂，但一旦你通过 `MVC` 模式积累了构建代码的实践经验，就会很快理解其工作原理并发现其固有的可扩展性。

## 观察者模式

观察者模式非常强大且高度可扩展。我个人认为，在我自己的项目中，它的实用性与 `MVC` 模式不相上下，甚至有过之而无不及。

`Observer` 模式依赖于两个概念：广播事件（*发布*和*触发*也是常用术语），以及监听事件（*订阅*也是常用术语）。代码被组织成逻辑块，每个逻辑块触发自己的事件，并监听整个应用程序中触发的其他事件。没有任何两个代码块是相互连接或了解彼此的，这与 `MVC` 模式（其中控制器了解模型和视图）有显著区别。

图 2-2 展示了 `Observer` 模式。在这个例子中，有两个代码块，彼此都不知道对方的存在，它们在一个共享的事件空间中触发和监听事件。理论上，可以有无限数量的代码块共享同一个（或多个）事件空间。

```text
触发      触发
代码      代码
事件      事件
块        块
监听      监听
块
```

**图 2-2.** *具有两个不同代码块和一个事件空间的 `Observer` 模式*

在代码中，你可以通过创建一个新类来实现 `Observer` 模式，这个类存储一个事件列表，并包含监听该列表中特定事件以及触发该列表中特定事件的方法。你可能希望在应用程序的不同位置重用这个模式。清单 2-21 展示了 `Observer` 模式被定义为一个可以在你的应用程序中使用的类。将这个清单添加到你在本章前面创建的 `$` 库代码中，放在文件末尾之前（它被声明为单例的地方）。

**清单 2-21.** *一个表示 `Observer` 模式的类*

```javascript
$.prototype.Observer = function() {

  // 创建一个数组来存储事件
  this.events = [];

  // listen 方法监听一个特定名称的事件的触发，
  // 并在事件触发时执行一个分配的方法
  this.listen = function(eventName, method) {
    if (typeof method == "function") {
      if (!this.events[eventName]) {
        this.events[eventName] = []
      }
      this.events[eventName].push(method);
    }
  };

  // fire 方法触发一个特定名称的事件，依次执行所有与该事件关联的方法，
  // 并传入随触发请求一起发送的任何可选参数
  this.fire = function(eventName, params, scope) {
    scope = scope || window;
    for (var methodIndex = 0; methodIndex < this.events[eventName].length; ➥
methodIndex++) {
      this.events[eventName][methodIndex].call(scope, params);
    }
  }

}
```

清单 2-22 展示了在示例 Web 邮件客户端应用程序中使用 `Observer` 模式类的代码。

**清单 2-22.** *使用 `Observer` 模式的 Web 邮件客户端应用程序*

```javascript
// 稍后我们将继承 Observer 模式，将 listen 和 fire 方法引入到 WebMail 构造函数中
var WebMail = function() {

  // 定义一个对象字面量常量来表示应用程序中允许的不同事件的列表
  this.EVENT = {
    UI_READY: 0,
    MESSAGES_LOADED: 1,
    DELETE_MESSAGE: 2
  }

  // 定义一个名为 Data 的代码块，用于容纳所有数据存储和操作
  // 属性及方法。该代码块不了解其他任何代码块，
  // 它只知道 WebMail 应用程序的 EVENT 列表，并能监听和触发该应用程序的事件，
  // 应用程序作为`app`输入变量传入代码块。
  var Data = new function(app) {

    var EmailMessage = function(){
      // TODO: 定义 EmailMessage 类
    };

    var Folder = function(){
      // TODO: 定义 Folder 类
    };

    // 当 UI_READY 事件被触发时，执行附加的函数
    app.listen(app.EVENT.UI_READY, function() {
      // TODO: 插入通过 Ajax 从服务器加载消息到 messages 变量的代码
      var messages = [];

      // 通知任何监听 MESSAGES_LOADED 事件的代码块，
      // 消息已从服务器加载完毕，同时传递消息列表
      app.fire(app.EVENT.MESSAGES_LOADED, messages);
    });

    // 当 DELETE_MESSAGE 事件被触发时，执行该函数
    app.listen(app.EVENT.DELETE_MESSAGE, function(messageId) {
```

# JavaScript设计模式与测试

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

Yahoo! 发布了一个非常有用的 JavaScript 测试框架，名为 `YUI Test`（[`developer.yahoo.com/yui/yuitest/`](http://developer.yahoo.com/yui/yuitest/)）。`YUI Test` 是 Yahoo! UI JavaScript 库的一部分，但如果您使用其他 JavaScript 库，它也能很好地工作。它允许您编写和运行有效的测试用例，并在错误发生时检测并记录它们。测试用例可以分组到更大的测试套件中，以提供一次运行多个测试的简单方法。在我使用过的其他测试框架中，这个框架的独特之处在于它支持测试异步方法调用，这使您能够测试 Ajax 回调方法。它还在大多数常用浏览器中提供了 DOM 事件模拟。

要使用 `YUI Test`，首先创建一个引用 `YUI Test` JavaScript 文件和您要测试的 JavaScript 代码的测试页面。然后，在页面的 `<script>` 标签内，多次调用您代码中的一个函数，并传入不同的输入。您告诉测试框架每个输入对应的预期输出，框架会记录函数调用的实际输出是否与您的预测匹配。任何与预测不匹配的输出都会被标记为错误。清单 2-23 展示了一个为与 `YUI Test` 框架一起运行而编写的简单测试套件。

■**注意** 编写测试用例时，请确保测试“边缘”情况——那些在典型使用中可能不会出现，但在特殊情况下可能发生的情况，例如当需要输入数据但数据缺失时。

**清单 2-23.** *使用 `YUI Test` 的基本测试套件 HTML 页面*

```markdown
# 测试套件示例与 YUI Test

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

## 使用第三方脚本

重用已经编写好的代码来解决您作为 RIA 开发者面临的问题是非常明智的。您在 JavaScript 库中已经看到了这一点。同样，允许他人重用您编写的代码也是对 Web 开发社区的回馈。

以下是在应用程序中使用第三方脚本的一些指南：

- 检查代码是否已使用测试框架（例如我们在上一节中介绍的 YUI Test 框架）进行了彻底测试。

- 确保脚本支持您应用程序所支持的所有浏览器，这样您就不必在之后用其他脚本来替换它，以提供所需的所有支持。

- 将所有第三方脚本放在一个单独的文件夹中，与您自己的脚本分开。将它们视为您应用程序的插件，而不是核心代码本身。

在本书的其余部分，我将向您介绍几个第三方脚本，当您尝试为 RIA 添加额外功能时，它们应该会对您有所帮助。

## 总结

在本章中，您学习了如何使用面向对象编程原则和软件架构设计模式在 JavaScript 中构建 RIA。本章涵盖了如何应用清晰且可读的编码风格，以及如何以稳健的方式使用 JavaScript 语言。您已经看到第三方库和框架如何让您将代码构建在坚实的跨浏览器基础之上。您还学习了如何全面测试代码，以确保应用程序中不存在错误。
```

# 至此，本书第一部分已告一段落。现在，你已经准备好开始构建自己的富互联网应用（RIA）。运用本部分涵盖的原则，你将创建出稳健、易用的应用程序，任何使用任意设备上任意网页浏览器的用户都能访问。在本书的下一部分，你将学习如何提升网页应用的性能与响应速度，从而为最终用户提供最佳体验。
