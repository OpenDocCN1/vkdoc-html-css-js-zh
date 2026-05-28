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