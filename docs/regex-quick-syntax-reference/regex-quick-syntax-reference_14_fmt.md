# 其他基于 PCRE 的正则表达式环境

JavaScript 正则表达式以及大多数编程语言的正则表达式执行环境都基于 PCRE 方言。我们现在将探讨如何在一些最流行的语言中执行正则表达式。

鉴于大多数语言执行正则表达式不如 JavaScript 那样简单，我们将使用一些工具，使得在浏览器中执行和测试正则表达式成为可能。

当你需要一种快速生成匹配正则表达式的代码的解决方案时，可以前往 `regex101.com` 的代码生成器。^(²)

### PHP

PHP 提供了用于处理正则表达式的全局函数。这些函数以以下前缀开头：

- `preg_` 匹配 Perl PCRE 语法
- `ereg_` 基于 ERE 方言
- `mb_ereg` 同样基于 ERE 方言，且能处理 Unicode 字符

我们在这里只讨论 PCRE 语法。

在 PHP 中，有些修饰符与 JavaScript 相同，而另一些则不同。

- `i` 表示不区分大小写，与 JavaScript 中相同。
- `m` 表示多行模式，与 JavaScript 中相同。
- `u` 表示 Unicode 匹配，与 JavaScript 中相同。
- `s` 使 `.` 字符匹配包括换行符在内的所有字符，与 ES2018 中相同。
- `x` 开启自由间距模式以提高可读性。自由间距模式会忽略正则表达式字符之间的空白字符。

请参考 PHP 手册的 PCRE 部分（^(³)）以获取所有 PCRE 正则函数的文档。

你可以使用许多在线沙盒工具（例如 `writephponline.com`^(⁴) 或 Joodle PHP 在线编辑器^(⁵)）在线执行 PHP 代码。

你可以使用 `preg_match` 从字符串中返回第一个匹配项。

```
preg_match('/xy+/', 'xyxyyxyyy', $matches, PREG_OFFSET_CAPTURE);
print_r($matches);
```

结果如下所示：

```
Array ( [0] => Array ( [0] => xy [1] => 0 ) )
```

这是一个包含一个元素的数组，该元素描述了匹配结果。匹配描述符本身也是一个数组，其中索引 0 包含匹配到的序列，索引 1 包含匹配结果中第一个字符的位置。

`preg_match_all` 返回所有匹配项，因此它类似于 JavaScript 中的 `g` 标志。请记住，PHP 没有 `g` 修饰符，因此全局匹配功能是通过 PCRE 正则表达式封装器的公共接口来实现的。

```
preg_match_all('/xy+/', 'xyxyyxyyy', $matches, PREG_OFFSET_CAPTURE);
print_r($matches);
```

结果包含所有匹配项。

```
Array (
[0] => Array (
[0] => Array ( [0] => xy   [1] => 0 )
[1] => Array ( [0] => xyy  [1] => 2 )
[2] => Array ( [0] => xyyy [1] => 5 )
)
)
```

尽管 `preg_` 函数充当 Perl 5 风格正则表达式的封装器，但在 Perl 语法上仍有一些细微差异。更多详情请查阅 `php.net` 上关于 PCRE 模式差异的文档^(⁶)。

### Python

现在我们将使用 `python.org` 的交互式 shell^(⁷) 开始熟悉 Python 中的正则表达式。`re` 这一 Python 模块为你提供了对 PCRE 风格正则表达式的支持。你可以通过 `import re` 导入此模块。

```
>>> import re
```

`re` 模块的 `compile` 方法会根据提供给它的模式字符串编译一个正则表达式。使用你已有的 PCRE 知识，你已经可以编写许多模式字符串了。

```
>>> regex = re.compile(r"xy+")
```

注意字符串前面的 `r`。在 Python 中，这表示一个原始字符串。虽然在之前的例子中可以省略这个 `r`，但在一般情况下，使用原始字符串可以省去大量转义字符的工作。

已编译正则表达式的 `search` 方法会在其字符串参数中搜索匹配项。

```
>>> regex.search( "QxyxyyQ" )
```

你可以通过一些方法获取关于结果的更多信息。

```
>>> result = regex.search( "QxyyxyQ" )
>>> result.group(), result.start(), result.end(), result.span()
('xyy', 1, 4, (1, 4))
```

`group` 方法以其最长形式返回第一个匹配项。可以使用 `start`、`end` 和 `span` 方法获取匹配项的第一个字符和匹配项之后的第一个字符。

已编译正则表达式的 `match` 方法容易引起误解，因为它是从字符串的*起始位置*开始寻找匹配。

```
>>> regex.match( "QxyxyyQ" )
None
>>> regex.match( "xyxyyQ" )

```

从技术上讲，`match` 是多余的，因为你可以使用 `^` 字符来匹配字符串的开头。

```
>>> re.compile( "^xy+" ).search( "xyxyyQ" )

```

我解释了 `match` 方法，目的是让你避免将其与 `search` 混淆。

并不一定非要编译正则表达式，因为 `match` 和 `search` 方法也可以通过 `re` 模块作为函数直接调用，这些函数接受一个正则表达式字符串和一个普通字符串作为参数。

```
>>> re.search( '^xy+', 'xyyyyxyQ' )

```

还可以枚举与搜索表达式匹配的所有结果。`findall` 和 `finditer` 方法可以实现这个功能。

```
>>> re.findall( 'xy+', 'QxyxyyQxyyyQ' )
['xy', 'xyy', 'xyyy']
>>> iterator = re.finditer( 'xy+', 'QxyxyyQxyyyQ' )
>>> for matchNum, match in enumerate(iterator):
...     print ( match.group(), match.span() )
...
xy (1, 3)
xyy (3, 6)
xyyy (7, 11)
```

在第一个例子中，我们将所有匹配项枚举到了一个数组中。在第二个例子中，我们创建了一个迭代器并开始遍历它，逐一打印结果。

现在你已经掌握了足够的知识来理解 `regex101.com`^(⁸) 生成的示例代码。

```
import re
regex = r"ab+"
test_str = "ababb"
matches = re.finditer(regex, test_str)
for matchNum, match in enumerate(matches):
matchNum = matchNum + 1
print ("Match {matchNum} was found at {start}-{end}: {match}"
.format(
matchNum = matchNum,
start = match.start(),
end = match.end(),
match = match.group()
)
)
for groupNum in range(0, len(match.groups())):
groupNum = groupNum + 1
print ("Group {groupNum} found at {start}-{end}: {group}"
.format(
groupNum = groupNum,
start = match.start(groupNum),
end = match.end(groupNum),
group = match.group(groupNum)
)
)
```

请注意，我对代码中的长行进行了格式化。由于本书的宽度有限，这一改动是合理的，而且编写过长的代码行也不是一个好习惯。

这段代码会输出以下内容：

```
Match 1 was found at 0-2: ab
Match 2 was found at 2-5: abb
```

在这个例子中，关于分组的内部循环没有执行。你在后面学习使用捕获组提取子字符串时，还会回到这段代码上。

更多信息请参考 Python 的正则表达式文档^(⁹)。

### Perl 5

Perl 5 文档^(¹⁰) 为你提供了如何编写正则表达式的信息。在本节中，你将学习如何执行一个简单的正则表达式并输出关于匹配结果的一些信息。

你可以在在线 Perl 编译器中执行这些代码片段，例如 `rextester.com`^(¹¹) 上的那个。可以使用 `=~` 运算符来执行一个简单的匹配。

```
if ( "ababb" =~ /ab+/ ) {
print "Match";
}
```

当匹配成功时，`"ababb" =~ /ab+/` 的值为 `1`，否则为 `0`。你还可以获取匹配到的字符串以及匹配项之前和之后的字符串。

```
if ( "ababb" =~ /ab+/ ) {
print "Match: ", $`, "|-->|", $&, "||ab|<--|abb
```

Perl 提供了 `g` 全局修饰符来获取所有匹配项。

```
while ( "ababb" =~ /ab+/g ) {
print "Match: ", $`, "|-->|", $&, "||ab||abb|<--|
```

好的，作为一名高级文档工程师和翻译员，我将严格遵循给定的注意事项和示例格式，为您提供高质量的翻译。

## Java

现在您将看到如何编写一个简单的 Java 类来执行一些正则表达式匹配。

如果您不想在本地机器上安装 Java 开发环境，可以使用 `tio.run`^((12)) 或 `compilejava.net`^((13)) 来运行 Java 代码。

我们先来看看源代码：

- `java.util.regex` 包含了一个 `Matcher` 和一个 `Pattern` 工具类。`Pattern` 编译正则表达式，而 `Matcher` 则创建一个对象来将模式与字符串进行匹配。

- 您可以使用 `Matcher` 对象的 `find` 方法对其进行迭代，以定位下一个目标。

- 可以通过 `Matcher` 对象访问匹配的详细信息。`group()`、`start()` 和 `end()` 这些 getter 函数分别返回匹配到的字符串、匹配的起始索引以及匹配后第一个字符的索引。

```
import java.util.regex.Matcher;
import java.util.regex.Pattern;
public class Main {
public static void main( String args[] ) {
final String regex = "xy+";
final String string = "QxyxyyQxyyyQ";
final Pattern pattern = Pattern.compile(regex);
final Matcher matcher = pattern.matcher(string);
while (matcher.find()) {
System.out.println(
"Full match: " + matcher.group() + "(" +
matcher.start() + "," + matcher.end() + ")"
);
}
}
}
```

执行结果如下：

```
Full match: xy(1,3)
Full match: xyy(3,6)
Full match: xyyy(7,11)
```

## R

在 R 语言中，使用正则表达式相当直接。

您可以使用 `grep` 函数根据正则表达式匹配来过滤字符串列表。`value = TRUE` 表示您对过滤后的字符串值本身感兴趣。`FALSE` 表示您对返回值的索引感兴趣。

```
grep("ab+", strings, value = TRUE)
grep("^ab+", strings, value = TRUE)
grep("ab+", strings, value = FALSE)
grep("^ab+", strings, value = FALSE)
```

让我们创建几个字符串。

```
(strings <- c("XabbbbX", "abb", "aaa", "a", "Abbb", "XabX"))
```

返回的值如下：

```
[1] "XabbbbX" "abb" "XabX"
[1] "abb"
[1] 1 2 6
[1] 2
```

`regexpr` 函数提供更多关于匹配的信息，如下所示：

```
regexpr("ab+", strings)
[1] 2 1 -1 -1 -1 2
attr(,"match.length")
[1] 5 3 -1 -1 -1 2
```

第一行包含每个字符串中第一个匹配字符的索引。

您也可以使用 `regmatches` 来显示匹配到的子字符串。首先，您需要将匹配结果保存到一个常量中，然后将此常量作为 `regmatches` 函数的第二个参数。

```
matches <- regexpr("ab+", strings)
regmatches( strings, matches )
```

返回值如下：

```
[1] "abbbb" "abb" "ab"
```

如需更多信息，请查阅 R 语言手册。^((14))

## C#

`regex101.com`^((15)) 生成的 C# 代码可以顺利执行，并展示了在 C# 中使用正则表达式是多么简单。

`System.Text.RegularExpressions` 提供了对正则表达式的支持。

与 Java 类似，您需要创建一个类和一个静态的 `Main` 函数作为应用程序的入口点。

在 `Main` 函数中，您需要定义一个模式和一个字符串。Regex101 分别在变量 `pattern` 和 `input` 中定义了这些。`Regex.Matches` 会逐一枚举所有匹配项。每个匹配项存储在一个 `Match` 类型的对象中。`Match` 对象具有 `Value` 和 `Index` 字段。

```
using System;
using System.Text.RegularExpressions;
public class Example
{
public static void Main()
{
string pattern = @"ab+";
string input = @"ababb";
foreach (Match m in Regex.Matches(input, pattern))
{
Console.WriteLine("'{0}' found at index {1}.", m.Value, m.Index);
}
}
}
```

例如，在 `dotnetfiddle.net`^((16)) 中执行此代码后，控制台将输出以下文本：

```
'xy' found at index 0.
'xyy' found at index 2.
```

## Ruby

让我们在在线 Ruby 控制台（如 `repl.it`^((17))）中测试 Ruby 正则表达式的基础知识。

```
> /ab+/ =~ 'XabbabX'
=> 1
```

使用 `=~` 运算符进行正则表达式匹配。正则表达式匹配的返回值是匹配的第一个字符的索引。当没有匹配时，返回 `nil`。

```
> /ab+/ =~ 'XX'
=> nil
```

您也可以使用正则表达式的 `match` 方法。匹配的返回值是一个包含第一个匹配项的 `MatchData` 对象。

```
> /ab+/.match( 'XabbabX' )
=> #<MatchData "abb">
```

您可以通过查询 `$&` 来获取匹配到的字符串。

```
> $&
=> "abb"
```

让我们再次执行匹配，并访问关于匹配结果的更多数据。

```
> result = /ab+/.match( 'XabbabX' )
=> #<MatchData "abb">
> result[0]
=> "abb"
> result.pre_match
=> "X"
> result.post_match
=> "abX"
```

如果您对包含所有匹配项的数组感兴趣，可以使用字符串的 `scan` 方法。

```
> 'XabbabX'.scan( /ab+/ )
=> ["abb", "ab"]
```

与 C# 代码类似，`regex101.com`^((18)) 生成的 Ruby 代码同样易于解读。

- `re` 包含正则表达式，`str` 是您尝试匹配的字符串。

- 通过使用 `str.scan` 方法，您会得到一个数组。您可以遍历该数组并打印所有匹配项。

```
re = /xy+/
str = 'xyxxyyy'
# Print the match result
str.scan(re) do |match|
puts match.to_s
end
```

如果您有兴趣了解更多关于 Ruby 正则表达式的知识，请查阅 Ruby 正则表达式文档^((19))。

## Golang

使用 Go Playground^((20)) 来执行 Golang 代码。

让我们使用 `regex101.com`^((21)) 生成一些正则表达式测试代码。

```
package main
import (
"regexp"
"fmt"
)
func main() {
var re = regexp.MustCompile(`ab+`)
var str = `ababb`
for i, match := range re.FindAllString(str, -1) {
fmt.Println(match, "found at index", i)
}
}
```

在代码中，我们创建了正则表达式 `ab+` 和字符串 `ababb`。

正则表达式方法 `FindAllStrings` 查找所有匹配正则表达式的子字符串。然后，我们可以打印出匹配的子字符串及其索引。

我们可以使用一些其他的 Golang 特性来进行正则表达式匹配。

```go
package main
import (
"regexp"
"fmt"
)
func main() {
var re = regexp.MustCompile(`ab+`)
var str = `ababb`
// re.MatchString returns a boolean indicating if there is a match
fmt.Println( re.MatchString(str) )
// re.FindString returns a string containing the first match
fmt.Println( re.FindString(str) )
// re.FindAllString collects all matches in an array
// Important: FindAllString is written in singular
fmt.Println( re.FindAllString(str, -1) )
}
```

Golang Regexp 文档^((22)) 包含更多关于 `regexp` 包的信息。