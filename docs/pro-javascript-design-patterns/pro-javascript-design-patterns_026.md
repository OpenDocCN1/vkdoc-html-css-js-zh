# 结论

正如您所见，其结构非常严格。每个方法都会收到一组精确的参数，并会在任何检查失败时抛出错误。这是有意为之，这样如果您收到无错误的结果，您就可以确信接口被正确地声明和实现了。

当您需要验证对象是否精确实现了特定接口时，可以使用 `Interface` 类。它看起来像是 Java 的 `@Override` 注解，但减少了灵活性，不过您的代码可以更松散地耦合，因为您可以传入任何类型的参数，并确保只有具有所需方法的对象才能被使用。

在大型项目中，当多个程序员编写代码时，接口是必不可少的。它们减少了代码的脆弱性，并通过允许您的对象解耦来真正提高灵活性。您可以更自由地编写函数，因为您可以传入任何类型，并且只使用需要的方法。这样可以更轻松地修改实现，而不会影响其他代码。存在一些少数情况，接口可能更适用。

在大型项目中，众多不同的程序员编写代码，接口是不可或缺的。它们可以降低代码的脆弱性，并通过让您的对象解耦来提高代码的灵活性。您的函数可以更灵活，因为您可以传入任何对象，并确保只有具有所需方法的对象才会被使用。这样，您就可以更轻松地修改实现，而不会影响其他代码。不过，在少数情况下，接口可能并非最佳选择。

在软件开发中，了解如何有效地使用 `Interface` 对象来创建可测试的架构至关重要。通常，`Interface` 对象用于将依赖项传递给类，以便在单元测试中可以轻松地用模拟实现替换它们。这样，您就可以编写更清晰、更可维护的代码，因为类不依赖于具体实现，而是依赖于抽象。

对于涉及多个依赖项的复杂场景，手动创建这些 `Interface` 对象可能会变得繁琐且容易出错。这就是依赖注入容器发挥作用的地方。手动依赖注入提供了一种更直接的控制方式，让您能够精确地了解应用程序中对象的创建方式，但它需要更多的手动工作。

另一方面，使用 `Interface` 对象可以提供更大的灵活性。您可以通过创建 `Interface` 对象来确保您的代码更加模块化。例如，您可能不直接控制您使用的第三方库，但可以通过为您没有直接控制的依赖项创建接口来确保您的代码更加健壮。

这对于需要支持第三方集成（如搜索引擎、电子邮件和地图）的应用程序来说是一个很好的方法。然而，这种方法也有其缺点。使用接口来确保您的代码能够适应变化是一个强大的技术，但如果您过度使用，可能会导致代码过于复杂。一个更好的方法是使用控制反转容器来管理这些依赖项。

`Interface` 对象可以用于在代码中创建更抽象和可测试的设计。对于实现接口的类，您可以使用 `newInstance()` 方法创建实例。

最后，这种方法的主要优势在于它使您的代码更加模块化。因为您不再依赖于具体的实现，所以您可以在运行时轻松地替换它们，而不需要修改代码的核心部分。

## 如何使用 `Interface` 类

最重要的一步，也是最难执行的一步，是确定是否值得在您的代码中使用 `Interface` 对象。小型和不太复杂项目可能不需要接口带来的额外复杂性。

1. 在您的项目中包含 `Interface` 类。`Interface.php` 文件可在书籍的网站上找到：`https://www.interfacephp.com/`
2. 遍历您代码中的方法，这些方法将对象作为参数。确定这些对象参数需要哪些方法才能让您的代码正常工作。
3. 为每一组所需的方法创建 `Interface` 对象。
4. 删除所有显式构造函数检查。因为我们使用的是动态类型输入，对象的类型不再重要。

就这样！您从这次经历中获得了什么？您的代码现在更加松散耦合了，因为您不再依赖于任何特定类。相反，您确保所需的功能都已就位；任何具体实现都可以使用，给您更多的自由度来优化和重构您的代码。

## 示例：使用 `Interface` 类

想象一下，您创建了一个类来处理一些自动化测试结果并将它们格式化为在网页上查看。这个类的构造函数接受一个 `TestResult` 类的实例作为参数。然后它将数据封装在 `TestResult` 对象中并输出它。以下是 `ResultFormatter` 类的初始样子：

```php
class ResultFormatter {
    public function __construct(TestResult $testResult) {
        $this->testResult = $testResult;
    }
    public function render(): string {
        $html = '<h1>Test Results</h1>';
        foreach ($this->testResult->getResults() as $result) {
            $class = $result->passed ? 'pass' : 'fail';
            $html .= "<div class=\"{$class}\">{$result->name}</div>";
        }
        return $html;
    }
}
```

这样做会强制您必须在代码的特定部分中使用 `TestResult` 类，这使得代码难以修改。如果您想使用不同的测试结果类，或者想要在不影响其他部分的情况下修改 `TestResult` 类的行为，怎么办？一个更好的解决方案是使用接口。

```php
interface TestResultInterface {
    public function getResults(): array;
}
```

然后，修改 `ResultFormatter` 类，使其依赖于接口而不是具体类：

```php
class ResultFormatter {
    public function __construct(TestResultInterface $testResult) {
        $this->testResult = $testResult;
    }
    // ... 其余代码保持不变
}
```

通过这种方式，`ResultFormatter` 类现在可以与任何实现了 `TestResultInterface` 接口的类一起工作。这使得您的代码更加灵活和可测试。

## 进一步的示例

您可能还想为其他依赖项创建接口，例如日志记录、缓存或配置管理。以下是一个更完整的示例，展示了如何为不同的功能创建接口：

```php
<?php
// interface/logger/LoggerInterface.php
interface LoggerInterface {
    public function log(string $message): void;
}

// interface/cache/CacheInterface.php
interface CacheInterface {
    public function get(string $key): mixed;
    public function set(string $key, mixed $value, int $ttl = 0): void;
}

// interface/config/ConfigInterface.php
interface ConfigInterface {
    public function get(string $key, mixed $default = null): mixed;
}
?>
```

然后，您可以在类中依赖这些接口，而不是具体实现：

```php
class UserService {
    public function __construct(
        private LoggerInterface $logger,
        private CacheInterface $cache,
        private ConfigInterface $config
    ) {}
    
    public function getUserById(int $id): ?User {
        $cacheKey = "user_{$id}";
        if ($cachedUser = $this->cache->get($cacheKey)) {
            $this->logger->log("User {$id} found in cache");
            return $cachedUser;
        }
        // ... 从数据库获取用户
    }
}
```

通过使用接口，您极大地提高了代码的模块化性和可测试性。您现在可以轻松地替换任何依赖项的实现，而无需修改 `UserService` 类本身。

```java
public class WeatherData {

    public WeatherData() {

    }

    public void measurementsChanged() {
        // code
    }

    // other methods...
}
```

```java
public class CurrentConditionsDisplay implements Observer, DisplayElement {
    private float temperature;
    private float humidity;
    private Subject weatherData;

    public CurrentConditionsDisplay(Subject weatherData) {
        this.weatherData = weatherData;
        weatherData.registerObserver(this);
    }

    public void update(float temperature, float humidity, float pressure) {
        this.temperature = temperature;
        this.humidity = humidity;
        display();
    }

    public void display() {
        System.out.println("Current conditions: " + temperature
            + "F degrees and " + humidity + "% humidity");
    }
}
```

You might think that simply calling getter methods in `WeatherData` would ensure the parameters are real-time and correct. However, doing so would require `WeatherData` to provide getter methods using a "pull" approach, whereas the "push" approach we used earlier seems more straightforward.

Using the "pull" approach in the `WeatherData` class requires implementing a getter method so observers can retrieve the data. This approach is feasible, but we need to add appropriate getter methods to the `WeatherData` class.

```java
public interface Subject {
    public void registerObserver(Observer o);
    public void removeObserver(Observer o);
    public void notifyObservers();
}
```

```java
public interface Observer {
    public void update(float temp, float humidity, float pressure);
}
```

However, observers might want to retrieve more data than just temperature, humidity, and pressure. For example, the `WeatherData` class might also contain methods like `getTemperature()`, `getHumidity()`, and `getPressure()`. This way, we can pass the subject object to observers and let them decide what data they need.

One approach is to, in addition to pushing specific data to observers, allow observers to pull more information as needed. By passing the subject object itself to observers, observers can use the subject's getter methods to retrieve any data they require.

The current issue is that observers need to know the subject object to call the `update()` method to get data. In the `Subject` interface, the `notifyObservers()` method can take the subject object as a parameter, allowing observers to fetch more data through that object.

While this approach is flexible, it also increases coupling because observers need to know the specific type of the subject to call its getter methods.

We aim to design a completely decoupled observer pattern, allowing any object that implements the `Observer` interface (regardless of what it looks like) to retrieve data from the subject. There are two ways to achieve data transfer: the "push" approach (the subject actively pushes data to observers) and the "pull" approach (observers pull data from the subject).

When implementing the observer pattern, we typically choose between "push" and "pull" based on specific requirements. If observers only care about a fixed set of data, the push approach is simpler; if observers might need various data, the pull approach is more flexible.

Now we have the `WeatherData` class and the `CurrentConditionsDisplay` class. Next, we also need other display boards, such as `StatisticsDisplay` and `ForecastDisplay`.

The main program can be launched like this:

```java
WeatherData weatherData = new WeatherData();
CurrentConditionsDisplay currentDisplay = new CurrentConditionsDisplay(weatherData);
StatisticsDisplay statisticsDisplay = new StatisticsDisplay(weatherData);
ForecastDisplay forecastDisplay = new ForecastDisplay(weatherData);

weatherData.setMeasurements(80, 65, 30.4f);
weatherData.setMeasurements(82, 70, 29.2f);
weatherData.setMeasurements(78, 90, 29.2f);
```

When `setMeasurements()` is called, all observers receive updates.

Now, let's look at the implementation of the `WeatherData` class:

## WeatherData 类的实现

```java
import java.util.*;

public class WeatherData implements Subject {
    private ArrayList observers;
    private float temperature;
    private float humidity;
    private float pressure;

    public WeatherData() {
        observers = new ArrayList();
    }

    public void registerObserver(Observer o) {
        observers.add(o);
    }

    public void removeObserver(Observer o) {
        int i = observers.indexOf(o);
        if (i >= 0) {
            observers.remove(i);
        }
    }

    public void notifyObservers() {
        for (int i = 0; i < observers.size(); i++) {
            Observer observer = (Observer)observers.get(i);
            observer.update(temperature, humidity, pressure);
        }
    }

    public void measurementsChanged() {
        notifyObservers();
    }

    public void setMeasurements(float temperature, float humidity, float pressure) {
        this.temperature = temperature;
        this.humidity = humidity;
        this.pressure = pressure;
        measurementsChanged();
    }

    // other methods of WeatherData...
}
```

## “拉取”与“推送”方式的讨论

现在我们来讨论“拉取”方式。我们之前使用的是“推送”方式：主题将数据作为参数传递给观察者。然而，观察者也可以从主题拉取数据。为了实现这一点，主题必须提供获取方法，而观察者需要知道主题对象。

在我们的实现中，`WeatherData` 已经有了获取方法，并且我们在构造函数中保存了主题（`weatherData`）。在 `update()` 方法中，我们使用传入的温度、湿度和压力数据，而不是从主题拉取。但如果我们想切换到拉取方式，就需要修改 `update()` 方法的签名，使其只接受主题对象作为参数，然后观察者在内部调用获取方法。

事实上，Java 的 `java.util.Observer` 接口同时支持推送和拉取方式。然而，在我们的示例中，我们更倾向于推送方式，因为它更简单。

## 观察者模式概述

观察者模式允许我们定义一种一对多的依赖关系，当一个对象（主题）的状态发生变化时，所有依赖于它的对象（观察者）都会被通知并自动更新。在气象站应用中，`WeatherData` 对象（主题）管理天气数据，而显示板（观察者）需要根据这些数据更新它们的显示。

这个问题的解决方案是将 `WeatherData` 和显示板解耦，使它们能够独立工作又协同配合。观察者模式为此提供了一个优雅的设计：`WeatherData` 作为主题，显示板作为观察者。当 `WeatherData` 的数据发生变化时，它会通知所有已注册的观察者，观察者随后根据接收到的数据更新它们的显示。

我们考虑的第一种实现方式是：`WeatherData` 类具有获取方法，显示板通过拉取数据来更新。然而，这种方式缺乏灵活性，因为每次添加新的显示板时，都需要修改 `WeatherData` 的代码。

我们需要 `WeatherData` 在数据变化时自动通知显示板，而不是让显示板主动拉取数据。这就是观察者模式的核心思想。

## 接口定义与实现

现在，我们开始实现观察者模式。首先，定义主题和观察者的接口：

```java
public interface Subject {
    public void registerObserver(Observer o);
    public void removeObserver(Observer o);
    public void notifyObservers();
}
```

```java
public interface Observer {
    public void update(float temp, float humidity, float pressure);
}
```

然后，`WeatherData` 实现 `Subject` 接口，并在数据变化时调用 `notifyObservers()`。每个显示板实现 `Observer` 接口，并在 `update()` 方法中更新自己的显示。

```java
public class WeatherData implements Subject {
    private List<Observer> observers;
    private float temperature;
    private float humidity;
    private float pressure;

    public WeatherData() {
        observers = new ArrayList<Observer>();
    }

    public void registerObserver(Observer o) {
        observers.add(o);
    }

    public void removeObserver(Observer o) {
        observers.remove(o);
    }

    public void notifyObservers() {
        for (Observer observer : observers) {
            observer.update(temperature, humidity, pressure);
        }
    }

    public void measurementsChanged() {
        notifyObservers();
    }

    public void setMeasurements(float temperature, float humidity, float pressure) {
        this.temperature = temperature;
        this.humidity = humidity;
        this.pressure = pressure;
        measurementsChanged();
    }
    // other methods of WeatherData...
}
```

现在，我们实现一个具体的显示板，例如 `CurrentConditionsDisplay`：

```java
public class CurrentConditionsDisplay implements Observer, DisplayElement {
    private float temperature;
    private float humidity;
    private Subject weatherData;

    public CurrentConditionsDisplay(Subject weatherData) {
        this.weatherData = weatherData;
        weatherData.registerObserver(this);
    }

    public void update(float temperature, float humidity, float pressure) {
        this.temperature = temperature;
        this.humidity = humidity;
        display();
    }

    public void display() {
        System.out.println("Current conditions: " + temperature
            + "F degrees and " + humidity + "% humidity");
    }
}
```

这个类同时实现了 `Observer` 和 `DisplayElement` 接口。在构造函数中，它向 `WeatherData` 注册了自身。当 `update()` 被调用时，它会保存数据并调用 `display()`。

现在，我们有了一个可以工作的气象站应用。未来可以轻松添加额外的显示板，只需实现 `Observer` 和 `DisplayElement` 接口，并向 `WeatherData` 注册即可。

这就是观察者模式的美妙之处：主题和观察者是松耦合的。主题只知道观察者实现了 `Observer` 接口，而观察者只知道主题实现了 `Subject` 接口。它们可以被独立地重用和修改。

通过使用观察者模式，`WeatherData` 和显示板实现了松耦合。`WeatherData` 通知所有已注册的观察者，而无需了解每个观察者的具体实现细节。每个观察者都实现相同的接口，因此它们可以互换。

现在，让我们总结一下观察者模式的关键点：

- `Subject` 维护一个观察者列表，并提供注册、移除和通知它们的方法。
- `Observer` 定义了一个更新接口，以便主题可以在状态变化时通知它们。
- `ConcreteSubject` 存储状态，并在状态变化时通知所有已注册的观察者。
- `ConcreteObserver` 实现更新接口，以接收来自主题的更新。

这种模式广泛应用于事件驱动系统中，例如 GUI 工具包和消息推送系统。在我们的气象站示例中，它完美地解决了将数据更新与显示解耦的问题。

### 核心概念：接口定义与实现分离

在编程中，我们经常听到“面向接口编程”的概念。这意味着我们不应该让代码直接依赖具体的实现类，而是依赖抽象接口。这样做的好处是能够提高代码的灵活性和可维护性。

`JavaScript` / `TypeScript` 是一种广泛使用的语言。我们展示了所有不同的面向接口概念的实现方式，它们都共享一组共同的特征——一种指定方法的方式，以及一种检查这些方法是否确实实现的方式。

我们能够通过组合文档注释、一个帮助类文件和类型系统来模拟这些特性。我们也有机会利用这些帮助类来创建更健壮、更稳定的代码。

## 封装与信息隐藏

创建私有成员是面向对象语言中最基础、最有用的特性之一。声明一个方法或属性为私有允许你隐藏实现细节，并促进与其他对象的松散耦合。它允许你保持数据的完整性和对数据进行修改的约束。

它也使你的代码更可靠、更易于调试，因为它限制了其他组件可以修改你的数据的方式。

### 信息隐藏原则

让我们用一个例子来说明信息隐藏原则。每一天，你都会从同事那里收到一份关于当天收入的报告。这是一个定义良好的接口——你请求信息，你的同事找到原始数据，计算收入，然后向你报告。如果同事或同事的公司发生了变化，接口仍然是相同的，确保你可以通过替换请求信息的方式来轻松获取信息。

有一天，你决定比你的同事更频繁地收到这些信息。你发现原始数据存储在你的计算机上，你不能再依赖你的同事为你进行计算。

你已经变得依赖于内部实现——当实现改变时，你必须重新学习整个系统并重新开始。在面向对象设计中，你已经变得紧密耦合到原始数据上。

信息隐藏原则服务于封装。不是分散的值文件，现在信息格式化为 JSON。此外，计算可以根据会计和税法而变化，你没有专业知识。如果你离职，你必须首先培训你的继任者来执行这些相同的任务，这些任务更复杂，而不仅仅是请求最终计算。

## PEPRINCIPLE ON INFORMATION HIDING

`Interface`是用于定义类型之间交互的契约。它指定了一组方法，这些方法必须在实现该`interface`的类中实现，但不提供这些方法的具体实现。

与抽象类不同，`interface`不能包含任何状态或具体方法。它只定义了一个类必须实现的方法签名，以确保不同类之间的一致性。

在`Java`中，`interface`是通过使用`private`关键字来将方法与数据表示分离开来的核心概念之一。在`Java`中，`interface`的所有成员都是`public`的，并且所有方法都是`abstract`的。

## `Encapsulation` 与 `Information Hiding`

`Encapsulation`是将数据和行为打包到一个单元中的机制，而`Information Hiding`则是隐藏实现细节并仅暴露接口的做法。`Encapsulation`是实现`Information Hiding`的一种技术，但反之则不然。`Information Hiding`是目标，而`Encapsulation`是实现该目标的技术手段。

在`Java`中，`Encapsulation`是通过使用`private`关键字来实现的，将类的字段声明为`private`，然后提供`public`的`getter`和`setter`方法来访问和修改这些字段。通过使用`Encapsulation`，你可以强制执行数据隐藏，并保护对象的状态不被外部代码意外或恶意修改。

许多面向对象的语言使用`keywords`来指定方法和属性应该隐藏。在`Java`中，对于实例，添加`private keyword`到一个方法将会确保只有代码在同一个类内部可以执行它。由于`JavaScript`中没有这样的关键字，因此我们将使用`closure`的概念来创建只能从对象内部访问的方法和属性。

## `接口的作用`

`Interface`通过提供`public`可访问的方法来帮助您隐藏来自其他对象的内部信息。它定义了文档的契约，规定了两个对象之间的关系：一个对象可以被替换为另一个对象，只要它的`interface`保持不变。虽然并不总是需要使用像我们在`Chapter 1`中定义的那样的`strict interface`，但大多数时候您会发现它非常有用，可以帮助您拥有可用的文档方法。

即使某处存在一个已知的`interface`，暴露该`interface`中未定义的方法也十分重要。反之，这可能是危险的，因为其他对象可能会依赖于这些不在`interface`中的方法。它们可能被改变或移除，导致整个系统崩溃。

理想的软件系统会为所有类定义`interface`s。这些类将只提供在其`interface`s 中定义的方法；任何其他方法将被保持为`private`。所有`attribute`s 将保持为`private`并且只可通过`accessor`和`mutator`操作来访问，如`interface`中所定义。只有在现实世界中，系统才拥有所有这些特性。好代码应该朝着这些设计目标不断靠近，但不要以增加不必要的复杂性为代价。

## 基本模式

在本节中，我们将探讨在创建一个对象时可以使用的各种模式以及每种模式可用的特性。有三种基本模式可以用来创建对象。完全暴露的对象是最简单的，但只提供`public members`。下一个模式通过使用`underscores`来标记方法和属性来表示意图为私有。第三个模式使用`closures`来创建真正的私有成员，这只能通过`privileged`方法访问。

```
BOOKSHELF
EXAMPLE  ON  ENCAPSULATION  AND  INFORMATION  HIDING
```

> **注** 没有所谓的“单一正确”模式。在定义一个类时，每种模式都有其自身的优点和缺点。取决于您的需求，任何一种模式都可能适合您。

我们将使用`Book`类作为我们的示例。您将收到这个任务：创建一个类来存储关于一本书的数据，并实现一个用于在`HTML`中显示该书数据的方法。您只需创建这个类；其他程序员将负责实例化它。下面是一个如何使用它的示例：

```
new Book("白鲸", "赫尔曼·梅尔维尔", 1851)
```

```
Book.title, author, yearPublished
```

## `完全暴露的对象`

实现`Book`最简单的方式就是以常规方式创建一个类，使用一个构造函数，然后使用一个函数来计算并填充一个`HTML`元素。`完全暴露的对象`的变量和方法都是`public`的，可以被任何人通过`accessor`和`mutator`操作访问。

# JavaScript

## Book.prototype.display

`Book.prototype.display` 是一个函数，用于显示本书的标题、作者和 ISBN。它将以 `String` 形式返回这三个属性的值。此外，它还会检查所有属性是否已定义。如果未定义，则会抛出一个新的 `Error("Book: invalid ISBN")`。

-   如果 `Book` 是一个有效的 ISBN，则返回一个字符串。

-   如果 `Book` 是一个无效的 ISBN，则抛出一个新的 `Error`。

`this.isbn` 是一个字符串，包含正确的 ISBN 数字和校验和。

## Book.display

`Book.display` 是一个静态函数，它需要传递一个字符串参数。如果该字符串的长度等于 13 并且以数字开头，则返回 `true`，否则返回 `false`。

```
function Book.display(isbn) {
    return isbn.length === 13 && /^[0-9]/.test(isbn);
}
```

---

## CheckIsbn

`CheckIsbn` 函数用于验证 ISBN。如果传递的 `isbn` 是一个字符串，并且长度等于 13，并且所有字符都是数字，则返回 `true`，否则返回 `false`。

`isbn` 是一个字符串，可以包含数字和字母，但通常只有数字是有效的。

`Book.prototype.display` 函数在检查 ISBN 时，如果 ISBN 无效，会抛出一个错误。如果 `Book` 对象没有定义 `title` 或 `author` 属性，则 `display` 方法会返回一个错误。

## 问题

最大的问题是验证数据的完整性和正确性。在构造 ISBN 时，我们需要确保其格式正确，并且校验和匹配。

## 解决方案

使用 `CheckIsbn` 函数来验证 ISBN 的有效性。该函数会检查字符串的长度和字符是否为数字。如果验证失败，则抛出一个 `Error("invalid ISBN")`。

`CheckIsbn` 函数还可以检查 ISBN 的前缀。例如，对于 `978` 前缀，它需要 13 个字符。如果不匹配，则返回 `false`。

## 示例

```
for (var i = 0; i < 10; i++) {
    sum += CheckIsbn(isbn.charAt(i) * (10 - i));
}
if (CheckIsbn(isbn) && display(match, "978") && ensure characters are digits) {
    return true;
}
else {
    return false;
}
```

---

## 另一个例子

```
if (this.isbn.length === 13 && this.isbn.match(/^[0-9]{13}$/)) {
    return isbn;
}
else {
    throw new Error("invalid ISBN");
}
```

## 更多示例

```
for (var i = 0; i < 10; i++) {
    sum += isbn.charAt(i) * (10 - i);
}
if (sum % 11 === 0) {
    return true;
}
else {
    return false;
}
```

---

```
for (var i = 0; i < 13; i++) {
    sum += isbn.charAt(i) * (i % 2 === 0 ? 1 : 3);
}
if (sum % 10 === 0) {
    return true;
}
else {
    return false;
}
```

---

```
return true;
// All tests passed.
```

---

# 结论

通过使用 `CheckIsbn` 方法，我们能够确保 ISBN 是有效的字符串，包含正确数量的数字和正确的校验和。这样，我们就可以在构造函数中使用该方法来验证 ISBN，并抛出错误以保护内部数据。我们还可以创建访问器和修改器方法来获取和设置每个属性的值。访问器方法通常被命名为 `getAttributeName`，修改器方法被命名为 `setAttributeName`，用于设置属性的值。

## 最终解决方案

`this.isbn` 被设置为 `isbn.replace(/[⁰-9]/g, "")`。如果 `this.isbn` 的长度不等于 13，或者 `this.isbn.match(/^[0-9]{13}$/)` 不匹配，则抛出新的 `Error("invalid ISBN")`。否则，继续执行构造函数。

`isbn` 被设置为 `isbn.replace(/[⁰-9]/g, "")`。如果 `isbn.length` 不等于 13，则抛出新的 `Error("invalid ISBN")`。如果 `isbn.match(/^[0-9]{13}$/)` 不匹配，则抛出新的 `Error("invalid ISBN")`。否则，返回 `isbn`。

---

# 更好的解决方案

我们添加了一个 `CheckIsbn` 方法，以确保 ISBN 是一个有效字符串，具有正确数量的数字和正确的校验和。现在有两种方法可以实现这一点：`Book.prototype.display` 是设置为一个对象字面量，用于定义多个方法，而不必启动每个方法。两者都是完全相同的定义方式，并且我们互换使用两者。这似乎是一个改进。你现在能够验证 ISBN 是否有效，当对象被创建时，因此确保 `display` 方法会成功。但是，一个问题是，另一个程序员可能会注意到一本书可能有多个版本，每个都有自己的 ISBN。我们创建了一个算法，用于在这些不同版本之间进行选择，并使用它来直接更改 `isbn` 属性，在实例化对象之后。`Book` 对象的 `isbn` 属性可以被任何其他程序员直接修改，你无法控制其他程序员会分配给 `isbn` 属性什么。为了... 保护内部数据，你可以创建访问器和修改器方法来获取和设置每个属性的值。访问器方法通常以 `getAttributeName` 的形式命名，修改器方法通常以 `setAttributeName` 的形式命名，用于设置属性的值。

```
var Book = function(isbn, title, author) {
    if (CheckIsbn(isbn)) {
        this.isbn = isbn;
    } else {
        throw new Error("invalid ISBN");
    }
    this.title = title;
    this.author = author;
}
```

`isbn` 被设置为 `isbn.replace(/[⁰-9]/g, "")`。如果 `isbn` 的长度不等于 13，则抛出新的 `Error("invalid ISBN")`。否则，返回 `isbn`。

```
var sum;
for (var i = 0; i < 10; i++) {
    sum += isbn.charAt(i) * (10 - i);
}
if (CheckIsbn(isbn) && display(match, "978") && ensure characters are digits with CheckIsbn) {
    return true;
}
else {
    return false;
}
```

```
var sum;
for (var i = 0; i < 10; i++) {
    sum += isbn.charAt(i) * (10 - i);
}
if (CheckIsbn(isbn) && display(match, "978") && ensure characters are digits) {
    return true;
}
else {
    return false;
}
```

```
for (var i = 0; i < 10; i++) {
    sum += CheckIsbn(isbn.charAt(i) * (10 - i));
}
if (CheckIsbn(isbn) && display(match, "978") && ensure characters are digits) {
    return true;
}
else {
    return false;
}
```

---

# 另一个示例

```
if (this.isbn.length === 13 && this.isbn.match(/^[0-9]{13}$/)) {
    return isbn;
}
else {
    throw new Error("invalid ISBN");
}
```

## 更多示例

```
for (var i = 0; i < 10; i++) {
    sum += isbn.charAt(i) * (10 - i);
}
if (sum % 11 === 0) {
    return true;
}
else {
    return false;
}
```

---

```
for (var i = 0; i < 13; i++) {
    sum += isbn.charAt(i) * (i % 2 === 0 ? 1 : 3);
}
if (sum % 10 === 0) {
    return true;
}
else {
    return false;
}
```

---

```
return true;
// All tests passed.
```

# 结论

通过使用 `CheckIsbn` 方法，我们能够确保 ISBN 是有效的字符串，包含正确数量的数字和正确的校验和。这样，我们就可以在构造函数中使用该方法来验证 ISBN，并抛出错误以保护内部数据。我们还可以创建访问器和修改器方法来获取和设置每个属性的值。访问器方法通常被命名为 `getAttributeName`，修改器方法被命名为 `setAttributeName`，用于设置属性的值。

## App 架构：为视图添加属性

当前的视图层次结构包含一个用于运行验证逻辑生命周期方法的 View。

App 将添加一个名为 `addAttributes(to:)` 的新方法，并将其作为 `viewDidAppear(_:)` 的一部分进行调用。

### 针对 Book 协议

以下是对 `Book` 协议的新增内容：

```swift
func display() -> String
```

```swift
mutating func display() -> String
```

```swift
mutating let display: String
```

```swift
mutating let title: String
```

```swift
mutating func display() -> String
```

```
return display
```

```swift
mutating func display() -> String
```

```
return title
```

```swift
var display: String
```

```
return self?.title ?? "No title"
```

```swift
var display: String
```

```
return title ?? "No title"
```

```swift
var display: String
```

```
self?.title = title ?? "default title"
```

```swift
var display: String
```

```
return self?.title ?? "No title"
```

### 针对 Author 协议

以下是对 `Author` 协议的新增内容：

```swift
func display() -> String
```

```
return author ?? "No author"
```

```swift
mutating func display() -> String
```

```
return author ?? "No author"
```

```swift
var display: String
```

```
return author ?? "No author"
```

```swift
mutating let display: String
```

```
return author ?? "No author"
```

```swift
var display: String
```

```
return author ?? "No author"
```

### 重要注意事项

请注意，一个接口在定义后，其他程序员不应只通过编程方式与其交互。从现在起，他们应该只使用接口中定义的那些方法。

不应在构造函数中使用这些方法。目前没有实现相同的验证方法，因此你应依赖这些内部方法。

这与完全暴露的对象模式一样好，因为它能让你获得完整的访问权限。你拥有一个定义良好的接口，访问器和 mutator 方法用于保护数据，以及验证方法。尽管拥有所有这些特性，设计中仍然存在一个漏洞。

即使我们提供了用于设置属性（attribute）的 mutator 方法，这些属性仍然是公开的，并且可以直接设置。对于此模式，没有办法防止这种情况。它有可能通过一个不知道如何设置它的程序员（通过直接设置）或一个知道它但忽略它的程序员意外设置一个无效的 IP。

尽管有这些缺陷，此模式仍然保留了许多好处。它易于使用，并且方便新的 Java 程序员快速上手。它并不需要深入理解作用域或调用链来创建像这样的类。子类化非常容易，因为所有方法和属性都是公开可用的。唯一的缺点是你无法保护内部数据，并且访问器和 mutator 方法添加了并非严格需要的额外代码。

这在文件（`File`）大小很重要的情况下可能是个问题。

### 使用命名约定定义私有方法

接下来，我们将研究一种模拟私有方法的模式。此模式解决了上一节中遇到的一个问题，即防止其他程序员意外地通过传递所有验证。这与完全暴露的对象本质上是相同的，但使用了下划线作为方法名的前缀，以及你希望保持私有的属性和方法名。

### 针对 Book 协议

以下是对 `Book` 协议的新增内容：

```swift
func display() -> String
```

```
return _display
```

```swift
mutating func display() -> String
```

```
if (!check_display()) { throw BookError.invalidIP }
_display = display
```

```swift
var display: String
```

```
return _display
```

```swift
mutating let display: String
```

```
_display = display ?? "default title"
```

```swift
func display() -> String
```

```
return _author
```

```swift
mutating let display: String
```

```
_author = author ?? "default author"
```

```swift
var display: String
```

```
return display
```

在这个例子中，所有的属性都被重命名了。在每一个的开始处添加了下划线，以表明它是私有的。这对于 Java 来说仍然是一个有效的变量名，因为下划线是首选的第一个合法字符。这种命名约定可以应用于方法以及属性。让我们认为一个程序员不应该直接设置它。

这仍然是合法的变量名，因为下划线是首选的第一个字符。该命名约定可以应用于方法以及属性。

此方法仍然是一个有效的变量名。

## 私有方法与隐藏内部数据

在 Java 中，私有方法（`private`）是面向对象编程中用于隐藏实现细节的一种机制。它允许类内部的方法相互调用，而无需暴露这些方法给外部代码。

### 核心观点

私有方法的主要目的是隐藏实现细节，防止外部代码直接访问这些方法。这对于创建稳定的公共接口非常有用。

### 私有方法的作用

私有方法主要用于支持公共方法的实现。它们通常用于分解复杂的公共方法，将重复的代码提取到单独的方法中，以减少代码冗余并提高可维护性。

#### 1. 代码复用（DRY 原则）

将重复的逻辑封装到私有方法中，可以在类内部多个位置调用，避免代码重复。

#### 2. 隐藏内部实现

私有方法隐藏了类的内部实现细节，外部代码只能通过公共接口访问类的功能。这有助于降低耦合度，提高代码的模块化程度。

### 使用场景

私有方法特别适用于以下场景：
- 公共方法需要执行多个步骤，其中一些步骤可以抽象为独立的私有方法。
- 需要避免在多个公共方法中重复相同的代码逻辑。

### 作用域与访问控制

私有方法只能在定义它们的类内部访问。外部代码无法直接调用私有方法，这有助于防止外部代码意外修改类的内部状态。

### 示例：作用域演示

在 JavaScript 中，函数拥有作用域（`scope`）。在函数内部定义的变量和函数只能在该函数内部访问。

```javascript
function foo() {
  var a = 10;
  function bar() {
    return a;
  }
}
```

在这个例子中，`bar` 是在 `foo` 内部定义的，但 `bar` 也可以访问 `a`，因为 `bar` 也定义在 `foo` 内部。

当 `bar` 被执行时，它返回 `a`。这使得 `bar` 可以访问 `a`，但如果尝试在 `foo` 外部执行 `bar`，则会报错，因为 `bar` 的作用域仅限于 `foo` 内部。

```javascript
function foo() {
  var a = 10;
  function bar() {
    return a;
  }
  return bar;
}
var baz = foo();
baz(); // 返回 10
```
