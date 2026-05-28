# 3. 使用默认参数与剩余参数构建高效函数签名

我们都深知函数在 JavaScript 中的广泛应用。因此，定义函数并设计函数签名是我们经常要做的事情。ES6 引入的新特性——默认参数，让我们能够改进函数签名的设计方式。它允许我们指定：如果在调用函数时未传入参数值，参数应取什么值。类似地，剩余参数让我们能够表达一个函数（或方法）不限制可提供的参数数量——换句话说，它们让我们能够显式地声明可变参数函数。

在本章中，我们将探索默认参数和剩余参数的新语法。我们将看到如何编写函数签名，让使用者拥有极大的灵活性，同时减少我们在函数内部需要进行的错误检查。学完本章后，我们可能会感叹：以前是如何凭借现有的函数知识来工作的！

## 无意中用 arguments 对象掩盖了意图

JavaScript 函数签名只包含函数名称。这与许多其他语言不同，其他语言的签名包含返回类型和参数列表。由于 JavaScript 是动态类型语言，函数不声明返回类型。然而，与我们这里讨论最相关的是：参数列表也不参与函数签名。这也是为什么我们在 JavaScript 中没有函数重载的原因。两个同名函数（即使参数列表不同）**会**发生冲突，后定义的函数会覆盖前一个。

此外，JavaScript 允许调用者根据意愿传入任意数量（少至零个多至任意）的参数来调用函数。这意味着，尽管函数有*形式*签名（也称为“元数”），但 JavaScript 中*每个*函数实际上都是多元函数。我们可以通过以下方式进行验证：

* ① 回顾：箭头函数在调用时不会创建 `arguments` 对象。这迫使我们使用函数声明。

* ② 用零个参数调用 `sayHello`

* ③ 用多于函数预期的参数调用 `buildUrl`

* ④ 正确调用 `add`

```javascript
const sayHello = name => 'Hello ' + name;
const buildUrl = (site, protocol) => protocol + '://' + site;
function add( /**args*/ ) { ①
  let result = 0;
  for (const i in arguments) {
    result += arguments[i];
  }
  return result;
}
console.assert(sayHello.length === 1);
console.assert(buildUrl.length === 2);
console.assert(add.length === 0);
// 输出 'Hello undefined'
console.log(sayHello()); ②
// 输出 'https://jsrocks.com'
console.log(buildUrl('jsrocks.com', 'https', '非必需参数')); ③
// 输出 42
console.log(add(12, 22, 8)); ④
```

这里，我们定义了三个元数不同的函数。官网上，所有三个函数都通过函数对象上的 `length` 属性报告了元数。

然而，正如所展示的，我们可以随心所欲地用任意数量（多或少）的参数调用这些函数，而输出结果显示，我们可能得到也可能得不到期望的结果。事实证明，JavaScript 函数可以通过在参数列表中正式列出参数来声明它们期望接收的参数数量。但是，在运行时并没有检查机制来确认函数是否被传入了正确数量的参数。本质上，我们只能*假设*函数会被正确调用，或者通过在实现中安装适当的检查来做好准备，以防我们没有得到所有期望的参数。

最后，在我们的例子中，`add` 本身就是一个试图实现多元功能的函数——也就是说，它确实可以消耗我们提供的任意数量的参数。但由于语言中没有一等的构造来将其声明为函数签名的一部分，我们只能被迫向使用者提供提示（通常通过文档或注释，如所示），告诉他们如果有需要可以传入额外的参数。

于是，默认参数和剩余参数应运而生。

## 默认参数

默认参数提供了一种机制来突显函数真正需要什么，以及一种在未提供某些参数时进行假设的方式。回想一下，JavaScript 没有任何运行时强制执行机制来确保函数被传入正确数量的参数。因此，我们开发者被迫在实现中处理缺失的参数。观察以下代码：

* ① 通过抛出运行时错误来强制要求提供 `site`

* ② 我们可以为 `protocol` 设置默认值

```javascript
/**
 * 构建 URL
 *
 * @param {site} 网址（必需）
 * @param {protocol} 可以是 http 或 https（可选）——默认为 http
 * @returns {string}
 */
const buildUrl = (site, protocol) => {
  if (!site) { ①
    throw new Error('site 是必需的');
  }
  const p = protocol || 'http'; ②
  return p + '://' + site;
};
```

这里，我们尝试构建一个更健壮的 `buildUrl` 函数实现。然而，`site` 是必需的而 `protocol` 是可选的这一事实，现在被隐藏在了实现内部。文档可以作为辅助手段——但众所周知，文档通常要么不完整，最坏情况下甚至根本不存在。

默认参数允许我们像这样表达同样的逻辑：

```javascript
const buildUrl = (site, protocol = 'http') => {
  if (!site) {
    throw new Error('site 是必需的');
  }
  return protocol + '://' + site;
};
```

现在，`protocol` 的默认值在方法签名中被显式声明，表明了*如果*使用者决定省略它，参数会是什么值。诚然，`site` 是必需的这一事实仍然隐含未明；但函数作者未能为其找到合适的默认值，这件事本身可能已经说明了问题。

换句话说，快速瞥一眼这个函数的签名就能发现更多信息，而以前这迫使用户去查看实现（如果可能的话）、查阅文档（如果有的话），或者通过参数组合进行“俄罗斯轮盘赌”式的猜谜，来判断哪些可行、哪些不可行。

既然我们已经将默认参数作为语言的一等构造，像 EsDocs^(²¹) 这样的工具甚至可以解析默认参数，并在生成的文档中自动高亮显示它们。这使得使用这个特性更加诱人。

现在让我们来看看默认参数的一些微妙之处。

### 限定与跳过默认值

我们可以将参数默认值设为任何*表达式*——赋值、函数调用，甚至三元表达式都可以。当默认值是配置或环境查找的结果时，这尤其有用：

```javascript
const getDefaultConn = () => {
  // 查找环境配置
  // 返回合适的连接
  // 默认假设为本地主机
  return {
    host: 'localhost',
    user: 'me',
    password: 'secret',
    database: 'my_db',
  };
};
const query = (sql, conn = getDefaultConn()) => {
  // 在此使用连接字符串和 sql 查询数据库
  // conn.connect();
  // conn.query(sql);
  // conn.close();
  return conn;
};
```

可能不太明显的一点是，带默认值的参数是在函数被调用时求值，而不是在函数定义时求值。正如我们所看到的，这让我们可以*推迟* `conn` 的值，直到实际调用它。

另一个细微之处在于，参数可以访问函数签名中先前声明的参数，并在分配自己的默认值时使用它们。这在我们考虑多态实现时尤其有用。请看以下示例：

- ① 参数访问先前参数

- ② 只提供一个边长来创建正方形

- ③ 提供两个边长来创建矩形

```javascript
const rectangle = (x, y = x) => [x, y]; ①
// 输出 '[ 2, 2 ]'
console.log(rectangle(2)); ②
// 输出 '[ 5, 12 ]'
console.log(rectangle(5, 12)); ③
```

当然，我们还可以用有趣的方式混合搭配所有这些：

```javascript
const triangle = (a, b = a, c = Math.hypot(a, b)) => [a, b, c];
// 输出 '[ 6, 4, 7.211102550927979 ]'
console.log(triangle(6, 4));
// 输出 '[ 10, 10, 14.142135623730951 ]'
console.log(triangle(10));
```

我们必须考虑什么会触发默认参数的使用。显然，*不*传入参数会触发它。换句话说，传入“空”会导致 JavaScript 使用默认值。仔细想想，JavaScript 中的 `undefined` 就相当于“空”。^((22)) 因此，对于给定的函数，如果我们确实希望某个参数使用默认值，那么在该位置传入 `undefined` 就相当于“跳过”它。

### 使用注意事项

JavaScript 的解析器允许任意组合的默认参数。我们可能希望将默认值设为函数的第一个参数，其余参数在调用时填充，或者在常规参数和带默认值的参数之间交替。虽然这看起来挺有趣，但它在开发中的实用性相当有限：

```javascript
const filter = (coll = [], predFn) => coll.filter(predFn);
// 输出 '[ 30 ]'
console.log(filter([10, 20, 30], n => n > 25));
```

如果未提供 `coll`，我们将其默认设为空数组。然而，一个没有谓词函数进行过滤的 `filter` 函数毫无意义。此外，如果我们希望传入绝对必要的谓词函数，就必须传入第一个参数，因为我们无法在函数调用时跳过参数。当然，有人可能会争辩说，我们可以传入 `undefined` 作为第一个参数，从而触发默认值，但这恰恰证实了关键点：即函数签名中参数的顺序**仍然**很重要！

按照惯例，函数的必填参数和“最重要”的参数放在左侧，其次是那些*可能*可选的参数。这一惯例并不会因为默认参数的使用而削弱。默认参数只能访问其*左侧*的参数这一事实更强调了这一点。

简而言之，将必要参数（以及那些*无法*假设的参数）放在左侧，带默认值的参数放在右侧。这有助于突出某些参数是绝对必要的，同时允许我们根据调用时的值为其他参数设置默认值。双赢。遵循这一惯例可以帮助我们让函数签名更具描述性，减少甚至消除使用者的猜测。

## 剩余参数

当函数期望接收固定数量的参数时，默认参数的效果非常出色。我们可以选择突出哪些是必需的，同时为其他参数设置默认值。但如果一个函数想要表达它可以接受不定数量的参数，并且确实能够使用所有参数呢？这时剩余参数就派上用场了。

### 反对使用 `arguments` 的理由

JavaScript 的 `arguments` 对象一直存在于调用上下文中，用于处理"额外"的参数。让我们回顾之前将传入所有参数进行 `add` 操作的例子：

```javascript
function add( /**args*/ ) {
  let result = 0;
  for (const i in arguments) {
    result += arguments[i];
  }
  return result;
}
// 输出 '20'
console.log(add(2, 3, 5, 10));
```

这个函数期望接收 `n` 个参数，并通过消耗所有参数来返回结果。它能完成我们的目标吗？当然能。然而，为了表明该函数确实是多参数的，我们不得不借助注释块，从而在实现中隐藏了这一事实。

使用 `arguments` 还有两个更明显的缺陷。虽然 `arguments` 支持 `length` 属性，并允许我们使用下标（`[]`）运算符来访问某个特定传入参数，但问题是 `arguments` 对象实际上**不是一个真正的数组**！如果我们想对传入参数进行操作（例如进行 `sort` 排序或 `filter` 过滤），只能将 `arguments` 强制转换为数组。

第二个问题是，`arguments` 对象包含**所有**传入函数的参数。如果希望对某些参数进行差异化处理，我们就需要将它们与其他参数分离开来。

我们可以通过一个模拟飞机登机的简单示例来突出这两个缺陷。示例中的飞机有一名飞行员、一名乘务员和若干名乘客。但我们必须确保先招待最忠诚的客户，因此需要让它们优先登机。每位乘客都有一个 `status`（身份），身份等级更高的乘客应*先于*其他乘客登机。让我们先编写一个简单的比较函数，通过乘客的身份进行两两比较：

```javascript
const byStatus = (a, b) => {
  let ret;
  if (a.status === 'platinum' && b.status !== 'platinum') {
    ret = -1;
  } else if (a.status === 'platinum' && b.status === 'platinum') {
    ret = 0;
  } else if (a.status !== 'platinum' && b.status === 'platinum') {
    ret = 1;
  }
  return ret;
};
```

比较函数就绪后，我们可以编写飞机登机算法。请注意，我们将飞机表示为一个数组，首先 `push` 最先登机的飞行员，然后是乘务员，最后是按个人身份排序的乘客：

*   ① 飞行员先登机
*   ② 接着是乘务员
*   ③ 将 `arguments` 转换为数组
*   ④ 丢弃前两项，因为它们已被处理
*   ⑤ 使用自定义排序函数对数组中的剩余项进行排序

```javascript
function boardPlane(pilot, crewMember /** , passengers */ ) {
  const plane = [];
  plane.push(pilot); ①
  plane.push(crewMember); ②
  const toArr = Array.prototype.slice.call(arguments); ③
  const passengers = toArr.slice(2, toArr.length); ④
  plane.push(passengers.sort(byStatus)); ⑤
  return plane;
}
const passengerOne = {
  name: 'James',
  status: 'silver',
};
const passengerTwo = {
  name: 'Joseph',
  status: 'platinum',
};
// 调用函数
const plane = boardPlane('Amelia', 'Rosemary', passengerOne, passengerTwo);
console.assert(Array.isArray(plane));
// 验证飞行员和乘务员先登机
console.assert(plane[0] === 'Amelia');
console.assert(plane[1] === 'Rosemary');
// 验证乘客按身份等级登机
console.assert(Array.isArray(plane[2]));
console.assert(plane[2][0] === passengerTwo);
console.assert(plane[2][1] === passengerOne);
```

这段代码可以运行，但感觉很笨拙。我们讨论过的所有缺陷都暴露无遗。首先，我们必须将 `arguments` 对象强制转换为数组，才能最终对乘客进行 `sort` 排序。其次，`arguments` 对象包含所有传入 `boardPlane` 的参数，因此为了获取乘客，我们必须丢弃前两个参数（即飞行员和乘务员），因为它们已经被处理过了。`arguments` 对象不会区分已通过函数参数名占位的参数，这迫使开发者自行判断哪些参数已被捕获，哪些代表“剩余”参数。

唉！要是能有一种方法，自动将所有"未处理"的参数打包成一个数组就好了……

### 新的 `...` 语法

剩余参数使用了一种新语法，即在参数列表中使用 `...argName`。这个参数会收集函数调用中传入的、**尚未被**处理的参数，并将它们封装成一个 `Array` 对象。

现在我们可以重构示例，使用剩余参数，如下所示：

*   ① 显式声明一个剩余参数
*   ② 直接对剩余参数调用数组方法

```javascript
const boardPlane = (pilot, crewMember, ...passengers) => { ①
  const plane = [];
  plane.push(pilot);
  plane.push(crewMember);
  plane.push(passengers.sort(byStatus)); ②
  return plane;
};
const passengerOne = {
  name: 'James',
  status: 'silver',
};
const passengerTwo = {
  name: 'Joseph',
  status: 'platinum',
};
// 调用函数
const plane = boardPlane('Amelia', 'Rosemary', passengerOne, passengerTwo);
console.assert(Array.isArray(plane));
// 验证飞行员和乘务员先登机
console.assert(plane[0] === 'Amelia');
console.assert(plane[1] === 'Rosemary');
// 验证乘客按身份等级登机
console.assert(Array.isArray(plane[2]));
console.assert(plane[2][0] === passengerTwo);
console.assert(plane[2][1] === passengerOne);
```

几乎像魔法一样，代码变得更简洁，意图也更清晰。不再需要不必要的强制转换，而且由于我们不再需要像处理 `arguments` 那样处理额外参数，像差一错误这样的问题也彻底消除了。取而代之的是，我们获得了一个真正的数组，其中包含构成“剩余”参数的所有传入参数。只需看一眼函数签名，我们就知道所需的一切——这是一个多参数函数，至少需要两个命名得当的参数，以及一个潜在的乘客列表。

此外，我们还获得了一个额外的好处。这一点很微妙，但让我们看看能否发现原始版本和重构版本之间的另一个区别。看出来了吗？

回想一下，箭头函数在被调用时不会创建 `arguments` 对象。如果我们需要使用 `arguments`，就不得不使用常规函数表达式或函数声明。但在这个例子中，我们可以重新使用更简洁的箭头函数来定义 `boardPlane`。耶！

### `rest` 参数的细节（或者说 `rest` 参数的细节？）

剩余参数（Rest parameters）远没有我们目前讨论的其他特性那么微妙复杂。如果一个函数使用了剩余参数，那么该参数必须是参数列表中的最后一个。这是有道理的，因为它作为一个贪婪运算符，会吞下所有尚未被指定的参数。在函数签名中，如果在剩余参数之后声明参数，将会导致 `SyntaxError` 错误。

与常规参数不同，剩余参数不能被赋予默认值。剩余参数有一个隐式的默认值，即一个空数组。换句话说，如果没有剩余参数可供消耗，它将默认为一个长度为 `0` 的数组。这非常方便，因为我们不必进行 `undefined` 检查。

鉴于剩余参数允许我们定义多参数函数，让我们看看它如何与函数的 `length` 属性配合工作。回想一下，函数的 `length` 属性返回其签名中定义的正式参数数量。事实证明，剩余参数不计入函数的参数计数。

函数也没有暴露一个 API 来确定某个特定函数是否确实使用了剩余参数。虽然这符合 JavaScript 最初设计中每个函数都是隐式多参数的风格，但也让我们无法通过编程方式发现最后一个参数是否是剩余参数。

最后，我们讨论剩余参数和 `arguments` 对象。虽然箭头函数没有 `arguments` 对象，但常规函数仍然有。正如你所料，`arguments` 对象的工作方式符合预期——它收集所有传递给函数的所有参数，而不考虑最后一个参数是剩余参数这一事实。

让我们用一个简单的例子快速探索所有这些内容：

* ① 参数个数不包含函数表达式中的剩余参数

* ② 箭头函数也是如此

* ③ `arguments.length` 报告调用时提供的总参数数量

```
function fnExpression(a, b, ...c) {
  console.log(arguments.length);
}
const fnArrow = (a, b, ...c) => {
  // 没有 arguments 对象
};
console.assert(fnExpression.length === 2); ①
console.assert(fnArrow.length === 2); ②
// 输出 '6'
fnExpression(1, 2, 3, 4, 5, 6); ③
```