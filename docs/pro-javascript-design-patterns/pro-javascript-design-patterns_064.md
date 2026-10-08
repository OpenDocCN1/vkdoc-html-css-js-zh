# React 组件与 CSS 样式

## Lambda 表达式：便捷的方法

进入`/path.to.xxx`目录，找到`xxx.json`。

在表达式`cvar = avar + bvar`中，加法运算符（`+`）将`avar`与`bvar`相加，得到它们的和`cvar`。

在`List.of(arg0, arg1, arg2)`中，`List`接口的工厂方法`of()`接受一系列的元素，返回包含它们的只读列表。

之后我们这样调用`cmd`命令：`cmd arg0 arg1 arg2`。

```
if (condVar > someVal) {console.log("xxx")}
```

这些组件被广泛用于现代 Web 开发。您可能会看到它们被用于`React`、`Vue`、`Angular`等框架中。

## 概念：属性（Props）与状态（State）

理解`Props`和`State`是掌握`React`、`Vue`、`Angular`等现代 UI 框架的核心。

`Props`（属性）是组件接收的外部数据，用于配置组件。它们由父组件传入，在组件内部是只读的，不可修改。

`State`（状态）是组件内部管理的数据，用于处理用户交互、API 响应等动态变化。组件可以主动修改自己的`State`，当`State`变化时，框架会自动更新视图。

### 核心区别

- `Props`：由父组件传入，组件内部不可修改（只读）。
- `State`：组件内部定义，组件可以更新它，驱动视图更新。

### 何时使用

- 当一个组件需要从父组件接收数据时，通过`Props`传入。
- 当一个组件需要维护自身动态数据（如用户输入、开关状态）时，使用`State`。

#### 代码示例

```
function MyComponent(props) {
  return <div>{props.message}</div>;
}
```

```
import React, { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0);
  return <button onClick={() => setCount(count + 1)}>Clicked {count} times</button>;
}
```

虽然两者模式相似，但`Props`和`State`不是可互换的。`Props`用于配置，不能与用户直接交互。`State`用于管理动态数据。

### 何时使用 Props (Props 是传递数据的首选方式)

推荐在组件间通过`Props`传递数据，而不是创建全局状态。这样可以形成清晰的数据流，使应用更易于理解和维护。

你可以将`Props`理解为函数的参数，`State`理解为函数内的局部变量。

## 示例：设置样式（Styles）与字体大小（Font Size）

设置元素的`font-size`是 CSS 的核心功能。你可能需要为特定元素设置字体大小。

### 行内样式与直接设置

如果你只想为单个元素设置字体大小，你可以使用`style`属性直接设置。例如，如果你希望一个`div`的字体大小为`20px`，你可以这样写：

```
<div style="font-size: 20px">This text is 20px</div>
```

对于一次性设置，这是最简单直接的方法。但如果你需要在多个地方复用相同的样式，更推荐使用 CSS 类。

## 示例：关于 CSS 类

设置元素的类名是通过`className`属性完成的。其核心在于通过字符串形式的类名来匹配 CSS 样式规则。

### 设置单个类

如果你希望在组件上设置一个特定的类（如`foo`），你只需将`className`属性设置为`"foo"`。例如：

```
<div className="foo">...</div>
```

### 根据条件动态设置

大多数情况下，你需要根据组件的状态或属性动态决定应用哪些类。这时，你可以在`className`属性中使用 JavaScript 表达式。

例如，如果你希望根据一个名为`isActive`的状态来切换`active`类，你可以这样写：

```
<div className={isActive ? "active" : ""}>...</div>
```

### 设置多个类

如果你需要同时应用多个类，你需要将它们组合成一个字符串。更常见的做法是使用像`classnames`这样的库来方便地管理多个类。

例如，如果你有三个状态`foo`、`bar`和`baz`，并希望根据它们来设置不同的文本颜色：

```
var element = document.getElementById('myElement');
element.style.color = 'red';
element.style.fontSize = '20px';
```
