# 附录 2：TypeScript 编译器

TypeScript 编译器可能隐藏在你的开发工具背后，但熟悉各种编译器选项仍然非常值得。由于现在有超过 70 个不同的编译器标志，我选择了提供关于更常见选项的更详细信息。编译器本身是为编译整个项目而非单个文件而优化的。针对整个项目运行编译器意味着你只需加载标准库一次，而不是多次。你可能会发现，编译单个文件（包括其所有依赖项）所需的时间几乎与编译整个项目一样长。

编译器名为`tsc.exe`，通常可以在 Windows 上的以下目录中找到：

- `C:\Program Files (x86)\Microsoft SDKs\TypeScript\[Version]\tsc.exe`

要运行编译器，只需在命令行中调用`tsc`并传入程序的根文件名，如下所示。你可能需要输入`tsc.exe`文件的完整地址。如果你厌倦了输入完整路径，可以将 TypeScript 文件夹的路径添加到你的环境变量中。

```sh
tsc app.ts
```

你也可以在任何操作系统上使用 Node 运行 TypeScript 编译器，因为编译器是用 TypeScript 编写的，并编译成了 JavaScript，如下所示。你需要输入`tsc.js`文件（而不是`.ts`文件）的完整路径。

```sh
node tsc.js app.ts
```

## 获取帮助

如果你在记住编译器的所有选项时遇到困难，可以使用以下命令之一获取帮助，这些命令将显示所有可设置的编译器标志的列表。每个编译器标志都暴露了一个设置，允许你更改编译器的行为。

```sh
tsc --help
node tsc.js
```

## 示例文件

为了说明编译器选项之间的一些关键区别，列表 A2-1 中的代码被用于使用一些不同的编译器选项生成输出。

```typescript
import * as Dependency from './module';

export class Example extends Dependency.BaseClass {
    exampleMethod(): number {
        return 5;
    }
}

// 注释
let example = new Example();
const val = example.exampleMethod();
```

**列表 A2-1. 示例 TypeScript 代码**

通过检查此列表生成的 JavaScript，你将了解模块加载和降级编译方面的一些差异。降级编译允许你使用 TypeScript 的最新特性，并将它们转换为与旧版 ECMAScript 规范兼容的 JavaScript。当输出基本相似时，我会删除相似的行，以使重要的差异更加突出。

## 常用标志

有一些编译器标志非常常见，你几乎肯定会在某个时候用到它们。这些标志将在以下章节中描述。

### 模块种类

`module` 编译器标志用于生成加载外部模块的代码，这些模块使用 CommonJS 或 AMD 模块模式。模块类型的有效值为 `commonjs` 和 `amd`。

```sh
tsc --module UMD app.ts
```

通用模块定义（UMD）创建的代码可以在两种主流的模块风格中运行：异步模块定义（AMD）和 CommonJS。例如，这允许你编写既能在 Web 浏览器中由 RequireJS 加载，又能在服务器端由 Node 加载的代码。这种模式的输出比 AMD 或 CommonJS 都要冗长，因为它包含了用于特性检测以使用正确加载风格的代码。

```javascript
(function (factory) {
    if (typeof module === "object" && typeof module.exports === "object") {
        var v = factory(require, exports);
        if (v !== undefined) module.exports = v;
    }
    else if (typeof define === "function" && define.amd) {
        define(["require", "exports", "./module"], factory);
    }
})(function (require, exports) {
    "use strict";
    exports.__esModule = true;
    var Dependency = require("./module");
    // Removed for brevity
    exports.Example = Example;
    // Comment
    var example = new Example();
    var val = example.exampleMethod();
});
```

*列表示例 A2-2. UMD 模块*

AMD 模块的输出在示例 A2-3 中更短，因为它不需要执行特性检测。该代码将与模块加载器（如 RequireJS 和 Dojo Toolkit）兼容。

```javascript
define(["require", "exports", "./module"], function (require, exports, Dependency) {
    "use strict";
    exports.__esModule = true;
    // Removed for brevity
    exports.Example = Example;
    // Comment
    var example = new Example();
    var val = example.exampleMethod();
});
```

*列表示例 A2-3. AMD 模块*

CommonJS 模块最著名的实现是 Node，但它们在 MongoDB 和 CouchDB 中也能找到。示例 A2-4 展示了使用 CommonJS 模块标志的输出。

```javascript
"use strict";
exports.__esModule = true;
var Dependency = require("./module");
// Removed for brevity
exports.Example = Example;
// Comment
var example = new Example();
var val = example.exampleMethod();
```

*列表示例 A2-4. CommonJS 模块*

一个新兴且值得关注的模块风格是 ECMAScript 模块语法，如示例 A2-5 所示。这种模块加载风格与原始的 TypeScript 代码最为相似。

```typescript
import * as Dependency from './module';
// Removed for brevity
export { Example };
// Comment
var example = new Example();
var val = example.exampleMethod();
```

*列表示例 A2-5. ESNext 模块*

AMD 和 System 模块类型也可以与 `--outFile` 标志一起使用，该标志会将你的程序合并成一个单独的 JavaScript 文件。在执行此操作之前，请先阅读下面“合并输出”部分中关于此标志的说明。

### ECMAScript 目标版本

`target` 标志允许你设置目标 ECMAScript 版本。目前，你可以针对多个版本的 ECMAScript 规范，包括 `ES3`、`ES5`、`ES2015`、`ES2016`、`ES2017` 或使用 `ESNext` 表示的最新版本。对于大多数语言特性，编译器会将代码编译到你指定的版本，尽管可能有一小部分特性不可用。

如果你针对 ECMAScript 3，则不能在 TypeScript 程序中使用属性，因为它们依赖于 ECMAScript 5 的特性。当添加 ECMAScript 6 选项时，它在编译期间将执行较少的转换。这是因为 ECMAScript 6 支持许多基本的 TypeScript 特性，例如模块、类、箭头函数和特殊参数类型。ECMAScript 6 支持也应允许新的 JavaScript 特性，如 `let`、生成器和解构。

```sh
tsc --target ES5 app.ts
```

示例 A2-6 展示了向下编译到 ECMAScript 5 规范的过程。关键字如 `const` 和 `let` 不能使用，因此它们被替换为 `var` 关键字。在会影响变量作用域的情况下，会生成额外的代码，将 `var` 关键字封装在一个新的函数作用域中。类被替换为立即调用函数表达式（IIFE）。

```javascript
import * as Dependency from './module';

var Example = (function (_super) {
    __extends(Example, _super);
    function Example() {
        return _super !== null && _super.apply(this, arguments) || this;
    }
    Example.prototype.exampleMethod = function () {
        return 5;
    };
    return Example;
}(Dependency.BaseClass));

export { Example };
// Comment
var example = new Example();
var val = example.exampleMethod();
```

*列表示例 A2-6. ES5 目标*

当针对支持你在 TypeScript 中使用的模块加载、类语法和变量声明的最新 ECMAScript 规范版本时，生成的 JavaScript 看起来几乎与你的 TypeScript 相同，如示例 A2-7 所示。

```typescript
import * as Dependency from './module';

export class Example extends Dependency.BaseClass {
    exampleMethod() {
        return 5;
    }
}
// Comment
let example = new Example();
const val = example.exampleMethod();
```

*列表示例 A2-7. ESNext 目标*

TypeScript 跟踪 ECMAScript 提案，这意味着最新版本的 TypeScript 和 JavaScript 将非常相似。编译后代码中唯一的实际区别是类型擦除，因为目前没有计划为 JavaScript 添加类型注解。

### 生成声明文件

`declaration` 标志将生成一个后缀为 `.d.ts` 的额外文件，其中包含你代码的环境声明。

```sh
tsc --declaration app.ts
```

示例 A2-8 展示了生成的声明文件，你可以将其作为程序打包策略的一部分。

```typescript
import * as Dependency from './module';

export declare class Example extends Dependency.BaseClass {
    exampleMethod(): number;
}
```

*列表示例 A2-8. app.d.ts 声明文件*

如果你通过包管理器（如 NPM）打包程序以便使用，此功能非常有用。

### 移除注释

`removeComments` 标志会从输出中删除所有注释，如果你的源代码中有大量注释，这会使输出文件变得更小。

```sh
tsc --removeComments true app.ts
```

### 合并输出

你可以使用 `out` 编译器标志将整个 TypeScript 程序合并到一个输出文件中。使用 `out` 标志时，你还必须为合并后的文件提供一个名称。

```sh
tsc --out final.js app.ts
```

将整个程序压缩到一个输出文件中的概念与 TypeScript 的目的——编写大型程序——背道而驰。即使你是为了学习习惯而使用 TypeScript 编写较小的程序，模块加载也是需要养成的关键习惯之一。

## 代码质量标志

以下标志可以帮助你提高代码质量，并在编译时捕获松散之处。如果你正在从头开始编写一个新程序，启用这些标志并遵循编译器的指导应该是一件简单的任务。对于现有程序，你可以决定启用这些标志，修复编译器发现的一些问题，然后再将其关闭。这将使你能够在许多离散的会话中改进代码，直到最终可以永久地保持它们启用。

### 默认值

就代码质量而言，许多选项默认设置为正确的值。以下所有标志的默认值都设置为 `false`，并且应该保持这样：

*   `--allowUnreachableCode`
*   `--allowUnusedLabels`
*   `--noImplicitUseStrict`
*   `--noStrictGenericChecks`
*   `--suppressExcessPropertyErrors`
*   `--suppressImplicitAnyIndexErrors`

### 严格模式开关

在默认设置的基础上，你可以启用严格模式来打开四个最常用的质量标志：

- `--noImplicitAny`：捕获无法推断类型且需要注解的情况。
- `--noImplicitThis`：捕获 `this` 关键字具有隐含 `any` 类型的表达式。
- `--alwaysStrict`：以严格模式解析，并为每个文件插入 `"use strict"` 语句。
- `--strictNullChecks`：捕获 `null` 和 `undefined` 值，防止意外的未赋值变量和属性。

严格标志的使用示例如下：

```sh
tsc --strict true app.ts
```

### 捕获意外的穿透

`noFallthroughCasesInSwitch` 标志会警告你 `switch` 语句中那些非空且缺少 `break` 关键字的 `case` 分支。

```sh
tsc --noFallthroughCasesInSwitch true app.ts
```

清单 A2-9 中的代码展示了一个常见的穿透错误，这种错误往往是意外发生的。只有当 `case` 分支非空时才会给出穿透错误提示，因为这代表了在可读性和可维护性方面结构不佳的代码。

```javascript
// 错误：switch 语句中存在穿透 case。
switch (num) {
    case 0:
    case 1:
        // 允许
        num = num * 2;
        break;
    case 2:
        // 允许
        num = num * 4;
        break;
    case 2:
        // 不允许
        num = num * 6;
        // 没有 break 语句 - 穿透
    case 4:
        // 允许
        num = num * 8;
        break;
}
```

清单 A2-9. 意外的穿透 case

### 捕获缺失的返回语句

`noImplicitReturns` 标志会警告你那些存在缺失 `return` 语句的代码分支的函数或方法。

```sh
tsc --noImplicitReturns true app.ts
```

清单 A2-10 会产生有效的 JavaScript 代码，因为允许省略 `return` 语句，从而导致返回 `undefined` 值。任何长期编写 JavaScript 代码的人都会体会到强制所有代码分支显式返回 `return` 语句的好处，否则你很可能会对程序的行为感到意外。

```typescript
// 错误：并非所有代码路径都返回值
function invert(cond: boolean) {
    if (cond) {
        return false;
    }
    // 此处缺少 return 语句
}
```

清单 A2-10. 意外的穿透 case

### 未使用的局部变量

`noUnusedLocals` 标志会在变量从未被使用时发出警告。这有助于识别可以删除的冗余代码。

```
tsc --noUnusedLocals true app.ts
```

### 未使用的参数

`noUnusedParameters` 标志会标记出未使用的函数和方法参数。这个标志比许多其他代码质量标志更棘手，因为有时你可能希望忽略传递给函数的某个参数。你可以使用此标志，直到遇到需要忽略参数的情况，然后如果它给你带来问题，再选择移除它。

```
tsc --noUnusedParameters true app.ts
```

## JavaScript 编译

如果你正在从 JavaScript 过渡到 TypeScript，当你的代码在这两种语言之间划分时，这些编译器选项可以帮助你弥合差距。

### 允许 JavaScript

`allowJs` 标志允许将 JavaScript 文件包含在编译中。

```
tsc --allowJs true app.ts
```

### 检查 JavaScript

`checkJs` 标志可以与 `allowJs` 标志一起使用，它会使编译器报告 JavaScript 文件中的错误。

```
tsc --allowJs true --checkJs true app.ts
```
