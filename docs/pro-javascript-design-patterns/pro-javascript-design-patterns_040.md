# 使用 Prototype Web 框架构建动态用户界面

## 页面类型

- `静态页面`：使用 `PrototypeWeb` 框架构建
- `动态页面`：使用 `PrototypeWeb` 框架构建
- `子页面`：使用 `PrototypeWeb` 框架构建
- `模态框`：使用 `PrototypeWeb` 框架构建
- `测试页面`：使用 `Pinia` 构建

## 概述

`EditView` 页面是 `PrototypeWeb` 框架的核心组成部分，它继承自 `ViewBase` 基类，包含一个输入区域和一个显示区域。

`经典`继承看起来是一种理想的技术，适用于这种情况。使用 `PrototypeWeb` 可以轻松构建动态用户界面，而无需处理复杂的 DOM 操作或手动管理状态。通过创建子类并覆盖继承的方法，我们可以改变类的行为，就像添加或覆盖原型上的方法一样。由于类之间的变化很小，继承在这种情况下是理想的。

使用 `原型` 继承

尽管 `经典` 继承与 `原型` 继承在根本上不同，但重复使用原型继承的实践真正表明，最终的代码在两者之间可以非常相似。唯一的区别在于创建对象和覆盖方法的方式上。我们可以通过创建另一个子类并覆盖相同的方法，将字段关联到另一个输出。

### `EditView` 对象

- `EditView`：`Object.create(EditViewField)`
- `var`：`this`
- `var`：`this.id`
- `this.id`：`id`
- `this.parent`：`parent`
- `this.type`：`"text"`
- `this.value`：`value`
- `this.field`：`this`（`/path.to.xxx`）
- `this.cancel`：`this`（`/path.to.xxx`）

### 创建事件

- `createEvent`：`Object.create(document)`（`/path.to.xxx`）
- `this.container`：`document.createEvent`（`/path.to.xxx`）/`this.cancel`
- `this.parent`：`this.container.appendChild(this.field)`
- `this.field.type`：`"text"`
- `this.field.value`：`this.value`
- `this.save`：`Object.create(document)`（`/path.to.xxx`）/`this.field`
- `this.save.type`：`"button"`
- `this.save.value`：`"保存"`
- `this.container.appendChild(this.cancel)`
- `this.cancel.type`：`"button"`
- `this.cancel.value`：`"取消"`
- `this.container.appendChild(this.save)`

### 附加事件

- `attachEvent`：`function`（`/path.to.xxx`）
- `var that`：`this`
- `addEventListener(this.save， "click"， this.convertToFunction)`
- `addEventListener(this.cancel， "click"， this.convertToFunction)`

### 转换函数

- `convertTo`：`function`（`/path.to.xxx`）
- `this.field.style.display`：`"none"`
- `this.container.appendChild(this.field)`
- `this.field.type`：`"text"`
- `this.field.value`：`this.value`
- `this.container.appendChild(this.cancel)`

### 设置值函数

- `setValue`：`function`（`/path.to.xxx`）
- `return this.field.value`

### 保存函数

- `save`：`function`（`/path.to.xxx`）
- `/path.to.xxx`

### 实例化一个对象

实例化对象时，使用 `Object.create()` 函数来创建一个新对象，而不是使用 `new` 关键字。因此，`EditView` 对象没有构造函数。原型继承看起来非常不同，从实例化一个类的方法开始。

`var title`：`Object.create(EditViewField)`

### 附加事件

- `attachEvent`：`function`（`/path.to.xxx`）
- `var that`：`this`
- `addEventListener(this.save， "click"， this.convertToFunction)`
- `addEventListener(this.cancel， "click"， this.convertToFunction)`

### 创建事件

- `createEvent`：`function`（`/path.to.xxx`）
- `this.container`：`document.createEvent`（`/path.to.xxx`）
- `this.parent.appendChild(this.container)`
- `this.save`：`Object.create(document)`（`/path.to.xxx`）
- `this.save.type`：`"button"`
- `this.save.value`：`"保存"`
- `this.cancel`：`Object.create(document)`（`/path.to.xxx`）
- `this.cancel.type`：`"button"`
- `this.cancel.value`：`"取消"`
- `this.container.appendChild(this.save)`
- `this.container.appendChild(this.cancel)`

### 设置值函数

- `setValue`：`function`（`/path.to.xxx`）
- `this.field.value`：`value`

### 获取值函数

- `getValue`：`function`（`/path.to.xxx`）
- `return this.field.value`
- `save`：`function`（`/path.to.xxx`）
- `/path.to.xxx`

### 结束

实例化一个类，现在有了一个对象。另外，代码几乎是相同的。唯一的区别在于创建对象的方式（`Object.create()` 与 `new`）以及方法覆盖的方式（直接覆盖与通过原型链覆盖）。由于这些差异很小，继承在这种情况下是理想的。

### 取消函数

- `cancel`：`function`（`/path.to.xxx`）
- `this.field.value`：`this.value`
- `this.field.value`：`value`

### 结束

在类的实例化中，现在有了一个对象。构造函数不再使用。因此，`EditView` 对象没有构造函数。原型继承看起来非常不同，从实例化一个类的方法开始。

`var title = Object.create(EditViewField)`

`var callback`：`this`

`var callback = 0`

`success`：`function`（`/path.to.xxx`）：`alert("保存值时出错：")`

### 结束

### 示例

`if (value == null) { value = "default value"; }`

### 获取值

- `getValue`：`function`（`/path.to.xxx`）
- `return this.field.value`

### 设置值

- `setValue`：`function`（`/path.to.xxx`）
- `this.field.value`：`value`
- `this.field.value`：`value`
- `this.field.style.display`：`"none"`
- `this.container.appendChild(this.field)`

### 结束

## 原型继承

在 JavaScript 中，原型继承是语言的核心机制，它允许对象从其他对象继承属性和方法。

### 理解原型

在 JavaScript 中，每个对象都有一个指向另一个对象的内部链接，这个对象被称为它的 `原型`。当试图访问一个对象的属性时，如果该对象本身没有这个属性，JavaScript 会沿着原型链向上查找，直到找到该属性或到达链的末端。

`__proto__` 是大多数浏览器实现用来访问对象原型的属性，但它并非标准的一部分。现代 JavaScript 推荐使用 `Object.getPrototypeOf()` 和 `Object.setPrototypeOf()` 来获取和设置原型。

### 原型链

原型链是由一系列对象组成的链，每个对象都有一个指向其原型的引用。当查找一个属性时，JavaScript 会遍历整个链，直到找到该属性或到达 `null`。

### 创建对象

你可以使用 `Object.create()` 方法创建一个以指定对象为原型的对象。例如：

```javascript
```

var parent = { x: 1 };

var child = Object.create(parent);

console.log(child.x); // 1

```

`child` 对象本身没有 `x` 属性，但它从 `parent` 原型继承了 `x`。

### 构造函数

构造函数是创建对象的另一种方式。当使用 `new` 关键字调用一个函数时，它便充当构造函数。这个函数会创建一个新对象，并将该对象的原型设置为构造函数的 `prototype` 属性。

### 在原型上添加方法

在原型上添加方法是实现代码复用的常用方式，例如：

```javascript
function Dog(name) {
    this.name = name;
}

Dog.prototype.bark = function() {
    console.log(this.name + " says woof!");
};

var fido = new Dog("Fido");
fido.bark(); // "Fido says woof!"
```

### 原型链的终点

所有对象的原型链最终都指向 `Object.prototype`，而 `Object.prototype` 的原型是 `null`。

## 基于类的语法（ES6 class）

JavaScript 的 ES6 引入了类语法，它提供了一种更清晰、更接近传统类的方式来实现继承。但这只是基于原型继承的语法糖，底层仍然是原型链。

当你使用 `class` 关键字时，你实际上创建了一个名为 `ClassName` 的函数及其 `prototype` 属性。ES6 class 提供了更简洁的语法来定义构造函数和方法，例如：

```javascript
class Animal {
    constructor(name) {
        this.name = name;
    }
    speak() {
        console.log(this.name + ' makes a sound.');
    }
}

class Dog extends Animal {
    speak() {
        console.log(this.name + ' barks.');
    }
}

let d = new Dog('Mitzie');
d.speak(); // Mitzie barks.
```
