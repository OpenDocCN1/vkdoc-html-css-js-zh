# 5. 模块

JavaScript 在其早期阶段，最初是作为 Web 浏览器的一种简单脚本语言开始的。因此，它最初是建立在“一切都是共享的”方法之上的。但随着时间的推移，随着语言的发展以及 JavaScript 应用程序变得越来越复杂，这种加载代码的方法开始变得更容易出错且令人困惑。这与大多数高级语言使用如模块和包等概念来帮助定义代码作用域的事实形成对比。在 `ES6` 之前，JavaScript 应用程序中的所有内容，包括应用程序不同文件中的代码，都共享相同的作用域。这正是模块发挥作用的地方，它们有助于更好地管理和引入代码分离，减少命名冲突等问题并提高数据安全性。JavaScript 开发者被迫求助于外部库来引入模块以帮助解决这些特定问题，但有了 `ES6`，模块现在已成为该语言的官方组成部分。

## 模块系统及一点历史

过去，JavaScript 有两个主要的模块系统：异步模块定义 (AMD) 和 `CommonJS`。`CommonJS` 是 `Node.js` 中常用实现的模块标准。它具有简洁的语法，专为同步加载和服务器设计。另一方面，`AMD` 是 `RequireJS` 中的流行实现，其语法稍显复杂，允许它在没有 `eval()` 或编译步骤的情况下工作。它专为异步加载和浏览器设计。两者在浏览器中都不是原生可用的。

当 TC39 决定在 ES6 中实现模块规范时，他们没有采用已有的两种方案，而是提出了一种新的实现方法。他们的目标是同时满足 CommonJS 和 AMD 的用户。这种新方法建立在两者之上，并试图解决开发者在这两种实现中经常遇到的许多问题。

ES6 模块提供了来自 AMD 和 CommonJS 的特性。它们使用紧凑的语法和静态模块结构，有助于代码优化、静态检查和消除死代码。它还像 CommonJS 一样支持循环依赖，并像 AMD 一样支持异步加载和可配置的模块加载。

终结 CommonJS 和 AMD 之间的碎片化，并拥有一个统一的本地模块标准，带来了额外的好处，例如废弃了传统上用于使同一文件能在不同系统中使用的 UMD（通用模块定义）模式。

### 传统 JavaScript 中的模块模式

为了帮助我们更好地理解模块，在深入探讨 ES6 模块之前，我们将使用传统 JavaScript 实现模块模式。简单来说，模块是一个包含内部变量和函数的函数，它可以通过返回一个“公共 API”来暴露给外部环境，该 API 通过拥有数据闭包的方法来访问函数内的数据。为了说明这一点，我们使用模块模式编写一个函数：

```javascript
function Message(text) {
    function printMessage() {
        console.log("This is the message: " + text + "!");
    }
    return {
        printMessage: printMessage
    };
}
var printer = Message("test");
printer.printMessage();          // This is the message: test!
```

在此示例中，变量 `printer` 被赋值为带有参数 `test` 的函数 `Message`。`Message` 是实现模块模式的函数，它为 `printer` 提供了一个公共 API，该 API 可以访问其内部函数 `printMessage`，作为打印 `text` 参数的方法。

## ES6 中的模块

ES6 中的模块使得将代码划分和分离成更小的独立片段变得更加容易，这些片段可以在需要时被重用并注入到其他地方，从而使测试代码的过程更加容易，因为模块与主代码解耦。模块在异步加载脚本方面也很有用，可以提高应用的加载时间。但模块系统对 JavaScript 来说并不新鲜。

模块就是被从其他文件加载到 JavaScript 代码中的 JavaScript 文件。但理解这与脚本的加载方式有何不同非常重要。如果我们将 JavaScript 模块的语义与传统脚本加载进行比较，会发现一些关键区别。首先，模块始终在严格模式下运行，而传统脚本则可以更改其模式。如果你查看模块的顶级作用域，`this` 的值是 `undefined`，并且在顶层创建的变量不会出现在共享的全局作用域中。你需要从模块内部获取的任何东西都必须显式地导出，我们才能在模块外部使用它们。现在，让我们通过理解导入和导出绑定的能力，来了解模块的核心价值。

### 导出

`export` 关键字可用于将模块内部的代码部分暴露给其他模块。你可以从模块中导出一个变量、一个函数或一个类声明。未从模块导出的变量、函数或类在模块外部不可访问。让我们看几个例子：

```javascript
export var text = "ES6 is awesome";
export let name = "Ian Murawski";
export const number = 7;
```

在此示例中，你正在导出 `text`、`name` 和 `number`，它们使用不同的变量声明关键字声明。你也可以从模块中导出一个函数或类。考虑以下示例：

```javascript
export function add(a, b) {
    return a + b;
}
export class Rectangle {
    constructor(length, width) {
        this.length = length;
        this.width = width;
    }
}
```

在这段代码片段中，我们正在导出函数 `add` 和类 `Rectangle`。你也可以导出一个模块私有的现有函数。

```javascript
function multiply(a, b) {
    return a * b;
}
export { multiply };
```

从这个例子中，我们可以看到不仅可以导出一个函数声明——而且还可以导出一个对模块私有函数的引用。

此时，理解这一点很重要：只有带有名称的函数或类声明才能以这种方式导出，所有这些导出都被称为命名导出。你不能从模块中导出匿名函数或类，除非它们被标记为使用 `default` 关键字指定的 `default` 导出。你可以在一个模块中有多个命名导出，但只能有一个默认导出。同时使用两者是可能的，但通常最佳实践是将它们分开。

### 默认导出

在 `Node.js` 中，模块只导出单个值是常见的做法。即使在前端 JavaScript 中，当我们为模型和组件使用类时，每个模块一个类也是常见的做法。ES6 模块被设计为选取一个默认导出，作为主要的导出值。

可以使用 `default` 关键字将单个变量、函数或类指定为模块的默认导出。在一个模块中，你只能有一个值作为默认导出。在模块内对多个导出使用该关键字会引发错误。默认导出有助于简化从模块导入导出的语法。

```javascript
export default function(a, b) {
    return a * b;
}
```

在此示例中，该函数作为其默认值从模块导出。你可以注意到该函数如何不需要名称，因为它是默认导出，并且模块本身代表该函数，允许你省略函数的名称。你也可以为其分配一个名称，并使用 `default` 导出函数名称。考虑以下示例：

```javascript
function multiply(a, b) {
    return a * b;
}
export default multiply;
```

### 导入

一旦设置了包含相关导出的模块，就可以通过使用 `import` 关键字在另一个模块内访问它。`import` 语句包含两部分：你要导入的标识符以及应从哪里导入这些标识符的模块。这是其基本形式的语句：

```javascript
import { identifier1, identifier2 } from "./moduleFile.js";
```

在上面的语句中，`identifier1` 和 `identifier2` 是从 `moduleFile.js` 导入的绑定。

模块通过 `from` 关键字后一个表示包含模块的文件路径的字符串来指定。请注意，绑定列表看起来像一个解构对象，但请记住它不是。此外，绑定类似于使用 `const` 定义的变量，也就是说，你不能使用相同的名称定义任何其他变量，也不能导入另一个具有相同名称的模块。

使用上述语法，你可以从模块中导入特定的绑定，如下所示：

```javascript
// 导入函数 sum 和 multiply
import { sum, multiply } from "./ moduleFile.js";
console.log(sum(1, 7));                // 8
console.log(multiply(2, 3));           // 6
```

这里，有两个绑定从 `moduleFile` 模块导入：`sum` 和 `multiply` 可以像本地定义的标识符一样使用。但让我们考虑一种情况，即你想要导入模块中所有可用的导出，而无需显式地按名称声明它们。这可以通过使用通配符 `*` 并将导入的标识符作为对象的属性来实现。

```
// 导入所有内容
import * as example from "./ moduleFile.js";
console.log(example.sum(1,7));          // 8
console.log(example.multiply(2, 3));    // 6
```

在这段代码中，`moduleFile` 中所有导出的绑定都被加载到一个名为 `example` 的对象中。

# JavaScript 模块指南

## 引言

模块中的所有导出都将作为已声明对象的属性被访问。这会创建一个新的命名空间，因为该对象并不存在于实际模块中。

需要注意的是，无论模块在导入语句中被导入多少次，它都只会被执行一次。当导入被执行时，该模块会被存储在内存中，并供所有后续导入重用。存储该模块的文件只会被执行一次。

### 导入默认值

从模块中导入默认值非常简单：

```javascript
import multiply from "./moduleFile.js";
```

该导入语句从模块中导入了默认值。请注意，与导入命名导出不同，这里我们没有使用任何花括号。此例中的名称 `multiply` 是本地的，用于引用从模块中导入的默认值。

对于同时导出默认值和非默认值的模块，你可以使用一条语句来导入所有绑定：

```javascript
export let message = "42 is the answer to the everything.";
export default function(a, b) {
return a * b;
}
import multiply, { message } from "./moduleFile.js";
console.log(multiply (21, 2));    // 42
console.log(message);             // "42 is the answer to the everything."
```

你需要使用逗号来分隔默认的本地名称和花括号内列出的非默认标识符。请确保始终将默认值放在非默认值之前。

另一种为默认模块指定特定本地名称的导入方式是：

```javascript
import { default as multiply, message } from "./moduleFile.js";
```

在此例中，与上一个示例类似，`multiply` 存储了从 `moduleFile.js` 导出的默认模块。我们将在本章后续部分讨论使用 `as` 关键字重命名标识符。

### 导出已导入的绑定

如果你需要重新导出已导入到模块中的绑定，可以使用导出语句：

```javascript
import { message } from "./moduleFile.js";
// 一些 JavaScript 代码
export message;
```

在此示例中，我们从 `moduleFile.js` 导入了绑定 `message`，并再次导出了相同的绑定。你也可以使用一条语句而不是两条来完成此操作，如下所示：

```javascript
export { multiply } from "./moduleFile.js";
```

此外，你可以使用通配符 `*` 重新导出所有内容，如下例所示：

```javascript
export * from "./moduleFile.js";
```

在上述示例中，来自 `moduleFile.js` 的所有内容（包括默认值）都会从当前模块中导出。

### 重命名标识符

在许多情况下，你可能不想使用导入的变量、函数或类的原始名称，因为它可能与代码中的其他声明发生冲突。ES6 通过允许你使用 `as` 关键字重命名导出和导入来帮助你解决此问题：

```javascript
function sum(a, b) {
return a + b;
}
export { sum as add };
```

在此示例中，`sum` 是导出函数的本地名称。但该函数被导出为 `add`，需要在另一个模块中使用其导出的名称 `add` 来导入：

```javascript
import { add } from "./moduleFIle.js";
```

同样，如果你想在导入标识符时使用不同的名称，可以这样做：

```javascript
import { add as sum } from "./moduleFile.js";
console.log(typeof add);            // "undefined"
console.log(sum(1, 2));             // 3
```

在这里，导出名称 `add` 被作为 `sum` 导入。因此，从技术上讲，在代码的其余部分没有名为 `add` 的标识符可供使用，它只能被引用为 `sum`。

你还可以将标识符重命名为 `default` 以使其成为默认导出：

```javascript
function multiply(a, b) {
return a * b;
}
export { multiply as default };
```

类似地，正如我们之前讨论过的，你也可以在重命名后导入默认值：

```javascript
import { default as multiply, message } from "moduleFile.js";
console.log(multiply(1, 2));      // 2
console.log(message);             // "42 is the answer to the everything."
```

由于 `default` 是 JavaScript 中的关键字，因此它不能用作变量、函数或类名。因此，使用 `default` 重命名导出可以被视为一种特殊情况，以与定义非默认导出的方式保持一致。如果你想使用单条导出语句同时指定多个导出（包括默认值），这种语法会非常有用。

### 加载模块

ES6 并未具体定义如何加载模块。这是因为模块规范的设计目标之一就是保持与实现环境的无关性。ES6 没有创建一种适用于所有 JavaScript 环境的规范，而只是指定了语法。浏览器和 Node.js 需要决定如何实现 `HostResolveImportedModule`，这是 ES6 抽象出加载机制的内部操作。

### Web 浏览器中的模块

在 Web 应用程序中使用 JavaScript 的方法有很多，但将 JavaScript 插入 HTML 的主要方式是通过 `<script>` 标签。`<script>` 标签有一个 `src` 属性，用于告知应用程序脚本位于特定位置。另一种方式是直接将代码包含在标签内部。你也可以使用 workers 来加载 JavaScript。让我们在模块的上下文中逐一探讨这些机制。

#### 使用 `<script>` 加载模块

默认情况下，`<script>` 元素将 JavaScript 文件作为脚本加载。这等同于显式地将 `type` 属性设置为内容类型 `text/javascript`。为了支持模块，`module` 值被添加到了类型选项中。例如：

```javascript
import { multiply } from "./moduleFile.js";
let result = multiply(1, 2);
```

与脚本不同，模块使用 `import` 来指定为了正确执行而需要加载的其他文件。为了使其功能正常工作，`script` 元素的模块类型始终表现得如同应用了 `defer` 属性一样。`defer` 属性对于加载脚本文件是可选的，但对于加载模块文件始终是应用的。一旦 HTML 解析器遇到 `<script type="module">` 元素，模块文件就会开始被下载。因此，所有模块都按照它们出现的顺序执行。

#### 在浏览器中异步加载模块

你可能已经在 HTML 中使用了 `<script>` 标签的 `async` 属性。当你在 script 标签中使用此属性时，它让浏览器知道脚本在获取并可用后需要立即执行。因此，异步脚本的执行顺序不是同步的。它们一旦可用就会立即执行，无论它们被声明的顺序如何。对于模块，你可以使用相同的异步执行过程。唯一需要记住的一点是，在模块可以执行之前，需要先获取导入资源。

### 常见陷阱

模块在分隔代码和创建可重用的逻辑组件方面非常有用。但是，为了有效使用它们，你需要了解一些陷阱。在本节中，我们将详细讨论这些陷阱。

#### 语法

请记住，`export` 和 `import` 都只能在顶层使用，不能成为其他语句或函数的一部分。例如，以下代码将抛出语法错误：

```javascript
if (condition) {
export condition;
// SyntaxError: 'import' and 'export' may only appear at the top level
}
```

这里，导出语句是 `if` 语句的一部分，这会抛出错误。导出和导入不能以任何方式被条件性地或动态地执行。在 `try-catch` 块中使用 import-export 语句时也是如此。`export` 和 `import` 关键字被特意设计为静态的，以便文本编辑器能够在导入特定模块时轻松地提供该模块中可用的信息。

#### 只读绑定

# ES6 模块

ES6 的 `import` 语句只会创建指向相应变量、函数或类的`只读绑定`。因此，导入这些绑定的模块无法修改其值。但导出标识符的模块可以对其进行修改。例如：

```javascript
// moduleFile.js 从这里开始
export var message = "模块文件中的消息";
export function setMessage(newText) {
    message = newText;
}
// moduleFile.js 在这里结束

// file.js
import { message, setMessage } from "./moduleFile.js"
console.log(message);          // "模块文件中的消息"
setMessage("新消息");
console.log(message);          // "新消息"
message = "这是另一条消息";      // 报错
```

在上述示例中，我们从模块中导入了两个绑定：`message` 和 `setMessage`。函数 `setMessage()` 可以更改变量 `message` 的值，但如果你尝试直接更改该值，则会抛出错误。

### 解构导入语句与使用变量

尽管 `import` 语句看起来像是解构，但事实并非如此。因此，以下代码是无效的，将会抛出错误：

```javascript
import { toto: { tata } } from 'someModule'; // 报错
```

你还需要牢记一点：`import` 语句不能依赖于任何需要在运行时计算的内容。因此，类似下面的示例也会抛出错误：

```javascript
import toto from "someModule" + tata; // 报错
```

因此，在导入模块时，请确保不要使用任何此类复杂的机制。

## 使用 `eval()`

你不能在模块上使用 `eval()`，因为模块对于 `eval()` 来说是一种高级结构。`eval()` 通常接受不允许使用 `import` 或 `export` 的脚本。

## 模块标识符

在上述所有示例中，我们在模块标识符中使用了相对路径，例如 `"./moduleFile.js"`。当浏览器尝试运行此代码时，它们会按以下方式理解这些路径：

- `/` 解析为根目录
- `./` 解析为当前目录
- `../` 解析为父目录

## 总结

ES6 中的模块为扩展 JavaScript 的功能提供了新的方式。它们有助于更好地组织代码，并维护更具模块化语义的代码库。ES6 引入了新的关键字，如 `import`、`export` 和 `default`，使得模块的使用成为可能。由于 ES6 集成了两者的优点，你不再需要使用 UMD 或诸如 CommonJS 或 AMD 之类的额外模块系统。

使用模块，你可以编写可重用且封装性更好的代码。与脚本不同，模块不会修改全局作用域，从而降低了出错的可能性。如果你想在其他地方使用模块中的值，你必须专门将它们从模块中导出。
