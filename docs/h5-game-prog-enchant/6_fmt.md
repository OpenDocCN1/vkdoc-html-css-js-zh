# 六、创建街机射击游戏

在第五章中，我们研究了游戏设计并创造了我们自己的打地鼠(或者我应该说是打地鼠机器人？)游戏。在这一章中，我们来看另一个游戏，一个比打地鼠更复杂的街机射击游戏。

在街机射击游戏中，我们包括几个有助于有趣游戏的功能:背景音乐，不可玩的角色(如好人和坏人)，爆炸，滚动背景，等等。将所有这些元素整合在一起需要在游戏设计方面进行仔细的规划，因为我们创建的所有类都必须相互交互。例如，从玩家的船上发射的子弹应该会让敌人消失。除了介绍街机射击游戏是如何设计和编码的，我们还介绍了几个新的主题，包括指示器、关卡和旋转。

汇总列表

1.  探索游戏和设置
2.  为游戏奠定基础
3.  创建玩家的船
4.  创建拍摄类
5.  创建 `PlayerShoot` 类并使船射击
6.  制造敌人
7.  创建敌人射击类并让敌人射击
8.  让敌人呈弧形移动
9.  爆炸
10. 添加滚动背景
11. 添加生活量表

## 建造街机射击游戏

让我们从我们能制作的最简单的街机射击游戏开始，然后逐步增加复杂度。本节我们编写的游戏是一个简单的射击游戏。当玩家触摸屏幕时，飞船会移动到触摸的位置，只要用户触摸它，飞船就会发射子弹穿过屏幕。敌人的船出现在屏幕的对面，向玩家的船射击。如果玩家的船被击中，游戏结束。玩家每消灭一艘敌舰，分数就会增加。游戏的目标是在被击中之前获得尽可能高的分数。

在第五章中，我们的方法是写一小段简单的代码，然后在开发过程中不断改进，在每一次迭代中玩游戏。我们在这里也是这样做的，但是因为街机射击游戏比打地鼠游戏更复杂，所以有更长的步骤。

## 探索游戏，设置

如果你在我们开始编码之前玩了我们将要编码的游戏，你会对游戏的工作原理有更好的了解。执行以下操作来熟悉游戏，并设置您将在其中编码的环境:

1.  在`http://9leap.net/games/1034`玩几次游戏，感受一下我们将要制作的游戏。特别注意敌人是如何随机出现的，当屏幕上有很多敌人子弹时，游戏会变得多么困难。这款游戏因其重玩价值而在`9leap.net`上广受欢迎。由于随机产生的敌人，每次的体验都不一样。
2.  在`http://code.9leap.net/codes/show/29839`叉模板。该模板包含您需要的必要图像文件。

## 为游戏打基础

和往常一样，最好先从一个游戏最简单的元素开始，然后再加入更复杂的元素。在这里，我们设置了`Core`对象，设置了背景，并创建了一个乐谱。执行以下操作进行设置:

1.  Initialize the `enchant.js` library and create the `Core` entity by copying the code in Listing 6-1 into the blank template. Because of the complexity of this section, we label sections of code with comments so we can refer to them later in the chapter.

    ### 清单 6-1。街机射手的基础

    ```js
    enchant();
    //Class Definitions

    window.onload = function() {
        game = new Core(320, 320);
        //Game Properties
        game.fps = 24;

        game.onload = function() {
        };

        game.start();
    };
    ```

2.  Create a black background by typing the code in Listing 6-2 into the `game.onload` function. This specifies the `backgroundColor` of `rootScene` to be black.

    ### 清单 6-2。创建黑色背景

    ```js
    //In-Game Variables and Properties
    game.rootScene.backgroundColor = 'black';
    ```

    `rootScene`的`backgroundColor`属性可以接受所有标准颜色的名称和颜色的十六进制值(例如，`#FF0000`代表红色)。

3.  单击运行。屏幕应该会变黑。

4.  Create a game variable for the player’s score by adding the code in Listing 6-3 to the `//Game Properties` section. Later, we'll make this variable increase in value whenever enemies are hit by bullets from the player’s ship.

    ### 清单 6-3。创建分数变量

    ```js
    game.score = 0;
    ```

5.  Create a `ScoreLabel` and add it to `rootScene` by adding the code in Listing 6-4 to the `//In-game Variables and Properties` section. The `(8,8)` specifies the top-left corner of the label to be placed 8 pixels to the right and 8 pixels down from the top-left corner of the game. Later, we’ll update the value of the `ScoreLabel` with the value of `game.score` every frame.

    ### 清单 6-4。为 `rootScene` 创建并添加分数标签

    ```js
    scoreLabel = new ScoreLabel(8, 8);
    game.rootScene.addChild(scoreLabel);
    ```

    `ScoreLabel`类是名为`ui.enchant.js`的插件的一部分，该插件包含在`enchantjs.com`的下载包中。如果你早点从 `code.9leap.net` 接手这个项目的话，它也包括在内。如果没有，在继续之前，您需要确保行`<script src='/static/enchant.js-latest/plugins/ui.enchant.js'></script>`被添加到您的`index.html`文件中。

6.  单击运行。`ScoreLabel`应出现在屏幕顶部，并带有单词“`SCORE:`”。

如果您的代码遇到任何问题，您可以在`http://code.9leap.net/codes/show/29841`找到一个完整的工作示例。

## 创建玩家的船

下一步是创建玩家的船，我们将创建一个类。执行以下操作来创建它:

1.  Create a class definition for the player by entering the code in Listing 6-5 to the `//Class Definitions` section. For a refresher, the `enchant.Sprite` declaration creates the new class as an extension of the `Sprite` class, which means all properties and methods of the `Sprite` class will work on the `Player` class as well. Everything in the `initialize` function will be run when a `Player` object is created.

    ### 清单 6-5。玩家类

    ```js
    // Player class
    var Player = enchant.Class.create(enchant.Sprite, {
        initialize: function(x, y){

        }
    });
    ```

2.  Preload `graphic.png`, which contains all the images used in this game, by adding the code in Listing 6-6 to the `//Game Properties` section. Figure 6-1 shows the graphic.

## 预加载图像

***清单 6-6。*** 预加载图像

```js
game.preload('graphic.png');
```

![9781430247432_Fig06-01.jpg](img/9781430247432_Fig06-01.jpg)

图 6-1 。Graphic.png

## 定义玩家类

3.  Define the `Player` class as a 16x16 instance of the `Sprite` class and specify `graphic.png` as its image by entering the code in Listing 6-7 into the `initialize` function of the `Player` class.

    ***清单 6-7。*** 指定尺寸和图像

    ```js
    enchant.Sprite.call(this, 16, 16);
    this.image = game.assets['graphic.png'];
    ```

4.  Make the location of `Player` to be whatever was specified when it was created and set the `frame` by adding the code in Listing 6-8 directly under what you just added.

    ***清单 6-8。*** 设置位置和帧

    ```js
    this.x = x;
    this.y = y;
    this.frame = 0;
    ```

5.  Create a variable to keep track of when the screen is being touched by adding the code in Listing 6-9 to `//Game Properties`. This will set in the touch event listeners we’ll create next, and will be used to determine if bullets should be fired from the ship later.

    ***清单 6-9。*** 跟踪触摸

    ```js
    game.touched = false;
    ```

6.  Back in the `initialize` function, add an event listener to move the `Player` to Y position of the touch event when a `touchstart` event occurs by entering the code in Listing 6-10. When the `touchstart` event occurs, we also set the `game.touched` variable to `true`. Notice how we don’t need to put a line break between `player.y = e.y;` and `game.touched = true;`. The semicolons delineate the commands.

    ***清单 6-10。*** 添加 Touchstart 事件监听器

    ```js
    game.rootScene.addEventListener('touchstart',
            function(e){ player.y = e.y; game.touched = true; });
    ```

7.  Below that, add event listeners for both `touchend` and `touchmove` events to take care of all possible interaction from the player by adding the code in Listing 6-11.

    ***清单 6-11。*** 附加事件监听器

    ```js
    game.rootScene.addEventListener('touchend',
            function(e){ player.y = e.y; game.touched = false; });
    game.rootScene.addEventListener('touchmove',
            function(e){ player.y = e.y; });
    ```

8.  Create an instance of the `Player` class and add it to `rootScene` by entering the code in Listing 6-12 directly below what you just added, still inside the `initialize` function.

    ***清单 6-12。*** 将`Player`添加到`rootScene`

    ```js
    game.rootScene.addChild(this);
    ```

9.  Create an instance of `Player` by adding the code in Listing 6-13 to the `//In-game Variables and Properties` section. The instance of `Player` is automatically added to `rootScene` because of the last step.

    ***清单 6-13。*** 创建`Player` 的实例

    ```js
    player = new Player(0, 152);
    ```

10.  Click Run. The ship appears on the screen. If you click and hold, the ship will follow your cursor up and down on the screen.

    如果您在本节中遇到问题，可以在`http://code.9leap.net/codes/show/29845`找到一个工作代码示例。

## 创建拍摄类

这个游戏中会出现两种弹药:玩家船射的弹药和敌人射的弹药。这两种类型使用相同的图像，所以我们将从创建一个通用的`Shoot`类开始。执行以下操作来创建它:

1.  Create the basic `Shoot` class by entering the code in Listing 6-14 beneath the `//Player Class` definition. It should have two functions: `initialize` and `remove`. It should also extend the `Sprite` class. When an instance of the `Shoot` class is created, we will pass three values to it: an X coordinate, a Y coordinate, and a direction.

    ***清单 6-14。*** 创建射击类

    ```js
    // Shoot class
    var Shoot = enchant.Class.create(enchant.Sprite, {
        initialize: function(x, y, direction){
        },
        remove: function(){
        }
    });
    ```

2.  Create the `Shoot` class as a 16x16 instance of the `Sprite` class, and create and assign necessary variables by entering the code in Listing 6-15 into the initialize function of the `Shoot` class. We will use the `moveSpeed` variable next to control the speed of movement and allow for easy modification of ammunition speed later on.

    ***清单 6-15。*** `Shoot`类的实例变量

## 用 Cos 和 Sin 控制方向

我们创建了`Shoot`类来接受方向，但是我们还不知道传递什么样的值来指示方向。我们也有一个事件监听器来处理运动，但是我们还不知道如何处理偏离方向的运动。我们如何做到这一点？具有功能`cosine`和`sine`，通常简称为`cos`和`sin`。

要理解这些功能，首先需要理解单位圆的方向，如图图 6-2 所示。

![9781430247432_Fig06-02.jpg](img/9781430247432_Fig06-02.jpg)

图 6-2 。单位圆

单位圆用弧度表示方向。一弧度等于一个圆的半径的长度，在它的圆周上排成一行。圆圈中的`pi`符号是一个数学常数，等于一个圆的周长除以其直径(约 3.14)。这有什么关系？因为 cos 和 sin 只接受以弧度表示的值，而左右主方向分别等于`pi`和`0`(或`2 * pi`)。

假设你想从圆心画一条长度为 1 的线。`Cos`和`sin`将分别给出该线端点的 x 和 y 坐标，根据`pi`给出一个方向。例如，如果我们想向点的右边移动 1，我们可以将`pi`传递给`cos`来找出我们需要沿着 x 轴移动多少才能到达那里(-1)。我们还将通过`pi`到`sin`来找出我们需要沿着 y 轴(0)移动多少。

代码就是这样处理运动的。我们根据`pi`选择一个方向，然后使用`cos`和`sin`来计算每一帧精灵移动多少。

```js
enchant.Sprite.call(this, 16, 16);
this.image = game.assets['graphic.png'];
this.x = x;
this.y = y;
this.frame = 1;
this.direction = direction;
this.moveSpeed = 10;
```

3.  Create an `enterframe` event listener to control movement by entering the code in Listing 6-16 directly beneath `this.movespeed = 10;`. Code entered here will be run on instances of `Shoot` every frame.

    `清单 6-16。` 创建一个`enterframe`事件监听器

    ```js
    this.addEventListener('enterframe', function(){
            });
    ```

我们将在下一节中向这个事件监听器添加代码。

*   4.  Inside the event listener you just created, specify how instances of the `Shoot` class should move based off `cos` and `sin` by entering the code in Listing 6-17. Multiplying the results of the calculations by `moveSpeed` allows the ammunition to be manipulated in terms of speed later, if needed.

    `清单 6-17。` 用`Cos`和`Sin` 控制移动

    ```js
    this.x += this.moveSpeed * Math.cos(this.direction);
    this.y += this.moveSpeed * Math.sin(this.direction);
    ```

*   5.  Designate instances of the `Shoot` class to call the `remove` function if the shots stray far outside the bounds of the game screen by entering the code in Listing 6-18 inside the `enterframe` event listener, below what you just added. We could use 0 for the minimum allowed values of X and Y before the `remove` function is called, but using `–this.width` and `–this.height` ensures the shots don’t disappear off the screen unnaturally. Do not worry about defining what the `remove` function does just yet.

    `清单 6-18。` 调用`remove`函数

    ```js
    if(this.y > 320 || this.x > 320 || this.x < -this.width || this.y < -this.height){
                this.remove();
    }
    ```

*   6.  Inside the definition of the `remove` function, under the definition of the `initialize` function, specify what should happen when the `remove` function is called by entering the code in Listing 6-19. The `delete` command removes a given instance of the `Shoot` class from memory. In a very long game, if this is not specified it could bog down the system.

    `清单 6-19。` `remove`功能

    ```js
    game.rootScene.removeChild(this);
    delete this;
    ```

*   7.  Add instances of the `Shoot` class to `rootScene` on creation by adding the code in Listing 6-20 to the `initialize` function, after the event listener.

    `清单 6-20。` 将`Shoot`添加到`rootScene`

    ```js
    game.rootScene.addChild(this);
    ```

## 创建 PlayerShoot 类并让船射击

我们创建了通用的`Shoot`类，但是现在我们需要为玩家的船发射的弹药创建一个类，并在船发射时创建它的实例。执行以下操作来创建该类:

1.  In the `//Class Definitions` section, create the `PlayerShoot` class and its `initialize` function by entering the code in Listing 6-21.

    `清单 6-21。` 创建 PlayerShoot 类

    ```js
    // PlayerShoot class
    var PlayerShoot = enchant.Class.create(Shoot, { // Succeeds bullet class
        initialize: function(x, y){
        }
    });
    ```

2.  Create the `PlayerShoot` class as an instance of the `Shoot` class, specifying 0 as the direction, by inserting the code in Listing 6-22 into the `initialize` function. Remember the unit circle? The value of 0 is equal to a direction facing the right side of the screen. Because the ship is on the left side of the screen, bullets fired will head toward the right.

    `清单 6-22。` 创建`Shoot`类的实例

    ```js
    Shoot.call(this, x, y, 0);
    ```

## 让飞船发射子弹

3.  我们现在需要让飞船发射`PlayerShoot`类的实例。回到`Player`类定义，在`initialize`函数中创建一个`enterframe`事件监听器，将清单6-23中的代码输入到`game.rootScene.addChild(this);`这一行的上方。

    **清单 6-23** Enterframe 事件监听器

    ```js
    this.addEventListener('enterframe', function(){
    });
    ```

4.  在这个事件监听器内部，添加清单6-24中的代码，创建一个`if`语句，该语句每三帧执行一次，并且仅在游戏被触摸时执行。

    **清单 6-24** If 语句控制出手

    ```js
    if(game.touched && game.frame % 3 === 0){
    }
    ```

5.  在`if`语句内部，通过输入清单6-25中的代码来创建一个新的`PlayerShoot`类实例。

    **清单 6-25** 创建一个`PlayerShoot`实例

    ```js
    var s = new PlayerShoot(this.x, this.y);
    ```

6.  单击运行。当你点击屏幕时，你的船会发射弹药穿过屏幕。

如果您在本节中遇到问题，您可以在`http://code.9leap.net/codes/show/29907`找到一个完整的工作代码示例。

## 创造敌人

我们的船现在可以发射子弹，但现在我们需要为船创造一些可以射击的东西。执行以下操作为敌人创建一个类并将他们添加到游戏中:

1.  通过将清单6-26中的代码添加到`//Class Definitions`部分（位于`Player`类定义下方），创建一个包含`initialize`和`remove`函数的基础`Enemy`类定义。

    **清单 6-26** 基础`Enemy`类

    ```js
    //Enemy class
    var Enemy = enchant.Class.create(enchant.Sprite, {
        initialize: function(x, y){
        },
        remove: function(){
        }
    });
    ```

2.  通过将清单6-27中的代码输入到`initialize`函数中，使`Enemy`类成为一个16x16的`Sprite`类实例，并为`frame`、`x`和`y`变量赋值。

    **清单 6-27** 制作 Sprite 实例并赋值变量

    ```js
    enchant.Sprite.call(this, 16, 16);
    this.image = game.assets['graphic.png'];
    this.x = x;
    this.y = y;
    this.frame = 3;
    ```

3.  通过将清单6-28中的代码输入到你刚刚输入的内容下方，指定敌人的移动方向为单位圆（`Math.PI`）的左侧，并创建一个用于移动速度的变量。

    **清单 6-28** 为方向和运动创建变量

    ```js
    this.direction = 0;
    this.moveSpeed = 3;
    ```

4.  在你刚刚输入的内容下方，通过输入清单6-29中的代码，创建一个事件监听器，使用你刚刚创建的变量来移动敌人。

    **清单 6-29** 移动敌人

    ```js
    // Define enemy movement
    this.addEventListener('enterframe', function(){
            this.x -= this.moveSpeed * Math.cos(this.direction);
            this.y += this.moveSpeed * Math.sin(this.direction);
    });
    ```

5.  通过将清单6-30中的代码直接输入到`this.y += this.moveSpeed * Math.sin(this.direction);`这一行下方，使`Enemy`在超出屏幕范围时调用`remove`函数（我们稍后会定义）。

    **清单 6-30** 除去屏幕外的敌人

    ```js
    // Disappear when outside of screen
    if(this.y > 320 || this.x > 320 || this.x < -this.width || this.y < -this.height){
            this.remove();
    }
    ```

6.  最后，通过将清单6-31中的代码输入到`initialize`函数中（位于你刚刚添加的事件监听器下方），让`Enemy`在创建时将自身添加到`rootScene`。

    **清单 6-31** 给 rootScene 添加敌人

    ```js
    game.rootScene.addChild(this);
    ```

7.  通过将清单6-32中的代码输入到`remove`函数中来定义`remove`函数。这将把敌人从`rootScene`中移除并从内存中删除。同时，它也会将敌人从我们即将创建的用于跟踪敌人的数组中移除。（因此有`delete enemies[this.key];`，这将在稍后解释。）

    **清单 6-32** 清除功能

    ```js
    game.rootScene.removeChild(this);
    delete enemies[this.key];
    delete this;
    ```

8.  在`//In-Game Variables and Properties`部分，通过输入清单6-33中的代码来创建一个用于跟踪敌人的数组。

    **清单 6-33** 制造敌阵

    ```js
    enemies = [];
    ```

9.  在新数组下，为游戏创建一个`enterframe`事件监听器，通过输入清单6-34中的代码来随机生成敌人。我们在`Enemy`内部创建一个变量`enemy.key`，并将游戏的当前帧赋值给它，因为这为我们提供了一个跟踪敌人的依据。如果不这样做，我们就无法在之后引用特定的敌人，而这在敌人偏离屏幕或被飞船的弹药击中时是必需的。敌人被创建的概率大约是十分之一，这是因为`if(Math.random()*100 < 10)`，如果被创建，则会随机放置在y轴上。

    ***清单 6-34。*** 在游戏中制造敌人

    ```js
    game.rootScene.addEventListener('enterframe', function(){
        if(Math.random()*100 < 10){
            var y = Math.random() * 320;
            var enemy = new Enemy(320, y);
            enemy.key = game.frame;
            enemies[game.frame] = enemy;
        }
    });
    ```

10.  通过在`if`语句下方（但仍在事件监听器内）输入清单6-35中的代码，每帧更新游戏的`scoreLabel`。

    ***清单 6-35。*** 每帧更新游戏分数

    ```js
    scoreLabel.score = game.score;
    ```

11.  点击运行。敌人被创造出来并飞过屏幕。然而，当船上的子弹击中他们时，什么也没有发生。

12.  通过在`PlayerShoot`类定义中（在`Shoot.call(this, x, y, 0);`下方）输入清单6-36中的代码，让飞船的弹药通过事件监听器摧毁敌人。这个`for`循环的构造方式使得程序会遍历`enemies`数组的每一个成员，检查给定的子弹是否与其接触。如果是，则数组中的子弹和敌人都会被移除，并且玩家的分数增加100。

    ***清单 6-36。*** 制伏敌人

    ```js
    this.addEventListener('enterframe', function(){
        // Judges whether or not player's bullets have hit enemy
        for(var i in enemies){
            if(enemies[i].intersect(this)){
                // Eliminates enemy if hit
                this.remove();
                enemies[i].remove();
                //Adds to score
                game.score += 100;
            }
        }
    });
    ```

13.  单击运行。你现在可以通过射击来消灭敌人。如果您在本节中遇到问题，可以在`http://code.9leap.net/codes/show/29929`找到一个工作代码示例。

## 创建敌人射击类并让敌人射击

在这一点上，没有办法输掉比赛，这不是很引人注目。让我们创建一个敌人弹药的职业，让敌人向飞船开火，通过以下方式增加难度:

1.  通过在`//Class Definitions`部分添加清单6-37中的代码来创建`enemyShoot`类。将其创建为`Shoot`类的一个实例，方向设置为`Math.PI`，因为在单位圆中这表示向左。

    ***清单 6-37。*** 排敌班

    ```js
    // Class for enemy bullets
    var EnemyShoot = enchant.Class.create(Shoot, { // Succeeds bullet class
        initialize: function(x, y){
            Shoot.call(this, x, y, Math.PI);
        }
    });
    ```

2.  通过在`Shoot.call`开头的行下方添加清单6-38中的代码，在`initialize`函数内添加一个`enterframe`事件监听器。该事件监听器应包含一个`if`语句，指定如果`player`的中心与给定敌人子弹的中心在任何时刻的距离为8像素或更小，则游戏应结束。

    ***清单 6-38。*** 指定`playerShoot`和`player` 之间的点击次数

    ```js
    this.addEventListener('enterframe', function(){
        if(player.within(this, 8)){
            game.end(game.score, "SCORE: " + game.score);
        }
    });
    ```

3.  通过将`Enemy enterframe`事件监听器内的`if`语句更改为清单6-39所示的内容，使`Enemy`类每10帧创建一次`enemyShoot`类的实例。变量`age`可以在任何`Entity`上调用，并给出该`Entity`存活的帧数。

    ***清单 6-39。*** 制敌射击

# 使敌人沿弧线移动

敌人现在沿直线移动。这使得玩游戏相当直接，但有点简单。让我们让敌人以弧线移动，让游戏更有趣。我们将通过创建一个变量来指定方向应该向上运动还是向下运动(`theta`)，这取决于敌人在哪里被创建，然后在每一帧稍微改变方向。为此，请执行以下操作:

1.  每个敌人需要有一个变量来改变其方向。通过将`initialize`函数的起始行更改为与清单 6-40 中的代码匹配，创建一个变量(`theta`)，该变量在创建`Enemy`时作为参数传递。

    **清单 6-40。** 添加了`theta`变量

    ```js
    initialize: function(x, y, theta){
    ```

2.  虽然方向是用单位圆指定的，但对于除`0`或`Math.PI`以外的特定量，使用度数更容易。我们将向`theta`传递一个度数值，因此请在读取`this.direction = 0;`的行之前输入清单 6-41 中的代码，将其转换为弧度。

    **清单 6-41。** 将`theta`转换成弧度

    ```js
    this.theta = theta * Math.PI / 180;
    ```

3.  在`Enemy`类的`enterframe`事件监听器内，通过在读取`this.addEventListener('enterframe', function(){`的行正下方输入清单 6-42 中的代码，使`theta`每帧改变`Enemy`的方向。

    **清单 6-42。** 递增`Enemy`方向

    ```js
    this.direction += this.theta;
    ```

4.  现在敌人可以接受`theta`的值，这将每帧改变`Enemy`的方向，但我们需要指定敌人是如何创建的才能真正使用它。为此，将游戏`rootScene enterframe`事件监听器内的`if`语句更改为与清单 6-43 中的代码匹配。如果`Enemy`在屏幕的上半部分创建，敌人将向上弧线移动（方向角每帧增加1）。如果在下半部分创建，它将向下弧线移动（方向角每帧减少1）。

    **清单 6-43。** 制造敌人那道弧线

    ```js
    if(rand(100) < 10){
            // Make enemies appear randomly
            var y = rand(320);
            if (y < 160) {
                    theta = 1;
            } else {
                    theta = -1;
            }
            var enemy = new Enemy(320, y, theta);
            enemy.key = game.frame;
            enemies[game.frame] = enemy;
    }
    ```

5.  点击运行。敌人以弧线移动，使游戏更具吸引力。您的游戏应如图 6-3 所示。

    ![9781430247432_Fig06-03.jpg](img/9781430247432_Fig06-03.jpg)

    图 6-3 。简单的射击游戏

如果您在本节中遇到问题，可以在`http://code.9leap.net/codes/show/30564`找到一个工作代码示例。

# 强化游戏

目前，我们的游戏在目前的状态下是一个工作游戏。我们可以把它留在这里，转到另一个游戏，但让我们研究一些方法，通过添加一些功能来使游戏更引人注目。

我们将对原来的游戏做一些补充，并在接下来的部分解释如何做。

## 爆炸

先来加点爆款。目前，当敌舰被击落时，它们就消失了。让我们通过在敌舰被击中时引起爆炸来增加一点刺激。

1.  从`http://enchantjs.com/assets/img/effect0.gif`下载爆炸（图 6-4）精灵表并将其添加到您的项目中。我们将使用这个精灵表来制作爆炸效果。

    ![9781430247432_Fig06-04.jpg](img/9781430247432_Fig06-04.jpg)

    图 6-4 。爆炸效果图

2.  创建一个基本类，称为`Blast`，作为`Sprite`类的扩展，通过将清单 6-44 中的代码添加到`//Class Definitions`部分。该类应有一个`initialize`和`remove`函数。

    **清单 6-44。** 爆炸类

    ```js
    // Class for explosions
    var Blast = enchant.Class.create(enchant.Sprite, {
        initialize: function(x, y){
        },
        remove: function(){
        }
    });
    ```

3.  在`initialize`函数内，将`Blast`类创建为`Sprite`类的16x16实例，并通过输入清单 6-45 中的代码将`x`和`y`参数传递给局部变量`x`和`y`。

    **清单 6-45。** 在爆炸中分配变量

### 4. 跳转到 `window.onload` 函数，在 `game.preload('graphic.png');` 下方添加一个预加载语句，通过输入清单 6-46 中的代码来添加爆炸的精灵图。

    `清单 6-46`。 预载爆炸图像

    ```js
    game.preload('effect0.gif');
    ```

### 5. 回到 `Blast` 类的 `initialize` 定义中，在 `this.y = y;` 下方，添加一条语句，通过输入清单 6-47 中的代码，将 `effect0.gif` 用作爆炸的图像。

    `清单 6-47`。 指定 `effect0.gif` 为 `Blast` 图像

    ```js
    this.image = game.assets['effect0.gif'];
    ```

### 6. 指定 `frame` 从 0 开始，并通过在下一行输入清单 6-48 来指定持续时间为 20 帧。我们稍后将使用持续时间来在特定时间内绘制动画。

    `清单 6-48`。 指定起始帧和持续时间

    ```js
    this.frame = 0;
    this.duration = 20;
    ```

### 7. 在那下面，通过输入清单 6-49 中的代码创建一个 `enterframe` 事件监听器。我们将使用这个事件监听器来控制爆炸的 `frame`。

    `清单 6-49`。 事件监听器为 `Blast`

    ```js
    this.addEventListener('enterframe', function(){
    }
    ```

### 8. 在事件监听器内部，创建一条语句，通过输入清单 6-50 中的代码，将爆炸的 `frame` 设置为在 20 帧的时间内从 0 变到 4。这是通过一个算法实现的。该算法首先获取爆炸已存活的当前帧数（`this.age`），并将其除以动画的期望持续时间（`this.duration`），得到一个表示爆炸应处于动画序列中哪个位置的分数。然后将其乘以 5，因为精灵图中显示的总序列长度为 5 帧。此时，结果很可能有一个小数值（例如 4.42），因此使用 `Math.floor` 向下取整，结果被赋值给爆炸的当前帧。结果是爆炸在 20 帧内平滑推进，并且只需将 `duration` 的值从 20 编辑为其他值即可轻松更改此值。

    `清单 6-50`。 给 `frame` 赋值

    ```js
    // Explosion animation
    this.frame = Math.floor(this.age/this.duration * 5);
    ```

### 9. 在那下面，但仍在事件监听器中，输入清单 6-51 中的代码，创建一个 `if` 语句，如果爆炸已存活了期望的持续时间，则调用 `remove` 函数。请注意，如果 `if` 语句后只有一条语句，则不需要花括号（`{}`）。

    `清单 6-51`。 调用 `remove` 函数

    ```js
    if(this.age == this.duration) this.remove();
    ```

### 10. 在 `if` 语句下方，事件监听器外部，但仍在 `initialize` 函数内部，通过输入清单 6-52 中的代码将冲击波添加到 `rootScene`。

    `清单 6-52`。 给 `rootScene` 添加冲击波

    ```js
    game.rootScene.addChild(this);
    ```

### 11. 通过将清单 6-53 中的代码输入到 `remove` 函数的定义中，使 `remove` 函数将冲击波从 `rootScene` 中移除。

    `清单 6-53`。 清除根景爆炸

    ```js
    game.rootScene.removeChild(this);
    ```

### 12. 通过重写 `playerShoot` 的定义以匹配清单 6-54 中的代码，使其在击中敌人时创建 `Blast` 类的一个实例。你应该添加的那行代码是粗体的。

    `清单 6-54`。 `playerShoot` 创建 `Blast` 的实例

## 添加滚动背景

现在，我们已经加入了爆炸，事情开始看起来有点好，但我们仍然有一段路要走。为了增加准现实主义的元素，让我们为我们的游戏创建一个滚动背景。

首先，我们需要一个正好是屏幕两倍宽的背景图像。图像的左右两边应该完全一样，最左边或最右边都没有星星。我们一会儿就知道为什么了。

我们一次向左滚动一个像素，当我们到达终点时，我们循环回到起点，在整个游戏中不断重复这个过程。这就是为什么我们希望背景图片的左右两边是一样的。如果玩家注意到背景中的突然变化，看起来会很不自然。我们将使用图 6-6 中所示的图像。

![9781430247432_Fig06-06.jpg](img/9781430247432_Fig06-06.jpg)

图 6-6 。png 图形

通过创建`Background`类并加入这个循环动作，我们创造了一个无休止滚动的背景效果。执行以下操作来创建`Background`类并实现它:

### 14. 创建背景类

在 `//Class Definitions` 部分，通过输入清单 6-55 中的代码，创建一个作为 `Sprite` 类扩展的背景类。唯一需要的方法是 `initialize` 方法，因为我们永远不会将其从 `rootScene` 中移除。

**清单 6-55。** `Background`类

```js
// Background class
var Background = enchant.Class.create(enchant.Sprite, {
    initialize: function(){
    }
});
```

### 15. 初始化精灵尺寸

在 `initialize` 函数内部，通过输入清单 6-56 中的代码，将 `Background` 创建为一个 640x320 的 `Sprite`。这个尺寸与游戏屏幕一样高，宽度是其两倍。

**清单 6-56。** 创建为精灵的实例

```js
enchant.Sprite.call(this,640,320);
```

### 16. 下载背景图像

转到`http://enchantjs.com/?p=731`并下载图像文件`bg.png`，用作背景。

### 17. 上传文件

将文件上传到您在`code.9leap.net`中的项目。

### 18. 预加载背景图像

在 `window.onload` 函数中，在 `//Game Properties` 部分下，添加清单 6-57 以预加载 `bg.png`。

**清单 6-57。** 预加载背景图像

```js
game.preload('bg.png');
```

### 19. 定位和设置图像

回到 `Background` 类的 `initialize` 函数内部，在变量声明部分添加清单 6-58，以定位背景并将 `bg.png` 分配为要使用的背景图像。

**清单 6-58。** 定位和图像变量

```js
this.x = 0;
this.y = 0;
this.image = game.assets['bg.png'];
```

### 20. 添加移动事件监听器

在那之下，但仍在 `initialize` 函数内部，添加一个 `enterframe` 事件监听器，通过输入清单 6-59 中的代码，使背景每帧向左移动一个像素。

**清单 6-59。** 移动`Background`每一帧

```js
this.addEventListener('enterframe', function(){
        this.x--;
});
```

### 21. 重置背景位置

在事件监听器内部，`this.x--;` 语句下方，编写一个语句，当背景被完全滚动过去时重置其位置，通过输入清单 6-60 中的代码。如果图像的 x 位置为 `-320` 或更小，我们就知道它已被完全滚动过去。

**清单 6-60。** 重置背景位置

```js
if(this.x<=-320) this.x=0;
```

### 22. 将背景添加到场景

在事件监听器下方，但仍在 `initialize` 函数内部，通过输入清单 6-61 中的代码，将 `Background` 添加到 `rootScene`。

**清单 6-61。** 给 rootScene 添加背景

```js
game.rootScene.addChild(this);
```

### 23. 创建背景实例

最后，在 `game.onload` 函数中，将读取 `game.rootScene.backgroundColor = 'black';` 的行替换为清单 6-62 中的代码，以创建 `Background` 的一个实例。

```js
var bg = new Background();
game.rootScene.addChild(bg);
```

# 添加生命量表

目前，玩家只需一击就会死亡。如果他们在至少三次击中后倒下，似乎会公平得多。考虑到这一点，让我们添加一些东西，让玩家有多条命，并允许他们在死亡前承受几次打击。为此，请执行以下操作:

1.  在 `game.onload` 函数中，在 `//In-Game Variables and Properties` 部分下，添加一个作为 `Core` 对象 (`game`) 一部分的变量，以通过输入清单 6-63 中的代码来跟踪玩家的生命值。

    ***清单 6-63。*** 创建一个生命变量

    ```js
    game.life = 3;
    ```

    我们现在已经初始化了一个代表玩家剩余生命数的变量。因为我们在 `game.onload` 内部这样做，并使用 `game.life` 作为变量名（使用 `game.` 前缀使其成为这里称为 `game` 的 `Core` 对象的一部分），所以它可以在游戏内部的任何地方被引用。

2.  重写 `EnemyShoot` 类，以在被敌人子弹击中时将玩家的生命值减少 1，使该类与清单 6-64 中的代码匹配。你应该擦除读取 `game.end(game.score, "SCORE: " + game.score);` 的行，并用粗体类型中的代码替换它。

    ***清单 6-64。*** 命中时减少生命

    ```js
    // Class for enemy bullets
    var EnemyShoot = enchant.Class.create(Shoot, { // Succeeds bullet class
        initialize: function(x, y){
            Shoot.call(this, x, y, Math.PI);
            this.addEventListener('enterframe', function(){
                if(player.within(this, 8)){     // Bullet has hit player
                        game.life--;
                    }
            });
        }
    });
    ```

3.  在 `game.life--;` 下方，但仍在 `if` 语句内，编写另一个 `if` 语句，以便在玩家的生命值小于或等于 0 时结束游戏，方法是输入清单 6-65 中的代码。

    ***清单 6-65。*** 结束游戏如果没有生命

    ```js
    if(game.life<=0)
    game.end(game.score, "SCORE: " + game.score);
    ```

    我们已经重写了游戏，当玩家被击中时生命值减少 1，如果生命值为 0，游戏就结束了。我们可以创建一个类来显示生活，但是不需要在屏幕上添加太多内容，所以在这种情况下，简单地在 `game.onload` 中创建指示器会更有效。

    我们将使用作为 `ui.enchant.js` 插件的一部分提供的 `MutableText` 类来制作指示器。`MutableText` 类与 `Label` 类非常相似，但是它不使用计算机中已经安装的字体在屏幕上创建文本，而是使用 sprite 表中文本字符的图像，很像 `Sprite`。

    ![image](img/sq.jpg) **注意** `ScoreLabel` 类是 `MutableText` 类的扩展，行为类似。

    如果我们在这里使用常规的 `Label` 类，结果看起来会很便宜，因为将使用系统字体。通过使用 `MutableText`，你可以使用更好看的字体，并且因为使用图像而不是系统字体，你可以确保字母看起来总是一样的，不管游戏运行在什么样的浏览器或操作系统上。

4.  在 `game.onload` 函数内，在读取 `player = new Player(0, 152);` 的行下方，通过输入清单 6-66 中粗体类型的代码来创建 `MutableText` 的新实例。第一个参数 (`8`) 指定新实例左上角的 X 位置，第二个参数 (`320 – 32`) 指定 Y 位置，第三个 (`game.width`) 指定标签的宽度，第四个指定应显示的文本。

    ***清单 6-66。*** 创造了 `lifeLabel`

    ```js
    game.onload = function() {
        //In-Game Variables and Properties
        background = new Background();
        game.life = 3;

        scoreLabel = new ScoreLabel(8, 8);
        game.rootScene.addChild(scoreLabel);
        player = new Player(0, 152);
        enemies = [];
        // Display life
        lifeLabel = new MutableText(8, 320 - 32, game.width, "");
    ```

5.  在读取 `lifeLabel = new MutableText(8, 320 – 32, game.width, "");` 的行下方，但仍在 `game.onload` 函数内，创建一个事件侦听器，通过输入清单 6-67 中的代码，用等于玩家剩余生命数量的 0 的数量来更新 `lifeLabel` 的文本。

    ***清单 6-67。*** 显示生命数量

    ```js
    lifeLabel.addEventListener('enterframe', function(){
        this.text = "LIFE " + "OOOOOOOOO".substring(0, game.life);
    });
    ```

    因为我们在一个 `enterframe` 事件侦听器中定义了 `lifeLabel` 的文本，所以生命量表将在每一帧中更新。

    我们来看看 `"OOOOOOOOO".substring(0, game.life);` 部分。这个“O”代表游戏中的单身生活。`"OOOOOOOOO"` 是一个 JavaScript 字符串对象，所以我们可以直接调用应用于该字符串的方法。我们可以通过使用 `substring()` 方法从一个字符串的开头到指定的点提取文本。

应该注意的是，`substring()`方法的第二个参数表示子字符串应该从中提取字符的点。请记住，字符串和数组中的位置引用以 0 开头。这意味着如果`game.life`等于 3，子串用`"OOOOOOOOO".substring(0, game.life);`处理，子串将在位置 0、位置 1 和位置 2 返回 0，但在到达第三个位置之前会停止。

这也是为什么游戏以三条命开头，在游戏中用三个 0 来代表(`"LIFE 000"`)。随着生命的减少，它会显示`"LIFE OO"`然后是`"LIFE O."`

正如你所看到的，`enchant.js`允许你轻松地创建游戏指示器，而不必创建一个全新的类。

6.  Finally, under the event listener, add the `lifeLabel` to `rootScene` by entering in the code in Listing 6-68.

    ***清单 6-68。*** 给`rootScene`添加 life label

    ```js
    game.rootScene.addChild(lifeLabel);
    ```

7.  单击运行。寿命计将出现在图 6-8 的中。

![9781430247432_Fig06-08.jpg](img/9781430247432_Fig06-08.jpg)

图 6-8 。带生命计的街机射击游戏

如果您在本节中遇到问题，您可以在`http://code.9leap.net/codes/show/30801`找到一个完整的工作代码示例。

### 结论

希望你能看到最终拍摄程序的总体流程与我们在本章前面看到的简单原型没有太大的不同。事实上，`Background`类和许多其他类与我们在本章开始时检查的原型完全相同。

正如我们看到的第五章一样，在重击机器人的例子中，通过一点一点地重写和改进原型，一个成熟而复杂的游戏可以被开发出来。

在这一章中，我们探索了射击游戏的开发，看看如何为子弹、敌人和玩家创建类，以及如何处理子弹和敌人或玩家飞船之间的碰撞检测。然后，我们研究了可以用来改进基本游戏的更高级的概念，包括爆炸、生命标尺等等。

在下一章，我们将看看如何在`code.9leap.net`之外创建独立游戏，并研究如何使用 3D 来创建带有`gl.enchant.js`插件的游戏。