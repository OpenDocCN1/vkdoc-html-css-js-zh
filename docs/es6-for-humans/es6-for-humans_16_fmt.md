# ES6 中的 Symbol

早在 1996 年，也就是 JavaScript 首次被标准化、ECMA 试图制定出标准规范的时候，JavaScript 就已经有五种原始数据类型：布尔型、数字型、字符串型、未定义型和空值型，每种类型都代表一个值。但现在，ES6 引入了一种全新的原始数据类型——Symbol。在本章中，我们将详细讲解这种新的原始类型及其应用。

## Symbols

Symbol 表示一个唯一的值，其本质是一个保证永远不会与其他 Symbol 冲突的唯一令牌。ES6 中的 Symbol 可以通过一个工厂函数来创建。`Symbol()` 方法可用于创建一个新的 Symbol。考虑以下示例：

```javascript
const foo = Symbol();
console.log(typeof foo);      // "symbol"
```

每次调用工厂函数，都会创建一个新的、唯一的 Symbol。对 Symbol 类型使用 `typeof` 运算符，其输出是 `"symbol"`，这是识别 Symbol 的主要方式。

**注意：** ES6 中的 Symbol 不像其他原始类型那样具有字面量形式。

（可选地）在创建 Symbol 时，你也可以通过向 `Symbol()` 方法传递一个字符串作为参数来为其添加标签：

```javascript
const chocolate = Symbol("这是我的巧克力");
console.log(chocolate);
// Symbol(这是我的巧克力)
console.log(chocolate.toString());
// "Symbol(这是我的巧克力)"
```

作为参数传入的标签不会影响 Symbol 的值。除了在打印 Symbol 时用作描述该 Symbol 的字符串之外，它没有其他作用。当你对返回的 Symbol 调用 `.toString()` 方法（该方法将 Symbol 转换为字符串）时，它也非常有用。当你将 Symbol 写入 `console.log()` 以及可能的错误消息时，该标签会显示出来。这对于调试目的非常有帮助。

请注意，你不应该使用 `new` 关键字来创建 Symbol。Symbol 没有对象构造函数；因此你不能使用 `new` 关键字创建 Symbol：

```javascript
const bar = new Symbol();           // 类型错误
```

如前所述，Symbol 永远是唯一的，两个 Symbol 永远不可能相同。但是，你可以使用相同的标签创建多个 Symbol，但返回的 Symbol 始终是唯一的。查看以下代码片段示例：

```javascript
const kit = Symbol("你好");
const kat = Symbol("你好");
console.log(kit === kat);       // false
```

或者你可以简单地尝试：

```javascript
Symbol() !== Symbol()         // true
```

因此，Symbol 永远是唯一的，并且在各种情况下都有其不可思议的用途。让我们在下一节中看看它们的应用。

### Symbol 的应用

从本质上讲，Symbol 主要用作唯一的属性键，因为一个 Symbol 永远不会与对象的任何其他属性（Symbol 或字符串）发生冲突，这在你不希望冒险覆盖对象的原生属性时非常有用。Symbol 也常用于为类或对象类型的内部使用创建匿名（或在某种程度上是私有的）属性。考虑以下示例，其中我们有一个名为 `Person` 的对象，它包含一些“公共”属性：

```javascript
let Person = {
    name: "John",
    age: 14,
    location: "New York"
}
```

现在，假设我们想向 `Person` 对象添加一个私有方法。我们可以像下面这样使用 Symbol 来实现：

```javascript
let votingEligibility = Symbol();
Person[votingEligibility] = function() {
    return this.age >= 18;
}
```

现在让我们看看 `Person` 对象，你会发现一个匿名方法已经被添加到其中：

```javascript
console.log(Person);
// {name: "John", age: 14, location: "New York"}
// 如果你在较新版本的 Google Chrome 中运行此代码，
// 你还会看到 - Symbol(): [Function] 属性
```

你也可以在这里使用计算属性键，我们已经在之前的章节中讨论过这一点：

```javascript
let Person = {
    name: "John",
    age: 14,
    location: "New York",
    [Symbol()]: function() {
        return this.age >= 18;
    }
}
```

Symbol 属性是不可枚举且匿名的。因此，在使用 `for...in` 循环时，遍历过程中无法访问对象的 Symbol 属性；并且由于它是匿名的，它也不会被列在 `Object.getOwnPropertyNames()` 方法的结果数组中。但是，你可以使用 `Object.getOwnPropertySymbols()` 来获取 Symbol 属性。例如：

```javascript
Object.getOwnPropertyNames(Person);
// ["name", "age", "location"]
Object.getOwnPropertySymbols(Person);
// [Symbol()]
```

让我们看看在上一个创建 `Person` 对象的示例中如何使用 `Object.getOwnPropertySymbols()`：

```javascript
let canVote = Object.getOwnPropertySymbols(Person)[0];
Person[canVote]();
// false
```

**注意：** 我们看到 `Object.getOwnPropertySymbols()` 会返回一个与对象关联的属性 Symbol 数组，因此这些属性是可访问的，而不是私有的。但是通过使用 Symbol，我们确实拥有不会随意发现这些属性的优势。

你也可以通过使用创建的引用来将属性绑定到 `Person` 对象来实现这一点。还记得我们是使用 `let votingEligibility = Symbol();` 来创建 Symbol 的吗？因此：

```javascript
Person[votingEligibility]();
// false
```

同样，自定义类也可以通过这种方式创建私有成员，使其仅能被类中定义的其他方法私有访问。动态创建的符号值可以保存到作用域内的类变量中，该类变量只能被类的内部方法访问，从而保护私有属性不被无意或随意发现。例如，请看以下代码片段：

```javascript
const age = Symbol("age");
class Person {
    constructor(value) {
        this[age] = value;
    }
    getAge() {
        console.log(this[age]);
    }
}
const jack = new Person(23);
console.log(jack);
// Person {}
console.log(jack.age);
// undefined
console.log(jack[Symbol("age")]);
// undefined
jack.getAge();
// 23
```

在这个例子中，我们有一个 `Person` 类，它使用符号创建了一个私有属性来存储人的年龄。从代码片段中可以看出，该属性是不可枚举的，因为它返回一个空对象，并且只能通过公开的 `getAge()` 方法访问。由于符号总是唯一的，通过创建具有相同标签 `age` 的符号 `Symbol("age")` 来访问私有属性，只会创建一个新的唯一符号，而无法访问对象的所需属性。

### 符号与注册表

到目前为止，我们已经讨论了局部符号以及如何通过获取引用访问它们。符号也可以放在一个全局注册表中，通过它可以在不同的上下文（称为领域）中访问。领域是代码片段存在的上下文，例如应用程序运行的页面、页面中的 `<iframe>`，或包含全局变量和已加载模块的 web worker 上下文。

符号拥有一项特殊能力，可以存在于整个运行时符号注册表中，你可以使用以下方法将符号添加到运行时符号注册表中：

```
Symbol.for(key) 和 Symbol.keyFor(symbol)。
```

## Symbol.for(key)

此方法在运行时符号注册表中查找具有给定键的现有符号。如果在全局注册表中存在具有该键的符号，则返回该符号。如果在注册表中未找到具有该键的符号，则会创建一个新符号。例如，考虑以下代码片段，其中第一次调用 `Symbol.for('myCar')` 创建一个符号，将其添加到注册表并返回；第二次调用则返回同一个符号，因为该键已存在于运行时符号注册表中：

```
Symbol.for('myCar') === Symbol.for('myCar')
// true
```

运行时全局注册表会跟踪所有使用 `Symbol.for(key)` 方法创建的符号，这些符号可以在不同的领域之间访问。例如，在网页中任何地方创建的符号也可以在 iframe 的上下文中访问。查看以下代码片段（你可以在浏览器控制台中运行）：

```
// 创建一个 iframe
const frame = document.createElement('iframe');
// 将 iframe 附加到 body
document.body.appendChild(frame);
console.log(Symbol.for('temperature') === frame.contentWindow.Symbol.for('temperature'));
// true
```

在上面的示例中，来自网页上下文的全局符号与 iframe 中的全局符号完全相同，这表明使用 `Symbol.for(key)` 方法创建的符号可以在不同领域之间访问。

## Symbol.keyFor(symbol)

`Symbol.keyFor(symbol)` 从全局符号注册表中检索与给定符号关联的键，该键是在将符号添加到注册表时关联的。如果在注册表中未找到该符号，则返回 `undefined`。

```
const symbol = Symbol.for('myHouse');
console.log(Symbol.keyFor(symbol)); // myHouse
const myCat = Symbol();
Symbol.keyFor(myCat); // undefined
```

### 内置符号

ES6 附带了一系列语言规范中预定义的符号，其中大部分暴露了 JavaScript 对象的一些元属性和行为。一些众所周知的符号如下：

`Symbol.hasInstance` - 允许我们确定对象是否是构造函数的实例。它由 `instanceof` 运算符在内部调用。

另一个是 `Symbol.iterator`，我们将在本书后面深入探讨。它保存对象的默认迭代器。数组、集合（`Set`）、映射（`Map`）和弱映射（`WeakMap`）在其结构中都有一个不可枚举的 `[Symbol.iterator]` 属性。

## 总结

符号是 ES6 中引入 JavaScript 的一种新的原始类型。当你想避免属性键的名称冲突时，它们非常有用，特别是在你不想冒险覆盖对象的原生属性的情况下。符号因其不可枚举性，通过创建受保护的属性提供优势，防止这些属性被轻易发现，因为你必须专门查找符号才能找到它们。