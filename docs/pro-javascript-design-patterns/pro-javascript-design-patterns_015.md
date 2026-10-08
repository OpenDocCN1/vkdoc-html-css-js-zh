# 观察者模式

## 意图

**观察者模式**是一种行为设计模式，允许你定义一种订阅机制，可在对象事件发生时通知多个“观察”该对象的其他对象。

## 问题

在使用诸如 `MVC`（Model-View-Controller）之类的架构模式时，你通常需要让多个视图组件保持与模型状态同步。但这样做通常会导致模型与视图之间产生紧密耦合，使得代码难以维护和扩展。观察者模式提供了一种解决方案，使得一个对象（称为“主题”）可以维护一个依赖项列表，并在其状态发生变化时自动通知所有依赖项，通常是通过调用它们提供的某个方法来实现。

## 解决方案

观察者模式建议你为订阅机制添加一个“订阅者”接口，声明一个通知方法。所有观察者都必须实现这个接口。然后，主题可以存储一个观察者对象列表，并在需要时遍历并调用它们的通知方法。这样，主题就不再直接依赖于具体的观察者类，而是依赖于一个抽象的观察者接口，从而实现了松耦合。

## 真实世界类比

想象你订阅了一份杂志。当你订阅后，每当有新一期出版，邮递员就会将其送到你的手上。只要你不取消订阅，你就会持续收到新杂志。如果你取消了订阅，邮递员就不再给你送杂志了。观察者模式的工作原理与此类似：主题（杂志社）维护一个订阅者（读者）列表，并在状态变化时通知所有订阅者。

## 结构

观察者模式主要由以下几个部分组成：

### 1. 发布者

发布者会向其他对象发出感兴趣的事件。这些事件会在发布者自身状态改变或执行特定行为时发生。发布者包含一个订阅机制，允许新订阅者加入或现有订阅者退出列表。

### 2. 订阅者

订阅者接口声明了通知接口。在大多数情况下，该接口仅包含一个 `update` 方法，该方法可以接收多个参数，允许发布者在更新发生时传递一些事件细节。

### 3. 具体订阅者

具体订阅者可以执行一些操作来响应发布者发出的通知。所有具体订阅者类都必须实现订阅者接口，以便发布者能够统一调用它们的通知方法。

### 4. 客户端

客户端负责创建发布者和订阅者对象，然后将订阅者注册到发布者的订阅列表中。

## 适用性

- 当一个对象状态的改变需要改变其他对象，且实际的对象集是未知的或动态变化时，可以使用观察者模式。

- 当应用中的一些对象必须观察其他对象，但只能在有限时间内或特定情况下观察时，可以使用观察者模式。

## 如何实现

1. 仔细检查你的业务逻辑，尝试将其拆分为两部分：核心功能（独立于其他代码）与可选功能（其他代码可能依赖的核心行为）。

2. 声明观察者接口。该接口至少应声明一个 `update` 方法。

3. 声明发布者接口并描述一对方法：`attach`（添加订阅者）和 `detach`（移除订阅者）。记住，发布者必须通过订阅者接口与所有订阅者交互。

4. 决定存放订阅者列表的位置以及订阅机制的实现方式。通常，订阅机制会放在发布者类中，但如果你希望代码更具复用性，也可以将其放在一个独立的类中。

5. 创建具体发布者类。每次当发布者内部发生重要事件时，它必须通知所有订阅者。

6. 在具体订阅者类中实现 `update` 方法。大多数订阅者需要一些关于事件上下文的数据。这些数据可以作为 `update` 方法的参数传递。

7. 客户端必须创建所有必要的订阅者，并将它们注册给适当的发布者。

## 优缺点

### 优点

- **开闭原则**：你无需修改发布者代码就能引入新的订阅者类（反之亦然）。

- **可以在运行时建立对象之间的关系**：你可以在运行时动态地添加或移除订阅者。

### 缺点

- 订阅者的通知顺序是随机的，无法控制。

## 与其他模式的关系

- **责任链模式**、**命令模式**、**中介者模式**和**观察者模式**：这些模式用于处理请求的发送者和接收者之间的不同连接方式。

  - **责任链**：将请求动态地传递给一个潜在接收者链，直到其中一个处理请求。

  - **命令**：将请求封装为对象，并将其发送给接收者。

  - **中介者**：移除发送者和接收者之间的直接连接，强制它们通过一个中介对象进行通信。

  - **观察者**：允许接收者动态地订阅或取消订阅请求。

### 代码示例

以下代码示例基于[本书的配套网站](https://refactoring.guru/design-patterns/observer)和 [Press website](https://www.press.com) 上的 Java 实现。完整代码可以从本书的网站下载。

### 事件监听器接口

```java
public interface EventListener {
    void update(String eventType, Object data);
}
```

### 事件管理器（发布者）

```java
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

public class EventManager {
    private Map<String, List<EventListener>> listeners = new HashMap<>();

    public EventManager(String... operations) {
        for (String operation : operations) {
            this.listeners.put(operation, new ArrayList<>());
        }
    }

    public void subscribe(String eventType, EventListener listener) {
        List<EventListener> users = listeners.get(eventType);
        users.add(listener);
    }

    public void unsubscribe(String eventType, EventListener listener) {
        List<EventListener> users = listeners.get(eventType);
        users.remove(listener);
    }

    public void notify(String eventType, Object data) {
        List<EventListener> users = listeners.get(eventType);
        for (EventListener listener : users) {
            listener.update(eventType, data);
        }
    }
}
```

### 编辑器类（发布者）

```java
import java.io.File;

public class Editor {
    public EventManager events;
    private File file;

    public Editor() {
        this.events = new EventManager("open", "save");
    }

    public void openFile(String filePath) {
        this.file = new File(filePath);
        events.notify("open", file);
    }

    public void saveFile() throws Exception {
        if (this.file != null) {
            events.notify("save", file);
        } else {
            throw new Exception("Please open a file first.");
        }
    }
}
```

### 具体订阅者：日志记录器

```markdown
## 具体订阅者：日志监听器

```java
public class LoggingListener implements EventListener {
    private String log;
    private String message;

    public LoggingListener(String log, String message) {
        this.log = log;
        this.message = message;
    }

    @Override
    public void update(String eventType, Object data) {
        System.out.println("LoggingListener: " + message + " - " + data.toString());
        // 实际应用中，这里会写入日志文件
    }
}
```

## 具体订阅者：邮件通知器

```java
public class EmailNotificationListener implements EventListener {
    private String email;

    public EmailNotificationListener(String email) {
        this.email = email;
    }

    @Override
    public void update(String eventType, Object data) {
        System.out.println("EmailNotificationListener: Sending email to " + email + " regarding event: " + eventType);
        // 实际应用中，这里会发送邮件
    }
}
```

## 客户端代码

```java
public class Demo {
    public static void main(String[] args) {
        Editor editor = new Editor();

        LoggingListener logger = new LoggingListener(
            "/path/to/log.txt",
            "Someone has opened the file: %s"
        );
        editor.events.subscribe("open", logger);

        EmailNotificationListener emailNotifier = new EmailNotificationListener(
            "admin@example.com"
        );
        editor.events.subscribe("save", emailNotifier);

        try {
            editor.openFile("test.txt");
            editor.saveFile();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

## 输出示例

```
LoggingListener: Someone has opened the file: test.txt
EmailNotificationListener: Sending email to admin@example.com regarding event: save
```

---

## 下载完整代码

示例代码可以从本书的网站下载：

- 在 [`refactoring.guru/design-patterns/observer`](https://refactoring.guru/design-patterns/observer) 和 [Press website](https://www.press.com) 上查看在线版本。

- 在 [`www.press.com`](https://www.press.com) 上查看 Press 网站。

## 联系作者

你可以通过以下方式联系作者：

- 发送邮件至 `dustindesignpatterns@refactoring.guru` 或 `ross@refactoring.guru`

- 访问 [`www.press.com`](https://www.press.com)
```
