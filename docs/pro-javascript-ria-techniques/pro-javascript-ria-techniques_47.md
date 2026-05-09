# 图像压缩与性能优化

在更高的压缩级别下，图像可能会出现肉眼可见的伪影。请选择既能保持图像无视觉异常，又能实现最小文件大小的压缩级别。

## PNG 格式

PNG 图像文件格式是为了改进 GIF 文件格式而开发的，同时保持开放且无专利限制。虽然这种文件格式是三种格式中最新的，但它在现代浏览器中得到了普遍支持。

PNG 文件格式能够存储 24 位图像调色板——在单个压缩文件中表示超过 1600 万种颜色——并且还支持 alpha 通道透明层。这意味着，与 GIF 限制性的单色透明度不同，可以对图像的任意部分独立应用不同级别的透明度。

**注意：** IE 6 默认不支持 PNG alpha 透明度层。尝试使用此功能会导致在图像的透明部分出现浅蓝色背景。您可以通过使用 `CSS AlphaImageTransform` DirectX 过滤器（但正如本章前面讨论的那样，这有巨大的性能缺陷）或重新将图像导出为其他格式（如 `GIF` 或 `PNG-8`，一种支持单色透明度、类似于 `GIF` 的 `PNG` 变体）来绕过此问题，这会在浏览器中显示时减少或移除图像的透明度。

与 `JPEG` 不同，`PNG` 是一种**无损**图像格式，这意味着原始图像中的所有数据在压缩后都保留在文件中，并且可以在任何时候恢复。应用于图像的压缩方法类似于 `GIF` 图像文件格式，数据只是以不同的方式表示，使用 `deflate` 压缩方法进行压缩，该方法也可用于压缩来自 Web 服务器的 HTTP 响应数据，如本章前面所述。

在几乎所有情况下，`PNG` 图像的文件大小都比同等的 `GIF` 图像要小（除了非常小的图像，通常小于 16 像素见方），尽管它们能够存储和表示更多的颜色。

由于文件体积更小，`PNG` 是 Web 上大多数图像的首选格式。您应该只在图像尺寸较小（使用 `PNG` 时结果文件更大）时使用 `GIF`。`JPEG` 格式应用于复杂的图像，如照片，其压缩算法效果最好，能移除人眼难以察觉的图像成分。

### 优化 PNG 图像

`PNG` 图像文件包含的不仅仅是原始图像数据。文件开头还提供了一组头数据来描述文件其余部分包含的图像。每组数据称为一个*块*。有些块是必需的，包括调色板和图像数据。许多块是可选的，如图像伽玛和白平衡数据。

由于 `PNG` 文件只需包含这些必需数据，您可以删除文件中的可选数据块来减小其体积。通常，图形软件在保存 `PNG` 文件时会保留可选块，因此您需要找到一种方法来移除这些块以减小文件大小。

大多数操作系统都提供一个命令行工具，允许您从 `PNG` 文件中剥离可选数据块。该工具名为 `Pngcrush`，可从 http://pmt.sourceforge.net/pngcrush/ 下载。您可以在自动化构建环境中自动对图像运行此工具，也可以根据特定的开发设置手动运行它。方便的是，一个名为 `Smush it`（http://www.smushit.com/）的网站使这项任务变得更加简单。将该网站指向您自己网站的 URL，或上传您的图像到该网站，它会生成一个包含所有图像且已移除所有不必要数据块的 zip 文件。该网站还在可以节省文件体积的情况下，将您的 `GIF` 和 `JPEG` 图像转换为 `PNG` 格式。该网站还提供 Mozilla Firefox 浏览器扩展，允许您按需对当前浏览器窗口中显示的站点应用优化。

### 不要忘记 Favicon

收藏夹图标，通常称为 *favicon*，是一个位于 Web 服务器根目录下、默认名为 `favicon.ico` 的图像文件。它是一个小图标（通常为 16×16 像素），显示在浏览器地址栏 URL 的左侧。当保存为浏览器“收藏夹”时，它也用作代表该 URL 的图像，因此得名。

每当您浏览网站时，浏览器都会请求这类文件，并且没有办法通过代码在所有浏览器中关闭此功能。因此，您应确保该位置存在一个图像，以避免浏览器收到 HTTP 404 错误（请参阅本章前面的讨论以了解为什么这是不好的）。图像越小越好——最好小于 1KB。同时，在文件上设置 HTTP 响应头，以确保它缓存在用户机器上，不会在每次访问该站点时都被下载。

## 优化 JavaScript 性能

高效的 JavaScript 代码对任何 RIA 都至关重要，因为它有能力让最终用户感到满意或沮丧。随着代码日益复杂，问题潜入的机会也更大，包括内存泄漏、锁定浏览器的性能密集型任务，以及缺乏对可用性和性能的考虑。

请记住，JavaScript 是一种客户端语言，其性能与执行它的浏览器以及运行该浏览器的设备性能直接相关。考虑到这一点，请考虑使用 Web 服务器来执行密集型任务，例如构建 RIA 的初始视图，或对大量数据进行计算分析。任何会导致最终用户等待过长时间才能得到结果的操作，都应尽可能在服务器上执行。

在本节中，我们将研究如何改进您的 JavaScript 代码，以最大限度地减少对服务器或最终用户的负面性能影响。这里涵盖的主题并非详尽无遗。由于 JavaScript 是一种大型编程语言，针对特定函数、循环和例程有许多大大小小的性能调整可能性。这里包含的材料针对构建 RIA 进行了定制，因此大部分信息专注于 DOM 和 Ajax 例程的性能影响。

### 使用 Dojo ShrinkSafe 压缩 JavaScript 文件

我们再次回到文件中额外信息的话题。JavaScript 文件充满了空白字符，并且（希望如此）也有大量的注释和文档。这些信息对于需要理解文件中存储的代码的开发人员来说非常理想，但浏览器不需要任何这些信息——它在接收后就会被忽略，所以没有发送它的必要。

根据您的文件，通过剥离不必要的空白字符和注释（这个过程称为*代码压缩*），您可以削减大约 50% 的 JavaScript 文件大小。您可以通过使用称为*代码混淆*的过程进一步减小文件大小，该过程会缩短变量名和函数名。但请注意，代码混淆通常相当有风险，因为它没有考虑被压缩的文件在其所在站点的上下文中是如何使用的。



Dojo JavaScript 库的开发者构建了他们自己的 JavaScript 文件压缩器，名为`ShrinkSafe`，它同时使用了压缩和混淆技术。你可以从`http://shrinksafe.dojotoolkit.org/`下载[此工具](http://shrinksafe.dojotoolkit.org/)。

`ShrinkSafe`的工作方式比大多数混淆器更智能，它通过仅更改脚本中无法从外部公开访问的变量名和函数名，消除了固有风险。这确保了文件中的任何公共 API 或代码在网站的上下文中仍能正常工作。你应该使用此工具来尽可能减小代码体积，以确保浏览器能以最快速度下载。

### 通过 CDN 访问 JavaScript 库

前面你了解到，使用 CDN 可以减少数据在互联网上传输的距离。许多 JavaScript 库（例如`jQuery`和`Prototype`）都托管在 CDN 上，为最终用户提供更快的网络响应时间。它们还有一个额外的好处：如果最终用户之前访问过另一个使用相同 URL 引用相同库的网站，该库可能已经存在于其浏览器缓存中。

Google 正致力于成为 CDN JavaScript 库托管领域的领导者。在撰写本文时，它提供了以下库和第三方 JavaScript 组件的访问：

- `jQuery`（及其相关的`jQuery UI`库）
- `Prototype`（及其相关的`script.aculo.us UI`库）
- `Dojo Toolkit`
- `Yahoo! UI`
- `MooTools`
- `SWFObject`（一个用于在页面中嵌入 Adobe Flash 的有用库）

每个库都可以通过一个固定 URL 直接获取，要么是完整的原始文件，要么是经过压缩的版本（体积通常缩小了 50% 以上）。Google 关于此服务的完整文档可在`http://code.google.com/apis/ajaxlibs/`[在线获取](http://code.google.com/apis/ajaxlibs/)。

从与当前网站不同域名访问文件的一个缺点是，需要进行额外的 DNS 查找来定位新域名的 IP 地址。值得庆幸的是，每个引用文件的下载速度提升远远超过了这一查找时间。CDN 允许用户从物理位置离其浏览器最近的服务器获取文件副本，从而确保网络性能瓶颈得到限制。

### 时机至关重要

当 JavaScript 文件被正常加载时（即通过 HTML 文件中的文件引用），浏览器会阻止页面其余部分的下载和解析，直到该脚本下载并执行完毕。这并不理想，尤其是在需要下载大量代码的情况下。有一种方法可以使用 JavaScript 本身来克服此性能瓶颈，但你必须谨慎行事。脚本通常会阻塞浏览器，以避免*竞争条件*——即一段代码在另一段代码之前完成，导致代码执行顺序错误。如果你打算以这种方式加载脚本，则需要一种技术，让一段 JavaScript 代码在文件下载完成后执行，以防止竞争条件。

第 4 章 ■ 性能调优

**165**

清单 4-1 展示了如何在不阻塞浏览器的情况下按需加载 JavaScript 文件，以及如何在文件下载完成后指定执行某些代码。将此代码添加到你在第 2 章创建的`$` JavaScript 库中。

**清单 4-1.** *在不阻塞浏览器的情况下按需加载 JavaScript 文件*
```
$.prototype.Remote.loadScript = function(fileName, callback) {

  var scriptTag = document.createElement("script");

  scriptTag.src = fileName;

  if (callback) {

    scriptTag.onload = callback;

    scriptTag.onreadystatechange = function() {

      if (scriptTag.readyState == 4) {

        callback();

      }

    }

  }

  document.getElementsByTagName("head")[0].appendChild(scriptTag);

}

// 示例用法
// 假设页面上已存在 $ 库的实例
// 加载 my-script.js，完成后输出 "script loaded and available"
$.Remote.loadScript("my-script.js", function() {

  alert("script loaded and available!");

});
```



## 第 4 章：性能调优

在第 2 章中，我介绍了在页面的整个 DOM 初始化完毕并可访问后立即执行 JavaScript 代码的能力。这发生在整个页面加载完成之前，因为它只考虑 HTML 已加载，而不考虑任何引用的外部资源（如图片）。通过编写代码使其在此事件触发时执行，如清单 4-2 所示，您可以让代码更早开始工作，从而减少最终用户的额外等待时间。您可以使用此事件并行加载多个 JavaScript 文件，采用清单 4-1 中概述的技术。

**清单 4-2.** 当 DOM 就绪时立即执行脚本

```
$.onDomReady(function() {
    // 此函数在 DOM 可访问时立即执行
    // 同时加载两个 JavaScript 文件：my-script.js 和 my-other-script.js
    $.Remote.loadScript("my-script.js", function() {
        // my-script.js 已加载
    });
    $.Remote.loadScript("my-other-script.js", function() {
        // my-other-script.js 已加载
    });
});
```

## 提升核心 JavaScript 性能

我所指的*核心*JavaScript，是指语言中专注于纯数据处理、执行、循环和逻辑的部分，而不是通过 DOM 或 Ajax 技术进行页面操作的部分。在此，我将提出一些改进措施，您可以将其应用到自己的核心 JavaScript 代码中，以加快执行速度。

### 使用记忆化器

*记忆化器*是一种兼作存储机制的函数。它保存该函数例程先前执行的结果，以便将来使用相同输入调用该函数时，能从存储的结果列表中返回输出，而无需重新计算结果。这提高了在代码中被多次调用的函数的性能。

只有那些基于相同输入集始终产生相同输出的函数才能被“记忆化”。

清单 4-3 展示了如何构建一个实用函数，允许您为任何函数添加存储结果值的能力，在可能的情况下自动从其内部存储属性返回函数结果，以提升性能。和之前一样，将此代码添加到`$`JavaScript 库中。

**清单 4-3.** 使用记忆化器加速重复函数的执行时间

```
// memoize 接受一个函数作为输入，并返回具有存储能力的同一函数
$.prototype.Utils.memoize = function(func) {
    return function() {
        // 如果不存在，则为该函数添加一个 memory 对象属性
        func.memory = func.memory || {};
        
        // 创建一个键，用于在 memory 对象属性中存储和检索函数结果。
        // 该键应基于传递给函数的所有参数的组合，以确保基于所有输入组合的唯一性
        arguments.join = Array.prototype.join;
        var key = arguments.join("|");
        
        // 键是否存在于 memory 对象中？
        if (key in func.memory) {
            // 如果存在，则返回关联的值以避免重新计算
            return func.memory[key];
        } else {
            // 如果不存在，则执行关联的函数，然后将结果保存到 memory 对象中
            func.memory[key] = func.apply(this, arguments);
            // 返回新保存的值，即函数执行的结果
            return func.memory[key];
        }
    }
};

// 示例用法
// 假设页面上存在一个$库的实例
// 编写一个计算给定数字阶乘的函数
// - 执行它 99999 次以夸大慢速运行函数的效果
var computeFactorial = function(input) {
    var result;
    for (var count = 0; count < 99999; count++) {
        result = 1;
        for (var num = 2; num <= input; num++) {
            result *= num;
        }
    }
    return result;
}

// 为阶乘函数添加记忆化能力
computeFactorial = $.Utils.memoize(computeFactorial);
```



