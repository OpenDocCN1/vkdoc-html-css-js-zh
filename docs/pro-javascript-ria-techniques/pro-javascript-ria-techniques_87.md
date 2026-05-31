# 第 12 章：富互联网应用中的无障碍性

## 让进度条更具无障碍性

回顾第 5 章，你已经学习了如何用两个页面元素、一些 CSS 和少量 JavaScript 来构建一个进度条。你可以采用 ARIA 推荐标准，以可用且无障碍的方式为所有用户提供这些信息，从而相当简单地对其进行更新，如下所示：

```html
<!--  polite 属性的 aria-live 值仅会在用户未在页面其他位置进行操作时，告知用户元素内发生的动态更新 -->
<div class="progress-bar" aria-live="polite">
<!-- aria-valuemin 属性表示范围的最小值，aria-valuemax 属性表示该范围的最大值，aria-valuenow 属性表示该范围内的当前值，而 aria-valuetext 属性表示与数值相关的当前文本值 -->
<div class="progress" id="code-progress" role="progressbar" 
aria-valuemin="0" aria-valuemax="100" aria-valuenow="25" 
aria-valuetext="25%">25%</div>
</div>
```

接着，你必须通过 JavaScript，在更新进度条元素宽度的同时，更新 `aria-valuenow` 属性的值，如下列代码行所示，该行表示当前进度位置为 25%：

```javascript
document.getElementById("code-progress").setAttribute("aria-valuenow", "25"); 
document.getElementById("code-progress").setAttribute("aria-valuetext", "25%");
```

#### 验证

在任何浏览器中，为 HTML 元素添加 ARIA 属性都不会产生不良影响，无论该浏览器是否支持 ARIA。不过，你可能会注意到，如果直接将 ARIA 属性添加到标记中，尝试使用[在线 W3C 验证器 http://validator.w3.org/](http://validator.w3.org/) 验证页面时，会显示一系列错误。这是因为验证器不将 ARIA 属性识别为有效的 XHTML——这确实是正确的，因为 ARIA 属性是 XHTML 的扩展，尚未成为 XHTML 规范本身的一部分。在 ARIA 推荐标准最终确定并得到 W3C 验证器支持之前，我建议你确保页面其余部分验证通过，并直接忽略与 ARIA 相关的错误。这些属性不会在任何浏览器中引起渲染问题。

确保标记仍能通过验证的方法是，使用 JavaScript 通过 `setAttribute()` DOM 方法以编程方式设置 ARIA 特定属性及其值：`document.getElementById("live-region").setAttribute("aria-live", "polite");` 由于 ARIA 仅在 JavaScript 启用时才有意义，因此以这种方式在 Web 应用程序中启用这些属性不会带来无障碍性问题。

#### 测试

如果你本人并非屏幕阅读器用户，那么针对屏幕阅读器用户所受到的 Web 应用程序影响进行测试会非常困难。仅通过下载屏幕阅读器软件并听取其输出来测试，并不能准确反映日常使用该软件的用户是如何与 Web 应用程序交互的。在这种情况下，你能进行的最佳测试是用户测试。请认真对待此类测试的结果，并考虑每一条有助于为辅助技术用户改进 Web 应用程序的建议。



