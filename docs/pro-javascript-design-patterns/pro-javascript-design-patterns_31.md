# 排版后的文档

进入`/path/to/xxx`目录，找到`xxx.json`。

在表达式`cvar = avar + bvar`中，加法运算符（`+`）将`avar`与`bvar`相加，得到它们的和`cvar`

在`List.of(arg0, arg1, arg2)`中，`List`接口的工厂方法`of()`接受一系列的元素，返回包含它们的只读列表。

之后我们这样调用`cmd`命令：`cmd arg0 arg1 arg2`。

```
if (condVar > someVal) {console.log("xxx")}
```

## 静态方法与属性

### `Book.getClassStaticVar()` → `String`

- 返回由`getStaticVar()`方法返回的`static`属性值。

### `Book.getBookCount()` → `int`

- 返回`static`方法中创建的`Book`实例数。

### `Book.getTitle()` → `String`

- 返回`Book`实例的标题。

### `Book.setTitle(String newTitle)` → `void`

- 设置`Book`实例的标题。

### `Book.getAuthor()` → `String`

- 返回`Book`实例的作者。

### `Book.setAuthor(String newAuthor)` → `void`

- 设置`Book`实例的作者。

### 构造函数

### `new Book(String title, String author)` → `Book`

- 创建一个新的`Book`实例。该方法类似于类的创建者，但此构造函数是一个嵌套函数，返回一个`Book`实例。

### `Book.toString()` → `String`

- 返回`Book`实例的字符串表示形式。

```
Book.toString()
```

## 公有静态方法

### `Book.convertToTitleCase(String input)` → `String`

- 将输入的字符串转换为标题大小写。

```
Book.convertToTitleCase("hello world") → "Hello World"
```

## 公有非特权方法

### `Book.prototype.display()` → `void`

- 显示`Book`实例的详细信息。

### `Book.prototype.getTitle()` → `String`

- 返回`Book`实例的标题。

### `Book.prototype.getAuthor()` → `String`

- 返回`Book`实例的作者。

### `Book.prototype.setTitle(String newTitle)` → `void`

- 设置`Book`实例的标题。

### `Book.prototype.setAuthor(String newAuthor)` → `void`

- 设置`Book`实例的作者。

## 私有方法

### `Book._getHiddenValue()` → `String`

- 返回`Book`实例的隐藏值。

### `Book._setHiddenValue(String newValue)` → `void`

- 设置`Book`实例的隐藏值。

## 静态方法

### `Book.getStaticVar()` → `String`

- 返回`Book`类的静态变量。

### `Book.setStaticVar(String newValue)` → `void`

- 设置`Book`类的静态变量。

## 示例

```
if (numBooks > 0) { throw new Error("Book count is invalid"); }
this._setHiddenValue("hidden");
```

## 总结

这与本章前面介绍的类创建类似。在“通过闭包实现私有成员”一节中，有几个关键区别。私有成员和特权成员仍然在构造函数内部使用`var`和`this`分别声明，但构造函数已从普通函数更改为嵌套函数，该嵌套函数将返回给变量`Book`。

该执行的结果是另一个函数，该函数被返回并设置为`Book`构造函数。当`Book`被实例化时，调用的是这个内部函数；外部函数仅用于创建一个闭包，你可以在其中放置私有静态成员。

在此示例中，`check getStaticVar`方法是静态的，因为没有必要为`Book`的每个实例都创建一个新的副本。还有一个名为`numBooks`的静态属性，它允许你跟踪`Book`构造函数被调用了多少次。在此示例中，我们使用该属性来限制构造函数仅创建新实例。

可以从构造函数内部访问这些私有静态成员，这意味着任何私有或特权函数都可以访问它们。它们比其他方法有一个明显的优势，即它们只在内存中存储一次。由于它们在构造函数外部声明，因此无法访问任何私有属性，因此不属于特权方法。私有方法可以调用私有静态方法，但反之则不然。

判断私有方法是否应为静态方法的一条经验法则是，查看它是否需要访问任何实例数据。如果不需要，则将方法设为静态



- **公有静态方法**与**实例方法**在类上的工作方式类似，但有一个关键区别：它们是在没有类实例的情况下调用的。

公有静态方法通常用于创建工具函数，例如创建类的副本。

构造函数通常使用`new`关键字调用。然而，有时需要以不同的方式创建实例，例如从现有实例复制属性。在 JavaScript 中，这可以通过静态工厂方法实现。

**注意**：在 *Java*Script 中，每个对象（除了原始值）都有一个名为`constructor`的属性。构造函数是创建该实例的函数。

以下是一个使用静态工厂模式的示例。请注意，这种模式在 JavaScript 中并不常见，但以下是工作原理：**私有**静态属性用作类自身内部的全局“注册表”，而静态方法是访问该注册表的入口。

由于`static`关键字用于方法，但尚未用于属性，因此该模式需要一个变通方法以在构造函数之外保留状态。

`var Class = getClass()`

```
// 常量（以私有静态属性形式创建）
```

`var CONSTANTS = {}`

```
//
```

