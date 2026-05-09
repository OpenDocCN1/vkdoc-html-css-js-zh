# 排版后的文档

**绝对定位**（absolute setting）指的是在文档窗口中通过具体坐标来指定位置的方式。另一种方式是**相对定位**（relative），当文档的一部分位于可能处于屏幕任意位置的包含块中时，可以使用相对定位。

## 第 6 章测验

度量单位是**像素**（pixel），因此从左和从上的位置以`0px`（0 像素）给出，而**边框**（border）、**外边距**（margin）和**内边距**（padding）的测量值分别为 2 像素、5 像素和 5 像素。

现在让我们看看如何使用**样式属性**（style attributes）来定位和格式化这些块。例如，在创建一个用于存放总统姓名的动态元素后，我们可以使用以下几行代码来获取刚创建对象的引用，将包含姓名的文本放入该元素中，然后将其定位在屏幕上的指定点。

```
thingElem = document.getElementBy(uniqueid);
thingElem.textContent = String(i+1)+": "+facts[c][0];
thingElem.style.top = String(my)+"px";
thingElem.style.left = String(mx)+"px";
```

这里，`my`和`mx`是数字。设置`style.top`和`style.left`需要一个字符串，因此我们的代码将数字转换为字符串，并在字符串末尾添加了`"px"`。

#### 响应玩家移动

在`pickelement`函数中，你会看到用于响应和记录玩家移动的代码。`pickelement`头部有一个名为`ev`的参数。然而，还存在一个我们可以称之为*隐式*参数的东西。该函数是因为对特定元素的操作而被调用的。代码中的`this`指向该元素。

在代码中，`this`指向当前实例，即玩家点击的元素。我们为每个元素设置了事件监听，因此当`pickelement`执行时，代码可以使用`this`来引用*听到*点击的特定元素。

当玩家点击一个显示“John Quincy Adams”的块时，代码会*知道*这一点，我在这里用了拟人化的说法，尽管我并不太想这样。

换句话说，同一个`pickElement`函数会被屏幕上的所有块调用，但是，通过使用`this`，代码每次都可以引用玩家点击的特定块。`pickElement`代码从元素中提取 ID，并从`textContent`中提取第一个字符。来自 ID 的信息用于填充一个名为`slots`的数组，该数组将用于检查玩家的排序。来自`textContent`的字符（1、2、3 或 4）将用于向玩家显示已做出的选择。

## 第 6 章测验

我们希望在玩家点击一个框时改变其颜色。我们可以像改变`top`和`left`来重新定位块那样实现。然而，JavaScript 中属性的名称与 CSS 中略有不同：没有连字符。

```
this.style.backgroundColor = "gold";
```

`gold`是一组已定义颜色中的一种，包括`red`、`white`、`blue`等，这些颜色可以通过名称引用。或者，你也可以使用来自 Adobe Photoshop 等程序或在线网站（如[pixlr.com](http://pixlr.com)）的十六进制 RGB 值。

`pickElement`函数执行了另一项任务，我认为它是有用的，尽管说起来有些尴尬——这是我后来才添加的。如果玩家，我们称他为烦人的玩家，多次点击同一个块怎么办？在测试中，我从未尝试过这一点，但我的技术审阅者指出了这个问题。你需要预料到，并为玩家和用户做出奇怪的事情做好准备。修复方法很简单。我使用代码来停止监听点击事件。语句如下：

```
this.removeEventListener('click', functionReference);
```

变量`functionReference`已被设置为指向`pickElement`。

`pickElement`函数提取块 ID 中的原始数字部分并将其转换为数字。该数字被添加（通过`push`）到一个名为`slots`的数组中。当`slots`数组的长度等于`nq`时，调用`checkOrder`函数。

**提示** 你可以在样式部分指定字体。你可以在任何搜索引擎中搜索“安全网页字体（safe web fonts）”，会得到一份据称在所有浏览器和所有计算机上都可用的字体列表。然而，另一种方法是指定一个有序的字体列表，这样如果第一个字体不可用，浏览器会尝试查找下一个字体。

更多信息请参见第[8 章](http://dx.doi.org/10.1007/978-1-4842-4155-4_8)。

#### 呈现音频和视频

HTML5 提供了`audio`和`video`元素，用于在静态 HTML 文档中或通过 JavaScript 控制来呈现音频和视频。

简而言之，音频和视频有不同的文件类型，就像图片一样。文件类型因视频及其关联音频（或单独的音频）的容器、以及视频和音频的编码方式而异。浏览器需要知道如何处理容器、如何解码视频以在屏幕上连续显示帧（构成视频的静态图像）、以及如何解码音频以将声音发送到计算机扬声器。

视频涉及大量数据，因此人们仍在研究压缩信息的最佳方法，例如利用帧之间的相似性而不损失太多质量。现在，网站在手机的小屏幕上以及大型高清电视屏幕上显示，因此利用任何关于显示设备信息的知识都很重要。考虑到这一点，尽管我们希望浏览器制造商将来能统一成为一种格式，但 HTML5 的`video`元素通过引用多个文件提供了一种绕开缺乏标准化问题的方法。因此，开发人员需要制作同一视频的不同版本（包括我们这些创建此测验应用程序的人）。

我下载了一个美国独立日烟花的视频片段，然后使用一个免费工具（Miro 视频转换器）制作了同一短视频片段的三个不同格式的版本。然后，我使用了新的 HTML5 的`video`元素以及`source`元素来编码对这三个视频文件的引用。`source`元素中的`codecs`属性提供了关于`src`属性中指定文件编码方式的信息。然后，我决定不想在烟花视频中使用音频，而是使用传统上为美国总统播放的歌曲“Ruffles and Flourishes”。幸运的是，`video`标签有一个名为`muted`的属性，可以静音视频的音频。我不需要视频和音频完全同步，因此这种方法可行。在`<body>`中，我有：

```
<audio id="ruffles" controls="controls" preload="auto" alt="Hail to the Chief">
    <source src="hail_to_the_chief.mp3" type="audio/mpeg">
    <source src="hail_to_the_chief.ogg" type="audio/ogg">
    Your browser does not accept the audio tag.
</audio>

<video id="vid" preload="auto" width="50%" alt="Fireworks video" muted>
    <source src="sfire3.webmvp8.webm" type='video/webm; codec="vp8, vorbis"'>
    <source src="sfire3.mp4">
    <source src="sfire3.theora.ogv" type='video/ogg; codecs="theora, vorbis"'>
    包含 controls="controls" 会在屏幕上显示熟悉的控件，允许玩家/用户开始或暂停音频剪辑。我没有为视频提供控件。
</video>
```

以“Your browser...”开头的文本仅在浏览器不识别音频元素时才会显示。

## 第 6 章测验

此时，你可能会问：测验开始时，视频和音频控件在哪里？答案是，我使用 CSS 使这两者不显示：

```
audio {visibility: hidden;}
video {visibility: hidden; display: none; position:absolute;}
```

你可能还会问，为什么我不写代码来创建视频和音频元素？



