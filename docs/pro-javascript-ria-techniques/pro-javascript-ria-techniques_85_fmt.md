# 聚焦于更新的内容

创建一个表单，通过 Ajax 提交到服务器：

```
<form id="form" method="post" action="/form-result.html">
  <fieldset>
    <legend>个人信息</legend>
    <div class="field">
      <label for="full-name">全名</label>
      <input type="text" name="full-name" id="full-name" />
    </div>
    <input type="submit" value="保存" />
  </fieldset>
</form>
```

创建一个空元素，用于填充服务器返回的表单提交响应：

```
<div id="result"></div>
```

引入 `$` JavaScript 库：

```
<script type="text/javascript" src="$.js"></script>
```

在你的 Web 应用程序中外部引用以下代码（为简洁起见，此处直接包含在页面中）：

```
<script type="text/javascript">
  $.onDomReady(function() {
    // 存储对表单和结果元素的引用，以便后续使用
    var result = document.getElementById("result");
    var form = document.getElementById("form");

    // 当表单提交时，通过 Ajax 发送数据，将响应放入 <div id="result"> 元素中，并将焦点移至该元素
    $.Events.add(form, "submit", function(e) {
      e.preventDefault();

      // 通过 Ajax 将表单数据发送到服务器
      $.Remote.save({
        url: document.getElementById("form").action,
        data: "full-name=" + document.getElementById("full-name").value,
        callback: function(response) {
          // 将服务器返回的 HTML 响应放入 <div id="result"> 页面元素中
          result.innerHTML = response.text;

          // 设置 tabIndex 属性，使该元素可获得焦点
          result.setAttribute("tabIndex", 0);

          // 聚焦到 <div id="result"> 页面元素，该元素现在包含服务器响应
          result.focus();

          // 移除 tabIndex 属性，避免对页面自然焦点顺序造成进一步干扰
          result.removeAttribute("tabIndex");

          // 从页面中移除 <form> 元素，因为结果已返回
          form.parentNode.removeChild(form);
        }
      });
    });
  });
</script>
```

## Web 无障碍倡议：可访问的富互联网应用程序 (WAI-ARIA)

W3C 的 Web 无障碍倡议（WAI）小组旨在让 Web 更具可访问性。他们通过为浏览器和辅助技术制造商以及 Web 开发者提供新的建议，来解决 RIA 中的可访问性问题。这些建议统称为可访问的富互联网应用程序（简称 WAI-ARIA 或 ARIA），涉及以下方面：向用户提醒内容更新；允许对小部件、表单控件以及可拖放元素进行通用访问（无论浏览器、设备或输入方式如何）；以及许多其他主题。你可以在 [`www.w3.org/TR/wai-aria/`](http://www.w3.org/TR/wai-aria/) 阅读这些建议。

截至撰写本文时，对 ARIA 建议的部分或全部支持已内置于 Firefox 3 及以上版本、Opera 9.5 及以上版本、IE 8 及以上版本、Safari 4 及以上版本以及 Google Chrome 2 及以上版本中。此外，JAWS 10 版、Window-Eyes 和 ZoomText 等辅助功能软件也支持 ARIA 建议。

在本节中，我将介绍 ARIA 建议的要点，并展示如何向你自己的 Web 应用程序添加 ARIA 支持，从而为最终用户提供最易访问的 RIA。你应该尽快在自己的 Web 应用程序中采用 ARIA 建议。

### 角色

ARIA 角色是一种为页面元素分配额外语义含义的手段，这使浏览器或辅助技术能更好地理解你为该页面元素及其内容所设计的功能类型。这可以改变该元素内容向浏览器用户呈现的方式，从而让用户更容易理解。

角色不会为元素添加行为；它们只是让浏览器和最终用户知晓相关信息。

你期望的行为表现。你仍需通过 JavaScript 自行实现该行为。

角色（`role`）通过页面元素上的 `role` 属性分配给元素，如下例所示，该例中一个用于网站导航的无序列表被标记为 `role="navigation"`：

```html
<ul id="navigation" role="navigation">
  <li> ... </li>
</ul>
```

`role` 属性的概念最初由 W3C 作为 XHTML 扩展的一部分引入，该扩展称为角色属性模块（Role Attribute Module，你可在此处阅读：<http://www.w3.org/TR/xhtml-role/>）。ARIA 推荐标准增加了具体的属性值及其含义。ARIA 推荐标准中定义了五个角色类别：

*   **用户输入控件（User input widget）**：标记有这些类型角色的元素的行为类似于用于收集用户输入的表单控件，例如滑块（slider）和自定义文本框。

*   **用户界面元素（User interface element）**：这些元素的行为类似于图形用户界面的组成部分，例如按钮、菜单、选项卡和工具提示。

*   **文档结构（Document structure）**：标记有这些角色的元素描述了组织页面内内容的结构。此类别包括用于标记网格、标题和图像的角色。

*   **应用程序结构（Application structure）**：这些元素标记了代表应用程序用户界面的独立区域，例如对话框、进度条和计时器。

*   **地标（Landmark）**：这些元素将页面中旨在作为导航参考点的区域进行标记，例如页眉、导航和主要内容。浏览器应提供一种方式，让用户能够使用键盘或其他适当的机制在这些地标点之间轻松跳转。

完整的角色列表可在 ARIA 推荐标准中找到。表 12-3 展示了该列表的一个小子集。

表 12-3. ARIA 角色属性值精选

| 角色 | 类别 | 含义 |
| :--- | :--- | :--- |
| `button` | 用户界面 | 一种输入控件，允许在按下时触发用户操作。 |
| `combobox` | 用户输入控件 | 一个可选项的下拉列表，同时包含一个文本框，允许用户输入列表中不存在的值。 |
| `dialog` | 应用程序结构 | 一个页面内模态窗口，旨在中断应用程序以提示用户输入某些信息或需要确认。 |
| `grid` | 文档结构 | 表示像表格一样按行和列排列的数据。 |
| `heading` | 文档结构 | 表示页面某个部分的标题。 |
| `main` | 地标 | 表示文档的主要内容。 |
| `navigation` | 地标 | 一组用于在页面内导航或跳转到其他页面的链接。 |
| `progressbar` | 应用程序结构 | 一个显示长时间任务完成进度的元素（如第 5 章中介绍的进度条）。 |
| `slider` | 用户输入控件 | 一种表单控件，允许用户从范围中选择一个值（如第 8 章中介绍的滑块控件）。 |
| `toolbar` | 用户界面 | 表示一组功能性的小型按钮。 |

你可以直接在标记中向元素应用 `role` 属性，或者通过 JavaScript 使用 `setAttribute()` DOM 方法动态设置它：

```javascript
var list = $.Elements.create("ul");
list.setAttribute("role", "navigation");
```

### 状态和属性

# ARIA状态与属性

角色向浏览器提供关于页面中特定元素所包含内容类型的一般信息，而**状态**和**属性**则向浏览器提供具体的详细信息。**状态**指的是其值在 Web 应用程序使用期间可能发生变化的元素，而**属性**则不太可能发生变化。与角色一样，这些属性值不会添加行为。它们仅向用户呈现信息。你必须使用 JavaScript 自行实现该行为。

状态和属性分为四个不同的类别：

*   **控件属性（Widget attributes）**：这些值特定于接收用户输入并处理操作的用户界面元素。它们支持用户输入控件和用户界面角色类别。

### 实时区域属性

*实时区域*（live region）是页面中通过 JavaScript 动态更新的元素，例如通过 Ajax 按固定时间间隔从服务器加载的新闻文章订阅源。实时区域属性的值向浏览器指示：当用户未聚焦于该元素时，该页面元素内部可能发生内容更新。

### 拖放属性

这些值指示了可拖拽元素及其相关放置目标的特定信息。额外的信息使浏览器或屏幕阅读器能够开发合适的方法，来呈现可拖拽和可放置元素的相关信息。

### 关系属性

这些值描述了无法通过正常页面结构确定的、不同页面元素之间的关系。

所有状态和属性名称均以`aria-`前缀开头。表 12-4 展示了部分状态与属性及其含义。完整的状态、属性及其允许值列表可在 ARIA 文档中获取。

**表 12-4.** *ARIA 状态与属性精选*

| **状态或属性** | **类别** | **含义** |
| :--- | :--- | :--- |
| `aria-activedescendant` | 关系 | 当用户与组件交互时，指示组件内当前激活的项目 |
| `aria-atomic` | 实时区域 | 指示当发生更新时，浏览器是否应呈现元素的全部内容，或仅呈现所发生的更改 |
| `aria-autocomplete` | 组件 | 指示与此属性关联的字段在用户输入时是否包含自动补全建议 |
| `aria-busy` | 实时区域 | 指示指定的实时区域当前是否正在更新 |
| `aria-checked` | 组件 | 指示复选框、单选按钮或其他模拟类似开/关状态的元素的当前状态 |
| `aria-controls` | 关系 | 标识其内容受当前元素控制的元素，例如更改当前显示页签的选项卡控件 |
| `aria-dropeffect` | 拖放 | 指示将拖拽元素放置到目标元素上时执行的功能，例如复制或移动操作 |
| `aria-grabbed` | 拖放 | 指示一个元素当前是否处于“被抓住”状态，准备拖拽到放置目标元素 |
| `aria-labelledby` | 关系 | 标识为当前元素提供标签的元素，例如为表单字段提供标签的元素 |
| `aria-live` | 实时区域 | 指示元素将被动态更新 |

`aria-live`属性的值决定了浏览器应如何处理更新：

*   值为`off`时，不会向用户发出更新通知。
*   值为`polite`（应为常规行为）时，将在用户完成当前操作后，在下一个可用的机会通知用户更新。
*   值为`assertive`时，将立即中断用户，通知他们内容已更新。

#### 焦点管理

您已经了解到元素焦点对于富互联网应用（RIA）的可访问性是多么重要。屏幕阅读器用户、键盘用户等需要聚焦到元素上才能与之交互。

在本章前面的内容中，您看到了如何通过分配`tabindex`属性值，使得元素可以使用 DOM 元素的`focus()`方法被选中。

WAI-ARIA 建议定义了如何扩展`tabindex`属性的行为以使其更易用，从而允许对页面元素进行不同类型的导航，如表 12-5 所示。

**表 12-5.** *基于`tabindex`值的键盘焦点行为*

| **`tabindex`属性** | **行为** |
| :--- | :--- |
| 无`tabindex`属性 | 只有超链接和表单元素可聚焦。键盘焦点遵循源代码顺序。 |
| `tabindex="0"` (零) | 允许任何类型的元素成为可聚焦元素。键盘焦点遵循源代码顺序。 |
| `tabindex="1"` (正数) | 允许任何类型的元素成为可聚焦元素。键盘焦点遵循源代码顺序。 |