# 优化排版后的文档

作为 Web 开发者，我们的目标之一是确保代码对最终用户来说不被察觉。如果代码足够高效和响应迅速，用户会感觉系统以自然的方式做出响应；一旦这种响应性被打断，用户就会感到沮丧。因此，你必须确保用户永远不会看到提示脚本无响应的消息。

## 将长时间运行的脚本拆分为多个块

如果你正在运行已知需要一些时间才能执行的代码，可能会导致出现此消息对话框，你可以重构 JavaScript 例程，将其分解为更小的块，每个块在短暂的延迟后调用下一个块。浏览器会将其识别为几个较小的脚本，而不是一个较长的例程，从而不会显示运行缓慢的脚本对话框。

要利用此技术，你可以使用清单 5-4 中所示的代码模式（由 Yahoo!的 Julien Lecomte 为 JavaScript 改编，他的个人博客位于 http://www.julienlecomte.net/），将长时间运行的例程分解为更小的块。使用此例程，你可以根据较大例程的当前完成状态更新进度条或其他视觉指示器：

**清单 5-4.** Julien Lecomte 用于将长脚本分解为较小块的代码模式

```
function myLongRoutine(progressFunction [, other arguments]) {
  // 在此初始化例程的数据变量

  // 以下匿名函数表示对数据的一系列操作
  (function () {
    // 在此处理或操作数据

    if ( *still data left to process* ) {
      // 如果还有数据需要处理，执行 progressFunction
      // 传入当前值和总值，使其能够将信息呈现给最终用户，例如作为进度条
      progressFunction(currentValue, total);

      // 在短暂暂停后对数据执行下一组操作，以使浏览器保持响应
      // 使用 0 毫秒的值，确保在浏览器允许的最短暂停后开始下一次操作
      // arguments.callee 表示匿名函数
      window.setTimeout(arguments.callee, 0);
    }
  })(); // 立即开始执行匿名函数
}
```

此技术的一个示例用法可见于清单 5-5 中所示的排序算法中。

**清单 5-5.** 一个拆分为更小代码块的长时间运行的排序例程

```
function sort(progressFunction, data) {
  var counter = 0;
  var total = 1000;
  var progressBar = document.getElementById('code-progress');

  // 创建匿名函数以封装排序算法的每个处理块
  (function() {
    // 排序算法本身
    for (var index = total; index < counter; index-- ) {
      if (data[index] < data[index - 1]) {
        var value = data[index];
        data[index] = data[index - 1];
        data[index - 1] = value;
      }
    }
    counter++;

    // 现在该迭代已完成，执行进度函数以保持用户了解进度
    progressFunction(counter, total, progressBar);

    if (counter < total) {
      // 如果尚未完成排序的所有迭代，则在短暂延迟后执行下一个块
      window.setTimeout(arguments.callee, 0);
    }
  })();
}

// 进度函数仅更新视觉进度条，指示代码已到达的进度
function progress(value, total, progressBar) {
  var percentageComplete = Math.round((value / total) * 100);
  progressBar.style.width = percentageComplete + "%";
}

// 一旦 DOM 准备好被访问，在页面上启动
$.onDomReady(function() {
  // 生成要排序的随机数数组
  var data = [];
  for (var index = 0, length = 1000; index < length; index++) {
    data[i] = Math.floor(Math.random() * length);
  }
  // 使用此数据执行排序例程
  sort(progress, data);
});
```



由于这种技术实际上会中断代码执行以保持浏览器响应，因此整个例程的执行时间会比直接运行更长。你可以通过让每个较小的代码块多次运行（而不是仅运行一次）来减少这种影响。

## 使用定时器多次运行代码块

让长例程的每个代码块多次运行能减少中断次数。你还可以在代码模式中引入一个定时器，确保浏览器不会卡死。

这个定时器将允许每个代码块多次执行，直到达到固定的持续时间，此时会强制发生一次中断。如果你确保这些固定持续时间不超过约 50 毫秒，就能持续保持浏览器响应性，同时允许更多代码无中断执行。可将其视为一种折衷方案，既能保持代码运行，又能让用户满意。

`清单 5-6` 展示了如何修改 `清单 5-5` 的排序函数，通过使用定时器来改善长例程的执行时间。

## 清单 5-6：改进清单 5-5 中排序算法的性能

```
function sort(progressFunction, data) {
  var counter = 0;
  var total = 1000;
  var progressBar = document.getElementById('code- progress');

  (function() {
    // 记录此代码块开始执行的时间
    var startTime = (new Date()).getTime();
    // 保存对该函数的引用以便稍后调用
    var anonFunction = arguments.callee;

    // 将排序算法封装在嵌套函数中，以便在匿名函数内多次调用
    function sortAlgortihm() {
      for (var index = total; index < counter; index-- ) {
        if (data[index] < data[index - 1]) {
          var value = data[index];
          data[index] = data[index - 1];
          data[index - 1] = value;
        }
      }
      counter++;

      // 获取算法执行后的当前时间
      var endTime = (new Date()).getTime();

      if ((endTime - startTime) < 50) {
        // 如果处理时间少于 50 毫秒，则继续排序
        sortAlgorithm();
      } else {
        // 否则已达到持续时间限制，强制中断后再继续执行
        window.setTimeout(anonFunction, 0);
      }

      // 更新进度条
      progressFunction(counter, total, progressBar);
    }

    // 开始执行排序算法
    sortAlgorithm();
  })();
}
```

### 预判网站访客的需求

当你为 Web 应用添加了所有可能的性能增强措施，并在必要时向用户提供反馈后，下一步提升 Web 应用感知性能的方法是尝试预判用户的操作。如果你能很好地预判最终用户在 Web 应用流程的特定阶段会执行什么操作，就可以为此操作做好准备，提前加载额外数据或预先执行某些代码。这样，当用户执行该操作时，就无需等待很长时间才能看到结果。这并非真正改进代码的实际性能，而是分散代码的加载和执行时间，从而调整 Web 应用的感知响应速度。

### 预加载内容

当页面加载完成，用户正在与页面交互时，浏览器存在大量的“空闲时间”，即浏览器既不执行脚本、不加载数据、不渲染页面——什么都不做。你可以利用这段时间预加载数据或执行那些用户稍后与应用程序交互时可能需要的脚本。

组件的“懒加载”有助于加快页面加载时间。通过仅加载显示页面初始视图所需的内容，页面能更快地准备好为用户服务。非必要组件可以通过 JavaScript 在预定时间或基于其他操作按需加载。

思考用户在 Web 应用流程中下一步可能需要完成什么，并利用这些信息来确定可以提前预加载和执行的组件及额外数据，从而为用户提供更好的体验。

一个简单的例子是 Web 邮件客户端，它现在已是大多数在线 Web 应用套件的基本组成部分，试图在浏览器窗口内重现桌面电子邮件包的功能。登录 Web 邮件客户端时的标准初始视图是收件箱。当 Web 邮件用户进入收件箱时，他们很可能想要打开并阅读任何未读邮件。大多数情况下，Web 邮件客户端在用户点击他们想要查看的邮件之前不会加载任何邮件。然后邮件数据被加载，数据被应用到 HTML 模板，并通过 DOM 操作放置在页面中，向用户显示邮件。

通过在 Web 应用的初始视图加载完成后预加载未读邮件数据，可以减少用户看到邮件的时间，这比通常的用户点击邮件时再加载数据的方式有了立竿见影的改进。图 5-4 展示了描述此行为的流程图。

**图 5-4.** *处理未读邮件消息的流程*

### 高效加载导航层级

许多大型网站的一个常见特性是页面顶部的下拉导航菜单。当用户将鼠标移到顶部导航上时，子导航会出现在其下方，如图 5-5 所示。

**图 5-5.** *多级主页导航*

通常，整个菜单结构会在页面中以 HTML 编码，如下所示：

```
<ul id="top- navigation">
  <li><a href="/">Home</a></li>
  <li>
    <a href="/products/">Products</a>
    <ul class="sub- navigation">
      <li><a href="/products/1/">Perpetual motion machine</a></li>
      ...
    </ul>
  </li>
  <li><a href="/company/">Company information</a></li>
</ul>
```

然后你会使用 CSS 默认隐藏 `<ul class="sub- navigation">` 元素，当用户将鼠标移到顶级菜单项上时，通过 JavaScript 显示它。

你可以利用懒加载技术，最初仅加载顶级导航的 HTML，不包含任何子级导航，如下所示：

```
<ul id="top- navigation">
  <li><a href="/">Home</a></li>
  <li><a href="/products/">Products</a></li>
  <li><a href="/company/">Company information</a></li>
</ul>
```

这样初始加载的 HTML 更少，使 Web 应用加载更快。

现在假设你有一个更大的导航结构，包含许多顶级链接，每个链接又包含多个子导航项。表示这些结构的代码可能会消耗每个页面多达一半的 HTML，导致页面的初始加载时间比使用此懒加载技术要长得多。

由于现在只加载了这种层级导航的顶级内容，你需要能够通过 JavaScript 使用 Ajax 请求加载完整的导航，然后在页面加载后的某个时间点，用层级导航替换扁平导航。关于何时加载完整导航，你有几个选择：

- 当页面（包括所有外部资源和图片）加载完成后，使用以下代码：
  `$.Events.add(window, "load", functionToLoadMenuHTML);`

- 当 HTML 在 DOM 中可访问时，使用以下代码：
  `$.onDomReady(functionToLoadMenuHTML);`

- 当用户将鼠标移动到导航或焦点移至导航上时



