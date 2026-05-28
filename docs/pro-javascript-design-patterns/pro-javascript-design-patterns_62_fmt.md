# 打印模型结构

print(model)

## 示例：汽车登记信息

假设您需要创建一个系统来表示城市中所有汽车的信息。您需要存储每辆车的详细信息：制造商（Make）、型号（Model）、年份（Year）、车主姓名（Owner Name）、车牌号（Tag Number）、最近登记日期（Last Registration Date）。通常，您会选择将每辆车表示为一个对象。

## 对象

### 类

在 `Car` 类中，未优化的。`var Car.make` 函数用于返回 `this.make`。

```
this.make = make
this.model = model
this.year = year
this.owner = owner
this.tag = tag
this.renewDate = renewDate
```

### `Car.prototype.make`

```
getMake() {
    return this.make
}
getModel() {
    return this.model
}
getYear() {
    return this.year
}
transferOwnership(newOwner, newTag, newRenewDate) {
    this.owner = newOwner
    this.tag = newTag
}
```

## 内部与外部状态

对对象的数据进行分类，可以区分为内部（`intrinsic`）或外部（`extrinsic`）状态。您可以这样看待：如果数据是内部状态，那么它是对象的固有属性，通常不会改变。例如，一个 `Car` 对象的 `make`（制造商）、`model`（型号）、`year`（年份）通常是内部状态。这些数据对于该特定对象来说是唯一的。

如果数据是外部状态，那么它可能在不同的上下文或操作之间改变。例如，一个 `Car` 对象的 `speed`（速度）或 `ownerName`（车主姓名）可能是外部状态。这些数据可以从外部源获取或修改。

我们使用一个工厂类来创建和管理这些 `Car` 对象。工厂类会检查一个特定的 `make/model/year` 组合是否已经被创建过。如果已经创建，它返回现有的对象。如果没有，它会创建一个新的 `Car` 对象并存储起来供以后使用。

## 使用工厂

工厂逻辑如下：

```
if (createdCars.make == make && createdCars.model == model && createdCars.year == year) {
    return createdCars;
} else {
    // 创建一个新的 Car 并保存它
}
```

工厂的 `getCar(make, model, year)` 函数接收 `make`、`model`、`year` 参数并返回一个 `Car` 对象。

```
Car getCar(make, model, year) {
    ...
}
```

## 使用 `CarRecordManager` 单例

`CarRecordManager` 函数很简单。它检查特定的 `make/model/year` 组合是否已经被创建过。如果是，返回它。如果不是，它会创建一个 `newCar` 并存储它。之后返回该 `newCar`。

```
if (createdCars.make == make && createdCars.model == model && createdCars.year == year) {
    return createdCars;
} else {
    Car newCar = createCar(make, model, year);
    storeCar(newCar);
    return newCar;
}
```

## 其他

- 对于 `Car` 对象，定义 `getMake()`、`getModel()`、`getYear()` 函数。
- 对于 `CarRecordManager` 单例，定义 `getCarRecordDataBase(tag)` 函数。

### 旧方法转移到新类

所有之前被移除的外部数据都转移到了 `CarRecordManager` 中。所有处理这些注册数据的方法也被移到了 `ManagerCarRecord` 对象中。您现在可以从 `Car` 类中移除这些方法，并留下参数，以便为所有保留在 `Car` 类中的外部数据提供参数。

## `CarRecordManager` 单例模式

`CarRecordDataBase(tag)` 函数如下：

```
CarRecordDataBase(tag) {
    owner = owner;
    renewDate = renewDate;
    car = car;
}
```

### 方法之前被包含在 `Car` 类中

```
transferOwnership(tag, newOwner, newTag, newRenewDate) {
    car = carRecordDataBase.getRecord(tag);
    car.owner = newOwner;
    car.tag = newTag;
    car.renewDate = newRenewDate;
}
```

# 对象池模式

## 对象池模式的应用

## 意图

`对象池模式`是一种创建型设计模式，它通过重用对象来减少资源消耗，而不是创建和销毁对象。

## 问题

在需要大量创建和销毁对象的场景下，频繁的创建和销毁操作可能会带来显著的性能开销。例如，在游戏开发中，管理大量对象（如子弹、粒子效果）时，如果每个对象都在创建时分配内存、在销毁时释放内存，可能会导致性能瓶颈。

## 解决方案

使用`对象池模式`，可以预先创建一组对象，并将其存储在一个“池”中。当需要一个对象时，从池中取出一个空闲对象，而不是新建一个；当对象使用完毕后，将其归还到池中，而不是销毁。这种方式避免了重复的内存分配和释放，从而提高了性能。

`对象池模式`的核心思想是重用，它通过维护一个对象集合来管理对象的生命周期。此模式特别适用于那些创建成本高、但使用频率高的对象。

## 示例：Web 日历页面

为了说明如何使用`对象池模式`来存储外部状态，让我们创建一个 Web 日历页面。首先，创建**未优化**的版本（`CalendarItem` 类），然后展示**优化**后的版本（使用`对象池模式`的 `CalendarItem` 类）。

### 未优化版本

```javascript
function CalendarItem(year, month, day, display) {
  this.year = year;
  this.month = month;
  this.day = day;
  this.display = display;
}
```

**问题**：每显示一个日期都需要创建一个`CalendarItem`对象，当需要显示一个月的所有日期时（例如 31 天），会创建 31 个对象。如果日历包含多个月份（如一年），对象数量将急剧增加，导致内存压力增大。

### 优化版本（使用对象池模式）

```javascript
// 对象池
var CalendarItemPool = [];

// 从池中获取或创建对象的函数
function getCalendarItem(year, month, day, display) {
  if (CalendarItemPool.length > 0) {
    var item = CalendarItemPool.pop();
    item.year = year;
    item.month = month;
    item.day = day;
    item.display = display;
    return item;
  } else {
    return new CalendarItem(year, month, day, display);
  }
}

// 归还对象到池中的函数
function returnCalendarItem(item) {
  CalendarItemPool.push(item);
}
```

**优化说明**：通过引入`CalendarItemPool`数组，我们实现了对象的复用。当需要一个新对象时，首先检查池中是否有可用对象；如果有，则直接取用并更新其属性，避免了新建对象；当对象使用完毕时，将其归还到池中，供后续复用。这显著减少了对象的创建和销毁次数。

### 优点

- **减少内存分配和回收的开销**：避免了频繁的垃圾回收，提升性能。
- **降低 CPU 使用率**：减少了创建和销毁对象的时间。
- **提高响应速度**：对于需要大量对象的场景（如游戏、高并发系统），效果明显。

### 缺点

- **增加代码复杂度**：需要管理对象的生命周期和池的状态。
- **对象状态管理**：必须确保对象在归还时状态被正确清理或重置，否则可能导致数据污染。
- **适用于特定场景**：对于创建成本低或使用频率低的对象，此模式可能带来不必要的复杂性。

## CalendarItem 类

`CalendarItem`类用于存储日历中每一天的显示信息。

### 原型

```javascript
function CalendarItem(year, month, day, display) {
  this.year = year;
  this.month = month;
  this.day = day;
  this.display = display;
}
```

**说明**：
- `year`：年份
- `month`：月份
- `day`：日期
- `display`：显示内容（如`"none"`、`"block"`等）

### 使用示例

```javascript
var item1 = new CalendarItem(2023, 10, 1, "block");
var item2 = new CalendarItem(2023, 10, 2, "block");
```

### 默认值

- `display`的默认值为`"none"`

## 优化建议

### 1. 使用简单对象池

对于小型应用，可使用数组作为对象池。

```javascript
var pool = [];
function acquire() { return pool.pop() || new CalendarItem(); }
function release(obj) { pool.push(obj); }
```

### 2. 使用复杂对象池

对于大型应用，可封装一个通用的对象池类，支持动态扩容、最大容量限制等功能。

### 3. 使用第三方库

例如 `object-pool`、`generic-pool` 等，它们提供了更完善的对象池实现。

## 总结

`对象池模式`通过重用对象来减少创建和销毁的开销，适用于创建成本高、使用频率高的对象场景。在 JavaScript 中，可以通过数组（`pool`）或闭包来实现。此模式尤其适用于游戏开发、图形渲染、高并发请求等需要大量对象的场景。但需注意，正确管理对象的状态和生命周期是成功应用此模式的关键。

---

# 概述

`ParentMonth`类用于存储日期信息，并提供了多种与日期计算相关的方法。

`ParentMonth`类是一个 `Comparable`，因此您可以使用 `compareTo()` 方法来比较两个 `ParentMonth` 实例的大小。

## 成员变量

`ParentMonth`类包含以下成员变量：

*   `year`：`int` 类型，表示年份。
*   `month`：`int` 类型，表示月份。
*   `parentMonthKey`：`String` 类型，是该对象的唯一键。
*   `parentMonth`：`String` 类型，表示月份的字符串表示形式。
*   `daysInMonth`：`int` 类型，表示该月的天数。

### 构造函数

```java
public ParentMonth(int year, int month)
```

该构造函数接收两个参数：年份 (`year`) 和月份 (`month`)。

## 核心方法

`ParentMonth`类提供了以下核心方法：

*   `getDaysInMonth()`：获取该月的天数。
*   `isBefore(ParentMonth)`：判断当前实例是否在指定实例之前（按时间排序）。
*   `isAfter(ParentMonth)`：判断当前实例是否在指定实例之后（按时间排序）。
*   `generateParentMonthRange(int length)`：返回从当前月份开始，长度为 `length` 月份序列的一个列表。此方法会根据 `length` 的正负值，向前或向后生成月份序列。
*   `getParentMonthKey()`：获取实例的唯一键。
*   `getParentMonthKeyCalendarSortable()`：获取用于日历排序的键。

## 日期计算

### 1. 从字符串解析日期

`ParentMonth`类重写了 `toString()` 方法，因此可以使用 `ParentMonth.parseParentMonth(monthlyKey)` 类方法将其解析回 `ParentMonth` 对象。您还可以使用 `YearMonth.parse(monthlyKey, parentMonth.format)` 方法将其解析为 `java.time.YearMonth`。

### 2. 比较日期

当您需要比较两个 `ParentMonth` 对象时，可以使用 `compareTo()` 方法。例如：

```java
parentA.compareTo(parentB)
```

如果返回值小于 0，则表示 `parentA` 的日期早于 `parentB`；如果返回值等于 0，则两个日期相同；如果返回值大于 0，则 `parentA` 的日期晚于 `parentB`。

### 3. 获取当前日期

您可以使用 `ParentMonthFactory` 工厂类的 `getCurrentMonth()` 方法获取当前 `ParentMonth` 实例。也可以直接实例化它，例如：`new ParentMonth(2023, 7)`。

### 4. 获取其他日期

使用 `generateParentMonthRange()` 方法可以生成其他日期。假设您需要获取前 3 个月的日期，可以这样调用：

```java
// 获取前 3 个月（包含当前月）
List<ParentMonth> monthsParent = parentMonthFactory.generateParentMonthRange(3);
```

### 5. 手动计算日期

仅需手动创建 `ParentMonth` 实例，并指定年份和月份即可。另外，使用 `ParentMonth(1900, 1)` 作为初始日期也是可行的。

## 与 `ParentMonth` 类的关系

`ParentMonth` 类与其它类有组合关系。已知其它类已经在所有位置进行了修改，因此只有 `ParentMonth` 类和 `ParentMonth` 对象需要进行额外处理。这种协同模式之所以有效，是因为组合结构的构造方法已经包含了所有外部数据。当需要移除一个 `ParentMonth` 对象时，时间序列中的数据会被自动移除。如需替换 `ParentMonth` 对象，可以使用 `ParentMonthFactory` 工厂类的 `replaceParentMonth(int oldIndex, ParentMonth newMonth)` 方法。

## 示例

```
Tooltip Objects
```

# Tooltip 类与事件系统

## Tooltip 的起源与演变

在 Web 开发中，Tooltip 最初是一种简单的 UI 模式：当用户将鼠标悬停在某个元素上时，会显示一个包含额外信息的浮动框。随着 Web 应用复杂度的提升，Tooltip 不再局限于仅显示文本；它可以包含丰富的内容，如图标、格式化文本甚至交互元素。

现代框架如 Vue.js 提供了更灵活、可复用的 Tooltip 组件。Vue 的组件化架构允许你从零开始构建符合需求的 Tooltip，但大多数情况下，开发者更倾向于使用现成的 UI 库，如 Element UI，它提供了开箱即用的 Tooltip 组件。

## Tooltip 的核心机制

在深入研究 Element UI 的 `Tooltip` 组件之前，我们首先需要理解它要解决的核心问题：悬停显示和隐藏。

### 事件监听与状态

Tooltip 的核心是鼠标的 `mouseenter` 和 `mouseleave` 事件。当鼠标进入目标元素时，显示 Tooltip；当鼠标离开时，隐藏它。然而，仅仅监听目标元素是不够的，因为 Tooltip 本身通常位于不同的 DOM 层级，用户可能会从目标元素移动到 Tooltip 内容上。如果 Tooltip 隐藏得太快，会导致糟糕的用户体验。为了解决这个问题，Tooltip 组件通常会引入一个短暂的延迟，并维护一个状态来跟踪鼠标是否仍在 Tooltip 区域内。

### Tooltip 类的基本结构

一个基础的 Tooltip 类通常包含以下属性和方法：

```
delayTimeout: null
show: func()
hide: func()
```

一个简单的实现如下：

```javascript
class Tooltip {
    constructor(options) {
        this.delay = options.delay || 0;
        this.tooltipElement = null; // 后续创建
        this.visible = false;
        this.delayTimeout = null;
        // ... 绑定事件等
    }
    show() {
        // 显示 Tooltip
        clearTimeout(this.delayTimeout);
        this.delayTimeout = setTimeout(() => {
            this.tooltipElement.style.display = 'block';
            this.visible = true;
        }, this.delay);
    }
    hide() {
        // 隐藏 Tooltip
        clearTimeout(this.delayTimeout);
        this.delayTimeout = setTimeout(() => {
            this.tooltipElement.style.display = 'none';
            this.visible = false;
        }, this.delay);
    }
}
```

## Element UI 的 Tooltip 组件

在 `Element UI` 的源码中，`Tooltip` 组件位于 `packages/tooltip` 目录下。其核心逻辑在 `src/main.js` 中定义。

```javascript
constructor: 
    this.tooltipId = 'el-tooltip-' + generateId()
    this.debounce = null
```

### 组件的主要方法

Element UI 的 Tooltip 组件包含一系列精心设计的方法，用于控制显示、隐藏、定位和更新。

- `show()`：显示 Tooltip。
- `hide()`：隐藏 Tooltip。
- `updatePopper()`：更新定位。

此外，还有用于处理延迟的方法：

- `handleShowPopper`：处理显示逻辑。
- `handleClosePopper`：处理隐藏逻辑。

```javascript
show: function() {
    // 逻辑
}
hide: function() {
    // 逻辑
}
updatePopper() {
    // 更新定位
}
```

### 处理鼠标事件

Element UI 的 Tooltip 组件通过监听 `mouseenter` 和 `mouseleave` 事件来控制显示与隐藏。

```javascript
handleShowPopper() {
    // 设置延迟
    this.debounce = setTimeout(() => {
        // 显示 Tooltip
    }, this.delay);
}
handleClosePopper() {
    // 清除延迟并隐藏
    clearTimeout(this.debounce);
    this.debounce = setTimeout(() => {
        // 隐藏 Tooltip
    }, this.delay);
}
```

### 延迟和防抖

为了提供更好的用户体验，Tooltip 组件支持延迟显示和隐藏。`delay` 属性可以控制延迟时间，而 `debounce` 机制用于防止在快速移动鼠标时频繁创建和销毁 Tooltip。

```javascript
handleShowPopper: function() {
    clearTimeout(this.debounce);
    this.debounce = setTimeout(() => {
        // 显示
    }, this.delay);
}
```

### 定位与样式

Tooltip 的定位通过 Popper.js 实现。`placement` 属性控制 Tooltip 相对于目标元素的位置，如 `top`，`bottom`，`left`，`right` 等。`popperClass` 属性允许自定义 Tooltip 的样式类。

```javascript
placement: 'top'
popperClass: ''
```

### 丰富的配置选项

Element UI 的 Tooltip 组件提供了丰富的配置选项，包括：
- `effect`：主题，默认为 `dark`。
- `content`：显示内容。
- `placement`：位置。
- `value` / `v-model`：控制显示状态。
- `disabled`：是否禁用。
- `offset`：偏移量。
- `transition`：动画名称。
- `visible-arrow`：是否显示箭头。
- `popper-options`：自定义 `Popper.js` 选项。
- `open-delay`：打开延迟。
- `close-delay`：关闭延迟。
- `manual`：手动模式（完全由开发者控制）。

## 从零实现一个简单的 Tooltip 组件

理解了核心机制后，我们可以尝试自己实现一个简单的 `Tooltip` 类。这个过程能帮助你深入理解组件是如何工作的。

### 第一篇：Tooltip 类的基本结构

首先，创建一个 `Tooltip` 类，它不依赖于框架，只做一件事：管理 Tooltip 的显示和隐藏。这里是一个极简的实现：

```javascript
var Tooltip = function(options) {
    this.delay = options.delay || 0;
    this.tooltipElement = null; // 稍后创建
    this.visible = false;
    this.delayTimeout = null;
};

Tooltip.prototype = {
    constructor: Tooltip,
    show: function() {
        clearTimeout(this.delayTimeout);
        this.delayTimeout = setTimeout(() => {
            this.tooltipElement.style.display = 'block';
            this.visible = true;
        }, this.delay);
    },
    hide: function() {
        clearTimeout(this.delayTimeout);
        this.delayTimeout = setTimeout(() => {
            this.tooltipElement.style.display = 'none';
            this.visible = false;
        }, this.delay);
    }
};
```

**问题**：上述代码中的 `Tooltip` 类并不能直接工作，因为它缺少对触发元素的事件绑定。我们需要将 `Tooltip` 绑定到一个具体的 DOM 元素上。

**改进**：创建一个新的变量 `that`，将其赋值给 `this`，以便在事件处理函数中正确引用 `Tooltip` 实例。这样，即使 `mouseenter` 和 `mouseleave` 事件被触发，`that` 仍然指向正确的 `Tooltip` 对象。

### 第二篇：事件绑定与作用域

```javascript
var Tooltip = function(options) {
    this.delay = options.delay || 0;
    this.tooltipElement = null;
    this.visible = false;
    this.delayTimeout = null;
    var that = this;
    var triggerElement = options.triggerElement;
    triggerElement.addEventListener('mouseenter', function() {
        that.show();
    });
    triggerElement.addEventListener('mouseleave', function() {
        that.hide();
    });
};

Tooltip.prototype = {
    constructor: Tooltip,
    show: function() { /* ... 同上 ... */ },
    hide: function() { /* ... 同上 ... */ }
};
```

**进一步优化**：使用 `bind()` 方法或 ES6 箭头函数来绑定 `this`，可以让代码更简洁。

### 第三篇：箭头函数与类语法

在现代 JavaScript 中，我们可以使用 `class` 语法和箭头函数来优雅地解决作用域问题。

```javascript
class Tooltip {
    constructor(options) {
        this.delay = options.delay || 0;
        this.tooltipElement = null;
        this.visible = false;
        this.delayTimeout = null;
        const triggerElement = options.triggerElement;
        triggerElement.addEventListener('mouseenter', () => this.show());
        triggerElement.addEventListener('mouseleave', () => this.hide());
    }
    show() {
        clearTimeout(this.delayTimeout);
        this.delayTimeout = setTimeout(() => {
            this.tooltipElement.style.display = 'block';
            this.visible = true;
        }, this.delay);
    }
    hide() {
        clearTimeout(this.delayTimeout);
        this.delayTimeout = setTimeout(() => {
            this.tooltipElement.style.display = 'none';
            this.visible = false;
        }, this.delay);
    }
}
```

## Vue.js 中的 Tooltip 实现

在构造函数中，事件监听器会绑定到 `mouseenter` 和 `mouseleave` 事件。这里存在一个问题：这些事件监听器通常是在 `mounted` 生命周期钩子中绑定的，这意味着 `mouseenter` 和 `mouseleave` 关键字会引用到元素本身，而不是 `Tooltip` 对象。因此，`this` 关键字将无法指向正确的 `Tooltip` 实例。为了解决这个问题，你可以使用一个技巧，即创建一个新的变量（如 `that`），并将其赋值给 `this`，这样即使在事件处理函数中也能正确引用到 `Tooltip` 实例。

```javascript
var that = this;
this.tooltipElement.addEventListener('mouseenter', function() {
    that.show();
});
```

但更好的方法是使用箭头函数，因为箭头函数不会创建自己的 `this` 上下文。

```javascript
this.tooltipElement.addEventListener('mouseenter', () => {
    this.show();
});
```

### 组件化

在 Vue 中，你可以将 `Tooltip` 封装成一个独立的组件。组件接收 `content` 等属性，并通过插槽（`slot`）来定义触发元素。

```vue
<template>
  <div class="tooltip-wrapper" @mouseenter="show" @mouseleave="hide">
    <slot></slot>
    <div class="tooltip-content" v-show="visible">{{ content }}</div>
  </div>
</template>
```

### 响应式数据

使用 Vue 的响应式系统，可以非常方便地管理 `Tooltip` 的显示状态。

```javascript
data() {
  return {
    visible: false
  }
},
methods: {
  show() {
    this.visible = true;
  },
  hide() {
    this.visible = false;
  }
}
```

### 更复杂的场景

在实际应用中，`Tooltip` 的定位可能需要根据触发元素的位置动态计算。这时可以结合 `getBoundingClientRect()` 或 `Popper.js` 等库来实现。

## 总结

从早期的简单 JavaScript 实现到现代框架中的组件化方案，`Tooltip` 的核心机制始终未变：通过鼠标悬停事件来控制一个浮动层的显示与隐藏。Element UI 的 `Tooltip` 组件在此基础上增加了丰富的配置选项、动画效果和定位能力，使其能够应对各种复杂的业务场景。

理解 `Tooltip` 的实现原理，不仅有助于你在需要时自定义 `Tooltip` 行为，还能帮助你更好地理解 Vue 组件化开发的核心思想——通过封装状态和行为，提供灵活、可复用的 UI 模块。

#### Tooltip 类的 show 和 hide 方法

```javascript
clearTimeout(this.delayTimeout);

this.delayTimeout = setTimeout(() => {

    this.tooltipElement.style.display = 'block';

    this.visible = true;

}, this.delay);

}

hide() {

clearTimeout(this.delayTimeout);

this.delayTimeout = setTimeout(() => {

    this.tooltipElement.style.display = 'none';

    this.visible = false;

}, this.delay);

}

}
```

#### 第四篇：创建并管理 Tooltip DOM 元素

现在，我们需要在 `Tooltip` 类内部创建和管理对应的 Tooltip DOM 元素。

```javascript
class Tooltip {

constructor(options) {

    // ... 初始化 ...

    this.createTooltipElement();

    document.body.appendChild(this.tooltipElement);

}

createTooltipElement() {

    this.tooltipElement = document.createElement('div');

    this.tooltipElement.className = 'tooltip-box';

    this.tooltipElement.textContent = this.content;

    this.tooltipElement.style.display = 'none';

    this.tooltipElement.style.position = 'absolute';

    this.tooltipElement.style.top = '0';

    this.tooltipElement.style.left = '0';

    // ... 其他样式设置 ...

}

// ... show/hide 方法 ...

}
```

#### 第五篇：考虑 Tooltip 的定位

为了让 Tooltip 出现在正确的位置，我们需要在 `show()` 方法中动态计算其位置。

```javascript
show() {

clearTimeout(this.delayTimeout);

this.delayTimeout = setTimeout(() => {

    const triggerRect = this.triggerElement.getBoundingClientRect();

    const tooltipRect = this.tooltipElement.getBoundingClientRect();

    let top, left;

    // 根据 placement 计算位置

    if (this.placement === 'top') {

        top = triggerRect.top - tooltipRect.height - this.offset;

        left = triggerRect.left + (triggerRect.width - tooltipRect.width) / 2;

    } else if (this.placement === 'bottom') {

        top = triggerRect.bottom + this.offset;

        left = triggerRect.left + (triggerRect.width - tooltipRect.width) / 2;

    }

    // ... 其他位置处理 ...

    this.tooltipElement.style.top = top + 'px';

    this.tooltipElement.style.left = left + 'px';

    this.tooltipElement.style.display = 'block';

    this.visible = true;

}, this.delay);

}
```

#### 第六篇：处理滚动和窗口变化

当页面发生滚动或窗口大小变化时，Tooltip 的位置需要重新计算。

```javascript
class Tooltip {

constructor(options) {

    // ...

    window.addEventListener('scroll', () => this.updatePosition());

    window.addEventListener('resize', () => this.updatePosition());

}

updatePosition() {

    if (this.visible) {

        // 重新计算并设置位置

        this.show(); // 复用 show 中的定位逻辑

    }

}

}
```

#### 第七篇：最终的 Tooltip 类

综合以上所有概念，我们得到了一个功能相对完整的 `Tooltip` 类。

```javascript
class Tooltip {

constructor(options) {

    this.content = options.content || '';

    this.placement = options.placement || 'top';

    this.delay = options.delay || 0;

    this.offset = options.offset || 10;

    this.triggerElement = options.triggerElement;

    this.tooltipElement = document.createElement('div');

    this.tooltipElement.className = 'tooltip-box';

    this.tooltipElement.textContent = this.content;

    this.tooltipElement.style.position = 'absolute';

    this.tooltipElement.style.display = 'none';

    document.body.appendChild(this.tooltipElement);

    this.visible = false;

    this.delayTimeout = null;

    this.triggerElement.addEventListener('mouseenter', () => this.show());

    this.triggerElement.addEventListener('mouseleave', () => this.hide());

    window.addEventListener('scroll', () => this.updatePosition());

    window.addEventListener('resize', () => this.updatePosition());

}

show() {

    clearTimeout(this.delayTimeout);

    this.delayTimeout = setTimeout(() => {

        this.updatePosition();

        this.tooltipElement.style.display = 'block';

        this.visible = true;

    }, this.delay);

}

hide() {
```

## Tooltip 组件实现

```javascript
clearTimeout(this.delayTimeout);

this.delayTimeout = setTimeout(() => {
    this.tooltipElement.style.display = 'none';
    this.visible = false;
}, this.delay);
```

### `updatePosition()` 方法

```javascript
updatePosition() {
    if (!this.visible) return;

    const triggerRect = this.triggerElement.getBoundingClientRect();
    const tooltipRect = this.tooltipElement.getBoundingClientRect();
    let top, left;

    switch (this.placement) {
        case 'top':
            top = triggerRect.top - tooltipRect.height - this.offset;
            left = triggerRect.left + (triggerRect.width - tooltipRect.width) / 2;
            break;
        case 'bottom':
            top = triggerRect.bottom + this.offset;
            left = triggerRect.left + (triggerRect.width - tooltipRect.width) / 2;
            break;
        case 'left':
            top = triggerRect.top + (triggerRect.height - tooltipRect.height) / 2;
            left = triggerRect.left - tooltipRect.width - this.offset;
            break;
        case 'right':
            top = triggerRect.top + (triggerRect.height - tooltipRect.height) / 2;
            left = triggerRect.right + this.offset;
            break;
    }

    this.tooltipElement.style.top = top + 'px';
    this.tooltipElement.style.left = left + 'px';
}
```

### `destroy()` 方法

```javascript
destroy() {
    this.tooltipElement.remove();
    // ... 移除事件监听 ...
}
```

## 进阶：EventTooltip——Tooltip 是一种 Event？

在事件驱动的架构中，`Tooltip` 可以被视为一种事件。在 `constructor` 中，事件监听器会绑定到 `mouseenter` 和 `mouseleave` 事件。这里存在一个问题：这些事件监听器通常是在 `mounted` 生命周期钩子中绑定的，这意味着 `mouseenter` 和 `mouseleave` 关键字会引用到元素本身，而不是 `Tooltip` 对象。因此，`this` 关键字将无法指向正确的 Tooltip 实例。为了解决这个问题，你可以使用一个技巧，即创建一个新的变量（如 `that`），并将其赋值给 `this`，这样即使在事件处理函数中也能正确引用到 Tooltip 实例。

```javascript
var that = this;
this.tooltipElement.addEventListener('mouseenter', function() {
    that.show();
});
```

但更好的方法是使用箭头函数，因为箭头函数不会创建自己的 `this` 上下文。

```javascript
this.tooltipElement.addEventListener('mouseenter', () => {
    this.show();
});
```

## 代码示例

```javascript
this.style = new CoolTool(this);
return this.style;
```

### `innerHTML` 方法

```javascript
var html = "";
for (var that this) {
    html += this.element.innerHTML;
}
return html;
```

### `addEventListener` 方法

```javascript
var event = new Event("click");
this.element.addEventListener(event, function() {
    // do something
});
```

## 使用方式

现在，您已经了解了如何创建和使用 `CoolToolKit` 工具。您可以从**构造函数**和另一个 `addCoolTool` 方法中移除所有参数和属性，因为此工具会自动管理所有数据。

## 方法目的

`addCoolTool` 方法用于创建 `CoolToolKit` 对象。它使用 `addCoolTool()` 函数，并在 `start()` 函数内部调用 `factory()` 方法创建 `CoolToolKit` 对象，并使用 `addCoolTool()` 方法添加事件。

此方法有两个主要目的，每个都有两个不同的角色：
- `addCoolTool` 是工具创建方法，它使用 `addCoolTool()` 函数创建新增事件。
- `clearSize` 是清除大小的方法，它使用 `clearSize()` 函数创建 `null` 值。

## 存储大量实例

在编写此示例时，您不需要创建一个集中的数据源，因为在匿名函数中创建的所有数据都存储在外部数据中。因此，存储的实例数量会减少到一种，而不是在页面上同时显示多个窗口。

换句话说，您可以使用此工具创建任意数量的实例，而无需担心创建数千个 `CoolTool` 实例。一个更好的解决方案是使用 `dialog box`。

## 存储实例

另一个相关的情况是使用 `dialog box`。`dialog box` 非常适合使用这种模式。一个 `dialog box` 对象封装了数据和 `DOM`，但是 `dialog box` 包含更多 `DOM` 元素，因此它更重要，因为它可以最小化创建的实例数量。问题是，它可能有多个窗口，因此它很容易有多个窗口，但您不知道您需要多少，因为您无法确定您需要多少窗口。现在，您可以根据需要创建任意数量的工具实例，而不必担心拥有数千个工具窗口。

一个相关的良好做法是使用 `dialog box`。一个 `dialog box` 对象封装了数据和 `DOM`，它包含许多 `DOM` 元素，而且它甚至可以包含多个 `dialog box`。问题是，它可能有多个窗口，因此可能超过一个窗口。现在，你可以创建任意数量的工具实例，而无需担心拥有数千个实例。

---

## 主要概念

`Dialog` 类用于在屏幕上创建一个对话框。对话框通常用于显示信息、请求用户输入或确认操作。它们通常是模态的，这意味着在对话框关闭之前，用户无法与应用程序的其他部分进行交互。

对话框是一种重量级的 UI 元素，它需要比标准 `View` 对象更多的资源来创建。因此，您只应在确实必要时才创建和使用它们。如果您只需要向用户显示一条简单的消息或提示，请考虑使用 `Toast` 或 `Snackbar` 这类轻量级替代方案。

`Dialog` 类的实现细节并非直接相关。您只需知道它资源密集，应谨慎使用。

对于 `Dialog` 类，您需要了解 `show()`、`hide()` 和 `dismiss()` 方法，以及 `onCreateDialog(Bundle)`、`onPrepareDialog(Bundle)` 和 `onSaveInstanceState(Bundle)` 这些回调方法。

## DialogFragment

### 为什么使用 DialogFragment？

`DialogFragment` 类提供了 `Dialog` 类的一种实现，该实现受 `FragmentManager` 的 `show()`、`hide()` 和 `save()` 生命周期管理。

`DialogFragment` 在 `Dialog` 之上提供了一个接口，用于确定 `Dialog` 是否应可见并确定其状态。这意味着 `DialogFragment` 的 `show()` 方法不仅可以显示 `Dialog`，还可以对其状态进行精细控制。

### DialogFragment 的主要组件

`DialogFragment` 的实现细节由三个方法定义：一个用于显示 `Dialog`、一个用于检查当前有多少个 `Dialog` 对象正在使用中，以及一个用于存储实例化的 `Dialog` 对象。这些组件将被打包在一个 `Singleton` 中，以确保在任一时刻只有一个 `DialogFragment` 管理器存在。

## 创建 DialogFragment

一个 `DialogFragment` 可以像普通的 `Fragment` 一样被实例化，并作为 Activity 生命周期的一部分被管理。它的行为与普通 `Fragment` 基本相同，但有一些重要的区别。

- `DialogFragment` 必须始终创建为全屏对话框，并且在旋转设备时会被销毁并重新创建。
- 除非重写了 `onCreateView()` 方法，否则 `DialogFragment` 总会创建一个 `Dialog` 窗口来放置它的 `View`。
- 由 `DialogFragment` 的 `onCreateView()` 方法创建的 `View` 会被自动放入一个 `Dialog` 窗口中。

## DialogFragment 示例

### DialogFragment 的三种方法

在 `DialogFragment` 接口中定义了三种方法：

- `show()`：显示一个 `Dialog` 对象。
- `hide()`：隐藏一个 `Dialog` 对象。
- `save()`：保存 `DialogFragment` 的当前状态并返回一个可供 `show()` 方法使用的 `Dialog` 对象。

### 示例解析

这个示例比之前的工具示例更复杂，但使用了相同的原则。通过使用 `DialogFragment` 的 `show()`、`hide()` 和 `save()` 方法，您可以轻松地管理 `Dialog` 对象及其状态。

- `mManager` 变量用于创建一个新的 `DialogFragment` 对象。
- `mManager.show(mDialog)`：显示一个 `Dialog` 对象。
- `mManager.hide(mDialog)`：隐藏一个 `Dialog` 对象。
- `mManager.save(mDialog)`：保存 `Dialog` 对象及其状态，以便它可以被恢复。

创建的数组存储了已经实例化的 `Dialog` 对象，以便它们可以被重用。

以下是一个示例，演示了如何使用 `DialogFragment` 创建和管理 `Dialog`：

```java
DialogFragment mManager = new DialogFragment();

// 创建并显示一个 Dialog
Dialog mDialog = mManager.show();
mDialog.setTitle("示例对话框");
mDialog.setMessage("这是一个简单的对话框。");
mManager.show(mDialog);

// 隐藏 Dialog
mManager.hide(mDialog);

// 保存 Dialog 状态
mManager.save(mDialog);

// 稍后重新显示 Dialog
mManager.show(mDialog);
```

# 享元模式

“只用最少的代码，以最快的方式对数据进行预处理。如果所有这三个条件都满足，那么你的程序就是使用**享元模式**来优化性能的绝佳候选。

下一个条件是，它旨在存储大量数据结构，每个对象都对应其中一种结构。

只需能够做到扩展。这意味着你必须能够将某些内部数据存储在对象外部，并将其作为参数传递给方法。此外，将数据存储在外部应该消耗更少的资源；否则，你实际上并不会提升性能。

如果一个对象包含大量基础架构代码和`REST`服务，那么它很可能成为此优化的良好候选者。如果它仅仅是数据的容器以及访问这些数据的方法，那么结果就不会那么理想。

最后一个条件是，一旦移除了外部数据，你必须只剩下相对少量的唯一对象。最佳情况是，你只剩下一个唯一的对象，就像日历和工具提示示例那样。并非总是能够将实例数量减少到一个，但你应该尽量最终拥有尽可能少的对象唯一实例。当需要多个副本时，这一点尤其重要，例如对话框示例。

`享元模式` `开启`

`通用` `模式`

`UML`

实现`享元`模式的通用步骤

如果所有这三个条件都满足，那么你的程序就是使用享元模式进行优化的良好候选者。几乎所有享元模式的实现都遵循相同的通用步骤：

1. 从目标类中剥离所有外部数据。通过尽可能多地移除类中的属性来完成此操作；这些属性应该是那些不同实例间会发生变化的属性。构造函数的参数也是如此。这些参数应该改为添加到类的方法中。这些数据将不再存储在类内部，而是由管理器传入。该类应该仍能执行与之前相同的功能。唯一的区别是数据来自不同的地方。

2. 创建一个工厂来控制类的实例化方式。这个工厂需要跟踪所有已创建该类的唯一实例。一种实现方法是将每个对象的引用保存在一个对象字面量中，并以用于创建它的唯一参数集合作为索引。这样：当请求一个对象时，工厂可以先检查这个对象字面量，看看这个特定请求是否之前已经发出过。如果是，它只需返回对已存在对象的引用。如果不是，它将创建一个新实例，将其引用存储在对象字面量中，并返回它。

另一种技术，即对象池技术，使用一个数组来保存对已实例化对象的引用。如果重要的是可用对象的数量，而不是唯一配置的实例，那么这种方法很有用。对象池可用于将实例化的对象数量保持在最低限度。工厂根据内部数据处理创建对象的所有方面。

3. 创建一个管理器来存储外部数据。管理器对象控制着处理外部数据的所有方面。在实现优化之前，每次需要目标类时你都会创建它的新实例，并传入所有数据。现在，每次你需要一个实例时，你将调用管理器的一个方法，并将所有数据传递给它。这个方法确定哪些是内部数据，哪些是外部数据。内部数据被传递给工厂对象，以便可以创建或重用对象（如果该对象已存在）。外部数据则存储在管理器内部的数据结构中。然后，管理器根据需要将这些数据传递给共享对象的方法，从而获得与类有许多实例时相同的效果。

`享元`模式的优点

享元模式可以将页面的资源负载降低几个数量级。在工具提示的示例中，它创建的`ToolTip`对象数量和`REST`元素数量都显著减少。”

# 飞行器模式

飞行器模式是游戏内的一种工具提示，用于显示游戏中特定工具提示的文本。它通常用于工具提示的上下文提示。它仍然可能获取一些有意义的信号。

这是针对一个大型文本上下文的简单说明。应用程序的潜在安全风险在于，如果代码未被正确使用，可能会泄露敏感信息。

飞行器模式是仅用于可视化场景的优化模式。它只用于视觉提示，以提高代码的效率和兼容性。

它主要用于内存优化。它是通过获取一个现有类并将其剥离掉所有数据来实现的。每个该类的唯一实例然后成为一个资源，在多个位置之间共享。

单个飞行器对象占据了许多原始对象的位置。对于飞行器对象来说，像这样被共享，几个新的对象必须被添加。一个工厂对象需要控制类如何被实例化并且限制创建的实例数量达到绝对最小值。

它也应该存储先前创建的实例，以便在稍后需要相似对象时重用它们。一个管理器对象需要存储外部数据并将其传入飞行器的方法。在这种方式中，原始类的方法可以被保留，同时大大减少所需副本的数量。

## 表面

### 飞行器模式的说明

它并不需要巨大的变化到你的代码来获得这些信号。一旦你创建了管理器、工厂和飞行器，你唯一必须对你的代码做的改变就是调用管理器对象的一个方法，而不是实例化该类直接。

如果你正在创建飞行器用于其他程序员以 API 使用，他们只需要稍微改动他们调用它的方式。这就是模式真正闪耀的地方——如果你做出这个优化到你的 API 一次，它将为每个使用它的人更加高效。

当你使用这个优化用于一个库时，该库被用于整个站点，你的用户可能会注意到一个巨大的改进速度。这是一个仅用于优化模式的简单优化。它并不能，也不应该，被用于任何条件集合。它可以实际地让你的代码更不高效，如果不必要地使用的话。

为了优化你的代码，这个模式增加了复杂性，这使得它更难调试和维护。

它也更棘手地追踪下游数据问题，因为它并不总是清除数据碎片来自哪里。如果工具提示正在显示错误的文本，这是因为错误文本被传入，或者因为它是一个共享资源并且它忘记了从它最后使用清除文本。这些类型的错误可能是昂贵的。

维护也可能更困难因为这种优化。而不是拥有一个干净的对象结构封装数据，你现在有一个碎片化的消息，数据被存储在至少两个地方。它重要的是文档为什么特定的数据是内在或外在的，例如一个区别可能会丢失在那些维护你的代码之后。

这些缺点是并非致命破坏者——它们只是意味着这种优化应该只在需要时使用。权衡必须在运行时间效率和可维护性之间。在这样的情况下，如果你不确定是否需要飞行器，它很可能不是。

## 总结

在本章中，我们讨论了飞行器模式的架构、使用和好处。它仅仅是一个优化模式，用于提高性能和让你的代码更高效，特别是它的使用内存。它通过获取一个现有类并剥离掉所有外部数据来实现。每个该类的唯一实例然后成为一个资源，在多个位置之间共享。

对于飞行器对象来说，像这样被共享，几个新的对象必须被添加。一个管理人员对象需要控制类如何被实例化并且限制创建的实例数量达到绝对最小值。

它也应该存储先前创建的实例，以便在稍后需要相似对象时重用它们。一个工厂对象需要对外部数据并将其传入飞行器的方法。在这种方式中，原始类的方法可以被保留，同时大大减少所需副本的数量。

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

# REST API 与代理模式

## REST API

这些调用可以封装在一个 `REST` 接口中，这样可以避免代码重复，并防止在调用接口时返回多个回调函数。

这样，每个调用都可以使用自己的回调函数，同时采用相同的模式来获取数据，并且可以使用其他代码来处理返回的结果，例如为返回结果进行 `XML` 调用，并显示在页面上。

成功封装后，您只需要返回：

```javascript
var callBack = function(responseText) {
    var stats = eval(responseText); // 解析 JSON 数据
    displayPageviews(stats.getPageviews); // 显示页面访问量
};

var urls = getUniques();
var success = function(){
    var stats = eval(responseText); // 解析 JSON 数据
    displayPageviews(stats.getBrowsers); // 显示浏览器统计
};
```

### 使用 REST 请求的优势

一个标准的 `REST` 格式请求，能让您方便地使用不同类型的请求资源。

1.  **定义接口**：使用一个通用的 `HTTP` 请求来处理 HTML 格式，并与现有数据匹配，这样能让请求更加符合逻辑，并且只需要一个请求接口来获取数据。
2.  **简化操作**：`REST` 将复杂的代码逻辑封装到一个简单的调用中，让您能够快速完成调用。
3.  **提高性能**：使用 `REST` 可以避免多次请求相同数据，并能缓存数据，从而提升应用性能。

HTTP 请求示例：

```javascript
var callback = function(responseText) {
    var stats = JSON.parse(responseText);
    DisplayPageviews(stats.getPageviews);
};
```

REST 的调用非常简单，只要定义好回调函数，就可以通过 `GET` 请求来获取数据。

```javascript
var url = "http://www.mydomain.com/stats/getPageviews";
xmlHttp.open("GET", url, true);
xmlHttp.send();
```

### 总结

`REST` API 是一种简单、高效的 Web 服务接口，它能够帮助您快速实现统计数据的获取和处理。通过使用 `REST`，您可以：
- 简化代码
- 提高性能
- 降低复杂性
- 提升开发效率

---

# 代理模式

## 问题

`ProxyPatternExample` 类中，`main` 方法用于演示代理模式。

## 解决方案

`ProxyPatternExample` 类中的 `main` 方法将演示如何创建和使用代理。

### 主要组件

- `Subject` 接口
- `RealSubject` 类
- `Proxy` 类

## 实现

### 1. 创建 `Subject` 接口

`Subject` 接口定义了一个 `request()` 方法。

### 2. 创建 `RealSubject` 类

`RealSubject` 类实现了 `Subject` 接口，并提供了 `request()` 方法的具体实现。

### 3. 创建 `Proxy` 类

`Proxy` 类也实现了 `Subject` 接口，它包含一个对 `RealSubject` 对象的引用。`Proxy` 的 `request()` 方法在调用 `RealSubject` 的 `request()` 方法前后，可以执行额外的操作，例如访问控制或日志记录。

### 4. 客户端代码

在 `ProxyPatternExample.main()` 中，我们创建一个 `Proxy` 对象，然后调用其 `request()` 方法。

## 示例

以下是一个使用代理模式访问 Web 服务的示例。该实现使用了 `java.net.http.HttpClient` 和 `java.net.http.HttpRequest` 等类。

### 使用通用模式创建您自己的代理

这个示例可以抽象出来，创建一个更通用的模式，该模式可用于包装任何 Web 服务。您的实现的具体细节将根据 Web 服务的类型而有所不同，但这个通用模式应该提供一个用于创建您自己代理的通用框架。

### 示例说明

在此示例中，您可以通过使用异步调用和回调函数（而不是阻塞和等待结果）来减轻此问题。但这需要一个回调函数，该函数会暴露底层实现的一些细节，这可能会使代码更复杂。因此，这可能并非总是必要的，特别是如果您不打算使用外部服务时。

您可以通过使用 `async` 和 `await` 关键字以同步方式编写异步代码，从而使这个问题得到缓解。在这个示例中，您可以通过使用 `Callable` 和 `Future` 接口来异步地发起调用，而不是使用 `Callable.call()` 方法进行阻塞和等待。

但是，这需要一个 `Callable` 函数，它会暴露底层实现的一些细节，这可能会使代码更复杂。因此，这可能并非总是必要的，特别是如果您不打算使用外部服务时。

## 讨论

### 通用模式：用于包装 Web 服务的代理

此示例可以抽象出来，创建一个更通用的模式，该模式可用于包装任何 Web 服务。您的实现的具体细节将根据 Web 服务的类型而有所不同，但这个通用模式应该提供一个用于创建您自己代理的通用框架。

## 代理模式的高级应用

### 延迟加载

代理模式最著名的用途之一是实现延迟加载。如果一个服务对象的创建和初始化代价很高，代理可以在真正需要时才创建该对象。

### 访问控制

代理可以控制对真实主题的访问。例如，只有特定用户才能访问某些方法。

### 日志记录

代理可以记录对真实主题方法的所有调用，这对于调试和审计非常有用。

### 远程代理

代理可以代表位于不同地址空间（例如，另一台服务器）的对象。本地代理会处理网络通信细节，让客户端像调用本地对象一样调用远程对象。

## 使用 `FetchUtil` 进行数据抓取

`FetchUtil` 类中的静态方法 `fetchData` 用于从给定 URL 获取数据。`fetchData` 方法接受一个 `URL` 字符串，并返回一个 `String` 类型的结果。

```javascript
var for DataFetch getArry Size > 0
url = xyz+abc+getArry.length > getArry.getSize()
```

`FetchUtil` 类中的 `fetchData` 方法使用 `HttpClient` 发送请求并获取响应。它使用 `DataCallBack` 和 `ErrorCallBack` 来处理成功和失败的情况。

```javascript
var that = this
var callBack = successCallback
var errorCallback = errorCallback
dataCallBack.onSuccess(result)
```

```javascript
var that = this
var callBack = errorCallBack
```

## 使用通用模式

要使用这个通用模式，只需创建您自己的类并扩展它以适配 `ServiceProxy`。

### 示例：`StatsProxy` 类

```javascript
var StatsProxy = function()
this.Handler = throw new Error()
```

```javascript
var for that = this
var callBack = successCallback
var callBack = errorCallback
getArry.push(varName)
var success = successCallback
var error = errorCallback
dataCallBack.onSuccess(200, "OK")
```

```javascript
if (getArry.length > 0)
url = xyz+abc+getArry.length > getArry.getSize()
```

```javascript
Handler.request().then(function(response) {
    var callBack = successCallback
    var callBack = errorCallback
})
```

### 示例：`CacheProxy` 类

```javascript
var CacheProxy = function()
this.Handler = throw new Error()
```

```javascript
var for that = this
```

# 目录

# 概述

版本 1.0.0

## 安装

### 前提条件

- 操作系统：Linux、macOS 或 Windows
- 已安装 `Node.js` 和 `npm`

### 安装步骤

1. 打开终端（或命令提示符）。
2. 运行以下命令安装 `create-person-directory`：
   ```
   npm install -g create-person-directory
   ```

### 验证安装

运行以下命令检查是否安装成功：
```
person-directory --version
```

### 更新

要更新到最新版本，请运行：
```
npm update -g create-person-directory
```

## 快速入门

### 创建项目

使用 `create-person-directory` 命令创建一个新的 PersonDirectory 项目：
```
create-person-directory my-person-app
```

### 启动开发服务器

进入项目目录，运行：
```
cd my-person-app
npm start
```

### 访问应用

打开浏览器，访问 `http://localhost:3000`。

## 主要功能

### 添加人员

在 `PersonDirectory` 应用的界面上，点击“添加人员”按钮，填写姓名、年龄和联系方式后提交。

### 查看人员列表

在主页上，可以看到所有已添加人员的卡片列表，包含姓名、年龄和联系方式。

### 编辑人员信息

点击每个人员卡片上的“编辑”按钮，可以修改该人员的信息。

### 删除人员

点击每个人员卡片上的“删除”按钮，可以移除该人员。

### 使用示例

### 命令行工具

#### 使用 `person-directory` 命令

在终端中运行以下命令查看帮助：
```bash
person-directory --help
```

### 代码示例

#### 创建 PersonDirectory 实例

```javascript
const personDirectory = new PersonDirectory();
```

#### 添加人员

```javascript
personDirectory.addPerson({ name: "张三", age: 25, email: "zhangsan@example.com" });
```

#### 获取所有人员

```javascript
const allPersons = personDirectory.getAllPersons();
console.log(allPersons);
```

#### 根据姓名查找人员

```javascript
const person = personDirectory.findPersonByName("张三");
console.log(person);
```

# 结论

代理模式是一种非常强大且灵活的设计模式，用于控制对其他对象的访问。通过使用代理，您可以在不修改原始对象的情况下添加额外的功能，例如延迟加载、访问控制、日志记录等。

## 附录

### 使用 `FetchUtil` 的通用模式

`FetchUtil` 类中的静态方法 `fetchData` 提供了一个用于进行 HTTP 请求的通用模式。它可以被抽象出来，以便用于任何数据获取场景。

#### `ServiceProxy` 类

```
var ServiceProxy = function()
this.Handler = throw...new...Error...Sync...Request...for...state...failed.
```

```
var for that = this
var callBack = successCallBack()
var callBack = errorCallBack()
getArry.push(varName)
var success = successCallBack()
var error = errorCallBack()
dataCallBack.onSuccess(200, "OK")
```

```
if (getArry.length > 0)
url = xyz + abc + getArry.length > getArry.getSize()
```

### `ServiceProxy` 原型

```
ServiceProxy.prototype.constructor = function()
this.Handler = throw new Error()
```

```
ServiceProxy.prototype.request = function()
url = xyz + abc + getArry.length > getArry.getSize()
Handler.request().then(callback).catch(errorCallback)
```

### 使用示例

要使用这个通用模式，只需创建您自己的类并扩展它以适配 `ServiceProxy`。

### 示例：`LoggingProxy` 类

```
var LoggingProxy = function()
this.Handler = throw new Error()
```

```
var for that = this
var callBack = successCallback
var callBack = errorCallback
log.push(varName)
var success = successCallback
var error = errorCallback
dataCallBack.onSuccess(200, "OK")
```

### 示例：`RateLimitingProxy` 类

```
var RateLimitingProxy = function()
this.Handler = throw new Error()
```

```
var for that = this
var callBack = successCallback
var callBack = errorCallback
rateLimit.push(varName)
var success = successCallback
var error = errorCallback
dataCallBack.onSuccess(200, "OK")
```

### 示例：`DistributedCacheProxy` 类

```
var DistributedCacheProxy = function()
this.Handler = throw new Error()
```

```
var for that = this
var callBack = successCallback
var callBack = errorCallback
distributedCache.push(varName)
var success = successCallback
var error = errorCallback
dataCallBack.onSuccess(200, "OK")
```

# API 参考

## 构造函数

### `new PersonDirectory()`

创建一个新的 `PersonDirectory` 实例。

### `new PersonDirectory(config)`

使用配置对象 `config` 创建一个新的 `PersonDirectory` 实例。配置对象可以包含以下属性：
- `data`（可选）：初始人员数据数组。
- `currentPage`（可选）：当前页码，默认为 `1`。
- `pageSize`（可选）：每页显示的人员数量，默认为 `10`。

### `new PersonDirectory(config, parent)`

创建一个新的 `PersonDirectory` 实例，并将其挂载到 `parent` 元素下。

## 方法

### `addPerson(personData)`

添加一个新的人员。`personData` 应是一个包含 `name`、`age` 和 `email` 属性的对象。成功时返回 `true`，失败时返回 `false`。

### `getAllPersons()`

返回所有人员的数组。每个人员对象包含 `name`、`age` 和 `email` 属性。如果没有人员，则返回空数组。

### `findPersonByName(name)`

根据姓名查找人员。如果找到，返回该人员对象；否则返回 `null`。

### `deletePerson(name)`

根据姓名删除人员。成功时返回 `true`，失败时返回 `false`。

## 事件

### `personAdded`

当添加一个新的人员时触发。回调函数接收新添加的人员对象作为参数。

### `personDeleted`

当删除一个人员时触发。回调函数接收被删除的人员对象作为参数。

# 配置

## 配置属性

### `config.apiUrl`

设置 API 的基础 URL。默认为 `"http://localhost:3000/api"`。

### `config.timeout`

设置请求超时时间（毫秒）。默认为 `5000`。

### `config.retryCount`

设置请求失败时的重试次数。默认为 `3`。

## 配置示例

```javascript
const config = {
  apiUrl: "https://my-api.example.com/v2",
  timeout: 10000,
  retryCount: 5
};

const directory = new PersonDirectory(config);
```

# 错误处理

## 常见错误

### 网络错误

如果网络连接失败，`addPerson()` 方法会抛出一个 `NetworkError`。您可以使用 `try...catch` 捕获它。

### 数据验证错误

如果 `personData` 缺少必填字段（如 `name`），`addPerson()` 方法会返回 `false`，并设置 `errorMessage` 属性为“缺少姓名”。

## 错误处理示例

```javascript
try {
  const result = directory.addPerson({ name: "李四" });
  if (!result) {
    console.error(directory.errorMessage);
  }
} catch (error) {
  console.error("发生错误：", error);
}
```

# 常见问题 FAQ

## Q: 如何导入 PersonDirectory？

A: 您可以使用 `require` 或 `import` 语句：

```javascript
const PersonDirectory = require('person-directory');
// 或者
import PersonDirectory from 'person-directory';
```

## Q: 是否可以同时添加多个人员？

A: 目前不支持批量添加。您需要多次调用 `addPerson()` 方法，每次传入一个人员对象。

## Q: 人员数据存储在哪里？

A: 默认情况下，人员数据存储在内存中。如果您需要持久化存储，可以配置 `apiUrl` 连接后端数据库。

## Q: 如何自定义样式？

A: 您可以通过 CSS 覆盖默认样式。所有元素的类名都以 `pd-` 开头，例如 `.pd-card`、`.pd-button`。

# 发布说明

## 版本 1.0.0（初始版本）

- 初始发布，提供核心的人员管理功能。

# DirectoryProxy 类

`DirectoryProxy` 是一个 `Directory` 类的代理。

## 概述

`DirectoryProxy` 类是一个在真实 `Directory` 对象上的代理。每个方法调用都简单地将相同的方法调用委托给真实的 `Directory` 对象。

`DirectoryProxy` 类是一个虚拟代理。`Directory` 构造函数是一个昂贵的操作，因此它会在第一次调用 `show()` 方法之前延迟执行。在 `Directory` 被初始化之前，所有方法调用都会被阻塞。第一次调用 `show()` 方法时会创建真实的 `Directory` 对象。

`DirectoryProxy` 构造函数不再实例化 `Directory`。它只是被告知 `Directory` 正在被加载并阻塞所有方法调用，直到真实的 `Directory` 被创建。

然而，在这种配置下，你无法获得 `Directory` 类的任何优势。你唯一能做的就是通知用户 `Directory` 正在加载并阻塞所有方法调用，直到真实的 `Directory` 被创建。实际上，你无法获得 `Directory` 类的任何好处——你只是用一个更简单的技术替换了它。你不会得到任何初始化的检查或更准确的触发器。

实际上，`DirectoryProxy` 只是简单地查询 `Directory` 的 `currentPage` 属性。如果它为空，`DirectoryProxy` 就知道 `Directory` 还没有被初始化。然而，这只是该类型代理的一个非常简单的示例。更复杂的版本可以实现更健壮的初始化检查以及更多准确的触发器。每个代理都将取决于你与用户的精确交互，你期望的交互方式也会有所不同。

接下来，我们将介绍一个动态虚拟代理，它可以被用作创建你自己的代理的模板。

## 一般模式：`DirectoryProxy` 类

`DirectoryProxy` 是一个 `Directory` 类，带有加载消息。它是一个非常简单的示例，说明这种类型的代理如何工作。更复杂的版本可以实现更多的初始化检查，并包含更多能够触发 `Directory` 初始化的用户交互。

### `DirectoryProxy` 类（包含 `show()` 方法）

```javascript
show() {
  if (this._initialized) {
    return this._show();
  }

  if (this._parent.this._warning) {
    return null;
  }

  this._initialized = true;
  this._interval = setInterval(
    CheckInitialization.this._checkInitialization,
    true
  );
}
```

# 显示页面

`show()` 函数返回 `this._show()`。

- 如果 `this._initialized` 为真，则返回 `this._show()`。
- 如果 `this._parent.this._warning` 为真，则返回 `null`。
- 否则，设置 `this._initialized` 为真，并设置 `this._interval` 为 `CheckInitialization.this._checkInitialization`，传入 `true`。

# 初始化检查

`CheckInitialization()` 函数检查 `this._director._currentPage` 是否不为空。

- 如果为空，则清除 `this._interval` 并移除 `this._warning`。
- 然后设置 `this._initialized` 为真。

# 阻塞方法调用

`block()` 函数会在 `this._director._currentPage` 为空时阻塞所有方法调用。

# 警告

`warning()` 函数返回 `this._parent.this._warning`。

- 如果 `this._initialized` 为真，则返回 `this._show()`。
- 否则，返回 `this._warning`。

# 父元素中的警告

`this._parent.this._warning` 是一个 `div` 元素，用于显示警告消息。它会被设置为 `"Loading..."`。

- 如果 `this._initialized` 为真，则返回 `this._show()`。
- 否则，返回 `this._warning`。

# 移除子元素

`removeChild()` 函数用于移除 `this._warning` 子元素。

# 间隔

`this._interval` 用于定期检查 `CheckInitialization.this._checkInitialization`。

# 初始化和检查

如果 `this._initialized` 为假，则设置 `this._initialized` 为真，并调用 `CheckInitialization.this._checkInitialization`。

# 父元素

`this._parent` 用于访问父元素。

# 初始化状态

- 如果 `this._initialized` 为真，则返回 `this._show()`。
- 否则，返回 `this._warning`。

# 设置间隔

`setInterval` 用于设置检查 `CheckInitialization.this._checkInitialization` 的间隔。

```
if (this._initialized) {
  return this._show();
}
```

# 返回

返回 `this._show()`。

# 显示页面

`show()` 函数返回 `this._show()`。

# 阻塞方法调用

`block()` 函数用于阻塞所有方法调用。当 `this._director._currentPage` 为空时，它会阻塞所有方法调用。

# 警告

如果 `this._initialized` 为真，则返回 `this._show()`。否则，返回 `this._warning`。

`this._parent.this._warning` 是一个 `div` 元素，用于显示警告消息。它会被设置为 `"The company directory is loading, please wait..."`，并显示在父元素中。

# 父元素

`this._parent` 用于访问父元素。

# 移除子元素

`removeChild()` 函数用于移除 `this._warning` 子元素。

`this._parent.removeChild(this._warning)` 用于移除警告。

# 设置间隔

`setInterval` 用于设置检查 `CheckInitialization.this._checkInitialization` 的间隔。

`this._interval` 用于定期检查 `CheckInitialization.this._checkInitialization`。

# 检查初始化

`CheckInitialization()` 函数检查 `this._director._currentPage` 是否不为空。如果为空，则清除 `this._interval` 并移除 `this._warning`。然后设置 `this._initialized` 为真。

如果 `this._director._currentPage` 不为空，则清除 `this._interval` 并移除 `this._warning`。然后设置 `this._initialized` 为真。

# 初始化和检查

如果 `this._initialized` 为假，则设置 `this._initialized` 为真，并调用 `CheckInitialization.this._checkInitialization`。

# 初始化状态

如果 `this._initialized` 为真，则返回 `this._show()`。否则，返回 `this._warning`。

# 控制流

```
if (this._initialized) {
  return this._show();
}
```

```
if (this._initialized)
  return this._show()
```

# 显示页面

`show()` 函数返回 `this._show()`。

# 阻塞方法调用

`block()` 函数会在 `this._director._currentPage` 为空时阻塞所有方法调用。

# 父元素中的警告

`this._parent.this._warning` 是一个 `div` 元素，用于显示警告消息。它会被设置为 `"Loading..."` 或 `"The company directory is loading, please wait..."`。

如果 `this._initialized` 为真，则返回 `this._show()`。否则，返回 `this._warning`。

# 移除子元素

`removeChild()` 函数用于移除 `this._warning` 子元素。

# 间隔

`this._interval` 用于定期检查 `CheckInitialization.this._checkInitialization`。

# 检查初始化

如果 `this._director._currentPage` 不为空，则清除 `this._interval` 并移除 `this._warning`。然后设置 `this._initialized` 为真。

# 初始化和检查

如果 `this._initialized` 为假，则设置 `this._initialized` 为真，并调用 `CheckInitialization.this._checkInitialization`。

# 警告

如果 `this._initialized` 为真，则返回 `this._show()`。否则，返回 `this._warning`。

# 父元素

`this._parent` 用于访问父元素。

# 初始化状态

如果 `this._initialized` 为真，则返回 `this._show()`。否则，返回 `this._warning`。

# 设置间隔

`setInterval` 用于设置检查 `CheckInitialization.this._checkInitialization` 的间隔。

# 控制流

```
if (this._initialized)
  return this._show()
```

# 显示页面

`show()` 函数返回 `this._show()`。

# 阻塞方法调用

`block()` 函数会在 `this._director._currentPage` 为空时阻塞所有方法调用。

# 父元素中的警告

`this._parent.this._warning` 是一个 `div` 元素，用于显示警告消息。它会被设置为 `"Loading..."` 或 `"The company directory is loading, please wait..."`。

如果 `this._initialized` 为真，则返回 `this._show()`。否则，返回 `this._warning`。

# 移除子元素

`removeChild()` 函数用于移除 `this._warning` 子元素。

# 间隔

`this._interval` 用于定期检查 `CheckInitialization.this._checkInitialization`。

# 检查初始化

如果 `this._director._currentPage` 不为空，则清除 `this._interval` 并移除 `this._warning`。然后设置 `this._initialized` 为真。

# 初始化和检查

如果 `this._initialized` 为假，则设置 `this._initialized` 为真，并调用 `CheckInitialization.this._checkInitialization`。

# 警告

如果 `this._initialized` 为真，则返回 `this._show()`。否则，返回 `this._warning`。

# 父元素

`this._parent` 用于访问父元素。

# 初始化状态

如果 `this._initialized` 为真，则返回 `this._show()`。否则，返回 `this._warning`。

# 设置间隔

`setInterval` 用于设置检查 `CheckInitialization.this._checkInitialization` 的间隔。

# 控制流

```
if (this._initialized)
  return this._show()
```

# 显示页面

`show()` 函数返回 `this._show()`。

# 阻塞方法调用

`block()` 函数会在 `this._director._currentPage` 为空时阻塞所有方法调用。

# 父元素中的警告

`this._parent.this._warning` 是一个 `div` 元素，用于显示警告消息。

它会被设置为 `"Loading..."` 或 `"The company directory is loading, please wait..."`。

如果 `this._initialized` 为真，则返回 `this._show()`。

否则，返回 `this._warning`。

# 移除子元素

`removeChild()` 函数用于移除 `this._warning` 子元素。

# 间隔

`this._interval` 用于定期检查 `CheckInitialization.this._checkInitialization`。

# 检查初始化

如果 `this._director._currentPage` 不为空，则清除 `this._interval` 并移除 `this._warning`。然后设置 `this._initialized` 为真。

# 初始化和检查

如果 `this._initialized` 为假，则设置 `this._initialized` 为真，并调用 `CheckInitialization.this._checkInitialization`。

# 警告

如果 `this._initialized` 为真，则返回 `this._show()`。

否则，返回 `this._warning`。

# 父元素

`this._parent` 用于访问父元素。

# 初始化状态

如果 `this._initialized` 为真，则返回 `this._show()`。

否则，返回 `this._warning`。

# 设置间隔

`setInterval` 用于设置检查 `CheckInitialization.this._checkInitialization` 的间隔。

```
if (this._initialized)
  return this._show()
```

# 控制流

```
if (this._initialized)
  return this._show()
```

# 显示页面

`show()` 函数返回 `this._show()`。

# 阻塞方法调用

`block()` 函数会在 `this._director._currentPage` 为空时阻塞所有方法调用。

# 父元素中的警告

`this._parent.this._warning` 是一个 `div` 元素，用于显示警告消息。

## 警告内容

它会被设置为 `"Loading..."` 或 `"The company directory is loading, please wait..."`。

## 初始化状态检查

如果 `this._initialized` 为真，则返回 `this._show()`。

否则，返回 `this._warning`。

# 移除子元素

`removeChild()` 函数用于移除 `this._warning` 子元素。

# 间隔

`this._interval` 用于定期检查 `CheckInitialization.this._checkInitialization`。

# 检查初始化

如果 `this._director._currentPage` 不为空，则清除 `this._interval` 并移除 `this._warning`。然后设置 `this._initialized` 为真。

# 初始化和检查

如果 `this._initialized` 为假，则设置 `this._initialized` 为真，并调用 `CheckInitialization.this._checkInitialization`。

# 警告

如果 `this._initialized` 为真，则返回 `this._show()`。

否则，返回 `this._warning`。

# 父元素

`this._parent` 用于访问父元素。

# 初始化状态

如果 `this._initialized` 为真，则返回 `this._show()`。

否则，返回 `this._warning`。

# 设置间隔

`setInterval` 用于设置检查 `CheckInitialization.this._checkInitialization` 的间隔。

```
if (this._initialized)

  return this._show()
```

# 动态代理

代理模式是面向对象编程中常见的设计模式。它允许为某个对象提供一个替代物，来控制对该对象的访问。

当存在一个类，它有一些方法，这些方法在执行前后需要执行一些额外操作时，代理模式就派上了用场。例如，在调用某个方法之前进行安全检查，或者在方法执行完成后进行一些清理工作。

`DynamicProxy` 类正是为此设计的。它的主要作用是动态地为真实对象的方法调用添加额外的行为。代理模式允许我们在不修改真实对象本身的情况下，为其添加额外的功能。

## `DynamicProxy` 类的 `newProxyInstance()` 方法

`DynamicProxy.newProxyInstance()` 是一个静态工厂方法，用于创建动态代理实例。它接受三个参数：类加载器、要代理的接口列表和调用处理器。

当调用者通过动态代理调用方法时，该调用会被转发到调用处理器的 `invoke()` 方法。调用处理器可以决定如何处理这次调用。例如：

- 在执行被代理方法之前，检查 `isInitialized` 是否已经为 `true`。
- 如果调用确实到达了真实对象，且检查通过，就执行真实对象的方法。
- 还可以进行其他操作，比如记录日志、权限校验等。

## `DynamicProxy` 的核心概念

`DynamicProxy` 是一个抽象类。它需要被继承并配置才能正常工作。

## 示例：`TestProxy` 类

让我们看看 `DynamicProxy` 在实际中是如何工作的：

```java
public class TestProxy<SELF extends TestProxy<SELF>> {

    public final String arg0 = null;

    public final SELF initialized = null;

    public final SELF subject = null;

    public SELF intervar = setInterval(setInterval.super)

    if (type == this.class.class.protokey) {

        return setInterval.super

    }

    if (type == clearInterval.super) {

        return clearInterval.super

    }

    return clearInterval.super

    return this.subject.arguments

}
```

现在，我们添加代码到构造器中，使其动态地为真实对象中的每个方法创建代理方法。这段代码看起来与动态代理的代码非常相似，但有几个重要的区别：

1.  `DynamicProxy` 抽象类：`abstract class DynamicProxy`
2.  类有一个 `for (DynamicProxy.arguments)` 方法：
    -   `this.arg0` = `Function(args)`
    -   `this.initialized` = `false`
    -   `if (typeof this.class.protokey == "Function")`：`throw new Error("DynamicProxy: the class attribute must be set before `super` constructor call.")`
    -   `this.initialized` = `true`
    -   `return`

    `new` `DynamicProxy`：`throw new Error("Unsupported operation on an abstract class.")`
    -   `return` `this.subject.apply(this, arguments)`
    -   `this.key`

    ```
    return this.subject.methodName.apply(this, arguments)
    ```

    ```
    } key
    ```

最重要的区别在于，你遍历的是类的原型，而不是对象本身。这是因为真实对象尚未被实例化，且不存在，所以你检查的是类本身，而不是真实对象，以确定要实现哪些方法。

你添加的每个方法由两部分组成：一个检查以确保真实对象已被初始化，以及一个对被代理方法的实际调用。

要使用这个类，你首先需要继承它。为了说明这是如何做到的，我们创建了一个 `TestProxy` 类，它被配置为作为 `TestProxy` 类的原型。

## `DynamicProxy` 原型 = `initialized` 函数

`this.subject` = `new` `Error("UnsupportedOperation on an abstract class.")`

`this.subject` = `TestProxy.arguments`

`for (this.protokey in this.class.protokey)`：`if (this.initialized)`：清除此函数的间隔，然后调用 `this.subject.apply(this, this.subject)`。

`this.initialized` = `true`。`Disinitialized` 函数：`if (this.isInitialized)`：清除间隔，然后 `this.subject.apply(this, this.initialized)`。`return` `this.subject.methodName(this, this.subject.arguments)`。

`return` `this.subject.methodName(this.subject, this.subject.arguments)`。

`return` `this`

```
for (var key in this.class.protokey) {

    this.initialized = false

    this.subject = new Error("Unsupported operation on an abstract class.")

}
```

`return`

```
DynamicProxy.prototype = function() {

    initialized function: if (this.isInitialized) { clearInterval(this.intervar); this.subject.apply(this, this.subject.arguments) }

    this.initialized = true

    clearInterval function: if (this.isInitialized) { clearInterval this.intervar; this.subject.apply(this, this.subject.arguments) }

    if (typeof this.class.protokey == "Function") { throw new Error("DynamicProxy: the class attribute must be set before `super` constructor call.") }

    this.initialized = true

    return setInterval(this.subject.methodName.apply(this, this.subject.arguments));

    this.intervar = setInterval(setInterval.super)

    if (typeof this.class.protokey == "Function") { return }

    this.initialized = true

    return this.subject

}
```

```
class TestProxy extends DynamicProxy<TestProxy> {

    public TestProxy() {

        super()

    }

    public final String arg0 = null;

    public final SELF initialized = null;

    public final SELF subject = null;

    public SELF intervar = setInterval(setInterval.super)

    if (type == this.class.class.protokey) {

        return setInterval.super

    }
```

# 动态代理类实现详解

## 参数与构造器

1.  `DynamicProxy` 抽象类：`abstract class DynamicProxy`
2.  `for (DynamicProxy.arguments)`：
    -   `this.arg0` = `Function(args)`
    -   `this.initialized` = `false`
    -   `if (typeof this.class.protokey == "Function")`：`throw new Error("DynamicProxy: the class attribute must be set before `super` constructor call.")`
    -   `this.initialized` = `true`
    -   `return`

    `new` `DynamicProxy`：`throw new Error("Unsupported operation on an abstract class.")`
    -   `return` `this.subject.apply(this, arguments)`
    -   `this.key`

    ```
    if (this.isInitialized) {
        clearInterval(this.intervar)
        this.subject.apply(this, this.subject.arguments)
    }
    ```

    ```
    return this.subject.methodName(this, this.subject.arguments)
    ```

    或者：
    ```
    for (var key in this.class.protokey) {
        this.initialized = false
        this.subject = new Error("Unsupported operation on an abstract class.")
    }
    ```

## 创建所需的方法

现在，你添加代码到构造器，使其动态地为真实对象中的每个方法创建代理方法。

11. 为`for (var key in this.class.protokey)` 创建`initialized` 函数：如果`this.isInitialized` 为真，则清除此函数的间隔，并调用`this.subject.apply(this, this.subject.arguments)`。
    `this.initialized` = `true`
    然后，在`else` 语句后（即没有命中`if`），继续处理。
    如果`typeof this.class.protokey == "Function"`：`throw new Error("DynamicProxy: the class attribute must be set before `super` constructor call.")`
    `this.initialized` = `true`
    `return setInterval(this.subject.methodName.apply(this, this.subject.arguments));`
    `this.intervar = setInterval(setInterval.super)`
    `if (typeof this.class.protokey == "Function") { return }`
    `this.initialized = true`
    `return this.subject`

## 添加方法

`for (var that = this.subject.methodName(this, this.subject.arguments); key in that.class.protokey; key++)`

`()` ：`DynamicProxy`，类的属性必须是一个函数。

`if (typeof this.class.protokey == "Function")`：`throw new Error("DynamicProxy: the class attribute must be set before `super` constructor call.")`
`this.initialized = true`

`clearInterval(this.intervar)`

`return this.subject.apply(this, this.subject.arguments)`

```
this.initialized = true
return setInterval(this.subject.methodName(this, this.subject.arguments), this.initialized);
```

`invoke` 方法：`return setInterval(this.subject.methodName(this, this.subject.arguments), this.initialized);`

`Disinitialized` 函数：`if (this.isInitialized)`：`clearInterval(this.intervar)`；`this.subject.apply(this, this.subject.arguments)`

`this.initialized = true`

`if (this.isInitialized)`：`this.subject.apply(this, this.subject.arguments)`

`this.initialized = true`

`return`

`return` `this.subject.methodName(this, this.subject.arguments)`

`return this.subject`

`DynamicProxy.prototype = function() { initialized function: if (this.isInitialized) { clearInterval(this.intervar); this.subject.apply(this, this.subject.arguments) } this.initialized = true clearInterval function: if (this.isInitialized) { clearInterval this.intervar; this.subject.apply(this, this.subject.arguments) } if (typeof this.class.protokey == "Function") { throw new Error("DynamicProxy: the class attribute must be set before `super` constructor call.") } this.initialized = true return setInterval(this.subject.methodName.apply(this, this.subject.arguments)); this.intervar = setInterval(setInterval.super) if (typeof this.class.protokey == "Function") { return } this.initialized = true return this.subject }`

```
class TestProxy extends DynamicProxy<TestProxy> {
    public TestProxy() {
        super()
    }
    public final String arg0 = null;
    public final SELF initialized = null;
    public final SELF subject = null;
    public SELF intervar = setInterval(setInterval.super)
```

## 立即测试

现在，你可以立即测试这个`TestProxy`类了。它必须被继承，因为动态代理本质上是一个抽象类，无法直接实例化。`DynamicProxy`类是一个用于测试的配置类，因此你不能像通常那样使用真实对象。请使用`class TestProxy extends DynamicProxy<TestProxy>`，所有接口方法将由`DynamicProxy`构造器自动创建。

现在，我们添加一个方法：

`for (var that = this.subject.methodName(this, this.subject.arguments); key in that.class.protokey; key++)`

`()`：`DynamicProxy`，类的属性必须是一个函数。

`if (typeof this.class.protokey == "Function")`：`throw new Error("DynamicProxy: the class attribute must be set before `super` constructor call.")`
`this.initialized = true`

`clearInterval(this.intervar)`

`return this.subject.apply(this, this.subject.arguments)`

`this.initialized = true`
`return setInterval(this.subject.methodName(this, this.subject.arguments), this.initialized);`

`invoke`方法：`this.intervar = setInterval(this.subject.methodName(this, this.subject.arguments), this.initialized);`

`Disinitialized`函数：`if (this.isInitialized)`：`clearInterval(this.intervar)`；`this.subject.apply(this, this.subject.arguments)`

`this.initialized = true`

`if (this.isInitialized)`：`this.subject.apply(this, this.subject.arguments)`

`this.initialized = true`

`return`

`return` `this.subject.methodName(this, this.subject.arguments)`

`return this.subject`

`} key`

你可能还想添加`checkInitialized()`函数。

`for (var key in that.class.protokey)`不应该是一个普通的`for`循环，而是`DynamicProxy`方法。

`DynamicProxy.prototype = function() { initialized function: if (this.isInitialized) { clearInterval(this.intervar); this.subject.apply(this, this.subject.arguments) } this.initialized = true clearInterval function: if (this.isInitialized) { clearInterval this.intervar; this.subject.apply(this, this.subject.arguments) } if (typeof this.class.protokey == "Function") { throw new Error("DynamicProxy: the class attribute must be set before `super` constructor call.") } this.initialized = true return setInterval(this.subject.methodName.apply(this, this.subject.arguments)); this.intervar = setInterval(setInterval.super) if (typeof this.class.protokey == "Function") { return } this.initialized = true return this.subject }`

```
class TestProxy extends DynamicProxy<TestProxy> {

    public TestProxy() {

        super()

    }

    public final String arg0 = null;

    public final SELF initialized = null;

    public final SELF subject = null;

    public SELF intervar = setInterval(setInterval.super)

    if (type == this.class.class.protokey) {

        return setInterval.super

    }

    if (type == clearInterval.super) {

        return clearInterval.super

    }

    return clearInterval.super

    return this.subject.arguments

}
```

## 参数与构造器

1.  `DynamicProxy`抽象类：`abstract class DynamicProxy`
2.  `for (DynamicProxy.arguments)`：
    -   `this.arg0` = `Function(args)`
    -   `this.initialized` = `false`
    -   `if (typeof this.class.protokey == "Function")`：`throw new Error("DynamicProxy: the class attribute must be set before `super` constructor call.")`
    -   `this.initialized` = `true`
    -   `return`

    `new` `DynamicProxy`：`throw new Error("Unsupported operation on an abstract class.")`
    -   `return` `this.subject.apply(this, arguments)`
    -   `this.key`

    ```
    if (this.isInitialized) {

        clearInterval(this.intervar)

        this.subject.apply(this, this.subject.arguments)

    }
    ```

    ```
    return this.subject.methodName(this, this.subject.arguments)
    ```

    }
    ```

    return this.subject.methodName(this.subject, this.subject.arguments)

    ```

    或者：
    ```
    for (var key in this.class.protokey) {

        this.initialized = false

        this.subject = new Error("Unsupported operation on an abstract class.")

    }
    ```

## 测试

`DynamicProxy` 原型 = `initialized` 函数

`this.subject` = `new` `Error("UnsupportedOperation on an abstract class.")`

`this.subject` = `TestProxy.arguments`

`for (this.protokey in this.class.protokey)`：`if (this.initialized)`：清除此函数的间隔，然后调用 `this.subject.apply(this, this.subject)`。

`this.initialized` = `true`。`Disinitialized` 函数：`if (this.isInitialized)`：清除间隔，然后 `this.subject.apply(this, this.initialized)`。`return` `this.subject.methodName(this, this.subject.arguments)`。

`return` `this.subject.methodName(this.subject, this.subject.arguments)`。

`return` `this`

```
for (var key in this.class.protokey) {
    this.initialized = false
    this.subject = new Error("Unsupported operation on an abstract class.")
}
```

`return`

```
DynamicProxy.prototype = function() {
    initialized function: if (this.isInitialized) { clearInterval(this.intervar); this.subject.apply(this, this.subject.arguments) }
    this.initialized = true
    clearInterval function: if (this.isInitialized) { clearInterval this.intervar; this.subject.apply(this, this.subject.arguments) }
    if (typeof this.class.protokey == "Function") { throw new Error("DynamicProxy: the class attribute must be set before `super` constructor call.") }
    this.initialized = true
    return setInterval(this.subject.methodName.apply(this, this.subject.arguments));
    this.intervar = setInterval(setInterval.super)
    if (typeof this.class.protokey == "Function") { return }
    this.initialized = true
    return this.subject
}
```

`TestProxy` 类：`class TestProxy extends DynamicProxy<TestProxy>`

### 测试 `TestProxy`

现在，我们手动操作，调用 `TestProxy` 的构造器。它应该会先检查真实对象是否被初始化，然后调用真实对象的方法，并验证设置的真实对象是否被正确触发。

### 关于 `Error` 和 `UnsupportedOperation`

最重要的一点是，你遍历的是类的原型，而不是对象本身。这是因为真实对象尚未被实例化，且不存在，所以你检查的是类本身，而不是真实对象，以确定要实现哪些方法。

你添加的每个方法由两部分组成：一个检查以确保真实对象已被初始化，以及一个对被代理方法的实际调用。

要使用这个类，你首先需要继承它。为了说明这是如何做到的，我们创建了一个 `TestProxy` 类，它被配置为作为 `TestProxy` 类的原型。

### `DynamicProxy` 原型 = `initialized` 函数

`this.subject` = `new` `Error("UnsupportedOperation on an abstract class.")`

`this.subject` = `TestProxy.arguments`

`for (this.protokey in this.class.protokey)`：`if (this.initialized)`：清除此函数的间隔，然后调用 `this.subject.apply(this, this.subject)`。

`this.initialized` = `true`。`Disinitialized` 函数：`if (this.isInitialized)`：清除间隔，然后 `this.subject.apply(this, this.initialized)`。`return` `this.subject.methodName(this, this.subject.arguments)`。

`return` `this.subject.methodName(this.subject, this.subject.arguments)`。

`return` `this`

### `TestProxy` 类

```
class TestProxy extends DynamicProxy<TestProxy> {
    public TestProxy() {
        super()
    }
    public final String arg0 = null;
    public final SELF initialized = null;
    public final SELF subject = null;
    public SELF intervar = setInterval(setInterval.super)
    if (type == this.class.class.protokey) {
        return setInterval.super
    }
    if (type == clearInterval.super) {
        return clearInterval.super
    }
    return clearInterval.super
    return this.subject.arguments
}
```

### 现在测试

```
DynamicProxy.prototype = function() {
    if (this.isInitialized) {
        clearInterval(this.intervar)
        this.subject.apply(this, this.subject.arguments)
    }
    this.initialized = true
    clearInterval(this.intervar)
    return this.subject.apply(this, this.subject.arguments)
}
```

### 关于 `UnsupportedOperation`

`UnsupportedOperation` 是一个错误类：`throw new Error("UnsupportedOperation on an abstract class.")`

## 测试

`DynamicProxy` 原型 = `initialized` 函数

`this.subject` = `new` `Error("UnsupportedOperation on an abstract class.")`

`this.subject` = `TestProxy.arguments`

`for (this.protokey in this.class.protokey)`：`if (this.initialized)`：清除此函数的间隔，然后调用 `this.subject.apply(this, this.subject)`。

`this.initialized` = `true`。`Disinitialized` 函数：`if (this.isInitialized)`：清除间隔，然后 `this.subject.apply(this, this.initialized)`。`return` `this.subject.methodName(this, this.subject.arguments)`。

`return` `this.subject.methodName(this.subject, this.subject.arguments)`。

`return` `this`

```
for (var key in this.class.protokey) {
    this.initialized = false
    this.subject = new Error("Unsupported operation on an abstract class.")
}
```

`return`

### 测试 `TestProxy`

现在，我们手动操作，调用 `TestProxy` 的构造器。它应该会先检查真实对象是否被初始化，然后调用真实对象的方法，并验证设置的真实对象是否被正确触发。

# 代理模式

## 定义

代理模式（`Proxy Pattern`）是一种结构型设计模式。

## 结构

代理模式包含以下角色：

- `Subject`（抽象主题）
- `RealSubject`（真实主题）
- `Proxy`（代理）

## 工作原理

代理模式通过引入一个代理对象来控制对真实对象的访问。代理对象与真实对象实现相同的接口，这样客户端可以透明地使用代理对象，无需关心真实对象的存在。

## 适用场景

代理模式适用于以下场景：

1. **延迟加载**：当创建一个真实对象需要消耗大量资源时，可以使用代理模式延迟对象的实例化，直到真正需要时才创建。

2. **访问控制**：代理可以控制对真实对象的访问权限。

3. **日志记录**：代理可以在调用真实对象的方法前后添加日志记录。

4. **远程代理**：代理可以代表位于不同地址空间的对象，如网络服务。

## 代码示例

```java
// Subject 接口
public interface Subject {
    void request();
}

// RealSubject 类
public class RealSubject implements Subject {
    @Override
    public void request() {
        System.out.println("RealSubject: Handling request.");
    }
}

// Proxy 类
public class Proxy implements Subject {
    private RealSubject realSubject;

    @Override
    public void request() {
        if (realSubject == null) {
            realSubject = new RealSubject();
        }
        realSubject.request();
    }
}
```

# 代理模式概述

## 优点

- **职责清晰**：代理模式可以将客户端与真实对象解耦，使职责更加清晰。
- **高扩展性**：代理类可以独立于真实对象进行扩展。
- **智能化**：代理可以在不修改真实对象的情况下增加额外的功能。

## 缺点

- **增加系统复杂度**：引入代理类会增加系统的复杂度和维护成本。
- **响应时间延长**：由于代理对象的存在，可能会增加响应时间。

## 实际应用

代理模式广泛应用于以下场景：

- **虚拟代理**：在图片加载、文档加载等场景中，使用虚拟代理延迟资源的加载。
- **保护代理**：在权限控制系统中，使用保护代理控制对敏感资源的访问。
- **远程代理**：在分布式系统中，使用远程代理访问远程服务，如 `RMI`、`WebService` 等。
- **智能引用代理**：在需要记录对象访问次数、加载时间等场景中使用。

## 总结

代理模式是一种非常有用的结构型设计模式，它可以在不修改原有代码的情况下，为对象提供额外的控制功能。在实际开发中，我们应该根据具体场景选择合适的代理类型，避免过度设计。

# 关于代理模式的深入剖析

代理模式是一种结构型设计模式，它提供了一个代理对象来控制对另一个对象的访问。这个模式在现代软件开发中扮演着至关重要的角色。

## 代理模式的核心概念

代理模式的核心思想是使用一个代理对象来控制对真实对象的访问。它通过提供一个与真实对象相同的接口，使得客户端可以透明地访问真实对象，而代理则可以在访问前后执行额外的逻辑。

代理模式的本质是控制对真实对象的访问。它通常用于以下几种场景：

- 延迟初始化（虚拟代理）：当创建一个真实对象的成本很高时，可以使用代理模式来延迟其创建，直到真正需要时才实例化。
- 访问控制（保护代理）：代理可以控制哪些客户端可以访问真实对象，基于权限或角色进行限制。
- 日志记录（日志代理）：代理可以在访问真实对象时记录日志，帮助调试或监控。
- 远程代理：代理可以代表一个位于不同地址空间（如另一台机器）的对象，使得客户端可以像访问本地对象一样访问远程对象。

## 代理模式的实现方式

在 `Java` 中，代理模式可以通过多种方式实现。最常见的方式包括静态代理和动态代理。静态代理需要为每个真实对象编写一个对应的代理类，而动态代理则可以在运行时动态地创建代理对象。

在 `Java` 中，动态代理主要通过`java.lang.reflect.Proxy`类和`InvocationHandler`接口来实现。这种方式非常灵活，可以在不修改原有代码的情况下，动态地为任意接口创建代理对象。

代理模式的实现通常涉及以下几个角色：

- 抽象主题（`Subject`）：定义了真实对象和代理对象的共同接口。
- 真实主题（`RealSubject`）：实现了抽象主题，是真正执行业务逻辑的对象。
- 代理（`Proxy`）：实现了抽象主题，并持有对真实主题的引用。客户端通过代理来访问真实主题。

这种模式可以帮助你在不修改真实对象代码的情况下，添加额外的功能，如日志记录、性能监控、访问控制等。

## 代理模式的分类

代理模式可以根据其用途分为多种类型：

- 虚拟代理：用于延迟创建开销较大的对象。当客户端需要访问一个对象，但该对象的创建成本很高时，虚拟代理可以延迟其创建，直到真正需要时才实例化。
- 保护代理：用于控制对对象的访问。保护代理可以在访问真实对象之前，检查客户端的权限，只有具有相应权限的客户端才能访问。
- 远程代理：用于访问位于不同地址空间的对象。远程代理可以隐藏网络通信的细节，使得客户端可以像访问本地对象一样访问远程对象。
- 智能引用代理：用于在访问对象时执行额外的操作，如引用计数、线程安全检查等。

## 代理模式的优势

代理模式在许多情况下都是非常有用的。它可以帮助你解耦客户端和真实对象，使得你可以更灵活地控制对真实对象的访问。代理模式还可以帮助你实现关注点分离，将横切关注点（如日志、安全、事务等）从业务逻辑中分离出来。

此外，代理模式还可以提高系统的可维护性和可扩展性。通过使用代理，你可以在不修改现有代码的情况下，轻松地添加新的功能。

## 示例：新闻报纸递送

在新闻报纸行业中，有一个关键角色和动作，使得发布和订阅运行得顺畅。首先，最重要的是读者们。他们是订阅者，是像你我一样的人。我们消费数据，并对所读的内容做出反应。我们也应该能够选择我们的喜好，并有报纸被递送到我们的家。

这个例子使用新闻报纸行业来演示观察者模式如何工作。

在新闻报纸行业中，关键角色和动作使得发布和订阅运行得顺畅。首先，最重要的是读者。他们是订阅者，是像你我一样的人。我们消费数据，并对所读的内容做出反应。我们也应该能够选择我们的喜好，并有报纸被递送到我们的家。

其次，我们有出版者，他们负责生产和分发报纸。出版者应该能够发布新内容，并通知所有订阅者。订阅者则应该能够订阅或取消订阅，以便他们可以接收或停止接收报纸。

这个模式可以在 `Java` 中通过多种方式实现。我们将在后续章节中详细讨论。

## 总结

在本章中，我们讨论了代理模式的各种形式。这种模式以某种方式控制对资源的访问。这个资源被称为真实对象。代理模式控制对真实对象的访问。这种模式以某种方式控制对资源的访问。这个资源被称为真实对象。

保护代理控制对类或对象的访问，这些类或对象可以被扩展、创建或维护。当用户的浏览器内存有限，无法运行你的代码时，这在 `Java` 中会非常有帮助。如果你发现真实对象加载缓慢，它也能派上用场。

虚拟代理可以提供加载消息或一个 UI 引导，在结束用户可以与真实 UI 交互之前，直到真实 UI 被加载。

代理模式可以随时使用，并给你的项目增加复杂度。它主要用于使你的代码可重用、更模块化或更高效。当在这些场景中使用时，代理可以使访问原本难以访问的其他资源变得更加容易。

# 发布者与订阅者模式

在发布者与订阅者模式中，发布者（`Publisher`）负责发布事件。当事件发生时，它会通知所有已订阅的观察者。

## 观察者模式与发布者-订阅者模式

在观察者模式（`Observer Pattern`）中，观察者（`Observer`）会直接订阅主题（`Subject`）。当主题状态发生变化时，它会直接通知所有观察者。这是一种松耦合的关系，但观察者需要知道主题的存在。

## 发布者-订阅者模式

相比之下，发布者-订阅者模式（`Publisher-Subscriber Pattern`）引入了一个消息代理（`Message Broker`）或事件通道（`Event Channel`），作为发布者与订阅者之间的中间层。发布者不直接知道订阅者的存在——它们只负责发布消息。同样地，订阅者也不直接知道发布者的存在。订阅者会表达对特定类型消息的兴趣，然后接收这些消息。在发布者-订阅者模式中，发布者与订阅者之间是高度解耦的。发布者只需将消息发送到消息代理，而订阅者则从消息代理接收消息。

## 核心区别

在观察者模式中，观察者直接订阅主题。在发布者-订阅者模式中，订阅者通过一个中间层接收消息。因此，发布者-订阅者模式通常比观察者模式更灵活，因为它允许发布者与订阅者之间进行更高级的抽象。发布者与订阅者可以彼此完全独立，并独立地改变行为。

## 理解发布者与订阅者

发布者（`Publisher`）负责发布事件。它们定义何时以及如何发布消息。订阅者（`Subscriber`）则定义它们对哪些消息感兴趣，并订阅这些消息。当一个事件发生时，发布者会通知所有订阅者。订阅者随后根据收到的消息执行相应的操作。

## 观察者模式示例

### 在 `JavaScript` 中实现观察者模式

- 主题（`Subject`）维护一个观察者（`Observer`）列表，并通知它们状态变化。
- 观察者（`Observer`）定义当接收到通知时执行的动作。

## 功能与机制

发布者-订阅者模式提供了一系列实现发布者-订阅者模式的方式。但在我们展示这些示例之前，让我们先确保所有关键角色（如主题（`Subject`）、观察者（`Observer`）和它们的方法）都已就绪。

## 核心接口

- 订阅者（`Subscriber`）可以订阅（`subscribe`）和取消订阅（`unsubscribe`）。它们接收通知。
- 发布者（`Publisher`）发布事件。它们拥有通知或是否被通知的选项。
- 主题（`Subject`）管理观察者（`Observer`）列表，添加或移除它们。

## 潜在问题

### 内存泄漏

如果订阅者没有正确取消订阅，并且发布者仍然持有对它们的引用，就可能导致内存泄漏。这通常发生在单页应用（`SPA`）中。

### 调试困难

发布者-订阅者模式使得跟踪事件流变得困难。在传统的事件驱动编程中，你可以看到哪个观察者订阅了哪个主题。但在发布者-订阅者模式中，由于中间层的存在，追踪事件流变得更加复杂。

### 性能问题

发布者-订阅者模式中的消息代理可能会成为性能瓶颈。当大量订阅者订阅同一主题时，消息代理可能需要处理大量消息。

## 高级技巧

### 使用命名空间

在大型应用中，你可能希望使用命名空间来避免事件名冲突。例如，`Publisher.subscriberCustomer.OrderPlaced`。

### 使用通配符

一些发布者-订阅者模式实现支持通配符订阅，允许订阅者订阅多个相关主题。例如，`Publisher.subscriberCustomer.*` 将匹配所有以 `Customer.` 开头的事件。

### 实际应用场景

- **事件驱动架构**：发布者-订阅者模式是事件驱动架构的核心。它允许系统组件之间进行解耦通信。
- **微服务通信**：在微服务架构中，发布者-订阅者模式常用于服务间通信，通过消息队列（`Message Queue`）或事件总线（`Event Bus`）实现。
- **用户界面（`UI`）事件处理**：在前端开发中，发布者-订阅者模式常用于处理用户界面事件，例如点击（`click`）、输入（`input`）等。

## 总结

发布者-订阅者模式是一种强大的设计模式，它允许系统组件之间进行解耦通信。通过引入消息代理，发布者与订阅者可以彼此完全独立。然而，这种模式也引入了一些挑战，例如调试困难、内存泄漏和性能问题。通过正确的实现和最佳实践，发布者-订阅者模式可以显著提高系统的灵活性和可扩展性。

## 进阶阅读

- `Observer` vs `Publisher-Subscriber` Pattern
- `Publish-Subscribe` vs `Observer` Pattern

## 在 `JavaScript` 中实现

在 `JavaScript` 中，你可以通过多种方式实现发布者-订阅者模式。以下是一个简单的实现示例：

```javascript
```

# 发布者-订阅者模式

```javascript
class Publisher {
  constructor() {
    this.subscribers = [];
  }

  subscribe(subscriber) {
    this.subscribers.push(subscriber);
  }

  unsubscribe(subscriber) {
    this.subscribers = this.subscribers.filter(s => s !== subscriber);
  }

  publish(data) {
    this.subscribers.forEach(subscriber => subscriber(data));
  }
}

// 使用示例
const publisher = new Publisher();
const subscriber1 = data => console.log('订阅者 1 收到:', data);
const subscriber2 = data => console.log('订阅者 2 收到:', data);
publisher.subscribe(subscriber1);
publisher.subscribe(subscriber2);
publisher.publish('Hello, world!');
```

在这个模型中，你可以看到发布者（`Publisher`）显然负责发布（`publish`）事件。它们对客户（`customer`）进行收费（`charge`），并且它们也有能力从交付（`delivery`）路线中删除它们。最后，顾客（`customer`）也拥有订阅（`subscribe`）和取消订阅（`unsubscribe`）的能力。

## 下一步

在下一部分中，我们将展示发布者-订阅者模式与观察者模式的具体示例。

```javascript
var Publisher = new Observable();
var Subscriber = function(data) {
    console.log('订阅者收到数据:', data);
};
Publisher.subscribe(Subscriber);
Publisher.publish('数据到达');
```

## 参考文献

- `https://en.wikipedia.org/wiki/Publish%E2%80%93subscribe_pattern`
- `https://www.oodesign.com/observer-pattern.html`
- `https://www.dofactory.com/javascript/design-patterns/observer`

---

## 附录

### 表格：发布者-订阅者模式 vs 观察者模式

| 特性 | 发布者-订阅者模式 | 观察者模式 |
|------|-------------------|------------|
| 耦合度 | 高度解耦 | 松耦合 |
| 中间层 | 有（消息代理） | 无 |
| 通信方式 | 间接通信 | 直接通信 |
| 扩展性 | 高 | 中等 |
| 调试难度 | 高 | 低 |

进入`/path.to.xxx`目录，找到`xxx.json`。

在表达式`cvar = avar + bvar`中，加法运算符（`+`）将`avar`与`bvar`相加，得到它们的和`cvar`。

在`List.of(arg0, arg1, arg2)`中，`List`接口的工厂方法`of()`接受一系列的元素，返回包含它们的只读列表。

之后我们这样调用`cmd`命令：`cmd arg0 arg1 arg2`。

```javascript
if (condVar > someVal) {console.log("xxx")}
```

在`/path.to.xxx`目录中，需要修改`xxx.json`文件，将`publisher`字段的值从`NewsPaper`改为`Magazine`，并将`deliver`字段更新为`Web`。然后重启应用。

对于`Web`订阅者，与`NewsPaper`和`Magazine`订阅者不同，`Web`订阅者有一个名为`deliver`的方法，它只接受一个`Publisher`对象，而不像其他订阅者那样需要`event`参数。

`Web`订阅者上的`deliver`方法假设`publication`的`publisher`与您为其配置的`Publisher`对象一致，并且它通过调用系统的`Mail`服务发送数据。

`Publisher`对象包含一个名为`NewsPaper`的`Publisher`对象，您可以通过调用`deliver`方法来发送数据。另外，`Publisher`类还包含一个方法`setSubscribers`，它接受一个订阅者数组，并将它们添加到内部的订阅者列表。

此外，`Subscriber`接口有两个抽象方法：`deliver`和`subscribe`。

`deliver`方法将`publication`作为参数，并返回一个布尔值，指示交付是否成功。

```javascript
deliver(publication)
```

`subscribe`方法接受一个`publisher`参数，并将订阅者注册到该发布者。

在`Publisher`类中，`setSubscribers`方法处理发布者与订阅者之间的链接。

发布者调用`deliver`方法时，需要遍历其订阅者列表，并为每个订阅者调用其`deliver`方法，传入相应的出版物对象。

在代码中，`publisher.setSubscribers(subscribers)`会更新发布者的订阅者列表。如果需要，您还可以通过`unsubscribe`方法从发布者中移除订阅者。

构建微服务时，可以选择使用`REST`、`gRPC`或`消息队列`。

`REST`是首选，因为它简单且易于理解。

如果使用`gRPC`，则需要定义`.proto`文件并生成客户端和服务端代码。

使用`消息队列`时，您可以使用`RabbitMQ`或`Kafka`。

在本文中，我们将使用`REST`和`JSON`。

---

## REST基础

*   `REST`代表`表述性状态转移`。
*   它使用标准的`HTTP`方法，如`GET`、`POST`、`PUT`、`DELETE`。

### 资源

*   `REST`中的核心概念是资源。
*   每个资源都有一个唯一的`URI`。
*   客户端通过`HTTP`方法对资源进行操作。

## 示例

*   `GET /users`：获取所有用户。
*   `POST /users`：创建新用户。

---

## JSON

*   `JSON`代表`JavaScript 对象表示法`。
*   它是一种轻量级的数据交换格式。
*   `JSON`易于阅读和编写。

## 示例

```json
{
  "name": "John",
  "age": 30,
  "city": "New York"
}
```

---

## 构建RESTful API

*   选择框架：`Express.js`（Node.js）、`Spring Boot`（Java）、`Flask`（Python）等。
*   定义端点：`/api/users`、`/api/products`。
*   处理请求：使用`HTTP`方法（`GET`、`POST`、`PUT`、`DELETE`）。
*   返回响应：通常使用`JSON`格式。

---

## 清理无用代码

在`Publisher`类中，`setSubscribers`方法接收一个订阅者数组。需要确保调用该方法时，传入的数组不为空，并且每个订阅者都实现了`Subscriber`接口。

此外，`Publisher`类的`deliver`方法负责遍历所有订阅者并调用他们的`deliver`方法。如果某个订阅者不再需要接收通知，可以通过`publisher.unsubscribe(subscriber)`方法将其移除。

---

## POST / PUT / PATCH

*   `POST`：用于创建新资源。
*   `PUT`：用于更新现有资源（整体替换）。
*   `PATCH`：用于部分更新现有资源。

## 示例

```http
POST /users
{
  "name": "Alice",
  "email": "alice@example.com"
}
```

---

## 错误处理

*   使用适当的`HTTP`状态码。
    *   `200 OK`
    *   `201 Created`
    *   `400 Bad Request`
    *   `404 Not Found`
    *   `500 Internal Server Error`
*   返回有意义的错误消息。

---

## 身份验证和授权

*   `身份验证`：验证用户身份。
    *   方法：`基本认证`、`令牌认证`（`JWT`）、`OAuth`。
*   `授权`：确定用户可以访问哪些资源。
    *   通常与身份验证结合使用。

---

## 测试`API`

*   使用工具如`Postman`、`curl`或`Insomnia`。
*   编写单元测试和集成测试。

---

## 下一步

*   学习`GraphQL`。
*   探索微服务架构。
*   了解容器化（`Docker`）。

---

## 关于`REST`的更多信息

*   `无状态`：每个请求包含所有必要的信息，服务器不存储客户端状态。
*   `可缓存`：响应可以被客户端或中间缓存。

---

## 总结

*   `RESTful API` 使用标准的`HTTP`方法和状态码。
*   `JSON`是常用的数据格式。
*   设计清晰的`URI`和资源模型。

---

### 订阅与发布模式

在 `pubSubObservable` 中，`pubSub` 用于 `functionPublisher()`。

`pubSub` 的 `PubSub` 模式 `subscribe()` 用于 `functionPublisher()` 和 `Observable` 模式。

`functionPublisher` 用于事件监听。

`if` ( `eventName` ) { `subscribe()` }

`var` `observer` = `Observable.create( new Emitter() )`。

`emitter` `publish` ( `event` ) 。

`if` ( `observer` ) { `emitter.publish(event)` }

`return` `observer` 。

`else`

`return` `this` 。

### 可观察对象 (Observable)

你可能已经注意到，在函数式编程中，`Observable` 模式本质上是一个只用于事件的 `pubSub`。

实际上，`Observable` 模式只是对 `pubSub` 模式的升级，它保证所有订阅者都能收到事件。

`functionPublisher` 的 `constructor` 返回 `publish.subscribe(eventName, event)`。

在 `subscribe` 和 `publish` 中，我们使用 `observers` 数组来管理订阅者。

`var` `observers` = `[]`。

`for` ( `observer` in `observers` ) { }

在 `Observable` 模式中，`return` 的 `observer` 可以调用 `unsubscribe` 方法。

因此，`publish` 会返回一个 `unsubscribe` 函数，以便之后取消订阅。

### 订阅者 (Subscriber)

订阅者在现实世界中无处不在，它们广泛应用于大型代码库，尤其是在多层架构中，多个作者协作开发时。

订阅者模式允许你灵活地在不同组件之间通信，并且你可以随时添加或移除观察者。

应用中的“有趣时刻”包括点击、加载、触摸、鼠标悬停等。一些有趣时刻可能由丰富的 UI 应用触发，例如拖拽、下拉、滑动、完成或切换。所有这些抽象动作最终都会由浏览器事件发布到各自对应的监听器。

### 示例：动画

动画是实现 `Observable` 对象的一个极好的起点。你至少需要三个关键点来启动、完成和进行动画。在本例中，我们将它们分别称为 `onStart`、`onComplete` 和 `onBetween`。请参考以下代码，了解如何使用之前编写的 `Publisher` 工具来实现这一点。

```javascript
// Publisher API
var onAnimation = function(event) {
    this.onStart = new Publisher;
    this.onComplete = new Publisher;
    this.onBetween = new Publisher;
}

// Animation method
onAnimation.prototype.subscribe = function(subscriber) {
    // ... implementation
}

var Supervisor = new Animation;
// ... configuration properties

// Begin implementing subscribers
var putOnCap = function(event) {
    // ... subscriber logic
}

var takeOffCap = function(event) {
    // ... subscriber logic
}

// Since the publisher allows you to listen for when Supervisor is about to take off and land back on the ground, all you have to do is subscribe to these moments.
Supervisor.onStart.subscribe(putOnCap);
Supervisor.onComplete.subscribe(takeOffCap);
```

# 动画生命周期

`var animation = function(event) { }`

`for (observer in observers) { }`

`begin animation`

`this.onStart.deliver(event)`。

`for (var frame in frames) { }`

`this.onBetween.deliver(frameNumber)`。

`end`

`this.onComplete.deliver(event)`。

`set up an account with the animation manager`

`var Supervisor = new Animation`。

`// config properties`

`// begin implementing subscribers`

`var putOnCap = function(event) { }`。`// ... subscriber logic`

`var takeOffCap = function(event) { }`。`// ... subscriber logic`

`// since the publisher allows you to listen for when Supervisor is about`

`to take off and land back on the ground, all you have to do is`

`subscribe to these moments.`

`Supervisor.onStart.subscribe(putOnCap);`

`Supervisor.onComplete.subscribe(takeOffCap);`

如你所见，它工作得非常完美。如果你是一个发布/订阅模式的实现者，`putOnCap` 和 `takeOffCap` 会被发布者调用。由于发布者允许你监听 `Supervisor` 即将起飞和着陆的时刻，你只需要订阅这些时刻即可。

# 标题

事件与监听器

## 核心概念

在事件驱动架构中，事件（`Event`）与监听器（`Listener`）是核心的两种元素。

事件是发生的事件，而监听器则是等待事件发生并处理它的程序。在事件驱动架构中，事件通常是一个包含状态信息的对象，而监听器则是注册到事件上的回调函数。

在事件模型（`Event Model`）中，事件与监听器之间的区别在于，事件表示“发生了什么”，而监听器则负责“响应什么”。

在事件模型中，监听器是被动等待事件发生的程序，而事件则是沿着事件总线（`Event Bus`）传递的消息。在事件驱动架构中，事件与监听器是绑定的，每个事件可以被分配给多个监听器，监听器之间通过事件进行通信。

在传统的回调模式（`Callback Pattern`）中，一个事件只能有一个监听器，而监听器可以独立处理其他事件。

在观察者模式（`Observer Pattern`）中，一个事件可以有多个监听器，监听器可以互相独立地处理事件。

当事件被触发时，它会被推送给所有注册的监听器。监听器通过事件对象（`Event Object`）获取事件的发生信息，比如事件类型、事件源等。

### 事件与监听器的区别

- 事件（`Event`）是“发生了什么”的表示。
- 监听器（`Listener`）是“响应什么”的接口。

## 事件的分类

事件可以分为两类：

1. 同步事件（`Synchronous Event`）：事件被触发后，监听器同步处理事件，直到处理完成。
2. 异步事件（`Asynchronous Event`）：事件被触发后，监听器异步处理事件，事件发布者不会被阻塞。

在事件模型（`Event Model`）中，事件与监听器是解耦的，事件源只负责发布事件，而监听器负责处理事件。

## 监听器的注册

监听器通过事件总线（`Event Bus`）注册到事件上。事件总线是一个全局的事件调度器，它负责将事件分发给注册的监听器。

### 示例代码

### 示例 1：使用监听器

```java
var element = document.getElementById("example");
element.addEventListener("click", function() {
    console.log("Element clicked!");
});
```

### 示例 2：使用事件对象

```java
element.addEventListener("click", function(event) {
    console.log("Event type: " + event.type);
});
```

### 示例 3：事件冒泡

```java
element.addEventListener("click", function(event) {
    event.stopPropagation();
    console.log("Event propagation stopped.");
});
```

## 监听器模式的优势

监听器模式是一种非常好的方式，用来解耦应用程序的各个部分。在大型应用中，监听器模式可以降低代码的耦合度，提高代码的可维护性和可扩展性。

在监听器模式中，监听器是独立于事件源的，事件源只需要发布事件，而监听器则处理事件。这样一来，如果事件源发生变化，监听器不需要修改；反之亦然。

## 监听器模式的缺点

1. 监听器模式增加了系统的复杂性
2. 监听器之间的依赖关系可能变得复杂
3. 监听器模式可能导致内存泄漏，如果监听器没有被正确移除

## 事件处理的几种方式

1. 同步处理：事件被触发后，监听器同步处理事件，直到处理完成。
2. 异步处理：事件被触发后，监听器异步处理事件，事件发布者不会被阻塞。
3. 事件冒泡：事件从内层元素向外层元素传播
4. 事件捕获：事件从外层元素向内层元素传播

## 事件监听器的生命周期

1. 注册：事件监听器注册到事件源
2. 触发：事件被触发后，监听器被调用
3. 处理：监听器处理事件
4. 移除：事件监听器从事件源移除

## 事件对象的属性和方法

事件对象包含以下常用属性和方法：

- `event.type`：事件类型
- `event.target`：事件目标
- `event.preventDefault()`：阻止默认行为
- `event.stopPropagation()`：阻止事件冒泡
- `event.stopImmediatePropagation()`：阻止事件冒泡并阻止其他监听器执行

## 事件驱动的应用场景

事件驱动架构适用于以下场景：

1. 用户界面交互（如点击、滚动、拖拽等）
2. 异步通信（如 AJAX 请求、WebSocket 等）
3. 系统通知（如文件写入、进程退出等）
4. 插件体系（如 WordPress 的钩子系统）

## 总结

通过事件驱动模型，应用程序可以实现模块间的解耦，提高代码的可维护性和可扩展性。事件总线（`Event Bus`）作为事件的核心调度器，负责将事件分发给注册的监听器。在大型应用中，正确使用事件驱动模型可以显著降低代码的耦合度，提高系统的灵活性。

## 何时使用观察者模式

当你想将人类行为与应用程序行为抽象分离时，就应该使用观察者模式。最好不要实现与用户交互紧密相关且源自浏览器的东西，例如基本的 DOM 事件，如点击（click）、鼠标悬停（mouseover）或按键（keypress）。

使用事件处理器（Event Handlers）无法实现这些事件，如下所示：

```java
// 使用监听器的示例
var element = document.getElementById("example");
element.addEventListener("click", function() {
    console.log("Element clicked!");
});

// 使用事件对象的示例
element.addEventListener("click", function(event) {
    console.log("Event type: " + event.type);
});

// 使用事件传播的示例
element.addEventListener("click", function(event) {
    event.stopPropagation();
    console.log("Event propagation stopped.");
});
```

在第一个使用监听器的示例中，当点击事件被分发时，`funcA` 和 `funcB` 都会被触发。但在第二个使用处理器的示例中，`funcA` 被 `funcB` 替代，因此 `funcA` 永远不会被调用，因为 `onclick` 属性被重新赋值，改由 `funcB` 来处理。

尽管如此，你可以看到监听器（`listeners`）和观察者（`observers`）之间存在相似之处。实际上，它们是同义词。两者都订阅特定的某个事件，等待该事件的发生。当事件发生时，它会通知订阅者的回调函数，通过事件对象传递有价值的信息，例如事件发生的时间、事件类型或最初分发事件的源目标。

## 观察者模式的优势

观察者模式基本上是在大型架构中维护基于操作型应用程序的好方法。在任何给定的应用程序中，在浏览器会话期间，可能零散地发生数十、数百甚至数千个事件。此外，你可以减少事件附加的数量，让可观察对象通过一个事件监听器为你处理所有操作，并将信息委派给它的所有订阅者，从而减少内存占用并提升交互性能。这样，你就不用不断地添加事件监听器了。

# 观察者模式

## 观察者模式概述

观察者模式是一种软件设计模式，在该模式中，一个被称为**主体**的对象维护着一个依赖列表（这些依赖被称为**观察者**），并在状态发生任何变化时自动通知它们，通常是通过调用观察者的某个方法来实现。

## 观察者模式的意图

观察者模式用于在设置可观察对象时，以较低的时间成本引入观察者接口的这一部分。你可以通过引入技术性的调用低负载来实现这一点：

这通常允许你将实例化新的可观察对象推迟到交付时间。这样一来，订阅者可以在事件尚未创建时就开始订阅，从而避免拖慢应用程序的初始加载时间。

## 总结

观察者模式是抽象化应用程序的一种极佳方式。你可以广播事件，并允许任何开发者利用这些事件的订阅机制，而无需深入其他开发者的实现代码。多人可以订阅同一个事件，而五个不同的事件也可以发送给同一个订阅者。在像浏览器这样交互性强的环境中，这是非常理想的。随着更大、更新的网络应用程序不断涌现，将可观察对象加入代码库是一种保持代码可维护性与整洁性的好方法。它们能够阻止第三方开发者或同事深入应用程序的内部，从而避免可能造成的混乱。通过实践观察者模式，给你的朋友和经理留下深刻印象。

在发布者实用工具中，我们开发了一个“推送”系统，发布者通过向每个订阅者推送数据来广播事件。试着编写一个实用工具，允许每个订阅者从各自的发布者那里“拉取”数据。提示：你可以尝试从一个订阅者方法入手，该方法以 `pull` 命名，并将发布者对象作为参数传入。

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

# 常见问题解答

## 恢复会话状态

如果您在使用过程中遇到会话中断，可以恢复到之前的状态。当您再次访问页面时，您的程序状态将被自动恢复，前提是浏览器没有清除缓存。

## 使用命令行恢复

在大多数情况下，使用`cmd`命令是最直接有效的方法。通过`cmd arg0 arg1 arg2`，您可以恢复到程序崩溃前的状态，而无需手动执行复杂的恢复流程。

例如，在`List.of(arg0, arg1, arg2)`中，`List`接口的工厂方法`of()`接受一系列的元素，返回包含它们的只读列表。

## 使用`HTTP`恢复

使用`HTTP`时，恢复机制会通过封装请求参数来工作，而不是直接传递参数。`HTTP`恢复允许您在`GET`或`POST`请求中传递一个回调函数，从而在服务端执行自定义逻辑。

## 其他恢复方法

除了上述两种主要方法，还有一些特定的恢复场景可以从恢复模式中受益。例如，在`POST`请求中，您可以封装参数并传递一个命令对象，而不是直接传递参数。恢复机制允许您在`POST`请求中封装多个函数调用，从而在服务端执行自定义逻辑。

## 常见误区

一个常见的误区是认为恢复只需要调用一个简单的方法即可。实际上，恢复是一个复杂的过程，涉及保存和恢复应用程序的完整状态。另一个常见误区是认为恢复是无害的，但事实上，如果使用不当，恢复操作可能会导致数据丢失或应用程序崩溃。

## 恢复模式的优点

使用恢复模式有两个主要优点：首先，当正确使用时，您的程序将更加模块化和灵活；其次，它允许您实现复杂且有用的功能，如`undo`和`state restoration`，从而轻松恢复应用程序状态。

有些人可能会认为恢复模式只是另一种复杂的方法，而一个简单的方法也可以在其基础上使用。但这只适用于恢复模式的简单实现。恢复模式提供了许多方法所不具备的特性，例如参数化和元数据定义。它允许您定义自己的方法，而不仅仅是执行现有的方法。恢复模式还允许您定义元数据，例如`txId`，用于记录日志或审计目的。

## 恢复模式的缺点

恢复模式的一个主要缺点是它可能引入复杂性，尤其是在分布式系统中。如果不小心使用，可能会导致难以调试的问题。此外，恢复模式可能不适合所有场景，特别是在实时系统中，恢复操作可能会导致不必要的延迟。

## 如何选择恢复方法

选择恢复方法时，应考虑应用程序的特定需求。对于简单的应用，使用`cmd`或`HTTP`恢复方法可能就足够了。对于更复杂的应用，可能需要实现自定义的恢复逻辑。在选择恢复方法时，还应考虑系统的可扩展性和维护性。

## 最佳实践

为了确保恢复机制的有效性，建议在恢复过程中记录详细的日志信息，以便在出现问题时进行调试。同时，应定期测试恢复机制，确保其在各种场景下都能正常工作。

## 总结

总的来说，恢复机制是现代应用程序中不可或缺的一部分。通过合理使用恢复方法，您可以显著提高应用程序的可靠性和用户体验。无论是使用`cmd`还是`HTTP`恢复，都应该根据实际需求选择合适的方法，并遵循最佳实践来确保恢复机制的有效性。

---

`CMD` `URL` `COMMAND` `PATTERN`

`TAG` `COMMAND` `PASS`

`API`

例如，在最后一个例子中，使用`cmd`允许`UI`元素完全从类中解耦，这意味着这些元素可以在任何页面或项目中被完全独立地使用。它们也可以被不同的`UI`元素使用。您可以创建一个简单的命令对象，然后从一个菜单项和一个工具栏图标中调用它，这样同一个操作就可以以不同的方式执行。

还有许多其他特殊化的场景可以从恢复模式中受益。例如，在`POST`请求中，您可以封装参数并传递一个命令对象，而不是直接传递参数。恢复机制允许您在`POST`请求中封装多个函数调用，从而在服务端执行自定义逻辑。

## 恢复模式的优缺点

恢复模式的一个主要缺点是它可能引入复杂性，尤其是在分布式系统中。如果不小心使用，可能会导致难以调试的问题。此外，恢复模式可能不适合所有场景，特别是在实时系统中，恢复操作可能会导致不必要的延迟。

## 如何选择恢复方法

选择恢复方法时，应考虑应用程序的特定需求。对于简单的应用，使用`cmd`或`HTTP`恢复方法可能就足够了。对于更复杂的应用，可能需要实现自定义的恢复逻辑。在选择恢复方法时，还应考虑系统的可扩展性和维护性。

## 最佳实践

为了确保恢复机制的有效性，建议在恢复过程中记录详细的日志信息，以便在出现问题时进行调试。同时，应定期测试恢复机制，确保其在各种场景下都能正常工作。

## 总结

总的来说，恢复机制是现代应用程序中不可或缺的一部分。通过合理使用恢复方法，您可以显著提高应用程序的可靠性和用户体验。无论是使用`cmd`还是`HTTP`恢复，都应该根据实际需求选择合适的方法，并遵循最佳实践来确保恢复机制的有效性。

---

`CMD` `URL` `COMMAND` `PATTERN`

`TAG` `COMMAND` `PASS`

`API`

例如，在最后一个例子中，使用`cmd`允许`UI`元素完全从类中解耦，这意味着这些元素可以在任何页面或项目中被完全独立地使用。它们也可以被不同的`UI`元素使用。您可以创建一个简单的命令对象，然后从一个菜单项和一个工具栏图标中调用它，这样同一个操作就可以以不同的方式执行。

还有许多其他特殊化的场景可以从恢复模式中受益。例如，在`POST`请求中，您可以封装参数并传递一个命令对象，而不是直接传递参数。恢复机制允许您在`POST`请求中封装多个函数调用，从而在服务端执行自定义逻辑。

# 排版后的内容

## 关键概念

在本章中，我们研究了命令模式。这是一种结构模式，用于将请求封装为一个对象，从而允许对操作进行参数化、排队、记录日志以及支持撤销操作。

## 主要观点

命令模式的核心思想是将请求封装为一个对象，使得代码与具体操作的调用者和接收者之间实现解耦。

## 理解命令模式

命令模式是一种行为设计模式，它将请求转换为一个包含执行该请求所需所有信息的独立对象。

这种转换可以让你根据不同的请求对方法进行参数化、延迟请求执行或将其放入队列，并且还能支持可撤销操作。

## 关键概念

### 发送者与接收者的解耦

在命令模式中，发送者（调用者）只知道如何发送命令，而不需要知道命令具体如何被执行。

### 命令对象

命令模式的核心是命令对象。每个命令对象都包含执行特定操作所需的所有信息，包括接收者（执行操作的对象）和要执行的操作。

### 基本工作流程

1. 创建命令对象：当用户（客户端）发起一个请求时，系统会创建一个对应的命令对象。

2. 设置接收者：命令对象会知道哪个接收者需要执行该命令。

3. 执行命令：通过调用命令对象的`execute()`方法来触发实际操作。

### 命令模式的优点

- 解耦：发送者和接收者之间实现了完全解耦，你可以独立地修改它们。

- 可扩展性：你可以轻松地添加新的命令，而无需修改现有代码。

- 支持撤销：命令对象可以存储执行前后的状态，从而支持撤销操作。

- 支持队列：命令可以被排队、记录日志或延迟执行。

## 总结

命令模式是一个非常强大的设计模式，它通过将请求封装为对象，实现了发送者和接收者之间的灵活解耦。这使得代码更加模块化、可维护和可扩展。

通过对命令的参数化和组合，你可以构建出复杂的操作序列，同时保持代码的简洁性。这是实现复杂系统中的行为控制的关键技术。

## 理解公共图书馆的目录系统

图书馆的目录系统是查找资料的关键工具。它允许您根据作者、书名、主题或关键词来搜索图书。

### 使用目录系统

#### 按作者搜索

在目录系统中，按作者搜索的方法与按书名搜索类似，但只使用作者的姓氏。

#### 按主题搜索

在目录系统中，按主题搜索有三种主要方法，允许您查找、检查借出并返回书籍对象。此外，您还可以使用按主题搜索来添加新书到图书馆。

#### 按关键词搜索

在目录系统中，按关键词搜索是一种新的搜索方式。它将用于创建存储书籍对象的类。在这个例子中，您将把书籍分成几个类别，按照类型（流派）进行分组。此系统有三种方法：`handleRequest`、`handleGetTitle` 和 `handleGetAuthor`，其中 `handleRequest` 接受一本书并将其添加到内部目录（如果符合某些标准）。`findBooks` 函数会搜索内部目录，基于一些参数，而 `setSummary` 函数将设置响应链中的下一个链接。

现在，让我们看看两个对象：`Book` 和 `PublicLibrary`。两者都将作为示例被修改，不同的是，`Book` 类的添加代码已被移动到一个新方法 `addBook` 中。我们将修改这个方法以及 `findBooks` 方法，稍后在示例中说明。

现在您已经有了现有的类，让我们来实现 `catalog` 对象。在此之前，请您先写下这些对象的代码，想象它们将如何使用。所有决定一本书是否应被添加到特定目录的逻辑都封装在 `PublicLibrary` 类中。这意味着您需要将每一本书作为 `PublicLibrary` 对象发送给每一个类型的类别。

### Book 类

`PublicLibrary` 类现在接受一个额外的参数，该参数指定了一个数组，其中的类型属于它。它同时也实现了 `getTitle` 和 `setTitle` 方法，但这些被忽略了，因为它们是简单的访问器和修改器方法。

### PublicLibrary 类

#### 按方法搜索

对于 `PublicLibrary` 类：`func(books)` -> `10` 个实现 `Library` 的实例。

## PublicLibrary 类

`this.catalog = new NonFictionCatalog("非虚构类目录");`

### NonFictionCatalog 原型

`NonFictionCatalog` 原型：`func(searchString)` -> `return` 结果。

```javascript
for (i = 0; i < len; i = i + 1) {
    if (this.catalog.isNonFiction(this.catalog.getTitle(searchString)))
        return this.catalog.isNonFiction(this.catalog.getTitle(searchString));
}
```

```javascript
if (this.catalog.hasOwnProperty("title")) {
    continue;
}
if (this.catalog.isNonFiction(getTitle())) {
    return this.catalog.isNonFiction(getTitle());
}
```

否则：
```javascript
throw new Error("PublicLibrary: book not found, book.getTitle().isFiction() != false");
```

```javascript
this.catalog.isNonFiction(this.catalog.getTitle(searchString));
throw new Error("PublicLibrary: book is not currently available, book.getTitle() is not found");
```

```javascript
addBook(func(this.newBook, newBook.available, true))
```

### 按标题搜索

```javascript
this.catalog = new FictionCatalog("虚构类目录");
```

```javascript
this.addBook(book, book.getTitle());
if (this.catalog.isNonFiction(getTitle())) return this.catalog.isNonFiction(getTitle());
```

```javascript
this.addBook(func(newBook, newBook.available, true));
```

### 按作者搜索

```javascript
this.catalog = new BiographyCatalog("传记类目录");
```

```javascript
this.biographyCatalog = new BiographyCatalog(this.catalog.getTitle());
```

### PublicLibrary 原型

```javascript
PublicLibrary.prototype = {
    findBooks: function(searchString) { ... },
    returnResults: function() { ... },
    ...
}
```

```javascript
for (i = 0; i < len; i = i + 1) {
    if (this.catalog.isNonFiction(this.catalog.getTitle(searchString)))
        return this.catalog.isNonFiction(this.catalog.getTitle(searchString));
}
```

```javascript
this.addBook(func(newBook, newBook.available, true));
```

```javascript
if (this.catalog.isNonFiction(getTitle())) // ...
if (this.catalog.isNonFiction(getTitle())) // continue
    if (this.catalog.isNonFiction(this.catalog.getTitle(searchString))) ...
```

```javascript
this.catalog.isNonFiction(this.catalog.getTitle(searchString));
```

`PublicLibrary` 现在已经基本未变，除了一个事实，即 `Book` 的添加代码已被移动到一个新方法 `addBook`。我们将修改这个方法以及 `findBooks` 方法，稍后在示例中说明。

`PublicLibrary` 被修改为可以处理“硬编码”的类别。对于 `PublicLibrary` 类：`func(books)` -> `10` 个实现 `Library` 的实例，`this.catalog` -> `newNonFictionCatalog(this.biographyCatalog)`。

```javascript
for (i = 0; i < len; i = i + 1) {
    if (this.catalog.isNonFiction(this.catalog.getTitle(searchString)))
        return this.catalog.isNonFiction(this.catalog.getTitle(searchString));
}
```

```javascript
return this.catalog.isFiction(this.catalog.getTitle(searchString));
```

### 非虚构目录

`this.nonFictionCatalog` = `new NonFictionCatalog("非虚构类目录")`。

```javascript
for (i = 0; i < len; i = i + 1) {
    if (this.catalog.isNonFiction(this.catalog.getTitle(searchString)))
        return this.catalog.isNonFiction(this.catalog.getTitle(searchString));
}
```

```javascript
this.catalog = new NonFictionCatalog(this.catalog.getTitle());
```

### 虚构目录

`this.fictionCatalog` = `new FictionCatalog("虚构类目录")`。

### 传记目录

`this.biographyCatalog` = `new BiographyCatalog("传记类目录")`。

- `this.nonFictionCatalog` = `new NonFictionCatalog("非虚构类目录")`
- `this.fictionCatalog` = `new FictionCatalog("虚构类目录")`
- `this.mysteryCatalog` = `new MysteryCatalog("神秘类目录")`

## 搜索与结果

### 返回结果

`returnResults(func(book))`

```javascript
for (i = 0; i < len; i = i + 1) {
    if (this.catalog.isNonFiction(getTitle())) return this.catalog.isNonFiction(getTitle());
}
```

```javascript
if (this.catalog[isNonFiction] = true) ...
if (this.catalog.isNonFiction(getTitle()) = true) ...
```

`return this.catalog`。

## 其他

```javascript
throw new Error("PublicLibrary: book not found, book.getTitle()" + " " + "not" + " " + "found");
```

`this` 不是当前可用的，`this` 没有被找到。

### 返回图书

`returnBook(func(book))`

```javascript
for (i = 0; i < len; i = i + 1) {
    if (this.catalog.isNonFiction(getTitle())) return this.catalog.isNonFiction(getTitle());
}
```

```javascript
if (this.catalog.isNonFiction(getTitle()) = true) ...
this.catalog.isNonFiction(getTitle()) = true;
return this.catalog;
```

## 添加图书

`addBook(func(newBook))`

`this.catalog[newBook.getTitle()]` = `newBook`，`newBook.available`，`true`。

## 结束

结束。

`PublicLibrary.prototype` = {}

## 结论

`PublicLibrary` 现在已经基本未变，除了 `book` 的添加代码已被移动到一个新方法 `addBook`。我们将修改这个方法以及 `findBooks` 方法，稍后在示例中说明。

现在您已经有了现有的类，让我们来实现 `catalog` 对象。在此之前，请您先写下这些对象的代码，想象它们将如何使用。所有决定一本书是否应被添加到特定目录的逻辑都封装在 `PublicLibrary` 类中。这意味着您需要将每一本书作为 `PublicLibrary` 对象发送给每一个类型的类别。

### PublicLibrary 类，带有硬编码的类别

对于 `PublicLibrary` 类：`func(books)` -> `10` 个实现 `Library` 的实例，`this.catalog` -> `newNonFictionCatalog(this.biographyCatalog)`。

`this.biographyCatalog` = `new BiographyCatalog("传记类目录")`。

`this.fantasyCatalog` = `new FantasyCatalog("奇幻类目录")`。

`this.mysteryCatalog` = `new MysteryCatalog("神秘类目录")`。

```
for (i = 0; i < len; i = i + 1) {
    if (this.catalog.isNonFiction(this.catalog.getTitle(searchString)))
        return this.catalog.isNonFiction(this.catalog.getTitle(searchString));
}
```

```
if (this.catalog.isNonFiction(getTitle()) = true) ...
this.catalog.isNonFiction(getTitle()) = true;
return this.catalog;
```

```
if (this.catalog[isNonFiction] = true) ...
this.catalog.isNonFiction(getTitle()) = true;
return this.catalog;
```

```
this.catalog = new NonFictionCatalog(this.catalog.getTitle());
```

## 结束

### 理解非虚构目录

`this.nonFictionCatalog` = `new NonFictionCatalog("非虚构类目录")`。

```
for (i = 0; i < len; i = i + 1) {
    if (this.catalog.isNonFiction(this.catalog.getTitle(searchString)))
        return this.catalog.isNonFiction(this.catalog.getTitle(searchString));
}
```

```
if (this.catalog.isNonFiction(getTitle()) = true) ...
this.catalog.isNonFiction(getTitle()) = true;
return this.catalog;
```

```
if (this.catalog.isNonFiction(getTitle()) = true) ...
this.catalog.isNonFiction(getTitle()) = true;
return this.catalog;
```

```
for (i = 0; i < len; i = i + 1) {
    if (this.catalog.isNonFiction(this.catalog.getTitle(searchString)))
        return this.catalog.isNonFiction(this.catalog.getTitle(searchString));
}
```

#### 按方法搜索

```
returnResults(func(book))
```

```
for (i = 0; i < len; i = i + 1) {
    if (this.catalog.isNonFiction(getTitle())) return this.catalog.isNonFiction(getTitle());
}
```

```
if (this.catalog.isNonFiction(getTitle()) = true) ...
this.catalog.isNonFiction(getTitle()) = true;
return this.catalog;
```

```
if (this.catalog[isNonFiction] = true) ...
this.catalog.isNonFiction(getTitle()) = true;
return this.catalog;
```

## 结论

`PublicLibrary` 已经基本未变，除了 `book` 的添加代码已被移动到一个新方法 `addBook`。我们将修改这个方法以及 `findBooks` 方法，稍后在示例中说明。

### 完整结论

`PublicLibrary` 原型已经修改，`NonFictionCatalog` 原型已经添加。

## 代码实例

```java
public class MyClass {}
```

```java
public class MyClass {}
```

```java
public class MyClass {}
```

```java
public class MyClass {}
```

```java
public class Main {
    public static void main(String[] args) {
        MyClass obj = new MyClass();
        obj.setSuccessor(new FirstHandler());
    }
}
```

## 代码段

```java
private FirstHandler->setSuccessor(new FirstHandler());
```

`public class FirstHandler implements Handler {`
`private FirstHandler nextHandler;`

`public FirstHandler(FirstHandler nextHandler) {`
    `this.nextHandler = nextHandler;
}`

`public void handle(Request request) {`
    `if (canHandle(request)) {`
        `    // 处理请求`
    `} else if (nextHandler != null) {`
        `    nextHandler.handle(request);
    } else {`
        `    System.out.println("请求无法被处理");
    }
}
```

## 处理链

```markdown
# 责任链模式示例

```java
public class Chain {
    private Handler firstHandler;

    public static void main(String[] args) {
        // 创建处理器
        FirstHandler first = new FirstHandler();
        SecondHandler second = new SecondHandler();
        ThirdHandler third = new ThirdHandler();
        FourthHandler fourth = new FourthHandler();

        // 构建链
        first.setSuccessor(second);
        second.setSuccessor(third);
        third.setSuccessor(fourth);

        // 处理请求
        first.handle(new Request());
    }
}
```

## 核心概念

这是责任链模式的一个很好的实现示例。责任链模式允许你以松散耦合的方式处理请求，使得多个对象都有机会处理同一个请求。`Chain` 类作为入口点，将请求沿着链传递，直到有对象能够处理它。这种设计模式在构建灵活、可扩展的系统中非常有用，尤其是在需要多个处理者按顺序处理请求的场景中。

## 通用结构

这种模式的核心在于将请求发送者与接收者解耦。`Handler` 接口定义了处理请求的方法，每个具体的处理者实现这个接口并决定是否处理该请求。如果能够处理，则处理；否则将请求传递给链中的下一个处理者。这种设计使得你可以轻松地添加新的处理者，而无需修改现有的代码。

## 责任链的优势

- 灵活性：可以动态地改变链的结构，增加或删除处理者。
- 可扩展性：新增处理者时，只需实现 `Handler` 接口并加入链中。
- 责任分离：每个处理者只关注自己能够处理的任务。

## 链定义

```java
Handler chain = new FirstHandler();
chain.setSuccessor(new FirstHandler());
```

## 链构建

```java
chain.setSuccessor(new SecondHandler());
chain.setSuccessor(new ThirdHandler());
chain.setSuccessor(new FourthHandler());
```

## 实例

```java
chain.setSuccessor(new FifthHandler());
```

## 调用链

```java
chain.handle(new Request());
```

在这个示例中，原始链是五个链长，以第六个链结尾。这意味着任何添加到链中的旧请求，都会在第一个链中通过处理链请求发送到链的每个节点，然后链的末尾节点会处理所有添加的请求，最后链的末端会返回结果。

## 核心要点

- 责任链模式通过将对象与它的后继者链接，形成一条链。
- 请求沿链传递，直到某个处理者能够处理它。
- 每个处理者只关心它是否能够处理当前请求，如果不能，则将其传递给下一个处理者。
- 这种模式非常适合处理需要多个处理者按顺序处理请求的场景。

## 实例代码

```java
// 实例化处理链
var biographyHandler = new BiographyHandler();
var fantasyHandler = new FantasyHandler();
var mysteryHandler = new MysteryHandler();
var nonFictionHandler = new NonFictionHandler();
var sciFiHandler = new SciFiHandler();

// 设置链中的链接
biographyHandler.setSuccessor(fantasyHandler);
fantasyHandler.setSuccessor(mysteryHandler);
mysteryHandler.setSuccessor(nonFictionHandler);
nonFictionHandler.setSuccessor(sciFiHandler);

// 发送请求到链
var historyHandler = new HistoryHandler();
historyHandler.setSuccessor(biographyHandler);
```

你可以将所有链连接到链中，甚至可以连接到任意位置。

```java
// 实例化链
var myLibraryHandler = new MyLibraryHandler();
myLibraryHandler.setSuccessor(new HistoryHandler());
```

在这个示例中，原始链是五个链长，以第六个链结尾。这意味着任何添加到链中的旧请求，都会在第一个链中通过处理链请求发送到链的每个节点，然后链的末尾节点会处理所有添加的请求，最后链的末端会返回结果。
```