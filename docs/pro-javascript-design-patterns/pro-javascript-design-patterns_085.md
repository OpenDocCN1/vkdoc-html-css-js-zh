# 文档排版

## 引言

本文将介绍如何创建一个可以随时添加任何对象的`Builder`类。这个`Builder`类可以轻松地添加对象，而无需修改代码结构。

## 主要特性

### 灵活的对象管理

无需修改代码即可添加和移除任何对象。这使得`Builder`类非常适合需要动态添加和删除对象的场景。

### 计时与性能

每个方法都会启动一个计时器，并在方法调用结束后报告耗时。总耗时必须完全透明，以便可以与任何没有计时干扰的标准代码执行结果进行比较。

### 示例：`MethodProfiler`类

**目标：** 创建一个可以被任何对象包裹的装饰器，用于添加代码并记录执行时间。

#### 核心需求

`MethodProfiler`是一个用于`Builder`类的装饰器。它实现了与`builder`相同的方法（`startTimer`、`elapsedTime`、`displayTime`），并在方法调用前后添加计时元素。它随后输出结果。

要测试这个功能，你可以创建一个包含 1000 个元素的列表，并观察经过的时间。

```java
var list = new Builder.ListContainer(); // 包含 1000 个元素的列表
var profiler = new MethodProfiler.List(list); // 将列表包裹在计时器中
profiler.buildList(); // 创建列表并显示“Builder built in X ms”
profiler.displayTime(); // 显示总耗时
```

现在你已经知道它是如何工作的了。是时候将其泛化，以便它可以被任何地方使用。

## 核心类：`MethodProfiler`

```java
public class MethodProfiler {
    private final Object component;
    private final long[] timers = new long[2]; // start 和 stop 时间
    private Method method;
    private Map<String, Object> properties = new HashMap<>();
}
```

### 属性说明

- `component`（`Object` 类型）：存储被包装的对象。
- `timers`（`long[]`数组）：用于记录开始和结束时间。
- `method`（`String` 类型）：需要监听的方法名。
- `properties`（`Map<String, Object>`）：存储额外的属性。

### 构造函数

```java
public MethodProfiler(Object component) {
    this.component = component;
}
```

## 核心方法

- `startTimer()`：启动计时。
- `elapsedTime()`：计算并返回经过的时间。
- `displayTime()`：打印经过的时间。

### 计时实现

```java
public long startTimer() {
    return System.currentTimeMillis();
}

protected long getElapsedTime(long start) {
    return new Date().getTime() - start;
}

public void displayTime(long start, long end, String methodName) {
    long elapsed = end - start;
    console.log("Method: " + methodName + " took " + elapsed + " ms");
}
```

### 使用示例

```java
// 创建一个原始对象
var list = new ArrayList<String>();
// 在调用方法前后记录时间
long start = startTimer();
list.add("item");
long end = startTimer(); // 注意：这里应该是获取当前时间
long elapsed = end - start;
console.log("Method took " + elapsed + " ms");
```

### 完整实现

```java
public class MethodProfiler {
    private final Object component;
    private final long[] timers = new long[2];
}
```

---

## 关于装饰器模式

装饰器模式是一种结构设计模式，它允许您将行为添加到单个对象，而不会影响同一类中的其他对象。

## 示例

```java
function logMethod(target: any, propertyKey: string, descriptor: PropertyDescriptor) {
    const originalMethod = descriptor.value;
    descriptor.value = function(...args: any[]) {
        console.log("Method " + propertyKey + " is being called");
        const result = originalMethod.apply(this, args);
        console.log("Method " + propertyKey + " returned " + result);
        return result;
    };
}

class Calculator {
    @logMethod
    add(x: number, y: number) {
        return x + y;
    }
}
```

### 示例 2

```java
function logMethod(target: any, propertyKey: string, descriptor: PropertyDescriptor) {
    const originalMethod = descriptor.value;
    descriptor.value = function(...args: any[]) {
        console.log("Method " + propertyKey + " is being called");
        const result = originalMethod.apply(this, args);
        console.log("Method " + propertyKey + " returned " + result);
        return result;
    };
}

class Calculator {
    @logMethod
    subtract(x: number, y: number) {
        return x - y;
    }
}
```

### 示例 5

```java
function logMethod(target: any, key: string, descriptor: PropertyDescriptor) {
    const originalMethod = descriptor.value;
    descriptor.value = function(...args: any[]) {
        console.log("Key: " + target);
        return originalMethod.apply(this, args);
    };
}
```

### 示例 6

```java
function logParameter(target: any, key: string, index: number) {
    console.log("Parameter " + key + " at index " + index + " is being defined");
}
```

### 参考

- [装饰器模式](https://en.wikipedia.org/wiki/Decorator_pattern)
- [GoF 设计模式](https://en.wikipedia.org/wiki/Design_Patterns)
