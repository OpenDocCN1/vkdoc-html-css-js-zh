# 音频元素

`audio`元素允许你指定多个资源来源，浏览器会逐个尝试这些资源，直到找到它支持的格式。你可以在`audio`元素内提供一个或多个`source`元素，而不是直接使用`src`属性，示例如下：

```
<audio autoplay controls>
  <source src="Media/Linus and Lucy.ogg" />
  <source src="Media/Linus and Lucy.mp3" />
  <p>HTML5 音频在你的浏览器上不被支持</p>
</audio>
```

浏览器会使用它支持的第一个资源，因此如果你有偏好，应将首选文件列在前面。例如，Chrome 同时支持 MP3 和 Vorbis 格式。如果你希望使用 MP3 文件，则应将其列在`.ogg`文件之前。

虽然仅像这样列出资源也能工作，但浏览器需要下载并打开文件以判断其能否播放。下载一个相当大的文件后发现无法使用，效率很低。你还应包含`type`属性，该属性指定资源的类型。浏览器可以通过查看标记来判断文件是否受支持。`type`属性指定 MIME 格式，如下所示：

```
<source src="Media/Linus and Lucy.ogg" type="audio/ogg" />
<source src="Media/Linus and Lucy.mp3" type="audio/mp3" />
```

你也可以在`type`属性中指定编解码器，如下所示：

```
<source src="Media/Linus and Lucy.ogg" type='audio/ogg; codecs="vorbis"'/>
```

![](img/index-148_1.jpg)

这将帮助浏览器更高效地选择兼容的媒体文件。请注意，编解码器值包含在双引号内，因此你需要对`type`属性值使用单引号。

**提示：** 这里有一个实用页面，用于测试浏览器对`audio`和`video`元素的支持：[`hpr.dogphilosophy.net/test/`](http://hpr.dogphilosophy.net/test/)。它还提供了各种浏览器支持情况的概览。

