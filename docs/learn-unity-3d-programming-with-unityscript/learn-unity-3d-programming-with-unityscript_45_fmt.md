# 强力着陆

虽然 DustPuff 粒子系统在视觉上强调了玩家角色着陆的力度，但附近的物体也可以受到一种力的扩散影响。在项目面板的 `Assets ➤ Scripts` 文件夹中，选择 `Create ➤ JavaScript` 并将其命名为 `ImpactForce`。将其附加到 `GrandEntrance` 游戏对象上，然后在 MonoDevelop 中打开它。将代码编辑为以下内容：

```
#pragma strict

function Start () {
    Invoke ("ImpactForce", 0.75);
}

function ImpactForce () {
    var colliders : Collider[] = Physics.OverlapSphere(transform.position - Vector3(0, 4, 0), 10);
    for(var cldr : Collider in colliders)
    {
        if(cldr.rigidbody == null) continue;
        cldr.rigidbody.AddExplosionForce(10, transform.position - Vector3(0, 4, 0), 10, 0, ForceMode.Impulse);
    }
}
```

保存脚本。这段代码的分解说明如下：

`(1) Invoke ("ImpactForce", 0.75);`

`Invoke` 函数允许你在函数执行之前，通过一个时间延迟来调用函数。它接受两个参数：要调用的函数，以及以秒为单位的时间延迟。在此示例中，力的作用时间应与玩家角色落地以及 DustPuff 粒子系统开始发射的时间一致，根据其起始延迟可知，这个时间是 0.75 秒。

`(2) (3) var colliders : Collider[] = (4) (5) (6) Physics.OverlapSphere(transform.position - Vector3(0, 4, 0), 10);`

在 `ImpactForce()` 函数内部，声明一个用于存储 `Collider` 类型数组的引用变量 `colliders`。将 `Physics.OverlapSphere` 边界内找到的所有碰撞器赋值给它，该球体的位置是它所附加到的 `GrandEntrance` 游戏对象的位置减去其 y 坐标值 4，以便将其定位在地面 DustPuff 粒子系统发射的中心位置，半径为 10。

`(7) for(var cldr : Collider in colliders)`

这个 `for` 循环会遍历碰撞器数组。

`(8) if(cldr.rigidbody == null) continue;`

检查碰撞器上是否附加了刚体。记住，使用物理力时必须要有刚体。如果没有刚体，`continue` 意味着不执行接下来的代码块，而是继续处理 `for` 循环中的下一个项目。

`. (9) (10) (11) cldr.rigidbody.AddExplosionForce(10, transform.position - Vector3(0, (12)(13) (14) 4, 0), 10, 0, ForceMode.Impulse);`

如果附加了刚体，则应用 `AddExplosionForce`，力的大小为 10，力的来源位置与 DustPuff 粒子系统发射位置相同，爆炸半径为 10，不添加额外的向上力（当你想要一点额外的向上“弹起”效果时可以使用此参数），并使用 `ForceMode.Impulse` 模式，该模式会瞬间施加全部力，而不是在一段时间内施加。这个力会随着距指定位置的距离增加而减小，并在指定的半径处降为零。

如果你现在进行游戏测试，什么也不会发生，因为 `Physics.OverlapSphere` 的半径内没有游戏对象。为了看到效果，请在项目面板中依次展开 `Assets ➤ Sample Assets ➤ Prototyping ➤ Prefabs ➤ Compound Prefabs` 文件夹。将一个 `Smash Boxes` 预制体拖拽到场景视图中，然后将其 `Transform.position` 设置为 (–5, 0, –5)，`Transform.rotation` 设置为 (0, 90, 0)。

在层级面板中使用 Ģ+D 复制此游戏对象，然后将此副本的 `Transform.position` 设置为 (8, 0, –5)。创建第三个副本，将其 `Transform.position` 设置为 (0, 0, –11)，`Transform.rotation` 设置为 (0, 0, 0)。保存场景，保存项目，然后进行游戏测试。

轰！注意第一堆 Smash Boxes 被完全炸飞，而第二堆中最远的盒子组件几乎没被碰到，第三堆 Smash Boxes 则完全没有被触碰到。这是由于它们相对于 `ImpactForce` 脚本中指定的 10 爆炸半径的位置所决定的。

就游戏设计和风格而言，这对于我们的玩家来说是一个相当夸张的登场方式，但它作为构建粒子效果的一个很好的练习。