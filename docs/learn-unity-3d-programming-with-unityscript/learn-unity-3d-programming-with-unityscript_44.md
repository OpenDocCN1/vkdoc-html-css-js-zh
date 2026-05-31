# 粒子效果

要将 `DustPuff` 和 `EnergyBurst` 这两个粒子系统组合成一个单一的粒子效果，请在编辑器顶部菜单中，选择 Game Object ➤ Create Empty。将其命名为 `GrandEntrance`，并设置其 `Transform.position` 为 (0, 4, 0)。在层级视图中，将 `DustPuff` 和 `EnergyBurst` 粒子系统拖放到 `GrandEntrance` 游戏对象上。

仔细检查它们的位置，作为子游戏对象，其位置现在是相对于父游戏对象的。`DustPuff` 的 `Transform.position` 应为 (0, –4, 0)，而 `EnergyBurst` 应为 (0, 0, 0)。保存场景并保存项目。



