# 第 7 章 迷宫

***表 7-5.***（*续*）

**代码**

**说明**

`<body onLoad="init();">`

`<canvas id="canvas" width="900" height="700">`
您的浏览器不支持 HTML5 元素 canvas。
`</canvas>`
`<br/>`
选择难度，点击“生成迷宫”按钮获取迷宫：
`<form name="gf" onSubmit="return getWalls()">`
`<br/>`
`<input type="radio" value="hard" name="level" />困难 <br/>`
设置单选按钮，通用难度；值为 hard。
`<input type="radio" value="moderate" name="level" />中等 <br/>`
设置单选按钮，通用难度；值为 moderate。
`<input type="radio" value="easy" name="level" />简单<br/>`
设置单选按钮，通用难度；值为 easy。
`<input type="submit" value="生成迷宫"/><br/>`
`</form>`

`<p>`
使用方向键移动标记。
`</p>`
`</body>`
`</html>`

