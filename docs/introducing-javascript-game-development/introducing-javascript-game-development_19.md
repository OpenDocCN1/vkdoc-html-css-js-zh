# 10. 简单的键盘交互

现在主要的游戏对象已经就位，我们需要将部分控制权交给用户。在本章中，我们将使用键盘事件来实现这一点。在 JavaScript 中，你可以使用许多事件来控制程序的运行。我们通过定义一个事件处理函数来使用事件，当事件被触发时，该函数会以事件对象作为参数被调用。在本例中，我们将使用键盘事件——特别是 `keyDown` 和 `keyUp` 事件。这些事件在按下和释放按键时被调用。事件对象包含了关于哪个按键触发了事件的信息。

## 控制吃豆人

让我们用一个简单的例子来演示如何使用键盘事件。复制你的 `exercise8` 文件夹，并另存为 `exercise10`。我们将升级该示例以接受用户输入。首先，我们需要向 `PacMan` 模型添加几个方法。将清单 10-1 所示的新方法添加到你的 `PacMan` 对象中。

```
PacMan.prototype.move_right = function() {
  this.x_speed = this.speed;
  this.y_speed = 0;
  this.angle = 0;
}

PacMan.prototype.move_down = function() {
  this.x_speed = 0;
  this.y_speed = this.speed;
  this.angle = 0.5 * Math.PI;
}

PacMan.prototype.move_left = function() {
  this.x_speed = -this.speed;
  this.y_speed = 0;
  this.angle = Math.PI;
}

PacMan.prototype.move_up = function() {
  this.x_speed = 0;
  this.y_speed = -this.speed;
  this.angle = 1.5 * Math.PI;
}
```

**清单 10-1** 为吃豆人添加控制功能

现在在浏览器中运行代码，并尝试在控制台中输入 `pacman.move_up()`。你会看到你已经可以对吃豆人的运动进行一些控制了。现在我们需要为 `keyDown` 事件连接一个事件处理函数。将清单 10-2 中的代码添加到你的 HTML 脚本末尾。

```
window.onkeydown = function(e) {
  let key = e.key || e.keyCode;
  switch(key) {
    case "ArrowLeft":
    case 37: // 左箭头键的 keyCode
      pacman.move_left();
      break;
    case "ArrowUp":
    case 38: // 上箭头键的 keyCode
      pacman.move_up();
      break;
    case "ArrowRight":
    case 39: // 右箭头键的 keyCode
      pacman.move_right();
      break;
    case "ArrowDown":
    case 40: // 下箭头键的 keyCode
      pacman.move_down();
      break;
  }
}
```

**清单 10-2** 处理 `onkeydown` 事件

现在刷新浏览器，你应该可以通过按方向键来控制吃豆人了！每次按键时，处理函数都会被触发，并传入一个事件对象作为参数。事件属性 `event.key` 包含了所按按键的字符串表示形式。在某些浏览器（包括撰写本书时的 Safari）中，不支持 `event.key` 属性，因此我们需要回退到使用 `event.keyCode` 的已弃用 API。这虽然令人烦恼，但如果你希望支持大多数常见浏览器，则是必要的。有关 `keyCode` 的列表，请参见 [`keycode.info`](http://keycode.info)。请注意，原先偶尔改变方向的行为仍包含在 `PacMan.prototype.update` 方法中。请将其移除，以便让你完全控制吃豆人。你可以移除相当多的代码。

这种方法有一个严重的问题。在大多数浏览器中，方向键已经有其功能。想象一下，你的画布是一个更大页面的一部分，页面下方还有其他内容。正常情况下，我们期望使用方向键可以向下滚动页面以查看这些内容。但因为我们将事件处理函数放在了主窗口对象上，我们覆盖了这种行为，现在方向键将无法滚动页面。一种更礼貌的方法是将处理函数仅添加到画布元素上。这要求焦点设置在画布元素上，以便我们的代码能够处理按键事件。如果画布没有焦点，则处理函数不会被触发。

首先，我们需要给画布一个 `tabindex` 属性。没有这个属性，你将无法将焦点设置在画布上。编辑你的 `<canvas>` 元素，如清单 10-3 所示。

```
<canvas id="pacman" width="300" height="300" tabindex="1"></canvas>
```

**清单 10-3** 处理 `onkeydown` 事件

现在，我们可以简单地将引用从 `window.onkeydown` 更改为 `context.canvas.onkeydown`。或者，我们可以将处理函数定义为一个函数，如清单 10-4 所示。

```
function keydown_handler(e) {
  let key = e.key || e.keyCode;
  let nothing_handled = false;
  switch(key) {
    case "ArrowLeft":
    case 37: // 左箭头键的 keyCode
      pacman.move_left();
      break;
    case "ArrowUp":
    case 38: // 上箭头键的 keyCode
      pacman.move_up();
      break;
    case "ArrowRight":
    case 39: // 右箭头键的 keyCode
      pacman.move_right();
      break;
    case "ArrowDown":
    case 40: // 下箭头键的 keyCode
      pacman.move_down();
      break;
    default:
      nothing_handled = true;
  }
  if(!nothing_handled) e.preventDefault();
}
```

**清单 10-4** 一个事件处理函数

请注意，我们确保只覆盖了我们想要使用的按键。我们使用 `default` 分支来在没有处理任何按键时设置 `nothing_handled` 变量。同时注意，仅当我们的代码处理了某些事情时，才额外调用 `e.preventDefault()`。这个调用会阻止事件的默认行为（例如，滚动窗口）发生。这意味着标准按键组合（例如刷新页面的 `Ctrl+R`）不会被阻止。我们只对我们选择的按键阻止默认行为。

现在，我们可以使用 `addEventListener` 方法以更通用的方式连接它。这仅当按下按键且画布拥有焦点时才会生效。清单 10-5 将 `keydown` 事件连接到我们的处理函数。

```
context.canvas.addEventListener("keydown", keydown_handler);
context.canvas.focus();
```

**清单 10-5** 添加事件监听器并将焦点设置在画布上

我们还通过编程方式将焦点设置在画布上，这样就不需要点击画布了。我们可以通过用鼠标点击画布以外的位置，或者使用 `Tab` 键在页面所有可选中元素间循环，来取消焦点设置。为了直观地指示画布是否拥有焦点，请更新你的 `styles.css` 文件，如清单 10-6 所示。

```
body {
  text-align: center;
  font-family: sans-serif;
}

canvas {
  background-color: black;
  border: 10px solid white;
}

canvas:focus {
  border: 10px solid grey;
}
```

**清单 10-6** 在焦点状态下高亮画布

现在，如果我们点击页面上的任何其他位置，画布将失去焦点，正常的按键处理将恢复。如果我们随后点击画布，我们将获得游戏的控制权，并且页面将不再滚动。当画布拥有焦点时，我们也应该能够使用 `Ctrl+R` 刷新页面。很好。

## 本章小结

本章简短地介绍了事件处理 API。你学习了如何定义事件处理函数以及如何从事件对象中获取信息。我们还讨论了如何将事件连接到处理函数。这些概念将在我们的游戏中广泛使用。在下一章中，我们将控制我们的飞船。



