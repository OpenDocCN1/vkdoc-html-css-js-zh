# 粘性匹配

在匹配之后，`y` 标志会将正则表达式的 `lastIndex` 属性设置为上一次匹配序列之后的第一个字符。如果正则表达式上一次执行没有匹配到任何结果，`lastIndex` 会被设置为 `0`。

这会对正则表达式的内部状态产生改变。务必注意这个副作用！当 `y` 标志开启时，正则表达式开头会自动添加一个 `^`。这意味着在 `lastIndex` 位置上的字符必须与正则表达式的起始位置匹配。

示例如下：

```
> regExp = /ab+/y
/ab+/y
> 'ababbabbb'.match( regExp )
{ 0:"ab", index: 0, input: "ababbabbb" }
> regExp.lastIndex

> 'ababbabbb'.match( regExp )
{ 0: "abb", index: 2, input: "ababbabbb" }
> regExp.lastIndex

> 'ababbabbb'.match( regExp )
{ 0:"abbb", index: 5, input: "ababbabbb" }
> regExp.lastIndex

> 'ababbabbb'.match( regExp )
null
> regExp.lastIndex

> 'ababbabbb'.match( regExp )
{ 0: "ab", index: 0, input: "ababbabbb" }
> regExp.lastIndex

// ...
```