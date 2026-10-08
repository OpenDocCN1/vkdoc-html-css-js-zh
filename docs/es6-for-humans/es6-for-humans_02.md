# 1\. ES6 入门指南

如今，全球约有 46%的人口能够接入互联网，并且这一数字还在稳步增长。当你阅读这段文字时，世界各地正有人通过互联网服务预订出租车、航班，或是在高档餐厅预定双人桌；关键在于，如今的互联网已让人们的生活变得更加便捷。每天都有越来越多的 Web 应用被创建出来，旨在节省人们的时间、提供更高标准的舒适度，或仅仅是为了娱乐。这些应用可以随时随地访问，其中许多都是使用`JavaScript`构建的。

根据`StackOverflow`开发者调查，`JavaScript`是地球上最常用的编程语言。Web 平台的普适性正推动开发者比其他任何语言都更多地使用`JavaScript`，这进而催生了像`Node.js`这样的`JavaScript`平台，像`Angular`和`React`这样的前端框架，以及像`MEAN`（`MongoDB`、`ExpressJS`、`AngularJS`和`NodeJS`）这样的完整 Web 技术栈。

本书将讨论`ES 2015`（通常被称为`ES6`）及之后版本的核心概念，同时聚焦于最佳开发实践。当你读完本书时，你将深刻理解核心`ES6`的概念、特性及其在现代开发工作流中的适用性。

## ES6 规范

多年来，`ECMAScript 6`（`ES6`）一直在研发中。但在 2015 年 6 月 17 日，第 109 届`Ecma`全体大会批准了第 6 版`ECMA-262`标准，正式对我们所熟知并热爱的`JavaScript`进行了一次重大升级。

`JavaScript`，更具体地说是`ECMAScript`的演进，主要由社区驱动，而`TC39`是负责此事的官方委员会。`TC39`主要由代表各方利益的成员组成，例如各大浏览器公司及其他受邀成员。他们定期会面与沟通，负责维护和升级`ECMAScript`编程语言及其扩展其能力的库的标准。在此过程中，他们审议和评估关于补充和附加技术以演进`ES`的提案。如果你有兴趣了解更多或查看他们会议的议程和纪要，这些内容都在 GitHub 上公开（`https://github.com/tc39/tc39-notes`）。

`ECMAScript`标准的设计流程围绕提案展开，这些提案通常是开发者社区对新特性或现有特性升级的热门请求。由于`TC39`参与者众多，进行协作设计工作较为困难，因此通常会指派一到两名委员会成员作为“负责人”，负责维护一个提案、进行设计工作并向委员会汇报。

一个提案要成为标准，必须经过多个阶段。提案的初始特性草图，也被称为“稻草人提案”，是委员会讨论的第一阶段；如果委员会认同其重要性，它将被视为正式提案。随后，该提议的特性至少需要在两个主要的`JavaScript`引擎中实现，以收集社区反馈并进一步演进提案。一旦提案通过这些阶段并整合了反馈，`TC39`便会批准它，并将其纳入新版的`ECMAScript`标准中。

## ECMA、ECMAScript 和 JavaScript 的历史

对于不了解`JavaScript`历史的人来说，看到诸如`JavaScript`、`ECMAScript`、不同的版本号，以及其他流行的分支如`ActionScript`、`JScript`和`TypeScript`等众多不同名称时，会很快变得非常困惑——而这些其实都是`JavaScript`的不同形式。

`JavaScript`最初由布兰登·艾奇（`Brendan Eich`）开发，作为`Netscape`浏览器中使用的 Web 脚本语言。由于当时`Java`日益流行，出于营销原因选择了这个名字，尽管它与`Java`毫无关系。为了将语言和规范标准化，它被提交给了`ECMA International`——一个专注于信息和通信技术及消费电子产品标准化的机构。最终，在`ECMA-262`中标准化的语言被简称为`ECMAScript`或`ES`，因为`JavaScript`是`Sun`公司的商标，现在归`Oracle`所有。尽管如此，大家仍然普遍称这种语言为`JavaScript`。在以`ECMAScript`为核心的不同实现中存在许多变体，例如微软为`Internet Explorer`稍作修改后采用的`JScript`。另一个衍生的例子是`Adobe`开发的`ActionScript`。

最初的版本`ES1`和`ES2`分别于 1997 年和 1998 年发布。但在 1999 年，`ES3`版本是一次重大升级，引入了诸如正则表达式、改进的字符串处理、更多控制语句、更好的错误处理以及`try catch`异常处理等许多我们今天在`JavaScript`中常用的增强功能。它在各大主流浏览器和引擎中有着广泛的基础实现形式。

在`ES3`发布后，`ES4`的工作进展顺利，但涉及许多根本性的变革和庞大的范围。更新的特性包括新语法、模块、类、经典继承、私有对象成员、可选类型注解等。提议的变更导致了社区内各利益相关方在技术和政治上的诸多分歧，最终导致其在 2003 年被搁置。部分提议的特性被纳入到像`ActionScript`和`Jscript.NET`这样的实现中。在收到这些实现的反馈后，`TC39`于 2005 年决定恢复`ES4`的工作。但此时社区内部已经出现巨大分裂，形成了两个主要阵营，在前进方向上存在分歧。

由微软和雅虎等公司支持的替代版本，在对现有规范进行较少特性添加和改进方面，被称为`ECMAScript 3.1`。由于阵营之间未能达成共识，且`JavaScript`的未来充满不确定性，数年内没有取得重大进展。最终在 2008 年，`TC39`在`ES4`和`ES3.1`之间达成了共识。`ECMAScript 3.1`最终被标准化为`ECMA-262`的第五版，也称为`ECMAScript 5`，而为了避免混淆，委员会从未发布过`ECMAScript 4`标准。`ES5`将是一个小规模的增量更新，同时他们将致力于下一个主要版本，该版本将比`ES4`更为温和，放弃了诸如包、命名空间和早期绑定等许多提议特性。由于此次会议的性质，这个提议的版本被代号为`ES Harmony`。

`ES5`于 2009 年 12 月发布，是目前现代浏览器中支持最广泛的版本。它带来了对标准库的许多增强，并通过严格模式更新了语言语义。

当`ES Harmony`的计划对于一个单一版本来说显得过于雄心勃勃时，他们将优先级最高的第一批特性拆分出来，并代号为`ES.Next`，以避免重蹈`ES4`的覆辙而使用版本号进行草率命名；一旦成熟，该规范便被命名为`ECMAScript 6`。

`ES6`经过漫长的时间才正式定稿。`ES6`提案的截止日期是 2011 年 5 月，此后不再考虑任何重大提案；但从后续版本开始，`TC39`决定采用时间箱式发布，每年发布一个包含增量小改动的新版本，使用当时已获批的任何特性。因此，委员会决定更改版本的命名约定，以表明发布年份。因此，`ES6`的官方名称在最终发布前改为`ECMAScript 2015`，但`ES6`这个名称多年来被广泛使用，以至于`ES2015`仍然常被称为`ES6`，这也是我们在本书中始终将新规范称为`ES6`的原因。但展望未来，`ES`版本将正式以其发布年份命名。所以当我们说`ES6`时，我们指的是`ES2015`中更广泛的改动。

从宏观角度来看，`ES6`规范草案分为四个主要部分：目标、要求、方法和主题。目标旨在修复`JavaScript`中的常见缺陷并添加新特性，而要求则指出这两点必须在不破坏任何现有代码的同时，保持语言的轻量级特性。

我们不会在此深入探讨所有目标。但你应该知道，这些目标旨在使其成为更适合编写复杂应用程序、库等的语言。`ES6`保持版本控制的简单和增量性，避免了版本分裂；“单一`JavaScript`”哲学对此做了最好的描述。例如，在`ES6`中，一切都是`ES6`代码：不存在仅针对`ES5`的部分。

`ES6`旨在为大型应用程序和库的创建提供更好的支持。它提供了诸如类、模块、词法块作用域、迭代器、生成器、对`Promise`的原生语言支持等增强功能。

`ECMAScript`标准的开发是由社区驱动的，该语言的特性要求在未来版本中将持续演進，以便做得更好。

## 单一 JavaScript

原则上，当语言实现的新版本出现时，这是一个清理旧有过时特性、为更新更好的实现让路的机会。这导致了版本分裂，这基本上意味着每段代码都需要关联到语言的特定版本。例如在`Python`中，要在`Python 2`和`Python 3`之间切换，就需要迁移代码库。但这种选择对于 Web 和像`JavaScript`这样的语言是不可行的。你总会遇到互联网上遗留的旧代码块。

因此，`ES`规范致力于升级语言并避免版本分裂。这是通过始终保持向后兼容来实现的，也就是说，所有有效的`ES5`代码也是有效的`ES6`代码。因此，`ES6`被设计为没有任何破坏性的更改，并且不会移除任何以前的特性。所以理论上，引擎不需要指定版本。因此，与其移除现有特性，不如引入新的、更好的特性。

这对你，一位 JavaScript 开发者，意味着什么？由于 ES6 是 ES5 的超集，你将无需迁移任何旧代码。你现有的 JS 代码同样是有效的 ES6 代码。因此，重申一遍，你无需做任何事。因此，你在现有 JavaScript 中学到并使用的所有东西都可以迁移到 ES6，并且它有更多更好的工具可以添加到你的现有技能库中。如果你希望逐步将当前系统移植到 ES6，这将非常有用，因为一切都是向后兼容的。

### 使用 ES6

许多 JavaScript 环境，包括 Web 浏览器和 Node.js，都在积极致力于实现 ECMAScript 6 及后续版本的所有特性。但要在整个互联网上普遍支持 ES6 还需要一些时间。在编写本书时，最新版本的 Chrome 已实现 97% 的特性，Safari 已实现 100% 的特性。你可以通过 [`http://kangax.github.io/compat-table/es6/`](http://kangax.github.io/compat-table/es6/) 查看所有引擎当前按特性划分的支持情况。

在所有 ES6 特性得到普遍支持之前，我们需要一种将 ES6 转换为兼容 JavaScript 代码的方法。这就是转译 (transpiling) 的作用所在。转译（转换 + 编译）是一种技术，我们使用特殊工具将 ES6 代码转换为其最接近的等价物——ES5 代码，以便在旧版浏览器或环境中运行。

考虑以下 ES6 代码：

```
const fruits = ["apples", "bananas", "oranges"];
let store = {
fruits
};
store.fruits; // ["apples", "bananas", "oranges"]
```

它大致转译成：

```
var fruits = ["apples", "bananas", "oranges"];
var store = {
fruits: fruits
};
store.fruits; // ["apples", "bananas", "oranges"]
```

一些 ES6 特性只需使用 polyfill 或 shim 即可工作，这些是简单模式，用于在旧环境中定义新行为。你也可以通过 ES6 REPL（如 [`https://jsfiddle.net/`](https://jsfiddle.net/) 和 [`babeljs.io/repl/`](http://babeljs.io/repl/)）在浏览器中在线运行和转译小段 ES6 代码。但对于大型项目，你需要使用任何可用的转译器。我们推荐使用 Babel，它是当今最流行的 JavaScript 转译器之一。

### 使用 Babel 和 webpack 搭建 ES6 环境

为了在今天的项目中使用 ES6，你需要一套构建工具来启动和运行。在本节中，我们将讨论一些构建工具，并搭建一个 ES6 样板项目，你可以将其作为 ES6 项目的入门套件。

#### 使用 Babel 进行转译

Babel.js 是一个很棒的工具，它能让你将 ES6 代码转译成 ES5 代码，然后可以在当前的 JavaScript 环境中运行。Babel 通过语法转换器支持最新版本的 JavaScript，这些插件允许你使用新的语法，而无需等待浏览器支持。

第一步是使用 npm 在你的本地机器上安装 Babel。

注意

在继续之前，请确保你已经安装了 Node.js 和 npm。如果你尚未安装 node 和 npm，可以访问 [`https://nodejs.org/en/`](https://nodejs.org/en/) 来搭建你的开发环境。

```
npm install -g babel-cli
```

现在，你可以从命令行使用以下命令运行任何包含 ES6 代码的文件：

```
babel-node 
```

但是，手动转译每个文件效率不高，并且在大多数情况下，不是管理大型项目的解决方案。所以让我们搭建一个 ES6 入门套件，它将帮助你自动化构建过程，并使开发过程更高效。

### 搭建 ES6 样板项目

为了转译一个 ES6 项目，我们将使用 `babel-loader` 和 `webpack`，它们将帮助我们生成一个包含所有转译代码及相关依赖的捆绑输出。

#### 启动一个新项目

在你的终端中运行以下一组命令来启动一个新项目：

```
mkdir es6-boilerplate
cd es6-boilerplate
npm init –yes
```

`npm init` 创建一个具有其自身 `package.json` 的新项目，`–yes` 标志阻止 `npm` 提示你任何选项，并将使用默认设置。

## 安装 webpack 和 webpack-dev-server

Webpack 是一个非常灵活的模块打包工具，它可以接收带有依赖关系的模块，并生成代表这些模块的静态资源。我们将使用 webpack 让 `babel-loader` 把我们的 ES6 代码转译为传统的 ES5 代码，并生成一个打包后的输出文件。

```
npm install –-save-dev webpack
```

除了 webpack，我们还需要使用 `webpack-dev-server` 来运行我们的应用并即时转译代码。但请注意，`webpack-dev-server` 是一个开发服务器，不应在生产环境中使用。

```
npm install –-save-dev webpack-dev-server
```

**注意：** 想更详细地了解 webpack 和 `webpack-dev-server`，可以访问 [`webpack.js.org/concepts/`](https://webpack.js.org/concepts/)。

## 在项目中安装 Babel

使用以下 npm 包，你可以非常轻松地将 babel 安装到你的项目中：

```
npm install --save-dev babel-loader babel-core babel-preset-es2015
```

下一步是通过在项目根目录下添加一个包含以下 JSON 内容的新文件 `.babelrc` 来配置 babel 使用 ES2015 预设：

```
{
"presets": ["es2015"]
}
```

你的 `package.json` 文件应该大致如下所示：

```
{
"name": "es6-boilerplate",
"version": "1.0.0",
"description": "ES6 Boilerplate",
"devDependencies": {
"babel-core": "⁶.24.1",
"babel-loader": "⁶.4.1",
"babel-preset-es2015": "⁶.24.1",
"webpack": "².4.1",
"webpack-dev-server": "².4.2"
}
}
```

现在，在你的项目根目录下创建一个新的 `index.html` 文件和 `index.js` 文件。你当前的项目目录应该如下所示：

```
.
├── index.html
├── node_modules
├── package.json
├── index.js
```

## 配置 Webpack

下一步是通过在项目根目录下创建一个配置文件 `webpack.config.js` 来设置 webpack。

一个 webpack 配置文件是一个 CommonJS 风格的模块，它从中导出一个配置对象。

```js
// webpack.config.js
module.exports = {
entry: './index.js',
output: {
path: './dist',
filename: 'bundle.js'
}
};
```

这里，`entry` 是你的项目源文件的路径，webpack 会分析你的入口文件以获取依赖关系，并生成一个打包后的输出（包含所有依赖模块）。请注意，启动时只会执行入口模块。

## 添加加载器

加载器让你可以在加载文件时对它们进行预处理。加载器提供了一种强大的方式来处理前端构建步骤，并且可以将文件从不同的语言（如 CoffeeScript）转换为 JavaScript，或将内联图像转换为数据 URL。例如，`babel-loader` 使用 Babel 来加载 ES2015 文件。

所以现在，修改 `webpack.config.js` 以使用 `babel-loader` 处理所有 `.js` 文件：

```js
module.exports = {
entry: './index.js',
output: {
path: __dirname + '/dist',
publicPath: '/dist/',
filename: 'bundle.js'
},
module: {
rules: [{
test: /\.js$/,
exclude: /node_modules/,
use: 'babel-loader'
}]
}
};
```

上述配置包含以下含义：

1. `./index.js` 是应用的入口点。

2. 输出将生成在 `./dist/bundle.js`。

3. 我们使用 `babel-loader` 处理每一个 `.js` 文件，并排除 `node_modules`，以避免外部库经过 Babel 处理，从而拖慢编译速度。

## 将你生成的 `bundle.js` 脚本添加到你的 `index.html`

现在我们可以将 `bundle.js` 脚本包含到我们的 html 文件中以运行代码。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>ES6 Boilerplate</title>
</head>
<body>
    <h1>ES6 Boilerplate</h1>
    <p>查看控制台了解详情</p>
    <script src="./dist/bundle.js"></script>
</body>
</html>
```

要编译你的 `.js` 文件，你可以在终端中运行以下命令：

```
webpack
```

你也可以将 `webpack` 与以下附加标志一起使用：

* `webpack` 用于为开发环境构建一次。
* `webpack -p` 用于为生产环境构建一次（压缩）。
* `webpack -w` 用于开发环境中的持续增量构建（快速！）。
* `webpack -d` 用于包含 source map。

`专业提示：` 使用 `webpack --progress --colors` 可以美化输出，它会在终端的 webpack 输出中添加一个进度条和颜色。

## 设置开发服务器

要启动开发服务器来测试你的代码，你可以运行以下命令：

```
webpack-dev-server -d --progress --colors
```

这会在 `localhost:8080` 上绑定一个小的 node-express 服务器，它提供你的静态资源以及（自动编译的）打包文件。当打包文件被重新编译时，它会自动更新浏览器页面。

我们还可以将这些 webpack 命令添加到你的 `package.json` 的脚本中。

```json
"scripts": {
"start": "webpack-dev-server --hot --inline",
"watch": "webpack -w -d",
"build": "webpack -p"
},
```

现在，你可以使用这个：

* `npm start` 在 `localhost:8080` 上运行一个开发服务器，并监听/重新编译更改。
* `npm run watch` 仅在你自己的 web 服务器上监听/重新编译更改。
* `npm run build` 生成一个压缩的、可用于生产环境的构建。

让我们用 `index.js` 文件中的一个 `helloWorld()` 函数来试试这个：

```js
const helloWorld = () => {
console.log("Hello! We are all set!");
console.log("Arrow functions are working");
};
helloWorld();
```

为确保一切运行正常，你可以在终端中运行 `npm start`，并检查服务器是否在 `http://localhost:8080` 上运行。

你可以运行 `npm run watch` 来查看位于 `dist` 目录下的 `bundle.js` 文件中的转译代码，其中将包含如下这段转译后的 ES5 代码：

```js
function(module, exports) {
"use strict";
var helloWorld = function sayHello() {
console.log("Hello! We are all set!");
console.log("Arrow functions are working");
};
helloWorld();
}
```

最后，我们还建议在项目中使用 ESLint，以在编写代码时遵循最佳实践并避免错误。你可以在 [`github.com/metagrover/ES6-boilerplate`](https://github.com/metagrover/ES6-boilerplate) 找到上述样板代码，该仓库还包含一个 `.eslintrc` 文件，其中包含我们使用的标准规则和配置。

## 摘要

JavaScript 是当今 Web 上最强大的语言之一，并且其能力还在不断增强。`ES6`（官方名称为`ES2015`）及其后续版本为 JavaScript 带来了全新的范式。正如我们将在后续章节中看到的，对于传统 JavaScript 开发者而言，存在大量新内容。但对于现有 JavaScript 开发者来说，了解这些新特性至关重要，以便在越来越多特性成为浏览器和框架世界主流时保持领先。

JavaScript 的发展速度比以往任何时候都快，而转译器与填补/垫片（shims/polyfills）是助你始终站在语言发展前沿的重要工具。`ECMAScript 6` 为该语言带来了非常广泛的新增功能，其中每个特性都经过社区的仔细考量、深入讨论并被选定为标准的一部分。现在，我们开发者有责任使用并应用这一新标准到我们的产品中，以更好地服务用户。

你可能会问自己，这些改变是否值得。将`ES6`集成到你的项目和工作是否必要？你真的需要改变现有的做事方式吗？我们想说的是：“是的，这是值得的”，并且通过本书，我们希望能鼓励你适应这个新世界。即使你在告别旧方式时遇到一些阻力，我们建议你坚持下去并改变你的实践。

JavaScript 社区充满活力且动态十足，一切都在快速变化。现在，`ES6/ES2015` 知识已成为 JavaScript 开发者的必备技能。新的语言结构不仅变得流行，而且得到了广泛支持。掌握`ES6`的内部机制将让你能够构建现代应用程序，获得更强大的语言能力，并帮助你提升编程技能。

通过`ES6`，你将能够使用更强大的编程概念，例如实用的面向对象代码以及其他一些在过去根本无法实现的新特性。我们希望向你介绍现代化的工作流程和工具（如`Babel`和`Webpack`），使应用程序开发更快、更愉快。你将能够衡量代码质量，并编写更易于测试的 JavaScript。
