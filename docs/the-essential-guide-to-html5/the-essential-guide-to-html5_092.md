# 排版后的文档

`preventDefault` 函数顾名思义：它用于阻止任何默认行为，例如与特定按键关联的特殊快捷操作。本应用中仅关注三个按键：**d**、**h** 和 **n**。以下 `switch` 语句用于判断按下的是哪个按键，并调用对应的函数：`deal`、`playerdone` 或 `newGame`。`switch` 语句会将括号内的值与 `case` 后的值进行比较，并从第一个匹配的值开始执行后续语句。`break;` 语句会使执行跳出 `switch` 语句。`default` 子句如其名称所示，它不是必需的，但如果存在，当没有任何值与提供的 `case` 值匹配时，将执行 `default:` 后的语句。

```
switch(keyCode) {
  case 68: //d
    deal();
    break;
  case 72: //h
    playerdone();
    break;
  case 78: //n
    newGame();
    break;
  default:
    alert ("请按 d、h 或 n 键。");
}
```

回想一下，你可以通过将整个 `switch` 语句修改为仅在 `default` 情况下包含以下一行代码来确定任意按键的键码：

```
alert("您刚刚按下的键码是 "+keyCode);
```

然后通过实际按键并记录出现的数字来进行实验。

## 第 10 章 二十一点

**注意**：如果你像我一样，有时会在电脑的不同窗口间切换，你可能会发现，当返回二十一点游戏并按下一个按键时，程序没有响应。你需要用鼠标点击包含二十一点文档的窗口。这会让操作系统将焦点恢复到二十一点文档上，从而能够监听按键事件。

#### 使用页眉和页脚元素类型

HTML5 新增了一些内置元素类型，包括 `header` 和 `footer`。引入这些以及其他新元素（例如 `article` 和 `nav`）的设计理念是提供服务于标准目的的元素，以便搜索引擎和其他程序能够识别如何处理这些内容，不过仍然需要指定其格式。以下是我们将在本例中使用的样式：

```
footer {
  display:block;
  font-family:Tahoma, Geneva, sans-serif;
  text-align: center;
  font-style:oblique;
}

header {
  width:100%;
  display:block;
}
```

`display` 设置可以是 `block`（块级）或 `inline`（行内）。将其设置为 `block` 会强制换行。请注意，对于某些浏览器，强制换行可能不是必需的，但这样做并无坏处。`font-family` 属性用于指定字体选项。如果用户的计算机上安装了 Tahoma 字体，则会使用它。接下来会尝试使用 Geneva 字体。如果两者都不存在，浏览器将使用默认设置的 `sans-serif` 字体。`text-align` 和 `font-style` 设置如其表面含义所示。`width` 设置将元素宽度设为包含元素（在此例中为 `body`）的整个宽度。请随意尝试！

请注意，你不能假设 `footer` 会出现在屏幕底部，或 `header` 会出现在屏幕顶部。我是在 HTML 文档中使用定位实现这一效果的。

### 第 10 章 二十一点

我使用了 `footer` 来显示扑克牌图片的来源和洗牌算法。提供致谢、显示版权信息和展示联系方式都是 `footer` 元素的典型用途，但对于如何使用这些新元素、在 HTML 文档中将其放置在何处以及如何格式化它们，并没有限制。

#### 构建应用程序并使其成为你的专属

表 10-1 描述了该游戏中使用的函数。

***表 10-1.** 二十一点函数*

| **函数** | **被谁调用** | **调用谁** |
|---|---|---|
| `init` | 由 `<body>` 标签中的 `onLoad` 函数调用 | `buildDeck`、`shuffle` 和 `dealStart` |
| `getKey` | 由 `init` 中的 `window.addEventListener` 调用 | `deal`、`playerDone` 和 `newGame` |
| `dealStart` | `init` | 四次调用 `dealFromDeck` |
| `deal` | `getKey` | 两次调用 `dealFromDeck` 和一次调用 `more_to_house` |
| `more_to_house` | `deal`、`playerDone` | `dealFromDeck` |
| `dealFromDeck` | `deal`、`dealStart`、`playerDone` | - |
| `buildDeck` | `init` | `MCard` |
| `MCard` | `buildDeck`、`swapInDeck` | - |
| `add_up_player` | `playerDone` | - |
| `playerDone` | `getKey` | - |



