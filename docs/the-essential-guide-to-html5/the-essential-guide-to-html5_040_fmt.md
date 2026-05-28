# 第 4 章 炮弹与弹弓

`cball.sx = cannonx + cannonLength * Math.cos(angleRadians);`

为即将旋转的炮口处的 `cball` 设置 x 坐标。

`cball.sy = cannony + cannonHt * .5 - cannonLength * Math.sin(angleRadians);`

为即将旋转的炮口处的 `cball` 设置 y 坐标。

*(续)*

## 表 4-4. *(续)*

| 代码 | 说明 |
|------|------|
| `drawAll();` | |
| `tid = setInterval(change, 100);` | |
| `return false;` | |
| `}` | |
| `function drawAll() {` | |
| `ctx.clearRect(0,0,cwidth,cheight);` | |
| `var i;` | |
| `for (i=0;i<everything.length;i++) {` | |
| `var ob = everything[i];` | 从数组中提取对象。 |
| `if (ob[1]) {` | 需要平移和旋转吗？ |
| `ctx.save();` | 保存原始坐标轴。 |
| `ctx.translate(ob[3],ob[4]);` | 执行指定的平移。 |
| `ctx.rotate(ob[2]);` | 执行指定的旋转。 |
| `ctx.translate(-ob[3],-ob[4]);` | 平移回原位。 |
| `ob[0].draw();` | 绘制对象。 |
| `ctx.restore(); }` | 恢复坐标轴。 |
| `else {` | 否则（无旋转）。 |
| `ob[0].draw();}` | 执行绘制。 |
| `}` | 关闭 `for` 循环。 |
| `}` | 关闭函数。 |
| `function change() {` | |
| `var dx = horVelocity;` | |
| `verticalVel2 = verticalVel1 + gravity;` | |
| `var dy = (verticalVel1 + verticalVel2) * .5;` | |
| `verticalVel1 = verticalVel2;` | |
| *(续)* | |

## 表 4-4. *(续)*

| 代码 | 说明 |
|------|------|
| `cball.moveit(dx,dy);` | |
| `var bx = cball.sx;` | |
| `var by = cball.sy;` | |
| `if ((bx>=target.sx)&&(bx<=(target.sx+target.swidth))&& (by>=target.sy)&&(by<=(target.sy+target.sheight))) {` | |
| `clearInterval(tid);` | |
| `everything.splice(targetindex,1,[htarget, false]);` | 移除目标并插入 `htarget`。 |
| `everything.splice(ballindex,1);` | 移除炮弹。 |
| `drawAll();` | |
| `}` | |
| `if (by>=ground.sy) {` | |
| `clearInterval(tid);` | |
| `}` | |
| `drawAll();` | |
| `}` | |

## HTML 代码

```
</script>
</head>
<body onLoad="init();">
<canvas id="canvas" width="600" height="400"> 您的浏览器不支持 HTML5 的 canvas 元素。</canvas>
<br/>
```

*(续)*

## 表 4-4. *(续)*

| 代码 | 说明 |
|------|------|
| `<form name="f" id="f" onSubmit="return fire();">` | 设置速度、角度并发射炮弹。 |
| `<br/>` | |
| `炮弹出口速度 <input name="vo" id="vo" value="10" type="number" min="-100" max="100" />` | 标签，表示此为炮弹从炮口射出的速度。 |
| `<br>` | |
| `角度 <input name="ang" id="ang" value="0" type="number" min="0" max="80"/>` | 标签，表示此为火炮的角度。 |
| `<input type="submit" value="发射"/>` | |
| `</form>` | |
| `</body>` | |
| `</html>` | |

该应用程序为您提供了许多个性化定制的可能性。您可以更改火炮、炮弹、地面和目标。如果您不想使用图片，也可以使用绘图来表现目标和被击中的目标。您还可以在 `canvas` 上绘制其他内容。只需确保炮弹（或您设定的任何抛射体）位于最上层或您想要的位置即可。例如，您可以让地面遮盖住炮弹。您可以为任何 `Image` 对象（包括 `htarget`）使用动画 GIF。您也可以使用图片来表示火炮和炮弹。一种可能性是使用动画 GIF 文件来表现旋转的炮弹。请记住，代码中引用的所有图像文件都必须与上传的 HTML 文件位于同一文件夹中。如果它们位于网络上的其他位置，请确保引用正确。

HTML5 对音频和视频的支持因浏览器而异。您可以先了解第 [6](http://dx.doi.org/10.1007/978-1-4842-4155-4_6) 章中作为完成测验奖励的视频展示，以及第 [8](http://dx.doi.org/10.1007/978-1-4842-4155-4_8) 章中作为石头剪刀布游戏一部分的音频展示。如果您想处理这个问题，那么当炮弹击中目标时播放音效，并显示目标爆炸的视频片段，将会非常棒。