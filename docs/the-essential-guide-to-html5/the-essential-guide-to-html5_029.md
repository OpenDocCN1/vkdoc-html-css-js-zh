# 第 3 章 弹跳球

#### 表 3-4（续）

##### 代码

##### 说明

`var ballx = 50;`

`var bally = 60;`

`var ctx;`

`var ballvx = 4;`

`var ballvy = 8;`

```
function init(){
    ctx = document.getElementById('canvas').getContext('2d');
    ctx.lineWidth = ballrad;
    moveBall();
    setInterval(moveBall,100);
}
```

```
function moveBall(){
    ctx.clearRect(boxx,boxy,boxwidth,boxheight);
    moveAndCheck();
    ctx.beginPath();
    ctx.fillStyle ="rgb(200,0,50)";
    ctx.arc(ballx, bally, ballrad,0,Math.PI*2,true) ;
    ctx.fill();
    ctx.strokeRect(boxx,boxy,boxWidth,boxHeight);
}
```

```
function moveAndCheck() {
    var nballx = ballx + ballvx;
    var nbally = bally +ballvy;
```

#### 表 3-4（续）

##### 代码

##### 说明

```
    if (nballx > boxBoundx) {
        ballvx =-ballvx;
        nballx = boxBoundx;
    }
    if (nballx < inboxBoundx) {
        nballx = inboxBoundx;
        ballvx = -ballvx;
    }
    if (nbally > boxBoundy) {
        nbally = boxBoundy;
        ballvy =-ballvy;
    }
    if (nbally < inboxBoundy) {
        nbally = inboxBoundy;
        ballvy = -ballvy;
    }
    ballx = nballx;
    bally = nbally;
}
```

```
function change() {
    ballvx = Number(document.f.hv.value);
    ballvy = Number(document.f.vv.value);
    return false;
}
```

`</script>`

`</head>`

#### 表 3-4（续）

##### 代码

##### 说明

```
<body onLoad="init();">
<canvas id="canvas" width="400" height="300">
您的浏览器不支持 HTML5 的 canvas 元素。
</canvas>
<br/>
<form name="f" id="f" onSubmit="return change();">
水平速度 <input name="hv" id="hv" value="4" type="number" min="-10" max="10" />
<br>
垂直速度 <input name="vv" id="vv" value="8" type="number" min="-10" max="10"/>
<input type="submit" value="更改"/>
</form>
</body>
</html>
```

第四个应用是弹跳棉花糖游戏。我做的第一件事超出了 HTML/JavaScript/CSS 编程的范围。我使用了在线工具 *pixlr* 来提取棉花糖原图的一部分，并用另一张图片填补了缺失的部分。

我不会在这里给出棉花糖游戏的完整代码，仅指出新增部分。请参见表 3-5。

