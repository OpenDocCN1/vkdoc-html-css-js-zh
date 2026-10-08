# 核心原则

## 接口与实现

Java 使用接口以一种典型的方式来实现大多数面向对象语言的机制，因此我们将从这里开始。

这里有一个来自 `java.io` 包中的接口示例：`DataOutput`。

- `void writeBoolean(boolean value)` 抛出 `IOException`
- `void writeByte(int value)` 抛出 `IOException`
- `void writeChar(int value)` 抛出 `IOException`
- `void writeInt(int value)` 抛出 `IOException`
- `void writeDouble(int value)` 抛出 `IOException`

它是一个方法列表，一个类应该通过这些方法和参数以及异常来实现。每个方法看起来都类似于一个方法声明，唯一不同的是它以分号结束，而不是一对花括号。

创建使用此接口的类需要实现 `DataOutput` 中的所有方法。一个实现 `DataOutput` 的公共类 `DataOutputStream` 扩展了 `FilterOutputStream` 并实现了 `DataOutput`。

```
public class DataOutputStream extends FilterOutputStream implements DataOutput
```

- `public final void writeBoolean(boolean v)` 抛出 `IOException`
- `public final void writeByte(int v)` 抛出 `IOException`
- `public final void writeShort(int v)` 抛出 `IOException`
- `public final void writeChar(int v)` 抛出 `IOException`
- `public final void writeInt(int v)` 抛出 `IOException`

```
public class RandomAccessFile implements DataOutput
```

- `public final void writeBoolean(boolean v)` 抛出 `IOException`
- `public final void writeByte(int v)` 抛出 `IOException`
- `public final void writeShort(int v)` 抛出 `IOException`
- `public final void writeChar(int v)` 抛出 `IOException`
- `public final void writeDouble(int v)` 抛出 `IOException`

## 无需方法声明

# 核心原则

## 接口与实现

列表中列出的每个方法都在接口中声明并具体实现。如果有任何方法没有被实现，则在编译时会显示一个错误。以下是 Java 编译器可能会显示的输出的样子：

- `Syntax error on token(s), misplaced construct(s)`
- `Fatal error: Class BadClass contains 0 abstract methods and must be the abstract class`

`BadClass` 类本身必须是抽象的。因此，`BadClass` 类必须被声明为抽象类，因为它没有为 `writeBoolean()`、`writeByte()`、`writeChar()`、`writeInt()` 或 `writeDouble()` 提供实现。

## 类的声明

语法上，一个类使用 `interface ClassName` 来声明接口。

- `public` 方法 `interfaceMethod(ArgumentOne, ArgumentTwo)`

`class BadClass implements BadInterface`：

- `public` 方法 `interfaceMethod(ArgumentOne, ArgumentTwo)`
- 返回 `ArgumentOne`，`ArgumentTwo`

`class BadClass implements BadInterface`：

- 导致此错误在运行时出现

## 无需方法声明

`BadClass` 类导致此错误在运行时出现：

- `Runtime error: Class BadClass contains 0 abstract methods and must be the abstract class`
- `Fatal error: Class BadClass contains abstract methods and must be the abstract class`

## 使用 `Interface` 实现多态

在 `Interface` 中，`Interface` 是一种约定。

`Interface` 的 `Interface` 定义了一组方法签名，而 `Interface` 的实现负责提供具体的行为。

```
class Interface {
    method addChild(child) {}
    method removeChild(child) {}
    method getChild(index) {}
}
```

```
class Class extends Interface {
    method addChild(child) {
        // 实现添加子节点的逻辑
    }
}
```

```
class Class extends Interface {
    method removeChild(child) {
        // 实现移除子节点的逻辑
    }
}
```

```
class Class extends Interface {
    method getChild(index) {
        // 实现获取子节点的逻辑
    }
}
```

```
var CompositeForm extends Interface {
    method save(id, method, action) {}
}
```

## 使用 `Attributes` 进行校验

`Attributes` 提供了一种声明式的方式来定义 `Interface` 的行为规则。

## 对 `Interface` 的限制

`Interface` 只能包含方法签名，不能包含实例变量或方法实现。所有实现 `Interface` 的类都必须实现 `Interface` 中定义的所有方法，否则编译器会报错。如果尝试实例化一个 `Interface`，也会导致编译错误。

## 实现多个 `Interface`

`Java` 类可以实现多个 `Interface`，这使得 `Java` 能够模拟多重继承，同时避免了与多重继承相关的复杂性和歧义性问题。

## 使用 `Attributes` 进行运行时检查

所有实现 `Interface` 的类都必须遵循 `Interface` 定义的约定。利用 `Interface` 类型，可以在运行时检查对象是否实现了某个 `Interface`，从而实现动态行为。由于这种检查是在运行时进行的，它不会影响编译时的类型安全。

## 总结

在面向对象的程序设计中，`Interface` 用于定义对象的行为契约。它不关心对象的具体内部状态，只关心对象能做什么。

`Interface` 在 `Java` 中的实现有以下三个关键点：对象的行为（即方法签名）、对象之间的交互方式，以及通过 `Interface` 实现多态。

### 描述 `Interface` 的常见应用场景

最简单且最有效的使用 `Interface` 的方式是用于单元测试。通过模拟 `Interface` 的实现，可以轻松地替换真实依赖，从而隔离测试目标。这种解耦方式便于进行依赖注入和控制反转。

`Interface` 也是实现多重继承的主要手段，并且是运行时检查（例如通过 `instanceof` 关键字）的基础。在 `Java` 中，`Interface` 本身不包含任何状态，因此无法通过 `Interface` 来共享实例变量。它主要用于定义类型和操作，提供了一种灵活的方式来组织代码。

### 使用 `Attributes` 进行运行时检查

所有实现 `Interface` 的类都被视为该 `Interface` 类型。因此，通过 `Interface` 类型的引用，可以调用该 `Interface` 中定义的所有方法。任何实现 `Interface` 的类都可以在需要 `Interface` 类型的地方使用。

### 示例：检查一个对象是否实现了某个 `Interface`

```java
if (obj instanceof MyInterface) {
    MyInterface myObj = (MyInterface) obj;
    myObj.myMethod();
}
```

### 为什么使用 `Interface` 而不是抽象类？

`Interface` 提供了比抽象类更强的抽象能力。抽象类可以包含部分实现，而 `Interface` 则完全专注于定义契约。`Interface` 支持多重继承，而类只能继承一个抽象类。

### 什么时候使用 `Interface`？

当你想要定义一组相关但可能由不同类实现的方法时，应该使用 `Interface`。例如，定义可比较的对象、可序列化的对象或可克隆的对象。当不同类之间没有明显的继承关系，但需要共享一组共同的行为时，`Interface` 是理想的选择。

### 在 `Java` 中使用 `Interface` 实现多重继承

```java
public class MyClass implements InterfaceA, InterfaceB {
    // 实现 InterfaceA 和 InterfaceB 的方法
}
```

### 使用 `Interface` 实现多态

```java
public class Animal {
    public void makeSound() {
        System.out.println("动物发出声音");
    }
}
```

```java
public class Dog extends Animal {
    @Override
    public void makeSound() {
        System.out.println("狗叫");
    }
}
```

```java
public class Cat extends Animal {
    @Override
    public void makeSound() {
        System.out.println("猫叫");
    }
}
```

```java
public class Main {
    public static void main(String[] args) {
        Animal myAnimal = new Dog();
        myAnimal.makeSound(); // 输出“狗叫”
        myAnimal = new Cat();
        myAnimal.makeSound(); // 输出“猫叫”
    }
}
```

## 使用 `Interface` 实现依赖注入

```java
public interface MessageService {
    void sendMessage(String message, String recipient);
}

public class EmailService implements MessageService {
    @Override
    public void sendMessage(String message, String recipient) {
        System.out.println("发送邮件给 " + recipient + ": " + message);
    }
}

public class SMSService implements MessageService {
    @Override
    public void sendMessage(String message, String recipient) {
        System.out.println("发送短信给 " + recipient + ": " + message);
    }
}

public class NotificationService {
    private MessageService messageService;

    public NotificationService(MessageService messageService) {
        this.messageService = messageService;
    }

    public void notify(String message, String recipient) {
        messageService.sendMessage(message, recipient);
    }
}
```

## 测试 `Interface` 实现

```java
public class MockMessageService implements MessageService {
    @Override
    public void sendMessage(String message, String recipient) {
        System.out.println("模拟发送消息");
    }
}
```

```java
// 在测试中使用 MockMessageService
MessageService mockService = new MockMessageService();
NotificationService notificationService = new NotificationService(mockService);
notificationService.notify("测试消息", "test@example.com");
```

## 使用 `Interface` 实现回调

```java
public interface Callback {
    void onComplete(String result);
}

public class Task {
    public void execute(Callback callback) {
        // 执行任务
        callback.onComplete("任务完成");
    }
}

public class Main {
    public static void main(String[] args) {
        Task task = new Task();
        task.execute(new Callback() {
            @Override
            public void onComplete(String result) {
                System.out.println(result);
            }
        });
    }
}
```

## 使用 `Interface` 实现策略模式

```java
public interface SortingStrategy {
    void sort(int[] array);
}

public class BubbleSort implements SortingStrategy {
    @Override
    public void sort(int[] array) {
        System.out.println("使用冒泡排序");
    }
}

public class QuickSort implements SortingStrategy {
    @Override
    public void sort(int[] array) {
        System.out.println("使用快速排序");
    }
}

public class Sorter {
    private SortingStrategy strategy;

    public Sorter(SortingStrategy strategy) {
        this.strategy = strategy;
    }

    public void sort(int[] array) {
        strategy.sort(array);
    }
}
```

## 总结

`Interface` 是面向对象设计中实现多态和代码解耦的强大工具。它允许你定义契约，而无需关心具体实现。通过 `Interface`，你可以编写更灵活、更可测试、更易于维护的代码。
