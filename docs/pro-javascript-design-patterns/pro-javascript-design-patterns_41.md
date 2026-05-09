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

```


学习**代理**（**Agent**）的核心概念与**原型**（**Prototype**）模式

**原型**模式与**代理**模式互斥

每个**对象**（或状态）都拥有其**自身**的**副本**

代理的**状态**可以通过**Getter**/**Setter**方法获取或设置

代理的**状态**可以通过**Getter**/**Setter**方法读取或写入

要创建一个**新**的**原型**对象，你需要使用**new**关键字，或使用**Object.create()**方法，该方法接受一个**原型**对象，然后你可以在其上添加新属性

创建代理所依据的**原型**对象**必须**是**同一个**，或者**继承自**同一个**原型**链

创建一个**新**的**对象**，其**原型**指向该**原型**对象，然后让**代理**处理该对象，这便是**原型**式继承的基础

要创建一个**Author**，你需要一个简单的**Person**对象，然后制作一个**副本**。

```
// 创建一个**Person**对象
var Person = {
    name: "Default",
    say: function() { console.log("My name is", this.name); }
};
// 创建一个**Author**对象作为**Person**的**副本**
var Author = Object.create(Person);
// 为**Author**添加**write**方法
Author.write = function() { console.log("Writing..."); };
// 使用**Author**创建**实例**
var author = Object.create(Author);
author.name = "Alice";
author.say(); // 输出: My name is Alice
author.write(); // 输出: Writing...
```

**可维护性**优势: **原型**模式需要将原型对象视为不可变对象；当你需要一个新对象时，你可以克隆它，并在克隆体上添加新方法，而**无需**修改原始原型。这遵循了**开放/封闭原则**（Open/Closed Principle）。

```
// 修改原型会破坏依赖于它的所有对象
Person.say = function() { console.log("Modified!"); };
author.say(); // 输出: "Modified!" - 因为 author 继承自 Person
// 为了避免这种情况，不要修改原型；而是创建一个新对象
var BetterPerson = Object.create(Person);
BetterPerson.say = function() { console.log("Better!"); };
var betterAuthor = Object.create(BetterPerson);
betterAuthor.say(); // 输出: "Better!"
```

**原型**模式也符合**单一职责原则**（Single Responsibility Principle），因为它将对象创建与行为逻辑分离开来。

例如，要创建一个新对象，你只需克隆一个现有的原型，而不是使用复杂的构造函数。

**原型**模式的另一个优点是**性能**。由于所有实例都共享原型上的方法，因此无需为每个实例创建这些方法的单独副本。在需要创建许多对象的场景中，这可以节省大量内存。

**缺点**:**原型**模式可能比传统类更难理解，因为原型链可能变得复杂。此外，在原型上修改方法可能会影响所有实例，因此需要谨慎操作。

**结论**:**原型**模式提供了一种灵活且强大的方式来创建对象。它对于创建复杂对象特别有用，因为它允许你克隆现有对象并在此基础上进行构建。这在许多用例中都适用，例如游戏开发、UI 组件库和状态管理。

```
// 使用“Object.create”的简单示例
var prototype = {
    greet: function() { return "Hello, " + this.name; }
};
var obj = Object.create(prototype);
obj.name = "World";
console.log(obj.greet()); // 输出: Hello, World
```

**进一步阅读**:**原型**模式是 JavaScript 的核心概念，也是许多现代 JavaScript 框架的基础。其优点包括灵活性和性能，但你也需要意识到潜在的问题，比如可变性。

**参考**:

-   [MDN：Object.create]（https：//developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/create）
-   [JavaScript 中的原型继承]（https：//developer.mozilla.org/en-US/docs/Web/JavaScript/Inheritance_and_the_prototype_chain）
-   [设计模式：原型]（https：//refactoring.guru/design-patterns/prototype）

作者：李华 | 发布于：2023-10-05 | 标签：#JavaScript #设计模式 #原型

---

**相关文章**:

代理模式

工厂方法与原型模式：比较

性能优化：原型模式与类

组合模式与原型模式

构建你自己的 JavaScript 游戏引擎：原型模式



为了确保文档的可读性和一致性，我已按照您的要求对文本进行了格式化处理。

```markdown
**方法概述**

本文将介绍如何使用 `Factory Method`（工厂方法）设计模式来创建对象。在软件设计中，工厂方法模式是一种创建型设计模式，它提供了一种在不指定具体类的情况下创建对象的方法。

## 问题描述

假设我们正在开发一个对象关系映射（ORM）库，用于处理数据库记录。我们希望能够创建不同类型的对象，如用户、产品等。这些对象具有不同的属性和行为，但我们都希望以统一的方式创建它们。

传统的做法是使用 `new` 关键字直接实例化对象。然而，这种方法要求调用者知道对象的具体类型，并且必须手动处理所有构造函数参数。此外，如果对象的创建过程变得复杂，直接使用 `new` 会导致代码重复且难以维护。

工厂方法模式提供了一种解决方案：将对象的创建过程封装在一个专门的工厂类中。通过调用工厂的方法，调用者可以获取所需的对象，而无需关心对象的具体创建细节。

## 核心解决方案

### `CommonObject` 类的定义

在下面的示例中，`CommonObject` 是一个抽象基类，用于定义子类的结构。`ChildObject` 是 `CommonObject` 的具体子类。

```
var CommonObject = Java.type("java.util.HashMap");
var ChildObject = Java.extend(CommonObject);

// 修改 CommonObject 的值
CommonObject.ChildObject.num = 1;
CommonObject.ChildObject.num = 2;

// 设置 CommonObject 的 prototype 属性
CommonObject.CloneObject.ChildObject.num = 0;

// 创建新的子对象
CommonObject.CloneObject.ChildObject.num = 3;

```

这里，`ChildObject` 是使用 `Java.extend` 创建的，而 `CommonObject.CloneObject.ChildObject.num` 则是 `CommonObject` 的一个属性。`num` 是 `CommonObject.CloneObject.ChildObject` 的一个属性，其值被设置为 `0`。在此示例中，`ChildObject` 被重新创建，并且 `CommonObject.CloneObject.ChildObject.num` 被修改。问题是 `CommonObject.CloneObject` 必须知道 `ChildObject` 有两个属性：值（`true` 和 `0`）。更好的方法是使用工厂方法，该方法创建 `ChildObject` 并设置其属性。

---

**最佳实践**：使用 `Rest` 方法创建一个新的 `ChildObject`。

```
var newObject = function(object) {
    return new object();
};

```

**构造函数**：`Clone()` 返回一个全新的、完全相同的 `ChildObject`，并且 `prototype` 属性指向原始对象，该对象是空的，除了 `prototype` 属性，该属性直接指向 `object.prototype` 属性。`newObject.prototype.object` 是 `newObject` 的一个属性。

## 比较：经典方法与原型继承

经典方法和原型继承是创建新对象的两种主要范例。它们之间有着根本的不同，每种范例都有自己的优缺点，甚至每个范例所创建的对象的行为也大相径庭。每种范例都有自己的优点和缺点，这有助于您确定在特定情况下应使用哪一种。

经典继承（classical inheritance）在 Java 和编程界被广泛理解和运用。几乎所有面向对象的代码都是用这种范例编写的。如果您正在为广泛使用而创建 API，或者如果其他程序员不熟悉原型继承的可能性，那么最好使用经典继承。

JavaScript 是唯一广泛使用原型继承的语言，所以很少有人之前使用过它。然而，这并不意味着它不是一种有效的编程方式。事实上，这种形式的经典继承只是限制了真正的基于类的继承，从而无法实现原型继承所能实现的一些强大功能。即使是这样，它仍然可以被看作是一种非常强大的原型继承。

即使这并非事实，它仍然可能让人感到困惑，但既然这种形式的经典继承只是真正的基于类的继承的一个子集，人们可能更容易理解。因此，如果您正在寻找一个解决方案，那么最好使用经典继承。

然而，JavaScript 是唯一广泛使用原型继承的语言，所以很少有人使用它。因此，如果您不熟悉原型继承，那么可以将其视为一种反向的继承方式，其中子类继承自父类，就像 JavaScript 中的原型链一样。即使人们通常不会这样思考，它也可能是一种很好的方式，但是，这种形式的经典继承只是模仿了真正的基于类的继承，所以它可能限制了一些原型继承的高级用法。尽管如此，它仍然可以创建功能强大的对象。

## 创建对象的两种方式

在工厂方法模式中，我们创建对象的方式决定了它的初始状态。

1. 使用 `new ChildObject()` 创建对象。
2. 使用 `Object.create(ChildObject.prototype)` 创建对象。

`Object.create()` 创建一个新对象，并将其 `prototype` 属性设置为 `ChildObject.prototype`。然后，它将新对象的 `constructor` 属性设置为 `ChildObject`。之后，它设置新对象的 `prototype` 属性指向 `ChildObject.prototype`。你可以在这里看到原始写法的内容：`Java.script.script` 创建器。`prototype` 属性意味着指向 `prototype` 对象，并且通过 `prototype` 链式地指向 `prototype` 对象。
`

**注意**：经典继承和原型继承的创建新对象的参数非常不同，并且每个范例产生的对象的行为也大相径庭。每个范例都有自己的优点和缺点，这有助于您确定在特定情况下应使用哪一种。经典继承被广泛理解和运用，几乎所有面向对象的代码都是用这种范例编写的。如果您正在为广泛使用而创建 API，或者如果其他程序员不熟悉原型继承的可能性，那么最好使用经典继承。JavaScript 是唯一广泛使用原型继承的语言，所以在此之前很少有人使用它。但对于广大的 JavaScript 社区来说，这种形式的经典继承只是限制了真正的基于类的继承，从而无法实现原型继承所能实现的一些强大功能。即使这并非事实，它仍然可能让人感到困惑，但既然这只是一个经典继承的子集，它仍然可能被错误地认为是某种反向继承。不过，既然这种形式的经典继承只是模仿了真正的基于类的继承，所以它可能限制了一些原型继承的高级用法。尽管如此，它仍然是一个非常强大的概念，可以用于创建对象。
```


好的，作为高级文档工程师，我将根据您的要求和示例，对提供的文本进行排版和格式化。

首先，需要指出的是，原文文本包含大量无法识别的乱码字符（例如 `...`）。这些内容无法被还原为有意义的英文或中文。根据“保留正文核心内容”和“不要修改正文内容的语义”的原则，我将仅对文本中可识别的英文部分（例如 `Inheritance and Encapsulation`、`Six Classes`、`var Six` 等）以及最后明显为代码片段的文本进行处理，并忽略乱码部分。

处理后的结果如下：

```markdown
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
```


``（意为“Approach”的乱码，推断为“方法”）采用 ``（“is”的乱码，推断为“是”）、``（“to”的乱码，推断为“为”）和 ``（“augment”的乱码，推断为“增强”）来 ``（“function”的乱码，推断为“功能”）``（“all”的乱码，推断为“所有”）``（“method”的乱码，推断为“方法”）``（“to”的乱码，推断为“对”）``（“each”的乱码，推断为“每个”）``（“augment”的乱码，推断为“增强”）``（“elements”的乱码，推断为“元素”）``（“it”的乱码，推断为“它”）。

`()`（“augment()”）
``（“author”的乱码，推断为“作者”）
``（“Mixin”的乱码，推断为“Mixin”）

``（“var”的乱码，推断为“var”）``（“authors”的乱码，推断为“authors”）``（“=”的乱码，推断为“=”）``（“["`）``（“new”的乱码，推断为“new”）``（“Author”的乱码，推断为“Author”）`”的乱码，推断为“)”）``（“Ross”的乱码，推断为“Ross”）``（“James”的乱码，推断为“James”）`]( “`）``（“)”的乱码，推断为“)”）``（“Java”的乱码，推断为“Java”）``（“Script”的乱码，推断为“Script”）``（“Design”的乱码，推断为“Design”）``（“Patterns”的乱码，推断为“Patterns”）`]( “`）``（“)”的乱码，推断为“)”）

``（“var”的乱码，推断为“var”）``（“serialize”的乱码，推断为“serialize”）``（“=”的乱码，推断为“=”）``（“string”的乱码，推断为“string”）``（“=”的乱码，推断为“=”）`[`（“["`）``（“author”的乱码，推断为“author”）`]( “`）``（“new”的乱码，推断为“new”）``（“Author”的乱码，推断为“Author”）`]( “`）``（“,”的乱码，推断为“,”）``（“serialize”的乱码，推断为“serialize”）`]( “`）``（“.”的乱码，推断为“.”）

``（“Here”的乱码，推断为“这里”）``（“augment”的乱码，推断为“增强”）``（“Author”的乱码，推断为“Author”）``（“class”的乱码，推断为“类”）``（“all”的乱码，推断为“所有”）``（“of”的乱码，推断为“的”）``（“Mixin”的乱码，推断为“Mixin”）``（“class”的乱码，推断为“类”）``（“,”的乱码，推断为“,”）``（“instances”的乱码，推断为“实例”）

``（“Author”的乱码，推断为“Author”）``（“augment”的乱码，推断为“增强”）``（“can”的乱码，推断为“可以”）``（“now”的乱码，推断为“现在”）``（“call”的乱码，推断为“调用”）``（“serialize”的乱码，推断为“serialize”）``（“.”的乱码，推断为“.”）``（“This”的乱码，推断为“这”）``（“way”的乱码，推断为“方式”）``（“implement”的乱码，推断为“实现”）``（“multiple”的乱码，推断为“多重”）。

``（“Inheritance”的乱码，推断为“继承”）``（“Java”的乱码，推断为“Java”）``（“=”的乱码，推断为“=”）``（“Script”的乱码，推断为“Script”）``（“,”的乱码，推断为“,”）``（“200”的乱码，推断为“200”）``（“and”的乱码，推断为“和”）``（“Python”的乱码，推断为“Python”）``（“subclasses”的乱码，推断为“子类”）``（“to”的乱码，推断为“到”）``（“inherit”的乱码，推断为“继承”）``（“from”的乱码，推断为“自”）

``（“AUGMENT”的乱码，推断为“AUGMENT”）

``（“DIFFERENT”的乱码，推断为“DIFFERENT”）

``（“）”的乱码，推断为“）”）

``（“More”的乱码，推断为“更多”）``（“superclass”的乱码，推断为“超类”）``（“.”的乱码，推断为“.”）``（“You”的乱码，推断为“你”）``（“cannot”的乱码，推断为“不能”）``（“do”的乱码，推断为“做”）``（“Java”的乱码，推断为“Java”）``（“=”的乱码，推断为“=”）``（“Script”的乱码，推断为“Script”）``（“because”的乱码，推断为“因为”）``（“prototype”的乱码，推断为“原型”）``（“attribute”的乱码，推断为“属性”）。

``（“Can”的乱码，推断为“可以”）``（“only”的乱码，推断为“只能”）``（“point”的乱码，推断为“指向”）``（“object”的乱码，推断为“对象”）``（“,”的乱码，推断为“,”）``（“But”的乱码，推断为“但是”）``（“class”的乱码，推断为“类”）``（“be”的乱码，推断为“被”）``（“augmented”的乱码，推断为“增强”）``（“more”的乱码，推断为“更多”）``（“mixin”的乱码，推断为“Mixin”）``（“class”的乱码，推断为“类”）``（“.”的乱码，推断为“.”）``（“Effectively”的乱码，推断为“有效地”）``（“provides”的乱码，推断为“提供”）``（“functionality”的乱码，推断为“功能”）。

``（“augment”的乱码，推断为“增强”）``（“function”的乱码，推断为“函数”）``（“fairly”的乱码，推断为“相当”）``（“simple”的乱码，推断为“简单”）``（“.”的乱码，推断为“.”）``（“Using”的乱码，推断为“使用”）``（“for”的乱码，推断为“以”）``（“.”的乱码，推断为“,”）``（“in”的乱码，推断为“在”）``（“loop”的乱码，推断为“循环”）``（“,”的乱码，推断为“，”）``（“walk”的乱码，推断为“遍历”）``（“each”的乱码，推断为“每个”）``（“mem”的乱码，推断为“成员”）``（“bers”的乱码，推断为“变量”）``（“prototype”的乱码，推断为“原型”）``（“and”的乱码，推断为“并”）``（“add”的乱码，推断为“添加”）``（“receiving”的乱码，推断为“接收的”）``（“class”的乱码，推断为“类”）``（“.”的乱码，推断为“.”）``（“If”的乱码，推断为“如果”）``（“member”的乱码，推断为“成员”）``（“already”的乱码，推断为“已经”）``（“exists”的乱码，推断为“存在”）``（“,”的乱码，推断为“，”）``（“skip”的乱码，推断为“跳过”）``（“it”的乱码，推断为“它”）``（“.”的乱码，推断为“.”）``（“Nothing”的乱码，推断为“没什么”）``（“gets”的乱码，推断为“变得”）``（“overwritten”的乱码，推断为“覆盖”）``（“receiving”的乱码，推断为“接收的”）``（“class”的乱码，推断为“类”）``（“.”的乱码，推断为“。”）

``（“01”的乱码，推断为“01”）``（“Augment”的乱码，推断为“增强”）``（“function”的乱码，推断为“函数”）``（“.”的乱码，推断为“，”）``（“10”的乱码，推断为“10”）

``（“Function”的乱码，推断为“函数”）``（“augment”的乱码，推断为“增强”）``（“：”的乱码，推断为“：”）``（“receiving”的乱码，推断为“接收”）``（“.”的乱码，推断为“的”）``（“class”的乱码，推断为“类”）``（“,”的乱码，推断为“，”）``（“giving”的乱码，推断为“给予”）``（“.”的乱码，推断为“的”）``（“class”的乱码，推断为“类”）``（“。”的乱码，推断为“。”）``（“”的乱码，推断为“”）

``（“For”的乱码，推断为“对于”）``（“：”的乱码，推断为“：”）``（“method”的乱码，推断为“方法”）``（“.”的乱码，推断为“的”）``（“have”的乱码，推断为“有”）``（“giving”的乱码，推断为“给予”）``（“.”的乱码，推断为“的”）``（“class”的乱码，推断为“类”）``（“,”的乱码，推断为“，”）``（“prototype”的乱码，推断为“原型”）``（“。”的乱码，推断为“。”）``（“”的乱码，推断为“”）

``（“If”的乱码，推断为“如果”）``（“：”的乱码，推断为“：”）``（“preceding”的乱码，推断为“前面的”）``（“.”的乱码，推断为“的”）``（“class”的乱码，推断为“类”）``（“,”的乱码，推断为“，”）``（“prototype”的乱码，推断为“原型”）``（“”的乱码，推断为“”）``（“method”的乱码，推断为“方法”）``（“”的乱码，推断为“”）``（“have”的乱码，推断为“有”）``（“”的乱码，推断为“”）``（“。”的乱码，推断为“。”）``（“”的乱码，推断为“”）

``（“Receiving”的乱码，推断为“接收”）``（“.”的乱码，推断为“的”）``（“class”的乱码，推断为“类”）``（“,”的乱码，推断为“，”）``（“prototype”的乱码，推断为“原型”）``（“”的乱码，推断为“”）``（“method”的乱码，推断为“方法”）``（“”的乱码，推断为“”）``（“have”的乱码，推断为“有”）``（“”）``（“=”的乱码，推断为“=”）``（“giving”的乱码，推断为“给予”）``（“.”的乱码，推断为“的”）``（“class”的乱码，推断为“类”）``（“,”的乱码，推断为“，”）``（“prototype”的乱码，推断为“原型”）``（“”的乱码，推断为“”）``（“method”的乱码，推断为“方法”）``（“”的乱码，推断为“”）``（“have”的乱码，推断为“有”）``（“”）``（“.”的乱码，推断为“。”）

``（“*”的乱码，推断为“*”）

``（“*”的乱码，推断为“*”）

``（“*”的乱码，推断为“*”）

``（“We”的乱码，推断为“我们”）``（“can”的乱码，推断为“可以”）``（“improve”的乱码，推断为“改进”）``（“slightly”的乱码，推断为“略微”）``（“.”的乱码，推断为“。”）``（“Let”的乱码，推断为“让”）``（“’s”的乱码，推断为“我们”）``（“say”的乱码，推断为“假设”）``（“mixin”的乱码，推断为“Mixin”）``（“class”的乱码，推断为“类”）``（“containing”的乱码，推断为“包含”）``（“several”的乱码，推断为“多个”）``（“meth”的乱码，推断为“方法”）``（“ods”的乱码，推断为“，”）``（“but”的乱码，推断为“但”）``（“only”的乱码，推断为“只”）``（“want”的乱码，推断为“想要”）``（“copy”的乱码，推断为“复制”）``（“one”的乱码，推断为“一个”）``（“or”的乱码，推断为“或”）``（“two”的乱码，推断为“两个”）``（“of”的乱码，推断为“的”）``（“them”的乱码，推断为“它们”）``（“over”的乱码，推断为“到”）``（“another”的乱码，推断为“另一个”）``（“class”的乱码，推断为“类”）``（“.”的乱码，推断为“。”）``（“With”的乱码，推断为“使用”）``（“augment”的乱码，推断为“增强”）``（“given”的乱码，推断为“给定的”）``（“previously”的乱码，推断为“之前的”）``（“,”的乱码，推断为“，”）``（“would”的乱码，推断为“会”）``（“be”的乱码，推断为“是”）``（“impossible”的乱码，推断为“不可能的”）``（“.”的乱码，推断为“。”）``（“This”的乱码，推断为“这种”）``（“new”的乱码，推断为“新的”）``（“version”的乱码，推断为“版本”）``（“looks”的乱码，推断为“看起来”）``（“for”的乱码，推断为“寻找”）``（“optional”的乱码，推断为“可选的”）``（“arguments”的乱码，推断为“参数”）``（“,”的乱码，推断为“，”）``（“and”的乱码，推断为“和”）``（“if”的乱码，推断为“如果”）``（“they”的乱码，推断为“它们”）``（“exist”的乱码，推断为“存在”）``（“,”的乱码，推断为“，”）``（“only”的乱码，推断为“只”）``（“copies”的乱码，推断为“复制”）``（“methods”的乱码，推断为“方法”）``（“with”的乱码，推断为“包含在”）``（“arguments”的乱码，推断为“参数”）`。

``（“01”的乱码，推断为“01”）``（`Augment`的乱码，推断为`Augment`）``（“function”的乱码，推断为“函数”）``（“,”的乱码，推断为“，”）``（“improved”的乱码，推断为“改进版”）``（“.”的乱码，推断为“。”）``（“10”的乱码，推断为“10”）

``（“Function”的乱码，推断为“函数”）``（“augment”的乱码，推断为“增强”）``（“：”的乱码，推断为“：”）``（“receiving”的乱码，推断为“接收”）``（“.”的乱码，推断为“的”）``（“class”的乱码，推断为“类”）``（“,”的乱码，推断为“，”）``（“giving”的乱码，推断为“给予”）``（“.”的乱码，推断为“的”）``（“class”的乱码，推断为“类”）``（“。”的乱码，推断为“。”）``（“”的乱码，推断为“”）

``（“If”的乱码，推断为“如果”）``（“：”的乱码，推断为“：”）``（“arguments”的乱码，推断为“参数”）``（“”的乱码，推断为“”）``（“[”的乱码，推断为“[”）``（“]”的乱码，推断为“]”）``（“”的乱码）``（“”的乱码，推断为“”）``（“11”的乱码，推断为“11”）``（“Only”的乱码，推断为“只”）``（“give”的乱码，推断为“给予”）``（“certain”的乱码，推断为“某些”）``（“methods”的乱码，推断为“方法”）``（“.”的乱码，推断为“。”）

``（“For”的乱码，推断为“对于”）``（“：”的乱码，推断为“：”）``（“var”的乱码，推断为“var”）``（“i”的乱码，推断为“i”）``（“=”的乱码，推断为“=”）``（“0”的乱码，推断为“0”）``（“,”的乱码，推断为“，”）``（“len”的乱码，推断为“len”）``（


# 排版后的文档

## 文档标题

这是一个示例文档，用于展示如何对内容进行排版处理。

## 语法规则

### 变量与函数

在表达式 `x = y + z` 中，加法运算符（`+`）将 `y` 与 `z` 相加，得到它们的和 `x`。

在 `List.of(item0, item1, item2)` 中，`List` 接口的工厂方法 `of()` 接受一系列的元素，返回包含它们的只读列表。

之后我们这样调用 `cmd` 命令：`cmd arg0 arg1 arg2`。

```
if (condition > value) {console.log("output")}
```

### 类与方法

创建一个 `TextArea` 实例，称为 `textInput`，用于显示和编辑多行文本。

`button` 是一个 `Button` 实例，用于触发操作。

`container` 是一个 `VBox` 实例，用于垂直排列子组件。

`scrollPane` 是一个 `ScrollPane` 实例，用于提供滚动功能。

`root` 是一个 `BorderPane` 实例，作为布局的根容器。

将 `textInput`、`button`、`scrollPane` 和 `container` 添加到 `root` 中。

### 事件处理

`onAction` 属性绑定到一个处理函数：`handleButtonClick`。

`handleButtonClick` 是一个函数，用于处理按钮点击事件。

`handleButtonClick` 方法的参数是一个 `ActionEvent` 对象。

### 布局管理

`setPrefSize(width, height)` 设置组件的首选宽度和高度。

`setPadding(Insets)` 设置组件的内边距。

`setSpacing(double)` 设置布局中组件之间的间距。

`setAlignment(Pos)` 设置组件的对齐方式。

`setStyle(String)` 设置组件的 CSS 样式。

`getChildren().add(node)` 将子节点添加到布局容器中。

## 高级特性

### 自定义组件

创建一个自定义的 `TextArea` 子类，覆盖其默认行为。

重写 `replaceText(int start, int end, String text)` 方法，限制输入的内容。

重写 `replaceSelection(String text)` 方法，限制选择替换的内容。

### 样式定制

通过 CSS 样式表定义组件的样式。

使用 `getStyleClass().add("class-name")` 方法添加样式类。

使用 `setId(String)` 方法设置组件的唯一标识符。

### 示例代码

### 创建一个简单的文本编辑器

```
TextArea textArea = new TextArea();
Button saveButton = new Button("保存");
VBox vbox = new VBox(10);
vbox.getChildren().addAll(textArea, saveButton);
Scene scene = new Scene(vbox, 400, 300);
primaryStage.setScene(scene);
primaryStage.show();
```

### 事件绑定示例

```
saveButton.setOnAction(event -> {
    String content = textArea.getText();
    System.out.println("保存内容：" + content);
});
```

### 注意事项

- 确保所有变量名、函数名、类名和路径名都使用反引号包裹。
- 代码块前后必须添加三个反引号。
- 保持内容的语义和完整性。



# 使用 Prototype Web 框架构建动态用户界面

## 页面类型

- **静态页面**：使用 `PrototypeWeb` 框架构建
- **动态页面**：使用 `PrototypeWeb` 框架构建
- **子页面**：使用 `PrototypeWeb` 框架构建
- **模态框**：使用 `PrototypeWeb` 框架构建
- **测试页面**：使用 `Pinia` 构建

# 概述

`EditView` 页面是 `PrototypeWeb` 框架的核心组成部分，它继承自 `ViewBase` 基类，包含一个输入区域和一个显示区域。

**经典**继承看起来是一种理想的技术，适用于这种情况。使用 `PrototypeWeb` 可以轻松构建动态用户界面，而无需处理复杂的 DOM 操作或手动管理状态。通过创建子类并覆盖继承的方法，我们可以改变类的行为，就像添加或覆盖原型上的方法一样。由于类之间的变化很小，继承在这种情况下是理想的。

使用 **原型** 继承

尽管 **经典** 继承与 **原型** 继承在根本上不同，但重复使用原型继承的实践真正表明，最终的代码在两者之间可以非常相似。唯一的区别在于创建对象和覆盖方法的方式上。我们可以通过创建另一个子类并覆盖相同的方法，将字段关联到另一个输出。

## `EditView` 对象

- `EditView`：`Object.create(EditViewField)`
- `var`：`this`
- `var`：`this.id`
- `this.id`：`id`
- `this.parent`：`parent`
- `this.type`：`"text"`
- `this.value`：`value`
- `this.field`：`this`（`/path.to.xxx`）
- `this.cancel`：`this`（`/path.to.xxx`）

## 创建事件

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

## 附加事件

- `attachEvent`：`function`（`/path.to.xxx`）
- `var that`：`this`
- `addEventListener(this.save， "click"， this.convertToFunction)`
- `addEventListener(this.cancel， "click"， this.convertToFunction)`

## 转换函数

- `convertTo`：`function`（`/path.to.xxx`）
- `this.field.style.display`：`"none"`
- `this.container.appendChild(this.field)`
- `this.field.type`：`"text"`
- `this.field.value`：`this.value`
- `this.container.appendChild(this.cancel)`

## 设置值函数

- `setValue`：`function`（`/path.to.xxx`）
- `return this.field.value`

## 保存函数

- `save`：`function`（`/path.to.xxx`）
- `/path.to.xxx`

## 实例化一个对象

实例化对象时，使用 `Object.create()` 函数来创建一个新对象，而不是使用 `new` 关键字。因此，`EditView` 对象没有构造函数。原型继承看起来非常不同，从实例化一个类的方法开始。

`var title`：`Object.create(EditViewField)`

## 附加事件

- `attachEvent`：`function`（`/path.to.xxx`）
- `var that`：`this`
- `addEventListener(this.save， "click"， this.convertToFunction)`
- `addEventListener(this.cancel， "click"， this.convertToFunction)`

## 创建事件

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

## 设置值函数

- `setValue`：`function`（`/path.to.xxx`）
- `this.field.value`：`value`

## 获取值函数

- `getValue`：`function`（`/path.to.xxx`）
- `return this.field.value`
- `save`：`function`（`/path.to.xxx`）
- `/path.to.xxx`

## 结束

实例化一个类，现在有了一个对象。另外，代码几乎是相同的。唯一的区别在于创建对象的方式（`Object.create()` 与 `new`）以及方法覆盖的方式（直接覆盖与通过原型链覆盖）。由于这些差异很小，继承在这种情况下是理想的。

## 取消函数

- `cancel`：`function`（`/path.to.xxx`）
- `this.field.value`：`this.value`
- `this.field.value`：`value`

## 结束

在类的实例化中，现在有了一个对象。构造函数不再使用。因此，`EditView` 对象没有构造函数。原型继承看起来非常不同，从实例化一个类的方法开始。

`var title = Object.create(EditViewField)`

`var callback`：`this`

`var callback = 0`

`success`：`function`（`/path.to.xxx`）：`alert("保存值时出错：")`

## 结束

## 示例

`if (value == null) { value = "default value"; }`

## 获取值

- `getValue`：`function`（`/path.to.xxx`）
- `return this.field.value`

## 设置值

- `setValue`：`function`（`/path.to.xxx`）
- `this.field.value`：`value`
- `this.field.value`：`value`
- `this.field.style.display`：`"none"`
- `this.container.appendChild(this.field)`

## 结束



# 原型继承

在 JavaScript 中，原型继承是语言的核心机制，它允许对象从其他对象继承属性和方法。

## 理解原型

在 JavaScript 中，每个对象都有一个指向另一个对象的内部链接，这个对象被称为它的 `原型`。当试图访问一个对象的属性时，如果该对象本身没有这个属性，JavaScript 会沿着原型链向上查找，直到找到该属性或到达链的末端。

`__proto__` 是大多数浏览器实现用来访问对象原型的属性，但它并非标准的一部分。现代 JavaScript 推荐使用 `Object.getPrototypeOf()` 和 `Object.setPrototypeOf()` 来获取和设置原型。

## 原型链

原型链是由一系列对象组成的链，每个对象都有一个指向其原型的引用。当查找一个属性时，JavaScript 会遍历整个链，直到找到该属性或到达 `null`。

## 创建对象

你可以使用 `Object.create()` 方法创建一个以指定对象为原型的对象。例如：

```javascript
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

## 总结

原型继承是 JavaScript 中实现代码复用的主要方式。理解它的工作原理对于编写高效的 JavaScript 代码至关重要。无论是使用传统的原型方式还是 ES6 的类语法，最终都依赖于原型链的机制。通过原型继承，你可以创建复杂且易于维护的对象结构。



# 排版后的文档

### 语法

### 命名空间

`this.namespace`，即 `Namespace`。

### 类

`this.class`，即 `Class`，继承自 `BaseClass`。

### 接口

`this.interface`，即 `Interface`，实现了 `Cloneable` 和 `Serializable`。

### 枚举

`this.enum`，即 `Enum`，包含 `VALUES`。

### 注解

`this.annotation`，即 `Annotation`。

### 异常

`this.exception`，即 `Exception`，继承自 `RuntimeException`。

### 函数

`this.method`，即 `Method`，是 `Function` 的子类型。

### 属性

`this.prop`，即 `Prop`，包含 `getter` 和 `setter`。

## 标准库

### 核心类型

`this.types`，即 `Types`，包含 `int`、`float`、`string` 和 `bool`。

### 容器

`this.collections`，即 `Collections`，包含 `List`、`Map` 和 `Set`。

## 工具

#### 文件系统

`this.fs`，即 `Fs`，包含 `File`、`Path`、`Directory` 和 `Symlink`。

## 高级特性

### 反射

`this.reflect`，即 `Reflect`，包含 `Class`、`Method` 和 `Property`。

---

## 标准库详细说明

### 核心类型

`this.numbers`，即 `Numbers`，包含 `int`、`float`、`double` 和 `long`。

`this.strings`，即 `Strings`，包含 `string` 和 `char`。

`this.booleans`，即 `Booleans`，包含 `bool`。

`this.arrays`，即 `Arrays`，包含 `array` 和 `list`。

### 容器

`this.collections`，即 `Collections`，包含 `List`、`Map` 和 `Set`。

#### 文件系统

`this.fs`，即 `Fs`，包含 `File`、`Path`、`Directory` 和 `Symlink`。

### 异常

`this.exceptions`，即 `Exceptions`，包含 `Exception`、`RuntimeException` 和 `Error`。

---

## 类型系统

### 类型定义

`this.types`，即 `Types`，包含 `int`、`float`、`string`、`bool`、`array` 和 `object`。

### 类型别名

`this.type_alias`，即 `TypeAlias`，是 `Type` 的别名。

---

## 标准库结构

`this.std`，即 `Std`，包含 `Core`、`Collections`、`IO`、`Math` 和 `Text`。

### 核心库

`this.core`，即 `Core`，包含 `Object`、`String`、`Number`、`Boolean` 和 `Function`。

### 集合库

`this.collections`，即 `Collections`，包含 `List`、`Map`、`Set` 和 `Queue`。

### IO 库

`this.io`，即 `IO`，包含 `File`、`Stream`、`Reader` 和 `Writer`。

### 数学库

`this.math`，即 `Math`，包含 `Math`、`Random` 和 `BigDecimal`。

### 文本库

`this.text`，即 `Text`，包含 `String`、`StringBuilder` 和 `StringBuffer`。

---

## 容器类型

`this.collections`，即 `Collections`，包含：

`this.list`，即 `List`，是 `Collection` 的子类型。

`this.map`，即 `Map`，是 `Collection` 的子类型。

`this.set`，即 `Set`，是 `Collection` 的子类型。

`this.array`，即 `Array`，是 `Collection` 的子类型。

`this.queue`，即 `Queue`，是 `Collection` 的子类型。

`this.stack`，即 `Stack`，是 `Collection` 的子类型。

`this.deque`，即 `Deque`，是 `Collection` 的子类型。

---

## 标准库扩展

`this.std_ext`，即 `StdExt`，包含：

`this.math_ext`，即 `MathExt`，包含 `Math`、`Random` 和 `BigDecimal`。

`this.text_ext`，即 `TextExt`，包含 `String`、`StringBuilder` 和 `StringBuffer`。

`this.io_ext`，即 `IOExt`，包含 `File`、`Stream`、`Reader` 和 `Writer`。

`this.net_ext`，即 `NetExt`，包含 `Socket`、`ServerSocket` 和 `URL`。

`this.thread_ext`，即 `ThreadExt`，包含 `Thread`、`Runnable` 和 `Executor`。

---

### 异常

`this.exceptions`，即 `Exceptions`，包含 `Exception`、`RuntimeException` 和 `Error`。

---

### 函数

`this.functions`，即 `Functions`，包含 `Function`、`Consumer` 和 `Supplier`。

### 属性

`this.properties`，即 `Properties`，包含 `Property`、`ReadOnlyProperty` 和 `WriteOnlyProperty`。

---

### 类型转换

`this.type_cast`，即 `TypeCast`，包含 `cast`、`as` 和 `is`。

---

## 综合示例

`this.example`，即 `Example`，展示了如何使用 `this.namespace`、`this.class`、`this.interface`、`this.enum` 和 `this.annotation`。

## 总结

`this.summary`，即 `Summary`，包含了所有核心类型、容器、文件系统和异常。

---

## 附录

### 标准库索引

`this.std_index`，即 `StdIndex`，包含了所有标准库的索引。

### 类型系统索引

`this.type_index`，即 `TypeIndex`，包含了所有类型的索引。

### 容器索引

`this.collection_index`，即 `CollectionIndex`，包含了所有容器的索引。

### 异常索引

`this.exception_index`，即 `ExceptionIndex`，包含了所有异常的索引。

### 函数索引

`this.function_index`，即 `FunctionIndex`，包含了所有函数的索引。

### 属性索引

`this.property_index`，即 `PropertyIndex`，包含了所有属性的索引。

---

## 高级特性

### 泛型

`this.generics`，即 `Generics`，支持类型参数。

### 反射

`this.reflect`，即 `Reflect`，支持运行时类型检查。

### 注解

`this.annotations`，即 `Annotations`，支持元数据。

### 函数式编程

`this.fp`，即 `FP`，支持高阶函数和 lambda 表达式。

### 异步编程

`this.async`，即 `Async`，支持协程和 Future。

## 安全性

`this.security`，即 `Security`，支持加密和认证。

## 性能

`this.performance`，即 `Performance`，支持性能分析和优化。

## 测试

`this.testing`，即 `Testing`，支持单元测试和集成测试。

## 文档

`this.documentation`，即 `Documentation`，支持自动生成文档。

## 国际化

`this.i18n`，即 `I18n`，支持多语言和本地化。

## 日志

`this.logging`，即 `Logging`，支持日志记录和审计。

#### 错误处理

`this.error_handling`，即 `ErrorHandling`，支持错误处理和恢复。

## 调试

`this.debugging`，即 `Debugging`，支持调试和诊断。

## 部署

`this.deployment`，即 `Deployment`，支持部署和运维。

## 监控

`this.monitoring`，即 `Monitoring`，支持监控和告警。

## 配置

`this.configuration`，即 `Configuration`，支持配置管理。

## 工具

`this.tools`，即 `Tools`，支持各种实用工具。

---

## 附录 A：标准库索引

### 核心类型

`this.types`，即 `Types`，包含 `int`、`float`、`string`、`bool`、`array` 和 `object`。

### 容器

`this.collections`，即 `Collections`，包含 `List`、`Map`、`Set`、`Queue`、`Stack` 和 `Deque`。

#### 文件系统

`this.fs`，即 `Fs`，包含 `File`、`Path`、`Directory` 和 `Symlink`。

### 异常

`this.exceptions`，即 `Exceptions`，包含 `Exception`、`RuntimeException` 和 `Error`。

### 函数

`this.functions`，即 `Functions`，包含 `Function`、`Consumer` 和 `Supplier`。

### 属性

`this.properties`，即 `Properties`，包含 `Property`、`ReadOnlyProperty` 和 `WriteOnlyProperty`。

---

## 附录 B：类型系统索引

### 类型定义

`this.types`，即 `Types`，包含 `int`、`float`、`string`、`bool`、`array` 和 `object`。

### 类型别名

`this.type_alias`，即 `TypeAlias`，是 `Type` 的别名。

### 类型转换

`this.type_cast`，即 `TypeCast`，包含 `cast`、`as` 和 `is`。

---

## 附录 C：容器索引

### 列表

`this.list`，即 `List`，是 `Collection` 的子类型。

### 映射

`this.map`，即 `Map`，是 `Collection` 的子类型。

### 集合

`this.set`，即 `Set`，是 `Collection` 的子类型。

### 队列

`this.queue`，即 `Queue`，是 `Collection` 的子类型。

### 栈

`this.stack`，即 `Stack`，是 `Collection` 的子类型。

### 双端队列

`this.deque`，即 `Deque`，是 `Collection` 的子类型。

### 数组

`this.array`，即 `Array`，是 `Collection` 的子类型。

---

## 附录 D：异常索引

### 运行时异常

`this.runtime_exceptions`，即 `RuntimeExceptions`，包含 `RuntimeException`、`NullPointerException` 和 `IndexOutOfBoundsException`。

### 检查异常

`this.checked_exceptions`，即 `CheckedExceptions`，包含 `IOException`、`ClassNotFoundException` 和 `InterruptedException`。

### 错误

`this.errors`，即 `Errors`，包含 `Error`、`StackOverflowError` 和 `OutOfMemoryError`。

---

## 附录 E：函数索引

### 函数类型

`this.function_types`，即 `FunctionTypes`，包含 `Function`、`Consumer` 和 `Supplier`。

### 高阶函数

`this.higher_order_functions`，即 `HigherOrderFunctions`，包含 `map`、`filter` 和 `reduce`。

### Lambda 表达式

`this.lambda_expressions`，即 `LambdaExpressions`，支持匿名函数。

---

## 附录 F：属性索引

### 属性类型

`this.property_types`，即 `PropertyTypes`，包含 `Property`、`ReadOnlyProperty` 和 `WriteOnlyProperty`。

### 属性绑定

`this.property_binding`，即 `PropertyBinding`，支持数据绑定。

### 属性变更通知

`this.property_change_notification`，即 `PropertyChangeNotification`，支持属性变更监听。

---

## 常见问题

### 如何在 `this.namespace` 中定义一个类？

使用 `this.class` 关键字。

### 如何在 `this.class` 中声明一个方法？

使用 `this.method` 关键字。

### 如何在 `this.class` 中声明一个属性？

使用 `this.prop` 关键字。

### 如何实现一个接口？

使用 `this.implements` 关键字。

### 如何继承一个类？

使用 `this.extends` 关键字。

---

## 最佳实践

### 命名规范

`this.naming_conventions`，即 `NamingConventions`，建议使用驼峰命名法。

### 代码组织

`this.code_organization`，即 `CodeOrganization`，建议按功能模块组织代码。

#### 错误处理

`this.error_handling_best_practices`，即 `ErrorHandlingBestPractices`，建议使用 try-catch 块处理异常。

### 性能优化

`this.performance_optimization`，即 `PerformanceOptimization`，建议使用缓存和延迟加载。

---

## 结语

本文档展示了 `this.std`、`this.core`、`this.collections`、`this.io`、`this.math` 和 `this.text` 等标准库的用法。通过 `this.example`，您可以了解如何使用 `this.namespace`、`this.class`、`this.interface`、`this.enum` 和 `this.annotation` 来构建健壮的应用程序。

---

## 贡献指南

`this.contribution`，即 `Contribution`，欢迎提交 Pull Request 和 Issue。

## 版本历史

`this.version_history`，即 `VersionHistory`，记录了所有版本的重大变更和修复。

## 许可证

`this.license`，即 `License`，项目采用 MIT 许可证。

---

## 联系我们

`this.contact`，即 `Contact`，欢迎通过电子邮件和社交媒体与我们联系。

---

**注意**：本文档中的示例代码和说明仅供参考，实际使用时请根据具体语言和框架进行调整。



# 排版后的内容

继承是面向对象编程中的一个核心概念，它允许一个类基于另一个类来定义，从而继承其字段和方法。通过继承，子类可以重用父类的代码，并添加或修改特定的行为。

在面向对象编程中，继承是一种强有力的机制，它使得程序员能够创建更复杂的类层次结构，其中子类继承父类的属性和方法。通过继承，类可以形成树状结构，子类自动获得父类的所有公共成员，从而简化了代码的复用和扩展。

使用继承时，子类可以重写（覆盖）父类的方法，提供更具体的实现。这允许同一个方法在不同的子类中表现出不同的行为，实现了多态。

在 Java 中，使用`extends`关键字来表示一个类继承另一个类。例如，`class Dog extends Animal`表示`Dog`类继承了`Animal`类。

继承使得代码复用变得更加容易，因为子类无需重新编写已经在父类中定义的方法和字段。通过继承，开发者可以创建更少的重复代码，并且更容易维护和扩展。

当使用继承时，需要注意不要滥用它。继承应该只在子类与父类之间存在“is-a”（是一个）关系时使用。例如，`Dog`是一个`Animal`，所以继承是合理的。继承不适用于仅仅为了代码复用而随意使用的情况。

在 Java 中，继承是通过`extends`关键字来实现的。Java 只支持单继承，即一个类只能直接继承一个父类。这避免了多继承带来的复杂性和歧义，但也意味着在需要多重功能时，需要结合接口（`interface`）来使用。

尽管如此，Java 通过接口提供了多继承的能力，因为一个类可以实现多个接口。接口定义了一组方法签名，但不提供实现。通过实现接口，一个类可以获得多种类型的行为。

在面向对象设计中，继承和组合（Composition）是两种主要的复用方式。组合是通过将一个对象作为另一个对象的属性来实现的。选择继承还是组合取决于具体的需求。如果子类与父类之间存在“is-a”关系，那么继承是合适的选择。否则，组合通常是更好的选择。

继承的另一个重要方面是它可能增加代码的复杂性。过深的继承层次会导致代码难以理解和维护。因此，应该谨慎使用继承，并尽量保持层次结构扁平。

简单来说，`Prototype`模式通过创建一个原型对象，然后克隆它来创建新对象，而不是通过继承来创建子类。原型模式非常适合那些需要避免继承带来的复杂性，或者需要动态创建对象的场景。

在本章中，我们讨论了继承的优缺点，以及三种实现继承的方式：经典继承（Classical Inheritance）、原型继承（Prototype Inheritance）和混入（Mixins）。

`JavaScript`的继承机制与其他面向对象的语言（如 Java 和 C++）不同。它是基于原型的，而不是基于类的。这意味着对象可以直接继承其他对象，而不需要类层级结构。虽然 ES6 引入了`class`关键字，但它只是原型继承的语法糖。

原型继承在`JavaScript`中通过创建对象，然后将其链接到另一个对象来创建子对象。使用`Object.create()`方法，你可以创建一个新对象，并将其原型指向另一个对象。这使你能够创建一个相对扁平的继承树，而无需复杂的类层级。

使用`Object.create()`，你可以轻松创建继承关系。例如，`var child = Object.create(parent)`会创建一个新对象`child`，其原型指向`parent`对象。然后，你可以为`child`添加新的属性和方法，而不会影响`parent`对象。

简单来说，原型继承更适合那些需要动态创建对象，并且继承层次较浅的场景。它避免了类继承的复杂性，使得代码更加灵活。

使用这些技术，可以创建复杂的对象层级，而不需要继承。通过组合，你可以将多个对象的功能组合在一起，实现代码的复用。继承在`JavaScript`中并不是必须的，你可以通过组合来达到同样的目的。

## 摘要

在本章中，我们讨论了继承的优缺点，以及三种实现继承的方式：经典继承、原型继承和混入。

经典继承试图模拟类继承的方式，即子类继承父类，并可以重写父类的方法。它非常适合那些需要代码复用和清晰层次结构的场景。但是，经典继承可能会导致代码复杂化，特别是当继承层级过深时。

原型继承则是`JavaScript`的原生继承方式。它通过对象之间的链接来实现继承，避免了类的层级结构。原型继承非常适合那些需要动态创建对象的场景，或者需要避免经典继承复杂性的场景。

混入（Mixins）是一种将多个对象的属性和方法合并到一个对象中的方式。它不同于继承，因为它不创建父子关系。混入非常适合那些需要将多个不相关的对象的功能组合在一起的场景。

在你学习了这些技术之后，你将发现简单的`JavaScript`程序很少需要深层次的继承。只有大型项目，尤其是涉及多个程序员参与的项目，这种组织方式才会变得必要。

## 总结

在本章中，我们讨论了几种对象继承的方式，以及它们的使用场景。经典继承试图模拟类继承的方式，即子类继承父类，并可以重写父类的方法。经典继承非常适合那些需要代码复用和清晰层次结构的场景。

原型继承则更适合那些需要动态创建对象，并且继承层次较浅的场景。它避免了类继承的复杂性，使得代码更加灵活。

混入（Mixins）是将多个对象的属性和方法合并到一个对象中的方式。它非常适合那些需要将多个不相关的对象的功能组合在一起的场景。

在 Java 和 C++中，使用经典继承需要小心，因为它可能会导致代码复杂化。而在 JavaScript 中，使用原型继承可以避免这些问题，使得代码更加灵活和易于维护。

在面向对象编程中，选择合适的方式取决于具体的场景。对于简单的程序，使用原型继承或混入通常更合适。而对于复杂的程序，尤其是需要深层次继承的场景，经典继承可能是更好的选择。但无论如何，都应该避免过度使用继承，因为它会增加代码的复杂性。



# 单例模式

单例是一种创建型设计模式，让你能够保证一个类只有一个实例，并提供一个访问该实例的全局节点。

单例拥有与全局变量相同的优缺点，但它更加简洁且易于使用。它确保了只有一个`Singleton`实例，并且该实例可以被轻松地访问到。

全局变量会污染命名空间，并且可能导致代码难以维护。而单例模式则提供了一种更优雅的方式来管理全局状态。

单例模式通常用于管理共享资源，例如数据库连接、配置对象等。

它提供了一种简单的方式来访问这些资源，确保整个应用程序中只有一个实例存在。

单例类通常使用私有构造函数来防止外部实例化，并通过静态方法（如`getInstance()`）来提供对唯一实例的访问。这种方式可以确保全局变量的数量被限制在最小范围内。

在`JavaScript`中，单例模式通过确保一个类只有一个实例，并提供对该实例的全局访问，从而减少全局变量的数量。它们可以被用于命名空间，这有助于封装浏览器差异，并通过一种技术称为“分支”来减少全局变量的数量。

单例模式是最基本的设计模式之一，但它可以用于许多不同的目的。它涵盖了最有用的一种模式——单例，这本身就是一个重大的飞跃。从实现的角度来看，单例模式本质上是对象字面量，因为它们在`JavaScript`中是最简单的创建对象的方式。

单例模式是`JavaScript`中最常用的模式之一。它本质上是一个对象，只能被实例化一次，并通过一个众所周知的访问点进行访问。严格遵循此定义，之前的示例不是一个单例，因为它不是一个可实例化的类。我们选择将单例模式定义得更宽泛一些：它是一个用于命名空间和分组相关方法和属性的对象集合，并且如果它在所有地方都是可实例化的，那么它只能被实例化一次。

使用对象字面量是创建单例的唯一技术。本章后面介绍的其他方法更像是以单例形式呈现的类。

另外，并非所有对象字面量都是单例。如果它仅仅用于模拟一个关联数组，或者用于传递一组相关参数到函数中，那么它就不是一个单例。

### 单例的基本结构

#### 在`JavaScript`中的实现方式

在`Singleton`模式中，所有成员现在都可以通过`Singleton`变量访问。

你可以使用点符号（`Singleton.attribute = false`）访问它们。

`var total = Singleton.attribute;`

`var result = Singleton.method(args);`

### 单例的使用场景

#### 用于命名空间

单例模式用于命名空间，即把相关方法和属性组织在一起，以减少全局变量的数量。在`JavaScript`中，单例模式尤其常见，因为它可以帮助组织代码，减少全局作用域的污染。

单例对象可以通过点符号（`.`）访问其方法和属性。你可以通过在一个单例对象中定义所有相关的方法和属性，来实现简单的命名空间。这可以作为组织代码的第一步，也是最重要的一步。

单例模式还可以用于管理共享资源。例如，你可以使用单例模式来管理一个数据库连接池。

```javascript
Singleton.attribute = false;
var total = Singleton.attribute;
var result = Singleton.method(args);
```

#### 用于管理共享资源

单例模式常用于管理共享资源，例如数据库连接、配置对象等。它提供了一种简单的方式来访问这些资源，确保整个应用程序中只有一个实例存在。

你可以使用单例模式来管理一个数据库连接池，或者一个配置对象。

```javascript
var Singleton = {
  attribute: true,
  method: function (args) {
    // 方法体
  }
};
```

#### 用于实现分支

分支是一种技术，它允许你根据浏览器的能力或特性来定义不同的方法实现。单例模式可以用于实现分支，因为它允许你根据条件选择不同的实现。

例如，你可能需要根据浏览器是否支持`XMLHttpRequest`来使用不同的网络请求实现。

```javascript
var Singleton = {
  method: function () {
    if (typeof XMLHttpRequest !== 'undefined') {
      // 使用 XMLHttpRequest
    } else {
      // 使用其它方法
    }
  }
};
```

#### 区别

单例模式与其它对象字面量的主要区别在于：

- 单例模式通常用于命名空间、管理共享资源和实现分支。
- 单例模式通常只有一个实例。
- 单例模式通常通过一个全局访问点来访问。

对象字面量只是单例模式的一种实现方式。单例模式的核心思想是确保一个类只有一个实例，并提供一个全局访问点。



# 单例对象的命名空间

Singleton 是一个用于确保单个全局实例的设计模式。它由两部分组成：对象本身以及其构造函数。

# 概述

Singleton 对象由两部分构成：对象本身，包含成员；以及其构造函数，用于访问它。构造函数是全局可访问的。

Singleton 是 JavaScript 中一个重要的部分，因为它常用于命名空间。在 JavaScript 中，通过使用单例对象，您可以将您的代码组织得井井有条。

因为全局命名空间容易受到污染，所有成员都应包含在单例对象中。它们不应全局可访问。通过使用带有闭包的模块模式，您可以模拟面向对象编程中的私有成员。

由于通常应用于整个应用程序的全局命名空间争用，命名空间是单例对象的一个关键特性。因此，单例对象应该是任何 JavaScript 程序的重要组成部分。它通常是作用域内的一个全局对象。

因为所有成员都包含在单例对象内，它们只能通过该对象的变量进行访问。这意味着变量不是全局的。在某种意义上，它们可以被视为对象内部的命名空间。

## 命名约定

目前，大多数 JavaScript 作者使用驼峰式命名（camelCase），这是一个良好的实践。除非使用构造函数，否则不应使用帕斯卡命名（PascalCase）。然而，一些第三方库的名称是全局的。通过使用单例对象，您可以将所有代码和变量组织在一个对象中。

当您使用单例对象时，您可以将所有变量或数据分组到一个全局变量下。然后，您可以将所有这些变量分组到单例对象或单例对象内的其他嵌套对象中。

## 单例与字母常量

`MyNamespace` 是一个用于单例全局变量的命名约定。它被选中是因为内部全局变量被声明为 `const` 或 `let`，而全局命名空间只用于标识该对象。所有成员都应包含在单例对象内。在大型应用程序中，多个开发人员共同协作时，使用 const 或 let 声明变量可以避免意外的命名冲突。

您的单例对象应该是唯一的，并且一旦创建就不能更改。单例在应用程序的整个生命周期中只被实例化一次。在大型项目中，由于代码可能由多个库、广告、第三方模块、小部件等组成，因此命名空间冲突很常见。为避免混淆，您应该将共享代码组合到单例对象下面。

# 命名空间的作用域

虽然每个第三方库都不同，但大多数都会声明一个单一的全局变量来包含所有代码。这个变量通常称为“命名空间”。您应该在您的代码中也这样做，将对象字面量用作命名空间。这意味着您可以将所有代码放在一个全局变量下，从而使您的代码库高度模块化和可维护。

# 相关文章

- [如何使用单例对象](http://www.yoururl.com)
- [全局变量的危险](http://www.yoururl.com)

---
*想要了解更多关于命名空间的知识？请阅读我们的下一篇文章。*



UI 框架（UI Framework）通常由两个核心部分构成：应用程序对象（Application Object）与页面集合（Page Collection）。

前者（Application Object）是一个用于页面级生命周期的容器（Container），用于容纳各个页面（Page）。

这意味着该框架提供了如何创建单例对象（Singleton Object）的名称空间（Namespace）的蓝图。它通常像一种模式（Pattern）或模板（Template）来使用。

单例（Singleton）的用途在于页面模式（Page Pattern）。在多页面（Multi-page）站点中，你经常会看到它的使用；而在绝大多数情况下，它专门用于单页面（Single Page）。

它并不在任何其他地方使用。它是一个将这两者分开的好思路（Idea），即创建两个独立的单例（Singleton）对象。

包裹页面集合（Page Collection）的容器（Container）通常具有与页面集合类似的页面级生命周期。它需要封装一些可能包含页面级活动（Page-level Activity）方法和初始化（Initialization）方法的数据。

大部分涉及具体元素（Specific Elements）的代码只能在像 DOM 这样的事件附件（Event Attachment）中执行一次，这些元素被加载后才行。

通过创建一个初始化（Init）方法并将它附着到窗口（Window）的加载（Load）事件中（或者类似的加载事件），你可以将所有初始化（init）代码集中在一个地方。

这里有一个用于包裹页面级生命周期（Page-level Lifecycle）的单例（Singleton）的框架：

```
01. 通用页面对象
   Namespace.PageName = new function() {}
   
01. 页面常量
   CONSTANTS = true
   CONSTANTS = false
   
01. 页面方法
   method.function() {}
   
   // 更多信息，前往 http://petermichaux.com/articles/singleton
   COPYRIGHT = 2011
   THE MIT LICENSE
~~
   method.function() {}
~~
01. 初始化方法
   init.function() {}
   
   
01. 在页面加载后调用初始化方法
   addLoadEvent(Namespace.PageName.init) {}

要解释这是如何使用的，让我们先看一个在 Web 开发中比较常见的任务，并逐步分析它。

假设 Namespace.PageName 已经作为一个空对象被创建。如果还没创建，这第一行代码会使用布尔值 OR 操作符来提供一个默认值。

```
var Namespace.PageName = window.Namespace.PageName || {}
```

在此例中，我们为两个我们关心的 DOM 元素（适用于常量中定义的常量）放置了 DOM 引用（References），因为它们在程序执行中不会改变。

初始化（init）方法获取这两个 DOM 元素并存储它们作为新的属性。

```
var data = {inputs: form.getElements()}
var data = {form: getElement}
   
99. 将格式化后的值发回服务器。
   Namespace.PageName.sendRegistration(data) {}
   
01. 初始化方法
   init.function() {}
   
   在页面加载后调用初始化方法。
   addLoadEvent(Namespace.PageName.init) {}
   
我们首先假设 `Namespace.PageName` 已经被创建为一个空对象。如果还没有，这第一行会报错。这个错误可以通过一行创建 Namespace.PageName（如果还不存在）的代码来避免，使用布尔或（OR）操作符：

```
var Namespace.PageName = window.Namespace.PageName || {}
```

在这个例子中，我们把 `var` 语句换成两个我们关心的 DOM 元素（在常量中定义的常量）。因为它们在程序运行期间不会改变。

`init` 方法获取这两个 DOM 元素并将它们存储为新属性。



# 单例模式

单例模式是创建型设计模式之一，其核心目标是确保一个类只有一个实例，并提供全局访问点来获取该实例。

当应用程序中的某个对象需要被精确地共享，并且所有操作都通过该单个实例进行时，单例模式便成为理想选择。原因在于，每次创建新实例都会消耗系统资源，而单例模式通过复用已有实例来避免这种开销。

单例模式确保一个类只有一个实例，并提供一个全局访问点。这种设计模式在框架和库中广泛应用，例如`Singleton`模式、`Logger`类、`DataSource`单例、`Configuration`管理器等。

在单例模式中，构造函数是私有的，外部代码无法直接实例化该类。相反，类本身负责创建和管理其唯一实例，并提供一个静态方法来返回该实例。

## 使用单例模式的场景

单例模式常用于需要控制共享资源访问的场景，例如数据库连接池、线程池、日志记录器、配置管理器等。在这些场景中，确保只有一个实例可以避免资源冲突和不一致的状态。

## 实现单例模式的注意事项

单例模式是最直接且最常用的实现方式之一。它适用于以下情形：

- 确保一个类只有一个实例
- 提供一个全局访问点来控制对该实例的访问

但需要注意的是，单例模式也存在一些缺点：

- 违反单一职责原则（SRP）：单例类不仅负责管理自己的实例，还负责业务逻辑
- 在多线程环境下需要考虑线程安全
- 测试困难：由于全局状态的存在，单例模式可能使单元测试变得复杂

## 单例模式的实现方式

### 懒汉式单例模式

```java
public class Singleton {
    private static Singleton instance;
    
    private Singleton() {}
    
    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```

### 饿汉式单例模式

```java
public class Singleton {
    private static final Singleton instance = new Singleton();
    
    private Singleton() {}
    
    public static Singleton getInstance() {
        return instance;
    }
}
```

### 双检锁/双重校验锁单例模式

```java
public class Singleton {
    private volatile static Singleton instance;
    
    private Singleton() {}
    
    public static Singleton getInstance() {
        if (instance == null) {
            synchronized (Singleton.class) {
                if (instance == null) {
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}
```

## 选择单例模式的依据

在单例模式中，最常见的实现方式是懒汉式和饿汉式。选择哪种方式取决于以下因素：

- 如果实例创建开销较大且不一定会被使用，推荐使用懒汉式
- 如果实例一定会被使用且创建开销较小，推荐使用饿汉式

饿汉式是线程安全的，因为类加载时就完成了实例化；懒汉式则存在线程安全问题，需要额外的同步机制。

使用`StringToArray`方法时，它接受一个字符串、一个分隔符和一个可选的布尔值，该布尔值指示是否移除所有空白字符。该方法使用两个私有方法来完成大部分工作——`StringToWhiteSpace`和`StringSplit`。这些方法不应该被公开，因为它们不是单例文档接口的一部分，并且不能保证在下一个更新中仍然存在。保持这些方法私有允许你重构所有内部代码而不用担心破坏其他人的程序。

让我们再来看一个例子。在`StringToArray`方法中，这是在一个单例对象中使用了一个公共方法，`StringToArray`。该方法接受参数：一个`string`、一个`delimiter`和一个可选的`boolean`，该布尔值告诉方法是否移除所有白色空格。这个方法使用两个私有方法来完成任务：`StringToWhiteSpace`和`StringSplit`。这些方法不应该被公开，因为它们不是单例文档接口的一部分，并且不能保证在下一个更新中仍然存在。保持这些方法私有允许你重构所有内部代码而不用担心破坏其他人的程序。

## 使用闭包

第二种获取私有成员的方法是在一个单例对象中创建一个闭包。这种方式看起来很像我们在第二章中创建真实私有成员的方式，但有一个主要区别。在此之前，我们向构造器主体中添加了变量和函数。而这次，我们是将变量和函数添加到构造器主体中，但没有`this`。这一区别——将变量和函数添加到构造器主体中，而不是添加到`this`——使得它们成为私有成员。

## 单例模式总结

单例模式的关键在于确保一个类只有一个实例，并提供全局访问点。它通过私有构造函数、静态实例变量和静态访问方法来控制实例的创建和访问。

单例模式适用于需要控制共享资源访问的场景，但需要谨慎使用，因为不当的实现可能导致线程安全问题和测试困难。



# 使用 `Singleton` 类型的 `Singleton.kt` 文件

`Singleton` 文件位于 `src/main/kotlin` 目录下，是 `Singleton.kt` 示例的一部分。

## 如何声明 `Singleton.kt` 文件

声明 `Singleton.kt` 文件的方法与声明普通类相似。您只需在 `class` 关键字前加上 `object` 关键字。

### 使用 `Private` 构造器的 `Singleton` 模式

`Singleton.kt` 文件是 `Singleton` 模式的 `Kotlin` 实现。它是一个 `Singleton` 对象，只能被实例化一次。

#### 代码示例: 基本 `Singleton` 声明

```
object Singleton.kt
```

### 带 `Private` 构造器的 `Singleton` 模式 (第二步)

```
object Singleton.kt
```

## 带有 `Private` 成员变量的 `Singleton` 模式

您还可以为同一个 `Singleton` 对象添加 `Private` 成员变量。

### 代码示例: 带有 `Private` 成员变量的 `Singleton`

```
object Singleton.kt {
    private var count: Int = 0

    fun incrementCount() {
        count++
    }

    fun getCount(): Int {
        return count
    }
}
```

## 示例：使用 `StringTokenizer` 类

在下面的示例中，我们创建了一个 `StringTokenizer` 对象，并使用它的方法来处理字符串。

`StringTokenizer` 类用于将字符串分割成多个子字符串。

### `StringTokenizer` 示例

```
StringTokenizer st = new StringTokenizer("this is a test");
while (st.hasMoreTokens()) {
    System.out.println(st.nextToken());
}
```

## `Singleton.kt` 示例：将字符串转换为 `int`

在接下来的示例中，您将学习如何使用 `Integer.parseInt()` 方法将字符串转换为整数。

```
public class StringToIntExample {
    public static void main(String[] args) {
        String str = "123";
        int num = Integer.parseInt(str);
        System.out.println(num);
    }
}
```

## `Singleton` 模式的使用

`Singleton` 模式是一种创建型设计模式，它确保一个类只有一个实例，并提供一个全局访问点。

### 例子：实现 `Singleton` 模式

```
public class Singleton {
    private static Singleton instance;

    private Singleton() {}

    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```

## 进阶主题：带参数的 `Singleton` 构造器

您可以通过在 `Singleton` 类中添加一个带有参数的私有构造器，并使用 `getInstance()` 方法来初始化 `Singleton` 实例，从而创建一个带参数的 `Singleton`。

### 示例：带参数的 `Singleton`

```
public class Singleton {
    private static Singleton instance;
    private String name;

    private Singleton(String name) {
        this.name = name;
    }

    public static Singleton getInstance(String name) {
        if (instance == null) {
            instance = new Singleton(name);
        }
        return instance;
    }

    public String getName() {
        return name;
    }
}
```

## 在 `Kotlin` 中使用 `Singleton`

在 `Kotlin` 中，使用 `object` 关键字声明一个对象，即可创建一个 `Singleton`。`Kotlin` 的 `object` 声明会创建一个类，并且同时创建一个该类的唯一实例。

```
object MySingleton {
    fun doSomething() {
        println("Doing something")
    }
}
```

## 总结

`Singleton` 模式是一种常用的设计模式，它确保一个类只有一个实例，并提供一个全局访问点。

## 外部链接

### 参考

- [Singleton 模式 - 维基百科](https://zh.wikipedia.org/wiki/单例模式)



# 公共构造函数 SingleInstance

## 单例模式的构造函数

通过将构造函数声明为 `private`，可以防止从类外部创建实例。

# 概述

在单例模式中，构造函数被声明为 `private`，这意味着它只能从类内部访问。通过将构造函数设为 `private`，你可以防止外部代码创建新的实例。这样做可以确保类只能有一个实例，从而控制对某个对象的访问。

使用 `private` 构造函数，你可以在类内部管理实例化过程。这种模式通常用于需要全局访问点的场景，比如配置管理器、日志记录器或资源池。通过将构造函数设为 `private`，你可以在类内部控制实例化的细节，而外部代码无法直接创建实例。

### 语法

`private ClassName() { }`

### 关键点

- `Singleton()` 构造函数是 `private` 的，因此无法从类外部访问。
- 类中的唯一实例是通过 `getInstance()` 方法返回的。
- 类的客户端只能通过 `getInstance()` 方法来获取实例。

### 懒汉式实例化（Lazy Initialization）

在某些情况下，你可能不希望实例在类加载时就被创建，而是希望等到第一次需要时才创建。这种模式称为**懒汉式实例化**。使用懒汉式实例化，实例只有在第一次调用 `getInstance()` 方法时才会被创建。

懒加载单例的通用结构如下：

```java
public class Singleton {
    private static Singleton instance = null;

    private Singleton() { }

    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```

`getInstance()` 方法首先检查 `instance` 是否为 `null`。如果是，则创建一个新的 `Singleton` 实例并返回；如果不是，则直接返回现有实例。

### 懒汉式 vs 饿汉式

懒汉式实例化的主要优势在于，实例只有在真正需要时才会创建，从而节省资源。然而，懒汉式实现不是线程安全的，如果多个线程同时调用 `getInstance()`，可能会创建多个实例。而饿汉式实现（直接在类加载时创建实例）则是线程安全的，但可能会浪费资源。

### 线程安全

如果你希望在多线程环境中使用懒汉式单例，你需要确保线程安全。一个简单的方法是将 `getInstance()` 方法声明为 `synchronized`。然而，这种方法可能会影响性能，因为每次调用 `getInstance()` 都会同步。更好的方法是使用**双重检查锁定**模式，这可以确保实例只被创建一次，同时避免不必要的同步开销。

### 懒汉式实例化的例子

```java
public class LazySingleton {
    private static LazySingleton instance;

    private LazySingleton() { }

    public static synchronized LazySingleton getInstance() {
        if (instance == null) {
            instance = new LazySingleton();
        }
        return instance;
    }
}
```

### 饿汉式实例化的例子

```java
public class EagerSingleton {
    private static final EagerSingleton instance = new EagerSingleton();

    private EagerSingleton() { }

    public static EagerSingleton getInstance() {
        return instance;
    }
}
```

## 总结

到目前为止，我们已经讨论了单例模式的所有常见实现。它们都有一个共同点：构造函数是 `private` 的。如果你有一个单例，它的构造函数是 `private`，那么所有标准单例代码都适用于这里。

对于懒加载单例，所有标准单例代码也适用。

## 关键点二：构造函数

到目前为止，没有什么改变。第一步是将单例中的所有代码移入构造函数方法中。

```java
MyNamespace.LazySingleton -> func() ->
    func constructor() -> All of the normal singleton code goes here ->
    var privateAttr = false;
    var privateAttr2 = null;
    func privateMethod() -> 
        ...
    privateAttr = true;
    return public singleton instance.
```

`public singleton instance` 是 `private` 的，因此无法从外部访问。

### 懒加载单例

```java
MyNamespace.LazySingleton -> func() ->
    func constructor() -> All of the normal singleton code goes here ->
    var privateAttr = false;
    var privateAttr2 = null;
    func privateMethod() -> 
        ...
    privateAttr = true;
    return public singleton instance.
```

`public singleton instance` 是 `public` 的，因此可以从外部访问。

### 懒加载单例（带参数）

```java
MyNamespace.LazySingleton -> func(args) ->
    func constructor(args) -> All of the normal singleton code goes here ->
    var privateAttr = false;
    var privateAttr2 = null;
    func privateMethod() -> 
        ...
    privateAttr = true;
    return public singleton instance.
```

`public singleton instance` 是 `public` 的，因此可以从外部访问。

## 最终总结

这种方法使得构造函数从类外部不可访问，这是一件好事。你想要完全控制何时调用它。`public getInstance()` 方法用于实现单例模式。



看起来你提供的文本似乎包含了一些乱码或无法正确显示的字符（可能是编码问题）。这些字符无法被识别为有意义的中文或英文内容，因此无法按照要求进行排版和格式化。

请检查原文的编码格式，并重新提供可读的文本内容。一旦接收到正确的文本，我将严格按照你的要求进行 Markdown 排版。



# 标题

## 创建和分配对象

JavaScript 中的对象是键值对集合，用于存储数据和更复杂的实体。使用对象字面量语法（`{}`）是最简单的创建对象的方式，然后可以使用点或方括号表示法添加、修改或删除属性。

## 创建对象

使用 `new Object()` 语法创建一个空对象。也可以使用对象字面量语法 `{}` 创建。

## 属性操作

### 添加属性

```
let obj = new Object();
obj.key = "value";
```

### 访问属性

```
let obj = new Object();
obj.key;
```

### 删除属性

```
delete obj.key;
```

## 对象字面量

```
let obj = {key: "value"};
```

## 简写属性名

```
let key = "value";
let obj = {key};
```

## 计算属性名

```
let key = "dynamicKey";
let obj = {[key]: "value"};
```

## 方法定义

```
let obj = {
  method() {
    return "Hello";
  }
};
```

## 示例：使用工厂函数创建对象

### 使用工厂函数创建对象

```
function createObject(key, value) {
  return {
    [key]: value
  };
}
let obj = createObject("name", "test");
```

## 属性存在性检查

使用 `hasOwnProperty()` 方法检查对象自身是否包含某个属性。

```
let obj = {key: "value"};
console.log(obj.hasOwnProperty("key")); // true
console.log(obj.hasOwnProperty("other")); // false
```

# 继承与原型链

JavaScript 使用原型链实现对象间的继承。每个对象都有一个内部链接到另一个对象（其原型），该对象具有一组属性和方法。当试图访问一个对象的属性时，不仅会在该对象上查找，还会在它的原型上查找，以此类推，直到找到该属性或到达原型链末尾。

## 原型链示例：使用 `Object.create()`

`Object.create()` 方法使用现有对象作为新创建对象的原型，创建一个新对象。

```
let parent = {name: "parent"};
let child = Object.create(parent);
console.log(child.name); // parent
```

## 原型链的终点是 `null`

```
let obj = {};
console.log(Object.getPrototypeOf(Object.getPrototypeOf(obj))); // null
```

## 使用构造函数创建对象

```
function Person(name) {
  this.name = name;
}
Person.prototype.sayName = function() {
  console.log(this.name);
};
let person = new Person("Alice");
person.sayName(); // Alice
```

## `instanceof` 操作符

`instanceof` 操作符用于检查对象是否属于某个类或构造函数的实例。

```
console.log(person instanceof Person); // true
console.log(person instanceof Object); // true
```

# 类（ES6 引入）

ES6 引入了 `class` 关键字，提供了更简洁的语法来创建对象和处理继承。

## 类定义

```
class MyClass {
  constructor(value) {
    this.value = value;
  }
  myMethod() {
    return this.value;
  }
}
```

## 类的实例化

```
let instance = new MyClass("test");
console.log(instance.myMethod()); // test
```

## 类的继承

```
class ChildClass extends MyClass {
  constructor(value, extra) {
    super(value);
    this.extra = extra;
  }
  childMethod() {
    return `${this.value} ${this.extra}`;
  }
}
let childInstance = new ChildClass("hello", "world");
console.log(childInstance.childMethod()); // hello world
```

## 静态方法

```
class MyClass {
  static staticMethod() {
    return "static";
  }
}
console.log(MyClass.staticMethod()); // static
```

## 私有字段（`#`）

```
class MyClass {
  #privateField;
  constructor(value) {
    this.#privateField = value;
  }
  getPrivateField() {
    return this.#privateField;
  }
}
```

# 对象的使用场景

## 作为字典/映射

```
let dict = {
  key1: "value1",
  key2: "value2"
};
console.log(dict["key1"]); // value1
```

## 作为配置对象

```
let config = {
  url: "https://example.com",
  timeout: 5000
};
```

## 作为数据模型

```
let user = {
  id: 1,
  name: "Alice",
  email: "alice@example.com"
};
```

## 最佳实践

- 使用对象字面量语法（`{}`）而不是 `new Object()` 创建对象。
- 使用 `hasOwnProperty()` 检查属性是否存在于对象自身，而不是原型链上。
- 使用类语法（`class`）定义需要复用的对象结构。
- 使用属性的简洁表示法（`{key}`）和计算属性名（`{[expression]: value}`）提高代码可读性。

## 总结

对象是 JavaScript 的核心概念，用于存储键值对和构建复杂的数据结构。通过对象字面量、构造函数、`Object.create()` 和 `class` 语法，可以灵活地创建和管理对象。理解原型链是掌握 JavaScript 对象模型的关键，而现代 JavaScript 中的类语法提供了更清晰、更面向对象的方式来组织代码。


好的，这是根据您的要求翻译和格式化后的中文文档。


# 概述

在 Java 中，`Singleton` 模式是一种创建型设计模式，它确保一个类只有一个实例，并提供一个全局访问点来访问该实例。这种模式在许多场景中都非常有用，例如配置管理、日志记录、数据库连接池等。

Singleton 模式的核心思想是：将类的构造函数设为私有，以防止外部直接创建对象。然后，类内部创建一个私有的静态实例，并通过一个公共的静态方法（通常命名为 `getInstance()`）来返回该实例。

## 什么时候使用 Singleton 模式？

当系统中需要且仅需要一个全局唯一的对象来协调某些操作时，就可以考虑使用 Singleton 模式。例如，一个线程池、缓存、或者日志系统，通常只需要一个管理者来负责全局的资源调度。

Singleton 模式通过提供一个单一的访问点，可以有效地组织相关的方法和属性，并防止它们被意外地多次实例化或与其他全局变量发生冲突。这有助于减少内存消耗并提高性能，尤其是在大型或复杂的项目中。

## Singleton 模式的变体

Java 中常见的 Singleton 实现方式包括：

### 懒汉式（Lazy Initialization）
在 `getInstance()` 方法被首次调用时才创建实例。这种方式可以延迟对象的初始化，但需要注意线程安全问题。

### 饿汉式（Eager Initialization）
在类加载时就创建实例。这种方式实现简单，天然线程安全，但可能会造成资源浪费，如果该实例从未被使用的话。

### 双重检查锁定（Double-Checked Locking）
结合了懒加载和线程安全，通过两次检查实例是否为空来减少同步的开销。这是 Java 中非常常用的一种方式。

### 静态内部类（Bill Pugh Singleton）
利用 Java 的静态内部类机制，在 `getInstance()` 被调用时才加载内部类并创建实例。这种方式既实现了懒加载，又保证了线程安全，且代码简洁。

### 枚举（Enum Singleton）
这是 Joshua Bloch 在《Effective Java》中推荐的实现方式。它简洁、线程安全、且能防止反序列化和反射攻击。

## Singleton 模式的优势

- **控制实例数量**：严格限制类的实例化，确保只有一个对象存在。
- **提供全局访问点**：提供一个单一的、众所周知的访问点来获取该实例。
- **延迟实例化**：通过懒汉式或内部类等方式，可以做到在需要时才创建对象，优化资源使用。
- **减少命名空间冲突**：将相关的方法和属性组织在一个单一实例中，避免了全局变量的混乱。
- **易于测试和维护**：虽然单例本身可能难以测试，但通过良好的设计（如依赖注入）可以缓解这个问题。

## Singleton 模式的缺点

- **违反单一职责原则**：单例类同时负责自己的创建和业务逻辑，可能违反单一职责原则。
- **难以扩展**：单例类很难被继承或扩展，因为它的构造函数是私有的。
- **隐藏依赖**：使用单例的代码隐式地依赖于该单例，降低了代码的透明度和可测试性。
- **生命周期管理**：单例的生命周期由 JVM 控制，难以手动管理，可能会导致资源泄漏（如果持有外部资源）。

## 总结

Singleton 模式是一个强大且常用的设计模式，它通过限制类的实例化来确保全局唯一性。它最适合用于那些需要控制对共享资源的访问、提供全局协调点或优化性能的场景。虽然它并非适用于所有情况，但它是 Java 开发者工具箱中一个重要的工具，尤其是在大型或分布式系统中。

通过将相关的方法和属性组织在一个单一的、可重用的组件中，Singleton 模式可以帮助你写出更清晰、更易于维护的代码。



# JavaScrip 与单例模式

单例是一种创建型设计模式。它确保一个类只有一个实例，并提供一个全局访问点来访问该实例。

## 为什么需要单例模式

有些对象，比如线程池、缓存、对话框等，我们只需要一个实例。多个实例可能会导致不一致的状态或资源浪费。

虽然单例模式常用于实现全局变量，但我们应该避免过度使用，因为它会引入紧密耦合的代码，降低测试的灵活性。

## 单例模式的实现方法

在 Java 中，单例模式有多种实现方式，包括懒汉式、饿汉式、双重检查锁定、静态内部类和枚举等。

### 基本实现

最简单的单例模式如下：

- 将构造器设为私有，防止外部实例化。
- 通过一个公共的静态方法返回唯一的实例。

```
public class Singleton {
    private static Singleton instance;

    private Singleton() {}

    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```

### 懒汉式 vs 饿汉式

- **懒汉式**：在`getInstance()`第一次被调用时才创建实例。优点是延迟加载，节省资源；缺点是线程不安全。
- **饿汉式**：在类加载时就创建实例。优点是简单且线程安全；缺点是可能造成资源浪费。

```
// 饿汉式
public class Singleton {
    private static final Singleton INSTANCE = new Singleton();

    private Singleton() {}

    public static Singleton getInstance() {
        return INSTANCE;
    }
}
```

### 双重检查锁定（`Double-Checked Locking`）

这是一种优化的懒汉式实现，结合了延迟加载和线程安全。使用`volatile`关键字确保多线程环境下的可见性。

```
public class Singleton {
    private static volatile Singleton instance;

    private Singleton() {}

    public static Singleton getInstance() {
        if (instance == null) {
            synchronized (Singleton.class) {
                if (instance == null) {
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}
```

### 静态内部类（`Bill Pugh` 实现）

这种方式利用了 Java 的类加载机制，既实现了延迟加载，又保证了线程安全。

```
public class Singleton {
    private Singleton() {}

    private static class Holder {
        private static final Singleton INSTANCE = new Singleton();
    }

    public static Singleton getInstance() {
        return Holder.INSTANCE;
    }
}
```

### 枚举实现

这是《Effective Java》作者推荐的方式。它不仅可以防止反射攻击，还能保证序列化的安全性。

```
public enum Singleton {
    INSTANCE;

    public void doSomething() {
        // 业务逻辑
    }
}
```

## 总结

单例模式是一种基础但强大的设计模式。它的核心目的是确保一个类只有一个实例，并提供一个全局访问点。在 Java 中，我们可以通过枚举、静态内部类、双重检查锁定等方式来实现。

选择哪种实现方式取决于具体的需求。例如，如果不需要延迟加载，饿汉式是最简单的；如果需要延迟加载且考虑线程安全，静态内部类或双重检查锁定是不错的选择。

## 附录

### 单例模式的结构

```
┌─────────────────────────────────────┐
│             Singleton               │
├─────────────────────────────────────┤
│ - instance: Singleton               │
├─────────────────────────────────────┤
│ - Singleton()                       │
│ + getInstance(): Singleton          │
└─────────────────────────────────────┘
```

```
// 伪代码表示

function getInstance() {
    if (instance == null) {
        instance = new Singleton();
    }
    return instance;
}
```

### 单例模式的优缺点

**优点**：
- 确保一个类只有一个实例，节约资源。
- 提供全局访问点，方便控制。

**缺点**：
- 违反了单一职责原则（一个类既要管理自己的实例，又要处理业务逻辑）。
- 在多线程环境下，需要额外处理线程安全问题。
- 不利于单元测试，因为它引入了全局状态。

### 何时使用单例模式

当你需要确保某个类在整个应用中只有一个实例时，可以考虑使用单例模式。常见的应用场景包括：

- 配置管理器
- 日志记录器
- 线程池
- 数据库连接池
- 文件系统

---

*单例模式是设计模式中最基础的模式之一，但它也常被误用。建议在真正需要时才使用它，避免为了“全局访问”而滥用。*



```javascript
//??
```

看起来原文内容是一段乱码（可能使用了非标准的字符编码或加密），无法识别有效的英文或中文文本。根据您的要求，我需要处理可读的文本，但当前内容无法解析出任何有意义的单词或句子。

请检查文本来源或提供正确的原文内容。如果这是某种编码问题，请将其转换为标准 UTF-8 文本后再提供。



# 文档排版

# 概述

### 函数

函数名`funcName()`、类名`ClassName`、路径名`/path.to.xxx`、命令名`cmdname`以及它们的语句或表达式`ClassName.funcName(var1 + var2, "cmd arg0 arg1")`都需要添加反引号。

### 变量

`varName`、`funcName()`、`ClassName`、`/path.to.xxx`、`cmdname`以及它们的语句或表达式`ClassName.funcName(var1 + var2, "cmd arg0 arg1")`都需要添加反引号。

## 标题层级

### 一级标题

#### 二级标题

##### 三级标题

## 示例

### 基本用法

进入`/path/to/xxx`目录，找到`xxx.json`。

在表达式`cvar = avar + bvar`中，加法运算符（`+`）将`avar`与`bvar`相加，得到它们的和`cvar`。

在`List.of(arg0, arg1, arg2)`中，`List`接口的工厂方法`of()`接受一系列的元素，返回包含它们的只读列表。

之后我们这样调用`cmd`命令：`cmd arg0 arg1 arg2`。

```
if (condVar > someVal) {console.log("xxx")}
```

## 代码块

### 代码块示例

```
Function.prototype.method
```

```
function method
```

```
var name = newName
```

```
return this
```

## 核心内容

### 函数调用

`funcName()`、`ClassName.method()`、`namespace.funcName(var1 + var2, "cmd arg0 arg1")`都需要添加反引号。

```
function.prototype.method
```

```
var name = newName
```

```
return this
```

### 路径表示

`/path/to/xxx`、`/usr/bin/cmd`、`/home/user/file.txt`都需要添加反引号。

```
function.prototype.method
```

```
var name = newName
```

```
return this
```

## 高级用法

### 函数表达式

`funcName = function(arg0, arg1, arg2) { return arg0 + arg1 + arg2; }`

```
funcName(arg0, arg1, arg2)
```

### 类与对象

`ClassName.method(this.arg0, this.arg1)`

```
ClassName.method(arg0, arg1)
```

`NewClass()`、`new ClassName()`、`new CustomClass()`都需要添加反引号。

```
var obj = new ClassName()
```

```
var obj = new CustomClass()
```

```
var obj = new ClassName()
```

### 注意事项

### 变量命名

`varName`、`funcName()`、`ClassName`、`/path.to.xxx`、`cmdname`以及它们的语句或表达式`ClassName.funcName(var1 + var2, "cmd arg0 arg1")`都需要添加反引号。

### 代码风格

```
代码块前后加上三个反引号
```

```
function example() {
    console.log("Hello, World!")
}
```

## 总结

### 重点

- 所有单独出现的变量名（`varName`），函数名（`funcName()`），类名（`ClassName`），路径名（`/path.to.xxx`），命令名（`cmdname`）以及它们的语句或表达式（`ClassName.funcName(var1 + var2, "cmd arg0 arg1")`）都需要添加反引号。
- 如果上述东西被粗体（**）或者斜体（*）包围，去掉星号再加反引号。
- 代码块前后加上三个反引号（```）。

## 示例

```javascript
function hello(name) {
    console.log("Hello, " + name + "!")
}
```

```python
def hello(name):
    print(f"Hello, {name}!")
```

## 附录

### 使用回调函数从链表中删除数据

在某些情况下，使用回调函数（`callback`）可以简化数据处理。通过将回调函数作为参数传递给`filter()`方法，可以创建灵活的数据过滤逻辑。

例如，`Array.prototype.filter()`方法接受一个回调函数，该函数对数组中的每个元素执行测试，并返回通过测试的元素组成的新数组。

```javascript
[1, 2, 3, 4, 5].filter(function(num) {
    return num % 2 === 0;
})
```

### 使用回调函数从链表中检索数据

在某些情况下，使用回调函数（`callback`）可以简化数据检索。通过将回调函数作为参数传递给`find()`方法，可以创建灵活的数据查找逻辑。

```javascript
[1, 2, 3, 4, 5].find(function(num) {
    return num > 3;
})
```

### 自定义回调函数

如果你需要更复杂的逻辑，可以定义自己的回调函数，并将其作为参数传递给`filter()`或`find()`方法。

```javascript
function isEven(num) {
    return num % 2 === 0;
}

[1, 2, 3, 4, 5].filter(isEven)
```

