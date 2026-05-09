# 结论

正如您所见，其结构非常严格。每个方法都会收到一组精确的参数，并会在任何检查失败时抛出错误。这是有意为之，这样如果您收到无错误的结果，您就可以确信接口被正确地声明和实现了。

当您需要验证对象是否精确实现了特定接口时，可以使用 `Interface` 类。它看起来像是 Java 的 `@Override` 注解，但减少了灵活性，不过您的代码可以更松散地耦合，因为您可以传入任何类型的参数，并确保只有具有所需方法的对象才能被使用。

在大型项目中，当多个程序员编写代码时，接口是必不可少的。它们减少了代码的脆弱性，并通过允许您的对象解耦来真正提高灵活性。您可以更自由地编写函数，因为您可以传入任何类型，并且只使用需要的方法。这样可以更轻松地修改实现，而不会影响其他代码。存在一些少数情况，接口可能更适用。

在大型项目中，众多不同的程序员编写代码，接口是不可或缺的。它们可以降低代码的脆弱性，并通过让您的对象解耦来提高代码的灵活性。您的函数可以更灵活，因为您可以传入任何对象，并确保只有具有所需方法的对象才会被使用。这样，您就可以更轻松地修改实现，而不会影响其他代码。不过，在少数情况下，接口可能并非最佳选择。




在软件开发中，了解如何有效地使用`Interface`对象来创建可测试的架构至关重要。通常，`Interface`对象用于将依赖项传递给类，以便在单元测试中可以轻松地用模拟实现替换它们。这样，您就可以编写更清晰、更可维护的代码，因为类不依赖于具体实现，而是依赖于抽象。

对于涉及多个依赖项的复杂场景，手动创建这些`Interface`对象可能会变得繁琐且容易出错。这就是依赖注入容器发挥作用的地方。手动依赖注入提供了一种更直接的控制方式，让您能够精确地了解应用程序中对象的创建方式，但它需要更多的手动工作。

另一方面，使用`Interface`对象可以提供更大的灵活性。您可以通过创建`Interface`对象来确保您的代码更加模块化。例如，您可能不直接控制您使用的第三方库，但可以通过为您没有直接控制的依赖项创建接口来确保您的代码更加健壮。

这对于需要支持第三方集成（如搜索引擎、电子邮件和地图）的应用程序来说是一个很好的方法。然而，这种方法也有其缺点。使用接口来确保您的代码能够适应变化是一个强大的技术，但如果您过度使用，可能会导致代码过于复杂。一个更好的方法是使用控制反转容器来管理这些依赖项。

`Interface`对象可以用于在代码中创建更抽象和可测试的设计。对于实现接口的类，您可以使用`newInstance()`方法创建实例。

最后，这种方法的主要优势在于它使您的代码更加模块化。因为您不再依赖于具体的实现，所以您可以在运行时轻松地替换它们，而不需要修改代码的核心部分。

## 如何使用`Interface`类

最重要的一步，也是最难执行的一步，是确定是否值得在您的代码中使用`Interface`对象。小型和不太复杂项目可能不需要接口带来的额外复杂性。

1. 在您的项目中包含`Interface`类。`Interface.php`文件可在书籍的网站上找到：`https://www.interfacephp.com/`

2. 遍历您代码中的方法，这些方法将对象作为参数。确定这些对象参数需要哪些方法才能让您的代码正常工作。

3. 为每一组所需的方法创建`Interface`对象。

4. 删除所有显式构造函数检查。因为我们使用的是动态类型输入，对象的类型不再重要。

就这样！您从这次经历中获得了什么？您的代码现在更加松散耦合了，因为您不再依赖于任何特定类。相反，您确保所需的功能都已就位；任何具体实现都可以使用，给您更多的自由度来优化和重构您的代码。

## 示例：使用`Interface`类

想象一下，您创建了一个类来处理一些自动化测试结果并将它们格式化为在网页上查看。这个类的构造函数接受一个`TestResult`类的实例作为参数。然后它将数据封装在`TestResult`对象中并输出它。以下是`ResultFormatter`类的初始样子：

```
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

这样做会强制您必须在代码的特定部分中使用`TestResult`类，这使得代码难以修改。如果您想使用不同的测试结果类，或者想要在不影响其他部分的情况下修改`TestResult`类的行为，怎么办？一个更好的解决方案是使用接口。

```
interface TestResultInterface {
    public function getResults(): array;
}
```

然后，修改`ResultFormatter`类，使其依赖于接口而不是具体类：

```
class ResultFormatter {
    public function __construct(TestResultInterface $testResult) {
        $this->testResult = $testResult;
    }
    // ... 其余代码保持不变
}
```

通过这种方式，`ResultFormatter`类现在可以与任何实现了`TestResultInterface`接口的类一起工作。这使得您的代码更加灵活和可测试。

## 进一步的示例

您可能还想为其他依赖项创建接口，例如日志记录、缓存或配置管理。以下是一个更完整的示例，展示了如何为不同的功能创建接口：

```
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

```
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

通过使用接口，您极大地提高了代码的模块化性和可测试性。您现在可以轻松地替换任何依赖项的实现，而无需修改`UserService`类本身。



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

Using the "pull" approach in the `WeatherData` class requires implementing a getter method so observers can retrieve the data.

This approach is feasible, but we need to add appropriate getter methods to the `WeatherData` class.

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

```
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

Now let's discuss the "pull" approach. We previously used the "push" approach: the subject passes data as parameters to observers. However, observers can also pull data from the subject. To achieve this, the subject must provide getter methods, and observers need to know the subject object.

In our implementation, `WeatherData` already has getter methods, and we saved the subject (`weatherData`) in the constructor. In the `update()` method, we use the passed temperature, humidity, and pressure data instead of pulling from the subject. But if we wanted to switch to the pull approach, we would need to modify the signature of the `update()` method to only take the subject object as a parameter, and then observers would call getter methods internally.

In fact, Java's `java.util.Observer` interface supports both push and pull approaches simultaneously. However, in our example, we prefer the push approach because it's simpler.

The observer pattern allows us to define a one-to-many dependency relationship, where when the state of one object (the subject) changes, all its dependents (observers) are notified and updated automatically. In the weather station application, the `WeatherData` object (the subject) manages weather data, while the display boards (observers) need to update their displays based on this data.

The solution to this problem is to decouple `WeatherData` and the display boards, allowing them to be independent yet work together. The observer pattern offers an elegant design for this: `WeatherData` serves as the subject, and the display boards serve as observers. When `WeatherData`'s data changes, it notifies all registered observers, who then update their displays based on the received data.

The first implementation we considered is: the `WeatherData` class has getter methods, and the display boards update by pulling data. However, this approach lacks flexibility, as adding new display boards would require modifying `WeatherData`'s code each time.

We need `WeatherData` to automatically notify display boards when data changes, rather than having display boards actively pull data. This is the core idea of the observer pattern.

Now, we begin implementing the observer pattern. First, define the interfaces for subject and observer:

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

Then, `WeatherData` implements the `Subject` interface and calls `notifyObservers()` when data changes. Each display board implements the `Observer` interface and updates its own display in the `update()` method.

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

Now, we implement a concrete display board, for example, `CurrentConditionsDisplay`:

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

This class implements both `Observer` and `DisplayElement` interfaces. In the constructor, it registers itself with `WeatherData`. When `update()` is called, it saves the data and calls `display()`.

Now, we have a working weather station application. Additional display boards can be easily added in the future by simply implementing the `Observer` and `DisplayElement` interfaces and registering them with `WeatherData`.

This is the beauty of the observer pattern: the subject and observers are loosely coupled. The subject only knows that observers implement the `Observer` interface, and observers only know that the subject implements the `Subject` interface. They can be independently reused and modified.

By using the observer pattern, `WeatherData` and the display boards achieve loose coupling. `WeatherData` notifies all registered observers without needing to know the specific implementation details of each. Every observer implements the same interface, so they can be interchanged.

Now, let's summarize the key points of the observer pattern:

- The Subject maintains a list of observers and provides methods for registering, removing, and notifying them.
- The Observer defines an update interface so that the subject can notify them of state changes.
- ConcreteSubject stores state and notifies all registered observers when its state changes.
- ConcreteObserver implements the update interface to receive updates from the subject.

This pattern is widely used in event-driven systems, such as GUI toolkits and message push systems. In our weather station example, it perfectly solves the problem of decoupling data updates from displays.

好的，作为高级文档工程师和翻译员，我已经根据您提供的注意事项和示例，对您给出的文本进行了处理。


`` `` `` `` `` `` `` `` `` `` `` `` `` `` `` `` `` `` `` `` `` `` `` ``

`` `` `` `` `` `` `` `` `` `` `` `` `` `` `` `` `` `` `` ``

`` `` `` `` `` `` `` `` `` `` `` `` `` `` `` `` `` `` `` `` `` `` `` ``

`` `` `` `` `` `` `` `` `` `` `` `` `` `` `` `` `` `` `` `` `` `` `` `` `` `` `` `` `` `` `` `` `` `` `` `` `` `` `` `` `` `` `` `` ``

`` `` `` `` `` `` `` `` `` `` `` `` `` `` `` `` `` ``

### 核心概念：接口定义与实现分离

在编程中，我们经常听到“面向接口编程”的概念。这意味着我们不应该让代码直接依赖具体的实现类，而是依赖抽象接口。这样做的好处是能够提高代码的灵活性和可维护性。

`JavaScript` / `TypeScript` 是一种广泛使用的语言。我们展示了所有不同的面向接口概念的实现方式，它们都共享一组共同的特征——一种指定方法的方式，以及一种检查这些方法是否确实实现的方式。

我们能够通过组合文档注释、一个帮助类文件和类型系统来模拟这些特性。我们也有机会利用这些帮助类来创建更健壮、更稳定的代码。

**封装与信息隐藏**

创建私有成员是面向对象语言中最基础、最有用的特性之一。声明一个方法或属性为私有允许你隐藏实现细节，并促进与其他对象的松散耦合。它允许你保持数据的完整性和对数据进行修改的约束。

它也使你的代码更可靠、更易于调试，因为它限制了其他组件可以修改你的数据的方式。

### 信息隐藏原则

让我们用一个例子来说明信息隐藏原则。每一天，你都会从同事那里收到一份关于当天收入的报告。这是一个定义良好的接口——你请求信息，你的同事找到原始数据，计算收入，然后向你报告。如果同事或同事的公司发生了变化，接口仍然是相同的，确保你可以通过替换请求信息的方式来轻松获取信息。

有一天，你决定比你的同事更频繁地收到这些信息。你发现原始数据存储在你的计算机上，你不能再依赖你的同事为你进行计算。

你已经变得依赖于内部实现——当实现改变时，你必须重新学习整个系统并重新开始。在面向对象设计中，你已经变得紧密耦合到原始数据上。

信息隐藏原则服务于封装。不是分散的值文件，现在信息格式化为 JSON。此外，计算可以根据会计和税法而变化，你没有专业知识。如果你离职，你必须首先培训你的继任者来执行这些相同的任务，这些任务更复杂，而不仅仅是请求最终计算。

你已经变得依赖于内部实现——当实现改变时，你必须重新学习整个系统并重新开始。在面向对象设计中，你已经变得紧密耦合到原始数据上。

**PE** `**` `PB`

**PEPRINCIPLE ON INFORMATION HIDING**



`Interface`是用于定义类型之间交互的契约。它指定了一组方法，这些方法必须在实现该`interface`的类中实现，但不提供这些方法的具体实现。

与抽象类不同，`interface`不能包含任何状态或具体方法。它只定义了一个类必须实现的方法签名，以确保不同类之间的一致性。

在`Java`中，`interface`是通过使用`private`关键字来将方法与数据表示分离开来的核心概念之一。在`Java`中，`interface`的所有成员都是`public`的，并且所有方法都是`abstract`的。

`Encapsulation` 与 `Information Hiding`

`Encapsulation`是将数据和行为打包到一个单元中的机制，而`Information Hiding`则是隐藏实现细节并仅暴露接口的做法。`Encapsulation`是实现`Information Hiding`的一种技术，但反之则不然。`Information Hiding`是目标，而`Encapsulation`是实现该目标的技术手段。

在`Java`中，`Encapsulation`是通过使用`private`关键字来实现的，将类的字段声明为`private`，然后提供`public`的`getter`和`setter`方法来访问和修改这些字段。通过使用`Encapsulation`，你可以强制执行数据隐藏，并保护对象的状态不被外部代码意外或恶意修改。

许多面向对象的语言使用`keywords`来指定方法和属性应该隐藏。在`Java`中，对于实例，添加`private keyword`到一个方法将会确保只有代码在同一个类内部可以执行它。由于`JavaScript`中没有这样的关键字，因此我们将使用`closure`的概念来创建只能从对象内部访问的方法和属性。

`接口的作用`

`Interface`通过提供`public`可访问的方法来帮助您隐藏来自其他对象的内部信息。它定义了文档的契约，规定了两个对象之间的关系：一个对象可以被替换为另一个对象，只要它的`interface`保持不变。虽然并不总是需要使用像我们在`Chapter 1`中定义的那样的`strict interface`，但大多数时候您会发现它非常有用，可以帮助您拥有可用的文档方法。

即使某处存在一个已知的`interface`，暴露该`interface`中未定义的方法也十分重要。反之，这可能是危险的，因为其他对象可能会依赖于这些不在`interface`中的方法。它们可能被改变或移除，导致整个系统崩溃。

理想的软件系统会为所有类定义`interface`s。这些类将只提供在其`interface`s 中定义的方法；任何其他方法将被保持为`private`。所有`attribute`s 将保持为`private`并且只可通过`accessor`和`mutator`操作来访问，如`interface`中所定义。只有在现实世界中，系统才拥有所有这些特性。好代码应该朝着这些设计目标不断靠近，但不要以增加不必要的复杂性为代价。

基本模式

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

`完全暴露的对象`

实现`Book`最简单的方式就是以常规方式创建一个类，使用一个构造函数，然后使用一个函数来计算并填充一个`HTML`元素。`完全暴露的对象`的变量和方法都是`public`的，可以被任何人通过`accessor`和`mutator`操作访问。



