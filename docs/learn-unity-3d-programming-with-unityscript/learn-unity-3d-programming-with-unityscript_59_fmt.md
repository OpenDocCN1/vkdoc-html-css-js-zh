# 音频

音频对于游戏体验至关重要，并以多种方式发挥巨大作用。背景音乐设定情绪基调，并且可以在游戏过程中变化，以反映玩家体验的各种情境，例如，悬疑音乐预示着危险将至，而战斗场景中则充满激昂的音乐。

它还能给玩家提供线索，比如通过加快音乐节奏来传达渐强的紧迫感。角色和非玩家角色的对话会向玩家提供与游戏剧情一致的提示和指示。即使在简单的街机类游戏中，以升调结束的短促音效也能对玩家起到正向强化作用，而降调则会强化负面事件或行动。

有时，声音的缺失也能被有效利用。例如，在射击游戏中，当附近的闪光弹爆炸后紧接着一片寂静，然后出现真实的耳鸣声，随着玩家角色恢复，正常的游戏音效逐渐淡入，耳鸣声也随之消失，这有助于保持真实感。

音效与游戏环境和动作结合使用。在写实主题的游戏中，你会听到环境背景噪音，无论是夜晚的蟋蟀声、背景交通声、附近运转的机械声，或是路过的非玩家角色声。更具体的游戏动作音效包括所驾驶车辆的引擎轰鸣声、战斗场景中的撞击和碎裂声，或玩家角色移动时的脚步声。

有些游戏将声音作为核心玩法，例如像《西蒙》和《吉他英雄》这样的流行游戏。游戏 `Lurking` 利用声音创造了一种新颖而诡异的应用方式，其中来自玩家动作（白色）和敌人（红色）的声波照亮了游戏世界（图 11-7）。请花点时间查看 `www.lurking-game.com` 上的预告片，这是一个极佳的例子，展示了声音如何既用于设定游戏情绪，又用于驱动实际游戏玩法。

![A978-1-4302-6587-0_11_Fig7_HTML.jpg](img/A978-1-4302-6587-0_11_Fig7_HTML.jpg)

图 11-7. 来自 `www.lurking-game.com` 的《Lurking》预告片截图

## 音频监听器

就像主摄像机是游戏的眼睛一样，音频监听器组件则是游戏的耳朵。当您创建新场景时，音频监听器默认会添加到主摄像机上。在层级视图中，选择 `Free Look Camera Rig ➤ Pivot ➤ Main Camera`。在 Inspector 中，您将找到音频监听器组件。如果您尝试展开音频监听器，什么也不会发生，因为它没有任何属性（图 11-8）。

![A978-1-4302-6587-0_11_Fig8_HTML.jpg](img/A978-1-4302-6587-0_11_Fig8_HTML.jpg)

图 11-8. 音频监听器组件在 Inspector 中没有可显示的属性

它按原样工作，用于接收或“听到”游戏场景中任何音频源的输入，然后通过游戏平台的扬声器播放声音。根据游戏需要，音频监听器也可以改为附加到主要玩家角色或某些其他游戏对象上，但随后必须将其从主摄像机上移除，因为每个场景中只能存在一个音频监听器组件。

## 音频源

音频源是负责播放声音的游戏对象组件。在项目面板中，打开 `Assets ➤ Prefabs` 文件夹，展开 `Land Mine` 预制件以显示 `Explosion` 图标。选择 `Explosion` 以在 Inspector 中显示其音频源（图 11-9）。

![A978-1-4302-6587-0_11_Fig9_HTML.jpg](img/A978-1-4302-6587-0_11_Fig9_HTML.jpg)

图 11-9. Inspector 中的爆炸音频源组件

与音频监听器不同，您可以在游戏中拥有任意数量的音频源。声音性能，如同游戏性能一样，受到目标平台处理能力以及其特定声音处理和扬声器功能的限制。

音频源的功能类似于游戏中的扬声器，其中音频剪辑是音频源播放的实际声音文件。脚本可用于调整音频源组件中的任何属性，包括更改要播放的音频剪辑。

`静音` 是一个简单的启用/禁用切换，用于静音。各种 `Bypass` 效果仅限专业版。启用了 `唤醒时播放` 的音频源将在创建包含音频源组件的游戏对象时开始播放。音频可以像动画剪辑一样循环播放，即每次完整播放完毕后，音频剪辑会重新开始。

分配优先级允许 Unity 适应平台的音频能力。如果超出性能限制，优先级较低的声音将被优先级更高的声音替换。在 Unity 中，0 表示最重要。

声音剪辑的相对响度由音量属性控制，该属性使用 0 到 1 的比例表示音量的 0% 到 100%，而音调则控制声音波形的频率，其中较高的音调对应于音阶上较高的音符。

您可以在 2D 和 3D 声音之间进行选择，这两种声音又各自具有各种可调整的属性。2D 声音最适合背景音乐等音频，无论玩家在游戏世界中的哪个位置，都能统一播放。3D 声音则根据玩家的位置而变化。

## 3D 与 2D 声音属性

音频剪辑是 2D 还是 3D 取决于其导入设置。3D 音频指的是 3D 游戏世界中的声音。音频源可以附加到整个游戏场景中的游戏对象上。3D 声音的音量会随着音频监听器的距离和移动而变化。

无论音频监听器的位置如何，2D 声音都以相同音量播放。其唯一属性 `声像` 控制两个扬声器之间的音量，默认值 0 表示两个扬声器以相同音量播放声音。将滑块向左移动会增加左扬声器的音量，而将滑块向右移动则会增加右扬声器的音量（图 11-10）。

![A978-1-4302-6587-0_11_Fig10_HTML.jpg](img/A978-1-4302-6587-0_11_Fig10_HTML.jpg)

图 11-10. Inspector 中的 3D 和 2D 声音设置属性

3D 声音可以通过 3D 声音设置属性进行微调。`多普勒级别` 会根据运动扭曲声音，以产生类似多普勒效应的效果，即靠近声源时音调较高，远离声源时音调较低。

3D 声音表现如同从游戏世界中的特定位置发出，因此声音在声源处最响，并随着远离而减弱。`音量衰减` 属性决定了声音随距离减弱的速率。默认值为 `对数衰减`，与现实世界一致。您还有 `线性衰减` 和 `自定义衰减` 选项。

`最小距离` 是音量达到 100% 时与音频源的最小距离。3D 声音由 Unity 游戏引擎根据 `声像级别` 进行管理，其中 0 基本上是 2D 声音，1 则是完全 3D。`扩散角` 是一个更高级的音频设置，与扬声器系统有关。`最大距离` 是音量衰减导致的声音变化结束的最大距离。通常情况下，这也是您仍能听到声源任何声音的最远点。

## 音频剪辑

源音频文件就是音频剪辑，无论文件类型如何。Unity 支持 `.aif`、`.wav`、`.mp3` 和 `.ogg` 格式。您的项目中已经有一个包含在 `Land Mine` 预制件中的音频剪辑（图 11-11）。

![A978-1-4302-6587-0_11_Fig11_HTML.jpg](img/A978-1-4302-6587-0_11_Fig11_HTML.jpg)

图 11-11. Land Mine 预制件音频源组件中的剧烈爆炸音频剪辑及其属性

### 音频设置

Unity 中有些设置会影响整个项目的音效。你可以通过 Unity 编辑器顶部的菜单选择 `Edit ➤ Project Settings ➤ Audio` 来访问 `Audio Manager`（音频管理器），随后 `Audio Manager` 会出现在 Inspector（检视面板）中（图 11-12）。

![A978-1-4302-6587-0_11_Fig12_HTML.jpg](img/A978-1-4302-6587-0_11_Fig12_HTML.jpg)

图 11-12. 由 `AudioManager` 处理的全局音频设置

`Audio Source`（音频源）组件的音量设置允许你调整其相对音量。`AudioManager Volume`（音频管理器音量）属性则控制所有正在播放声音的整体音量。

对于 3D 音频，`Volume Rolloff`（音量衰减）允许你在 `Linear`（线性）、`Logarithmic`（对数）和 `Custom`（自定义）之间进行选择。在 `AudioManager` 中，`Rolloff Scale`（衰减缩放）用于调整 `Logarithmic`（对数）曲线的形状，从而影响衰减速度。

`Speed of Sound`（声速）默认值为海平面声速，并会影响启用了多普勒效应的声音。`Audio Source`（音频源）的 `Doppler Level`（多普勒等级）属性设置了应用于音频片段的多普勒失真程度，而 `AudioManager`（音频管理器）的 `Doppler Factor`（多普勒因子）则设置了失真效果的明显程度，其中 `0` 表示完全关闭，`1` 表示最明显。

`Default Speaker Mode`（默认扬声器模式）设置可用的声道数，对于立体声扬声器，默认值为 `2`。

`DSPBuffer Size`（DSP 缓冲区大小）最好保持默认，因为它与 CPU 如何处理聚合音频文件有关，属于本书范围之外的高级话题。

### 音频测试

在 `Scene view`（场景视图）的顶部，你会找到一个带有扬声器图标的按钮，它代表 `Audio Preview mode`（音频预览模式）（图 11-13）。不必为了测试游戏音频而进行游戏试玩，你只需切换开启 `Audio Preview mode`（音频预览模式），并在游戏世界中移动场景摄像机即可。

![A978-1-4302-6587-0_11_Fig13_HTML.jpg](img/A978-1-4302-6587-0_11_Fig13_HTML.jpg)

图 11-13. `Scene view`（场景视图）中的 `Audio Preview`（音频预览）切换开关

## 为障碍赛道添加音频

在将音频添加到障碍赛道原型之前，你需要先从 `Asset Store`（资源商店）下载一些免费资源。在 Unity 编辑器顶部菜单中选择 `Window ➤ Asset Store`。搜索“Free music pack”，找到 Alchemy Studio 提供的示例音频包（图 11-14）。点击蓝色的 `Download`（下载）按钮，然后在提示时点击 `Import`（导入）。

![A978-1-4302-6587-0_11_Fig14_HTML.jpg](img/A978-1-4302-6587-0_11_Fig14_HTML.jpg)

图 11-14. Unity 资源商店中的免费音乐包

首先，请保持项目结构清晰。在 `Project panel`（项目面板）中选择 `Assets`（资源）文件夹，然后选择 `Create ➤ Folder`（创建 ➤ 文件夹），并将其命名为 `Audio`。将你刚刚导入的 `Free Pack`（免费包）文件夹移动到 `Audio` 文件夹中，然后选中它查看其内容。

### 权限与鸣谢

在游戏中使用的 `Asset Store`（资源商店）资源时，务必查看任何 `Readme`（自述）文件，确认是否有任何要求或限制（图 11-15）。要求可能包括在鸣谢中列出资源创建者，而限制则可能包括你不能转售或再销售你下载的资源，无论是免费还是购买的。

![A978-1-4302-6587-0_11_Fig15_HTML.jpg](img/A978-1-4302-6587-0_11_Fig15_HTML.jpg)

图 11-15. `Free Pack`（免费包）文件夹中的 `Readme`（自述）文件

单击 `Readme`（自述）文件可在 `Inspector`（检视面板）中查看其内容，或双击在 MonoDevelop 中打开（图 11-16）。

![A978-1-4302-6587-0_11_Fig16_HTML.jpg](img/A978-1-4302-6587-0_11_Fig16_HTML.jpg)

图 11-16. 在 `Inspector`（检视面板）中显示的 `Readme`（自述）文件内容

首先是感谢，并邀请你在资源商店中对此资源进行评分和评论。反馈对于了解用户需求至关重要，尤其有助于帮助遇到任何问题的人，包括识别新的 Bug。你的游戏同样需要反馈，因此请花时间提供相同的支持。

接下来是请求发送电子邮件，告知用户如何使用该资源，以及希望在鸣谢中包含的信息。了解自己的作品如何被使用是件有趣的事，得知人们喜欢自己倾注心血的产品，也会获得个人满足感。

将资源商店中的贡献者视为你扩展的团队成员。Alchemy Studio 的 Matteo Bosi 慷慨地允许我们在此项目中使用这个包。给 Matteo 快速发一封邮件，做个自我介绍，并告诉他你正在使用他的免费包制作原型。养成这个习惯，不久你就会结识来自世界各地同样热爱游戏制作的新朋友。游戏开发社区对开发过程中的艰辛和磨难非常支持且理解。他们会帮助你、为你加油，并在你发布游戏时与你一同庆祝。一定要通过回馈来分享这份关爱。

继续阅读 `Readme`（自述）文件，你会找到 Alchemy Studio 提供的更多音效资源的链接，以及可以在导入到项目之前试听的地址。这是常见做法，正如你从 Alien 和 Spartan King 动画剪辑的示例视频中回忆的那样，这可以避免为了测试而导入所有资源。

### 音乐

音乐有多种用途，包括背景音乐和根据游戏玩法做出的响应。对于几乎所有行为比 `Play On Awake`（唤醒时播放）更复杂的音效，你将使用脚本来指导其行为。

### 背景音乐

首先，从一个最简单的例子开始。在 Unity 编辑器顶部菜单中，依次选择 `GameObject` > `Create Empty`，将其命名为 `Audio`，并将 `Transform` 位置设置为 `(0, 0, 0)`。在层级面板中选中 `Audio` 游戏对象，然后在 `Inspector` 面板中选择 `Add Component` > `Audio` > `Audio Source`。

在项目面板中打开 `Free Pack` 文件夹，你会看到一个 `Metal Loop` 文件夹。打开该文件夹，可以看到四个音频剪辑和另一个 `Readme` 文件。从 `Readme` 文件中你将了解到，第一个剪辑是一段完整的音乐轨道，另外三个则是同一轨道拆分出的**前奏**、**为循环播放设计的中间部分**以及**结尾**。选择 `“Loop 16 - 145 bpm - complete”` 文件，在 `Inspector` 面板中查看其属性（图 11-17）。

![A978-1-4302-6587-0_11_Fig17_HTML.jpg](img/A978-1-4302-6587-0_11_Fig17_HTML.jpg)

图 11-17. 项目面板和 `Inspector` 面板中的 `“Loop 16 - 145 bpm - complete”`

取消勾选 `3D Sound`，这样无论玩家角色的位置或移动如何，音量都会保持一致。在层级面板中选中 `Audio` 游戏对象，如果弹出 `Unapplied import settings` 对话框，请选择 “Apply”，然后将 `“Loop 16 - 145 bpm - complete”` 音频剪辑拖拽到 `Audio Source` 组件的 `Audio Clip` 属性字段中。`Play On Awake` 默认应已勾选。如果没有，请手动勾选。由于这是你希望在游戏全程始终均匀播放的背景音乐，因此为其设置最高优先级 `0`。保存场景并播放。

音频剪辑会从头播放到尾，然后停止。尝试勾选 `Audio Source` 的 `Loop` 属性并再次播放。现在音频剪辑会重复播放，但循环之间的衔接不太平滑，这正是部分音频剪辑发挥作用的地方。

要实现连续播放的背景音乐，你希望前奏在开始时只播放一次，接着是循环部分持续循环播放。要实现这种行为，你需要一个脚本。

在项目面板中，选择 `Assets > Scripts` 文件夹，然后创建一个名为 `BackgroundMusic` 的新脚本。在 MonoDevelop 中打开它，并将代码编辑如下：

```
#pragma strict

function Start () {
    PlayBackgroundMusic();
}

function PlayBackgroundMusic () {
    if(audio.isPlaying == true) {
        audio.Stop();
    }
    audio.Play();
}
```

代码分解如下：

(1) `PlayBackgroundMusic();`

保持 `Start()` 函数的简洁，将播放背景音乐的代码块放入其自身的函数 `PlayBackgroundMusic()` 中，该函数从 `Start()` 函数中调用。

(2) `if(audio.isPlaying == true)`

检查音频剪辑是否已经在播放。添加这个测试是良好的实践，而不是简单地单独使用 `audio.Play()`，以避免出现奇怪的音频行为。

(3) `audio.Stop();`

如果音频剪辑已经在播放，则将其停止。

(4) `audio.Play();`

从头开始播放音频剪辑。

在 `Inspector` 面板中，取消勾选 `Audio Source` 组件的 `Play On Awake` 和 `Loop` 属性。将 `BackgroundMusic` 脚本附加到 `Audio` 游戏对象上。保存场景并进行播放测试。此时，你已实现了等同于 `Play On Awake` 的脚本功能。

接下来，通过将项目面板中的 `“Loop 16 - 145 bpm - intro”` 前奏剪辑拖拽到 `Inspector` 中 `Audio Source` 组件的 `Audio Clip` 属性字段中，来更换音频剪辑。保存并播放测试，现在前奏剪辑会立即播放且只播放一次。

在 MonoDevelop 中，将 `BackgroundMusic` 脚本编辑为：

# Unity 音频播放教程

## 代码分解

```csharp
#pragma strict

public var musicLoop : AudioClip;
private var introMusicIsPlaying : boolean = true;
private var looping : boolean = false;

function Start () {
    PlayBackgroundMusic();
}

function Update() {
    if (!audio.isPlaying) {
        introMusicIsPlaying = false;
    }
    if (!introMusicIsPlaying && !looping) {
        looping = true;
        LoopBackgroundMusic();
    }
}

function PlayBackgroundMusic () {
    if(audio.isPlaying == true) {
        audio.Stop();
    }
    audio.Play();
}

function LoopBackgroundMusic () {
    if(audio.isPlaying == true) {
        audio.Stop();
    }
    audio.clip = musicLoop;
    audio.loop = enabled;
    audio.Play();
}
```

代码分解如下：

(1) `public var musicLoop : AudioClip;`
声明一个类型为 `AudioClip` 的引用变量来容纳将要播放的第二个音频剪辑，并将其命名为 `musicLoop`。

(2) `private var introMusicIsPlaying : boolean = true;`
声明一个名为 `introMusicIsPlaying` 的私有布尔变量，并将其设置为 `true`，因为 `PlayBackgroundMusic()` 是从 `Start()` 函数中调用的，它将立即开始播放前奏剪辑。

(3) `private var looping : boolean = false;`
声明一个名为 `looping` 的私有布尔变量，并将其设置为 `false`。

(4) `if (!audio.isPlaying)`
在 `Update()` 函数中，检查前奏剪辑何时播放完毕。记住 `!` 表示“非”，所以这行代码可以理解为“如果音频剪辑没有在播放”，当前奏剪辑结束时，这个条件将为真。

(5) `introMusicIsPlaying = false;`
一旦前奏剪辑播放完毕，将 `introMusicIsPlaying` 布尔变量切换为 `false`。

(6) `if (!introMusicIsPlaying && !looping)`
如果前奏音乐没有在播放，且 `looping` 仍为 `false`，那么是时候切换音频剪辑了。

(7) `LoopBackgroundMusic();`
调用 `LoopBackgroundMusic()` 函数，你将在下一步编写这个函数。

(8) `if(audio.isPlaying == true) {` `audio.Stop();` `}`
在更换音频剪辑之前调用 `audio.Stop()` 函数是一个良好的实践。

(9) `audio.clip = musicLoop;`
将当前音频剪辑切换为 `musicLoop` 所引用的音频剪辑。

(10) `audio.loop = enabled;`
启用 `Audio Source` 的 `Loop` 属性。

(11) `audio.Play();`
播放新的音频剪辑。

在 Unity 编辑器中，将 `"Loop 16 - 145 bpm - loop"` 音频剪辑拖放至 `Audio` 游戏对象的 `BackgroundMusic` 脚本组件中的 `Music Loop` 属性字段。保存场景并播放测试。现在，前奏音频剪辑会无缝衔接到循环音频剪辑，而循环剪辑则会持续循环播放。

然而，这里有些不对劲：随着玩家角色在障碍赛道中不断前进，音乐声逐渐减弱了。这是因为音频剪辑的初始导入设置是 3D 模式。在项目面板中，打开 `Assets > Free Pack > Metal Loop Pack`，在 Inspector 面板中依次选中每个音频剪辑，取消勾选 `3D Sound` 属性。如果弹出 `Unapplied Import Settings` 对话框，请选择 `Apply`。保存场景，进行播放测试，现在无论玩家角色在障碍赛道中的哪个位置，背景音乐都能连续且均匀地播放了。

### 事件触发的音效

通过触发器是在游戏过程中播放短音频片段的一种方法。

Free Pack 文件夹中有一个 Fantasy Victory (free) 音频片段。玩家成功躲避敌方无人机并抵达高架轨道的最终平台，标志着障碍赛的成功完成，因此当玩家角色到达此处时，你将播放这个音频片段。

如果你事先想到先创建一个空游戏对象，添加一个音频源组件，然后将 Fantasy Victory (free) 音频片段拖入音频剪辑属性中，那你就想对了。更棒的是，Unity 提供了一些便捷的快捷方式，可以省去这些步骤。

将 Fantasy Victory (free) 音频片段拖放到层次结构中。请务必将其放置在游戏对象之间，而不是放在游戏对象上（那样会高亮显示）。Fantasy Victory (free) 游戏对象将出现在层次结构中，并附带一个音频源组件，且 Fantasy Victory (free) 音频片段已自动分配给音频剪辑属性。

再次将音频片段拖入场景视图，注意当你在场景中将其拖到其他游戏对象上时，会出现一个带有 + 符号的绿色圆圈。如果你将音频片段放到某个游戏对象上，Unity 会自动创建一个音频源组件并将其附加到该游戏对象上。请注意——如果该游戏对象已有一个分配了剪辑的音频源，而你又将这个片段拖上去，那么新的音频剪辑将替换之前分配的剪辑。现在先不要真的将 Fantasy Victory (free) 拖入场景视图。如果你已经拖了，请使用删除组件选项将其删除，这样你只保留第一个，即仅包含变换和音频源组件的 Fantasy Victory (free) 游戏对象。

将这个游戏对象的变换位置设置为 `(0, 11.5, 150)`，使其位于最终坡道顶部的中心位置。如果你现在进行试玩，由于默认勾选了“唤醒时播放”，该剪辑会立即播放。在检查器中取消勾选它。仍在检查器中时，选择添加组件 ➤ 物理 ➤ 盒子碰撞体。勾选 `是触发器`，并将“中心”的值改为 `(0, 1, 0)`，“大小”的值改为 `(8, 2, 0.2)`。

你需要一个脚本，当玩家与这个触发碰撞体接触时播放音频，因此在检查器中选择添加组件 ➤ 新建脚本并将其命名为 `VictoryMusic`，然后点击创建并添加。请注意，通过此方法创建的新脚本会位于资源根文件夹中。通过将其移动到 `Assets ➤ Scripts` 文件夹来保持项目组织有序，然后在 MonoDevelop 中打开它。将代码编辑如下：

```csharp
#pragma strict

public var backgroundMusic : GameObject;

function OnTriggerEnter () {

backgroundMusic.GetComponent(BackgroundMusic).PlayVictoryMusic();

}
```

你之前见过这类代码。其分解如下：

(1) `public var backgroundMusic : GameObject;`
声明一个类型为 `GameObject` 的公共引用变量 `backgroundMusic`，用于持有对音频游戏对象的引用。

(2) `function OnTriggerEnter ()`
当玩家角色进入碰撞体体积时，由于碰撞体设置为 `是触发器`，因此会调用 `OnTriggerEnter()`。

(3) `backgroundMusic.GetComponent(BackgroundMusic).PlayVictoryMusic();`
获取附加到音频游戏对象上的 `BackgroundMusic` 脚本组件的引用，并调用你接下来要编写的 `PlayVictoryMusic()` 函数。另一种方法是停止背景音乐，然后使用此游戏对象和脚本播放胜利音乐音频剪辑，但由于这是背景音乐，为保持项目组织有序，更好的做法是将其包含在一个专用的游戏对象中。

保存脚本，然后将其附加到 Fantasy Victory (free) 游戏对象上。在层次结构中选择 Fantasy Victory (free) 游戏对象，然后将层次结构中的音频游戏对象拖到胜利音乐脚本组件的背景音乐字段中。

在 MonoDevelop 中，返回到 `BackgroundMusic` 脚本并添加以下代码行：

# UnityScript 音频系统详解

## 代码示例与分解

```csharp
#pragma strict

public var musicLoop : AudioClip;

private var introMusicIsPlaying : boolean = true;

private var looping : boolean = false;

public var victoryMusic : AudioClip;

function Start () {

PlayBackgroundMusic();

}

function Update() {

}

function PlayBackgroundMusic () {

}

function LoopBackgroundMusic () {

}

function PlayVictoryMusic () {

if(audio.isPlaying == true) {

audio.Stop();

}

audio.clip = victoryMusic;

audio.Play();

}
```

分解如下：

`(1) public var victoryMusic : AudioClip;`

创建一个类型为 `AudioClip` 的引用变量 `victoryMusic`。作为公共变量，你可以轻松地通过将不同音频剪辑拖放到检查器的胜利音乐字段中来测试它们。

`(2) function PlayVictoryMusic () {`

创建一个新的 `PlayVictoryMusic()` 函数。

`(3) if(audio.isPlaying == true) { audio.Stop(); } audio.clip = victoryMusic; audio.Play();`

这些步骤与你之前替换音频剪辑的步骤相同，只是这次新的音频剪辑是 Fantasy Victory (free) 音频片段。

保存脚本。在层次结构中选中音频游戏对象，将项目面板中 Assets ➤ Free Pack 文件夹中的 Fantasy Victory (free) 音频片段拖到背景音乐脚本组件的胜利音乐字段中。保存场景并保存项目。

让玩家角色四处跑动，你会注意到当玩家角色进入触发体积时，音频剪辑会播放，这正是你想要的，但你也会注意到，如果玩家角色远离 Fantasy Victory (free) 游戏对象附近，音乐会逐渐消失。该音频剪辑设置为 3D 音效，音量衰减设置为对数衰减。

在项目面板中选择 Fantasy Victory (free) 剪辑，然后在检查器中取消勾选 3D 音效，如果提示则点击应用。保存场景，再次试玩，现在设置为 2D 音效后，无论玩家角色如何移动，音乐都会保持相同音量。甜蜜的胜利！

## 音效

音效一词通常指非音乐或非对话的音频片段。音效更常使用 3D 声音，因为它们通常与场景中的特定游戏对象相关联。

打开 `Assets` ➤ `Sample Assets` ➤ `Vehicles` ➤ `Car` ➤ `Audio` 文件夹，找到 `Low Acceleration` 音频片段。选中它并在 `Inspector` 中确认 `3D Sound` 复选框已勾选。在 `Hierarchy` 中选择 `Enemy Drone` 游戏对象，然后将光标移至 `Scene` 视图并按 `F` 键，将 `Enemy Drone` 游戏对象显示在视图中。

将 `Low Acceleration` 音频片段从 `Project` 面板拖放到 `Drone` 上。此时会自动创建一个 `Audio Source` 组件，当其在 `Inspector` 中显示时，`Low Acceleration` 文件已自动分配给 `Audio Clip` 属性。在 `Scene` 视图中，还会出现一个由扬声器符号组成的音频源辅助图标（图 11-18）。

![A978-1-4302-6587-0_11_Fig18_HTML.jpg](img/A978-1-4302-6587-0_11_Fig18_HTML.jpg)

图 11-18. `Scene` 视图中 `Drone` 游戏对象的音频源辅助图标

在 `Inspector` 中，勾选 `Play On Awake` 和 `Loop`。在 `3D Sound Settings` 部分将 `Min Distance` 设置为 `2`，`Max Distance` 设置为 `20`。

暂时取消勾选 `Audio` 游戏对象上的 `Audio Source` 组件以禁用背景音乐。保存场景并进行试玩测试。

当玩家接近最终区域时，您会开始听到无人机的音频。当玩家进入该区域且敌方无人机开始移动时，音量会随着无人机靠近而增大。由于 `Volume Rolloff` 属性默认设置为 `Logarithmic Rolloff`，无人机越近，音量增加速率越快。添加音效后，敌方无人机无疑更具威慑力。

目前，`Audio Listener` 挂载在 `Free Look Camera Rig - Pivot` 的 `Main Camera` 子对象上。请记住，每个场景只能有一个 `Audio Listener`，因此在 `Hierarchy` 中找到 `Main Camera`，然后在 `Inspector` 中取消勾选其名称旁的复选框以禁用 `Audio Listener` 组件。

返回 `Hierarchy`，找到 `Third Person Character Ragdoll` 并选中。在 `Inspector` 中选择 `Add Component` ➤ `Audio` ➤ `Audio Listener`。再次试玩，您会发现当无人机靠近时，其声音效果更好，因为玩家角色比摄像机更接近无人机。

在 `Hierarchy` 中找到 `Audio` 游戏对象，重新启用背景音乐的 `Audio Source`。使用 `Audio Source` 组件中的 `Volume` 设置来平衡背景音乐与音效的合适音量。

每一个细节都至关重要，决定了游戏是优秀还是卓越。激光火花的噼啪声应从火花产生的位置发出，而非玩家上方的激光源。为抛射体添加多普勒效应能增强其速度感，而带有厚重低音的“嗖嗖”声则能让人感受到摆锤的沉重。

## 总结

请为自己鼓掌——您已经掌握了 UnityScript 的基础知识，正走在成为游戏开发者的康庄大道上。您能够与玩家沟通，并使用 GUI 控件和 `OnGUI()` 函数增加交互性，还能自定义这些控件以匹配游戏的外观。您还添加了多种形式的音频，包括音效和背景音乐，丰富了游戏体验的深度。

您已在 `Asset Store` 中找到了更多资源，并记得在游戏最终版本中使用的任何作品上注明资产创建者。同时，您也在游戏开发社区中结识了新伙伴。在下一章中，您将学习更多关于测试和优化游戏以便发布的技巧。胜利在望！