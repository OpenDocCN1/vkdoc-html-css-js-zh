# 性能调优

## 循环控制：`continue` 与 `break`

你可以使用 `continue` 语句跳过当前循环迭代，直接进入下一次迭代。`break` 语句则会完全终止循环。这两条命令能帮助你找到所需的值，或者跳过与当前循环迭代无关的某些代码块。下面展示了一个同时使用这两条命令的示例。

```
var myArray = [10, 20, 30, 40, 50, 60, 70, 80, 90, 100];

for (var index = 0, length = myArray.length; index < length; index++) {
    if (myArray[index] < 50) {
        // 忽略数组中所有低于 50 的值
        // continue 会立即执行下一次迭代，跳过循环内的任何其他代码
        continue;
    }
    if (myArray[index] == 90) {
        // 忽略数组中所有高于 90 的值
        // break 会立即停止循环迭代，跳过循环内的任何其他代码
        // 循环中不会再执行任何其他迭代
        break;
    }
}
```

## 即将推出：后台 Web Worker 进程

Web 超文本应用程序技术工作组（WHATWG）是一个由开发者和公司组成的社区，致力于推动 Web 新技术的采纳和推荐。目前，该工作组正忙于制定一项名为 Web Workers 的规范，它将允许开发者将 JavaScript 代码分配到后台线程中执行，而不会妨碍用户与浏览器的交互，也不会阻塞其他代码的运行。

这项推荐规范尚处于早期阶段，但已在 Mozilla Firefox 3.5 和基于 WebKit 的浏览器中以初步形态构建。它发布于 [`www.whatwg.org/specs/web-workers/current-work/`](http://www.whatwg.org/specs/web-workers/current-work/)。待规范完成后，你将能够在后台运行耗时较长的代码，而不会影响页面其他部分的性能。（我个人已迫不及待地希望这项规范能完成并实现在浏览器中——它将使核心 JavaScript 性能问题成为历史！）

---

