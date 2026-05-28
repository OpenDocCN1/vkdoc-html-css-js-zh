# 一、什么是反应式编程？

> 千里之行，始于足下。—老子，陶德昌

Have you ever heard about Reactive Programming or Reactive Systems? Do you think React.js is a reactive library? Have you ever thought about why you should use Rx.JS inside an Angular project? Is Rx.JS the new Loadash? If at least one of these questions is often in your mind, this is exactly the right book to find an answer! In these pages you will have a chance to learn more about reactive programming and reactive architecture for front-end development: a programming paradigm that is becoming more popular, every day, in the front-end community; but these days it is probably one of the most misunderstood and abused paradigm. The main goal of this book is to provide a good understanding of what reactive programming is, how to use it in our projects, and particularly how to create fully reactive architectures for creating resilient and maintainable projects. During this journey you will learn the following:

*   什么是反应式编程，为什么它很重要
*   这种编程范式的最佳用例是什么
*   如何用不同的框架构建一个完全反应式的架构
*   在前端生态系统中，反应式编程的未来会是怎样的

If you are wondering if the concepts learned inside this book are applicable also on a back-end architecture, my answer would be YES, or at least, the majority of them could be applied to your back-end architecture too. Bear in mind that this book will focus the attention on front-end architectures with JavaScript, but some of the concepts illustrated should be easily portable to other back-end programming languages such as Node.js, for instance. This book assumes that you already have good understanding of JavaScript, in particular ECMAScript 6 and 7 syntax; object-oriented programming; and possibly some knowledge of functional programming, but it’s not mandatory. Let the journey begin!

## 什么是反应式编程？

Every day when we open an editor or IDE to develop a program, we use our favorite programming language; sometimes we study a new one, but, consciously or unconsciously, we are making the decision of what kind of programming paradigm we are going to work with. Reactive programming is not a new paradigm: it’s one of the buzzwords we are used to hearing about in the JavaScript community in the past year or so, and it will become more than just a buzzword in the future. I don’t want to begin immediately by using too many technical terms because we will have enough time to learn about them while reading this book, but it’s important that you understand what is the benefit of working in a “reactive way.” If you read blog posts or articles on the Web, few of them are going to explain reactive programming with the spreadsheet cells example, where spreadsheet cells are reacting to changes happening in other cells after user input. This is definitely a good example but we can do better than this. I’m sure you are familiar with the dependency injection pattern where an object is injected via the constructor or in a public method exposed by a class or module. This pattern leverages several benefits like decoupling between two objects and the possibility of testing the hosting object in isolation without creating dependencies and so on. In some programming languages when we use dependency injection we are going to define an interface as function’s argument in the hosting object and then we can interact with the methods available in the injected object. The injected object in this case is used as an interactive object, because the host knows exactly what the contract is and how to use it. In reactive programming instead, the hosting object will just subscribe to the injected one, and it will react to the propagation of changes during the application lifetime. See Figure 1-1.

![A446957_1_En_1_Fig1_HTML.jpg](img/A446957_1_En_1_Fig1_HTML.jpg)

Figure 1-1 Interactive vs. Reactive programming : in Reactive Programming the producer is A and the consumer is B

Looking at the image above, we can immediately grasp the main difference between the two approaches :

## 交互式与反应式编程

*   在交互的例子中，对象 A 知道调用哪个方法，因为它确切地知道 B 的对象契约，而且如果我们必须了解谁影响了被摄取对象的状态，我们将搜索所有与我们交互的项目。

*   在反应型中，契约是标准的，对象 A 对对象 B 中发生的变化作出反应，最重要的是，我们确信任何操作都会发生在被注入的对象内部；因此，我们将对对象之间的关注点进行更强的分离。

*   因为宿主对象对注入的对象内部传播的任何值做出反应，所以我们的程序将是最新的，而不需要实现任何额外的逻辑。

It’s time for a simple example before we move ahead with other concepts. Let’s assume we have a class called `Calculator` with a method `sum` and a class `Receipt` with a method `print` as shown in `Listing 1-1`.

```javascript
class Calculator {
  sum(a, b){
    return a + b;
  }
}

class Receipt {
  constructor(calculator){
    this.calc = calculator;
  }
  print(itemA, itemB){
    const total = this.calc.sum(itemA, itemB);
    console.log(`total receipt £${total}`);
  }
}

const pizza = 6.00;
const beer = 5.00;
const calc = new Calculator();
const receipt = new Receipt(calc);
receipt.print(pizza, beer);
```

`Listing 1-1` Check `Snippet1.js`

As you can imagine, the program outputs “total receipt £11.” What we are doing in this example is creating the `Calculator` object and a `Receipt` object, and then we inject the `Calculator` instance called `calc` and we call the method `print` from the `receipt` instance with few arguments. Inside the `print` method we are writing in the console the total price of the elements passed. Checking the `Receipt` class implementation, you can spot in the `print` method that we are interacting with the method `sum` of the `Calculator` class and then getting the final result.

Now let’s try to implement the same example in a reactive way in `Listing 1-2`.

```javascript
class Calculator {
  constructor(itemA, itemB){
    const obs = Rx.Observable.of(itemA, itemB);
    const sum$ = obs.reduce((acc, item) => (acc + item));
    return {
      observable: sum$
    }
  }
}

class Receipt {
  constructor(observable$){
    observable$.subscribe(value => console.log(`total receipt: £${value}`))
  }
}

const pizza = 6.00;
const beer = 5.00;
const calc = new Calculator(pizza, beer);
const receipt = new Receipt(calc.observable);
```

`Listing 1-2` Check `Snippet2.js`

As you can see in this example, the `Receipt` class is subscribing to an object called `observable`, injected via the constructor, and all the logic of how to sum the prices and propagate them is delegated to the `Calculator` class. Therefore, the `Receipt` class is just reacting to a change, happening in a certain moment of the program’s lifetime, displaying in the console the value emitted by the `Calculator` instance. Another thing to highlight here is the contract between the objects: instead of knowing exactly what method we should call, we pass an, with a default contract, and we react when something changes inside it. Overall, it’s a very simple example but I hope it helps you to understand the shift of mindset we are going to have when we work with reactive programming; in the next few chapters we will see more reactive examples like this one.

## 编程范例

It’s time to spend some words on programming paradigms to shed some light on reactive concepts and comparing them with functional and imperative programming.

### 命令式编程

命令式编程可能是最广为人知的编程范式之一。通常，这是我们学习理解计算机如何处理程序时首先接触的范式：它为我们提供了深入定义实现细节和精确指定程序应如何逐步执行的所有工具。让我们看一个命令式编程的例子：

```javascript
class Calculator {
  constructor(){
    this.VAT = 22;
  }
  sum(...items){
    let total = 0;
    let i = 0;
    for(i; i < items.length; i++){
      total = total + items[i];
      total = total + items[i] * this.VAT/100;
    }
    return total;
  }
}

class Receipt {
  constructor(calculator){
    this.calc = calculator;
  }
  print(...items){
    let total = this.calc.sum(...items);
    console.log(`total receipt £${total.toFixed(2)}`);
  }
}

const JEANS = 80.00;
const SHIRT = 35.00;
const SHOES = 90.00;
const COAT = 140.00;
const HAT = 29.00;

const calc = new Calculator();
const receipt = new Receipt(calc);
receipt.print(JEANS, SHIRT, SHOES, COAT, HAT);
//"total receipt £456.28"
```

与之前讨论的例子类似，`calculator`对象的`sum`方法接受多个参数，而不仅仅是两个，我们对它们求和并应用按项目计算的增值税值。如你所见，我们正在描述我们想要执行的确切实现：从定义一个`for`语句来遍历数组中的值，到表达每个单项的增值税计算。基本上，我们正在关注任何实现细节；我们可能会改变遍历数组元素的方式，从数组的最后一个元素开始而不是第一个，或者使用与“i”不同的变量名；这些是我们通常在命令式编程中处理的细节层次。

现在，让我们看看这个例子在函数式和响应式编程中将如何处理。

### 函数式编程

函数式编程正日益流行。许多语言纷纷采纳这一范式，许多现有语言也因其带来的可读性、可维护性和可测试性提升而拥抱它。如果你曾思考函数式编程为何如此受欢迎，答案就藏在这一范式的核心概念中。当我们谈论函数式编程时，我们谈论的是函数：具体而言，是**纯函数**。纯函数是指给定一个参数，总是返回相同结果的函数；它是可预测的、易于测试的，并且不会在程序内部产生任何副作用。与函数式编程相关的另一个重要概念是**不可变性**。不可变性意味着一个特定的值在其生命周期内永远不会改变，但如果我们需要操作它，我们将创建一个包含原始对象操作后版本的新实例。即使你不熟悉这个概念或从未处理过，我也希望你至少看一个具体的例子。通常，当你处理一个数组并想以命令式编程的方式遍历其值并与之交互时，你会写类似这样的代码：

```javascript
const originalArray = [1, 4, 8, 12];
for(let i = 0; i < originalArray.length; i++){
   originalArray[i] = originalArray[i] + 1;
}
console.log(originalArray) //[2, 5, 9, 13]
```

在这个阶段，我们已经完全丢失了名为`originalArray`的数组的初始值；如果我们想保留初始值的副本，就需要创建一个包含修改后值的新数组，然后检查那个新数组。而在函数式编程中，我们默认使用不可变对象；因此，我们需要应用的任何修改都不会影响原始值，而是会生成一个新值。考虑前面的例子，我们可以用函数式编程的方式这样写：

```javascript
const originalArray = [1, 4, 8, 12];
const finalArray = originalArray.map(value => value+1);
console.log(finalArray); //[2, 5, 9, 13]
```

从这些简单的例子可以看出，函数式编程更关注你试图实现的目标，而非其实现细节。这是与命令式编程的一个根本区别。事实上，使用函数式编程，我们是在描述我们的程序，专注于每一个动作；我们需要描述程序的数据流，而不是专注于每个细节，比如为了遍历数组必须定义哪个变量，或者如何在`for`语句中递增变量。软件编程的另一个重要方面是如何处理**状态管理**。在面向对象编程中，我们习惯于将状态封装在一个对象中，并通过同一对象中描述的不同方法来改变它。但在函数式编程中，我们试图通过纯函数来组合状态，这些函数接受一个状态作为参数并返回一个新状态。Redux（[`redux.js.org/`](http://redux.js.org/)），React社区中一个非常知名的库，旨在通过实现状态机模式来解决状态管理问题。在Redux中，当我们想要改变应用状态时，我们需要调用一个接受两个参数的方法：先前的应用状态和一个动作。它返回新状态而不改变先前的状态。动作是一个简单的对象，用于标识应用应该转换到哪个状态。依赖纯函数将使我们的代码更具模块化和可重用性，产生更少的bug，并且突然变得更容易测试！现在让我们尝试将之前的命令式示例转换为函数式编程。参见清单1-3。

```javascript
class Calculator {
   getTotal(...items){
       const total = items.map(::this.addVAT)
                    .reduce(this.sumElements);
       return total;
   }
   addVAT(itemValue){
     return itemValue + this.calculateVAT(itemValue);
   }
   calculateVAT(value){
     const VAT = 22;
     return value * VAT/100;
   }
   sumElements(accumulator, value){
     return accumulator + value
   }
}
class Receipt {
   print(total){
     console.log(`total receipt £${total.toFixed(2)}`);
   }
}
const JEANS = 80.00;
const SHIRT = 35.00;
const SHOES = 90.00;
const COAT = 140.00;
const HAT = 29.00;
const calc = new Calculator();
const receipt = new Receipt();
receipt.print(calc.getTotal(JEANS, SHIRT, SHOES, COAT, HAT));
// "total receipt £456.28"
```

清单1-3 检查 Snippet3.js

正如我们在`sum`方法实现中看到的，我们更关注想要实现的动作，而不是如何应用它们；因此，首先我们知道需要为每个元素计算增值税值（`map`方法），然后对项目求和以获取总计（`reduce`方法）。在这个实现中，我们不需要指定如何遍历数组元素或指定变量来保持迭代状态；我们只专注于我们的目标。在这个实现中需要注意的另一点是我们如何将函数作为其他函数的参数使用；这种机制称为**高阶函数**，它是函数式编程的另一个基石。显然，函数式编程不止于此：它是一个广泛的主题，还有许多其他模式需要考虑，如柯里化、实际应用、记忆化等等，但这不是本书的主要主题。请记住这些概念，并可能在网上阅读一些关于它们的文章以掌握不同的实现方式，因为熟悉函数式编程将使你更容易接受响应式编程。如果你不习惯函数式思维，我强烈建议你找一本用你最喜欢的语言描述这一范式的书来浏览一下；这是一项很棒的时间投资，你不会后悔的。

## 反应式编程

我们刚刚简要了解了命令式编程和函数式编程，那么反应式编程呢？反应式编程并非新概念——它在服务端编程中已使用多年——但在前端生态系统中也正变得非常流行。我们已经看到了如何轻松地将一个简单的例子转变为反应式编程，但远不止于此。通过反应式编程，我们可以轻松地将代码转变为命令式反应式编程或函数式反应式编程。这一范式背后的核心概念是数据流，以及对象如何观察并响应应用程序生命周期中发生的变化。让我们开始定义反应式编程的含义：反应式编程是一种基于异步数据流的范式，这些数据流在应用程序生命周期中传播变化。这在实践中意味着什么？假设我们需要开发一个金融仪表盘，所有数据都来自一个为客户端可视化而聚合数据的服务器。对于客户端，我们需要建立一个轮询机制或打开一个 WebSocket 通信来获取这些信息，然后我们需要将它们转换成一个漂亮的用户界面供用户使用。尝试分解我们想要实现的挑战，以及在没有反应式编程的情况下我们会如何思考，请考虑以下几点：

1.  我们需要创建一个代理、一个动作或一个命令来每隔几秒钟从服务器检索数据，从而触发轮询机制。
2.  在检索数据之后，我们需要分析这些数据，可能修改或分析它们以提供更友好的可视化。
3.  然后，我们通过中介、存储、控制器或任何其他与视图耦合的层将这些数据传递给不同的组件，以便更新数据。
4.  在最后一部分，我们将从视图中更新 DOM，可能只突出显示自上次更改以来已经更改的数据（在这种情况下，虚拟 DOM 机制可以帮助减少工作量）。

有很多部分需要处理，也有很多事件、命令或信号需要使用，以便让这些数据从我们应用程序的不同部分流动并在视图中显示。如果你知道有一种更好的方法，你会怎么说？这是一种范式，它将允许我们编写更少的代码，并在我们当前的应用程序中变得更具表达力和可插拔性。显然，我并不是说我们不会实现所有这些任务——反应式编程不是万能药或魔杖——但我们很快就会了解到，这种范式在简洁性和表达力方面可能会有不少惊喜。反应式编程中另一个重要的概念是对象之间的通信方式。如果你还记得，几段之前，我展示了一个例子，说明我们如何通过注入一个可观察的实例而不是特定类或模块的实例来解决对象间通信的简单问题。这可能会导致许多目前在前端领域尚未完全探索的有趣场景。如果对象之间的契约成为标准，我们注入的实例将具有一个预定义的契约，允许宿主对象订阅来自注入实例的变化，你会怎么说？这也不是一个新概念：1978 年，一个类似的想法随着通信顺序进程（`Communicating Sequential Processes (CSP)`）而诞生。尽管名字如此，`CSP` 允许你处理顺序和并行进程；进程不过是一个用于对象间异步通信的“通道”。通过这种方式，你使用通道进行通信来解耦对象；这个通道不仅允许你传递数据，还允许你执行以下操作：

*   汇总数据
*   转换数据
*   减少数据
*   装饰数据

## 什么时候应该使用反应式编程？

除此之外，我们还将能够操作通道（分割、管道、合并等），以在对象之间创建复杂的交互（图 1-2）。

![A446957_1_En_1_Fig2_HTML.jpg](img/A446957_1_En_1_Fig2_HTML.jpg)

图 1-2 在此图中，我们向视图和控制器、表示模型、中介者或模型注入了一个通道，以便在主状态发生更改时允许视图更新。

CSP 是我们将在响应式世界中探索的内容的一个细粒度实现。如果你有兴趣花些时间了解 CSP，我建议查看名为 CSP-JS（[`github.com/ubolonton/js-csp`](https://github.com/ubolonton/js-csp)）的库。理解 CSP 的工作原理将加速你在响应式编程方面的学习过程。

经过这个简短的题外话，我们可以回到本书的主要话题；现在是时候看看响应式编程与命令式和函数式编程结合的实际应用了。让我们从清单 1-4 所示的命令式示例开始。

```javascript
class Calculator {
  constructor(){
    this.VAT = 22;
  }
  sum(items){
    const items$ = Rx.Observable.from(items);
    const total$ = items$.map(value => value + (value * this.VAT/100))
                        .reduce((acc, value) => acc + value);
    return total$;
  }
}

class Receipt {
  constructor(calculator){
    this.calc = calculator;
  }
  print(...items){
    const total$ = this.calc.sum(items);
    total$.subscribe(total => console.log(`total receipt £${total.toFixed(2)}`));
  }
}

const JEANS = 80.00;
const SHIRT = 35.00;
const SHOES = 90.00;
const COAT = 140.00;
const HAT = 29.00;

const calc = new Calculator();
const receipt = new Receipt(calc);
receipt.print(JEANS, SHIRT, SHOES, COAT, HAT);
```

清单 1-4 查看 Snippet4.js

在这个例子中，与命令式示例相比，我们没有做太多改变，但让我们分析一下我们做了什么：

*   在 `Receipt` 对象的 `print` 方法中，我们将参数转换为可观察值。
*   我们将可观察对象注入到计算器类的 `sum` 方法中。
*   在这里，我们首先通过 `map` 方法将 VAT 应用于每个单个元素。
*   然后我们将这些值相加，返回另一个可观察的对象。
*   最后但同样重要的是，我们订阅由 `sum` 方法返回的可观察对象，并在控制台中显示总价。

在这个阶段，我不想过多深入响应式术语；但现在，请将可观察对象视为一个包装数据并暴露一些方法来操作值的对象——一个数据在其中流动的通道，我们可以对这些数据应用转换。

在理解如何为命令式编程实现一些响应性之后，让我们看看函数式示例如何，如清单 1-5 所示。

```javascript
class Calculator {
  getTotal(...items){
    const items$ = Rx.Observable.from(items);
    const total$ = items$.map(::this.addVAT)
                        .reduce(this.sumElements);
    return total$;
  }
  addVAT(itemValue){
    return itemValue + this.calculateVAT(itemValue);
  }
  calculateVAT(value){
    const VAT = 22;
    return value * VAT/100;
  }
  sumElements(accumulator, value){
    return accumulator + value
  }
}

class Receipt {
  print(total$){
    total$.subscribe(total => console.log(`total receipt £${total.toFixed(2)}`));
  }
}

const JEANS = 80.00;
const SHIRT = 35.00;
const SHOES = 90.00;
const COAT = 140.00;
const HAT = 29.00;

const calc = new Calculator();
const receipt = new Receipt();
receipt.print(calc.getTotal(JEANS, SHIRT, SHOES, COAT, HAT));
```

清单 1-5 查看 Snippet5.js

同样，在这种情况下，示例几乎相同，但现在 `Receipt` 使用了名为 `total$` 的可观察对象，其签名与前一个示例不同，前一个示例中我们只得到一个简单的数字。再一次，一个可观察对象允许订阅它并检索其中流动的值。

乍一看，这些概念可能显得不重要，但当我们尝试创建一个完整的响应式架构时，它们将对我们大有帮助。

Often, when a new trend is rising, a lot of developers are used to abusing the new technology or framework (hype-driven development). As we understood during this chapter, Reactive Programming is used for handling the propagation of data during the life cycle of an application. Therefore, a perfect fit for this paradigm would be a real-time data application like a financial dashboard or any monitoring system for instance. In a nutshell, we can say that any application that is heavily data driven could be a great fit for Reactive Programming. Obviously, it doesn’t mean you shouldn’t use this paradigm in other applications, but real-time web apps and applications with a large amount of asynchronous transactions and mutations are where Reactive Programming really shines.

If we decide to use Reactive Programming inside existing architectures, such as Angular or Redux, for instance, it could be a good design decision because it could facilitate the update of our views or the state propagation inside components. Nowadays the Web is full of plenty of reactive examples, libraries and frameworks are raising them with great success, and embracing them will impose a shift of mindset in order to embrace the real power of this paradigm. Another great benefit of Reactive Programming is the simplicity of testing your code and describing data flows in a concise but clear manner.

Reactive Programming is already implemented in production environments of several large organizations such as Netflix, Google, and Microsoft. Microsoft and Google, for instance, are a great contributor of the Reactive Programming movement ( [`reactivex.io/`](http://reactivex.io/) ). Netflix, as well, is another company that is contributing heavily to the evolution of this paradigm with Rx.JS 5, and Reactive Programming is applied in several implementations currently in production.

## 你应该如何编写一个完全反应式的应用程序？

One night I went to a meetup in London where we were discussing the JavaScript trends for 2017\. Funny enough, Reactive Programming was one of these trends and the facilitator asked a question to all of the attendees: “How many of you are working with Reactive Programming now?” And half of the room raised their hands asserting a positive answer. After this question, the facilitator asked the following one: “How many of you are using React.js only as Reactive library?” Then over 85% of the people that raised their hand at the previous question raised their hand again. That wasn’t surprising for me because it provides an understanding how few people are interpreting Reactive Programming in a correct way.

We could decide to structure our projects with a fully Reactive architecture where the communication/interaction between objects, the state management, and the interaction with endpoints are all handled in a Reactive paradigm. Reactive Programming on the front end and back end brought up different architectures and implementations that are interesting to use but at the same time different from what we are used to dealing with. In this book we are going to explore different approaches currently available inside the front-end Reactive community.