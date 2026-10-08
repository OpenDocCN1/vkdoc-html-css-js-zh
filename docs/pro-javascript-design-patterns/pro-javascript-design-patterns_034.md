# 如何使用继承性

在 Java 中，我们可以通过 `extends` 关键字来实现继承。被继承的类称为父类或基类，继承的类称为子类或派生类。

Java 不支持类的多重继承（即一个类不能继承多个类），但支持多层继承。例如：`class A extends B`，而 `class B extends C` 是允许的。

继承性允许子类访问父类中 `public` 或 `protected` 修饰的属性和方法。同时，子类可以重写（`Override`）父类中的方法，以实现更具体的功能。

## 示例：使用继承性

```java
// 父类
class Animal {
    public void eat() {
        System.out.println("This animal eats food.");
    }
}

// 子类
class Dog extends Animal {
    public void bark() {
        System.out.println("The dog barks.");
    }
}
```

## 继承性的核心概念

- `super` 关键字：用于调用父类的构造方法、方法或属性。
- 方法重写（`Override`）：子类可以重新定义父类中已有的方法。
- `final` 关键字：如果类被 `final` 修饰，则不能被继承；如果方法被 `final` 修饰，则不能被子类重写。
- 抽象类（`abstract class`）：不能被实例化，只能被继承，可以包含抽象方法和具体方法。

## 继承性与组合的选择

在 Java 中，组合（`Composition`）是实现代码复用的另一种方式。通过在一个类中持有另一个类的实例，而非通过继承来复用功能。组合提供了更大的灵活性，减少了类之间的耦合，更易于维护。

因此，在实践中，当我们面对“有一个”而非“是一个”的关系时，优先考虑组合而非继承。

## 示例：组合优于继承

```java
class Engine {
    public void start() {
        System.out.println("Engine started.");
    }
}
class Car {
    private Engine engine; // 组合
    public Car() {
        this.engine = new Engine();
    }
    public void start() {
        engine.start();
    }
}
```

---
