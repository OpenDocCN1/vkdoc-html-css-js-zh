# JavaScript 字符串拼接与定时事件

## 字符串拼接与数组操作

`going on`——那些加号（`+`）在做什么？记住，我们的任务是生成表示特定 RGB 值的字符串。这里的加号**不是**表示数字的加法，而是表示字符串的拼接。这意味着这些值被粘合在一起，而不是数学上的相加，所以 `5+5` 的结果是 `10`，而 `'5'+'5'` 的结果是 `55`。第二个例子中的 `5` 被引号括起来，因此它们是字符串而不是数字。方括号用于提取数组中的元素。

`JavaScript` 将数字转换为对应的字符串，然后将其组合。请记住，我们处理的是数组中的数组，因此方括号内的第一个数字（在本例中由变量 `h` 提供）给出第一个数组，而方括号内的第二个数字则给出该数组中的数字。让我们看一个简单的例子。循环第一次运行时，`h` 的值将为 `0`，这会给出 `hue` 数组中的第一个条目。然后我们查找该条目的各个部分，以构建最终的颜色。

## 绘制渐变矩形

经过以上所有步骤，我们的代码设置了变量 `grad`，用于指示填充模式。代码没有将 `fillStyle` 设置为颜色，而是将其设置为变量 `grad`。

```javascript
ctx.fillStyle = grad;
```

绘制矩形的方式与之前相同，但现在使用了指定的填充。这些是原始矩形左侧、右侧、顶部和底部的四条窄墙。我让这些墙的厚度等于球的半径。对于垂直墙，厚度就是宽度；对于水平墙，厚度就是高度。

```javascript
ctx.fillRect(boxx,boxy,ballrad,boxheight);
ctx.fillRect(boxx+boxwidth-ballrad,boxy,ballrad,boxheight);
ctx.fillRect(boxx,boxy,boxwidth,ballrad);
ctx.fillRect(boxx,boxy+boxheight-ballrad,boxwidth,ballrad);
```