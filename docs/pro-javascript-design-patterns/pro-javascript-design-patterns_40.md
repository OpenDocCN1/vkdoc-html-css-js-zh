# 经典继承性示例：`Person` 类

## 示例：定义一个 `Person` 类

```java
class Person {
    String name;
    public String getName() {
        return this.name;
    }
}

class Student extends Person {
    // 继承自 Person
}
```

## 示例：使用 `new` 关键字创建实例并调用方法

```java
Person person = new Person();
person.getName(); // 调用 Person 类的 getName 方法
```

```


# 继承与多态

**Java** 是一种面向对象的编程语言，其核心特性之一就是支持继承与多态。

### 继承

继承允许一个类（子类）从另一个类（父类）继承字段和方法。通过关键字 `extends` 实现。

在最常见的面向对象语言中，**单继承**是主流。也就是说，一个子类只能直接继承自一个父类。这与 `C++` 中的多继承不同。

## 原型链

在 `JavaScript` 中，继承是通过原型链（`prototype chain`）实现的。

### 原型链的查找机制

当访问一个对象的属性时，`JavaScript` 首先在该对象自身查找。如果没有找到，它会沿着原型链向上查找，直到找到该属性或到达链的顶端（`null`）。

### 原型链的构成

每个对象都有一个 `__proto__` 属性（内部属性 `[[Prototype]]`），指向其构造函数的 `prototype` 对象。而 `prototype` 对象本身也是一个对象，因此它也有自己的 `__proto__`，以此类推，形成一条链。

### 原型链的终点

所有原型链的终点都是 `Object.prototype`，而 `Object.prototype.__proto__` 是 `null`。

## 构造函数与原型

在 `JavaScript` 中，构造函数（`constructor`）与原型（`prototype`）共同实现了类的创建与继承。

### 构造函数的职责

构造函数用于创建对象实例，并初始化实例自身的属性。

### 原型的职责

原型对象用于存储所有实例共享的方法和属性。通过将方法定义在构造函数的 `prototype` 属性上，可以避免在每个实例中重复创建相同的方法，从而节省内存。

## class 关键字

ES6 引入了 `class` 关键字，提供了一种更清晰、更接近传统面向对象语言的语法来创建类和继承。

### 定义类

```javascript
class Person {
    constructor(name) {
        this.name = name;
    }
    sayHello() {
        console.log(`Hello, I'm ${this.name}`);
    }
}
```

### 继承

`class` 通过 `extends` 关键字实现继承：

```javascript
class Student extends Person {
    constructor(name, grade) {
        super(name); // 调用父类构造函数
        this.grade = grade;
    }
    study() {
        console.log(`${this.name} is studying in grade ${this.grade}`);
    }
}
```

`super` 关键字用于调用父类的构造函数，确保父类的初始化逻辑被执行。

## 多态

多态意味着同一个方法在不同类中可以有不同行为。在 `JavaScript` 中，由于动态类型系统的存在，多态通常是隐式的。

### 方法重写

子类可以重写父类的方法，从而提供自己的实现：

```javascript
class Animal {
    speak() {
        console.log("Animal speaks");
    }
}

class Dog extends Animal {
    speak() {
        console.log("Woof!");
    }
}

const animals = [new Animal(), new Dog()];
animals.forEach(animal => animal.speak());
// 输出：
// Animal speaks
// Woof!
```

### 接口与抽象类

`JavaScript` 没有传统的接口或抽象类概念，但可以通过约定或 `TypeScript` 等工具模拟。

## 总结

继承与多态是面向对象编程的基石。`JavaScript` 通过原型链实现了继承，而 `class` 语法提供了更简洁的写法。理解原型链的机制对于深入掌握 `JavaScript` 至关重要。

---

**附录：相关主题**

- `extends` 关键字
- `new` 操作符
- `prototype` 属性
- `__proto__` 属性
- `Object.create()`
- `Object.setPrototypeOf()`
- `Object.getPrototypeOf()`
- `instanceof` 操作符

---

## 示例：实现继承

下面是一个完整的示例，展示如何通过 `class` 实现继承：

```javascript
// 父类
class Animal {
    constructor(name) {
        this.name = name;
    }
    speak() {
        console.log(`${this.name} makes a sound.`);
    }
}

// 子类
class Dog extends Animal {
    constructor(name, breed) {
        super(name);
        this.breed = breed;
    }
    speak() {
        console.log(`${this.name} barks.`);
    }
}

const dog = new Dog("Buddy", "Golden Retriever");
dog.speak(); // Buddy barks.
```

## 使用原型链实现继承

在 `class` 语法出现之前，`JavaScript` 通过原型链实现继承：

```javascript
function Animal(name) {
    this.name = name;
}
Animal.prototype.speak = function() {
    console.log(`${this.name} makes a sound.`);
};

function Dog(name, breed) {
    Animal.call(this, name);
    this.breed = breed;
}
Dog.prototype = Object.create(Animal.prototype);
Dog.prototype.constructor = Dog;
Dog.prototype.speak = function() {
    console.log(`${this.name} barks.`);
};

const dog = new Dog("Buddy", "Golden Retriever");
dog.speak(); // Buddy barks.
```

这种方式较为繁琐，且容易出错，因此 `class` 语法成为更推荐的选择。

## 总结

| 特性 | 描述 |
|------|------|
| 继承 | 允许子类复用父类的属性和方法 |
| 原型链 | `JavaScript` 实现继承的机制 |
| `class` | ES6 提供的语法糖，简化了类的定义和继承 |
| 多态 | 同一方法在不同对象上表现出不同行为 |

理解继承与多态是掌握面向对象编程的关键，而 `JavaScript` 的原型机制使其具有独特的灵活性。


```markdown

