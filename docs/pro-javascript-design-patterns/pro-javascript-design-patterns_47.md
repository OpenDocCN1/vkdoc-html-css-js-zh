# 选项

## 选项 1：使用 Decorator 模式

`Decorator` 模式对于实现这些选项来说是一个理想的选择。相比于创建子类，为了每种组合都创建子类，你只需要为每个选项创建一个子类，然后使用这些选项来包装组件。这些选项类可以放在 `Bicycle` 子类之外，因为它们只是包装器，需要被传递到 `Bicycle` 类中。

## 选项 2：使用 Ace Comfort Cruiser 类

`Ace Comfort Cruiser` 看起来像是一个无变化的类，需要使用装饰器。在 `Ace Comfort Cruiser` 类的构造函数中，调用 `super()` 并传入需要的参数。

```
AceComfortCruiser.prototype = Object(Bicycle.prototype)
```

接下来的四个选项：`Assemble()`、`Wash()`、`Ride()`、`Repair()`、`GetPrice()`——都仅仅是调用相同的方法，并返回结果。

```
AceComfortCruiser.prototype.Assemble = function() {
    return this.bicycle.Assemble();
}
```

其实这就是一个简单的装饰器模式，本质上和 `Bicycle` 类是一样的工作方式。它接受一个对象并使用它作为组件。它实现了 `Bicycle` 接口，并对每个方法简单地调用组件上的相同方法。这个时候，看起来非常类似于 compose 的设计模式。我们来看看区别，以及优化后的两种模式在下一节中的体现。

## 何时使用哪种模式

作为一条基本原则：当需要添加新的选项时，你可以创建一个抽象的 `BicycleDecorator` 类，并将其作为所有选项类的超类。这个抽象类继承自 `Bicycle`。

```
BicycleDecorator.prototype = Object(Bicycle.prototype)
```

然后，所有选项类（`Assemble`、`Wash`、`Ride`、`Repair`、`GetPrice`）都将从 `BicycleDecorator` 继承。

## 核心差异

在 `Bicycle` 类中，构造函数接收一个对象并保存为组件。而对于 `BicycleDecorator`，装饰器同样接收一个对象作为组件，并简单地调用组件上的相同方法。这时候，它看起来与 `Bicycle` 非常相似，但区别在于 `Bicycle` 是一个具体类，而 `BicycleDecorator` 是一个抽象类。

## 装饰器模式的实现细节

对于 `GetPrice` 方法，实现细节并不重要。你将会看到当我们在下一节定义四个选项类时，它们是如何实现的。

## 总结

这其实是一个简单的装饰器模式，在本质上和 `Bicycle` 类一样，它接受一个对象并使用它作为组件。它实现了 `Bicycle` 接口，并对每个方法简单地调用组件上的相同方法。这个时候，它看起来非常类似于 compose 的设计模式。我们来看看区别，以及优化后的两种模式在下一节中的体现。

## 下一节

我们将继续讨论 `BicycleDecorator` 的实现细节，包括如何实现 `Assemble`、`Wash`、`Ride`、`Repair` 和 `GetPrice` 方法。

```
BicycleDecorator.prototype.Assemble = function() {
    return this.bicycle.Assemble();
}
```

```
BicycleDecorator.prototype.Wash = function() {
    return this.bicycle.Wash();
}
```

```
BicycleDecorator.prototype.Ride = function() {
    return this.bicycle.Ride();
}
```

```
BicycleDecorator.prototype.Repair = function() {
    return this.bicycle.Repair();
}
```

```
BicycleDecorator.prototype.GetPrice = function() {
    return this.bicycle.GetPrice();
}
```

