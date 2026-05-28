# 四、JavaScript 字符串

这一章将集中讨论字符串、JavaScript `String`对象和`String`对象的内置函数。您将学习如何访问、比较、分解和搜索现实生活中常用的字符串。此外，本章将探讨字符串编码、解码、加密和解密。到本章结束时，你将理解如何有效地使用 JavaScript 字符串，并对字符串编码和加密有一个基本的了解。

## JavaScript 字符串原语

JavaScript 的原生`String`原语带有各种常见的字符串函数。

### 字符串访问

访问字符时，使用`.charAt()`。

```js
'dog'.charAt(1); // returns "o"
```

`.charAt(index)`获取一个索引(从 0 开始)并返回字符串中该索引位置的字符。

对于字符串(多字符)访问，您可以使用`.substring(startIndex, endIndex)`，它将返回指定索引之间的字符。

```js
'YouTube'.substring(1,2); // returns 'o'
'YouTube'.substring(3,7); // returns 'tube'
```

如果您没有传递第二个参数(`endIndex`)，它将返回从指定的开始位置到结束位置的所有字符值。

```js
return 'YouTube'.substring(1); // returns 'outube'
```

### 字符串比较

大多数编程语言都有比较字符串的功能。在 JavaScript 中，这可以通过使用小于和大于操作符来实现。

```js
var a = 'a';
var b = 'b';
console.log(a < b); // prints 'true'
```

这对于在排序算法时比较字符串非常有用，这将在本书后面介绍。

但是，如果您正在比较两个不同长度的字符串，它将从字符串的开头开始比较，直到较小字符串的长度。

```js
var a = 'add';
var b = 'b';
console.log(a < b); // prints 'true'
```

在这个例子中，比较了`a`和`b`。由于`a`小于`b`，所以`a < b`的计算结果为`true`。

```js
var a = 'add';
var b = 'ab';
console.log(a < b); // prints 'false'
```

在这个例子中，在比较了`'a'`和`'b'`之后，比较了`'d'`和`'b'`。处理无法继续，因为`'ab'`的一切都已被关注。这和比较`'ad'`和`'ab'`是一样的。

```js
console.log('add'<'ab' == 'ad'<'ab'); // prints 'true'
```

### 字符串搜索

要在字符串中查找特定的字符串，可以使用`.indexOf(searchValue[, fromIndex])`。这需要一个作为要搜索的字符串的参数，以及一个用于搜索的起始索引的可选参数。它返回匹配字符串的位置，但是如果什么也没有找到，那么返回-1。请注意，该函数区分大小写。

```js
'Red Dragon'.indexOf('Red');    // returns 0
'Red Dragon'.indexOf('RedScale'); // returns -1
'Red Dragon'.indexOf('Dragon', 0); // returns 4
'Red Dragon'.indexOf('Dragon', 4); // returns 4
'Red Dragon'.indexOf(", 9);    // returns 9
```

要在更大的字符串中检查搜索字符串的出现，只需检查-1 是否从`.indexOf`返回。

```js
function existsInString (stringValue, search) {
    return stringValue.indexOf(search) !== -1;
}
console.log(existsInString('red','r')); // prints 'true';
console.log(existsInString('red','b')); // prints 'false';
```

您可以使用附加参数在字符串中的某个索引后进行搜索。一个例子是计算某些字母的出现次数。在以下示例中，将计算字符`'a'`的出现次数:

```js
var str         = "He's my king from this day until his last day";
var count       = 0;
var pos         = str.indexOf('a');
while (pos !== -1) {
  count++;
  pos = str.indexOf('a', pos + 1);
}
console.log(count); // prints '3'
```

最后，如果字符串以指定的输入开始，`startsWith`返回 true(布尔值)，并且`endsWith`检查字符串是否以指定的输入结束。

```js
'Red Dragon'.startsWith('Red'); // returns true
'Red Dragon'.endsWith('Dragon'); // returns true
'Red Dragon'.startsWith('Dragon'); // returns false
'Red Dragon'.endsWith('Red'); // returns false
```

### 字符串分解

要将一个字符串分解成多个部分，可以使用`.split(separator)`，这是一个非常有用的函数。它接受一个参数(分隔符)并创建一个子字符串数组。

```js
var test1 = 'chicken,noodle,soup,broth';
test1.split(","); // ["chicken", "noodle", "soup", "broth"]
```

传递一个空分隔符将创建一个包含所有字符的数组。

```js
var test1 = 'chicken';
test1.split(""); // ["c", "h", "i", "c", "k", "e", "n"]
```

当一个字符串中列出多个项目时，这很有用。可以将字符串转换成数组，以便轻松地遍历它们。

### 字符串替换

`.replace(string, replaceString)`用另一个字符串替换字符串变量中的指定字符串。

```js
"Wizard of Oz".replace("Wizard","Witch"); // "Witch of Oz"
```

## 正则表达式

正则表达式( *regexes* )是定义搜索模式的一组字符。学习如何使用正则表达式本身就是一项艰巨的任务，但是作为一名 JavaScript 开发人员，了解正则表达式的基础知识非常重要。

JavaScript 还带有原生对象`RegExp`，用于正则表达式。

`RegExp`对象的构造函数有两个参数:正则表达式和可选的匹配设置，如下所示:

```
i      Perform case-insensitive matching
g      Perform a global match (find all matches rather than stopping after first match)
m      Perform multiline matching
```

`RegExp`有以下两个功能:

*   `search()`:测试字符串中的匹配。这将返回匹配的索引。

*   `match()`:匹配测试。这将返回所有匹配项。

JavaScript `String`对象还有以下两个与 regex 相关的函数，它们接受`RegExp`对象作为参数:

*   `exec()`:测试字符串中的匹配。这将返回第一个匹配项。

*   `test()`:测试字符串中的匹配。这将返回`true`或`false`。

### 基本正则表达式

以下是基本的正则表达式规则:

`^`:表示字符串/行的开始

`\d`:查找任意数字

`[abc]`:查找括号之间的任何字符

`[^abc]`:查找括号中除以外的任何字符

`[0-9]`:查找括号之间的任意数字

`[⁰-9]`:查找括号中除以外的任何数字

`(x|y)`:查找任何指定的备选项

以下返回索引 11，它是字符`D`的索引，该字符是匹配的正则表达式的第一个字符:

# JavaScript中的正则表达式与编码

```js
var str = "JavaScript DataStructures";
var n = str.search(/DataStructures/);
console.log(n); // prints '11'
```

## 常用的正则表达式

正则表达式对于检查 JavaScript 中用户输入的有效性非常有帮助。一种常见的输入检查是验证它是否有任何数字字符。

以下是开发人员经常使用的五种正则表达式。

### 任何数字字符

```js
/\d+/

var reg = /\d+/;
reg.test("123"); // true
reg.test("33asd"); // true
reg.test("5asdasd"); // true
reg.test("asdasd"); // false
```

### 仅数字字符

```js
/^\d+$/

var reg = /^\d+$/;
reg.test("123"); // true
reg.test("123a"); // false
reg.test("a"); // false
```

### 浮动数字字符

```js
/^[0-9]*.[0-9]*[1-9]+$/

var reg = /^[0-9]*.[0-9]*[1-9]+$/;
reg.test("12"); // false
reg.test("12.2"); // true
```

### 仅字母数字字符

```js
/[a-zA-Z0-9]​/

var reg = /[a-zA-Z0-9]/;
reg.test("somethingELSE"); // true
reg.test("hello"); // true
reg.test("112a"); // true
reg.test("112"); // true
reg.test("^"); // false
```

### 查询字符串

```js
/([^?=&]+)(=([^&]*))/
```

在 web 应用程序中，出于路由或数据库查询的目的，web URLs 经常在 URL 中传递参数。

例如，对于 URL `http://your.domain/product.aspx?category=4&product_id=2140&query=lcd+tv`，URL 可能会对后端 SQL 查询做出如下响应:

```js
SELECT LCD, TV FROM database WHERE Category = 4 AND Product_id=2140;
```

要解析这些参数，正则表达式会很有用。

```js
var uri = 'http://your.domain/product.aspx?category=4&product_id=2140&query=lcd+tv';
var queryString = {};
uri.replace(
    new RegExp("([^?=&]+)(=([^&]*))?", "g"),
    function ($0, $1, $2, $3) { queryString[$1] = $3; }
);
console.log('ID: ' + queryString['product_id']); // ID: 2140
console.log('Name: ' + queryString['product_name']); // Name: undefined
console.log('Category: ' + queryString['category']); // Category: 4
```

## 编码

编码是计算机科学中的一个通用概念，它以一种专门的格式来表示字符，以便有效地传输或存储。

所有计算机文件类型都以特定的结构编码。

例如，当您上传 PDF 时，编码可能如下所示:

```js
JVBERi0xLjMKMSAwIG9iago8PCAvVHlwZSAvQ2F0YWxvZwovT3V0bGluZXMgMiAwIFIKL1BhZ2VzIDMgMCBS\
ID4+CmVuZG9iagoyIDAgb2JqCjw8IC9UeXBlIC9PdXRsaW5lcyAvQ291bnQgMCA+PgplbmRvYmoKMyAwIG9i\
ago8PCAvVHlwZSAvUGFnZXMKL0tpZHMgWzYgMCBSCl0KL0NvdW50IDEKL1Jlc291cmNlcyA8PAovUHJvY1Nl\
dCA0IDAgUgovRm9udCA8PCAKL0YxIDggMCBSCj4+Cj4+Ci9NZWRpYUJveCBbMC4wMDAgMC4wMDAgNjEyLjAw\
MCA3OTIuMDAwXQogPj4KZW5kb2JqCjQgMCBvYmoKWy9QREYgL1RleHQgXQplbmRvYmoKNSAwIG9iago8PAov\
Q3JlYXRvciAoRE9NUERGKQovQ3JlYXRpb25EYXRlIChEOjIwMTUwNzIwMTMzMzIzKzAyJzAwJykKL01vZERh\
dGUgKEQ6MjAxNTA3MjAxMzMzMjMrMDInMDAnKQo+PgplbmRvYmoKNiAwIG9iago8PCAvVHlwZSAvUGFnZQov\
UGFyZW50IDMgMCBSCi9Db250ZW50cyA3IDAgUgo+PgplbmRvYmoKNyAwIG9iago8PCAvRmlsdGVyIC9GbGF0\
ZURlY29kZQovTGVuZ3RoIDY2ID4+CnN0cmVhbQp4nOMy0DMwMFBAJovSuZxCFIxN9AwMzRTMDS31DCxNFUJS\
FPTdDBWMgKIKIWkKCtEaIanFJZqxCiFeCq4hAO4PD0MKZW5kc3RyZWFtCmVuZG9iago4IDAgb2JqCjw8IC9U\
eXBlIC9Gb250Ci9TdWJ0eXBlIC9UeXBlMQovTmFtZSAvRjEKL0Jhc2VGb250IC9UaW1lcy1Cb2xkCi9FbmNv\
ZGluZyAvV2luQW5zaUVuY29kaW5nCj4+CmVuZG9iagp4cmVmCjAgOQowMDAwMDAwMDAwIDY1NTM1IGYgCjAw\
MDAwMDAwMDggMDAwMDAgbiAKMDAwMDAwMDA3MyAwMDAwMCBuIAowMDAwMDAwMTE5IDAwMDAwIG4gCjAwMDAw\
MDAyNzMgMDAwMDAgbiAKMDAwMDAwMDMwMiAwMDAwMCBuIAowMDAwMDAwNDE2IDAwMDAwIG4gCjAwMDAwMDA0\
NzkgMDAwMDAgbiAKMDAwMDAwMDYxNiAwMDAwMCBuIAp0cmFpbGVyCjw8Ci9TaXplIDkKL1Jvb3QgMSAwIFIK\
L0luZm8gNSAwIFIKPj4Kc3RhcnR4cmVmCjcyNQolJUVPRgo=.....
```

这是一个 Base64 编码的 PDF 字符串。像这样的数据通常在上传 PDF 文件时被传递到服务器。

### Base64 编码

`btoa()`函数从一个字符串创建一个 Base64 编码的 ASCII 字符串。字符串中的每个字符都被视为一个字节(8 位:8 个 0 和 1)。

`.atob()`函数对使用 Base64 编码的数据字符串进行解码。例如，Base64 编码字符串中的字符串“hello I love learning to computer program”如下所示:agvsbg 8 GSS BSB 3 zlig xlyxjuaw 5 nihrvignvbxb 1 dgvyihby B2 dyyw 0。

```js
btoa('hello I love learning to computer program');
//  aGVsbG8gSSBsb3ZlIGxlYXJuaW5nIHRvIGNvbXB1dGVyIHByb2dyYW0
atob('aGVsbG8gSSBsb3ZlIGxlYXJuaW5nIHRvIGNvbXB1dGVyIHByb2dyYW0');
// hello I love learning to computer program
```

在 [`https://en.wikipedia.org/wiki/Base64`](https://en.wikipedia.org/wiki/Base64) 了解 Base64 的更多信息。

## 字符串缩短

你有没有想过像[这样的网址缩短网站。ly](http://bit.ly) 工作？一种简化的 URL 压缩算法遵循一定的结构，如下图所示为 [`www.google.com`](http://www.google.com) :

![img/465726_1_En_4_Fig2_HTML.png](img/465726_1_En_4_Fig2_HTML.png)

图 4-2

缩短后的数据库条目

1.  整数 ID 被缩短为一个字符串。使用 Base62 编码缩短时，`11231230` 将是 `VhU2`。

![img/465726_1_En_4_Fig1_HTML.png](img/465726_1_En_4_Fig1_HTML.png)

图 4-1

数据库条目

1.  数据库为 URL 创建一个唯一的基于整数的 ID。在图 4-1 ， [`www.google.com`](http://www.google.com) 数据库中有条目 `11231230`。

对于缩短部分，可以使用下面的算法。有 62 个可能的字母和数字，由 26 个小写字母、26 个大写字母和 10 个数字(0 到 9)组成。

```js
var DICTIONARY = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789".split("");

function encodeId(num) {
    var base = DICTIONARY.length;
    var encoded = "";

    if (num === 0) {
        return DICTIONARY[0];
    }

    while (num > 0) {
        encoded += DICTIONARY[(num % base)];
        num = Math.floor(num / base);
    }

    return reverseWord(encoded);
}

function reverseWord(str) {
    var reversed = "";
    for (var i = str.length - 1; i >= 0; i--) {
        reversed += str.charAt(i);
    }
    return reversed;
}

function decodeId(id) {
    var base = DICTIONARY.length;
    var decoded = 0;

    for (var index = 0; index < id.split("").length; index++) {
        decoded = decoded * base + DICTIONARY.indexOf(id.charAt(index));
    }

    return decoded;
}

console.log(encodeId(11231230)); // prints 'VhU2'
console.log(decodeId('VhU2')); // prints '11231230'
```

## 加密

在保护人们的在线信息时，加密极其重要。你在谷歌 Chrome 浏览器中见过图 4-3 中的警告吗？

![img/465726_1_En_4_Fig3_HTML.jpg](img/465726_1_En_4_Fig3_HTML.jpg)

图 4-3

SSL 警告

这可能意味着您尝试访问的网站没有正确的安全套接字层(SSL)证书。

![img/465726_1_En_4_Fig4_HTML.png](img/465726_1_En_4_Fig4_HTML.png)

图 4-4

TSL 过程

TSL 是一种标准的安全技术，用于在服务器和客户端(浏览器)之间建立加密链接。以下是 TSL 过程的简化步骤。在这个过程中，服务器对不同的密钥使用非对称加密进行加密和解密。浏览器只使用对称加密，即使用一个密钥来加密和解密数据。

1.  服务器将其非对称公钥发送给浏览器。

2.  浏览器为当前会话创建一个对称密钥，该密钥由服务器的非对称公钥加密。

3.  服务器通过其私钥解密浏览器的会话，并检索会话密钥。

4.  两个系统都有会话密钥，并将使用该密钥安全地传输数据。

这是安全的，因为只有浏览器和服务器知道会话密钥。如果浏览器第二天连接到同一个服务器，将会创建一个新的会话密钥。

SSL 警告消息表示浏览器和服务器可能没有加密该连接上的数据。

最常用的公钥加密算法是 RSA 算法。

### RSA 加密

RSA 是一种基于分解大整数难度的加密算法。在 RSA 中，生成两个大素数和一个补充值作为公钥。任何人都可以用公钥加密信息，但是只有那些有质因数的人才能解码信息。

这个过程有三个阶段:密钥生成、加密和解密。

*   密钥生成:生成公钥(共享)和私钥(保密)。生成的密钥的构造方法也应该是保密的。

*   加密:秘密消息可以通过公钥加密。

*   解密:只有私钥可以用来解密消息。

以下是该算法的概述:

1.  选择两个(通常是大的)素数， `p` 和 `q` 。

    1.  `p` 和 `q` 的乘积记为 `n` 。

    2.  ( `p` -1)和( `q` -1)的乘积表示为 `φ`。

2.  选择两个指数， `e` 和 `d` 。

    1.  `e` 通常为 3。可以使用大于 2 的其他值。

    2.  `d` 是使得 `(e × d) % phi = 1` 的值。

```js
Encryption
process is as shown:
        m - message:
        m^e % n = c
        c - encrypted message

Decryption process is as shown:
        c^d % n = m
```

这是计算 `d` 的实现:

```js
function modInverse(e, phi) {
    var m0 = phi, t, q;
    var x0 = 0, x1 = 1;

    if (phi == 1)
        return 0;

    while (e > 1) {
        // q is quotient
        q = Math.floor(e / phi);

        t = phi;

        // phi is remainder now, process same as
        // Euclid's algo
        phi = e % phi, e = t;

        t = x0;

        x0 = x1 - q * x0;

        x1 = t;
    }

    // Make x1 positive
    if (x1 < 0)
        x1 += m0;

    return x1;
}
modInverse(7, 40); // 23
```

还需要生成公钥和私钥的密钥对。

```js
function RSAKeyPair(p, q) {
    // Need to check that they are primes
    if (! (isPrime(p) && isPrime(q)))
        return;

    // Need to check that they're not the same
    if (p==q)
        return;

    var n = p * q,
        phi = (p-1)*(q-1),
        e = 3,
        d = modInverse(e,phi);

    // Public key: [e,n], Private key: [d,n]
    return [[e,n], [d,n]]
}
```

让我们选择 5 和 11 作为质数，看看`message`是 50 的例子。

```js
RSAKeyPair(5,11); //Public key: [3,55], Private key: [27,55]

p = 5, 11
n = p x q = 55
phi = (5-1) x (11-1) = 4 x 10 = 40
e = 3
(e x d) % phi = 1 (3 x d) % 40 = 1
(81) % 40 = 1\. 81 = 3 x d = 3 x 27
d = 27

Encryption:
    m - message: 50
    m^e % n = c
    50³ % 55 = 40

Encrypted message.,c:
    40
Decryption:
c^d % n = m
40²⁷ % 55 = 50
```

这完全加密了 50，而接收者可以将其解密回 50。通常，对于 RSA 算法，所选择的素数非常大。这是因为大数的质因数分解需要很长的计算时间。今天的标准是使用 4096 位的 prime 产品。即使是先进的计算机，计算它的质因数也需要数年时间。图 4-5 显示了 4096 位数的最大可能值。

![img/465726_1_En_4_Fig5_HTML.jpg](img/465726_1_En_4_Fig5_HTML.jpg)

图 4-5

2 <sup>4096</sup>

### 摘要

本章涵盖了各种本机实现的字符串函数，并在表 4-1 中进行了总结。

表 4-1

字符串函数摘要

| 功能 | 使用 |
| --- | --- |
| `charAt(index)` | 在`index`访问单个字符 |
| `substring(startIndex, endIndex)` | 访问从`startIndex`到`endIndex`的部分字符串 |
| `str1 > str2` | 如果`str1`在字典上比`str2`大，则返回`true` |
| `indexOf(str, startIndex)` | 从`startIndex`开始的期望`str`的索引 |
| `str.split(delimiter)` | 用指定的`delimiter`将一个字符串拆分成一个数组 |
| `str.replace(original,new)` | 用`new`替换`original` |

此外，JavaScript 原生`Regex`对象可以用于常用的字符串验证。表 4-2 提供了一个总结。

表 4-2

正则表达式摘要

| 正则表达式模式 | 使用 |
| --- | --- |
| `/\d+/` | 任何数字字符 |
| `/^\d+$/` | 仅数字字符 |
| `/^[0-9]*.[0-9]*[1-9]+$/` | 浮点数字字符 |
| `/[a-zA-Z0-9] /` | 仅字母数字字符 |