# 动画与变换

动画看起来非常令人印象深刻，但实际上实现起来却相当简单。动画本质上就是随时间改变一个或多个属性。过去这需要编写 JavaScript 来设置计时器，并在每次计时事件中操作属性。而现在，你只需使用 CSS 就能完成。

#### 关键帧

在 CSS 中，创建动画的第一步是定义*关键帧*。每个关键帧指定了在特定时间点上的一组 CSS 属性。这大致类似于一部动画电影，它由一组称为帧的图片组成，这些图片被快速连续播放，从而产生画面在运动的错觉。

幸运的是，我们不需要像大多数视频那样每秒提供 30 或 60 帧。

使用关键帧类似于定义渐变，这在第 [12](http://dx.doi.org/10.1007/978-1-4842-2463-2_12) 章中已经解释过。你至少需要定义起始帧和结束帧，浏览器会在它们之间提供平滑的过渡。

不过，你也可以定义额外的帧来控制动画的进程。动画有一个指定的持续时间，每个关键帧被分配到这个持续时间内的一个特定时间点，该时间点以百分比表示。初始帧位于 `0%`，最终帧位于 `100%`。

关键帧有时被称为 `@` 规则（at-rules）。它们类似于普通的 CSS 规则，只是使用百分比作为选择器。在每个帧内部，你可以指定任意数量的 CSS 属性。一个简单的关键帧可能看起来像这样：

```css
@keyframes colors {
  0% {
    background-color: yellow;
  }
  100% {
    background-color: blue;
  }
}
```

在动画开始时，背景颜色是黄色，到动画结束时，颜色逐渐变为蓝色。你还可以添加更多的帧。假设你想要颜色从红色变为黄色，再变为蓝色，然后又变回红色。关键帧可以这样定义：

```css
@keyframes colors {
  0%, 100% {
    background-color: red;
  }
  33% {
    background-color: yellow;
  }
  66% {
    background-color: blue;
  }
}
```

注意，初始帧和最终帧具有相同的值，因此它们可以在同一个帧中指定，百分比之间用逗号分隔，这在 CSS 选择器语法中是逻辑“或”运算符。

另外，`0%` 和 `100%` 可以分别用别名 `from` 和 `to` 来表示。所以，第一组关键帧也可以这样写：

```css
@keyframes colors {
  from {
    background-color: yellow;
  }
  to {
    background-color: blue;
  }
}
```

#### 配置动画

动画被应用于一个或多个元素。例如，前面的关键帧修改了 `background-color` 属性。这需要应用到某个元素上，然后该元素的背景颜色就会随动画变化。为了提供一个简单的演示，我将使用一个空的 `div` 元素：

```html
<div class="circle"></div>
```

为了增加一点趣味性，我将通过以下 CSS 规则把它变成一个红色的圆：

```css
.circle {
  width: 300px;
  height: 300px;
  margin: 10px 0 0 0;
  border-radius: 50%;
  background-color: red;
}
```

这将创建一个高度等于宽度的 `div` 元素。将半径设置为 `50%` 会将这个正方形变成一个圆形。最后，为了给背景颜色添加动画效果，使用以下规则：

```css
.circle {
  animation: colors 5s;
}
```

这里使用了 `animation` 简写属性来指定两个必需的属性。第一个指示要使用的关键帧的名称，也就是之前创建的那个。第二个指定动画的持续时间，这里是 5 秒。当页面加载时，圆形会从红色开始，然后逐渐变为黄色，再变为蓝色，最后变回红色。

## 动画属性

有八个属性可用于配置动画。我之前提到了前两个；以下是完整的集合及其可用选项：

- `animation-name` - 定义要设置的属性及其相对时间点的关键帧名称。


### CSS 动画属性说明

- `-duration` - (默认 `0s`) 单次执行动画所需的总时间。

- `-timing-function` - (默认 `ease`) 指示帧之间的过渡方式。这里有多种选项，Mozilla 有一篇详细描述每种选项的文章，参见 [`developer.mozilla.org/en-US/docs/Web/CSS/timing-function`](https://developer.mozilla.org/en-US/docs/Web/CSS/timing-function)。支持以下常见值：

  - `linear` - 帧之间均匀过渡
  - `ease-in` - 过渡在开始时较慢
  - `ease-out` - 过渡在结束时变慢
  - `ease-in-out` - 过渡在开始和结束时都较慢
  - `ease` - 类似于 `ease-in-out`，但开始时的减速程度小于结束时

- `-delay` - (默认 `0s`) 页面加载后到动画开始前的暂停时长。

- `-iteration-count` - (默认 `1`) 动画应执行的次数；若动画持续运行，可设为 `infinite`。

- `-direction` - (默认 `normal`) 指示动画在关键帧之间是正向还是反向运行。支持以下值：

  - `normal` - 动画按顺序从 `0%` 到 `100%` 遍历关键帧；如果重复执行，则从头开始。
  - `alternate` - 首次执行从 `0%` 到 `100%`，但后续执行将反向进行。奇数次执行为正向，偶数次执行为反向。
  - `reverse` - 动画从 `100%` 到 `0%` 反向执行；后续执行也是反向的。
  - `alternate-reverse` - 首次执行为反向（从 `100%` 到 `0%`），但下一次执行为正向。所有奇数次执行为反向，偶数次执行为正向。

- `-fill-mode` - (默认 `none`) 指定动画前后如何应用关键帧属性。注意，实际初始帧由 `animation-direction` 属性控制；结束帧也取决于使用 `alternate` 方向时执行的迭代次数。支持以下值：

  - `none` - 动画前后不影响元素的值。
  - `forwards` - 动画完成后，目标元素将保留最后一个关键帧的值。
  - `backwards` - 如果动画启动有延迟，目标元素将使用初始关键帧的值。
  - `both` - 同时应用 `forwards` 和 `backwards` 的行为。

- `-play-state` - (默认 `running`) 指示动画当前是运行还是暂停。

> **提示**：时间单位可以用秒或毫秒指定。必须使用 `s` 或 `ms` 后缀，否则该值将被视为数值。值与其单位之间不允许有空格。

### 动画简写

如前所述，所有八个属性都可以使用 `animation` 简写设置。从技术上讲，它们都是可选的，若省略则使用默认值。但是，如果不指定 `animation-name` 和 `animation-duration`，则无法进行任何动画。

简写中的属性由空格分隔，可以按任意顺序指定，但需注意几个细节。有两个时间值：`animation-duration` 和 `animation-delay`。第一个时间值将赋给 `animation-duration`，第二个（如果提供）将用于 `animation-delay`。

有四个属性具有预设的支持值列表，它们不会重叠：`animation-timing-function`、`animation-direction`、`animation-fill-mode` 和 `animation-play-state`。这些属性可以按任意顺序提供，因为浏览器可以通过值来确定将其分配给哪个属性。任何无法识别的值将被赋给 `animation-name` 属性。



