# Object.getOwnPropertyDescriptors()

顾名思义，`Object.getOwnPropertyDescriptors()` 是 ES2017 中的一个新方法，它允许你精确检查给定对象所有自身属性的描述。JavaScript 对象中的属性通常由一个字符串名称和一个属性描述符组成。描述符是属性值、布尔值 `writable`、`get` 函数、`set` 函数、布尔值 `configurable` 和布尔值 `enumerable` 的记录。思考以下示例：

```javascript
const myObj = {
[Symbol('mySymbol')]: 42,
get random() { return 'test' },
};
console.log(Object.getOwnPropertyDescriptors(myObj));
// Object {random: Object, Symbol(mySymbol): Object}
// {random:{
//     configurable: true,
//     enumerable: true,
//     get: function random(),
//     set: undefined}
//[Symbol('mySymbol')]:{
//     configurable: true,
//     enumerable: true,
//     value: 42,
//     writable: true}
//}
```

在像 `myObj` 这样的对象上使用 `Object.getOwnPropertyDescriptors()` 的输出将大致如上所示。请注意，上述所有特性都相当新且是实验性的，因此你可能不总是能得到完全相同的结果。话虽如此，它们现在正变得越来越标准化。

## 函数参数列表与调用中的尾逗号

ES2017 中的这个特性是一个简单的语法更新。在引入此特性之前，即使规范的其他部分（如数组和对象字面量的情况）中已普遍允许，但在 JavaScript 中是不允许在最后一个函数参数后面有逗号的。通过 ES2017 的这一更新，你在整个 JavaScript 中拥有了更统一的尾逗号语法。所以你可以这样做：

```javascript
const trailFunct = function(x, y, z,) {
// 函数体
}
```

这纯粹是语言的一个语法更新，对该语言的行为或功能没有重大改变。