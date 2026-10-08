# 类（ES6 引入）

ES6 引入了 `class` 关键字，提供了更简洁的语法来创建对象和处理继承。

## 类定义

```javascript
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

```javascript
let instance = new MyClass("test");
console.log(instance.myMethod()); // test
```

## 类的继承

```javascript
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

```javascript
class MyClass {
  static staticMethod() {
    return "static";
  }
}
console.log(MyClass.staticMethod()); // static
```

## 私有字段（`#`）

```javascript
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
