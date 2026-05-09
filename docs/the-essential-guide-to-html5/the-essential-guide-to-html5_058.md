# 排版后的内容

这个问题的答案是：我想确保音频和视频文件能够完整下载。由于人类播放确实需要一些时间，因此即使不做特殊处理，下载通常也能完成，但采取这一预防措施是明智的。

**提示** CSS 有自己的语言，有时会包含带连字符的术语。CSS 中用于表达元素在屏幕上分层方式的术语是 `z-index`；而 JavaScript 中的对应术语是 `zIndex`。

#### 检查玩家的回答

`checkOrder` 函数负责检查玩家是否按正确顺序点击了方块。起初我并不清楚这一点，但我确实意识到我的程序不需要对选定的名称集合进行排序。相反，我的代码会检查玩家在 `slots` 数组中表示的列表是否顺序错乱。`slots` 数组将存储玩家排序后每位总统的索引位置。代码会遍历所有项目，检查是否有任何项目大于其后续项目。这个 `for` 循环完成了这一任务：

```
var ok = true;
for (var i=0;i<nq-1;i++){
    if (slots[i]>slots[i+1]){
        ok = false;
        break;
    }
}
```

变量 `ok` 的初始值为 `true`。如果顺序出现任何不一致，`for` 循环中的代码会将 `ok` 的值改为 `false`。一旦发生这种情况，`break` 语句将导致控制权退出 `for` 循环。如果 `ok` 被设为 `false`，`for` 循环即告退出。下一步是提供音频/视频奖励，同时显示结果为 CORRECT（正确）或 WRONG（错误）。

```
if (ok){
    res.innerHTML= "CORRECT";
    song.style.visibility="visible";
    song.currentTime = 4; //防止无声的几秒钟
    song.play();
    v.style.visibility="visible";
    v.currentTime=0;
    v.style.display="block";
    v.play();
}
else {
    res.innerHTML = "WRONG";
}
```

基于对 JavaScript、HTML 和 CSS 的这些背景知识，我们现在可以描述这个测验应用程序的编码过程了。

#### 构建应用程序并使其成为你自己的

该测验的知识库由 `facts` 变量表示，它是一个由数组组成的数组。如果你想将测验改成另一个主题（由名称或其他文本对组成），只需更改 `facts` 即可。当然，你还需要更改 `body` 元素中作为 `h1` 元素显示的文本，让玩家了解问题的类别。我定义了一个名为 `nq` 的变量，表示每次测验的数量（屏幕上显示的对数），其值为 4。当然，如果你想向玩家展示不同数量的配对，也可以更改这个值。其他变量用于记录方块的初始位置以及保存状态信息，例如是第一次点击还是第二次点击。

我为这个应用程序创建了四个函数：`init`、`setupGame`、`pickElement` 和 `checkOrder`。我本可以将 `init` 和 `setupGame` 合并，将 `pickElement` 和 `checkOrder` 合并，但为了让“重玩”按钮更便捷，同时也出于通用原则，我将它们分开了。为不同的任务定义不同的函数，是一种良好的实践。

**表 6-3.** 测验应用程序中的函数

| 函数           | 由谁调用/调用来源                       | 调用哪些函数     |
|----------------|--------------------------------------------|---------------|
| `init`         | 由 `<body>` 标签中 `onLoad` 的动作为触发 | `setupGame`   |
| `setupGame`    | `init`                                     | `pickElement` |
| `pickElement`  | 由 `setupGame` 中的 `addEventListener` 调用触发 | `checkOrder`  |
| `checkOrder`   | `pickElement`                              |               |

`setupGame` 函数负责为方块创建 HTML。简而言之，该函数会计算一个使用 `Math.random` 的表达式，从 `facts` 数组中选取一行。如果该行已被使用，代码会重新尝试。当找到未使用的行时，会将其标记为已使用（第三个元素，索引值为 2），并创建相应的方块。



```markdown
`pickElement`函数在点击区块时被调用。它将字符串添加到`Your Order`显示的内容中，并添加到`slots`数组中，该数组将由`checkOrder`使用。`checkOrder`函数执行检查。它显示`WRONG`或`CORRECT`，如果顺序正确，则使音频控件和视频可见，并开始播放。

请注意，程序中存在冗余代码。这样做的目的是为了在不重新加载或重新开始的情况下轻松启用重复播放。

表 6-4 提供了代码的逐行解释。

## 第 6 章测验

***表 6-4.** 总统测验完整代码*

**<html>**

**HTML 标签。**

`<meta charset="UTF-8">`

定义字符集，在本例中是一种 Unicode 形式。它可以省略，我在许多示例中确实省略了它，但我在此处包含它以让你了解它。

`<head>`

head 标签。

`<title>Ordering Quiz with Rewards</title>` 完整的标题元素。

`<style>`

样式标签。

`.thing {position:absolute; left: 0px;`

我称之为*区块*的格式化，带有总统的名字。

`top: 0px; border: 2px; border-style:`

`double; background-color: white; margin:`

`5px; padding: 5px; }`

`audio {visibility: hidden;}`

一开始将音频控件设置为隐藏。默认定位。

`video {visibility: hidden; display: none;` 一开始将视频设置为隐藏。

`position:absolute;}`

`</style>`

关闭样式元素。

`<script type="text/javascript">`

脚本标签，开始脚本元素，指定为 JavaScript。

`var facts = [`

声明`facts`数组。

`["George Washington",false],`

名称以及该名称未被使用的指示。

`["John Adams",false],`

`["Thomas Jefferson",false],`

`["James Madison",false],`

`["James Monroe",false],`

`["John Quincy Adams",false],`

( *续*)

## 第 6 章测验

***表 6-4.*** ( *续*)

**<html>**

**HTML 标签。**

`["Andrew Jackson",false],`

`["Martin Van Buren",false],`

`["William Harrison",false],`

`["John Tyler",false],`

`["James Polk", false],`

`["Zachary Taylor",false],`

`["Millard Fillmore",false],`

`["Franklin Pierce",false],`

`["James Buchanan",false],`

`["Abraham Lincoln",false],`

`["Andrew Johnson",false],`

`["Ulysses Grant",false],`

`["Rutherford Hayes",false],`

`["James Garfield",false],`

`["Chester Arthur",false],`

`["Grover Cleveland (1)",false],`

这是我选择表示格罗弗·克利夫兰第一个任期的方式。

`["Benjamin Harrison",false],`

`["Grover Cleveland (2)",false],`

这是我选择表示格罗弗·克利夫兰第二个任期的方式，该任期与其第一个任期不连续。

`["William McKinley",false],`

`["Theodore Roosevelt",false],`

`["William Taft",false],`

`["Woodrow Wilson",false],`

`["Warren Harding",false],`

( *续*)

## 第 6 章测验

***表 6-4.*** ( *续*)

**<html>**

**HTML 标签。**

`["Calvin Coolidge",false],`

`["Herbert Hoover",false],`

`["Franklin Roosevelt",false],`

`["Harry Truman",false],`

`["Dwight Eisenhower",false],`

`["John Kennedy",false],`

`["Lyndon Johnson",false],`

`["Richard Nixon",false],`

`["Gerald Ford",false],`

`["Jimmy Carter",false],`

`["Ronald Reagan",false],`

`["George H. W. Bush",false],`

`["Bill Clinton",false],`

`["George W. Bush",false],`

`["Barack Obama",false],`

`["Donald Trump",false],`

`["Joseph Biden",false]`

我为这个版本添加的内容。

`];`

关闭`facts`数组。

`var thingelem;`

用于保存已创建的元素。

`var nq = 4;`

呈现的 fact 数量。

`var col1 = 20;`

名称列的水平位置。

`var row1 = 200;`

第一个名称的垂直位置。

`var rowsize = 50;`

为每个区块分配的间距。

`var slots = [];`

用于在检查中保存指向`facts`的索引。

`var answertext=" ";`

答案的初始值。

( *续*)

## 第 6 章测验

***表 6-4.*** ( *续*)

**<html>**

**HTML 标签。**

`var song;`

将保存对音频元素的引用。

`var functionReference;`

将保存对`pickelement`的引用。

`var v;`

将保存对视频元素的引用。

`var res;`

将保存对结果位置的引用。

`var ans;`

将保存对答案位置的引用。

`function init(){`

header init 函数。

`res = document.getElementById("results");` 获取引用。

`ans = document.getElementById("answer");` 获取引用。

`functionReference = pickElement;`

设置为用于移除事件处理。

`song = document.`
```



