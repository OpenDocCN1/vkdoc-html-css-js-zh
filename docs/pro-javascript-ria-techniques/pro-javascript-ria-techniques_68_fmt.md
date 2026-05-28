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