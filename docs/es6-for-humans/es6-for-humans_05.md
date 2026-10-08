# 4. ES6 中的类

如果你来自 C++、Java 或 Python 等面向对象编程语言，你一定已经了解类是如何工作的。在 ES6 之前，JavaScript 中从未存在过类，但从早期开始，就已经存在许多模式来模拟经典的面向类开发。类的概念以各种不同的形式被尝试，这主要是因为无法在 JavaScript 内部以一种简洁直接的方式支持类和经典继承。这导致了许多来自其他语言的传统开发者感到困惑，因为在 JavaScript 中，类和继承并不是创建相似或相关对象的主要方法。

这些语法和编码模式试图在原型系统内部，以隐藏的方式使用面向类开发，比如使用 `new`、`instanceOf` 和 `constructor` 属性。许多库，如 JSClass、Classify.js 等，都试图让 JavaScript 看起来支持传统的类。因此，尽管许多传统的 JS 开发者认为不需要传统意义上的类，但仅仅因为存在数量庞大的此类库，就影响了 TC39 在 ES6 中引入“类”。其目标是让它们看起来像真正的类，这通过引入 `class` 关键字和相关的声明机制来实现。

## ES6 中的类

在 ES6 的设计过程中，类的模式被广泛争论和讨论，并最终达成了一项折衷方案。重要的是要理解，ES6 的类的工作方式与其他面向对象语言并非完全相同。也就是说，TC39 仍在 ES6 之后陆续添加更多特性来增强类，使其更接近经典定义。

从概念上讲，传统的 JavaScript 中并没有“类”。ES6 中的类本质上只是一个函数。它只不过是对对象和原型的语法糖，提供了一种方便的声明式形式来实现类模式，从而促进互操作性。ES6 中这个新的 `class` 关键字支持基于原型的继承、构造函数、`super` 调用、实例方法和静态方法。

让我们看看如何在 ES6 中编写最基本形式的类声明，这与在其他语言中定义类非常相似。考虑以下示例：

```javascript
class Car {
  constructor(brand) {
    this.brand = brand;
  }
}
const myTesla = new Car("Tesla");
console.log(myTesla.hasOwnProperty("brand"));  // true
console.log(typeof Car);                       // function
```

在这个示例中，`brand` 是赋值在 `myTesla` 对象上的属性。这个对象也可以用对象字面量的形式表示如下：

```javascript
const myTesla = {
  brand: "Tesla"
};
```

上面的例子简单展示了如何创建一个带有可选 `constructor` 方法的类，当该类的对象被实例化时，此方法将被调用。ES6 中的类并没有为语言中已有的功能添加任何新功能；它们只是构建对象的更简洁语法。任何传入 `new Car()` 的参数都将被 `Car` 的 `constructor` 方法接收作为参数，你可以使用这些参数来初始化同一类的实例。因此，`constructor()` 方法是初始化对象属性的地方，这些属性是可枚举的。同时，由于 `constructor()` 方法是可选的，你可以声明一个像下面例子中的空类：

```javascript
class EmptyClass {
}
```

如果你没有在类中定义 `constructor()` 方法，JavaScript 引擎会为你插入一个空构造函数：

```javascript
class EmptyClass {
  /* JavaScript 会插入一个空构造函数：
  constructor () { }
  */
}
```

现在我们知道如何在类中初始化属性以及在哪里初始化属性，接下来看看如何定义方法。类声明允许你定义类的方法而无需使用 `function` 关键字：

```javascript
class Car {
  constructor(brand) {
    this.brand = brand;
  }
  start() {
    console.log(`你的 ${this.brand} 已经准备出发！`);
  }
}
const myTesla = new Car("Tesla");
myTesla.start();
// 你的 Tesla 已经准备出发！
```

在上面的例子中，我们在类定义中定义了一个 `start()` 方法，它使用 `this` 关键字访问类的 `brand` 属性。这个方法会自动附加到类的原型上，因此它是不可枚举的。我们将在本章后面更详细地讨论这一点以及类方法，但首先，让我们看看在 ES6 中定义类的不同方式。

### 在 ES6 中定义类

就像函数一样，在 ES6 中有两种定义类的方式。我们来看看每种方式。

#### 类声明

正如前面例子所示，类声明非常简单。你使用 `class` 关键字后跟一个类名来声明一个类，按照惯例类名通常使用 `TitleCase`（首字母大写）写法。这里需要注意的一点是，类名在类定义内部的行为表现类似于常量，这意味着你不能在类内部使用类名来覆盖它：

```javascript
class SimpleClass {
  constructor() {
    SimpleClass = "42";      // 执行时会抛出错误
  }
}
SimpleClass = "42";              // 声明后可以正常工作
```

与函数声明不同，类声明不会被提升（hoisted）。类似于 `let` 和 `const`，类声明会处于暂时性死区（TDZ），直到执行到达类声明的位置。因此，你需要在访问类之前声明它，否则会引发 `ReferenceError`：

```javascript
const b = new Bike(); // ReferenceError
class Bike {}
```

在上面的例子中，由于类尚未声明，它存在于暂时性死区中，因此当你尝试使用 `new` 关键字初始化一个新实例时，会抛出一个引用错误。

#### 类表达式

除了类声明之外，在 ES6 中定义新类的第二种方式是通过类表达式。它们与函数表达式完全相同，并且和函数表达式一样，也可以是匿名的。请看下面的例子：

```javascript
const Circle = class {
  constructor() {
    this.radius = 20;
  }
}
```

注意，在上面的例子中，`class` 关键字后面缺少标识符；这使它成为一个匿名的类表达式。除了变量声明外，这些类表达式还可以用于向函数传递参数。考虑以下代码片段，它有一个用于创建汽车的工厂函数：

```javascript
function carFactory(car) {
  return new car();
}
const Toyota  = carFactory(class {
  start() {
    console.log("Your car is ready to roll");
  }
  stop() {
    console.log("Shutting down the engines");
  }
});
Toyota.start();
// Your car is ready to roll
Toyota.stop();
// Shutting down the engines
```

工厂函数接受类定义作为参数，并返回传入参数的类的对象。在编程语言中，如果一个实体可以作为参数传递、从函数返回、被修改以及赋值给变量，它就被认为是该语言的一等公民。函数式编程在 JavaScript 中流行的众多原因之一，就是因为函数是语言的一等公民，这使我们能够最大程度地利用函数，并轻松创建高阶函数。在 ES6 中，与函数类似，类也是语言的一等公民，允许将类作为参数传递给函数（如上面的例子所示），以及通过类表达式将其赋值给变量。

## 类方法和访问器属性

在 ES6 之前，你需要使用 `Object.defineProperty()` 来使一个方法不可枚举，但在 ES6 中，类的所有方法都是不可枚举的。这些方法被附加到原型（prototype）上，这使得该方法可以被类的所有实例共享，从而提供了一种节省内存的有效方式。

在 ES6 中定义类方法的语法类似于对象字面量的方法简写（即，不带 `function` 关键字的函数）：

## ES6 类与原型

上面的例子展示了一个`AeroPlane`类，我们在构造每个实例时初始化名为`model`和`capacity`的实例属性。`getData()`方法声明告诉`AeroPlane`类的实例，它们将拥有一个`getData()`方法，无论何时调用该方法，都会返回该方法的执行结果。这个`getData()`方法会自动添加到原型中。

```javascript
class AeroPlane {
  constructor(model, capacity) {
    this.model = model;
    this.capacity = capacity;
  }
  getData() {
    console.log(`You're flying a ${this.model} aeroplane`);
    console.log(`This plane can fly with ${this.capacity} passengers`);
  }
}
const jet = new AeroPlane("Jet", 60);
jet.getData();
// You're flying a Jet aeroplane
// This plane can fly with 60 passengers
console.log(jet.hasOwnProperty("getData"));                     // false
console.log(jet.__proto__.hasOwnProperty("getData"));           // true
```

## ES5 等效实现

用 ES5 的方式重新创建上面的例子将是：

```javascript
function AeroPlane(model, capacity) {
  this.model = model;
  this.capacity = capacity;
}
AeroPlane.prototype.getData = function() {
  console.log("You're flying a " + this.model + " aeroplane");
  console.log("This plane can fly with " + this.capacity + " passengers");
}
var jet = new AeroPlane("Jet", 60);
jet.getData();
// You're flying a Jet aeroplane
// This plane can fly with 60 passengers
```

## 访问器属性

ES6 类还允许你在原型上创建访问器属性。我们可以使用`get`和`set`关键字放在标识符前面来分别创建 getter 和 setter 属性。这些属性描述符是不可枚举的，并且可以通过在类的原型上使用`Object.getOwnPropertyDescriptor()`来访问，因为这些属性驻留在原型对象上。

例如，请考虑以下代码片段：

```javascript
class AeroPlane {
  constructor(model, capacity) {
    this._model = model;
    this._capacity = capacity;
  }
  get model() {
    return this._model;
  }
  get capacity() {
    return this._capacity;
  }
  set model(model) {
    this._model = model;
  }
  set capacity(capacity) {
    this._capacity = capacity;
  }
}
const jet = new AeroPlane("Jet", 100);
console.log(jet.capacity);
// 100
console.log(Object.getOwnPropertyDescriptor(AeroPlane.prototype, "model"));
/*
{
  get: [Function: get model],
  set: [Function: set model],
  enumerable: false,
  configurable: true
}
*/
```

在上面的例子中，你必须注意，我们在类定义中的属性名前添加了一个`_`（下划线）。让我们使用下面的代码片段来看看我们为什么这么做：

```javascript
class StudyGroup {
  constructor(name) {
    this.name = name;
  }
  set name(newName) {
    this.name = newName;
  }
}
const jsGroup = new StudyGroup("js");
// RangeError: Maximum call stack size exceeded
```

如果你尝试运行上面的代码片段，你会看到一个`RangeError`，提示超出最大调用堆栈大小，这是因为你的 setter 属性在递归地设置自身，导致无限函数调用。因此，你绝不能让你的 setter 方法名与属性名相同，因为在 setter 内部通过其自身名称访问属性 setter 会创建一个无限递归的函数调用。

## 使用 getter 和 setter 的原因

如果你此时想知道为什么首先要使用 setter 和 getter，以下是一些应该考虑使用它们的原因：

# 设置器与访问器

设置器（setter）和访问器（getter）允许我们封装与对象属性获取或设置相关的行为，这使得代码更具扩展性，因为你可以根据需求轻松添加更多功能，例如输入验证或对输入数据的一些转换。此外，将这些验证或转换功能继承给子类，并根据应用需求进行保留或修改也会很有帮助。

有时，通过属性访问器向用户暴露属性的另一种表示形式，同时隐藏属性的内部表示形式，似乎是个不错的主意。

拥有属性访问器方法允许我们设置不同的访问级别，例如，访问器可以是公开的，但设置器方法可以针对特定用户群设置为受保护的。

你还应注意，设置器必须有且只有一个形式参数，这意味着你应该为类实例的每个属性创建独立的设置器和访问器。

## 计算式方法名

与对象字面量类似，类内部的方法也可以拥有计算式名称。这也适用于类的访问器和设置器访问器属性。正如在对象字面量中所见，计算式方法名可以用`[ ]`包裹起来；请查看以下代码片段作为示例：

```javascript
const methodName = "getColor";
const propName = "color";
class AeroPlane {
  constructor(color) {
    this._color = color
  }
  [methodName]() {
    return this._color;
  }
  get [propName]() {
    return this[`_${propName}`];
  }
  set propName {
    return this[`_${propName}`] = value;
  }
}
const whiteJet = new AeroPlane("white");
console.log(whiteJet.color);
// white
```

在上例中，我们通过将方法名和访问器与设置器访问器属性名包裹在`[ ]`内，在类定义中对其进行计算。我们还在访问器和设置器方法内部使用字符串字面量来求值动态属性名。

### 类属性与隐私

到目前为止，我们已经看到如何通过在类定义的构造函数方法内定义属性来向类的实例添加属性。然而，我们目前看到的所有属性都是公共属性，可以在类定义外部不使用设置器或访问器方法就能轻松访问。JavaScript 社区中关于私有属性的讨论由来已久。遗憾的是，在 ES6 之前，JavaScript 中从未存在过私有属性，因此必须模拟它们。最常见的约定是在属性或方法名称前加上下划线，我们在前面的示例中已经看到过。

在 ES6 中，有几种方法可以在类内部管理数据隐私。

#### 使用构造函数环境

将方法和属性存储在构造函数环境中，并在闭包中捕获变量，这种思路确保这些方法或属性不会被添加到原型中，并且只能由构造函数及其创建的函数访问。

```javascript
class AeroPlane {
  constructor(capacity) {
    this.checkCapacity = function(value) {
      if (capacity >= value) {
        return true;
      }
      return false;
    }
  }
}
const jet = new AeroPlane(200);
console.log(jet.checkCapacity(100));    // true
console.log(jet.capacity);              // undefined
```

在上面的示例中，构造函数环境存储了参数和局部变量，通过使类属性（此处为`capacity`）在构造函数外部不可访问，从而保持其私有性。这使得私有数据完全安全，并且无法从类实例访问。

#### 使用 WeakMaps

在我们继续了解 WeakMaps 如何帮助在类内部保持数据私有之前，让我们快速了解一下什么是 WeakMaps。`WeakMap` 是 ES6 中引入的一种新数据类型。它们只是一个键值对的集合，主要限制是必须使用对象作为键。我们将在本书后面更详细地学习它们。但目前，你应该注意，你可以使用以下方式创建 `WeakMap`：

```javascript
const myMap = new WeakMap();
```

并且你可以在返回的 `WeakMap` 上使用 `set` 和 `get` 方法来存储和检索与之关联的值：

```javascript
const myMap = new WeakMap();
const myObj = { name: "jack" };
myMap.set(myObj, "developer");
console.log(myMap.get(myObj));
// developer
```

如上述代码片段所示，我们使用一个对象作为键来在 `WeakMap` 内部存储值。

`WeakMap` 的另一个特性是，如果对对象没有其他引用，它们不会阻止垃圾回收。因此，使用 WeakMap 来存储类的属性有助于保持数据私有，并且每当与类关联的实例被销毁时，它也会销毁这些私有属性。

```javascript
const data = new WeakMap();
class AeroPlane {
  constructor(seats) {
    data.set(this, {
      capacity: seats
    });
  }
  get seats() {
    return data.get(this).capacity;
  }
  set seats(value) {
    data.get(this).capacity = value;
  }
}
const jet = new AeroPlane(200);
console.log(jet.capacity);                // undefined
console.log(jet.seats);                   // 200
```

在上面的示例中，我们使用 `WeakMap` 来保持数据私有，并公开设置器和访问器来与类的私有数据进行交互。但这样做的代价是，需要将你的 `WeakMap` 对外部世界隐藏。只要它对外部访问是隐藏的，你的属性就会保持安全且私有。

除了 `WeakMap` 之外，ES6 还提供了另一种新技术来保持数据私有（或者更确切地说，防止随意访问），即使用 Symbols，我们将在本书后面学习。

#### 静态方法与属性

使用类，你还可以定义属于类本身而非类的任何特定实例的属性和方法。这些方法和属性与类关联，而不是与类的实例关联，这通常有助于为应用程序创建实用函数。ES6 允许你通过在方法名之前使用 `static` 关键字轻松创建静态方法：

```javascript
class AeroPlane {
  constructor(capacity) {
    this.capacity = capacity;
  }
  static radio(message) {
    console.log(`Message from broadcast: ${message}`)
  }
}
AeroPlane.radio("Sky is clear");
// Message from broadcast: Sky is clear
```

如前所述，需要重点注意的是，静态方法和属性是直接从其类中访问的，你不能从类的实例访问静态方法或属性。因此，静态方法也不能使用 `this` 访问在类的实例上定义的属性或方法。

在编写本书时，ES6 不支持使用 `static` 关键字创建属性。有一个关于将它们添加到语言规范的提案，但在此提案被接受且实现发布之前，你可以使用静态访问器和设置器，或者手动将静态属性附加到类，如下所示：

```javascript
class AeroPlane {
  constructor(capacity) {
    this.capacity = capacity;
  }
}
AeroPlane.color = "white";
console.log(AeroPlane.color);
// white
console.log(AeroPlane.capacity);
// undefined
```

在这个例子中，请注意我们如何使用类名访问静态属性，而对非静态属性则返回 `undefined`。

## 类继承与 super 关键字

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
