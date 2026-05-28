# 类附录

## 摘要

本附录包含 enchant.js 中最常用类的信息。这些类首先按其源自 enchant.js 的哪个部分（核心库或插件）进行列表，然后按字母顺序组织。每个类页面包含对该类整体功能的简要描述，列出从其继承属性和方法的类，提供该类最常用的属性和方法，并且对于某些类，还包含如何使用该类的示例。

这些类按层级结构组织，并从其父类继承方法和属性。继承属性和方法的类在每个类别表的“继承”部分中列出，并显示为一系列类的形式。最顶层的父类显示在左侧，而列出的类显示在右侧。例如，`Label` 类的继承树如下：

`EventTarget-Node-Entity-Label`

因此，除了能够使用 `Label` 类中指定的所有属性和方法外，作为标签创建的对象还将能够使用 `Entity`、`Node` 和 `EventTarget` 中的所有适用方法。`addEventListener` 方法（来自 `EventTarget`）可以在标签上运行，而 `age` 属性（来自 `Node`）也可以在 `Label` 上调用。

有关 enchant.js 中类的完整列表，请参阅参考部分下的官方 API：[`http://enchantjs.com`](http://enchantjs.com/) 。

## 附录内容

核心类 `Core` `EventTarget` `Game` `Group` `Label` `Map` `Node` `Scene` `Sound` `Sprite`

gl.enchant.js `AmbientLight` `Camera3D` `DirectionalLight` `Light3D` `Scene3D` `Sprite3D`

ui.enchant.js `APad` `Button` `LifeLabel` `MutableText` `Pad` `ScoreLabel`

## 核心类

核心类包含在主要的 enchant.js 文件中。除了基本库（enchant.js）之外，无需向一组文件添加任何内容即可访问核心类。

### Core

`Core` 类是主要的游戏容器。整个游戏都必须位于一个 `Core` 对象内部。一次只能存在一个，如果创建另一个，则原始的将被停用。

`Core` 类还负责管理游戏中的 `Scene` 对象。更改活动场景、移除场景和添加场景都是 `Core` 类对象的一部分。

#### 继承

`EventTarget`

➥ `Core`

#### 常用属性

*   `assets`：用于存储预加载的图像的对象。
*   `currentScene`：当前显示的场景。
*   `fps`：游戏的帧速率。
*   `frame`：自 `Core` 对象创建以来已过去的当前帧数。
*   `height`：游戏屏幕的高度。
*   `loadingScene`：游戏加载时显示的场景。
*   `rootScene`：游戏的默认场景。
*   `width`：游戏屏幕的宽度。

#### 常用方法

所有示例都假设已创建一个名为 `game` 的变量作为 `Core` 类的实例。

| 方法 | 说明 | 示例 |
| --- | --- | --- |
| `popScene();` | 结束当前场景并使底层场景变为活动状态。 | `game.popScene();` |
| `preload(assets);` | 预加载稍后将在游戏中使用的图像或声音文件。 | `game.preload("img/chara1.jpg");` |
| `pushScene(scene);` | 将场景推送到场景堆栈的顶部，使其成为活动场景。 | `game.pushScene(Scene3);` |
| `removeScene(scene);` | 从场景堆栈中移除场景。 | `game.removeScene(Scene2);` |
| `replaceScene(scene);` | 用指定场景替换当前场景。 | `game.replaceScene(Scene4);` |
| `start();` | 开始游戏。 | `game.start();` |

#### 示例

```
var game = new Core(320,320);
```

### Entity

`Entity` 类包含显示为 DOM 元素的对象，或作为网页内对象创建的元素。此类不直接使用（例如，你从不指定 `var bear = new Entity();`），但它包含 enchant.js 中大多数视觉元素共有的属性和方法集合。由于大多数视觉元素（`Sprite`、`Label` 等）都是 `Entity` 类的子类，因此视觉元素也可以访问这些属性和方法。

#### 继承

`EventTarget-Node-Entity`

#### 常用属性

*   `backgroundColor`：指定背景颜色。颜色应使用 CSS `color` 属性支持的任何有效格式指定。（例如，`#ffffff`、`red` 等）
*   `buttonMode`：将此 `Entity` 指定为按钮。当随后点击该 `Entity` 时，将调度相应的按钮事件。有效值包括 `left`、`right`、`up`、`down`、`a` 和 `b`。
*   `buttonPressed`：如果此 `Entity` 正在被点击，则设置为 `true`。只有在设置了 `buttonMode` 时才有效。
*   `height`：`Entity` 的高度（以像素为单位）。
*   `opacity`：`Entity` 的不透明度。测量范围从 0（完全透明）到 1（完全不透明）。
*   `originX`：用于旋转和缩放的原点的 X 坐标。
*   `originY`：用于旋转和缩放的原点的 Y 坐标。
*   `rotation`：`Entity` 的旋转角度（以度为单位）。
*   `visible`：指示是否显示 `Entity`。默认为 `true`。请注意，`Entity` 必须仍然作为 `Scene` 的子级添加才能在屏幕上显示。

#### 常用方法

所有示例都假设已创建一个名为 `game` 的变量作为 `Sprite` 类的实例，该类扩展了 `Entity` 类。这是因为 `Entity` 本身无法被创建。

| 方法 | 说明 | 示例 |
| --- | --- | --- |
| `intersect(Entity);` | 如果 `Entity` 正在接触或与指定 `Entity` 相交，则返回 `true`。 | `bear.intersect(ball);` |
| `rotate(degree);` | 将 `Entity` 旋转指定的度数。 | `bear.rotate(15);` |
| `scale(x,y);` | 按指定的因子在 x 和 y 方向上缩放 `Entity`。 | `bear.scale(2,2);`（将熊缩放 200%） |
| `within(Entity, distance);` | 如果 `Entity` 的中心点位于指定 `Entity` 中心点指定距离（以像素为单位）内，则返回 `true`。 | `bear.within(ball, 50);` |

### Event

事件并非由开发者直接创建，而是通常由 `Core`、`Scene` 或 `Node` 对象发出。通过使用 `EventListener`，可以监听这些事件，然后在事件发生时执行指定的代码。

#### 继承

事件在 enchant.js 中的最高层级定义，并且不继承自任何类。

#### 常见事件

请注意，下表中描述的“A”和“B”按钮是指由 `ui.enchant.js` 中的 `Button` 类创建并被指定为“A”和“B”按钮的虚拟按钮。更多信息，请参阅本附录末尾附近 `ui.enchant.js` 部分中 `Button` 类的条目。

| 事件类型 | 描述 | 发出的对象 |
| :--- | :--- | :--- |
| `Event.A_BUTTON_DOWN` | 按下“A”按钮时发生的事件 | `Core`, `Scene` |
| `Event.A_BUTTON_UP` | 释放“A”按钮时发生的事件 | `Core`, `Scene` |
| `Event.ADDED` | 当一个 `Node` 被添加到一个 `Group` 时发生的事件 | `Node` |
| `Event.ADDED_TO_SCENE` | 当一个 `Node` 被添加到一个 `Scene` 时发生的事件 | `Node` |
| `Event.B_BUTTON_DOWN` | 按下“B”按钮时发生的事件 | `Core`, `Scene` |
| `Event.B_BUTTON_UP` | 释放“B”按钮时发生的事件 | `Core`, `Scene` |
| `Event.DOWN_BUTTON_DOWN` | 按下方向下按钮时发生的事件 | `Core`, `Scene` |
| `Event.DOWN_BUTTON_UP` | 释放方向下按钮时发生的事件 | `Core`, `Scene` |

## 事件

| 事件 | 说明 | 适用对象 |
| :--- | :--- | :--- |
| `Event.ENTER` | 当一个`Scene`开始时发生的事件 | `Scene` |
| `Event.ENTER_FRAME` | 当正在处理新帧时发生的事件 | `Core`, `Scene` |
| `Event.EXIT` | 当`Scene`结束时发生的事件 | `Scene` |
| `Event.EXIT_FRAME` | 当帧处理即将结束时发生的事件 | `Core` |
| `Event.INPUT_CHANGE` | 当按钮输入发生改变时发生的事件 | `Core`, `Scene` |
| `Event.INPUT_END` | 当按钮输入结束时发生的事件 | `Core`, `Scene` |
| `Event.INPUT_START` | 当按钮输入开始时发生的事件 | `Core`, `Scene` |
| `Event.LEFT_BUTTON_DOWN` | 按下方向左按钮时发生的事件 | `Core`, `Scene` |
| `Event.LEFT_BUTTON_UP` | 释放方向左按钮时发生的事件 | `Core`, `Scene` |
| `Event.LOAD` | 游戏加载完成后分发的事件 | `Core` |
| `Event.PROGRESS` | 游戏加载过程中发生的事件 | `Core` |
| `Event.REMOVED` | 当一个`Node`从一个`Group`中移除时发生的事件 | `Node` |
| `Event.REMOVED_FROM_SCENE` | 当一个`Node`从一个`Scene`中移除时发生的事件 | `Node` |
| `Event.RENDER` | 当一个`Entity`被渲染时发生的事件 | `Entity` |
| `Event.RIGHT_BUTTON_DOWN` | 按下方向右按钮时发生的事件 | `Core`, `Scene` |
| `Event.RIGHT_BUTTON_UP` | 释放方向右按钮时发生的事件 | `Core`, `Scene` |
| `Event.TOUCH_END` | 当与`Node`相关的触摸结束时发生的事件 | `Node` |
| `Event.TOUCH_MOVE` | 当与`Node`相关的触摸移动时发生的事件 | `Node` |
| `Event.TOUCH_START` | 当与`Node`相关的触摸开始时发生的事件 | `Node` |
| `Event.UP_BUTTON_DOWN` | 按下方向上按钮时发生的事件 | `Core`, `Scene` |
| `Event.UP_BUTTON_UP` | 释放方向上按钮时发生的事件 | `Core`, `Scene` |

## Game

`Game`对象在 0.6 版本中已更名为`Core`对象。请参阅`Core`对象条目。0.6 以上版本的 enchant.js 向后兼容`Game`对象，但由于`Game`对象已过时，我们强烈建议使用`Core`对象。

## Group

`Group`类用于将多个`Node`对象链接在一起，目的是将它们作为一个整体移动或以其他方式作为一个单元进行操作。由于 enchant.js 中所有可见实体都属于扩展了`Node`类的`Entity`类，因此所有可见实体都可以添加到`Group`对象中。

### 继承链

`EventTarget`-`Node`-`Group`

### 常用属性

*   `childNodes`: 一个对象数组，包含作为`Group`成员的所有节点。
*   `firstChild`: `Group`中的第一个`Node`。
*   `lastChild`: `Group`中的最后一个`Node`。

### 常用方法

所有示例均假设已创建了一个变量`set`作为`Group`类的实例。因为`Group`扩展了`Node`类，所以任何对`Node`有效的方法都适用于`Group`对象。（例如，`moveTo`、`moveBy`等）

| 方法 | 说明 | 示例 |
| :--- | :--- | :--- |
| `addChild(node);` | 将一个`Node`添加到`Group`的末尾。 | `set.addChild(bear);` |
| `insertBefore(node, reference);` | 在`Node`对象数组中的引用位置之前将一个`Node`插入到`Group`中。 | `set.insertBefore(bear,3);` |
| `removeChild(node);` | 从`Group`中移除一个`Node`。 | `set.removeChild(bear);` |

### 使用示例

```javascript
var stage = new Group();
stage.addChild(player);
stage.addChild(enemy);
stage.addChild(map);
stage.addEventListener('enterframe', function() {
     // 根据玩家的 x 坐标移动整个帧。
     if (this.x > 64 - player.x) {
             this.x = 64 - player.x;
     }
});
```

## Label

标签用于在游戏内显示文本。

### 继承链

`EventTarget`-`Node`-`Entity`-`Label`

### 常用属性

*   `color`: `Label`中文本的颜色。颜色应以 CSS `color` 属性有效的任何格式指定（例如 `#ffffff`、`red` 等）。
*   `font`: `Label`中文本的字体。字体应以 CSS `font` 属性有效的任何格式指定（例如 `16px serif`、`12px sans-serif` 等）。
*   `text`: 由`Label`显示的文本。
*   `textAlign`: 文本的水平对齐方式。对齐方式应以 CSS `text-align` 属性有效的任何格式指定（例如 `left`、`right`、`center` 等）。

### 常用方法

除了从`Entity`、`Node`和`EventTarget`继承的方法之外，`Label`没有其他常用方法。

### 使用示例

```javascript
var title = new Label();
title.color = "blue";
title.font = "16px serif";
score.text = "Example Title";
game.rootScene.addChild(title);
```

## Map

地图用于根据图块集创建地图并显示它们。

### 继承链

`EventTarget`-`Node`-`Entity`-`Map`

### 常用属性

*   `collisionData`: 二维数组，指定是否应在`Map`中的特定图块上检测碰撞。
*   `image`: 用于`Map`中图块的图块集图像。
*   `tileHeight`: `Map`中使用的图块的高度。
*   `tileWidth`: `Map`中使用的图块的宽度。

### 常用方法

所有示例均假设已创建了一个变量`map1`作为`Map`类的实例。

| 方法 | 说明 | 示例 |
| :--- | :--- | :--- |
| `checkTile(x, y);` | 检查给定位置处存在哪个图块。 | `map1.checkTile(50, 50);` |
| `hitTest(x, y);` | 如果指定点在`collisionData`数组中被标记为包含障碍物，则返回`true`。 | `map1.hitTest(50, 50);` |
| `loadData(data);` | 根据图块集图像设置地图中的图块。 | `map1.loadData(array);` |

### 使用示例

```
var map = new Map(16, 16);
map.image = game.assets['http://enchantjs.com/assets/images/map0.gif'];
map.loadData([
    [0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0],
    [0,2,2,2,2,0,0,0,0,0,0,0,0,0,0,0,0,0,2,2,2,2,0],
    [0,2,2,2,2,0,0,2,2,2,2,2,2,2,2,0,0,0,2,2,2,2,0],
    [0,2,2,2,2,0,0,2,2,2,2,2,2,2,2,0,0,0,2,2,2,2,0],
    [0,0,2,2,0,0,0,2,2,0,0,0,0,2,2,0,0,0,0,2,2,0,0],
    [0,0,2,2,0,0,0,2,2,0,0,0,0,2,2,0,0,0,0,2,2,0,0],
    [0,0,2,2,0,0,0,2,2,0,0,0,0,0,0,0,0,2,2,2,2,0,0],
    [0,0,2,2,2,2,2,2,2,2,2,2,2,2,0,0,0,2,2,2,2,0,0],
    [0,0,2,2,2,2,2,2,2,2,2,2,2,2,0,0,0,2,2,0,0,0,0],
    [0,0,0,0,0,2,2,0,0,0,0,0,2,2,2,2,2,2,2,0,0,0,0],
    [0,0,0,0,0,2,2,0,0,0,0,0,2,2,2,2,2,2,2,0,0,0,0],
    [0,0,0,2,2,2,2,0,0,0,0,0,2,2,0,0,0,0,0,0,0,0,0],
    [0,0,2,2,2,2,2,0,0,0,0,0,2,2,2,2,2,2,2,2,2,0,0],
    [0,0,2,2,2,0,0,0,0,0,0,0,2,2,2,2,2,2,2,2,2,0,0],
    [0,0,2,2,2,0,0,0,0,0,0,0,0,0,0,0,0,0,0,2,2,0,0],
    [0,0,0,2,2,0,0,0,0,0,0,0,0,0,0,0,0,0,0,2,2,0,0],
    [0,0,0,2,2,0,0,0,0,0,0,0,0,0,0,0,0,0,2,2,2,2,0],
    [0,0,0,2,2,2,2,2,2,2,2,2,2,2,2,2,0,0,2,2,2,2,0],
    [0,0,0,2,2,2,2,2,2,2,2,2,2,2,2,2,0,0,2,2,2,2,0],
    [0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0]
]);
```

### Node

`Node`类是 enchant.js 中通过显示树进行显示的对象的基础类。开发者不会直接创建`Node`类的实例。

#### 继承关系

`EventTarget`-`Node`

#### 常用属性

- `age`：`Node`对象已存活的帧数。
- `scene`：`Node`所属的`Scene`。
- `x`：`Node`对象的 X 轴位置。
- `y`：`Node`对象的 Y 轴位置。

#### 常用方法

所有示例均假设已创建了一个变量`bear`，它是`Sprite`类的实例，`Sprite`类继承自`Entity`类，而`Entity`类继承自`Node`类。

| 方法 | 说明 | 示例 |
| --- | --- | --- |
| `moveBy(x, y);` | 将`Node`对象在 X 轴和 Y 轴上移动指定的像素量。 | `bear.moveBy(50, 50);` |
| `moveTo(x, y);` | 将`Node`对象移动到 X 轴和 Y 轴上指定的位置。 | `bear.moveTo(60, 60);` |

### Scene

显示对象树的根节点。所有`Entity`对象都必须添加到一个`Scene`对象中才能可见。

#### 继承关系

`EventTarget`-`Node`-`Group`-`Scene`

#### 常用属性

除了从`EventTarget`、`Node`和`Group`继承的属性外，`Scene`没有其他常用属性。

#### 常用方法

除了从`EventTarget`、`Node`和`Group`继承的方法外，`Scene`没有其他常用方法。

#### 使用示例

```
var scene = new Scene();
scene.addChild(player);
scene.addChild(enemy);
core.pushScene(scene);
```

### Sprite

精灵用于在游戏中显示图像，通常是角色或游戏元素的图像。

#### 继承关系

`EventTarget`-`Node`-`Entity`-`Sprite`

#### 常用属性

- `frame`：要显示的帧的索引编号。
- `height`：`Sprite`的高度（像素）。
- `image`：用于在屏幕上呈现`Sprite`的图片（精灵表），从中提取某一帧。
- `width`：`Sprite`的宽度（像素）。

#### 常用方法

除了从`EventTarget`、`Node`和`Entity`继承的方法外，`Sprite`没有其他常用方法。

#### 使用示例

```
var bear = new Sprite(16,16);
bear.image = game.assets("img/chara1.gif");
bear.frame = [0, 1, 0, 2];
// 按顺序显示帧 0、1、0 和 2，每帧递增一次
game.rootScene.addChild(bear);
```

## gl.enchant.js

此插件为 enchant.js 扩展了 WebGL 支持，从而可以创建 3-D 游戏。

### AmbientLight

`AmbientLight`类表示一种从其原点向所有方向发出光线的光源。

#### 继承关系

`EventTarget`-`Light3D`-`AmbientLight`

#### 常用属性

除了从`EventTarget`、`Light3D`和`Node`继承的属性外，`AmbientLight`没有其他常用属性。

#### 常用方法

除了从`EventTarget`和`Light3D`继承的方法外，`AmbientLight`没有其他常用方法。

#### 使用示例

```
var scene = new Scene3D();
var light = new AmbientLight();
light.color = [1.0, 1.0, 0.0];
scene.setAmbientLight(light);
```

### Camera3D

`Camera3D`类创建一个视角，玩家可以通过该视角查看`Scene3D`内部。

#### 继承关系

此类定义于 gl.enchant.js 的最高层级。它没有父类。

#### 常用属性

- `centerX`：摄像机指向的 X 轴点。
- `centerY`：摄像机指向的 Y 轴点。
- `centerZ`：摄像机指向的 Z 轴点。
- `upVectorX`：摄像机视为“上方”的 X 轴点。
- `upVectorY`：摄像机视为“上方”的 Y 轴点。
- `upVectorZ`：摄像机视为“上方”的 Z 轴点。
- `x`：摄像机在 X 轴上的位置。
- `y`：摄像机在 Y 轴上的位置。
- `z`：摄像机在 Z 轴上的位置。

#### 常用方法

所有示例均假设已创建了一个变量`camera`，它是`Camera3D`类的实例。

| 方法 | 说明 | 示例 |
| --- | --- | --- |
| `altitude(amount);` | 沿 Y 轴移动`Camera3D`。 | `camera.altitude(2);` |
| `forward(amount);` | 沿 Z 轴移动`Camera3D`。 | `camera.forward(2);` |
| `lookAt(sprite);` | 将`Camera3D`指向`Scene3D`中的`Sprite3D`。 | `camera.lookAt(droid);` |
| `rotatePitch(degrees);` | 沿 X 轴旋转`Camera3D`。 | `camera.rotatePitch(15);` |
| `rotateRoll(degrees);` | 沿 Z 轴旋转`Camera3D`。 | `camera.rotateRoll(15);` |
| `rotateYaw(degrees);` | 沿 Y 轴旋转`Camera3D`。 | `camera.rotateYaw(15);` |
| `sidestep(amount);` | 沿 X 轴移动摄像机。 | `camera.sidestep(2);` |

#### 使用示例

```
# 3D 图形库 API

## Camera3D

```javascript
camera = new Camera3D();
camera.y = 1.1;
camera.z = -1.65;
camera.centerZ = -10;
camera.upVectorZ = 10;
camera.upVectorY = 100;
```

## DirectionalLight

`DirectionalLight` 类表示一种向单一方向而非所有方向投射光线的光源。

### 继承关系

`EventTarget`-`Light3D`-`DirectionalLight`

### 常用属性

- `directionX`：`DirectionalLight` 向其投射光线的点的 X 轴位置。
- `directionY`：`DirectionalLight` 向其投射光线的点的 Y 轴位置。
- `directionZ`：`DirectionalLight` 向其投射光线的点的 Z 轴位置。

### 常用方法

除了从 `EventTarget` 和 `Light3D` 继承的方法外，`DirectionalLight` 没有其他常用方法。

### 使用示例

```javascript
var scene = new Scene3D();
var light = new DirectionalLight();
light.color = [1.0, 1.0, 0.0];
light.directionY = 10;
light.directionX = 4;
light.directionZ = 1;
scene.setDirectionalLight(light);
```

## Light3D

`Light3D` 是 3-D 光源的基类。开发者不会直接创建 `Light3D` 类的实例。

### 继承关系

`EventTarget`-`Light3D`

### 常用属性

- `color`：光源的颜色。

### 常用方法

除了从 `EventTarget` 继承的方法外，`Light3D` 没有其他常用方法。

## Scene3D

`Scene3D` 类是与 `Scene` 类等效的 3-D 版本。

### 继承关系

`EventTarget`-`Scene3D`

### 常用属性

- `backgroundColor`：`Scene3D` 的背景色。
- `childNodes`：`Scene3D` 的子元素数组。
- `lights`：`Scene3D` 中 `Light3D` 元素的数组。

### 常用方法

所有示例均假设已创建了一个名为 `scene`` 的变量，作为 `Scene3D` 类的实例。

| 方法 | 说明 | 示例 |
| --- | --- | --- |
| `addChild(sprite);` | 向 `Scene3D` 添加一个 `Sprite3D`。 | `scene.addChild(droid);` |
| `addLight(light);` | 向 `Scene3D` 添加一个光源。 | `scene.addLight(light3);` |
| `getAmbientLight();` | 获取 `Scene3D` 中的环境光源。 | `scene.getAmbientLight();` |
| `getCamera();` | 获取 `Scene3D` 中的摄像机。 | `scene.getCamera();` |
| `getDirectionalLight();` | 获取 `Scene3D` 中的方向光源。 | `scene.getDirectionalLight();` |
| `removeChild(sprite);` | 从 `Scene3D` 中删除一个 `Sprite3D`。 | `scene.removeChild(droid);` |
| `removeLight(light);` | 从 `Scene3D` 中删除一个 `Light3D`。 | `scene.removeLight(light2);` |
| `setAmbientLight(light);` | 在 `Scene3D` 中设置环境光源。 | `scene.setAmbientLight(light2);` |
| `setCamera(camera);` | 在 `Scene3D` 内设置活动的 `Camera3D`。 | `scene.setCamera(camera);` |
| `setDirectionalLight(light);` | 在 `Scene3D` 中设置方向光源。 | `scene.setDirectionalLigh(light6);` |

### 使用示例

```javascript
var scene = new Scene3D();
var sprite = new Sprite3D();
scene.addChild(sprite);
```

## Sprite3D

`Sprite3D` 类是 `Sprite` 类的三维等效类。

### 继承关系

`EventTarget`-`Sprite3D`

### 常用属性

- `childNodes`：用于存放子 `Sprite3D` 元素的数组。
- `scaleX`：X 轴上的缩放因子。
- `scaleY`：Y 轴上的缩放因子。
- `scaleZ`：Z 轴上的缩放因子。
- `x`：`Sprite3D` 在 X 轴上的位置。
- `y`：`Sprite3D` 在 Y 轴上的位置。
- `z`：`Sprite3D` 在 Z 轴上的位置。

### 常用方法

所有示例均假设已创建了一个名为 `droid` 的变量，作为 `Sprite3D` 类的实例。

| 方法 | 说明 | 示例 |
| --- | --- | --- |
| `addChild(sprite);` | 向 `Sprite3D` 对象添加一个子 `Sprite3D`。 | `droid.addChild(droid2);` |
| `altitude(amount);` `clone();` | 沿 Y 轴移动 `Sprite3D`。复制 `Sprite3D` 对象。 | `droid.altitude(2);` `droid.clone();` |
| `forward(amount);` | 沿 Z 轴移动 `Sprite3D`。 | `droid.forward(2);` |
| `intersect(object);` | 如果 `Sprite3D` 与指定的三维对象相交，则返回 `true`。 | `droid.intersect(droid2);` |
| `removeChild(sprite);` | 从 `Sprite3D` 中移除一个子 `Sprite3D` 对象。 | `droid.removeChild(droid2);` |
| `rotatePitch(amount);` | 沿 X 轴旋转 `Sprite3D`。 | `droid.rotatePitch(15);` |
| `rotateRoll(amount);` | 沿 Z 轴旋转 `Sprite3D`。 | `droid.rotateRoll(15);` |
| `rotateYaw(amount);` | 沿 Y 轴旋转 `Sprite3D`。 | `droid.rotateYaw(15);` |
| `scale(x,y,z);` | 沿 X、Y、Z 轴缩放 `Sprite3D`。 | `droid.scale(2,2,2);` |
| `set(colladaFile);` | 设置 Collada 文件作为 `Sprite3D` 使用的模型。 | `droid.set(game.assets["images/droid.dae"]);` |
| `sidestep(amount);` | 沿 X 轴移动 `Sprite3D`。 | `droid.sidestep(2);` |

## ui.enchant.js

此插件包含各种用于为 `enchant.js` 游戏创建用户界面的元素，并支持各种按钮、手柄和标签。对于大多数类，所需的图像也必须加载到给定的 `enchant.js` 游戏中。所需图像与其各自类同名。

### APad

此类用于模拟手柄，可控制任意二维方向上的移动。

#### 继承关系

`EventTarget`-`Node`-`Group`-`APad`

#### 常用属性

- `isTouched`：如果 `APad` 当前正被触摸，则返回 `true`。

#### 常用方法

除了从 `EventTarget`、`Node` 和 `Group` 继承的方法之外，`APad` 没有其他常用方法。

### Button

此类创建一个带有文本的按钮，玩家可以点击该按钮。

#### 继承关系

`EventTarget`-`Node`-`Entity`-`Button`

#### 常用属性

- `color`：文本的颜色。
- `font`：文本的字体。
- `size`：文本的字体大小。
- `text`：按钮上要显示的文本。

#### 常用方法

除了从 `EventTarget`、`Node` 和 `Entity` 继承的方法之外，`Button` 没有其他常用方法。

#### 使用示例
```

```
//在 (260, 250) 位置创建一个虚拟的 'a' 按钮，并将其添加到 rootScene
abtn = new Button(260, 250, 'a');
```

### LifeLabel

此类创建一个显示心形数量以表示角色生命值的标签。

#### 继承关系

此类定义在 `ui.enchant.js` 的顶层。它没有父类。

#### 常用属性

*   `life`：`LifeLabel` 显示的当前生命值。
*   `maxLife`：`LifeLabel` 显示的最大生命值。
*   `x`：`LifeLabel` 的 X 位置。
*   `y`：`LifeLabel` 的 Y 位置。

#### 常用方法

此类没有方法。

### MutableText

此类允许使用每个字符的图像来显示文本，从而使文本在不同操作系统和浏览器上显示完全相同。

#### 继承关系

此类定义在 `ui.enchant.js` 的顶层。它没有父类。

#### 常用属性

*   `posX`：`MutableText` 对象的 X 位置。
*   `posY`：`MutableText` 对象的 Y 位置。
*   `text`：`MutableText` 对象显示的文本。
*   `width`：`MutableText` 对象的宽度（像素）。

#### 常用方法

所有示例均假设已创建了一个名为 `message` 的变量，作为 `MutableText` 类的实例。

| 方法 | 说明 | 示例 |
| --- | --- | --- |
| `setText(text);` | 设置 `MutableText` 对象的文本。 | `message.setText("Hello!");` |

### Pad

此类创建一个四方向方向键，供玩家交互。按下手柄上的按钮时，会发出与其方向相对应的事件（例如 `UP`、`DOWN`、`LEFT`、`RIGHT`）。

#### 继承关系

`EventTarget-Node-Entity-Sprite-Pad`

#### 常用属性

除了从 `EventTarget`、`Node`、`Entity` 和 `Sprite` 继承的属性之外，`Pad` 没有其他常用属性。

#### 常用方法

除了从 `EventTarget`、`Node`、`Entity` 和 `Sprite` 继承的方法之外，`Pad` 没有其他常用方法。

### ScoreLabel

此类是 `MutableText` 类的特殊工程版本，旨在显示游戏的分数。

#### 继承关系

此类定义在 `ui.enchant.js` 的顶层。它没有父类。

#### 常用属性

*   `score`：指定 `ScoreLabel` 要显示的分数。
*   `x`：`ScoreLabel` 的 X 位置。
*   `y`：`ScoreLabel` 的 Y 位置。

#### 常用方法

此类没有方法。