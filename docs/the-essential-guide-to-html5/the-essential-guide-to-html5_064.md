# 第 7 章：迷宫 S

首先，我们将定义一个函数 `Wall` 来表示墙壁对象，以及另一个函数 `Token` 来表示路径点对象。我们会以比本应用所需更通用的方式来定义这些函数，但我认为这没问题：通用性对性能几乎（即便有也很少）没有影响，同时让我们能够自由地将代码用于其他应用，例如具有不同游戏棋子的游戏。我选择五边形是因为我喜欢它，并用 `myPent` 作为游戏棋子的变量名。

墙壁定义的属性包括鼠标操作指定的起点和终点。我将它们命名为 `sx`、`sy`、`fx` 和 `fy`。墙壁还有宽度和 `strokeStyle` 字符串，并指定了一个 `draw` 方法为 `drawAline`。之所以说这比必要更通用，是因为所有墙壁都会有相同的宽度和样式字符串，并且都会使用 `drawAline` 函数。当需要将墙壁保存到本地存储时，我只保存 `sx`、`sy`、`fx` 和 `fy` 的值。如果你在编写其他程序时需要存储数值，可以使用相同的技术来编码更多信息。

在迷宫中移动的路径点通过调用 `Token` 函数来定义。这个函数类似于多边形记忆游戏中定义的 `Polygon` 函数。`Token` 函数存储路径点的中心点 `sx` 和 `sy`，以及半径（`rad`）、边数（`n`）和 `fillStyle`，并将 `draw` 方法与 `drawToken` 函数关联，将 `moveit` 方法与 `moveToken` 函数关联。此外，还有一个名为 `angle` 的属性被立即计算为 `(2*Math.PI)/n`。回想一下，在弧度制测量角度时，`2*Math.PI` 代表一个完整的圆，因此这个数除以边数就是中心到每一边端点的角度。

与之前的应用（请参阅[第 4 章](http://dx.doi.org/10.1007/978-1-4842-4155-4_4)）一样，创建对象后，代码会将其添加到 `everything` 数组中。我还会将所有墙壁添加到 `walls` 数组中。正是这个数组用于将墙壁信息保存到本地存储。

##### 用于构建和定位墙壁的鼠标事件

回想一下，在前几章中，我们使用 HTML5 和 JavaScript 来定义事件并指定事件处理程序。`init` 函数包含设置事件处理的代码，用于处理玩家按下主鼠标按钮、移动鼠标和释放按钮的操作。

```javascript
canvas1 = document.getElementById('canvas');
canvas1.addEventListener('mousedown', startWall, false);
canvas1.addEventListener('mousemove', stretchWall, false);
canvas1.addEventListener('mouseup', finish, false);
```

我们还会使用一个名为 `inMotion` 的变量来跟踪鼠标按钮是否被按下。`startWall` 函数确定鼠标坐标（有关事件后访问鼠标坐标的内容，请参阅[第 4 章](http://dx.doi.org/10.1007/978-1-4842-4155-4_4)和[第 5 章](http://dx.doi.org/10.1007/978-1-4842-4155-4_5)），创建一个新的 `Wall` 对象（其引用存储在全局变量 `curWall` 中），将该墙壁添加到 `everything` 数组中，绘制 `everything` 中的所有项目，并将 `inMotion` 设置为 `true`。如果 `inMotion` 不是 `true`，则 `stretchWall` 函数会立即返回而不执行任何操作。如果 `inMotion` 为 `true`，代码会获取鼠标坐标，并用它们来设置 `curWall` 的 `fx` 和 `fy` 值。当玩家按住按钮移动鼠标时，这个操作会反复执行。当按钮释放时，会调用 `finish` 函数。该函数将 `inMotion` 设置回 `false`，并将 `curWall` 添加到一个名为 `walls` 的数组中。

##### 检测箭头键

检测键盘上的某个键是否被按下以及按下的是哪个键，这被称为*捕获*按键。这是 HTML5 和 JavaScript 可以处理的另一种事件类型。我们需要设置对按键事件的响应，这与设置对鼠标事件的响应类似。对任何按键按下的响应将是我编写的一个名为 `getKeyAndMove` 的函数，稍后会解释。设置事件涉及调用 `addEventListener` 方法，这次是针对 `window`（即包含 HTML 文件的内置 HTML 对象）：

```javascript
window.addEventListener('keydown', getkeyAndMove, false);
```

这条语句在第一个参数中指定了事件 `keyDown`，在第二个参数中指定了事件处理程序 `getkeyAndMove`。第三个参数与其它对象响应事件的顺序有关（由于 `false` 是默认值，因此可以省略），但这对本应用来说不是问题。这意味着当某个键被按下时，`getkeyAndMove` 函数将被调用。

**提示：** 事件处理是编程中很重要的一部分。基于事件的编程通常比本书展示的要复杂得多。例如，你可能需要考虑包含的对象或被包含的对象是否也应该响应事件，或者如果用户打开了多个窗口该怎么办。手机等设备可以检测倾斜、摇晃或用手指划过屏幕等事件。整合视频可能涉及在视频播放完毕时调用某些操作。HTML5 JavaScript 在处理事件方面并不完全一致（设置超时或时间间隔不使用 `addEventListener`），但在这一点上，你已经掌握了足够的知识去做研究，以确定你想要的事件，尝试多种可能性来弄清楚事件需要与什么关联（例如，`window` 或画布元素或其他对象），然后编写函数作为事件处理程序。另请注意，某些事件处理使用了术语 *回调*。调用指定函数的行为称为回调。

现在，你可能已经预料到，获取按下哪个键的信息的代码因浏览器而异。以下代码提供了两种获取键对应数字的方法，可在识别 HTML5 其他新功能的所有当前浏览器中工作：

```javascript
if (event == null) {
    keyCode = window.event.keyCode;
    window.event.preventDefault();
} else {
    keyCode = event.keyCode;
    event.preventDefault();
}
```

`preventDefault` 方法如其名：阻止任何默认操作，例如特定浏览器中与特定键关联的特殊快捷操作。本应用中唯一感兴趣的键是箭头键。以下 `switch` 语句用于移动变量 `myPent` 引用的 `Token`；也就是说，位置信息会被更改，以便下次绘制所有内容时路径点会移动。（这并不完全正确。`moveit` 函数包含碰撞检测，以确保我们首先不会碰到任何墙壁，但这将在后面描述。）

```javascript
switch (keyCode) {
    case 37: // 左箭头
        mypent.moveit(-unit, 0);
        break;
    case 38: // 上箭头
        mypent.moveit(0, -unit);
        break;
    case 39: // 右箭头
        mypent.moveit(unit, 0);
        break;
    case 40: // 下箭头
        mypent.moveit(0, unit);
        break;
    default:
        window.removeEventListener('keydown', getkeyAndMove, false);
}
```

**提示：** 请务必像示例代码中用注释标明不同箭头键的 `keyCode` 那样，在代码中添加注释。本书中的示例没有很多注释，因为我在相关表格中对每一行代码都提供了解释，所以这里是“按我说的做，而不是照我做的”。注释对于团队项目以及当你回到旧代码时提醒自己发生了什么至关重要。在 JavaScript 中，你可以使用 `//` 表示该行剩余部分为注释，或者使用 `/*` 和 `*/` 包围多行注释。JavaScript 解释器会忽略注释。



