# 13. 音效

我最喜爱的游戏回忆中，有一部分来自于玩游戏时聆听的音乐和音效。无论是《堡垒》和《菲斯》的音乐，还是《星露谷物语》的音效，我们的耳朵帮助我们充分享受游戏。

## 添加背景音乐

没有什么比一首好的背景音乐更能让你沉浸于游戏之中了。最优秀的游戏会根据玩家的情绪和所在位置切换背景音乐。我们将从一个更简单的方案开始：

```
game.addEventListenerTo(window)
game.addRendererTo(document.querySelector(".camera"))
game.animate()
const music = new Audio("path/to/sounds/background.mp3")
music.loop = true
music.play()
```

点击此链接前请调低音量——因为它会播放音乐。

此段代码来源于 [`http://codepen.io/assertchris/pen/wzmQWb`](http://codepen.io/assertchris/pen/wzmQWb)。

在现代浏览器中播放音效其实相当简单。快速搜索一个可循环的 MP3 文件，再配合这个新的`Audio`对象，基本上就是我们所需要的全部了。不过，如果我们希望背景音乐循环播放，需要将`loop`属性设置为`true`。

**注意：** 没有简便的方法能让`Audio`对象在循环时不出现播放间隙。你可能需要考虑替代方案，比如 SoundManager2 ([`github.com/scottschiller/SoundManager2`](https://github.com/scottschiller/SoundManager2))。

## 添加动作与事件音效

添加游戏音效（针对玩家发起的动作和事件），需要我们深入到动作和事件发生的地方：

```
element.addEventListener("keydown", (event) => {
this.state.keys[event.keyCode] = true
if (event.keyCode === 32) {
new Audio("path/to/sounds/jump.wav").play()
}
})
```

此段代码来源于 [`http://codepen.io/assertchris/pen/wzmQWb`](http://codepen.io/assertchris/pen/wzmQWb)。

当我们第一次尝试跳跃时，音效似乎需要加载一下才能播放。我们可以通过在玩家开始移动前预加载所有音效来解决这个问题。

事实上，通常的做法是在游戏开始前预加载所有游戏资源（如字体、图像和音效）。就音效而言，我们只需要在游戏开始前添加以下代码进行预加载：

```
new Audio("path/to/sounds/jump.wav").load()
```

这将启动加载过程，音效文件应该能在被使用前加载完成（只要网络连接速度足够快）。还有其他方法可以确保所有音效文件都已加载，但它们会使过程变得相当复杂。我认为，这是另一个话题了……

## 小结

在本章中，我们简要了解了如何将背景音乐和动作/事件音效嵌入到游戏中。现代浏览器为此提供了良好的工具，但如果需要支持旧版浏览器，或需要对播放进行更精细的控制，我们始终可以回退到像 SoundManager2 这样的库。

购买大量音效、音乐文件和精灵（或精灵包）可能会很昂贵。你可能更希望在游戏制作过程中邀请图形和音效艺术家参与。

