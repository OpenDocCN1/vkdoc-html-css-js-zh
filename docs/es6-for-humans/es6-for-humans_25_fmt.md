# padStart 和 padEnd

`padStart()` 和 `padEnd()` 方法是 ES2017 中引入的两个新的字符串方法，它们有助于向字符串添加填充，以便结果字符串的长度等于作为第一个参数传入的值。你还可以传入第二个可选参数（另一个字符串）来代替默认的空格进行填充。思考以下示例：

```javascript
'string'.padStart(10);           // "    string"
'string'.padStart(10, "abc");    // "abcastring"
'string'.padStart(10,"123465");  // "1234string"
'string'.padStart(8, "0");       // "00string"
'string'.padStart(3);            // "string"
'string'.padEnd(10);             // "string    "
'string'.padEnd(10, "abc");      // "stringabca"
'string'.padEnd(8, "123456");    // "string12"
'string'.padEnd(1);              // "string"
```

正如你在上面的示例中所看到的，添加到给定字符串的填充使得最终结果字符串的长度要么大于第一个参数的长度，具体取决于原始字符串的长度；如果给定的长度更大，则会根据你使用的方法在字符串的开头或结尾添加适当数量的填充。你也可以使用第二个参数来使用不同的字符串代替空格进行填充。