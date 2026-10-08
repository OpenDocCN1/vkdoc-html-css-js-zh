# 发布说明

## 版本 1.0.0（初始版本）

- 初始发布，提供核心的人员管理功能。

# DirectoryProxy 类

`DirectoryProxy` 是一个 `Directory` 类的代理。

## 概述

`DirectoryProxy` 类是一个在真实 `Directory` 对象上的代理。每个方法调用都简单地将相同的方法调用委托给真实的 `Directory` 对象。

`DirectoryProxy` 类是一个虚拟代理。`Directory` 构造函数是一个昂贵的操作，因此它会在第一次调用 `show()` 方法之前延迟执行。在 `Directory` 被初始化之前，所有方法调用都会被阻塞。第一次调用 `show()` 方法时会创建真实的 `Directory` 对象。

`DirectoryProxy` 构造函数不再实例化 `Directory`。它只是被告知 `Directory` 正在被加载并阻塞所有方法调用，直到真实的 `Directory` 被创建。

然而，在这种配置下，你无法获得 `Directory` 类的任何优势。你唯一能做的就是通知用户 `Directory` 正在加载并阻塞所有方法调用，直到真实的 `Directory` 被创建。实际上，你无法获得 `Directory` 类的任何好处——你只是用一个更简单的技术替换了它。你不会得到任何初始化的检查或更准确的触发器。

实际上，`DirectoryProxy` 只是简单地查询 `Directory` 的 `currentPage` 属性。如果它为空，`DirectoryProxy` 就知道 `Directory` 还没有被初始化。然而，这只是该类型代理的一个非常简单的示例。更复杂的版本可以实现更健壮的初始化检查以及更多准确的触发器。每个代理都将取决于你与用户的精确交互，你期望的交互方式也会有所不同。

接下来，我们将介绍一个动态虚拟代理，它可以被用作创建你自己的代理的模板。

## 一般模式：`DirectoryProxy` 类

`DirectoryProxy` 是一个 `Directory` 类，带有加载消息。它是一个非常简单的示例，说明这种类型的代理如何工作。更复杂的版本可以实现更多的初始化检查，并包含更多能够触发 `Directory` 初始化的用户交互。

### `DirectoryProxy` 类（包含 `show()` 方法）

```javascript
show() {
  if (this._initialized) {
    return this._show();
  }

  if (this._parent.this._warning) {
    return null;
  }

  this._initialized = true;
  this._interval = setInterval(
    CheckInitialization.this._checkInitialization,
    true
  );
}
```

# 显示页面

`show()` 函数返回 `this._show()`。

- 如果 `this._initialized` 为真，则返回 `this._show()`。
- 如果 `this._parent.this._warning` 为真，则返回 `null`。
- 否则，设置 `this._initialized` 为真，并设置 `this._interval` 为 `CheckInitialization.this._checkInitialization`，传入 `true`。

# 初始化检查

`CheckInitialization()` 函数检查 `this._director._currentPage` 是否不为空。

- 如果为空，则清除 `this._interval` 并移除 `this._warning`。
- 然后设置 `this._initialized` 为真。

# 阻塞方法调用

`block()` 函数会在 `this._director._currentPage` 为空时阻塞所有方法调用。

# 警告

`warning()` 函数返回 `this._parent.this._warning`。

- 如果 `this._initialized` 为真，则返回 `this._show()`。
- 否则，返回 `this._warning`。

# 父元素中的警告

`this._parent.this._warning` 是一个 `div` 元素，用于显示警告消息。它会被设置为 `"Loading..."`。

- 如果 `this._initialized` 为真，则返回 `this._show()`。
- 否则，返回 `this._warning`。

# 移除子元素

`removeChild()` 函数用于移除 `this._warning` 子元素。

# 间隔

`this._interval` 用于定期检查 `CheckInitialization.this._checkInitialization`。

# 初始化和检查

如果 `this._initialized` 为假，则设置 `this._initialized` 为真，并调用 `CheckInitialization.this._checkInitialization`。

# 父元素

`this._parent` 用于访问父元素。

# 初始化状态

- 如果 `this._initialized` 为真，则返回 `this._show()`。
- 否则，返回 `this._warning`。

# 设置间隔

`setInterval` 用于设置检查 `CheckInitialization.this._checkInitialization` 的间隔。

```
if (this._initialized) {
  return this._show();
}
```

# 返回

返回 `this._show()`。

# 显示页面

`show()` 函数返回 `this._show()`。

# 阻塞方法调用

`block()` 函数用于阻塞所有方法调用。当 `this._director._currentPage` 为空时，它会阻塞所有方法调用。

# 警告

如果 `this._initialized` 为真，则返回 `this._show()`。否则，返回 `this._warning`。

`this._parent.this._warning` 是一个 `div` 元素，用于显示警告消息。它会被设置为 `"The company directory is loading, please wait..."`，并显示在父元素中。

# 父元素

`this._parent` 用于访问父元素。

# 移除子元素

`removeChild()` 函数用于移除 `this._warning` 子元素。

`this._parent.removeChild(this._warning)` 用于移除警告。

# 设置间隔

`setInterval` 用于设置检查 `CheckInitialization.this._checkInitialization` 的间隔。

`this._interval` 用于定期检查 `CheckInitialization.this._checkInitialization`。

# 检查初始化

`CheckInitialization()` 函数检查 `this._director._currentPage` 是否不为空。如果为空，则清除 `this._interval` 并移除 `this._warning`。然后设置 `this._initialized` 为真。

如果 `this._director._currentPage` 不为空，则清除 `this._interval` 并移除 `this._warning`。然后设置 `this._initialized` 为真。

# 初始化和检查

如果 `this._initialized` 为假，则设置 `this._initialized` 为真，并调用 `CheckInitialization.this._checkInitialization`。

# 初始化状态

如果 `this._initialized` 为真，则返回 `this._show()`。否则，返回 `this._warning`。

# 控制流

```
if (this._initialized) {
  return this._show();
}
```

```
if (this._initialized)
  return this._show()
```

# 显示页面

`show()` 函数返回 `this._show()`。

# 阻塞方法调用

`block()` 函数会在 `this._director._currentPage` 为空时阻塞所有方法调用。

# 父元素中的警告

`this._parent.this._warning` 是一个 `div` 元素，用于显示警告消息。它会被设置为 `"Loading..."` 或 `"The company directory is loading, please wait..."`。

如果 `this._initialized` 为真，则返回 `this._show()`。否则，返回 `this._warning`。

# 移除子元素

`removeChild()` 函数用于移除 `this._warning` 子元素。

# 间隔

`this._interval` 用于定期检查 `CheckInitialization.this._checkInitialization`。

# 检查初始化

如果 `this._director._currentPage` 不为空，则清除 `this._interval` 并移除 `this._warning`。然后设置 `this._initialized` 为真。

# 初始化和检查

如果 `this._initialized` 为假，则设置 `this._initialized` 为真，并调用 `CheckInitialization.this._checkInitialization`。

# 警告

如果 `this._initialized` 为真，则返回 `this._show()`。否则，返回 `this._warning`。

# 父元素

`this._parent` 用于访问父元素。

# 初始化状态

如果 `this._initialized` 为真，则返回 `this._show()`。否则，返回 `this._warning`。

# 设置间隔

`setInterval` 用于设置检查 `CheckInitialization.this._checkInitialization` 的间隔。

# 控制流

```
if (this._initialized)
  return this._show()
```

# 显示页面

`show()` 函数返回 `this._show()`。

# 阻塞方法调用

`block()` 函数会在 `this._director._currentPage` 为空时阻塞所有方法调用。

# 父元素中的警告

`this._parent.this._warning` 是一个 `div` 元素，用于显示警告消息。它会被设置为 `"Loading..."` 或 `"The company directory is loading, please wait..."`。

如果 `this._initialized` 为真，则返回 `this._show()`。否则，返回 `this._warning`。

# 移除子元素

`removeChild()` 函数用于移除 `this._warning` 子元素。

# 间隔

`this._interval` 用于定期检查 `CheckInitialization.this._checkInitialization`。

# 检查初始化

如果 `this._director._currentPage` 不为空，则清除 `this._interval` 并移除 `this._warning`。然后设置 `this._initialized` 为真。

# 初始化和检查

如果 `this._initialized` 为假，则设置 `this._initialized` 为真，并调用 `CheckInitialization.this._checkInitialization`。

# 警告

如果 `this._initialized` 为真，则返回 `this._show()`。否则，返回 `this._warning`。

# 父元素

`this._parent` 用于访问父元素。

# 初始化状态

如果 `this._initialized` 为真，则返回 `this._show()`。否则，返回 `this._warning`。

# 设置间隔

`setInterval` 用于设置检查 `CheckInitialization.this._checkInitialization` 的间隔。

# 控制流

```
if (this._initialized)
  return this._show()
```

# 显示页面

`show()` 函数返回 `this._show()`。

# 阻塞方法调用

`block()` 函数会在 `this._director._currentPage` 为空时阻塞所有方法调用。

# 父元素中的警告

`this._parent.this._warning` 是一个 `div` 元素，用于显示警告消息。

它会被设置为 `"Loading..."` 或 `"The company directory is loading, please wait..."`。

如果 `this._initialized` 为真，则返回 `this._show()`。

否则，返回 `this._warning`。

# 移除子元素

`removeChild()` 函数用于移除 `this._warning` 子元素。

# 间隔

`this._interval` 用于定期检查 `CheckInitialization.this._checkInitialization`。

# 检查初始化

如果 `this._director._currentPage` 不为空，则清除 `this._interval` 并移除 `this._warning`。然后设置 `this._initialized` 为真。

# 初始化和检查

如果 `this._initialized` 为假，则设置 `this._initialized` 为真，并调用 `CheckInitialization.this._checkInitialization`。

# 警告

如果 `this._initialized` 为真，则返回 `this._show()`。

否则，返回 `this._warning`。

# 父元素

`this._parent` 用于访问父元素。

# 初始化状态

如果 `this._initialized` 为真，则返回 `this._show()`。

否则，返回 `this._warning`。

# 设置间隔

`setInterval` 用于设置检查 `CheckInitialization.this._checkInitialization` 的间隔。

```
if (this._initialized)
  return this._show()
```

# 控制流

```
if (this._initialized)
  return this._show()
```

# 显示页面

`show()` 函数返回 `this._show()`。

# 阻塞方法调用

`block()` 函数会在 `this._director._currentPage` 为空时阻塞所有方法调用。

# 父元素中的警告

`this._parent.this._warning` 是一个 `div` 元素，用于显示警告消息。

## 警告内容

它会被设置为 `"Loading..."` 或 `"The company directory is loading, please wait..."`。

## 初始化状态检查

如果 `this._initialized` 为真，则返回 `this._show()`。

否则，返回 `this._warning`。

# 移除子元素

`removeChild()` 函数用于移除 `this._warning` 子元素。

# 间隔

`this._interval` 用于定期检查 `CheckInitialization.this._checkInitialization`。

# 检查初始化

如果 `this._director._currentPage` 不为空，则清除 `this._interval` 并移除 `this._warning`。然后设置 `this._initialized` 为真。

# 初始化和检查

如果 `this._initialized` 为假，则设置 `this._initialized` 为真，并调用 `CheckInitialization.this._checkInitialization`。

# 警告

如果 `this._initialized` 为真，则返回 `this._show()`。

否则，返回 `this._warning`。

# 父元素

`this._parent` 用于访问父元素。

# 初始化状态

如果 `this._initialized` 为真，则返回 `this._show()`。

否则，返回 `this._warning`。

# 设置间隔

`setInterval` 用于设置检查 `CheckInitialization.this._checkInitialization` 的间隔。

```
if (this._initialized)

  return this._show()
```

# 动态代理

代理模式是面向对象编程中常见的设计模式。它允许为某个对象提供一个替代物，来控制对该对象的访问。

当存在一个类，它有一些方法，这些方法在执行前后需要执行一些额外操作时，代理模式就派上了用场。例如，在调用某个方法之前进行安全检查，或者在方法执行完成后进行一些清理工作。

`DynamicProxy` 类正是为此设计的。它的主要作用是动态地为真实对象的方法调用添加额外的行为。代理模式允许我们在不修改真实对象本身的情况下，为其添加额外的功能。

## `DynamicProxy` 类的 `newProxyInstance()` 方法

`DynamicProxy.newProxyInstance()` 是一个静态工厂方法，用于创建动态代理实例。它接受三个参数：类加载器、要代理的接口列表和调用处理器。

当调用者通过动态代理调用方法时，该调用会被转发到调用处理器的 `invoke()` 方法。调用处理器可以决定如何处理这次调用。例如：

- 在执行被代理方法之前，检查 `isInitialized` 是否已经为 `true`。
- 如果调用确实到达了真实对象，且检查通过，就执行真实对象的方法。
- 还可以进行其他操作，比如记录日志、权限校验等。

## `DynamicProxy` 的核心概念

`DynamicProxy` 是一个抽象类。它需要被继承并配置才能正常工作。

## 示例：`TestProxy` 类

让我们看看 `DynamicProxy` 在实际中是如何工作的：

```java
public class TestProxy<SELF extends TestProxy<SELF>> {

    public final String arg0 = null;

    public final SELF initialized = null;

    public final SELF subject = null;

    public SELF intervar = setInterval(setInterval.super)

    if (type == this.class.class.protokey) {

        return setInterval.super

    }

    if (type == clearInterval.super) {

        return clearInterval.super

    }

    return clearInterval.super

    return this.subject.arguments

}
```

现在，我们添加代码到构造器中，使其动态地为真实对象中的每个方法创建代理方法。这段代码看起来与动态代理的代码非常相似，但有几个重要的区别：

1.  `DynamicProxy` 抽象类：`abstract class DynamicProxy`
2.  类有一个 `for (DynamicProxy.arguments)` 方法：
    -   `this.arg0` = `Function(args)`
    -   `this.initialized` = `false`
    -   `if (typeof this.class.protokey == "Function")`：`throw new Error("DynamicProxy: the class attribute must be set before `super` constructor call.")`
    -   `this.initialized` = `true`
    -   `return`

    `new` `DynamicProxy`：`throw new Error("Unsupported operation on an abstract class.")`
    -   `return` `this.subject.apply(this, arguments)`
    -   `this.key`

    ```
    return this.subject.methodName.apply(this, arguments)
    ```

    ```
    } key
    ```

最重要的区别在于，你遍历的是类的原型，而不是对象本身。这是因为真实对象尚未被实例化，且不存在，所以你检查的是类本身，而不是真实对象，以确定要实现哪些方法。

你添加的每个方法由两部分组成：一个检查以确保真实对象已被初始化，以及一个对被代理方法的实际调用。

要使用这个类，你首先需要继承它。为了说明这是如何做到的，我们创建了一个 `TestProxy` 类，它被配置为作为 `TestProxy` 类的原型。

## `DynamicProxy` 原型 = `initialized` 函数

`this.subject` = `new` `Error("UnsupportedOperation on an abstract class.")`

`this.subject` = `TestProxy.arguments`

`for (this.protokey in this.class.protokey)`：`if (this.initialized)`：清除此函数的间隔，然后调用 `this.subject.apply(this, this.subject)`。

`this.initialized` = `true`。`Disinitialized` 函数：`if (this.isInitialized)`：清除间隔，然后 `this.subject.apply(this, this.initialized)`。`return` `this.subject.methodName(this, this.subject.arguments)`。

`return` `this.subject.methodName(this.subject, this.subject.arguments)`。

`return` `this`

```
for (var key in this.class.protokey) {

    this.initialized = false

    this.subject = new Error("Unsupported operation on an abstract class.")

}
```

`return`

```
DynamicProxy.prototype = function() {

    initialized function: if (this.isInitialized) { clearInterval(this.intervar); this.subject.apply(this, this.subject.arguments) }

    this.initialized = true

    clearInterval function: if (this.isInitialized) { clearInterval this.intervar; this.subject.apply(this, this.subject.arguments) }

    if (typeof this.class.protokey == "Function") { throw new Error("DynamicProxy: the class attribute must be set before `super` constructor call.") }

    this.initialized = true

    return setInterval(this.subject.methodName.apply(this, this.subject.arguments));

    this.intervar = setInterval(setInterval.super)

    if (typeof this.class.protokey == "Function") { return }

    this.initialized = true

    return this.subject

}
```

```
class TestProxy extends DynamicProxy<TestProxy> {

    public TestProxy() {

        super()

    }

    public final String arg0 = null;

    public final SELF initialized = null;

    public final SELF subject = null;

    public SELF intervar = setInterval(setInterval.super)

    if (type == this.class.class.protokey) {

        return setInterval.super

    }
```

# 动态代理类实现详解

## 参数与构造器

1.  `DynamicProxy` 抽象类：`abstract class DynamicProxy`
2.  `for (DynamicProxy.arguments)`：
    -   `this.arg0` = `Function(args)`
    -   `this.initialized` = `false`
    -   `if (typeof this.class.protokey == "Function")`：`throw new Error("DynamicProxy: the class attribute must be set before `super` constructor call.")`
    -   `this.initialized` = `true`
    -   `return`

    `new` `DynamicProxy`：`throw new Error("Unsupported operation on an abstract class.")`
    -   `return` `this.subject.apply(this, arguments)`
    -   `this.key`

    ```
    if (this.isInitialized) {
        clearInterval(this.intervar)
        this.subject.apply(this, this.subject.arguments)
    }
    ```

    ```
    return this.subject.methodName(this, this.subject.arguments)
    ```

    或者：
    ```
    for (var key in this.class.protokey) {
        this.initialized = false
        this.subject = new Error("Unsupported operation on an abstract class.")
    }
    ```

## 创建所需的方法

现在，你添加代码到构造器，使其动态地为真实对象中的每个方法创建代理方法。

11. 为`for (var key in this.class.protokey)` 创建`initialized` 函数：如果`this.isInitialized` 为真，则清除此函数的间隔，并调用`this.subject.apply(this, this.subject.arguments)`。
    `this.initialized` = `true`
    然后，在`else` 语句后（即没有命中`if`），继续处理。
    如果`typeof this.class.protokey == "Function"`：`throw new Error("DynamicProxy: the class attribute must be set before `super` constructor call.")`
    `this.initialized` = `true`
    `return setInterval(this.subject.methodName.apply(this, this.subject.arguments));`
    `this.intervar = setInterval(setInterval.super)`
    `if (typeof this.class.protokey == "Function") { return }`
    `this.initialized = true`
    `return this.subject`

## 添加方法

`for (var that = this.subject.methodName(this, this.subject.arguments); key in that.class.protokey; key++)`

`()` ：`DynamicProxy`，类的属性必须是一个函数。

`if (typeof this.class.protokey == "Function")`：`throw new Error("DynamicProxy: the class attribute must be set before `super` constructor call.")`
`this.initialized = true`

`clearInterval(this.intervar)`

`return this.subject.apply(this, this.subject.arguments)`

```
this.initialized = true
return setInterval(this.subject.methodName(this, this.subject.arguments), this.initialized);
```

`invoke` 方法：`return setInterval(this.subject.methodName(this, this.subject.arguments), this.initialized);`

`Disinitialized` 函数：`if (this.isInitialized)`：`clearInterval(this.intervar)`；`this.subject.apply(this, this.subject.arguments)`

`this.initialized = true`

`if (this.isInitialized)`：`this.subject.apply(this, this.subject.arguments)`

`this.initialized = true`

`return`

`return` `this.subject.methodName(this, this.subject.arguments)`

`return this.subject`

`DynamicProxy.prototype = function() { initialized function: if (this.isInitialized) { clearInterval(this.intervar); this.subject.apply(this, this.subject.arguments) } this.initialized = true clearInterval function: if (this.isInitialized) { clearInterval this.intervar; this.subject.apply(this, this.subject.arguments) } if (typeof this.class.protokey == "Function") { throw new Error("DynamicProxy: the class attribute must be set before `super` constructor call.") } this.initialized = true return setInterval(this.subject.methodName.apply(this, this.subject.arguments)); this.intervar = setInterval(setInterval.super) if (typeof this.class.protokey == "Function") { return } this.initialized = true return this.subject }`

```
class TestProxy extends DynamicProxy<TestProxy> {
    public TestProxy() {
        super()
    }
    public final String arg0 = null;
    public final SELF initialized = null;
    public final SELF subject = null;
    public SELF intervar = setInterval(setInterval.super)
```

## 立即测试

现在，你可以立即测试这个`TestProxy`类了。它必须被继承，因为动态代理本质上是一个抽象类，无法直接实例化。`DynamicProxy`类是一个用于测试的配置类，因此你不能像通常那样使用真实对象。请使用`class TestProxy extends DynamicProxy<TestProxy>`，所有接口方法将由`DynamicProxy`构造器自动创建。

现在，我们添加一个方法：

`for (var that = this.subject.methodName(this, this.subject.arguments); key in that.class.protokey; key++)`

`()`：`DynamicProxy`，类的属性必须是一个函数。

`if (typeof this.class.protokey == "Function")`：`throw new Error("DynamicProxy: the class attribute must be set before `super` constructor call.")`
`this.initialized = true`

`clearInterval(this.intervar)`

`return this.subject.apply(this, this.subject.arguments)`

`this.initialized = true`
`return setInterval(this.subject.methodName(this, this.subject.arguments), this.initialized);`

`invoke`方法：`this.intervar = setInterval(this.subject.methodName(this, this.subject.arguments), this.initialized);`

`Disinitialized`函数：`if (this.isInitialized)`：`clearInterval(this.intervar)`；`this.subject.apply(this, this.subject.arguments)`

`this.initialized = true`

`if (this.isInitialized)`：`this.subject.apply(this, this.subject.arguments)`

`this.initialized = true`

`return`

`return` `this.subject.methodName(this, this.subject.arguments)`

`return this.subject`

`} key`

你可能还想添加`checkInitialized()`函数。

`for (var key in that.class.protokey)`不应该是一个普通的`for`循环，而是`DynamicProxy`方法。

`DynamicProxy.prototype = function() { initialized function: if (this.isInitialized) { clearInterval(this.intervar); this.subject.apply(this, this.subject.arguments) } this.initialized = true clearInterval function: if (this.isInitialized) { clearInterval this.intervar; this.subject.apply(this, this.subject.arguments) } if (typeof this.class.protokey == "Function") { throw new Error("DynamicProxy: the class attribute must be set before `super` constructor call.") } this.initialized = true return setInterval(this.subject.methodName.apply(this, this.subject.arguments)); this.intervar = setInterval(setInterval.super) if (typeof this.class.protokey == "Function") { return } this.initialized = true return this.subject }`

```
class TestProxy extends DynamicProxy<TestProxy> {

    public TestProxy() {

        super()

    }

    public final String arg0 = null;

    public final SELF initialized = null;

    public final SELF subject = null;

    public SELF intervar = setInterval(setInterval.super)

    if (type == this.class.class.protokey) {

        return setInterval.super

    }

    if (type == clearInterval.super) {

        return clearInterval.super

    }

    return clearInterval.super

    return this.subject.arguments

}
```

## 参数与构造器

1.  `DynamicProxy`抽象类：`abstract class DynamicProxy`
2.  `for (DynamicProxy.arguments)`：
    -   `this.arg0` = `Function(args)`
    -   `this.initialized` = `false`
    -   `if (typeof this.class.protokey == "Function")`：`throw new Error("DynamicProxy: the class attribute must be set before `super` constructor call.")`
    -   `this.initialized` = `true`
    -   `return`

    `new` `DynamicProxy`：`throw new Error("Unsupported operation on an abstract class.")`
    -   `return` `this.subject.apply(this, arguments)`
    -   `this.key`

    ```
    if (this.isInitialized) {

        clearInterval(this.intervar)

        this.subject.apply(this, this.subject.arguments)

    }
    ```

    ```
    return this.subject.methodName(this, this.subject.arguments)
    ```

    }
    ```

    return this.subject.methodName(this.subject, this.subject.arguments)

    ```

    或者：
    ```
    for (var key in this.class.protokey) {

        this.initialized = false

        this.subject = new Error("Unsupported operation on an abstract class.")

    }
    ```

## 测试

`DynamicProxy` 原型 = `initialized` 函数

`this.subject` = `new` `Error("UnsupportedOperation on an abstract class.")`

`this.subject` = `TestProxy.arguments`

`for (this.protokey in this.class.protokey)`：`if (this.initialized)`：清除此函数的间隔，然后调用 `this.subject.apply(this, this.subject)`。

`this.initialized` = `true`。`Disinitialized` 函数：`if (this.isInitialized)`：清除间隔，然后 `this.subject.apply(this, this.initialized)`。`return` `this.subject.methodName(this, this.subject.arguments)`。

`return` `this.subject.methodName(this.subject, this.subject.arguments)`。

`return` `this`

```
for (var key in this.class.protokey) {
    this.initialized = false
    this.subject = new Error("Unsupported operation on an abstract class.")
}
```

`return`

```
DynamicProxy.prototype = function() {
    initialized function: if (this.isInitialized) { clearInterval(this.intervar); this.subject.apply(this, this.subject.arguments) }
    this.initialized = true
    clearInterval function: if (this.isInitialized) { clearInterval this.intervar; this.subject.apply(this, this.subject.arguments) }
    if (typeof this.class.protokey == "Function") { throw new Error("DynamicProxy: the class attribute must be set before `super` constructor call.") }
    this.initialized = true
    return setInterval(this.subject.methodName.apply(this, this.subject.arguments));
    this.intervar = setInterval(setInterval.super)
    if (typeof this.class.protokey == "Function") { return }
    this.initialized = true
    return this.subject
}
```

`TestProxy` 类：`class TestProxy extends DynamicProxy<TestProxy>`

### 测试 `TestProxy`

现在，我们手动操作，调用 `TestProxy` 的构造器。它应该会先检查真实对象是否被初始化，然后调用真实对象的方法，并验证设置的真实对象是否被正确触发。

### 关于 `Error` 和 `UnsupportedOperation`

最重要的一点是，你遍历的是类的原型，而不是对象本身。这是因为真实对象尚未被实例化，且不存在，所以你检查的是类本身，而不是真实对象，以确定要实现哪些方法。

你添加的每个方法由两部分组成：一个检查以确保真实对象已被初始化，以及一个对被代理方法的实际调用。

要使用这个类，你首先需要继承它。为了说明这是如何做到的，我们创建了一个 `TestProxy` 类，它被配置为作为 `TestProxy` 类的原型。

### `DynamicProxy` 原型 = `initialized` 函数

`this.subject` = `new` `Error("UnsupportedOperation on an abstract class.")`

`this.subject` = `TestProxy.arguments`

`for (this.protokey in this.class.protokey)`：`if (this.initialized)`：清除此函数的间隔，然后调用 `this.subject.apply(this, this.subject)`。

`this.initialized` = `true`。`Disinitialized` 函数：`if (this.isInitialized)`：清除间隔，然后 `this.subject.apply(this, this.initialized)`。`return` `this.subject.methodName(this, this.subject.arguments)`。

`return` `this.subject.methodName(this.subject, this.subject.arguments)`。

`return` `this`

### `TestProxy` 类

```
class TestProxy extends DynamicProxy<TestProxy> {
    public TestProxy() {
        super()
    }
    public final String arg0 = null;
    public final SELF initialized = null;
    public final SELF subject = null;
    public SELF intervar = setInterval(setInterval.super)
    if (type == this.class.class.protokey) {
        return setInterval.super
    }
    if (type == clearInterval.super) {
        return clearInterval.super
    }
    return clearInterval.super
    return this.subject.arguments
}
```

### 现在测试

```
DynamicProxy.prototype = function() {
    if (this.isInitialized) {
        clearInterval(this.intervar)
        this.subject.apply(this, this.subject.arguments)
    }
    this.initialized = true
    clearInterval(this.intervar)
    return this.subject.apply(this, this.subject.arguments)
}
```

### 关于 `UnsupportedOperation`

`UnsupportedOperation` 是一个错误类：`throw new Error("UnsupportedOperation on an abstract class.")`

## 测试

`DynamicProxy` 原型 = `initialized` 函数

`this.subject` = `new` `Error("UnsupportedOperation on an abstract class.")`

`this.subject` = `TestProxy.arguments`

`for (this.protokey in this.class.protokey)`：`if (this.initialized)`：清除此函数的间隔，然后调用 `this.subject.apply(this, this.subject)`。

`this.initialized` = `true`。`Disinitialized` 函数：`if (this.isInitialized)`：清除间隔，然后 `this.subject.apply(this, this.initialized)`。`return` `this.subject.methodName(this, this.subject.arguments)`。

`return` `this.subject.methodName(this.subject, this.subject.arguments)`。

`return` `this`

```
for (var key in this.class.protokey) {
    this.initialized = false
    this.subject = new Error("Unsupported operation on an abstract class.")
}
```

`return`

### 测试 `TestProxy`

现在，我们手动操作，调用 `TestProxy` 的构造器。它应该会先检查真实对象是否被初始化，然后调用真实对象的方法，并验证设置的真实对象是否被正确触发。

# 代理模式

## 定义

代理模式（`Proxy Pattern`）是一种结构型设计模式。

## 结构

代理模式包含以下角色：

- `Subject`（抽象主题）
- `RealSubject`（真实主题）
- `Proxy`（代理）

## 工作原理

代理模式通过引入一个代理对象来控制对真实对象的访问。代理对象与真实对象实现相同的接口，这样客户端可以透明地使用代理对象，无需关心真实对象的存在。

## 适用场景

代理模式适用于以下场景：

1. **延迟加载**：当创建一个真实对象需要消耗大量资源时，可以使用代理模式延迟对象的实例化，直到真正需要时才创建。

2. **访问控制**：代理可以控制对真实对象的访问权限。

3. **日志记录**：代理可以在调用真实对象的方法前后添加日志记录。

4. **远程代理**：代理可以代表位于不同地址空间的对象，如网络服务。

## 代码示例

```java
// Subject 接口
public interface Subject {
    void request();
}

// RealSubject 类
public class RealSubject implements Subject {
    @Override
    public void request() {
        System.out.println("RealSubject: Handling request.");
    }
}

// Proxy 类
public class Proxy implements Subject {
    private RealSubject realSubject;

    @Override
    public void request() {
        if (realSubject == null) {
            realSubject = new RealSubject();
        }
        realSubject.request();
    }
}
```

# 代理模式概述

## 优点

- **职责清晰**：代理模式可以将客户端与真实对象解耦，使职责更加清晰。
- **高扩展性**：代理类可以独立于真实对象进行扩展。
- **智能化**：代理可以在不修改真实对象的情况下增加额外的功能。

## 缺点

- **增加系统复杂度**：引入代理类会增加系统的复杂度和维护成本。
- **响应时间延长**：由于代理对象的存在，可能会增加响应时间。

## 实际应用

代理模式广泛应用于以下场景：

- **虚拟代理**：在图片加载、文档加载等场景中，使用虚拟代理延迟资源的加载。
- **保护代理**：在权限控制系统中，使用保护代理控制对敏感资源的访问。
- **远程代理**：在分布式系统中，使用远程代理访问远程服务，如 `RMI`、`WebService` 等。
- **智能引用代理**：在需要记录对象访问次数、加载时间等场景中使用。

## 总结

代理模式是一种非常有用的结构型设计模式，它可以在不修改原有代码的情况下，为对象提供额外的控制功能。在实际开发中，我们应该根据具体场景选择合适的代理类型，避免过度设计。

# 关于代理模式的深入剖析

代理模式是一种结构型设计模式，它提供了一个代理对象来控制对另一个对象的访问。这个模式在现代软件开发中扮演着至关重要的角色。

## 代理模式的核心概念

代理模式的核心思想是使用一个代理对象来控制对真实对象的访问。它通过提供一个与真实对象相同的接口，使得客户端可以透明地访问真实对象，而代理则可以在访问前后执行额外的逻辑。

代理模式的本质是控制对真实对象的访问。它通常用于以下几种场景：

- 延迟初始化（虚拟代理）：当创建一个真实对象的成本很高时，可以使用代理模式来延迟其创建，直到真正需要时才实例化。
- 访问控制（保护代理）：代理可以控制哪些客户端可以访问真实对象，基于权限或角色进行限制。
- 日志记录（日志代理）：代理可以在访问真实对象时记录日志，帮助调试或监控。
- 远程代理：代理可以代表一个位于不同地址空间（如另一台机器）的对象，使得客户端可以像访问本地对象一样访问远程对象。

## 代理模式的实现方式

在 `Java` 中，代理模式可以通过多种方式实现。最常见的方式包括静态代理和动态代理。静态代理需要为每个真实对象编写一个对应的代理类，而动态代理则可以在运行时动态地创建代理对象。

在 `Java` 中，动态代理主要通过`java.lang.reflect.Proxy`类和`InvocationHandler`接口来实现。这种方式非常灵活，可以在不修改原有代码的情况下，动态地为任意接口创建代理对象。

代理模式的实现通常涉及以下几个角色：

- 抽象主题（`Subject`）：定义了真实对象和代理对象的共同接口。
- 真实主题（`RealSubject`）：实现了抽象主题，是真正执行业务逻辑的对象。
- 代理（`Proxy`）：实现了抽象主题，并持有对真实主题的引用。客户端通过代理来访问真实主题。

这种模式可以帮助你在不修改真实对象代码的情况下，添加额外的功能，如日志记录、性能监控、访问控制等。

## 代理模式的分类

代理模式可以根据其用途分为多种类型：

- 虚拟代理：用于延迟创建开销较大的对象。当客户端需要访问一个对象，但该对象的创建成本很高时，虚拟代理可以延迟其创建，直到真正需要时才实例化。
- 保护代理：用于控制对对象的访问。保护代理可以在访问真实对象之前，检查客户端的权限，只有具有相应权限的客户端才能访问。
- 远程代理：用于访问位于不同地址空间的对象。远程代理可以隐藏网络通信的细节，使得客户端可以像访问本地对象一样访问远程对象。
- 智能引用代理：用于在访问对象时执行额外的操作，如引用计数、线程安全检查等。

## 代理模式的优势

代理模式在许多情况下都是非常有用的。它可以帮助你解耦客户端和真实对象，使得你可以更灵活地控制对真实对象的访问。代理模式还可以帮助你实现关注点分离，将横切关注点（如日志、安全、事务等）从业务逻辑中分离出来。

此外，代理模式还可以提高系统的可维护性和可扩展性。通过使用代理，你可以在不修改现有代码的情况下，轻松地添加新的功能。

## 示例：新闻报纸递送

在新闻报纸行业中，有一个关键角色和动作，使得发布和订阅运行得顺畅。首先，最重要的是读者们。他们是订阅者，是像你我一样的人。我们消费数据，并对所读的内容做出反应。我们也应该能够选择我们的喜好，并有报纸被递送到我们的家。

这个例子使用新闻报纸行业来演示观察者模式如何工作。

在新闻报纸行业中，关键角色和动作使得发布和订阅运行得顺畅。首先，最重要的是读者。他们是订阅者，是像你我一样的人。我们消费数据，并对所读的内容做出反应。我们也应该能够选择我们的喜好，并有报纸被递送到我们的家。

其次，我们有出版者，他们负责生产和分发报纸。出版者应该能够发布新内容，并通知所有订阅者。订阅者则应该能够订阅或取消订阅，以便他们可以接收或停止接收报纸。

这个模式可以在 `Java` 中通过多种方式实现。我们将在后续章节中详细讨论。

## 总结

在本章中，我们讨论了代理模式的各种形式。这种模式以某种方式控制对资源的访问。这个资源被称为真实对象。代理模式控制对真实对象的访问。这种模式以某种方式控制对资源的访问。这个资源被称为真实对象。

保护代理控制对类或对象的访问，这些类或对象可以被扩展、创建或维护。当用户的浏览器内存有限，无法运行你的代码时，这在 `Java` 中会非常有帮助。如果你发现真实对象加载缓慢，它也能派上用场。

虚拟代理可以提供加载消息或一个 UI 引导，在结束用户可以与真实 UI 交互之前，直到真实 UI 被加载。

代理模式可以随时使用，并给你的项目增加复杂度。它主要用于使你的代码可重用、更模块化或更高效。当在这些场景中使用时，代理可以使访问原本难以访问的其他资源变得更加容易。
