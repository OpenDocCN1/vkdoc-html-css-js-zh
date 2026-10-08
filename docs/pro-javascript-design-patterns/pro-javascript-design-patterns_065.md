# Facade 模式 (Facade Pattern)

Facade 模式（外观模式）为复杂子系统提供了一个简单统一的接口。它封装了子系统的复杂性，对外提供一个更易用的高层接口。

### 示例：在 UI 框架中，Facade 模式常用于简化复杂操作

对开发者来说，Facade 模式隐藏了底层的实现细节，让你只需要调用一个简单的函数就能完成复杂的任务。

# Facade 与 Prop Drilling

`Props`与`State`是构建现代 Web 应用的事实标准。

设置元素的类名是通过`className`属性完成的。其核心在于通过字符串形式的类名来匹配 CSS 样式规则。

Facade 模式是开发者常用的设计模式之一，它通过提供一个统一的接口来简化复杂系统的使用。其核心目的是减少耦合、提高易用性。

一个典型的 Facade 通常包含以下几个部分：

*   `event.stopPropagation()`：阻止事件冒泡。
*   `event.preventDefault()`：阻止事件默认行为。

第一个方法`stopPropagation`本质上会关闭事件冒泡过程，防止事件流进一步传播。第二个方法`preventDefault`会阻止浏览器对该事件的默认行为（例如，点击链接时的页面跳转）。

这可以用于防止意外点击导致链接跳转，或防止表单因误触而提交。由于每个浏览器厂商都提供了一个略有不同的接口，这两种方法提供了统一的方式来阻止这些行为。

现在你已经了解了使用`Facade`模式的最佳实践。

## Props 与 State 的应用

回到我们最初的问题：如何在组件间传递数据并管理动态行为。

-   `Props`：负责组件的配置，如标题、内容、初始值等。
-   `State`：负责组件的动态行为，如开关状态、输入框内容、网络请求加载状态等。

一个常见的误解是认为`Props`应该是静态的，而`State`是动态的。实际上，`Props`也可以传递回调函数，让子组件能够通知父组件某些事件发生了（例如用户点击），从而实现动态交互。这是`Props`更强大的用途之一。

最后，你可能会疑惑：为什么我们不能只用一个全局对象来管理所有状态？因为这样做会导致数据流难以追踪，组件间耦合度增高，最终使应用变得不可维护。使用`Props`和`State`的组件化模式，正是为了解决这个问题而生的。

## 现实世界中的 Props 与 State

对于大多数前端框架而言，`Props`和`State`机制在底层实现上略有不同，但核心原则是一致的。

例如，在`React`中，`Props`是不可变的（immutable），由父组件传递。`State`是可变的，通过`useState`或`this.setState`来更新。当`State`或`Props`变化时，组件会重新渲染。

在`Vue`中，`Props`通过`props`选项定义，同样不可修改。`State`通常通过`data`函数返回响应式对象，或使用`Composition API`中的`ref`和`reactive`来定义。Vue 的响应式系统会自动追踪依赖并更新视图。

尽管具体实现不同，但它们都遵循相同的核心理念：**数据向下流动，事件向上流动**。

## 使用 `Set` 来管理多个元素

当您需要在一个集合中存储多个元素，并且这些元素都是唯一的、无序的，那么 `Set` 就是一个理想的集合类型。`Set` 不会包含重复的元素。

与 `List` 不同，`Set` 没有固定的顺序。这听起来可能像是一个缺点，但实际上它是一个非常有用的特性。它意味着您不必担心元素的顺序，并且 `Set` 能够快速地进行成员检查、添加和删除操作。

`Set` 非常适合用于去重。如果您有一个元素列表，并且希望确保每个元素只出现一次，那么您可以将该列表转换为 `Set`。

此外，`Set` 还提供了许多有用的集合操作，例如并集、交集和差集。

以下是 `Set` 的一些常用方法：

*   `Set.of(foo, position, absolute)`
*   `Set.of(foo, stop, top)`
*   `Set.of(foo, left, ‘ ‘)`

通过一个更复杂的接口，您可以组合所有这些逻辑功能，并将它们合并到一次函数调用中。它将使用 `Set` 作为底层数据结构，甚至无需客户端代码知道它。我们直接调用 `Set.of(foo, position=absolute, top=0, left=0)`。

这样看起来更像是一种语法糖，考虑到对象符号本质上是由 `name-value` 对组成的。这里就是 `Set.of()` 的实现。

对于 `Set.of(foo, position`)：

*   `Set.of(foo, bar, baz)`：
    *   `color`: `white`
    *   `background`: `black`
    *   `font-size`: `XXpx`
    *   `font-family`: `georgia`, `times`, `serif`

### 示例：创建一个 Event 实体

如前所述，当处理跨越浏览器发展时，常常需要创建一组函数。如果您正在开始深入创建大型库代码平台，这是一个绝佳的想法，可以将您的实用工具整合到一个单一的集合中，使之易于访问且简单易用。

开发一个事件实体通常是一个很好的想法。考虑到许多浏览器处理事件的方式存在差异。

首先，让我们从基础骨架开始，使用单例模式。它将包含我们所有静态方法中的 `Set.of()`。

让我们开始利用并解决一些开发人员常遇到的问题，例如获取事件目标和抓取事件对象。当然，我们也会借用事件传播和默认事件行为，从早期在本章中探讨的内容。

您现在有以下内容：

```
Set.of(event: function(e) {

    var target = getTarget(e);

    return target; // 返回目标元素

})
```

我们可以通过添加分支和检测对象的可用性来填补这些空白。这样，您就创建了五个可以让您更舒适地工作的方法，并拥有一个更一致的界面：

```
Set.of(event: function(e) {

    var event = getEvent(e);

    return window.event; // 返回事件对象

})
```

```
Set.of(target: function(e) {

    var target = getTarget(e);

    return e.srcElement; // 返回目标元素

})
```

```
Set.of(stopPropagation: function(e) {

    if (e.stopPropagation) {

        e.stopPropagation();

    } else {

        e.cancelBubble = true;

    }

})
```

```
Set.of(preventDefault: function(e) {

    if (e.preventDefault) {

        e.preventDefault();

    } else {

        e.returnValue = false;

    }

})
```

```
Set.of(stopEvent: function(e) {

    this.stopPropagation(e);

    this.preventDefault(e);

})
```

事件实体现在已经完整了，并且可以轻松地与之前编写的 `addEvent` 函数一起使用：

```
addEvent(element, "click", function(e) {

    e = e || window.event;

    // 处理点击事件

    console.log("点击了！");

});
```

### 实现这个 Set 模式的一般步骤

1. 从基础骨架开始，使用单例模式。
2. 它包含所有静态方法中的 `Set.of()`。
3. 开发一个事件实体通常是一个很好的想法，考虑到许多浏览器处理事件的方式存在差异。

我们通常可以通过添加分支和检测对象的可用性来填补这些空白，这样您就可以创建五个让您更舒适地工作的方法，并拥有一个更一致的界面。

### 具体实现示例

```javascript
Set.of(event: function(e) {
    e = e || window.event;
    // 处理事件
})
```

```javascript
Set.of(target: function(e) {
    var target = e.target || e.srcElement;
    // 返回目标元素
})
```

```javascript
Set.of(stopPropagation: function(e) {
    if (e.stopPropagation) {
        e.stopPropagation();
    } else {
        e.cancelBubble = true;
    }
})
```

```javascript
Set.of(preventDefault: function(e) {
    if (e.preventDefault) {
        e.preventDefault();
    } else {
        e.returnValue = false;
    }
})
```

```javascript
Set.of(stopEvent: function(e) {
    this.stopPropagation(e);
    this.preventDefault(e);
})
```

## 外观模式

外观模式是一种结构型设计模式，它为一个子系统中的一组接口提供一个统一的接口。外观模式定义了一个高层接口，使得子系统更容易使用。

### 动机

- 当需要将子系统中的一组接口统一为一个简化接口时，可以使用外观模式。
- 当希望将子系统与客户端解耦，或者将子系统组织成层次结构时，外观模式也很有用。
- 外观模式可以减少客户端与子系统之间的依赖关系，提高代码的可维护性和可读性。

### 定义

外观模式为子系统中的一组接口提供一个统一的接口。外观模式定义了一个高层接口，使得子系统更容易使用。

### 外观模式的适用场景

当你想为一个复杂的子系统提供一个简单的接口时，可以使用外观模式。这个接口通常只包含子系统的一部分功能，但足以满足大多数客户端的需求。

当客户端与子系统之间存在大量依赖关系时，外观模式可以降低这些依赖关系，使得系统更容易维护和扩展。当系统需要分层时，外观模式可以作为各层之间的入口点。

### 外观模式的优点

- 简化了子系统的使用，客户端无需了解子系统的内部细节。
- 降低了客户端与子系统之间的耦合度，提高了系统的灵活性和可维护性。
- 有助于将系统分层，使得各层之间更加清晰。

### 外观模式的缺点

- 如果不加限制地使用外观模式，可能会导致外观类变得过于庞大和复杂。
- 在某些情况下，外观模式可能会成为系统的瓶颈。

### 如何实现外观模式

1. 确定需要对外提供哪些功能。
2. 创建一个外观类，将子系统中的复杂操作封装起来。
3. 在外观类中提供简单的方法，供客户端调用。
4. 客户端只需与外观类交互，无需直接与子系统中的多个类打交道。

### 示例

```java
public class ComputerFacade {
    private CPU cpu;
    private Memory memory;
    private HardDrive hardDrive;

    public ComputerFacade() {
        this.cpu = new CPU();
        this.memory = new Memory();
        this.hardDrive = new HardDrive();
    }

    public void start() {
        cpu.freeze();
        memory.load(BOOT_ADDRESS, hardDrive.read(BOOT_SECTOR, SECTOR_SIZE));
        cpu.jump(BOOT_ADDRESS);
        cpu.execute();
    }
}
```

在上面的示例中，`ComputerFacade` 类封装了启动计算机的复杂过程，客户端只需要调用 `start()` 方法即可。

### 总结

外观模式是一种简单而强大的设计模式，它可以帮助你简化复杂系统的使用。通过提供一个统一的接口，外观模式可以减少客户端与子系统之间的依赖关系，提高代码的可维护性和可读性。同时，它也有助于将系统分层，使得各层之间更加清晰。

## 适配器模式

**适配器**是一种结构型设计模式，它能使不兼容的对象相互协作。

**别名：** 包装器

**适配器模式**允许你将一个接口转换成另一个客户端期望的接口，从而让原本因接口不兼容而无法协作的类能够一起工作。

**适用场景：** 当你希望使用某个类，但其接口与其他代码不兼容时，可以使用适配器模式。

### 适配器模式的结构

适配器用于连接两个不同的接口，以协调它们之间的交互。如果现有的代码已经有一个正在良好工作的接口，那么可能不需要适配器。但若接口不直观或对当前任务来说过于复杂，你就可以使用适配器来提供一个更清晰或更简单的接口。

在表面上，适配器模式看起来非常类似于外观模式。两者都包装一个对象并改变其呈现给世界的界面。区别在于，外观呈现的是一个简化的界面，它不提供任何额外选项，且不会修改现有接口。而适配器则是将一个接口转换为另一个，它不会移除任何功能，也不会简化界面。

### 适配器实现：`ClientToInterfaceAdapter`

```java
function clientToInterfaceAdapter(clientObject: string, string: string): void
```

你可能会有一个特定的函数，它接受三个字符串作为参数，但客户端持有一个包含三个元素的数组。适配器可以被用来让两者能够协同工作。

### 真实世界案例：适配器如何工作

想象一个案例，你有一个单一对象，但一个函数需要三个独立的字符串作为参数。

```java
var clientObject: { string: string, string: string, string: string }
```

```java
function interfaceMethod(string: string, string: string, string: string): void
```

```java
clientToInterfaceAdapter(clientObject.string, clientObject.string, clientObject.string);
```
