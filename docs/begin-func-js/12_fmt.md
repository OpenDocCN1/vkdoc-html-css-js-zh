# 十二、测试和最后的想法

> 所有的代码都是有罪的，直到被证明是无辜的。
> —匿名

我们已经介绍了围绕函数式 JavaScript 的大部分概念。我们已经学习了 ES8 规范中的基础知识、先进理念和最新概念。我们的学习完成了吗？我们能断言我们已经写出了可行的代码吗？没有；除非代码经过测试，否则没有代码是完整的。

在这最后一章中，我们将学习为我们已经编写的功能性 JavaScript 代码编写测试。我们将学习使用业界最好的测试框架和编码模式来创作灵活、易于学习的自动化测试。本章讨论的模式和实践可以用来测试所有可能场景的任何功能代码。我们还将学习测试使用高级 JavaScript 的代码，比如 Promises 和异步方法。本章的剩余部分涉及使用各种工具来运行测试，报告测试状态，计算代码覆盖率，以及应用林挺来实施更好的编码标准。最后，我们总结了第二版的一些结论性想法。

### 注意

章节示例和库源代码在第十二章。回购的网址是 [`github.com/antsmartian/functional-es8.git`](https://github.com/antsmartian/functional-es8.git)。

一旦你检查出代码，请检查分支第十二章:

```
git checkout -b 第十二章来源/第十二章
```

以管理员身份打开命令提示符，导航到包含 `package.json` 的文件夹，然后运行

```
npm 安装
```

下载代码运行所需的包。

## 介绍

每个开发人员都应该知道，编写测试用例是证明代码运行并确保没有错误路径的唯一方法。测试有很多种——单元测试、集成测试、性能测试、安全/渗透测试等等——每一种都满足代码的某些标准。编写哪些测试完全取决于功能和功能的优先级:这完全取决于投资回报(ROI)。您的测试应该回答这些问题:这个功能对应用程序重要吗？如果我写这个测试，我能证明这个功能工作吗？应用程序的核心功能包含在前面提到的所有测试中，而很少使用的功能可能只需要单元和集成测试。宣扬单元测试并不是本节的主旨。相反，我们将学习在当前 DevOps 场景中创作自动化单元测试的重要性。

DevOps (Development + Operations) is a set of processes, people, and tools together used to define and ensure continuous frictionless delivery of software applications. Now where does testing fit into this model? The answer lies within continuous testing. Every high-performing Agile team with a DevOps delivery model should ensure they follow practices like continuous integration, testing, and delivery. In simple terms, every code check-in done by a developer is integrated into the one single repository, all the tests are run automatically, and the latest code is deployed automatically (provided the tests’ passing criteria are met) to a staging environment. Having a flexible, reliable, and fast delivery pipeline is the key to success for the most successful companies as shown in Table 12-1.

Table 12-1 成功公司的交付渠道

| 组织 | 部署 |
| --- | --- |
| 脸谱网 | 每天 2 次部署 |
| 亚马孙 | 每 11.6 秒部署一次 |
| 网飞 | 每天 1000 次 |

*资料来源:维基百科。*

假设您是使用 Node 构建应用程序的敏捷团队的一员，您已经使用本书中解释的最佳实践编写了大量代码，现在您也有责任为您的代码编写测试，以便它达到可接受的代码覆盖率和通过标准。本章的目的是教你如何为 JavaScript 函数编写测试。

Figure 12-1 shows where the continuous testing phase sits in the overall application life cycle.

![img/429083_2_En_12_Fig1_HTML.jpg](img/429083_2_En_12_Fig1_HTML.jpg) Figure 12-1 应用程序生命周期的持续测试阶段

## 测试类型

The following are the most important categories of tests.

*   *单元测试* *:* 编写单元测试是为了孤立地测试每一个功能。这将是本章的主要焦点。单元测试通过提供输入并确保输出符合预期来测试单个功能。单元测试模仿依赖行为。本章后面会有更多关于嘲讽的内容。

*   *集成测试* *:* 集成测试是为了测试端到端的功能而编写的。例如，对于一个用户注册场景，这个测试可能会在数据存储中创建一个用户，并确保它存在。

*   *UI(功能测试)* *:* UI 测试是针对 web 应用的；编写这些测试是为了控制浏览器和实现用户旅程。

其他类型的测试包括冒烟测试、回归测试、验收测试、系统测试、飞行前测试、渗透测试和性能测试。有各种框架可用于编写这些类别的测试，但是对这些测试类型的解释超出了本书的范围。本章只讨论单元测试。

## BDD 和 TDD

在我们深入研究 JavaScript 测试框架之前，让我们简单介绍一下最著名的测试开发方法，行为驱动开发(BDD)和测试驱动开发(TDD)。

BDD suggests testing the behavior of the function instead of its implementation. For example, consider the following function that just increments a given number by 1.

```javascript
var mathLibrary = new MathLibrary();
var result = mathLibrary.increment(10);
```

BDD advises the test to be written as shown next. Although this looks like a simple unit test, there is a subtle difference. Here we are not worried about the implementation logic (like the initial value of Sum).

```javascript
var expectedValue = mathlibrary.seed + 10; // imagine seed is a property of MathLibrary
Assert.equal(result, expectedValue);
```

断言是帮助我们对照期望值验证实际值的函数，反之亦然。在这里，我们不担心实现细节；相反，我们断言该函数的行为，即将值递增 1。如果种子的值明天改变，我们不必更新函数。

### 注意

`Assert` 是大多数测试框架中术语的一部分。它主要用于以各种方式比较预期值和实际值。

TDD suggests you write the test first. For example, in the current scenario we write the following test first. Of course it would fail because there is no `MathLibrary` or its corresponding function called `increment`.

```javascript
Assert.equal(MathLibrary.increment(10), 11);
```

TDD 背后的思想是首先编写满足功能需求的断言，这些断言最初会失败。通过进行必要的修改(编写代码)来通过测试，开发就取得了进展。

## JavaScript 测试框架

JavaScript 作为一种广泛用于编写函数式代码的语言，拥有众多测试框架，例如 Mocha、Jest（由 Facebook 开发）、Jasmine 和 Cucumber 等。其中最著名的是 Mocha 和 Jasmine。要为 JavaScript 函数编写单元测试，我们需要能够满足以下基本需求的库或工具。

*   测试结构，它定义了文件夹结构、文件名和相应的配置。
*   断言函数，一个可以用来灵活断言的库。
*   Reporter，一个以控制台、HTML、JSON 或 XML 等各种格式显示结果的框架。
*   Mocks，一个可以提供测试替身来伪造依赖组件的框架。
*   代码覆盖率，所以框架应该能够清楚地说出测试覆盖的行数或函数数。

不幸的是，没有一个测试框架提供所有这些功能。例如，Mocha 没有断言库。幸运的是，像 Mocha 和 Jasmine 这样的大多数框架都是可扩展的；我们可以使用 Babel 的断言库或带有 Mocha 的 `expect.js` 来执行干净的断言。在 Mocha 和 Jasmine 之间，我们将编写 Mocha 测试，因为我们觉得它比 Jasmine 更灵活。当然，在这一节的最后，我们还会看到 Jasmine 测试的一瞥。

### 注意

在撰写本文时，Jasmine 不支持对 ES8 特性的测试，这也是偏向 Mocha 的原因之一。

### 使用 Mocha 测试

以下部分解释了如何为创作测试设置 Mocha，以及用模拟创作同步和异步测试的本质。我们开始吧。

#### 装置

`mocha`（[`mochajs.org`](https://mochajs.org)）是一个社区支持的、功能丰富的 JavaScript 测试框架，可以在 Node.js 和浏览器上运行。Mocha 自诩让异步测试变得简单有趣，这一点我们一会儿就能见证。

安装 Mocha 到全局和开发环境，如下所示：
```
npm install –global mocha
npm install –save-dev mocha
```
添加一个名为 `test` 的新文件夹，并在 `test` 文件夹中添加一个名为 `mocha-tests.js` 的新文件。以下是更新后的文件结构：
```
functional-playground
|------play.js
| lib
|------es8-functional.js
| test
|-----mocha-tests.js
```

#### 简单 Mocha 测试

将以下简单的 Mocha 测试添加到 `mocha-tests.js` 中。
```javascript
var assert = require('assert');
describe('Array', function () {
    describe('#indexOf()', function () {
        it('should return -1 when the value is not present', function () {
            assert.equal(-1, [1, 2, 3].indexOf(4));
        });
    });
});
```
让我们逐点理解。第一行代码用于导入 Babel 断言库。如前所述，Mocha 没有开箱即用的断言库，因此需要这一行。你也可以使用任何其他断言库，如 `expect.js`、`chai.js`、`should.js` 等。
```javascript
var assert = require('assert');
```
Mocha 测试本质上是分层的。前面显示的第一个 `describe` 函数描述了第一个测试类别 `'Array'`。每个主要类别可以有多个 `describe`，如 `'#indexOf'`。这里的 `'#indexOf'` 是一个子类别，包含与数组的 `indexOf` 函数相关的测试。实际的测试以 `it` 关键字开始。`it` 函数的第一个参数应始终描述预期行为（Mocha 使用 BDD）。
```javascript
it('should return -1 when the value is not present', function(){})
```
一个子类别中可以有多个 `it` 函数。以下代码用于断言预期值与实际值。在一个测试用例中也可以有多个断言（这里的 `it` 函数是一个测试用例）。默认情况下，在多次断言的情况下，测试在第一次失败时停止，但是这种行为是可以改变的。

以下代码添加到 `package.json` 中以运行 Mocha 测试。检出分支时，也请检查 `dev dependencies` 和 `dependencies` 部分，以了解引入的支持库。
```json
"mocha": "mocha --compilers js:babel-core/register --require babel-polyfill"
```
这里的开关 `--compilers` 和 `--require` 是可选的；在这种情况下，它们用于编译 ES8 代码。运行以下命令来执行测试：
```
npm run mocha
```
图 12-2 显示了一个示例响应。
![img/429083_2_En_12_Fig2_HTML.jpg](img/429083_2_En_12_Fig2_HTML.jpg)
图 12-2

开关响应示例

观察测试结果呈现的方式。`Array` 是层次结构中的第一级，后面是 `#indexOf`，然后是实际的测试结果。上面的语句 **1 通过** 显示了测试的总结。

#### Currying、Monads 和 Functors 的测试

我们已经学习了很多函数式编程的概念，比如 currying、函子和单子。在这一节中，我们将学习为我们之前学过的概念编写测试。

让我们从为 currying 编写单元测试开始，currying 是将一个具有 *n* 个参数的函数转换为嵌套的一元函数的过程。嗯，这是正式定义，但它可能无法帮助我们编写单元测试。为任何函数编写单元测试都相当容易。第一步是列出其主要特性集。这里我们指的是第 6 章中编写的 `curryN` 函数。让我们定义它的行为：

1.  `curryN` 应该总是返回一个函数。
2.  `curryN` 应该只接受函数，传递任何其他值都应该抛出错误。
3.

当使用相同数量的参数调用时，`curryN` 函数应该返回与普通函数相同的值。

## 测试 CurryN 函数

现在，让我们开始为这些功能编写测试。

```javascript
it("should return a function", function(){
    let add = function(){}
    assert.equal(typeof curryN(add), 'function');
});
```

此测试将断言 `curryN` 是否总是返回一个函数对象。

```javascript
it("should throw if a function is not provided", function(){
    assert.throws(curryN, Error);
});
```

此测试将确保当未传递函数时，`curryN` 会抛出 `Error`。

```javascript
it("calling curried function and original function with same arguments should return the same value", function(){
    let multiply = (x,y,z) => x * y * z;
    let curriedMultiply = curryN(multiply);
    assert.equal(curriedMultiply(1,2,3), multiply(1,2,3));
    assert.equal(curriedMultiply(1)(2)(3), multiply(1,2,3));
    assert.equal(curriedMultiply(1)(2,3), multiply(1,2,3));
    curriedMultiply = curryN(multiply)(2);
    assert.equal(curriedMultiply(1,3), multiply(1,2,3));
});
```

前面的测试可用于测试柯里化函数的基本功能。现在让我们为函子编写一些测试。在此之前，就像我们为柯里化所做的那样，让我们回顾一下函子的特性。

1.  函子是保存值的容器。
2.  函子是实现函数映射的普通对象。
3.  像 `MayBe` 这样的函子应该处理 `null` 或 `undefined`。
4.  像 `MayBe` 这样的函子应该支持链式调用。

## 测试函子

现在，根据我们如何定义函子，让我们看一些测试。

```javascript
it("should store the value", function(){
    let testValue = new Container(3);
    assert.equal(testValue.value, 3);
});
```

此测试断言像 `Container` 这样的函子持有一个值。那么，如何测试函子是否实现了 `map` 呢？有几种方法：你可以断言原型，或者调用函数并期望得到一个正确的值，如下所示。

```javascript
it("should implement map", function(){
    let double = (x) => x + x;
    assert.equal(typeof Container.of(3).map == 'function', true)
    let testValue = Container.of(3).map(double).map(double);
    assert.equal(testValue.value, 12);
});
```

以下测试断言函数是否处理 `null` 并支持链式调用。

```javascript
it("may be should handle null", function(){
    let upperCase = (x) => x.toUpperCase();
    let testValue = MayBe.of(null).map(upperCase);
    assert.equal(testValue.value, null);
});

it("may be should chain", function(){
    let upperCase = (x) => x.toUpperCase();
    let testValue = MayBe.of("Chris").map(upperCase).map((x) => "Mr." + x);
    assert.equal(testValue.value, "Mr.CHRIS");
});
```

现在，使用这种方法，应该很容易为单子编写测试。从哪里开始呢？这里有一点帮助：让我们看看你是否能自己为以下规则编写测试。

### 测试单子

1.  单子应该实现 `join`。
2.  单子应该实现 `chain`。
3.  单子应该去掉嵌套。

如果你需要帮助，请查看 GitHub 网址的第十二章分支。

我们已经在 `es-functional.js` 库中编写了许多函数，并使用 `play.js` 来执行它们。在本节中，我们将学习如何为到目前为止编写的函数式 JavaScript 代码编写测试。与 `play.js` 类似，在使用这些函数之前，它们应该被导入到 `mocha-tests.js` 文件中，因此请将以下行添加到 `mocha-tests.js` 文件中：

```javascript
import { forEach, Sum } from "../lib/es8-functional.js";
```

以下代码展示了为 JavaScript 函数编写的 Mocha 测试。

```javascript
describe('es8-functional', function () {
    describe('Array', function () {
        it('Foreach should double the elements of Array, when double function is passed', function () {
            var array = [1, 2, 3];
            const doublefn = (data) => data * 2;
            forEach(array, doublefn);
            assert.equal(array[0], 1)
        });
        it('Sum should sum up elements of array', function () {
            var array = [1, 2, 3];
            assert.equal(Sum(array), 6)
        });
        it('Sum should sum up elements of array including negative values', function () {
            var array = [1, 2, 3, -1];
            assert.notEqual(Sum(array), 6)
        });
    });
});
```

### 用 Mocha 进行异步测试

令人惊讶的是！Mocha 也支持 `async` 和 `await`，并且测试 Promise 或异步函数出奇地简单，如下所示。

```javascript
describe('Promise/Async', function () {
    it('Promise should return es8', async function (done) {
        done();
        var result = await fetchTextByPromise();
        assert.equal(result, 'es8');
    })
});
```

注意这里对 `done` 的调用。如果不调用 `done` 函数，测试将会超时，因为它没有等待我们的 Promise 所需的 2 秒。这里的 `done` 函数用于通知 Mocha 框架。使用以下命令再次运行测试：

```bash
npm run mocha
```

结果如图 12-3 所示。

![img/429083_2_En_12_Fig3_HTML.jpg](img/429083_2_En_12_Fig3_HTML.jpg)

图 12-3

下图显示了测试结果。

重申一下开头的陈述，Mocha 最初可能很难建立，因为它固有的灵活性坚持了这样一个事实，即它几乎可以与任何用于编写优秀单元测试的框架很好地结合在一起，但是最终，回报是丰厚的。

### 用 Sinon 进行模拟

假设你是团队 A 的一部分，团队 A 是一个大的敏捷团队的一部分，这个团队被划分成更小的团队，比如团队 A、团队 B 和团队 C。更大的敏捷团队通常被业务需求或地理区域所划分。假设团队 B 使用团队 C 的库，团队 A 使用团队 B 的函数库，每个团队都应该提交经过全面测试的代码。作为团队 A 的开发人员，在使用团队 B 的功能时，您会再次编写测试吗？不。那么当你依赖于调用团队 B 的函数时，你如何确保你的代码工作？这就是模拟库的由来，Sinon 就是这样一个库。如前所述，Mocha 没有开箱即用的模拟库，但它与 Sinon 无缝集成。

`sinon` ([Sinonjs.org](http://sinonjs.org)) 是一个独立的框架，为 JavaScript 提供间谍、存根和模拟。Sinon 可以轻松地与任何测试框架集成。

### 注意

间谍、模拟或存根，虽然它们解决了类似的问题，听起来也相关，但有一些微妙的区别，理解起来很重要。我们建议更详细地了解假货、仿制品和存根之间的区别。本节仅提供摘要。

一个假对象（fake）可以模拟任何 JavaScript 对象，比如函数或对象。考虑以下函数：

```javascript
var testObject= {};
testObject.doSomethingTo10 = (func) => {
    const x = 10;
    return func(x);
}
```

这段代码接受一个函数并在常量 10 上运行它。以下代码展示了如何使用 Sinon 的假对象来测试这个函数。

```javascript
it("doSomethingTo10", function () {
    const fakeFunction = sinon.fake();
    testObject.doSomethingTo10(fakeFunction);
    assert.equal(fakeFunction.called, true);
});
```

如你所见，我们没有创建一个实际的函数来作用于 10；相反，我们伪造了一个函数。断言 `fake` 是很重要的，因此 `assert.equal(fakeFunction.called, true)` 语句确保调用了伪造的函数，它断言了函数 `doSomethingTo10` 的行为。Sinon 在测试函数的上下文中提供了更全面的方法来测试假对象的行为。有关更多详细信息，请参见文档。

考虑这个函数：

```javascript
testObject.tenTimes = (x) => 10 * x;
```

以下代码展示了使用 Sinon 的存根（stub）编写的测试用例。如你所见，存根可用于定义函数的行为。

```javascript
it("10 Times", function () {
    const fakeFunction = sinon.stub(testObject, "tenTimes");
    fakeFunction.withArgs(10).returns(10);
    var result = testObject.tenTimes(10);
    assert.equal(result, 10);
    assert.notEqual(result, 0);
});
```

更常见的是，我们编写与外部依赖项交互的代码，如 HTTP 调用。如前所述，单元测试是轻量级的，应该模拟外部依赖，在本例中是 HTTP 调用。

Let’s say we have the following functions:

```javascript
var httpLibrary = {};
function httpGetAsync(url, callback) {
    // HTTP Get Call to external dependency
}
httpLibrary.httpGetAsync = httpGetAsync;
httpLibrary.getAsyncCaller = function (url, callback) {
    try {
        const response = httpLibrary.httpGetAsync(url, function (response) {
            if (response.length > 0) {
                for (let i = 0; i < response.length; i++) {
                    httpLibrary.usernames += response[i].username + ",";
                }
                callback(httpLibrary.usernames)
            }
        });
    } catch (error) {
        throw error
    }
}
```

If you would like to test only `getAsyncCaller` without getting into the nitty-gritty of `httpGetAsync` (let’s say it is developed by Team B), we can use Sinon mocks as shown here.

```javascript
it("Mock HTTP Call", function () {
    const getAsyncMock = sinon.mock(httpLibrary);
    getAsyncMock.expects("httpGetAsync").once().returns(null);
    httpLibrary.getAsyncCaller("", (usernames) => console.log(usernames));
    getAsyncMock.verify();
    getAsyncMock.restore();
});
```

This test case makes sure while testing `getAsyncCaller`, `httpGetAsync` is mocked. The following test case tests the same method without using mock.

```javascript
it("HTTP Call", function () {
    httpLibrary.getAsyncCaller("https://jsonplaceholder.typicode.com/users");
});
```

在结束编写函数式 JavaScript 代码的测试之前，让我展示一下如何使用 Jasmine 编写测试。

### 用 Jasmine 测试

Jasmine ([`jasmine.github.io`](https://jasmine.github.io)) is also a famous testing framework; in fact, the APIs of Jasmine and Mocha are similar. Jasmine is the most widely used framework when building applications with AngularJS (or Angular). Unlike Mocha, Jasmine comes with a built-in assertion library. The only troublesome area with Jasmine at the point of writing was testing asynchronous code. Let’s learn to set up Jasmine in our code in the next few steps.

```bash
npm install –save-dev jasmine
```

If you intend to install it globally, run this command:

```bash
npm install -g jasmine
```

Jasmine dictates a test structure including a configuration file, so running the following command will set up the test’s structure.

```bash
../node_modules/.bin/jasmine init
```

That command creates the following folder structure:

```
|-Spec
 |-----Support
 |---------jasmine.json (Jasmine configuration file)
```

`jasmine.json` contains the test configuration; for example, `spec_dir` is used to specify the folder to look for Jasmine tests, while `spec_files` describes the common keywords used to identify test files. For more configuration details, visit [`jasmine.github.io/2.3/node.html#section-Configuration`](https://jasmine.github.io/2.3/node.html#section-Configuration).

让我们在用 `init` 命令创建的 `spec` 文件夹中创建一个 Jasmine 测试文件，并将该文件命名为 `jasmine-tests-spec.js`。(记住，如果没有关键字 `spec`，Jasmine 将无法找到我们的测试文件。)

The following code shows a sample Jasmine test.

```javascript
import { forEach, Sum, fetchTextByPromise } from "../lib/es8-functional.js";
import 'babel-polyfill';

describe('Array', function () {
    describe('#indexOf()', function () {
        it('should return -1 when the value is not present', function () {
            expect([1, 2, 3].indexOf(4)).toBe(-1);
        });
    });
});

describe('es8-functional', function () {
    describe('Array', function () {
        it('Foreach should double the elements of Array, when double function is passed', function () {
            var array = [1, 2, 3];
            const doublefn = (data) => data * 2;
            forEach(array, doublefn);
            expect(array[0]).toBe(1)
        });
    });
});
```

如您所见，除了断言之外，代码看起来非常类似于 Mocha 测试。您可以完全使用 Jasmine 来重建测试库，我们让您来决定如何去做。

The following command is added to `package.json` to execute Jasmine tests.

```json
"jasmine": "jasmine"
```

Running the following command executes the tests:

```bash
npm run jasmine
```

![img/429083_2_En_12_Fig4_HTML.jpg](img/429083_2_En_12_Fig4_HTML.jpg)
Figure 12-4

下图显示了使用 Jasmine 的测试结果。

## 代码覆盖率

我们有多确定测试已经覆盖了关键领域？对于任何语言来说，代码覆盖率是唯一能够解释测试所覆盖的代码的度量。JavaScript 也不例外，因为我们可以获得测试覆盖的代码行数或百分比。

Istanbul ([`gotwarlost.github.io/istanbul/`](https://gotwarlost.github.io/istanbul/)) is one of the best known frameworks that can calculate the code coverage for JavaScript at the statement, Git branch, or function level. Setting up Istanbul is easy. `nyc` is the name of the command-line argument that can be used to get code coverage, so let us run this command to install `nyc`:

```
npm install -g --save-dev nyc
```

The following command can be used to run Mocha tests with code coverage, so let us add it to `package.json`.

```
"mocha-cc": "nyc mocha --compilers js:babel-core/register --require babel-polyfill"
```

Run the following command to run the Mocha tests and also get the code coverage.

```
npm run mocha-cc
```

The results are shown in Figure 12-5.

![img/429083_2_En_12_Fig5_HTML.jpg](img/429083_2_En_12_Fig5_HTML.jpg)

Figure 12-5

下图显示了使用 Mocha 编写的测试的代码覆盖率

正如你所看到的，除了文件 `es8-functional.js` 中的第 20 行和第 57 行，我们覆盖了 93%。代码覆盖率的理想百分比取决于几个因素，所有因素都考虑到了投资回报。通常 85%是一个推荐的数字，但是如果代码被任何其他测试覆盖，那么低于这个数字也是可行的。

## 林挺

代码分析和代码覆盖率一样重要，尤其是在大型团队中。代码分析帮助您实施统一的编码规则，遵循最佳实践，并为可读性和可维护性实施最佳实践。到目前为止，我们编写的 JavaScript 代码可能不符合最佳实践，因为这更适用于生产代码。在这一节中，让我们看看如何将编码规则应用于功能性 JavaScript 代码。

ESLint ([`eslint.org/`](https://eslint.org/)) is a command-line tool for identifying incorrect coding patterns in ECMAScript/JavaScript. It is relatively easy to install ESLint into any new or existing project. The following command installs `eslint`.

```
npm install --save-dev -g eslint
```

ESLint is configuration driven, and the command that follows creates a default configuration. You might have to answer a few questions as shown in Figure 12-6 here. For this coding sample we are using coding rules recommended by Google.

```
eslint --init
```

![img/429083_2_En_12_Fig6_HTML.jpg](img/429083_2_En_12_Fig6_HTML.jpg)

Figure 12-6

下图显示了 eslint 初始化步骤

Here is the sample configuration file.

```
{
    "parserOptions": {
        "ecmaVersion": 6,
        "sourceType": "module"
    },
    "rules": {
        "semi": ["error", "always"],
        "quotes": ["error", "double"]
    },
    "env": {
        "node": true
    }
}
```

Let's look at the first rule.

```
"semi": ["error", "always"],
```

This rule says a semicolon is mandatory after every statement. Now if we run it against the code file `es-functional.js` we have written so far, we get the results shown in Figure 12-7. As you can see, we violated this rule in many places. Imposing coding rules or guidelines should be done at the very beginning of the project. Introducing coding rules or adding new rules after accumulating a huge code base results in an enormous amount of code debt, which will be difficult to handle.

![img/429083_2_En_12_Fig7_HTML.jpg](img/429083_2_En_12_Fig7_HTML.jpg)

Figure 12-7

下图显示了 eslint 工具的结果

ESLint helps you fix these errors. As suggested earlier, you just have to run this command:

```
eslint lib\es8-functional.js --fix
```

所有错误都消失了！你可能并不总是幸运的，所以确保你在开发阶段的早期施加限制。

## 单元测试库代码

在前一章中，我们学习了如何创建有助于构建应用程序的库。一个好的库是可测试的，所以测试的代码覆盖率越高，消费者信任你的代码的可能性就越大。当您更改某些内容时，测试有助于快速检查代码中受影响的区域。在这一节中，我们为我们在前一章中编写的 Redux 库代码编写 Mocha 测试。

The following code is available in the `mocha-test.js` file. The `mocha-test.js` file refers to the code from our Redux library. The following test ensures that initially the state is always empty.

```javascript
it('is empty initially', () => {
    assert.equal(store.getState().counter, 0);
});
```

One of the main functions in our library was to assert if actions can influence state change. In the following state we initiate state change by calling `IncrementCounter`, which is called when a click event is raised. `IncrementCounter` should increase the state by 1.

```javascript
// test for state change once
it('state change once', () => {
    global.document = null;
    incrementCounter();
    assert.equal(store.getState().counter, 1);
});

// test for state change twice
it('state change twice', () => {
    global.document = null;
    incrementCounter();
    assert.equal(store.getState().counter, 2);
});
```

The last function we are going to assert is to check if there is at least one listener registered for state change. To ensure we have a listener we also register a listener; this is also called an Arrange phase.

```javascript
// test for listener count
it('minimum 1 listener', () => {
    //Arrange
    global.document = null;
    store.subscribe(function () {
        console.log(store.getState());
    });
    //Act
    var hasMinOnelistener = store.currentListeners.length > 1;
    //Assert
    assert.equal(hasMinOnelistener, true);
});
```

You can run `npm run mocha` or `npm run mocha-cc` to execute the tests with code coverage. You will notice in Figure 12-8 that we have covered more than 80% of the code we have written in the library.

![img/429083_2_En_12_Fig8_HTML.jpg](img/429083_2_En_12_Fig8_HTML.jpg)

Figure 12-8

下图显示了代码覆盖率的结果

有了这个经验，为我们在前一章中构建的类似 HyperApp 的库编写单元测试将是一个很好的练习。

## 结束语

Another wonderful journey comes to an end. We hope you had fun like we did learning new concepts and patterns in JavaScript functional programming. Here are some closing thoughts.

*   如果你刚开始一个项目，试着使用本书中的概念。本书中使用的每个概念都有特定的使用范围。在浏览一个用户场景时，分析你是否可以使用任何解释过的概念。例如，如果您正在进行 REST API 调用，您将分析是否可以创建一个库来异步执行 REST API 调用。

*   如果您正在处理一个现有的项目，该项目包含大量杂乱的 JavaScript 代码，那么分析这些代码，将其中的一些代码重构为可重用的、可测试的功能。最好的学习方法是通过实践，所以扫描你的代码，找到松散的部分，把它们缝合在一起，形成一个可扩展的、可测试的、可重用的 JavaScript 函数。

*   请继续关注 ECMAScript 更新，因为随着时间的推移，ECMAScript 将继续成熟并变得更好。你可以在[`github.com/tc39/proposals`](https://github.com/tc39/proposals)上关注这些提议，或者如果你有一个新的想法或者想法可以改进 ECMAScript 或者帮助开发者，你可以继续你的提议。

## 摘要

在这一章中，我们学习了测试的重要性，测试的类型，以及像 `BDD` 和 `TDD` 这样的开发模型。我们开始理解 JavaScript 测试框架的需求，并了解了最著名的测试框架 `Mocha` 和 `Jasmine`。我们使用 `Mocha` 编写了简单测试、函数库测试和异步测试。`Sinon` 是一个 JavaScript 模仿库，它为 JavaScript 提供了间谍、存根和模仿。我们学习了如何将 `Sinon` 与 `Mocha` 相结合来模仿依赖行为或对象。我们还学习了使用 `Jasmine` 为 JavaScript 函数编写测试。`Istanbul` 与 `Mocha` 集成得很好，并提供了我们的代码覆盖率，可以作为可靠性的衡量标准。`ESLint` 帮助我们编写干净的 JavaScript 代码，在本章中，我们学习了使用 `ESLint` 定义编码规则。