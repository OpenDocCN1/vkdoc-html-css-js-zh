# 二、架构比较

> 如果你觉得好的建筑很贵，那就试试差的建筑。—布莱恩·富特

在本章中，我们将探讨前端架构的现状。当我们为项目选择一个框架时，别人已经为我们做出了关于如何构建架构的决策，他们遵循最佳实践和设计模式，从而让我们能够专注于应用程序应该实现什么，而不是如何构建它们。这里需要强调架构与设计之间的区别，因为这两个术语经常被误解。当我们谈论架构时，我们是在定义系统中的不同元素将如何交互。例如，思考模型与视图之间的通信。通常，当我们定义架构时，我们是在定义对象之间的关系、它们如何相互通信等等。架构决策很难改变，因为它们通常会将项目引向一个特定的方向，而要将其转向另一个方向则需要巨大的努力。另一方面，设计决策是局部决策，比如为解决项目中的某个问题而选择特定的库、算法或设计模式。当我们对项目做出设计决策时，通常不需要付出巨大代价来纠正它，但在某些情况下，糟糕的设计决策会导致系统长时间的重构。假设我们需要解决一个问题：每隔几分钟就需要刷新特定视图中的数据，而无需刷新整个页面，只刷新需要更改的元素；我们可以决定使用`React.js`，利用其差异算法，或者创建自己的算法，以便更贴近我们的需求来定义差异算法，从而拥有更多控制权。理清架构决策与设计决策的区别后，是时候看看我们将在本章中探讨的内容了。前端生态系统在不断演进，尤其是在过去几年里，我们经历了不同但根本性的变化，这些变化正在改善我们项目的创建和可维护性。我们可能正经历着过去十年中最激动人心的十年，被革命性的概念所淹没，这些概念往往源自过去，但被赋予了新的视角，将其转化为有用的方式，并采用实际的方法来解决我们日常的挑战。前端架构在过去30年里发生了很大变化，我们从经典的`Model View Control (MVC)`架构，转向了当今许多现代框架中更常见的架构。这可能是这种演进的时间线表示。在本章中，我们将实际了解最著名的架构并对它们进行比较。图2-1展示了一个时间线，其中突出显示了我们将要探讨的所有架构和实现，以及它们被创建的年份。你会看到，许多来自20世纪80年代或90年代的概念在当今JavaScript生态系统中最著名的框架实现中仍然非常现代和流行。![A446957_1_En_2_Fig1_HTML.jpg](img/A446957_1_En_2_Fig1_HTML.jpg) 图2-1 架构时间线 最重要的是要记住，这些架构并未过时，它们仍然是有效的概念，可以通过极大地促进项目的开发和维护来为我们的项目增添巨大价值。创建结构良好且灵活的架构也为我们提供了所需的敏捷性，以拥抱设计变更乃至架构变更。此外，我们需要记住，如果以适当的方式应用`Reactive`概念，这些架构可以转换为响应式架构；因此，如果我们的项目当前正在使用其中一种架构，我们仍然可以拥抱`Reactive`模型，通过应用一些更改来增强它们，并转向更具响应性的范式。

## MV*架构

In this section, we are going to dig into the base concepts of MV* architectures; in order to do that we will work on a simple project (a basic calculator) for understanding how these architectures are composed, and how objects are communicating by embracing the SOLID principles that any project should take in consideration in order to be maintainable and extensible. The three architectures we are going to explore in this section are MVC, MVP, and MVVM; let’s start with the oldest one, MVC!

### S.O.L.I.D.

SOLID is a set of principles created by Uncle Bob. SOLID is an acronym that stands for:
- S – Single-responsibility principle
- O – Open-closed principle
- L – Liskov substitution principle
- I – Interface segregation principle
- D – Dependency inversion principle

If you are interested in knowing more about them, I suggest watching this Uncle Bob lecture: [`www.youtube.com/watch?v=t86v3N4OshQ`](https://www.youtube.com/watch?v=t86v3N4OshQ)

Bear in mind though, that all the code presented in this chapter are just highlights of the final examples; therefore if you have a hard time following the snippets, feel free to download the chapter examples first and switch from this book to your favorite editor or IDE for consulting the code. Remember first to install all the dependencies with the command:
```
npm install
```
And then you can run the `n.p` script called `build` in this way:
```
npm run build
```

### 模型视图控件

Model View Control (MVC) is an architectural pattern introduced in Smalltalk in the late 1980s by Trivet Reenskaug, and it is probably the most popular architecture of the past 30 years, used by millions of developers in any project independently of the language used (Figure 2-2). Its main characteristic is the separation of concerns and the single responsibility of its actors; the main innovation of this pattern was finally separating the data from their visual representation, a concept not fully explored until then. In fact the model objects are completely separated from their representation; therefore there isn’t any knowledge of the view inside the model. This detail becomes important when multiple views or controllers are accessing the same data because our model objects could be reused across different screens without any problem.

MVC is based on three basic principles:
- **模型**: 应用程序状态和领域数据所在的位置
- **视图**: 用户交互的应用程序的用户界面
- **控制器**: 模型和视图之间的粘合剂，通常负责协调应用程序内部的通信流

![A446957_1_En_2_Fig2_HTML.jpg](img/A446957_1_En_2_Fig2_HTML.jpg)
Figure 2-2 Model View Control diagram (MVC)

#### MVC 如何工作

Usually the controller instantiates the view and the model in the constructor or, in certain implementations, are injected by the main application class via dependency injection. The relation between a controller and the views could be one to many, so a controller could handle multiple views: the same relationship is valid for the models as well. When we want to scale up this architecture for large projects usually we try to split up these relationships in order to have almost a 1 to 1 relation between these three objects so we can reuse components that are self-contained and architected in the same way the entire application works, like a Russian doll. As described before the main aim of a model is storing the application state and everything that should be persistent across a single or multiple views, usually every time the application state changes or data are updated; the model is triggering an event in order to update the view. In the vast majority of implementations the object listening for any state or data change is the view, but in certain implementations we can also find a total separation between the model and the view where the controller in this case is listening to the change and is propagating this information to the view. The view is simply responsible for displaying the application data and listening for any user interactions.

## Figure 2-3
Figure 2-3 shows how MVC works with an implementation of a calculator in JavaScript. This is the final result of what we want to achieve.
![A446957_1_En_2_Fig3_HTML.jpg](img/A446957_1_En_2_Fig3_HTML.jpg)
This is the output of our Calculator application It is a simple calculator where every time we are clicking a button, we add the value on the display on the top and when the user is going to click the button “=” we will display the result. Let’s start to explore our MVC example from the controller. See Listing 2-1.

## Listing 2-1
```javascript
initialize(){
    const emitter = this.initEmitter();
    this.model = this.initModel(emitter);
    this.initView(emitter, this.model);
}
```
`CalculatorController.js`
In our implementation, the controller is going to instantiate the view and the model, and it’s injecting an event emitter for communicating between objects via events. This will immediately improve the decoupling between objects because if in the future we want to reuse some of these objects in other projects, we won’t need to copy more than the object we are interested in, considering they are communicating via events, and as long as we respect the contract our code becomes reusable and flexible. We are going to use `React.js` for handling the views of our project. `React` usually renders the components again when there is a properties update, but in our case what we implement is using the event bus for notifying the view that a new result should be rendered on the calculator’s display, and then the view will retrieve the data from the model, updating with the new string the display. In order to do that, we need to inject the model and the emitter instance inside the view. See Listing 2-2.

## Listing 2-2
```javascript
initView(emitter, model){
    const cont = document.getElementById("app");
    ReactDOM.render(<Calculator emitter={emitter} model={model} />, cont);
}
```
`CalculatorController.js`
Then we will use the `React` Component life-cycle methods to store these two objects locally and listen for any change from the model; when a change happens, we are going to update a state property inside the `React` component to display the correct value. See Listing 2-3.

## Listing 2-3
```javascript
componentWillMount(){
    this.model = this.props.model;
    this.emitter = this.props.emitter;
    this.emitter.on("TotalChanged", _ => this.setState({displayValue: this.model.total}));
    this.setState({displayValue: this.model.total})
}
```
`Calculator.jsx`
So, every time the `displayValue` property is updated, this will trigger the render function; therefore the view will be updated with a new result as shown in Listing 2-4.

## Listing 2-4
```javascript
render(){
    return(
        <div>
            <div style={displayStyle}>{this.state.displayValue}</div>
            {this.createRow("row1", "AC")}
            {this.createRow("row2", "7", "8", "9", "/")}
            {this.createRow("row3", "4", "5", "6", "*")}
            {this.createRow("row4", "1", "2", "3", "-")}
            {this.createRow("row5", "0", ".", "=", "+")}
        </div>
    );
}
```
`Calculator.jsx`
Inside the model instead, we are going to keep the application state, handling what should be displayed in our view and performing some calculations. Our model is composed by a few methods that will allow the controller to call every time a button is clicked from the Calculator keyboard as you can see in the following code in Listings 2-5 and 2-6.

## Listing 2-5
```javascript
initEmitter(){
    let emitter = new LiteEventEmitter();
    emitter.on("CalculateEvent", _ => {
        this.model.calculate();
    });
    emitter.on("AddValueEvent", content => {
        this.model.addValue(content.value, content.type);
    });
    emitter.on("ResetEvent", _ => {
        this.model.reset();
    })
    return emitter;
}
```
`CalculatorController.js`

## Listing 2-6
```javascript
calculate(){
    this.totalOperation = math.eval(this.totalOperation);
    this.state = FIRST_OPERATION;
    this.emitter.emit("TotalChanged");
}
addValue(value, type){
    if(type === NUMERIC){
        this.totalOperation = this.getValue(value);
    } else if(type === SYMBOL){
        this.totalOperation = this.checkSymbol(value);
    }
    this.state = IN_PROGRESS_OPERATION;
    this.emitter.emit("TotalChanged");
}
reset(){
    this.totalOperation = 0;
    this.state = FIRST_OPERATION;
    this.emitter.emit("TotalChanged");
}
```
`CalculatorModel.js`
The Controller, via the `emitter` object, is listening for any event triggered by the view and it is requesting to the model to perform some operations like reset, calculate, or add a new value. The model, instead, every time `totalOperation` value is changing it, notifies via the event bus our view that will display the new value updating the state property of our `React` component, triggering then the render function. MVC is a pretty simple and straightforward pattern: there is a good separation of concern, each object has a unique responsibility, and all the information is well encapsulated.

# 模型视图演示者

Model View Presenter (MVP) is an architecture created in the 1990s, and one of its first appearances was made in IBM software (`Figure 2-4`). From my point of view, MVP shines when we need to reuse views or behaviors in different projects or different areas of the same application; with this architecture we start to give more importance to the modularization of our front-end applications and provide architecture specific for a front end more than a generic one that could fit a back-end or front-end application like for MVC. MVP is very helpful, in particular, when we work on cross-platform applications and we want to reuse the application data, communication layer, and behaviors or when we want to swap the views changing them at runtime or compile/transpile time. The main differences between MVP and MVC could be summarized in the following list:

*   用一个演示者代替一个控制者，我们马上就会看到演示者带来的好处。
*   视图和演示者之间的关系不像 MVC 中那样是一对多的，而总是一对一的。
*   拥有可重用组件的最佳 MVP 实现是当我们将视图设计为被动视图时，因为只要表示者和视图之间的契约得到尊重，交换它们就变得更容易。

![A446957_1_En_2_Fig4_HTML.jpg](img/A446957_1_En_2_Fig4_HTML.jpg) `Figure 2-4` MVP diagram where the view is unaware of the model’s existence

## MVP 如何工作

# The Presenter Object and MVP Architecture

The presenter object is inspired by the presentation model pattern, and my favorite implementation is when the presenter is designed as a `Supervising Controller` where it retrieves all the data useful for a view from the model, and at the same time it should handle any user interaction updating the model. As mentioned before, the views are passive or if you prefer dumb, they just know how the rendering logic works, possible animations, integration with CSS styles, and so on. On top, the presenter is also dealing with updating the model and retrieving the information needed for rendering a view. Usually, in complex applications, you could have a persistent model (or more than one model maybe exposed by a façade) across the entire life cycle of an application and multiple presenters that retrieve and update the application data in the models.

Another important point to highlight is the fact that the model and the view should not be aware of each other; maintaining these two completely isolated from each other will help a lot in the case of large applications or when we need to swap views for targeting different devices. Imagine for a moment that our assignment is a project where we need to target different devices like browsers, mobile, and smartTVs – exactly the same application but different UI for different targets considering that each target has different input methods and UI patterns. With an MVP architecture, maintaining the behaviors inside the presenter, the business domain in the model and having just passive views for the UI will allow us to have similar behaviors across the application, reusing the same code for the models and changing the views – adapting them for the device we are targeting without much effort.

## Passive View

A passive view is a view without any knowledge of how the system works but just relying on another object for handling the communication with the system. A `Passive view` doesn’t even update itself by retrieving data from the model; this view is completely passive, and its main scope is focusing on what to render on the screen when a specific render function is called from a controller or presenter.

## Supervising Controller

A supervising controller is a specific type of controller that is handling user interaction as well as manipulating the view for updating it. When a supervising controller is present in an architecture, the view needs only to redirect the user events to the supervising controller (in MVP the presenter is our supervising controller), and it will take care of handling the logic and updating the view with new data. The supervising controller is responsible for the communication in the system and it’s taking care to update the view it is associated with.

## MVP in Action: Calculator Example

It’s time to see MVP in action with the Calculator example discussed above, but this time with the Model-View-Presenter in mind. We can start from the `App.js` file where in the constructor we are going to create the model and the presenter, and we import the view called `Calculator.jsx`. We then inject React component and model inside the presenter as shown in Listing 2-7.

```javascript
export default class App{
    constructor(){
        const mainModel = new CalculatorModel();
        const mainPresenter = new CalculatorPresenter();
        mainPresenter.init(mainModel, Calculator);
    }
}
```

Then we can move inside the presenter where we are going to store all the objects injected in variables and then we render the React component injected. See Listing 2-8.

```javascript
initialize(model, view){
    this.model = model;
    this.view = view;
    this.cont = document.getElementById("app");
    this.renderView();
}

renderView(){
    const component = React.createElement(this.view, {result: this.model.total, onBtnClicked: ::this.onBtnClicked});
    ReactDOM.render(component, this.cont)
}
```

In our Presentation model we are injecting the model and the view for having complete controls on them; we then call the method `renderView` that will be our trigger for communicating to a React component to render again because something happened inside the application and the UI should be updated. As you can see, the view doesn’t have any knowledge of the model but we pass the result to display in our calculator via the `props` object exposed by React.

Now it’s time to take a look at the view; and as we defined at the beginning of this section, the view should be a passive view, so in this case it is taking care of what to render and how nothing else should be integrated in a passive view. The communication with the presenter is happening via a method passed via the `props` object. Like we have seen in the `renderView` method, the presenter is passing a callback that should be invoked every time the user is selecting a button of our calculator. See Listing 2-9.

```javascript
import React from "react";
import ReactDOM from "react-dom";
import {ulStyle, acStyle, btnStyle, displayStyle} from "./Calculator.css";

export default class Calculator extends React.Component{
    constructor(props){
        super(props);
    }

    componentWillMount(){
        this.btnClicked = this.props.onBtnClicked;
    }

    onButtonClicked(evt){
        evt.preventDefault();
        this.btnClicked(evt.target.innerHTML);
    }

    createRow(id, ...labels){
        const items = labels.map((value, index) => {
            return <li key={`${id}_${index}`}
                       style={value === "AC"? acStyle : btnStyle}
                       onClick={::this.onButtonClicked}>
                       {value}
                   </li>;
        })
        return(
            <ul key={id} style={ulStyle}>
                {items}
            </ul>
        )
    }

    render(){
        return(
            <div>
                <div style={displayStyle}>{this.props.result}</div>
                {this.createRow("row1", "AC")}
                {this.createRow("row2", "7", "8", "9", "/")}
                {this.createRow("row3", "4", "5", "6", "*")}
                {this.createRow("row4", "1", "2", "3", "-")}
                {this.createRow("row5", "0", ".", "=", "+")}
            </div>
        );
    }
}
```

All the methods present in the React component are defined for rendering the correct button or the display, and `this.props.onBtnClicked` is the method passed by the presenter. This method is identifying which button was selected so the presenter can capture the user interaction and it calls the correct method exposed inside the main model as you can see in the snippet below in Listing 2-10.

```javascript
onBtnClicked(value){
    switch (value) {
        case "AC":
            this.model.reset();
            break;
        case "=":
            this.model.calculate(value);
            break;
        case "+":
        case "-":
        case "/":
        case "*":
        case ".":
            this.model.addValue(value, SYMBOL);
            break;
        default:
            this.model.addValue(value, NUMERIC);
            break;
    }
    this.renderView();
}
```

It’s time to see what the model is doing; in this case the model is taking care of all the calculations and maintaining the application state. That will facilitate the debugging of our application because we know that the view is dealing merely with the UI of our application, and the presenter is handling the user interactions and the model keeps the application state and the caching of application data. Let’s take a look to the model shown in Listing 2-11.

```javascript
export default class CalculatorModel{
    constructor(){
        this.totalOperation = 0;
    }

    calculate(){
        this.totalOperation = math.eval(this.totalOperation);
        this.state = FIRST_OPERATION;
    }

    addValue(value, type){
        if(type === NUMERIC){
            this.totalOperation = this.getValue(value);
        } else if(type === SYMBOL){
            this.totalOperation = this.checkSymbol(value);
        }
        this.state = IN_PROGRESS_OPERATION;
    }

    checkSymbol(value){
        const str = this.totalOperation;
        if(this.state === FIRST_OPERATION){
            return str + value;
        }
        return !isNaN(str.charAt(str.length - 1)) ? str + value : str.substr(0, str.length - 1) + value
    }

    getValue(value){
        return (this.totalOperation == 0 || this.state === FIRST_OPERATION) ? value : this.totalOperation + value;
    }

    reset(){
        this.totalOperation = 0;
        this.state = FIRST_OPERATION;
    }

    get total(){
        return this.totalOperation;
    }
}
```

As you can see in the model, there are all the methods called by the presenter, like `reset` or `calculate`, and a few others like `checkSymbol` and `getValue`, used for handling the application logic. The separation of concerns in MVP is very strong and any application would be easier to debug and maintain if we properly apply these few concepts; it’s definitely an architecture that largely improved the MVC concept created 10 years before it.

## 模型视图视图-模型

Model View View-Model (MVVM) is an architecture created by Microsoft in 2005 for handling the GUI management with Windows Presentation Foundation (WPF) . It sticks with a true separation between the view and the model like we have seen in the MVP architecture, but MVVM encapsulates few differences compared to other architecture (Figure 2-5).

![A446957_1_En_2_Fig5_HTML.jpg](img/A446957_1_En_2_Fig5_HTML.jpg) Figure 2-5 MVVM diagram, similar to MVP but the view is not a passive one anymore

### MVVM 是如何运作的

The first difference is that we have a `View-Model` instead of a presenter; this specific object will be the bridge between the data stored inside a `model` and the `view`. In a nutshell the `view-model` is responsible for exposing the data present inside the `model` to the `view` and it is also responsible for preparing the data in a way that the `view` expects, so you can immediately understand that the logic of the `view-model` is tightly coupled with what the `view` should render. For instance, let’s imagine that we have a stock value stored in the `model` with `dollars` currency but the `view` needs it to be shown in `euro`. In MVVM the `model` stores the raw data, that is, `dollars` value, and the `view-model` would be responsible for converting the value to a given currency, in this case `euro`. Another important aspect of the `View-Model` is the fact that it has a relationship with the `views` that is 1 to many: therefore we can have one `view-model` that is handling multiple `views` or components. The communication of the `View-Model` and the `view` usually happen via bindings; therefore every time a value is updated on the `view` or in the `View-Model`, this value is communicated to the other object in order to keep it in sync. It’s very important to understand that with MVP we enforced the concept of complete separation between the `model` and the `view` and in MVVM this strong concept doesn’t change at all. The `model` is very simplistic; we store data in a raw format without any particular change, and in this case we should even avoid keeping the state in the `model` and moving this information to the `view-model` or even the `view` if it’s a small component.

Let’s see now how our calculator would work with an MVVM architecture. The `App.js` file is instantiating all the objects for an MVVM architecture: `model`, `view-model`, and `view`; in the `view-model` constructor we are going to inject the `model` and `view` instances so it will be able to retrieve data from the `model` and serve the `view` with the correct value to display. See Listing 2-12.

```
export default class App{
    constructor(){
        const model = new CalculatorModel();
        const emitter = new LiteEventEmitter();
        const vm = new CalculatorViewModel(model, emitter);
        const cont = document.getElementById("app");
        ReactDOM.render(<Calculator emitter={emitter} initValue={model.total} />, cont);
    }
}
```

Listing 2-12 `App.js`

The `Calculator` `view` is a React component that has two key concepts: dispatching the data from the `view` to the `view-model` and retrieving the data from the `view-model`. In order to do that we can use a data binding library or simply events as both are accepted from MVVM architecture, therefore we are using an event emitter, injected when the `view` and the `view-model` were instantiated, for handling the communication between these two objects, as shown in Listing 2-13.

```
componentWillMount(){
    this.setState({displayValue: this.props.initValue})
    this.emitter = this.props.emitter;
    this.emitter.on("UpdateDisplayValue", value => this.setState({displayValue: value}));
}

manageDisplayState(value){
    switch (value) {
        case "AC":
            this.emitter.emit("ResetEvent")
            break;
        case "=":
            this.emitter.emit("CalculateEvent")
            break;
        case "+":
        case "-":
        case "/":
        case "*":
        case ".":
            this.emitter.emit("AddValueEvent", {value: value, type: SYMBOL})
            break;
        default:
            this.emitter.emit("AddValueEvent", {value: value, type: NUMERIC})
            break;
    }
}
```

Listing 2-13 `Calculator.jsx`

In `manageDisplayState` we have all the events we will communicate to the `view-model`, but in the `componentWillMount` we define only the event we need to listen for updating the calculator’s display. The rest of the `view` is very similar to the other `views` we have discussed in the previous examples.

Now it’s the turn of our `view-model`. Here we need to do exactly the opposite of how we handled the events in the `view`; therefore we are going to listen for all the events emitted by the `view` and dispatch the event for updating the `view`’s display every time the value changes. See Listing 2-14.

```
initialize(){
    this.emitter.on("CalculateEvent", _ => {
        this.calculate()
    });
    this.emitter.on("AddValueEvent", content => {
        this.addValue(content.value, content.type);
    });
    this.emitter.on("ResetEvent", _ => {
        this.reset();
    })
}

updateDisplayAndState(state){
    this.state = state;
    this.emitter.emit("UpdateDisplayValue", this.model.total);
}
```

Listing 2-14 `CalculatorViewModel.js`

The `view-model` is responsible for retrieving and updating the data stored in the `model`, so every time the user clicks a button in our `view` we are going to update the value in the `model` and then call the `updateDisplayAndState` method for dispatching this change to the `view`. We keep a state internal to the `view-model` to understand which method we need to call during the calculation; therefore we need to update it when the state is changed. Let’s see, for instance, how we handle the changes in the three methods we have created for handling these operations, as shown in Listing 2-15.

```
addValue(value, type){
    let valueToAdd;
    if(type === NUMERIC){
        valueToAdd = this.getValue(value);
    } else if(type === SYMBOL){
        valueToAdd = this.checkSymbol(value);
    }
    this.model.add(valueToAdd)
    this.updateDisplayAndState(IN_PROGRESS_OPERATION)
}

reset(){
    this.model.reset();
    this.updateDisplayAndState(FIRST_OPERATION)
}

calculate(value){
    this.model.calculate(value);
    this.updateDisplayAndState(FIRST_OPERATION)
}
```

Listing 2-15 `CalculatorViewModel.js`

All of them are doing two main operations:

# 1. 他们正在更新模型。
# 2. 他们正在向视图分派要显示的新值。

It’s now clear that the majority of application business logic is present in the `view-model`; therefore if we want to reuse a specific component or part of the application. we need to maintain the event or the binding contract between the view and the `view-model` as it is. So we can say that these two objects become slightly more tightly coupled compared to their relation in the `MVP` architecture. Last but not least, it’s time to discuss the model. As you can see, the model just exposes a few methods in order to update the value to display; it doesn’t have any data manipulation or any state, and in this case the model is just a data representation of the main application so if we need to use it in combination with another `view-model`, the model will provide the data expected and nothing more. See `Listing 2-16`.

```javascript
export default class CalculatorModel{
    constructor(){
        this.totalOperation = 0;
    }
    calculate(operation){
        this.totalOperation = math.eval(this.totalOperation);
    }
    add(value){
        this.totalOperation = value;
    }
    reset(){
        this.totalOperation = 0;
    }
    get total(){
        return this.totalOperation;
    }
}
```

`Listing 2-16 CalculatorModel.js`

## JavaScript 框架

Now that we have explored the basics of `MV*` architectures, understanding the benefits and the issues related to each of them, it’s time to review what the JavaScript ecosystem is proposing and how these architectures are implemented. I’m sure you will realize very soon that understanding the three architectures mentioned before will help you to capture the technical approaches provided by well-known frameworks like `Angular`, `Ember`, or `Redux`. In this section we are going to re-create our calculator application three more times: the first one with `Angular`, then with `Ember`, and we will conclude this journey with the combination of `React` and `Redux`.

### 有角的

`Angular` is an open source framework created and maintained by Google. It’s been around since 2010 and has reached recently version 5, but from now on Google explained that we should just call it `Angular`. This framework passed through different stages in the past years, from a huge hype when launched with version 1; then, after the announcement of version 2 not being retro compatible with the previous version, many developers decided to move away from it and embrace other frameworks – in particular, `React` and `Flux` or `Redux` or similar combinations. Recently, as in 2017, Google released a new version that should enhance the experience of the JavaScript developers with `Angular`, providing a complete ecosystem of tools and patterns in order to work easily with this framework without the need to scrap the Web for assembling multiple libraries inside the same project. Since version 2, `Angular` embraces `TypeScript` as its main language; JavaScript ES6 and Dart are supported too, but the vast majority of the resources in the `Angular` website are prepared with `TypeScript`. `Angular` as a framework is providing a full architecture and utilities out of the box with also an interesting CLI tool for speeding up a developer’s productivity.

`TypeScript` is a typed superset of JavaScript, and it enhances the language adding `Enum`, `Generics`, `Abstract`, and `Interfaces` very familiar with strictly typed languages like Java, for instance. More information on the official website: [`www.typescriptlang.org`](https://www.typescriptlang.org)

#### 角度是如何工作的

The architecture we will evaluate in this section is related to `Angular` 2 and onward (Figure 2-6), so we won’t take into consideration the previous one because it works in a different manner.

![A446957_1_En_2_Fig6_HTML.jpg](img/A446957_1_En_2_Fig6_HTML.jpg)

Figure 2-6 `Angular` architecture example

`Angular` architecture introduces four interesting concepts :

*   依赖注入

*   使用 `NgModules` 和组件的模块化

*   模板和组件之间的数据绑定

*   大量使用 decorators 来定义对象，如组件或模块

我们将更深入地探讨这个架构，分析构成 Angular 项目的不同部分。任何 Angular 应用程序都至少有一个称为根模块的 `NgModule`；`NgModule` 不同于普通的 JavaScript 模块。`NgModule` 的特殊之处在于其元数据；在每个 `NgModule`（包括根模块）中，我们将描述模块的组成，特别是定义哪些组件属于该模块，模块是否依赖于其他模块、指令或组件；我们还会描述模块中组件所使用的服务。如前所述，任何 Angular 应用程序都至少包含一个称为根模块的模块。例如，这是我们为 Angular 计算器项目创建的根模块：

```typescript
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { AppComponent } from './app.component';

@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

正如你在这段代码片段中看到的，Angular 主要由装饰器（`@NgModule`）组成，用于将我们的代码包装在 Angular 框架内。

### 装饰器

JavaScript 中的装饰器实际上处于提案（第 2 阶段），有望成为语言的一部分（有关该提案的更多信息，请参见此链接：[`github.com/tc39/proposal-decorators`](https://github.com/tc39/proposal-decorators)）。它们基于一个简单的概念：装饰器包装一个函数，在不操作被装饰函数的情况下增强其功能。通常，在装饰器中，我们会添加在项目运行时应该通用的方法或属性；这就是为什么 Angular 大量使用它们的原因。这样我们就可以编写逻辑而无需继承某个基类，从而避免在代码和框架内置功能之间创建紧密耦合的连接。

下一个要介绍的主题是 Angular 组件。自 Angular 2 起，我们就可以创建组件，可以将它们视为 Angular 中存在的模板系统与我们可以从 REST API 或特定端点检索的数据之间的桥梁。它们的主要作用是检索数据，并通过 Angular 2 中开箱即用的绑定系统用新数据填充模板。Angular 组件是演示者和视图模型中属性的混合体。事实上，以下几点是正确的：

*   组件通过绑定更新视图，就像在视图模型对象中一样。
*   组件和模板之间的关系始终是一对一的，就像演示者一样。
*   该组件处理模板中发生的所有用户交互，就像对演示者一样。

In order to define a component in Angular we need to specify another decorator, `@Component`. For our calculator example, we have defined just one component considering how simple the application is; but potentially we could have split them in multiple parts as shown in Listing 2-17.

```typescript
const ON_SCREEN_KEYBOARD = [
  ['7', '8', '9', '/'],
  ['4', '5', '6', '*'],
  ['1', '2', '3', '-'],
  ['0', '.', '=', '+']
];

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css'],
  providers: [UpdateTotalService, CalculatorModel]
})
export class AppComponent implements OnInit {
  onScreenKeyboard = ON_SCREEN_KEYBOARD;
  total: string;

  constructor(private updateTotalService: UpdateTotalService) {}

  ngOnInit(): void {
    this.total = this.updateTotalService.reset();
  }

  updateTotal(value: string) {
    switch (value) {
      case 'AC':
        this.total = this.updateTotalService.reset();
        break;
      case '=':
        this.total = this.updateTotalService.calculate();
        break;
      case '+':
      case '-':
      case '/':
      case '*':
      case '.':
        this.total = this.updateTotalService.addSymbol(value);
        break;
      default:
        this.total = this.updateTotalService.addValue(value);
        break;
    }
  }
}
```

Listing 2-17 `App.component.ts`

As you can see in the decorator (`@Component`) we are specifying the HTML template and the CSS associated with the component. The objects to inject will be accessible inside the component but they will be instantiated by Angular and a selector that is just an ID to identify the component. The last thing to mention about Angular components is that we can use life-cycle hooks like `ngOnInit` method that is triggered when the component is initialized and in our case we use it to set the first value in our calculator display.

Now it’s time to see how the components interact with templates. Angular has its own way to handle HTML markup; we can use some directives in order to iterate through a data structure for displaying multiple HTML elements or for adding specific attributes if a certain condition happens in our code.

## Directives

Directives in Angular are instructions for the HTML template on how the template should handle the DOM. In our example we have associated an `app.component.html` template to the `app.component.ts` described above and this is the code used for the template:

```html
<div class="displayStyle">{{total}}</div>
<div class="acStyle" (click)="updateTotal('AC')">AC</div>
<ul *ngFor="let row of onScreenKeyboard">
  <li *ngFor="let value of row"
      class="btnStyle"
      (click)="updateTotal(value)">
    {{value}}
  </li>
</ul>
```

In this code it is important to highlight a few concepts. The first concept is that the binding with the `total` property is created inside the component, so every time we are updating this value, the new value will automatically be displayed inside our template. The other interesting thing is how we have created the buttons in our calculator; we are iterating an array present in the component object and with `*ngFor` directive we are creating multiple tags with different values; then, when a user clicks on the button we are calling the method `updateTotal` passing the button’s label; also in this case we use Angular markup for triggering the user interaction (`(click)="updateTotal(value)"`).

Interestingly enough, just with few lines of code. Angular provides a simple but very powerful mechanism of data binding, templating, and styling our views. It’s time to talk about the dependency injection part; as we saw in the component decorator we are defining an array of providers. Basically we are injecting `UpdateTotalService` and `CalculatorModel` to the component. The `CalculatorModel` won’t be used by the component but if we want to inject this object inside the service, we need to specify it there.

What is a Service in Angular? A Service is inspired to the Command Pattern, and it’s used for fetching data from an endpoint or for retrieving/passing data to a model like in our case.

## Command Pattern

The command pattern is a behavioral pattern. A command contains all the objects needed to perform an action when a specific event happens inside an application such as user interaction, data fetching, and so on. Usually the command pattern has an interface with just one public method called `execute`; and after instantiation, where we can inject the objects needed for performing the action, we can call this method for performing the logic described inside the command. Usually the command is used for decoupling the interaction within objects like the controller and model or presenter and model, and then we maintain them totally decoupled.

This is our `UpdateTotalService` class as shown in Listing 2-18.

```typescript
@Injectable()
export class UpdateTotalService {
  constructor(private model: CalculatorModel) {}

  calculate() {
    this.model.calculate();
    return this.model.total;
  }

  addValue(value) {
    this.model.add(value, VALUE);
    return this.model.total;
  }

  addSymbol(value) {
    this.model.add(value, SYMBOL);
    return this.model.total;
  }

  reset() {
    this.model.reset();
    return this.model.total;
  }
}
```

Listing 2-18 `UpdateTotalService.ts`

This service exposes a few methods that will be used for manipulating the model in a specific way. The model is injected in the constructor via the framework; therefore when we call `this.updateTotalService.reset();` from the component class we won’t need to have any knowledge of the model but the service will return the value retrieved from the model, isolating the view from the model like we have seen in the MVVM pattern, for instance. Another important thing to notice in this class is the `@Injectable` decorator provided by Angular for describing a class that is injected inside a component; just adding this decorator, the framework knows when to instantiate and inject.

The last part to describe is our model that is a very basic model with some knowledge on how to manipulate the data to store. Here again we are using the `@Injectable` decorator for the possibility of being injected by Angular. See Listing 2-19.

```typescript
@Injectable()
export class CalculatorModel {
  private state: string;
  private totalOperation: string;

  constructor() {
    this.state = FIRST_OPERATION;
    this.totalOperation = '0';
  }

  get total() {
    return this.totalOperation;
  }

  reset() {
    this.totalOperation = '0';
    this.state = FIRST_OPERATION;
  }

  add(value, type) {
    if (type === VALUE) {
      this.totalOperation = this.getValue(value);
    } else {
      this.totalOperation = this.checkSymbol(value);
    }
    this.state = IN_PROGRESS_OPERATION;
  }

  calculate() {
    this.totalOperation = math.eval(this.totalOperation);
    this.state = FIRST_OPERATION;
  }

  checkSymbol(value) {
    const str = this.totalOperation;
    if (this.state === FIRST_OPERATION) {
      return str + value;
    }
    return !isNaN(Number(str.charAt(str.length - 1))) ? str + value : str.substr(0, str.length - 1) + value;
  }

  getValue(value) {
    return (this.totalOperation === '0' || this.state === FIRST_OPERATION) ? value : this.totalOperation + value;
  }
}
```

Listing 2-19 `CalculatorModel.ts`

Our model is a simple object that contains the source of truth for our application, maintaining the state and the value that should be displayed inside the views.

Just to summarize the Angular calculator example: we have seen the key concepts of Angular, we also noticed quite a few similarities with MVVM and MVP architectures like the relation between view and template or who is handling the user interactions. The last thing worth mentioning is that Angular can become a reactive architecture pretty easily, because it incorporates Rx.js library inside the framework; therefore, within a certain degree, we can twist this architecture to a Reactive one almost out of the box.

# 余烬

Ember 是一个面向 Web 应用程序的框架。它在 JavaScript 社区中广为人知，并被 Netflix 或 Groupon 等大型组织使用。Ember 拥有一个有趣的生态系统，由 `EmberCLI`、`EmberData` 和作为 JavaScript 框架的 Ember 组成。Ember 背后的范式与其他框架略有不同，但如果应用程序符合该范式，其生产力会很高。Ember 倾向于约定优于配置；需要记住的关键一点是拥抱 `EmberCLI` 工具，因为它将简化你的工作并提升你的生产力。CLI 负责生成特定视图（模板、路由、单元测试文件等）、模型、路由甚至控制器所需的所有文件。当项目采用“胖服务器 – 瘦客户端”架构时，Ember 框架会大放异彩，其中大部分逻辑在服务器端处理；客户端应尽可能简单，并且应该是多页面应用程序，而非单页面应用程序（SPA）。

## Ember 是如何工作的

Ember 架构（图 2-7）基于 MVVM 架构，该框架由以下几个关键元素组成：

*   路线

*   模板

*   成分

*   控制器

*   模型

![A446957_1_En_2_Fig7_HTML.jpg](img/A446957_1_En_2_Fig7_HTML.jpg)

# Ember 架构示例

在 Ember 中，一切始于路由模块，每个视图都与关联的端点紧密耦合。默认情况下，任何 Ember 应用程序都有一个主路由系统，我们在其中定义所有应用路由；例如，在我们的计算器应用中，我们定义了如清单 2-20 所示的路由。

```javascript
Router.map(function() {
  this.route('calculator', {path: "/"});
});
```

**清单 2-20** `Router.js`

这意味着我们 Web 应用程序的根路径将被路由到 `calculator` 路由。由于 Ember 基于约定工作，我们需要将模块保存在正确的文件夹中才能被识别。但幸运的是，Ember CLI 提供了帮助，它提供了一些有用的命令来开箱即用地准备所有需要的文件：

`Ember generate route <name>`

CLI 会自动生成一个路由文件、关联的测试文件和模板，然后还会将新路由的代码插入到应用路由系统中。

我们为计算器创建的路由将模型暴露给模板；在 Ember 中，只有路由和控制器可以访问模型。因此，视图与模型之间实现了强关注点分离，这符合 MVVM 架构的理念。

```javascript
export default Ember.Route.extend({
    model(){
        this.store.push({
            data:[{
                id: 1,
                type: "calculator",
                attributes: {
                    total: "0",
                    state: AppState.FIRST_OPERATION
                },
                relationships: {}
            }]
        });
        return this.store.peekRecord('calculator', 1);
    }
});
```

Ember 中的 Model 概念与我们通常的理解略有不同；`Model` 类定义了值将存在于存储中（具体的模型），这有助于在数据到达 Ember 应用客户端时进行验证。如果我们查看位于 `models` 文件夹中的模型类，可以看到我们定义了两个属性，都是字符串类型，一个叫 `total`，另一个叫 `state`。参见清单 2-21。

```javascript
import DS from 'ember-data';

export default DS.Model.extend({
    total: DS.attr("string"),
    state: DS.attr("string")
});
```

**清单 2-21** `models/calculator.js`

从上面的路由代码可以看出，`store` 是一个为整个应用程序集中的数据结构（可以将其视为单例），可由路由和控制器访问。因此，无论我们的应用程序由多少个模板、路由器或控制器组成，我们都有一个获取或缓存数据的唯一真实来源。

`store` 对象非常有趣，因为它允许数据自动化并从端点获取，然后它会将响应直接存储在 `store` 对象内部，而无需处理定义这些操作的所有代码。`store` 与记录（record）一起工作；记录是从 REST 端点获取或在应用程序内缓存（如我们的案例）的具体模型。注意存储在记录中的对象结构非常重要，如清单 2-22 所示。

```javascript
{
    id: 1,
    type: "calculator",
    attributes: {
        total: "0",
        state: AppState.FIRST_OPERATION
    },
    relationships: {}
}
```

**清单 2-22** `routes/calculator.js`

在 `store` 内部，我们需要定义具有唯一 ID 的 `data` 对象；一个 `type` 值，用于后续检索；以及我们需要存储在其中的属性，对于我们的应用，将是 `total` 值和应用状态。由于我们没有从任何远程端点获取数据，我们使用 `peekRecord` 方法，它将跳过服务器请求并直接从具体模型检索数据。相反，如果我们需要从特定的 REST 端点检索数据，我们可以使用 `findRecord`，它将对指定的端点执行请求，并将响应存储在 `store` 对象中。

我们的应用逻辑位于控制器中，我们在其中为视图提供可调用的公共方法，并处理与模型之间的数据设置和获取。当我们从 Ember 框架扩展基础控制器时，我们有一个名为 `actions` 的对象，我们可以在其中向模板暴露所有方法；在我们的案例中，我们只有一个名为 `updateTotal` 的方法：

```javascript
export default Ember.Controller.extend({
    onScreenKeyboard: [
        ["7", "8", "9", "/"],
        ["4", "5", "6", "*"],
        ["1", "2", "3", "-"],
        ["0", ".", "=", "+"]
    ],
    actions:{
        updateTotal(value){
            let result;
            let model = this.store.peekRecord('calculator', 1);
            switch (value) {
                case "AC":
                    model.set("total", 0);
                    model.set("state", AppState.FIRST_OPERATION);
                    break;
                case "=":
                    result = math.eval(model.get("total"));
                    model.set("total", result);
                    model.set("state", AppState.FIRST_OPERATION);
                    break;
                case "+":
                case "-":
                case "/":
                case "*":
                case ".":
                    result = checkSymbol(model.get("total"), value, model.get("state"));
                    model.set("total", result);
                    model.set("state", AppState.CALCULATING);
                    break;
                default:
                    result = getValue(model.get("total"), value, model.get("state"));
                    model.set("total", result);
                    model.set("state", AppState.CALCULATING);
                    break;
            }
        }
    }
});
```

在这里，每个 `case` 中我们都从 `store` 检索当前值并进行操作，但令人烦恼的部分是 Ember 大量使用字符串来标识特定对象或值。因此，我们将没有编辑器提供的代码补全，如果未正确包装在常量语句中，可能容易出错。

最后要讨论的是模板。Ember 开箱即用地使用 Handlebars；因此，如果我们熟悉这个著名的模板库，我们将很快上手。Handlebars 有一些特定的标记，类似于 Angular，用于识别特定行为，如过滤、创建由控制器或路由检索的数据填充的相似标签等。这是渲染计算器所需的 Handlebars 代码：

```html
<div class="displayStyle">{{model.total}}</div>
<div class="acStyle" {{action "updateTotal" 'AC' on="mouseDown"}}>AC</div>
{{#each onScreenKeyboard as |row|}}
<ul>
  {{#each row as |value|}}
    <li class="btnStyle" {{action "updateTotal" value on="mouseDown"}}>{{value}}</li>
  {{/each}}
</ul>
{{/each}}
```

有趣的是，我们如何通过 `action` 命令处理控制器暴露的方法：

`{{action "updateTotal" value on="mouseDown"}}`

我们通过传递一个名为 `value` 的参数来标识方法名，并指定方法应何时被调用：在本例中是鼠标按下时。

## 总结

总而言之，Ember 是一个基于 MVVM 架构的高效框架，提供了许多开箱即用的常见活动。它推崇约定优于配置，拥有由不同工具和库组成的丰富生态系统，并且文档非常详尽。

# 反应+还原

React and Redux is a combination of libraries that together can resemble a minimal framework with a large ecosystem that is not imposed at the beginning like for Ember or Angular but is more oriented to a plug-in system where we can use what we really need without the need to import everything up front. React is a library useful for manipulating the DOM, based on components as first citizen; it takes care of the view part of an architecture only, implementing smart algorithm like the reconciliation one – a diff algorithm used for rendering only the part of the DOM that should change. React introduced a very powerful concept embraced nowadays by several other libraries: the `Virtual DOM`. The `Virtual DOM` is a DOM representation where the diffing algorithm operates at first glance and where React understands what should change and when, minimizing the changes to the real DOM and improving the performances of our web applications.

Reconciliation is a key concept for React. If you want to know more, I suggest reading the official documentation regarding this topic: [`react-cn.github.io/react/docs/reconciliation.html`](https://react-cn.github.io/react/docs/reconciliation.html)

On the other hand, Redux is a state container not tightly coupled with React because we can find examples of Redux used in combination with other frameworks like Angular. Redux is solving a well-known problem of how to manage the state inside an application. The most interesting part of it is that it leverages a concept introduced in 2015 from another library called Flux, created by Facebook, of `unidirectional flow`. `Unidirectional flow` is a powerful but simple concept: the objects communication inside an application should be unidirectional, and this, in combination with good encapsulation, will allow any application to be easier to debug, to be picked by any team because also complex applications are easy to understand and debug, thereby improving the code quality and the possibility of extending them.

The Redux paradigm is straightforward and is composed by only three key elements:

*   `action`
*   `reducer`
*   `store`

The `action` is just a plain JavaScript object containing the information of what happened inside the application. The `reducer` is retrieving from an `action` that the interaction happened in the application and knows how the state should change based on the `action` dispatched. Finally, the `store` is the object that brings all together; the `store` is passing to the `reducer` the current state tree and the `action`, and it waits until the `reducer` provides back the new state, then the `store` will append to the state tree and all the objects that are listening for a change will be notified.

Redux was created on top of three core concepts:

*   `Single source of truth`: The state of your application is stored in an object tree within a single `store`.
*   `State is read-only`: The only way to change the state is to emit an `action`.
*   `Changes are made with pure functions`: The `reducer` is a pure function that takes the current state tree and an `action`, and returns the next state tree. If we always pass the same arguments to a pure function, we know the output; in this case, the `reducer` is always the same.

Let’s see React and Redux in action with our calculator example written for the last time with a different architecture.

## Redux 如何工作

```javascript
if (condVar > someVal) {console.log("xxx")}
```

The starting point of any Redux project (Figure 2-8) is usually a main application where we create the `store` object and we wrap our main view inside a `Provider` object from the Redux library. See Listing 2-23.

```javascript
export default class App{
    constructor(){
        const store = createStore(CalculatorReducer);
        const cont = document.getElementById("app");
        ReactDOM.render(
            <Provider store={store}>
                <CalculatorContainer/>
            </Provider>, cont);
    }
}
let app = new App();
```

Listing 2-23 `App.js`

![A446957_1_En_2_Fig8_HTML.jpg](img/A446957_1_En_2_Fig8_HTML.jpg)

Figure 2-8 Redux project architecture diagram

As we mentioned before, after creating the `store` and associating it to a specific reducer (`CalculatorReducer`), we are wrapping our main view (`CalculatorContainer`) inside a `Provider` object from the redux library. Think about the `Provider` as an object that receives as input the `store` and it propagates it to all the container components inside an application in order to have complete access to it.

Considering we have mentioned the container components, it’s time to move to the view part, where we need to distinguish between presentational components and container components. The creator of Redux, Dan Abramov, wrote a post on Medium.com regarding this topic where is explaining the difference between the two types of components. To summarize Dan’s thoughts, in Redux we distinguish the presentational components as component with the only scope of managing the look and feel of the view, more or less like the Passive View described in this chapter. Meanwhile we identify the container components as the ones that can handle the presentational component logic like user interactions, having access to the `store`, and mapping the `store` values to the React component via a `props` object, similar to the Supervising controller of the MVP architecture. This approach will lead to a better separation of concern and reusability of our components across different projects.

**Presentational vs. Containers components**

In Redux this is a very important topic. I strongly suggest having a look at this link to Dan Abramov’s Medium post on the presentational and containers components explanation: [http://bit.ly/1N83Oov](http://bit.ly/1N83Oov)

Based on what we have just described, it’s time to see what a presentational component looks like and how we handle the communication with the Redux framework. In the calculator example, our presentational component code looks like that shown in Listing 2-24.

```javascript
export default class Calculator extends React.Component{
    constructor(props){
        super(props);
        this.onBtnClicked = this.props.onButtonClicked;
    }
    createRow(id, ...labels){
        let items = labels.map((value, index) => {
            return <li key={`${id}_${index}`}
                       style={value === "AC"? acStyle : btnStyle}
                       onClick={::this.onBtnClicked}>
                       {value}
                   </li>;
        })
        return(
            <ul key={id} style={ulStyle}>
                {items}
            </ul>
        )
    }
    render(){
        return(
            <div>
                <div style={displayStyle}>{this.props.result}</div>
                {this.createRow("row1", "AC")}
                {this.createRow("row2", "7", "8", "9", "/")}
                {this.createRow("row3", "4", "5", "6", "*")}
                {this.createRow("row4", "1", "2", "3", "-")}
                {this.createRow("row5", "0", ".", "=", "+")}
            </div>
        );
    }
}
```

Listing 2-24 `Calculator.jsx`

We can immediately spot that we are focusing more on what our component looks like than what it does. The only method that is going to be used is the `onButtonClicked` method retrieved from the `props` object as well as the `result` property that will be used to show the calculation inside the calculator display `div` element. The entire logic of this component as well as the communication with the rest of the application is made inside the container component, as shown in Listing 2-25.

```javascript
const mapDispatchToProps = (dispatch) => {
    return {
        onButtonClicked: (evt) => {
            evt.preventDefault();
            let value = evt.target.innerHTML
            manageDisplayState(value, dispatch);
        }
    }
}
const manageDisplayState = (value, dispatch) => {
    switch (value) {
        case "AC":
            dispatch(reset())
            break;
        case "=":
            dispatch(calculate())
            break;
        case "+":
        case "-":
        case "/":
        case "*":
        case ".":
            dispatch(appendValue(value));
            break;
        default:
            dispatch(appendValue(value));
            break;
    }
}
const mapStateToProps = (state) => {
    return{
        result: state.result
    }
}
export default connect(mapStateToProps, mapDispatchToProps)(Calculator);
```

Listing 2-25 `CalculatorContainer.js`

In this module, we can find many interesting things like the following:

### Redux 核心概念

表示组件是在调用 `connect` 方法后创建的。`connect` 方法用于将存储“连接”到组件，它返回一个更高阶的 React 组件类，该类将状态和动作传递到从所提供的参数派生的组件中。

`mapStateToProps` 是我们用来将存储传递的新状态转换成将在表示组件中呈现的属性的方法。

`mapDispatchToProps` 是我们用来触发容器组件内部发生的所有调度操作的方法。`dispatch` 方法接受一个动作，该动作将被触发并被列在存储中，然后被传递给 reducer。

在进入 reducer 之前，最好先理解什么是 action。基本上，action 就是一个普通的 JavaScript 对象。检查 `CalculatorAction` 模块，我们可以在清单 2-26 中清楚地看到它。

```javascript
export function calculate(){
    return {
        type: CALCULATE
    }
}

export function reset(){
    return {
        type: INIT,
        result: 0
    }
}

export function appendValue(value){
    return {
        type: APPEND,
        toAppend: value
    }
}
```

**清单 2-26 CalculatorActions.js**

Action 类似于命令，它们携带了 reducer 所需的信息，以便将当前状态更改为新状态。通常它们有一个 `type` 属性，用于定义从 `store.dispatch` 方法调用的动作类型。

终于轮到 reducer 了。当我们需要更改应用程序状态时，就会使用 reducer，因为 action 通知我们应用程序内部发生了某些事情，但它不知道应用程序应如何更改——这是 reducer 的职责。参见清单 2-27。

```javascript
const calculation = (state = reset(), action) => {
    switch (action.type) {
        case CALCULATE:
            return {
                type: action.type,
                result: math.eval(state.result)
            }
            break;
        case APPEND:
            return {
                type: action.type,
                result: resultHandler(state, action.toAppend)
            }
            break;
        case INIT:
            return {
                type: action.type,
                result: resultHandler(state, action.toAppend)
            }
            break;
        default:
            return state;
            break;
    }
}
```

**清单 2-27 CalculatorReducer.js**

在我们的 reducer 中，我们将默认状态设置为 `reset` action，它以初始状态（在我们的 `switch` 语句中是 `INIT`）启动应用程序。每次分发一个 action 时，store 都会调用 reducer，传递当前状态和分发的 action，然后 reducer 将 action 映射到下一个应用程序状态。

Redux 是一个非常简单但有效的状态管理库，有趣的是它与 MVP 架构有许多相似之处——特别是视图-呈现器的关系，就像我们在 Redux 中看到的表示组件及其容器一样。在 Redux 生态系统中，还有其他库可以与 Rx.js 或其他响应式库结合使用。

### 总结

在本章中，我们评估了不同的架构，从最古老的 MVC 到最新的 Redux。我们看到了它们清晰的演变，但也有很多对过去原则的引用。我们经常花很多时间在一个框架上，却没有问自己为什么作者选择这个决定而不是另一个。我希望这次穿越前端开发中最著名的架构/框架的旅程能为你们未来的项目提供一些好处，因为我真心相信，为了给特定项目选择合适的架构，对它们有很好的了解是非常重要的。我经常看到开发人员和架构师总是使用相同的架构来套用任何项目，而不是“用正确的工具做正确的事”。从下一章开始，我们将开始响应式之旅，这将引导我们从不同的角度学习响应式架构的工作原理，并将更多地了解 Cycle.js、MobX 和 SAM。