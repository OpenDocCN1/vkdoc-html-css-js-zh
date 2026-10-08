## 附录 3：位标志

如第 1 章所述，你可以使用枚举来定义位标志。位标志允许通过开关序列中的各个位来选择或取消选择一系列项目。为了确保枚举中的每个值都对应一个位，编号必须遵循二进制序列，其中每个值都是 2 的幂，例如：

```
1, 2, 4, 8, 16, 32, 64, 128, 256, 512, 1024, 2048, 4096, 以此类推
```

清单 A3-1 是第 1 章示例的复制，展示了位标志的实际应用。为了代替手动分配 2 的幂，使用移位运算符来开启相应的位。要向此枚举添加新值，只需为每个新值将右侧操作数递增 1 即可。在处理枚举时，TypeScript 编译器会用普通数字替换表达式，而其他地方的其他表达式则保持不变。

- `1 << 0` 替换为 1
- `1 << 1` 替换为 2
- `1 << 2` 替换为 4
- `1 << 3` 替换为 8，以此类推。

```
enum DiscFlags {
    None = 0,
    Drive = 1 << 0,
    Influence = 1 << 1,
    Steadiness = 1 << 2,
    Conscientiousness = 1 << 3
}

// 使用标志
var personality = DiscFlags.Drive | DiscFlags.Conscientiousness;

// 测试标志
// true
var hasD = (personality & DiscFlags.Drive) == DiscFlags.Drive;
// false
var hasI = (personality & DiscFlags.Influence) == DiscFlags.Influence;
// false
var hasS = (personality & DiscFlags.Steadiness) == DiscFlags.Steadiness;
// true
var hasC = (personality & DiscFlags.Conscientiousness) == DiscFlags.Conscientiousness;
```

清单 A3-1. 标志

### 位标志详解

清单 A3-1 中的标志在表 A3-1 中进行了说明。由于位标志在大端序中表示为 32 位整数，因此代表最大值的位出现在左侧。为了按它们表示的顺序显示值，“责任心”显示在左侧，因为在示例中它由具有最大值的位表示：8。

表 A3-1. 位标志说明

| 责任心 (8) | 稳健性 (4) | 影响力 (2) | 驱动力 (1) | 二进制 | 十进制 |
| --- | --- | --- | --- | --- | --- |
| 0 | 0 | 0 | 1 | 0001 | 1 |
| 0 | 0 | 1 | 0 | 0010 | 2 |
| 0 | 1 | 0 | 0 | 0100 | 4 |
| 1 | 0 | 0 | 0 | 1000 | 8 |
| 0 | 0 | 1 | 1 | 0011 | 3 |
| 1 | 1 | 1 | 1 | 1111 | 15 |

如果标志开启，则该列显示 1。如果标志关闭，则该列显示 0。二进制值显示，二进制表示中的每一位对应于枚举中的一个值；每当“驱动力”开启时，最右侧的列将包含 1。这就是用于标志的枚举值必须使用二进制序列的原因，因为只有这种序列才能确保每个标志只影响集合中的单个位。

### 按位运算

使用位标志还意味着你可以使用按位运算符来操作表示枚举中各项的位。清单 A3-2 使用按位与运算符（`&`）来查找两组标志中的匹配特征。只有在两组标志中都开启的项，在按位与运算的结果中才会被开启；这就是为什么用按位与运算来判断某个值是否被开启。

```
// personalityA 和 personalityB 都包含 DiscFlags.Influence
var personalityA = DiscFlags.Drive | DiscFlags.Influence | DiscFlags.Conscientiousness;
var personalityB = DiscFlags.Influence | DiscFlags.Steadiness;

// 按位与的结果只包含匹配的标志
// DiscFlags.Influence
var matchingTraits = personalityA & personalityB;

// false
var hasD = (matchingTraits & DiscFlags.Drive) == DiscFlags.Drive;
// true
var hasI = (matchingTraits & DiscFlags.Influence) == DiscFlags.Influence;
// false
var hasS = (matchingTraits & DiscFlags.Steadiness) == DiscFlags.Steadiness;
// false
var hasC = (matchingTraits & DiscFlags.Conscientiousness) == DiscFlags.Conscientiousness;
```

清单 A3-2. 使用按位与查找匹配项

# 按位运算

各种按位运算的结果如表 A3-2 所示。它们产生以下结果：

*   **按位与**：如果两个集合中的该位均为开启状态，则结果中该位为开启状态。
*   **按位或**：如果两个集合中任一集合（或两个集合）中的该位为开启状态，则结果中该位为开启状态。
*   **按位异或**：如果仅有一个集合中的该位为开启状态，则结果中该位为开启状态。如果两个源中均未开启该位，或两个源中都开启了该位，则结果中该位为关闭状态。
*   **按位取反**：对一个集合进行反转，将所有“开”位关闭，将所有“关”位打开。

## 表 A3-2. 按位运算

| 第一个集合 | 第二个集合 | 按位运算 | 结果 |
| --- | --- | --- | --- |
| 1011 | 1101 | `&` (按位与) | 1001 |
| 1001 | 0011 | `|` (按位或) | 1011 |
| 1011 | 1101 | `^` (按位异或) | 0110 |
| 1011 | n/a | `~` (按位取反) | 0100 |
