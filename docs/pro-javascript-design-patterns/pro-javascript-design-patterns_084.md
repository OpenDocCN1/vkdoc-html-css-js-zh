# 装饰器

## 在函数装饰器定义中，使用`@`符号

`@`符号用于在函数装饰器定义中应用装饰器。这意味着它将在运行时对函数进行包装，并将原始函数作为参数传递给装饰器。

## 装饰器参数

`decorator`模式是`GoF`设计模式中的一种。

装饰器模式在软件设计中广泛使用。它通常用于向类添加功能，而无需修改类的源代码。

这种模式允许您通过将对象包装在装饰器类中来为对象添加行为。这是一种比使用继承更灵活的方法，因为您可以动态地添加或删除行为。

### 装饰器的优点

-   灵活性：您可以动态地添加或删除行为。
-   遵循开闭原则：类可以对扩展开放，对修改关闭。
-   遵循单一职责原则：每个类只负责一件事。

### 装饰器的缺点

-   复杂性：此类模式可能会引入许多小类，从而增加代码的复杂性。
-   性能：由于包装，装饰器可能会引入轻微的性能开销。

### 何时使用装饰器

-   当您需要在不修改现有代码的情况下，向对象添加功能时。
-   当您需要组合多个行为时。
-   当您需要根据条件来添加或删除行为时。

## 为什么使用装饰器

装饰器是重用类和类行为的便捷方式。装饰器模式允许您将对象包装在装饰器类中，以添加或修改行为。这可以在不修改原始类的情况下完成。

## 装饰器 vs 继承

装饰器和继承都是用于重用代码的技术。区别在于，继承是静态的，而装饰器是动态的。

### 何时使用继承

-   当您有一个明确的“是一个”关系时。
-   当您需要子类共享父类的实现时。

### 何时使用装饰器

-   当您需要在不影响其他对象的情况下，向单个对象添加功能时。
-   当您需要动态地添加或删除功能时。

## 装饰器模式 vs 策略模式

装饰器模式和策略模式都用于在运行时改变对象的行为。区别在于，装饰器模式通过包装对象来添加功能，而策略模式通过委托给不同的策略对象来改变行为。

## 装饰器在 TypeScript 中

装饰器是 TypeScript 中的一个实验性特性。您可以在 TypeScript 编译器配置文件中启用它们。

### 启用装饰器

您可以在`tsconfig.json`文件中将`experimentalDecorators`选项设置为`true`。

### 装饰器类型

装饰器可以是：

-   类装饰器
-   方法装饰器
-   属性装饰器
-   参数装饰器

### 装饰器工厂

装饰器工厂是一个返回装饰器函数的函数。

## 示例

### 示例：类装饰器

```typescript
function classDecorator(target: Function) {
    console.log("Class decorator called");
}

@classDecorator
class DecoratedClass {
    // ...
}
```

### 示例：方法装饰器

```typescript
function enumerable(value: boolean) {
    return function (target: any, propertyKey: string, descriptor: PropertyDescriptor) {
        descriptor.enumerable = value;
    };
}

class Greeter {
    greeting: string;
    constructor(message: string) {
        this.greeting = message;
    }

    @enumerable(false)
    greet() {
        return "Hello, " + this.greeting;
    }
}
```

### 示例：属性装饰器

```typescript
function format(formatString: string) {
    return function (target: any, propertyKey: string): any {
        let value = target[propertyKey];
        const getter = function () {
            return `${formatString} ${value}`;
        };
        const setter = function (newVal: string) {
            value = newVal;
        };
        return {
            get: getter,
            set: setter,
            enumerable: true,
            configurable: true
        };
    };
}

class Greeter {
    @format("Hello")
    greeting: string;
}
```

`format`函数可以被用作属性装饰器。它将`getter`和`setter`应用于属性，以便在获取或设置属性值时应用格式。

### 示例：带参数的装饰器

```typescript
function logClass(target: Function) {
    console.log("Class is being created");
}

@logClass
class MyClass {
    // ...
}
```

`logClass`函数可以被用作装饰器。

### 示例：返回值

```typescript
function returnValue(value: string) {
    return function (target: any, propertyKey: string, descriptor: PropertyDescriptor) {
        const originalMethod = descriptor.value;
        descriptor.value = function(...args: any[]) {
            console.log("Method called");
            return originalMethod.apply(this, args);
        };
        return descriptor;
    };
}

class Greeter {
    greeting: string;
    constructor(message: string) {
        this.greeting = message;
    }

    @returnValue("Hello")
    greet() {
        return "Hello, " + this.greeting;
    }
}
```

装饰器的返回值可以用来修改类的行为。

### 示例：日志

```typescript
function logMethod(target: any, propertyKey: string, descriptor: PropertyDescriptor) {
    const originalMethod = descriptor.value;
    descriptor.value = function(...args: any[]) {
        console.log("Method " + propertyKey + " is being called");
        return originalMethod.apply(this, args);
    };
}

class Calculator {
    @logMethod
    add(a: number, b: number) {
        return a + b;
    }
}
```

### 示例：装饰器工厂

```typescript
function color(value: string) {
    return function (target: any) {
        // ...
    };
}
```

## 总结

装饰器是一种设计模式，允许您向现有对象添加新功能，而无需修改其结构。装饰器通常用于向类添加行为，而无需创建子类。

### 何时使用装饰器

-   当您需要向现有类添加功能时。
-   当您需要组合行为时。
-   当您需要动态地添加或删除行为时。

### 何时不使用装饰器

-   当您需要添加简单的、静态的行为时，继承可能更简单。
-   当性能至关重要时，装饰器可能会引入不必要的开销。

# 结论

装饰器是向类或对象添加功能的一种强大方式。它们提供了一种灵活的、可重用的、遵循单一职责原则的替代继承的方法。

# 基础装饰器示例

```typescript
function classDecorator(constructor: Function) {
    Object.seal(constructor);
    Object.seal(constructor.prototype);
}
```

### 示例 2

```typescript
function enumerable(value: boolean) {
    return function (target: any, propertyKey: string, descriptor: PropertyDescriptor) {
        descriptor.enumerable = value;
    };
}
```

### 示例 3

```typescript
function configurable(value: boolean) {
    return function (target: any, propertyKey: string, descriptor: PropertyDescriptor) {
        descriptor.configurable = value;
    };
}
```

### 示例 4

```typescript
function logProperty(target: any, key: string) {
    console.log("Property " + key + " is being defined");
}
```

### 示例 5

```typescript
function logMethod(target: any, propertyKey: string, descriptor: PropertyDescriptor) {
    const originalMethod = descriptor.value;
    descriptor.value = function(...args: any[]) {
        console.log("Method " + propertyKey + " is being called");
        return originalMethod.apply(this, args);
    };
}
```

---

## 何时使用装饰器

装饰器在您需要向现有类添加功能，而又无法修改它时非常有用。例如，当您使用第三方库或框架时，您可能希望向类添加日志记录或验证功能。

## 装饰器应用示例

```typescript
@logClass
class MyClass {
    // ...
}
```

### 示例 2

```typescript
@sealed
class MyClass {
    // ...
}
```

### 示例 3

```typescript
@logger
class MyClass {
    // ...
}
```

---

# 结论

装饰器是一种强大的设计模式，它允许您在不修改代码的情况下，向现有对象添加新功能。它们通常用于向类添加行为，而无需创建子类。

### 示例 1

```typescript
function classDecorator(constructor: Function) {
    console.log("Class decorator called");
}
```

### 示例 2

```typescript
function logClass(target: Function) {
    console.log("Class is being created");
}
```

### 示例 3

```typescript
function seal(constructor: Function) {
    Object.seal(constructor);
    Object.seal(constructor.prototype);
}
```

### 示例 4

```typescript
function logProperty(target: any, key: string) {
    console.log("Property " + key + " is being defined");
}
```

### 示例 5

```typescript
function logMethod(target: any, key: string, descriptor: PropertyDescriptor) {
    const originalMethod = descriptor.value;
    descriptor.value = function(...args: any[]) {
        console.log("Key: " + target);
        return originalMethod.apply(this, args);
    };
}
```

### 示例 6

```typescript
function logParameter(target: any, key: string, index: number) {
    console.log("Parameter " + key + " at index " + index + " is being defined");
}
```

---

## 关于装饰器模式

装饰器模式是一种结构设计模式，它允许您将行为添加到单个对象，而不会影响同一类中的其他对象。

## 示例

```typescript
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

```typescript
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

---

# 结论

在函数装饰器定义中，`@`符号用于应用装饰器。装饰器是一个函数，它接收目标、属性键和描述符作为参数，并返回一个修改后的描述符。

## 示例

```typescript
function logMethod(target: any, propertyKey: string, descriptor: PropertyDescriptor) {
    const originalMethod = descriptor.value;
    descriptor.value = function(...args: any[]) {
        console.log("Method " + propertyKey + " is being called");
        return originalMethod.apply(this, args);
    };
}

class Calculator {
    @logMethod
    multiply(x: number, y: number) {
        return x * y;
    }
}
```

# 装饰器

## 使用场景

装饰器在您需要向现有对象添加功能时非常有用。它们可以在不修改原始对象的情况下添加功能。

## 示例

```
function logClass(target: Function) {
    console.log("Class is being created");
}

@logClass
class MyClass {
    // ...
}
```

### 示例 2

```
function logMethod(target: any, propertyKey: string, descriptor: PropertyDescriptor) {
    const originalMethod = descriptor.value;
    descriptor.value = function(...args: any[]) {
        console.log("Method " + propertyKey + " is being called");
        return originalMethod.apply(this, args);
    };
}

class Calculator {
    @logMethod
    divide(x: number, y: number) {
        return x / y;
    }
}
```

---

## 结论

装饰器模式在您需要向现有对象添加功能时非常有用。它们可以在不修改原始对象的情况下添加功能。

## 示例

```
function logClass(target: Function) {
    console.log("Class is being created");
}

@logClass
class MyClass {
    // ...
}
```

### 示例 2

```
function logMethod(target: any, propertyKey: string, descriptor: PropertyDescriptor) {
    const originalMethod = descriptor.value;
    descriptor.value = function(...args: any[]) {
        console.log("Method " + propertyKey + " is being called");
        return originalMethod.apply(this, args);
    };
}

class Calculator {
    @logMethod
    modulo(x: number, y: number) {
        return x % y;
    }
}
```

---

## 注意事项

装饰器是一种高级主题。在使用它们之前，您应该熟悉类、函数和对象。

## 示例

```
function logClass(target: Function) {
    console.log("Class is being created");
}

@logClass
class MyClass {
    // ...
}
```

### 示例 2

```
function logMethod(target: any, propertyKey: string, descriptor: PropertyDescriptor) {
    const originalMethod = descriptor.value;
    descriptor.value = function(...args: any[]) {
        console.log("Method " + propertyKey + " is being called");
        return originalMethod.apply(this, args);
    };
}

class Calculator {
    @logMethod
    power(base: number, exponent: number) {
        return Math.pow(base, exponent);
    }
}
```

---

## 总结

装饰器是一种设计模式，它允许您向现有对象添加功能，而无需修改其结构。装饰器通常用于向类添加行为，而无需创建子类。

## 示例

```
function logClass(target: Function) {
    console.log("Class is being created");
}

@logClass
class MyClass {
    // ...
}
```

### 示例 2

```
function logMethod(target: any, propertyKey: string, descriptor: PropertyDescriptor) {
    const originalMethod = descriptor.value;
    descriptor.value = function(...args: any[]) {
        console.log("Method " + propertyKey + " is being called");
        return originalMethod.apply(this, args);
    };
}

class Calculator {
    @logMethod
    squareRoot(x: number) {
        return Math.sqrt(x);
    }
}
```

---

## 结论

装饰器是一种有用的设计模式，它允许您在不修改现有对象的情况下，向它们添加新功能。它们通常用于向类添加行为，而无需创建子类。

## 示例

```
function logClass(target: Function) {
    console.log("Class is being created");
}

@logClass
class MyClass {
    // ...
}
```

### 示例 2

```
function logMethod(target: any, propertyKey: string, descriptor: PropertyDescriptor) {
    const originalMethod = descriptor.value;
    descriptor.value = function(...args: any[]) {
        console.log("Method " + propertyKey + " is being called");
        return originalMethod.apply(this, args);
    };
}

class Calculator {
    @logMethod
    absolute(x: number) {
        return Math.abs(x);
    }
}
```

---

## 何时使用装饰器

装饰器在您需要向现有类添加功能，而又无法修改它时非常有用。例如，当您使用第三方库或框架时，您可能希望向类添加日志记录或验证功能。

## 示例

```
@logClass
class MyClass {
    // ...
}
```

### 示例 2

```
@sealed
class MyClass {
    // ...
}
```

### 示例 3

```
@logger
class MyClass {
    // ...
}
```

---

## 结论

装饰器是一种强大的设计模式，它允许您在不修改代码的情况下，向现有对象添加新功能。它们通常用于向类添加行为，而无需创建子类。

### 示例 1

```
function classDecorator(constructor: Function) {
    console.log("Class decorator called");
}
```

### 示例 2

```
function logClass(target: Function) {
    console.log("Class is being created");
}
```

### 示例 3

```
function seal(constructor: Function) {
    Object.seal(constructor);
    Object.seal(constructor.prototype);
}
```

### 示例 4

```
