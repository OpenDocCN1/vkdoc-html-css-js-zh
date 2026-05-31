# 为

`mouseup` 事件设置事件处理。

`everything.push(target);`

**注意：**我已经将这几行移到了 `init` 函数内部。

将目标对象添加到列表中。

`everything.push(ground);`

将地面放在小鸡脚的上方。

`everything.push(mysling);`

添加 `mysling`。

`everything.push(cball);`

添加 `cball`。

`drawAll();`

绘制所有内容。

}

关闭 `init`。

```javascript
function findBall(ev) {
```

`mousedown` 事件的函数头。

```javascript
var mx;
```

用于存储鼠标 x 坐标的变量。

```javascript
var my;
```

用于存储鼠标 y 坐标的变量。

```javascript
mx = ev.pageX;
```

设置 `mx`。

```javascript
my = ev.pageY;
```

设置 `my`。

```javascript
if (distsq(mx, my, cball.sx, cball.sy) < ballradsq) {
```

鼠标是否在球上？

```javascript
    inmotion = true;
```

设置 `inmotion`。

```javascript
    drawall();
```

绘制所有内容。

}

关闭 `if` 判断（关于球）。

}

关闭函数。

```javascript
function distsq(x1, y1, x2, y2) {
```

`distsq` 的函数头。

```javascript
    return (x1 - x2) * (x1 - x2) + (y1 - y2) * (y1 - y2);
```

返回距离的平方。

}

关闭函数。

```javascript
function moveit(ev) {
```

`mousemove` 事件的函数头。

```javascript
var mx;
```

用于鼠标 x 坐标。

```javascript
var my;
```

用于鼠标 y 坐标。

```javascript
if (inMotion) {
```

正在移动中？

```javascript
    mx = ev.pageX;
```

用于设置 `mx`。

```javascript
    my = ev.pageY;
```

使用 `offsetY` 来设置 `my`。

```javascript
    cball.sx = mx;
```

设置球的 x 坐标。

```javascript
    cball.sy = my;
```

……和 y 坐标。

```javascript
    mysling.bx = mx;
```

设置弹弓的 `bx`。

```javascript
    mysling.by = my;
```

……和 `by`。

```javascript
    drawall();
```

绘制所有内容。

}

关闭 `if` 判断（正在移动中）。

}

关闭函数。

```javascript
function finish(ev) {
```

`mouseup` 事件的函数。

```javascript
if (inMotion) {
```

正在移动中？

```javascript
    inMotion = false;
```

重置 `inmotion`。

```javascript
    var outOfCannon = distsq(mysling.bx, mysling.by, mysling.s1x, mysling.s1y) / 700;
```

使 `outOfCannon` 与 `bx`、`by` 到 `s1x`、`s1y` 的距离平方成正比。

```javascript
    var angleRadians = -Math.atan2(mysling.s1y - mysling.by, mysling.s1x - mysling.bx);
```

计算角度。

```javascript
    horVelocity = outOfCannon * Math.cos(angleRadians);
```

计算水平速度。

```javascript
    verticalvel1 = -outOfCannon * Math.sin(angleRadians);
```

计算垂直速度。

```javascript
    drawAll();
```

绘制所有内容。

```javascript
    tid = setInterval(change, 100);
```

启动动画。

}

关闭 `inmotion` 文本。

}

关闭 `finish`。

```javascript
function drawAll() {
```

`drawall` 的函数头。

```javascript
    ctx.clearRect(0, 0, cwidth, cheight);
```

清除画布。

```javascript
    var i;
```

用于遍历 `everything`。

```javascript
    for (i = 0; i < everything.length; i++) {
```

For 循环。

```javascript
        everything[i].draw();
```

绘制 `everything` 数组中的每个对象。

}

关闭循环。

}

关闭 `drawall`。

```javascript
function change() {
```

`change` 的函数头。

```javascript
    var dx = horVelocity;
```

设置为 `horvelocity`。此值不会改变。

```javascript
    verticalVel2 = verticalVel1 + gravity;
```

设置该时间间隔结束时的垂直速度。

```javascript
    var dy = (verticalVel1 + verticalVel2) * 0.5;
```

计算平均垂直速度。

```javascript
    verticalVel1 = verticalVel2;
```

现在为下一次迭代设置 `verticalVel1`。

```javascript
    cball.moveit(dx, dy);
```

按计算出的量移动球。

```javascript
    var bx = cball.sx;
```

为下一次计算访问 `bx`。

```javascript
    var by = cball.sy;
```

……和 `by`。

```javascript
    if ((bx >= target.sx + 40) && (bx <= (target.sx + target.swidth - 40)) && (by >= target.sy + 40) && (by <= (target.sy + target.sheight - 40))) {
```

检查是否在目标内部（40 像素范围）。

```javascript
        target.img = feathers;
```

更改 `target` 的 img 值。

}

关闭 `if` 子句（如果命中）。

```javascript
    if (by >= ground.sy) {
```

检查球是否超出（低于）地面。

```javascript
        clearInterval(tid);
```

如果是，则停止动画。

}

关闭 `if` 子句（地面检测）。

```javascript
    drawAll();
```

绘制所有内容。

}

关闭 `change`。

```javascript
</script>
```

脚本结束。

```html
</head>
```

头部结束。

```html
<body onLoad="init();">
```

Body 标签。设置对 `init` 的调用。

```html
<canvas id="canvas" width="1200" height="600">
```

Canvas 头部。

```html
Your browser doesn't support the HTML5 element canvas.
```

针对旧版浏览器的提示信息。

```html
</canvas>
```

Canvas 结束。

```html
<br/>
```

换行。

按住鼠标左键并拖动小球。释放鼠标按钮将发射弹弓。弹弓将保持在最后的位置。重新加载页面可再次尝试。

```html
</body>
```

Body 结束。

```html
</html>
```

HTML 结束。

#### 测试与上传应用程序



