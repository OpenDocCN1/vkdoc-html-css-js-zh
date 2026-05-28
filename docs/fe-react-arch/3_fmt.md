# 三、反应式编程

> 所有的生活都是一场实验。你做的实验越多越好。—拉尔夫·瓦尔多·爱默生

在概述了最著名的前端架构及其实现之后，现在是时候深入探讨本书的主要主题了：使用 JavaScript 进行反应式编程。我观看了许多视频，阅读了许多关于这个主题的文章和书籍，我必须承认，我常常觉得有些东西缺失了，就像我们只是在表面摸索，而没有深入反应式生态系统内部。本章的主要目标是提供理解反应式编程所需的所有工具，而不是专注于某个单一的库，旨在拥抱这种编程范式背后的概念。这应该能让我们接纳任何反应式库，而无需花费太多时间来转换思维模式。我经常看到开发者精通某个特定的库或框架，成为真正的专家，但一旦离开，他们在尝试不同的实现时就会感到困难——主要是因为他们不理解实现背后的东西。理解反应式范式的核心原则将使你能够掌握任何库或框架，而无需将其视为一个“黑盒”。开源生态系统中有许多反应式库可用，我们将讨论最著名的，如 `Rx.JS` 和 `XStream`。之后，我们将理解反应式编程的关键原则，例如什么是 `stream`，热 `observable` 和冷 `observable` 之间的区别，以及背压（`back pressure`）。不幸的是，反应式编程的学习曲线很高，主要是因为它所依赖的概念，但一旦你习惯了它们，你将开始以反应式的方式思考一切，而不会有任何问题。

## 反应式编程 101

首先，实现反应式编程主要意味着处理事件流（`event streams`），但为了理解什么是事件流，我们应该从一个简单的图表开始，以更好地理解流的工作原理。图 3-1 被称为弹珠图（`marble diagram`），用于表示事件流。

![A446957_1_En_3_Fig1_HTML.jpg](img/A446957_1_En_3_Fig1_HTML.jpg)

图 3-1 弹珠图表示示例

理解它的工作原理非常重要，因为在文档中，你通常可以通过查看图表来理解 API 的工作方式，而无需阅读完整的解释。此外，弹珠图在测试阶段也很有用，因为当我们需要模拟事件流时，我们将使用它们来测试我们的事件流实现。但让我们看看它们是什么，这样我们就能更好地理解事件流的概念。在弹珠图中，我们可以看到一条代表时间（我们的流）的水平线，彩色圆圈内的值是在应用程序中某个时间点发生的事件（`events`）。垂直线则代表流的结束，在那之后流就完成了。所以，如果我们想描述这个图表，我们可以很容易地这样做：

*   应用程序启动后，流发出值 4。
*   一定时间后，流发出值 1，紧接着 3。
*   在所有这些事件之后，流完成了。

# The stream could also end after an error

The stream could also end after an error, and in that case instead of seeing a vertical line, the marble diagram (Figure 3-2) will show an X symbol notifying when the error happened inside the stream.

![A446957_1_En_3_Fig2_HTML.jpg](img/A446957_1_En_3_Fig2_HTML.jpg)
Figure 3-2 Marble diagram with error

## The last bit to mention is when we apply transformation to the values inside a stream via operators

An operator is a method that allows us to manipulate the data in a stream or the stream itself. In this case the marble diagrams can show how the values are transformed like in this example with the `map` operator where we are adding 1 to any number emitted by the stream on the top and creating a second stream with the values manipulated (Figure 3-3).

![A446957_1_En_3_Fig3_HTML.jpg](img/A446957_1_En_3_Fig3_HTML.jpg)
Figure 3-3 Marble diagram with `map` operator applied to an initial stream that returns a new stream with data manipulated

## In the reactive panorama we need to introduce a particular type of stream: the observable

Until now, we understood that an event stream is a sequence of values (events) passed in a certain period (stream). We can then assume that everything inside an application could be a stream, and in a certain way it’s exactly what the reactive paradigm is leveraging: we can transform listeners with callbacks in event streams, we can wrap an http request in a stream and handle the response by manipulating the data before updating a view, and we can use streams as communication channels for passing data between two or more objects, and many other use cases.

A great definition of `observable` is available on the Rx.JS 5 documentation:

> 可观察值是一种延迟求值的计算，从它被调用时起，可以同步或异步地返回零到(可能)无穷大的值。([`bit.ly/2sWVEAf`](http://bit.ly/2sWVEAf)

In order to consume data emitted by an `observable`, we need to create an `observer` (consumer); this subscribes to the `observable` and reacts every time a value is emitted by the `observable` (producer). If we want to summarize in technical words what an `observable` is, we could say that an `observable` is an object that wrap some data consumed by an `observer` (an object with a specific contract) and once instantiated provides a cancellation function. The `observer` is an object that allows us to retrieve the value emitted by an `observable` and has a specific contract exposing three methods: `next`, `error`, and `complete` functions. When you deal with `observable`s and `observer`s bear in mind two well-known design patterns for fully understand their mechanisms: the observer and the iterator patterns.

# 观察者模式

The Observer Pattern is a behavioral pattern where an object called Subject maintains a list of other objects (observers) that want to be notified when a change happens inside the program. The observers are usually subscribing to a change and then every time they receive a notification, they verify internally if they need to handle the notification or not. Usually, in typed languages, the Observer Pattern is composed by a subject and one or multiple observers. The subject is handling the subscription, unsubscription, and the notification to an observer; then each observer implements a specific interface that contains a publish method (`update`) for reacting to a notification originated by another object or a user interaction and shared through the Subject (Figure 3-4).

![A446957_1_En_3_Fig4_HTML.jpg](img/A446957_1_En_3_Fig4_HTML.jpg)
Figure 3-4 Observer Pattern UML

## 迭代器模式

# 迭代器模式

迭代器模式是一种行为模式，用于遍历数据容器，例如数组或对象。作为 JavaScript 开发者，我们应该熟悉这种模式，因为它在 ECMAScript 2015 中被引入。该模式允许一个对象通过调用一个方法（`next`）来遍历，从当前值获取后续值（如果存在）。迭代器模式通常暴露 `next` 方法和 `hasNext` 方法，后者返回一个布尔值，用于检查被遍历的对象是否包含更多值。在 MDN 上，我们可以找到更多关于 ECMAScript 2015 中官方 JavaScript 实现的信息：[`mzl.la/2sfrMxJ`](https://mzl.la/2sfrMxJ)（图 3-5）。

![A446957_1_En_3_Fig5_HTML.jpg](img/A446957_1_En_3_Fig5_HTML.jpg) 图 3-5 迭代器模式 UML

## 将守则付诸实践

让我们通过代码示例来更好地理解可观察者/观察者关系在实践中是如何工作的：

```javascript
const observable = Observable.range(1, 10);
const observer = observable.subscribe(onData, onError, onComplete);

function onData(value) {
    console.log(value);
}

function onError(err) {
    console.error(err);
}

function onComplete() {
    console.log("stream complete!");
}
```

从这个第一个例子中可以看到，第一行我们创建了一个包含从 1 到 10 的值范围的可观察对象；这意味着当观察者订阅它时，这些值已准备好被发出。第二行我们订阅了可观察对象；在许多库中，`subscribe` 方法有三个参数，分别对应于在可观察对象发出值、错误或完成信号时被调用的回调函数。在代码的其余部分，我们只是对可观察对象发出的信息做出反应，将输出打印到控制台，该输出可能是来自流的事件、错误或完成信号。当可观察对象接收事件时，它也允许我们操作数据以提供不同的输出，就像我们之前在大理石图中看到的那样，我们映射每个发出的值，将每个值增加 1。

不同的响应式库提供不同的工具集，但在本书撰写时，最完整的无疑是 Rx.js。该库提供了一组操作符，不仅可以操作可观察对象发出的事件，还可以与其他可观察对象一起操作；将可观察对象的可观察对象转换为扁平的可观察对象并不少见：可以将它们视为数组的数组。这些操作符将允许我们扁平化对象嵌套，直接访问可观察对象中的值，而无需迭代的复杂性。我们将在下一节探索不同的响应式库时看到操作符提供的可能性，以便我们实际了解如何使用它们以及我们可以用它们做什么。显然，我们将只回顾最常用的操作符，因为实际上有数百种，而我们的重点是理解它们背后的机制，而不是探索整个库。

## 流实现

既然我们已经理解了什么是流，现在是时候更好地了解前端响应式生态系统中有什么可用了。我们要考虑的第一个库是 Rx.JS 5。

## RxJS

Rx.JS 是目前最著名且使用最广泛的响应式库；它几乎无处不在，从将库集成到框架内部的 Angular 2，到许多其他正在拥抱并利用其强大功能的中小型框架。它是功能最完整的响应式库之一，拥有众多操作符和优秀的文档。Rx.JS 是 Reactive Extensions（[`reactivex.io`](http://reactivex.io)）的一部分。学习它意味着能够使用相同的范式（通常是相同的 API）在不同语言之间切换。Rx.JS 既可用于前端应用，也可用于基于 Node.js 的后端应用；显然，其异步特性对应用开发的两端都有帮助。目前有两个主要版本可用。版本 4 是 Reactive Extension 的第一个 JavaScript 移植版本，而最近另一个实现开始兴起，即版本 5。这两个库在我们的示例中存在一些差异，因此我们将使用版本 5。在本节中，我们无法尝试 Rx.JS 中所有可用的操作符，因为这个库足够庞大，可以单独写一本书（事实上，有很多相关书籍，我强烈建议你去读一读）。我们的主要目标是掌握几个关键概念，考虑到我们将在本章和下一章的许多其他示例中使用它。因此，我们将在三个不同的场景中实际运用 Rx.JS：

*   当我们需要减少数据以获取最终值时。
*   当我们从 HTTP 端点获取数据时。
*   当我们需要通过可观察物与多个物体交流时。

Considering how large Rx.JS is, we introduce another important concept such as the difference between hot and cold observables, to help to understand better how the other libraries work. Let’s start with a simple example on reducing an array of data and retrieve the sum of values filtering the duplicates present in the initial array.

```javascript
import Rx from "rxjs"

const data = [1,2,10,1,3,9,6,13,11,10,10,3,19,18,17,15,4,8,4];
const onData = (value) => console.log(`current sum is ${value}`);
const onError = _ => console.log("stream error");
const onComplete = _ => console.log("stream completed");

const obs = Rx.Observable.from(data)
                       .filter(value => value % 2 === 0)
                       .distinct()
                       .reduce((acc, value) => acc + value);

obs.subscribe(onData, onError, onComplete);
```

As you can see in the example above, we are converting an array of numbers to an observable (`Rx.Observable.from(data)`), then we start to transform the values inside the array step by step, applying multiple transformations. In fact, first we are filtering the values creating a new array containing only the even numbers, then we remove all the duplicates inside the array with the `distinct` operator provided by Rx.JS; and finally we sum the values inside the array with the `reduce` operator. Figure 3-6 shows the final output in the browser console.

![A446957_1_En_3_Fig6_HTML.jpg](img/A446957_1_En_3_Fig6_HTML.jpg)

Figure 3-6 Final result returned by the previous snippet with Rx.JS

Every time we are applying a transformation via an operator, we are creating a new observable that is returned at the end of the operation. This means we can concatenate multiple operators by applying several transformations to the same data source. In order to output the final result of our observable, we are subscribing to the observable with the `subscribe` method, and this method accepts three functions. The first one is triggered every time the observable emits data, the second one if the observable emits an error, and the last one is triggered once the observable receives a complete signal from the producer (in our case, the end of the array). Remember that these callbacks are not all mandatory; we can potentially skip to declare the error and complete callbacks if we don’t need to react to these events.

Imagine for a moment how you would implement the same logic in imperative programming… done it? Ok, now you probably understood how functional and reactive paradigms can help to express complex operations in few lines of code, having a clear idea of what’s happening, without storing temporary values anywhere, and without generating any side effects. Everything is contained inside the observables and cannot be modified from external operations.

The next example, instead, aims to retrieve the response from an API on the Web and then propagate the result to a hypothetical view. In this case we won’t use any specific architecture but we are going to work well with the single responsibility principle and good encapsulation.

```javascript
import Rx from "rxjs";

const URL = "https://jsonplaceholder.typicode.com/users";

const simplifyUserData = (user) => {
    return {
        name: user.name,
        email: user.email,
        website: user.website
    }
}

const intervalObs = Rx.Observable.interval(1000)
                                 .take(2)
                                 .mergeMap(_ => fetch(URL))
                                 .mergeMap(data => data.json())
                                 .mergeAll()
                                 .map(simplifyUserData)

intervalObs.subscribe(user => {
    console.log(`user name is ${user.name}`);
    console.log(`user email is ${user.email}`);
    console.log(`user website is ${user.website}`);
    console.log('------------------------------');
}, error =>  console.error("error", error),
   complete => console.log("completed"))
```

After creating the constant with the `URL` to consume, we are creating a method (`simplifyUserData`) for filtering the data we want to use in our application, by just returning a subset of the information instead of the entire record retrieved from that URL. The endpoint used is a public endpoint usually used for mocking data, but in our case we are going to receive an array of objects that looks like this:

```json
{
  "id": 1,
  "name": "Leanne Graham",
  "username": "Bret",
  "email": "Sincere@april.biz",
  "address": {
    "street": "Kulas Light",
    "suite": "Apt. 556",
    "city": "Gwenborough",
    "zipcode": "92998-3874"
    [...]
  }
}
```

We want to consume this endpoint every second but only twice during the application life cycle. In order to do that we create an observable with an interval of a second (`Rx.Observable.interval`), specifying how many times we want to perform this operation (`take` operator) and then we want to fetch the data from a URL (first `mergeMap`) and then return the data fetched as JSON, splitting the object retrieved (`mergeAll` operator, we could have use other operators like `concatAll`, for instance, obtaining the same result) in order to emit a value per time instead of the entire array in one go. Finally we simplify the data with the method we created at the beginning of the script (`map` operator).

The final output of this example should look like Figure 3-7.

![A446957_1_En_3_Fig7_HTML.jpg](img/A446957_1_En_3_Fig7_HTML.jpg)

Figure 3-7 Output of previous example with Rx.JS

Now we need to understand in depth why we have done all these operations for manipulating the data to get the result displayed in the screenshot above. After creating the interval observable, we perform at each tick a `fetch` operation. `fetch` is not an observable but, as you can see from the example, we are not transforming it via an operator for translating it to an observable, but we are using the operator `mergeMap` and inside there we are performing the `fetch` operation.

这是因为`fetch`是一个承诺A+，这些承诺被Rx识别。所以不需要像标准承诺一样使用`fromPromise`操作符，我们可以在`mergeMap`操作符创建的可观察对象中直接使用它们。

`MergeMap`是一个做两件事的操作符。第一种方法是将两个流合并成一个唯一的流，然后用流内部发出的值进行迭代：在这种情况下，为了返回一个带有服务响应的流，它将承诺变平。

第二个`mergeMap`操作符与另一个promise一起使用，我们指定该流中发出的返回值应该是从端点获取的数据的JSON表示。

第二个承诺是由于`fetch` API契约，正如你在MDN规范中看到的：[`bit.ly/2sWVEAf`](http://bit.ly/2sWVEAf)。

最后一个操作符是`mergeAll`操作符，它通常用于合并一个可观测量中的所有可观测量，但在这种情况下，它将包含数组的最后一个承诺与检索到的数据合并，并以迭代的方式发出数组的每个单个值，而不是将整个数组作为唯一值发出，从而允许我们使用最后一个操作符(`map`)来简化发出的数据。

很容易理解Rx是多么的多才多艺和强大。JS可能就是这种情况。显然，在这个阶段，我们知道需要做一些工作来熟悉Rx提供的所有不同运营商。但不要太难过，因为这是我们在掌握反应范式之前都做过的一步。

在回顾另一段代码之前，我们需要解释可观测量的另一个关键概念：当一个可观测量是热的还是冷的时，这意味着什么。

## 热和冷的可观测量

我们可以有两种不同类型的可观测量：热的和冷的。冷的可观测量本质上是惰性的和单播的；它只有在有人订阅它时才开始发出值。相反，热的可观测量可以在有人监听之前就发出事件，而无需等待触发器来启动其操作；它们在设计上也是多播的。我们为了识别热和冷的可观测量而应该理解的另一个重要特征是理解生产者在两种场景中是如何处理的。在冷的可观测量中，生产者位于可观测量本身内部；因此，每次我们订阅一个冷的可观测量时，生产者都会被一次又一次地创建。相反，在热的可观测量中，生产者是唯一的，并在多个观察者之间共享；因此，我们每次订阅时都会收到新鲜的值，而不会从第一个发出的值开始就收到所有值。显然，在热的可观测量中有方法可以创建一个值缓冲区，以便在每次观察者订阅时发出，但我们现在不打算评估每一个单独的场景。让我们看看一个热的和一个冷的可观测量的实际应用，以便更好地理解如何使用Rx.JS的这两种类型的对象。

### 冷可观察

理解冷的可观测量的最佳方式是看它的实际应用：

```javascript
import Rx from "rxjs";

const source = Rx.Observable.interval(2000).startWith(123)

source.subscribe(value => console.log("first observer", value))

setTimeout(_ =>{
    source.subscribe(value => console.log("second observer", value))
}, 5000);

setTimeout(_ =>{
    source.subscribe(value => console.log("third observer", value))
}, 8000)
```

这个小例子的输出如图3-8所示。

![A446957_1_En_3_Fig8_HTML.jpg](img/A446957_1_En_3_Fig8_HTML.jpg)

图3-8 使用Rx.JS的冷可观察输出

在这个例子中，我们创建了一个每2秒发出一个顺序值的可观测量：它将从0开始直到无限，因为我们没有指定在事件流完成之前要发出多少个值。当我们想在用户界面中显示一个初始值或在不等待异步传递的值的情况下启动一系列事件时，我们使用`startWith`操作符。在冷的可观测量中，我们有生产者，在这个例子中是发出顺序数字的可观测量，它被实例化了三次——基本上每次消费者订阅它时都会实例化。在上图中，你可以清楚地看到数字序列每次在消费者订阅生产者时都会重新开始。我们可以得出结论，冷的可观测量在每次消费者订阅时都会重新实例化生产者，并且它是单播的，因此产生的值一次只被一个消费者监听；同时，我们可以有多个消费者订阅同一个生产者。默认情况下，我们在Rx.JS中创建的所有可观测量都是冷的，但我们有不同的方法将它们转换为热的可观测量。

### 可观察到的热

Let’s see what a hot observable looks like:

```javascript
import Rx from "rxjs";
const source = Rx.Observable.interval(2000)
    .startWith(123)
    .publish()
    .refCount();

source.subscribe(value => console.log("first observer", value))

setTimeout(_ => {
    source.subscribe(value => console.log("second observer", value))
}, 5000);

setTimeout(_ => {
    source.subscribe(value => console.log("third observer", value))
}, 8000)
```

The output is shown in Figure 3-9. The example is very similar to the cold observable one, but in this case we are using other two operators: `publish` and `refCount`.

![A446957_1_En_3_Fig9_HTML.jpg](img/A446957_1_En_3_Fig9_HTML.jpg)

Figure 3-9 Output of hot observable with Rx.JS

`Publish` operator is useful to transform our cold observables to hot observables because it returns a `ConnectableObservable` instead and observable object. A `ConnectableObservable` starts emitting values only when a `connect` method is called or, like in our example, when we use `refCount` operator.

`RefCount` operator is used when we don’t need to control the start and stop of the `ConnectableObservable` but instead we automate this process; when a consumer subscribes to a `ConnectableObservable`, it is the logic provided by `refCount` that will trigger the `connect` method for emitting the values to the subscribers. Also the `refCount` logic will unsubscribe once there are any subscribers ready for receiving new values.

When we have a hot observable the producer becomes multicast and the values emitted are shared across all the subscribers but at the same time we need to remember that by default, it’s not waiting for any consumer to subscribe. Instead it is emitting values immediately after the `connect` method is called.

There are operators that will allow you to control when a hot observable starts to emit values like `multicast` and `connect` instead of `refCount`, which is automating these steps. Just keep this in mind when you work with Rx.JS because there are many opportunities available with this great library so keep an eye on the documentation and the implementation will become very smooth.

Understanding what we are subscribing to and what are the characteristics and benefits of a hot or a cold observable could save a lot of debugging time and many headaches once our code hits the production stage.

I think it is clear that Rx.JS is not just that – it’s way more than that but with these simple examples we are trying to memorize a few useful key concepts that will facilitate the integration of this library in our existing or new applications.

Now it’s time to see another reactive library flavor with XStream. It’s important to understand that the concepts showed in the Rx.JS sections are very similar in other libraries; therefore owning them will allow you to pick the right library for the a project.

### X 流

XStream 是由 André Staltz 创建的最新响应式库之一，它提供了一种简单的方法来处理专为 Cycle.js 量身定制的响应式编程。XStream 利用了我们已经在 Rx.JS 中见过的一些概念，如可观察对象，但简化了流背后的核心概念。所有的流都是热可观察对象，使用此库无法创建冷可观察对象。我个人认为，作者在开发这个库时考虑了响应式编程的学习曲线，并试图平滑入门门槛，以便新手和专家都能更容易上手。此外，XStream 非常快，仅次于 Most.js（另一个响应式库），而且非常轻量，大约 30kb；它提供的操作符少于 30 个，是一个处理可观察对象的超基础库，非常适合任何需要使用可观察对象但又不需要其他库提供的众多“便利”操作符的原型或项目。XStream 使用流的概念而不是可观察对象，流是带有多个监听器的事件发射器。监听器是一个具有特定契约的对象，它有三个公共方法：`next`、`error` 和 `complete`；顾名思义，监听器对象正在监听流发出的事件。将流和监听器与 Rx.JS 的可观察对象和观察者进行比较，我们可以说流的作用类似于热可观察对象，而监听器则模仿了相同的实现，类似于观察者。最后但同样重要的是，在 XStream 中，我们可以使用生产者来生成通过流广播给多个对象的事件。生产者控制流的生命周期，根据其便利性发出值。生产者具有特定的签名，并暴露两个主要方法：`start` 和 `stop`。我们稍后将看到一个介绍生产者概念的示例。现在是时候看看 XStream 的实际应用了，我们将之前在 Rx.JS 部分介绍过的示例移植过来。第一个示例与从值数组中提取偶数、去除重复项并计算它们的总和有关：

```javascript
import xs from 'xstream';

const data = [1,2,10,1,3,9,6,13,11,10,10,3,19,18,17,15,4,8,4];

const filterEven = (value) => value % 2 === 0;

const removeDuplicates = (inputStream) => {
    const buffer = [];
    const outputStream = xs.fromArray(buffer);
    inputStream.addListener({
        next(value){
            if(buffer.length === 0 || buffer.indexOf(value) < 0)
                buffer.push(value);
        }
    })
    return outputStream;
}

const sumValues = (acc, value) => acc + value;

const listener = {
    next(value){
        console.log(`current sum is ${value}`);
    },
    error(){
        console.error("stream error");
    },
    complete(){
        console.log("stream completed");
    }
}

const stream = xs.fromArray(data)
                 .filter(filterEven)
                 .compose(removeDuplicates)
                 .fold(sumValues, 0);

stream.addListener(listener);
```

`data` 对象包含一个无序整数数组，其中有重复值以及偶数和奇数。我们这个练习的目标是过滤数组，只检索偶数，在第一次转换后去除重复项，最后计算最终总和。在实例化数组源之后，我们定义了要应用的转换；第一个是用 `filterEven` 方法从初始数组中过滤偶数。在这个方法中，我们检查每个要接收的值是否能被 2 整除。如果是，意味着该值是偶数，我们想保留它，否则我们将跳过它（记住流一次只会发出 1 个值）。第二个方法是 `removeDuplicates`，但在 XStream 中没有像 Rx.JS 那样自动执行此操作的操作符。正如我们在开头所说，XStream 旨在学习如何处理流，而不是提供一个包含许多操作符的完整库。因此，我们将使用 `compose` 操作符，它返回一个流并期望一个新的流作为输入。`outputStream` 将是我们的新流，供下一个操作符使用，它发出在 `removeDuplicates` 方法内生成的数组。在 `removeDuplicates` 内部，我们创建一个数组来存储唯一值，并且仅当它们不在数组中时才将它们推入。这个练习的最后一个转换是计算前面步骤中过滤后的值的总和。我们将使用 `fold` 操作符，它需要一个带有两个参数的函数：一个累加器和当前值，用于计算最终结果，这与你想计算数组内值时使用的 `reduce` 方法非常相似。最后，我们可以使用 `xs.create` 创建流并传入初始数组，这将产生一个发出数组值的初始流。之后，我们通过 XStream 操作符（如 `filter`、`compose` 和 `fold`）应用所有不同的转换。请记住，`fold` 操作符的第二个参数是累加器的初始值，在我们的例子中，我们希望从值零开始并求和所有其他值。最后一步是通过创建一个监听器来监听流。正如我们在本节开头所说，监听器只是一个具有特定签名（`next`、`error`、`complete`）的对象，在我们的例子中，我们将流内部的转换输出到控制台。打开你最喜欢的浏览器的开发者工具，你应该会看到类似图 3-10 的内容。![A446957_1_En_3_Fig10_HTML.jpg](img/A446957_1_En_3_Fig10_HTML.jpg) 图 3-10 XStream 示例的输出，由于 `fold` 操作符，与 Rx.JS 示例略有不同。正如我们所见，这个用 XStream 制作的例子与我们之前用 Rx.JS 做的例子相似。这里的关键要点是，理解可观察对象的工作原理有助于我们从一个响应式库切换到另一个，而无需花时间学习新概念，只需应用一些具有不同语法和操作符的基本概念。我们的第二个示例基于每隔几秒从 REST 端点检索一些数据。在这种情况下，我们可以利用 XStream 中可用的生产者对象来获取远程数据并将结果发射到流中。考虑到我们希望每隔几秒检索一定次数的数据，我们可以使用间隔流，而不是像在普通 JavaScript 中那样使用 `setInterval`。这是我们的代码示例：

```javascript
import xs from "xstream";
import fetch from "fetch";

const URL = "https://jsonplaceholder.typicode.com/users";

const producer = {
    emitter(){
        const that = this;
        return {
            next(){
                emitUserData(listener);
            },
            complete(){
                that.stop()
            }
        }
    },
    start(listener){
        xs.periodic(5000).take(2).addListener(this.emitter())
    },
    stop(){
        listener.complete();
    }
}

const emitUserData = (listener) => {
    fetch.fetchUrl(URL, (error, meta, body) => {
        if(error) return;
        const data = JSON.parse(body.toString());
        data.forEach(user => {
            listener.next(user)
        }, this);
    })
}

const simplifyUserData = (user) => {
    return {
        name: user.name,
        email: user.email,
        website: user.website
    }
}

const listener = {
    next(user){
        console.log(`user name is ${user.name}`);
        console.log(`user email is ${user.email}`);
        console.log(`user website is ${user.website}`);
        console.log('------------------------------');
    },
    error(){
        console.error("stream error");
    },
    complete(){
        console.log("stream completed");
    }
}

const userStream = xs.create(producer).map(simplifyUserData);

userStream.addListener(listener);
```

正如你在上面的示例中看到的，我们首先定义了生产者对象；请记住，生产者只是一个需要两个方法的对象：`start` 和 `stop`。`start` 方法将在第一个消费者订阅流时被调用。请记住，一个生产者一次只能有一个监听器；因此，为了将结果广播给多个监听器，我们必须创建一个使用生产者来发射值的流。我们的生产者还包含另一个名为 `emitter` 的方法，它返回一个监听器对象，我们将在 `start` 方法中创建的间隔流中使用它。`start` 方法使用 `xs.periodic` 操作符，它接受一个时间间隔作为参数，流会在此间隔发出事件；所以在我们的例子中，每 5 秒会发出一个新事件。我们还使用了 `take` 操作符，用于从该流中检索一定数量的值，忽略所有其他发出的值。最后要做的是用监听器订阅该流，并且每次滴答（`next` 方法）都从端点获取数据。该端点与 Rx.JS 中的前一个示例相同，因此我们需要期望从端点获取相同的 JSON 对象。这个示例的主要目标是输出这个数据的简化版本，该版本可用于我们应用程序的假设视图中。当我们创建 `simplifyUserData` 方法以仅从流中发出的值中提取我们需要的信息时；此函数返回一个过滤后的对象，仅包含几个字段，而不是整个集合。之后，我们创建监听器对象，其中包含典型的 `next`、`error` 和 `complete` 方法签名，我们在其中处理流发出的值。最后，我们通过使用 `xs.create` 创建流并传入我们的生产者作为参数，来创建流和监听器对象之间的粘合剂。然后我们遍历所有发出的值，过滤用户数据，并在脚本的最后一行将监听器关联到流，这将触发生产者开始发出值。在这种情况下，与 Rx.JS 示例相比存在一些差异，但关键概念仍然存在。XStream 库的最后一个示例侧重于我们如何将值广播给多个监听器对象；在这种情况下，XStream 对我们很有帮助，因为所有的流都是热的，因此本质上是多播的。我们不需要执行任何操作或了解我们正在处理哪种流。这也是为什么我建议总是从像 XStream 这样简单的库开始，它包含了我们开始动手处理流所需的一切，然后再转向像 Rx.JS 或 Most.js 这样更完整的工具箱库。

```javascript
import xs from "xstream";

const periodicStream = xs.periodic(1000)
                         .map(_ => Math.floor(Math.random()*1000) + 100);

periodicStream.addListener({
    next(value){
        console.log("first listener", value);
    }
})

setTimeout(_ =>{
    periodicStream.addListener({
        next(value){
            console.log("second listener", value);
        }
    })
}, 3000);

setTimeout(_ =>{
    periodicStream.addListener({
        next(value){
            console.log("third listener", value);
        }
    })
}, 6000);
```

首先要做的是创建一个流（`periodicStream` 常量），它每秒发出一个随机数。然后每次我们向主流添加一个新的监听器时，每个监听器都会接收到 `periodicStream` 发出的值。检查浏览器控制台的输出，我们可以看到流的工作方式；请记住，我们说过 XStream 中的任何流都是热的并且是多播的。因此，我们将只有一个生产者，它不会被重新创建，但每次有新的监听器订阅时，我们都会从那个时刻开始接收到发出的值（图 3-11）。![A446957_1_En_3_Fig11_HTML.jpg](img/A446957_1_En_3_Fig11_HTML.jpg) 图 3-11 使用热流的 XStream 示例输出。现在我们可以看到预期的行为，当第二个和第三个监听器订阅流时，它们接收到发出的值，没有任何关于之前值的信息，就像我们在 Rx.JS 的冷可观察对象示例中看到的那样。从最后一个示例可以清楚地看出，如果你需要在项目中以某种粒度管理流类型，Rx.JS 将是正确的选择。否则，XStream 可以简化并加速你的开发，因为它非常直接。

## 背压

另一个重要的响应式编程概念是背压（backpressure），以及我们如何利用它来改进响应式应用。当我们处理多个流时，它们可能在短时间内发出大量事件。因此，如果我们不需要所有数据，或者处理这些数据的计算过程过于密集，导致消费者无法跟上，我们就需要一种方法来减轻观察者消耗的数据量。通常，我们有两种处理应用中背压的可能性：首先，我们可以对值进行排队，创建一个缓冲区并处理所有接收到的值，这样我们就不会错过发出的值。这种策略被称为无损策略（loss-less strategy）。另一种策略可能是跳过一些事件，仅在一定时间后做出反应，过滤我们接收到的信息，因为这些信息可能对消费者需要做的事情并不关键；在这种情况下，我们称这种策略为有损策略（lossy strategy）。

想象一下，例如，我们使用`zip`操作符合并两个可观察对象。第一个可观察对象每秒提供一些大写字母，第二个可观察对象每200毫秒发出一个小写字母。在这种情况下，`zip`操作符将创建一个新的可观察对象，将两个流的值耦合在一起，但由于它需要耦合以不同速度发出值的不同流中的字母，在`zip`操作符内部，我们有一个缓冲区来存储第二个可观察对象的值，直到第一个可观察对象发出下一个值。图3-12应该阐明了我们的最终目标。

![A446957_1_En_3_Fig12_HTML.jpg](img/A446957_1_En_3_Fig12_HTML.jpg)
图3-12 背压示例

从上面的弹珠图可以看出，第二个流在相同时间内产生的值是第一个流的5倍，因此新的可观察对象需要将数据保存在缓冲区中以匹配值，然后再将它们发送给消费者。不幸的是，响应式编程中的这类场景并不少见，此时背压操作符就派上用场了。这些操作符允许我们通过简单地停止对可观察对象发出的值的反应来减轻观察者的压力，暂停接收值，直到我们定义何时恢复接收值。

让我们用`Rx.JS`写一个具体的例子，以便更好地理解所描述的概念。我们将用`Rx.JS`和React创建的是一个简单的方框，模拟一个实时接收数据的股票，并且需要在组件内显示股票价值（图3-13）。

![A446957_1_En_3_Fig13_HTML.jpg](img/A446957_1_En_3_Fig13_HTML.jpg)
图3-13 我们下一个关于背压的练习结果

这个小例子由两个文件组成：主应用程序和React组件。主应用程序将生成一个可观察对象，该对象将在特定范围内产生随机值，准备在组件内显示。

```javascript
import React from "react";
import ReactDOM from "react-dom";
import Rx from "rxjs";
import Stock from "./Stock.jsx";

export default class App{
    constructor(){
        const cont = document.getElementById("app");
        const observable = this.generateProducer();
        const AAPL = "AAPL - Apple Inc.";
        ReactDOM.render(<Stock producer$={observable} title={AAPL} />, cont);
    }

    generateProducer(){
        const stockValuesProducer = Rx.Observable.interval(50)
            .map(value => {
                return (Math.random() * 50 + 100).toFixed(2);
            })
        return stockValuesProducer;                       
    }
}

let app = new App();
```

在主应用程序文件中发生的是，我们正在生成一个生产者（`generateProducer`方法），它应该模拟与数据源的持续交互，并且每50毫秒发出一个介于100到150之间、保留两位小数的值。这是一个背压操作符可以提供帮助的典型例子；我们真的不需要每50毫秒更新一次UI，因为这与其说是有用的体验，不如说会创建一个持续的刷新，用户不会喜欢，而且在低端机器上会非常耗资源。那么，我们可以做些什么来减轻即将接收这些值的观察者的压力呢？如果在`generateProducer`方法中，我们不是直接返回可观察对象，而是像这样添加一个背压操作符：

```javascript
generateProducer(){
    const stockValuesProducer = Rx.Observable.interval(50)
        .map(value => {
            return (Math.random() * 50 + 100).toFixed(2);
        })
    return stockValuesProducer.sampleTime(500);
}
```

在这种情况下，`sampleTime`操作符将仅每500毫秒发出一个值，忽略在此期间接收到的其他值。为了更好地理解这个概念，图3-14展示了这在弹珠图中是如何工作的。

![A446957_1_En_3_Fig14_HTML.jpg](img/A446957_1_En_3_Fig14_HTML.jpg)
图3-14 显示`sampleTime`操作符与半秒防抖的弹珠图

在组件代码中，我们将订阅从`producer`属性接收到的可观察对象，并在UI中显示该值：

```javascript
import React from "react";

export default class Stock extends React.Component{
    constructor(){
        super();
        this.state = {stockValue: 0};
    }

    componentDidMount(){
        this.props.producer$.subscribe(this.setStockValue.bind(this));
    }

    setStockValue(value){
        this.setState({stockValue: value});
    }

    render(){
        return (
            <div className="stock">
                <h2>{this.props.title}</h2>
                <p>${this.state.stockValue}</p>
            </div>
        )
    }
}
```

正如我们在React组件生命周期提供的`componentDidMount`方法中所注意到的，我们正在订阅生产者（之前在主应用程序中创建的可观察对象），然后将股票价值设置在React状态对象中，以便在段落元素中显示它。

在`Rx.JS`中，我们可以使用多种背压操作符，如`debounce`或`throttle`，但还有许多其他操作符可以正确处理背压。重要的是要记住，当生产者发出大量数据时，不要创建巨大的数据缓冲区。因此，根据经验，记住当我们不需要可观察对象发出的所有数据时，我们应该真正过滤它们，以便为用户提供更好的用户体验并提高响应式应用程序的性能。

## 总结

在本章中，我们解释了如何处理几个关键的响应式概念；特别是，我们介绍了 `Rx.JS` 中的流、可观察对象和观察者概念，以及 `XStream` 中的流和监听器。我们还回顾了最著名的库实现，以及如何使用它们来操作我们的流。我们发现了什么是热观察者和冷观察者，以及当我们在流中接收到大量不需要全部共享的数据时，如何处理背压。现在，我们拥有了所有需要的工具，可以查看具体的实现，以及不同的框架/库如何在前端世界中引入响应式编程。显然，本书的目的不是深入探讨每个单一操作符或特定库，而是提供知识，让你能够轻松地从一个库切换到另一个库。本章中未提及的其他库也可以替代 `Rx.JS` 和 `XStream`，例如 `Most.js`（[`github.com/cujojs/most`](https://github.com/cujojs/most)）、`IxJS`（[`github.com/ReactiveX/IxJS`](https://github.com/ReactiveX/IxJS)）、`Kefir`（[`rpominov.github.io/kefir/`](https://rpominov.github.io/kefir/)）、`FlyD`（[`github.com/paldepind/flyd`](https://github.com/paldepind/flyd)）或 `Bacon.js`（[`baconjs.github.io/`](https://baconjs.github.io/)）。它们都有各自的优缺点，我强烈建议你查看每一个库，并为你的项目选择合适的库。

在接下来的几章中，我们将看到响应式编程的实际应用。具体来说，我们将探索 `Cycle.js` 以及这个简单的库如何通过其创新的 `MVI (Model-View-Intent)` 架构来处理函数响应式编程。然后，我们将介绍 `MobX`，一个在响应式前端世界中非常著名的库，它正日益流行，并以简单且响应式的方式解决应用状态管理问题。我们将以 `SAM Pattern` 结束本书，这是一种架构模式，提供了一个完全与框架无关的响应式结构，使我们能够将其集成到任何现有或全新的项目中。