# 设备无关的事件委托

在 JavaScript 中，当用户与特定元素交互时会触发事件，这些事件会沿着 DOM 节点树*冒泡*，依次在每个元素上触发，直到到达树的顶层节点——HTML 文档本身。正如我在第 4 章中解释的那样，您可以利用这种行为，通过在 DOM 树顶层附近的单个元素（例如 `<body>` 标签）上监听页面各处发生的事件。由于事件会冒泡，您可以直接在该元素上捕获它们，而无需将代码附加到每个需要响应事件的元素上，从而提高 Web 应用程序的性能。例如，要监听 Web 应用程序中触发的所有 `click` 事件，可以将以下代码添加到页面中：

```javascript
$.Events.add(document.body, "click", function(e) {
    // 当页面内发生点击事件时要执行的代码放在这里
});
```

不幸的是，某些事件不会从它们发生的元素向上冒泡。这些事件包括设备无关的 `focus`、`blur`、`change` 和 `submit` 事件。人们怀疑这些特定事件类型不冒泡是因为它们并非对所有元素都有意义，不过没有人真正知道它们缺乏这一行为的确切原因。存在一种变通方法可以实现对 `focus` 和 `blur` 事件的委托，如下所述；但是，`change` 和 `submit` 事件则无法实现此类委托。

您可以通过在 `focus` 事件处理程序中将字段的值与 `blur` 事件处理程序中同一字段的值进行比较，来模拟 `change` 事件。如果值发生了变化，您就知道应该触发 `change` 事件，并可以相应地执行代码。不幸的是，`submit` 事件可能通过多种方式触发，因此，在这种情况下，最好直接将此事件处理程序与 `<form>` 元素关联，而不是使用事件委托。值得庆幸的是，在 Web 应用程序的一个页面中，任何时候通常都不会有太多 HTML 表单，因此这对代码性能的影响应该很小。

为了实现对 `focus` 和 `blur` 事件类型的事件委托，在 IE 中，您可以使用专有的 `focusin` 和 `focusout` 事件，它们分别与 `focus` 和 `blur` 同时触发。这些专有事件类型像普通事件一样冒泡，因此您只需将代码与这些事件处理程序关联即可，问题便迎刃而解。

对于其他使用 W3C 标准 `addEventListener()` 方法的浏览器，您需要转而采用与事件冒泡相反的方式，即*事件捕获*，来实现对 `focus` 和 `blur` 事件的委托。使用事件捕获时，事件从 DOM 节点树的顶层开始触发，逐渐向下传递到目标元素，这与您习惯的事件冒泡方向相反。与事件冒泡不同，事件捕获会在 DOM 树中的所有元素上触发事件，无论该事件在特定元素上触发是否有意义。要通过事件捕获阶段（而非冒泡阶段）监听事件，只需将 `addEventListener()` 方法的最后一个参数设置为 `true`（在迄今为止的示例中，该参数均设置为 `false`）：

```javascript
document.body.addEventListener("focus", function(e) {
    // 当页面触发焦点事件时要执行的代码放在这里
}, true);
```

为了将所有这些整合成一个可重用的例程来简化您的代码，让我们修改一下我们一直在使用的 `$` JavaScript 库中的事件添加代码，以处理这种事件异常情况。清单 12-3 显示了应替换 `$` JavaScript 库中当前 `$.Events.add()` 例程的代码。

**清单 12-3.** *改进 $ JavaScript 库中的事件处理*

```javascript
$.prototype.Events.add = function(element, eventType, callback) {
    var self = this;
    eventType = eventType.toLowerCase();
    if (element.addEventListener) {
        // 如果支持 W3C 标准的 addEventListener 方法，则正常关联事件。
        // 如果事件类型是 focus 或 blur，则方法的最后一个参数设置为 true
        element.addEventListener(eventType, function(e){
            callback(self.standardize(e));
        }, (eventType == "focus" || eventType == "blur"));
    } else if (element.attachEvent) {
        // 检测事件类型，并为 focus 和 blur 事件类型切换行为，
        // 改用 IE 专有的 focusin 和 focusout 事件类型
        switch (eventType) {
            case "focus":
                element.onfocusin = function(){
                    callback(self.standardize(window.event));
                }
                break;
            case "blur":
                element.onfocusout = function(){
                    callback(self.standardize(window.event));
                }
                break;
            default:
                element.attachEvent("on" + eventType, function(){
                    callback(self.standardize(window.event));
                });
                break;
        }
    }
}
```

