# 自动生成正则表达式 FSM

将正则表达式可视化有助于你理解其工作原理以及如何进行简化。在你将正则表达式的执行模型牢记于心之前，可以随身携带一个笔记本，为你遇到的每个正则表达式构建一个有限状态机。如果你想节省时间，也可以使用一些在线服务。

## 在线工具

[`http://ivanzuzak.info/noam/webapps/fsm_simulator/`](http://ivanzuzak.info/noam/webapps/fsm_simulator/) 模拟了正则表达式匹配给定字符串的执行过程。它不仅为你创建自动机，还会逐字符展示匹配算法的执行过程。遗憾的是，这个自动机的语言受到限制，因为我们只能拼接字符串、使用*或*运算符，以及使用 Kleene（`*`）运算符来表示任意次数的重复。我们也可以使用括号进行分组。使用加号代替竖线有点不方便，因为在这个特殊方言中，加号表示可选执行，而不是“至少一次”的重复修饰符。虽然存在这种语义上的困难，但该软件会向你展示三种不同类型的简化自动机，可用于匹配字符串。本书中的结构已经非常接近 *eNFA* 结构。

[`http://ivanzuzak.info/noam/webapps/regex_simplifier/`](http://ivanzuzak.info/noam/webapps/regex_simplifier/) 向你展示了如何简化正则表达式。例如，包含单词 *list*、*lost* 和 *lust* 的表达式可以按如下方式简化：

```
输入: list+lost+lust
R1   list+lost+lust
规则 (ab+ac) => a(b+c)
R2   l(ist+ost)+lust
规则 (ab+ac) => a(b+c)
R3   l((ist+ost)+ust)
规则 (ab+cb) => (a+c)b
R4   l(((is+os)t)+ust)
规则 (a) => a
R5   l((is+os)t+ust)
规则 (ab+cb) => (a+c)b
R6   l(((is+os)+us)t)
规则 (ab+cb) => (a+c)b
R7   l((((i+o)s)+us)t)
规则 (a) => a
R8   l(((i+o)s+us)t)
规则 (ab+cb) => (a+c)b
R9   l((((i+o)+u)s)t)
规则 ab(cd) => abcd
R10  l(((i+o)+u)s)t
规则 ab(cd) => abcd
R11  l((i+o)+u)st
规则 (a+(b+c)) => a+b+c
R12  l(i+o+u)st
```

[`https://regexper.com/`](https://regexper.com/) 可以根据 JavaScript 正则表达式构建执行图。例如，在线查看一个复杂 JavaScript 正则表达式的表示。如果你研究这张图，就可以轻松反向推断出每个元字符的含义。如果这个正则表达式让你感到畏惧，别担心，因为我们大家阅读像这样的表达式时都会遇到困难。

这种图形化表示与有限状态自动机关系不大，因为在 regexper 的图中，边不消耗字符。在这张图中，节点消耗字符，而边可以以非确定性的方式被选中。还要注意，边可能有条件，例如 *1..3 次* 或 *1+ 次*。这些边的 FSM 表示可能完全不同。这是为了便于理解而做的简化。

[`https://www.debuggex.com/`](https://www.debuggex.com/) 使用与 regexper 相同的格式，其界面可以帮助你可视化正则表达式如何逐字符匹配字符串。你可以使用 JavaScript、Python 和通用的 PCRE 语法。

[`https://regexr.com/`](https://regexr.com/) 为你提供 JavaScript 以及通用 PCRE 语法的文字说明和速查表。

[`https://regex101.com/`](https://regex101.com/) 是另一个用于测试正则表达式是否匹配字符串的便捷工具。PCRE (PHP)、JavaScript、Python 和 GoLang 都可用。该工具还可以为 C#、Java、Ruby、Rust 和 Perl 5 生成代码。

[`http://qfsm.sourceforge.net/download.html`](http://qfsm.sourceforge.net/download.html) 是我最喜欢的有限状态机设计工具。你可以方便地可视化和模拟有限状态机。