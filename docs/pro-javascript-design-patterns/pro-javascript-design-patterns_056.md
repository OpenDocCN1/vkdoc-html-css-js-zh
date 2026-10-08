# 函数示例

## 核心逻辑

### 函数声明

```python
def func():
    pass
```

### 函数调用

```python
func()
```

## 变量与表达式

在表达式 `cvar = avar + bvar` 中，加法运算符（`+`）将 `avar` 与 `bvar` 相加，得到它们的和 `cvar`。

在 `List.of(arg0, arg1, arg2)` 中，`List` 接口的工厂方法 `of()` 接受一系列的元素，返回包含它们的只读列表。

之后我们这样调用 `cmd` 命令：`cmd arg0 arg1 arg2`。

```python
if (condVar > someVal) {console.log("xxx")}
```

## 关于 `Memo` 函数

### 函数定义

`memo` 函数接受一个参数 `func`，返回一个新的函数。这个新函数会维护一个缓存（通常是一个对象或 `Map`），用于存储之前调用 `func` 的结果。当新函数被调用时，它首先检查缓存中是否存在对应的键（即传入的参数）。如果存在，则直接返回缓存中的值，避免重复计算；如果不存在，则调用 `func` 计算结果，将结果存入缓存，然后返回该值。

### 缓存实现

缓存通常是一个对象或 `Map`，用于存储之前的计算结果。键是传入的参数，值是对应的计算结果。

### 适用场景

- **递归函数**：如斐波那契数列、阶乘等，可以避免重复计算，提高性能。
- **纯函数**：对于相同的输入总是返回相同输出的函数，缓存结果可以节省计算资源。
- **耗时计算**：如数据处理、复杂数学运算等，缓存可以显著提升效率。

### 示例代码

```python
from functools import lru_cache

@lru_cache(maxsize=None)
def fibonacci(n):
    if n < 2:
        return n
    return fibonacci(n-1) + fibonacci(n-2)
```

### 注意事项

- 缓存会占用内存，因此需要合理设置缓存大小或过期策略。
- 对于非纯函数（如依赖外部状态或时间），缓存可能导致错误结果。
- 在并发环境中，需要考虑缓存的线程安全性。

## 更多方法

### `push` 方法

```python
def push(self, item):
    self.items.append(item)
```

### `pop` 方法

```python
def pop(self):
    return self.items.pop()
```

## 完整代码示例

```python
class Memo:
    def __init__(self, func):
        self.func = func
        self.cache = {}

    def __call__(self, *args):
        if args not in self.cache:
            self.cache[args] = self.func(*args)
        return self.cache[args]

### 使用示例
@Memo
def slow_function(x):
    # 模拟耗时计算
    import time
    time.sleep(1)
    return x * 2
```

## 总结

`Memo` 函数是一种优化技术，通过缓存函数调用的结果来避免重复计算。它特别适用于递归函数、纯函数和耗时计算。然而，使用时需要注意缓存大小、函数纯度和线程安全性等问题。通过合理实现，可以显著提升程序性能。

---

**参考链接**：<https://example.com/memo>

## 文本排版

### 父级

- **变量**: `parent`，字符串类型，存储父节点的引用。
- **子节点列表**: `children`，数组，存储子节点的引用。
- **节点高度**: `height`，数值类型，当前节点的高度值。
- **是否为叶子节点**: `isLeaf`，布尔类型，判断当前节点是否为叶子节点。

#### 用于实现树的遍历

**深度优先搜索 (DFS)**

使用栈（`stack`）模拟递归过程。

```
push(root)
while (stack not empty) {
    node = stack.pop()
    // 处理当前节点
    for (child in node.children) {
        stack.push(child)
    }
}
```

# 广度优先搜索 (BFS)

使用队列（`queue`）进行层次遍历。

```
queue.push(root)

while (queue not empty) {

    node = queue.shift()

    // 处理当前节点

    for (child in node.children) {

        queue.push(child)

    }

}
```

### 树结构示例

假设我们有一个简单的树，其结构和父子关系如下：

```
A

├── B

│   ├── D

│   └── E

└── C

    ├── F

    └── G
```

- `B`、`C` 是 `A` 的直接子节点。
- `D`、`E` 是 `B` 的直接子节点。
- `F`、`G` 是 `C` 的直接子节点。
- `A` 是根节点。
- 没有父节点的节点：`None`。
- 父节点相同：`A`。
- `A` 的父节点：`None`。
- `B` 的父节点：`A`。
- `C` 的父节点：`A`。
- `D` 的父节点：`B`。
- `E` 的父节点：`B`。
- `F` 的父节点：`C`。
- `G` 的父节点：`C`。

### 使用递归构造上述树

在编写树结构时，我们通常使用 `Function` 和 `Element` 类。

**父级**: `parent`
**子节点**: `children`
**节点高度**: `height`
**是否是叶子节点**: `isLeaf`

### 代码示例

在 `path.to.xxx` 路径下找到 `configurations.json`。

```
configurations.json
```

```
path.to.xxx/

├── config.yaml

└── data/

    └── ...
```

在 `List.of(elements)` 中，`List` 接口的工厂方法 `of()` 接受一系列元素，返回包含它们的只读列表。

### 数据结构

#### 定义节点

```javascript
class TreeNode {

    constructor(value) {

        this.value = value;

        this.children = [];

    }

}
```

#### 构建树

```javascript
function buildTree() {

    const root = new TreeNode('A');

    const nodeB = new TreeNode('B');

    const nodeC = new TreeNode('C');

    const nodeD = new TreeNode('D');

    const nodeE = new TreeNode('E');

    const nodeF = new TreeNode('F');

    const nodeG = new TreeNode('G');

    root.children.push(nodeB, nodeC);

    nodeB.children.push(nodeD, nodeE);

    nodeC.children.push(nodeF, nodeG);

    return root;

}

const tree = buildTree();

console.log(tree);
```

### 遍历算法

#### 深度优先搜索 (DFS)

```javascript
function dfs(root) {

    if (!root) return;

    console.log(root.value);

    for (const child of root.children) {

        dfs(child);

    }

}
```

#### 广度优先搜索 (BFS)

```javascript
function bfs(root) {

    const queue = [root];

    while (queue.length > 0) {

        const node = queue.shift();

        console.log(node.value);

        for (const child of node.children) {

            queue.push(child);

        }

    }

}
```

### 实际应用场景

#### 文件系统

```javascript
const fs = require('fs');

const path = require('path');

function printDirectoryTree(dirPath, indent = '') {

    const items = fs.readdirSync(dirPath);

    for (const item of items) {

        const fullPath = path.join(dirPath, item);

        const stat = fs.statSync(fullPath);

        console.log(`${indent}${item}`);

        if (stat.isDirectory()) {

            printDirectoryTree(fullPath, indent + '  ');

        }

    }

}

printDirectoryTree('./');
```

#### HTML DOM 树

```javascript
// 假设有一个 HTML 结构

const htmlString = `

<div id="root">

    <div class="child">Child 1</div>

    <div class="child">Child 2</div>

</div>

`;

function parseHtml(html) {

    // 使用 DOMParser 解析 HTML

    const parser = new DOMParser();

    const doc = parser.parseFromString(html, 'text/html');

    return doc.getElementById('root');

}

const rootElement = parseHtml(htmlString);

console.log(rootElement);
```
