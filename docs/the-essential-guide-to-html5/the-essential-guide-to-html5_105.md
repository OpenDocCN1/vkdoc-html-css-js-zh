# 附录 更多绘图技巧

| **代码语句** | **说明** |
| :--- | :--- |
| `function moveOvalh() {` | 函数`moveOvalh`头部。 |
| `headx +=10;` | 为水平移动增加`headx`值。 |
| `if (headx>300) {headx = 0;}` | 检查是否超出范围。`300`是任意值。重置回起始位置。 |
| `var trans = "translate("+String(headx)+", "+String(heady)+") scale("+String(scaleFactor)+")";` | 设置变换字符串。 |
| `myHead.setAttributeNS(null,"transform", trans);` | 将变换属性设置为`trans`。 |
| `}` | 结束`moveOvalh`函数。 |
| `function moveOvalv() {` | 函数`moveOvalv`头部。 |
| `heady +=10;` | 增加`heady`。 |
| `if (heady>100) {heady = 0;}` | 检查是否过大。如果是，则重置为`0`。 |
| `var trans = "translate("+String(headx)+", "+String(heady)+") scale("+String(scaleFactor)+")";` | 设置`trans`变量。 |
| `myHead.setAttributeNS(null,"transform", trans);` | 使用`trans`设置`myHead`的变换属性。 |
| `}` | 结束`moveOvalv`函数。 |
| `function changeScale(val) {` | 函数`changeScale`头部。参数将用于设置缩放因子。 |
| `scaleFactor = val/100;` | 将`scaleFactor`设置为`val`除以`100`的分数。 |
| `var trans = "translate("+String(headx)+", "+String(heady)+") scale("+String(scaleFactor)+")";` | 定义`trans`。 |

(*续*)
