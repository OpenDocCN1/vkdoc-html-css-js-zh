# 附录 更多绘图技巧

| **代码语句** | **说明** |
| :--- | :--- |
| `myHead.setAttributeNS(null,"transform", trans);` | 使用`trans`设置`myHead`的变换属性。 |
| `}` | 结束`changeScale`函数。 |
| `function addText() {` | 函数`addText`头部。 |
| `if (textAdded) {` | 检查是否已经添加。如果是…… |
| `alert("text already added ");` | 弹出消息。 |
| `return;}` | 返回。结束条件子句。 |
| `myText = document.createElementNS(svgNS, "text");` | 创建一个类型为`text`的新元素并设置为`myText`。 |
| `myText.setAttributeNS(null,"x",110);` | 设置属性。 |
| `myText.setAttributeNS(null,"y",12);` | |
| `myText.setAttributeNS(null,"font-family","serif");` | |
| `myText.setAttributeNS(null,"font-size",16);` | |
| `myText.setAttributeNS(null,"font-weight","bold");` | |
| `myText.appendChild(document.createTextNode("My Daddy Joe"));` | 添加文本节点。 |
| `myHead.appendChild(myText);` | 通过附加到`myHead`使其可见。 |
| `textAdded = true;` | 将`textAdded`设置为`true`。 |
| `}` | 结束`addText`函数。 |
| `</script>` | 结束`<script>`元素。 |
| `</head>` | 结束`<head>`元素。 |

(*续*)
