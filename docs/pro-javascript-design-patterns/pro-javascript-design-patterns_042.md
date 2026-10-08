# 单例模式

单例是一种创建型设计模式，让你能够保证一个类只有一个实例，并提供一个访问该实例的全局节点。

单例拥有与全局变量相同的优缺点，但它更加简洁且易于使用。它确保了只有一个 `Singleton` 实例，并且该实例可以被轻松地访问到。

全局变量会污染命名空间，并且可能导致代码难以维护。而单例模式则提供了一种更优雅的方式来管理全局状态。

单例模式通常用于管理共享资源，例如数据库连接、配置对象等。

它提供了一种简单的方式来访问这些资源，确保整个应用程序中只有一个实例存在。

单例类通常使用私有构造函数来防止外部实例化，并通过静态方法（如 `getInstance()`）来提供对唯一实例的访问。这种方式可以确保全局变量的数量被限制在最小范围内。

在 `JavaScript` 中，单例模式通过确保一个类只有一个实例，并提供对该实例的全局访问，从而减少全局变量的数量。它们可以被用于命名空间，这有助于封装浏览器差异，并通过一种技术称为“分支”来减少全局变量的数量。

单例模式是最基本的设计模式之一，但它可以用于许多不同的目的。它涵盖了最有用的一种模式——单例，这本身就是一个重大的飞跃。从实现的角度来看，单例模式本质上是对象字面量，因为它们在 `JavaScript` 中是最简单的创建对象的方式。

单例模式是 `JavaScript` 中最常用的模式之一。它本质上是一个对象，只能被实例化一次，并通过一个众所周知的访问点进行访问。严格遵循此定义，之前的示例不是一个单例，因为它不是一个可实例化的类。我们选择将单例模式定义得更宽泛一些：它是一个用于命名空间和分组相关方法和属性的对象集合，并且如果它在所有地方都是可实例化的，那么它只能被实例化一次。

使用对象字面量是创建单例的唯一技术。本章后面介绍的其他方法更像是以单例形式呈现的类。

另外，并非所有对象字面量都是单例。如果它仅仅用于模拟一个关联数组，或者用于传递一组相关参数到函数中，那么它就不是一个单例。

### 单例的基本结构

#### 在 `JavaScript` 中的实现方式

在 `Singleton` 模式中，所有成员现在都可以通过 `Singleton` 变量访问。

你可以使用点符号（`Singleton.attribute = false`）访问它们。

`var total = Singleton.attribute;`

`var result = Singleton.method(args);`

### 单例的使用场景

#### 用于命名空间

单例模式用于命名空间，即把相关方法和属性组织在一起，以减少全局变量的数量。在 `JavaScript` 中，单例模式尤其常见，因为它可以帮助组织代码，减少全局作用域的污染。

单例对象可以通过点符号（`.`）访问其方法和属性。你可以通过在一个单例对象中定义所有相关的方法和属性，来实现简单的命名空间。这可以作为组织代码的第一步，也是最重要的一步。

单例模式还可以用于管理共享资源。例如，你可以使用单例模式来管理一个数据库连接池。

```javascript
```

# 单例对象的命名空间

`Singleton` 是一个用于确保单个全局实例的设计模式。它由两部分组成：对象本身以及其构造函数。

## 概述

`Singleton` 对象由两部分构成：对象本身，包含成员；以及其构造函数，用于访问它。构造函数是全局可访问的。

`Singleton` 是 JavaScript 中一个重要的部分，因为它常用于命名空间。在 JavaScript 中，通过使用单例对象，您可以将您的代码组织得井井有条。

因为全局命名空间容易受到污染，所有成员都应包含在单例对象中。它们不应全局可访问。通过使用带有闭包的模块模式，您可以模拟面向对象编程中的私有成员。

由于通常应用于整个应用程序的全局命名空间争用，命名空间是单例对象的一个关键特性。因此，单例对象应该是任何 JavaScript 程序的重要组成部分。它通常是作用域内的一个全局对象。

因为所有成员都包含在单例对象内，它们只能通过该对象的变量进行访问。这意味着变量不是全局的。在某种意义上，它们可以被视为对象内部的命名空间。

### 命名约定

目前，大多数 JavaScript 作者使用驼峰式命名（camelCase），这是一个良好的实践。除非使用构造函数，否则不应使用帕斯卡命名（PascalCase）。然而，一些第三方库的名称是全局的。通过使用单例对象，您可以将所有代码和变量组织在一个对象中。

当您使用单例对象时，您可以将所有变量或数据分组到一个全局变量下。然后，您可以将所有这些变量分组到单例对象或单例对象内的其他嵌套对象中。

### 单例与字母常量

`MyNamespace` 是一个用于单例全局变量的命名约定。它被选中是因为内部全局变量被声明为 `const` 或 `let`，而全局命名空间只用于标识该对象。所有成员都应包含在单例对象内。在大型应用程序中，多个开发人员共同协作时，使用 `const` 或 `let` 声明变量可以避免意外的命名冲突。

您的单例对象应该是唯一的，并且一旦创建就不能更改。单例在应用程序的整个生命周期中只被实例化一次。在大型项目中，由于代码可能由多个库、广告、第三方模块、小部件等组成，因此命名空间冲突很常见。为避免混淆，您应该将共享代码组合到单例对象下面。

## 命名空间的作用域

虽然每个第三方库都不同，但大多数都会声明一个单一的全局变量来包含所有代码。这个变量通常称为“命名空间”。您应该在您的代码中也这样做，将对象字面量用作命名空间。这意味着您可以将所有代码放在一个全局变量下，从而使您的代码库高度模块化和可维护。

## 相关文章

- [如何使用单例对象](http://www.yoururl.com)
- [全局变量的危险](http://www.yoururl.com)

---
*想要了解更多关于命名空间的知识？请阅读我们的下一篇文章。*

---

## 单例模式的应用

### 用于管理共享资源

单例模式常用于管理共享资源，例如数据库连接、配置对象等。它提供了一种简单的方式来访问这些资源，确保整个应用程序中只有一个实例存在。

你可以使用单例模式来管理一个数据库连接池，或者一个配置对象。

```javascript
var Singleton = {
  attribute: true,
  method: function (args) {
    // 方法体
  }
};
```

使用示例：

```javascript
Singleton.attribute = false;
var total = Singleton.attribute;
var result = Singleton.method(args);
```

### 用于实现分支

分支是一种技术，它允许你根据浏览器的能力或特性来定义不同的方法实现。单例模式可以用于实现分支，因为它允许你根据条件选择不同的实现。

例如，你可能需要根据浏览器是否支持 `XMLHttpRequest` 来使用不同的网络请求实现。

```javascript
var Singleton = {
  method: function () {
    if (typeof XMLHttpRequest !== 'undefined') {
      // 使用 XMLHttpRequest
    } else {
      // 使用其它方法
    }
  }
};
```

### 区别

单例模式与其它对象字面量的主要区别在于：

- 单例模式通常用于命名空间、管理共享资源和实现分支。
- 单例模式通常只有一个实例。
- 单例模式通常通过一个全局访问点来访问。

对象字面量只是单例模式的一种实现方式。单例模式的核心思想是确保一个类只有一个实例，并提供一个全局访问点。

---

## UI 框架中的单例

UI 框架（`UI Framework`）通常由两个核心部分构成：应用程序对象（`Application Object`）与页面集合（`Page Collection`）。

前者（`Application Object`）是一个用于页面级生命周期的容器（`Container`），用于容纳各个页面（`Page`）。

这意味着该框架提供了如何创建单例对象（`Singleton Object`）的名称空间（`Namespace`）的蓝图。它通常像一种模式（`Pattern`）或模板（`Template`）来使用。

单例（`Singleton`）的用途在于页面模式（`Page Pattern`）。在多页面（`Multi-page`）站点中，你经常会看到它的使用；而在绝大多数情况下，它专门用于单页面（`Single Page`）。

它并不在任何其他地方使用。它是一个将这两者分开的好思路（`Idea`），即创建两个独立的单例（`Singleton`）对象。

包裹页面集合（`Page Collection`）的容器（`Container`）通常具有与页面集合类似的页面级生命周期。它需要封装一些可能包含页面级活动（`Page-level Activity`）方法和初始化（`Initialization`）方法的数据。

大部分涉及具体元素（`Specific Elements`）的代码只能在像 DOM 这样的事件附件（`Event Attachment`）中执行一次，这些元素被加载后才行。

通过创建一个初始化（`Init`）方法并将它附着到窗口（`Window`）的加载（`Load`）事件中（或者类似的加载事件），你可以将所有初始化（`init`）代码集中在一个地方。

这里有一个用于包裹页面级生命周期（`Page-level Lifecycle`）的单例（`Singleton`）的框架：

```
01. 通用页面对象
   Namespace.PageName = new function() {}

01. 页面常量
   CONSTANTS = true
   CONSTANTS = false

01. 页面方法
   method.function() {}
   // 更多信息，前往 http://petermichaux.com/articles/singleton
   COPYRIGHT = 2011
   THE MIT LICENSE
~~
   method.function() {}
~~

01. 初始化方法
   init.function() {}

01. 在页面加载后调用初始化方法
   addLoadEvent(Namespace.PageName.init) {}
```

要解释这是如何使用的，让我们先看一个在 Web 开发中比较常见的任务，并逐步分析它。

假设 `Namespace.PageName` 已经作为一个空对象被创建。如果还没创建，这第一行代码会使用布尔值 OR 操作符来提供一个默认值。

```javascript
var Namespace.PageName = window.Namespace.PageName || {};
```

在此例中，我们为两个我们关心的 DOM 元素（适用于常量中定义的常量）放置了 DOM 引用（`References`），因为它们在程序执行中不会改变。

初始化（`init`）方法获取这两个 DOM 元素并存储它们作为新的属性。

```javascript
var data = {inputs: form.getElements()};
var data = {form: getElement};

99. 将格式化后的值发回服务器。
   Namespace.PageName.sendRegistration(data) {}

01. 初始化方法
   init.function() {}

   在页面加载后调用初始化方法。
   addLoadEvent(Namespace.PageName.init) {}
```

我们首先假设 `Namespace.PageName` 已经被创建为一个空对象。如果还没有，这第一行会报错。这个错误可以通过一行创建 `Namespace.PageName`（如果还不存在）的代码来避免，使用布尔或（OR）操作符：

```javascript
var Namespace.PageName = window.Namespace.PageName || {};
```

在这个例子中，我们把 `var` 语句换成两个我们关心的 DOM 元素（在常量中定义的常量）。因为它们在程序运行期间不会改变。

`init` 方法获取这两个 DOM 元素并将它们存储为新属性。

## 单例模式

单例模式是创建型设计模式之一，其核心目标是确保一个类只有一个实例，并提供全局访问点来获取该实例。

当应用程序中的某个对象需要被精确地共享，并且所有操作都通过该单个实例进行时，单例模式便成为理想选择。原因在于，每次创建新实例都会消耗系统资源，而单例模式通过复用已有实例来避免这种开销。

单例模式确保一个类只有一个实例，并提供一个全局访问点。这种设计模式在框架和库中广泛应用，例如 `Singleton` 模式、`Logger` 类、`DataSource` 单例、`Configuration` 管理器等。

在单例模式中，构造函数是私有的，外部代码无法直接实例化该类。相反，类本身负责创建和管理其唯一实例，并提供一个静态方法来返回该实例。

### 使用单例模式的场景

单例模式常用于需要控制共享资源访问的场景，例如数据库连接池、线程池、日志记录器、配置管理器等。在这些场景中，确保只有一个实例可以避免资源冲突和不一致的状态。

### 实现单例模式的注意事项

单例模式是最直接且最常用的实现方式之一。它适用于以下情形：

- 确保一个类只有一个实例
- 提供一个全局访问点来控制对该实例的访问

但需要注意的是，单例模式也存在一些缺点：

- 违反单一职责原则（SRP）：单例类不仅负责管理自己的实例，还负责业务逻辑
- 在多线程环境下需要考虑线程安全
- 测试困难：由于全局状态的存在，单例模式可能使单元测试变得复杂

### 单例模式的实现方式

#### 懒汉式单例模式

```java
public class Singleton {
    private static Singleton instance;
    private Singleton() {}
    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```

#### 饿汉式单例模式

```java
public class Singleton {
    private static final Singleton instance = new Singleton();
    private Singleton() {}
    public static Singleton getInstance() {
        return instance;
    }
}
```

#### 双检锁/双重校验锁单例模式

```java
public class Singleton {
    private volatile static Singleton instance;
    private Singleton() {}
    public static Singleton getInstance() {
        if (instance == null) {
            synchronized (Singleton.class) {
                if (instance == null) {
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}
```

### 选择单例模式的依据

在单例模式中，最常见的实现方式是懒汉式和饿汉式。选择哪种方式取决于以下因素：

- 如果实例创建开销较大且不一定会被使用，推荐使用懒汉式
- 如果实例一定会被使用且创建开销较小，推荐使用饿汉式

饿汉式是线程安全的，因为类加载时就完成了实例化；懒汉式则存在线程安全问题，需要额外的同步机制。

使用 `StringToArray` 方法时，它接受一个字符串、一个分隔符和一个可选的布尔值，该布尔值指示是否移除所有空白字符。该方法使用两个私有方法来完成大部分工作——`StringToWhiteSpace` 和 `StringSplit`。这些方法不应该被公开，因为它们不是单例文档接口的一部分，并且不能保证在下一个更新中仍然存在。保持这些方法私有允许你重构所有内部代码而不用担心破坏其他人的程序。

让我们再来看一个例子。在 `StringToArray` 方法中，这是在一个单例对象中使用了一个公共方法，`StringToArray`。该方法接受参数：一个 `string`、一个 `delimiter` 和一个可选的 `boolean`，该布尔值告诉方法是否移除所有白色空格。这个方法使用两个私有方法来完成任务：`StringToWhiteSpace` 和 `StringSplit`。这些方法不应该被公开，因为它们不是单例文档接口的一部分，并且不能保证在下一个更新中仍然存在。保持这些方法私有允许你重构所有内部代码而不用担心破坏其他人的程序。

### 使用闭包

第二种获取私有成员的方法是在一个单例对象中创建一个闭包。这种方式看起来很像我们在第二章中创建真实私有成员的方式，但有一个主要区别。在此之前，我们向构造器主体中添加了变量和函数。而这次，我们是将变量和函数添加到构造器主体中，但没有 `this`。这一区别——将变量和函数添加到构造器主体中，而不是添加到 `this`——使得它们成为私有成员。

### 单例模式总结

单例模式的关键在于确保一个类只有一个实例，并提供全局访问点。它通过私有构造函数、静态实例变量和静态访问方法来控制实例的创建和访问。

单例模式适用于需要控制共享资源访问的场景，但需要谨慎使用，因为不当的实现可能导致线程安全问题和测试困难。

## 使用 `Singleton` 类型的 `Singleton.kt` 文件

`Singleton` 文件位于 `src/main/kotlin` 目录下，是 `Singleton.kt` 示例的一部分。

### 如何声明 `Singleton.kt` 文件

声明 `Singleton.kt` 文件的方法与声明普通类相似。您只需在 `class` 关键字前加上 `object` 关键字。

#### 使用 `Private` 构造器的 `Singleton` 模式

`Singleton.kt` 文件是 `Singleton` 模式的 `Kotlin` 实现。它是一个 `Singleton` 对象，只能被实例化一次。

##### 代码示例: 基本 `Singleton` 声明

```
object Singleton.kt
```

#### 带 `Private` 构造器的 `Singleton` 模式 (第二步)

```
object Singleton.kt
```

### 带有 `Private` 成员变量的 `Singleton` 模式

您还可以为同一个 `Singleton` 对象添加 `Private` 成员变量。

#### 代码示例: 带有 `Private` 成员变量的 `Singleton`

```
object Singleton.kt {
    private var count: Int = 0
    fun incrementCount() {
        count++
    }
    fun getCount(): Int {
        return count
    }
}
```

### 示例：使用 `StringTokenizer` 类

在下面的示例中，我们创建了一个 `StringTokenizer` 对象，并使用它的方法来处理字符串。

`StringTokenizer` 类用于将字符串分割成多个子字符串。

#### `StringTokenizer` 示例

```
StringTokenizer st = new StringTokenizer("this is a test");
while (st.hasMoreTokens()) {
    System.out.println(st.nextToken());
}
```

### `Singleton.kt` 示例：将字符串转换为 `int`

在接下来的示例中，您将学习如何使用 `Integer.parseInt()` 方法将字符串转换为整数。

```
public class StringToIntExample {
    public static void main(String[] args) {
        String str = "123";
        int num = Integer.parseInt(str);
        System.out.println(num);
    }
}
```

# 单例模式的使用

`Singleton` 模式是一种创建型设计模式，它确保一个类只有一个实例，并提供一个全局访问点。

## 例子：实现 `Singleton` 模式

```java
public class Singleton {

    private static Singleton instance;

    private Singleton() {}

    public static Singleton getInstance() {

        if (instance == null) {

            instance = new Singleton();

        }

        return instance;

    }

}
```

## 进阶主题：带参数的 `Singleton` 构造器

您可以通过在 `Singleton` 类中添加一个带有参数的私有构造器，并使用 `getInstance()` 方法来初始化 `Singleton` 实例，从而创建一个带参数的 `Singleton`。

### 示例：带参数的 `Singleton`

```java
public class Singleton {

    private static Singleton instance;

    private String name;

    private Singleton(String name) {

        this.name = name;

    }

    public static Singleton getInstance(String name) {

        if (instance == null) {

            instance = new Singleton(name);

        }

        return instance;

    }

    public String getName() {

        return name;

    }

}
```

## 在 `Kotlin` 中使用 `Singleton`

在 `Kotlin` 中，使用 `object` 关键字声明一个对象，即可创建一个 `Singleton`。`Kotlin` 的 `object` 声明会创建一个类，并且同时创建一个该类的唯一实例。

```kotlin
object MySingleton {

    fun doSomething() {

        println("Doing something")

    }

}
```

## 总结

`Singleton` 模式是一种常用的设计模式，它确保一个类只有一个实例，并提供一个全局访问点。

## 外部链接

### 参考

- [Singleton 模式 - 维基百科](https://zh.wikipedia.org/wiki/单例模式)

# 公共构造函数 `SingleInstance`

## 单例模式的构造函数

通过将构造函数声明为 `private`，可以防止从类外部创建实例。

# 概述

在单例模式中，构造函数被声明为 `private`，这意味着它只能从类内部访问。通过将构造函数设为 `private`，你可以防止外部代码创建新的实例。这样做可以确保类只能有一个实例，从而控制对某个对象的访问。

使用 `private` 构造函数，你可以在类内部管理实例化过程。这种模式通常用于需要全局访问点的场景，比如配置管理器、日志记录器或资源池。通过将构造函数设为 `private`，你可以在类内部控制实例化的细节，而外部代码无法直接创建实例。

### 语法

`private ClassName() { }`

### 关键点

- `Singleton()` 构造函数是 `private` 的，因此无法从类外部访问。
- 类中的唯一实例是通过 `getInstance()` 方法返回的。
- 类的客户端只能通过 `getInstance()` 方法来获取实例。

### 懒汉式实例化（Lazy Initialization）

在某些情况下，你可能不希望实例在类加载时就被创建，而是希望等到第一次需要时才创建。这种模式称为**懒汉式实例化**。使用懒汉式实例化，实例只有在第一次调用 `getInstance()` 方法时才会被创建。

懒加载单例的通用结构如下：

```java
public class Singleton {

    private static Singleton instance = null;

    private Singleton() { }

    public static Singleton getInstance() {

        if (instance == null) {

            instance = new Singleton();

        }

        return instance;

    }

}
```

`getInstance()` 方法首先检查 `instance` 是否为 `null`。如果是，则创建一个新的 `Singleton` 实例并返回；如果不是，则直接返回现有实例。

### 懒汉式 vs 饿汉式

懒汉式实例化的主要优势在于，实例只有在真正需要时才会创建，从而节省资源。然而，懒汉式实现不是线程安全的，如果多个线程同时调用 `getInstance()`，可能会创建多个实例。而饿汉式实现（直接在类加载时创建实例）则是线程安全的，但可能会浪费资源。

### 线程安全

如果你希望在多线程环境中使用懒汉式单例，你需要确保线程安全。一个简单的方法是将 `getInstance()` 方法声明为 `synchronized`。然而，这种方法可能会影响性能，因为每次调用 `getInstance()` 都会同步。更好的方法是使用**双重检查锁定**模式，这可以确保实例只被创建一次，同时避免不必要的同步开销。

### 懒汉式实例化的例子

```java
public class LazySingleton {

    private static LazySingleton instance;

    private LazySingleton() { }

    public static synchronized LazySingleton getInstance() {

        if (instance == null) {

            instance = new LazySingleton();

        }

        return instance;

    }

}
```

### 饿汉式实例化的例子

```java
public class EagerSingleton {

    private static final EagerSingleton instance = new EagerSingleton();

    private EagerSingleton() { }

    public static EagerSingleton getInstance() {

        return instance;

    }

}
```

## 总结

到目前为止，我们已经讨论了单例模式的所有常见实现。它们都有一个共同点：构造函数是 `private` 的。如果你有一个单例，它的构造函数是 `private`，那么所有标准单例代码都适用于这里。

对于懒加载单例，所有标准单例代码也适用。

## 关键点二：构造函数

到目前为止，没有什么改变。第一步是将单例中的所有代码移入构造函数方法中。

```java
MyNamespace.LazySingleton -> func() ->

    func constructor() -> All of the normal singleton code goes here ->

    var privateAttr = false;

    var privateAttr2 = null;

    func privateMethod() -> 

        ...

    privateAttr = true;

    return public singleton instance.
```

`public singleton instance` 是 `private` 的，因此无法从外部访问。

### 懒加载单例

```java
MyNamespace.LazySingleton -> func() ->

    func constructor() -> All of the normal singleton code goes here ->

    var privateAttr = false;

    var privateAttr2 = null;

    func privateMethod() -> 

        ...
```

# 标题

## 创建和分配对象

JavaScript 中的对象是键值对集合，用于存储数据和更复杂的实体。使用对象字面量语法（`{}`）是最简单的创建对象的方式，然后可以使用点或方括号表示法添加、修改或删除属性。

## 创建对象

使用 `new Object()` 语法创建一个空对象。也可以使用对象字面量语法 `{}` 创建。

## 属性操作

### 添加属性

```javascript
let obj = new Object();
obj.key = "value";
```

### 访问属性

```javascript
let obj = new Object();
obj.key;
```

### 删除属性

```javascript
delete obj.key;
```

## 对象字面量

```javascript
let obj = {key: "value"};
```

## 简写属性名

```javascript
let key = "value";
let obj = {key};
```

## 计算属性名

```javascript
let key = "dynamicKey";
let obj = {[key]: "value"};
```

## 方法定义

```javascript
let obj = {
  method() {
    return "Hello";
  }
};
```

## 示例：使用工厂函数创建对象

### 使用工厂函数创建对象

```javascript
function createObject(key, value) {
  return {
    [key]: value
  };
}
let obj = createObject("name", "test");
```

## 属性存在性检查

使用 `hasOwnProperty()` 方法检查对象自身是否包含某个属性。

```javascript
let obj = {key: "value"};
console.log(obj.hasOwnProperty("key")); // true
console.log(obj.hasOwnProperty("other")); // false
```
