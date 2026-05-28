# 第 3 章 弹跳球

**表 3-2.** (*续*)

**代码** | **说明**
---|---
`ballvy = -ballvy;` | 改变垂直位移方向。
`}` | 关闭该子句。
`ballx = nballx;` | 将 x 位置设置为 `nballx`。
`bally = nbally;` | 将 y 位置设置为 `nbally`。
`}` | 关闭 `moveAndCheck` 函数。

```javascript
function change() {
    // 开始 change 函数。
    ballvx = Number(document.f.hv.value);
    // 将输入转换为数字并赋值给 `ballvx`。
    ballvy = Number(document.f.vv.value);
    // 将输入转换为数字并赋值给 `ballvy`。
    return false;
    // 返回 false 以确保页面不会重新加载。
}
// 关闭该函数。
</script>
// 关闭 script 标签。
</head>
// 关闭 head 标签。
<body onLoad="init();">
// 开始 body 元素。设置对 `init` 函数的调用。
<canvas id="canvas" width="400" height="300">
// 开始 canvas 元素。
Your browser doesn't support the HTML5 element canvas.
</canvas>
// 关闭 canvas 元素。
<br />
// 换行。
<form name="f" id="f" onSubmit="return change();">
// 开始表单。指定名称和 ID（某些浏览器可能需要）。设置提交按钮的动作。
```

(*续*)