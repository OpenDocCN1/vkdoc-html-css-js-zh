# 第 3 章 弹跳球

**表 3-5.** (续)

```
<img src="candy.png" />
```

一个 `img` 标签，用于在发生任何操作之前完全加载 `candy.png` 文件。

```
<img src="reunion.jpg" />
```

一个 `img` 标签，用于在发生任何操作之前完全加载 `reunion.jpg` 文件。

```
</body>
</html>
```

最后，表 3-6 描述了弹跳视频中的函数。

**表 3-6.** 弹跳视频的函数

| **函数** | **被谁调用** | **调用谁** |
|---|---|---|
| `init` | `body` 标签中 `onLoad` 的动作 | `startV` |
| `startV` | “点击此处开始”按钮的事件处理调用 | `moveball` |
| `moveball` | 由 `startV` 直接调用，并由 `setInterval` 动作调用 | `moveandcheck` |
| `moveAndCheck` | 由 `moveball` 调用 | – |

请理解，`ended` 事件的事件处理会调用匿名（未命名）函数来重置视频的 `currentTime` 并播放视频。另请注意，我将 `moveBall` 和 `moveAndCheck` 保持为两个不同的函数，是为了遵循其他程序的示例，在这些程序中，`moveBall` 确实有其他任务要完成。

弹跳视频的代码如表 3-7. 所示。我将包含几乎所有代码，尽管其中大部分你应该已经很熟悉。

