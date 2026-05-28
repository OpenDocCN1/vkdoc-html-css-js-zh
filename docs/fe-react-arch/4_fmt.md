# 四、Cycle.js 和 MVI

> 我们不能用创造问题时的思维来解决问题。
> —Albert Einstein

In this chapter, we are introducing `Cycle.js`, a functional and reactive framework that is very interesting in the front-end panorama for the number of concepts ported from different languages and wired together from the knowledgeable mind of the creator and the Cycle community. `Cycle.js` (cycle.js.org) was created by André Staltz, one of the most famous and active advocate of the front-end reactive panorama; in fact, André is a contributor of `Rx.JS` and the mind behind `XStream` library. As we are going to see in the chapter, `Cycle.js` can be used in several ways. Its modularity guarantees great results in any project, providing a great level of encapsulation and enhancing the components’ reusability across multiple projects without the need of “copying and pasting” part of a specific flow from different files like in other framework implementations.

## Cycle.js 简介

The `Cycle.js` focus is on the interaction between computer and user, taking into consideration research on human-computer interaction and studies focused on the interfaces between human beings and computers. This interaction is a circular one between the inputs and outputs as shown in `Figure 4-1`.

![A446957_1_En_4_Fig1_HTML.jpg](img/A446957_1_En_4_Fig1_HTML.jpg)

`Figure 4-1` Interaction schema between human and computer

As we can see, a person can interact with a computer via its input methods (mouse, keyboard, microphone, touchpad…); for doing this, the person will use a hand or the voice as output. When the computer receives the input from the user, it will elaborate a new output related to the input received, providing it on the screen. The user then will be able to recognize and elaborate on the computer’s output via the user’s input sense (eyes or ears, for instance), understanding how his interactions affect the computer’s output, creating de facto, a circular interaction between computer and user: from here, the name of `Cycle.js`. We could summarize this interaction saying that between the human and the computer there is a dialogue where the two main actors are interacting with each other, reacting to the inputs and providing a new output. As we understand, this framework uses a different approach from the others we used to work with where the GUI is at the center; instead `Cycle.js` privileges more the interactions aspect over the graphical representation. In order to do that, `Cycle.js` introduces a message passing architecture where we send a message to an object and this one knows which part of the code to run based on the input received. This highly decoupled architecture stands on the opposite edge of the spectrum compared to more traditional architectures where dependency injection is heavily used and we favor objects interactions over reactivity. `Cycle` is not the only framework leveraging a message passing architecture; other examples could be retrieved in `Akka` with the actor-model architecture or in `CSP` (Communicating Sequential Processes) with channels. During the chapter, we will discover that using this approach will lead `Cycle.js` projects to a better separation of concerns and encapsulation, in favor of better components reusability across projects. But it’s time to see `Cycle` in action though, so let’s move to some concrete examples to truly appreciate the ideas behind this reactive framework.

## 构建简单的 Cycle.js 应用程序

# Cycle.js 核心概念

在使用 Cycle.js 时，我们需要牢记的第一个关键概念是：该框架明确地将应用逻辑与副作用分离。例如，假设我们希望在用户点击界面按钮后从某个端点加载数据。Cycle.js 将 DOM 渲染和远程数据获取，与准备 HTTP 请求以及处理要在 DOM 中展示的数据的逻辑完全分开；这样我们就可以专注于应用中真正重要的部分，而将实际操作 DOM 或使用 `HTTPRequest` 对象获取远程数据的任务委托出去。在研究一个简单的 Cycle 应用之前，我们需要先了解这个框架的构成。在 Cycle.js 中，有三个关键概念需要记住：

*   `纯函数`

*   `流`

*   `司机`

A pure function is a functional concept where a function can be defined as pure if it doesn’t create side effects inside the program, so when we pass a specific parameter it always returns the same output. We could summarize the pure function concept as a function where its output is determined only by the input values, and an example could be:

```javascript
function add(a, b) {
    return a + b;
}
```

In this case when I call the function `add` I receive a result based on the function’s arguments (`a` and `b`), and there is no possibility that external states or variables could affect the final result considering that our pure function is totally independent from the application where it is running.

Another key concept is the stream, but considering we extensively talked about them in the previous chapters we can move to the next concept: the drivers.

For better understanding the drivers, we need to analyze how Cycle.js is composed; otherwise we will struggle to catch why the drivers are used in this framework.

Cycle.js is a modular architecture composed of multiple libraries. The core library is really small and it exposes just one method used for creating the glue between the application logic and the side effects like a DOM state rendering or remote data connection. By default, Cycle.js uses `XStream` as a main stream library but it allows us to use other libraries like `Rx.JS`, `Most.js`, or even a custom one created by us.

We have already discussed how Cycle.js separates the application logic from the side effects: this is the key part for understanding the drivers. All the side effects in a Cycle application are handled by drivers, for instance, one of the most used ones is the DOM driver that performs the DOM manipulation received by the application logic that instead prepares a virtual DOM representation instead of interacting directly with the DOM. The communication between the drivers and the application is always made via observables; a driver can be a read and write or a read-only driver. The rule of thumb here is that a driver has always as input an observable but may or may not return an output.

If we want to draw the anatomy of a Cycle application we could use this example as a skeleton:

```javascript
import xs from 'xstream';
import {run} from '@cycle/run';
import {makeDOMDriver, p} from '@cycle/dom'

const main = sources => {
    const sinks = {
        DOM: xs.periodic(1000).map(v => p(`seconds: ${v}`))
    }
    return sinks;
}

const drivers = {
    DOM: makeDOMDriver('#app')
};

run(main, drivers);
```

Let’s analyze what we have in this basic example. After importing `XStream`, `Cycle run` function, and the `DOMDriver`, all the Cycle.js applications have a `run` function; a pure function, composed by a `sources` object; and a `sink` object as output that contains the logic for the side effects to be applied after our application finishes the elaboration. For instance, in this example we have a stream that every second is incrementing a variable and returning a virtual DOM, in this case a virtual paragraph object.

Taking a look at the DOM driver, we can see that as a parameter of the `DOMDriver` method we need to pass the HTML element to use for appending our future DOM elements created dynamically by our Cycle.js application.

The last, but essential, thing to do is calling the method `run` provided by the framework for creating the glue between the main function and the drivers. What the `run` method is doing is simple to explain; this method is creating a circular dependency between the main function and the drivers retrieving the output of each function and returning as a source of the other as we explained at the beginning of this chapter. Obviously the input and output are always object-containing observables and it’s where Cycle is really shining with its architecture.

An important piece of information that we didn’t mention before is related to the virtual DOM library used in Cycle.js. Cycle uses `Snabbdom` ([`bit.ly/2gtpUKP`](http://bit.ly/2gtpUKP)) out of the box, a JavaScript library leveraging similar concepts expressed in `React.js` like the Virtual DOM and good diffing algorithm, on top `Snabbdom` offers a modular and minimal implementation compared to `React` (only 200 lines of code). Performance wise, `Snabbdom` results faster and more performant than `React.js` version 15 and below; it’s heavily based on function and could be used with `JSX` syntax with `Babel` if we are familiar with `React.js`.

`Snabbdom` would require another chapter for explaining in depth the beauty of this Virtual DOM library, but because it’s not the main topic of this chapter we can get more information with these resources:

- `Snabbdom` on github: [`bit.ly/2gtpUKP`](http://bit.ly/2gtpUKP)
- `Snabbdom JSX`: [`bit.ly/2wxqDSE`](http://bit.ly/2wxqDSE)
- React-less Virtual DOM with `Snabbdom`: [`bit.ly/1QFVayF`](http://bit.ly/1QFVayF)

Now let’s try to see a more complete example where we are going to create a simple weather forecast application that allows the user to search for a specific city and retrieve the weather forecasts for the next five days, and the UI looks like `Figure 4-2`.

![A446957_1_En_4_Fig2_HTML.jpg](img/A446957_1_En_4_Fig2_HTML.jpg)

`Figure 4-2` Cycle.js Project UI

This example will follow us for the entire chapter and we will refine it with two different approaches in order to explore properly the different possibilities offered by Cycle.js.

Let’s start to list what we need to do in order to create the weather application with Cycle.js:

*   我们需要使用两个驱动程序:一个用于 DOM 操作，另一个用于从远程 REST 服务获取数据。
*   我们需要创建一个带有按钮的输入字段，允许用户搜索特定的城市。
*   我们需要请求将数据呈现给天气预报服务(在本例中是第三方服务)。
*   我们需要创建一个 UI，顶部有一个标题，突出显示当天的预测，以及接下来几天的列表。

The first thing to create is the typical Cycle skeleton application with a `run` method, the drivers, and the main function:

```javascript
const main = sources => {
    // here there will be our application logic
}

const drivers = {
    DOM: makeDOMDriver('#app'),
    HTTP: makeHTTPDriver()
};

run(main, drivers);
```

As planned, we have a DOM driver (`makeDOMDriver`) that will manipulate the DOM inside the `div` with id `app` and the HTTP driver that instead will perform the request to the weather forecast’s endpoint. That means in our `main` function we are going to return an object with two observables: one for the endpoint request providing which city the user is interested on, and one with the virtual DOM of our page. Then the drivers will take care to perform the actions for us.

Let’s go ahead creating our application view, for instance, if we want to create the input field with the button shown in the application picture presented before, we need to create a function called `getForm` that will return to us the virtual DOM version of our elements:

```javascript
const getForm = () => div(".form", [
    input("#location-input"),
    button("#location-btn", "get forecasts")
])
```

Now we can observe for changes happening in both interactive elements in order to capture the text inserted by the user in the input field and when the user clicks the button for retrieving the forecast. In order to do that we are going to add these few lines in our `main` function:

```javascript
const input$ = sources.DOM.select("#location-input").events("focusout")
    .map(evt => evt.target.value);

const btn$ = sources.DOM.select("#location-btn").events("mousedown");
```

Remember that everything can be a stream when we work with reactive programming; therefore once the driver will render our interactive elements in the real DOM, it will provide us access to the real DOM available in the `DOM` object and we are able to observe the user interactions, thanks to the APIs provided by the DOM driver.

Every time the user will click the button we will need to retrieve what he typed and prepare the request URL and the query string for allowing the HTTP driver to perform the real request. Because we need to react when the user clicks the button but also to understand what the user wrote in the input field, we are going to combine the two streams in a unique one, and we prepare the URL with the new parameters any time the producer is producing new values, so in the `main` function we will add:

```javascript
const merged$ = xs.combine(input$, btn$);

const request$ = merged$.map(([city, mouseEvt]) => getRequest(city))
    .startWith(getRequest(INIT_CITY))
```

And we then create `getRequest` function that returns the composed URL:

```javascript
const getRequest = city => {
    return {
        url: `http://api.apixu.com/v1/forecast.json?key=04ca1fa2705645e4830214415172307&q=${city}&days=7`,
        category: CATEGORY
    }
}
```

The `request$` stream will be the one that we are going to pass to the HTTP driver, and this one will perform the real HTTP request for us, as you can see Cycle is separating the application logic from the side effect, defining what the application should do from how to perform the real effect.

In the combined stream, we can spot that there is a `startWith` method that returns a default city, in our case London, just for providing some information to the user the first time that accesses our weather application and he didn’t interact with our input field yet.

It’s time to handle the response once the HTTP driver receives it; inside our `main` function again we are going to retrieve the HTTP object exposed by the driver, and we are going to prepare the data for a set of functions for generating the virtual DOM based on the data retrieved by the HTTP driver.

```javascript
const response$ = sources.HTTP.select(CATEGORY)
    .flatten()

const vdom$ = response$.map(parseResponse)
    .map(simplifyData)
    .map(generateVDOM)
    .startWith(h1("Loading..."))
```

And outside our `main` function we then create the functions needed for generating the UI:

```javascript
const parseResponse = response => JSON.parse(response.text);

const simplifyData = data => {
    return {
        city: data.location.name,
        current: data.current,
        forecast: data.forecast.forecastday
    }
}

const generateVDOM = data => div(".main-container", [
    h1(`Your forecasts for ${data.city}`),
    getForm(),
    generateCurrentForecast(data.current),
    generateNext5Days(data.forecast)
])
```

As you can see in the `main` function, once we receive the response we need to select which one we handle, in this case the `CATEGORY` one described at the beginning of our application. Then we need to flatten the result because the HTTP driver returns always a stream of streams, so if we want to manipulate the data in this complex structure we need to create a flat stream (`flatten` method).

For creating the virtual DOM that will be passed to the DOM driver, we need now to do the following:

1. 解析响应并返回一个 JSON 对象（`parseResponse` 方法）。
2. 只提取我们的 UI 需要的数据，以便呈现最终结果（`simplifyData` 方法）。
3. 生成传递详细数据的虚拟 DOM（`generateVDOM` 方法）。

These three operations are generating a final stream with the virtual DOM that will be rendered via the DOM driver. The last bit of our main function is what it returns, so a sink object containing a stream for the DOM driver and one for the HTTP driver that represent the output of our Cycle application. This is the final implementation of our first Cycle example:

```javascript
import xs from 'xstream';
import {run} from '@cycle/run';
import {makeDOMDriver, div, h1, h2, h3, img, p, input, button} from '@cycle/dom';
import {makeHTTPDriver} from '@cycle/http';
import debounce from 'xstream/extra/debounce'
import moment from 'moment';

const CATEGORY = "forecast";
const INIT_CITY = "London";

const getForm = () => div(".form", [
    input("#location-input"),
    button("#location-btn", "get forecasts")
])

const generateNext5Days = forecasts => {
    const list = forecasts.map(forecast => {
        return div(".forecast-box", [
            h3(moment(forecast.date).format("dddd Do MMM")),
            p(`min ${forecast.day.mintemp_c}°C - max ${forecast.day.maxtemp_c}°C`),
            img(".forecast-img", {
                props: {
                    src: `http:${forecast.day.condition.icon}`
                }
            }),
            p(".status", forecast.day.condition.text)
        ])
    });
    return div(".forecasts-container", list)
}

const generateCurrentForecast = forecast => div(".current-forecast-container", [
    div(".today-forecast", [
        img(".forecast-img", {
            props: {
                src: `http:${forecast.condition.icon}`
            }
        }),
        p(".status", forecast.condition.text)
    ]),
    h3(moment(forecast.last_updated).format("dddd Do MMMM YYYY")),
    h2(`${forecast.temp_c}°C`),
    p(`humidity: ${forecast.humidity}%`)
])

const parseResponse = response => JSON.parse(response.text);

const simplifyData = data => {
    return {
        city: data.location.name,
        current: data.current,
        forecast: data.forecast.forecastday
    }
}

const generateVDOM = data => div(".main-container", [
    h1(`Your forecasts for ${data.city}`),
    getForm(),
    generateCurrentForecast(data.current),
    generateNext5Days(data.forecast)
])

const getRequest = city => {
    return {
        url: `http://api.apixu.com/v1/forecast.json?key=04ca1fa2705645e4830214415172307&q=${city}&days=7`,
        category: CATEGORY
    }
}

const main = sources => {
    const input$ = sources.DOM.select("#location-input").events("focusout")
        .map(evt => evt.target.value);
    const btn$ = sources.DOM.select("#location-btn").events("mousedown");
    const merged$ = xs.combine(input$, btn$);
    const request$ = merged$.map(([city, mouseEvt]) => getRequest(city))
        .startWith(getRequest(INIT_CITY))
    const response$ = sources.HTTP.select(CATEGORY)
        .flatten()
    const vdom$ = response$.map(parseResponse)
        .map(simplifyData)
        .map(generateVDOM)
        .startWith(h1("Loading..."))
    return {
        DOM: vdom$,
        HTTP: request$
    }
}

const drivers = {
    DOM: makeDOMDriver('#app'),
    HTTP: makeHTTPDriver()
};

run(main, drivers);
```

It’s important to highlight a couple of things in this example. First of all, in our main function we are handling the input and the output of our application; we are not operating any real side effects that, instead, are delegated to the drivers. The drivers and the main applications are communicating via streams; remember that Cycle.js is a message passing architecture, and this approach facilitates the data flow of our applications maintaining a high separation between application logic and side effects and a strong encapsulation. For the first time in this book, we are looking to a reactive implementation where the communication between different parts of our architecture are made by streams; interestingly there isn’t any knowledge in our application on how a driver is going to handle the side effects and we are not calling any specific method exposed by a driver. There is just a circular dependency between our main function and the driver that communicates only via streams. It’s important to iterate again these concepts because they will become very useful from now on considering we are going to discover MVI (model view intent), a reactive architecture heavily based on them.

## 模型视图意图架构

如果你熟悉 ELM 语言及其架构，MVI 不会让你感到惊讶，但我们必须承认，这绝对是我们过去使用的架构以及其他著名框架（如 Redux 或 Angular）的一个巨大改进。但首先，让我们看看什么是模型视图意图，以及它与其他框架有何不同。这种架构的第一个特点是它遵循单向数据流，就像 Facebook 在 React.js 之后引入的 Flux 模式一样，单向数据流正在成为许多前端响应式架构中的一个常态。它的意思是数据流总是朝着一个唯一的方向流动，并且永远不会改变；这有助于调试你的应用程序，并且在没有长时间的入职期来解释你的应用程序架构如何工作或系统如何工作的情况下，也便于新团队成员的加入。

Flux 模式是 Facebook 引入的一种模式，它在 Web 应用程序中强制执行单向通信流（图 4-3）。
![A446957_1_En_4_Fig3_HTML.jpg](img/A446957_1_En_4_Fig3_HTML.jpg)
`图 4-3` Flux 数据流图

从上面的示意图可以看出，每当用户与视图中的一个元素交互时，该元素就会分发一个动作，该动作被一个全局调度器捕获。调度器的主要目的是触发那些已经注册了回调的存储，以便监听它们感兴趣的动作。一旦存储从执行的动作中接收到数据，视图所需的更改就会向视图发出一个更改事件，视图将从存储中检索数据，然后渲染更改并更新组件的状态。更多信息请访问：[`bit.ly/2rHN8F0`](http://bit.ly/2rHN8F0)。

我们之前提到的另一个特点是，模型、视图和意图之间的通信仅通过流进行；因此，不同模块之间没有任何直接控制，只有一个作为输入的流和一个作为输出的流，就像我们在 Cycle.js 应用程序逻辑和驱动程序之间的通信中看到的那样。

MVI 由三个主要模块组成：

*   我们精心设计用户交互并保存应用程序状态的模型。
*   我们将 UI 与模型提供的状态连接起来的视图。
*   我们订阅用户交互或输入，并将它们提供给模型以将状态更改为新状态的意图。

# MVI 架构在 Cycle.js 中的应用

## 介绍

图 4-4 展示了这种架构。![A446957_1_En_4_Fig4_HTML.jpg](img/A446957_1_En_4_Fig4_HTML.jpg) 图 4-4 MVI 数据流图。渲染器部分在此案例中代表 DOM 驱动。现在，让我们看看如何使用模型-视图-意图架构来修改我们简单的 Cycle.js 示例。我们将要探索的代码与之前的示例非常相似，因此我们将只强调关键部分，而不会花太多时间讲解如何解析从 HTTP 驱动检索到的数据，或者如何组合由 DOM 驱动稍后渲染的虚拟 DOM 元素。

## 分解 `main` 函数

首先要做的是在我们之前的示例中，识别出 `main` 函数中应该分配给 MVI 架构不同部分的部分。最初，我们的 `main` 函数实现如下：

```javascript
const main = sources => {
    const input$ = sources.DOM.select("#location-input").events("focusout")
                                .map(evt => evt.target.value);
    const btn$ = sources.DOM.select("#location-btn").events("mousedown");
    const merged$ = xs.combine(input$, btn$);
    const request$ = merged$.map(([city, mouseEvt]) => getRequest(city))
                       .startWith(getRequest(INIT_CITY))
    const response$ = sources.HTTP.select(CATEGORY)
                                 .flatten()
    const vdom$ = response$.map(parseResponse)
                      .map(simplifyData)
                      .map(generateVDOM)
                      .startWith(h1("Loading..."))
    return {
        DOM: vdom$,
        HTTP: request$
    }
}
```

我们可以在实现的前几行中立即识别出意图部分。正如我们所说，意图用于捕获用户意图，因此是所有的 DOM 交互。实际上，意图接收渲染后的真实 DOM 作为输入，并将用户意图作为流输出：

```javascript
const intent = DOM => {
    const input$ = DOM.select("#location-input").events("focusout")
                                  .map(evt => evt.target.value);
    const btn$ = DOM.select("#location-btn").events("mousedown");
    return xs.combine(input$, btn$)
             .map(([city, mouseEvt])=> getRequest(city))
             .startWith(getRequest(INIT_CITY))
}
```

包含请求的流将被传递给模型和 HTTP 驱动，以执行对远程端点的请求。然后，我们需要处理从 HTTP 驱动接收到的响应，在这种情况下，模型将负责通过为视图准备数据来处理它：

```javascript
const model = (actions$, HTTP) => {
    return HTTP.select(CATEGORY)
               .flatten()
               .map(parseResponse)
               .map(simplifyData)
}
```

如我们所见，模型接收 `actions` 流和 `HTTP` 对象，在这种情况下，我们不需要对用户输入的数据执行任何操作，因为端点的响应提供了我们需要的所有数据，但我们可以潜在地组合接收到的数据和用户操作，以便为视图准备新的状态。

最后一部分是将模型准备的数据与视图合并，并生成将传递给 DOM 驱动的虚拟 DOM 元素：

```javascript
const view = state$ => {
    return state$.map(generateVDOM)
                 .startWith(h1("Loading..."))
}
```

因此，我们现在的 `main` 函数将如下所示：

```javascript
const main = sources => {
    const actions$ = intent(sources.DOM);
    const state$ = model(actions$, sources.HTTP)
    const vdom$ = view(state$);
    return {
        DOM: vdom$,
        HTTP: actions$
    }
}
```

## 完整示例

然后我们可以看看应用了 MVI 的完整示例：

```javascript
import xs from 'xstream';
import {run} from '@cycle/run';
import {makeDOMDriver, div, h1, h2, h3, img, p, input, button} from '@cycle/dom';
import {makeHTTPDriver} from '@cycle/http';
import moment from 'moment';

const CATEGORY = "forecast";
const INIT_CITY = "London";

const getForm = () => div(".form", [
    input("#location-input"),
    button("#location-btn", "get forecasts")
])

const generateNext5Days = forecasts => {
    const list = forecasts.map(forecast => {
        return div(".forecast-box", [
            h3(moment(forecast.date).format("dddd Do MMM")),
            p(`min ${forecast.day.mintemp_c}°C - max ${forecast.day.maxtemp_c}°C`),
            img(".forecast-img", {
                props: {
                    src: `http:${forecast.day.condition.icon}`
                }
            }),
            p(".status", forecast.day.condition.text)
        ])
    });
    return div(".forecasts-container", list)
}

const generateCurrentForecast = forecast => div(".current-forecast-container", [
     div(".today-forecast", [
            img(".forecast-img", {
                props: {
                    src: `http:${forecast.condition.icon}`
                }
            }),
            p(".status", forecast.condition.text)
        ]),
        h3(moment(forecast.last_updated).format("dddd Do MMMM YYYY")),
        h2(`${forecast.temp_c}°C`),
        p(`humidity: ${forecast.humidity}%`)
    ])

const generateVDOM = data => div(".main-container", [
        h1(`Your forecasts in ${data.city}`),
        getForm(),
        generateCurrentForecast(data.current),
        generateNext5Days(data.forecast)
    ])

const parseResponse = response => JSON.parse(response.text);

const simplifyData = data => {
    return {
        city: data.location.name,
        current: data.current,
        forecast: data.forecast.forecastday
    }
}

const getRequest = city => {
    return {
        url: `http://api.apixu.com/v1/forecast.json?key=04ca1fa2705645e4830214415172307&q=${city}&days=7`,
        category: CATEGORY
    }
}

const model = (actions$, HTTP) => {
    return HTTP.select(CATEGORY)
               .flatten()
               .map(parseResponse)
               .map(simplifyData)
}

const intent = DOM => {
    const input$ = DOM.select("#location-input").events("focusout")
                                .map(evt => evt.target.value);
    const btn$ = DOM.select("#location-btn").events("mousedown");
    return xs.combine(input$, btn$)
             .map(([city, mouseEvt])=> getRequest(city))
             .startWith(getRequest(INIT_CITY))
}

const view = state$ => {
    return state$.map(generateVDOM)
                 .startWith(h1("Loading..."))
}

const main = sources => {
    const actions$ = intent(sources.DOM);
    const state$ = model(actions$, sources.HTTP)
    const vdom$ = view(state$);
    return {
        DOM: vdom$,
        HTTP: actions$
    }
}

const drivers = {
  DOM: makeDOMDriver('#app'),
  HTTP: makeHTTPDriver()
};

run(main, drivers);
```

## 总结

MVI 并不像它看起来那么复杂；我们只需要习惯它。我想强调几个关键概念，当我们在 Cycle.js 应用中集成这种架构时需要牢记：

首先，我们将我们的 Cycle.js 项目转换成一个结构化的项目，其中的每个部分都可以被重用和单独测试。

这种架构甚至允许我们进一步将 MVI 架构应用于每个组件：一个 MVI 架构应用于表单，一个应用于当前日期，一个应用于工作日列表（由于其性质）。

使用流进行通信允许整个架构更加灵活，并增强了关注点的分离。

在结束本章之前，我们需要再次增强这个示例，介绍 Cycle.js 的官方状态管理库 Onionify。

## Cycle.js 和状态管理

在了解了 MVI 的实际应用后，我们的旅程继续介绍 Onionify，一个为 Cycle.js 应用程序状态管理而创建的库。正如我们所知，处理状态是任何 Web 应用程序的关键部分。Cycle.js 提供了一种独特的方法，与我们在 Angular 或 Redux 中习惯看到的略有不同。Onionify 是一个非常小的库（仅 2kb），只有一个目的：管理 Cycle 应用程序中的应用程序状态。这个库不像我们在其他场合看到的那样提供一个驱动程序，而是将整个 Cycle.js 应用程序包装在一个唯一的 `state` 中，该状态被注入到多个组件中。应用程序状态是由 Onionify 内部管理的一个流，每个组件都可以通过 `reducer` 操作自己的状态和父组件的状态。组件需要通过同名的 `isolate` 库进行“隔离”。`isolate` 是 Cycle.js 提供的一个工具，它允许我们真正地隔离一个组件，只共享主应用程序或父组件提供的 `source`；并返回一个可以与主应用程序和/或其他组件共享的 `sink` 对象。

让我们稍作停顿，尝试总结一下我们目前关于 Cycle.js 所学到的内容：

*   我们知道我们可以创建一个 MVI 架构的应用程序。
*   MVI 不仅可以应用于整个循环应用，还可以应用于组件。
*   这导致相同的架构应用于我们架构的所有级别，其中 `sink` 和 `source` 是对象之间通信的唯一方式。

考虑到所有这些事实，我们可以说，通过 Cycle.js 应用 MVI，我们可以创建一个分形架构，这将允许我们始终使用相同的“片段”（模型-视图-意图）来生成一个由相同片段多次应用组成的更大组合。

**分形架构** 并不是软件开发中的一个新概念。这种具有相同子系统结构的架构允许高度的关注点分离，以便构建一个大型项目，其中修改、删除或创建新部分不会影响整个应用程序，因为子系统生活在隔离之中。Onionify 与 MVI 架构结合使用，有助于创建一个具有强大关注点分离和良好封装的坚实且可重用的架构。因此，我们应该能够将 Cycle.js 应用程序的一部分重用到其他应用程序中，只需遵守组件所需的契约，即使用正确的 `source`（驱动程序和流）并与它们返回的 `sink` 进行交互。

为了看到 Onionify 的实际应用，我们将再次修改我们的天气应用程序，将我们的 MVI 应用程序拆分为多个组件，并使用 Onionify 来更改状态。图 4-5 展示了引入 Onionify 后我们的 Cycle.js 应用程序的样子。

![A446957_1_En_4_Fig5_HTML.jpg](img/A446957_1_En_4_Fig5_HTML.jpg)

图 4-5 Cycle Onionify 项目图

让我们分析一下这里与之前应用程序相比有什么不同：

*   我们有三个组成部分：城市形态、今日预测和未来预测。
*   我们有一个状态，由主应用程序传递给由状态 `observable` 组成的 Onionify 包装器。
*   我们仍然以和以前一样的方式和驱动程序交流；因此，我们仍然有 HTTP 和 DOM 驱动程序。

# CityForm 组件详解

`CityForm` 是包含主要逻辑的组件，负责获取用户在输入框中输入的内容，并准备将由 HTTP 驱动执行的请求。

```javascript
import onionify from 'cycle-onionify';
import xs from 'xstream';
import {div, input, button, h1} from '@cycle/dom';

const INIT_CITY = "London";
const CITY_SEARCH = "citySearchAction";
const CATEGORY = "forecast";

const getRequest = city => ({
    type: CITY_SEARCH,
    city: city,
    url: `http://api.apixu.com/v1/forecast.json?key=04ca1fa2705645e4830214415172307&q=${city}&days=7`,
    category: CATEGORY
})

const getForm = location => div(".form", [
    h1(`Your forecasts in ${location.city}`),
    input("#location-input", {props: {value: `${location.city}`}}),
    button("#location-btn", "get forecasts")
])

const parseResponse = response => JSON.parse(response.text);

const simplifyData = data => function changeState(prevState) {
    return {
        city: data.location.name,
        current: data.current,
        forecasts: data.forecast.forecastday
    }
}

const model = (actions$, HTTP) => {
    const reducer$ = HTTP.select(CATEGORY)
        .flatten()
        .map(parseResponse)
        .map(simplifyData)
    return reducer$
}

const intent = DOM => {
    const input$ = DOM.select("#location-input").events("focusout")
        .map(evt => evt.target.value);
    const btn$ = DOM.select("#location-btn").events("mousedown");
    return xs.combine(input$, btn$)
        .map(([city, mouseEvt]) => getRequest(city))
        .startWith(getRequest(INIT_CITY))
}

const view = state$ => state$.map(state => getForm(state))

export const CityForm = sources => {
    const state$ = sources.onion.state$;
    const actions$ = intent(sources.DOM);
    const reducer$ = model(actions$, sources.HTTP);
    const vdom$ = view(state$);
    return {
        DOM: vdom$,
        onion: reducer$,
        HTTP: actions$
    }
}
```

我们可以立即识别出，我们有一个来自 `sources` 的新参数，它由 `Onionify` 包装器提供；这个库提供了一个包含状态流的 `onion` 对象，基于此我们可以与父状态流进行交互，对其做出反应，或者操作内部组件状态。总体而言，该组件与我们在上一个应用程序中的非常相似，唯一的变化是与表示来自天气 API 响应的应用程序状态相关，因此 `CityForm` 仅使用状态流来检索用户选择的位置。

最后需要提及的是理解此组件作为 sink 返回了什么，以及我们如何立即发现包含 HTTP 响应作为应用程序状态的 `onion` 属性。我们可以认识到，这个组件是自包含的，因此如果我们想在另一个应用程序中重用它，我们无需更改任何内容即可做到：这就是使用分形架构的力量，其中可重用性、关注点分离和封装是架构中的首要公民。

## 被动组件：TodayForecast 与 FutureForecast

在研究如何修改 `App.js` 以处理 `Onionify` 库之前，让我们快速浏览一下另外两个被动组件：`TodayForecast` 和 `FutureForecast`。这两个组件是被动的，因为它们只需要渲染由状态提供的一些内容；它们没有用户交互，并且不会操作任何父状态或执行新的 HTTP 请求。

### TodayForecast 组件

```javascript
import {div, h2, h3, img, p} from '@cycle/dom';
import moment from 'moment';

const generateCurrentForecast = forecast => div(".current-forecast-container", [
    div(".today-forecast", [
        img(".forecast-img", {
            props: {
                src: `http:${forecast.condition.icon}`
            }
        }),
        p(".status", forecast.condition.text)
    ]),
    h3(moment(forecast.last_updated).format("dddd Do MMMM YYYY")),
    h2(`${forecast.temp_c}°C`),
    p(`humidity: ${forecast.humidity}%`)
])

const view = state$ => state$.map(state => generateCurrentForecast(state.current))

export const TodayForecast = sources => {
    const state$ = sources.onion.state$;
    const vdom$ = view(state$)
    return {
        DOM: vdom$
    }
}
```

在这段代码中，我们可以在 `TodayForecast` 函数中发现用于渲染视图的状态流，该视图对应于用户请求预报时特定时刻的天气数据表示。考虑到这是一个被动组件，其唯一职责是向 DOM 驱动提供虚拟 DOM 以进行视图渲染。显然，如果有任何可能改变应用程序状态的用户交互，这将反映在 sink 中，并且它将能够通过 sink 的 `onionify` 属性共享新状态——如果它需要触发对天气预报端点的新 HTTP 请求，情况也是如此。

### FutureForecast 组件

```javascript
import {div, h3, img, p} from '@cycle/dom';
import moment from 'moment';

const generateNext5Days = forecasts => {
    const list = forecasts.map(forecast => div(".forecast-box", [
        h3(moment(forecast.date).format("dddd Do MMM")),
        p(`min ${forecast.day.mintemp_c}°C - max ${forecast.day.maxtemp_c}°C`),
        img(".forecast-img", {
            props: {
                src: `http:${forecast.day.condition.icon}`
            }
        }),
        p(".status", forecast.day.condition.text)
    ]));
    return div(".forecasts-container", list)
}

const view = state$ => state$.map(state => generateNext5Days(state.forecasts))

export const FutureForecast = sources => {
    const state$ = sources.onion.state$;
    const vdom$ = view(state$)
    return {
        DOM: vdom$
    }
}
```

同样，这个组件与前一个非常相似，它不需要共享任何状态更新。它只是消费状态流，以便渲染新的虚拟 DOM 并提供给 DOM 驱动。

## App.js：应用的粘合剂

最后，轮到 `App.js` 了，我们可以在这里找到我们“onionified 应用程序”的粘合剂：

```javascript
import xs from 'xstream';
import {run} from '@cycle/run';
import {makeDOMDriver, div, h1} from '@cycle/dom';
import {makeHTTPDriver} from '@cycle/http';
import isolate from '@cycle/isolate';
import onionify from 'cycle-onionify';
import {CityForm} from './CityForm';
import {TodayForecast} from './TodayForecast';
import {FutureForecast} from './FutureForecast';

const generateVDOM = ([formVNode, todayVNode, futureVNode]) => div(".main-container", [
    formVNode,
    todayVNode,
    futureVNode
])

const view = (locationDOM$, todayForecastDOM$, futureForecastDOM$) => {
    return xs.combine(locationDOM$, todayForecastDOM$, futureForecastDOM$)
        .map(combinedStreams => generateVDOM(combinedStreams))
        .startWith(h1("Loading..."));
}

const main = sources => {
    const cityLens = {
        get: state => state,
        set: (state, childState) => childState
    }
    const locationSink = isolate(CityForm, {onion: cityLens})(sources);
    const todayForecastSink = isolate(TodayForecast, {onion: cityLens})(sources);
    const futureForecastSink = isolate(FutureForecast, {onion: cityLens})(sources);
    const locationReducer$ = locationSink.onion;
    const httpRequest$ = locationSink.HTTP;
    const vdom$ = view(locationSink.DOM,
        todayForecastSink.DOM,
        futureForecastSink.DOM);
    return {
        DOM: vdom$,
        HTTP: httpRequest$,
        onion: locationReducer$
    }
}

const drivers = {
    DOM: makeDOMDriver('#app'),
    HTTP: makeHTTPDriver()
};

const mainOnionified = onionify(main);
run(mainOnionified, drivers);
```

这里我们可以发现一些有趣的新概念，所以让我们从 JavaScript 文件的末尾开始描述我们在做什么。正如我们提到的，`Onionify` 不是一个驱动，而是我们应用程序的一个包装器；因此我们用它来包装我们的 `main` 函数，并将装饰过的，或 onionified 版本的 `main` 函数传递给 `run` 方法。这允许我们通过 `sources` 和 `sink` 中的 `onionify` 属性在不同组件之间传递状态。

现在让我们看看 `main` 函数是什么样子的。我们从一个名为 `cityLens` 的透镜开始。透镜用于当组件需要访问与其父组件相同的对象（如我们的情况），或者当我们需要在状态进入特定组件之前对其进行操作时。从技术上讲，透镜是一个带有 getter 和 setter 的对象，没什么复杂的，但它们在函数式编程中非常有用，特别是当我们用它们来组合对象时。透镜在 Haskell 语言中也很有名，在 JavaScript 世界中，它们与 Immutable.js 一起使用，并且肯定存在于 Ramda（例如 `lensProp` 方法）中，这是一个 JavaScript 函数式库。

在透镜定义之后，它将允许我们共享组件中存在的数据与其他组件共享，我们使用 Cycle.js 提供的 `isolate` 工具实例化了我们的三个自定义组件。我们将透镜和 `sources` 传递给每个组件，这允许我们在每个组件内部与父组件进行交互。在这种情况下，它是应用程序本身的父组件，但很明显，通过这种方式工作，我们可以包装我们的组件，定义标准的输入/输出契约，并轻松地在同一应用程序的不同部分重用它，或者更好的是在其他应用程序中重用。

创建组件实例之后，是时候将操作过的状态和 HTTP 请求传递给 sink 对象中的关联属性了。在这种情况下，我们需要强调几点。在这个项目中，状态由端点响应处理，并且没有来自其他组件的任何操作，这些组件仅用于渲染阶段。显然，如果我们查看 `Onionify` 仓库中存在的其他示例（[`bit.ly/2eQXh9S`](http://bit.ly/2eQXh9S)），我们可以看到，我们不是像现在这样传递一个流作为应用程序状态，而是可以组合来自不同组件的多个流，以存储更复杂的应用程序状态。

最后一点是检索包含由不同组件准备的虚拟 DOM 的所有流，并将它们全部组合在一起，以产生 Cycle 应用程序的外观和感觉。为此，我们创建了一个 `view` 方法，该方法收集所有流并将它们组合成一个唯一的流，该流将生成最终的虚拟 DOM 流，该流将由 DOM 驱动进行渲染。

我想请大家注意我们这样做的方式，因为如你所见，它有点冗长，特别是当应用程序增长并且我们需要合并多个组件时。在这些情况下，我们应该准备一些工具方法来处理这些情况（包装在单个或多个函数中会解决问题）。Cycle.js 本身没有提供任何开箱即用的有用工具，但有一些库正在为此目标努力。然而，目前还没有官方的方法来实现更可读和灵活的方法。

## 总结

In this chapter, we evaluated our first fully reactive framework, understanding what Cycle.js brings to the table, and the different architectural approaches used for achieving that. Cycle is maintained by a small but very talented community that is porting many reactive and functional concepts from other languages in the JavaScript community. I believe that it is one of the most innovative approaches we can find in the front-end reactive architecture ecosystem at the moment. MVI is, in my opinion, merging the best from older architectures like MVP, for instance, and the unidirectional flow that is characterizing the latest front-end architectures like Redux. There is still a lot to do in our reactive journey, so it’s time to move on and talk about our next functional reactive state management library: MobX.