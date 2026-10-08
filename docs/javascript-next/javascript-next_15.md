# 12. 使用模块进行命名空间管理

JavaScript 对“打包”或命名空间的回应可以相当简洁地概括——它根本不存在。这是从语言设计之初起最明显的缺失之一，并导致了各种模块化代码的尝试。这些尝试从自制的解决方案到像 `AMD` 这样的完整规范。^(⁷¹)

ES6 对这个问题的回应是模块。在本章中，我们将学习如何使用 ES6 提供的新支持来创建和使用模块。我们将准备好摒弃像 `IEFE` 这样的临时解决方案，并准备好拥抱一种一致的、与平台无关的机制，将我们的代码封装成可重用的模块并命名空间化。到本章结束时，我们不仅将对模块的微妙语法有全面的理解，还能随时在我们的代码库中与任何第三方库一起使用它。

## JavaScript 中命名空间的缺失

在 JavaScript 中，只有一个且唯一的环境，即全局作用域。我们所做的任何事情，例如声明变量或定义函数，最终都会进入这个全局作用域，从而对所有内容可见。再加上 JavaScript 是动态类型的，因此如果两个“事物”同名，即使它们不是同一类型（变量 vs. 函数），也会发生冲突，最后被求值的那一个会胜出。

恰好只有一个构造可以引入作用域，那就是函数。^(⁷²) 我们开发者迅速抓住了这个想法，将其用作在我们的代码和全局作用域之间创建缓冲区的机制，几乎所有关于创建模块的想法都利用了这一点。

我们在之前的章节中已经讨论过 `IEFE` 的概念，但为了唤起记忆，这里是我们之前的永久记忆示例：

- ① 通过创建一个匿名函数来引入人工块
- ② 这个 `var` 将被限定在声明的函数作用域内
- ③ 通过 `return` 一个对象来暴露公共 API
- ④ 这将导致一个 `ReferenceError`
- ⑤ 你可以使用公共 API
- ⑥ 调用公共 getter

```javascript
const counter = (function createCounter() { ①
  var steps = 0; ②
  function increment() {
    steps++;
  }
  function getCount() {
    return steps;
  }
  return {
    increment,
    getCount,
  }; ③
}());
// 使用它
// console.log(steps); ④
counter.increment(); ⑤
console.assert(counter.getCount() === 1); ⑥
```

`IEFE` 与揭示模块模式一起，正好提供了我们所需的机制——一种声明“私有”成员的方法，同时允许我们向外部世界暴露 API。这是开源生态系统中大多数库采用的方法。然而，虽然这似乎解决了*生产*封装构造的问题，但它对解决消费它的问题几乎没有任何帮助。

以 `jQuery` 为例。`jQuery` 将其所有功能封装在一个 `IEFE` 中，并向全局作用域暴露了一个对象，即 `$`。在我们的应用程序代码中，如果我们想使用 `$`，我们应该怎么做呢？嗯，我们必须确保 `jQuery` 库*首先*被加载，这会将 `$` 引入全局作用域，然后我们才能愉快地在代码库中使用它。

这就是说，库的加载顺序很重要！如果我们重新排列库的顺序，这个纸牌屋就会倒塌。

这里的核心问题是我们使用全局作用域来共享代码，这使得代码库不同部分之间的相互依赖关系变得*隐式*，而不是显式。更不用说，如果两个或多个“导出的”变量同名，我们很容易发生冲突。当加载了许多迷你库，并且它们之间存在相互依赖关系时，这个问题会进一步加剧——试图确定加载顺序，同时避免冲突，很快就变成了一场俄罗斯轮盘赌。

总结一下，`IEFE` 和揭示模块模式可以帮助封装，但它们不对所提供的内容进行命名空间化，将避免冲突的责任留给了使用者。另一方面，消费这些库需要精心地按正确顺序加载库，因为唯一的共享上下文是全局作用域。

在我们探讨这个问题的解决方案之前，我们必须考虑 JavaScript 的另一个方面，那就是 JavaScript 不再仅仅是一种客户端语言。开发者们已经利用 `Node` 等技术在服务器上拥抱了 JavaScript，^(⁷³) 与客户端脚本相比，它有一套完全不同的参数。

考虑一下 JavaScript 在浏览器中是如何加载的。这是通过 `script` 标签完成的，这要求浏览器从服务器请求一个资源。不用说，我们不想同步执行此操作，因为它会阻塞主线程，所以这种加载通常是异步完成的。将此与服务器端编程对比。当代码库的某一部分“require”另一部分时，该脚本可以同步加载，因为它就位于那里——磁盘上，无论是在同一个项目中，还是在 `node_modules` 文件夹中。

同样，这里的问题是按正确顺序加载我们需要的库，并使它们的公共成员可供我们使用，而不会产生冲突的风险。*如何*解决这些依赖关系是另一个问题。在接下来的章节中，我们将讨论 ES6 如何处理模块的声明和使用。然而，在开始之前，让我们看看如何运行本章中的代码片段。

## 如何运行模块化 JavaScript

本章运行代码示例的最简单方法是使用 `Node`。^(⁷⁴) 在撰写本文时，`Node` 的版本为 12 以上，并在实验性标志下支持 ES6 模块。

首先，请确保你已安装 `Node`，或者正在运行一个相对较新的 `Node` 版本。当然，如果你已在机器上安装了一个想保留的 `Node` 版本，你可以使用“Node 版本管理器”^(⁷⁵) 或“`nvm`-windows”^(⁷⁶) 来同时安装和维护多个 `Node` 版本。安装完成后，打开命令提示符并输入以下内容：

```bash
node --version
## 在我的机器上显示 'v12.4.0'
```

如果你看到有效响应，则说明 `Node` 已正确安装。

为了在 `Node` 中加载并运行 ES6 模块，有两个要求：

* 必须使用 `--experimental-modules` 标志来调用 `Node`。
* 任何需要被视为 ES6 模块的源文件都应使用 `.mjs` 文件扩展名（而不是 `.js`）。

让我们考虑一个简单的例子来让事情更清楚。`cd` 到一个临时目录，创建一个名为 `exporter.mjs` 的新文件，内容如下：

```javascript
// 文件名必须以 "mjs" 结尾 — 例如 exporter.mjs
export const sayHello = name => `Hello ${name}!!`
```

现在，在同一个目录下创建另一个文件，并将其命名为 `importer.mjs`，包含以下内容：

* ① 从 `exporter` 导入成员

```javascript
import { sayHello } from './exporter.mjs'; ①
// 打印 'Hello ES6 Modules!!'
console.log(sayHello("ES6 Modules"));
```

确保保存这两个文件，然后在控制台中输入以下内容：

```bash
node --experimental-modules importer.mjs
```

你应该会看到 `Hello ES6 Modules!!`。^(⁷⁷)

在我们的设置中，`exporter.mjs`“导出”某些成员，而 `importer.mjs` 在自身内部使用这些成员。当你按照本章的示例进行练习时，你可以随意命名你的文件，只要文件具有 `.mjs` 文件扩展名，并且你使用 `Node` 对 ES6 模块的实验性支持即可。`.mjs` 文件扩展名是 `Node` 特有的，它默认支持 CommonJS。然而，ES6 模块文件与常规 JavaScript 文件没有区别，除了它们声明了一个模块，因此*应该*使用 `.js` 扩展名。相应地，本章中的代码片段将使用 `.js` 扩展名来引用所有文件——只需在你的工作区中将它们替换为 `.mjs` 即可。

让我们来谈谈模块！

## 声明模块

ES6 中的模块就是一组相关的变量、常量、函数和类的集合。模块允许我们将代码库拆分成更小、更独立的功能单元，这些单元可以被我们代码库的其他部分使用，甚至可以发布给其他应用程序使用。模块允许我们将功能“隐藏”在命名空间中，从而消除命名冲突。

ES6 中的模块包含在文件中，其中文件和模块之间是一一对应的关系。模块的名称是文件本身的（绝对或相对）路径。默认情况下，模块在 `严格模式` 下运行，我们在第 1 章中简要讨论过这一点。模块的所有成员都隐式地为“私有”——也就是说，除非它们被显式地 `export` 导出，否则使用该模块的人无法看到它们。请考虑以下内容：

* ① 显式地 `export` 成员
* ② 这些是模块的私有成员
* ③ 导出一个包含所有公共成员的对象，类似于揭示模块模式

# 模块导出与导入

```javascript
export const degreesToRadians = d => d * (Math.PI / 180); ①
export const gradiansToRadians = g => g * 15.707 * Math.pow(10, -3);
const calc = (fn, x, unit) => { ②
switch (unit) {
case 'degrees':
return fn.call(null, degreesToRadians(x));
case 'gradians':
return fn.call(null, gradiansToRadians(x));
default:
return fn.call(null, x);
}
};
export const sin = (x, unit = 'degrees') => calc(Math.sin, x, unit);
export const cos = (x, unit = 'degrees') => calc(Math.cos, x, unit);
export const tan = (x, unit = 'degrees') => calc(Math.tan, x, unit);
export const trig = { ③
degreesToRadians,
gradiansToRadians,
sin,
cos,
tan,
};
```

这里我们声明了一组三角函数，导出那些我们认为是公共 API 的，而将其余部分保留为私有。请注意，我们还 `export` 了一个包含所有公共成员作为属性的对象，就像我们使用揭示模块模式所做的那样。这是一种有用的技术，它赋予我们的使用者选择权——他们可以挑选自己感兴趣的具体功能，或者使用整个导出的对象，并使其下的所有内容都可供他们使用。这些导出的成员，也称为“命名”导出，对任何使用此模块的人都是可见的。

逐个 `export` 每个成员可能会显得冗长，并且可能模糊了公共 API 的实际内容。有一种替代语法可以帮助解决这个问题：

* ① 定义我们所有的成员
* ② 使用替代的导出语法

```javascript
const degreesToRadians = d => d * (Math.PI / 180); ①
const gradiansToRadians = g => g * 15.707 * Math.pow(10, -3);
const calc = (fn, x, unit) => {
switch (unit) {
case 'degrees':
return fn.call(null, degreesToRadians(x));
case 'gradians':
return fn.call(null, gradiansToRadians(x));
default:
return fn.call(null, x);
}
};
const sin = (x, unit = 'degrees') => calc(Math.sin, x, unit);
const cos = (x, unit = 'degrees') => calc(Math.cos, x, unit);
const tan = (x, unit = 'degrees') => calc(Math.tan, x, unit);
const trig = {
degreesToRadians,
gradiansToRadians,
sin,
cos,
tan,
};
export { ②
degreesToRadians,
gradiansToRadians,
sin,
cos,
tan,
trig,
};
```

与我们之前逐个显式导出每个成员不同，这里我们一次性导出所有公共成员。这有助于使公共 API 更加明确——然而，虽然公共 API 当然变得更清晰了，但维护模块会稍微繁琐一些，因为必须协调文件的不同部分，同时确定哪些是公共的，哪些不是。

无论我们是逐一显式导出每个成员，还是一次性导出所有成员，这都取决于个人品味和偏好。然而，JavaScript 社区整体似乎正倾向于显式导出每个公开成员。此外，像`ESDoc`^(⁷⁸)这样的文档工具会智能地仅展示导出的成员，因此即使采用显式导出方式，公开 API 也一目了然。

我们也可以导出一个“默认”成员，这是一种表达模块导出“唯一一个”东西的方式。考虑这样一个场景：我们正在编写一个数学库，并且恰好有一个三角学模块，其中我们定义了所有三角学函数。鉴于这个模块的全部内容就是这些函数，使用默认导出是合理的。

然而，这可能会令人困惑，因为我们可以同时混合使用默认导出和显式导出其他成员，如下所示：

*   ① 显式导出我们的公开成员
*   ② 注意我们不再导出`trig`
*   ③ 将`trig`设为该模块的默认导出

```javascript
export { ①
degreesToRadians,
gradiansToRadians,
sin,
cos,
tan, ②
};
export default trig; ③
```

这里，我们使用`default`关键字来导出我们包罗万象的`trig`对象，并假设这是大多数客户端无论如何都想要使用的。然而，我们仍然可以导出单独的成员，以防客户愿意“单点”而不是“全选”。（也可以使用内联命名导出机制，或者根据需要两者结合使用。）不用说，顾名思义，只能有一个默认导出。

尽管默认导出看起来是个好主意，但它们是一种过渡机制，因为这是`CommonJs`^(⁷⁹)所使用的模型，并且其缺点也多过优点。

与命名导出（顾名思义）不同，默认导出不会将导出成员的名称暴露给外部世界。这将对模块的使用者施加责任，由他们来决定如何称呼该导出。考虑`degreesToRadians`，它恰好是一个命名导出——外部世界使用该成员时，它都使用相同的名称。如果我们把`degreesToRadians`改为`degToRad`，那么任何使用此模块的人都必须进行同样的更改。然而，作为默认导出的`trig`，并不会强制使用者将其称为`trig`。这意味着如果我们重命名`trig`，客户端将看不到这一更改，从而使重构工作变得相当繁琐。没有名称也意味着支持`IntelliSense`的编辑器无法帮助进行符号自动补全或自动导入。最后，默认导出限制了构建工具执行任何形式的摇树优化（即死代码消除）的能力。^(⁸⁰)

此外，使用命名导出可以帮助任何使用我们模块的人（可能包括我们自己）缩小生产包的大小，有助于重构等开发实践，并且，正如我们很快将看到的，还能减少导入模块时的混淆。总而言之，使用默认导出在很大程度上被认为是一种反模式^(⁸¹)，我们应该尽可能少用，甚至不用这个特性。

导出还提供了另一种功能，即别名。让我们看看这对于命名导出是如何工作的。

### 别名导出

别名导出允许我们重命名模块的成员，使得我们在模块内部引用成员的名称，与使用者看到的名称不同。如果希望将模块与其使用者完全解耦，从而让我们可以自由地重构内部名称而不影响外部世界，这可能很有价值。语法如下所示，并不令人意外：

*   ① 显式导出我们的公开成员
*   ② 同时也用别名导出我们包罗万象的对象

```javascript
// 为简洁起见，代码已缩写
export {
degreesToRadians,
gradiansToRadians,
sin as sine, ①
cos as cosine,
tan,
trig as trignometry, ②
};
```

`as`语法如预期那样工作，允许我们在“导出时”重命名成员。现在，我们可以随意以任何方式重命名内部别名的成员，而无需强制所有使用者也随之重构。

别名导出揭示了默认导出的另一个缺点，即由于默认导出是未命名的导出，为它们设置别名是没有意义的。

### 快速小结

总结一下，导出意味着将模块的成员设为公开，也就是说，模块的使用者可以访问它们。我们看到了两种不同的导出机制——命名导出（可以内联，因此是逐个导出）与一次性导出所有符号。我们还学习了默认导出的语法，并讨论了使用这种方法的某些缺点。

现在让我们转向等式的另一边，即导入或消费一个模块。

## 导入

ES6 引入了 `import` 关键字，允许一个模块导入另一个模块。只有模块才能使用其他模块——普通的旧式 JavaScript 文件无法消费模块。此外，请记住，模块与文件存在一一对应的关系，即一个文件包含且仅包含一个模块，而该模块的名称就是文件本身的名称。

`import` 语法由两部分组成——模块的名称以及要从该模块中导入的内容。指定模块就是简单地指定模块位置的路径。这里有趣的部分是实际导入已导出成员的机制，所以我们来讨论一下。

如何导入另一个模块的成员，取决于这些成员最初是如何导出的。我们知道，一个模块可以导出命名成员和默认成员。我们从如何使用命名成员开始讲起。

消费一个具有命名导出的模块，会强制要求消费方使用与导出成员相同的名称。考虑以下示例：

* ① 导入此模块所需的成员

# ES6 模块的导入与导出

## 基本导入语法

```javascript
/*
'030-named-exports' 导出了以下内容
export {
degreesToRadians,
gradiansToRadians,
sin,
cos,
tan,
trig,
}
*/
import {
sin,
cos,
tan,
} from './030-named-exports';
// 使用这些函数
console.log(sin(0));
```

关于如何从另一个模块导入我们感兴趣的片段，有几点需要注意。`import` 语法看起来与我们之前在章节中看到的结构赋值语法非常相似。我们还能够挑选我们感兴趣的成员，因此允许只导入某些成员，尽管该模块可能导出更多内容。

请记住，成员的名称就是模块中导出的名称。因此，如果导出的成员被起了别名，那么我们必须导入这些别名：

* ① 在导入中使用别名

```javascript
/*
'050-alias-exports' 导出了以下内容
export {
degreesToRadians,
gradiansToRadians,
sin as sine,
cos as cosine,
tan,
trig as trignometry,
};
*/
import {
sine,
cosine,
tan,
trignometry,
} from './050-alias-exports';
```

我们依赖的模块为其成员起了别名；因此，对于任何消费方模块而言，这些就是公共名称，因此也是唯一可用于导入的名称。

## 使用通配符导入所有成员

如果我们想导入另一个模块的所有导出成员呢？`import` 语法支持通配符（`*`），但有一个限制——导入必须像此处所示命名空间化：

*导入命名成员*

* ① 使用通配符一次性导入所有导出的成员
* ② 归入命名空间下的成员，使用其公共名称即可访问

```javascript
/*
'050-alias-exports' 导出了以下内容
export {
degreesToRadians,
gradiansToRadians,
sin as sine,
cos as cosine,
tan,
trig as trignometry,
};
*/
import * as trigFns from './050-alias-exports';
console.log(trigFns.sine(0));
```

使用通配符会增加名称冲突的可能性，尤其是在导入大量其他模块时。`as` 命名空间试图消除这种可能性。相应地，如果我们不小心导入了两个同名但不同的模块，将会收到一个错误。

## 别名导入

有可能一个或多个被导入的模块导出了同名的成员；在这种情况下，我们需要一种机制来解决名称冲突。当我们使用通配符语法时，这不是问题，因为导入从一开始就是命名空间化的。然而，如果我们正在导入模块的单个成员，并且必须解决冲突，我们可以像给导出起别名一样给导入起别名：

* ① 为导入起别名

```javascript
/*
'030-named-exports' 导出了以下内容
export {
degreesToRadians,
gradiansToRadians,
sin,
cos,
tan,
trig,
}
*/
import {
sin as sine,
cos as cosine,
trig as trignometry,
} from './030-named-exports';
```

为导入起别名，除了有助于避免冲突之外，还允许我们“重命名”那些可能过于冗长的函数，或者使它们更好地适应我们正在处理的领域。

## 默认导出的导入

默认导出再次证明是个“问题儿童”。由于默认导出是未命名的导出，消费方模块可以任意命名该默认导出！

* ① 导入默认导出并赋予一个任意名称
* ② 像使用任何其他成员一样使用默认导入

```javascript
/*
'040-default-exports' 导出了以下内容
export {
degreesToRadians,
gradiansToRadians,
sin,
cos,
tan,
};
export default trig;
*/
import myTrignometry, {
sin,
cos,
tan,
} from './040-default-exports';
console.log(myTrignometry.sin());
```

通常，当模块导出默认成员时，由于导入的名称并非强制规定，同一项目中的不同消费方可能会使用不同的名称来引用默认导出，这使得查找某个成员在何处被使用变得异常困难。这迫使处理此类导出的开发者们必须约定一个命名规范，试图让命名与命名导出保持一致。^(⁸²)

## 混合导入风格

导入的语法，与导出的语法类似，相当灵活，我们可以混合搭配任何及所有导入组合。例如，如果我们愿意，我们可以在导入默认导出的同时，对其他命名成员使用别名：

* ① 用任意名称导入默认导出
* ② 用别名导入命名成员
* ③ 像往常一样使用这些导入

```javascript
/*
'040-default-exports' 导出了以下内容
export {
degreesToRadians,
gradiansToRadians,
sin,
cos,
tan,
};
export default trig;
*/
import myTrignometry, {
sin as sine,
cos as cosine,
tan,
} from './040-default-exports';
console.log(myTrignometry.sin());
console.log(sine(45));
```

导入风格的主要驱动力仍然取决于模块如何导出其成员。与此同时，在特定模块中为导入起别名等功能，*可能* 是出于诸如避免名称冲突或试图让导入的函数更适合当前领域等考虑。然而，我们必须努力避免出于风格化的心态而使用这些功能，以免给任何试图维护代码库的人带来过多负担。

## 注意事项与影响

ES6 模块的语法设计基于导出和导入是“静态”的这一原则——也就是说，它们必须是模块文件中的顶级语句。我们**不允许**动态地或有条件地导出或导入成员。^(⁸³)

静态模块有若干好处，包括更快的查找速度、由于确定性死代码消除而生成的更小的打包文件（如果我们使用像 `Webpack` 或 `Rollup` 这样的打包工具）。你可以在这篇由 Dave Herman 撰写的优秀博文（ [`http://calculist.org/blog/2012/06/29/static-module-resolution/`](http://calculist.org/blog/2012/06/29/static-module-resolution/) ）中阅读更多关于此方法好处的内容。

# 总结

呼！谁能想到把我们的代码打包成易于消费的形式会这么棘手？虽然`ES6`引入的模块语法看起来有些过度设计，但我们必须记住，JavaScript 是一种横跨浏览器和服务端两端的语言。因此，我们最好牢记，为模块添加的任何语法都需要兼容这两种运行环境。

在下一章中，我们将探讨代理（`Proxy`），它能让我们在不实际修改对象本身的情况下，神奇地为新对象和现有对象添加额外的功能。

脚注 1 2 3 4 5 6 7 8 9 10 11 12 13
