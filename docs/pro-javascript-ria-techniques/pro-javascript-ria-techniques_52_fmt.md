# 高效创建 DOM 元素

在富互联网应用中，动态创建 DOM 元素是相当常见的需求。然而，使用标准的`document.createElement()`方法每次创建元素时都会带来性能损耗。根据经验，我发现复制现有节点比从头创建新节点更快。如果你需要创建多个具有相似属性的元素，可以先创建一个实例，然后使用`cloneNode()`方法复制该元素及其关联属性，从而提升性能。

基于这一认识，你可以在`$` JavaScript 库中添加一个新方法，以便在某些情况下加速 DOM 元素的创建，如清单 4-8 所示。

## 清单 4-8. 高效创建 DOM 元素

```javascript
$.prototype.Elements.create = function(tagName) {
  // 该方法利用了记忆化技术
  this.memory = this.memory || {};
  if (tagName in this.memory) {
    // 如果已存储了该标签名的元素，则复制它
    return this.memory[tagName].cloneNode(true);
  } else {
    // 创建该标签名的新元素并存储它
    this.memory[tagName] = document.createElement(tagName);
    return this.memory[tagName].cloneNode(true);
  }
};

// 使用示例
// 假设页面上存在一个 $ 库的实例
// 从头创建两个元素
var newH2Tag = $.Elements.create("h2");
var newPTag = $.Elements.create("p");

// 创建另一个与已创建元素类型相同的元素。
// 复制已存储的元素，比再次从头创建元素性能更高
var anotherH2Tag = $.Elements.create("h2"); // 使用复制的元素
```