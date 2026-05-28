# C++

最后一个例子是 C++。首先，为了简单起见，你需要一个在线 C++ 编译器。你可以使用 `onlinegdb.com`^(²³) 或者 `cpp.sh`^(²⁴)。

来看一个简单的正则匹配：

```cpp
#include 
#include 
using namespace std;
int main()
{
regex r( "ab+" );
cout << regex_search( "XabbabX", r );
return 0;
}
```

我们可以使用 `regex r( regexString )` 来定义一个正则表达式。然后可以在 `regex_search` 函数中使用变量 `r`，该函数接受一个字符串和一个正则表达式作为参数。`regex_search` 的返回值是：如果找到匹配项则为 `1`，否则为 `0`。

如果你有兴趣逐个获取所有匹配项，则需要使用 `sregex_iterator`。这个迭代器基于一个字符串和一个正则表达式进行定义，并且它会逐个输出匹配项。

```cpp
#include 
#include 
using namespace std;
int main()
{
regex r( "ab+" );
string str = "XabbabX";
sregex_iterator iter(str.begin(), str.end(), r),
iter_end;
smatch nextMatch;
for ( ;iter != iter_end; ++iter ) {
cout << (*iter).str() << "\n";
}
return 0;
}
```

在任一时刻，`iter` 都指向一个正则匹配项。`*iter` 提供 `iter` 指定位置的内容。你可以通过 `str()` 获取方法来获取匹配到的标记本身。

如你所见，解读正则表达式需要相当多的 C++ 知识。如果你不懂 C++，你仍然可以使用这些代码片段来执行正则表达式。如果你懂 C++，这段代码对你来说就是小菜一碟。

## 总结

尽管本章只是浅尝辄止，但你可以看到，在大多数编程语言中，执行正则表达式是很容易的。虽然具体的规范有所不同，但它们在本质上是相似的。我强烈建议你多动手实践，这样你就能体会如何在你自己的编程语言之外的其他语言中执行正则表达式。

脚注 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 21 22 23