# 第 2 章 骰子游戏

第二个应用需要处理两个骰子面的位置，为此将使用两个额外的变量：`dx` 和 `dy`。它还需要重复使用 `Math.random` 的代码，并两次调用 `drawFace` 来生成每个骰子面。并且，触发投掷的方式也需要改变。表 2-3 描述了被调用和调用其他函数的函数，其结构与表 2-1 基本相同，只是现在增加了一个名为 `throwDice` 的函数，该函数由 `<button>` 标签的 `onClick` 属性设置的动作调用。表 2-4 包含了投掷两个骰子应用的完整 HTML 文档。

## 表 2-3. 双骰子应用中的函数

| 函数 | 被谁调用/由谁调用 | 调用谁 |
|---|---|---|
| `throwDice` | 由 `<button>` 标签中的 `onClick` 动作调用 | `drawFace` |
| `drawFace` | 由 `throwDice` 调用 | `draw1`, `draw2`, `draw4`, `draw2mid` |
| `draw1` | 由 `drawFace` 在表示 1、3 和 5 的三个位置调用 | |
| `draw2` | 由 `drawFace` 在表示 2、3 和 4 的两个位置调用 | |
| `draw4` | 由 `drawFace` 在表示 4、5 和 6 的三个位置调用 | `draw2` |
| `draw2mid` | 由 `drawFace` 在表示 6 的一个位置调用 | |

## 表 2-4. 完整的双骰子应用

| 代码 | 说明 |
|---|---|
| `<html>` | HTML 开标签。 |
| `<head>` | head 开标签。 |
| `<title>Throwing dice</title>` | 完整的标题元素。 |
| `<script>` | script 开标签。 |
| `var cwidth = 400;` | 存放画布宽度的变量。 |
| `var cheight = 300;` | 存放画布高度的变量；也用于清除画布以准备重绘。 |
| `var dicex = 50;` | 存放单个骰子水平位置的变量；也用于清除画布以准备重绘。 |
| `var dicey = 50;` | 存放单个骰子垂直位置的变量。 |
| `var diceWidth = 100;` | 存放骰子面宽度的变量。 |
| `var diceHeight = 100;` | 存放骰子面高度的变量。 |
| `var dotrad = 6;` | 存放圆点半径的变量。 |
| `var ctx;` | 存放画布上下文对象的变量，用于所有绘图命令。 |
| `var dx;` | 用于水平定位的变量，会针对两个骰子面分别修改。 |
| `var dy;` | 用于垂直定位的变量。两个骰子面使用相同的值。 |
| `function throwDice() {` | `throwDice` 函数的开始。 |
| `var ch = 1+Math.floor(Math.random()*6);` | 声明变量 `ch` 并用随机值为其赋值。 |
| `dx = dicex;` | 为第一个骰子面设置 `dx`。 |
| `dy = dicey;` | 为第一个和第二个骰子面设置 `dy`。 |
| `drawFace(ch);` | 调用 `drawFace`，并将 `ch` 作为点数传入。 |
| `dx = dicex + 150;` | 为第二个骰子面调整 `dx`。 |
| `ch = 1 + Math.floor(Math.random()*6);` | 用随机值重新设置 `ch`。 |
| `drawFace(ch);` | 调用 `drawFace`，并将 `ch` 作为点数传入。 |
| `}` | 关闭 `throwDice` 函数。 |
| `function drawFace(n) {` | `drawFace` 函数定义的开始，其参数是点数。 |
| `ctx = document.getElementById('canvas').getContext('2d');` | 获取用于在画布上绘图的对象。 |
| `ctx.lineWidth = 5;` | 将线宽设置为 5。 |
| `ctx.clearRect(dx,dy,diceWidth,diceHeight);` | 清除可能已绘制过骰子面的区域。首次调用时无效果。 |
| `ctx.strokeRect(dx,dy,diceWidth,diceHeight);` | 绘制骰子面的轮廓。 |
| `var dotx;` | 存放水平位置的变量。 |
| `var doty;` | 存放垂直位置的变量。 |
| `ctx.fillStyle = "#009966";` | 设置颜色。 |
| `switch(n) {` | 开始 switch 语句，根据点数进行分支。 |
| `case 1:` | 如果点数为 1。 |
| `draw1();` | 调用 `draw1` 函数。 |
| `break;` | 跳出 switch 语句。 |
| `case 2:` | 如果点数为 2。 |
| `draw2();` | 调用 `draw2` 函数。 |
| `break;` | 跳出 switch 语句。 |
| `case 3:` | 如果点数为 3。 |
| `draw2();` | 先调用 `draw2`，然后。 |
| `draw1();` | 调用 `draw1`。 |
| `break;` | 跳出 switch 语句。 |
| `case 4:` | 如果点数为 4。 |
| `draw4();` | 调用 `draw4` 函数。 |
| `break;` | 跳出 switch 语句。 |
| `case 5:` | 如果点数为 5。 |
| `draw4();` | 调用 `draw4` 函数，然后。 |
| `draw1();` | 调用 `draw1` 函数。 |
| `break;` | 跳出 switch 语句。 |
| `case 6:` | 如果点数为 6。 |
| `draw4();` | |