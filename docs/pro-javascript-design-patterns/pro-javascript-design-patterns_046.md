# 使用设计模式创建对象

在 JavaScript 中，你可以使用多种创建对象的方式。每种方式都有其适用场景，具体取决于应用程序的复杂性和你希望使用的访问器（accessors）与方法（methods）。经过多年的演变，JavaScript 的创建对象模式已经趋于成熟，但问题在于，这种成熟导致了一种“两难境地”：你既想要构造函数，又想要访问器方法，并且希望二者能够互相兼容。

在 JavaScript 中，你可以使用函数调用来创建对象。你可以在函数内部使用一个“链”（chain）来产生更少的代码，如果你想要保持一致的结构。你可以在构造函数内部使用 `this` 关键字来引用新创建的对象，并使用 `new` 关键字调用函数。你还可以使用 `return` 语句来返回一个对象。但如果你忘记了 `new` 关键字，你可能会污染全局作用域。

在本章中，我们将探讨一种模式，它能帮助解耦这两种创建对象的方式，并使用一种基于接口（interface）的方法来实例化对象，而不是使用 `new` 关键字。我们将讨论简单的工厂模式（Simple Factory pattern），它使用一个单独的类（class）来根据你的需求创建不同类型的对象。

## 简单的工厂模式

简单的工厂模式最好用一个例子来阐明。假设你想创建一个新的自行车商店（bicycle shop），其中每个店铺提供多种不同型号的自行车以供销售。这可以用一个类似于以下的类来表示：

```javascript
var BicycleShop = class BicycleShop {
  constructor() {
    this.models = {};
  }

  defineModel(name, builder, prototype) {
    this.models[name] = { builder, prototype };
  }

  sellModel(name) {
    var model = this.models[name];
    if (!model) {
      return null;
    }
    var bicycle = new model.builder(model.prototype);
    bicycle.prepare();
    bicycle.wash();
    bicycle.ride();
    return bicycle;
  }
};
```

以下是 `BicycleShop` 类的工作原理：

- `defineModel(name, builder, prototype)`：注册一个名为 `name` 的自行车模型。它接受一个构造器函数 `builder` 和一个原型对象 `prototype`。
- `sellModel(name)`：根据名称 `name` 销售一辆自行车。它会创建自行车实例，并执行 `prepare()`、`wash()` 和 `ride()` 方法，最后返回该自行车。

你可以向 `BicycleShop` 添加新的自行车模型。例如，添加一个名为 `"The Speeder"` 的高速自行车模型：

```javascript
var BicycleShop = class BicycleShop {
  // ... 上述方法 ...
  defineModel("The Speeder", Speedster, Speedster.prototype);
  // ...
};
```

或者添加一个名为 `"The Cruiser"` 的城市自行车模型：

```javascript
BicycleShop.defineModel("The Cruiser", Cruiser, Cruiser.prototype);
```

此外，如果你想销售特定型号的自行车，你需要调用 `sellBicycle` 方法。例如，销售一辆名为 `"The Lowrider"` 的低底盘自行车：

```javascript
var shop = new BicycleShop();
shop.defineModel("The Lowrider", Lowrider, Lowrider.prototype);
```

## 客户端的调用

在客户端，你可以这样使用 `BicycleShop`：

```javascript
var californiaCruisers = new BicycleShop();
californiaCruisers.defineModel("The Speeder", Speedster, Speedster.prototype);
californiaCruisers.defineModel("The Lowrider", Lowrider, Lowrider.prototype);
```

然后，你可以通过调用 `sellModel` 方法并传入模型名称来销售自行车：

```javascript
var yourNewBike = californiaCruisers.sellModel("The Lowrider");
```

## 使用 `BicycleFactory` 命名空间

`BicycleFactory` 是一个单例（singleton），它被用作命名空间（namespace）来容纳 `createBicycle` 方法。该方法返回一个与 `Bicycle` 接口响应一致的对象，该对象可以被组装（assemble）和清洗（wash），就像之前提到的那样。

要销售特定型号的自行车，请调用 `sellBicycle` 方法：

```javascript
BicycleShop.sellBicycle = function(model) {
  var bicycle = BicycleFactory.createBicycle(model);
  bicycle.prepare();
  bicycle.wash();
  bicycle.ride();
  return bicycle;
};
```

## 改进的 `BicycleShop` 类

```javascript
var BicycleShop = class BicycleShop {
  // ... 上述方法 ...
  defineModel(name, builder, prototype) {
    this.models[name] = { builder, prototype };
  }

  sellModel(name) {
    var model = this.models[name];
    if (!model) {
      return null;
    }
    var bicycle = new model.builder(model.prototype);
    bicycle.prepare();
    bicycle.wash();
    bicycle.ride();
    return bicycle;
  }
};
```

`BicycleFactory` 是一个单例，它被用作命名空间来容纳 `createBicycle` 方法。该方法返回一个与 `Bicycle` 接口响应一致的对象，该对象可以被组装和清洗。

## 完整示例：创建并销售自行车

```javascript
var BicycleShop = class BicycleShop {
  constructor() {
    this.models = {};
  }

  defineModel(name, builder, prototype) {
    this.models[name] = { builder, prototype };
  }

  sellModel(name) {
    var model = this.models[name];
    if (!model) {
      return null;
    }
    var bicycle = new model.builder(model.prototype);
    bicycle.prepare();
    bicycle.wash();
    bicycle.ride();
    return bicycle;
  }
};
```

# 总结

简单的工厂模式（`BicycleFactory`）是一个单例，它充当命名空间，包含 `createBicycle` 方法。该方法返回一个与 `Bicycle` 接口一致的对象。这个模式非常适合当你需要创建多种对象，且不希望直接使用 `new` 关键字时使用。它简化了对象的创建过程，并使得添加新类型的对象变得容易。

# 嵌套示例

## `createWidget`

### 概览

`CreateWidget` 是一个简单的工厂模式示例。它接受成员的创建和移动，使其能够移动到外部对象。这个对象可以是一个简单的命名空间，如在该示例中，或是一个类的实例。它常常使用单例模式或静态类方法，使创建方法更有意义。

### 创建成员实例的方法

当创建方法不直接用于创建这些成员实例时，你会使用不同的成员创建方法。这里有两种方法，一种是使用同一个公司提供的构建器（Builder），另一种是从通用产品公司提供的构建器。

#### `SimpleWidget` 工厂类

**`CreateWidget` 方法**：
```
CreateWidget(widget)
```

**方法**：
```
sellWidget(widget)
```

#### `Speedster` 工厂类

**`CreateWidget` 方法**：
```
CreateWidget(new Speedster())
```

**备注**：由于 `CreateWidget` 对象只能使用一个特定工厂，一个简单的 `CreateWidget` 对象本身不是单例，只应被子类化。要创建一个使用特定 `CreateWidget` 工厂的子类，扩展 `CreateWidget` 并覆盖 `createWidget` 方法。

# 内部接口

## `Widget` 接口

`Widget` 接口是一个简单的工厂模式示例，简化了这些成员的实例化，并允许它们被移至外部对象或类。这个对象可以是一个简单的命名空间，如在该示例中，或是一个类的实例。因此，`CreateWidget` 对象不是一个工厂，而是一个简单的创建方法模式，也用于 `createWidget` 方法。

## `GeneralProductsWidget` 工厂类

**`GeneralProductsWidget` 方法**：
```
extensionGeneralProductsWidget, Widget()
```

**方法**：
```
createWidget(sellWidget)
```

**示例**：
```
GeneralProductsWidget.prototype.createWidget = function(widget) {}
```

**备注**：`throw new Error("Unsupported operation on an abstract class.")`。

## 定义 `CreateWidget` 方法

```
CreateWidget = function(widget) {}
```

你定义一个 `CreateWidget` 方法，但它会在实际被调用时抛出错误。

`CreateWidget` 现在是抽象的，不应被实例化，只能子类化。要创建一个使用特定 `CreateWidget` 工厂的子类，扩展 `CreateWidget` 并覆盖 `createWidget` 方法。

## `AcmeWidget` 工厂类

**`AcmeWidget` 方法**：
```
createWidget(model)
```

**方法**：
```
extensionAcmeWidget, Widget()
sellWidget(widget)
```

**示例**：
```
AcmeWidget.prototype.createWidget = function(model) {
    widget = new AcmeWidget(model);
}
break;
default:
    widget = new AcmeWidget("Default Widget");
}
return widget;
```

## `GeneralProductsWidget` 工厂类

**`GeneralProductsWidget` 方法**：
```
extensionGeneralProductsWidget, Widget()
```

**方法**：
```
createWidget(model)
```

**示例**：
```
GeneralProductsWidget.prototype.createWidget = function(model) {
    widget = new GeneralProductsWidget(model);
}
break;
default:
    widget = new GeneralProductsWidget("Default Widget");
}
return widget;
```

所有从这些工厂方法创建的对象都响应于 `Widget` 界面，以便任何使用它们的代码都能正确运行。

`` 是 `` 在 `` 中的 `` 和 `` 的 ``。`` 是 `` 在 `` 中。

``。`` 创建 ``，`` 销售 `` `` 或 ``。

`` `` `` `` `` `` `` ``。

``

`` `` `` `` `` `` ``。`` `` `` ``。

``

`` `` `` `` `` `` `` `` ``。

``

`` `` `` `` `` `` `` ``。`` `` `` ``。

``

`` `` `` `` `` `` `` `` ``。`` `` `` `` `` ``。

`` `` `` `` `` `` `` `` `` `` ``。`` `` `` `` `` `` ``。

`` `` `` `` `` `` `` `` `` `` `` ``。

`` `` `` `` ``。`` `` `` `` `` `` ``。

`` `` `` `` `` `` ``。`` `` `` `` `` `` `` `` ``。

`` `` `` `` `` `` ``。`` `` `` `` `` `` `` ``。

`` ``。`` `` `` `` `` `` `` `` `` `` `` `` ``，`` ``。

`` `` `` `` `` `` `` `` `` `` `` `` ``。`` `` ``

`` `` `` `` `` `` ``，`` `` `` `` `` `` `` `` `` ``。

## 介绍

在企业中，最常见的问题是在 `PIM` 系统内创建 `SKU` 或 `Product` 数据的系统集成项目中，`HTTP` 请求方法（`POST`、`PUT`、`PATCH`）的选择。`POST` 通常用于创建新资源，而 `PUT` 则用于替换整个资源。`PATCH` 请求是在 `RFC` 规范中定义的，用于对现有资源进行部分更新。

## 示例：`HTTP` 请求方法

一个常见的任务是调用 `API` 来创建或更新产品。例如，在 `RFC` 规范中，`PATCH` 方法被设计用于对资源进行部分更新，而不是完全替换。`HTTP` 规范中，`PATCH` 请求体包含一组指令，描述如何修改当前驻留在源服务器上的资源，以生成新的版本。

### 定义 `PATCH` 方法

`PATCH` 方法用于对资源应用部分修改。

`PATCH` 请求体包含一组指令，描述如何修改当前驻留在源服务器上的资源，以生成新的版本。`PATCH` 方法会影响由 `Request-URI` 标识的资源，并且它也可能导致创建新的资源。

另一个常见的任务是调用 `API` 来创建资源。`POST` 方法用于向源服务器提交一个实体，该实体通常会改变源服务器的状态或产生副作用。一个典型的例子是使用 `POST` 方法向服务器提交一个表单。

### 示例：`SimpleHandler` 类

`SimpleHandler` 类在处理 `PATCH` 请求时表现出色。在实例化它之后，您可以使用 `request` 方法执行异步请求。

```
```

# SimpleHandler 类与 PATCH 请求

## 定义 `SimpleHandler` 类

```javascript
var myHandler = new SimpleHandler();

var callBack = function(responseText) { alert(responseText); };

myHandler.request("GET", "example.txt", callBack);
```

```javascript
var SimpleHandler = function() {};

SimpleHandler.prototype = new function() {

    request: function(method, url, callBack, postVars) {

        var xhr = this.createXhrObject();

        xhr.onreadystatechange = function() {

            if (xhr.readyState !== 4) return;

            (xhr.status === 200) ? callBack.success(xhr.responseText, xhr.responseXML) : callBack.failure(xhr.status);

        };

        xhr.open(method, url, true);

        if (method !== "POST") postVars = null;

        xhr.send(postVars);

    },

    createXhrObject: function() {

        var methods = [

            function() { return new XMLHttpRequest(); },

            function() { return new ActiveXObject("Msxml2.XMLHTTP"); },

            function() { return new ActiveXObject("Microsoft.XMLHTTP"); }

        ];

        for (var i = 0, len = methods.length; i < len; i++) {

            try {

                methods[i]();

            } catch (e) {

                continue;

            }

            this.createXhrObject = methods[i];

            return methods[i]();

        }

        throw new Error("SimpleHandler: Could not create an XHR object.");

    }

};
```

### 使用 `PATCH` 方法

```javascript
createXhrObject: function() { ... }
```

在上面的例子中，`createXhrObject` 方法尝试创建 `XMLHttpRequest` 对象。它遍历一个方法数组，这些方法尝试不同的方式来创建对象。如果某个方法失败，它会继续尝试下一个。如果所有方法都失败了，它会抛出一个错误，说明无法创建 `XHR` 对象。

### 使用 `SimpleHandler` 类发送 `PATCH` 请求

```javascript
var myHandler = new SimpleHandler();

var callBack = {

    success: function(responseText) { alert("Success: " + responseText); },

    failure: function(statusCode) { alert("Failure: " + statusCode); }

};

myHandler.request("PATCH", "/api/resource/1", callBack, JSON.stringify({name: "New Name"}));
```

`SimpleHandler` 类也可以用于创建 `PUT` 或 `DELETE` 请求。它提供了一种简洁的方式来执行 `HTTP` 请求。

## `PATCH` 与 `PUT` 的区别

`PATCH` 方法用于部分更新，而 `PUT` 方法用于完全替换资源。假设您有一个用户资源，包含 `name`、`email` 和 `age` 字段。如果您只想更新用户的 `email` 地址，使用 `PATCH` 请求是合适的。而 `PUT` 请求则需要发送整个用户对象的所有字段。

## 连接池

`PATCH` 请求也可以受益于连接池。连接池可以减少建立和关闭连接的开销，从而提高性能。`HttpClient` 连接池可以帮助您更有效地管理连接。

## 结论

`PATCH` 方法提供了一种灵活的方式来更新资源，它允许您只发送需要更改的字段，而不是整个资源。这使得 `API` 调用更加高效，尤其是在处理大型资源时。

### 专业化连接对象

您可以更进一步，使用工厂模式在多个地方创建 `PATCH` 请求。例如，您可以创建一个专门的 `PATCH` 连接对象，用于处理 `PATCH` 请求的连接。

## 高级主题：`PATCH` 请求处理

### `PATCH` 请求处理

`PATCH` 请求方法会执行发送请求和处理响应所需的步骤。它创建一个 `HTTP` 对象，配置该对象，然后发送请求。这个 `createXhrObject` 方法会返回一个基于当前环境可用的 `XMLHttpRequest` 对象。在第一次运行时，它会测试三种不同的创建 `HTTP` 对象的方式，一旦找到可行的方法，它就会覆盖自身并返回创建的对象。

这种新技术称为 `memoizing`，它可以用于创建那些存储复杂计算结果，从而无需重复计算的函数和方法。所有复杂的设置代码仅在首次执行，之后仅执行特定的代码。

使用 `SimpleHandler` 类发送 `PATCH` 请求是很直观的。在实例化它之后，您可以使用 `request` 方法来执行异步 `PATCH` 请求：

```javascript
var myHandler = new SimpleHandler();

var callBack = {

    success: function(responseText) { alert("Success: " + responseText); },

    failure: function(statusCode) { alert("Failure: " + statusCode); }

};

myHandler.request("PATCH", "/api/resource/1", callBack, JSON.stringify({name: "New Name"}));
```

### 专用连接对象

您可以更进一步，使用工厂模式在多个地方创建 `PATCH` 请求。例如，您可以创建一个专门的 `PATCH` 连接对象，用于处理 `PATCH` 请求的连接。

```javascript
var myPatchHandler = new SimpleHandler();

myPatchHandler.request("PATCH", "/api/resource/1", callBack, JSON.stringify({name: "New Name"}));
```

# Session管理器概述

`PATCH` 方法提供了一种灵活的方式来更新资源，它允许您只发送需要更改的字段，而不是整个资源。这使得 `API` 调用更加高效，尤其是在处理大型资源时。`Factory` 方法可以使您的代码更加高效，因为所有的设置和测试代码只执行一次。`Factory` 方法是封装这种代码的理想方式，因为您可以调用它们，并知道无论代码在哪个平台上运行，都将返回正确的对象。所有的复杂性都集中在一个地方。

进入 `/path.to/xxx` 目录，找到 `xxx.json`。

## Session管理器核心

使用 `Session` 管理器，您可以通过简单的 `API` 来创建、管理、存储和检索 HTTP 会话。`Manager` 是会话的核心：
- `SimpleSession` 类：管理所有会话的创建、存储、检索和销毁。
- `AppendableSession` 类：存储所有请求，直到用户存储它们。
- `CachedSession` 类：缓存所有响应，如果用户离线，则返回缓存的响应。

## Manager类

`Manager` 类：
- `Manager` 是所有会话的工厂和存储库。它会在允许新会话之前检查是否已有其他请求在进行中。
- `Manager` 的 `storedRequests` 属性存储所有已存储的请求。它在内部使用 `SimpleSession` 的 `request` 方法来确保用户在线。
- 如果用户不在线，`Manager` 的 `storedRequests` 将 `push` 请求，直到用户再次上线。

## SimpleSession类

`SimpleSession` 类：
- `SimpleSession` 是所有登录会话的基础类。它提供了直接发送网络请求的方法：`HTTP`、`URL`、`callback`、`POST` 等。
- `SimpleSession` 的 `prototype` 存储了 `request` 方法（method），它首先检查用户在线状态。如果用户在线，`request` 直接发送请求并返回。
- `SimpleSession` 的 `onerror` 属性保存了错误处理函数（callback）。如果发送请求失败，它会调用这个函数。
- `SimpleSession` 的 `ontimeout` 属性保存了超时处理函数（callback）。如果请求超时，它会调用这个函数。

`SimpleSession` 的 `request` 方法如下：
```javascript
if (this.manager.isOnline) {
    // 请求成功
} else {
    // 请求失败
}
```

## CachedSession类

`CachedSession` 类：
- `CachedSession` 扩展了 `SimpleSession` 类。它修改了 `request` 方法：在发送请求之前，检查是否有已存储的请求。如果有，它创建一个新的 `SimpleSession` 实例来发送这些请求，然后继续处理新请求。
- `CachedSession` 的 `pushStoredRequests` 函数：将所有已存储的请求推送到新的 `SimpleSession` 中。
- `CachedSession` 的 `flushStoredRequests` 方法：清空所有已存储的请求。`setTimeout(callback, 2000)`：设置一个两秒的定时器来检查是否有新的请求，如果没有，则调用 `flushStoredRequests`。

`CachedSession` 的 `request` 方法返回所有已存储的请求。如果 `CachedSession` 的 `manager.isOnline`，则发送请求；否则，将所有请求推送到 `storedRequests` 中。

`CachedSession` 的 `prototype` 的 `request` 方法：`for (var i = 0; i < storedRequests.length; i++)`，遍历所有已存储的请求。如果 `storedRequests[i]` 存在，则发送请求并返回。

## AppendableSession类

`AppendableSession` 类：
- `AppendableSession` 扩展了 `SimpleSession` 类。它覆盖了 `request` 方法：在发送请求之前，总是先创建一个新的 `SimpleSession` 实例。
- 如果新的 `SimpleSession` 发送请求失败，则返回原始 `SimpleSession` 的 `request` 方法。

`AppendableSession` 的 `request` 方法：如果 `this.manager.isOnline`，则发送请求；否则，将请求推送到 `storedRequests` 中。

`AppendableSession`（续）：如果当前有等待中的请求，则将所有请求发送到新的 `SimpleSession` 中，包括 `HTTP`、`URL`、`callback`、`POST` 等。然后，使用 `SimpleSession` 的 `prototype` 来发送所有请求。

`AppendableSession` 的 `request` 方法：如果当前没有等待中的请求，则直接发送请求并返回。

`AppendableSession` 的 `request` 方法：`return this`。

## Manager的request方法

`Manager` 的 `request` 方法如下：
```javascript
manager.request(method, url, callback, POST)
```
- `for (var i = 0; i < storedRequests.length; i++)`：遍历所有已存储的请求。
- `if (manager.isOnline)`：检查用户是否在线。
- `if (manager.storedRequests == 200)`：检查是否有已存储的请求。

## 回调与原型方法

`SimpleSession` 的 `onerror` 回调函数：在请求失败时调用 `SimpleSession` 的 `errorHandler` 函数。

`CachedSession` 的 `ontimeout` 回调函数：在请求超时时调用 `SimpleSession` 的 `timeoutHandler` 函数。

`Session` 类：`Session` 的 `request` 方法类似于 `SimpleSession` 的 `request` 方法。它首先检查用户是否在线：如果用户在线，则直接发送请求并返回；否则，调用 `Session` 的 `request` 对象的 `request` 方法，以 `POST` 方式发送请求。

`SimpleSession` 的 `prototype` 发送请求，直到所有已存储的请求都被发送。同样地，`CachedSession` 的 `prototype` 也发送所有已存储的请求。

`SimpleSession` 的 `request` 方法返回一个新创建的 `SimpleSession` 实例。`SimpleSession` 的 `prototype` 发送所有已存储的请求，直到所有请求都被发送。

`SimpleSession` 的 `prototype` 的 `request` 方法返回 `this`。如果 `this.request` 存在，则发送请求并返回；否则，返回 `false`。

`SimpleSession` 的 `request` 方法返回所有已存储的请求。如果 `storedRequests` 为空，则返回 `null`。

```javascript
return http
```
