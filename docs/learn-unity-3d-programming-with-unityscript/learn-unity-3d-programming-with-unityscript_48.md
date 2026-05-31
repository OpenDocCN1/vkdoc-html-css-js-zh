# 弹丸预制体

创建一个基本球体游戏对象并命名为 `Projectile`。将其 `Transform.position` 设为 `(0, 0, 0)`，`Transform.rotation` 设为 `(0, 0, 0)`，缩放设为 `(0.05, 0.05, 0.05)`。在 Inspector 中，通过选择 添加组件 ➤ 物理 ➤ Rigidbody 来添加 `Rigidbody` 组件。

接下来，通过选择 添加组件 ➤ 特效 ➤ 拖尾渲染器 添加 `Trail Renderer` 组件。在材质属性中，将尺寸保持为 1。使用元素 0 字段右侧的圆形选择按钮定位并选择 `part_afterburner` 材质。这是一个来自示例资源包的粒子材质。将时间设为 0.5，并将起始宽度和结束宽度均设为 0.1。在颜色属性中，为 `Color[0]` 指定浅黄色。为其余颜色元素依次选择渐深的黄色到橙色。

为了使它成为更致命的障碍物，玩家角色被弹丸击中时应被击杀。你可以使用与钟摆障碍物相同的 `KillOnCollision` 脚本，通过选择 添加组件 ➤ 脚本 ➤ KillOnCollision 来添加。

最后，将 `Projectile` 游戏对象从层级视图拖拽到项目面板中的 `Assets ➤ Prefabs` 文件夹中，使其成为预制体。现在你可以安全地从场景视图中删除 `Projectile` 游戏对象。

