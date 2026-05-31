# 装饰器模式

## 意图

动态地给一个对象添加一些额外的职责。就增加功能来说，装饰器模式相比生成子类更为灵活。

## 适用性

以下情况可以使用装饰器模式：

*   在不影响其他对象的情况下，以动态、透明的方式给单个对象添加职责。
*   处理那些可以撤销的职责。
*   当不能采用生成子类的方法进行扩充时。一种情况是，可能有大量独立的扩展，为支持每一种组合将产生大量的子类，使得子类数目呈爆炸性增长。另一种情况是因为类定义被隐藏，或类定义不能用于生成子类。

## 结构

### 参与者

*   `Component`（`Warrior`）
    *   定义一个对象接口，可以给这些对象动态地添加职责。
*   `ConcreteComponent`（`SimpleWarrior`）
    *   定义一个对象，可以给这个对象添加一些职责。
*   `Decorator`（`WeaponDecorator`）
    *   维持一个指向`Component`对象的指针，并定义一个与`Component`接口一致的接口。
*   `ConcreteDecorator`（`SwordDecorator`, `ShieldDecorator`）
    *   向组件添加职责。

### 类图

```
           ┌──────────┐
           │ Component │
           ├──────────┤
           │+attack() │
           └──────────┘
                 ▲
                 │
           ┌──────────┐
           │ Decorator │
           ├──────────┤
           │-component: Component │
           ├──────────┤
           │+attack()  │
           └──────────┘
                 ▲
    ┌────────────┼────────────┐
    │            │            │
┌──────────┐ ┌──────────┐ ┌──────────┐
│Concrete  │ │Concrete  │ │Concrete  │
│Component │ │DecoratorA│ │DecoratorB│
├──────────┤ ├──────────┤ ├──────────┤
│+attack() │ │+attack() │ │+attack() │
└──────────┘ └──────────┘ └──────────┘
```

## 协作者

*   `Decorator` 将请求转发给它的 `Component` 对象，并可能在转发前后执行一些额外的操作。

## 效果

1.  比静态继承更灵活。
2.  避免在层次结构高层的类有太多的特征。
3.  装饰器与它的组件不相同。
4.  有许多小对象。

## 实现

### 适用以下问题：

*   接口一致性。装饰对象的接口必须与它所装饰的`Component`的接口一致。
*   省略抽象的`Decorator`类。
*   保持`Component`类简单（`Component`类应该只定义接口，而不是存储数据）。
*   改变对象外壳和改变对象内核（如果`Component`很重，那么`Decorator`模式太昂贵了）。

## 示例

### `Warrior` 接口

```java
public interface Warrior {
    int attack();
}
```

### `SimpleWarrior` 类

```java
public class SimpleWarrior implements Warrior {
    @Override
    public int attack() {
        return 10;
    }
}
```

### `WeaponDecorator` 抽象类

```java
public abstract class WeaponDecorator implements Warrior {
    protected Warrior warrior;

    public WeaponDecorator(Warrior warrior) {
        this.warrior = warrior;
    }

    @Override
    public int attack() {
        return warrior.attack();
    }
}
```

### `SwordDecorator` 类

```java
public class SwordDecorator extends WeaponDecorator {
    public SwordDecorator(Warrior warrior) {
        super(warrior);
    }

    @Override
    public int attack() {
        return super.attack() + 15;
    }
}
```

### `ShieldDecorator` 类

```java
public class ShieldDecorator extends WeaponDecorator {
    public ShieldDecorator(Warrior warrior) {
        super(warrior);
    }

    @Override
    public int attack() {
        return super.attack() + 5;
    }
}
```

### 使用示例

```java
Warrior warrior = new SimpleWarrior();
System.out.println("Base attack: " + warrior.attack()); // 10

warrior = new SwordDecorator(warrior);
System.out.println("With Sword: " + warrior.attack()); // 25

warrior = new ShieldDecorator(warrior);
System.out.println("With Sword and Shield: " + warrior.attack()); // 30
```

### `Decorator` 类图

```
IDrawable <<interface>>
    + draw()

    SimpleShape
    + draw()

    ShapeDecorator
    - IDrawable drawable
    + draw()

    BorderDecorator
    + draw()

    ScrollDecorator
    + draw()
```

## 在 `java.io` 中的应用

最典型的例子就是 `java.io` 包。装饰器模式被用来为 `InputStream` 和 `OutputStream` 添加功能。

例如：
*   `BufferedInputStream`
*   `LineNumberInputStream`

这些类装饰了 `InputStream` 对象，增加了缓冲、行号等功能。

**添加新方法**

所有在前面的例子中进行的修改都被定义在接口中，而这并非严格必需的。但这样使得新的方法可以动态地添加到对象中。不过，这种方式实现起来很麻烦，因为要识别最外层的装饰器来调用新的方法。因为新方法不在接口中，所以如果不使用装饰器模式，就不能直接对该对象进行包装。因此，为了使用这个新方法，周围的代码必须首先知道它在那里。

因为不在接口中，且由于这个新方法是动态添加的，所以类型检查必须被用来识别最外层的装饰器来包装对象。它要容易得多，且不易出错，用来修改已有的方法，而不是用新方法来装饰对象，因为这样，对象可以被同样的方式对待，就像之前一样，不需要修改周围的代码。

**说的是**，添加新方法用装饰器可以是一个极其强大的方法，来给一个类添加功能。但是，因为没有一个方法能够执行这个任务，除非是修改已有的方法，所以这个新功能不能被用于装饰器模式，因为装饰器模式用于向一个对象添加功能，而无需修改现有的类。


