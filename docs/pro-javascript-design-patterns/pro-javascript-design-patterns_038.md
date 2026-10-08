# 原型继承

JavaScript 的面向对象编程基于原型继承，而非传统的类继承。这意味着对象直接从其他对象继承属性。

## 基于原型的继承

在基于原型的继承中，对象直接从其他对象继承。这种机制被称为原型链。它通过让一个对象引用另一个对象（即其原型）来实现。当访问一个对象的属性时，如果该对象本身没有定义该属性，JavaScript 会沿着原型链向上查找，直到找到该属性或到达链的末尾。

原型继承的核心思想是：你可以创建一个对象，然后将其用作其他对象的蓝图。这种模式比类继承更灵活，但需要理解其工作方式。

## 原型链

每个 JavaScript 对象都有一个内部链接指向另一个对象，这个对象就是它的原型。当你访问一个对象的属性时，JavaScript 首先检查该对象本身是否具有该属性。如果没有，它会检查该对象的原型，然后检查原型的原型，以此类推，直到找到该属性或到达原型链的末端。原型链的末端通常是 `Object.prototype`。

## 构造函数与原型

虽然 JavaScript 没有类（在 ES6 之前），但你可以使用构造函数来创建对象。构造函数是一个普通的函数，当使用 `new` 关键字调用时，它会创建一个新对象并返回。

```javascript
function Person(name) {
  this.name = name;
}
```

构造函数 `Person` 有一个 `prototype` 属性，它是一个对象。当你创建一个新对象时，这个新对象的原型会被设置为构造函数的 `prototype` 属性。

```javascript
Person.prototype.getName = function() {
  return this.name;
};

var person = new Person('Alice');
console.log(person.getName()); // "Alice"
```

在这个例子中，`person` 对象本身没有 `getName` 方法，但 JavaScript 可以在其原型 `Person.prototype` 上找到它。

## 原型继承与类继承的区别

- **类继承**：类继承使用类来定义对象的蓝图。对象是类的实例，并且类可以继承其他类。这种模式在 Java 和 C++ 中很常见。

- **原型继承**：原型继承不使用类。相反，对象直接从其他对象继承。这意味着你可以直接从一个现有对象创建新对象，而不是从一个类创建。

## 原型继承的优缺点

### 优点

* 更灵活：你可以动态地改变对象的原型。

* 更简洁：不需要定义类，可以直接创建对象。

* 内存效率高：方法在原型上共享，而不是在每个实例上复制。

### 缺点

* 更难以理解：原型链的概念可能比类继承更复杂。

* 容易出错：直接操作原型可能导致意外行为。

## 使用 `Object.create()` 实现原型继承

`Object.create()` 是创建具有特定原型的新对象的标准方法。

```javascript
var Person = {
  name: 'Default name',
  getName: function() {
    return this.name;
  }
};

var Author = Object.create(Person);
Author.name = 'Default Author name';
Author.books = [];

var author = Object.create(Author);
author.name = 'Alice';
author.books = ['Book A', 'Book B'];

console.log(author.getName()); // "Alice"
```

在这个例子中，`author` 的原型是 `Author`，`Author` 的原型是 `Person`。`author` 本身没有 `getName` 方法，但可以从 `Person` 继承它。

## 添加超类属性

除了继承自超类外，原型继承还允许你直接从超类调用方法。

```javascript
Author.prototype.getName = function() {
  return this.name;
};

Author.superClass.getAuthorName = function() {
  return "Author of " + this.books.join(", ");
};

var author = new Author();
author.name = 'Bob';
author.books = ['Book C', 'Book D'];

console.log(author.getName()); // "Bob"
console.log(author.getAuthorName()); // "Author of Book C, Book D"
```

这种模式虽然灵活，但可能会导致代码难以维护。通常，`Object.create()` 和构造函数模式是更常见且易用的实现原型继承的方法。

## 原型继承与类继承的对比

使用类继承的示例：

```java
class Person {
  String name;
  Person(String name) {
    this.name = name;
  }
  String getName() {
    return name;
  }
}

class Author extends Person {
  String[] books;
  Author(String name, String[] books) {
    super(name);
    this.books = books;
  }
  String[] getBooks() {
    return books;
  }
}
```

使用原型继承的示例：

```javascript
var Person = {
  name: 'Default name',
  getName: function() {
    return this.name;
  }
};

var Author = Object.create(Person);
Author.books = [];
Author.getBooks = function() {
  return this.books;
};

var author = Object.create(Author);
author.name = 'Charlie';
author.books = ['Book E', 'Book F'];

console.log(author.getName()); // "Charlie"
console.log(author.getBooks()); // ["Book E", "Book F"]
```

如你所见，原型继承更简洁，但需要理解原型链如何工作。它没有类、构造函数或 `new` 关键字。

## 原型继承的适用场景

原型继承非常适合以下场景：

* 当你需要创建大量相似对象时。

* 当你需要动态地改变对象的原型时。

* 当你想要避免类的复杂性和冗长性时。

## 总结

原型继承是 JavaScript 的基石。它提供了一种灵活且强大的方式来创建对象和共享行为。虽然它可能因不同于传统的类继承而难以理解，但一旦你掌握了它的概念，你会发现它是一种非常优雅且强大的工具。通过使用 `Object.create()` 和构造函数，你可以实现复杂的对象层次结构，同时保持代码的简洁性和可维护性。

在原型继承中，对象直接从其他对象继承，而不是从类继承。每个对象都有一个原型，原型本身也是一个对象，从而形成原型链。这种模式比类继承更灵活，但需要理解原型链的工作方式。通过使用 `Object.create()` 和构造函数，你可以轻松地实现原型继承，并在需要时添加属性和方法。

## 原型继承的示例

```javascript
function Person(name) {
  this.name = name;
}

Person.prototype.getName = function() {
  return this.name;
};

function Author(name, books) {
  Person.call(this, name);
  this.books = books;
}

Author.prototype = Object.create(Person.prototype);
Author.prototype.constructor = Author;

Author.prototype.getBooks = function() {
  return this.books;
};

var author = new Author('David', ['Book G', 'Book H']);
console.log(author.getName()); // "David"
console.log(author.getBooks()); // ["Book G", "Book H"]
```

在这个示例中，我们使用构造函数和原型链实现了继承。`Author` 的原型被设置为 `Person.prototype` 的一个新对象，从而继承了 `Person` 的所有方法。

## 使用 `class` 关键字（ES6+）

ES6 引入了 `class` 关键字，使得类继承在语法上更像传统的面向对象语言。然而，`class` 本质上仍然是基于原型的继承。

```javascript
class Person {
  constructor(name) {
    this.name = name;
  }
  getName() {
    return this.name;
  }
}

class Author extends Person {
  constructor(name, books) {
    super(name);
    this.books = books;
  }
  getBooks() {
    return this.books;
  }
}

var author = new Author('Eve', ['Book I', 'Book J']);
console.log(author.getName()); // "Eve"
console.log(author.getBooks()); // ["Book I", "Book J"]
```

理解原型继承对于掌握 JavaScript 至关重要。无论是使用传统的构造函数模式还是现代的 `class` 语法，底层的原型链机制始终是相同的。通过理解原型继承，你可以更好地设计和管理你的代码，创建灵活且可扩展的应用程序。

## 学习 `代理`（`Agent`）的核心概念与 `原型`（`Prototype`）模式

`原型`模式与`代理`模式互斥。

每个`对象`（或状态）都拥有其`自身`的`副本`。

代理的`状态`可以通过`Getter`/`Setter`方法获取或设置。

代理的`状态`可以通过`Getter`/`Setter`方法读取或写入。

要创建一个`新`的`原型`对象，你需要使用`new`关键字，或使用`Object.create()`方法，该方法接受一个`原型`对象，然后你可以在其上添加新属性。

创建代理所依据的`原型`对象`必须`是`同一个`，或者`继承自`同一个`原型`链。

创建一个`新`的`对象`，其`原型`指向该`原型`对象，然后让`代理`处理该对象，这便是`原型`式继承的基础。

要创建一个`Author`，你需要一个简单的`Person`对象，然后制作一个`副本`。

```javascript
// 创建一个 Person 对象
var Person = {
    name: "Default",
    say: function() { console.log("My name is", this.name); }
};

// 创建一个 Author 对象作为 Person 的副本
var Author = Object.create(Person);

// 为 Author 添加 write 方法
Author.write = function() { console.log("Writing..."); };

// 使用 Author 创建实例
var author = Object.create(Author);
author.name = "Alice";
author.say(); // 输出: My name is Alice
author.write(); // 输出: Writing...
```

`可维护性`优势: `原型`模式需要将原型对象视为不可变对象；当你需要一个新对象时，你可以克隆它，并在克隆体上添加新方法，而`无需`修改原始原型。这遵循了`开放/封闭原则`（Open/Closed Principle）。

```javascript
// 修改原型会破坏依赖于它的所有对象
Person.say = function() { console.log("Modified!"); };
author.say(); // 输出: "Modified!" - 因为 author 继承自 Person

// 为了避免这种情况，不要修改原型；而是创建一个新对象
var BetterPerson = Object.create(Person);
BetterPerson.say = function() { console.log("Better!"); };
var betterAuthor = Object.create(BetterPerson);
betterAuthor.say(); // 输出: "Better!"
```

`原型`模式也符合`单一职责原则`（Single Responsibility Principle），因为它将对象创建与行为逻辑分离开来。

例如，要创建一个新对象，你只需克隆一个现有的原型，而不是使用复杂的构造函数。

`原型`模式的另一个优点是`性能`。由于所有实例都共享原型上的方法，因此无需为每个实例创建这些方法的单独副本。在需要创建许多对象的场景中，这可以节省大量内存。

**缺点**:`原型`模式可能比传统类更难理解，因为原型链可能变得复杂。此外，在原型上修改方法可能会影响所有实例，因此需要谨慎操作。

**结论**:`原型`模式提供了一种灵活且强大的方式来创建对象。它对于创建复杂对象特别有用，因为它允许你克隆现有对象并在此基础上进行构建。这在许多用例中都适用，例如游戏开发、UI 组件库和状态管理。

```javascript
// 使用“Object.create”的简单示例
var prototype = {
    greet: function() { return "Hello, " + this.name; }
};
var obj = Object.create(prototype);
obj.name = "World";
console.log(obj.greet()); // 输出: Hello, World
```

**进一步阅读**:`原型`模式是 JavaScript 的核心概念，也是许多现代 JavaScript 框架的基础。其优点包括灵活性和性能，但你也需要意识到潜在的问题，比如可变性。

**参考**:
-   [MDN：Object.create](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/create)
-   [JavaScript 中的原型继承](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Inheritance_and_the_prototype_chain)
-   [设计模式：原型](https://refactoring.guru/design-patterns/prototype)

作者：李华 | 发布于：2023-10-05 | 标签：#JavaScript #设计模式 #原型

---

**相关文章**:
- 代理模式
- 工厂方法与原型模式：比较
- 性能优化：原型模式与类
- 组合模式与原型模式
- 构建你自己的 JavaScript 游戏引擎：原型模式

## 方法概述

本文将介绍如何使用 `Factory Method`（工厂方法）设计模式来创建对象。在软件设计中，工厂方法模式是一种创建型设计模式，它提供了一种在不指定具体类的情况下创建对象的方法。

## 问题描述

假设我们正在开发一个对象关系映射（ORM）库，用于处理数据库记录。我们希望能够创建不同类型的对象，如用户、产品等。这些对象具有不同的属性和行为，但我们都希望以统一的方式创建它们。

传统的做法是使用 `new` 关键字直接实例化对象。然而，这种方法要求调用者知道对象的具体类型，并且必须手动处理所有构造函数参数。此外，如果对象的创建过程变得复杂，直接使用 `new` 会导致代码重复且难以维护。

工厂方法模式提供了一种解决方案：将对象的创建过程封装在一个专门的工厂类中。通过调用工厂的方法，调用者可以获取所需的对象，而无需关心对象的具体创建细节。

## 核心解决方案

### `CommonObject` 类的定义

在下面的示例中，`CommonObject` 是一个抽象基类，用于定义子类的结构。`ChildObject` 是 `CommonObject` 的具体子类。

## 继承与封装

在本章中，到目前为止，很少提及封装如何影响 `Inheritance`。当您从现有类创建子类时，只有公共成员和受保护成员会被传递下去。这与其他面向对象语言类似。例如，在 `Java` 中，子类无法访问任何私有方法。您必须显式地将方法定义为 `protected`，才能将其传递给子类。

`Inheritance` 与 `Encapsulation`

在本章中，到目前为止，很少提及封装如何影响 `Inheritance`。只有公共成员和受保护成员会直接写入克隆对象。与此相对的是，使用经典继承创建的对象，每个对象都拥有每个属性和方法的副本。这些属性和方法直接写入克隆对象。

因此，完全暴露的类是进行子类化的最佳候选。所有成员都是公共的，并将被传递给子类。如果需要屏蔽某个成员，可以始终使用下划线约定。

如果对一个具有真正私有成员的类进行子类化，特权方法将被传递下去，因为它们是可以公开访问的。这些方法将允许间接访问私有属性，但子类的任何实例方法都无法直接访问这些私有属性。

私有成员只能通过这些已建立的特权方法来访问。在子类中无法添加新的特权方法。

## 六类

有一种方法可以在不使用严格继承的情况下重用代码。如果您希望在某一个以上的类中使用某个函数，你可以通过扩展（augmentation）在多个类之间共享它。在实践中，它大致是这样的：你创建一个包含通用方法的类，然后使用它来扩展其他类。这些包含通用方法的类被称为*混入类*。它们通常不会被实例化或直接调用。它们的存在仅仅是为了将其方法传递给其他类。以下示例可以很好地说明这一点：

```
var Six = {};

var Six.constructor = function() {};

Six.prototype = {};

var serialize = function() {};

var forkey in this {};

return forkey + ": " + thisforkey;
```

`Six` 类只有一个方法，即 `serialize()`。该方法遍历此对象中的每个成员，并将其作为字符串输出。这只是一个简单的示例。在 `http://json.org/json.js` 上，Douglas Crockford 的 `JSON` 库的 `toString()` 方法中可以找到此类函数的更健壮版本。这类方法可能对许多不同类型的类都有用，但让这些类都继承自 `Six` 是没有意义的。同样，在每个类中重复代码也没什么意义。最好的
