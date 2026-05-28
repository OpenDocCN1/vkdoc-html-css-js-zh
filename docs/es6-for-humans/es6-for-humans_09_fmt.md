# 3. 解构

对象字面量和数组可能是 JavaScript 中使用最广泛的符号之一。这些 JavaScript 符号的流行甚至扩展到了语言之外，体现在`JSON`数据格式中。在 JavaScript 中，对象和数组通常用于分组数据，并且存在多种模式，以便在需要时系统地从中提取数据。ES6 通过一个称为解构的过程，使这一过程更加容易和简单，进一步扩展了这种能力。

## 对象和数组的解构

解构基本上是一种将数据结构分解为更小部分，以便更容易访问其数据并从对象或数组中提取多个值的便捷方式。为了更好地理解解构，只需将其视为从对象或数组进行结构化赋值。考虑以下示例：

```javascript
var letters = ['a', 'b', 'c'],
x = letters[0],
y = letters[1],
z = letters[2];
console.log( x, y, z );             // a b c
```

在上面的例子中，我们给一个名为`letters`的数组赋值，然后通过`letters`变量上的索引给变量`x`、`y`和`z`赋值。让我们看另一个使用对象的例子：

```javascript
var numbers = {a: 1, b: 2, c: 3},
a = numbers.a,
b = numbers.b,
c = numbers.c;
console.log( a, b, c );             // 1 2 3
```

在这个例子中，我们使用`numbers.a`的值来给变量`a`赋值，类似地，使用`numbers.b`和`numbers.c`来给`b`和`c`赋值。ES6 通过一种称为解构的新专用语法，使这种结构化赋值模式更加简单。这种语法消除了对临时中间变量（`letters`和`numbers`）的需求。考虑以下示例：

```javascript
var [ x, y, z ] = ['a', 'b', 'c'];
var { a: a, b: b, c: c } = {a: 1, b: 2, c: 3};
console.log( x, y, z );             // a b c
console.log( a, b, c );             // 1 2 3
```

> **注意**
>
> 我们通常习惯在赋值语句的右侧看到像`{a: a, b:b, c:c }`和`[x, y, z]`这样的对象和数组语法。解构翻转了这种模式，将它们放在左侧，并带有变量声明关键字，允许你将右侧分解为单独的值，并将它们分配给左侧的变量。

如前面的两个示例所示，在 ES6 之前，从对象和数组中获取信息并将其放入局部变量需要更多的代码。想象一下，你需要从一个非常大的对象或数组中提取值，并将它们存储到同名的变量中。你必须编写大量代码逐个赋值，但使用解构，这个过程简化为一个单一的赋值语句。

使用解构将数据结构分解为更小的部分，使得获取所需数据变得更加容易。此外，ES6 中的解构使用了对象和数组字面量语法，这对传统的 JavaScript 用户来说已经非常熟悉。

#### 对象解构语法

让我们更仔细地看一下对象解构语法。我们看到的对象解构语法是：

```javascript
var { a: a, b: b, c: c } = {a: 1, b: 2, c: 3};
```

在这个例子中，你可以注意到，被赋值的变量名与返回对象的属性名相同。但它们不必相同，你可以为被赋值的局部变量使用任何名称。但是，如果它们相同，则可以通过省略`"a: "`部分来进一步缩短语法。这个声明语句可以直接写成：

```javascript
var { a, b, c } = {a: 1, b: 2, c: 3};
console.log( a, b, c );             // 1 2 3
```

使用更短的语法而不是长形式对我们来说非常有意义，除非你想将属性赋值给一个不同的变量名。但是在使用长形式时，有一个重要的细微差别你需要小心。考虑以下示例：

```javascript
var { a: foo, b: bar, c: baz } = {a: 1, b: 2, c: 3};
console.log( foo, bar, baz );         // 1 2 3
console.log( a, b, c );               // ReferenceError
```

仔细观察这个例子。这种模式看起来可能很直接：左侧从右侧的对象表示法`{ a: foo, b: bar, c: baz }`中获取值。但实际上它是相反的。它并不是我们通常习惯的“目标：源”模式，而更准确地说是其逆模式，即“源：目标”模式。为什么会这样令人困惑呢？如果你换个角度看，这个模式会更有意义。考虑以下赋值语句：

```javascript
var foo = 42, bar = 100;
var obj = { a: foo, b: bar };
var { a: FOO, b: BAR } = obj;
console.log( FOO, BAR );              // 42 100
```

在这个例子中，`a`和`b`指的是对象的属性，而在对象解构赋值语句中，`a`和`b`同样代表对象属性。回想一下之前在简化代码的例子中，我们提到可以省略`a:`部分，正是这个原因。在这个例子中，如果你去掉语法中的`a:`和`b:`部分，你将只剩下`FOO`和`BAR`。这种语法有时会令人困惑，但如果你将其理解为相应的属性名和值彼此映射，就很容易掌握了：在这个例子中，`foo`被赋值给`FOO`，`bar`被赋值给`BAR`，使用的就是`a`和`b`属性名。

使用对象解构，你实际上可以通过同一个属性值来设置多个变量的值。考虑以下示例：

```javascript
var { x: foo, x: bar } = { x: 42 };
console.log( foo );  // 42
console.log( bar );  // 42
```

在这种情况下，`foo`和`bar`都通过对象的`x`属性获取其值。

在解构声明语句中，你可以根据情况和需求自由使用`let`、`const`或`var`。但请确保语句中始终存在赋值操作，即语句的右侧是必需的。

```javascript
var { x, y };         // syntax error!
let { x, y };         // syntax error!
const { x, y };       // syntax error!
```

以上所有声明都会导致语法错误，因为解构声明中缺少初始化器。虽然我们知道`const`在所有情况下都需要初始化器，但使用解构时，`var`和`let`也需要初始化器。

#### 使用解构进行赋值

到目前为止，在我们的讨论中，我们只看到了使用解构声明语句的情况。但解构也可以用于赋值语句。考虑以下使用之前例子中的`getChars()`和`getNumbers()`函数的示例：

```javascript
var a, b, c, x, y, z;
[x, y ,z] = getChars();
( { a, b, c } = getNumbers() );
console.log( x, y, z );             // a b c
console.log( a, b, c );             // 1 2 3
```

在这个例子中，`x`、`y`、`z`、`a`、`b`和`c`在声明后都通过解构进行了赋值。

> **注意**
>
> 你必须始终在对象解构赋值语句周围加上括号。这是因为左花括号用于表示块语句的开始。周围的括号则表示该花括号应被解释为在赋值语句中使用解构的表达式。

你也可以在变量通过解构赋值后更改它们的值。考虑以下示例：

```javascript
let item = {
name: "Apples",
quantity: 5
},
name = "Oranges",
quantity = 3;
// assigning different values using destructuring
({ name, quantity } = item);
console.log(name);          // "Apples"
console.log(quantity);      // 5
```

在这个例子中，`name`、`quantity`和`item`在一个声明语句中初始化了值。然后，`name`和`quantity`通过从`item`中解构获取值而被赋予了新值。

#### 默认值

当使用解构从没有相应属性名的对象中给变量赋值时，该变量的值会被设置为`undefined`。例如：

```javascript
var item = {
name: "Apples",
quantity: 5
};
var { name, quantity, value } = item;
console.log(name);           // "Apples"
console.log(quantity);       // 5
console.log(value);          // undefined
```

在这个例子中，一个额外的变量`value`在`item`中没有对应的属性，它被声明在解构语句中。当`name`和`quantity`从`item`获取各自的值时，`value`被设置为`undefined`。

此外，与其让额外的变量值为`undefined`，你也可以选择在指定属性缺失的情况下为其定义一个默认值。为此，只需在属性名后面使用等号`=`并指定一个默认值，如下所示：

```
var item = {
name: "Apples",
quantity: 5
};
var { name = "Oranges", quantity = 3, value = 25 } = item;
console.log(name);          // "Apples"
console.log(quantity);      // 5
console.log(value);         // 25
```

在这个例子中，变量`quantity`和`value`分别被赋予了默认值`3`和`25`。由于没有`item.value`属性，变量`value`使用了其默认值`25`。这与我们之前章节中看到的函数默认参数值的工作方式类似。

> **注意**
>
> 只有在`item`中缺少某个属性时，默认值（如果未指定则为`undefined`）才会被赋给该变量。

你也可以在长格式的解构赋值语句中使用默认值。但此时代码可能会变得混乱且有些令人困惑。考虑以下示例：

```
var { a, b, c: c = 3, d: FOO = 42 } = {a: 1, b: 2};
console.log( a, b, c, FOO );     // 1 2 3 42
```

在这种情况下，变量`a`和`b`被声明并设置为右侧对象字面量中它们对应的属性值，但由于对象中不存在属性`c`，其值被设置为默认值`3`。声明的变量`FOO`也被设置为其默认值`42`，因为对象中不存在对应的属性`d`。这里你需要记住我们之前讨论的“目标：源”语法，这有时会导致混淆。因此，除非绝对必要，否则最好避免这种混合的编码风格。

另一个建议是，避免在解构语句中使用对象或数组作为默认值。这同样会导致任何阅读你代码的人（包括你自己）感到困惑。请看下面的例子：

```
var a = 1, b = 2, c = 3;
var obj = { a: { b: 42 }, c: { b: c } };
( { b: a = { b: b } } = obj );
( { c: b = { b: c } } = obj );
( { a: c = { b: a } } = obj );
console.log( a.b, b.b, c.b );
```

尝试猜测`console.log`语句的输出会是什么。如果你猜出是`"2 3 42"`，那完全正确。但你明白为什么这会令人困惑了。

### 嵌套解构

解构语法，与对象字面量语法非常相似，可用于导航到嵌套对象内部以检索信息。如果你解构的值包含嵌套的对象或数组，你也可以解构这些嵌套的值。这里有一个例子：

```
let items = {
count : 2,
name: "fruits",
apple: {
quantity: 5,
value: 25
},
orange: {
quantity: 3,
value: 5
}
};
let { apple: { quantity }} = items;
console.log(quantity);        // 5
```

这里的花括号用于表示我们需要首先深入到`items`对象内部的`apple`属性一级，然后获取`quantity`属性。根据我们之前的讨论，我们知道`':'`之前的标识符表示我们需要去的位置，而右侧则进行赋值。如果冒号后面跟着一个花括号，则表示目标位于对象的下一个嵌套深度层级，以此类推。

由于空的花括号在对象解构中是合法的，你在使用嵌套解构时必须非常小心。考虑以下示例：

```javascript
let { apple: {} } = items;
console.log(apple);      // ReferenceError: apple is not defined
```

此解构语句没有绑定任何变量，由于右侧的花括号，`apple`并不会被创建为变量绑定，而是作为`items`对象内部的一个位置进行探查。在这种情况下，如果你想要为变量`apple`创建绑定，应该使用`=`来定义默认值，而不是使用`:`来定义位置，例如：

```javascript
let { apple = {} } = items;
console.log(apple);      // {quantity: 5, value: 25}
```

我们再看一个使用嵌套数组解构的例子。你可以在整体模式中插入另一个数组模式，解构操作就会深入到嵌套数组中：

```javascript
let fruits = [ "apple", [ "blueberry", "raspberry" ], "orange" ];
let [ fruit1, [ fruit2 ], fruit4 ] = fruits;
console.log(fruit1);       // "apple"
console.log(fruit2);       // "blueberry"
console.log(fruit4);       // "orange"
```

在这个例子中，变量`fruit2`会被赋值为从第二个数组（表示`fruits`数组中的浆果）中获取的`"blueberry"`。解构语句中`fruit2`周围的额外方括号是必要的，用于解包`fruits`数组及其内部的浆果数组。与我们之前看到对象类似，你可以拥有任意深度的嵌套数组，并且需要在解构语句中使用相应的符号来从中提取值。

我们可以为对象和数组的解构构建复杂的表达式，以获取正确的值并将其分配给不同的变量，无论对象或数组嵌套多深，或者混合了对象和数组，只需借助解构的力量，使用一条语句即可完成。请看下面的例子：

```javascript
let student = {
  name: "Tony",
  courses: {
    english: {
      id: 1,
      score: 7
    },
    math: {
      id: 2,
      score: 9
    }
  },
  scoreRange: [0, 10]
};
let {
  courses: { english },
  scoreRange: [ minScore ]
} = student;
console.log(english.id);        // 1
console.log(english.score);     // 7
console.log(minScore);          // 0
```

这段代码将`student.courses.english`和`student.scoreRange[0]`分别提取到`english`和`minScore`中。在这个例子中，`courses`和`scoreRange`都是解构模式中的变量，它们从`student`对象中的相应属性获取值。当你需要从复杂的 JSON 结构中获取某些特定数据或值，而无需遍历整个结构时，这种方法非常有用。

### 使用剩余语法进行解构

在解构模式中使用剩余运算符，可以成为 ES6 中变量赋值的一种非常强大且简洁的语法。我们已经在上一章中探讨了剩余模式。现在，让我们看看它与解构模式结合使用的例子。请看下面的示例：

```javascript
var num1, num2, rest;
var x, y, z;
[num1, num2, ...rest] = [1, 2, 3, 4, 5];
[x, y, z] = [1, 2, 3, 4, 5];
console.log(num1);        // 1
console.log(num2);        // 2
console.log(rest);        // [3, 4, 5]
console.log(x, y, z)      // 1 2 3
```

在上面的例子中，我们使用解构将值赋给变量`num1`、`num2`和`rest`（仅对`rest`使用了剩余运算符）。我们对另一组变量`x`、`y`和`z`执行了完全相同的操作。但如果你注意到输出结果，当我们使用包含五个数字的数组赋值时，前两个值被赋给了`num1`和`num2`，但其余的值被收集到一个数组中并赋给了变量`rest`，因为我们对其使用了剩余运算符。将此与第二个解构语句进行对比，在第二个语句中，`z`只被赋值为`3`。

这里需要重点注意的一点是，如果在左侧对剩余元素使用了尾随逗号，则会抛出`SyntaxError`。请看以下示例：

## 解构参数

解构语法除了为我们提供更简单的声明和赋值语法外，还可以在传递函数参数时使用。前面讨论过的所有解构变体，在参数解构中同样适用。解构还与其他 ES6 函数参数特性（如默认参数值和剩余参数）很好地融合在一起。

让我们看一个使用解构数组参数的简单示例：

```javascript
function sum( [ num1, num2 = 0 ] ) {
  console.log( num1 + num2 );
}
sum( [ 1, 2 ] );         // 3
sum( [ 1 ] );            // 1
sum( [  ] );             // NaN
```

在上面的例子中，有几点需要注意：当向`sum()`函数传入包含两个值`1 和 2`的数组时，它会打印它们的和`3`；但当只传入一个值`1`时，由于`num2`被分配了默认值`0`，它会输出`1`。当没有向解构数组传入任何值时，它会打印`NaN`（Not a Number），因为尽管`num2`默认为`0`，但`num1`被设置为`undefined`。

当你需要为函数提供大量参数时，一种常见的模式是创建一个对象，其属性指定参数，然后将该对象传入函数。但这种模式的问题在于，仅通过查看函数定义，很难猜测函数需要什么样的输入。你需要遍历函数体才能了解函数所需的不同输入。请看下面这个函数接受选项对象的示例：

```javascript
function createObj(name, value, options) {
  let obj = {};
  obj.x = options.a;
  obj.y = options.b;
  obj.z = options.c;
  obj[name] = value;
  return obj;
}
let options = {a: 1, b: 2, c: 3};
let testObj = createObj('test', 4, options);
console.log(testObj)// {x:1, y:2, z:3, test:4}
```

这里，`options`对象是函数`createObj`所需的参数之一，但解构可以帮助我们更好地读取和获取函数内部所需的 options 属性。我们可以使用解构参数重写函数定义，如下所示：

```javascript
function createObj(name, value, {x:a, y:b, z:c}) {
  // 返回对象的代码
}
```

这个函数的行为与上一个示例完全相同，但由于第三个参数使用解构从`options`对象中获取相应的值，因此仅通过查看函数定义就能轻松理解函数所需的输入。而且，这些值甚至可以在函数调用语句本身中直接赋值。因此，你可以使用默认值、混合的对象和数组模式，以及与你读取的属性不同的变量名。

**注意**：解构参数的行为与常规参数类似，如果没有传入，它们会被设置为`undefined`。

使用对象解构参数的能力还为我们带来了额外的好处：它们使我们能够在参数列表中的任何位置拥有可选参数。请看下面的示例：

```javascript
function printNums( { num1, num2 } ) {
  console.log( num1, num2 );
}
printNums( { num2: 1, num1: 2 } );           // 2 1
printNums( { num2: 42 } );                   // undefined 42
```

在此示例中，`printNums()` 函数将两个数字打印出来，这些数字是通过解构包含 `num1` 和 `num2` 属性的对象作为参数传入的。请注意，在第一次函数调用中，你可以通过先指定 `num2` 再指定 `num1` 来交换参数的顺序。类似地，通过只指定 `num2`，实际上你可以将第一个预期参数 `num1` 的值设置为 `undefined`。

此时，有必要指出解构默认值和函数参数默认值在行为上的区别。请看以下示例：

```javascript
function foo({ num1 = 42 } = {}, { num2 } = { num2: 42 }) {
  console.log( num1, num2 );
}
foo();                               // 42 42
foo( {}, {} );                       // 42 undefined
```

在此示例中，如果不看输出结果，可能看起来我们以两种不同方式为 `num1` 和 `num2` 都声明了默认值 `42`。第一次函数调用也表明了这一点。但第二次函数调用——向函数中传入了两个空对象——则揭示了一个不同的情况。

如果仔细观察，这两种声明参数值的方式存在细微差别。在上面的例子中，函数 `foo` 接受两个参数：

*   `{ num1 = 42 } = {}`

*   `{ num2 } = { num2: 42 }`

当没有参数传给函数时，第一个参数默认为一个空对象 `{}`，但由于它使用了对象解构，因此无论在什么情况下，只要传入的参数对象中没有 `num1` 这个属性，命名参数 `num1` 都会默认为 `42`。

对于第二个参数，它的默认参数值是一个对象 `{ num2: 42 }`。当没有提供第二个参数给函数时，该参数会默认使用这个对象。由于左侧是解构对象，当参数默认使用该对象时，`num2` 被设置为 `42`。但是，如果将一个空对象 `{}` 作为第二个参数传入，则默认值永远不会被使用，然后对传入的空对象 `{}` 进行 `{ num2 }` 解构，导致 `num2` 被设置为 `undefined`。

## 总结

随着你在 JavaScript 中处理数据，你会逐渐意识到通过分解对象和数组来逐个检查其组成部分以提取特定信息的必要性。在 ES6 之前，开发者需要编写相当多的代码来解构数据，但现在他们可以使用 ES6 带来的新解构语法，就所需编写的代码量而言，这个过程变得简单得多。

此外，解构有助于使函数的对象参数更加明确。使用解构，你可以拥有默认值而无需检查属性是否存在于对象中，并且现在我们可以通过为属性提供默认值来使可选属性更加明确。显式可见的内容使代码更易于维护，因为你无需猜测或阅读整个函数就能弄清楚。