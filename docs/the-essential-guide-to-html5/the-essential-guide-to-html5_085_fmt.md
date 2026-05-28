# 第 9 章 猜单词

***表 9-2.*** (*续*)

| 代码 | 说明 |
| :--- | :--- |
| `if (not) {` | 检查是否没有识别出任何字母。 |
| `cur++;` | 递增计数器。 |
| `showProgress(cur);` | 反馈文本和绘图。 |
| `if (cur >= guessLimit) {` | 检查是否所有步骤都已完成。 |
| ``` for (i = 0; i < secret.length; i++) { ``` | 开始对秘密单词中的字母进行新一轮迭代，以揭示所有字母。 |
| ``` id = "s" + String(i); ``` | 构造标识符。 |
| ``` document.getElementById(id).textContent = secret[i]; ``` | 获取元素的引用并将其设置为秘密单词中的该字母。 |
| ``` } ``` | 结束迭代。 |
| `ctx.fillStyle = "red";` | 设置颜色。 |
| `out = "You lost! Reload the page to try again.";` | 设置消息。 |
| `ctx.clearRect(clearX, clearY, clearW, clearH);` | 清除画布。 |
| `ctx.fillText(out, msgx, msgy);` | 显示`out`消息。 |
| ``` for (j = 0; j < alphabet.length; j++) { ``` | 遍历字母表中的所有字母。 |
| ``` uniqueid = "a" + String(j); ``` | 构造唯一标识符。 |
| ``` thingelem = document.getElementById(uniqueid); ``` | 获取元素。 |
| ``` thingelem.removeEventListener('click', pickElement, false); ``` | 移除此元素的事件处理。 |
| ``` } ``` | 结束`j`循环 |
| ``` } ``` | 结束`if (cur > guessLimit)` |
| ``` } ``` | 结束`if (not)`测试（玩家猜错）。 |

(*续*)