## 第 7 章：多媒体播放

利用互联网向消费者广播音频和视频的愿望几乎与 Web 本身的历史一样悠久。多媒体浏览器插件（通常局限于某些制造商开发的特定音频和视频格式）以及向这些插件提供内容的流媒体服务器已经存在多年。近年来，许多 Web 开发者已经摒弃了特定格式的插件，转而选择使用 Adobe Flash Player 浏览器插件，这很大程度上归功于其高普及率。Flash Player 宣称在联网浏览器中的安装用户基数约为 99%（根据 Adobe 自身在 `http://www.adobe.com/products/player_census/flashplayer/` 的统计数据），并且可用于 Windows、Mac、Solaris 和 Linux 平台。

Flash Player 能够显示视频和播放音频。视频最常采用 Flash 视频（`FLV`）文件格式，音频则最常使用流行的 `MP3` 格式。Flash Player 插件版本 10 还增加了对 `H.264` 的支持，这是一种现代化的视频压缩标准，旨在生成更小的视频文件，同时适用于移动设备和桌面电脑。`H.264` 被广泛认为是当前可用的最佳视频压缩方法之一。关于该压缩格式的简要概述可在苹果公司的网站上找到（[`www.apple.com/quicktime/technologies/h264/`](http://www.apple.com/quicktime/technologies/h264/)）。

本章首先讨论如何处理多媒体内容的可访问性这一重要主题。接着介绍一些利用 Flash Player 插件的预置音频和视频播放器组件。最后，我们将展望浏览器原生支持音频和视频的未来。

## 处理可访问性

作为网页开发者，我们最重要的考量之一是为页面内容提供充分的访问途径，这当然也适用于多媒体内容。你的最终用户可能正在通过基于文本的浏览器或移动浏览器浏览网页，或者他们可能没有安装 Flash Player 或未启用 JavaScript。你需要向这些用户提供一些相关的内容来代替视频组件。同样地，对于那些有听力障碍，或者身处无法收听音频的环境中（比如耳机忘在家里了）的用户，你需要提供音频轨道的替代方案。

大多数第三方组件使用 JavaScript 来将网页上特定标签的内容替换为所需的多媒体内容：

```
<div id="replace-with-multimedia- component"></div>
```

这个标签在 HTML 中提供了一个有用的位置，用于存储关于视频或音频的额外信息。它可以包含多媒体文件的元数据、描述，甚至是音频内容的完整文字稿。这些信息可以被那些浏览器不支持该组件的用户读取，也可以被抓取网页以建立索引的搜索引擎读取。

在被替换的标签内，你还可以包含一个指向组件所呈现的原始音频和视频文件的下载链接。这为那些在没有 JavaScript 或 Flash Player 插件的情况下浏览页面的用户提供了一个备选方案，使他们能够看到和听到你的多媒体内容。

另一个可访问性选项是为视频添加字幕。许多 Flash 多媒体组件支持显示字幕。这些字幕可以在一个单独的文件中描述，该文件包含内容的文字稿以及文字对应的音频时间戳。W3C 为字幕文件定义了一项推荐标准，称为 Timed Text，你可以在 [`www.w3.org/AudioVideo/TT/`](http://www.w3.org/AudioVideo/TT/) 上了解更多信息。

## 使用可复用的音频播放组件

绝大多数第三方多媒体组件侧重于视频而非音频。然而，你可能需要单独播放音频，例如为广播网站或在线音乐商店。

在这里，我们将介绍一个功能完备、开箱即用的音频组件，它提供了为网页添加音频播放功能所需的一切。然后，我们将会回顾一些尝试在不使用 Flash Player 的情况下强制浏览器播放音频文件的实验。

> **注意** 对于那些热衷于在网页上播放声音的人，有一点要提醒：不要过度使用！背景音和音效有其存在的意义，但过度或不当使用可能会使用户不愿使用你的应用。请记住，我们的目标不是激怒网页应用的用户，而是给他们带来愉悦的浏览体验。

### SoundManager 组件

网页开发者兼狂热音乐爱好者 Scott Schiller 开发了 `SoundManager`，这是一个即插即用的组件，它利用 Flash Player 插件第 8 版，通过 JavaScript API 暴露其声音播放能力。

为了演示该组件的使用，让我们为一个假设的、销售 `MP3` 音频轨道的在线音乐商店创建一个示例页面，如图 7-1 所示。在这个示例中，你将提供播放音频轨道简短片段的功能，以吸引听者付费下载完整歌曲。当用户点击每首曲目的标题时，你将使用 `SoundManager` 组件来提供这些音频样本。

**图 7-1.** *在线 `MP3` 商店示例，具备试听曲目功能*

首先，访问 `SoundManager` [项目网站 http://www.schillmania.com/projects/soundmanager2/](http://www.schillmania.com/projects/soundmanager2/) 并下载该组件的文件。最重要的文件是 Flash 影片文件（它仅仅向 JavaScript 暴露了 Flash 声音播放器的 API）和 JavaScript API（你将在自己的 JavaScript 代码中使用它来播放页面上的声音文件）。

接下来，构建页面的 HTML 代码，如代码清单 7-1 所示。

**代码清单 7-1.** *在线 `MP3` 商店示例的 HTML 标记*

```
<!DOCTYPE html PUBLIC "- //W3C//DTD XHTML 1.0 Strict//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1- strict.dtd">
[<html lang="en" xml:lang="en">](http://www.w3.org/1999/xhtml)
<head>
<title>SoundManager 示例</title>
<link rel="stylesheet" href="my-example.css" type="text/css" />
</head>
<body>
<h1>在线商店</h1>
<p>点击曲目标题试听简短预览。</p>
<table>
<thead>
<tr>
<th>曲目标题</th>
<th>艺术家</th>
<th>购买</th>
</tr>
</thead>
<tbody>
<tr>
<td id="track1">Ghost Town</td>
<td>Phil And The Spectres</td>
<td><a href="/pay?id=1">立即购买 $0.99</a></td>
</tr>
<tr>
<td id="track2">Higgs Boson</td>
<td>The Large Hadron Colliders</td>
<td><a href="/pay?id=2">立即购买 $0.99</a></td>
</tr>
</tbody>
</table>
<!- - 包含我们的 $ JavaScript 库 -- >
<script src="$.js" type="text/javascript"></script>
<!- - 包含 SoundManager 组件，它将我们的页面与音频播放组件连接起来 -- >
<script src="soundmanager2.js" type="text/javascript"></script>
<!- - 包含我们稍后编写的用于播放音频文件的自定义脚本 -- >
<script src="my- example.js" type="text/javascript"></script>
</body>
</html>
```

`SoundManager` 将在 DOM 加载完成后尽快执行其所有初始化。初始化完成后，`SoundManager` 会触发其 `onload()` 事件，你可以在代码中挂钩该事件来设置你的音频。将代码清单 7-2 所示的代码添加到一个名为 `my-example.js` 的文件中。

**代码清单 7-2.** *在线 `MP3` 商店示例的 JavaScript 代码*

```
// 在 SoundManager 组件初始化后执行此代码，以避免产生任何时序错误
soundManager.onload = function() {
  // 定义我们稍后要播放的两个音频轨道
  var track1 = soundManager.createSound({
    // 此轨道的内部标识符
    id: "track- one",
    // MP3 文件的位置
    url: "/track1.mp3",
    // 音量级别，范围为 0 到 100
    volume: 50
  });
  var track2 = soundManager.createSound({
    id: "track- two",
    url: "/track2.mp3",
    volume: 50
  });
  $.Events.add(document.getElementById("track1"), "click", function() {
    // 当用户点击 id 为 track1 的 HTML 元素时，播放第一个 MP3 文件
    track1.play();
    // SoundManager 的其他方法包括：stop(), pause(), resume(),
    // setVolume(x) 和 mute()
  });
  $.Events.add(document.getElementById("track2"), "click", function() {
    // 当用户点击 id 为 track2 的 HTML 元素时，播放第二个 MP3 文件
    track2.play();
  });
}
```

这是一个简单的示例，但展示了`SoundManager`组件的易用性。

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

## 使用可重用的视频播放组件

网页中的视频播放现已十分普遍，因此了解如何为您的 Web 应用程序添加此类功能至关重要。在这里，我们将介绍两个第三方可重用组件，它们允许您借助 Flash 播放器插件在页面中嵌入视频文件。

正如本章前面所述，请不要忘记那些可能未安装 Flash 播放器插件或禁用了 JavaScript 的用户的可访问性。提供备用内容也能让搜索引擎蜘蛛有内容可读取和索引。

> **注意** 页面上的每个 Flash 组件都会占用用户一定比例的 CPU 时间，而页面中包含多个组件会加剧这种影响。请注意您在页面上包含的 Flash 组件数量，不要过度。请记住，它在您的开发机器上可能运行良好，但您的最终用户可能没有那么幸运地拥有一台性能强大的机器。

此外，也请不要忘记用户的网络速度也可能对每个组件的下载时间产生不利影响。

为了演示这些组件，我们创建一个简单的示例。假设您正在构建一个网站中的帮助页面，该页面播放关于如何安装特定产品的视频教程。此页面提供了一个视频播放组件，其中包含自定义的播放按钮和反馈控件，其外观旨在匹配自定义设计。图 7-2 展示了最终页面的布局。

**图 7-2.** 带有视频播放和自定义控件的视频教程页面

`Listing 7-3` 展示了此页面的 HTML 标记，您可以稍后调整它以适应具体的实现。

**`Listing 7-3`.** 视频教程页面的 HTML 标记

# 视频教程示例

```html
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

`Listing 7-4`. 向视频教程页面添加播放控件的 JavaScript 代码

```javascript
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

```html
<div id="playhead">
  <div id="playhead- position"></div>
</div>
```

使用 CSS 将其样式设置为类似进度条的效果，详见第 5 章。

构建好示例页面的基本框架后，您就可以使用可重用组件在页面中嵌入视频文件，并将页面上的控件与视频文件的播放处理连接起来。我们将介绍两种基于 Adobe Flash Player 的可重用组件：YouTube Chromeless Player 和 JW FLV Player。

## 使用 YouTube Chromeless Player

迄今为止，最流行的视频上传和分享网站是谷歌旗下的 YouTube 网站（`http://www.youtube.com/`）。通过此服务将您的视频文件托管在谷歌的分布式 CDN 上，通常能比您自己托管获得更好的性能，正如您在第 4 章中所了解的那样。

一直以来，您都可以将标准的 YouTube 播放器嵌入到您自己的网站中，但谷歌最近为开发者提供了一个新版本的播放器。这个版本被称为 YouTube Chromeless Player，它移除了所有默认的播放控件，仅保留视频本身。这使您能够创建自己的自定义控件，通过公开的 JavaScript API 来操控 YouTube 视频。图 7-3 展示了带有自定义控件的 YouTube Chromeless Player 示例。

> **注意** 应用程序的*chrome*被定义为主要关注点周围的控件和其他元素。在这种情况下，*Chromeless*表示视频本身周围没有任何控件。

**图 7-3.** 页面上带有自定义 HTML 控件的 YouTube Chromeless Player

使用 Chromeless Player 的要求是 Flash Player 版本 8 或更高，并且自然也需要 JavaScript。YouTube Chromeless Player 的完整参考信息可在以下网址获得：

[`code.google.com/intl/en/apis/youtube/chromeless_player_reference.html`](http://code.google.com/intl/en/apis/youtube/chromeless_player_reference.html)

与大多数第三方组件（它们将所有必需的代码封装在自己的 JavaScript API 中）不同，YouTube Chromeless Player 不包含用于将 Flash 影片组件实际放置到页面中的代码。相反，Google 推荐使用一个名为 `SWFObject` 的第三方 JavaScript 组件，该组件也方便地托管在 Google 的 CDN 上。

### SWFObject 组件

# SWFObject

`SWFObject` 是一个 JavaScript 组件，允许你在页面中嵌入 Flash 影片文件。它还提供了检测当前安装的 Flash Player 插件版本的功能，从而允许你仅当用户浏览器中存在特定版本的播放器时，才嵌入需要该版本的影片文件。它还克服了 IE 浏览器中一个恼人的许可限制，即必须先点击 Flash 内容才能播放。

你可以[从其项目主页](http://code.google.com/p/swfobject/)下载 `SWFObject` 组件。该组件也由 Google 直接托管在 `http://ajax.googleapis.com/ajax/libs/swfobject/2.1/swfobject.js`。在撰写本文时，该组件的当前版本是 2.1。

## 使用 SWFObject 嵌入 Flash

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

## YouTube Chromeless Player 的实现

YouTube Chromeless Player 由两个 Flash 影片文件组成：

*   `apiplayer.swf`：包含播放视频文件的实际功能。
*   `cl.swf`：为第一个 Flash 文件提供一个封装器，提供安全限制，并暴露播放器的 API 函数以供 JavaScript 访问。直接访问此封装 Flash 文件的 URL 是 [`www.youtube.com/apiplayer?enablejsapi=1`](http://www.youtube.com/apiplayer?enablejsapi=1)。

要为示例页面实现 YouTube Chromeless Player，请在

# SWFObject JavaScript 库集成

在*清单 7-3*中代码的末尾处，引入 SWFObject JavaScript 库文件，然后将*清单 7-5*中的代码添加到`my-example.js`（即*清单 7-3*中引用的 JavaScript 文件）中。这会为你的页面添加视频播放功能，并让你页面上的控件能够控制电影文件的播放。

**清单 7-5.** *使用 YouTube 无铬播放器显示视频教程*

```
$.onDomReady(function() {

// 要将播放器嵌入到我们的页面中，只需使用

// SWFObject 将 YouTube 播放器嵌入到页面中

var movieURL = "http://www.youtube.com/apiplayer?enablejsapi=1";

var tagIDToReplace = "movie";

var width = 320;

var height = 240;

var flashVersionRequired = "8";

var expressInstallURL = null;

var flashVars = null;

var parameters = {

allowScriptAccess: "always"

};

var attributes = {

id: "flash-movie"

};

// 实际将视频播放器嵌入到页面中

swfobject.embedSWF(movieURL, tagIDToReplace, width, height,

flashVersionRequired, expressInstallURL, flashVars, parameters, attributes);
```

现在我们已经将播放器嵌入到页面中；接下来需要让它指向我们想要播放的视频文件。可以想见，我们只能链接到由 YouTube 服务本身托管的视频。我们使用播放器的`loadVideoById()`方法，根据分配给每个视频的唯一标识符来加载视频。当 YouTube 播放器准备好进行交互时，会调用一个名为`onYouTubePlayerReady`的特定函数。我们通过将自定义的初始化代码添加到一个同名函数中，来绑定此事件。

```
function onYouTubePlayerReady() {

// 我们之前使用 SWFObject 创建了 id 为 flash-movie 的元素

var youTubePlayer = document.getElementById("flash-movie");

// 这是我们要显示的视频对应的唯一 YouTube 标识符

var videoID = "u1zgFlCw8Aw";

// 从开头开始播放视频

var startTime = 0;

// 将视频加载到播放器中

youTubePlayer.loadVideoById(videoID, startTime);

// 让我们将页面上的控件连接到视频

// 播放器。先从播放/暂停按钮开始。当

// 按下此按钮时，如果视频处于暂停状态，我们想开始播放；

// 如果视频正在播放，则将其暂停。YouTube

// JavaScript API 中的 playVideo() 和 pauseVideo() 方法

// 相当不言自明。我们只需要检测视频是否正在播放，

// 以便切换我们的逻辑。实现方式是使用

// getPlayerState() 方法，该方法返回一个数字，

// 表示播放器的当前状态，即电影是正在播放、暂停、缓冲还是其他状态。

var playPauseControl = document.getElementById("play-pause");

$.Events.add(playPauseControl, "click", function() {

var playingState = 1;

var pausedState = 2;

var currentPlayerState = youTubePlayer.getPlayerState();

if (currentPlayerState == playingState) {

youTubePlayer.pauseVideo();

} else if (currentPlayerState == pausedState) {

youTubePlayer.playVideo();

}

});
```

现在我们可以随心所欲地播放和暂停视频了。接下来，让我们显示电影的时长，该时长将显示在示例页面中视频的旁边。只有当视频关联的元数据加载完成后，才能获取到此信息；根据谷歌的说法，这发生在视频开始播放之后。因此，我们将编写代码等待视频开始播放，当视频开始播放时，获取时长值并将其写入浏览器。我们可以监听播放器状态变化事件，并编写代码在该事件触发时更新页面上的时长值。

```
youTubePlayer.addEventListener("onStateChange", function(newState) {

var playingState = 1;

if (newState == playingState) {

// 如果视频正在播放，获取

// 电影的时长，使其更用户

// 友好，并将其显示在页面上

var durationField = document.getElementById("duration");

var durationInSeconds = youTubePlayer.getDuration();
```

```javascript
var durationInMinutes = durationInSeconds / 60;

var durationFullMinutes = Math.ceil(durationInMinutes);

var durationRemainder = durationInMinutes - Math.floor(durationInMinutes);

var durationRemainderInSeconds = durationRemainder * 60;

var durationText = "Duration: " + durationFullMinutes + "m " + durationRemainderInSeconds + "s";

durationField.innerHTML = durationText;
```

现在我们需要连接播放进度条控件，以便向用户展示影片的播放进度，这里使用第 5 章介绍过的进度控制技术。

每隔一秒钟执行一次以下例程：

```javascript
window.setInterval(function() {
  // 计算当前播放位置在影片总时长中所占的百分比
  var playheadPosition = document.getElementById("playhead-position");
  var currentPlaybackPositionInSeconds = youTubePlayer.getCurrentTime();
  var durationInSeconds = youTubePlayer.getDuration();
  var playbackPositionAsPercentage = (currentPlaybackPositionInSeconds / durationInSeconds) * 100;

  // 更新进度条的宽度以反映这个百分比
  playheadPosition.style.width = playbackPositionAsPercentage + "%";
}, 1000); // 每 1000 毫秒（即 1 秒）执行一次
```

至此，你已经创建了视频教程页面，该页面能够显示 YouTube 服务中的视频，并通过自定义页面控件与之交互。完整的 JavaScript API 还包含本例未使用到的额外功能。有关此 API 的更多详细信息，请参阅文档：[`code.google.com/intl/en/apis/youtube/js_api_reference.html`](http://code.google.com/intl/en/apis/youtube/js_api_reference.html)。

## YouTube Chromeless Player 评估

Google 的 YouTube Chromeless Player 提供了一种简单实用的方式，可以自定义 YouTube 上传视频的回放设计，以匹配你网站的整体风格。由于视频片段、JavaScript API 和 Flash 电影包装器都直接链接自 Google 的服务器，因此性能相当出色。此外，Google 能够动态更新软件以修复错误并添加新功能，而你无需对代码做任何修改。支持该播放器所需下载到浏览器的文件大小也出奇地小。

最明显的限制是你的视频文件必须托管在 YouTube 上，鉴于你想呈现的内容类型，这可能并不合适。YouTube 徽标会以叠加层的形式出现在播放中的视频片段上方，这可能同样会被视为不恰当或分散注意力。JavaScript API 也相当基础，仅提供了控制视频播放所需的最基本功能。

简而言之，只要你不介意通过 YouTube 托管视频，并且只需要基本的播放功能，Google 的 Chromeless Player 很可能是你网站嵌入视频播放的最佳选择。

## JW FLV Player

开发者 Jeroen Wijering 的 Flash Video（FLV 格式）可复用视频播放器组件，目前已推出第四版，是一款成熟、文档完善且广泛使用的第三方组件，用于在网页中嵌入视频。请访问项目主页[`www.longtailvideo.com/players/jw-flv-player/`](http://www.longtailvideo.com/players/jw-flv-player/)并将组件文件下载到你的电脑。

## JW FLV Player 实现

与 YouTube Chromeless Player 类似，JW FLV Player 通过 Flash Player 向页面提供其 JavaScript API。此外，你需要像之前讨论的那样，使用 `SWFObject` 将此 Flash 文件嵌入到页面中。

要在示例页面中实现 JW FLV Player，首先将 `SWFObject` 库添加到代码清单 7-3 中，放在`<body>`标签结束之前：

```html
<script type="text/javascript" src="http://ajax.googleapis.com/ajax/libs/swfobject/2.1/swfobject.js" />
```

接下来，在 `my- example.js` 文件中，使用清单 7-6 中的代码将 JW FLV 播放器嵌入到页面中。

```javascript
// **清单 7-6.** 使用 JW FLV 媒体播放器的视频教程页面

$.onDomReady(function() {

// JW FLV 播放器以 `player.swf` 形式呈现，
// 该文件从项目网站下载

var movieURL = "player.swf";
var tagIDToReplace = "movie";
var width = 320;
var height = 240;
var flashVersionRequired = "8";
var expressInstallURL = null;

// 指定我们想要显示的视频文件，并
// 指示播放器立即开始播放，
// 不在 Flash 影片中显示播放器的默认控件

var flashVars = {
file: "my- video.flv",
autostart: "true",
controlbar: "none",
icons: "false"
};

var parameters = {
allowScriptAccess: "always"
};

var attributes = {
id: "flash- movie"
};

// 将媒体播放器嵌入到页面中
swfobject.embedSWF(movieURL, tagIDToReplace, width, height, ➥
flashVersionRequired, expressInstallURL, flashVars, parameters, attributes);

// 一旦播放器初始化完成，它会调用
// 页面上一个名为 `playerReady` 的函数。我们编写
// 代码来连接我们的控件，通过将我们的
// 代码添加到一个同名的函数中

function playerReady(playerInstance) {
// playerInstance 是一个表示页面上媒体播放器的对象。
// 其 id 表示 HTML 中页面元素的 id

var myJWPlayer = document.getElementById(playerInstance["id"]);
var playPauseControl = document.getElementById("play- pause");

$.Events.add(playPauseControl, "click", function() {
// 当用户点击播放/暂停按钮时，
// 向播放器发送 PLAY 事件，该事件
// 将播放模式在播放和暂停之间切换
myJWPlayer.sendEvent("PLAY");
});

// 播放器允许我们编写代码来钩住
// Flash 组件本身触发的某些事件。
// 在此，我们分配一个名为 `setDuration` 的函数
// （稍后定义），当组件触发 `LOADED` 事件时
// 调用该函数（该事件在视频文件加载完成时触发），
// 我们将用它来填充页面上的时长元素

myJWPlayer.addModelListener("LOADED", "setDuration");

// 稍后定义的 `setPlayheadPosition` 函数
// 将在播放器组件内部触发 `TIME` 事件时被调用
// （该事件以固定间隔发生，每 100 毫秒一次），
// 我们可以利用它来更新进度条

myJWPlayer.addModelListener("TIME", "setPlayheadPosition");
}
});

// `setDuration` 方法将在视频文件加载完成时由 Flash
// 组件调用，并向其传递一个对象字面量，该字面量包含
// 触发该事件的页面上播放器的 id

function setDuration(options) {
var durationField = document.getElementById("duration");

// 使用传递给此函数的 id 查找播放器组件
var myJWPlayer = document.getElementById(options.id);

// 获取播放器组件的当前状态，并
// 据此确定视频文件的时长
var durationInSeconds = myJWPlayer.getConfig().duration;

// 使时长更易于用户阅读
var durationInMinutes = durationInSeconds / 60;
var durationFullMinutes = Math.ceil(durationInMinutes);
var durationRemainder = durationInMinutes - Math.floor(durationInMinutes);
var durationRemainderInSeconds = durationRemainder * 60;
var durationText = "时长: " + durationFullMinutes + "分 " + durationRemainderInSeconds + "秒";

// 将时长输出到页面上的元素中
durationField.innerHTML = durationText;
}

// `setPlayheadPosition` 方法将由 Flash 组件每 100 毫秒
// 调用一次，并向其传递一个对象字面量，该字面量包含
// 视频中的当前播放位置以及视频的总时长

function setPlayheadPosition(options) {
var playheadPosition = document.getElementById("playhead- position");

// 获取视频中的当前播放位置，表示为
// 总时长的百分比
var currentPlaybackPositionInSeconds = options.position;
var durationInSeconds = options.duration;
var playbackPositionAsPercentage = (currentPlaybackPositionInSeconds / durationInSeconds) * 100;

// 根据之前计算出的百分比，
// 将页面上的进度条移动到正确位置
playheadPosition.style.width = playbackPositionAsPercentage + "%";
}
```

至此，您的视频教程页面已准备就绪，其中嵌入了 JW FLV 播放器，所有控件均已连接并正常运行，如前文图 7-2 所示的示例。

## JW FLV 播放器功能集

JW FLV 播放器提供了示例中未包含的若干额外功能：

# JW FLV 播放器功能与评估

- **播放列表支持**：`JW FLV` 播放器支持以 `XML` 格式指定的播放列表文件。您可以指定一个外部文件，其中包含您希望在播放器中显示的影片文件列表的详细信息，播放器将依次播放每个文件。如果您希望在自己的自定义控件中显示所有视频的列表，也可以通过 `API` 访问该播放列表。您甚至可以动态加载播放列表文件，这在您不希望刷新整个页面来加载一组新的视频文件呈现给用户的 `Web` 应用场景中非常有用。

- **渐进式下载**：托管在任何 `Web` 服务器上的标准影片文件将进行渐进式下载和播放；也就是说，影片会部分下载，并在整个影片下载完成之前开始播放。播放期间，影片的其余部分将在后台继续下载。

- **HTTP 流媒体**：`JW FLV` 播放器也支持 `HTTP` 流媒体。这需要您编写少量额外的代码在服务器端运行。使用此功能，最终用户可以直接跳转到视频的任意位置，无论该位置是否已下载到本地计算机。有关如何在自己的服务器上实现此功能的详细信息，请参阅 `JW FLV` 播放器项目主页。

- **无障碍支持**：如前所述，`Web` 应用的无障碍访问是一个重要课题，尤其在多媒体领域。`JW FLV` 播放器支持隐藏式字幕（字幕）和隐藏式音频描述，后者是一种额外的音轨，用于为视障人士提供场景中无法仅通过标准音轨分辨的动作指导。字幕使用 `W3C` 的定时文本格式进行描述，该格式基于 `XML`，用于描述要显示的字幕文本、其开始时间和结束时间。

- **插件支持**：如果 `JW FLV` 播放器组件不能满足您的所有需求，其架构支持添加插件以提供额外功能。还可以通过使用 `API` 的 `JavaScript` 访问插件，这对于发现播放器在特定方面存在不足的开发人员来说极具吸引力。

除了这些功能外，开发人员还在不断添加新功能。您需要随时了解该组件的变化，它旨在成为满足您所有视频播放需求的一站式解决方案。

## JW FLV 播放器评估

`JW FLV` 播放器是一个功能极其丰富、稳定且成熟的组件，可让您轻松地在 `Web` 应用中嵌入视频和视频播放列表。如果您需要的不仅仅是简单地在页面中嵌入一个 `YouTube` 影片，那么它将是您的最佳选择。

该组件在非营利性网站中可免费使用，但用于商业用途则需要购买商业许可。许可费用相当低廉，物超所值。还有一个高级选项，允许您在最多 `50` 个不同的网站和 `Web` 应用中使用该组件。

在文件大小方面，包含并公开 `JavaScript API` 的 `Flash` 组件大小为 `44KB`。这使其比 `YouTube Chromeless` 播放器要大，因此请谨慎选择使用哪个组件。

# 浏览器兼容性

浏览器兼容性非常好。只要最终用户的浏览器安装了 Flash Player 插件，该组件就可以毫无困难地运行。

## 未来：HTML 5 中的音频与视频

撰写本文时，新的 HTML 5 推荐标准仍处于草案阶段。但这并未阻止浏览器厂商挑选一些更有趣的特性纳入自家浏览器，以在竞争中获得优势。HTML 5 中备受关注的领域之一，是在浏览器中实现原生音频和视频播放，无需依赖 JavaScript、Flash 或任何其他外部插件。

> **注：** 本次规范更新中，HTML 5 新增的其他功能还包括：直接在页面上绘制图形、允许离线存储 Web 应用程序数据（当用户网络连接断开时，你仍希望继续运行 RIA）、元素拖放、浏览器历史记录管理以及实时文档编辑（允许开发者构建原生富文本文档编辑器）。

### `<audio>` 和 `<video>` 标签

原生音频和视频支持的实现基于两个新提出的 HTML 标签：`<audio>` 和 `<video>`。在这些标签内放置内容，提供了一种有用的机制，用于向不支持这些标签的浏览器显示替代内容。当某个特性不受支持时提供备用内容的能力，普遍适用于 HTML 5。

让我们看看如何使用`<video>`标签。`<audio>`标签非常类似。以下是用于在浏览器中原生显示视频文件的 HTML 5 代码：

```
<video id="my-video" src="/my-video.mp4" type="video/mp4" autoplay="true">
    抱歉，您的浏览器不支持原生播放视频文件。
</video>
```

`<video>` 标签接受多个可选属性：

- `src`：视频文件的源 URL。
- `type`：要显示的视频文件的 MIME 类型。
- `autoplay`：布尔设置，决定页面加载后是否自动播放视频文件。
- `width` 和 `height`：设置影片的尺寸。
- `controls`：布尔设置，决定是否显示浏览器的默认控件（播放、暂停等）或将其关闭。
- `loop`：布尔设置，决定影片播放完毕后是否从头开始重新播放。
- `poster`：视频文件加载期间显示的图像文件 URL。这通常应该是影片本身的静止帧，以便在最终用户选择播放前提供预览。播放发生后，海报帧在页面刷新前不会再次显示。

由于该标签可能包含其他内容和页面元素作为备用方案，你完全可以考虑通过嵌入在`<video>`标签内的标签来提供基于 Adobe Flash 的视频播放，如下例所示：

```
<video id="my-video" src="/my-video.mp4" type="video/mp4" autoplay="true">
    <div id="my-backup-video">
        <p>抱歉，您的浏览器无法显示我们的视频文件，但您可以
        <a href="/myVideo.mp4">将文件下载到您的计算机</a>
        进行观看。</p>
    </div>
</video>
```

通过 JavaScript，你可以判断 API 是否支持`<video>`标签。如果不支持，则像往常一样通过 Flash 组件加载视频文件。这样，优先选择浏览器的原生视频显示方式，如果不存在则回退到 Flash Player，如果备用组件不受支持或 JavaScript 被禁用，则进一步回退到下载链接，如上述代码示例所示。这种方法提供了良好的可访问性。

### JavaScript API

HTML 5 增加了一个 JavaScript API，并支持这些标签。该 API 允许你通过 JavaScript 控制播放，或设计和构建自己的控件来操作播放中的音频或视频的播放和反馈属性。

可用的方法类似于我们在本章中介绍的第三方多媒体组件所提供的方法，例如 `play()`、`pause()` 和 `load()`。此外，还有一系列由播放文件设置的动态属性，允许你获取当前播放位置、媒体的大小和时长，以及其他可用作向用户反馈的信息。

API 的完整构成还包括一组事件，你可以通过 JavaScript 钩入每个事件。这些事件在响应特定情况时触发，例如播放的启动和停止。甚至还有一个在播放期间定期触发的事件，用于更新你开发的任何用户界面组件，以向最终用户显示播放位置。

### 当前采用水平

目前，对`<audio>`和`<video>`标签的支持在 Safari 3.1 及以上版本、Google Chrome、Opera 9.5 及以上版本（仅视频播放，不含音频）以及 Firefox 3.5 及以上版本中有限可用。虽然这并未构成按市场份额计算的大部分已安装浏览器，但它确实为这些标签及其关联的 JavaScript API 提供了一个有用的测试平台。这使开发者有机会在更广泛采用之前，解决错误和实现细节。

这些早期采用者之间存在争议的一点是音频和视频文件格式以及支持的编解码器。对于哪种是最佳格式，每家厂商都有自己的偏好，并试图将其强加为采用的标准。标准制定机构希望采用一种开源编解码器，但这似乎遭到了一些认为自家编解码器更优越的厂商的反对。就目前情况而言，你会发现在不同浏览器中使用这些标签支持的格式各不相同，因此广泛使用还为时过早。你可以在 [`www.whatwg.org/specs/web-apps/current-work/`](http://www.whatwg.org/specs/web-apps/current-work/) 上阅读最新的进展。

## 总结

本章回顾了几个第三方音频和视频播放器组件，它们使用了 Adobe 的 Flash Player 浏览器插件。你了解了如何在自己的网页中使用这些组件来播放和处理多媒体文件。我们还展望了浏览器对原生音频和视频播放支持的未来。现在，你应该能够在自己的页面中实现多媒体播放，在页面上构建自己的控件，并通过每个第三方组件暴露的 JavaScript API 从这些控件控制媒体的播放。

在下一章中，我们将探讨使用 JavaScript 自定义页面中的表单控件，为表单的填写提供比原生 HTML 控件更友好的用户界面。
