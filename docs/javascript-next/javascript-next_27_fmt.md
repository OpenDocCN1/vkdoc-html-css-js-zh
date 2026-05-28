# 13. 利用 Proxy 和 Reflect 实现变形

没有任何 API 能够或应该试图做到完美。根据具体用例和问题领域，我们常常发现可能需要为对象或类型添加、移除甚至约束行为。过去，我们的做法是利用 JavaScript 的动态特性，简单地将我们想要的行为附加到任何对象上。符号（Symbol）在这方面可以帮上忙，它保证了键不会发生冲突。然而，在许多情况下，这感觉有点粗糙，尤其是当我们不希望*永久*修改对象在应用程序生命周期内的行为时。

ES6 为我们提供了代理（Proxy），它本质上是在语言层面实现了代理模式。在本章中，我们将了解如何在不实际修改对象本身的情况下改变新对象和现有对象的行为，同时还能让我们在需要时明确选择启用或禁用这些行为。

## 猴子补丁的风险

JavaScript 是一种动态类型语言，这意味着它不需要我们预先知道或声明变量或对象的类型。这种动态特性可以扩展到对象的结构，即对于给定的对象，我们可以修改其属性（和方法）来满足我们的需求。JavaScript 的这种灵活性，以及该语言固有的其他特性（如原型继承和一等函数），已被证明是 JavaScript 的一大卖点，允许开发者构建强大而优雅的解决方案，例如面向切面编程设施^(⁸⁴)以及使用 Sinon 的模拟（mock）和间谍（spy）进行行为驱动测试^(⁸⁵)，但同时也存在被滥用的风险^(⁸⁶)。

一种特定的（滥）用场景是为现有类型和对象添加行为。设想一个场景：每当我们获取或设置对象的属性时，我们希望自动执行一个副作用，比如日志记录。我们并不希望到处都这样做，因为它可能过于冗长、开销过大，或者两者兼而有之。我们可以利用 JavaScript 的动态特性，修改代码库中特定部分的行为，之后再小心地将其恢复为原始行为，如下所示：

*   ① 定义我们的拦截函数

*   ② 确保我们只拦截提供的对象的方法

*   ③ 获取该方法的引用

*   ④ 首先调用拦截函数

*   ⑤ 然后委托给原始方法

*   ⑥ 定义我们的“记录器”函数

*   ⑦ 定义一个普通的 JavaScript 对象

*   ⑧ 确保在调用对象方法之前修改它

```javascript
const intercept = (obj, interceptionFn) => { ①
for (const m in obj) {
if ((Object.prototype.hasOwnProperty.call(obj, m)) && (obj[m] instanceof Function)) { ②
const method = obj[m]; ③
obj[m] = function(...args) {
interceptionFn.call(obj, m); ④
return method.apply(obj, args); ⑤
};
}
}
};
const logger = m => console.log(`${m} was called`); ⑥
const toBeIntercepted = { ⑦
name: 'Jonathan',
getName() {
return this.name;
},
setName(name) {
this.name = name;
},
};
intercept(toBeIntercepted, logger); ⑧
// 打印 'getName was called' 后跟 'Jonathan'
console.log(toBeIntercepted.getName());
// 打印 'setName was called'
toBeIntercepted.setName('Johnson');
// 打印 'getName was called' 后跟 'Johnson'
console.log(toBeIntercepted.getName());
```

我们首先编写了一个相当通用的方法拦截函数，它接受两个参数——我们想要修改其行为的对象，以及当该对象的任何方法被调用时要调用的函数。然后我们遍历提供的对象的所有属性，确保我们**不**爬升继承链，并且只修改那些恰好是方法的属性。最后，我们定义“记录器”函数和一个对象，并确保装饰该对象的方法，以便每次调用时都调用我们的记录器函数。

在这种情况下，我们针对某些功能（即任何方法调用）修改了现有对象的行为。也就是说，我们的实现使得关闭新引入的行为变得困难。为了将行为恢复为原始行为，我们必须存储我们修改过的方法的原始实现，然后重新安装它们。

当然，我们冒着忘记关闭行为（假设我们可以关闭）的风险，并承受后果，包括将意料之外的行为泄漏到其他地方，以及用意想不到的副作用让其他不知情的客户端感到惊讶。这种修改现有行为的技术被 Sinon 等测试库非常有效地用于创建间谍（spy）和模拟（mock），并且展示了一个理想用例——测试限制了新行为可见的范围。

这个论点可以扩展到那些我们并未创建、因此也不“拥有”的对象。考虑数组的使用。如果我们想让数组能够进行比较，应该怎么做？它们会比较各自的单个元素，如果元素本身是嵌套数组，则递归比较。以下是一种实现方式：

* ① 使用 `Object.defineProperty` 添加一个 `equals` 方法

* ② 瞧！现在每个数组都可以使用该方法了

```javascript
Object.defineProperty(Array.prototype, 'equals', { ①
value(other) {
if (!other) return false;
if (!Array.isArray(other)) return false;
if (this.length !== other.length) return false;
const [fMe, ...rMe] = this;
const [fOther, ...rOther] = other;
if (fMe === fOther) {
return true;
}
if ((rMe.length === 0) && (rOther.length === 0)) {
return true;
}
return rMe.equals(rOther);
},
enumerable: false,
configurable: true,
});
const first = [1, 2, 3, [4, 5]];
const second = [1, 2, 3, [4, 5]];
const third = [1, 2, 3];
// 输出 'true'
console.log(first.equals(second)); ②
// 输出 'false'
console.log(first.equals(third));
```

在这个示例中，我们使用 `Object.defineProperty` 在 `Array` 的原型上定义了一个 `equals` 方法。我们也可以直接使用 `Array.prototype.equals = function() { … }` 来添加该属性；然而，使用 `Object.defineProperty` 让我们有机会配置这个新方法的可见性——将 `enumerable` 设置为 `false` 会隐藏该属性，使其不出现在 `for-in` 循环中。同时，我们将它的 `configurable` 属性设置为 `true`，这样如果我们愿意，就可以删除该属性。

请注意，在这里我们是在增强 `Array`，而不是修改数组现有方法的行为。

在 Ruby 和 Python 这类语言中，在运行时对现有类型或对象进行的任何修改，都被称为“猴子补丁”。^(⁸⁷) 这种做法可能非常有用——例如 Ruby on Rails^(⁸⁸) 就利用了这一技术并取得了巨大优势；然而，如果执行或文档记录不当，它可能会造成极大的混乱，甚至是有害的。

除了修改和增强行为之外，我们还可能希望实现其他一些增强或限制——比如为未定义的属性返回合理的默认值、动态添加方法、限制对象的构造以便使用单例模式，等等。理想情况下，我们应当能够以一种不引人注目的方式修改行为，**并且**修改行为应该是带命名空间的，或者隔离到我们应用程序的特定部分，在这些部分中，我们特意请求修改后的行为，同时知道我们没有对原始对象的行为产生任何影响。为此，ES6 引入了 `Proxy` 和 `Reflect`。虽然它们各自提供了大量的功能，但两者结合却能组成一支非凡的“梦之队”。

## 使用 Proxy 和 Reflect 进行元编程

正如其名，ES6 的 `Proxy` 类允许我们将代理模式^(⁸⁹)作为语言级别的构造来实现。它允许我们创建一个代理，包裹住我们想要增强或修改的对象。然而，由于它拥有语言级别的支持，我们可以拦截或代理基本操作，比如获取/设置/添加/删除属性，以及对象构造函数和函数调用。换句话说，`Proxy` 让我们能够挂钩到 JavaScript 自身的内部机制中。

干预并由此修改行为的机制通过使用一个包含“陷阱”的“handler”对象来实现。如果我们没有为某个特定功能定义陷阱，代理会将会话正确地转发给原始对象。

让我们从一个关于代理和 handler 的简单示例开始，然后我们将深入探讨所有使 ES6 中的代理如此强大的活动部件。

* ① 我们希望增强的一个对象

* ② 构造一个代理

# 代理与元编程

## 为代理提供一个 handler

## 定义“get”陷阱

## 使用代理

```javascript
const toBeIntercepted = { ①
name: 'Jonathan',
getName() {
return this.name;
},
setName(name) {
this.name = name;
},
};
const proxied = new Proxy(toBeIntercepted, ②
{ ③
get(target, property) { ④
console.log(`${property} was called`);
return target[property];
},
},
);
// 输出 'name was called'，接着输出 'Jonathan'
console.log(proxied.name); ⑤
// 输出 'getName was called'，接着输出 'Jonathan'
console.log(proxied.getName());
```

我们使用 `Proxy` 构造函数在一个对象周围构造一个代理，该构造函数接收两个参数——我们希望代理的对象，以及一个定义了陷阱的 `handler` 对象。陷阱是具有非常特定^(⁹⁰)名称和签名的函数，每个函数针对 JavaScript 内部机制中的一个特定部分。代理会调用合适的陷阱，并向其传递原始对象的引用、被查找的属性名称，以及代理对象本身的引用。

请注意，代理并不会尝试修改原始对象！相反，它为我们提供了一个新的对象，该对象附加了新的行为——然而，我们仍然可以持有对原始对象的引用，如果我们愿意，可以恢复使用它。

我们最初的尝试可能相当简单，但我们可以看到如何使用代理在 JavaScript 中实现一些极具颠覆性的元编程魔法。让我们扩展我们的代理实现，以重现拦截对象上所有方法调用的功能：

* ① 定义一个包装函数来创建代理
* ② 我们的包含 `get` 陷阱的 `handler`
* ③ 确保我们只拦截提供的对象的方法
* ④ 先调用拦截函数
* ⑤ 然后委托给原始方法
* ⑥ 返回代理
* ⑦ 定义我们的“logger”函数
* ⑧ 定义一个普通 JavaScript 对象
* ⑨ 务必将我们的对象用代理包裹起来，并继续使用该代理

```javascript
const intercept = (obj, interceptionFn) => { ①
const handler = { ②
get(target, property) {
if ((Reflect.has(target, property))
&& (target[property] instanceof Function)) { ③
interceptionFn.call(target, property); ④
}
return Reflect.get(target, property); ⑤
},
};
return new Proxy(obj, handler); ⑥
};
const logger = m => console.log(`${m} was called`); ⑦
const toBeIntercepted = { ⑧
name: 'Jonathan',
getName() {
return this.name;
},
setName(name) {
this.name = name;
},
};
const proxied = intercept(toBeIntercepted, logger); ⑨
// 输出 'getName was called'，接着输出 'Jonathan'
console.log(proxied.getName());
// 输出 'setName was called'
proxied.setName('Johnson');
// 输出 'getName was called'，接着输出 'Johnson'
console.log(proxied.getName());
```

好的，遵照您的指示，以下是翻译后的中文版本：

我们的尝试与我们之前拦截对象方法调用的尝试并无不同，只是使用了代理。然而，我们获得了许多好处——我们不再改变对象；相反，代理使我们能够精确地实现我们想要的目标，只需切换引用即可恢复到原始行为。由于代理本身就是一个引用，它可以被限定在函数或模块的作用域内。离开这个作用域，外界就永远不会知道代理的存在！

我们在这里巧妙地引入了 `Reflect` API，所以你可能会疑惑——这到底是干什么用的？`Reflect` 是一个与 `Proxy` 一同在 ES6 中出现的新 API，它作为代理处理器的镜像 API。因此，对于我们能安装在代理处理器上的每一种陷阱，`Reflect` 上都有一个对应的静态属性。每当我们在处理器上定义一个陷阱，并且希望将同一调用转发给原始对象时，我们可以简单地使用镜像的 `Reflect` API，传入目标对象及其可能需要的任何参数。在我们的示例中，我们正在捕获 `get`——然而，在调用我们的拦截函数之后，我们希望将调用转发给原始对象，因此我们可以直接使用 `Reflect.get`。

我们已经初步了解了如何使用代理和 `Reflect` API。现在，我们将尝试重新创建之前的示例，即在不直接修改 `Array.prototype` 的情况下定义一个 `equals` 方法。回想一下，在这个例子中，我们试图为数组增加一个新方法，即 `equals`。我们将从一个递归函数开始，该函数可以将一个数组（通过 `this` 引用）与另一个数组进行比较：

```javascript
const equals = function (other) {
if (!other) return false;
if (!Array.isArray(other)) return false;
if (this.length !== other.length) return false;
const [fMe, ...rMe] = this;
const [fOther, ...rOther] = other;
if (fMe === fOther) {
return true;
}
if ((rMe.length === 0) && (rOther.length === 0)) {
return true;
}
return rMe.equals(rOther);
};
```

接下来，我们将编写我们的处理器，它将拦截所有对 `equals` 的调用，并将调用分派给我们新创建的 `equals` 方法：

* ① 定义我们的代理创建助手
* ② 定义我们的处理器
* ③ 捕获 `get`
* ④ 增强对 `equals` 方法的所有调用
* ⑤ 否则委托给原始对象

```javascript
const comparableArray = (arr) => { ①
const handler = { ②
get(target, property) { ③
if (property === 'equals') { ④
return equals;
}
return Reflect.get(target, property); ⑤
},
};
return new Proxy(arr, handler);
};
const first = comparableArray([1, 2, 3, [4, 5]]);
const second = comparableArray([1, 2, 3, [4, 5]]);
const third = comparableArray([1, 2, 3]);
// 打印 'true'
console.log(first.equals([1, 2, 3, [4, 5]])); ②
// 打印 'false'
console.log(first.equals(third));
```

我们再次捕获 `get`，不过这次，如果正在寻找的属性恰好是 `equals`，我们就返回自定义实现；否则，只需将调用转发给原始对象。尽管我们的 `equals` 实现与我们之前的尝试没有任何不同，但我们的方法却截然不同。我们通过代理获得了新的行为，而无需直接修改 `Array` 本身。

这种增强仅对使用代理的用户可见。此外，由于代理在获取 `equals` 方法之外的任何操作都会委托给原始对象，因此对于代理的任何其他使用者来说，这种修改是完全透明的。

## 强制实现单例

现在，我们或许可以看到如何实现许多巧妙的功能，比如添加方法或修改现有方法。让我们考虑另一个用例，即使用代理来构建单例模式。为了实现这一点，我们需要捕获对象的构造过程，确保一旦我们有了一个实例，就始终返回该实例。查看 MDN 文档，我们找到了 `handler.construct`——完美！这正是我们需要的。

# 使用代理实现单例模式

```javascript
const singletonFactory = (constructorFn) => { ①
let instance; ②
const handler = {
construct(target, args) { ③
if (!instance) {
instance = Reflect.construct(constructorFn, args); ④
}
return instance; ⑤
},
};
return new Proxy(constructorFn, handler);
};
class SuperHeroService { ⑥
constructor() {
this.heroes = [
'IronMan',
'Captain America',
'Wasp',
'Black Widow',
];
}
getHeroes() {
return this.heroes;
}
addHero(hero) {
this.heroes.push(hero);
}
}
const SingletonSuperHeroService = singletonFactory(SuperHeroService); ⑦
const service1 = new SingletonSuperHeroService();
const service2 = new SingletonSuperHeroService(); ⑧
// 打印 'true'
console.log(service1 === service2); ⑨
```

*   ① 定义我们的代理助手
*   ② 一个用于缓存实例的局部变量
*   ③ 捕获构造函数调用
*   ④ 如果没有实例，则调用提供的构造函数
*   ⑤ 否则返回我们缓存的实例
*   ⑥ 我们希望视为单例的类
*   ⑦ 创建代理
*   ⑧ 对代理的多次调用返回同一个实例
*   ⑨ 确保实例相同

我们的代理创建函数期望将我们希望设为单例的对象的构造函数作为其唯一参数。我们的实现，除了捕获`construct`之外，相当简单——我们在创建单例时保存其局部引用，并在后续调用构造函数时直接返回相同的引用。正如我们所见，我们可以用这个简单的实现包装任何希望视为单例的构造函数。对代理构造函数的多次调用只会返回同一个实例。

## 构建领域特定语言

最后，让我们看看如何使用代理构建一个简单的领域特定语言（DSL）。我们将尝试处理一个相当流行的领域，即构建 XML 标记。不过，我们需要走一条捷径；我们的 DSL 只会生成必要的 JSON，以供诸如 FreeFormatter.com 的“JSON 到 XML 转换器”之类的工具使用。^(⁹⁴) 这让我们能够专注于将代理应用于此类场景。

该工具（默认情况下）假定元素的所有属性都以 `@` 为前缀，元素的文本则以 `#` 为前缀。因此，给定以下 JSON 片段：

```json
{
  "books": {
    "@count": "1",
    "book": {
      "@id": "1",
      "title": {
        "@isbn": "1590592395",
        "#text": "Practical Common Lisp"
      }
    }
  }
}
```

该工具将生成对应的 XML：

```xml
<books count="1">
  <book id="1">
    <title isbn="1590592395">Practical Common Lisp</title>
  </book>
</books>
```

这就是我们的 DSL^(⁹⁵) 的样子：

```javascript
const books = jsonToXmlBuilder()
.books({ count: '1' })
.book({ id: '1' })
.title({ isbn: '1590592395' }, 'Practical Common Lisp')
.up()
.up()
.end();
console.log(books);
// 输出
// {
//   "books": {
//     "@count": "1",
//       "book": {
//       "@id": "1",
//         "title": {
//         "@isbn": "1590592395",
//         "#text": "Practical Common Lisp"
//       }
//     }
//   }
// }
```

我们的 DSL 具有以下特点：

- 元素被表达为方法，并且可以嵌套。
- 元素可以附带一个代表其属性的对象，以及元素的文本。
- `up` 方法让我们返回到父节点，这样我们就可以根据需要向同一个父节点添加额外的子元素。
- `end` 方法标志着链式调用的结束，并生成所需的 JSON。

兴奋吗？让我们开始吧！首先，我们将编写一个单参数辅助函数来构建一个元素。这个函数依次返回一个函数，该函数期望接收元素的属性和文本，并简单地构造必要的 JSON 来表示该元素。这个函数 `return` 代理对象本身，一旦我们完成实现，该代理对象就处于作用域内。以下是我们 `ele`-ment 构建器的样子：

1. 我们的函数接收一个目标对象
2. 我们遍历提供的所有属性，并将其附加到目标对象上
3. 如果提供了文本，也将其附加到目标对象上

```javascript
const ele = function(target) { // ①
  return function (attrs = {}, text = '') {
    for(const [k, v] of Object.entries(attrs)) {
      target[`@${k}`] = v; // ②
    }
    if(text) target['#text'] = text; // ③
    // 代理对象将处于作用域内
    return proxy;
  }
}
```

我们的 `ele` 函数相当简单——它接收一个 `target` 对象，然后构造一个两参数函数并最终返回。这个新构造的函数期望两个（可选）参数——一个代表此元素属性的对象和文本。我们遍历属性对象中的 `entries`，将每个条目以必需的 `@` 前缀附加到目标对象上。如果提供了任何文本，也将其以 `#` 为前缀附加到目标对象上。

*假设* 作用域内有一个叫做 `proxy` 的对象，我们可以像这样使用这个函数：

1. 返回内部函数
2. 用一些属性和一些文本调用返回的函数

```javascript
const obj = {};
const builder = ele(obj); // ①
builder({ isbn: '1590592395' }, 'Practical Common Lisp'); // ②
// 输出 '{ '@isbn': '1590592395', '#text': 'Practical Common Lisp' }'
console.log(obj);
```

正如我们在这里看到的，属性应作为一个对象提供，其中的每个键值对代表一个属性。这允许我们提供多个属性。`ele` 函数简单地构造一个函数（由 `builder` 引用）；当这个函数被调用时，它接收属性和一些文本，然后将必要的元数据附加到 `target` 上。

很好！现在，我们将编写我们的代理处理器函数。

1. 如果遇到 `end`，只需 `stringify` 栈中的最后一个元素
2. 如果遇到 `up`，则丢弃栈顶元素
3. 由于 `up` 是一个方法，调用时返回 `proxy` 本身
4. 获取栈顶元素
5. 将最新元素压入栈顶
6. 处理已经见过的元素
7. 返回用于装饰最新元素的机制，包括属性和文本

```markdown
```javascript
// 'elements' 是先前构造的元素的栈，
// 用一个空对象（根对象）初始化
// 每当我们遇到一个新元素，就将其放在
// 栈顶。
// 每当我们遇到 'up()'，就丢弃栈顶条目
const handler = {
  get(target, property) {
    if (property === 'end') {
      return () => JSON.stringify(elements.pop()); // ①
    }
    if (property === 'up') {
      elements.shift(); // ②
      return () => proxy; // ③
    }
    const [curParent] = elements; // ④
    const child = {};
    elements.unshift(child); // ⑤
    if(curParent[property]) { // ⑥
      const existing = curParent[property];
      if(Array.isArray(existing)) {
        curParent[property] = [...existing, child];
      } else {
        curParent[property] = [existing, child];
      }
    } else {
      curParent[property] = child;
    }
    return ele(child); // ⑦
  },
};
```

这里有很多内容，所以我们一步一步来。XML 允许嵌套，一个元素可以拥有任意数量的子元素。为了跟踪“当前”元素（同时不丢失其所有父元素的踪迹），我们维护了一个栈，即 `elements`，它是一个由先前构造的元素组成的数组。也就是说，“根”元素将始终是数组中的*最后一个*元素。

当我们遇到 `end()` 时，我们通过调用 `pop` 简单地 `JSON.stringify` 数组中的最后一个元素。注意，调用 `end` 会返回 JSON 字符串，从而标志着我们的 DSL 链式调用的结束。

每当客户端调用 `up()` 时，我们简单地丢弃栈中的*第一个*元素，从而使其“父”元素成为下一个操作将影响的元素。这里我们必须小心地返回 `proxy`，以实现流畅的 API；这允许链式调用继续进行。

最后，我们来到实现的核心部分。我们使用解构语法获取 `elements` 数组中的第一个元素。^(⁹⁶) 我们为子元素构造一个占位符（一个空对象），并将其插入到 `elements` 栈的顶部，从而使其成为“当前”元素。

接下来，我们使用 `property` 作为键，将子元素关联到父元素。然而，XML 允许一个父元素拥有多个同名子元素。如果发生这种情况，我们确保在将*所有*同名的子元素关联到父元素之前，将它们包装在一个数组中。

现在，父元素已经与一个空子元素（或一个子元素数组）关联，以 `property` 为其键，我们只需返回一种机制，以便用户在需要时用属性和文本来装饰子元素。

最后一步，我们将所有内容整齐地包装在 `jsonToXmlBuilder` 函数中。请注意，包装函数包含了 `ele` 和 `handler` 的定义：

```javascript
const jsonToXmlBuilder = () => {
  // 为简洁起见省略了部分内容
  // const ele = function(target) { get(target, property) { ... } }
  // const handler = { ... }
  const root = {};
  const elements = [root];
  const proxy = new Proxy(root, handler);
  return proxy;
};
```

我们从一个空的 `root` 元素开始，并将其作为 `elements` 栈中的第一个元素。接下来，我们通过调用 `Proxy` 构造函数来构造 `proxy` 对象，向其提供 `root` 对象和处理器函数，然后返回代理对象。大功告成！让我们来试一下，看看效果如何：

```javascript
const books = jsonToXmlBuilder()
.books({ count: '2' })
.book({ id: '1' })
.title({ isbn: '1590592395' }, 'Practical Common Lisp')
.up()
.up()
.book({ id: '2' })
.title({ isbn: '9780133708752' }, 'ANSI Common LISP')
.up()
.up()
.subject({}, "Lisp")
.up()
.end();
console.log(books);
// prints
// {
//   "books": {
//     "@count": "2",
//     "book": [{
//       "@id": "1",
//       "title": {
//         "@isbn": "1590592395",
//         "#text": "Practical Common Lisp"
//       }
//     }, {
//       "@id": "2",
//       "title": {
//         "@isbn": "9780133708752",
//         "#text": "ANSI Common LISP"
//       }
//     }],
//     "subject": {
//       "#text": "Lisp"
//     }
//   }
// }
// paste the output in https://www.freeformatter.com/json-to-xml-converter.html
// and you can verify the generated XML
```

借助代理，我们已经解锁了“精通”级别。这个例子展示了代理的真正威力——仅用大约 50 行代码，我们就实现了一个强大的领域特定语言，能够以简洁且声明式的方式表达意图。
```