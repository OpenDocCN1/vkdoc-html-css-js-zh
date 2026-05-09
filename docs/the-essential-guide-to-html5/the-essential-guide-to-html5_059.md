# 第 6 章 测验

**表 6-4.**（*续*）

`<html>` **HTML 标签。**

获取引用。

`getElementById("ruffles");`

`v = document.getElementById("vid");`

获取引用。

`row1 = .5 * window.innerHeight;`

适应窗口高度。

`setupGame();`

调用`setupGame`。

}

关闭`init`。

`function setupGame() {`

头部`setupgame`。

`slots = [];`

初始化`slots`。冗余，但在此处完成以便为增强做准备。

`answertext = "";`

初始化`answertext`。冗余，但在此处完成以便为增强做准备。

`var i;`

索引变量。

`var c;`

将保存指向`facts`的索引。

`var mx = col1;`

初始水平设置。它不会改变。

`var my = row1;`

初始垂直设置。这将会改变。

`var d;`

保存新创建的元素。

（*续*）

***表 6-4.***（*续*）

**<html>**

**HTML 标签。**

`var uniqueid;`

将保存 ID。它将根据`facts`的随机索引生成。

`for (i = 0; i < facts.length; i++) {`

`for`循环，标记所有`facts`为未使用状态。

`facts[i][2] = false;`

将事实标记为未使用。

}

关闭`for`循环。

`for(i = 0; i < nq; i++) {`

`for`循环，选择并创建四个带有总统名字的盒子。

`do { c = Math.floor(Math.`

获取一个随机选择。

`random() * facts.length); }`

`while (facts[c][1] == true);`

如果它已被选中，则重复`do`子句。

`facts[c][1] = true;`

现在将此事实设置为已使用。

`uniqueid = "p" + String(c);`

通过将`"p"`附加到转换为字符串的索引上，创建一个唯一 ID。

`d = document.createElement('pres');`

创建一个元素。

`d.innerHTML =`

将其`innerHTML`设置为...

`"<div class='thing'`

...是一个`div`，`class="thing"`，并且 ID 为

`id='" + uniqueid + "'>placeholder</div>";`

生成的唯一 ID。

`document.body.appendChild(d);`

将其追加到`body`。此操作使其可见。

`thingelem = document.`

获取对它的引用。

`getElementById(uniqueid);`

`thingelem.textContent = String(i + 1) + ": " + facts[c][0];`

使其内容由数字后跟名称组成。

`thingelem.style.top = `

垂直定位。

`String(my) + "px";`

（*续*）

***表 6-4.***（*续*）

**<html>**

**HTML 标签。**

`thingelem.style.left = `

水平定位。

`String(mx) + "px";`

`thingelem.addEventListener('click',`

启用对点击的响应。

`pickElement);`

`my += rowsize;`

增加`my`以便垂直定位。

}

关闭`for`循环。

}

关闭`setupGame`。

`function pickElement(ev) {`

`pickElement`的函数头。当玩家点击一个方块时调用。注意：`ev`未被使用，但对事件处理程序是必要的。实际使用的是`this`关键字。

`var answert;`

将保存数字 1、2 等。

`var positiont;`

将保存原始数组中的位置，作为文本。

`var positionn;`

将保存位置，作为数字。

`positiont = this.id.substring(1);`

通过移除 ID 的第一个字母来创建位置。

`answert = this.textContent.`

通过提取`textContent`的第一个字符来创建将添加到答案中的内容。

`substring(0,1);`

注意：如果选项少于 10 个，此方法有效。

`answertext = answertext + answert + " ";`

将此答案添加到已有的答案中。

`ans.innerHTML = answertext;`

显示`answertext`。

`positionn = Number(positiont);`

生成数字。

`this.style.backgroundColor = "gold";`

将方块设为金色。

`this.removeEventListener('click',`

移除事件处理。

`functionReference);`

（*续*）

***表 6-4.***（*续*）

**<html>**

**HTML 标签。**

`slots.push(positionn);`

将`positionn`添加到`slots`数组中，以便在检查时使用。

`if (slots.length == nq) {`

是否已对方块进行了`nq`次点击？

`checkOrder();`

如果是，则调用`checkOrder`。

}

关闭`if`。

}

关闭`pickElement`函数。

`function checkOrder() {`

`checkOrder`的函数头。

`var ok = true;`

开始时将`ok`设置为`true`。

`for (var i = 0; i < nq - 1; i++) {`

遍历`slots`中的所有元素。

`if (slots[i] > slots[i + 1]) {`

如果第`i`个槽大于第`i+1`个槽。

`ok = false;`

将`ok`设置为`false`。答案未按顺序排列。

`break;`

离开`for`循环。

}

关闭`if`。

}

关闭`for`循环。

`if (ok) {`

`ok`变量持有`true`或`false`。如果为`true`...

`res.innerHTML = "CORRECT";`

...显示`"CORRECT"`。

`song.style.visibility = "visible";`

使`song`元素（即控件）可见。

`song.currentTime = 4;`

此音频剪辑有若干秒的静音，因此这可以防止出现若干秒没有声音的情况。

`song.play();`

播放歌曲。

（*续*）

***表 6-4.***（*续*）

**<html>**

**HTML 标签。**


好的，作为高级文档工程师和翻译员，我将遵循您提供的注意事项和示例，将给定的英文文本翻译成中文。


