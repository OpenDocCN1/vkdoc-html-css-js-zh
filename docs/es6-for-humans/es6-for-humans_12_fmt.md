# ES6 类与继承

`ES6` 引入了 `extends` 关键字，允许创建一个类作为另一个类的子类。继承让我们能够将另一个类的状态和行为合并到我们自己的类中。从父类扩展子类可以防止代码重复。考虑以下示例：

```javascript
class AeroPlane {
  constructor(capacity) {
    this.capacity = capacity;
  }
  showCapacity() {
    console.log(`Capacity of this plane: ${this.capacity}`);
  }
}
class FighterPlane {
  constructor(capacity) {
    this.capacity = capacity;
  }
  showCapacity() {
    console.log(`Capacity of this plane: ${this.capacity}`);
  }
  fire() {
    console.log("Loading weapons and firing");
  }
}
```

在上面的示例中，数据属性 `capacity` 和方法 `showCapacity` 在两个类中重复出现。我们可以通过让 `FighterPlane` 继承自 `AeroPlane` 来轻松消除这种重复，从而将 `AeroPlane` 的状态和行为合并到 `FighterPlane` 中。考虑以下示例，其中 `FighterPlane` 类使用 `extends` 关键字从父类 `AeroPlane` 继承：

```javascript
class AeroPlane {
  constructor(capacity) {
    this.capacity = capacity;
  }
  showCapacity() {
    console.log(`Capacity of this plane: ${this.capacity}`);
  }
}
class FighterPlane extends AeroPlane {
  fire() {
    console.log("Loading weapons and firing");
  }
}
const phantom = new FighterPlane(2);
phantom.showCapacity();
// Capacity of this plane: 2
phantom.fire();
// Loading weapons and firing
```

如果你也想在派生类中定义构造函数方法，你需要使用 `super()`，它允许在派生类中调用父类的构造函数。`super()` 负责初始化上下文，因此你必须在构造函数方法中访问上下文（`this`）之前调用 `super()`；否则会导致错误。

如果未定义构造函数，则在创建类的新实例时会使用所有给定的参数自动调用 `super()`，如下例所示：

```javascript
class FighterPlane extends AeroPlane {
  // 没有构造函数
  fire() {
    console.log("Loading weapons and firing");
  }
}
// 等价于
class FighterPlane extends AeroPlane {
  constructor(...args) {
    super(...args);
  }
  fire() {
    console.log("Loading weapons and firing");
  }
}
```

## 继承静态属性

`ES6` 允许我们将父类的静态属性继承到子类中。考虑以下示例，其中 `AeroPlane` 的静态方法在其派生类（`FighterPlane`）上被调用：

```javascript
class AeroPlane {
  static radio() {
    console.log("Radio works");
  }
}
class FighterPlane extends AeroPlane {}
FighterPlane.radio();
// Radio works
```

在这段代码中，一个新的静态方法 `radio()` 被添加到 `AeroPlane` 类中。通过继承，此方法可以作为 `FighterPlane.radio()` 使用，其行为方式与 `AeroPlane.radio()` 方法相同。

## 方法重写

父类的方法可以在派生类内部被同名的父类方法轻松覆盖。例如，检查以下代码片段，其中父类（`AeroPlane`）中的 `fly()` 方法在派生类（`FighterPlane`）内部被重写：

```javascript
class AeroPlane {
  constructor(capacity) {
    this.capacity = capacity;
  }
  showCapacity() {
    console.log(`Capacity of this plane: ${this.capacity}`);
  }
  fly() {
    console.log("Engines on, and the plane will take off soon");
  }
}
class FighterPlane extends AeroPlane {
  fly() {
    console.log("Engines on, and the plane is gone");
  }
  fire() {
    console.log("Loading weapons and firing");
  }
}
const phantom = new FighterPlane(2);
phantom.fly();
// Engines on, and the plane is gone
phantom.showCapacity();
// 2
```

如果你希望访问父类版本的方法，你可以通过 `super.fly()` 来实现，如下所示：

```javascript
class FighterPlane extends AeroPlane {
  fly() {
    super.fly();
    console.log("Engines on, and the plane is gone");
  }
}
```

这确保了每次在 `FighterPlane` 的实例上调用 `fly()` 方法时，首先会调用父类的该方法。除了方法，你也可以重写构造函数；例如，考虑以下代码片段：

```javascript
class AeroPlane {
  constructor(capacity, color) {
    this.capacity = capacity;
    this.color = color;
  }
}
class FighterPlane extends AeroPlane {
  constructor(color) {
    // 这个战斗机是双座的
    super(2, color);
  }
}
const phantom = new FighterPlane("grey");
console.log(phantom.capacity);       // 2
```

## 可继承的内置对象

`ES6` 使得继承自内置类（如 `Array`，`String`，`RegEx` 等）变得非常容易，这在需要像栈、队列或任何其他链表结构这样的数据结构时非常有用。你也可以利用此功能根据需求为数组提供额外的辅助方法，例如 `first`，`last`，`shuffle` 等。

考虑以下代码片段，我们通过扩展内置的 `String` 类来实现一个 `ReversedString` 数据类型：

```javascript
class ReversedString extends String {
  print() {
    return this.split('').reverse().join('');
  }
}
const str = new ReversedString("Awesome");
console.log(str.print());
// emosewA
```

## 总结

`ES6` 类将 JavaScript 中构建伪类以模仿其他编程语言中基于类的开发模式的现有模式正式化。它们使 JavaScript 中的继承更易于使用，并为你提供了语法糖，以便用更简单的基于类的语法实现 JavaScript 的经典原型继承模型。

在本章中，我们研究了 `ES6` 中的类如何在底层使用原型继承，通过在类原型上定义非静态方法，而所有静态方法则定义在类本身上，使得所有类方法都不可枚举。我们还学习了新的基于类的继承语法，它允许你从基类创建派生类，也允许你继承自 JavaScript 中的内置类，如 `Array`，`String` 和其他数据类型。总的来说，类是对 JavaScript 非常重要的语法补充，通过提供简洁的语法，使我们能够以更清晰的方式声明自定义对象类型。