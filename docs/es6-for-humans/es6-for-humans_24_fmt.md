# Object.entries() 和 Object.values()

`Object.entries()` 和 `Object.values()` 是 `ES2017` 中引入的两个新方法。`Object.entries()` 方法在对象上运行时，会以 `for-in` 循环提供的相同顺序返回该对象自身的可枚举属性 `[key, value]` 对。但与 `for-in` 循环不同，它不会枚举原型链上的属性。类似地，`Object.values()` 在传入对象时，会返回一个包含其自身可枚举属性值的数组：

```javascript
const myObj = { a: 1, b: 42 };
Object.entries(myObj);     // [['a', 1], ['b', 42]]
Object.values(myObj);      // [1, 42]
```

正如你在上面的示例中所看到的，当传入 `Object.entries()` 方法时，该对象会返回一个包含其 `键/值对` 的数组，而 `Object.values()` 方法则返回属性值。