# DOM 操作

该类元素的文档中所有元素的`array`。由于文档中通常只有一个 body 元素，因此它直接获取第一个实例。接下来，通过调用 `createElement()` 方法，并指定元素类型，创建了一个段落元素。该方法返回一个 `HTMLParagraphElement` 对象。段落元素的内容通过更新 `innerHTML` 属性来设置。最后，通过调用 `appendChild()` 方法将段落插入到 body 元素中。现在，要调用此函数，只需在 `SampleScript.js` 文件中调用它即可。

## 基本的 DOM 操作

这个简单的示例演示了操作 DOM 时使用的四种基本操作：

- 查找一个或多个元素
- 创建新元素
- 定位元素
- 修改元素

#### 查找元素

通常，操作 DOM 的第一步是找到需要访问的元素。在前面的示例中，我必须获取 body 元素，以便修改其内容。根据用于搜索的数据，这里有几种可用的方法。

最简单的方法是依据元素的 `id` 属性来检索它。由于 `id` 属性在文档中必须是唯一的，因此该方法将返回零个或一个元素。如果未找到匹配的元素，则返回 `null`。

```
var element = document.getElementById(id);
```

接下来的两种方法会返回一个元素数组，这些元素匹配元素类型或 class 属性。第一种方法已在最初的演示中使用过：

```
var elementArray = document.getElementsByTagName(name);
```

```
var elementArray = document.getElementsByClassName(names);
```

后一种方法允许你提供一个以空格分隔的 class 名称列表。如果提供了多个名称，则只会返回包含所有指定 class 名称的元素。如果未找到匹配的元素，这两种方法都将返回一个空数组。

在第 9 章中，我解释了 CSS 选择器的功能。你可以使用 `querySelector()` 和 `querySelectorAll()` 方法在 JavaScript 中利用此功能。使用这两种方法时，传入的选择器与 CSS 中使用选择器的方式完全相同。`querySelectorAll()` 方法返回所有匹配的元素，而 `querySelector()` 方法仅返回第一个匹配的元素。你可以使用复杂选择器，例如：

```
var elementArray = document.querySelectorAll("header+p, .book, p:first-child");
```

> **提示：** 在这些示例中，我在 `document` 对象上调用搜索方法。这将对整个文档执行搜索。所有元素也都支持这些方法，因此你可以从某个子节点调用它们。这将从调用该方法的元素开始执行搜索。它会忽略其父元素和兄弟元素。

#### 创建元素

正如我已经演示过的，你可以使用 `createElement()` 方法并传入元素类型（例如 `div`、`p` 或 `span`）来创建新元素。新元素在返回值中提供。返回的实际对象类型会根据指定的元素类型而有所不同。

除元素节点外，文档还具有*文本节点*。文本节点包含元素开始标签和结束标签之间的内容。例如，要创建一个段落元素并设置其内容：

```
var p = document.createElement("p");
var text = document.createTextNode("This is a test");
p.appendChild(text);
```

如果在执行 JavaScript 后检查文档，你将看到元素定义如下：

```html
<p>This is a test</p>
```

你可以通过设置 `textContent` 属性来实现相同的目的。例如：

```
p.textContent = "This is a test";
```

你也可以像在第一个示例中那样设置 `innerHTML` 属性。当使用 `innerHTML` 属性时，元素的内容可以包含子元素。在第一个示例中，部分文本包含在 `strong` 元素内。设置 `innerHTML` 属性时，其内容会被指定的文档片段替换。相比之下，`createTextNode()` 方法仅用文本更新元素的内容。如果你尝试在传递给它的字符串中嵌入元素，它们将被视为字面文本。

> **警告：** 设置 `innerHTML` 属性存在安全隐患。为了避免跨站脚本攻击，`script` 元素如果包含在 `innerHTML` 属性中将被忽略。设置 `innerHTML` 属性时仍应小心谨慎。不要使用 `innerHTML` 属性将用户输入插入到 DOM 中。请改用 `textContent` 属性。

#### 移动元素

创建元素或文本节点只是创建了节点；它并没有将其放置在任何位置。要使其包含在文档中，你需要将其添加进去，可以将其作为文档的根节点，也可以作为现有节点的子元素。一个文档只能有一个根节点，除非你完全用 JavaScript 构建整个文档，否则你很可能要将新元素添加到现有节点中。

为此，在最初的示例中使用了 `appendChild()` 方法。你首先需要找到想要添加新元素的元素。然后，`appendChild()` 方法将使新元素成为该元素的子节点。该方法在父元素上调用，子元素作为参数传入。`appendChild()` 方法也会返回子节点，即正在添加的节点。

```
var child = parentNode.appendChild(childNode);
```

如果父节点已有子元素，则新元素将位于现有子元素之后。如果这不是你想要的位置，你可以改用 `insertBefore()` 方法。它的工作方式与 `appendChild()` 类似，只是多了一个参数来指定新元素应放在哪个兄弟元素之前。

```
var child = parentNode.insertBefore(childNode, sibling);
```

你在父节点上调用 `insertBefore()` 方法，第一个参数是要添加的元素。如果为兄弟元素参数传入 `null`，则该元素将被添加为最后一个子元素；这等效于调用 `appendChild()` 方法。

> **提示：** 既然有 `insertBefore()` 方法，你可能会认为也存在 `insertAfter()` 方法。但并没有。不过，这可以通过使用 `nextSibling` 属性轻松实现：`parentNode.insertBefore(childNode, sibling.nextSibling)`。所有 HTML 元素都继承自 `Node` 对象，该对象提供了一个 `nextSibling` 属性。在某个元素之后插入等同于在该元素的下一个兄弟元素之前插入。如果该元素恰好是最后一个子元素，那么它的 `nextSibling` 属性将为 `null`，而 `insertBefore()` 方法会将新元素添加为最后一个子元素。

你还可以使用 `removeChild()` 方法移除元素。与 `appendChild()` 和 `insertBefore()` 方法一样，`removeChild()` 方法在父节点上调用，要移除的元素作为参数传入。该元素仍然存在，但它不再是文档的一部分。与其他方法一样，被移除的元素会通过方法调用返回。

```
var removedElement = parentNode.removeChild(childNode);
```

如果你想移动一个元素，你可以使用 `removeChild()` 方法将其从原始位置删除，然后使用 `appendChild()` 或 `insertBefore()` 将其移动到新位置。

> **注意：** 需要父元素来移除节点可能看起来有点奇怪，但它的工作原理就是这样。你可以通过元素的 `parentNode` 属性轻松获取父元素。因此，如果你有想要移除的元素，可以调用 `element.parentNode.removeChild(element)`。

最后，你可以使用 `replaceChild()` 方法用一个元素替换另一个现有元素。



```markdown
它的工作方式与其他方法类似；它在父元素上被调用。它接受两个参数：第一个是要添加的元素，第二个是要移除的元素。新元素被添加到被移除元素的相同位置。被移除的元素会通过方法调用返回。

```javascript
var removedElement = parentNode.replaceChild(newElement, existingElement);
```

#### 修改元素

大多数 HTML 元素由它们的内容（开始和结束标签之间的文本）以及在开始标签中指定的属性组成。一些元素，如图像，没有任何内容，完全由其属性定义。正如我已经解释过的，元素的内容可以通过以下方式设置：

*   创建文本节点并将其附加到元素
*   设置 `textContent` 属性
*   设置 `innerHTML` 属性

![](img/index-360_1.jpg)
![](img/index-360_2.jpg)

为了操作属性，每个元素都有一个 `attributes` 属性，其中包含在该元素上定义的所有属性。这是一个名称/值对的集合。你可以通过调用 `hasAttribute()` 方法并传入属性名称来检查是否已指定特定属性。

你还可以向此集合添加属性或修改现有属性。以下代码演示了如何访问 `attributes` 属性。它会将第一个 `link` 元素的属性写入控制台。输出如图 18-3 所示。

```javascript
var link = document.getElementsByTagName("link")[0];
var attr = link.attributes;

for (var i=0; i < attr.length; i++) {
    console.log(attr[i].name + "='" + attr[i].value + "'");
}
```

***图 18-3.** 显示元素属性*

但是，这些属性也可以作为单个属性使用。你可以直接获取或给这些属性赋值。`HTMLElement` 对象支持所有全局属性，例如 `id` 和 `class`。为了说明这一点，我将修改前面的示例，设置 `id`、`class` 和 `lang` 属性。这些属性将包含在 `attributes` 属性中，如图 18-4 所示。

```javascript
var link = document.getElementsByTagName("link")[0];
link.id = "myID";
link.className = "myClass";
link.lang = "en";

var attr = link.attributes;

for (var i=0; i < attr.length; i++) {
    console.log(attr[i].name + "='" + attr[i].value + "'");
}
```

***图 18-4.** 显示附加属性*

> **注意** 属性名称可能与属性名不同。在这种情况下，`class` 是 JavaScript 中的保留字，因此该属性被称为 `className`。

其他专门的对象，如 `HTMLImageElement`，从 `HTMLElement` 继承全局属性，并提供特定于该元素的附加属性。例如，`HTMLImageElement` 暴露了 `alt`、`src` 和 `srcset` 属性。

#### 相关元素

正如我所提到的，每个元素都是文档中的一个节点，可以有一个父节点（除了根节点之外的所有节点都有一个父节点）。一个节点也可以有零个或多个子节点。在从文档中获取元素后，有几个属性可以用来导航到相关元素。

以下是可用的属性：

*   `parentNode` - 节点的直接父节点；仅对根节点为 `null`
*   `children` - 一个 `HTMLCollection` 对象，包含节点的所有直接子节点
*   `firstChild` - `children` 集合中的第一个元素
*   `lastChild` - `children` 集合中的最后一个元素
*   `previousSibling` - 与当前节点具有相同直接父节点，并且在父节点的 `children` 集合中位于当前节点之前的元素。如果当前元素是父节点的第一个子节点，则为 `null`。
*   `nextSibling` - 与当前节点具有相同父节点，并且位于当前节点之后的元素。如果当前元素是其父节点的最后一个子节点，则为 `null`。

> **提示** 代表 HTML 元素的对象上有数百个属性、方法和事件。我已经解释了更常用的功能，但这并非详尽无遗的集合。
>
> Mozilla 有一个非常好的 API 参考，如果你需要更多细节，我推荐它。可以从这里开始：
> `https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement`。本文描述了 `HTMLElement` 对象的成员，并提供了指向其他对象的链接。

#### 使用 jQuery

> **注意** 在本书中，我专注于原生 HTML、CSS 和 JavaScript 的功能。但是，有许多库和框架扩展了这些功能。当然，它们各有优缺点。如果你正在进行任何严肃的 Web 开发，你会想要考虑其中一个或多个库，但我在本书中特意避免了它们。然而，在 DOM 操作方面，jQuery 已成为事实上的标准。

在[第 3 章](http://dx.doi.org/10.1007/978-1-4842-2463-2_3)中，我解释了如何定义一个命名空间来将变量保持在全局命名空间之外。如果你在名为 `MyNamespace` 的命名空间中定义了一个函数（例如 `log()`），你可以通过指定命名空间和函数名称来执行它。这避免了与其他可能也定义了 `log()` 函数的脚本发生任何潜在冲突。

```javascript
MyNamespace.log("Hello World");
```

在 jQuery 中，所有成员都定义在一个命名空间中，这个命名空间恰好被命名为“`$`”。每当你看到 `$.` 或 `$()` 时，就知道正在使用一个 jQuery 成员。

## 基础知识

使用 jQuery 非常简单，但你需要了解一些基础知识。

#### 选择元素

所有 jQuery 操作的基本模式是选择一些东西，然后对它执行操作。语法如下所示：

```javascript
$(selector).action();
```

选择器将返回零个或多个元素。然后对集合中的每个元素执行指定的操作。选择器可以是任何有效的 CSS 选择器，这些选择器在[第 9 章](http://dx.doi.org/10.1007/978-1-4842-2463-2_9)中进行了描述。

jQuery 还支持额外的功能。这是一篇列出所有选择器及其示例和文档的文章：`http://codylindley.com/jqueryselectors/`。

你可以将选择和操作拆分为两个单独的调用。例如：

```javascript
var $elements = $(selector);
$elements.action();
```

如果你要对元素集执行多个操作，这尤其有用，因为它避免了再次执行选择。但请记住，选择器的结果是静态的；如果文档发生更改，它们不会更新。要反映最新状态，你需要重新执行选择器。

你也可以将对象传递给选择器，而不是选择器字符串。最常见的例子是使用 `document` 对象。例如，这将分配匿名函数在文档准备好时执行：

```javascript
$(document).ready(function(){... add code here ...});
```

jQuery 提供了许多基于索引的选择器，这些选择器根据元素在其父对象中的相对顺序来选择元素。索引是从零开始的，因此 `:eq(0)` 等同于使用 `first()` 方法。你还可以使用 `:lt()`、`:gt()`、`:even()` 和 `:odd()`。如果你为 `:eq()`、`:lt()` 或 `:gt()` 指定负值，则索引从末尾开始计数，而不是从开头。因此，`:eq(-1)` 等同于 `last()`。

### jQuery 对象

务必记住，从 jQuery 选择器返回的对象是 jQuery 对象，而不是像 `HTMLElement` 这样的原生 DOM 对象。原生对象被 jQuery 对象包装，并支持我稍后将演示的方法。如果你需要原生对象，请在 jQuery 对象上调用 `get()` 方法。
```



