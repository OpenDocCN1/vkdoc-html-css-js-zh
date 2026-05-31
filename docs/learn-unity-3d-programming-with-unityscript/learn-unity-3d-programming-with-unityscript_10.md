# 布尔逻辑与条件语句

布尔类型变量只有两个可能值：`true` 和 `false`。

在 MonoDevelop 中，将脚本编辑为以下内容：

`var skyIsClear : boolean = true;`

`function Start () {`

`Debug.Log(skyIsClear);`

`}`

运行脚本，控制台将显示 `true`。布尔变量常用于监控或“标记”某个状态或条件。例如，我可以说：“如果天空晴朗，我就出去玩。”我需要检测太阳是否升起，然后根据这个条件做出决定。

将 `Start()` 函数中的代码更新为：

`if (skyIsClear == true) {`

`Debug.Log("I will go out and play");`

`}`

这段代码的可读性相当高。你希望测试的条件放在关键字 `if` 后的括号中。如果条件满足，则执行花括号内的语句块。注意，“等于”的比较运算符是 `==`，与用于设置变量值的赋值运算符 `=` 不同。继续运行脚本。耶——我能够出去玩了！

**注意**：初学者程序员最常见的错误之一就是在条件语句中使用 `=` 代替 `==`。

等等——如果天空晴朗但现在是夜晚呢？我不应该在黑暗中出去玩耍。在你的第一个变量声明下方，添加第二个变量：

`var sunIsUp: boolean = true;`

现在我想说：“如果天空晴朗且太阳升起，我就出去玩。”将你的条件 `if` 语句编辑为：

`if (skyIsClear == true && sunIsUp == true) {`

`&&` 是逻辑运算符 AND。现在必须两个条件都满足，语句块内的代码才会执行。

逻辑运算符 OR 表示为 `||`，在 Mac 上通过按 Shift+\ 两次输入。OR 的含义正如其名：如果满足其中一个条件（或两个条件都满足），则执行语句块。

如果你希望在某些条件下执行不同的代码块，可以使用多个 `if` 语句：

`function Start () {`

`if (sunIsUp) {`

`Debug.Log("I will get out of bed");`

`}`

`if (skyIsClear) {`

`Debug.Log("I do not need an umbrella");`

`}`

`}`

由于两个变量都设置为 `true`，运行脚本时你会看到两条语句都出现在控制台中。

在这个例子中，如果太阳没有升起且天空也不晴朗，则什么也不会发生。当你希望在前面的条件都不满足时执行另一段代码，可以使用 `else`：

`function Start () {`

`if (sunIsUp) {`

`Debug.Log("I will get out of bed");`

`}`

`if (skyIsClear) {`

`Debug.Log("I do not need an umbrella");`

`}`

`else {`

`Debug.Log("I am staying in bed");`

`}`

`}`

你的变量仍然设置为 `true`，因此运行脚本不会有任何不同，但你可以看到有多种方法可以测试各种条件来控制游戏进程。

`if` 语句也可以比较数值。你可以使用的其他比较运算符（表 2-2）包括 `<`、`>`、`<=`、`>=` 和 `!=`。`!` 表示“非”，因此 `!=` 是比较运算符“不等于”。

**表 2-2. 比较运算符定义**

| 比较运算符 | 含义 |
| --- | --- |
| `==` | 等于 |
| `!=` | 不等于 |
| `<` | 小于 |
| `>` | 大于 |
| `<=` | 小于等于 |
| `>=` | 大于等于 |

当你需要将变量与不同值进行比较时，`switch-case` 条件语句可以替代一系列 `if-else` 语句。

将脚本编辑为以下内容：

```
#pragma strict

var diceRoll : float;

function Start () {
    diceRoll = Random.Range(1, 6);
    switch (diceRoll) {
        case 1:
            print ("You rolled a 1!");
            break;
        case 2:
            print ("You rolled a 2!");
            break;
        case 3:
            print ("You rolled a 3!");
            break;
        case 4:
            print ("You rolled a 4!");
            break;
        case 5:
            print ("You rolled a 5!");
            break;
        case 6:
            print ("You rolled a 6!");
            break;
        default:
            print ("It's your turn - roll!");
            break;
    }
}
```

这里的情况是，你声明了一个变量但没有立即赋值。相反，当调用 `Start()` 函数时，`diceRoll` 变量会被赋一个介于 1 到 6 之间的随机生成数。

随机数生成是游戏中的另一个常见功能，用于确定诸如下一个攻击波次中新游戏对象出现的位置或敌人数量等事项。从 `diceRoll = Random.Range(1, 6);` 这行代码中你大概可以猜到发生了什么，但继续点击 `Random.Range` 查看脚本参考（图 2-27）：

![A978-1-4302-6587-0_2_Fig27_HTML.jpg](img/A978-1-4302-6587-0_2_Fig27_HTML.jpg)

**图 2-27.** Unity 脚本参考中的 `Random.Range` 说明

正如你所料，当调用 `Start()` 函数时，`diceRoll` 会根据括号内的两个参数（`min` 和 `max`）被赋一个随机生成的数值。注意这些参数是“包含边界”的，否则你需要使用不同的参数来生成 1 到 6 的数字。

接下来，`switch` 语句获取赋给 `diceRoll` 的值，并将其与每个 `case` 的常量值进行比较，在此示例中为 1 到 6。当找到匹配的 `case` 值时，执行其后的语句块。

`break` 语句结束 `switch` 语句的执行。在最后一个 `case` 之后，你会看到一个 `default` 语句，它的作用类似于 `else` 语句——如果没有任何 `case` 值与最初传递给 `switch` 语句的参数匹配，则执行该语句。

## 循环

循环对于重复执行代码行非常有用。你可能正在检查一组游戏对象的状态，或者创建新的游戏对象，此时需要对组中的每个项目使用相同的代码行。


### `for` 循环

为了演示 `for` 循环，在 MonoDevelop 中将你的代码修改如下：

```
#pragma strict
var targets : int = 4;
function Start () {
    for(var i: int = 0; i < targets; i++)
    {
        Debug.Log("This is target #" + i);
    }
}
```

将 `for` 循环分解为各个部分：

```
   (1)             (2)         (3)         (4)       (5)  (6)
for(var i: int = 0; i < targets; i++)
   (7)
   {
                    (8)
    Debug.Log(" This is target #" + i);
(9)
}
```

- 关键字 `for`。
- 将索引变量声明为整数，并赋予初始值 0。计算机从 0 而不是 1 开始计数，这需要一点时间来适应，并且经常因忘记而导致错误。
- 分号，用于分隔 `for` 语句的第一部分和第二部分。
- 条件语句。只要条件满足，循环语句块就会执行。如果条件不满足，循环终止，程序执行跳转到循环后的语句，在此例中即为 `Start()` 函数的右大括号。
- 分号，用于分隔 `for` 语句的第二部分和第三部分。
- 循环的增量。这可以写作 `i = i + 1`，甚至 `i += 1`，但增量运算符 `++` 表示加 1，是一种便捷的简写形式。这里在 `for` 循环的下一次迭代中被条件语句评估之前，索引变量加 1。类似地，索引变量也可以使用递减运算符 `--`（或 `-=`）进行递减。
- 左大括号，表示 `for` 循环体的开始。
- 语句块，包含每次迭代 `for` 循环时要执行的代码。
- 右大括号，表示 `for` 循环体的结束。与函数类似，`for` 循环以右大括号结束，而不是分号。

运行你的脚本，你会看到四条目标语句，反映了从零计数的结果（图 2-28）。

![A978-1-4302-6587-0_2_Fig28_HTML.jpg](img/A978-1-4302-6587-0_2_Fig28_HTML.jpg)

图 2-28. `for` 循环在控制台面板的输出

### `while` 循环

`while` 循环提供了更大的灵活性，因为只要条件结果为 `true`，它就会无限次地循环。

将你的代码修改如下：

```
#pragma strict
var numberOfTires : int = 0;
function Start () {
    while(numberOfTires < 4)
    {
        Debug.Log("I replaced an old tire on my car!");
        numberOfTires++;
    }
}
```

将其分解，展示如下部分：

```
      (1)                (2)
while(numberOfTires < 4)
              (3)
        {
                                          (4)
                Debug.Log("I replaced an old tire on my car!");
                numberOfTires++;
              (5)
        }
```

- 关键字 `while`。
- 条件语句。
- 左大括号，表示 `while` 循环体的开始。
- 语句块，包含每次迭代 `while` 循环时要执行的代码。
- 右大括号，表示 `while` 循环体的结束。与 `for` 循环一样，`while` 循环以右大括号结束，而不是分号。

这里最大的区别之一，也是常见问题的根源在于：如果你的条件表达式永远不会计算出 `false`，那么 `while` 循环就会持续运行。这被称为无限循环——你可不希望你的游戏陷入其中！

运行脚本，你会看到你的车换了四个新轮胎。你可以互换使用 `for` 循环或 `while` 循环来重写这些简单示例。当你在实际游戏开发场景中继续应用它们时，你会在选择使用哪一种时体会到它们之间的微妙差异。

一般来说，`for` 循环更常用于条件更加精确的情况下，例如当要执行的循环次数是预先确定的，或者当初始值、条件语句和增量都使用同一个变量时。

### `do-while` 循环

与 `for` 循环一样，如果 `while` 循环的条件从未计算为 `true`，那么语句块永远不会被执行。有时你可能希望在测试条件之前至少执行一次语句块。`do-while` 循环扩展了 `while` 循环，为你提供了这种能力。

按如下方式编辑你的代码：

```
#pragma strict
function Start () {
    var someCondition : boolean = false;
    do
    {
        Debug.Log("Print something anyway");
    }
    while(someCondition == true);
}
```

这里，你首先将变量 `someCondition` 设置为 `false`。在关键字 `do` 之后，`do` 语句的主体先执行一次，然后才执行 `while` 语句，在那里它最终有一个条件需要测试。根据你的变量声明，条件将计算出 `false`，程序退出循环。运行你的脚本，“Print something anyway” 这行文字会如你所期望的那样在控制台中显示一次。

`for` 循环和 `while` 循环之间的差异可能并不明显，但你在接下来的演示中会练习使用它们。在游戏开发的上下文中，你会对循环以及何时使用各种类型有更深刻的理解。



