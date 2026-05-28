# 结论

通过使用 `CheckIsbn` 方法，我们能够确保 ISBN 是有效的字符串，包含正确数量的数字和正确的校验和。这样，我们就可以在构造函数中使用该方法来验证 ISBN，并抛出错误以保护内部数据。我们还可以创建访问器和修改器方法来获取和设置每个属性的值。访问器方法通常被命名为 `getAttributeName`，修改器方法被命名为 `setAttributeName`，用于设置属性的值。

## App 架构：为视图添加属性

当前的视图层次结构包含一个用于运行验证逻辑生命周期方法的 View。

App 将添加一个名为 `addAttributes(to:)` 的新方法，并将其作为 `viewDidAppear(_:)` 的一部分进行调用。

### 针对 Book 协议

以下是对 `Book` 协议的新增内容：

```swift
func display() -> String
```

```swift
mutating func display() -> String
```

```swift
mutating let display: String
```

```swift
mutating let title: String
```

```swift
mutating func display() -> String
```

```
return display
```

```swift
mutating func display() -> String
```

```
return title
```

```swift
var display: String
```

```
return self?.title ?? "No title"
```

```swift
var display: String
```

```
return title ?? "No title"
```

```swift
var display: String
```

```
self?.title = title ?? "default title"
```

```swift
var display: String
```

```
return self?.title ?? "No title"
```

### 针对 Author 协议

以下是对 `Author` 协议的新增内容：

```swift
func display() -> String
```

```
return author ?? "No author"
```

```swift
mutating func display() -> String
```

```
return author ?? "No author"
```

```swift
var display: String
```

```
return author ?? "No author"
```

```swift
mutating let display: String
```

```
return author ?? "No author"
```

```swift
var display: String
```

```
return author ?? "No author"
```

### 重要注意事项

请注意，一个接口在定义后，其他程序员不应只通过编程方式与其交互。从现在起，他们应该只使用接口中定义的那些方法。

不应在构造函数中使用这些方法。目前没有实现相同的验证方法，因此你应依赖这些内部方法。

这与完全暴露的对象模式一样好，因为它能让你获得完整的访问权限。你拥有一个定义良好的接口，访问器和 mutator 方法用于保护数据，以及验证方法。尽管拥有所有这些特性，设计中仍然存在一个漏洞。

即使我们提供了用于设置属性（attribute）的 mutator 方法，这些属性仍然是公开的，并且可以直接设置。对于此模式，没有办法防止这种情况。它有可能通过一个不知道如何设置它的程序员（通过直接设置）或一个知道它但忽略它的程序员意外设置一个无效的 IP。

尽管有这些缺陷，此模式仍然保留了许多好处。它易于使用，并且方便新的 Java 程序员快速上手。它并不需要深入理解作用域或调用链来创建像这样的类。子类化非常容易，因为所有方法和属性都是公开可用的。唯一的缺点是你无法保护内部数据，并且访问器和 mutator 方法添加了并非严格需要的额外代码。

这在文件（`File`）大小很重要的情况下可能是个问题。

### 使用命名约定定义私有方法

接下来，我们将研究一种模拟私有方法的模式。此模式解决了上一节中遇到的一个问题，即防止其他程序员意外地通过传递所有验证。这与完全暴露的对象本质上是相同的，但使用了下划线作为方法名的前缀，以及你希望保持私有的属性和方法名。

### 针对 Book 协议

以下是对 `Book` 协议的新增内容：

```swift
func display() -> String
```

```
return _display
```

```swift
mutating func display() -> String
```

```
if (!check_display()) { throw BookError.invalidIP }
_display = display
```

```swift
var display: String
```

```
return _display
```

```swift
mutating let display: String
```

```
_display = display ?? "default title"
```

```swift
func display() -> String
```

```
return _author
```

```swift
mutating let display: String
```

```
_author = author ?? "default author"
```

```swift
var display: String
```

```
return display
```

在这个例子中，所有的属性都被重命名了。在每一个的开始处添加了下划线，以表明它是私有的。这对于 Java 来说仍然是一个有效的变量名，因为下划线是首选的第一个合法字符。这种命名约定可以应用于方法以及属性。让我们认为一个程序员不应该直接设置它。

这仍然是合法的变量名，因为下划线是首选的第一个字符。该命名约定可以应用于方法以及属性。

此方法仍然是一个有效的变量名。

## 私有方法与隐藏内部数据

在 Java 中，私有方法（`private`）是面向对象编程中用于隐藏实现细节的一种机制。它允许类内部的方法相互调用，而无需暴露这些方法给外部代码。

### 核心观点

私有方法的主要目的是隐藏实现细节，防止外部代码直接访问这些方法。这对于创建稳定的公共接口非常有用。

### 私有方法的作用

私有方法主要用于支持公共方法的实现。它们通常用于分解复杂的公共方法，将重复的代码提取到单独的方法中，以减少代码冗余并提高可维护性。

#### 1. 代码复用（DRY 原则）

将重复的逻辑封装到私有方法中，可以在类内部多个位置调用，避免代码重复。

#### 2. 隐藏内部实现

私有方法隐藏了类的内部实现细节，外部代码只能通过公共接口访问类的功能。这有助于降低耦合度，提高代码的模块化程度。

### 使用场景

私有方法特别适用于以下场景：
- 公共方法需要执行多个步骤，其中一些步骤可以抽象为独立的私有方法。
- 需要避免在多个公共方法中重复相同的代码逻辑。

### 作用域与访问控制

私有方法只能在定义它们的类内部访问。外部代码无法直接调用私有方法，这有助于防止外部代码意外修改类的内部状态。

### 示例：作用域演示

在 JavaScript 中，函数拥有作用域（`scope`）。在函数内部定义的变量和函数只能在该函数内部访问。

```javascript
function foo() {
  var a = 10;
  function bar() {
    return a;
  }
}
```

在这个例子中，`bar` 是在 `foo` 内部定义的，但 `bar` 也可以访问 `a`，因为 `bar` 也定义在 `foo` 内部。

当 `bar` 被执行时，它返回 `a`。这使得 `bar` 可以访问 `a`，但如果尝试在 `foo` 外部执行 `bar`，则会报错，因为 `bar` 的作用域仅限于 `foo` 内部。

```javascript
function foo() {
  var a = 10;
  function bar() {
    return a;
  }
  return bar;
}
var baz = foo();
baz(); // 返回 10
```

# JavaScript 闭包与私有成员

`baz` 是 `foo()` 返回的 `bar` 函数引用。`baz()` 仍然可以访问 `foo` 内部的 `a`，因为闭包（`closure`）保留了 `bar` 的作用域链。但 `bar` 的外部（`foo` 之外）无法直接访问 `a`。

`var baz = foo()` 是一个对函数 `bar` 的引用。`baz()` 返回 `10`。`baz` 是另一个指向 `bar` 的引用。`baz()` 返回 `10`，`blat()` 返回 `20`，`blat()` 返回 `30`。

`var blat = foo()()` 是另一个指向 `bar` 的引用。`blat()` 返回 `10`，因为一个新的 `bar` 副本正在被使用。

这里有一个对 `bar` 的引用，然后将其赋值给变量 `baz`。这个函数现在在 `foo` 外部执行，并且仍然可以访问 `a`。这是因为 JavaScript 是词法作用域的。函数在定义时确定作用域，而不是执行时。`bar` 定义在 `foo` 内部，因此它可以访问 `foo` 的所有变量，即使 `foo` 已经执行完毕。

`bar` 与 `foo` 以及 `foo` 的局部变量形成了一个闭包（`closure`）。

## 私有成员与闭包

使用闭包是创建私有成员的一种常见方式。它通过返回一个嵌套函数来实现，这个嵌套函数可以访问外部函数的变量，从而模拟私有属性的效果。

## 访问私有函数

私有函数通常只用于内部实现，但也可以通过返回一个对象来暴露其功能，该对象只包含公共方法。

## 私有方法的作用域

私有方法的作用域仅限于其定义的类内部，外部代码无法直接访问。因此，私有方法只能通过类内部的公共方法间接调用。

`bar` 和 `blat` 各自都有一个 `a` 的副本，并且只有它们可以修改 `a`。最常见的创建闭包的方式是返回一个嵌套函数。

## 通过闭包实现私有成员

回到手头的问题：你需要创建一个只能被内部访问的变量。

一个闭包（`closure`）似乎是一个完美的契合，因为它允许你创建只能被某些函数访问的变量，并且这些变量被保存在这些函数调用之间。

### 示例：演示闭包中的作用域

```javascript
function foo() {
  var a = 10;
  function bar() {
    return a;
  }
  return bar;
}
```

`foo()` 返回 `bar`，但 `bar` 也可以访问 `a`，因为 `bar` 也定义在 `foo` 内部。

当 `bar` 被执行时，它返回 `a`。这使得 `bar` 可以访问 `a`，但如果尝试在 `foo` 外部执行 `bar`，则会报错，因为 `bar` 的作用域仅限于 `foo` 内部。

`bar()` 返回 `10`。`bar` 是另一个引用，`blat` 也是。`blat()` 返回 `10`，`blat()` 返回 `20`，`blat()` 返回 `30`。

`var baz = foo()` 是一个对函数 `bar` 的引用。`baz()` 返回 `10`。`baz` 是另一个指向 `bar` 的引用。`baz()` 返回 `10`，`blat()` 返回 `20`，`blat()` 返回 `30`。

`var blat = foo()()` 是另一个指向 `bar` 的引用。`blat()` 返回 `10`，因为一个新的 `bar` 副本正在被使用。

这里有一个对 `bar` 的引用，然后将其赋值给变量 `baz`。这个函数现在在 `foo` 外部执行，并且仍然可以访问 `a`。这是因为 JavaScript 是词法作用域的。函数在定义时确定作用域，而不是执行时。`bar` 定义在 `foo` 内部，因此它可以访问 `foo` 的所有变量，即使 `foo` 已经执行完毕。

`bar` 与 `foo` 以及 `foo` 的局部变量形成了一个闭包（`closure`）。

## 私有方法、作用域与闭包

私有方法的主要目的是隐藏实现细节，防止外部代码直接访问这些方法。

在 JavaScript 中，函数拥有作用域（`scope`）。在函数内部定义的变量和函数只能在该函数内部访问。这意味着私有方法（在函数内部定义）只能在该函数内部访问。

通过返回一个嵌套函数，可以创建闭包（`closure`），从而在函数外部访问其内部变量。

## 作用域与私有方法

私有方法的作用域仅限于其定义的类内部，外部代码无法直接访问。因此，私有方法只能通过类内部的公共方法间接调用。

## 总结

私有方法是面向对象编程中实现信息隐藏的重要机制。它们有助于保持代码的整洁，并确保只有必要的部分暴露给外部。通过使用私有方法，你可以更好地控制类的内部状态，并防止意外修改。

## 示例：作用域演示

```javascript
function foo() {
  var a = 10;
  function bar() {
    return a;
  }
  return bar;
}
var baz = foo();
baz(); // 返回 10
```

`bar` 是在 `foo` 内部定义的，但 `bar` 也可以访问 `a`，因为 `bar` 也定义在 `foo` 内部。

当 `bar` 被执行时，它返回 `a`。这使得 `bar` 可以访问 `a`，但如果尝试在 `foo` 外部执行 `bar`，则会报错，因为 `bar` 的作用域仅限于 `foo` 内部。

`bar` 和 `blat` 各自都有一个 `a` 的副本，并且只有它们可以修改 `a`。最常见的创建闭包的方式是返回一个嵌套函数。

## 通过闭包实现私有成员

回到手头的问题：你需要创建一个只能被内部访问的变量。

一个闭包（`closure`）似乎是一个完美的契合，因为它允许你创建只能被某些函数访问的变量，并且这些变量被保存在这些函数调用之间。

在 JavaScript 中，函数拥有作用域（`scope`）。在函数内部定义的变量和函数只能在该函数内部访问。这意味着私有方法（在函数内部定义）只能在该函数内部访问。

通过返回一个嵌套函数，可以创建闭包（`closure`），从而在函数外部访问其内部变量。