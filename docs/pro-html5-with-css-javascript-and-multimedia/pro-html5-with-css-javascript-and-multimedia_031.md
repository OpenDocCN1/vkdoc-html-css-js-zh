# JavaScript 函数与命名空间

通常情况下，当您在 JavaScript 中定义一个函数时，您只是定义了它；它还没有被调用。如果您希望它运行，您必须随后调用它。例如：

```
var myFunction = function() {
  doSomething();
}
```

现在，如果您想调用它，您需要添加一条像 `myFunction();` 这样的语句。或者，您可以将该函数定义为一个**立即调用函数表达式**（IIFE），发音为“iffy”。

您可以通过将函数放在一对圆括号内，然后添加一对开闭圆括号来将其转换为 IIFE，如下所示：

```
(function() {
  doSomething();
})();
```

如果这段代码定义在全局作用域中，那么 `doSomething()` 函数会在脚本被解析时立即执行。

#### 命名空间

在 JavaScript 中，全局作用域内的所有对象都需要具有唯一的名称，以避免名称冲突。请记住，浏览器上运行的所有脚本共享全局作用域。因此，即使您的代码没有冲突，其他第三方脚本也在全局作用域中拥有对象。如果您将所有对象都放在全局作用域中，与其他对象使用相同名称发生冲突的可能性就会大大增加。为了编写良好的防御性代码，您应该使用命名空间来组织代码。

命名空间是一种按层次结构组织代码的技术。一个对象的完全限定名称包含其在层次结构中定义的路径。因此，即使您有两个都名为 `Item` 的类，只要它们位于不同的路径（命名空间）中，它们的完全限定名称就是唯一的。

虽然 JavaScript 不直接支持 `namespace` 关键字，但命名空间的概念可以很容易地通过嵌套对象来实现。这里的基本原则是：在全局作用域中创建一个单一对象，然后将所有其他内容作为该全局对象的成员添加进去。例如，以下代码在全局作用域中创建了一个单一对象，然后创建了一些嵌套对象来定义一个层次结构。

```
var mySample = {}; // 全局对象

// 定义命名空间层次结构
mySample.things = {};
mySample.things.helpers = {};
mySample.otherThings = {};

// 向命名空间中添加内容
mySample.things.count = 0;
mySample.things.helpers.logger = function (msg) {
  console.log(msg);
}
```

如果您的代码分布在多个文件中，或者您在多个地方需要这段代码，您需要确保对象不会被创建两次。您可以通过使用 `||` 运算符来实现这一点：`this.mySample = this.mySample || {};`

一个未定义的对象会返回 `falsy` 值，因此 `||` 运算符会继续执行语句的下一部分，即创建该对象。如果该对象已经被创建，这会返回 `true`，其余代码将被跳过。

您的所有代码都会使用 `mySample` 对象内的对象，因此非常关键的一点是，这个对象必须在任何其他代码执行之前被创建。最简单的方法是在脚本开头将其包含在一个 IIFE 中，所以让我们将其重写为一个 IIFE：

```
(function () {
  window.mySample = window.mySample || {};

  // 在层次结构中添加其他节点
  window.mySample.things = window.mySample.things || {};
  window.mySample.things.helpers = window.mySample.things.helpers || {};
  window.mySample.otherThings = window.mySample.otherThings || {};

  // 设置快捷方式
  window.helpers = window.mySample.things.helpers;

  // 现在添加一些成员
  window.mySample.things.count = window.mySample.things.count || 0;
  window.mySample.things.helpers.logger = function (msg) {
    console.log(msg);
  }
})();
```

请注意，`var` 关键字被移除，并替换为了 `window` 对象；这指的是全局作用域。通过将 `mySample` 定义在 `window` 对象上，您可以使其在任何地方都可用。

最后，我偷偷加入了一个快捷方式，这样我就可以使用较短的引用来访问命名空间。您可以通过遵循命名空间层次结构（`mySample`、`things`、`helpers`）来访问 `logger()` 方法。但您也可以直接从快捷方式 `helpers` 访问它：

`mySample.things.helpers.logger("some text");`



