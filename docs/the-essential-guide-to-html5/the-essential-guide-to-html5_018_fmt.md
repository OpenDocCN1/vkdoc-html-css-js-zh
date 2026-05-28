# 第 2 章 骰子游戏

第三个应用是完整的双骰子游戏。同样，我们可以大量沿用前一个应用的代码。但现在，我们需要加入游戏规则。

除此之外，这意味着要使用条件语句 `if` 和 `switch`，以及全局变量（即定义在任何函数之外的变量），来记录当前是否是第一次投掷（`firstTurn`）以及玩家的点数（`point`）。

这两个变量保存了双骰子游戏的应用状态。正是这种相对简单的应用状态，以及全局变量和局部变量的使用、条件语句和随机处理，使得双骰子游戏成为编程教师们钟爱的教学主题。

函数表格与第二个应用给出的表格相同（见表 2-3），因此这里不再重复。表 2-5 包含了本应用的代码。新的操作全部在 `throwDice` 函数中。我将对新增的代码行进行注释。

**表 2-5.** 完整的双骰子游戏应用

| 代码 | 说明 |
| :--- | :--- |
| `<html>` | |
| `<head>` | |
| `<title>双骰子游戏</title>` | |
| `<script>` | |
| `var cwidth = 400;` | |
| `var cheight = 300;` | |
| `var dicex = 50;` | |
| `var dicey = 50;` | |
| `var diceWidth = 100;` | |
| `var diceHeight = 100;` | |
| `var dotrad = 6;` | |
| `var ctx;` | |
| `var dx;` | |
| `var dy;` | |
| `var firstturn = true;` | 全局变量，初始化为值 `true`。 |
| `var point;` | 全局变量，无需初始化，因为会在使用前被赋值。 |
| `function throwDice() {` | `throwDice` 函数开始。 |
| `var sum;` | 用于存放两个骰子点数之和的变量。 |
| `var ch = 1+Math.floor(Math.random()*6);` | 用第一个随机值设置 `ch`。 |
| `sum = ch;` | 将此值赋给 `sum`。 |
| `dx = dicex;` | 设置 `dx`。 |
| `dy = dicey;` | 设置 `dy`。 |
| `drawFace(ch);` | 绘制第一个骰子面。 |
| `dx = dicex + 150;` | 调整水平位置。 |
| `ch=1 + Math.floor(Math.random()*6);` | 用随机值设置 `ch`。这是第二个骰子的值。 |
| `sum += ch;` | 将 `ch` 加到已有的 `sum` 中。 |
| `drawFace(ch);` | 绘制第二个骰子。 |
| `if (firstTurn) {` | 现在开始实现规则。这是第一次投掷吗？ |
| `switch(sum) {` | 如果是，则以 `sum` 为条件开始一个 `switch` 语句。 |
| `case 7:` | 对于 7… |
| `case 11:` | …或 11。 |
| `document.f.outcome.value="你赢了！";` | 显示“你赢了！”。 |
| `break;` | 退出 `switch` 语句。 |
| `case 2:` | 对于 2… |
| `case 3:` | …或 3… |
| `case 12:` | …或 12。 |
| `document.f.outcome.value="你输了！";` | 显示“你输了！”。 |
| `break;` | 退出 `switch` 语句。 |
| `default:` | 对于其他任何情况。 |
| `point = sum;` | 将点数之和保存在变量 `point` 中。 |
| `document.f.pv.value=point;` | 显示点数。 |
| `firstTurn = false;` | 将 `firstTurn` 设置为 `false`。 |
| `document.f.stage.value="需要继续投掷。";` | 显示“需要继续投掷。”。 |
| `document.f.outcome.value=" ";` | 擦除（清除）结果输入框。 |
| `}` | 结束 `switch` 语句。 |
| `}` | 结束 `if` 为真时的子句。 |
| `else {` | 否则（不是第一次投掷）。 |
| `switch(sum) {` | 开始 `switch` 语句，再次使用 `sum`。 |
| `case point:` | 如果 `sum` 等于 `point` 中的值。 |
| `document.f.outcome.value="你赢了！";` | 显示“你赢了！”。 |
| `document.f.stage.value="回到第一次投掷。";` | 显示“回到第一次投掷。”。 |
| `document.f.pv.value=" ";` | 清除点数。 |
| `firstTurn = true;` | 重置 `firstTurn` 使其再次为 `true`。 |
| `break;` | 退出 `switch` 语句。 |
| `case 7:` | 如果点数之和等于 7。 |
| `document.f.outcome.value="你输了！";` | 显示“你输了！”。 |
| `document.f.stage.value="回到第一次投掷。";` | 显示“回到第一次投掷。”。 |
| `document.f.pv.value=" ";` | 清除点数。 |
| `firstTurn = true;` | 重置 `firstTurn` 使其再次为 `true`。 |
| `}` | 结束 `switch` 语句。 |
| `}` | 结束 `else` 子句。 |
| `}` | 结束 `throwDice` 函数。 |
| `function drawFace(n) {` | |
| `ctx = document.getElementById('canvas').getContext('2d');` | |
| `ctx.lineWidth = 5;` | |
| `ctx.clearRect(dx,dy,diceWidth,diceHeight);` | |
| `ctx.strokeRect(dx,dy,diceWidth,diceHeight);` | |
| `ctx.fillStyle = "#009966";` | |
| `switch(n) {` | |
| `case 1:` | |
| `draw1();` | |
| `break;` | |
| `case 2:` | |
| `draw2();` | |
| `break;` | |
| `case 3:` | |
| `draw2();` | |
| `draw1();` | |
| `break;` | |
| `case 4:` | |
| `draw4();` | |
| `break;` | |
| `case 5:` | |
| `draw4();` | |
| *(续)* | |