# 六、分形，高度图，粒子系统

这一章介绍了我们可以用数学达到的各种效果。我们将涉及的主题包括：

*   直接用 GPU 绘画
*   分形和 Mandelbrot 集简介
*   高度图和地形生成
*   用鼠标旋转摄像机
*   粒子系统

因为我长久以来一直迷恋于数学和它所产生的美丽意象的强烈交集，所以这一章对我来说写起来特别有趣。即使你不是特别喜欢数学，你也可以跳过大部分细节/技术解释，直接用代码进行实验。我确信这里展示的例子和技术会让你感兴趣，并且可以被修改用于任何 WebGL 程序。

## 用 GPU 直接绘画

在讨论分形图像和 Mandelbrot 集合之前，我们将展示如何用纯粹包含在着色器程序中的逻辑来绘制图像。我们的 WebGL 应用将使用的唯一几何图形是来自五个顶点的四个三角形，它们将形成一个平面。然后，我们将使用图形处理单元(GPU) 的片段着色器，以编程方式设置平面上每个单独的像素颜色。不会对视图进行任何操作。方形平面的设置如清单 6-1 所示。

### 清单 6-1。功能：在 xy 平面上创建一个由两个三角形组成的正方形

```javascript
function createSquare(size) {
    size = (typeof size !== 'undefined') ? size : 2.0;
    var vertexPositionData = [
        0.0, 0.0, 0.0,
        -size/2.0, -size/2.0, 0.0,
        size/2.0, -size/2.0, 0.0,
        size/2.0, size/2.0, 0.0,
        -size/2.0, size/2.0, 0.0
    ];
    var indexData = [0, 1, 2, 2, 3, 3, 4, 4, 1];
    triangleDeploymentBuffer = gl.createBuffer();
    gl.bindBuffer(gl.ARRAY_BUFFER, triangleDeploymentBuffer);
    gl.bufferData(gl.ARRAY_BUFFER, new Float32Array(vertexPositionData), gl.STATIC_DRAW);
    triangleDeploymentBuffer.itemSize = 3;
    triangleDeploymentBuffer.numItems = vertexPositionData.length / 3;
    vertexIndexBuffer = gl.createBuffer();
    gl.bindBuffer(gl.ELEMENT_ARRAY_BUFFER, vertexIndexBuffer);
    gl.bufferData(gl.ELEMENT_ARRAY_BUFFER, new Uint16Array(indexData), gl.STREAM_DRAW);
    vertexIndexBuffer.itemSize = 3;
    vertexIndexBuffer.numItems = indexData.length;
}
```

清单 6-1 中平面的默认尺寸是 2.0 x 2.0。

我们的顶点着色器获取 `x`，`y` 输入坐标，并将它们传递给片段着色器。`z` 值固定为 0.0。