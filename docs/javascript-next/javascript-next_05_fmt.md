# 2. 使用箭头函数的 Lambda 表达式

函数在 JavaScript 中无处不在。长期以来，人们一直需要对它们进行语法整容，使其更简洁，从而更易于使用和理解。

JavaScript 本质上是一种函数式编程语言。它支持函数作为一等公民，这意味着函数就像语言中的任何其他类型一样。函数可以赋值给变量，作为参数传递给其他函数，并且可以像 JavaScript 中任何其他类型一样从函数中 `return` 返回。JavaScript 也是单线程的。设计良好的涉及长时间运行操作的 API 和库会异步工作，通常接受一个回调函数，该回调函数在（异步）任务完成时被调用。这些回调恰好也是函数。最后，在对象上定义方法的机制恰好也是函数。

在本章中，我们将熟悉一种用于函数表达式的新语法，也称为 ES6 引入的“箭头”函数。我们将看到如何通过更短的语法使我们的代码富有表现力，从而清晰地表达我们的意图。我们的讨论将包括箭头函数的行为与常规函数有何不同，从而使我们能够辨别在何时最好地使用箭头函数。在本章结束时，你将渴望回到你的代码库，因为你知道你可以自信地消除任何不必要的冗余，留下的是……嗯，我们姑且称之为诗，好吗？

## 重新评估函数定义的冗余性

在 ES5 中，我们可以使用几种结构来定义函数，两种主要机制是函数语句和函数表达式。本章的重点将是函数表达式；然而，我有必要详细阐述两者之间的语法差异。考虑以下函数*语句*。

- ① 一个函数语句
- ② 打印 `function`
- ③ 打印 `identity`

```javascript
function identity(n) { ①
return n;
}
// 调用它
console.assert(identity(42) === 42);
// 检查它
console.assert((typeof identity) === 'function'); ②
console.assert(identity.name === 'identity'); ③
```

函数语句要求为新创建的函数提供一个名称；否则，我们以后如何调用它？从我们对新定义函数的检查可以看出，它*是* `function` 类型。函数是 `Function` 类型的对象，相应地，它们具有我们可以查询的属性，例如它们的 `name` 和 `length`。^(¹⁵)

现在我们将看看同一个函数，这次是使用函数表达式定义的。

- ① 一个函数表达式
- ② 打印 `function`
- ③ 打印 `identity`

```javascript
const identity = function(n) { ①
return n;
};
// 调用它
console.assert(identity(42) === 42);
// 检查它
console.assert((typeof identity) === 'function'); ②
console.assert(identity.name === 'identity'); ③
```

正如我们所见，这也定义了一个函数，其行为与使用语句定义的函数完全相同。我们没有为函数提供名称，而是将表达式的值赋给一个变量，或者在这个例子中，赋给一个 `const`。这个函数与使用语句定义的函数相同，因此行为也相同。

然而，不同之处在于它们是如何被加载的。函数语句在任何代码执行之前被加载。这意味着你可以任意位置定义函数语句，并在任意位置使用它——你甚至可以在将其定义为函数语句之前调用该函数。另一方面，函数表达式涉及对变量的赋值，正如我们从上一章所了解的，这些赋值受提升规则约束。因此，在将变量*赋值*给函数表达式*之前*使用该变量将导致错误。请注意：

- ① 允许在实际定义之前使用定义为语句的函数
- ② 一个函数语句
- ③ 在实际定义之前使用定义为表达式的函数会导致错误
- ④ 一个函数表达式

```javascript
// 在定义之前调用它
fnStatement(); ①
function fnStatement() { ②
console.log('I am a function statement');
}
// 调用它
fnStatement();
// fnExpression(); ③
const fnExpression = function () { ④
console.log('I am a function expression');
};
// 调用它
fnExpression();
```

选择将函数定义为语句还是表达式很大程度上是品味问题。但是考虑到函数表达式允许我们省略函数名称，它们往往比语句对应的函数稍微简洁一些。然而，有时即使是这种相对简洁的函数表达式语法也常常被证明是相当冗长的。考虑这样一个场景，我们尝试对 `Array` 的元素进行一系列操作以得出最终值：

- ① 将数组中的每个数字乘以三
- ② 仅过滤出偶数
- ③ 将结果归约为它们的和

```javascript
const nums = [1, 2, 3, 4, 5];
const result = nums
.map(function(n) {
return n * 3; ①
})
.filter(function(n) {
return (n % 2) === 0; ②
})
.reduce(function(acc, n) {
return acc + n; ③
}, 0);
```

这个例子可能有点老套；然而，它确实突显了即使编写最简单的函数所涉及的形式化流程。因此，很难解析出我们*实际*想要完成什么。这经常导致开发者将回调提取到（命名的）函数语句中，这不仅更加冗长，而且最终会使代码分散，使得更难以理解我们试图完成的任务。

箭头函数来救场了！

## 箭头函数

箭头函数（有时称为“胖”箭头函数——你会看到这些术语可以互换使用）为我们提供了一种更加简洁的函数定义语法。让我们从探索箭头函数的语法开始。

### 语法

箭头函数消除了函数表达式带来的大量冗余。让我们通过一个简单的例子来吊起胃口：

```javascript
const doubleIt = n => n * 2;
// 调用它
console.assert(doubleIt(8) === 16);
```

在深入细节之前，让我们尽情欣赏箭头函数的优雅。`function` 关键字、括号、花括号和 `return` 关键字都消失了。（最后两个并非完全正确，正如我们将在接下来的讨论中看到的。）剩下的只有函数的**真正**意图。

正如我们所见，与函数表达式非常相似，我们不为函数本身提供名称。相反，我们将*求值*表达式的结果赋值给一个变量，以便稍后可以引用它。

现在我们已经尝到了箭头函数的甜头，让我们讨论一些语法细节。

### 带括号还是不带括号？

箭头函数的参数列表位于胖箭头左侧。**当且仅当**函数签名只有一个参数时，可以省略括号^(¹⁶)。以下代码片段展示了箭头函数参数允许的各种排列方式：

- ① 零参数列表必须带括号
- ② 单参数列表可省略括号
- ③ 两个或更多参数必须带括号

```javascript
const noop = () => {}; ①
const identity = n => n; ②
const get = (obj, k) => obj[k]; ③
```

这引出一个问题：为单参数箭头函数省略括号是否真的给我们带来好处？JavaScript 社区广泛采用的惯例是尽可能省略括号。换句话说，虽然这似乎违背了单参数箭头语句在语法上应与无参数和多参数箭头函数保持一致的理念，但它确实减少了冗余，尤其是在高阶函数中，我们很快就会看到这一点。

箭头函数语法的右侧部分则更为微妙，我们接下来将进行探讨。

### 表达式体与语句体

到目前为止，我们看到的所有示例都是单行函数体。如果函数需要做更多工作该怎么办？

事实上，我们确实可以使用花括号来定义函数体。但你可能记得，花括号在 JavaScript 中创建的是代码块（*blocks*），而代码块是**语句**，不是表达式！换句话说，我们之前看到的所有示例都使用表达式作为函数体，因此无需显式使用 `return` 从函数返回值。如果我们改用代码块（代码块是语句，因此不会被“求值”），则必须显式地从函数中 `return` 值。（否则，该函数会像任何普通函数一样，隐式返回 `undefined`。）让我们定义一个多行函数体的箭头函数：

- ① 使用花括号开始函数体
- ② 必须显式使用 return

```javascript
const reducer = (acc, n) => { ①
  acc.push(n);
  return acc; ②
};
// 调用它
console.log(reducer([2, 3], 1)); // 输出 '[ 2, 3, 1 ]'
```

**建议**：如果我们要使用花括号，就应该在函数体中显式包含 `return`。不这样做可能会导致令人困惑的结果。请看：

```javascript
const confuser = () => { profession: 'JS Ninja' };
// 调用它
console.log(confuser());
```

快速回答！这个函数被调用时返回什么？它返回一个包含一个键的对象，还是一个带有`label`的代码块，因此返回`undefined`？正如你所见，花括号同时用于定义代码块和对象，这种重载特性与箭头函数语法结合使用时会引起混淆。

实际上，如果JavaScript在箭头函数定义中“看到”花括号，它会将其视为代码块函数体。这意味着调用时，该函数将返回`undefined`。

如果我们确实想返回一个对象呢？我们有两种选择——显式使用带有`return`的代码块，或者将函数体包裹在圆括号中，如下所示：

```
const confuserOne = () => ({ profession: 'JS Ninja' });
// 输出 '{ profession: 'JS Ninja' }'
console.log(confuserOne());
// 或者
const confuserTwo = () => {
  return { profession: 'JS Ninja' };
};
// 输出 '{ profession: 'JS Ninja' }'
console.log(confuserTwo());
```

以上总结了箭头函数所有的语法细节。现在我们来考虑箭头函数与普通函数之间的语义差异。但在讨论之前，我们必须简要回顾一下普通函数以及它们如何处理`this`。

### 普通函数动态设置 `this`

当一个普通函数（使用函数声明或函数表达式定义）引用`this`时，该引用会根据所使用的“调用模式”动态解析：

- ① 设置一个指向作用域内局部函数的属性

- ② 使用“方法调用”模式调用该函数

- ③ 使用“apply调用”模式

- ④ 使用“函数调用”模式调用该函数

```
'use strict';
const returnsThis = function () {
  return this;
};
var obj1 = {
  name: 'obj1',
  method: returnsThis, ①
};
console.assert(obj1.method() === obj1); ②
var obj2 = {
  name: 'obj2',
};
console.assert(returnsThis.apply(obj2) === obj2); ③
console.assert(returnsThis() === undefined); ④
```

我必须强调，我们是在`严格模式`下运行代码，以避免“非安全”环境倾向于强制的自动装箱行为^(¹⁷)。尽管如此，我们使用三种不同的调用模式^(¹⁸)调用了同一个函数，并看到每次调用时`this`的值都不同！

我们首先在对象上设置一个属性，使其指向局部函数，然后像调用该对象上的任何其他函数属性一样，使用“点”运算符调用它。正如我们所料，`this`指向调用时“点”左侧的内容。换句话说，它指向“当前上下文”。

接着我们使用`apply`（或其兄弟`call`），这实际上在调用函数之前设置了函数内部的`this`。另一种理解方式是，这一行等同于方法调用模式，区别在于我们使用`apply`设置`this`，而不是被迫在对象上添加一个新属性。

最后，我们简单地在没有任何上下文的情况下调用该函数——也就是说，我们不在任何对象上调用该函数；在`严格模式`下，该函数简单地返回`undefined`，这正是我们所期望的。

正如我们所看到的，函数表达式（或声明）中的`this`是在调用时设置的，`而不是`在定义时设置的。这就是“普通”函数的行为与箭头函数行为的分歧点。普通函数在调用时动态设置`this`的含义，而箭头函数则词法地绑定`this`；换句话说，箭头函数在*定义*时绑定`this`。

### 词法绑定的 `this`

箭头函数在调用时不会设置`this`的含义。相反，它们将`this`设置或“固化”为创建时`this`求值的结果。请考虑以下代码：

- ① 获取全局对象的句柄以便进行`assert`断言

- ② 定义一个简单地`return` `this`的箭头函数

- ③ 尝试将其用作方法并不会改变上下文

- ④ `apply`也不会改变

- ⑤ 这里的普通函数（正如我们所见）会返回`undefined`

```
'use strict';
const GLOBAL_OBJECT = this; ①
const returnMe = () => this; ②
var obj1 = {
  name: 'obj1',
  method: returnMe,
};
console.assert(obj1.method() === GLOBAL_OBJECT); ③
var obj2 = {
  name: 'obj2',
};
console.assert(returnMe.apply(obj2) === GLOBAL_OBJECT); ④
console.assert(returnMe() === GLOBAL_OBJECT); ⑤
```

定义箭头函数似乎与定义普通函数，然后将`this``bind`到定义时`this`的值完全相同！而事实上正是如此。

让我们看看适用于箭头函数的其他一些注意事项。

### 其他注意事项

箭头函数在其他细微方面也与常规函数有所不同。由于箭头函数的内部实现方式，它们无法被用作构造函数。如果我们想要定义一个构造函数，必须继续使用常规函数。

另一个区别在于，`this`并非唯一按词法作用域绑定的东西。当将定义为表达式或语句的函数被调用时，JavaScript会创建一个对象，该对象会捕获调用时提供给函数的所有参数。这个名为`arguments`的对象在函数体内是可用的。然而，箭头函数不会获得`arguments`对象。不过，`strict mode`限制了`arguments`的API使用范围，并且ES6为我们提供了可变参数（我们很快就会看到），这进一步减少了对`arguments`的需求，因此这本身并不成为一个很大的注意事项。让我们通过一个简单的例子来看看它是如何工作的：

- ① 使用语句定义一个函数

- ② 将函数语句作为构造函数调用，成功返回

- ③ 将函数定义为箭头函数

- ④ 将箭头函数作为构造函数调用会抛出错误

- ⑤ 常规函数在调用时会获得一个`arguments`对象

- ⑥ 而箭头函数则不会

```
function PersonAsRegularFunction(name) { ①
this.name = name;
}
const nate = new PersonAsRegularFunction('Nate'); ②
// 输出 { name: 'Nate' }
console.log(nate);
const PersonAsArrowFunction = (name) => { ③
this.name = name;
};
// const neal = new PersonAsArrowFunction('Neal'); ④
function regularFunction() {
return arguments;
}
// 输出 { '0': 10, '1': 'a', '2': true, '3': false }
console.log(regularFunction(10, 'a', true, false)); ⑤
const arrowFunction = () => arguments;
// 输出 'ReferenceError: arguments is not defined'
// console.log(arrowFunction(10, 'a', true, false)); ⑥
```

让我们退一步思考这意味着什么——常规函数在调用时动态设置`this`的值。同时，会创建`arguments`对象来捕获调用时提供的所有参数。也就是说，在函数体内使用的`this`和`arguments`对象都是*基于上下文的*——它们依赖于调用上下文以便能够正确定义。然而，箭头函数在定义时就固定了`this`的值，并且根本不会被提供`arguments`对象。换句话说，箭头函数被特意设计为尽可能少地使用其周围的上下文。如果我们的函数实现需要使用在运行时定义的上下文，那么使用常规函数将是最佳选择。

### 使用场景

有人可能会想，既然有这么多微妙的变化需要我们注意，箭头函数值得使用吗？正如我们所看到的，JavaScript是一种函数式编程语言。将函数作为参数传递以及从高阶函数中返回函数是JavaScript中非常常见的范式。事实上，大多数`Array`的迭代和操作函数往往是高阶函数，并且为使用箭头函数提供了绝佳的机会。让我们重新审视之前关于对数组执行`map`和`reduce`操作的例子，看看如何改用箭头函数。

- ① 将数组中的每个数字乘以三

- ② 仅筛选出偶数

- ③ 将结果归约为其总和

```
const nums = [1, 2, 3, 4, 5];
const result = nums
.map(n => n * 3) ①
.filter(n => (n %  2) === 0) ②
.reduce((acc, n) => acc + n, 0); ③
```

首先，让我们稍作停顿，思考一下这段代码的优雅之处。如我们所见，所有多余的仪式性代码都消失了，剩下的只是我们试图实现的核心意图。几乎可以说，箭头函数就是为此意图而设计的！

另一个使用场景是当我们*确定地*知道在箭头函数定义时 `this` 代表什么。

*   ① 设置"上下文"

*   ② 箭头函数按预期工作

```
const neo = {
friends: [
'Morpheus',
'Brian',
'Switch',
],
addFriends: function () {
const args = Array.prototype.slice.call(arguments);
args.forEach(f => this.friends.push(f)); ②
},
};
// 调用 addFriends
neo.addFriends('Niobe', 'Tank'); ①
// 输出 '[ 'Morpheus', 'Brian', 'Switch', 'Niobe', 'Tank' ]'
console.log(neo.friends);
```

这里我们知道，`addFriends` 内部的 `this` 将指向 `const neo` 所引用的对象，因为 `addFriends` 是作为 `neo` 的一个方法被调用的。因此，在 `addFriends` 内部，我们可以定义一个能够使用 `this` 的箭头函数，因为在箭头函数被定义时，`this` 指向所有者对象。但是，我们不能将 `addFriends` 本身定义为一个箭头函数，因为在函数定义时，`this` 指向全局对象！正如我们可以推断出的，在箭头函数内部引用 `this` 时，最好确定 `this` 指向什么，或者干脆考虑使用函数表达式。

让我们考虑另一个使用高阶函数来构建小型领域特定语言（DSL）的例子。在这个例子中，我们将使用纯函数——即那些只使用传递给它们的参数的函数。也就是说，它们是"无状态的"，因此是使用箭头函数的绝佳候选。这将为我们提供一个极好的机会，来看看箭头函数的简洁性如何帮助我们。

*   ① 定义一个高阶谓词函数

*   ② 测试提供的参数是否大于 10

*   ③ 测试数字是否为偶数

*   ④ 创建一个测试两个条件的函数

```
const and = (a, b) => arg => a(arg) && b(arg); ①
const gt10 = n => n > 10; ②
const even = n => n %  2 === 0; ③
const gt10AndEven = and(gt10, even); ④
console.assert(gt10AndEven(15) === false);
console.assert(gt10AndEven(22) === true);
```

这里我们定义了 `and` 函数，一个高阶谓词函数^((19))，它接收两个函数，并返回一个新函数。这个新函数期望接收一个参数 `args`，该参数会针对两个谓词进行测试，当且仅当两个谓词都满足时才返回 true。^((20))

然后我们定义了两个谓词，并使用 `and` 创建了一个新函数，以测试其提供的参数是否既大于 10 又是偶数。

再次，我们观察到箭头函数使我们能够敏锐地表达我们的意图，省略了与函数表达式相关的任何多余的仪式性代码。