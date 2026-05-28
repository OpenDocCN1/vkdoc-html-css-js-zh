# 算法实现文档

## 几何判断逻辑

取端点的`min`和`Math.max`。代码随后检查垂直线段是否在矩形外部。

如果`a`到`b`线段和`p`到`q`线段都是垂直的（参见`bothVertical`的编码），且`ax`不等于`px`，则无重叠。若这两个值相等，则认为存在跳跃围栏的情况——当`p`到`q`线段完全位于`a`到`b`线段内部时。此判断同样利用了包围盒。

相同斜率的情况包含两种可能：两条直线不是同一条直线。此时它们平行且不相交，*或者*它们是同一条直线。我的代码通过比较`y`轴截距来确定是否为同一直线。`y`轴截距是直线与`y`轴交点的值，即直线表达式中`x`为零时的值。如果它们*不是*同一条直线，则平行且不相交，因此不存在跳跃围栏。如果它们是同一条直线，当`a`到`b`线段完全覆盖`p`到`q`线段（即围栏）时，代码判定为跳跃。

## 数据准备

完成数学运算后，让我们转向实现。我们可以将其视为计算后的两个额外任务：准备信息和展示信息。从截图中可以看出，我的程序初始时围栏处于特定位置。用户/玩家通过按下鼠标按钮、拖动然后释放鼠标按钮来定义跳跃（即从点`a`(ax, ay)到`b`(bx, by)的移动）。此操作产生用于计算的数据。我提供了定义数据的替代方式。一种方式是通过调用随机函数来定位围栏，此术语称为*随机处理*。最后一种方式是让用户/玩家输入所有四个点（`a`、`b`、`p`、`q`）的坐标，共八个数字。准备数据的主要编码工作位于 HTML 中。

## 附录：更多绘图技术

```html
<body onload="init();">
```

```html
Mouse down to mark first position; drag and mouse up to mark the second.
```

```html
<button onClick="changeLine();">Change the fence randomly </button> <br/>
<canvas id="canvas" width="1200" height="600" style="border: 1px solid black;"> no canvas support
</canvas>
<br/>
```

```html
OR set positions yourself using coordinate values. Blanks are treated as 0s. <br/>
<form onSubmit="setValues(); return false;" name="f">
Fence start px: <input name="pxv"/> py: <input name="pyv"/> <br/>
Fence end qx: <input name="qxv"/> qy: <input name="qyv"/> <br/>
Ball travel start ax: <input name="axv"/> ay: <input name="ayv"/> <br/>
Ball travel end bx: <input name="bxv"/> by: <input name="byv"/>
<input type="submit" value="Enter coordinates"/> <br/>
Result: <input type="text" name="results" style="width: 400px;"/>
</form>
</body>
```

HTML 代码包含 JavaScript 函数的调用。这些函数将在表 A-3 和表 A-4 中详细描述。

### 用户/玩家反馈

表示围栏的线条由`drawLine`函数绘制。小方框用于指示`a`到`b`线段（即跳跃）的起点和终点。`a`位置由`firstPosition`绘制，`b`位置由`secondPosition`绘制。如果找到两条线的交点，则通过显示一个小紫色方框来图形化展示结果。如果线段相交，紫色方框将位于围栏上。

### 实现细节

JavaScript 函数可以通过直接函数调用或通过其他函数（或 HTML）中设置的事件处理来调用。这正是 HTML 和 JavaScript 组合的强大之处。

## 附录：更多绘图技术

**表 A-3.** 跳跃围栏的调用/被调用表

| 函数 | 被谁调用 | 调用谁 | 通过事件处理设置 |
|------|----------|--------|------------------|
| `init` | `onload`、`onresize` | `drawLine` | `mousedown`、`mouseup` |
| `drawLine` | `init`、`setValues` | — | — |
| `firstPosition` | `mousedown`（在`init`中设置） | — | — |
| `secondPosition` | — | — | — |