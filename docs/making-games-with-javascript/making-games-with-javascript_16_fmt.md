# 12. 动画

我们短暂而愉快的旅程即将结束。是时候添加一些收尾工作了。例如，我们的精灵太呆板了。让我们为它们添加动画吧！

## 为玩家添加动画

玩家精灵是整个游戏中最早也是最重要的部分之一。我们可以花很多时间盯着这个小家伙，如果不添加一点动画，那将非常无聊。

幸运的是，PixiJS 提供了一些工具来简化这个过程。让我们开始使用其中一个：

```javascript
const playerIdleLeftImages = [
  "path/to/sprites/player-idle-1.png",
  "path/to/sprites/player-idle-2.png",
  "path/to/sprites/player-idle-3.png",
  "path/to/sprites/player-idle-4.png",
]
const playerIdleLeftTextures =
  playerIdleLeftImages.map(function(image) {
    return PIXI.Texture.fromImage(image)
  })
const playerIdleLeftSprite =
  new PIXI.MovieClip(playerIdleLeftTextures)
playerIdleLeftSprite.play()
playerIdleLeftSprite.animationSpeed = 0.12
const player = new Player(
  playerIdleLeftSprite,
  new PIXI.Rectangle(
    Math.round(width / 2),
    Math.round(height / 2),
    48,
    56,
  ),
)
player.idleLeftSprite = playerIdleLeftSprite
```

这段代码来自 [`http://codepen.io/assertchris/pen/ALyPGw`](http://codepen.io/assertchris/pen/ALyPGw)。

我们用 `new PIXI.MovieClip` 替换了 `new PIXI.Sprite.fromImage`。我们首先创建了一个图像数组（任何好的精灵包都应该包含这些），并为每个图像创建了新的 `PIXI.Texture` 对象。

我们需要调用 `play` 来启动动画，调整动画速度也无妨。我们很快就会明白为什么存储对动画的引用是个好主意。

**提示：** 没有静态图像能完美展现这个动画。请查看 CodePen 来观看它的运动效果！

## 切换动画

我们不仅可以让角色在静止站立时呈现动画效果，还能为其添加行走、跳跃甚至受伤时的动画。让我们先添加奔跑动画。动画初始化代码整体如下：

```javascript
const playerIdleLeftImages = [
  "path/to/sprites/player-idle-1.png",
  "path/to/sprites/player-idle-2.png",
  "path/to/sprites/player-idle-3.png",
  "path/to/sprites/player-idle-4.png",
]
const playerIdleLeftTextures =
  playerIdleLeftImages.map(function(image) {
    return PIXI.Texture.fromImage(image)
  })
const playerIdleLeftSprite = new PIXI.MovieClip(playerIdleLeftTextures)
playerIdleLeftSprite.play()
playerIdleLeftSprite.animationSpeed = 0.12
const playerRunLeftImages = [
  "path/to/sprites/player-run-1.png",
  "path/to/sprites/player-run-2.png",
  "path/to/sprites/player-run-3.png",
  "path/to/sprites/player-run-4.png",
  "path/to/sprites/player-run-5.png",
  "path/to/sprites/player-run-6.png",
  "path/to/sprites/player-run-7.png",
  "path/to/sprites/player-run-8.png",
  "path/to/sprites/player-run-9.png",
  "path/to/sprites/player-run-10.png",
]
const playerRunLeftTextures =
  playerRunLeftImages.map(function(image) {
    return PIXI.Texture.fromImage(image)
  })
const playerRunLeftSprite = new PIXI.MovieClip(playerRunLeftTextures)
playerRunLeftSprite.play()
playerRunLeftSprite.animationSpeed = 0.2
const player = new Player(
  playerIdleLeftSprite,
  new PIXI.Rectangle(
    Math.round(width / 2),
    Math.round(height / 2),
    48,
    56,
  ),
)
player.idleLeftSprite = playerIdleLeftSprite
player.runLeftSprite = playerRunLeftSprite
game.addObject(player)
game.player = player
```

此段代码来源于 [`http://codepen.io/assertchris/pen/ALyPGw`](http://codepen.io/assertchris/pen/ALyPGw)。

添加动画虽然很有趣，但过程也可能相当繁琐。我们需要为每个动作的每一帧进行裁剪和导出，然后将它们整合到多个图像数组和动画精灵中。

将每个动画精灵的引用赋值给 `player` 是个好习惯。这样，我们就能在 `Player.animate` 内部切换它们：

```javascript
this.rectangle.x += this.velocityX
if (!this.isOnLadder && !this.isOnSlope) {
  this.rectangle.y += this.velocityY
}
if (this.isOnGround && Math.abs(this.velocityX) < 0.5) {
  state.game.stage.removeChild(this.sprite)
  state.game.stage.addChild(this.runLeftSprite)
  this.sprite = this.runLeftSprite
}
this.sprite.x = this.rectangle.x
this.sprite.y = this.rectangle.y
```

此段代码来源于 [`http://codepen.io/assertchris/pen/ALyPGw`](http://codepen.io/assertchris/pen/ALyPGw)。

在 `animate` 方法的末尾，我们可以检测玩家是否仍在移动（或移动幅度很小）。如果不是，则将当前可见的精灵切换为待机动画。

如果玩家仍在移动，则将可见精灵切换为奔跑动画。此逻辑目前仅支持单一方向，但我们可以推断其行为，以涵盖更广泛的动画和方向。

## 小结

在本章中，我们为游戏添加了一些动画（非巡逻类怪物动画）。我们可以花上几个小时看着角色的头发上下摆动。

我们还可以添加多种不同类型的动画：

*   跳跃和/或下落
*   受伤
*   着陆（伴随一阵尘土）
*   爬梯子
*   发射弹丸（手持武器）

虽然我们只添加了两种，但还有很多可以添加。关键在于找到（或设计）一套好的精灵包，并将所有元素完美地拼接在一起。如果你能做到这一点，就能制作出一款画面精美的动画游戏！