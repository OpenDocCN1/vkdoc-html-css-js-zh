# 十四、附录 C：WebGL 规格和零碎事项

本附录包含我们提到过但未完全涵盖的规范的某些部分，在此列出以供参考。

## WebGL 上下文属性

当我们获得我们的 WebGL 上下文时，我们可以有选择地向它传递一个包含以下部分或全部属性的对象：

```
字典 WebGLContextAttributes {
    布尔 alpha = true;
    布尔 depth = true;
    布尔 stencil = false;
    boolean antialias = true;
    boolean premultipliedAlpha = true;
    boolean preserveDrawingBuffer = false;
};
```

我们展示了如何在一个第五章的投射实例中保存绘图缓冲区：

```
gl = canvas.getContext("webgl", { preserveDrawingBuffer: true }) ||
     canvas.getContext("experimental-webgl", { preserveDrawingBuffer: true });
```

通过保留缓冲区内容而不是自动交换缓冲区，我们可以看到物体运动的轨迹，还可以产生运动模糊等效果。如果性能是关键，并且我们不需要 `alpha` 或 `depth` 测试，我们可以禁用这些属性。如果我们需要模板缓冲区，我们可以启用它。我们在第十章中展示了如何禁用抗锯齿。`premultipliedAlpha` 值影响画布的 Alpha 组件如何影响图像的整体颜色。将该值设置为 `false` 会使画布元素的 WebGL 颜色计算与 2D 上下文相同。

## 纹理属性

在第三章中，我们忽略了一些纹理选项。

### 立方体贴图目标

对于立方体贴图纹理，`target` 属性可以是以下之一：

`TEXTURE_CUBE_MAP`，`TEXTURE_BINDING_CUBE_MAP`，
`TEXTURE_CUBE_MAP_POSITIVE_X`，`TEXTURE_CUBE_MAP_NEGATIVE_X`，
`TEXTURE_CUBE_MAP_POSITIVE_Y`，`TEXTURE_CUBE_MAP_NEGATIVE_Y`，
`TEXTURE_CUBE_MAP_POSITIVE_Z`，`TEXTURE_CUBE_MAP_NEGATIVE_Z`。

### 最大立方体贴图纹理大小

`TEXTURE_2D`

纹理的格式如下：

`ALPHA`：Alpha
`RGB`：R、G、B 颜色
`RGBA`：红、绿、蓝颜色和 Alpha
`LUMINANCE`：亮度
`LUMINANCE_ALPHA`：亮度，Alpha

这些类型可以是：

`UNSIGNED_BYTE`
`UNSIGNED_SHORT_4_4_4_4`
`UNSIGNED_SHORT_5_5_5_1`
`UNSIGNED_SHORT_5_6_5`

以下组合是合法的：

`UNSIGNED_BYTE` / `RGBA`，`RGB`，`LUMINANCE`，`LUMINANCE_ALPHA`
`UNSIGNED_SHORT_4_4_4_4` / `RGBA`
`UNSIGNED_SHORT_5_5_5_1` / `RGBA`
`UNSIGNED_SHORT_5_6_5` / `RGB`

## 帧缓冲区和渲染缓冲区目标和附件

在第十章中，我们介绍了帧缓冲区和渲染缓冲区。其他合法附件/格式组合如下：

`DEPTH_ATTACHMENT` / `DEPTH_COMPONENT16`
`STENCIL_ATTACHMENT` / `STENCIL_INDEX8`
`DEPTH_STENCIL_ATTACHMENT` / `DEPTH_STENCIL`

颜色附件：`COLOR_ATTACHMENT0`
附加格式：`RGBA`、`RGBA4`、`RGB5_A1`、`RGB565`、`STENCIL_INDEX`

以下并发附件组合是非法的：

`DEPTH_ATTACHMENT` / `DEPTH_STENCIL_ATTACHMENT`
`STENCIL_ATTACHMENT` / `DEPTH_STENCIL_ATTACHMENT`
`DEPTH_ATTACHMENT` / `STENCIL_ATTACHMENT`