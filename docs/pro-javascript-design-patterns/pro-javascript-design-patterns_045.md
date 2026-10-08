# 对象的使用场景

## 作为字典/映射

```javascript
let dict = {
  key1: "value1",
  key2: "value2"
};
console.log(dict["key1"]); // value1
```

## 作为配置对象

```javascript
let config = {
  url: "https://example.com",
  timeout: 5000
};
```

## 作为数据模型

```javascript
let user = {
  id: 1,
  name: "Alice",
  email: "alice@example.com"
};
```

## 最终总结

这种方法使得构造函数从类外部不可访问，这是一件好事。你想要完全控制何时调用它。`public getInstance()` 方法用于实现单例模式。

# 最佳实践

- 使用对象字面量语法（`{}`）而不是 `new Object()` 创建对象。
- 使用 `hasOwnProperty()` 检查属性是否存在于对象自身，而不是原型链上。
- 使用类语法（`class`）定义需要复用的对象结构。
- 使用属性的简洁表示法（`{key}`）和计算属性名（`{[expression]: value}`）提高代码可读性。

# 总结

对象是 JavaScript 的核心概念，用于存储键值对和构建复杂的数据结构。通过对象字面量、构造函数、`Object.create()` 和 `class` 语法，可以灵活地创建和管理对象。理解原型链是掌握 JavaScript 对象模型的关键，而现代 JavaScript 中的类语法提供了更清晰、更面向对象的方式来组织代码。

# 概述

在 Java 中，`Singleton` 模式是一种创建型设计模式，它确保一个类只有一个实例，并提供一个全局访问点来访问该实例。这种模式在许多场景中都非常有用，例如配置管理、日志记录、数据库连接池等。

Singleton 模式的核心思想是：将类的构造函数设为私有，以防止外部直接创建对象。然后，类内部创建一个私有的静态实例，并通过一个公共的静态方法（通常命名为 `getInstance()`）来返回该实例。

## 什么时候使用 Singleton 模式？

当系统中需要且仅需要一个全局唯一的对象来协调某些操作时，就可以考虑使用 Singleton 模式。例如，一个线程池、缓存、或者日志系统，通常只需要一个管理者来负责全局的资源调度。

Singleton 模式通过提供一个单一的访问点，可以有效地组织相关的方法和属性，并防止它们被意外地多次实例化或与其他全局变量发生冲突。这有助于减少内存消耗并提高性能，尤其是在大型或复杂的项目中。

## Singleton 模式的变体

Java 中常见的 Singleton 实现方式包括：

### 懒汉式（Lazy Initialization）
在 `getInstance()` 方法被首次调用时才创建实例。这种方式可以延迟对象的初始化，但需要注意线程安全问题。

### 饿汉式（Eager Initialization）
在类加载时就创建实例。这种方式实现简单，天然线程安全，但可能会造成资源浪费，如果该实例从未被使用的话。

### 双重检查锁定（Double-Checked Locking）
结合了懒加载和线程安全，通过两次检查实例是否为空来减少同步的开销。这是 Java 中非常常用的一种方式。

### 静态内部类（Bill Pugh Singleton）
利用 Java 的静态内部类机制，在 `getInstance()` 被调用时才加载内部类并创建实例。这种方式既实现了懒加载，又保证了线程安全，且代码简洁。

### 枚举（Enum Singleton）
这是 Joshua Bloch 在《Effective Java》中推荐的实现方式。它简洁、线程安全、且能防止反序列化和反射攻击。

## Singleton 模式的优势

- **控制实例数量**：严格限制类的实例化，确保只有一个对象存在。
- **提供全局访问点**：提供一个单一的、众所周知的访问点来获取该实例。
- **延迟实例化**：通过懒汉式或内部类等方式，可以做到在需要时才创建对象，优化资源使用。
- **减少命名空间冲突**：将相关的方法和属性组织在一个单一实例中，避免了全局变量的混乱。
- **易于测试和维护**：虽然单例本身可能难以测试，但通过良好的设计（如依赖注入）可以缓解这个问题。

## Singleton 模式的缺点

- **违反单一职责原则**：单例类同时负责自己的创建和业务逻辑，可能违反单一职责原则。
- **难以扩展**：单例类很难被继承或扩展，因为它的构造函数是私有的。
- **隐藏依赖**：使用单例的代码隐式地依赖于该单例，降低了代码的透明度和可测试性。
- **生命周期管理**：单例的生命周期由 JVM 控制，难以手动管理，可能会导致资源泄漏（如果持有外部资源）。

## 总结

Singleton 模式是一个强大且常用的设计模式，它通过限制类的实例化来确保全局唯一性。它最适合用于那些需要控制对共享资源的访问、提供全局协调点或优化性能的场景。虽然它并非适用于所有情况，但它是 Java 开发者工具箱中一个重要的工具，尤其是在大型或分布式系统中。

通过将相关的方法和属性组织在一个单一的、可重用的组件中，Singleton 模式可以帮助你写出更清晰、更易于维护的代码。

# JavaScrip 与单例模式

单例是一种创建型设计模式。它确保一个类只有一个实例，并提供一个全局访问点来访问该实例。

## 为什么需要单例模式

有些对象，比如线程池、缓存、对话框等，我们只需要一个实例。多个实例可能会导致不一致的状态或资源浪费。

虽然单例模式常用于实现全局变量，但我们应该避免过度使用，因为它会引入紧密耦合的代码，降低测试的灵活性。

## 单例模式的实现方法

在 Java 中，单例模式有多种实现方式，包括懒汉式、饿汉式、双重检查锁定、静态内部类和枚举等。

### 基本实现

最简单的单例模式如下：

- 将构造器设为私有，防止外部实例化。
- 通过一个公共的静态方法返回唯一的实例。

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

### 懒汉式 vs 饿汉式

- **懒汉式**：在 `getInstance()` 第一次被调用时才创建实例。优点是延迟加载，节省资源；缺点是线程不安全。
- **饿汉式**：在类加载时就创建实例。优点是简单且线程安全；缺点是可能造成资源浪费。

```java
// 饿汉式
public class Singleton {
    private static final Singleton INSTANCE = new Singleton();
    private Singleton() {}
    public static Singleton getInstance() {
        return INSTANCE;
    }
}
```

### 双重检查锁定（`Double-Checked Locking`）

这是一种优化的懒汉式实现，结合了延迟加载和线程安全。使用 `volatile` 关键字确保多线程环境下的可见性。

```java
public class Singleton {
    private static volatile Singleton instance;
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

### 静态内部类（`Bill Pugh` 实现）

这种方式利用了 Java 的类加载机制，既实现了延迟加载，又保证了线程安全。

```java
public class Singleton {
    private Singleton() {}
    private static class Holder {
        private static final Singleton INSTANCE = new Singleton();
    }
    public static Singleton getInstance() {
        return Holder.INSTANCE;
    }
}
```

### 枚举实现

这是《Effective Java》作者推荐的方式。它不仅可以防止反射攻击，还能保证序列化的安全性。

```java
public enum Singleton {
    INSTANCE;
    public void doSomething() {
        // 业务逻辑
    }
}
```

# 总结

单例模式是一种基础但强大的设计模式。它的核心目的是确保一个类只有一个实例，并提供一个全局访问点。在 Java 中，我们可以通过枚举、静态内部类、双重检查锁定等方式来实现。

选择哪种实现方式取决于具体的需求。例如，如果不需要延迟加载，饿汉式是最简单的；如果需要延迟加载且考虑线程安全，静态内部类或双重检查锁定是不错的选择。

# 附录

## 单例模式的结构

```
┌─────────────────────────────────────┐
│             Singleton               │
├─────────────────────────────────────┤
│ - instance: Singleton               │
├─────────────────────────────────────┤
│ - Singleton()                       │
│ + getInstance(): Singleton          │
└─────────────────────────────────────┘
```

```
// 伪代码表示
function getInstance() {
    if (instance == null) {
        instance = new Singleton();
    }
    return instance;
}
```

## 单例模式的优缺点

**优点**：
- 确保一个类只有一个实例，节约资源。
- 提供全局访问点，方便控制。

**缺点**：
- 违反了单一职责原则（一个类既要管理自己的实例，又要处理业务逻辑）。
- 在多线程环境下，需要额外处理线程安全问题。
- 不利于单元测试，因为它引入了全局状态。

## 何时使用单例模式

当你需要确保某个类在整个应用中只有一个实例时，可以考虑使用单例模式。常见的应用场景包括：

- 配置管理器
- 日志记录器
- 线程池
- 数据库连接池
- 文件系统

---

*单例模式是设计模式中最基础的模式之一，但它也常被误用。建议在真正需要时才使用它，避免为了“全局访问”而滥用。*

# 目录

## 使用回调函数从链表中检索数据

### 方法

在某些情况下，使用回调函数（`callback`）可以简化数据处理。通过将回调函数作为参数传递给`filter()`方法，可以创建灵活的数据过滤逻辑。

例如，`Array.prototype.filter()`方法接受一个回调函数，该函数对数组中的每个元素执行测试，并返回通过测试的元素组成的新数组。

```javascript
[1, 2, 3, 4, 5].filter(function(num) {
    return num % 2 === 0;
})
```

在某些情况下，使用回调函数（`callback`）可以简化数据检索。通过将回调函数作为参数传递给`find()`方法，可以创建灵活的数据查找逻辑。

```javascript
[1, 2, 3, 4, 5].find(function(num) {
    return num > 3;
})
```

## 方法签名

### 函数签名

`funcName(args)`、`ClassName.method(args)`、`namespace.funcName(args)`都需要添加反引号。

```javascript
functionName(args)
```

## 自定义回调函数

### 自定义回调函数示例

如果你需要更复杂的逻辑，可以定义自己的回调函数，并将其作为参数传递给`filter()`或`find()`方法。

```javascript
function isEven(num) {
    return num % 2 === 0;
}

[1, 2, 3, 4, 5].filter(isEven)
```

## 示例代码

### 示例：使用回调函数

```javascript
[1, 2, 3, 4, 5].filter(function(num) {
    return num % 2 === 0;
})
```

```javascript
[1, 2, 3, 4, 5].find(function(num) {
    return num > 3;
})
```

## 总结

在某些情况下，使用回调函数（`callback`）可以简化数据处理。通过将回调函数作为参数传递给`filter()`方法，可以创建灵活的数据过滤逻辑。

例如，`Array.prototype.filter()`方法接受一个回调函数，该函数对数组中的每个元素执行测试，并返回通过测试的元素组成的新数组。

```javascript
[1, 2, 3, 4, 5].filter(function(num) {
    return num % 2 === 0;
})
```

在某些情况下，使用回调函数（`callback`）可以简化数据检索。通过将回调函数作为参数传递给`find()`方法，可以创建灵活的数据查找逻辑。

```javascript
[1, 2, 3, 4, 5].find(function(num) {
    return num > 3;
})
```

例如，`Array.prototype.filter()`方法接受一个回调函数，该函数对数组中的每个元素执行测试，并返回通过测试的元素组成的新数组。

```javascript
[1, 2, 3, 4, 5].filter(function(num) {
    return num % 2 === 0;
})
```

## 使用回调函数从链表中检索数据

### 方法

在某些情况下，使用回调函数（`callback`）可以简化数据检索。通过将回调函数作为参数传递给`find()`方法，可以创建灵活的数据查找逻辑。

```javascript
[1, 2, 3, 4, 5].find(function(num) {
    return num > 3;
})
```

### 方法

在某些情况下，使用回调函数（`callback`）可以简化数据处理。通过将回调函数作为参数传递给`filter()`方法，可以创建灵活的数据过滤逻辑。

例如，`Array.prototype.filter()`方法接受一个回调函数，该函数对数组中的每个元素执行测试，并返回通过测试的元素组成的新数组。

```javascript
[1, 2, 3, 4, 5].filter(function(num) {
    return num % 2 === 0;
})
```

### 自定义回调函数

如果你需要更复杂的逻辑，可以定义自己的回调函数，并将其作为参数传递给`filter()`或`find()`方法。

```javascript
function isEven(num) {
    return num % 2 === 0;
}

[1, 2, 3, 4, 5].filter(isEven)
```

### 示例代码

```javascript
[1, 2, 3, 4, 5].filter(function(num) {
    return num % 2 === 0;
})
```

```javascript
[1, 2, 3, 4, 5].find(function(num) {
    return num > 3;
})
```

## 总结

在某些情况下，使用回调函数（`callback`）可以简化数据处理。通过将回调函数作为参数传递给`filter()`方法，可以创建灵活的数据过滤逻辑。

例如，`Array.prototype.filter()`方法接受一个回调函数，该函数对数组中的每个元素执行测试，并返回通过测试的元素组成的新数组。

```javascript
[1, 2, 3, 4, 5].filter(function(num) {
    return num % 2 === 0;
})
```

### 方法

在某些情况下，使用回调函数（`callback`）可以简化数据检索。通过将回调函数作为参数传递给`find()`方法，可以创建灵活的数据查找逻辑。

```javascript
[1, 2, 3, 4, 5].find(function(num) {
    return num > 3;
})
```

### 方法

在某些情况下，使用回调函数（`callback`）可以简化数据处理。通过将回调函数作为参数传递给`filter()`方法，可以创建灵活的数据过滤逻辑。

例如，`Array.prototype.filter()`方法接受一个回调函数，该函数对数组中的每个元素执行测试，并返回通过测试的元素组成的新数组。

```javascript
[1, 2, 3, 4, 5].filter(function(num) {
    return num % 2 === 0;
})
```

### 自定义回调函数

如果你需要更复杂的逻辑，可以定义自己的回调函数，并将其作为参数传递给`filter()`或`find()`方法。

```javascript
function isEven(num) {
    return num % 2 === 0;
}

[1, 2, 3, 4, 5].filter(isEven)
```

### 示例代码

```javascript
[1, 2, 3, 4, 5].filter(function(num) {
    return num % 2 === 0;
})
```

```javascript
[1, 2, 3, 4, 5].find(function(num) {
    return num > 3;
})
```

# 使用回调函数从链表中检索数据

在某些情况下，使用回调函数（`callback`）可以简化数据处理。通过将回调函数作为参数传递给`filter()`方法，可以创建灵活的数据过滤逻辑。

例如，`Array.prototype.filter()`方法接受一个回调函数，该函数对数组中的每个元素执行测试，并返回通过测试的元素组成的新数组。

```javascript
[1, 2, 3, 4, 5].filter(function(num) {
    return num % 2 === 0;
})
```

## 方法

在某些情况下，使用回调函数（`callback`）可以简化数据检索。通过将回调函数作为参数传递给`find()`方法，可以创建灵活的数据查找逻辑。

```javascript
[1, 2, 3, 4, 5].find(function(num) {
    return num > 3;
})
```

## 自定义回调函数

如果你需要更复杂的逻辑，可以定义自己的回调函数，并将其作为参数传递给`filter()`或`find()`方法。

```javascript
function isEven(num) {
    return num % 2 === 0;
}

[1, 2, 3, 4, 5].filter(isEven)
```

## 示例代码

```javascript
[1, 2, 3, 4, 5].filter(function(num) {
    return num % 2 === 0;
})
```

```javascript
[1, 2, 3, 4, 5].find(function(num) {
    return num > 3;
})
```

# 使用回调函数从链表中检索数据

## 方法

在某些情况下，使用回调函数（`callback`）可以简化数据处理。通过将回调函数作为参数传递给`filter()`方法，可以创建灵活的数据过滤逻辑。

例如，`Array.prototype.filter()`方法接受一个回调函数，该函数对数组中的每个元素执行测试，并返回通过测试的元素组成的新数组。

```javascript
[1, 2, 3, 4, 5].filter(function(num) {
    return num % 2 === 0;
})
```

通过将回调函数作为参数传递给`find()`方法，可以创建灵活的数据查找逻辑。

```javascript
[1, 2, 3, 4, 5].find(function(num) {
    return num > 3;
})
```

## 自定义回调函数

如果你需要更复杂的逻辑，可以定义自己的回调函数，并将其作为参数传递给`filter()`或`find()`方法。

```javascript
function isEven(num) {
    return num % 2 === 0;
}

[1, 2, 3, 4, 5].filter(isEven)
```

## 示例代码

```javascript
[1, 2, 3, 4, 5].filter(function(num) {
    return num % 2 === 0;
})
```

```javascript
[1, 2, 3, 4, 5].find(function(num) {
    return num > 3;
})
```

```markdown
# 使用回调函数从链表中检索数据

在某些情况下，使用回调函数（`callback`）可以简化数据处理。通过将回调函数作为参数传递给`filter()`方法，可以创建灵活的数据过滤逻辑。

例如，`Array.prototype.filter()`方法接受一个回调函数，该函数对数组中的每个元素执行测试，并返回通过测试的元素组成的新数组。

```javascript
[1, 2, 3, 4, 5].filter(function(num) {
    return num % 2 === 0;
})
```

在某些情况下，使用回调函数（`callback`）可以简化数据检索。通过将回调函数作为参数传递给`find()`方法，可以创建灵活的数据查找逻辑。

```javascript
[1, 2, 3, 4, 5].find(function(num) {
    return num > 3;
})
```
