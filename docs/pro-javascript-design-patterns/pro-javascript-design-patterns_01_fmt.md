# 使用接口进行编程

使用接口进行编程主要好处是可扩展性。通过接口进行编程可以减少后续软件维护的成本。

在 Java 和类似语言中，用于实现多态性的概念称为接口。在接口中定义的类是契约。在接口内部方法的实现发生在实现接口的类中。

## 在 Java 中使用接口进行编程

在 Java 中，接口是一个抽象类型，包含抽象方法的集合。一个类可以通过使用 `implements` 关键字来实现一个接口。

### 使用接口的好处

- 接口用于在 Java 中实现抽象。
- 通过接口可以帮助实现多态性。
- 接口的松散耦合特性有助于实现多重继承。

## 接口变更问题

当向接口添加新方法时，必须修改所有实现该接口的类。为了克服这个问题，Java 8 引入了默认方法。默认方法是那些在接口内部拥有一些默认实现的方法。它们不需要在实现类中被覆盖。

### 静态方法和常量

Java 8 也允许在接口中定义静态方法和常量。

### 函数式接口和 Lambda 表达式

Java 8 还引入了函数式接口和 Lambda 表达式。函数式接口是只包含一个抽象方法的接口。它们也被称为单一抽象方法接口（SAM 接口）。Lambda 表达式提供了函数式接口的实现，这简化了在 Java 中的函数式编程。

### 使用接口进行继承

`Interface` 扩展了 `Inheritance` 的概念。`Class` 可以被视为 `Blueprint`。

### 类与接口的区别

一个类可以继承另一个类，而一个接口可以扩展另一个接口，但一个类可以实现多个接口。

### 类实现接口的语法

使用 `implements` 关键字实现一个接口：

```
class ClassName implements InterfaceName {
    // 方法
}
```

### 一个例子

为了通过使用一个执行具体实现的 `Parent` 类与 `Interface` 交互，可以使用一个 `Parent` 类或一个 `Anonymous Class`。

### 例子代码

假设我们有 `Animal` 接口，该接口包含一个抽象方法 `sound()`。

```
public interface Animal {
    public void sound();
}
```

`Dog` 和 `Cat` 类提供了 `sound` 方法的实现：

```
public class Dog implements Animal {
    public void sound() {
        System.out.println("Dog barks");
    }
}

public class Cat implements Animal {
    public void sound() {
        System.out.println("Cat meows");
    }
}
```

`Main` 类使用这些实现：

```
public class Main {
    public static void main(String[] args) {
        Animal a = new Dog();
        Animal b = new Cat();
        a.sound();
        b.sound();
    }
}
```

上面的程序输出：

```
Dog barks
Cat meows
```

## 接口使用指南

如何使用接口编程？

### 何时使用接口

如果你想创建一些方法，这些方法应该对所有实现类通用，那么使用接口是有益的。这些方法可以被重用。

### 松散耦合

接口有助于实现松散耦合，这意味着一个类不知道其他类的实现细节。

### 多重继承

Java 类不能直接支持多重继承。然而，我们可以通过使用接口来实现多重继承。

### 抽象类与接口

一个类只能继承一个抽象类，但可以实现多个接口。一个抽象类可以有构造方法，而接口不能有构造方法。抽象类可以有实例变量，而接口只能有静态最终变量（常量）。接口中的方法默认是 `public` 的，而抽象类中的方法可以有不同的访问修饰符。

## 接口与继承

- 接口用于实现抽象。
- 接口可以帮助实现多态性。
- 一个接口可以继承另一个接口。

### 接口与类

- 一个类可以继承另一个类，一个接口可以继承另一个接口，但一个类可以实现多个接口。

## 接口继承

继承一个接口：

```
interface Printable {
    void print();
}

interface Showable {
    void show();
}

class A7 implements Printable, Showable {
    public void print() {
        System.out.println("Hello");
    }
    public void show() {
        System.out.println("Welcome");
    }
}
```

上面的例子展示了通过接口实现多重继承。

## 接口的内部实现

对于一个给定的接口，在调用接口中的方法时，JVM 会定位到实现类中的实际方法体。

## 接口与多态性

通过接口可以实现多态性。

### 运行时多态性

Java 中的运行时多态性是通过方法覆盖实现的。在接口的上下文中，一个接口引用可以指向实现该接口的任何类的对象。例如：

```
Animal a = new Dog();
a.sound();
```

## 接口与抽象类的比较

| 特性 | 接口 | 抽象类 |
| :--- | :--- | :--- |
| 多重继承 | 支持 | 不支持 |
| 实例变量 | 不允许（只能是静态常量） | 允许 |
| 构造方法 | 不允许 | 允许 |
| 访问修饰符 | 默认 `public` | 可以有各种访问修饰符 |

### 何时使用抽象类而不是接口

- 当需要定义共享的通用行为时，使用抽象类。
- 当需要定义子类必须实现的行为契约时，使用接口。

## 接口变更问题

如果我们向一个已发布的接口添加一个新方法，那么所有实现该接口的类都必须提供该方法的实现。这可以通过使用默认方法来解决。

### 默认方法

默认方法是在接口内部有默认实现的方法。

```
interface Vehicle {
    default void message() {
        System.out.println("Inside Vehicle");
    }
}
```

## 静态方法

接口中的静态方法属于接口本身，而不是实现类的实例。

```
interface Vehicle {
    static void blowHorn() {
        System.out.println("Blowing horn");
    }
}
```

## 函数式接口与 Lambda 表达式

函数式接口是只包含一个抽象方法的接口。例如 `Runnable`、`Comparator` 等。Lambda 表达式提供了函数式接口的实现。

```
interface Square {
    int calculate(int x);
}

class Test {
    public static void main(String args[]) {
        int a = 5;
        Square s = (int x) -> x * x;
        int ans = s.calculate(a);
        System.out.println(ans);
    }
}
```

## 接口与继承

继承接口：

```
interface Printable {
    void print();
}

interface Showable extends Printable {
    void show();
}

class TestInterface4 implements Showable {
    public void print() {
        System.out.println("Hello");
    }
    public void show() {
        System.out.println("Welcome");
    }
}
```

## 接口与封装

接口的松散耦合特性有助于封装。

## 接口与内部类

接口可以包含内部类和内部接口。这些内部类和接口被隐式地视为 `public static`。

## 标记接口

一个没有任何成员的接口被称为标记接口（Tag/Marker Interface）。例如 `Serializable`、`Cloneable`。标记接口用于向 JVM 或编译器提供一些元信息或运行时信息。

## 接口与常量

接口可以包含常量（`public static final` 变量）。这些常量可以通过 `InterfaceName.CONSTANT_NAME` 来访问。

```
# 接口基础

```java
interface Constants {
    int MAX_VALUE = 100;
    String DEFAULT_NAME = "Unknown";
}
```

## 接口与枚举

枚举可以实现接口。

```java
interface Print {
    void print();
}

enum Color implements Print {
    RED, GREEN, BLUE;

    public void print() {
        System.out.println(this.name());
    }
}
```

## 接口与注解

注解是一种特殊的接口。它们以 `@` 符号开头，用于提供元数据。

## 接口与泛型

泛型可以与接口一起使用来创建类型安全的代码。

```java
interface Container<T> {
    void add(T item);
    T get(int index);
}
```

## 接口与设计模式

许多设计模式，如策略模式、观察者模式和工厂模式，都广泛使用接口。

### 策略模式

策略模式允许在运行时选择算法的行为。
```