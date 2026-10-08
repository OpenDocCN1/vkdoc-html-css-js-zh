# 6. 映射与集合——数据结构家族的新成员

一门编程语言必须为我们提供所有必要的构造，使开发者能够清晰高效地为其领域和问题空间建模。迄今为止，JavaScript 缺乏对大多数其他语言都提供的两种基本类型的支持，即映射（`Map`）和集合（`Set`）。这种情况在 ES6 中得到了改变。

在本章中，我们将探讨 ES6 中引入的两种新数据结构的用途，即 `Map` 和 `Set`。我们将从它们的构造开始，理解它们的 API，甚至深入底层了解它们是如何实现的。到本章结束时，我们将完全理解如何以及在何时可以在代码中利用映射和集合，以及何时最好避免使用它们。

## 为什么对象还不够用

JavaScript 提供了两种类似“集合”的对象——普通对象（使用 `{}` 字面量表示）和数组（使用 `[]` 字面量表示）。普通对象通常被称为关联数组或字典，通过键进行查找。而数组则提供了一种顺序数据结构，能够通过索引进行高效的随机查找。

对象和数组虽然有用，但并不能满足所有使用场景。对象作为*真正*的键值存储的一个限制是，对象的键都是字符串。换句话说，当在对象中使用时，键的“类型”无法保留。这一点可以简单地演示如下：

- ① 创建一个包含两个*看似*不同的键的对象
- ② 用特定类型查询对象中的某个键
- ③ **仅**交换键值对的顺序，不做其他更改
- ④ 现在相同的查询结果不同了

```
const obj1 = {
  'true': 'String',
  true: 'Boolean',
}; ①
console.assert(obj1[true] === 'Boolean'); ②
const obj2 = {
  true: 'Boolean',
  'true': 'String',
}; ③
console.assert(obj2[true] === 'String'); ④
```

这一点对于任何曾将 JavaScript 对象序列化为 JSON 的人来说（我们*所有人*都遇到过）都是显而易见的。

鉴于这个限制，如果我们需要保留键的*类型*，我们别无他法。^(⁴²)

因此，ES6 为我们提供了映射（Maps）和集合（Sets）。映射与对象非常类似，也是键值存储；但映射会保留键的类型，更接近其他语言中提供的映射。而集合则填补了一个角色，在 ES6 之前，开发者不得不对数组中的条目进行大量的 `===` 检查，或者仅仅使用字符串作为对象中的“唯一条目”，如果都不行，就只能求助于第三方库^(⁴³) 来满足需求。

我们的探索到此为止。我们先从映射开始，接着介绍集合。

## 映射

映射与对象非常相似，也通过键进行查找，区别在于映射会保留键的*类型*（而不是像对象那样将它们强制转换为字符串）。映射提供了一个构造函数作为创建新映射的方式。与数组和对象不同，映射**没有**提供字面量构造方式，这对其可序列化性有一定影响（我们稍后会看到）。映射还提供了一套全面的 API，用于在映射中添加、更新和删除键值对，下面一一演示：

*映射的键值 API*

- ① 创建映射的构造函数
- ② 使用 `set` 设置键
- ③ `set` 是一个流畅接口，返回映射本身
- ④ `has` 表示是否拥有该键
- ⑤ `get` 是查询 API
- ⑥ 强制发生冲突会导致之前的*值*被覆盖
- ⑦ 最后设置的值胜出
- ⑧ 如果删除一个已有键，映射返回 `true`
- ⑨ 删除不存在的键

```
const obj = {};
const map = new Map(); ①
map.set('string', 'is a string'); ②
map.set(true, 'is a boolean');
map.set(1, 'is a number').set(obj, 'is an object'); ③
console.assert(map.has(1)); ④
console.assert(map.get(obj) === 'is an object'); ⑤
map.set(1, 'overrides first'); ⑥
console.assert(map.get(1) === 'overrides first'); ⑦
console.assert(map.delete('string')); ⑧
console.assert(map.has('string') === false);
console.assert(map.delete({}) === false); ⑨
```

`Map` API 提供了我们处理键值所需的一切。`set` 接口是流畅的，这无疑是一个强大的盟友，可以避免因非流畅接口而产生的冗余代码。冲突的处理方式也符合预期，最后，我们可以使用 `delete` API 从映射中移除一个键值对，如果键确实从映射中被删除，则返回 `true`（如果键不存在，`delete` 会返回 `false`，表示没有东西可删）。

在更高层面，还有一些 API 允许我们整体操作映射，例如获取所有条目、条目计数，甚至清空映射（如果我们想这样做的话）：

- ① 可接受可迭代对象的替代构造函数
- ② 表示键值对的元组
- ③ 元组中多余的元素会被丢弃
- ④ 只有键，因此值被设置为 `undefined`
- ⑤ 键值对都缺失
- ⑥ 一个略显奇怪但用于查询数量的属性
- ⑦ 用于遍历键值对的函数式 API
- ⑧ 这些方法都返回迭代器
- ⑨ 清空映射并将其 `size` 设为 0 的 API

```
const map = new Map([ ①
  ['s', 'is a string'], ②
  [true, 'is a boolean', 'willBeIgnored'], ③
  [1], ④
  [], ⑤
]);
console.assert(map.size === 4); ⑥
console.assert(map.get('s') === 'is a string');
console.assert(map.get(true) === 'is a boolean');
console.assert(map.get(1) === undefined);
console.assert(map.get(undefined) === undefined);
console.assert(map.has(undefined));
map.forEach((v, k) => console.log(v, k)); ⑦
console.log(map.entries());
console.log(map.keys());
console.log(map.values()); ⑧
map.clear(); ⑨
console.assert(map.size === 0);
```

映射还提供了另一个有用的构造函数，可以使用任何“可迭代对象”来创建映射。一种常见用法是向构造函数传入一个数组的数组。每个嵌套数组被视为一个二元组，分别表示键和值。如果嵌套数组只包含一个元素，则该值被设置为 `undefined`。如果数组包含超过两个元素，则所有多余的元素都会被忽略。最后，如果数组为空，则键和值都被设置为 `undefined`。如果提供的数组**没有**产生冲突，那么生成的映射将按数组相同的顺序排列其键，并且生成的映射的 `size` 将等于数组的 `length`。

映射（`Map`）会保留其条目的插入顺序，具体来说就是**键**被添加的顺序。然而，如果发生键冲突，新值会覆盖*该键所在位置*的旧值。也就是说，它们的行为非常类似于数组，只是它们的“索引”可以是任意类型。

由于映射维护了顺序，它们允许我们使用 `forEach` 等方法以及 `entries` API 来遍历其 `entries`。该 API 每次返回一个键值对，被封装在长度为 2 的数组中，表示为 `[key, value]`。映射还提供了 `keys` 和 `values` API，顾名思义，它们会按照插入顺序返回映射中所有键和值的数组。

映射和数组一样遵循“迭代器”协议，正如我们之前讨论过的，这使得映射可以通过 `...` 运算符进行展开，并能进行解构。正如我们所见，映射可以通过数组的数组来创建；反过来，可以推测，展开一个映射会将其分解为一个数组的数组，其中每个数组都是一个键和值的元组，而外层的数组反映了插入顺序，其 `length` 等于映射的 `size`。

- ① 创建一个映射
- ② 将映射展开为数组
- ③ 解构获取第二个和第三个条目

# Map 的深入探讨

```javascript
const firstQuarter = new Map([
[1, 'Jan'],
[2, 'Feb'],
[3, 'Mar'],
[4, 'Apr'],
]); ①
const mapToStr = [...firstQuarter].join(':'); ②
console.assert(mapToStr === '1,Jan:2,Feb:3,Mar:4,Apr');
const [
,
[n2, m2],
[n3, m3],
] = firstQuarter; ③
console.assert(n2 === 2);
console.assert(m2 === 'Feb');
console.assert(n3 === 3);
console.assert(m3 === 'Mar');
```

鉴于映射是可迭代的，并且 `Map` 构造函数期望接收一个可迭代对象，当我们尝试从现有映射创建新映射时，这变得非常方便。当创建恰好是其他映射的并集的新映射时，这甚至更加便利：

- ① 两个映射的并集
- ② “后到者优先”原则

```javascript
const nihar = new Map([
['car', 'mazda'],
['residence', 'apartment'],
]);
const ericka = new Map([
['pet', 'oscar'],
['residence', 'house'],
]);
const union = new Map([...nihar, ...ericka]); ①
console.assert(union.get('car') === 'mazda');
console.assert(union.get('residence') === 'house'); ②
console.assert(union.get('pet') === 'oscar');
```

我们从两个独立的映射开始，然后利用映射是“可展开的”这一特性以及映射构造函数可以使用数组来构建新映射的事实，尝试构建一个新的映射。注意，在这种情况下，传递给构造函数的数组有**四个**条目；然而，当映射尝试将每个条目添加到新映射时，它在遇到 `residence` 键时会检测到冲突。因此，结果映射中 `residence` 键对应的值将是 `house`。

为了全面了解映射，让我们回答一个仍未解决的相关问题——两个键如何被视为相等？答案在于 `===` 操作符。如果 `set` 操作时，映射中已存在的任何键通过 `===` 测试返回 `true`，则视为冲突，该键的值将被设置为新值。否则，一个新的键值对条目会被插入到映射的末尾。

## 注意事项

与映射相关的注意事项并不多。其 API 很全面，能够完成我们对映射期望的所有功能。

然而，第一个不足之处是缺少字面量语法。这迫使我们使用构造函数，这似乎与我们通常处理对象和数组的方式相悖。在 JavaScript 中有很多理由不使用构造函数^(⁴⁴)，但可惜的是，我们别无选择。让我们通过一个简单的例子来观察这种差异：

- ① 对象为我们提供了字面量 `{}` 语法
- ② 同样地，数组提供了 `[]` 字面量
- ③ 我们却被迫使用构造函数

```javascript
const kiran = { ①
name: 'kiran',
profession: 'pharmacist',
};
const favFruits = [ ②
'mango',
'guava',
'pineapple',
];
const numbersInHindi = new Map(); ③
numbersInHindi.set(1, 'ek').set(2, 'do').set(3, 'teen');
```

第二个问题涉及 JSON 的序列化（和反序列化）。JSON 规范^(⁴⁵) 只支持两种数据结构——对象和数组。因此，对任何映射使用 `JSON.stringify` 都不会产生预期的结果。

事实证明，我们能为映射的序列化版本做到的最接近的方式是将其视为一个数组的数组。在另一端，我们可以简单地 `JSON.parse` 接收到的字符串，然后将其交给映射构造函数以创建一个新的映射。观察如下：

- ① 序列化映射的便捷函数
- ② 序列化的逆操作

```javascript
const nihar = new Map([
['car', 'mazda'],
['residence', 'apartment'],
]);
const mapToJson = map => JSON.stringify([...map]); ①
const jsonToMap = mapStr => new Map(JSON.parse(mapStr)); ②
const serialized = mapToJson(nihar);
// 输出 '[["car","mazda"],["residence","apartment"]]'
console.log(serialized);
const deserialized = jsonToMap(serialized);
// 输出 'Map { 'car' => 'mazda', 'residence' => 'apartment' }'
console.log(deserialized);
```

将序列化/反序列化逻辑封装到工具函数中会很有益处，我们很快就会看到这一点。另外，映射的“可迭代性”再次证明了它是我们的朋友。

然而，如果这让你有点担心，那么你并不孤单。在数据交换层缺乏对映射的支持，将判断某个数据是否为映射的责任转移到了应用层。换句话说，仅仅查看 JSON 内容，根本无法表明某些东西应该被解析为映射，还是仅仅是一个数组的数组。

此外，如果某个对象或数组可能包含映射，我们就不能再简单地对其使用 `JSON.stringify`！我们现在被迫要检查所有值，如果恰好是映射，就使用我们的便捷函数先序列化那个值，然后再序列化父对象。或者，我们可以尝试使用第三方库^(⁴⁶) 来为我们完成转换。^(⁴⁷)

这里的关键问题在于，映射的键可以是*任何*类型，包括引用类型。让我们将其与序列化对象进行对比；对象中的键总是字符串，并且字符串自身就是其表示。因此，序列化一个对象键简化为简单地将该字符串放入 JSON 中。映射则不同！如果我们使用一个对象引用作为映射的键，并且该引用被序列化，那么在反序列化时，该引用就会丢失！考虑以下情况：

- ① 序列化一个包含对象引用作为键的映射
- ② 将其反序列化回映射
- ③ 查找该对象引用失败

```javascript
const obj = {
name: 'some object',
};
const map = new Map([
[obj, 'value against an object key'],
]);
const mapToJson = m => JSON.stringify([...m]);
const jsonToMap = mapStr => new Map(JSON.parse(mapStr));
const serialized = mapToJson(map); ①
// 输出 '[[{"name":"some object"},"value against an object key"]]'
console.log(serialized);
const deserialized = jsonToMap(serialized); ②
// 输出 'Map { { name: 'some object' } => 'value against an object key' }'
console.log(deserialized);
console.assert(deserialized.get(obj) === undefined); ③
```

从 JSON 反序列化后，要重新获得一个本身是对象引用的键的唯一方法是遍历映射的所有键并获取其引用。这完全违背了键查找的目的！

那么，映射真的有实际用途吗？当然有。只要我们不打算序列化映射，它们就很有用。它们提供了普通的旧式 JavaScript 对象（`POJO`）所不允许的灵活性。当然，如果你*确实*打算序列化，那么最好所有的键本身都是可序列化的（换句话说，是字符串），这样我们才能“重新找到”它们。如果这不可行，那么你最好还是只使用 `POJO`。

## WeakMaps

ES6 引入了另一种映射变体，即 `WeakMap`。我们不会花太多时间深入讲解 weakmap，只会重点说明它们与常规 map 的区别。

第一个区别是 `WeakMap` 的键**必须**是对象；不允许使用原始值。^((48))

第二个区别在于 weakmap 如何持有其键。(常规) `Map` 对其键保持“强”引用。为了便于说明，假设我们有一个对象，并把它用作常规 map 中的键。如果我们在维护该对象的引用计数，那么计数就会加一。换句话说，即使对该对象的所有其他引用都丢失了，该对象也**无法**被垃圾回收，因为引用计数仍然为 1——这个引用就是 map 所持有的那个。为了使该键及其关联值能够被垃圾回收，我们必须显式地将其从 map 中 `delete` 掉。请看下面的示例：

- ① 对一个对象的引用
- ② 将该引用用作 map 中的键
- ③ 丢弃对该对象的引用
- ④ Map 继续持有引用

## Sets

`WeakMap` 对其任何键都**不**持有强引用。相反，它们“弱弱地”持有键。可以将弱引用理解为：它们太弱了，无法强制“被引用”对象留在内存中。如果所有*其他*对键的引用都丢失了，这允许作为键的对象被垃圾回收。

我们来分解一下。假设有一个 DOM 元素，我们希望用一个数组来关联一些额外数据。这听起来很适合用 `map`，对吧？于是，我们将 DOM 元素用作 `map` 中的键，将关联的数组作为值。现在，每当我们想检索数据时，只需 `get` 该 DOM 元素，就大功告成了。完美。

现在，某些用户交互导致该元素从 DOM 中删除，从而消除了保留该数组的必要。由于 `map` 持有对该 DOM 元素的（强）引用，垃圾回收器无法回收它，这就成了我们开发者的责任，必须记得从 `map` 中 `delete` 这个键。如果我们*万一*忘记了，那么内存泄漏就出现了！

这正是使用 `WeakMap` 的理想场景。如果改用 `WeakMap` 存储 DOM 与数组的键值对，当 DOM 元素被删除时，`WeakMap` 会让垃圾回收器回收掉这个键，以及与之关联的数组。我们就不用再手动从 `map` 中移除引用及其关联数据了。

`WeakMap` 的 API 相当有限，原因很简单：`WeakMap` 可能在任意时刻丢失一个键（及其关联值）。因此，任何强制 `WeakMap` 查看所有条目的 API，例如 `size` 和任何迭代 API，都是根本不可行的。`WeakMap` 只允许 `set`、`get`、`delete` 键，以及检查 `WeakMap` 是否 `has` 某个键的条目。`WeakMap` 的构造函数接口与 `Map` 类似，如果我们在构造时有可用的可迭代对象，也可以传入它来初始化 `WeakMap`。

`WeakMap` 主要面向库和框架的作者。想想 jQuery 的 `data` API（它允许你将任意数据附加到任何元素），或者你想在对象外部存储关于某个对象的元数据（想象一下规范缓存），你就会找到 `WeakMap` 的用武之地。我相信随着 ES6 的普及，我们将会看到 `WeakMap` 的其他有趣用途。

`Set` 允许我们存储唯一的值。如果我们尝试向 `Set` 添加一个已经包含的值，该值会被拒绝。从实现角度看，`Set` 在底层*就是* `Map`，只不过键和值是同一个东西，即值本身。这意味着 `Map` 的所有特性（和注意事项）也适用于 `Set`。由于 `Map` 和 `Set` 如此相似，我们对 `Set` 的探索会相当简洁。

与 `Map` 一样，`Set` 也不提供字面量，只有构造函数。`Set` 的 API 很完善，允许我们像预期那样操作 `Set`。

我们从构造和值的基本操作开始：

- ① 构造函数创建 `Set`
- ② `add` 一个值
- ③ `add` 是流式 API，返回 `Set` 本身
- ④ `has` 表示包含
- ⑤ 强制重复添加……
- ⑥ `Set` 保持不变
- ⑦ 如果 `delete` 一个已有的值，`Set` 返回 `true`
- ⑧ 删除一个不存在的值返回 `false`

```javascript
const obj = {};
const set = new Set(); ①
set.add('string'); ②
set.add(true);
set.add(1).add(obj); ③
console.assert(set.size === 4);
console.assert(set.has(1)); ④
set.add(1); ⑤
console.assert(set.size === 4); ⑥
console.assert(set.delete('string')); ⑦
console.assert(set.has('string') === false);
console.assert(set.delete({}) === false); ⑧
```

这里没有什么意外，和 `Map` 非常相似，`Set` 提供了一套完整的 API 来操作值，并且也具有 `Map` 那样的流式调用能力。当然，我们必须注意，`Set` 没有 `get` 方法，因为对于 `Set` 来说 `get` 等同于 `has`（因为键*就是*值）。

`Set` 还提供了更高级的 API，允许我们整体操作 `Set`。还有一个补充的构造函数接受可迭代对象，可迭代对象中的每个值都代表 `Set` 中的一个值（假设没有冲突）。通过可迭代对象构造这一点暗示了：和 `Map` 一样，`Set` 也会维护插入顺序（假设没有冲突），从而支持诸如 `forEach` 这样的迭代 API：

- ① 使用可迭代对象创建 `Set` 的构造函数
- ② 表示 `Set` 值的数组
- ③ 再次提醒，用于查询计数的属性可能有点令人困惑
- ④ 遍历值的函数式 API
- ⑤ 有趣的是，有一个 `keys` API，它与 `values` 相同
- ⑥ 以上所有方法都返回迭代器
- ⑦ 用于清空 `Set` 并将 `size` 设为 0 的 API

```javascript
const set = new Set([ ①
  'string',
  true,
  1, ②
]);
console.assert(set.size === 3); ③
console.assert(set.has('string'));
console.assert(set.has(true));
console.assert(set.has(1));
set.forEach(k => console.log(k)); ④
console.log(set.entries());
console.log(set.keys()); ⑤
console.log(set.values()); ⑥
set.clear(); ⑦
console.assert(set.size === 0);
```

`Set` 和 `Map` 一样，表现类似于数组，其中“索引”和该索引处的值是相同的。我们可以向 `Set` 的 `forEach` 传递一个双参数回调来确认这一点：

```javascript
set.forEach((k, v) => console.assert(k === v))
```

`Set` 也提供了一个 `entries` API，表示 `Set` 中的条目。由于 `Set` 中的值充当键，所以这只是一个可以表示为 `[value, value]` 的两元素数组。同样地，也存在 `keys` 和 `values` API；不过，毫不意外，`keys` API 只是 `values` 的别名。

由于 `Set` 是可迭代的，因此它们也是可展开的。考虑到我们有一个接受可迭代对象的构造函数，我们就有了一种便捷的方式来从现有 `Set` 创建新 `Set`，或者求多个 `Set` 的并集。

- ① 两个 `Set` 的并集
- ② 重复项会被丢弃

```javascript
const colorsOne = new Set([
  'red',
  'blue',
]);
const colorsTwo = new Set([
  'yellow',
  'blue',
]);
const union = new Set([...colorsOne, ...colorsTwo]); ①
console.assert(union.has('red'));
console.assert(union.has('blue')); ②
console.assert(union.has('yellow'));
```

最后，和 `Map` 一样，相等性是通过 `===` 运算符确定的。换句话说，如果我们 `add` 一个值，该值与 `Set` 中任何已有值进行 `===` 检查返回 `true`，那么这个值就不会被添加到 `Set` 中。

### 注意事项

`Set` 在底层*就是*一种 `Map`，因此进一步探讨只会重复强调：我们针对 `Map` 讨论的所有内容都适用于 `Set`。由于缺少字面量，我们不得不使用构造函数；并且由于 JSON 不支持 `Set`，与 JSON 之间的转换可能会导致部分信息丢失。

`Set` 与 `Map` 在序列化时的唯一区别在于：`Set` 会被序列化为一个仅包含不同值的数组。

### WeakSet

`WeakSet` 之于 `Set`，正如 `WeakMap` 之于 `Map`。它提供了一种“弱”引用机制，允许其值在没有其他引用指向该特定值时被垃圾回收。

正如预期的那样，`WeakSet` 不提供任何迭代 API，只允许通过 `add` 和 `delete` 方法添加和删除值，并通过 `has` 方法检查是否拥有某个值。其构造函数与 `Set` 类似，可以在构造时接受一个可迭代对象。

WeakSet 的一个用例是“标记”对象。假设我们需要序列化一个包含循环引用的 JavaScript 对象。`JSON.stringify` 会导致堆栈溢出。在这种情况下，我们必须手动遍历对象及其引用——每次“记录”是否已经见过某个特定对象，如果见过，则不再序列化该引用。可以使用 `WeakMap` 来标记已经见过的对象，并且可以确保事后不会有任何悬空引用需要清理。请看：

* ① 如果已经遇到过该键，这里直接 `return`

* ② 为 `JSON.stringify` 创建一个 `replacer` 函数

* ③ 创建一个包含循环引用的对象

* ④ 这会导致堆栈溢出错误

* ⑤ 使用丢弃所有已访问条目的 `replacer` 函数

```
let weakSet = new WeakSet();
const replacer = (key, value) => {
  if (typeof value === 'object' && value !== null) {
    if (weakSet.has(value)) {
      // 发现循环引用，丢弃该键
      return; // ①
    }
    // 将值存入集合
    weakSet.add(value);
  }
  return value;
}; // ②
const rootObj = {
  name: 'rootObj',
};
rootObj.children = [{
  parent: rootObj,
  name: 'childOne',
  children: [],
}, {
  parent: rootObj,
  name: 'childTwo',
  children: [],
}]; // ③
// console.log(JSON.stringify(rootObj)); // ④
console.log(JSON.stringify(rootObj, replacer, 2)); // ⑤
// 输出
// {
//   "name": "rootObj",
//   "children": [
//     {
//       "name": "childOne",
//       "children": []
//     },
//     {
//       "name": "childTwo",
//       "children": []
//     }
//   ]
// }
```

我们首先编写“replacer”函数，该函数将由 `JSON.stringify` 调用。当 `JSON.stringify` 遍历作为第一个参数传递给 `stringify` 的对象时，它会调用我们的函数，并将当前尝试转换为字符串表示形式的键和值传递给它。我们的第一项检查是，通过询问内部 `weakset` 是否已经包含该键，来判断是否已经“见过”它——如果已经见过，则直接返回。否则，我们*首先*将该键添加到 `weakset`，然后返回该“值”。当然，我们可以简单地使用一个数组来保存之前见过的引用列表，但那样我们就必须清空该数组或完全解除引用，以确保不会造成内存泄漏。

# 总结

JavaScript 似乎终于成熟了。我们现在拥有了其他许多语言中习惯使用的四种数据结构。`Map` 和 `Set` 填补了之前在 JavaScript 中难以原生实现的功能。

`Map` 和 `Set` 都有一些局限性。然而，这些限制是为了保持语言（和 `JSON`）的语义而存在的。尽管如此，它们仍然是对该语言的极好补充，使我们开发者能够更好地在代码中表达意图。

在下一章中，我们将探讨增强的对象字面量，并探索 ES6 中引入的一些新的 Object API。

脚注 1 2 3 4 5 6 7
