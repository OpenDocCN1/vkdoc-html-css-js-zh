# 对象字面量的扩展

ES6 为对象字面量引入了一些新的扩展特性。

要声明对象字面量，目前使用变量时我们不得不采用以下编码模式：

```javascript
var price = 4.20, quantity = 20;
var invoiceData = {
price: price,
quantity: quantity
};
console.log(invoiceData);
```

同时声明 `price` 和 `quantity` 两次有些冗余，但现在 ES6 提供了一种简写形式，让写法更简洁。请看下面的示例：

```javascript
const price = 4.20, quantity = 20;
const invoiceData = {
price,
quantity
};
console.log(invoiceData);
```

我们只需列出字段一次，ES6 的对象字面量就能智能识别出我们想要一个名为 `price` 的字段，并将其上下文数据设置为同名变量的值。这种简写语法不仅减少了代码量，也提高了可读性。

ES6 还提供了一种在对象字面量中编写函数的简写形式。例如，请看下面的代码片段：

```javascript
const price = 4.20, quantity = 20;
const invoiceData = {
price,
quantity,
calculateTotal() {
return this.price * this.quantity;
}
};
console.log(invoiceData.calculateTotal());
```

在上述简写形式中，你不再需要 `function` 关键字。当我们在对象字面量中使用函数简写时，`this` 指向代码的上下文，其行为与箭头函数类似。它并不指向包含该函数的对象。其行为与箭头函数完全一致。

请注意，你可以在对象字面量中使用动态字段名：

```javascript
const field = 'dynamicRandom';
const price = 5.99;
const quantity = 2;
const invoiceData = {
[field]: price,
quantity,
calculateTotal() {
return this.price * this.quantity;
}
};
console.log(invoiceData);
// { dynamicRandom: 5.99,
//   quantity: 2,
//   caculateTotal: [Function: calculateTotal] }
```

对象中的字段被命名为 `dynamicRandom`。实际上，你还可以使用完整的表达式，让属性命名更具动态性：

```javascript
const field = 'dynamicRandom';
const price = 5.99, quantity = 2;
const invoiceData = {
[field + "-01"]: price,
quantity,
calculateTotal() {
return this.price * this.quantity;
}
};
console.log(invoiceData);
// { dynamicRandom-01: 5.99,
//   quantity: 2,
//   caculateTotal: [Function: calculateTotal] }
```

你甚至可以在方法中创建动态字段名。它们也可以与 setter 和 getter 一起使用。

## 模板字面量与分隔符

JavaScript 中的字符串在历史上一直受限，缺乏其他编程语言所具备的能力。ES6 引入了模板字面量，它提供了一种定义字符串的方式，并具备以下额外功能：

* 字符串插值
* 嵌入表达式
* 多行字符串（无需技巧）
* 字符串格式化

模板字面量使用反引号（`` ` ``）而不是单引号或双引号。模板字面量最终总会生成字符串。模板字面量可以这样写：

```javascript
let user = `Kevin`;
```

模板字面量支持字符串替换，允许我们在字符串中替换任意有效的 JavaScript 表达式。模板字面量可以使用 `${ }` 语法作为字符串替换的占位符。请看以下示例：

```javascript
console.log(`Hi ${user}!`); // Hi Kevin!
```

在上述示例中，模板字面量由反引号（`` ` ``）界定，字面量内部的插值表达式由 `${` 和 `}` 界定。

我们不仅可以替换变量名，还可以替换更多内容。模板字面量允许使用表达式插值来嵌入可读的内联数学运算，例如：

```javascript
let a = 10;
let b = 20;
console.log(`Sum of ${a} and ${b} is ${a+b}`);
```

模板字面量还允许你轻松添加多行字符串（无需使用 `\n`）：

```javascript
console.log(`I am line-one
I am line-two`);
// I am line-one
// I am line-two
```

### 标签模板字面量

一种更高级的模板字面量形式是标签模板字面量。标签模板通过在模板字符串前放置一个函数名来转换模板字符串。例如：

```javascript
output`Hi, my name is ${name} and I love ${language}`;
```

这个表达式可以转换成一个函数调用，该函数接受两种参数：

1. 字面量字符串数组，即 `["Hi, my name is", "and I love", " "]`
2. 替换值，即 `name`, `language`。

字面量字符串的总数始终比替换值的数量多一。你可以将每个替换值视为被两侧的两个字面量包裹。这就是为什么上述字面量字符串数组中有一个空字符串作为最后一个元素。

因此，上述标签模板可以写成：

```javascript
output(["Hi, my name is ", " and I love ", ""], name, language)
```

标签函数通常使用剩余参数（rest arguments）来定义，以便更轻松地处理参数。上述函数可以定义为：

```javascript
function output(literals, ...substitutions) {
let result = "";
// 拼接字面量字符串和替换值
for (let i=0; i<substitutions.length; i++) {
result += literals[i] + substitutions[i];
}
// 拼接字面量数组中的最后一个元素
// 该数组总是比替换值数组多一个元素
result += literals[literals.length - 1];
return result;
}
const  name = 'John',
language = 'JavaScript';
let text = output`Hi, my name is ${name} and I love ${language}`
console.log(text); // Hi, my name is John and I love JavaScript
```

由于这些函数将模板字符串的各个部分作为参数接收，你可以决定如何使用这些字符串和替换值来确定最终的字符串输出。例如，你可以使用 `output` 函数将所有字符转换为大写、将输入转换为哈希字符串，或实现任何你需要的功能。

## 使用 `for...of` 进行迭代

在 JavaScript 过去的二十年里，我们一直使用 `for`、`for-in` 和 `forEach`（针对数组）进行迭代。ES6 引入了另一种结构——`for...of` 循环，它允许对可迭代对象（如 `array`、`map`、`set`、`string` 等）进行迭代。我们将在本书后面更详细地探讨迭代器和可迭代对象。下面是一个用于遍历数组的简单 for 循环示例：

```javascript
let names = ['matt', 'smith', 'jack'];
for (let i = 0; i<names.length; i++) {
console.log(names[i]);
}
// matt
// smith
// jack
```