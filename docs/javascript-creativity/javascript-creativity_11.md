# 11. 附录

对于一本名为《JavaScript 创造力》的书来说，要缩小内容范围确实很困难，否则它可以一直写下去！因此，这本附录将相当松散，包含一些我想提及但无法放入主体内容的零散信息片段。


### 未来

我们正处在网络发展的一个非常有利的节点，不再需要应对多年来习以为常的诸多限制（老旧浏览器、拨号上网等），但更重要的是，我们能利用的资源比以往更多。浏览器不仅拥有更好的 CSS；它们还具备了本地存储和设备 API 等功能。在本书中，我们大量使用了 Web Audio API 和 WebRTC，但还有许许多多其他的 API。我写这本书所用的笔记本电脑（一台 MacBook Pro）内置了一个加速度计，我可以使用`JavaScript`来读取它的数据。这也许不是最实用的功能，但或许有人能为其找到巧妙的用途（比如测量我此刻正在写书的这列火车的 G 力？）。

我将这本书命名为“JavaScript 创造力”，并不是因为你能用 canvas 做出漂亮的东西（尽管这仍是一项了不起的功能），而是因为如今你可以用创造性的方式使用网络技术。一个通过摄像头控制的钢琴，能即时生成音符？它绝非完美，但我们做到了！一个利用 GPS 根据你所在区域提供相关新闻的网站？完全可行。在第 2 章中，我们从照片生成了线条画，并将其变成了一个涂色书应用——这些事情即便在几年前也是不可能实现的。

网络上正在添加许多功能（或者至少在规范中被提及），其中一些并没有明显的用例。当然，每个功能的提出都有其原因，但有些功能（例如“设备接近度”）可以被用于许多有趣且非显而易见的用途。这非常好，这就是“JavaScript 创造力”。事实上，我见过能在大楼里导航避障的机器人，甚至还有能飞的机器人——它们都是用`JavaScript`编写的。

创造未来技术的关键，一直以来都是提出“如果……会怎样”的问题。如果我能用手指控制键盘会怎样？如果新闻网站知道我的位置会怎样？并非所有问题都有显而易见的答案，我甚至曾认为并非所有东西都应该被创造出来。但思考各种情况和想法总是有价值的，即使它们最初看起来有些怪异。

## 延伸阅读

在撰写本书的过程中，我遇到了许多你可能感兴趣的、非常有趣的文章和书籍。它们并非都与本书各章节直接相关，但无论如何都值得一读。有些非常技术性，例如`JavaScript`的复杂性或某个特定的算法，而其他的则更偏向概念性。正如我在全书中所说，你不应该只学习如何制作某个特定项目，而应该学习如何在各种情况下运用这些技术，因此那些概念性和高层次的文章（尽管不在本书直接范围内）也值得一读。我会在我的网站[`www.shanehudson.net/javascript-creativity/`](http://www.shanehudson.net/javascript-creativity/)上维护一个更新的列表，以及本书的其他资源。

### 算法

本书中提到了许多算法，如果你对更理论化的方面感兴趣，还有很多其他算法值得一读。通常情况下，你可以使用预先写好的算法，比如我们在第 9 章中使用的`js-objectdetect`库，但你们中的许多人可能（像我自己一样）渴望深入了解所使用技术的方方面面。由于算法大多发表在学术期刊上，可能不易在线获取，但通常网上会有解释这些算法的文章（通常是大学讲座的讲义）。我将简要概述几个我推荐你进一步了解的算法，但我会避免涉及数学方面。

在第 2 章中，我们使用了一种朴素的边缘检测算法来将照片转换为线条画。我们通过平均每个像素的相邻像素并将其与阈值进行比较来实现。可以想象，这种方法效率非常低。最终结果虽尚可，但远非完美。在那一章中我提到，有一种更好的方法叫做“Canny 边缘检测”，它是由约翰·F·坎尼在 1986 年提出的。这是一个五步算法。第一步是应用高斯滤波器（一种模糊形式）来平滑边缘，从而减少噪声；例如，这可以平滑桌面上木材的纹理，使其更容易检测到桌子的边缘。第二步是寻找梯度。这些梯度是使用 Sobel 算子找到的，它有助于找到梯度的大小和方向。下一步被称为“非极大值抑制”，本质上意味着将模糊的边缘转换为尖锐的边缘；这主要是通过去除所有低梯度值来实现的。第四步使用阈值。我们在朴素的边缘检测算法中使用了阈值；它们有助于降低噪声出现的几率。我们的版本与 Canny 算法的区别在于，后者使用了两个阈值，称为“双阈值”，这样非常高和非常低的值都被视为边缘，而中间的任何值很可能是噪声。这使得它能够更准确地找到不明显的边缘。最后，为了尽可能排除噪声，弱边缘只有与强边缘相连时才会计入。

说到算法，我想快速提醒你一下我们在第 3 章中通过 Web Audio API 使用的快速傅里叶变换（FFT）。你不需要实现它，因为 FFT 是 Web Audio API 规范的一部分，所以我不会深入细节（此外，它涉及大量数学计算），但值得记住它，你们中的一些人可能渴望深入研究傅里叶变换。从根本上说，这些变换在用于音频时（还有许多其他应用），用于从一段音频中提取频率数据。我们用它来进行音频可视化，但这仅仅触及了直接访问此类原始数据所能做的事情的皮毛。

各章节中提到的最后一个算法是 Viola-Jones 目标识别框架（它被归类为框架，因为原始论文并未描述该算法的实现，只描述了所需的组件），在解释它时，我也简要描述了 Haar-like 特征。这里我不再进一步解释，但请进一步了解 Viola-Jones 算法，甚至尝试自己实现它。

## 链接

在整本书中，我链接了各种资源，我将它们收集于此，以便查找。

*   [`www.apress.com/9781430259442`](http://www.apress.com/9781430259442) —这是本书在 Apress 的官方页面。此处可下载源代码。
*   [`www.shanehudson.net/javascript-creativity`](http://www.shanehudson.net/javascript-creativity) —这是我自己的本书页面。我会尽量保持更新，作为本书的补充资源。这里也提供源代码。

### 第 1 章

*   [`www.html5please.com`](http://www.html5please.com/) —HTML5 Please 就某个功能是否应该在生产环境中使用，或者是否有可用的 polyfill 提供有用的建议。
*   [`www.caniuse.com`](http://www.caniuse.com/) —Can I Use 直接提供了浏览器支持的数据，精确到浏览器版本以及实现与规范之间的不一致之处。

### 第 3 章

*   [`www.incompetech.com/music/royalty-free`](http://www.incompetech.com/music/royalty-free) —Incompetech 有不错的免版税音乐选集。



### 第 4 章

*   [`mrdoob.github.io/three.js/`](http://www.mrdoob.github.com/three.js%20-%20Three.js)—`Three.js`是一个广泛使用的`WebGL`封装库，用于在网页上实现 3D 效果。
*   [`typeface.neocracy.org`](http://www.typeface.neocracy.org/)—在这里你可以找到`Three.js`作为默认字体使用的 Helvetiker 字体。

### 第 5 章

*   [`www.svgjs.com`](http://www.svgjs.com/)—这是一个轻量级的 SVG 库，我们在第 5 章中使用它来处理音乐播放器中的时间轴创建。
*   [`github.com/mattdiamond/Recorderjs`](http://www.github.com/mattdiamond/Recorderjs%20-%20Matt%20Diamond's%20Recorder.js)—Matt Diamond 的`Recorder.js`是一个脚本，它使用 Web Workers 来实现对 Web Audio API 节点的录制。

### 第 6 章

*   [`www.acko.net`](http://www.acko.net/)—Steven Wittens 的网站本身就是一件杰作，里面充满了关于网络数学和图形的精彩文章。
*   `www.acko.net/blog/js1k-demo-the-making-of`—Steven 的这篇特定文章被用作第 6 章的基础，我强烈推荐你阅读它。

### 第 7 章

*   [`www.nodejs.org`](http://www.nodejs.org/)—这是`Node.js`的官方网站，书中所有服务器端代码都使用该语言。
*   [`www.github.com/creationix/nvm`](http://www.github.com/creationix/nvm)—推荐使用 Node 版本管理器（`NVM`）来处理`Node.js`的多个版本。
*   [`www.nodejs.org/api/index.html`](http://www.http//nodejs.org/api/index.html)—这是`Node.js`的文档，也可以从`Node.js`主站首页找到。

### 第 8 章

*   [`www.ietf.org/rfc/rfc5245.txt`](http://www.ietf.org/rfc/rfc5245.txt)—RFC 5245 详细介绍了交互式连接建立（`ICE`）。虽然阅读量很大，但在处理`WebRTC`时非常有用。

### 第 9 章

*   [`www.github.com/mtschirs/js-objectdetect`](http://www.github.com/mtschirs/js-objectdetect)—这个由 Martin Tschirsich 编写的库是基于 Viola-Jones 算法的 JavaScript 实现。它使得进行对象检测变得容易，而无需深究实现细节。

### 索引

A
算法
动画
巧妙的动画
群体行为模拟
同伴
群体半径
辅助函数
`setup()`
音频和视频
属性和特性
`autoplay`
缓冲部分
`controls`
`duration` 属性
`loop` 属性
网络活动
`playbackRate`
`played` 属性
`preload`
`volume` 属性
音频实现
浏览器兼容性
编解码器
AC-3
高级音频编码 (AAC)
H.264
MP3
Theora
Vorbis
VP8
容器
高级系统格式 (ASF)
音频视频交错格式 (AVI)
定义
Matroska
MP4
Ogg
QuickTime 文件格式/MOV
波形音频文件格式 (WAV)
WebM
视频编码
视频实现
Web Audio API
音频分析
`createMediaElementSource`
定义
快速傅里叶变换 (FFT)
频率可视化
`getFloatFrequencyData()`/`getByteFrequencyData()`
视频处理
节点
频谱图
音频视频交错格式 (AVI)

B
断点
条件断点
DOM 断点
事件
异常
浏览器兼容性

C
Canvas
弹跳方块
群体行为模拟
航向变量
笛卡尔坐标系
帧配置文件
`getContext()`
线性插值 (`lerp`)
`mousemove` 回调函数
`constraint`
`ctx.beginPath()`
画笔
微小变化
`useCapture`
矩形
`requestAnimationFrame()` (rAF)
设计模式
DOM 元素
`var ctx = canvas.getContext()`
CSS

D
调试
拖放
样板代码
`e.dataTransfer.files[0]`
`FileReader`
`findEdges` 变量
`ImageData.data`
`onload` 事件处理器
`preventDefault` 方法
`Uint8ClampedArray`

E
发射

F, G, H
每秒帧数 (FPS)
未来展望

I
交互式连接建立 (ICE)
`iceConnectionState`
`iceGatheringState`

J, K, L
JavaScript 控制台提示符
源代码断点
实时编辑变量

M
运动检测
帧
哈尔小波
`js-objectDetect`
跟踪
视频处理
`ctx.getImageData(0, 0, w, h)`
`invertFrame`
`manip.data`
`navigator.getUserMedia()`
基于 Viola-Jones 的面部检测算法
WebRTC
运动跟踪
音乐创作
键盘
客户端代码
`playSound` 函数
服务器端代码
`Socket.IO`
`stopSound` 函数
运动控制
黑键
帧
音乐播放器
`AppView`
`changePlaylist` 函数
`createTrack` 函数
`initialize` 函数
`render` 函数
Backbone 创建
`localStorage`
录制
`recordSound` 函数
事件监听器
`handleDrop` 函数
`player` 对象
播放列表
模型和集合类
视图
规范
可视化
`draw` 函数
`lastPointVisible`
布尔运算
程序实现
乐理
键盘
`mousemove`
八度
振荡器
`osc.noteOn(0)`
参数
QWERTY 键盘
录制
声音管理
时间轴
`var keyboard = SVG('keyboard')`
音高
节奏
速度
波形
定义
锯齿波
正弦波
方波
三角波
波长
频率
半音
声音包络
Web Audio API

N, O
网络地址转换器 (NAT)
Node.js
聊天室消息
核心模块
`createServer`
依赖项
`devDependencies`
不同的模块
`emit()`
`express()` 函数
`Express.js`
`index.html`
`module.exports`
`nodemon`
Node 版本管理器 (NVM)
`package.json`
`response.end()`
`response.writeHead()`
服务器方法
`Socket.IO`
Node 版本管理器 (NVM)

P
播放列表
`Backbone.Collection.extend()`
模型
`Backbone.Model.extend()`
播放列表模型
曲目模型
视图
`Backbone.View.extend()`
`play` 函数
`PlaylistListView`
`renderItem`
`TrackListView`

Q, R
QuickTime 文件格式/MOV

S
会话描述协议 (SDP)
NAT 会话穿透工具 (STUN)

T, U
3D
坐标轴数据可视化
添加数据
数据表示
`drawEdges()` 函数
`drawGraph()` 函数
`drawNode()` 函数
节点图定义
网格
标准笛卡尔坐标图
Three.js
添加网格
代码实现
`DirectionalLight`
`PolyhedronGeometry`
`THREE.MeshLambertMaterial`
`THREE.SphereGeometry`
内置材质
`ExtrudeGeometry`
代码实现
选项
路径
`THREE.Shape` 构造函数
几何体
光照
`AmbientLight`
`DirectionalLight`
`DirectionalLightHelper`
`hex`
`intensity`
`PointLight`
阴影
`SpotLightHelper`
`MeshLambertMaterial`
`MeshPhongMaterial`
场景制作
`CanvasRenderer`
`frustum`
`OrthographicCamera`
`PerspectiveCamera`
`renderer.render`
`WebGLRenderer`
`TextGeometry`
Hello World 创建
选项
时间轴
音频剪辑
DOM 操作
`draggable()`
空事件
每秒帧数 (FPS)
帧
内存提示
录制类
使用中继穿透 NAT (TURN)

V
基于 Viola-Jones 的面部检测算法

W, X, Y, Z
Web Audio API
定义
频率可视化
代码实现
`getByteFrequencyData()`
`setInterval()`
`volumeChange()` 函数
视频处理
反转视频
`requestAnimationFrame()`
节点
频谱图
代码实现
`onaudioprocess`
`requestAnimationFrame()`
WebRTC
客户端代码
`addstream` 事件
`client.js` 文件
`createObjectURL()` 方法
`getUserMedia()` 方法
`mediaConstraints`
`pc.createOffer()` 方法
`received_answer` 消息
`received_candidate` 消息
`received_offer` 消息
`RTCSessionDescription`
代码实现
`getUserMedia()` 方法
服务器
规范
数据通道 API
媒体流 API
点对点连接 API
技术
交互式连接建立 (ICE)
NAT 穿透
会话描述协议 (SDP)
STUN/TURN
