# 如何使用

使用方法很简单：定义一个 `Bicycle` 对象，然后使用 `BicycleDecorator` 类的构造函数来包装它。装饰器类会将所有的方法调用委托给内部的 `Bicycle` 对象。

## 扩展选项

现在，你可以非常轻松地创建更多的选项类了。它们只需要调用 `BicycleDecorator` 的构造函数并重写少数方法。这里是 `BicycleDecorator` 的代码：

```
BicycleDecorator.prototype = Object(Bicycle.prototype)
```

### 选项类的代码

```
Assemble: function() {
    return this.bicycle.Assemble();
}
```

```
Wash: function() {
    return this.bicycle.Wash();
}
```

```
Ride: function() {
    return this.bicycle.Ride();
}
```

```
Repair: function() {
    return this.bicycle.Repair();
}
```

```
GetPrice: function() {
    return this.bicycle.GetPrice();
}
```
