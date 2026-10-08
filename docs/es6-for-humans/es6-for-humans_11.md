# 10. 元编程

“元”这个术语描述了某种本质上自指的特性。在编程领域，元编程意味着对程序本身的编程进行编程。它可以检查或修改程序的结构，或者改变语言本身的工作方式。ES6 引入了一系列与元编程相关的重要特性，为程序的代码机制提供了底层钩子。在本章中，我们将深入探讨这些特性，并了解元编程在 JavaScript 中是如何工作的。

## ES5 与 ES6 中元编程概述

元编程的概念对 JavaScript 来说并不新鲜。你可能每天都在使用元编程，甚至没有意识到这一点。例如，检查一个对象是否是另一个对象的原型，我们使用 `a.isPrototypeOf(b)`。这就是元编程的一种形式。通常，所有 `Object.*` 方法都可以被视为元编程的一种形式。元编程提供了一种利用语言特性和功能的方法，使你能够创建更灵活的程序，并高效地处理应用程序的自定义需求。

元编程的一个关键策略是反射。它允许我们在运行时检查或修改程序的结构，或者改变内部行为（特别是元数据属性和函数）。ES6 通过三种形式的元编程提供了反射能力。

元编程有三种形式：自省、自我修改和调解。

- **自省（Introspection）** – 当代码检查自身时，例如：`a.isPrototypeOf(b)` 就是自省。`typeof`、`instanceof` 也是自省操作符，因为它们收集有关代码的信息。

- **自我修改（Self-modification）** – 当你修改程序的结构时。一个例子是 `Object.defineProperty()` 方法，它可以在对象上定义一个新属性，或者修改已有属性（如果存在的话）。

```javascript
let cake = {};
console.log(cake);      // {}
Object.defineProperty(cake, "ingredients", {
  value: ["sugar", "all things nice"],
  enumerable: true,
  configurable: true,
  writable: true
});
console.log(cake);
// { ingredients: [ 'sugar', 'all things nice' ] }
```

在上述示例中，请注意我们是如何使用 `Object.defineProperty()` 修改 `cake` 对象结构的。自修改指的是在运行时访问或创建新属性以及修改程序结构的能力。

- **中介** – 当你的代码修改了默认行为，从而影响到其余代码时，就涉及中介。中介处理的是语言语义的修改，或者在运行时向程序添加新结构。这是 ES6 中的新特性，我们将在本章后续内容中进一步了解。

ES6 通过三个新的 API 引入了元编程：`Symbol`、`Proxy` 和 `Reflect`。在本书前面的内容中，我们已经看到符号如何让我们创建唯一且不可变的标识符。它们为我们提供了一种高效的方式，为对象创建唯一且不可枚举的属性，这对于将对象的元数据与其公共接口分离非常有用。符号还能帮助我们自定义对象的默认迭代行为，本质上是通过允许我们在运行时修改程序行为或为对象创建新的（且唯一的）属性，来提供反射（自修改）能力。现在，让我们来看看 `Proxy` 和 `Reflect` 带来了什么。

## ES6 中的代理

代理代表了元编程的中介形式。代理对象允许我们拦截任何程序的行为。它们使我们能够修改对象的操作并实现自定义行为。简而言之，对象上可以执行许多操作，而代理帮助我们修改这些操作并实现所需的行为。这些操作可以是属性查找、赋值、枚举、函数调用等。

代理对象会包裹目标对象并修改其行为。我们可以通过以下方式创建代理：

```javascript
const proxy = new Proxy(target, handler);
```

其中，`target` 是其行为被修改的对象，`handler` 是一个对象，其属性是函数（称为陷阱），当对代理执行各种操作时会调用这些函数。处理程序对象允许我们定义目标对象的新行为。它使我们能够在将操作转发给目标/被包裹对象之外，执行额外的逻辑。

## 代理处理程序中的陷阱

处理程序对象提供了一系列方法，这些方法作为代理的陷阱。这些方法是可选的，如果未定义陷阱，则默认行为是将操作转发给目标对象。

最常见的陷阱是针对 getter 和 setter 的。例如，请看下面的代码片段，我们在其中为 get 属性访问器方法定义了一个陷阱：

```javascript
const restaurant = {
  soda: 2,
  burger: 1
};
const restHandler = {
  get: function(target, property) {
    if (property in target && target[property] > 0) {
      target[property] -= 1;
      return `享受您的${property}吧`;
    }
    return `抱歉，我们的${property}已经卖完了！`;
  }
};
const restProxy = new Proxy(restaurant, restHandler);
console.log(restProxy.soda);
// 享受您的 soda 吧
console.log(restProxy.soda);
// 享受您的 soda 吧
console.log(restProxy.soda);
// 抱歉，我们的 sodas 已经卖完了！
console.log(restProxy.burger);
// 享受您的 burger 吧
console.log(restProxy.burger);
// 抱歉，我们的 burgers 已经卖完了！
```

在上面的示例中，我们有一个 `restaurant` 对象，其中包含可用库存的列表和数量。我们使用代理来包裹 `restaurant` 对象，这使我们能够拦截（或“捕获”）`restaurant` 对象的原生操作，并对其执行修改后的行为。这里，`handler` 对象包含一个针对 get 属性访问器的陷阱，该陷阱在被调用时接收 `target` 和属性名。此方法会捕获所有“get 属性访问器”的调用，并检查该属性是否存在且其值是否大于 0，如果是，则将其值减 1，并返回消息 `享受您的${property}吧`；否则，返回消息 `抱歉，我们的${property}已经卖完了！`。这个“get”陷阱可用于捕获目标对象上的所有 getter 事件。类似地，你也可以拦截 setter 事件。例如，请看下面的代码片段：

```javascript
const restaurant = {
  soda: 5
};
const restHandler = {
  set: function(target, property, value) {
    target[property] = value;
    console.log(`${property} 已添加到库存中`);
  }
}
const restProxy = new Proxy(restaurant, restHandler);
restProxy.beer = 10;
// beer 已添加到库存中
```

在这个示例中，我们重写了 set 属性访问器方法，覆盖了默认的赋值操作。在进行验证时，代理可能非常有用。我们可以使用 set 处理程序轻松验证传递给对象的值。例如，假设我们在创建一个投票应用，只有年满 18 岁且拥有该国居留权的人才能投票：

```javascript
const voterValidator = {
  set: function(obj, prop, value) {
    if (prop === "age") {
      if (!Number.isInteger(value)) {
        throw new TypeError("输入的年龄不是整数");
      }
      if (value < 18) {
        throw new RangeError("输入的年龄无效");
      }
    } else if (prop === "residency") {
      if (value === false) {
        throw new Error("必须有居留权才能投票");
      }
    }
    // 存储值的默认行为
    obj[prop] = value;
    // 表示成功
    return true;
  }
};
const person = new Proxy({}, voterValidator);
person.age = 23;
person.residency = false;     // 抛出异常
person.age = "young";         // 抛出异常
person.age = 200;             // 抛出异常
```

`set` 陷阱的另一个很好的用例是数据绑定，你可以在其中设置一个回调方法，当属性被设置时会调用该方法，并且该回调方法可以对目标对象属性的更改做出响应。

除了 setter 和 getter，代理还提供了一系列你可以设置的其他陷阱。以下是一些可以帮助你更好地理解代理的陷阱。

### has

`has` 用于拦截 `in` 操作符。如果你想隐藏对象的某个特定属性，`has` 会非常有用。即使该属性存在于对象上，我们也可以返回 `false`。请看下面的示例，我们使用 `has` 方法拦截 `in` 操作符，从而隐藏 `restaurant` 对象中的 `beer` 库存。

```javascript
const restaurant = {
  soda: 5,
  beer: 10
};
const restHandler = {
  has: function(target, property) {
    if (property === "beer") {
      return false;
    }
    return property in target;
  }
}
const restProxy = new Proxy(restaurant, restHandler);
console.log("beer" in restProxy);    // false
console.log("soda" in restProxy);    // true
```

在上面的示例中，所有 `in` 操作符的调用都会被 `has` 方法捕获，如果属性名是 `"beer"`，则返回 `false`；否则保持默认行为（检查属性是否在目标对象中）。请注意，`has` 只能帮助你阻止通过 `in` 操作符检测特定属性。该属性仍然是可枚举的，并且可以通过 `for...in` 循环访问。

### ownKeys

`ownKeys` 用于拦截通过 `Object.keys()`、`Object.getOwnPropertyNames()` 或 `Object.getOwnPropertySymbols()` 访问自有属性和自有符号属性的行为。它还可以与 `has` 陷阱处理器结合使用，以增强目标对象属性的私密性（请注意，这些属性仍然不是完全私有的）：

```javascript
const restaurant = {
  soda: 5,
  beer: 10
};
const restHandler = {
  has: function(target, property) {
    if (property === "beer") {
      return false;
    }
    return property in target;
  },
  ownKeys: function(target) {
    return ["soda"];
  }
}
const restProxy = new Proxy(restaurant, restHandler);
console.log("beer" in restProxy);          // false
console.log(Object.keys(restProxy));       // ["soda"]
```

你还可以添加另一个陷阱处理器：`getOwnPropertyDescriptor`，它用于拦截 `Object.getOwnPropertyDescriptor()` 调用，以进一步增强私密性。

### apply

`apply` 用于拦截函数调用。它接受三个参数：

*   `target`：行为被修改的目标函数；
*   `context`：调用时作为 `this` 传递给目标函数的上下文；
*   `args`：调用时传递的参数。

看看下面的代码片段，我们使用 `apply` 陷阱处理器对最终账单金额应用季节性折扣：

```javascript
function getBill(amount) {
  return amount;
}
const billHandler = {
  apply: function(target, context, args) {
    console.log("Applying Discount of 35%");
    return args[0] - (args[0] * 0.35);
  }
}
const billProxy = new Proxy(getBill, billHandler);
console.log(billProxy(300));
// Applying Discount of 35%
// 195
```

由于这里的目标对象是一个函数，这些也被称为函数陷阱，你也可以为 `.call()` 和 `.bind()` 方法设置陷阱。

到目前为止，我们已经介绍了足够多的陷阱处理器，让你对整体概念有所了解。我们看到了 `Proxy` 如何帮助我们拦截设置器（setter）和获取器（getter）、装饰对象、添加验证规则、增强私密性，以及本质上修改对象的默认行为。你可以通过以下链接探索更多陷阱处理器：[`https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Meta_programming`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Meta_programming)。

## 可撤销代理（Revocable Proxy）

ES6 允许我们创建可撤销的代理。`Proxy.revocable()` 方法用于创建一个可撤销的代理对象。与代理对象类似，它也需要一个处理器和一个目标对象，并返回一个新创建的可撤销代理对象。

一个可撤销代理对象有两个属性：`proxy` 和 `revoke`。

```javascript
const { proxy, revoke } = Proxy.revocable(target, handler);
```

其中：

*   `proxy` – 通过 `new Proxy(target, handler)` 调用创建的 `Proxy` 对象。
*   `revoke` – 一个无参函数，用于使代理失效（关闭）。

如果调用了 `revoke()` 函数，代理将变得不可用，这意味着对处理器的任何陷阱都会抛出 `TypeError`。代理一旦被撤销，将保持撤销状态并可被垃圾回收。后续对 `revoke` 的调用没有进一步效果。请看下面的代码片段示例：

```javascript
const restaurant = {
  soda: 10,
  beer: 5
};
const { proxy, revoke } = Proxy.revocable(restaurant, {});
console.log(proxy.soda);   // 10
console.log(proxy.beer);   // 5
revoke();
console.log(proxy.soda);   // TypeError: Revoked
```

由于可撤销代理允许你完全切断代理及其陷阱，它可以在某些情况下用于增强对象的安全性，例如在检测到试图访问私有属性时撤销代理。我们可以修改上面的示例来增强 `beer` 属性的私密性，如下所示：

```javascript
const restaurant = {
  soda: 10,
  beer: 5
};
const resthandler = {
  get: function(target, property) {
    if (property === "beer") {
      revoke();
      return undefined;
    }
    return target[property];
  }
}
const { proxy, revoke } = Proxy.revocable(restaurant, resthandler);
console.log(proxy.soda);    // 10
console.log(proxy.beer);    // undefined
console.log(proxy.soda);    // TypeError: Revoked
```

在上面的示例中，如果访问了 `beer` 属性，我们会返回 `undefined` 并撤销代理，从而切断通过代理对目标对象的后续调用。

到目前为止，我们已经了解了 ES6 中的自修改和中介（intercession）机制。现在，让我们来看看通过内省（introspection）实现的反射。

### Reflect

`Reflect` 是一个内置的全局对象，提供了一系列内省方法。这些方法本质上是收集关于对象运行时元操作的信息。除了已有的内省方法如 `typeof`、`instanceof` 等，`Reflect` 作为一个单一对象包装器，封装了许多仅通过 JavaScript 引擎内部可访问的重要内部方法。`Reflect` 的一个简单实现如下：

```javascript
const restaurant = {
  soda: 10,
  beer: 5
};
console.log(Reflect.ownKeys(restaurant));
// ["soda", "beer"]
```

请注意，`Reflect` 是一个静态对象，因此不能与 `new` 关键字一起使用。

对于 ES6 `Proxy` 中的每一个陷阱方法，`Reflect` 中都有一个对应的反射方法。因此，`Reflect` 也适用于在代理中实现陷阱。查看下面的代码片段，我们在代理中使用了 `Reflect`：

```javascript
const restaurant = {
  soda: 2,
  beer: 5
};
const restHandler = {
  get: function(target, property) {
    if (property === "beer") {
      return undefined;
    }
    return Reflect.get(target, property);
  }
};
const restProxy = new Proxy(restaurant, restHandler);
console.log(restProxy.beer);
// undefined
console.log(restProxy.soda);
// 2
```

再次说明，在上面的示例中，我们阻止了对 `beer` 的访问，并使用 `Reflect.get()` 来访问目标对象的其他属性。你应该优先使用 `Reflect` 而不是传统的 `target[property]` 方式的原因之一是：如果在非对象目标上使用 `Reflect.get`，它会抛出错误，而 `target[property]` 则简单地返回 `undefined`。因此，在这里使用 `Reflect.get` 是一个好习惯。查看下面的代码片段示例，我们尝试在一个非对象目标上获取属性：

```javascript
console.log(Reflect.get(1, "name"));
// TypeError: Reflect.get called on non-object
console.log(1["name"]);
// undefined
```

除此之外，在 `Function.prototype` 上调用方法时，也建议使用 `Reflect`，因为如果参数列表为 `null` 或 `undefined`，`Function.prototype.apply` 会用无参方式调用函数，而 `Reflect.apply` 会抛出错误。另一个原因是 `Reflect` 提供了更简洁的语法，而 `Function.prototype` 显得过于冗长。查看下面的代码片段示例：

```javascript
function sayHello() {
  console.log(`${this.name} says hello`);
}
const person = {
  name: "Jack"
};
Function.prototype.apply.call(sayHello, person);
// Jack says hello
Reflect.apply(sayHello, person);
// TypeError: CreateListFromArrayLike called on non-object
Reflect.apply(sayHello, person, []);
// Jack says hello
```

类似地，还有许多其他有用的静态函数，其中一些与代理处理器方法同名，以下是一些常见的可用的函数：

*   `Reflect.get()` – 返回属性值的函数。
*   `Reflect.set()` – 为属性赋值的函数。如果更新成功，返回布尔值 `true`。
*   `Reflect.getPrototypeOf()` – 与 `Object.getPrototypeOf()` 相同。
*   `Reflect.has()` – 作为函数的 `in` 操作符。返回一个布尔值，指示是否存在自有或继承的属性。

# 摘要

ES6 中的元编程通过为程序代码机制提供底层钩子，使我们能够修改语言内部特性的行为。它有三种形式：内省、自我修改和中介。在本章中，我们学习了新的`Proxy`和`Reflect` API，它们帮助我们实现不同形式的元编程。我们还看到了一些有趣的示例，其中`Proxy`帮助我们管理对象中的私有属性。我们了解了`Proxy`提供的各种陷阱处理方法，以及它们一一对应的静态`Reflect`方法，并探讨了如何在应用程序中充分利用这些方法。随着未来 ECMAScript 版本的发展，我们预期将看到更多有趣的元编程特性，从而更广泛地摆弄语言的默认内部操作。
