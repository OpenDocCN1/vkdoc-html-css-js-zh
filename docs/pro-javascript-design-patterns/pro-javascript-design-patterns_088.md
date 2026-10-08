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
