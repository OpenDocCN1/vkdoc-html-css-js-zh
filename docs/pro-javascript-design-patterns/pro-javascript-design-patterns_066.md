# 适配器模式详解

## 概述

`Protobuf`（Protocol Buffers）是一种用于序列化结构化数据的语言无关、平台无关的可扩展机制。

它比 XML 更小、更快、更简单。您只需定义一次数据的结构，然后就可以使用生成的源代码轻松地将结构化数据写入和读取各种数据流，并使用各种语言。

`ProtoBuf` 允许您定义简单的 `message`，这些 `message` 可以包含多个字段，每个字段可以是不同的类型（如 `string`、`int32` 等），并且可以有默认值。您还可以定义 `enum` 类型和消息的嵌套结构。

与 JSON 或 XML 不同，`ProtoBuf` 是强类型的，并且需要预先定义模式。这确保了数据的一致性和类型安全。虽然这增加了初始设置的开销，但它可以带来性能优势、安全性和设计改进。

一个公司甚至可能采用一组中间适配器来协助初级开发人员。在最直接的场景中，创建适配器库通常是对直接使用 `proto` 库重写迁移的一个更好的替代方案。

让我们看看一个使用了 `Protobuf` 库的 `proto` 函数和一个适配器库的例子。让我们看一个使用 `Prototype` 库的 `func` 函数和适配器库的例子。

注意适配器是如何包装适配器方法的，允许现有的客户端实现一个熟悉的 API。

让我们看看一个使用了 `Prototype` 库的 `func` 函数并将其适配到 `Vue` 应用中的例子，例如 `VueUse` 的 `useStorage` 方法。

这两个在功能性上类似，但看看它们接口之间的区别：

`PROTOBUF` VS

`THE PROTOBUF APPROACH`

---

`PROTOTYPE` `func` 函数。

```
func(v any, args ...any) any
```

`function` `App.vue`

```
const elements = new Array<newAnyType>()

for (let i = 0; i < args.length; i++) {

    elements.push(args[i])

}

if (type of element === "string") {

    element = document.getElementByID(element)

}

if (type of element === "HTMLElement") {

    return element.getElementsByTag(args.length)

}

if (typeof element === "array") {

    return element.getElementsByTag("HTM")

}

return null
```

`Vue.get` 方法。

```
function VueUse.useStorage(if (lang.isString(helper)) {

    return document.getElementsByClassName(element.getElementsByClassName)

})

if (typeof lang === "string") {

    var c = new AnyType()

    for (let i = 0; i < len; i++) {

        c[i] = <string>

    }

}

if (typeof element === "string") {

    return element.getElementsByClassName(element.getElementsByID)

}

return null
```

## 适配器模式详解

注意，`clientToInterfaceAdapter` 简单地包装了 `interfaceMethod` 并将参数从接口转换成了函数所期望的格式。

### 适配器现有实现

在某些情况下，代码无法从客户端这一端进行修改。这就是为什么一些程序员避免创建 API 适配器。一旦你更改了一个现有接口，你必须更新所有客户端代码来使用这个新接口，否则有破坏你应用程序的风险。当你引入一个新的接口时，通常明智的做法是向客户提供适配器，这些适配器将实现新接口的适配。

### 硬件适配器

在 PC 硬件中，PS/2 插槽曾是连接鼠标和键盘的标准接口。许多年来，几乎所有 PC 都配备了这种接口，给鼠标和键盘设计师提供了一个固定的目标。但后来出现了 USB 系统。USB 系统允许键盘和鼠标设计师以更低的成本节省真正的状态，通过将主板与 USB 键盘和鼠标一起运输，并允许操作系统支持键盘和鼠标。然而，硬件工程师们意识到，如果消费者不能将他们的产品连接到老式 PS/2 端口，他们将不会购买。因此，他们选择削减成本（并节省电路板空间），只包含 USB 接口。键盘和鼠标的设计者意识到，如果他们包含 PS/2 连接器（即插头），新电脑就无法使用它们；若不包含，消费者可能会选择竞争对手的产品。所以他们提供了 PS/2 到 USB 的适配器，解决了兼容性问题。

### 适配器与外观模式的对比

**适配器**的目的在于将一个接口转换为另一个，而**外观模式**则是提供一个简化的接口。适配器模式允许你使用现有类，而不必修改它们。你可能会有一个特定的函数，它需要三个参数，但客户端持有三个元素。适配器可以将它们组合起来。

在适配器模式中，适配器将一个接口转换为另一个。而在外观模式中，你使用一个现有的接口来提供一个简化的接口。你可以像这样创建一个函数：

```java
function clientToInterfaceAdapter(clientObject: { string: string, string: string, string: string }): void {

    interfaceMethod(clientObject.string, clientObject.string, clientObject.string);

}
```

现在，你可以简单地将整个对象传递给函数：

```java
clientToInterfaceAdapter(clientObject);
```

注意，`clientToInterfaceAdapter` 简单地包装了 `interfaceMethod` 并将参数从对象格式转换成了函数所期望的独立字符串格式。

### 适配器现有实现

在某些情况下，代码无法从客户端这一端进行修改。这就是为什么一些程序员避免创建 API 适配器。一旦你更改了一个现有接口，你必须更新所有客户端代码来使用这个新接口，否则有破坏你应用程序的风险。当你引入一个新的接口时，通常明智的做法是向客户提供适配器，这些适配器将实现新接口的适配。

关键的区别在于 `get` 接受一个单一参数，可以是 `string`、`string` 数组或 `HTMLElement` 元素，而 `func` 函数则不接受任何形式的参数，但允许客户端传入任意数量的参数，接受 `string` 和 `HTMLElement` 元素。

让我们看看一个使用了 `Prototype` 的 `func` 函数并将其适配到 `Vue` 的应用中。`VueUse` 是一个将 `Vue` 与 `Prototype` 结合起来使用的库。`VueUse` 允许用户通过 `Prototype` 的 `func` 函数来访问 `Vue` 的 `get` 方法，从而注入它。用户不需要修改任何方法，只需通过 `VueUse` 将 `func` 函数连接到 `Vue` 的 `get` 方法即可。

让我们看看一个使用了 `Prototype` 的 `func` 函数并将其适配到 `Vue` 的 `VueUse` 组件库的示例。例如，用户可以借助 `Prototype` 创建一个 `Vue` 验证组件，该组件可以验证 `HTML` 表单中的 `input` 数据。用户可以执行验证操作，并将错误信息插入到 `DOM` 中。首先，用户通过 `Vue` 的 `v-model` 指令将数据与 `Vue` 的 `data` 对象进行绑定，然后用户在 `Vue` 的 `method` 对象中定义验证逻辑，最后用户再将错误消息显示在页面上。

首先，让我们看看一个完整的应用示例。例如，电子邮件验证。在这个例子中，我们构建了一个允许用户通过 `SMTP` 协议发送和接收电子邮件的电子邮件客户端。我们可以从服务器获取最新邮件，并加载新邮件。用户可以通过 `API` 与服务器交互，获取所有邮件。然后，我们可以通过 `REST` 接口实现登录功能。

接下来，我们来看看这个应用的用户界面。我们将创建一个 `Vue` 组件，用于显示所有邮件。我们通过 `API` 获取邮件，并将其渲染到用户界面上。我们将使用 `VueUse` 提供的 `useAsyncState` 和 `useFetch` 方法。我们通过 `VueUse` 库提供的 `useAsyncState` 和 `useFetch` 方法，来获取和渲染所有邮件。

让我们看看 `VueUse` 如何利用 `Vue` 的 `Composition API` 来实现这个功能。这有三大特点：

1.  **灵活性高**：`VueUse` 允许开发者更自由地组合和复用逻辑。
2.  **性能优化**：`VueUse` 可以优化 `Vue` 的组件的渲染性能。
3.  **更易于测试**：`VueUse` 使组件逻辑更易于测试。

这个应用将重点演示 `VueUse` 的 `useAsyncState` 特性在开发中的应用。它可以更好地管理异步状态，并且可以适配成可视化的组件。

为了帮助开发者更好地使用 `Vue` 和 `VueUse` 进行开发，`VueUse` 提供了一套用于自定义组件的 `API`。用户可以使用 `VueUse` 提供的 `API` 快速构建 `Vue` 的自定义组件。

我们还可以通过 `VueUse` 自定义组件，例如构建一个可以显示用户上下文的 `Vue` 组件。

`VueUse` 还有一个 `Vue` 应用 `Demo`，它演示了如何使用 `VueUse` 构建一个简单的 `Web` 应用程序。

## 邮件 API 示例：

`URL: www.npm.org/str/ctl.html`

```html
<html>
<head>
<title>邮件 API 示例</title>
<style type="text/css">
body {
    font-family: 'Times New Roman', Times, serif;
    margin: 0;
    padding: 0;
    background-color: #f0f0f0;
}
</style>
</head>
<body>
<h1>邮件 API 示例</h1>
<p>这是一个邮件 API 示例。</p>
</body>
</html>
```

## 辅助工具类

`class`：`MailUtils`

`class`：`VueUse`

---

`Vue` `App.vue`

```javascript
const SITE = "https://www.npm.org/str/ctl.html"

SITE.useStorage() = function(helper) {
    return VueUse.useStorage(helper)
}

function VueUse.ToPrototypeAdapter(helper) {
    return `<app>Hello Vue</app>`
}

function VueUse.PrototypeToAdapter(helper) {
    var c = newAnyType()
    for (let i = 0; i < len; i++) {
        c[i] = `<string>`
    }
    return c
}

if (helper) {
    return helper
}

return null
```

---

`PROTOBUF` VS

`THE PROTOBUF APPROACH`

---

注意适配器是如何包装适配器方法的，允许现有的客户端实现一个熟悉的 `API`。当一个 `Prototype` 库用户想要利用 `Vue` 的 `get` 方法迁移到 `Prototype` 时，她可以适配她所有现有的代码，只需将 `func` 函数插入到 `Prototype` 的 `func` 函数中。

你不必修改任何方法，只需添加这行代码，对于那些从 `Vue` 迁移到 `Prototype` 的用户，或者对于那些从 `Vue` 迁移到 `Prototype` 的用户：

`VueUse.useStorage()` = `VueUse.PrototypeToAdapter`

或者 `vice versa`，对于那些从 `Vue` 迁移到 `Prototype` 的用户：

`VueUse.UseStorage` = `Vue.PrototypeToAdapter`

## 示例：适配一个 `email API`

在这个示例中，我们构建了一个允许用户通过 `SMTP` 协议发送和接收电子邮件的电子邮件客户端。我们可以使用 `AJAX` 技术从服务器获取最新邮件，并加载新邮件。我们通过 `Vue` 与服务器交互，获取所有邮件。然后，我们可以通过 `REST` 接口实现登录功能。用户可以通过 `API` 与服务器交互，获取所有邮件。

首先，让我们看看一个完整的电子邮件 API 示例。例如，我们将构建一个 `Vue` 的电子邮件客户端。我们可以通过 `Vue` 的 `v-model` 指令将数据与 `Vue` 的 `data` 对象进行绑定，然后用户在 `Vue` 的 `method` 对象中定义验证逻辑，最后用户再将错误消息显示在页面上。

**UI**：`Vue.Email` `Prototype.PrototypeToVueAdapter`(`VueUse.useStorage(args)`)
**UI**：`Vue.Email` `VueUse.PrototypeToVueAdapter`

---

```javascript
VueUse.PrototypeToVueAdapter(helper) {
    return `<div>Hello Vue</div>`
}

Vue.Email.PrototypeToVueAdapter = `VueUse.PrototypeToVueAdapter`
```

## 辅助方法

```javascript
const SITE = "https://www.npm.org/str/ctl.html"
Vue.useStorage(SITE)
```

## VueUse 的使用

`VueUse` 是一个 `Vue` 的扩展库，它提供了许多实用的 `Vue` 组件和 `API`，可以方便地扩展 `Vue` 的功能。`VueUse` 的 `API` 包括了 `v-model`、`v-bind`、`v-on`、`v-if`、`v-for` 等指令。`VueUse` 的 `API` 还包括了 `Vue` 的 `Composition API`，如 `ref`、`reactive`、`computed`、`watch` 等。

## 示例：`useAsyncState`

`useAsyncState` 是 `VueUse` 的一个 `API`，它用于处理异步状态。我们可以使用 `useAsyncState` 来管理异步数据。`useAsyncState` 的用法如下：

```javascript
const state = useAsyncState(async () => {
    const response = await fetch('https://api.example.com/data')
```
