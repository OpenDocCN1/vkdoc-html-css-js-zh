# 继承性的类型

## 单继承

Java 只支持单继承，即一个类只能有一个直接父类。单继承简化了对象模型，避免了多重继承带来的复杂性和菱形问题。如果必须实现多个类的特性，您可以使用接口（`Interface`）。

## 多层继承

多层继承指的是一个类继承自另一个类，而后者又继承自第三个类，以此类推。例如：`class A extends B`，`class B extends C`。

## 多重继承（接口实现）

Java 不支持类的多重继承，但支持接口的多重实现。一个类可以同时实现多个接口，从而获得多种行为。接口使用 `implements` 关键字。

```java
interface Flyable {
    void fly();
}
interface Swimmable {
    void swim();
}
class Bird implements Flyable, Swimmable {
    @Override
    public void fly() {
        System.out.println("Bird is flying.");
    }
    @Override
    public void swim() {
        System.out.println("Bird is swimming.");
    }
}
```
