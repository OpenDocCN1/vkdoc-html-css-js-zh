# 弹丸发射器

创建一个基本圆柱体游戏对象并命名为 `Cannon`。将其 `Transform.position` 设为 `(4, 10.5, 95)`，`Transform.rotation` 设为 `(0, 0, 90)`，`Transform.scale` 设为 `(0.2, 0.2, 0.2)`。现在创建一个空游戏对象并命名为 `Launcher`。这个空游戏对象将是弹丸实例化的位置。在层级视图中将 `Launcher` 拖拽到 `Cannon` 上方，使其成为 `Cannon` 的子对象。此时其变换坐标将相对于 `Cannon` 游戏对象而非世界坐标。将其 `Transform.position` 设为 `(0, 1.25, 0)`，`Transform.rotation` 设为 `(270, 270, 0)`，其缩放应已重置为 `(5, 5, 5)`。

在项目面板的 `Assets ➤ Scripts` 文件夹中，创建一个名为 `LaunchProjectile` 的脚本。将其附加到 `Launcher` 游戏对象上，然后在 `MonoDevelop` 中打开。将代码编辑为以下内容：

```csharp
#pragma strict

public var projectile : GameObject;

public var force : float = 25;

public var frequency : float = 2.0;

private var nextLaunch : float = 0.0;

function Update () {

    if (Time.time > nextLaunch) {

    nextLaunch = Time.time + frequency;

            var projectileInstance : GameObject = Instantiate(projectile, transform.position, transform.rotation);

            projectileInstance.rigidbody.AddForce (projectileInstance.transform.forward * force, ForceMode.Impulse);

        Destroy(projectileInstance, 2);

    }

}
```

保存脚本。其分解如下：

`(1)     public var projectile : GameObject;`

        `public var force : float = 25;`

        `public var frequency : float = 2.0;`

        `private var nextLaunch : float = 0.0;`

变量声明：`projectile` 用于持有对 `GameObject` 的引用，而浮点变量 `force` 和 `frequency` 供你在 `Inspector` 中微调弹丸行为。浮点变量 `nextLaunch` 跟踪每次弹丸发射之间的时间。

`(2)     if (Time.time > nextLaunch)`

`Time.time` 是游戏开始以来的秒数。`if` 条件语句用于测试经过的时间是否大于计算出的下一次弹丸发射时间。如果大于，

`(3)     nextLaunch = Time.time + frequency;`

`nextLaunch` 重置为当前已流逝的游戏时间加上浮点变量 `frequency` 指定的每次发射间隔秒数。

`(4)     var projectileInstance : GameObject = Instantiate(projectile,`

        `transform.position, transform.rotation);`

在 `transform.position` 和 `transform.rotation` 处（即此脚本附加到的父级 `Launcher` 游戏对象的位置和旋转）实例化 `Projectile` 预制体游戏对象的克隆。

`(5)     projectileInstance.rigidbody.AddForce`

        `(projectileInstance.transform.forward * force,`

        `ForceMode.Impulse);`

使用 `AddForce()` 函数向弹丸预制体的刚体施加力，赋予其一个 `Vector3` 前向方向，大小由可调节的 `force` 因子决定，并使用 `ForceMode.Impulse` 瞬间施加所有力。

`(6)     Destroy(projectileInstance, 2);`

出于性能考虑，你不希望这些弹丸游戏对象在视野之外堆积，因此每个克隆将在两秒后被销毁。

在层级视图中选择 `Launcher` 游戏对象。将 `Projectile` 预制体从 `Assets ➤ Prefabs` 文件夹拖拽到 `LaunchProjectile` 脚本的弹丸属性字段中。曳光火障已完成。将 `Cannon` 游戏对象从层级视图拖拽到项目文件夹中的 `Assets ➤ Prefabs` 文件夹，使其成为预制体资源。保存脚本，保存场景，并保存项目。进行游戏测试，现在你拥有了一个致命的弹丸式曳光火障。做得好！

## 总结

现在你已经掌握了使用 `Shuriken` 创建粒子系统和特效的方法，并且开始想象你的障碍赛道将如何形成一个游戏。你已在视觉特效工具包中新增了 `LineRenderer` 和 `TrailRenderer`，并为障碍赛道场景创建了大量新的障碍物预制体。

在将这些障碍物随机分布到赛道之前，在下一章中，你将学习游戏设计理论等相关内容，这反过来将指导你如何以及为何要像障碍赛道那样设计场景布局。