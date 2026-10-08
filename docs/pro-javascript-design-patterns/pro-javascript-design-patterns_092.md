# 代理模式

代理模式用于控制对某个对象的访问。代理对象实现与真实对象相同的接口，并可用来拦截和管理对该对象的访问。

## 问题

代理模式与装饰器模式类似，但它的目的是控制对对象的访问，而非添加行为。它适用于懒加载、访问控制、日志记录和缓存等场景。

## 解决方案

代理模式控制对真实主题（real subject）的访问。代理对象会实现与另一个对象（即真实主题）相同的接口，并将对其发起的方法调用传递给真实主题。这另一个对象通常被称为真实主题。代理可被用来控制对它的访问。

在本章中，我们将探讨**代理模式**。

## 代理如何控制对其真实主题的访问？

最简单的代理类型是根本不实现任何访问控制。它只是将所有方法调用传递给真实主题。这种代理虽然无用，但确实为后续开发提供了基础。

在此示例中，我们构建一个代表图书馆的类。这个类封装了一个 `Book` 对象的目录，这些 `Book` 对象在 `XX` 章中定义过。

## 公共图书馆

### 新接口：`PublicLibrary`

#### **接口声明**

```java
```

## PublicLibrary 接口

```java
public interface PublicLibrary {

    // ... 方法声明 ...

}
```

### 方法

```java
List<String> getISBNs();

String getTitle(String isbn);

String getAuthor(String isbn);

String getTitleAndAuthor(String isbn);

String display();

// ... 等等 ...
```

---

### 真实图书馆实现：`RealLibrary`

```java
public class RealLibrary implements PublicLibrary {

    // ... 实现细节 ...

}
```

---

### 代理实现：`PublicLibraryProxy`

```java
public class PublicLibraryProxy implements PublicLibrary {

    private final PublicLibrary realLibrary;

    public PublicLibraryProxy(PublicLibrary realLibrary) {

        this.realLibrary = realLibrary;

    }

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

---

### 构造函数：`PublicLibraryProxy`

```java
public class PublicLibraryProxy implements PublicLibrary {

    private PublicLibrary realLibrary;

    public PublicLibraryProxy(PublicLibrary realLibrary) {

        this.realLibrary = realLibrary;

    }

    // ... 如上所述的方法实现 ...

}
```

---

### 初始化：`initialize` 和 `getTitleAndAuthor`

```java
public class PublicLibraryProxy implements PublicLibrary {

    private PublicLibrary realLibrary;

    public PublicLibraryProxy(PublicLibrary realLibrary) {

        this.realLibrary = realLibrary;

    }

    public String getTitleAndAuthor(String isbn) {

        return realLibrary.getTitleAndAuthor(isbn);

    }

    // ... 其他方法 ...

}
```

---

### 懒加载：`checkOutBook` 和 `getTitleAndAuthor`

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

---

### 计数使用：`getTitleAndAuthor`

```java
public class PublicLibraryProxy implements PublicLibrary {

    private PublicLibrary realLibrary;

    private int titleAndAuthorCount = 0;

    public String getTitleAndAuthor(String isbn) {

        titleAndAuthorCount++;

        return realLibrary.getTitleAndAuthor(isbn);

    }

    public int getTitleAndAuthorCount() {

        return titleAndAuthorCount;

    }

}
```

---

### 借出图书：`checkOutBook`

```java
public class PublicLibraryProxy implements PublicLibrary {

    private PublicLibrary realLibrary;

    public void checkOutBook(String isbn) {

        if (realLibrary.isAvailable(isbn)) {

            realLibrary.checkOutBook(isbn);

        } else {

            throw new BookNotAvailableException("ISBN 为 " + isbn + " 的图书当前不可借阅。");

        }

    }

}
```

---

### 抛出异常：`PublicLibraryProxy`

```java
public class PublicLibraryProxy implements PublicLibrary {

    // ...

    public void checkOutBook(String isbn) {

        if (!realLibrary.isAvailable(isbn)) {

            throw new BookNotAvailableException("ISBN 为 " + isbn + " 的图书不可借阅。");

        }

        realLibrary.checkOutBook(isbn);

    }

    // ...

}
```

---

### 归还图书：`returnBook`

```java
public class PublicLibraryProxy implements PublicLibrary {

    public void returnBook(String isbn) {

        // 归还图书无需访问控制。

        realLibrary.returnBook(isbn);

    }

}
```
