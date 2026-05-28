# 6. 角色职业

对于许多 Roguelike 游戏来说，游戏体验的重要组成部分之一是提供多种玩家角色原型选择。我认为这源于许多游戏受到桌面角色扮演游戏的启发，其中大多数游戏提供角色职业作为模板，可以为每个玩家构建独特的角色。

在本章中，我们将创建角色职业。这是借鉴自桌面角色扮演游戏（TTRPG）的概念，你可能之前接触过这些精彩游戏，或玩过包含相同概念的其他游戏，因此对此已有所了解。为了简化我们的示例游戏，我们将角色职业和角色种族视为同一概念。我们不想提供太多组合，以免使示例游戏过于复杂。

将职业视为原型。它们是一个虚构幻想职业应具备的常见特征。在 TTRPG 中，人们常常会花大力气定制角色，创造出独特、可信且丰满的角色。但这不是我们在这里要做的；我们追求的是原型——只需足够多的特征，让角色能被辨识为我们想要代表的那种原型。

在实现我们的职业之前，需要重构一些代码。

## 又是一次重构

我们似乎每一章都以重构开头；事实确实如此，而且这是有意为之。随着章节推进，我们将从易于理解的代码逐步转向更灵活、更强大的结构。本章中，我们将再次进行重构，但在讨论需要修补的部分之前，理解为何要在此处修改代码至关重要。

我们的角色职业将是经典类型，比如战士、精灵、矮人等。我们需要让它们在游戏体验上更加差异化，从而让游戏感觉更丰富。目前，我们可以更改角色使用的图像、移动方式、生命值和行动点数，但仅此而已。我认为我们需要更多要素来充实这些职业。

如果初步粗略地思考一下这些职业的主要特征，我们很可能会得出结论：他们的装备和武器是我们期望这些职业在游戏或故事中呈现方式的核心组成部分——矮人手持盾牌、身穿铠甲、挥舞战斧；精灵行动敏捷、用弓箭射出大量箭矢；法师则充满法术与奇迹。

为了支持不同的职业及其装备和玩法，我们将重构源代码的许多部分。与其在实现某个职业时逐片段地添加所需功能，我认为更好的做法是预先添加所有功能，然后在本章末尾逐节创建每个职业。实际上这也更有趣，因为所有功能都已就绪，你只需在创建职业时发挥创意即可。

由于游戏某些部分之间存在一定的紧耦合，最好在阅读源代码之前先解释我们要添加哪些功能，因为某些功能需要跨多个文件进行多处修改才能生效。

### 支持防御加成

矮人以持盾闻名；圣骑士通常身着重甲。要增加能帮助防御的装备支持，我们本可以通过在每个物品的 `turn` 和 `refresh` 函数中强行添加数值到玩家生命值来实现，但这将难以维护且非常不优雅。

我们将采取的做法是：确保所有物品都有一个名为 `protection` 的新函数，该函数返回一个数值。不提供任何防御效果的物品应返回零。这样，要制作一个盾牌，我们只需创建一个物品并使其返回大于零的数值即可。

然后，我们修改地下城模块中的攻击逻辑以考虑这一点。伤害值应为攻击值减去受害者已装备物品提供的防御值。

### 支持远程攻击

法师和精灵以远程攻击著称。当敌人过于接近时，他们通常表现不佳，但在远距离上，他们令人畏惧。当前的源代码将攻击视为试图进入敌人占据的格子。我们需要增加对远程攻击的支持，以便这些职业能在游戏中使用。

添加此功能需要多处修改。首先，我们需要更改武器，使其具有射程值。就像处理 `attack` 和 `protection` 一样，我们将使用一个名为 `range` 的新函数。仅用于近战战斗的装备射程值设为零。

既然已知特定武器的射程，我们需要一种方式来指示要用它攻击某个特定格子。目前，玩家使用方向键移动，或用空格键跳过回合，但无法选择格子。为了解决这个问题，我们将修改玩家角色类，使其包含鼠标输入处理器。我们只将其用于远程攻击。如果玩家有足够的行动点数，并点击了某个射程内有敌人的格子，我们将发动一次远程攻击。

这需要修改地下城模块以支持远程攻击。一个关键改动是动画：当前动画是玩家角色图块移动到敌人图块再返回。对于远程攻击，我们不能这样做；需要动画化某种投射物。我们将通过为用于远程攻击的武器提供另一个名为 `attackTile` 的图块来实现这一点。

完成这些修改后，我们终于可以开始实现职业了。为了启动重构，让我们先修补地下城模块以支持这两个功能。

## 重构地牢模块

这个初始示例将重构与我们即将实现的第一个职业结合在一起。相关文件位于 `chapter-6/example-1-warrior/` 目录下。该示例中一处重要的改动是：骷髅现在每回合移动三格。这样做的目的是让它们对某些英雄职业构成挑战，而对其他职业则只是普通敌人。如果它们每回合仍然只移动一格，那么敏捷的精灵和笨重的矮人面对它们时就会毫无区别。通过这些改动，我们调整了地牢中的危险平衡。说到地牢，让我们继续重构工作，补丁将应用于 `dungeon.js` 中的地牢模块。

我们需要修改 `attackEntity` 函数，使其支持加成效果和远程攻击。首先要做的就是在函数签名中增加一个额外参数。

```
attackEntity: function (attacker, victim, rangedAttack = false) {
```

这个 `rangedAttack` 参数要么使用默认值 `false`，要么是一个代表远程攻击使用格子的数值。该参数也是 `attackEntity` 判断近战与远程攻击的依据。我们不希望这个函数直接探查玩家装备的物品来查找当前武器，然后检查该物品的射程函数返回值，这样做侵入性太强且耦合度过高；我们将判断攻击类型的职责委托给调用 `attackEntity` 的实体。要注意，这也使得怪物也能发动远程攻击，这很有趣。

```
attacker.moving = true
attacker.tweens = attacker.tweens || 0
attacker.tweens += 1
if (!rangedAttack) {
```

函数开头与之前基本相同，为攻击者设置一些补间计数器，然后根据是否为远程攻击进行分支处理。

```
this.scene.tweens.add({
targets: attacker.sprite,
onComplete: () => {
attacker.sprite.x = this.map.tileToWorldX(attacker.x)
attacker.sprite.y = this.map.tileToWorldX(attacker.y)
attacker.moving = false
attacker.tweens -= 1
let attack = attacker.attack()
let protection = victim.protection()
let damage = attack - protection
if (damage > 0) {
victim.healthPoints -= damage
this.log(`${attacker.name} 对 ${victim.name} 造成 ${damage} 点伤害。`)
if (victim.healthPoints <= 0) {
this.removeEntity(victim)
}
}
},
x: this.map.tileToWorldX(victim.x),
y: this.map.tileToWorldY(victim.y),
ease: "Power2",
hold: 20,
duration: 80,
delay: attacker.tweens * 200,
yoyo: true
})
} else {
```

补间动画基本保持不变，但请注意我们新增了一个名为 `protection` 的变量，用于存储受害者的防御加成值，该值会从对受害者造成的伤害中扣除。

远程攻击基本相同，我们只修改了补间动画的参数。我们不需要它来回摇摆；射出一支箭时，你不会指望它飞回来（除非你在实现回旋镖）。动画的时间需要延长，因为起始坐标和结束坐标之间的距离更大。如果保持这些值不变，动画会快得无法看清。

## 远程攻击实现

远程攻击使用的格子与玩家格子不同。该格子通过一个直接从玩家坐标到受害者坐标的补间动画实现。为此，我们创建一个全新的精灵，将其添加到场景中，并对其进行补间动画。补间动画完成后，该精灵不再有用，我们将其销毁。除此之外，其余代码与近战攻击相同；我们同样会考虑潜在的防御加成。

这些更改假设所有物品都具备某些函数；为了实现这一点，我们需要修补通用的物品类。

## 修补通用物品类

通用物品类的代码位于 `items/genericItem.js` 中；我们需要向其中添加两个函数。

```javascript
protection() {
    return 0
}
range() {
    return 0
}
```

现在我们可以确保，在攻击中使用已构建好的物品时，地牢模块不会报错。应用这些代码更改后，游戏的运行方式应与第五章完全相同。武器的工作方式不变，由于没有物品提供任何防御或射程加成，所有体验也保持不变，这意味着是时候改变这一点了。

为了更容易实现多种物品，我们创建了一个通用的物品类，并让所有物品都继承它，重写需要修改的部分以使其独特。我们将对角色职业采用相同的方法。我们将创建一个基础英雄职业，其感觉与当前玩家角色非常相似，然后让其他职业在此基础上扩展。

## 创建基础英雄类

我们将创建一个`classes`文件夹来存放所有角色职业，并将`basicHero.js`放在其中，作为后续所有职业的基类。这里有个容易混淆的地方：我借用了 RPG 中的“职业”概念来称呼这个功能，同时又用 JavaScript 的“类”来实现它。这就导致在讨论两个使用相同术语且同时实现的概念时有些拗口。因此，我们将使用 JavaScript 类来实现角色职业。

将之前的`player.js`（本示例中已移除）重构为`basicHero.js`，也是一个调整该类某些运作方式的好机会。

我对其中输入事件的处理方式不太满意，而且由于还需要实现鼠标输入处理，我们将把输入处理逻辑从`turn`函数中分离出来。`turn`函数将用于处理每个回合都需要执行但与玩家输入无关的内容。例如，如果玩家生命值过低，我们会将玩家精灵染成红色。

我们将重构输入处理器；需要将装备切换功能扩展为同时处理移动。具体做法是将`turn`中的代码复制粘贴到一个新函数中，由处理器调用。此外，还会添加一个鼠标输入处理器来处理远程攻击。需要注意的是，两个处理器在非玩家回合时也会被激活，因此它们需要双重检查玩家是否还有行动点和移动点，然后再执行操作。

在第五章 5 之前，当玩家移动点为零时，回合就结束了。这意味着可能会出现未消耗的行动点。这以前不是问题，因为攻击与移动绑定，没有移动点就无法攻击并消耗行动点。

现在加入了远程攻击选项，我们需要确保回合结束时玩家的行动点归零，否则他们就能在自己的回合外射箭。最简单的做法是在切换回合前检查移动点和行动点是否都为零。但这种做法的弊端是，有时玩家没有需要使用行动点的事情，也无法消耗它们。这就会迫使他们每回合都要按空格键跳过，导致游戏体验变得很繁琐。

为了解决这个问题，我们将保持移动结束即切换回合的机制，但确保当玩家移动点归零时，行动点也同时归零。这样玩家可以在不消耗行动点的情况下继续行走，游戏循环保持不变。

这引出了我们的《纳米地牢》游戏玩法的一个重要方面：它是一款“后移动”类游戏，玩家需要在消耗所有移动点之前进行攻击和采取行动。这是一个设计决策，你自己的游戏项目可能有不同的处理方式。但这一点必须明确，因为它影响着游戏的手感，并会左右战术策略，进而反哺整个游戏设计。

那么，我们开始实现基础英雄类吧。

```javascript
import dungeon from "../dungeon.js"
export default class BasicHero {
    constructor(x, y) {
        this.name = "英雄"
        this.movementPoints = 1
        this.actionPoints = 1
        this.healthPoints = 30
        this.x = x
        this.y = y
        this.tile = 29
        this.moving = false
        this.type = "character"
        this.items = []
        dungeon.scene.input.keyboard.on("keyup", (event) => {
            if (!this.over()) {
                this.processInput(event)
            }
        });
        dungeon.scene.input.on("pointerup", (event) => {
            if (!this.over()) {
                this.processTouchInput(event)
            }
        });
    }
```

这段代码我们很熟悉，只是移除了处理方向键的`cursor`属性，并添加了两个事件处理器：一个用于按键释放，另一个用于鼠标按钮释放（或手指从屏幕抬起）。

在按键、鼠标或手指抬起时执行操作，是出于一个非常具体的原因。如果持续按下，事件会重复触发，并且频率非常快。处理`keydown`和`pointerdown`会导致处理器在按键或手指按下期间多次触发，使游戏出现错误。如果你试着将`keyup`替换为`keydown`并尝试让玩家角色行走，就会发现几乎不可能只走一格；即使是最快的按键，事件也会多次触发。

在两个函数中，我们都使用了`over`的结果来判断当前是否是玩家回合，然后再尝试处理任何可能的输入。

我们先实现`processInput`。该函数接收一个来自`keyup`事件的事件对象作为参数。

```javascript
processInput(event) {
    let oldX = this.x
    let oldY = this.y
    let moved = false
    let newX = this.x
    let newY = this.y
    let key = event.key
```

到目前为止没问题，代码与之前在`turn`函数中的代码非常相似。主要区别在于我们设置了一个`key`变量来保存按键的值。

```javascript
// 装备物品
if (!isNaN(Number(key))) {
    if (key == 0) {
        key = 10
    }
    this.toggleItem(key - 1)
}
```

第一个检查是看按键是否是切换装备的数字键。

```javascript
// 跳过回合
if (event.keyCode == 32) {
    this.movementPoints = 0
    this.actionPoints = 0
}
```

在本示例中，我们为空格键添加了一个处理器。按下空格键会让玩家跳过当前回合。这对玩家来说是一个有用的战术资源，例如他们希望怪物靠近或等待药水生效，同时不改变自己在地图上的位置，或者仅仅是避免用完所有移动点。

```javascript
// 移动决策
if (event.key == "ArrowLeft") {
    newX -= 1
    moved = true
}
if (event.key == "ArrowRight") {
    newX += 1
    moved = true
}
if (event.key == "ArrowUp") {
    newY -= 1
    moved = true
}
if (event.key == "ArrowDown") {
    newY += 1
    moved = true
}
```

现在不再像之前的示例那样使用`this.cursor`，而是直接使用`event.key`的值来检查是否按下了方向键。这与之前的代码非常相似，但由于所有的键盘输入处理逻辑现在都封装在自己的函数中，维护起来更加容易。

## 战斗与移动逻辑

```javascript
// 执行移动
if (moved) {
this.movementPoints -= 1
if (!dungeon.isWalkableTile(newX, newY)) {
let entity = dungeon.entityAtTile(newX, newY)
// 检查目标位置实体是否为敌人
if (entity && entity.type == "enemy" && this.actionPoints > 0) {
const currentWeapon = this.currentWeapon()
const rangedAttack = currentWeapon.range() > 0 ? currentWeapon.attackTile || currentWeapon.tile : false
dungeon.attackEntity(this, entity, rangedAttack)
this.actionPoints -= 1
this.movementPoints += 1
}
```

战斗处理代码需要调整。游戏被设计为玩家一次只能装备一把武器。如果装备了远程武器，那么使用它攻击时需要检查敌人是否在射程内，并将武器的`attackTile`传递给`dungeon.attackEntity`函数（从而实现远程攻击）。这意味着即使是在近战中，装备远程武器的玩家也会用它来攻击。如果怪物靠近，玩家需要自己切换武器。由于切换武器是免费动作（不消耗行动点），玩家可以在自己的回合内根据情况更换武器。

```javascript
// 检查目标位置的实体是否为物品
if (entity && entity.type == "item" && this.actionPoints > 0) {
    this.items.push(entity)
    dungeon.itemPicked(entity)
    dungeon.log(`${this.name} 捡起了 ${entity.name}：${entity.description}`)
    this.actionPoints -= 1
} else {
    newX = oldX
    newY = oldY
}
```

拾取物品的操作与之前保持一致。如果目标格子上存在实体且该实体为物品，则玩家会将其拾起并移动到该格子。

```javascript
if (newX !== oldX || newY !== oldY) {
    dungeon.moveEntityTo(this, newX, newY)
}
```

最后，只需要实际移动玩家角色即可。鼠标和触摸输入的处理方式略有不同。选择格子作为远程攻击目标是我们通过鼠标输入执行的唯一操作，因此其处理逻辑更简单。非常方便的是，Phaser 默认在场景中包含了输入插件^(³⁹)，这大大简化了输入处理。

```javascript
processTouchInput(event) {
    let x = dungeon.map.worldToTileX(event.worldX)
    let y = dungeon.map.worldToTileY(event.worldY)
    let entity = dungeon.entityAtTile(x, y)
```

鼠标或触摸输入的坐标在事件处理函数中可通过多种方式获取。当使用 `worldX` 和 `worldY` 访问时，这些值会映射到我们设定的世界和摄像机参数中，因此可以轻松转换为瓦片坐标，并借助 `dungeon` 模块获取该位置的潜在实体。

```javascript
if (entity && entity.type == "enemy" && this.actionPoints > 0) {
    const currentWeapon = this.currentWeapon()
    const rangedAttack = currentWeapon.range() > 0 ? currentWeapon.attackTile || currentWeapon.tile : false
    const distance = dungeon.distanceBetweenEntities(this, entity)
    if (rangedAttack && distance <= currentWeapon.range()) {
        dungeon.attackEntity(this, entity, rangedAttack)
        this.actionPoints -= 1
    }
}
```

执行远程攻击需要玩家拥有可用行动点，并点击射程内的敌人格子。如果所有条件都满足，则攻击可以执行。

如果不做进一步修改，我们的游戏将引入一个非常严重的错误：玩家可以在非其回合时攻击敌人。如果他们在回合结束时留有剩余行动点，并且敌人在回合外进入射程，玩家就能点击并攻击它们。为了解决这个问题，我们必须修改 `over` 函数，确保当玩家耗尽力点（移动点）时，同时将行动点归零并结束回合。

```javascript
over() {
    let isOver = this.movementPoints <= 0 && !this.moving
    if (isOver && this.UIheader) {
        this.UIheader.setColor("#cfc6b8")
        this.actionPoints = 0
    } else {
        this.UIheader.setColor("#fff")
    }
    return isOver
}
```

在回合结束时添加 `this.actionPoints = 0` 可以防止这个漏洞。

现在是时候实现我们的第一个角色职业——战士（Warrior）了。

### 创建战士职业

说实话，我们基础英雄类（`BasicHero`）本身就类似于战士职业；修改主要限于外观和移动方面。同时，我们修改了基础英雄的构造函数，不再给玩家配置任何装备，并移除了对 `dungeon` 模块的初始化调用。所有角色职业都将通过构造函数自定义参数，然后自行调用 `dungeon.initializeEntity`。

在 `basicHero.js` 旁边是战士职业的文件，名为 `warrior.js`。在该文件中，我们将自定义构造函数，为战士赋予比基础英雄更多的力点和行动点。我们还会为其装备一把基础长剑。不需要自定义精灵图块，因为基础英雄的精灵就是战士的精灵；不过其他职业的精灵会有所不同。

```javascript
import BasicHero from "./basicHero.js"
import Sword from "../items/sword.js"
import dungeon from "../dungeon.js"
```

所有角色职业都需要导入基础英雄类（以便继承）、游戏开始时想要赋予玩家的物品，以及 `dungeon` 模块（以便在构造函数末尾初始化实体）。

```javascript
export default class Warrior extends BasicHero {
    constructor(x, y) {
        super(x, y)
        this.name = "战士"
        this.movementPoints = 3
        this.actionPoints = 2
        this.items.push(new Sword())
        this.toggleItem(0)
        dungeon.initializeEntity(this)
    }
```

充分利用 JavaScript 新增的 `class` 关键字非常有益；继承基础英雄类省去了创建新英雄原型时编写大量重复样板代码的麻烦。同时，由于所有共享代码都封装在基础英雄类中，维护和改进这些代码也变得更加容易。

```javascript
    refresh() {
        this.movementPoints = 3
        this.actionPoints = 2
    }
}
```

虽然基础英雄每回合只能移动一格并攻击一次，但战士可以移动更多格并执行多次攻击。使用这个职业的感觉与之前的基础英雄截然不同。凭借额外的移动力和攻击次数，玩家不再那么畏惧被敌人围攻，在移动和战术上可以更加大胆。

为了便于尝试多种角色职业，我们将创建一个 `classes.js` 文件，该文件仅作为一个新对象，用于存储所有职业类的引用。该文件将由世界场景（world scene）导入，从而使所有原型在该场景中可用。要切换使用的英雄，只需修改世界场景中初始化的职业类即可。

```javascript
import Warrior from "./classes/warrior.js"
const classes = {
    Warrior
}
export default classes
```

在世界场景中导入该文件非常简单：

# 游戏开发：创建不同英雄职业

```javascript
import classes from "./classes.js"
```

在世界场景的 `create` 函数中初始化战士同样简单：

```javascript
dungeon.player = new classes.Warrior(15, 15)
```

所有进一步探索英雄原型所需的修改均已就位，我们的第一个职业——战士，虽然与旧版基础英雄非常相似，但玩法风格更加大胆。现在让我们实现另一个职业，是时候创建我们的第一个矮人了。

### 创建矮人

矮人是奇幻题材中的常客。在近期的虚构作品中，其形象已超越我们通常关联的原型范畴，如果能见到更多游戏受益于更灵活的矮人设定，那将十分酷炫。不过我们的示例将相当复古，重点聚焦于该原型最古老的特征：我们的矮人会受到护甲保护，移动距离比战士短，且具备强大的近战攻击力。

从本节开始，每个英雄原型都将包含在独立的示例文件中。矮人的代码位于 `chapter-6/example-2-dwarf/`。

正如本章前面所述，为某个职业赋予特色物品会让其感觉更丰满、更具可玩性。对于矮人，我们将先制作一把适合矮人的武器，再着手设计职业本身。矮人的基础武器是一把斧头，其代码位于 `items/axe.js`。

```javascript
import GenericItem from "./genericItem.js"
import dungeon from "../dungeon.js"

export default class Axe extends GenericItem {
  constructor(x, y) {
    super(x, y)
    this.tile = 934
    this.name = "一把斧头"
    this.description = "基础斧头。可造成 2 至 7 点伤害。"
    this.weapon = true
    dungeon.initializeEntity(this)
  }
  damage() {
    return Phaser.Math.Between(2, 7)
  }
}
```

该物品的实现方式与我们前几章创建的剑类似，但拥有独特的瓦片图标、描述和伤害值。任何谨慎的矮人都不可能不带盾牌就踏入地下城。让我们再创建一件物品，这次放在 `items/shield.js` 中。

```javascript
import GenericItem from "./genericItem.js"
import dungeon from "../dungeon.js"

export default class Sword extends GenericItem {
  constructor(x, y) {
    super(x, y)
    this.tile = 776
    this.name = "一面盾牌"
    this.description = "基础盾牌。提供+1 护甲值。"
    dungeon.initializeEntity(this)
  }
  protection() {
    return 1
  }
}
```

盾牌是一件简单的物品。除了瓦片和描述等元数据外，它所做的就是通过从 `protection()` 函数返回一个正值来为玩家增加防御加成。还记得我们之前制作的诅咒宝石吗？你可以制作一面诅咒盾牌，使其 `protection()` 返回负值，从而在每次攻击时对玩家造成额外伤害；或者制作一把诅咒锤子，既能造成额外伤害，又能对敌人造成巨量伤害。这样一来，玩家就会在“使用强力武器但承受额外伤害”与“使用较弱武器但获得更好防护”之间不断权衡。

实现矮人职业也很简单，代码位于 `classes/dwarf.js`。

```javascript
import dungeon from "../dungeon.js"
import BasicHero from "./basicHero.js"
import Axe from "../items/axe.js"
import Shield from "../items/shield.js"

export default class Dwarf extends BasicHero {
  constructor(x, y) {
    super(x, y)
    this.name = "矮人"
    this.movementPoints = 2
    this.actionPoints = 2
    this.healthPoints = 35
    this.tile = 61
    this.items.push(new Axe())
    this.toggleItem(0)
    this.items.push(new Shield())
    this.toggleItem(1)
    dungeon.initializeEntity(this)
  }
  refresh() {
    this.movementPoints = 2
    this.actionPoints = 2
  }
}
```

在构造函数中可以看到，矮人比战士移动的格数少，但拥有更高的生命值。他们还同时装备了斧头和盾牌，这为英雄提供了额外的防护层。操作这类角色与操作战士不同：战士偏好大胆的移动战术和多次攻击；而矮人更像一辆坦克，行动缓慢但势不可挡。

别忘了将新职业添加到 `classes.js` 中。

```javascript
import Warrior from "./classes/warrior.js"
import Dwarf from "./classes/dwarf.js"

const classes = {
  Warrior,
  Dwarf
}

export default classes
```

并修改世界场景以使用它。

```javascript
dungeon.player = new classes.Dwarf(15, 15)
```

下一个要实现的更复杂的原型是牧师。

### 创建牧师

牧师是奇幻题材中不断变化的职业。人们似乎普遍认同的是，牧师就像一名能够通过与某种神圣或神圣实体联系而获得治疗能力的战士。在游戏和奇幻世界中存在着一个牧师能力光谱，从拥有治疗能力的战士到基本上不参与战斗的中世纪奇幻医疗僧侣，这其中蕴含着广阔的解读和创意设计空间。

我们坚持专注于构建一个简单的游戏，因此我们的牧师将拥有其他职业不具备的一项独特能力：牧师每回合都会恢复少量生命值。这很简单，但非常有效。每回合，牧师会在 `refresh()` 期间恢复 1 点生命值。在多人游戏中，牧师应该拥有治疗他人的能力，但在这个游戏中没有其他英雄角色，因此自我治疗就足够了。

在许多古老的 TTRPG 游戏中，牧师被禁止使用锋刃武器。在我们的地下城中很难真正实现这一点，除非我们开始给通用物品类添加新属性来告知某个物品属于哪种武器类型。我们采取的方法是在构造牧师时为其提供一件非锋刃武器，然后让玩家在游戏过程中收集战利品时自行决定是否要更换为锋刃武器。

牧师默认的武器将是一把锤子；牧师示例的代码位于 `chapter-6/example-3-cleric`，锤子代码在 `items/hammer.js` 中。

```javascript
import GenericItem from "./genericItem.js"
import dungeon from "../dungeon.js"

export default class Hammer extends GenericItem {
  constructor(x, y) {
    super(x, y)
    this.tile = 933
    this.name = "一把战锤"
    this.description = "基础战锤。可造成 3 至 8 点伤害。"
    this.weapon = true
    dungeon.initializeEntity(this)
  }
  damage() {
    return Phaser.Math.Between(3, 8)
  }
}
```

它与剑和斧头属于同一种武器模板，但造成的伤害更高。

```javascript
import dungeon from "../dungeon.js"
import BasicHero from "./basicHero.js"
import Hammer from "../items/hammer.js"

export default class Cleric extends BasicHero {
  constructor(x, y) {
    super(x, y)
    this.name = "牧师"
    this.movementPoints = 3
    this.actionPoints = 2
    this.healthPoints = 40
    this.tile = 30
    this.items.push(new Hammer())
    this.toggleItem(0)
    dungeon.initializeEntity(this)
  }
```

正如我们所见，牧师的移动和攻击方式与战士相同，但拥有更多生命值，老实说，这使得他们比战士稍微好操作一些。

治疗动作将在 `refresh()` 函数内执行。如果玩家的生命值低于 40 点，他们将以每回合 1 点的速率开始恢复。如果玩家生命值过低，值得躲到地下城某个偏僻位置，不断按空格键来消耗回合进行治疗。

```javascript
refresh() {
    this.movementPoints = 3
    this.actionPoints = 2
    // 牧师每回合恢复少量生命值
    if (this.healthPoints < 40) {
      this.healthPoints += 1
      dungeon.log("牧师恢复 1 点生命值")
    }
  }
}
```

牧师的操作方式应与战士不同。尽管他们拥有战士职业的所有特性外加治疗能力，但操作牧师是一场“前进→后退治疗片刻→再次前进”的博弈。牧师职业更适合更危险的地下城，但用拥有种种限制的战士来清理地下城，对许多玩家来说也是一种挑战。

要尝试它，你需要像处理矮人那样，将其添加到 `classes.js` 中，并在世界场景中切换所使用的职业。

最后，我们将要实现一个拥有远程武器的职业；接下来登场的是精灵。

### 创建精灵族

精灵族是一种非常多变的原型，以至于很难精确定位他们在游戏玩法上的核心特征。与之前提及的原型类似，精灵族在奇幻作品中的呈现方式也呈现出爆炸式增长。我们将聚焦于一个灵活敏捷、擅长射箭的角色。

他们比其他职业角色更弱；这样玩家就会优先选择远程战斗，并对敌人过于靠近保持高度警惕。他们的默认武器是弓。由于这只是一个示例，我们赋予他们无限箭矢，但如果你想要更完善的设计，可以在物品上使用内部属性来记录玩家剩余的箭矢数量。

本示例的代码位于 `chapter-6/example-4-elf/`，新的弓位于 `items/bow.js`。

```javascript
import GenericItem from "./genericItem.js"
import dungeon from "../dungeon.js"
export default class Bow extends GenericItem {
constructor(x, y) {
super(x,y)
this.tile = 901
this.attackTile = 872
this.name = "一把弓"
this.description = "一把弓和箭矢。造成 1 到 3 点伤害。射程为四格。"
this.weapon = true
dungeon.initializeEntity(this)
}
damage() {
return Phaser.Math.Between(1, 3)
}
range() {
return 5
}
}
```

这是我们的第一件远程武器。它的射程为五格；这包括玩家所在的格子和敌人所在的格子。因此，拥有五格射程意味着只要你和敌人之间最多有三格空位，你就可以向敌人射击。

精灵族类位于 `classes/elf.js`。

```javascript
import dungeon from "../dungeon.js"
import BasicHero from "./basicHero.js"
import Bow from "../items/bow.js"
export default class Elf extends BasicHero {
constructor(x, y) {
super(x, y)
this.name = "精灵"
this.movementPoints = 4
this.actionPoints = 3
this.healthPoints = 20
this.tile = 56
this.items.push(new Bow())
this.toggleItem(0)
dungeon.initializeEntity(this)
}
refresh() {
this.movementPoints = 4
this.actionPoints = 3
}
}
```

精灵族比其他职业更灵活，每回合可以移动四格。这让玩家可以靠近敌人、攻击，然后撤离，基本上形成了一种游击式的地牢探险风格。每回合拥有三点行动点，精灵每回合可以射击三次，使其能够在敌人行动前攻击（如果幸运的话还可以击杀）多个目标。

本章的最后一个角色职业将是巫师。

### 创建巫师

巫师种类繁多，以至于我们有不同的名称来指代他们——术士、咒术师等——但在我们的游戏中，我们将回归本源。受角色扮演游戏的启发，我们的巫师将是一位复古风格的睿智英雄，使用卷轴来记忆和施放法术。

本质上，这些卷轴就是远程武器。它们会以卷轴的形式出现在玩家的用户界面中，它们的攻击图块看起来像法术，但在内部，它们的行为将与精灵族的弓完全一致。巫师的代码位于 `chapter-6/example-5-wizard/`。

在实现第一个卷轴之前，我们需要做一些准备工作。在我们的精灵图集图形中，卷轴的图片并不多。它们都是相同的颜色，这会让玩家难以分辨该装备哪一个，因为它们看起来都一样。为了解决这个问题，我们将在游戏中引入精灵染色功能。这允许我们用选择的任意颜色覆盖精灵的颜色。这需要修改地牢模块和基础英雄。

让我们从修改基础英雄开始。我们需要做的是双重检查添加到界面的物品是否具有 `tint` 属性，如果有，则在精灵中使用该属性。在 `refreshUI` 函数内部，我们将第一个 `if` 子句更改为：

```javascript
if (!item.UIsprite) {
let x = this.UIitems[i].x + 10
let y = this.UIitems[i].y + 10
item.UIsprite = this.UIscene.add.sprite(x, y, "tiles", item.tile)
if (item.tint) {
item.UIsprite.tint = item.tint
item.UIsprite.tintFill = true
}
}
```

有趣的新增内容是检查 `tint` 值并应用它的内部 `if` 子句。

在地牢模块的 `initializeEntity` 函数中也需要进行类似的更改，以便染色的精灵在地牢和玩家用户界面中显示相同的颜色。

```javascript
initializeEntity: function (entity) {
if (entity.x && entity.y) {
let x = this.map.tileToWorldX(entity.x)
let y = this.map.tileToWorldY(entity.y)
entity.sprite = this.scene.add.sprite(x, y, "tiles", entity.tile)
entity.sprite.setOrigin(0)
if (entity.tint) {
entity.sprite.tint = entity.tint
entity.sprite.tintFill = true
}
}
},
```

我们还应该为用于远程攻击的攻击精灵染色。为此，我们将修改地牢模块中的 `attackEntity` 函数。由于该函数不知道攻击中使用了什么物品，我们需要向其调用传递另一个参数，即要使用的颜色值。与远程攻击参数类似，此颜色值是可选的，默认设置为 `false`。

```javascript
attackEntity: function (attacker, victim, rangedAttack = false, tint = false) {
```

可选的 `tint` 将仅用于远程攻击，因此我们在为此类攻击创建精灵后添加以下代码行：

```javascript
const sprite = dungeon.scene.add.sprite(x, y, "tiles", rangedAttack).setOrigin(0)
if (tint) {
sprite.tint = tint
sprite.tintFill = true
}
```

仅仅在那里声明该参数是不够的；我们必须从基础英雄类的攻击处理例程中使用它。因此，在 `processInput` 和 `processTouchInput` 中对 `attackEntity` 的所有调用都需要修改以携带 `tint` 参数。

```
processTouchInput(event) {
let x = dungeon.map.worldToTileX(event.worldX)
let y = dungeon.map.worldToTileY(event.worldY)
let entity = dungeon.entityAtTile(x, y)
if (entity && entity.type == "enemy" && this.actionPoints > 0) {
const currentWeapon = this.currentWeapon()
const rangedAttack = currentWeapon.range() > 0 ? currentWeapon.attackTile || currentWeapon.tile : false
const tint = currentWeapon.tint || false
const distance = dungeon.distanceBetweenEntities(this, entity)
if (rangedAttack && distance <= currentWeapon.range()) {
dungeon.attackEntity(this, entity, rangedAttack, tint)
this.actionPoints -= 1
}
}
}
```

在 `processInput` 中，我们使用了完全相同的更改，创建了一个 `tint` 常量并将其传递给 `attackEntity`。

这些变化将为实体创建带来许多机会，尤其是在下一章开始程序化生成之后。现在让我们来实现第一个法术——火球术，其代码位于`items/scrolloffireball.js`中。

```
import GenericItem from "./genericItem.js"
import dungeon from "../dungeon.js"
export default class ScrollOfFireball extends GenericItem {
constructor(x, y) {
super(x,y)
this.tile = 881
this.tint = 0xdd0000
this.attackTile = 335
this.name = "A Scroll of Fireball"
this.description = "A scroll of fireball. Causes between 1 and 4 damage. Range is four tiles."
this.weapon = true
dungeon.initializeEntity(this)
}
damage() {
return Phaser.Math.Between(1, 4)
}
range() {
return 4
}
}
```

火球术的实现与弓类似，本质上是一个简单的远程武器。我们在构造函数中使用`tint`属性为其添加了红色色调，使其与我们即将构建的其他法术卷轴在外观上有所不同。火球术威力强大，但射程有限。另一种射程更远的法术是闪电术，其代码位于`items/scrolloflightning.js`中。

```
import GenericItem from "./genericItem.js"
import dungeon from "../dungeon.js"
export default class ScrollOfLightning extends GenericItem {
constructor(x, y) {
super(x,y)
this.tile = 881
this.tint = 0x0022ff
this.attackTile = 413
this.name = "A Scroll of Lightning"
this.description = "A scroll of Lightning. Causes between 1 and 2 damage. Range is seven tiles."
this.weapon = true
dungeon.initializeEntity(this)
}
damage() {
return Phaser.Math.Between(1, 2)
}
range() {
return 7
}
}
```

代码与火球术几乎相同，但这是一个威力较弱、射程更远的法术。玩家操控的法师需要在怪物接近时切换使用这些法术。现在让我们实现最后一个药水——治疗药水，其代码位于`items/healthPotion.js`中。

```
import GenericItem from "./genericItem.js"
import dungeon from "../dungeon.js"
export default class HealthPotion extends GenericItem {
constructor(x,y) {
super(x,y)
this.tile = 761
this.name = "Health Potion"
this.description = "A potion that cures between 3 and 5 health points when."
dungeon.initializeEntity(this)
}
equip(itemNumber) {
const points = Phaser.Math.Between(3, 5)
dungeon.log(`A warm feeling is felt when drinking the potion as it restores ${points} health points.`)
dungeon.player.healthPoints += points
dungeon.player.removeItem(itemNumber)
}
}
```

我们会给法师提供几瓶治疗药水。地牢是危险的地方，法师应该尽可能装备齐全再出发。说到法师，让我们实现`classes/wizard.js`。

```
import dungeon from "../dungeon.js"
import BasicHero from "./basicHero.js"
import ScrollOfFireball from "../items/scrolloffireball.js"
import ScrollOfLightning from "../items/scrolloflightning.js"
import HealthPotion from "../items/healthPotion.js"
export default class Wizard extends BasicHero {
constructor(x, y) {
super(x, y)
this.name = "Wizard"
this.movementPoints = 3
this.actionPoints = 1
this.healthPoints = 20
this.tile = 88
this.items.push(new ScrollOfFireball())
this.items.push(new ScrollOfLightning())
this.items.push(new HealthPotion())
this.items.push(new HealthPotion())
this.toggleItem(1)
dungeon.initializeEntity(this)
}
refresh() {
this.movementPoints = 3
this.actionPoints = 1
}
}
```

法师很脆弱，如果怪物能够成功攻击他们，很容易被杀死。他们的移动点数不多，因此玩家需要巧妙（加上一点运气）地移动。为了提高法师的生存几率，我们给了他们两瓶治疗药水。装备它们能回复不少生命值，但和圣水一样，它们是消耗品。

将法师添加到`classes.js`中，并修改在游戏场景中初始化的职业即可使用该职业进行游玩。

## 练习

你能创建更多角色职业吗？猫如何作为英雄使用？也许它不能攻击，所有玩法基于躲避和潜行，而且在永久死亡真正降临之前，猫有九条命。那如果是一只魔法猫呢？你可以给猫一些卷轴，让它变得相当危险。精灵图上有一个猫的图块，仅供参考。

### 总结

至此，我们完成了本章开头设定的所有基础角色类型。为了实现这些角色职业，我们不得不对部分游戏代码进行了修补，但现在我们有了一个非常灵活的框架，可以在创建物品、怪物和英雄时让创造力自由驰骋。

理解每个角色原型如何基于`genericItem.js`和`basicHero.js`构建，并通过重写其属性和函数来创造玩法各异、体验丰富的英雄，这一点很重要。

到目前为止，我们实现的都是构建 roguelike 游戏所必需的基础游戏功能。目前，游戏还不太像 roguelike，我们缺少了最关键的元素：通过程序化生成带来的新鲜游戏体验。在下一章中，我们将通过向地牢中添加程序化生成的实体来开启这一进程。

脚注 1   2