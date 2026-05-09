# 从正则表达式中提取子串

你在第 1 章中了解了许多正则表达式的不同用例。让我重复其中一些用例：

- 测试字符串是否匹配搜索表达式
- 在字符串中查找某些字符
- 替换字符串中与正则表达式匹配的子串
- 处理和格式化用户输入
- 从服务器日志、配置文件和文本文件中提取信息
- 验证 Web 应用程序和终端中的输入

许多用例要求我们查找字符串中的*子串*，并将其提取、重新排序、删除或替换。

正则表达式通过子串提取的形式为我们提供了一些帮助来执行这些任务。

当我介绍字面字符和元字符时，我提到括号有两种语义：对字符进行分组和提取子串。即使在第 2 章关于正则表达式运算符优先级和括号的部分，我也提到过我们会使用括号进行子串提取。让我们来学习这个重要的用例。

## 定义捕获组

当正则表达式子串`x`位于括号内，如`(x)`，它就是一个*捕获组*。捕获组不能在字符类内部定义。

捕获组的值会被捕获，并且可以在正则表达式内部或使用你所使用的编程语言的 API 进行检索。只有当正则表达式匹配过程中到达了右括号时，才能进行检索。

在除 Perl 6 之外的所有语言中，捕获组的标识符是一个正整数，由表达式中左括号的位置决定。

**练习 1**：让我们确定以下 PCRE 正则表达式中的捕获组：

```
/^a(b|c(d|(e))(f))$/
```

**解答：**

表 7-1 显示了解答。

| 捕获组编号 | 子串模式                   |
|-----------|---------------------------|
| 1         | `b\|c(d\|(e))(f)`          |
| 2         | `d\|(e)`                   |
| 3         | `e`                        |
| 4         | `f`                        |

捕获组编号由左括号的位置决定。

```
/^a(b|c(d|(e))(f))$/
^   ^  ^   ^
|   |  |   |
1   2  3   4
```

## Perl 6 捕获组

在 Perl 6 中，捕获组的编号方式不同。捕获组编号从零开始。捕获组标识符以嵌套分层的方式递增。

在最顶层，有一个标识符为`0`的捕获组。

```
/^a(b|c(d|(e))(f))$/
^
|
0
```

在捕获组`0`内部，下一层有两个捕获组：`0.0`和`0.1`。捕获组编号如下开始：

```
/^a(b|c(d|(e))(f))$/
^      ^
|      |
0.0    0.1
```

在捕获组`0.0`内部，还有另一个捕获组。

| Perl 6 捕获组编号 | 子串模式                   |
|------------------|---------------------------|
| 0                | `b\|c(d\|(e))(f)`          |
| 0.0              | `d\|(e)`                   |
| 0.0.0            | `e`                        |
| 0.1              | `f`                        |

```
/^a(b|c(d|(e))(f))$/
^
|
0.0.0
```

## 捕获子串的检索

请查阅你正在使用的编程语言的文档。作为示例，我们将用以下语言检索子串：

- JavaScript
- PHP
- Python
- Perl 5

我们将使用与之前相同的示例。假设我们想从如下格式的字符串中检索货币、数字价格值以及带货币的完整价格：

```
Price: €19.00
```

匹配此字符串的正则表达式如下：

```
/^Price: [€\$]\d\d\.\d\d$/
```

我们必须对作为货币的美元符号进行转义，因为`$`是一个表示字符串结尾或行尾锚点的元字符。我们还必须对点号进行转义，因为它是一个表示任意单个字符的元字符。

让我们为需要捕获的子串添加一些括号。

```
/^Price: (([€\$])(\d\d\.\d\d))$/
```

添加的捕获组包含如表 7-3 所示的数据。

| 捕获组编号 | 数据         |
|-----------|--------------|
| 1         | 完整价格      |
| 2         | 货币符号      |
| 3         | 数字价格      |

让我们探索这三个捕获组的检索。

### JavaScript

API 的工作方式如下：

- `regex.exec( str )`
- `str.match( regex )`

这些方法在匹配`str`时返回`regex`中定义的所有捕获组。

```javascript
const regex = /^Price: (([€\$])(\d\d\.\d\d))$/;
const str = 'Price: €19.00';
const matches = regex.exec( str );
// str.match( regex ); // 效果相同
console.table( matches );
```

输出结果为：

```
(index) Value
0       "Price: €19.00"
1       "€19.00"
2       "€"
3       "19.00"
index   0
input   "Price: €19.00"
```

顺便提一下，如果你还不了解`console.table`，可以查看我关于[console API](http://www.zsoltnagy.eu/understanding-the-console-api-in-javascript-7-tips-for-smoother-debugging/)的文章。

### PHP

我们将使用`preg_match_all( $regex, $str, $result )`。PHP 会在`$result`数组中返回所有捕获组。

```php
$regex = "/^Price: ((€|\$)(\d\d\.\d\d))$/";
$str = "Price: €19.00";
preg_match_all($regex, $str, $result);
print_r($result);
```

执行代码，例如在[PHP Sandbox](http://sandbox.onlinephpfunctions.com/)中。结果如下：

```
Array
(
[0] => Array
(
[0] => Price: €19.00
)
[1] => Array
(
[0] => €19.00
)
[2] => Array
(
[0] => €
)
[3] => Array
(
[0] => 19.00
)
)
```

### Python

让我们回忆一下首次用 Python 测试正则表达式时看到的代码，并将正确的`regex`和`test_str`值放入其中。

```python
import re
regex = r"^Price: ((€|\$)(\d\d\.\d\d))$"
test_str = "Price: €19.00"
matches = re.finditer(regex, test_str)
for matchNum, match in enumerate(matches, start=1):
    print ("Match {matchNum} was found at {start}-{end}: {match}"
           .format(matchNum=matchNum, start=match.start(),
                   end=match.end(), match=match.group()))
    for groupNum in range(0, len(match.groups())):
        groupNum = groupNum + 1
        print ("Group {groupNum} found at {start}-{end}: {group}"
               .format(groupNum=groupNum, start=match.start(groupNum),
                       end=match.end(groupNum), group=match.group(groupNum)))
```

我们将此代码粘贴到[Python shell](https://www.python.org/shell/)中后，结果便可见。

```
Match 1 was found at 0-13: Price: €19.00
Group 1 found at 7-13: €19.00
Group 2 found at 7-8: €
Group 3 found at 8-13: 19.00
```

如你所见，捕获组可以通过`match`对象访问。

是的，这就是最初介绍同一 Python 代码时所承诺的伏笔。最初，相同的代码未能在表达式中找到任何捕获组，但现在你可以看到内部的`for`循环逐个打印出了捕获组。

### Perl 5

我们已经看到可以使用`=~`运算符执行简单匹配。

```perl
if ( "Price: €19.00" =~ /^Price: ((€|\$)(\d\d\.\d\d))$/ ) {
    print "Match";
}
```

我们已经看到了一些变量，它们为我们提供了匹配的一些上下文，例如：

```perl
if ( "Price: €19.00" =~ /^Price: ((€|\$)(\d\d\.\d\d))$/ ) {
    print "String before the match: ", $`, "\n";
    print "String after the match:", $', "\n";
    print "Matched string:", $&, "\n";
}
```

类似地，每个捕获组都有以`$1`、`$2`、`$3`等形式定义的变量。

```perl
if ( "Price: €19.00" =~ /^Price: ((€|\$)(\d\d\.\d\d))$/ ) {
    print "String before the match: ", $`, "\n";
    print "String after the match:", $', "\n";
    print "Matched string:", $&, "\n";
    print "First capture group:", $1, "\n";
    print "Second capture group:", $2, "\n";
    print "Third capture group:", $3, "\n";
}
```

执行结果如下：

```
String before the match:
String after the match:
Matched string:Price: €19.00
First capture group: €19.00
Second capture group: €
Third capture group:19.00
```



