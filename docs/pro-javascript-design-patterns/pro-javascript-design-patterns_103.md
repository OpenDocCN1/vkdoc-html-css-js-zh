# 发布者与订阅者模式

在发布者与订阅者模式中，发布者（`Publisher`）负责发布事件。当事件发生时，它会通知所有已订阅的观察者。

## 观察者模式与发布者-订阅者模式

在观察者模式（`Observer Pattern`）中，观察者（`Observer`）会直接订阅主题（`Subject`）。当主题状态发生变化时，它会直接通知所有观察者。这是一种松耦合的关系，但观察者需要知道主题的存在。

## 发布者-订阅者模式

相比之下，发布者-订阅者模式（`Publisher-Subscriber Pattern`）引入了一个消息代理（`Message Broker`）或事件通道（`Event Channel`），作为发布者与订阅者之间的中间层。发布者不直接知道订阅者的存在——它们只负责发布消息。同样地，订阅者也不直接知道发布者的存在。订阅者会表达对特定类型消息的兴趣，然后接收这些消息。在发布者-订阅者模式中，发布者与订阅者之间是高度解耦的。发布者只需将消息发送到消息代理，而订阅者则从消息代理接收消息。

## 核心区别

在观察者模式中，观察者直接订阅主题。在发布者-订阅者模式中，订阅者通过一个中间层接收消息。因此，发布者-订阅者模式通常比观察者模式更灵活，因为它允许发布者与订阅者之间进行更高级的抽象。发布者与订阅者可以彼此完全独立，并独立地改变行为。

## 理解发布者与订阅者

发布者（`Publisher`）负责发布事件。它们定义何时以及如何发布消息。订阅者（`Subscriber`）则定义它们对哪些消息感兴趣，并订阅这些消息。当一个事件发生时，发布者会通知所有订阅者。订阅者随后根据收到的消息执行相应的操作。

## 观察者模式示例

### 在 `JavaScript` 中实现观察者模式

- 主题（`Subject`）维护一个观察者（`Observer`）列表，并通知它们状态变化。
- 观察者（`Observer`）定义当接收到通知时执行的动作。

## 功能与机制

发布者-订阅者模式提供了一系列实现发布者-订阅者模式的方式。但在我们展示这些示例之前，让我们先确保所有关键角色（如主题（`Subject`）、观察者（`Observer`）和它们的方法）都已就绪。

## 核心接口

- 订阅者（`Subscriber`）可以订阅（`subscribe`）和取消订阅（`unsubscribe`）。它们接收通知。
- 发布者（`Publisher`）发布事件。它们拥有通知或是否被通知的选项。
- 主题（`Subject`）管理观察者（`Observer`）列表，添加或移除它们。

## 潜在问题

### 内存泄漏

如果订阅者没有正确取消订阅，并且发布者仍然持有对它们的引用，就可能导致内存泄漏。这通常发生在单页应用（`SPA`）中。

### 调试困难

发布者-订阅者模式使得跟踪事件流变得困难。在传统的事件驱动编程中，你可以看到哪个观察者订阅了哪个主题。但在发布者-订阅者模式中，由于中间层的存在，追踪事件流变得更加复杂。

### 性能问题

发布者-订阅者模式中的消息代理可能会成为性能瓶颈。当大量订阅者订阅同一主题时，消息代理可能需要处理大量消息。

## 高级技巧

### 使用命名空间

在大型应用中，你可能希望使用命名空间来避免事件名冲突。例如，`Publisher.subscriberCustomer.OrderPlaced`。

### 使用通配符

一些发布者-订阅者模式实现支持通配符订阅，允许订阅者订阅多个相关主题。例如，`Publisher.subscriberCustomer.*` 将匹配所有以 `Customer.` 开头的事件。

### 实际应用场景

- **事件驱动架构**：发布者-订阅者模式是事件驱动架构的核心。它允许系统组件之间进行解耦通信。
- **微服务通信**：在微服务架构中，发布者-订阅者模式常用于服务间通信，通过消息队列（`Message Queue`）或事件总线（`Event Bus`）实现。
- **用户界面（`UI`）事件处理**：在前端开发中，发布者-订阅者模式常用于处理用户界面事件，例如点击（`click`）、输入（`input`）等。

## 总结

发布者-订阅者模式是一种强大的设计模式，它允许系统组件之间进行解耦通信。通过引入消息代理，发布者与订阅者可以彼此完全独立。然而，这种模式也引入了一些挑战，例如调试困难、内存泄漏和性能问题。通过正确的实现和最佳实践，发布者-订阅者模式可以显著提高系统的灵活性和可扩展性。

## 进阶阅读

- `Observer` vs `Publisher-Subscriber` Pattern
- `Publish-Subscribe` vs `Observer` Pattern

## 在 `JavaScript` 中实现

在 `JavaScript` 中，你可以通过多种方式实现发布者-订阅者模式。以下是一个简单的实现示例：

```javascript
```

# 发布者-订阅者模式

```javascript
class Publisher {
  constructor() {
    this.subscribers = [];
  }

  subscribe(subscriber) {
    this.subscribers.push(subscriber);
  }

  unsubscribe(subscriber) {
    this.subscribers = this.subscribers.filter(s => s !== subscriber);
  }

  publish(data) {
    this.subscribers.forEach(subscriber => subscriber(data));
  }
}

// 使用示例
const publisher = new Publisher();
const subscriber1 = data => console.log('订阅者 1 收到:', data);
const subscriber2 = data => console.log('订阅者 2 收到:', data);
publisher.subscribe(subscriber1);
publisher.subscribe(subscriber2);
publisher.publish('Hello, world!');
```

在这个模型中，你可以看到发布者（`Publisher`）显然负责发布（`publish`）事件。它们对客户（`customer`）进行收费（`charge`），并且它们也有能力从交付（`delivery`）路线中删除它们。最后，顾客（`customer`）也拥有订阅（`subscribe`）和取消订阅（`unsubscribe`）的能力。

## 下一步

在下一部分中，我们将展示发布者-订阅者模式与观察者模式的具体示例。

```javascript
var Publisher = new Observable();
var Subscriber = function(data) {
    console.log('订阅者收到数据:', data);
};
Publisher.subscribe(Subscriber);
Publisher.publish('数据到达');
```

## 参考文献

- `https://en.wikipedia.org/wiki/Publish%E2%80%93subscribe_pattern`
- `https://www.oodesign.com/observer-pattern.html`
- `https://www.dofactory.com/javascript/design-patterns/observer`

---

## 附录

### 表格：发布者-订阅者模式 vs 观察者模式

| 特性 | 发布者-订阅者模式 | 观察者模式 |
|------|-------------------|------------|
| 耦合度 | 高度解耦 | 松耦合 |
| 中间层 | 有（消息代理） | 无 |
| 通信方式 | 间接通信 | 直接通信 |
| 扩展性 | 高 | 中等 |
| 调试难度 | 高 | 低 |

进入`/path.to.xxx`目录，找到`xxx.json`。

在表达式`cvar = avar + bvar`中，加法运算符（`+`）将`avar`与`bvar`相加，得到它们的和`cvar`。

在`List.of(arg0, arg1, arg2)`中，`List`接口的工厂方法`of()`接受一系列的元素，返回包含它们的只读列表。

之后我们这样调用`cmd`命令：`cmd arg0 arg1 arg2`。

```javascript
if (condVar > someVal) {console.log("xxx")}
```

在`/path.to.xxx`目录中，需要修改`xxx.json`文件，将`publisher`字段的值从`NewsPaper`改为`Magazine`，并将`deliver`字段更新为`Web`。然后重启应用。

对于`Web`订阅者，与`NewsPaper`和`Magazine`订阅者不同，`Web`订阅者有一个名为`deliver`的方法，它只接受一个`Publisher`对象，而不像其他订阅者那样需要`event`参数。

`Web`订阅者上的`deliver`方法假设`publication`的`publisher`与您为其配置的`Publisher`对象一致，并且它通过调用系统的`Mail`服务发送数据。

`Publisher`对象包含一个名为`NewsPaper`的`Publisher`对象，您可以通过调用`deliver`方法来发送数据。另外，`Publisher`类还包含一个方法`setSubscribers`，它接受一个订阅者数组，并将它们添加到内部的订阅者列表。

此外，`Subscriber`接口有两个抽象方法：`deliver`和`subscribe`。

`deliver`方法将`publication`作为参数，并返回一个布尔值，指示交付是否成功。

```javascript
deliver(publication)
```

`subscribe`方法接受一个`publisher`参数，并将订阅者注册到该发布者。

在`Publisher`类中，`setSubscribers`方法处理发布者与订阅者之间的链接。

发布者调用`deliver`方法时，需要遍历其订阅者列表，并为每个订阅者调用其`deliver`方法，传入相应的出版物对象。

在代码中，`publisher.setSubscribers(subscribers)`会更新发布者的订阅者列表。如果需要，您还可以通过`unsubscribe`方法从发布者中移除订阅者。

构建微服务时，可以选择使用`REST`、`gRPC`或`消息队列`。

`REST`是首选，因为它简单且易于理解。

如果使用`gRPC`，则需要定义`.proto`文件并生成客户端和服务端代码。

使用`消息队列`时，您可以使用`RabbitMQ`或`Kafka`。

在本文中，我们将使用`REST`和`JSON`。

---

## REST基础

*   `REST`代表`表述性状态转移`。
*   它使用标准的`HTTP`方法，如`GET`、`POST`、`PUT`、`DELETE`。

### 资源

*   `REST`中的核心概念是资源。
*   每个资源都有一个唯一的`URI`。
*   客户端通过`HTTP`方法对资源进行操作。

## 示例

*   `GET /users`：获取所有用户。
*   `POST /users`：创建新用户。

---

## JSON

*   `JSON`代表`JavaScript 对象表示法`。
*   它是一种轻量级的数据交换格式。
*   `JSON`易于阅读和编写。

## 示例

```json
{
  "name": "John",
  "age": 30,
  "city": "New York"
}
```

---

## 构建RESTful API

*   选择框架：`Express.js`（Node.js）、`Spring Boot`（Java）、`Flask`（Python）等。
*   定义端点：`/api/users`、`/api/products`。
*   处理请求：使用`HTTP`方法（`GET`、`POST`、`PUT`、`DELETE`）。
*   返回响应：通常使用`JSON`格式。

---

## 清理无用代码

在`Publisher`类中，`setSubscribers`方法接收一个订阅者数组。需要确保调用该方法时，传入的数组不为空，并且每个订阅者都实现了`Subscriber`接口。

此外，`Publisher`类的`deliver`方法负责遍历所有订阅者并调用他们的`deliver`方法。如果某个订阅者不再需要接收通知，可以通过`publisher.unsubscribe(subscriber)`方法将其移除。

---

## POST / PUT / PATCH

*   `POST`：用于创建新资源。
*   `PUT`：用于更新现有资源（整体替换）。
*   `PATCH`：用于部分更新现有资源。

## 示例

```http
POST /users
{
  "name": "Alice",
  "email": "alice@example.com"
}
```

---

## 错误处理

*   使用适当的`HTTP`状态码。
    *   `200 OK`
    *   `201 Created`
    *   `400 Bad Request`
    *   `404 Not Found`
    *   `500 Internal Server Error`
*   返回有意义的错误消息。

---

## 身份验证和授权

*   `身份验证`：验证用户身份。
    *   方法：`基本认证`、`令牌认证`（`JWT`）、`OAuth`。
*   `授权`：确定用户可以访问哪些资源。
    *   通常与身份验证结合使用。

---

## 测试`API`

*   使用工具如`Postman`、`curl`或`Insomnia`。
*   编写单元测试和集成测试。

---

## 下一步

*   学习`GraphQL`。
*   探索微服务架构。
*   了解容器化（`Docker`）。

---

## 关于`REST`的更多信息

*   `无状态`：每个请求包含所有必要的信息，服务器不存储客户端状态。
*   `可缓存`：响应可以被客户端或中间缓存。

---

## 总结

*   `RESTful API` 使用标准的`HTTP`方法和状态码。
*   `JSON`是常用的数据格式。
*   设计清晰的`URI`和资源模型。

---

### 订阅与发布模式

在 `pubSubObservable` 中，`pubSub` 用于 `functionPublisher()`。

`pubSub` 的 `PubSub` 模式 `subscribe()` 用于 `functionPublisher()` 和 `Observable` 模式。

`functionPublisher` 用于事件监听。

`if` ( `eventName` ) { `subscribe()` }

`var` `observer` = `Observable.create( new Emitter() )`。

`emitter` `publish` ( `event` ) 。

`if` ( `observer` ) { `emitter.publish(event)` }

`return` `observer` 。

`else`

`return` `this` 。

### 可观察对象 (Observable)

你可能已经注意到，在函数式编程中，`Observable` 模式本质上是一个只用于事件的 `pubSub`。

实际上，`Observable` 模式只是对 `pubSub` 模式的升级，它保证所有订阅者都能收到事件。

`functionPublisher` 的 `constructor` 返回 `publish.subscribe(eventName, event)`。

在 `subscribe` 和 `publish` 中，我们使用 `observers` 数组来管理订阅者。

`var` `observers` = `[]`。

`for` ( `observer` in `observers` ) { }

在 `Observable` 模式中，`return` 的 `observer` 可以调用 `unsubscribe` 方法。

因此，`publish` 会返回一个 `unsubscribe` 函数，以便之后取消订阅。

### 订阅者 (Subscriber)

订阅者在现实世界中无处不在，它们广泛应用于大型代码库，尤其是在多层架构中，多个作者协作开发时。

订阅者模式允许你灵活地在不同组件之间通信，并且你可以随时添加或移除观察者。

应用中的“有趣时刻”包括点击、加载、触摸、鼠标悬停等。一些有趣时刻可能由丰富的 UI 应用触发，例如拖拽、下拉、滑动、完成或切换。所有这些抽象动作最终都会由浏览器事件发布到各自对应的监听器。

### 示例：动画

动画是实现 `Observable` 对象的一个极好的起点。你至少需要三个关键点来启动、完成和进行动画。在本例中，我们将它们分别称为 `onStart`、`onComplete` 和 `onBetween`。请参考以下代码，了解如何使用之前编写的 `Publisher` 工具来实现这一点。

```javascript
// Publisher API
var onAnimation = function(event) {
    this.onStart = new Publisher;
    this.onComplete = new Publisher;
    this.onBetween = new Publisher;
}

// Animation method
onAnimation.prototype.subscribe = function(subscriber) {
    // ... implementation
}

var Supervisor = new Animation;
// ... configuration properties

// Begin implementing subscribers
var putOnCap = function(event) {
    // ... subscriber logic
}

var takeOffCap = function(event) {
    // ... subscriber logic
}

// Since the publisher allows you to listen for when Supervisor is about to take off and land back on the ground, all you have to do is subscribe to these moments.
Supervisor.onStart.subscribe(putOnCap);
Supervisor.onComplete.subscribe(takeOffCap);
```

# 动画生命周期

`var animation = function(event) { }`

`for (observer in observers) { }`

`begin animation`

`this.onStart.deliver(event)`。

`for (var frame in frames) { }`

`this.onBetween.deliver(frameNumber)`。

`end`

`this.onComplete.deliver(event)`。

`set up an account with the animation manager`

`var Supervisor = new Animation`。

`// config properties`

`// begin implementing subscribers`

`var putOnCap = function(event) { }`。`// ... subscriber logic`

`var takeOffCap = function(event) { }`。`// ... subscriber logic`

`// since the publisher allows you to listen for when Supervisor is about`

`to take off and land back on the ground, all you have to do is`

`subscribe to these moments.`

`Supervisor.onStart.subscribe(putOnCap);`

`Supervisor.onComplete.subscribe(takeOffCap);`

如你所见，它工作得非常完美。如果你是一个发布/订阅模式的实现者，`putOnCap` 和 `takeOffCap` 会被发布者调用。由于发布者允许你监听 `Supervisor` 即将起飞和着陆的时刻，你只需要订阅这些时刻即可。

# 标题

事件与监听器

## 核心概念

在事件驱动架构中，事件（`Event`）与监听器（`Listener`）是核心的两种元素。

事件是发生的事件，而监听器则是等待事件发生并处理它的程序。在事件驱动架构中，事件通常是一个包含状态信息的对象，而监听器则是注册到事件上的回调函数。

在事件模型（`Event Model`）中，事件与监听器之间的区别在于，事件表示“发生了什么”，而监听器则负责“响应什么”。

在事件模型中，监听器是被动等待事件发生的程序，而事件则是沿着事件总线（`Event Bus`）传递的消息。在事件驱动架构中，事件与监听器是绑定的，每个事件可以被分配给多个监听器，监听器之间通过事件进行通信。

在传统的回调模式（`Callback Pattern`）中，一个事件只能有一个监听器，而监听器可以独立处理其他事件。

在观察者模式（`Observer Pattern`）中，一个事件可以有多个监听器，监听器可以互相独立地处理事件。

当事件被触发时，它会被推送给所有注册的监听器。监听器通过事件对象（`Event Object`）获取事件的发生信息，比如事件类型、事件源等。

### 事件与监听器的区别

- 事件（`Event`）是“发生了什么”的表示。
- 监听器（`Listener`）是“响应什么”的接口。

## 事件的分类

事件可以分为两类：

1. 同步事件（`Synchronous Event`）：事件被触发后，监听器同步处理事件，直到处理完成。
2. 异步事件（`Asynchronous Event`）：事件被触发后，监听器异步处理事件，事件发布者不会被阻塞。

在事件模型（`Event Model`）中，事件与监听器是解耦的，事件源只负责发布事件，而监听器负责处理事件。

## 监听器的注册

监听器通过事件总线（`Event Bus`）注册到事件上。事件总线是一个全局的事件调度器，它负责将事件分发给注册的监听器。

### 示例代码

### 示例 1：使用监听器

```java
var element = document.getElementById("example");
element.addEventListener("click", function() {
    console.log("Element clicked!");
});
```

### 示例 2：使用事件对象

```java
element.addEventListener("click", function(event) {
    console.log("Event type: " + event.type);
});
```

### 示例 3：事件冒泡

```java
element.addEventListener("click", function(event) {
    event.stopPropagation();
    console.log("Event propagation stopped.");
});
```

## 监听器模式的优势

监听器模式是一种非常好的方式，用来解耦应用程序的各个部分。在大型应用中，监听器模式可以降低代码的耦合度，提高代码的可维护性和可扩展性。

在监听器模式中，监听器是独立于事件源的，事件源只需要发布事件，而监听器则处理事件。这样一来，如果事件源发生变化，监听器不需要修改；反之亦然。

## 监听器模式的缺点

1. 监听器模式增加了系统的复杂性
2. 监听器之间的依赖关系可能变得复杂
3. 监听器模式可能导致内存泄漏，如果监听器没有被正确移除

## 事件处理的几种方式

1. 同步处理：事件被触发后，监听器同步处理事件，直到处理完成。
2. 异步处理：事件被触发后，监听器异步处理事件，事件发布者不会被阻塞。
3. 事件冒泡：事件从内层元素向外层元素传播
4. 事件捕获：事件从外层元素向内层元素传播

## 事件监听器的生命周期

1. 注册：事件监听器注册到事件源
2. 触发：事件被触发后，监听器被调用
3. 处理：监听器处理事件
4. 移除：事件监听器从事件源移除

## 事件对象的属性和方法

事件对象包含以下常用属性和方法：

- `event.type`：事件类型
- `event.target`：事件目标
- `event.preventDefault()`：阻止默认行为
- `event.stopPropagation()`：阻止事件冒泡
- `event.stopImmediatePropagation()`：阻止事件冒泡并阻止其他监听器执行

## 事件驱动的应用场景

事件驱动架构适用于以下场景：

1. 用户界面交互（如点击、滚动、拖拽等）
2. 异步通信（如 AJAX 请求、WebSocket 等）
3. 系统通知（如文件写入、进程退出等）
4. 插件体系（如 WordPress 的钩子系统）

## 总结

通过事件驱动模型，应用程序可以实现模块间的解耦，提高代码的可维护性和可扩展性。事件总线（`Event Bus`）作为事件的核心调度器，负责将事件分发给注册的监听器。在大型应用中，正确使用事件驱动模型可以显著降低代码的耦合度，提高系统的灵活性。

## 何时使用观察者模式

当你想将人类行为与应用程序行为抽象分离时，就应该使用观察者模式。最好不要实现与用户交互紧密相关且源自浏览器的东西，例如基本的 DOM 事件，如点击（click）、鼠标悬停（mouseover）或按键（keypress）。

使用事件处理器（Event Handlers）无法实现这些事件，如下所示：

```java
// 使用监听器的示例
var element = document.getElementById("example");
element.addEventListener("click", function() {
    console.log("Element clicked!");
});

// 使用事件对象的示例
element.addEventListener("click", function(event) {
    console.log("Event type: " + event.type);
});

// 使用事件传播的示例
element.addEventListener("click", function(event) {
    event.stopPropagation();
    console.log("Event propagation stopped.");
});
```

在第一个使用监听器的示例中，当点击事件被分发时，`funcA` 和 `funcB` 都会被触发。但在第二个使用处理器的示例中，`funcA` 被 `funcB` 替代，因此 `funcA` 永远不会被调用，因为 `onclick` 属性被重新赋值，改由 `funcB` 来处理。

尽管如此，你可以看到监听器（`listeners`）和观察者（`observers`）之间存在相似之处。实际上，它们是同义词。两者都订阅特定的某个事件，等待该事件的发生。当事件发生时，它会通知订阅者的回调函数，通过事件对象传递有价值的信息，例如事件发生的时间、事件类型或最初分发事件的源目标。

## 观察者模式的优势

观察者模式基本上是在大型架构中维护基于操作型应用程序的好方法。在任何给定的应用程序中，在浏览器会话期间，可能零散地发生数十、数百甚至数千个事件。此外，你可以减少事件附加的数量，让可观察对象通过一个事件监听器为你处理所有操作，并将信息委派给它的所有订阅者，从而减少内存占用并提升交互性能。这样，你就不用不断地添加事件监听器了。
