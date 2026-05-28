# 处理 URL 的查询字符串

## 练习 2
创建一个正则表达式，将 URL 的查询字符串转换为一个键值对对象，使得如下形式：

```
?key1=value1&key2=value2&...&keyN=valueN
```

变成：

```
{
key1: 'value1',
key2: 'value2',
...
keyN: 'valueN'
}
```

你可以假设文本以问号开头。键的长度均为正数。值的长度可为非负数。等号始终存在。

## 解决方案
首先，我们需要读取一个问号或一个与号，后跟任意长度的、非等号字符列表。我们需要捕获与号之后的字符。

```
/?&+/g
```

接着匹配一个等号，后跟任意数量的非与号字符。我们需要捕获这些字符。

```
/?&+=([^&]*)/g
```

每个键最终位于捕获组 1 中，每个值最终位于捕获组 2 中。由于表达式是全局的，我们将逐一匹配键值对。

现在，我们将使用 JavaScript 创建结果对象。

```javascript
let queryParameters = {};
let queryString = '?key1=value1&key2=value2&key3=&key4=value4';
let regex = /?&+=([^&]*)/g;
let result = null;
while ( result = regex.exec( queryString ) ) {
    queryParameters[ result[1] ] = result[2];
}
console.log( queryParameters );
```