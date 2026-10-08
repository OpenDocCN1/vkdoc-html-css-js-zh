# 第五部分

## 对象

### 创建对象的语法

一个对象可以看作是一个“键值对”的集合。使用 `Employee` 作为类名，我们可以通过 `new Employee()` 来创建一个对象实例。

### 在 Java 中使用 `new` 关键字

在 Java 中，我们使用 `new` 关键字来创建对象。例如：

```java
Employee employee = new Employee();
```

这会在堆内存中为 `Employee` 对象分配空间，并调用其构造函数进行初始化。

### 对象的属性（字段）

对象通常包含一些属性，也称为字段或成员变量。例如，`Employee` 类可能有 `name`、`age`、`salary` 等字段。

```java
public class Employee {
    String name;
    int age;
    double salary;
}
```

### 访问对象的属性

我们可以通过点号（`.`）运算符来访问对象的属性。例如：

```java
Employee emp = new Employee();
emp.name = "John Doe";
emp.age = 30;
emp.salary = 50000.0;
System.out.println(emp.name);
```

### 对象的方法

对象也可以包含方法，用于执行某些操作。例如：

```java
public class Employee {
    String name;
    // ... 其他字段

    public void work() {
        System.out.println(name + " is working.");
    }
}
```

调用对象的方法：

```java
Employee emp = new Employee();
emp.name = "Jane Doe";
emp.work(); // 输出：Jane Doe is working.
```

### 对象的生命周期

对象在 Java 中通过 `new` 关键字创建，当不再有引用指向它时，会被 Java 垃圾回收器自动回收。

```java
Employee emp = new Employee(); // 创建对象
emp = null; // 对象不再被引用，等待垃圾回收
```

### 键盘输入创建对象

你也可以通过从键盘读取输入来初始化对象的属性。例如，使用 `Scanner` 类：

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        Employee emp = new Employee();
        
        System.out.print("Enter employee name: ");
        emp.name = scanner.nextLine();
        
        System.out.print("Enter employee age: ");
        emp.age = scanner.nextInt();
        
        System.out.println("Employee: " + emp.name + ", Age: " + emp.age);
        
        scanner.close();
    }
}
```

好的，已为您排版优化。

---

这是一个非常棒的设计，但它看起来像一个由符号组成的语言，而非可读的文本。虽然我理解您对语言组织的想法，但我无法处理这种呈现方式，因为它看起来是乱码。

您可以用一个平淡的或复杂的形状来编程，但这会让人非常疑惑。令人着迷的是，它可以通过编程来实现不同的排列方式。

问题。

## 对象化编程

### 既是对象又是函数

是的，一个函数可以像一个对象。在 JavaScript 中，函数本身就是对象。

```
start() {
  // 做一些事
}
```

```
stop() {
 // 做其他事
}
```

这种代码与那种高层次、信息丰富的风格截然不同，它通常用于一个规模更大的场景。

### 既是对象又是函数

一个代码定义了一个名为 `Anim` 的类，并为其 `prototype` 属性分配了两种方法。我们将在第 H 章详细讨论这种技术。如果你更喜欢在单一声明中创建封装类，也可以这样写：

```
var Anim = function() {
  // ..一些代码...
};
Anim.prototype.start = function() {
  // ...
};
Anim.prototype.stop = function() {
  // ...
};
```

这会创建一个名为 `Anim` 的“类”，并为其 `prototype` 属性分配了两个方法。我们将在第 H 章详细讨论这种技术。如果你更喜欢在单一声明中创建封装类，也可以这样写：

```
var Anim = function() {
  // ...
};
Anim.prototype = {
  start: function() {
    // ...
  },
  stop: function() {
    // ...
  }
};
```

这对于习惯了在大括号内声明类的经典面向对象程序员来说，可能看起来更熟悉一些。如果你以前用过这种风格，你可能会想尝试下一个例子。同样，如果你不理解代码的某些部分，不用担心；我们在此使用的所有模式和技术都会在本书中解释。现在，你可以把这部分看作是一个任务在 JavaScript 中可以通过不同方式完成的实际例子。

## 摘要

### 既是对象又是函数

如果你来自过程式背景，你可能会这样做：

```javascript
function startAnimation() { ... }
function stopAnimation() { ... }
```

这种方法非常简单，但无法让你创建可以存储状态并拥有仅作用于其内部状态的方法的动画对象。下面的代码定义了一个允许你创建此类对象的类：

```javascript
var Anim = function() { ... }
Anim.prototype.start = function() { ... }
Anim.prototype.stop = function() { ... }
```

```javascript
var myAnim = new Anim();
myAnim.start();
...
myAnim.stop();
```

这会创建一个名为 `Anim` 的“类”，并为其 `prototype` 属性分配了两个方法。我们将在第 H 章详细讨论这种技术。如果你更喜欢在单一声明中创建封装类，也可以这样写：

```
var Anim = function() { ... };
Anim.prototype = {
  start: function() { ... },
  stop: function() { ... }
};
```

这对于习惯了在大括号内声明类的经典面向对象程序员来说，可能看起来更熟悉一些。如果你以前用过这种风格，你可能会想尝试下一个例子。同样，如果你不理解代码的某些部分，不用担心；我们在此使用的所有模式和技术都会在本书中解释。现在，你可以把这部分看作是一个任务在 JavaScript 中可以通过不同方式完成的实际例子。让我们快速看一下组织代码以完成一个任务的不同方法：**启动和停止动画**。如果你不理解这些例子也没关系；我们在此使用的所有模式和技术都会在本书中解释。

## 小节

### 章节

**向 `Function` 对象添加一个可用于声明方法的方法。**

```javascript
Function.prototype.method = function(name, fn) {
  this.prototype[name] = fn;
};
```

```javascript
var Anim = function() { ... };
Anim.method('start', function() { ... });
Anim.method('stop', function() { ... });
```

这对于经典面向对象程序员来说可能看起来更熟悉一些。同样，如果你不理解代码的某些部分，不用担心；我们在此使用的所有模式和技术都会在本书中解释。

### 章节

**向 `Function` 对象添加一个可用于声明方法的方法。**

```javascript
Function.prototype.method = function(name, fn) {
  this.prototype[name] = fn;
};
```

```javascript
var Anim = function() { ... };
Anim.method('start', function() { ... });
Anim.method('stop', function() { ... });
```
这对于习惯了类声明的经典面向对象程序员来说，可能看起来更熟悉一些。
