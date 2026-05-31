# 使用第三种方案

通过第三种方案，你可以将加载操作延迟到确信用户真正想要操作导航时再进行。与无论用户做什么都加载内容不同，这种方式能预判用户的真实需求。清单 5-7 中的代码可实现此行为。

**清单 5-7.** *在用户与占位符交互时加载完整导航 HTML*

```javascript
$.onDomReady(function() {

  // 定义一个通过 Ajax 获取完整导航 HTML 的函数
  var getNavigationHTML = function() {
    $.Remote.load("full-navigation.html", function(response) {
      // 将 HTML 放置在页面正确的位置
      document.getElementById("top-navigation").innerHTML = response.text;
    });
  }

  // 当鼠标移过导航元素时，不再重复执行此函数
  $.Events.remove(document.getElementById("top-navigation"), 
      "mouseover", getNavigationHTML);

  // 当鼠标移动到导航元素上方时执行此函数
  $.Events.add(document.getElementById("top-navigation"), "mouseover", 
      getNavigationHTML);
});
```

## 提前捕获鼠标点击

如果你希望在用户点击页面中的链接或按钮时执行某些 JavaScript 代码，可以考虑绑定`mousedown`事件，而非`click`事件。浏览器会在鼠标按钮释放后触发`click`事件；而`mousedown`事件在鼠标按钮按下时便触发。通过绑定`mousedown`事件，你可以在监听`click`事件之前提前几毫秒执行代码。

```javascript
$.Events.add(document.body, "mousedown", function() {
  alert("我先触发！");
});

$.Events.add(document.body, "click", function() {
  alert("相比之下我慢多了！");
});
```

### 本章小结

在本章中，你了解了一些让网站看起来响应迅速的方法，即使某些操作需要一定时间才能完成。你可以通过给用户提供视觉反馈，来改善他们在耗时操作影响 RIA 性能时的体验。你也学会了如何利用用户浏览器的"空闲时间"预加载和执行他们可能要在页面生命周期后续阶段使用的数据，从而使你的 Web 应用显得更具响应性。对于常常试图在浏览器中复制桌面应用体验的 Web 应用而言，响应性至关重要。

至此，本书关于 Web 应用性能的部分已告一段落。请尝试将你学到的技术应用到自己的 Web 应用中。你会发现用户会对结果非常满意。满意的用户意味着更少的挫败感、更少的抱怨、更满意的客户，以及作为开发者的你更少失眠的夜晚。

本书的下一部分将聚焦于用户界面，并向你介绍多种改善 RIA 呈现效果的技术。具体来说，下一章将展示如何通过终端用户计算机上未安装的自定义字体，为页面应用标题文本。

