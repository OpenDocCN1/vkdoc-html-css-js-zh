# 第 9 章 猜单词

***表 9-2.*** (*续*)

**代码** | **说明**
-------|--------
`var id = this.id;` | 提取此元素的标识符。
```
thingelem = document.getElementById(id);
```
| 获取元素引用。
`thingelem.style.display = "none";` | 使这个特定的字母按钮消失。
`thingelem.removeEventListener('click', pickElement, false);` | 移除事件处理。
```
}
```
| 结束`pickElement`函数
```
function showProgress(cur) {
```
| `showProgress`的函数头。
`ctx.clearRect(clearX, clearY, clearW, clearH);` | 清除画布。
`var remain = guessLimit - cur;` | 计算剩余次数。
`drawRemain(remain);` | 转到另一个函数进行绘图。
`out = String(remain) + " wrong guesses remain.";` | 准备文本。
`ctx.fillText(out, msgx, msgy);` | 显示文本。
```
}
```
| 结束`showProgress`。
```
function drawRemain(remain) {
```
| `drawRemain`的函数头。
`var ypos = startRy - unitY;` | 确定起始垂直点（最低点）。
```
for (i = 0; i < remain; i++) {
```
| 循环绘制矩形。
`ctx.strokeRect(startRx, ypos, unitX, unitY);` | 绘制矩形。
`ypos = ypos - unitY;` | 递减`ypos`。
```
}
```
| 结束循环。
```
}
```
| 结束`drawRemain`函数。

(*续*)

