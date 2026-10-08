# 继承与原型链

JavaScript 使用原型链实现对象间的继承。每个对象都有一个内部链接到另一个对象（其原型），该对象具有一组属性和方法。当试图访问一个对象的属性时，不仅会在该对象上查找，还会在它的原型上查找，以此类推，直到找到该属性或到达原型链末尾。

## 原型链示例：使用 `Object.create()`

`Object.create()` 方法使用现有对象作为新创建对象的原型，创建一个新对象。

```javascript
let parent = {name: "parent"};
let child = Object.create(parent);
console.log(child.name); // parent
```

## 原型链的终点是 `null`

```javascript
let obj = {};
console.log(Object.getPrototypeOf(Object.getPrototypeOf(obj))); // null
```

## 使用构造函数创建对象

```javascript
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

```javascript
console.log(person instanceof Person); // true
console.log(person instanceof Object); // true
```
