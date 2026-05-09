# ES6 模块

ES6 的 `import` 语句只会创建指向相应变量、函数或类的**只读绑定**。因此，导入这些绑定的模块无法修改其值。但导出标识符的模块可以对其进行修改。例如：

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

#### 解构导入语句与使用变量

尽管 import 语句看起来像是解构，但事实并非如此。因此，以下代码是无效的，将会抛出错误：

```javascript
import { toto: { tata } } from 'someModule'; // 报错
```

你还需要牢记一点：import 语句不能依赖于任何需要在运行时计算的内容。因此，类似下面的示例也会抛出错误：

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
- `./` 解析为父目录

## 总结

ES6 中的模块为扩展 JavaScript 的功能提供了新的方式。它们有助于更好地组织代码，并维护更具模块化语义的代码库。ES6 引入了新的关键字，如 `import`、`export` 和 `default`，使得模块的使用成为可能。由于 ES6 集成了两者的优点，你不再需要使用 UMD 或诸如 CommonJS 或 AMD 之类的额外模块系统。

使用模块，你可以编写可重用且封装性更好的代码。与脚本不同，模块不会修改全局作用域，从而降低了出错的可能性。如果你想在其他地方使用模块中的值，你必须专门将它们从模块中导出。

