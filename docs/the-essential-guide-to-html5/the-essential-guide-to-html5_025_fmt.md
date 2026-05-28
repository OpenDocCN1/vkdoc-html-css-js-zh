# 第 3 章 弹跳球

**表 3-2.** (*续*)

| **代码** | **说明** |
|---|---|
| `ctx.beginPath();` | 开始路径。 |
| `ctx.arc(ballx, bally, ballrad, 0, Math.PI*2, true);` | 设置在当前球的位置绘制圆形。 |
| `ctx.fill();` | 填充路径；即绘制一个实心圆。 |
| `ctx.strokeRect(boxx, boxy, boxWidth, boxHeight);` | 绘制矩形轮廓。 |
| `}` | 关闭 `moveBall`。 |

```javascript
function moveAndCheck() {
    // 开始 moveAndCheck。
    var nballx = ballx + ballvx;
    // 设置试探性的下一个 x 位置。
    var nbally = bally + ballvy;
    // 设置试探性的下一个 y 位置。
    if (nballx > boxBoundx) {
        // 这个 x 值是否超出了右边界？
        ballvx = -ballvx;
        // 如果是，则改变水平位移方向。
        nballx = boxBoundx;
        // 将下一个 x 值准确地设为该边界值。
    }
    // 关闭该子句。
    if (nballx < inboxBoundx) {
        // 这个 x 值是否小于左边界？
        nballx = inboxBoundx;
        // 如果是，则将 x 值准确地设为边界值。
        ballvx = -ballvx;
        // 改变水平位移方向。
    }
    // 关闭该子句。
    if (nbally > boxBoundy) {
        // y 值是否超出了下边界？
        nbally = boxBoundy;
        // 如果是，则将 y 值准确地设为边界值。
        ballvy = -ballvy;
        // 改变垂直位移方向。
    }
    // 关闭该子句。
    if (nbally < inboxBoundy) {
        // y 值是否小于上边界？
        nbally = inboxBoundy;
        // 如果是，则将 y 值准确地设为边界值。
    }
}
```

(*续*)