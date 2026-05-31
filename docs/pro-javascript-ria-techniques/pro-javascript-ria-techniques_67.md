# SoundManager 组件

您可能希望扩展此示例，以便在音频播放时，播放图标切换为暂停图标，而在暂停时，播放图标重新出现。此外，目前可以同时播放多个音频轨道。您可以选择添加一些防御性编码技术来防止这种情况发生。

## SoundManager 功能集

`SoundManager` 组件功能丰富，提供了在网页上播放音频轨道所需的一切。通过 JavaScript API，您可以读取音频的许多动态设置属性，包括文件的时长和大小、在任何时刻已加载的比例、当前播放的音频位置，以及音频当前是正在播放还是已暂停。

在创建 `SoundManager` 对象时，您还可以自行配置某些属性，包括：

- 音频是否应流式播放（边加载边播放）
- 其音量
- 其在立体声场中的声像位置
- 其默认起始位置
- 音频是应自动播放，还是仅加载然后等待用户交互后再播放

每个 `SoundManager` 对象随后会暴露事件，您可以挂钩这些事件，在音频的某些属性发生变化时执行特定代码，例如音频开始播放、暂停、恢复或完成加载时。您甚至可以挂钩在音频播放期间定期调用的事件。例如，您可以编写代码，根据音频的当前播放位置来更新页面中可见进度条的位置。

`SoundManager` 还为安装了 Flash Player 9 或更高版本的最终用户提供了额外的功能，允许显示源自正在播放曲目的波形和均衡器数据。这意味着，如果需要，您可以向用户提供更多的视觉反馈。

## SoundManager 评估

最初的 `SoundManager` 组件自 2004 年起可用，其当前版本自 2007 年起可用。这赋予了产品一定的成熟度；有足够的时间让任何错误浮出水面并被修复，并且在此期间该组件已在许多网站上使用。

正如所料，除非您的最终用户正在运行 JavaScript 并且已安装 Flash Player 8 或更高版本，否则该组件将无法工作。浏览器支持非常好，包括 IE 6 及以上版本、Firefox 1.5 及以上版本、Safari 1.3 及以上版本、Opera 9.5 及以上版本以及 Google Chrome。

性能可以认为是相当不错的，尽管一些较旧的浏览器/机器配置报告称，在执行某些方法到从 Flash Player 获得响应之间存在明显的延迟。这被认为与 Adobe 自己的 Flash 到 JavaScript 的连接接口有关，而不是开发人员具体编码的问题。您很可能不会注意到任何性能缺陷，除非您选择利用 Flash Player 9 特有的功能。众所周知，这种额外的功能会给最终用户的 CPU 带来很大负担，导致页面明显变慢。因此，您应尽可能避开此功能以避免问题。

您应该了解 `SoundManager` 的一些已知限制，这些限制直接源于 Flash Player 8 的限制：

- 加载声音文件的方法仅支持 MP3 文件格式。无论如何，我建议使用这种文件格式，但这可能意味着在页面中使用此组件之前，您需要将音频重新编码为此格式。
- 循环播放音频轨道存在一个问题。如果您有一个希望循环播放的轨道，您会发现在轨道结束与重新开始播放之间存在明显的音频间隙。目前，此问题没有解决方法，因此请注意。

就文件大小而言，该组件相当轻量。该组件的 JavaScript 部分压缩后为 24KB。如果您选择在服务器上使用 gzip 压缩，如第 4 章所述，您会发现当文件下载到最终用户的浏览器时，其大小实际上变为 6KB。执行实际音频播放的 Flash 电影文件只有 4KB，不过如果您想使用 Flash Player 9 特有的功能，则会使用另一个 8KB 的文件来替代它。

总的来说，我对这个组件的评价非常高，因为它几乎支持您可能需要的所有音频播放情况。您可以在页面内构建播放按钮，并让这些按钮触发音频播放。您可以设置音量、声像和其他设置。您还可以构建一个页面组件，用于向最终用户反馈正在播放音频中播放头的位置，并允许他们通过点击组件内的鼠标或按下键盘上的某些预定义键来跳转到音频中的新位置。

### 无需 Flash 播放音频文件

目前，浏览器对原生播放音频的支持相当差。在本章后面，我们将探讨未来支持原生音频和视频播放 HTML 标签的提议。现在，让我们看看开发者 Reinier Zwitserloot 进行的一些实验，这些实验旨在仅使用 JavaScript 在浏览器中实现音频播放。伙计们，这里没有 Flash Player！

他在以下 URL 中写下了他的笔记并展示了演示：[`www.zwitserloot.com/files/soundkit/soundcheck.html`](http://www.zwitserloot.com/files/soundkit/soundcheck.html)。您会发现三个测试，每个测试都包含用于开始和停止测试的“播放”和“停止”按钮：

- 第一个测试使用 JavaScript 在页面上动态添加一个 `<iframe>`，指向一个外部 HTML 文件，该文件仅包含一个链接到未压缩 WAV 音频格式文件的 `<embed>` 标签。
- 第二个测试使用 JavaScript 直接在页面中动态添加一个链接到 WAV 文件的 `<embed>` 标签。其关联的“停止”按钮会从页面中移除该元素；再次单击“播放”按钮会将其添加回页面。该 DOM 元素在 JavaScript 中一直存在，并通过这两个按钮在页面中添加和移除。
- 第三个测试与第二个类似，不同之处在于，当单击“播放”按钮时，每次都会创建一个新的 DOM 对象，而不是重用现有的对象。

测试结果很有趣。总的来说，它们证明，由于不同浏览器的结果不同，围绕该技术构建一个可重用的、跨浏览器的组件的空间不大。

- 在 Opera 中，三个测试均未播放任何声音。
- 在 Safari 中，所有测试都成功开始音频播放；但是，第一个测试在听到音频反馈之前有轻微延迟，并且一旦开始，第二个和第三个测试无法停止音频播放。
- 在 Firefox 中运行第一个测试会产生相同的延迟，之后才能听到或停止音频；但是，在该浏览器的所有三个测试案例中，两个按钮均按预期工作。
- IE 6 拒绝使用第一种技术播放任何声音。第二个测试成功，除了在音频停止后单击“播放”会产生意外结果，即从上一次停止的位置重新开始播放音频。第三个测试在 IE 6 中按预期工作。

Reinier Zwitserloot 还指出，除 Opera 外，所有浏览器都可以同时播放多个声音，并且 MP3、WAV 和 MIDI 音频格式似乎都受支持。



理想情况下，开发人员希望在浏览器中本地播放音频，从而避免对外部 Flash 播放器组件的需求。Reiner 的实验突显了这种支持的不稳定性。在本章末尾，我们将探讨一项关于浏览器内原生音频播放的拟议标准，该标准正获得一些浏览器厂商的支持。目前，在浏览器中播放音频文件的解决方案仍依赖于 Flash 播放器插件。

**使用可重用的视频播放组件**

网页中的视频播放现已十分普遍，因此了解如何为您的 Web 应用程序添加此类功能至关重要。在这里，我们将介绍两个第三方可重用组件，它们允许您借助 Flash 播放器插件在页面中嵌入视频文件。

正如本章前面所述，请不要忘记那些可能未安装 Flash 播放器插件或禁用了 JavaScript 的用户的可访问性。提供备用内容也能让搜索引擎蜘蛛有内容可读取和索引。

■**注意** 页面上的每个 Flash 组件都会占用用户一定比例的 CPU 时间，而页面中包含多个组件会加剧这种影响。请注意您在页面上包含的 Flash 组件数量，不要过度。请记住，它在您的开发机器上可能运行良好，但您的最终用户可能没有那么幸运地拥有一台性能强大的机器。

此外，也请不要忘记用户的网络速度也可能对每个组件的下载时间产生不利影响。

为了演示这些组件，我们创建一个简单的示例。假设您正在构建一个网站中的帮助页面，该页面播放关于如何安装特定产品的视频教程。此页面提供了一个视频播放组件，其中包含自定义的播放按钮和反馈控件，其外观旨在匹配自定义设计。图 7-2 展示了最终页面的布局。

**图 7-2.** 带有视频播放和自定义控件的视频教程页面

`Listing 7-3`展示了此页面的 HTML 标记，您可以稍后调整它以适应具体的实现。

**`Listing 7-3`.** 视频教程页面的 HTML 标记

```
<!DOCTYPE html PUBLIC "- //W3C//DTD XHTML 1.0 Strict//EN" ➥
["http://www.w3.org/TR/xhtml1/DTD/xhtml1- strict.dtd">](http://www.w3.org/TR/xhtml1/DTD/xhtml1- strict.dtd)
[<html lang="en" xml:lang="en">](http://www.w3.org/1999/xhtml)
<head>
<title>Video Tutorial Example</title>
<link rel="stylesheet" href="my- example.css" type="text/css" />
</head>
<body>
<h1>Video tutorial</h1>
<h2>Demonstration 1</h2>
<!- - JavaScript will be used to replace the following tag with the video
playback component -- >
<div id="movie">
<p>The movie could not be displayed. ➥
<a href="movie.mp4">Download the movie to your computer.</a></p>
</div>
<!- - Placeholder element which we will populate with video playback
controls using JavaScript later -- >
<div id="controls"></div>
<!- - Include the $ JavaScript library -- >
<script src="$.js" type="text/javascript"></script>
<!- - Include a custom script we'll write later to initialize the video
playback controls -- >
<script src="build- controls.js" type="text/javascript"></script>
<!- - Include a custom script we'll write later to display the video -- >
<script src="my- example.js" type="text/javascript"></script>
</body>
</html>
```

请注意，HTML 代码相当简洁。您没有在页面本身上包含视频播放控件，而是将通过 JavaScript 构建它们并将其添加到页面。这使得页面对于未启用 JavaScript 浏览器的用户来说不那么令人困惑。如果电影能够在浏览器中显示，您将添加控件；如果不能，用户将看到视频标题和指向视频文件的链接，以便下载到本地离线观看。因此，HTML 代码是仅适用于非 JavaScript 用户的唯一代码。

`Listing 7-4`展示了`Listing 7-3`中 HTML 页面所包含的`build-controls.js` JavaScript 文件的代码，您将使用该代码构建要添加到页面的视频控件。

**`Listing 7-4`.** 向视频教程页面添加播放控件的 JavaScript 代码

```
$.onDomReady(function() {
// 创建持续时间控件，<p id="duration">，插入在<h2>标签之后
var durationControl = $.Elements.create("p");
p.id = "duration";
durationControl.insertAfter(document.getElementsByTagName("h2")[0]);
// 创建播放/暂停控件元素，<div id="play- pause">，插入在
// <div id="controls">标签内
var playPauseControl = $.Elements.create("div");
playPauseControl.id = "play- pause";
playPauseControl.innerHTML = "play / pause";
document.getElementById("controls").appendChild(playPauseControl);
// 创建播放头控件，<div id="playhead"><div id="playhead- position">
// </div></div>，插入在页面上的<div id="controls">元素内
var playheadControl = $.Elements.create("div");
playheadControl.id = "playhead";
var playheadPositionControl = $.Elements.create("div");
playheadPositionControl.id = "playhead- position";
playheadControl.appendChild(playheadPositionControl);
document.getElementById("controls").appendChild(playheadControl);
});
```

现在，您已通过 JavaScript 向页面添加了新控件，您需要在`Listing 7-3`的 HTML 页面中引用的`my-example.css`文件中设置 CSS。请注意，您通过`Listing 7-4`中的 JavaScript 创建的播放头控件由页面上的以下 HTML 代码生成：

```
<div id="playhead">
  <div id="playhead- position"></div>
</div>
```

使用 CSS 将其样式设置为类似进度条的效果，详见第 5 章。

构建好示例页面的基本框架后，您就可以使用可重用组件在页面中嵌入视频文件，并将页面上的控件与视频文件的播放处理连接起来。我们将介绍两种基于 Adobe Flash Player 的可重用组件：YouTube Chromeless Player 和 JW FLV Player。

**使用 YouTube Chromeless Player**

迄今为止，最流行的视频上传和分享网站是谷歌旗下的 YouTube 网站（http://www.youtube.com/）。通过此服务将您的视频文件托管在谷歌的分布式 CDN 上，通常能比您自己托管获得更好的性能，正如您在第 4 章中所了解的那样。

一直以来，您都可以将标准的 YouTube 播放器嵌入到您自己的网站中，但谷歌最近为开发者提供了一个新版本的播放器。这个版本被称为 YouTube Chromeless Player，它移除了所有默认的播放控件，仅保留视频本身。这使您能够创建自己的自定义控件，通过公开的 JavaScript API 来操控 YouTube 视频。图 7-3 展示了带有自定义控件的 YouTube Chromeless Player 示例。

■**注意** 应用程序的*chrome*被定义为主要关注点周围的控件和其他元素。在这种情况下，*Chromeless*表示视频本身周围没有任何控件。

**图 7-3.** 页面上带有自定义 HTML 控件的 YouTube Chromeless Player

使用 Chromeless Player 的要求是 Flash Player 版本 8 或更高，并且自然也需要 JavaScript。YouTube Chromeless Player 的完整参考信息可在以下网址获得：


好的，作为一名高级文档工程师和翻译员，我将严格遵循您提供的注意事项和示例格式，对给定的英文文本进行翻译。

译文如下：


[`code.google.com/intl/en/apis/youtube/chromeless_player_reference.html`](http://code.google.com/intl/en/apis/youtube/chromeless_player_reference.html)

与大多数第三方组件（它们将所有必需的代码封装在自己的 JavaScript API 中）不同，YouTube Chromeless Player 不包含用于将 Flash 影片组件实际放置到页面中的代码。相反，Google 推荐使用一个名为 `SWFObject` 的第三方 JavaScript 组件，该组件也方便地托管在 Google 的 CDN 上。

### SWFObject 组件

`SWFObject` 是一个 JavaScript 组件，允许你在页面中嵌入 Flash 影片文件。它还提供了检测当前安装的 Flash Player 插件版本的功能，从而允许你仅当用户浏览器中存在特定版本的播放器时，才嵌入需要该版本的影片文件。它还克服了 IE 浏览器中一个恼人的许可限制，即必须先点击 Flash 内容才能播放。

你可以[从其项目主页 http://code](http://code.google.com/p/swfobject/).google.com/p/swfobject/ 下载 `SWFObject` 组件。该组件也由 Google 直接托管在 http://ajax.googleapis.com/ajax/libs/swfobject/2.1/swfobject.js。在撰写本文时，该组件的当前版本是 2.1。

使用 `SWFObject` 组件在页面中嵌入 Flash 文件与使用任何其他第三方组件非常相似。首先，将组件的 JavaScript API 包含在 `<head>` 标签内，或 HTML 的底部、`<body>` 标签结束之前：

```html
<script type="text/javascript" src="http://ajax.googleapis.com/ajax/libs/swfobject/2.1/swfobject.js" />
```

在页面中嵌入 Flash 影片文件就像从 HTML 页面引用以下 JavaScript 代码一样简单：

```javascript
//  指定要显示的 Flash 影片的 URL
var movieURL = "my-movie.swf";

// 指定要替换 HTML 页面上的哪个标签。在此例中，id 属性值为 movie 的标签将被替换
var tagIDToReplace = "movie";

// 指定 Flash 影片的尺寸 - 320px x 240px
var width = 320;
var height = 240;

// 指定显示此影片所需的 Flash 版本。使用旧版本插件的用户将看不到 Flash 影片，且 HTML 标签不会被替换
var flashVersionRequired = "8";
var expressInstallURL = null; // 可以安全地忽略此项

// 指定要传递给 Flash 影片的任何变量
var flashVars = {}; // 同样，此项可以忽略

// 指定 Flash 影片的参数
var parameters = {
    // 允许 Flash 和 JavaScript 之间进行通信
    allowScriptAccess: "always"
};

// 指定生成的 HTML 标签（用于在页面上嵌入 Flash 影片）的一些属性
var attributes = {
    id: "flash-movie"
};

// 在页面上嵌入 Flash 影片
swfobject.embedSWF(movieURL, tagIDToReplace, width, height, 
                   flashVersionRequired, expressInstallURL, flashVars, parameters, attributes);
```

`SWFObject` JavaScript 代码在压缩后大小约为 12KB，并且在几乎所有支持 JavaScript 和 Flash 的浏览器中都能很好地工作。

现在你已经了解了如何利用 `SWFObject` 组件在页面中嵌入任何 Flash 影片文件，接下来让我们看看如何借助此组件实现 YouTube Chromeless Player。

### YouTube Chromeless Player 的实现

YouTube Chromeless Player 由两个 Flash 影片文件组成：

*   `apiplayer.swf`：包含播放视频文件的实际功能。
*   `cl.swf`：为第一个 Flash 文件提供一个封装器，提供安全限制，并暴露播放器的 API 函数以供 JavaScript 访问。直接访问此封装 Flash 文件的 URL 是 [`www.youtube.com/apiplayer?enablejsapi=1`](http://www.youtube.com/apiplayer?enablejsapi=1)。

要为示例页面实现 YouTube Chromeless Player，请在



