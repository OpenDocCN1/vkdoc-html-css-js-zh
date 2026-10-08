# 命令模式

## 命令模式

在本章中，我们将探讨方法的调用。命令模式在几个方面与普通函数不同：它提供了参数化并传递方法调用的能力，你可以随时执行该调用。它还允许你将调用动作的对象与实现该动作的对象解耦，从而在交换具体类时提供极大的灵活性。命令模式可用于多种不同情境，但在创建用户界面时尤其有用，特别是在需要无限撤销操作的情况下。该模式还可以替代回调函数使用，因为它在对象间传递动作时具有更高的模块化程度。

在接下来的几节中，我们将讨论命令模式的结构，并给出几个如何在 Java‑Script 中使用它的示例。我们还会介绍使用命令对象的最佳场景，以及不适宜使用它们的情况。

## 命令的结构

在最简单的形式下，一个命令对象将两样东西绑定在一起：一个动作，以及一个可能希望调用该动作的对象。所有命令对象都有一个共同点——一个执行操作，该操作涉及它所绑定的动作。在大多数命令对象中，这个操作是一个名为 `execute` 或 `run` 的方法。所有使用相同接口的命令对象可以被等同对待，并可以随意替换。这也是命令的魅力所在。

为了展示命令模式的典型用法，我们将通过一个关于动态用户界面的示例进行讲解。假设你经营一家广告公司，希望创建一个网页，允许客户对其账户执行某些操作，例如启动或停止特定广告的投放。由于无法预知会有多少广告，因此你需要创建一个尽可能灵活的用户界面（UI）。为此，你将使用命令模式来松散耦合 UI 元素（如按钮）与动作。

首先，你需要一个所有命令都必须响应的接口：

```
var AdCommand = new Interface(AdCommand, ["execute"]);
```

接下来，我们创建用于每个特定动作的具体类（命令）。例如，要启动一个广告，我们可以创建一个 `StartAd` 命令类；要停止一个广告，则可以创建一个 `StopAd` 命令类。

进入 `/path.to.xxx` 目录，找到 `xxx.json`。

在表达式 `cvar = avar + bvar` 中，加法运算符（`+`）将 `avar` 与 `bvar` 相加，得到它们的和 `cvar`。

在 `List.of(arg0, arg1, arg2)` 中，`List` 接口的工厂方法 `of()` 接受一系列的元素，返回包含它们的只读列表。

之后我们这样调用 `cmd` 命令：`cmd arg0 arg1 arg2`。

```
if (condVar > someVal) {console.log("xxx")}
```

对于 `encapsulating` 函数，`stop` 方法由 `for` 和 `while` 循环语句的 `break` 语句调用的 `StopIteration` 异常触发。

`StopIteration` 异常是 `for` 循环的内部机制，用于在迭代器被 `for` 消耗完毕时，或通过调用 `raise StopIteration` 显式停止迭代时通知循环结束。

## 静态与动态变量

`for` 循环的变量具有静态作用域，而 `while` 则每次迭代都重新计算变量生命周期，这类似于 `let` 和 `var` 的区别。

`for` 循环的变量生命周期跨越多轮循环，而 `while` 循环则每轮都重新创建变量，这影响了闭包和生成器中的行为。

## 可测试性

`for` 循环的迭代器模式允许你传入任何可迭代对象，因此可以传递给函数。`while` 循环则不那么灵活，它依赖于外部的变量状态。

## 动态分发

`while` 循环在每次迭代时都检查条件，因此可以用来实现动态变化的终结条件。`for` 循环则更受限于其迭代器的成员数。

例如，在服务器处理时，`while` 循环可判断新连接是否到来。而在处理固定已知集合时，`for` 循环是更好的选择。

```
var iterator = iter(iterable)
while True:
    try:
        value = next(iterator)
    except StopIteration:
        break
    # 处理 value

var iterable = iterable
for var index = 0; index < len(iterable); index++:
    var value = iterable[index]
```

这些例子灵活多变，展现了对循环机制的深入理解，但真正区分二者的在于：
> 运行时是否知道需要迭代的总次数。

**变量作用域**，**测试方法的支持**，以及**动态调用**这三者决定了循环的使用模式。

对于 `for`：
- 变量可被代码中的函数引用，或者传入生成器。

对于 `while`：
- 变量则必须是外部的，例如在调用 `cmd` 时使用的 `client` 实例。

## 打包与发送

- `createConnection` 函数的 `type` 字段是固定的，通常不是错误。
- 在分步场景中，客户端代码创建请求并传给调用方，调用方将请求打包并传入 `Runnable` 实例。

`sendMessage` 函数通常通过 `makeRequest` 函数完成其核心操作。

```
POST /api/startStop HTTP/1.1
Host: example.com
Content-Type: application/json
{"command": "stop"}
```

## 更简化的 API 设计

在某些回调模型中，对象和闭包通过常规函数参数传递给了函数。例如：

```
sendMessage("stop", on_success=stop_complete_handler, on_error=error_handler)
```

# 概述

## 什么是`CompletableFuture`？

`CompletableFuture`是 Java 中的一个类，用于实现异步编程。

## 为什么使用`CompletableFuture`？

传统的`Future`接口有一些局限性，比如无法手动完成、无法链式调用、无法组合多个异步操作等。`CompletableFuture`解决了这些问题。

## 主要特性

- 手动完成：可以手动设置`CompletableFuture`的结果
- 链式调用：支持`thenApply`、`thenAccept`、`thenRun`等方法
- 组合操作：支持`thenCombine`、`allOf`、`anyOf`等组合方法
- 异常处理：提供`exceptionally`、`handle`等方法

## 示例：`CompletableFuture`的基本用法

### 创建一个简单的`CompletableFuture`

```java
CompletableFuture<String> future = new CompletableFuture<>();
future.complete("Hello, World!");
```

### 使用`supplyAsync`创建异步任务

```java
CompletableFuture<String> future = CompletableFuture.supplyAsync(() -> {
    // 模拟耗时操作
    try {
        Thread.sleep(1000);
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
    return "Hello, World!";
});
```

### 链式调用

```java
CompletableFuture<String> future = CompletableFuture.supplyAsync(() -> {
    // 模拟耗时操作
    try {
        Thread.sleep(1000);
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
    return "Hello, World!";
}).thenApply(s -> s + " CompletableFuture");

future.thenAccept(System.out::println);
```

这种方式本质上允许类型推断，适用于只依赖单一特定方法的对象。

## 架构模式

在面向对象的循环架构中，核心是**客户端**、**调用者**和**接收者**三个角色的配合。这种模式支持高度模块化和松耦合的设计。

例如，在`observer`模式中，一个对象变化时，会通知所有依赖它的观察者，而无需责任链：观察者必须自己注册成为通知节点。

**观察者模式**要求被观察者维护一个列表，用于跟踪观察者。但在不同的业务场景下，`for`循环和`while`循环会有不同的表现。

在分布式系统中，`while`循环一般用于连接池和任务队列，`for`循环则更适合用于遍历。

## 序列保证

`while`循环通常不保证执行顺序，它仅仅执行条件是否为真，适合无固定范围的轮询。`for`循环则按容器顺序执行，适用于有序列表。

例如，在数据库索引场景下，`for`循环和`Runnable`模式配合得很好：
- 当不断读入数据库行时，可采用`for`循环。
- 处理无序并发任务时，可采用`while`循环。

## 通用模式

在没有函数式结构的情况下，直接使用`for`或`while`循环也足够了，不过更简洁的方法是使用列表推导式。

### 代码示例

```python
def makeStopRequest(on_success, on_error):
    def inner():
        client = get_client()
        client.send_stop()
        on_success(client.last_response)
    return inner
```

```python
def makeStopRequest(on_success, on_error):
    def inner():
        client = get_client()
        connection = client.connect()
        message = {"command": "stop"}
        connection.send(message)
        on_success(connection.last_response)
    return inner
```

这些模式在分布式系统中可以进一步推广，命令既可以由客户端发起，也可以由服务端发起。

这种模式的优点包括：
- 清晰地传递状态。
- 简化异常处理。
- 在不同环境下均可复用。

## 调用机制

`Invoker`执行`Command`的示例。

此方法执行命令。`Invoker`是`Client`创建的，`Client`配置了`Invoker`。

`Receiver`执行实际操作，但`Invoker`不知道具体细节。`Command`是唯一知道如何调用`Receiver`方法以执行所需操作的对象。

当`Command`被调用时，执行接收者（`receiver`）的`action()`方法，从而完成请求的执行。如果需要，`Client`可以随后查询命令以检查执行结果。

所有使用此模式的系统都有`Client`和`Invoker`，但`Receiver`不总是被需要。可以创建复杂的“思路”较少的模块化命令，不涉及接收者对象。不过，这些命令可以执行更复杂的查询或调用，不依赖于接收者。

## 使用`Command`接口的示例类型

所有类型的命令对象都执行相同的任务：它们通过解耦调用者和接收者，将执行操作封装在一个对象中。但这一定义内有一个范围，由两个极端组成。

一端是简单命令（`SimpleCommand`），这是一个松散耦合的简单命令类。另一端是复杂命令（`ComplexCommand`），一个紧密耦合的复杂命令类。

### 简单命令（`SimpleCommand`）

简单命令将接收者（`receiver`）的`action()`方法封装起来，供调用者（`invoker`）使用。客户端（`client`）创建接收者和命令对象，然后将命令传递给调用者。

- 简单命令：松散耦合，简单命令类。实现`SimpleCommand.receiver.action()`。
- 如果使用闭包，这个检查甚至更简单，因为你只需要检查调用是否实际是一个函数。

### 复杂命令（`ComplexCommand`）

复杂命令并不真正拥有一个接收者，因为动作是命令本身内部实现的。它包含一组指令，执行需要传递的复杂操作，不涉及接收者。这个类型实际上是一个“宏命令”，封装了一组子命令或复杂的业务逻辑。

- 复杂命令：紧密耦合，复杂命令类。实现`ComplexCommand.execute()`。
- 如果使用日志，这个检查也是必要的，因为你要确保执行的操作有定义。如果没有，一个描述性的异常将被抛出。

## 使用`Client`调用`Command`的示例

在第一个示例中，我们保持简单，但在后续示例中会使用更高级的类型。我们强烈推荐你也使用它们。

### 类型

所有类型的命令对象都执行相同的任务：它们执行操作，将调用者与接收者解耦。但在这个定义内，有一个范围，由两个极端组成。一端是简单命令，这是一个松散耦合的简单命令类。另一端是复杂命令，一个紧密耦合的复杂命令类。

创建命令对象时，客户端通常创建一个命令对象，它封装了要执行的操作以及所有必要的参数。然后，客户端将这个命令对象传递给一个调用者，该调用者在需要时执行它。在执行过程中，调用者调用命令对象的`execute()`方法，该方法会执行实际的操作。在这个过程中，调用者不知道具体执行的是什么操作，它只负责触发命令的执行。

### 简单命令（`SimpleCommand`）示例

- `SimpleCommand`：松散耦合，简单命令类。实现`SimpleCommand.receiver.action()`。
- 如果使用闭包，这个检查甚至更简单，因为你只需要检查调用是否实际是一个函数。

### 复杂命令（`ComplexCommand`）示例

- `ComplexCommand`：紧密耦合，复杂命令类。实现`ComplexCommand.execute()`。
- 如果使用日志，这个检查也是必要的，因为你要确保执行的操作有定义。如果没有，一个描述性的异常将被抛出。

我们把这些检查放在第一个示例中，以保持简单，但我们在后续示例中会使用它们。我们强烈推荐你也使用它们。

# 概述

`CompletableFuture` 是 Java 8 中引入的一个强大的异步编程工具。它解决了传统 `Future` 的局限性，提供了丰富的 API 来创建、组合、处理异步任务。通过 `CompletableFuture`，您可以更轻松地编写高效、可读性强的异步代码。

## 基本用法

```java
CompletableFuture<String> future = CompletableFuture.supplyAsync(() -> "Hello")
        .thenApply(s -> s + ", World!")
        .thenApply(String::toUpperCase);

future.thenAccept(System.out::println);
```

### 组合多个 `CompletableFuture`

```java
CompletableFuture<String> future1 = CompletableFuture.supplyAsync(() -> "Hello");
CompletableFuture<String> future2 = CompletableFuture.supplyAsync(() -> "World");
CompletableFuture<String> combined = future1.thenCombine(future2, (s1, s2) -> s1 + ", " + s2 + "!");
```

### 异常处理

```java
CompletableFuture<String> future = CompletableFuture.supplyAsync(() -> {
    if (true) {
        throw new RuntimeException("Something went wrong");
    }
    return "Success";
}).exceptionally(ex -> "Recovered from: " + ex.getMessage());
```

## `CompletableFuture` 的构造函数

`CompletableFuture` 提供了多种构造函数，用于创建不同状态的 `CompletableFuture` 实例。

```java
// 默认构造函数，创建一个未完成的 CompletableFuture
CompletableFuture<String> future = new CompletableFuture<>();

// 通过 CompletableFuture.completedFuture() 创建一个已完成的 CompletableFuture
CompletableFuture<String> completedFuture = CompletableFuture.completedFuture("Hello");
```

## `CompletableFuture` 的核心方法

### 创建 `CompletableFuture` 的方法

- `supplyAsync(Supplier<U> supplier)`：使用 `ForkJoinPool.commonPool()` 执行异步任务
- `runAsync(Runnable runnable)`：使用 `ForkJoinPool.commonPool()` 执行异步任务，无返回值
- `completedFuture(U value)`：创建一个已完成的 `CompletableFuture`

### 链式操作方法

- `thenApply(Function<? super T,? extends U> fn)`：对结果进行转换
- `thenAccept(Consumer<? super T> action)`：消费结果，无返回值
- `thenRun(Runnable action)`：执行一个 `Runnable`，无需结果
- `thenCompose(Function<? super T, ? extends CompletionStage<U>> fn)`：将两个 `CompletableFuture` 扁平化组合

### 组合方法

- `thenCombine(CompletionStage<? extends U> other, BiFunction<? super T,? super U,? extends V> fn)`：将两个 `CompletableFuture` 的结果合并
- `allOf(CompletableFuture<?>... cfs)`：等待所有 `CompletableFuture` 完成
- `anyOf(CompletableFuture<?>... cfs)`：等待任意一个 `CompletableFuture` 完成

### 异常处理方法

- `exceptionally(Function<Throwable, ? extends T> fn)`：发生异常时的回调
- `handle(BiFunction<? super T, Throwable, ? extends U> fn)`：无论是否发生异常都执行
- `whenComplete(BiConsumer<? super T, ? super Throwable> action)`：完成后执行的回调

## `CompletableFuture` 的完整示例

```java
import java.util.concurrent.CompletableFuture;
import java.util.concurrent.ExecutionException;

public class CompletableFutureExample {
    public static void main(String[] args) throws ExecutionException, InterruptedException {
        CompletableFuture<String> future = CompletableFuture.supplyAsync(() -> {
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            return "Hello, World!";
        });

        // 链式调用
        CompletableFuture<String> upperCaseFuture = future.thenApply(String::toUpperCase);
        CompletableFuture<Void> printFuture = upperCaseFuture.thenAccept(System.out::println);

        // 等待完成
        printFuture.get();
    }
}
```

在上面的示例中，我们创建了一个 `CompletableFuture`，使用 `supplyAsync` 异步执行任务，然后通过 `thenApply` 转换结果，最后通过 `thenAccept` 打印结果。

## `CompletableFuture` 的内部机制

`CompletableFuture` 的实现基于 `ForkJoinPool`，它是 Java 8 引入的一个轻量级线程池。`CompletableFuture` 使用 `ForkJoinPool.commonPool()` 作为默认的线程池来执行异步任务。

`CompletableFuture` 内部使用了一个 `Completion` 链表来管理依赖关系。当 `CompletableFuture` 完成时，它会遍历这个链表，逐个执行后续的操作。

## 总结

`CompletableFuture` 是 Java 8 中引入的一个强大的异步编程工具。它解决了传统 `Future` 的局限性，提供了丰富的 API 来创建、组合、处理异步任务。通过 `CompletableFuture`，您可以更轻松地编写高效、可读性强的异步代码。

## 成员变量

- `Rent` 类包含以下私有成员变量：
    - `person`：`Person` 类型，表示租赁的租户。
    - `rent`：`double` 类型，表示租金金额。
    - `startDate`：`LocalDate` 类型，表示租赁开始日期。
    - `endDate`：`LocalDate` 类型，表示租赁结束日期。
    - `property`：`Property` 类型，表示被租赁的物业。
    - `tenants`：`List<Person>` 类型，表示所有租户的列表，包含 `person`。
    - 该 `container` 包含 `container` 中的 `Person` 对象。
    - 该 `rent` 包含 `rent` 中的 `Person` 对象。

## 构造方法

### `Rent(Person person, double rent, LocalDate startDate, LocalDate endDate, Property property)`

创建一个新的 `Rent` 对象。所有参数都是必需的。租金金额不能为负数。

### `Rent(Person person, double rent, LocalDate startDate, Property property)`

创建一个新的 `Rent` 对象。此构造方法使用 `endDate` 参数，将其设置为 `null`。这表示租赁尚未结束，是一个开放式的租赁。当调用 `endRent()` 方法时，`endDate` 将被设置为当前日期。

## 方法

### `addTenant(Person person)`

将一个新的租户添加到租户列表中。

### `removeTenant(Person person)`

从租户列表中移除一个租户。

### `getProperty()`

返回与此租赁关联的 `Property` 对象。

### `getPerson()`

返回与租赁关联的 `Person` 对象。

### `getEndDate()`

返回租赁的结束日期。

### `show()`

显示此租赁的详细信息。对于租户列表中的每个 `Person` 对象，调用其 `show()` 方法。

## 运行示例

以下是一个演示如何使用 `Rent` 类的示例：

```java
public class RentDemo {
    public static void main(String[] args) {
        // 创建物业和人员
        Property property = new Property("123 Main St", 2, 1);
        Person person = new Person("John Doe", 30);
        // 创建租赁
        Rent rent = new Rent(person, 1000.0, LocalDate.now(), property);
        rent.addTenant(person);
        // 显示租赁信息
        rent.show();
    }
}
```

## 整合所有内容

设置完这个复杂架构的结果是，实现代码非常简洁。这些 `Person` 对象没有对如何执行它们被绑定到的动作的概念；它们只知道它们被绑定到了一个命令。每个 `Rent` 实例的 `person` 属性存储了对这个命令的引用，而这个引用被封装在一个闭包中。

每个 `Rent` 实例都绑定到一个命令。那个命令不能被更改，因为它是被封装在闭包中并被附加为一个事件监听器的。如果你需要更改一个 `Person` 实例被绑定到的命令，你必须创建一个新的 `Rent` 对象。

## `Command` 类

`Command` 类，即 `RentCommand`，极其简单，几乎和一个简单的命令类一样简单。构造函数接受一个参数：要执行的动作。

```java
public class RentCommand {
    private final Runnable action;
    public RentCommand(Runnable action) {
        this.action = action;
    }
    public void execute() {
        action.run();
    }
}
```

如果某个动作方法内部使用了关键字 `this`，那么它必须被包装在一个匿名函数中。这里有一个例子：

```java
RentCommand someCommand = new RentCommand(this::someRentCommandMethod);
```

这个架构的最终结果是，实现代码非常简洁。这些 `Person` 对象没有对如何执行它们被绑定到的动作的概念；它们只知道它们被绑定到了一个命令。每个 `Rent` 实例的 `person` 属性存储了对这个命令的引用，而这个引用被封装在一个闭包中。它们只知道它们被绑定到了一个命令。它们不需要知道如何执行这些动作，而且它们不能改变这些动作。它们只需要知道如何调用 `execute` 方法。

## 添加/追加菜单项到 SentMenu

在 `SentMenu` 的主菜单中添加菜单项。

### 方式一：使用 `addMenuItem()` 方法

在 `ViewController` 的 `viewDidLoad` 方法中添加菜单项。

#### 示例代码

```swift
// Swift
```

```objective-c
// Objective-C
```

```objective-c
// Objective-C 添加菜单项
```

```objective-c
// 添加新菜单项
```

```objective-c
// 编辑菜单项
```

```objective-c
// 编辑新菜单项
```

```objective-c
// 插入菜单项
```

```objective-c
// 插入新菜单项
```

```objective-c
// 保存菜单项
```

```objective-c
// 保存新菜单项
```

```objective-c
// 添加任意文件到 SentMenu
```

```objective-c
// 添加文件
```

```objective-c
// 打开文件
```

```objective-c
// 关闭文件
```

```objective-c
// 保存文件
```

```objective-c
// 文件路径: file_addSentMenu_ViewModel/Open_ViewModel
```

```objective-c
// 文件路径: file_addSentMenu_ViewModel/Close_ViewModel
```

```objective-c
// 文件路径: file_addSentMenu_ViewModel/Save_ViewModel
```

```objective-c
// 文件路径: file_addSentMenu_ViewModel/Save_As_ViewModel
```

```objective-c
addSentMenu_ViewModel/fileViewModel.xib
```

### 方式二：使用 `Edit` 菜单

```objective-c
// 编辑菜单项
```

```objective-c
// 编辑 SentMenu 菜单项
```

```objective-c
// 复制 SentMenu 菜单项
```

```objective-c
// 粘贴 SentMenu 菜单项
```

```objective-c
// 删除 SentMenu 菜单项
```

```objective-c
// 编辑 ViewModel
```

```objective-c
// 复制 ViewModel
```

```objective-c
// 粘贴 ViewModel
```

```objective-c
// 删除 ViewModel
```

```objective-c
addSentMenu_ViewModel/editSentMenu.xib
```

### 方式三：使用 `Insert` 菜单

```objective-c
// 插入菜单项到 Insert 菜单
```

```objective-c
// TextBlock 添加菜单项
```

```objective-c
// 插入 ViewModel
```

```objective-c
// 插入文本
```

```objective-c
insertSentMenu_ViewModel/insertText.xib
```

### 方式四：使用 `Help` 菜单

```objective-c
// 帮助菜单项
```

```objective-c
// showHelp 添加菜单项
```

```objective-c
// 显示帮助
```

```objective-c
helpSentMenu_ViewModel/showHelp.xib
```

## 构建菜单栏

```objective-c
components/getElements/DatePicker.xib -> appendChild(getElements/appendChild.xib)
```

```objective-c
appendSentMenu_ViewModel/show.xib
```

## 更多菜单项 Material UI

很容易向 `SentMenu` 中添加更多菜单项。例如，如果你想向 `Insert` 菜单添加一个 `ImageCommand`，只需两行代码。假设 `Insert` 类实现了 `ImageCommand`，那么 `ImageCommand` 将调用该操作。你甚至可以拥有多个使用相同接收器的用户界面，或者使用相同的 `Command` 对象。`ImageCommand` 可以被重用，并作为`第一类对象`传递，因此没有理由不能执行它，即使是由不同的调用者执行。

### 示例：Undo 和 Redo

还有一种方法，有时在 `SentMenu` 中实现。它允许调用者回滚执行的操作，例如，`execute()`。这个 `undo` 方法可以用来实现无限撤销。用户只需要跟踪所有已经被推送到堆栈顶部的 `command`，然后当用户想要撤销最后一个动作时，他们可以点击 `undo` 按钮。这时 `undo` 方法将被调用。用户甚至可以撤销所有已经执行过的 `actions`，一直向下到堆栈底部。然后所有 `actions` 可以被重新推回堆栈，他们可以重做所有 `actions`，直到堆栈底部。

为了说明如何使用命令模式实现无限撤销，我们创建一个类似于 `Etch-A-Sketch` 的游戏。将有四个移动按钮，每个按钮将光标移动相应的像素数量：向下、向左、向右，以及一个允许所有动作被反转的 `undo` 按钮。首先必须修改 `Command` 接口以添加 `undo` 方法。

```swift
// Swift
```

```objective-c
// Objective-C
```

```java
// 使用 Command 接口实现 undo 方法
```

```java
// UndoRedo 示例
```

## 可撤销的 Command 接口

```java
// 定义可撤销的命令接口
```

## 支持撤销操作

```java
// 实现撤销功能
```

在`ReverseCalling`类中，`a`和`b`是`int`类型变量。

`getSum()`方法调用`a`和`b`的`getValue()`方法，并使用`+`操作符将结果相加，得到`c`。

`setResult(c)`方法将`c`赋值给`this.result`。

`print()`方法输出`this.result`。

`return`返回`this.result`。

`Main`类的`main()`方法创建`ReverseCalling`对象，并调用其方法。

## 变量与运算符

对于`Variable a`和`func(a, b)`：

- `a.getInt()`：返回`a`的数值。
- `a.setValue(val)`：设置`a`的新值。
- `a.toString()`：返回`a`的字符串表示。
- `a.clone()`：返回`a`的深层副本。
- `a.equals(other)`：判断`a`是否与`other`相等。
- `a.hashCode()`：返回`a`的哈希值。
- `a.compareTo(other)`：比较`a`与`other`的大小。
- `this.val`：当前对象的`val`属性。
- `func(this.val)`：调用`func`，参数为`this.val`。

`运算符重载`：`a + b`调用`a.plus(b)`，`a - b`调用`a.minus(b)`，`a * b`调用`a.multiply(b)`，`a / b`调用`a.divide(b)`，`a % b`调用`a.mod(b)`。

`Variable a`的构造函数：`a = new Variable(1)`。

`a.getValue()`返回`val`。`a.setValue(val)`设置`val`为`val`。

`a.getAge()`返回`age`。`a.setAge(age)`设置`age`为`age`。

`this.val`的默认值为`0`（`int`类型）。

`a instanceof Variable`：判断`a`是否是`Variable`的实例。

`Variable`类的`clone()`方法：`return new Variable(this.val)`。`Variable`类的`equals()`方法：`return this.val == other.val && this.age == other.age`。

`Variable`类的`hashCode()`方法：`return Objects.hash(this.val, this.age)`。

`Variable`类的`compareTo()`方法：`return Integer.compare(this.val, other.val)`。

`Variable`类的`toString()`方法：`return String.format("Variable{val=%d, age=%d}", val, age)`。

## 类与继承

`Variable`类继承自`Object`类，实现了`Comparable<Variable>`接口。

## 调用方式

`ReverseCalling`类通过不同的方式实现变量和函数的调用。

## 变量与函数反向调用

`ReverseCalling`类通过反射机制实现变量和函数的反向调用。

## 应用场景

`ReverseCalling`类适用于需要动态调用变量和函数的场景。

## 实现方式

`ReverseCalling`类通过`java.lang.reflect`包中的`Field`和`Method`类实现变量和函数的反向调用。

## 示例

```java
// 创建一个 Variable 对象
Variable var = new Variable(10);

// 获取 var 的 value 属性
int value = var.getValue();

// 设置 var 的 value 属性
var.setValue(20);

// 调用 var 的 print 方法
var.print();

// 调用 var 的 add 方法
int result = var.add(5);
```

## 总结

`ReverseCalling`类提供了一种灵活的方式来动态调用变量和函数，适用于需要高度灵活性的场景。通过`Field`和`Method`类，我们可以实现对任意对象的方法和变量的动态调用。

## 实现细节

`ReverseCalling`类通过`java.lang.reflect`包中的`Field`和`Method`类实现变量和函数的反向调用。

### 注意事项

在使用`ReverseCalling`类时，需要注意以下几点：

- 确保要调用的变量和函数是可访问的。
- 注意处理异常情况。
- 确保代码的性能和安全性。

## 示例

```java
// 创建一个 ReverseCalling 对象
ReverseCalling rc = new ReverseCalling();

// 调用 rc 的 callMethod 方法
rc.callMethod("print");

// 调用 rc 的 callField 方法
rc.callField("value", 20);
```

# 总结

`ReverseCalling`类提供了一种灵活的方式来动态调用变量和函数，适用于需要高度灵活性的场景。通过`Field`和`Method`类，我们可以实现对任意对象的方法和变量的动态调用。

# 标题

## 示例

`Cursor.getElementsByTagName("tagName")` → `Cursor.getElementsByTagName("tagName")`

`Cursor.forward` → `Cursor.forward`

`Cursor.getParserPosition()` → `Cursor.getParserPosition()`

`Cursor.getToken` → `Cursor.getToken`

`all` → `all`

`Cursor.getTokenList(event)` → `Cursor.getTokenList(event)`

`for body` → `for body`

`for cursor` → `for cursor`

`for left` → `for left`

`for right` → `for right`

`for up` → `for up`

`for down` → `for down`

`for undo` → `for undo`

`for redo` → `for redo`

`for left` → `for left`

`for right` → `for right`

`for undo` → `for undo`

这会创建一个带有画布和五个按钮的界面。点击四个命令按钮中的任何一个，光标都会向相应方向移动。点击撤销按钮则会逆转这些移动。

用不可逆的操作——比如移动光标——来实现撤销功能。如果操作本身不可逆，实现无限撤销就会困难得多。例如，若你想将之前的示例改进为更精确的动作模型，像从一点移动到另一点这样的操作，其本身就有一个精确的逆向操作，能使第一个操作看起来被逆转了。但如果你从点  画一条线到点 ，再从点  画一条线到点 ，并不会逆转该操作；它只会在这条线上再画一条新线。

真正撤销操作的唯一方法是清除当前状态，重做迄今为止执行的所有操作，并跳过最后一个操作。这可以通过将所有已执行的命令记录到一个栈中轻松实现。当你想撤销一个操作时，从栈中弹出最后一个命令并丢弃它。然后清除画布，从头开始重新执行所有命令。使用命令日志记录来实现撤销的系统不需要可逆的命令。在这个例子中，你只需使用原始的`Command`接口。

前一个示例与当前示例之间的变化很细微。大部分代码是相同的，因此我们只讨论发生变化的代码。第一个变化是，我们从所有命令中移除了`undo`方法，因为这些操作不再可逆。以下是移除该方法后的一个命令类。

## 移动命令

`for MoveUp` → `function cursor` → `11 implements Command`
`this.cursor` → `cursor`
`\n`
`MoveUp.prototype.execute` → `\n`
`execute()` → `\n`
`cursor.move(0, -10);`
`\n`
下一个，也是最大的变化，在于`undo`栈。`undo`栈已被替换为`Cursor.getTokenList` `Cursor.getToken` `all` `body` `cursor` `for` `left` `right` `up` `down` `Cursor.getElementsByTagName("tagName")` `Cursor.forward` `Cursor.getParserPosition()` `Cursor.getToken` `all` `for body` `for cursor` `for left` `for right` `for up` `for down` `for undo` `for redo` `for left` `for right` `for undo`

# `Unga` 类介绍

`Unga` 类有一个内部类型 `Unga`，它是 `Cursor` 类型的别名，用于表示一个指向 `Unga` 类型的指针。

`Unga` 类已被移除。以下是新的 `Cursor` 类。

## `Cursor` 类（包含内部 `Unga` 类型）

`var Cursor` 是一个函数/宽度/高度/父级/局部变量/引用/类名。
`this.width` 是宽度。
`this.height` 是高度。
`this.callingTick` 是 `""`。
`this.inv` 是 `gcument.createElement('div')`。
`this.inv` 的宽度 = `this.width`。
`this.inv` 的高度 = `this.height`。
执行 `parent.appendChild(this.inv)`。
`this.outerHTML` = `this.inv.outerHTML`。
`this.stx` = `this.inv.getContext('2d')`。
`this.move(x, y)`。
`this.clearRect(0, 0, this.width, this.height)`。

## `Cursor` 类（`prototype`）

`move(dx, dy)` 函数(x, y) {
  `var that = this`
  `this.callingTick.push(function() { lineTo(x, y); });`
  `this.executeCallings()`
}

`lineTo(dx, dy)` 函数(x, y) {
  `LINE:` `A`
  `INT` `for var i = 0, len = this.callingTick.length; i < len; i++`
  `this.callingTick[i]` `()`
}

`this.callingTick` = `[]`

`this.stx.beginPath()`

`this.stx.moveTo(this.position.x, this.position.y)`

`for var i = 0, len = this.callingTick.length; i < len; i++` {
  `this.callingTick[i]()`
}

`this.stx.clearRect(0, 0, this.width, this.height)`
`this.stx.stroke()`

`Unga` 函数(x) {
  `for i = 0, len = this.callingTick.length; i < len; i++`
  `this.callingTick.pop()`
}

`this.callingTick = []`

`Unga` 函数() {
  `this.callingTick.push(function() {`
    `this.stx.lineTo(this.position.x, this.position.y)`
  `});`
  `this.executeCallings()`
}

## 已添加三个新方法。

`move` 方法已被更改。它不再像 `push` 那样将动作添加到调用栈顶部，而是执行 `lineTo` 操作并执行调用栈，然后遍历调用栈，移除最后一个调用，最后调用 `executeCallings` 来重建系统状态。
这里 `Cursor` 类中似乎存在一个命名问题。`callingTick` 实际上并不包含调用对象，它只包含函数引用。`executeCallings` 方法并不实际调用任何调用对象，它只涉及栈中的函数。所以你需要回去并移除属性中提到的 `push` 等操作，因为这里的命令并非真正的 `push`，它们只是引用。
现在你有了一个在线的 `Unga` 类型，具备无限次调用能力。因为调用是由按钮触发的，所以你可以很容易地添加新按钮。例如，你可以添加 `UpCursor`、`DownCursor`、`LeftCursor`、`RightCursor`、`UpButton`、`DownButton`、`LeftButton`、`RightButton`、`UngaButton`，以及一个带有 `body` 的 `Cursor` 按钮。

## `Cursor` 类实现

`var Cursor` 是 `gcument.createElement('div')`
`var new Cursor(100, 100, body)`
`var upCursor = new KeyUpCursor()`
`var downCursor = new KeyDownCursor()`
`var leftCursor = new KeyLeftCursor()`
`var rightCursor = new KeyRightCursor()`
`var upButton = new CursorButton('Up', upCursor, body)`
`var downButton = new CursorButton('Down', downCursor, body)`
`var leftButton = new CursorButton('Left', leftCursor, body)`
`var rightButton = new CursorButton('Right', rightCursor, body)`
`var ungaButton = new UngaButton('Unga', new Cursor(100, 100, body), body)`

现在你有了一个在线的 `KeyUnga` 类型，具备无限次 `Unga` 调用能力。因为调用是由按钮触发的，所以你可以很容易地添加新的按钮。例如，你可以添加
