# 类型检查与类型推断

在许多类型安全的编程语言中，通常依赖类型检查和类型推断来确保程序的正确性。下面是一个关于如何进行类型检查与类型推断的方法的示例，通过两种不同的实现来说明。

## 两种方法

1. **手动类型检查**：你编写显式的代码，通过操作符（例如 `instanceof` 或 `type()`）来检查一个对象是否是特定类型的实例。
2. **类型推断与自动检查**：编译器或运行时环境自动推断变量的类型，并确保类型转换的安全性。如果类型不匹配，系统会抛出错误。

假设你需要在运行时检查一个对象的类型，并基于该类型执行不同的操作。

```java
// 使用类型检查
public void processObject(Object obj) {
    if (obj instanceof String) {
        // 针对字符串的处理
        System.out.println("Processing string: " + obj);
    } else if (obj instanceof Integer) {
        // 针对整数的处理
        System.out.println("Processing integer: " + obj);
    } else {
        throw new IllegalArgumentException("Unsupported type");
    }
}
```

现在，我们通过一个更高级的、使用 `instanceof` 和模式匹配（Java 16+）的例子来展示一个更现代和安全的方式。

```java
public void processObjectModern(Object obj) {
    if (obj instanceof String s) {
        // 模式匹配，自动将 obj 转为 String 并赋值给 s
        System.out.println("Processing string: " + s);
    } else if (obj instanceof Integer i) {
        // 模式匹配，自动将 obj 转为 Integer 并赋值给 i
        System.out.println("Processing integer: " + i);
    } else {
        System.out.println("Unknown type");
    }
}
```

上面 `processObject` 方法接受一个参数，首先判断它是否是 `String` 类型，如果是则执行字符串逻辑。如果不是，再判断它是否是 `Integer` 类型，如果是则执行整数逻辑。如果类型不匹配，则抛出一个异常或执行默认逻辑。

与手动检查相比，这种方式更加简洁和类型安全。如果 `obj` 不包含预期的方法，你可以在编译期或运行期获得清晰的错误信息，减少了在运行时发生 `ClassCastException` 的风险。

## 局限性

虽然这种基于子类型或反射的 `instanceof` 检查是处理多类型对象的常用方法，但它也有一些潜在的问题。它要求你对所有可能的类型进行显式的判断，导致代码不够灵活（违反了“开闭原则”）。但它非常适合处理那些有限的、已知类型的集合。

总的来说，`interface` 和类型检查是实现多态和需要判断对象类型的场景下的两种基础且重要的技术手段。
