# 排版后的文档

## 使用接口确保完整性

在编写接口时，确保实现该接口的类能够提供所有必需的方法。如果某个类未能实现接口中声明的所有方法，它将被视为抽象类，并且无法使用 `new` 关键字直接实例化。

接口用于定义方法或类型，但只有那些具有正确名称和签名的方法才能通过接口进行调用。

**接口实现的重要性**

对于这种模式，我们采用的是 `第一个` 和 `第三个` 方法的组合。我们使用 `default` 方法。这使我们能够在引入新方法的同时保持向后兼容性，从而在不破坏现有实现的情况下改进接口。

### 类与方法

接口的主要优势在于能够将接口作为参数类型传递，从而允许编写灵活且可重用的代码。

#### `Interface` 构造器

```java
public Interface(String name, Method... methods) throws IllegalArgumentException {
    // 实现细节
}
```

上述构造函数会验证方法名称、方法数量以及方法签名是否匹配。

#### 静态工厂方法

`Interface.ensureImplements` 提供了严格的接口检查机制。

例如：

```java
Interface.newInstance("MyInterface", someMethod, someOtherMethod);
```

如果传递的方法中存在重复名称或参数类型不兼容，则会抛出 `IllegalArgumentException`。

```java
Interface.ensureImplements(
    targetObject, 
    Interface.newStrict("MyStrictChecker", method1, method2)
);
```

上述代码验证 `targetObject` 是否正确实现了 `method1` 和 `method2` 中指定的所有方法。如果不匹配，将抛出 `ImplementationError`。

## 工作原理

该方法会验证 `target` 是否实现了 `Interface` 中定义的所有方法。如果 `target` 缺少其中任何一个方法，就会发生 `ImplementationError`。它首先检查 `target` 的类结构，然后逐一验证每个方法是否可用且可访问。如果验证失败，程序会立刻抛出错误。

`ImplementationError` 消息会明确指出哪个方法未实现，以及如何修复。例如，如果某个方法没有找到，会抛出 `ImplementationError` 并附带具体的错误信息。

```java
ImplementationError: The method 'computeResult' is not implemented in class 'MyCalculator'. Ensure that the method signature matches exactly: public int computeResult(int a, int b).
```

## 接口类

我们使用的接口类如下：

- `Interface`
  - 构造器：`Interface(String name, Method... methods)`
  - 方法：`isImplementable(Object target)`
  - 方法：`ensureImplements(Object target)`
- `InterfaceInstance`
  - 方法：`invoke(String methodName, Object... args)`

## `Interface.ensureImplements`

这是一个严格检查方法实现正确性的工具方法。它首先检查 `targetObject` 的类是否实现了指定方法，然后使用这些方法执行特定操作。如果对象缺少某个方法或者方法签名不匹配，会立即抛出 `ImplementationError`。

```java
public static void ensureImplements(Object target, Method method1, Method method2) {
    // 检查 target 的方法
    if (!target.getClass().getMethods().contains(method1)) {
        throw new ImplementationError("Method " + method1.getName() + " not found.");
    }
    if (!target.getClass().getMethods().contains(method2)) {
        throw new ImplementationError("Method " + method2.getName() + " not found.");
    }
    // 其他验证步骤...
}
```