# 第 3 章 弹跳球

## 表 3-5 棉花糖游戏代码

```html
<style>
...
img {visibility: hidden;}
```

将任何 `img` 元素设置为不可见。这两个 `img` 元素将不会被显示。不过，加载的图像文件将通过 `drawImage` 绘制在画布上。

```html
</style>
```

```javascript
<script type="text/javascript">
...
var bkg = new Image();
```

`bkg` 是一个全局变量，用于保存一个 `Image` 对象。

```javascript
var stoppedx = ballvx;
```

该变量将被 `stopcc` 修改。

```javascript
var stoppedy = ballvy;
```

该变量将被 `stopcc` 修改。

```javascript
function init(){
...
bkg.src = "reunion.jpg";
```

设置这两个 `Image` 对象的 `src` 属性值。

```javascript
ball.src = "candy.png";
...
}
...
```

```javascript
function stopcc() {
```

`stopcc` 函数的头部。

```javascript
clearInterval(tid);
```

停止定时器事件。

```javascript
stoppedx = ballvx;
```

保存当前的 `ballvx` 值。

```javascript
stoppedy = ballvy;
```

保存当前的 `ballvy` 值。

```javascript
moveBall();
```

调用 `moveBall` 来显示当前场景。这一步有时是多余的。

```javascript
return false;
```

返回 `false` 以防止页面重新加载。

```javascript
}
```

关闭 `stopcc` 函数。

## 表 3-5（续）

```javascript
function resume(){
```

`resume` 函数的头部。

```javascript
clearInterval(tid);
```

停止定时器事件。

```javascript
ballvx = stoppedx;
```

将 `ballvx` 设置为 `stoppedx` 的值。在大多数情况下，这将是 `stopcc` 中设置的值。

```javascript
ballvy = stoppedy;
```

将 `ballvy` 设置为 `stoppedy` 的值。在大多数情况下，这将是 `stopcc` 中设置的值。

```javascript
tid = setInterval
(moveBall,100);
```

启动定时器事件。

```javascript
return false;
```

返回 `false` 以防止页面重新加载。

```javascript
}
```

关闭 `resume` 函数。

```html
</script>
</head>
```

```html
<body onload="init();">
```

`body` 标签。注意：当所有元素（包括 `<img>` 标签中提到的图像文件）加载完成时，会调用 `init` 函数。

```html
...
<form name="f" id="f" onSubmit="return change();">
...
<button onClick="return stopcc();">停止 </button>
```

用于调用 `stopcc` 的按钮。注意使用 `&nbsp;` 来定位下一个按钮。

```html
&nbsp; &nbsp;
<button onClick="return resume();">继续 </button>
```

用于调用 `resume` 的按钮。

```html
</form>
```