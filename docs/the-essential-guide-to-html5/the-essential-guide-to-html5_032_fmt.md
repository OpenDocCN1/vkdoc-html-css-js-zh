# 第 3 章 弹跳球

## 表 3-7. 弹跳视频的代码

| 代码 | 说明 |
| :--- | :--- |
| `<html>` | |
| `<head>` | |
| `<title>Bouncing Video</title>` | |
| `<style>` | |
| `#videoE {position: absolute; display: none; z-index: 1;}` | 设置视频位于图片之上。初始状态不显示。 |
| `#con {position: absolute;}` | 设置 `div` 容器的定位。 |
| `</style>` | |
| `<script type="text/javascript">` | |
| `var rightEdge;` | 虚拟盒子的右边缘。 |
| `var leftEdge;` | 左边缘。 |
| `var topEdge;` | 上边缘。 |
| `var botEdge;` | 下边缘。 |
| `var ballx = 250;` | 弹跳容器/视频的初始 x 坐标。 |
| `var bally = 260;` | 初始 y 坐标。 |
| `var v;` | 将保存对视频的引用。 |
| `var c;` | 将保存对 `div`（我称之为容器）的引用。 |
| `var img;` | 将保存对图片的引用。 |
| `var iWidth;` | 将保存图片的宽度。 |
| `var iHeight;` | 将保存图片的高度。 |
| `var vWidth;` | 将保存视频的宽度。 |
| `var vHeight;` | 将保存视频的高度。 |
| `var ballvx = 14;` | 水平坐标的初始变化量。 |
| `var ballvy = 18;` | 垂直坐标的初始变化量。 |
| `function init(){` | `init` 函数的头部。 |
| `v = document.getElementById("videoE");` | 获取视频对象的指针/引用。 |
| `c = document.getElementById("con");` | 作为视频容器的 `div` 对象。 |
| `img = document.getElementById("AandF");` | 填充 `div` 对象的图片。 |
| `iwidth = img.clientWidth;` | 设置图片的宽度。 |
| `iheight = img.clientHeight;` | 设置图片的高度。 |
| `vwidth = v.videoWidth;` | 设置视频的宽度。 |
| `vheight = v.videoHeight;` | 设置视频的高度。 |
| `leftEdge = 5;` | 将 `leftEdge` 设置为离实际边缘稍远一点的位置。 |
| `rightEdge = leftEdge+iwidth-.6*vwidth;` | 设置 `rightEdge` 以使弹跳迅速发生。 |
| `topEdge = 5;` | 将 `topEdge` 设置为离实际边缘稍远一点的位置。 |
| `botEdge = topEdge+iheight-.6*vheight;` | 设置 `botEdge` 以使弹跳迅速发生。 |
| `}` | 关闭 `init`。 |
| `function startV(){` | `startV` 的头部。 |
| `v.play();` | 开始播放视频。 |
| `v.style.display= "block";` | 使视频可见。 |
| `c.style.top = bally +"px";` | 设置初始 x 坐标。 |
| `c.style.left = ballx + "px";` | 设置 y 坐标。 |
| `v.addEventListener('ended', function(){` | 为视频结束设置事件处理，使用匿名函数。 |
| `v.currentTime = 0;` | 将 `currentTime` 设置为 0，即起点。 |
| `v.play();` | 开始播放视频。 |
| `});` | 关闭匿名函数的定义，并关闭 `addEventListener` 调用。 |
| `moveball();` | 调用 `moveball`。 |
| `setInterval(moveball,100);` | 使用 `setInterval` 函数重复调用 `moveball`。 |
| `}` | 关闭 `startV`。 |
| `function moveBall(){` | `moveBall` 的头部。 |
| `moveAndCheck();` | 调用 `moveAndCheck`。 |
| `}` | 关闭 `moveBall`。 |
| `function moveAndCheck() {` | `moveAndCheck` 的头部。 |
| `var nballx = ballx + ballvx;` | 计算移动对象的可能下一个 x 值。 |
| `var nbally = bally + ballvy;` | 计算 y 值。 |
| 现在开始对每个边缘进行检测。 | |
| `if (nballx < leftEdge) {` | 如果对象位于左边缘的左侧。 |
| `ballvx = -ballvx;` | 反转 `ballvx` 的符号。 |
| `nballx = leftEdge;` | 将下一个 x 位置设置为 `leftEdge`。 |
| `}` | 关闭 if 语句。 |
| `if (nballx > rightEdge) {` | 如果对象位于右边缘的右侧。 |
| `nballx = rightEdge;` | 将下一个 x 设置为 `rightEdge`。 |
| `ballvx = -ballvx;` | 反转 `ballvx` 的符号。 |
| `}` | 关闭 if 语句。 |
| `if (nbally > botEdge) {` | 如果对象位于底边缘的下方。 |
| `nbally = botEdge;` | 将下一个 y 位置设置为 `botEdge`。 |
| `ballvy = -ballvy;` | 反转 `ballvy` 的符号。 |
| `}` | 关闭 if 语句。 |
| `if (nbally < topEdge) {` | 如果对象位于顶边缘的上方。 |
| `nbally = topEdge;` | 将下一个位置设置为 `topEdge`。 |
| `ballvy = -ballvy;` | 反转 `ballvy` 的符号。 |
| `}` | 关闭 if 语句。 |
| `ballx = nballx;` | 现在设置 `ballx`。 |
| `bally = nbally;` | 设置 `bally`。 |
| `c.style.top = bally + "px";` | 使用 px 设置 top 属性。 |
| `c.style.left = ballx + "px";` | 使用 px 设置 left 属性。 |
| `}` | 关闭 `moveAndCheck` 函数。 |
| `</script>` | 关闭 script 元素。 |
| `</head>` | 关闭 head 元素。 |
| `<body onLoad="init();">` | 开始 body。在 body 标签中设置对 `init` 的调用。 |
| `<img id="AandF" src="readers.jpg" width="auto" height="100%"/>` | 设置图像，指定 ID 和源。将宽度设为自动，高度设为 100%，使其在无失真的情况下适应屏幕。右侧可能会留有空白空间。 |
| `<div id="con" width="300">` | 定义一个 div 作为容器。将其宽度设置为与视频匹配。 |
| `<video controls width="300" id="videoE">` | 设置视频并指定 ID。注意：控件存在但难以使用。 |
| `<source src="talk.theora.ogv" type="video/ogg" />` | 提供了三个视频剪辑。它们建议用于不同的浏览器。 |
| `<source src="talk.mp4video.mp4" type="video/mp4" />` | 第二个视频源。 |
| `<source src="talk.webvmp8.webm" type="video/webm" />` | 第三个视频源。 |


`Sorry, your browser doesn't support embedded videos.` | 将根据需要显示错误消息。

`</video>` | 关闭 `video` 元素。

`</div>` | 关闭 `div` 元素。

`<button onclick="startV()">Click to start</button>` | 提供用户交互的按钮。这将调用`startV()`。

`</body>` | 关闭 `body` 元素。

`</html>` | 关闭 `html` 元素。

有很多方法可以制作类似的应用程序。你可以为球选择自己的图像，并尝试使用墙壁的颜色（带或不带渐变）。你可以更改每面墙壁的位置和尺寸。你可以向页面添加文本和 HTML 标记。你可以更改表单的外观。你可以将第一个弹跳应用程序中的表单和其他功能添加到弹跳视频中。当然，你可以添加自己的视频和图像作为背景。一个有用的补充是提供一种访问控件的方式，而不是让它们在屏幕上四处移动。

你可以包含多个球，并跟踪每个球的位置。如果决定使用两个球，则需要两组变量，并且之前的一行代码现在需要两行。一种系统化的方法是使用编辑器中的搜索功能查找所有`ball`的实例，对于每一行，替换为两行。因此，在`ballx`的位置，你将使用`ball1x`和`ball2x`；在`var ballx = 50;`的位置，使用以下代码：

```
var ball1x = 50;
var ball2x = 250;
```

这会将第二个球放置在画布上向右 200 像素的位置。

你还需要一组用于墙壁的所有比较逻辑。

### 第 3 章 弹跳球

如果你想使用两个以上的球，建议考虑使用数组。后续章节将向你展示如何处理对象集合。

你也可以尝试编写代码，让球每次撞墙时速度都减慢。这是一种很酷的效果，且能模拟真实的物理结果。在代码中所有通过改变变量符号来改变方向的位置，添加一个因子来减小绝对值。例如，如果选择将值减小 10%，可以编写如下代码：

```
if (nballx > boxBoundx) {
  ballvx =-ballvx *.9;
  nballx = boxBoundx;
}
```

这意味着垂直方向的增量变化将降至原来的 90%。

你可以通过使用自己的照片并让游戏更具“游戏感”，在棉花糖游戏的基础上进行扩展或从中汲取灵感。思考一下如何判定“落点位置”是否足够好。限制暂停和继续操作的次数。研究本部分其余内容（以及继续阅读 *《HTML5 与 JavaScript 项目》* 一书）中的示例，学习其他操作，例如鼠标或触摸的使用。

#### 测试并上传应用程序

第一个和第三个应用程序可以在 HTML 文档中完整实现。第二个和第四个应用程序需要将图像文件放在同一文件夹中，第五个则需要图像和视频文件。你可以访问网络上的任何文件，但必须确保包含正确的地址。例如，如果将 HTML 文档上传到名为`myGames`的文件夹，并将`pearl.jpg`上传到`myGames`的子文件夹`images`中，则相应的代码行必须为：

`img.src = "images/pearl.jpg";`

同时，你必须使用准确的文件扩展名，例如`.JPG`，以指明正确的文件类型。有些浏览器会容错，但很多不会。你可以尝试提交错误数据，查看不同浏览器的响应。但无论如何，你都应尊重知识产权，不得使用未经许可的图像或视频。
