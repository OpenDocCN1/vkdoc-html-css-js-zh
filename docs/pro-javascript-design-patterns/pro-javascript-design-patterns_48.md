# 选项

## 使用 BicycleDecorator 抽象类

使用 `BicycleDecorator` 抽象类的目的是为了让你能够轻松地创建实现这些选项的类。你只需要创建一些新的子类，覆盖少数方法即可。这里提供了 `BicycleDecorator` 的代码实现，以及对应的四个选项类。

### BicycleDecorator 的代码

```
BicycleDecorator.prototype = Object(Bicycle.prototype)
```

### Assemble 方法的实现

```
Assemble: function() {
    return this.bicycle.Assemble();
}
```

### Wash 方法的实现

```
Wash: function() {
    return this.bicycle.Wash();
}
```

### Ride 方法的实现

```
Ride: function() {
    return this.bicycle.Ride();
}
```

### Repair 方法的实现

```
Repair: function() {
    return this.bicycle.Repair();
}
```

### GetPrice 方法的实现

```
GetPrice: function() {
    return this.bicycle.GetPrice();
}
```

### 示例：ReadlightDecorator

```
ReadlightDecorator.prototype = Object(BicycleDecorator.prototype)
```

```
ReadlightDecorator.prototype.Assemble = function() {
    return this.bicycle.Assemble();
}
```

