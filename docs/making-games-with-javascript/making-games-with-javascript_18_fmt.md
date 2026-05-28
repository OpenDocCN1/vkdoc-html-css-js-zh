# 14. 游戏手柄

我们快要完成了。在分别之前，我想尝试一下游戏手柄会很有趣。虽然只有少数浏览器通过 JavaScript 支持它们，但它们确实非常好玩！

## 事件处理

游戏手柄事件与我们目前看到的键盘和鼠标事件的工作方式略有不同。由于它们的实验性质，我们需要用一种特定的方式在 `Game.animate` 中捕获它们：

```
constructor(w, h) {
this.w = w
this.h = h
this.state = {
"game": this,
"keys": {},
"clicks": {},
"mouse": {},
"buttons": {},
"objects": [],
"player": null,
"crosshair": null,
}
this.animate = this.animate.bind(this)
}
// ...稍后
animate() {
requestAnimationFrame(this.animate)
this.state.renderer = this.renderer
this.state.stage = this.stage
this.state.player = this.player
this.state.crosshair = this.crosshair
let gamepads = []
if (navigator.getGamepads) {
gamepads = navigator.getGamepads()
}
if (navigator.webkitGetGamepads) {
gamepads = navigator.webkitGetGamepads
}
if (gamepads) {
const gamepad = gamepads[0]
gamepad.buttons.forEach((button, i) => {
this.state.buttons[i] = button.pressed
})
}
// ...剩余的动画代码
}
```

此代码来自 [`http://codepen.io/assertchris/pen/WGzYgA`](http://codepen.io/assertchris/pen/WGzYgA)。

在找到我们所用浏览器支持的游戏手柄列表之前，我们需要尝试几种不同的方法。我使用的是现代版 Chrome 浏览器，它支持 JavaScript Gamepad API。

我们捕获每个按钮的按下状态，并将其存储在 `state.buttons` 对象（在 `constructor` 中初始化）中。考虑到我们在 `Player.animate` 中增加了不少复杂性，我认为是时候对它进行一些重构了：

```
animate(state) {
const leftKey = state.keys[37] || state.keys[65]
const leftButton = state.buttons && state.buttons[13]
const rightKey = state.keys[39] || state.keys[68]
const rightButton = state.buttons && state.buttons[14]
const upKey = state.keys[38] || state.keys[87]
const upButton = state.buttons && state.buttons[11]
const jumpKey = state.keys[32]
const jumpButton = state.buttons && state.buttons[0]
if (leftKey || leftButton) {
this.velocityX = Math.max(
this.velocityX - this.accelerationX,
this.maximumVelocityX * -1,
)
}
if (rightKey || rightButton) {
this.velocityX = Math.min(
this.velocityX + this.accelerationX,
this.maximumVelocityX,
)
}
this.velocityX *= this.frictionX
this.velocityY = Math.min(
this.velocityY + this.accelerationY,
this.maximumVelocityY,
)
state.objects.forEach((object) => {
if (object === this) {
return
}
const me = this.rectangle
const you = object.rectangle
const collides = object.collides
if (me.x  you.x &&
me.y  you.y) {
if (object.constructor.name === "Ladder") {
if (upKey || upButton) {
this.rectangle.y -= this.climbingSpeed
this.isOnLadder = true
this.isOnGround = false
this.velocityY = 0
this.velocityX = 0
}
if (me.y <= you.x - me.height) {
this.isOnLadder = false
}
return
}
// ...剩余的碰撞检测代码
}
})
if ((jumpKey || jumpButton) && this.isOnGround) {
this.velocityY = this.jumpVelocity
this.isOnGround = false
this.isOnSlope = false
}
// ...剩余的移动代码
}
```

此代码来自 [`http://codepen.io/assertchris/pen/WGzYgA`](http://codepen.io/assertchris/pen/WGzYgA)。

这里我们定义了许多常量（来表示按下的键盘键和游戏手柄按钮）。我们很容易假设键盘是存在的，但游戏手柄的存在就没那么确定了。

这就是为什么我们将几个不同的键盘按键组合到一次检查中，而每次检测游戏手柄按钮都需要我们先确保已经定义了游戏手柄按钮。

使用这段代码，我们将方向按钮（D-Pad）和跳跃按钮（PS 兼容手柄上的 A 键）映射到了相应的玩家动作。

## 扳机和摇杆

与按钮相比，扳机和摇杆要复杂得多（并且更容易因游戏手柄设计不同而产生差异）。我使用的是一个兼容 PS 的罗技游戏手柄，扳机和摇杆都被映射到了 `gamepad.axes` 对象中。

捕获的值范围从 `-1` 到 `1`，涵盖所有 6 个轴。在静止状态下，摇杆并不精确为 `0`，这意味着我们需要使用某个 Epsilon 值 ([`https://en.wikipedia.org/wiki/Epsilon`](https://en.wikipedia.org/wiki/Epsilon)) 或四舍五入，来判断轴是否处于静止状态。

我们还需要修改我们的三角函数方程，以适应输入值/比例尺度的差异。我想说的是，我们需要考虑我们打算支持哪些游戏手柄，以及玩家需要使用哪些浏览器才能使用它们。

## 总结

在本章中，我们初探了游戏手柄这片领域。我们很幸运生活在一个浏览器日益支持使用 JavaScript 操作游戏手柄的时代，但距离其变得简单或普及，仍有大量工作要做。

如果你感觉有勇气，或许可以尝试将你游戏手柄的扳机和摇杆映射到游戏操作中（比如瞄准和发射弹丸）。

在此，我想感谢你阅读本书。对我来说，这是一个短暂但激动人心的项目，我希望你也能像我一样（甚至更多）从中学习和享受乐趣。

正如我在开头所说：如有问题或改进建议，请随时联系我。鉴于示例的动态性，我将能够修复错误，并添加关于如何改进它们的评论。

我的 Twitter 个人资料是 [`https://twitter.com/assertchris`](https://twitter.com/assertchris)。

我偶尔也会在编码时进行直播。你可以加入进来，提问（实时地）并和我一起学习。

我的 Twitch 频道是 [`https://www.twitch.tv/assertchris`](https://www.twitch.tv/assertchris)。