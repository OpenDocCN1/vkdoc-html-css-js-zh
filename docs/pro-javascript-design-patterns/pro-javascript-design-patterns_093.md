# 代理模式

在最基本的形式中，代理模式根本不实现任何访问控制。它只是将所有方法调用传递给真实主题。这种代理虽然无用，但确实为后续开发提供了基础。

在本章中，我们探讨了代理模式。

## 示例

### 公共图书馆代理

```java
public class PublicLibraryProxy implements PublicLibrary {

    private PublicLibrary realLibrary;

    @Override
    public List<String> getISBNs() {
        return realLibrary.getISBNs();
    }

    @Override
    public String getTitle(String isbn) {
        return realLibrary.getTitle(isbn);
    }

    @Override
    public String getAuthor(String isbn) {
        return realLibrary.getAuthor(isbn);
    }

    @Override
    public String getTitleAndAuthor(String isbn) {
        return realLibrary.getTitleAndAuthor(isbn);
    }

    @Override
    public String display() {
        return realLibrary.display();
    }
}
```

### 懒加载

```java
public class PublicLibraryProxy implements PublicLibrary {

    private PublicLibrary realLibrary;

    public PublicLibraryProxy() {
        // 尚未初始化 realLibrary。
    }

    private void initialize() {
        if (realLibrary == null) {
            this.realLibrary = new RealLibrary();
        }
    }

    @Override
    public String getTitleAndAuthor(String isbn) {
        initialize();
        return realLibrary.getTitleAndAuthor(isbn);
    }

    // ... 其他方法 ...
}
```

### 计数使用

```java
public class PublicLibraryProxy implements PublicLibrary {

    private PublicLibrary realLibrary;
    private int titleAndAuthorCount = 0;

    @Override
    public String getTitleAndAuthor(String isbn) {
        titleAndAuthorCount++;
        return realLibrary.getTitleAndAuthor(isbn);
    }

    public int getTitleAndAuthorCount() {
        return titleAndAuthorCount;
    }
}
```

### 借出图书

```java
public class PublicLibraryProxy implements PublicLibrary {

    private PublicLibrary realLibrary;

    @Override
    public void checkOutBook(String isbn) {
        if (realLibrary.isAvailable(isbn)) {
            realLibrary.checkOutBook(isbn);
        } else {
            throw new BookNotAvailableException("ISBN 为 " + isbn + " 的图书当前不可借阅。");
        }
    }
}
```

### 抛出异常

```java
public class PublicLibraryProxy implements PublicLibrary {

    // ...

    @Override
    public void checkOutBook(String isbn) {
        if (!realLibrary.isAvailable(isbn)) {
            throw new BookNotAvailableException("ISBN 为 " + isbn + " 的图书不可借阅。");
        }
        realLibrary.checkOutBook(isbn);
    }

    // ...
}
```

### 归还图书

```java
public class PublicLibraryProxy implements PublicLibrary {

    @Override
    public void returnBook(String isbn) {
        // 归还图书无需访问控制。
        realLibrary.returnBook(isbn);
    }
}
```

## 总结

- 代理模式为另一个对象提供替身或占位符，以控制对该对象的访问。
- 代理的类型包括：
    - 远程代理：代表不同地址空间中的对象。
    - 虚拟代理：延迟对象的创建，直到需要时才创建（懒加载）。
    - 保护代理：控制对真实对象的访问。
    - 智能引用代理：在访问对象时执行额外操作，例如计数引用。
- 示例：`PublicLibraryProxy` 可以是一个虚拟代理，仅在需要时才创建 `RealLibrary`；或者是一个保护代理，在借出图书前检查其可借性。

```java
public class Main {

    public static void main(String[] args) {
        RealLibrary realLibrary = new RealLibrary();
        PublicLibraryProxy proxy = new PublicLibraryProxy(realLibrary);

        // 代理将请求委托给真实的图书馆。
        System.out.println(proxy.getTitleAndAuthor("1234567890"));
    }
}
```

---

**本章结束。**

`InitializingBean` 接口的 `afterPropertiesSet()` 方法在所有 bean 属性被设置后、`init-method` 之前被调用。

通常在 `ProxyFactoryBean`（参见附录 A）中，`afterPropertiesSet()` 会创建一个 `Advised` 对象。

`ProxyFactoryBean` 的父类是 `ProxyCreatorSupport`。`ProxyCreatorSupport` 是 `Advised` 接口的默认实现（`Advised` 接口提供对 AOP 代理配置的控制，例如添加/移除拦截器）。

`ProxyFactoryBean` 类的 `getObject()` 方法有两种行为：取决于 `singleton` 或 `prototype`。
- 如果是 `singleton`，每次返回同一个实例。
- 如果是 `prototype`，每次返回一个新实例。

如果 `ProxyFactoryBean` 是 `singleton`，它将使用 `SingletonTargetSource`；否则，它将使用 `PrototypeTargetSource`。

## 关键差异: `ProxyFactoryBean` vs `SimpleBean` 自动装配

`ProxyFactoryBean` 是一个 `FactoryBean`，而 `SimpleBean` 本身是一个普通的 POJO。它们之间的关键区别在于：`ProxyFactoryBean` 不会创建一个 AOP 实例，它只是将参数存储到构造函数中，直到调用一个方法来实际执行实例化。

这类似于我们创建了一个带有动态接口的装饰器：如果对象从未被需要，就不会被创建。一个虚拟代理通常有一些事件来触发实例化，比如方法调用。

### `VirtualProxy` 示例

```java
// 假设 AopProxy 类对于大型对象来说初始化太慢，无法在页面加载时立即实例化。

// 你可以创建一个虚拟代理，它会延迟实例化，直到需要时才进行。
```

### 代理类型: 远程代理、虚拟代理和保护代理

虚拟代理很可能是对 Java 脚本程序员最有用的代理类型。让我们简要地介绍一下其他类型，并解释它们为什么不太适用于 Java 脚本。

**远程代理**用于在不同环境中访问对象。使用 Java，这可能意味着一个对象位于不同虚拟机上，或者一个对象位于世界的另一边。远程对象通常是持久化的，可以从任何其他环境在任何时间访问。这种类型的代理难以转化为 Java 脚本，原因有二：第一，不太可能有一个典型的 Java 脚本运行时会持久化；第二，大多数 Java 脚本环境存在于一个 Web 浏览器的上下文中，每个运行时间通常是在几毫秒内加载和卸载的。

**保护代理**通常用于控制对具有不同访问权限集的方法的访问。

`ProxyFactoryBean` 是 `Advised` 接口的默认实现（`Advised` 接口提供对 AOP 代理配置的控制，例如添加/移除拦截器）。

## 代理模式 (Proxy Pattern)

在代理模式中，一个类代表另一个类的功能。这种类型的设计模式属于结构型模式，它创建了一个具有现有对象的对象，以便向外界提供功能接口。

### 目的

为其他对象提供一种代理以控制对这个对象的访问。使用代理模式，我们可以创建具有更多功能的代理对象，而无需修改原始对象的代码。它常用于需要更复杂的方式引用对象，而非简单指针的场景。

**代理模式**与**装饰器模式**类似，但侧重点不同。代理模式控制对对象的访问，而装饰器模式则动态地为对象添加功能。两者都是对现有对象进行包装，但代理模式更侧重于控制访问，而装饰器模式更侧重于增强功能。

### 何时使用

- 想在访问一个类时做一些控制时。

### 示例

一个保护代理（`Protection Proxy`）可以控制对特定方法的访问。例如，在 Java 中，我们可以使用`java.lang.reflect.Proxy`类来创建一个保护代理，根据客户端的身份来决定是否允许调用某个方法。

另一个例子是**远程代理**（`Remote Proxy`）。在一个分布式系统中，一个对象可以存在于不同的地址空间（例如，不同的 JVM 或网络节点）。在这种情况下，我们可以使用代理模式来创建一个代表远程对象的本地对象，从而让客户端可以像访问本地对象一样访问远程对象。

### 关键区别

代理模式与装饰器模式最大的区别在于：
- **代理**控制对对象的访问，但它不会修改对象的方法调用。它可能会在访问前后添加一些逻辑，但最终会将对客户端的请求转发给真正的服务对象。代理常常用于延迟初始化、访问控制、日志记录等场景。
- **装饰器**则通过将客户端请求包装在装饰器对象中来动态地增强对象的功能。装饰器可以添加新的行为或修改现有行为，而无需改变原始对象的接口。

另一个区别在于对象的创建方式。在代理模式中，代理对象通常在编译时就确定了它与真实主题类的关系（如远程代理），或者通过工厂方法创建。而装饰器模式则是在运行时通过递归组合的方式构建的。

### 代码示例

最清晰的例子是当我们需要为一个对象提供访问控制时，可以使用一个保护代理。如果你有一些远程资源需要访问，使用代理模式总是有用的。与其直接设置 HTTP 请求对象，不如用一个类或对象包装它，这样可以在请求发送前提供一个类型为“加载中…”的消息给用户，而不是让用户面对一个空白页面。

一个远程代理（`Remote Proxy`）没有明确的使用场景，除非你有很多远程资源的方法需要访问。如果你的远程资源的所有方法都是通过网络调用的，那么一个远程代理就是**代理模式**最典型的例子。如果它只在运行时动态添加方法，那么它就是一个动态代理。

所以，问题应该是：“**我们何时应该使用代理模式？**”

最清晰的例子是当我们需要为一个对象提供访问控制时，可以使用一个保护代理。如果你有一些远程资源需要访问，使用代理模式总是有用的。与其直接设置 HTTP 请求对象，不如用一个类或对象包装它，这样可以在请求发送前提供一个类型为“加载中…”的消息给用户，而不是让用户面对一个空白页面。

一个远程代理（`Remote Proxy`）没有明确的使用场景，除非你有很多远程资源的方法需要访问。如果你的远程资源的所有方法都是通过网络调用的，那么一个远程代理就是代理模式最典型的例子。如果它只在运行时动态添加方法，那么它就是一个动态代理。

## rest - 页面统计概览

`rest` 是一个 `RESTful` 接口，用于访问从 `rest` 资源返回的统计信息。它是一个 `native` 的 `Java` 脚本，用于处理统计信息的访问。

一个 `rest` 资源是一个独立的端点，用于在统计信息上运行应用。

```
[rest] - rest api 统计
```

### 示例：页面统计

在此示例中，您将创建一个 `rest` 代理，用于封装一个提供页面统计的 Web 服务。该 Web 服务由一系列 `GET` 请求组成，每个请求都带有可选参数。它不依赖于特定的编程语言实现，无论 Web 服务使用何种语言，您在服务器端都会收到 `JSON` 格式的数据。

该 Web 服务提供了五种方法：

```
REST. \

GET API 端点

http://my.domain.com/stats/getPageviews/

http://my.domain.com/stats/getUniques/

http://my.domain.com/stats/getBrowsers/

http://my.domain.com/stats/getTopSearchTerms/

http://my.domain.com/stats/getMostVisitedPages/
```

每个方法都可以带一个可选参数，用于缩小统计数据的收集时间范围：`startDate` 和 `endDate`。前四个方法还可以指定您只想统计来自特定页面的数据。

您希望能够在您的网站上显示所有这些统计数据，但只在需要时才请求它们。

目前，您正在使用 `XML` 请求每个页面；传统做法通常会反复调用这些方法，您可以通过 JavaScript 调用 `XML`，并本地化时间窗口，从而从根本上消除代码中的重复操作，这些重复操作会导致服务器负载和不必要的调用。

### REST API 的使用

```
REST. \

使用 GET 请求

var callBack = function(responseText) {

var stats = eval(responseText); // 解析 JSON 数据

displayPageviews(stats.getPageviews); // 显示页面访问量

};

var urls = getUniques();

var success = function(){

var stats = eval(responseText); // 解析 JSON 数据
```
