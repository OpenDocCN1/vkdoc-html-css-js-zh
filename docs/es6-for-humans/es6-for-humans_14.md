# JavaScript 模块指南

## 引言

模块中的所有导出都将作为已声明对象的属性被访问。这会创建一个新的命名空间，因为该对象并不存在于实际模块中。

需要注意的是，无论模块在导入语句中被导入多少次，它都只会被执行一次。当导入被执行时，该模块会被存储在内存中，并供所有后续导入重用。存储该模块的文件只会被执行一次。

#### 导入默认值

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

#### 导出已导入的绑定

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

#### 重命名标识符

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

#### 加载模块

ES6 并未具体定义如何加载模块。这是因为模块规范的设计目标之一就是保持与实现环境的无关性。ES6 没有创建一种适用于所有 JavaScript 环境的规范，而只是指定了语法。浏览器和 Node.js 需要决定如何实现 `HostResolveImportedModule`，这是 ES6 抽象出加载机制的内部操作。

#### Web 浏览器中的模块

在 Web 应用程序中使用 JavaScript 的方法有很多，但将 JavaScript 插入 HTML 的主要方式是通过 `<script>` 标签。`<script>` 标签有一个 `src` 属性，用于告知应用程序脚本位于特定位置。另一种方式是直接将代码包含在标签内部。你也可以使用 workers 来加载 JavaScript。让我们在模块的上下文中逐一探讨这些机制。

### 使用 `<script>` 加载模块

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



