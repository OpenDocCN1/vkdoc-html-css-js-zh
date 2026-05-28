# 选项类

## Option 1: Assemble

```
Assemble: function() {
    return this.bicycle.Assemble();
}
```

## Option 2: Wash

```
Wash: function() {
    return this.bicycle.Wash();
}
```

## Option 3: Ride

```
Ride: function() {
    return this.bicycle.Ride();
}
```

## Option 4: Repair

```
Repair: function() {
    return this.bicycle.Repair();
}
```

## Option 5: GetPrice

```
GetPrice: function() {
    return this.bicycle.GetPrice();
}
```

### 关于 getPrice 方法的说明

对于 `getPrice` 方法，实现细节并不重要。你只需要知道，当你调用 `getPrice` 时，它会返回相应的价格。

现在我们来讨论一下何时使用哪种模式。

### 使用哪种模式？

作为一条基本原则：当需要添加新的选项时，你可以创建一个抽象的 `BicycleDecorator` 类，并将其作为所有选项类的超类。这个抽象类继承自 `Bicycle`。

```
BicycleDecorator.prototype = Object(Bicycle.prototype)
```

然后，所有的选项类（`Assemble`、`Wash`、`Ride`、`Repair`、`GetPrice`）都将从 `BicycleDecorator` 继承。