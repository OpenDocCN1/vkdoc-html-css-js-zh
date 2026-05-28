# 概述

在 `GeneralCatalog` 类中，`default` 方法是最简单的一种实现方式。该方法仅会调用一次 `findBooks` 方法，并返回其结果。

在许多情况下，子类只需要重写 `findBooks` 方法即可定义自己的搜索逻辑，而无需修改其他方法。

`GeneralCatalog` 的子类需要实现 `findBooks` 方法。在 `Library` 类中，`findBooks` 方法用于执行实际的搜索逻辑。

子类只需要实现该接口，而不需要关心搜索请求是如何提交的。

## 示例

```java
public class GeneralCatalog {
    public List<Book> findBooks(SearchRequest request) {
        // default implementation
        return Collections.emptyList();
    }
}
```

在之前的示例中，我们使用了 `findBooks` 方法。该方法接受一个 `SearchRequest` 对象，并返回匹配的书目列表。

---

## GeneralCatalog 协议

### GeneralCatalog 类的定义

```java
public abstract class GeneralCatalog {
    // 抽象方法，由子类实现
    public abstract List<Book> findBooks(SearchRequest request);
}
```

### `findBooks` 方法的实现

```java
// 如果请求中的参数是空的，则返回 false
if (request.getQueryString() == null || request.getQueryString().isEmpty()) {
    return false;
}
```

```java
// 如果请求成功，则执行搜索
if (this.success) {
    this.findBooks(request);
}
```

### 示例：传递 `request` 对象

```java
// 创建一个空的构造函数，并实现 `BookSearchCriteria` 接口
public class BookSearchCriteria {
    // 完成搜索逻辑
    public List<Book> search(SearchRequest request) {
        // 检查书籍是否属于某个类别
        if (this.catalog instanceof GeneralCatalog) {
            return this.catalog.findBooks(request);
        }
        return Collections.emptyList();
    }
}
```

```java
// 如果搜索成功，则返回书目列表
if (this.success) {
    return this.findBooks(request);
}
```

```java
// 如果请求成功，则返回 true
return true;
```

---

## 高级用法：创建更健壮的搜索

你可以创建一个空的构造函数，扩展 `GeneralCatalog` 类，并实现 `BookSearchCriteria` 接口。这样，在实现过程中，可以检查书名、作者、ISBN 等信息是否匹配。

例如：

```java
public class AdvancedBookCatalog extends GeneralCatalog {
    @Override
    public List<Book> findBooks(SearchRequest request) {
        // 检查书名是否匹配
        if (request.getTitle() != null && !request.getTitle().isEmpty()) {
            // 执行搜索
        }
        return Collections.emptyList();
    }
}
```

```java
// 如果搜索成功，则返回成功状态
return true;
```

---

## 冒烟测试用例

### 示例：`GeneralCatalog` 的简单实现

```java
// 创建一个空的构造函数，并实现 `BookSearchCriteria` 接口
BookSearchCriteria criteria = new BookSearchCriteria();
// 调用搜索方法
List<Book> results = criteria.search(request);
// 如果搜索成功，则打印结果
if (results != null && !results.isEmpty()) {
    System.out.println("Found " + results.size() + " books.");
}
```

### 验证空请求

```java
// 创建一个空请求
SearchRequest emptyRequest = new SearchRequest();
// 执行搜索
List<Book> results = catalog.findBooks(emptyRequest);
// 断言结果为空
assertTrue(results.isEmpty());
```

### 验证无效参数

```java
// 创建一个无效的请求（例如，作者名为空字符串）
SearchRequest invalidRequest = new SearchRequest();
// 执行搜索
List<Book> results = catalog.findBooks(invalidRequest);
// 断言结果为空
assertTrue(results.isEmpty());
```

---

## 最终总结

本文档介绍了如何创建一个空的构造函数，扩展 `GeneralCatalog` 类，并实现 `BookSearchCriteria` 接口。在实现过程中，你需要检查书名、作者、ISBN 等信息是否匹配。这是对 `findBooks` 方法的一种简单直接的实现方式。

### 传递请求

有多种方式可以将搜索请求传递给链中的下一个方法。最常见的方式是使用一个专门的请求对象，或者在没有任何参数的情况下调用方法本身。最简单的方式是直接调用方法，不传递任何参数。

例如：

```java
// 创建一个搜索请求，并检查书名是否匹配
if (request.getTitle() != null && !request.getTitle().isEmpty()) {
    // 执行搜索
}
```

在之前的示例中，我们使用了 `findBooks` 方法。该方法接受一个 `SearchRequest` 对象，并返回匹配的书目列表。

例如：

```java
// 创建一个搜索请求，并检查书名是否匹配
SearchRequest request = new SearchRequest();
request.setTitle("Java Programming");
List<Book> results = catalog.findBooks(request);
if (results != null && !results.isEmpty()) {
    System.out.println("Found " + results.size() + " books.");
}
```

---

## 关于 `PublicLibrary` 类的扩展

`PublicLibrary` 类扩展了 `GeneralCatalog` 类，并实现了 `BookSearchCriteria` 接口。在实现过程中，我们检查了书名、作者、ISBN 等信息，并返回匹配的书籍列表。

```java
public class PublicLibrary extends GeneralCatalog {
    @Override
    public List<Book> findBooks(SearchRequest request) {
        // 执行搜索逻辑
        return Collections.emptyList();
    }
}
```

```java
// 如果请求参数为空，则返回空列表
if (request.getTitle() == null || request.getTitle().isEmpty()) {
    return Collections.emptyList();
}
```

---

## 库的架构

首先，你需要修改 `PublicLibrary` 类中的 `findBooks` 方法，以允许搜索基于 `genre` 进行过滤。如果可选的 `genre` 参数没有提供，则只搜索那些属于 `GeneralCatalog` 类别的书籍。

如果提供了 `genre` 参数，则只有那些匹配的书籍才会被搜索。

```java
public class PublicLibrary extends GeneralCatalog {
    @Override
    public List<Book> findBooks(SearchRequest request) {
        // 检查是否提供了 genre 参数
        if (request.getGenre() != null && !request.getGenre().isEmpty()) {
            // 只搜索该类型的书籍
            return searchByGenre(request.getGenre());
        }
        // 否则，搜索所有书籍
        return searchAll(request);
    }
}
```

---

## 测试用例

### 测试 `findBooks` 方法

```java
// 创建一个空的请求
SearchRequest emptyRequest = new SearchRequest();
// 执行搜索
List<Book> results = catalog.findBooks(emptyRequest);
// 断言结果为空
assertTrue(results.isEmpty());
```

```java
// 创建一个包含标题的请求
SearchRequest request = new SearchRequest();
request.setTitle("Java");
List<Book> results = catalog.findBooks(request);
// 断言结果不为空
assertFalse(results.isEmpty());
```

```java
// 如果搜索成功，则返回书目列表
if (results != null && !results.isEmpty()) {
    return results;
} else {
    return Collections.emptyList();
}
```

---

## 使用 `findBooks` 方法

```java
// 创建一个空的构造函数，并实现 `BookSearchCriteria` 接口
BookSearchCriteria criteria = new BookSearchCriteria();
// 设置搜索参数
criteria.setTitle("Java Programming");
// 执行搜索
List<Book> results = criteria.search(criteria);
// 如果搜索成功，则打印结果
if (results != null && !results.isEmpty()) {
    System.out.println("Found " + results.size() + " books.");
}
```

```java
// 如果搜索失败，则返回空列表
return Collections.emptyList();
```

---