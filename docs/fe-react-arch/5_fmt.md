# 五、MobX：简单状态管理

> 做，或者不做。没有‘尝试’。—尤达

After a deep dive into the first reactive framework, we can continue our journey discovering others’ reactive architectures, reviewing a flexible and easy-to-use state management system like `MobX`. `MobX` is simple but very effective state management system that we can integrate in many projects independently from the stack we are using. Obviously being simple doesn’t mean incomplete; we can really structure complex applications with the help of `MobX` state tree: an opinionated, transactional and `MobX` powered state container heavily based on the tree data structure for handling the application state. During this chapter, we are going to review a couple of examples that will give us an idea of how to structure a `MobX` application and what are the benefits of using it in our projects.

## `MobX` 简介

The philosophy behind `MobX` is very simple: Anything that can be derived from the application state, should be derived. Automatically. Usually when we have a non-reactive application and we want to update its state, we create methods for manipulating manually specific parts of the model and for updating the views, potentially causing inconsistency between the two, sometimes increasing the coupling between them. With `MobX` we are trying to minimize these situations having an automated update, via subscription (`observer`), to reactions that happened at the application state level (`observables`). `MobX` has a few key concepts to remember in order to properly embrace the library:

*   `observables` 用于导出我们的应用程序的状态；这里的经验法则是，一个可观察对象应该存储一个定义我们的应用程序中的状态的值。

*   `computed values` 是用于以同步方式使用函数从应用程序状态自动导出复杂值的属性；这种机制允许更好地预测和调试我们的计算值。计算值延迟更新。

*   `reactions` 类似于计算值，因为从应用程序状态开始生成新值，但在这种情况下，它们用于生成副作用。

*   `actions` 是用于改变应用程序状态的方法；如果我们指定在 `strict mode` 下使用 `MobX`，动作是更新应用程序状态的唯一方式。

*   `observers` 在视图中用于对任何状态变化做出反应。

With just these concepts we can start to build a simple MobX application to get familiar with it. MobX finds the perfect fit with Virtual DOM libraries like React.js; actually, React and MobX are a great combination for creating reactive applications, but obviously this won’t mean other frameworks are out of scope, but React is combined perfectly with the MobX philosophy.

Before going ahead, let’s see MobX in practice with a simple counter:

```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import {observable} from 'mobx';
import {observer} from 'mobx-react';

const counterState = observable({
    value: 0,
    inc: function(){
        counter.value++
    },
    dec: function(){
        counter.value--
    }
});

@observer
export class Counter extends React.Component{
    render(){
        return (<div>
            {this.props.state.value}
            <div>
                <button type="button" onClick={this.props.state.inc}>increment</button>
                <button type="button" onClick={this.props.state.dec}>decrement</button>
            </div>
        </div>)
    }
}

ReactDOM.render(
    <Counter state={counterState}></Counter>,
    document.getElementById("app")
);
```

In this example, we used React.js creating a component called `Counter` and passing an external object called `counterState` that represents the application state. `CounterState` is an observable, and it contains a property called `value` that is updated by two actions (`inc` and `dec` functions). The observable object, created in this way, automatically treats the methods that are updating the observable as actions. Every time one of these actions is triggered by the user interface, the `value` property will be updated and, by reaction, the view that is observing the application state.

If we now check the React component we can immediately spot an `@observer` – this is a decorator that wraps our component into a function allowing us to create the reaction between what is observed in the React component and the application state present in the MobX observable.

## ES7 Decorators and MobX Project Setup

The Decorator pattern is used to express high-order function with a simple syntax; in fact, a decorator expressed with the syntax `@decoratorName`, it represents a function that is wrapping another function, extending its behavior. Decorators in JavaScript are introduced with ES7; therefore they are a fairly new addition to JavaScript specifications, and any decorator we are using in our examples can be removed if you prefer working directly with functions, as they are just a convenient way that MobX provides them out of the box to save you from having to write tons of code.

In our example, we were decorating a React component in order to augment its behaviors and observe any change happening to a computed value or an observable. When we work with decorators and we use transpilers like Babel, we need to remember to set up the MobX project correctly using the Babel’s plug-ins: `babel-plugin-transform-decorators-legacy` and `babel-plugin-transform-decorators`. These two plug-ins allow our MobX project to be transpiled correctly without any issue. Anyway, you can consult the setup of our examples, as they are all using Babel in combination with webpack and I kept to the bare minimum modules to strictly transpile the project.

In few lines of code we were able to create a simple MobX – React application separating completely the application state and its behaviors from the user interface, allowing us to potentially reuse these two elements or changing them without incurring any potential bugs introduced by the changes. Also, MobX embraces fully the reactive paradigm where the application state is updated via observables decoupling de facto the view from the model via a common contract that can be understood by the two main actors.

A nice thing to highlight is how MobX optimizes the computed values updated behind the scenes; this reactive library is taking care of updating only the computed values that are linked to a specific state, only when they change, and it won’t compute all the others present in the application. The mechanism is very similar to what React.js is doing during the reconciliation phase: checking what are the differences inside the dependency tree and via a diffing algorithm, updating only the values that should be changed, maintaining high performances also when the tree is deep and complex like in large applications.

## React Reconciliation and MobX Diffing Algorithm

React and MobX are sharing a couple of concepts like the diffing algorithm. Understanding how these algorithms work is very important because we can improve the performance of our applications just by following the best practices described in the documentation. Therefore I thought it would be beneficial to share some links for getting deeper into this argument:

- React reconciliation: [`bit.ly/2xHYifx`](http://bit.ly/2xHYifx)
- In-depth explanation of MobX: [`bit.ly/2g2fYIW`](http://bit.ly/2g2fYIW)
- Fundamental principles behind MobX: [`bit.ly/2x7YcJL`](http://bit.ly/2x7YcJL)

After this introduction on the key elements behind MobX, it’s time to see a MobX application in the real world, so in this case we are going to build a MobX and React application based on an image gallery and the same one with MobX state tree. In this way we can compare the two approaches, understanding the pros and cons of both and describing the different architectural approach.

## 我们的第一个 MobX 和 React 应用

In this chapter’s projects, we are exploring four key architectural areas to cover in the majority of front-end applications like these:

*   如何在应用程序中管理数据流。
*   如何处理用户交互？
*   如何使用远程端点。
*   如何管理应用程序的状态？

The application we are going to build is an image gallery similar to Figure 5-1.
![A446957_1_En_5_Fig1_HTML.jpg](img/A446957_1_En_5_Fig1_HTML.jpg)
Figure 5-1 The project UI
It could be interpreted as a component of a larger application but it’s important to cover the concepts listed above and then we will be able to replicate them in projects at scale. The service we are going to consume for searching images is the `Flickr` APIs. `Flickr`s allow us to perform a search on their catalogue based on a specific search term. Figure 5-2 shows how our `MobX` application is structured.
![A446957_1_En_5_Fig2_HTML.jpg](img/A446957_1_En_5_Fig2_HTML.jpg)
Figure 5-2 `MobX` project architecture diagram
On the left of the diagram we have all the views, and in our case are passive views that are used only for rendering the user interface and gathering the interactions handled by the stores. Each view has a 1 to 1 relation with a store; this is very similar to what we have done in the second chapter when we analyzed the `MVP` architecture. Doing it will guarantee a great separation of concerns (view representation and application behaviours are separated) and components reusability (we could change completely the views without affecting the application behaviours and vice-versa) – two important principles to take into consideration in any software architecture. On the right we have instead all the stores instantiated from the `stores` object. In each store we are going to inject an application model called `GalleryModel`. This is not a concept requested by `MobX`, but we want to create solid architectures with great flexibility and high code standards, creating a model that will allow us to gather all the application states, or component states, inside a unique object that will be visible to all the stores. In this way, it will be easier to maintain separation of the different stores that are not aware of the existence of others and on top will facilitate the application debugging because the application state is present in a unique place instead of being spread in different stores. Considering `MobX` uses `observables` for storing the application state, this means that inside the model we are going to have `observables` and potentially some `actions` for changing the `observable`, but all the `computed values` will be closer to the view at the store level. Let’s now take a look at the folder structure used for this project.

### 检查文件夹结构

# MobX 项目结构与实现

显然，该项目也存在于本书的 GitHub 仓库中，因此请随意下载，以便更好地帮助您完成后续步骤。如图 5-3 所示，文件夹结构类似于我们的架构，按文件类型（stores、models、components 等）进行划分，而不是按领域方法（一个搜索文件夹、一个图片库文件夹……）。

![A446957_1_En_5_Fig3_HTML.jpg](img/A446957_1_En_5_Fig3_HTML.jpg)
图 5-3 MobX 项目结构

在大型应用中，我通常更喜欢将文件收集在领域文件夹中，这确实有助于更好地理解应用，特别是对于首次接触应用的新开发者。当名称足够有意义时，这种方式更容易找到屏幕上看到的内容与文件夹名称之间的关联。

是时候编写我们的 MobX 应用了，但我们要做的第一件事是构建应用的总体结构，并在 `App.js` 文件中定义它。我们需要创建三个主要的 React 组件：一个用于执行搜索的文本输入框、一个图片网格和一个用于更好查看图片的选中图片组件。

```javascript
@observer
export class App extends React.Component{
    render(){
        return (
        <Provider {...stores}>
            <MuiThemeProvider>
                <div>
                    <Form/>
                    <Grid style={style.child}/>
                    <SelectedPicture style={style.child}/>
                </div>
            </MuiThemeProvider>
        </Provider>)
    }
}

ReactDOM.render(
    <App/>,
    document.getElementById("app")
);
```

在这几行代码中，有几点需要强调：首先是 `@observer` 装饰器。我们在第一个 MobX 示例中已经见过它；该装饰器允许我们对状态变化做出反应，触发 React 组件内的 `render` 函数，就像我们更改 `props` 或 `state` 属性时会发生的情况一样。

组件的另一个关键点是 `Provider`，它包装了我们的整个 UI。这是由 `mobx-react` 库提供的，它是一个用于指定上下文的实用工具，该上下文可由应用程序的任何部分访问。在这种情况下，MobX 使用它通过依赖注入机制为我们的任何组件提供对 store 对象的访问，我们将在讨论 stores 时看到它的实际应用。

`Provider` 对象包含一个名为 `stores` 的属性，我们在其中提供了组成此应用程序的所有 stores：

```javascript
const model = new Model();
const photoGallery = new PhotoGalleryStore(model);
const search = new SearchStore(model);
const selectedPicture = new SelectedPictureStore(model);

export {
    photoGallery,
    search,
    selectedPicture
}
```

在 `stores` 对象中，我们刚刚暴露了与它们链接的视图交互的三个 stores。正如我们在项目架构图中之前看到的，我们希望视图和 stores 之间有 1 对 1 的关联；这将在未来帮助我们通过在其他项目中重用 MobX 应用程序的特定部分。

一个需要描述的重要事项是 `Model` 对象的作用。在传统的 MobX 架构中，我们没有模型的概念，应用程序状态分散在多个 stores 中。而在我们的应用程序中，我们使用它来包装所有表示应用程序状态的 observables，将特定领域的 observables、计算值和 actions 留在 stores 中。我们使用这种技术是因为，否则，为了更改另一个 store 中存在的特定 observable 的值或观察它，我们需要访问和了解其他 stores，不仅暴露 observable，而且可能调用 store 本身中可用的 actions 或其他方法。通过这种方式，我们拥有注入到每个 store 中的模型所表示的应用程序状态，它们可以直接观察或更改应用程序状态。这与响应式范式相结合，将触发更新所有观察特定 observable 的计算值，并更新相关的视图。

搜索领域由一个带按钮的输入文本组成：

```javascript
@inject('search')
@observer
export default class Form extends React.Component{
    setSearchTerm(evt){
        this.props.search.changeSearchTerm(evt.target.value)
    }
    search(){
        this.props.search.requestPics();
    }
    getButton(){
        return (
            <RaisedButton label="SEARCH" primary={true} style={style} onClick={this.search.bind(this)}/>
        )
    }
    getSpinner(){
        return (
            <CircularProgress size={35} thickness={5}/>
        )
    }
    render(){
        return (
            <div>
                <TextField hintText="e.g. Bear" onBlur={this.setSearchTerm.bind(this)}/>
                {
                    this.props.search.isSearching ? this.getSpinner() : this.getButton()
                }
            </div>
        )
    }
}
```

在 React 组件中，我们也可以注入我们的 stores 以便与它们交互；这里的经验法则是始终将组件的行为与外观解耦；实际上，将它们的逻辑委托给 store，该 store 将负责与应用程序的其余部分交互；从远程端点检索；或操作应用程序状态。这种方法允许我们在不引入行为错误的情况下更改视图，而且它绝对有助于测试组件及其行为，因为定义两个世界的边界变得非常容易。

MobX 提供了一个 `@inject` 装饰器来实现依赖注入；因此我们可以使用这个装饰器将 store 注入到 React 组件中。该 store 可通过任何 React 组件中可用的 `props` 属性访问，因此我们可以通过编写 `this.props.store.method` 来调用一个方法，其中 `store` 是我们分配给 store 的名称，`method` 是我们要调用的方法的名称。

该组件的最后一个重点是变量 `isSearching`，显然是一个布尔值，它是一个具有特定于该组件而非整个应用程序的领域的 observable。此属性不会在其他 stores 之间共享，因此我们可以将其定义在专门用于 `Form` 组件的 store 中，而不是将其定义为应用程序状态。作为一个 observable，每次值发生变化时，订阅该 observable 属性的视图将自动对变化做出反应，再次调用 `render` 函数。

让我们看看 `Form` 的 store 是如何实现的：

```javascript
import {observable, computed, action} from 'mobx';
import Config from '../configs/Configuration';

export default class SearchStore{
    @observable isSearching = false;

    constructor(model){
        this.model = model;
    }

    @computed get url(){
        return Config.getSearchURL(this.model.searchTerm);
    }

    @action changeSearchTerm(term){
        this.model.searchTerm = term;
    }

    @action requestPics(){
        this.isSearching = true;
        fetch(this.url)
            .then(this.onData)
            .then(this.onResult.bind(this))
    }

    @action onData(response){
        return response.json();
    }

    @action onResult(response){
        this.isSearching = false;
        this.model.picsList = response.photos.photo.map(value => {
            return {
                id: value.id,
                title: value.title,
                image: Config.getPicURL(value),
                large_image: Config.getLargePicURL(value)
            }
        });
    }
}
```

在构造函数中，我们在实例化 store 时注入了 `model`，并且我们还有之前描述的作为 observable 的 `isSearching` 属性。最后，我们实际看到了本章开头提到的计算值和 actions。在这个 store 中，我们有一个用于在 Flickr 上搜索图片的 URL 的 getter，这个计算值将在每次 `searchTerm`（存在于 `model` 中的 observable）发生变化时返回一个新的 URL。我们将在本描述结束时回顾主要的模型。

我想请您注意 `Config` 对象；它被用作静态对象来组合应用程序中的 URL，在这种情况下，我们只是在需要它的 store 中导入 `Config` 对象，但在下一个关于 MobX 状态树的示例中，我们将看到一种更优雅的不同方法。

每次文本输入触发 `blur` 事件时，我们都在调用 store 中的 `changeSearchTerm` 方法，更新 `model` 中的 observable。相反，一旦用户在 UI 中单击搜索按钮，我们就在执行搜索方法（`requestPics` 方法），在那里我们立即更改 `isSearching` observable，强制组件更新，将按钮替换为加载指示器，防止向服务器发出额外请求的可能性。之后，我们正在执行对 Flickr API 层的真实请求，一旦我们检索到响应，我们就在应用程序模型中存储接收到的数据的过滤版本，同时组合小图和大图的 URL（`onResult` 方法）。

正如我们到目前为止所看到的，我们已经更新了几个表示应用程序状态的 observables，位于 `model` 对象内部；现在是时候看看模型是什么样子了：

```javascript
import {autorun, observable} from 'mobx'

export default class GalleryModel{
    @observable searchTerm = "";
    @observable picsList = [];
    @observable selectedPictureURL = null;

    constructor(){
        autorun(()=> {
            if(this.picsList.length > 0){
                this.selectedPictureURL = this.picsList[0].large_image
            }
        })
    }
}
```

我们的模型由一组可被所有 stores 使用的 observables 组成；因此，任何使用它们的计算值都会对变化做出反应。同样在构造函数中，我们可以看到另一个 MobX 方法 `autorun`，它用于处理副作用，在我们的例子中，每次我们更改 `picsList` 数组时，我们都会定义第一张选中的图片，这样用户无需与网格交互就能看到图片。在这种情况下，我们无法使用计算值，因为它们只能是与特定 observable 值相关的 getter；所以 `autorun` 来拯救我们，它观察其主体中表达的任何值，并在每次该值更新时做出反应。`autorun`，就像计算值一样，只在 observable 发生变化时做出反应。在像这样的情况下，它非常方便，我们需要创建一个副作用，因为替代方案将是在一个不应该知道此值存在的 store 中更改 `selectedPictureURL`，考虑到它根本没有使用它。相反，通过 `autorun`，我们可以将更新逻辑包装在模型中，维护每个 store 与其视图 1 对 1 关联的契约，并更新与其自身相关的应用程序状态，而无需知道其他 observable 的存在。

在 `SearchStore` 中，我们更新了主模型中的 `picsList` observable 的值。现在轮到网格组件来渲染从远程服务检索到的结果了。

```javascript
@inject('photoGallery')
@observer
export default class Grid extends React.Component{
    render(){
        const tiles = this.props.photoGallery.pics.map(pic => {
            return (
                <Tile key={pic.id} data={pic}/>
            )
        })
        return (
          <div style={styles.root}>
            <Chip style={styles.chip}>
                {`total images: ${this.props.photoGallery.totalPics}`}
            </Chip>
            <GridList
                cellHeight={180}
                cols={3}
                padding={5}
                style={styles.gridList}>
                    {tiles}
            </GridList>
          </div>
        )
    }
}
```

`Grid` 组件有自己的 store `photoGallery`，它从中检索两个主要值：图片数组和一个名为 `totalPics` 的属性，用于显示从服务检索到的图片数量。另一件需要注意的事情是，我们正在动态创建 `Tile` 组件来填充 `GridList` 组件，并且每次我们从 store 更新 `pics` 属性时都会创建这些 `tiles`。

网格组件的 store 是这个：

```javascript
import {observable, computed, action, autorun} from 'mobx'

export default class PhotoGalleryStore {
    constructor(model){
        this.model = model;
    }

    @computed get pics(){
        return this.model.picsList
    }

    @computed get totalPics(){
        return this.model.picsList.length;
    }

    @action selectedPicture(url){
        this.model.selectedPictureURL = url;
    }
}
```

我们可以立即发现，`pics` 属性是一个计算值。这是我提供的一个建议，因为如果出于任何原因我们需要更改模型或 store，只要对象之间的契约保持不变，我们就不会与视图有任何耦合，我们将能够重构我们需要的内容而不影响视图。此外，模型与视图完全解耦，就像在 MVP 架构中一样，以简单的方式拥抱我们应用程序内部的未来变化，使其更健壮且易于测试。

每次用户单击图片时，我们需要更改选中的图片，但为了做到这一点，我们需要看看谁在调用 `PhotoGalleryStore` 中的 `selectedPicture` 方法。

```javascript
@inject('photoGallery')
@observer
export default class Tile extends React.Component{
    selectedPic(e){
        this.props.photoGallery.selectedPicture(this.props.data.large_image)
    }
    render(){
        return (
            <GridTile
                style={styles.gridTile}
                onClick={this.selectedPic.bind(this)}
                title={this.props.data.title}>
                <img src={this.props.data.image} />
            </GridTile>
        )
    }
}
```

在 `Tile` 组件中，我们注入了 `photoGallery` store 实例；这使我们能够通过简单地提供单个 tile 的 `props` 对象中存在的大图 URL 来更改大图。`selectedPicture` 方法随后将更新模型中的 observable，我们在其中存储用于检索大图的图片 URL，因此本示例的最后一部分是引入 `SelectedPicture` 组件。

```javascript
@inject('selectedPicture')
@observer
export default class SelectedPicture extends React.Component{
    render(){
        const url = this.props.selectedPicture.picPath;
        return (
            <Card style={style}>
                <CardMedia>
                    {
                        url ? <img src={url} /> : ""
                    }
                </CardMedia>
            </Card>
        )
    }
}
```

这个组件非常简单；我们只是在每次 `url` 属性更改时显示不同的图片。这里我们也可以看到 MobX 的模式：注入一个 store（`selectedPicture` store）并观察计算值或 observable 发生的任何变化。

`SelectedPicture` 组件的 store 组成如下：

```javascript
import {computed, action} from 'mobx';

export default class SelectedPictureStore{
    constructor(model){
        this.model = model;
    }

    @computed get picPath(){
        return this.model.selectedPictureURL || ""
    }
}
```

同样在这种情况下，我们不是向视图提供 observable，而只是通过 getter 提供一个计算值。

图 5-4 显示了此应用程序中的数据流。

![A446957_1_En_5_Fig4_HTML.jpg](img/A446957_1_En_5_Fig4_HTML.jpg)
图 5-4 数据流图

1. 用户与调用商店中的动作的 UI 进行交互。
2. 该操作更新了主模型中的一个可观察对象。
3. 订阅该可观察对象的所有计算值都在更新该值。
4. 视图中存在的计算值或可观察值导致呈现具有新状态的组件。

这是一个简单的 `MobX` 示例，展示了该框架如何通过 `observables` 使用响应式编程范式。我们可以看到，单一职责原则得到了尊重，并且在某种程度上由 `observables`、`computed values` 和 `actions` 强制执行。同时，我们也能发现一定的灵活性，这在大型团队中可能没有太大帮助，因为任何开发者都可能将这种灵活性视为在需要时走捷径的方式。不仅因为这个原因，也为了提供一种更结构化的方式来编写大型应用程序，我们可能会使用 `MobX state tree`，这是一个有主见的库，它为应用程序结构创建了清晰的边界，并有助于创建一个坚实且可扩展的架构，以有效的方式强制执行自动派生和不可变状态的概念。

### `MobX` 状态树

顾名思义，`MobX state tree` 基于不可变树，特别是该库提供的好处如下：

*   编写隐藏了反应式实现的“命令式”代码的可能性，因此接触反应式范例的新开发人员的学习曲线应该更容易。
*   可变数据结构，其中在幕后由库本身创建和维护不可变的树快照，还提供了与同构应用程序很好地工作的可能性，其中我们可以为特定状态注入树，应用程序将对此作出反应。
*   商店类型系统提供开箱即用的可能性，指定原始和自定义值。
*   商店级别的生命周期挂钩，类似于 `React` 组件中的挂钩。
*   明确如何构建存储以及如何更新应用程序状态的界限。

这些只是 `MobX state tree` 提供的一些关键特性。有趣的一点是，我们正在处理一个树结构，类似于我们在 UI 中使用 `DOM` 时所习惯的。同时，这意味着我们需要处理一个强大的层次结构。因此，当我们构建状态树时，我们需要更加注意如何封装数据以及如何处理树的不同分支之间的依赖关系。理解使用 `MobX state tree` 可能遇到的问题的最佳方式是尝试在我们之前的项目中实现它，以找到可能的解决方案和新的方法。重构之前的项目不需要太多更改；事实上，视图将几乎保持不变。在深入代码之前，请参考图 5-5 了解我们架构的鸟瞰图。![A446957_1_En_5_Fig5_HTML.jpg](img/A446957_1_En_5_Fig5_HTML.jpg) 图 5-5 `MobX state tree` 应用程序架构。将此架构与之前的架构进行比较，我们立即注意到一些差异：

*   在组件内部，我们将注入整个树，而不像前面的实现那样只是一个特定的商店。
*   `GalleryModel` 不是在商店内创建的对象，而是由应用程序树组成的。
*   商店之间有一个层次结构，有一个根和多个节点（`PhotoGalleryStore`、`SearchStore` 和 `SelectedPictureStore`），因此我们可以在商店之间双向遍历数据。

## MobX 状态树架构与应用

从某种意义上说，这最后一个特性可能是最需要谨慎处理的，因为如果我们不能以一种智能的方式构建 store 并尊重我们的领域，我们最终可能会得到一个大型应用，其中 store 之间存在大量依赖，从而在不需要的地方引入耦合，并使我们项目的演进或特定设计实现的重构变得复杂。我认为一个有用的建议是，我们应该用多个树来构建大型应用，因为这将帮助我们以更好的方式维护和潜在地重用代码。诀窍始终是按领域划分我们的应用；当我们能够清晰地识别特定领域的边界时，构建状态树就会变得更容易。

在简要介绍了 MobX 状态树架构之后，现在是时候看看我们如何将其应用到我们的图片画廊项目中了，因此我们将分析与之前仅使用自定义架构的 MobX 项目相比的差异。

### App.js 中的新概念

`App.js` 引入了一些与 MobX 状态树库严格相关的新概念：

```javascript
const augmentedStore = types.compose(GalleryModel, Stores);
const PicsGalleryStore = augmentedStore.create({
    picsList: [],
    searchTerm: "",
    selectedPictureURL: ""
}, {config: Config});

@observer
export class App extends React.Component{
    render(){
        return (
            <Provider store={PicsGalleryStore}>
                <MuiThemeProvider>
                    <div>
                        <Form/>
                        <Grid style={style.child}/>
                        <SelectedPicture style={style.child}/>
                    </div>
                </MuiThemeProvider>
            </Provider>)
    }
}

ReactDOM.render(
    <App/>,
    document.getElementById("app")
)
```

应用程序结构保持不变，但在定义组件之前，我们使用了 MobX 状态树提供的 `types` 对象来将主应用模型与不同的 store 进行组合。这将允许我们从应用树的根部访问模型中存在的应用状态，而无需创建额外的嵌套节点。

MobX 状态树提供了组合树的可能性，这是一个非常有趣的特性，主要是因为我们的应用可能会随着时间变得相当大，而使用组合而非继承将使我们足够灵活地进行重构、添加新功能或进行重大更改，而不会遇到太多问题。

另一件需要强调的事情是，在我们的组合树变量 `augmentedStore` 内部，我们定义了应用状态的初始值。这是 MobX 状态树的另一个特性：它强制我们为必填对象设置默认值，例如本例中的 `searchTerm`、`picsList` 和 `selectedPictureURL`。此特性是 MobX 状态树在运行时执行的检查的一部分；如果没有定义这些值，应用程序将触发一个有意义的错误，指出问题的根本原因，这得益于库中存在的类型系统检查。

最后，一旦我们创建了 `augmentedStore` 节点，并传递了第三个参数作为配置对象，这就是 MobX 状态树将对象注入到 store 中的方式；它只是一个简单的依赖注入机制。这种机制非常方便，因为它使我们能够灵活地将多个对象注入到树中，而无需维护单例或实例化不同的对象。在本例中，我们使用依赖注入来注入配置工具，但潜在地我们可以将其用于日志记录对象或我们希望在应用程序中使用的任何其他工具。

### Stores 对象的修改

我们在主应用中看到的更改导致了 `stores` 对象的更改：

```javascript
import { types } from "mobx-state-tree"
import PhotoGalleryStore from './PhotoGalleryStore';
import SearchStore from './SearchStore';
import SelectedPictureStore from './SelectedPictureStore';

const stores = types.model({
    photoGallery: types.optional(PhotoGalleryStore, {}),
    search:types.optional(SearchStore, {}),
    selectedPicture: types.optional(SelectedPictureStore, {})
})

export default stores;
```

我们正在做的是描述一个由多个节点组成的树，其中我们将 store 描述为可选值，并将它们附加到状态树的根部。`types.model` 代表一个对象的形状，我们将在其他 store 中看到我们如何修改它们以适应上一节中描述的 MobX 关键部分。

### GalleryModel 的实现

考虑到 React 组件保持与之前示例相同的结构，我们将重点关注 MobX 状态树如何处理 store 以及我们对模型的自定义添加。我们从 `GalleryModel` 开始：

```javascript
import {types} from 'mobx-state-tree';

const Picture = types.model("Picture", {
    id: types.identifier(),
    title: types.string,
    image: types.string,
    large_image: types.string
})

const model = types.model({
    searchTerm: types.string,
    picsList: types.array(Picture),
    selectedPictureURL: types.string
}).actions(self => ({
    setPicsList(arr){
        self.picsList = arr;
    },
    setSelectedPictureURL(url){
        self.selectedPictureURL = url;
    },
    setSearchTerm(value){
        self.searchTerm = value;
    }
}))

export default model;
```

在这个对象中，我们可以开始看到一个树节点更完整的结构。在本例中，我们定义了应用程序的状态，这将保证每次模型中的值发生变化时都会产生反应：`types.model` 定义之后的第一个对象代表可观察对象，就像我们在之前的 MobX 示例中一样。它们每一个都需要用特定的原始类型或复杂类型进行类型化，例如 `picsList` 的情况，我们定义了 `Picture` 模型，它代表数组中每个单一元素包含的数据结构。这个特性将为我们节省大量的调试时间，因为我们确信模型内部不会出现数据不一致的情况。这正是我们所期望的：否则应用程序将无法工作。这也是为什么一开始我建议您花时间定义应用程序领域的另一个原因；这样做将使您避免以错误的方式构建模型的可能性，从而产生一个实际上有助于解决应用程序内部问题的系统。因此，我们应始终从定义模型及其与主树的交互开始我们的项目，然后专注于应用程序的 UI。

MobX 状态树的另一个约束是我们不能直接更新可观察对象，只能通过操作（actions）来更新。这些边界在大型应用程序中非常有帮助，因为它们提供了方向，而不会出现为实现相同结果而采用不同实现的可能性。因此，我们定义了多个操作，以提供更新应用程序状态的可能性。在操作对象实现内部，我们添加了参数 `self`：它由 MobX 定义，允许我们在不创建手动绑定的情况下保持正确的作用域。`self` 将在我们稍后在本章中声明计算值时可用。

### SearchStore 与计算值

现在我们可以为 MobX 状态树的知识添加另一块内容，在模型中引入计算值，就像我们在 `SearchStore` 中所做的那样：

```javascript
import { types, getParent, getEnv} from "mobx-state-tree";

const searchStore = types.model({
    isSearching: false,
}).views(self => ({
    get url(){
        return getEnv(self).config.getSearchURL(getParent(self).searchTerm);
    }
})).actions(self => ({
    requestPics(){
        self.isSearching = true;
        fetch(self.url)
            .then(self.onData)
            .then(self.onResult.bind(this))
            .catch((err) => console.log(err))
    },
    onData(response){
        return response.json();
    },
    onResult(response){
        const configuration = getEnv(self).config;
        const model = getParent(self);
        self.isSearching = false;
        const picsArr = response.photos.photo.map(value => {
            return {
                id: value.id,
                title: value.title,
                image: configuration.getPicURL(value),
                large_image: configuration.getLargePicURL(value)
            }
        })
        model.setPicsList(picsArr);
        model.setSelectedPictureURL(picsArr[0].large_image);
    },
    changeSearchTerm(term){
        getParent(self).setSearchTerm(term);
    },
}))

export default searchStore;
```

在本例中，我们向模型中添加了另一个名为 `views` 的元素，视图用对象字面量表示，它们是 MobX 状态树模型对象中计算值的转换。视图的行为与计算值完全相同：它们对状态的任何变化都会做出反应。因此，我们应该已经熟悉这个概念了。

现在让我们尝试总结一下 MobX 状态树模型是如何组成的，考虑到我们已经探索了关键部分。

## 概述

### 核心概念

*   模型需要一个描述模型状态的对象（类似于 MobX 中的 observables）。
*   视图需要一个对象，我们在其中指定链接到应用程序状态的计算值，它们对应用程序状态做出反应。
*   动作是 UI 用来与应用程序状态交互的方法。

With this in mind we can move forward analyzing the rest of the stores. In the `SearchStore` there are a couple of other important things to highlight, so let’s check the implementation of the computed value inside the `views` object:

```javascript
get url(){
    return getEnv(self).config.getSearchURL(getParent(self).searchTerm);
}
```

`getEnv` and `getParent` are the other two methods provided by MobX state tree. The first one is used for getting access to the objects injected inside the main tree, in our case the configuration util object used for creating the final URL to use when we consume the Flickr APIs. `getParent`, instead, allows us to retrieve the parent node of the tree, in our case the root that we composed with the main `GalleryModel`; `searchTerm` is an observable described inside the main model.

As we can see, this approach could result in difficult maintenance, particularly when the tree has many nested nodes. That’s why we need to think twice when we work in this way: the secret here is keeping the trees with not many nested levels and to work more encapsulating the components of our application.

The functionalities in the search store are exactly the same as the previous example but expressed in the MobX state tree way. Moving forward we can briefly take a look at the other two stores; the `PhotoGalleryStore` is implemented like this:

```javascript
import { types, getParent } from 'mobx-state-tree'

const photoGalleryStore = types.model("Gallery", {
}).views(self => ({
    get totalPics(){
        return getParent(self).picsList.length;
    }
})).actions(self =>({
    setPictures(arr){
        getParent(self).setPicsList(arr);
    }
}))

export default photoGalleryStore;
```

Once again, we are communicating with the `GalleryModel` via the `getParent` method, considering the main model is composed inside the root of our state tree. For the `SelectedPictureStore`, instead, the implementation is this one:

```javascript
import { types, getParent } from "mobx-state-tree";

const selectedPictureStore = types.model({
}).actions(self => ({
    setPictureURL(url){
        getParent(self).setSelectedPictureURL(url);
    }
}))

export default selectedPictureStore;
```

And also in this case we are just setting the final URL to display and automatically the components will react to the change happening to the `selectedPictureURL` observable in the main model.

Overall, the porting from a MobX project to a state tree one doesn’t request much effort but provides a lot of value due to its opinionated nature of doing things and enforcing rules for a better application structure.

## 总结

In this chapter, we have evaluated how `MobX` can be used in the real world as a reactive state management providing automated and optimized derivations for improving our application performances and providing predictability. We discovered that `MobX` can be used in conjunction with `React`, but there are also several implementations for other virtual DOM libraries like `Vue.js` or `Snabbdom`. Last but not least we have evaluated two examples: one with `MobX` only; and another one with `MobX state tree`, an opinionated library, which provides a tree structure to our application state, mirroring the concepts behind a DOM tree and providing several utilities for composing and decorating our application state. Architecturally speaking, `MobX` provides a level of flexibility that is hard to find with other frameworks, and it’s the perfect companion for moving existing applications to a reactive paradigm improving their predictability and for anyone that wants to embrace the reactive paradigm quickly. Using a `MobX state tree` for new applications or proof of concepts provides a quick and well-designed structure improving our productivity without compromising on quality. And the last important point is that in both examples, we extracted the application state from the stores guaranteeing a better separation of concerns and less coupling between stores that are unaware of the existence of others maintaining their bounded context appropriately.