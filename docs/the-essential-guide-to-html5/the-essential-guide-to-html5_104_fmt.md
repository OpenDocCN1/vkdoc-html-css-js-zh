# 附录 更多绘图技巧

| 代码语句 | 解释 |
| :--- | :--- |
| `var myRightEye;` | 对右眼的引用。 |
| `var myMouth;` | 对嘴巴的引用。 |
| `var myHair;` | 对头发的引用。 |
| `var faceadded = false;` | 布尔值，表示脸部尚未添加。 |
| `var textadded = false;` | 布尔值，表示文本尚未添加。 |
| `var nosedatatail="l 15 22 a 20 20 0 0 1 细节描述鼻子：一条线然后一个弧。-15 4";` | |
| `var hairdatatail = "c -50 30, 0 25, 15 头发细节：一条三次贝塞尔曲线。30 s 2 12 -20 30";` | |
| `var upperradius = 50;` | 上方圆的半径。 |
| `var lowerradius = 50;` | 下方圆的半径。 |
| `var middleradius = 40;` | 中间圆的半径（用于擦除边界）。 |
| `var myPlace;` | 对整个绘图的引用。 |
| `var myText;` | 对文本的引用。 |
| `function init() {` | `init` 函数的头部。 |
| `myHead = document.getElementById("head");` | 设置 `myHead`。 |
| `if(myHead.namespaceURI != "http://www.w3.org/2000/svg")` | 检查命名空间。 |
| `alert("HTML5 中的内联 SVG 不受支持。此文档需要支持 HTML5 内联 SVG 的浏览器。");` | 如果用户的浏览器不支持 SVG，则向用户发出警告。 |
| `myPlace = document.getElementById("place");` | 设置 `myPlace`。 |
| `myUpper = document.getElementById("upper");` | 设置 `myUpper`。 |
| `myLower = document.getElementById("lower");` | 设置 `myLower`。 |
| `myMiddle = document.getElementById("middle");` | 设置 `myMiddle`。 |
| `}` | 关闭 `init`。 |
| `function createFace() {` | `createFace` 函数的头部。 |
| `if (faceAdded) {` | 检查是否已经完成，如果是…… |
| `alert("face already added");` | 向玩家显示消息。 |
| `return;}` | 返回。 |
| `myNose = document.createElementNS(svgNS, "path");` | 创建新元素并设置 `myNose`。 |
| `myMouth = document.createElementNS(svgNS,"path");` | 创建一个新元素并设置 `myMouth`。 |
| `myLeftEye = document.createElementNS(svgNS,"ellipse");` | 创建一个新元素并设置 `myLeftEye`。 |
| `myRightEye = document.createElementNS(svgNS,"ellipse");` | 创建一个新元素并设置 `myRightEye`。 |
| `myMouth = document.createElementNS(svgNS,"path");` | 创建一个新元素并设置 `myMouth`。 |
| `myHair = document.createElementNS(svgNS,"path");` | 创建一个新元素并设置 `myHair`。 |
| `myLeftEye.setAttributeNS(null,"id","myLeftEye");` | 此语句及后续语句用于设置属性。 |
| `myLeftEye.setAttributeNS(null,"fill","blue");` | |
| `myLeftEye.setAttributeNS(null, "stroke","black");` | |
| `myRightEye.setAttributeNS(null, "id","myRightEye");` | |
| `myRightEye.setAttributeNS(null, "fill","blue");` | |
| `myRightEye.setAttributeNS(null, "stroke","black");` | |
| `myMouth.setAttributeNS(null,"id", "myMouth");` | |
| `myMouth.setAttributeNS(null, "stroke","red");` | |
| `myMouth.setAttributeNS(null, "fill","transparent");` | |
| `myMouth.setAttributeNS(null, "stroke-width",3);` | |
| `myNose.setAttributeNS(null,"id", "myNose");` | |
| `myNose.setAttributeNS(null, "stroke","black");` | |
| `myNose.setAttributeNS(null, "fill","transparent");` | |
| `myHair.setAttributeNS(null, "stroke","black");` | |
| `myHair.setAttributeNS(null, "fill","transparent");` | |
| `setAttributesFace();` | 调用该函数以完成设置属性的任务。 |
| `myHead.appendChild(myNose);` | 通过将这些元素追加到 `myHead` 使其可见。 |
| `myHead.appendChild(myLeftEye);` | |
| `myHead.appendChild(myRightEye);` | |
| `myHead.appendChild(myMouth);` | |
| `myHead.appendChild(myHair);` | |
| `faceAdded = true;` | 将 `faceAdded` 设置为 true。 |
| `}` | 关闭 `createFace`。 |
| `function setAttributesFace() {` | `setAttributesFace` 函数的头部。创建这个独立的函数只是为了分割代码。 |
| `var nosedata = "M"+String(cheadx)+" "+String(cheady)+" "+nosedatatail;` | 定义 `nosedata`。这包括将数字转换为字符串。 |
| `var mouthx1 = cheadx - .25*rx;` | 起始点 x 坐标。 |
| `var mouthy = cheady + ry;` | 起始点 y 坐标。 |
| `var mouthx2 = cheadx+.25*rx;` | 终点 x 坐标。 |
| `var mouthyc = mouthy + .25*ry;` | 控制点 y 坐标。 |
| `var mouthx1c = mouthx1+.15*rx;` | 左侧控制点 x 坐标。 |
| `var mouthx2c = mouthx2-.15*rx;` | 右侧控制点 x 坐标。 |
| `var mouthdata = "M"+String(mouthx1)+" "+ String(mouthy)+" C"+ String(mouthx1c) +" "+ String(mouthyc)+", "+String(mouthx2c)+" "+String(mouthyc)+", "+String(mouthx2)+" "+String(mouthy);` | 现在可以设置 `mouthdata`。 |
| `myMouth.setAttributeNS(null,"d",mouthdata);` | 为 `myMouth` 设置 `d` 属性。 |

`myNose.setAttributeNS(null,"d",nosedata);` 设置`myNose`的`d`属性。

`var hairdata = "M" +String(cheadx)+"` 定义`hairdata`。

`"+String(hairy)+" "+hairdatatail;`

`myHair.setAttributeNS(null,"d",hairdata);` 设置`myHair`的`d`属性。

`var leftx = cheadx - .5*rx;` 为眼睛元素准备变量。设置`leftx`。

`var eyey = cheady ;`

`var rightx = cheadx + .5*rx;`

`var ex = .2*rx;`

`var ey = .5*ex;`

`myLeftEye.setAttributeNS(null,"cx",leftx);` 为`myLeftEye`设置属性。

`myLeftEye.setAttributeNS(null,"cy",eyey);`

`myLeftEye.setAttributeNS(null,"rx",ex);`

`myLeftEye.setAttributeNS(null,"ry",ey);`

`myRightEye.setAttributeNS(null,"cx",rightx);` 为`myRightEye`设置属性。

`myRightEye.setAttributeNS(null,"cy",eyey);`

`myRightEye.setAttributeNS(null,"rx",ex);`

`myRightEye.setAttributeNS(null,"ry",ey);` 关闭`setattributesface`。

(*续*)