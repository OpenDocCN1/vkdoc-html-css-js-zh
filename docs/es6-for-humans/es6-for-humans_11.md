# 设置器与访问器

设置器（setter）和访问器（getter）允许我们封装与对象属性获取或设置相关的行为，这使得代码更具扩展性，因为你可以根据需求轻松添加更多功能，例如输入验证或对输入数据的一些转换。此外，将这些验证或转换功能继承给子类，并根据应用需求进行保留或修改也会很有帮助。

有时，通过属性访问器向用户暴露属性的另一种表示形式，同时隐藏属性的内部表示形式，似乎是个不错的主意。

拥有属性访问器方法允许我们设置不同的访问级别，例如，访问器可以是公开的，但设置器方法可以针对特定用户群设置为受保护的。

你还应注意，设置器必须有且只有一个形式参数，这意味着你应该为类实例的每个属性创建独立的设置器和访问器。

## 计算式方法名

与对象字面量类似，类内部的方法也可以拥有计算式名称。这也适用于类的访问器和设置器访问器属性。正如在对象字面量中所见，计算式方法名可以用`[ ]`包裹起来；请查看以下代码片段作为示例：

```
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

#### 类属性与隐私

到目前为止，我们已经看到如何通过在类定义的构造函数方法内定义属性来向类的实例添加属性。然而，我们目前看到的所有属性都是公共属性，可以在类定义外部不使用设置器或访问器方法就能轻松访问。JavaScript 社区中关于私有属性的讨论由来已久。遗憾的是，在 ES6 之前，JavaScript 中从未存在过私有属性，因此必须模拟它们。最常见的约定是在属性或方法名称前加上下划线，我们在前面的示例中已经看到过。

在 ES6 中，有几种方法可以在类内部管理数据隐私。

#### 使用构造函数环境

将方法和属性存储在构造函数环境中，并在闭包中捕获变量，这种思路确保这些方法或属性不会被添加到原型中，并且只能由构造函数及其创建的函数访问。

```
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

```
const myMap = new WeakMap();
```

并且你可以在返回的 `WeakMap` 上使用 `set` 和 `get` 方法来存储和检索与之关联的值：

```
const myMap = new WeakMap();
const myObj = { name: "jack" };
myMap.set(myObj, "developer");
console.log(myMap.get(myObj));
// developer
```

如上述代码片段所示，我们使用一个对象作为键来在 `WeakMap` 内部存储值。

`WeakMap` 的另一个特性是，如果对对象没有其他引用，它们不会阻止垃圾回收。因此，使用 WeakMap 来存储类的属性有助于保持数据私有，并且每当与类关联的实例被销毁时，它也会销毁这些私有属性。

```
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

```
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

```
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



