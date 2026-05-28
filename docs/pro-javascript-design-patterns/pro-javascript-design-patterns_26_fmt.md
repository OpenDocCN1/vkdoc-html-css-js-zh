# JavaScript

## Book.prototype.display

`Book.prototype.display` 是一个函数，用于显示本书的标题、作者和 ISBN。它将以 `String` 形式返回这三个属性的值。此外，它还会检查所有属性是否已定义。如果未定义，则会抛出一个新的 `Error("Book: invalid ISBN")`。

-   如果 `Book` 是一个有效的 ISBN，则返回一个字符串。

-   如果 `Book` 是一个无效的 ISBN，则抛出一个新的 `Error`。

`this.isbn` 是一个字符串，包含正确的 ISBN 数字和校验和。

## Book.display

`Book.display` 是一个静态函数，它需要传递一个字符串参数。如果该字符串的长度等于 13 并且以数字开头，则返回 `true`，否则返回 `false`。

```
function Book.display(isbn) {
    return isbn.length === 13 && /^[0-9]/.test(isbn);
}
```

---

## CheckIsbn

`CheckIsbn` 函数用于验证 ISBN。如果传递的 `isbn` 是一个字符串，并且长度等于 13，并且所有字符都是数字，则返回 `true`，否则返回 `false`。

`isbn` 是一个字符串，可以包含数字和字母，但通常只有数字是有效的。

`Book.prototype.display` 函数在检查 ISBN 时，如果 ISBN 无效，会抛出一个错误。如果 `Book` 对象没有定义 `title` 或 `author` 属性，则 `display` 方法会返回一个错误。

## 问题

最大的问题是验证数据的完整性和正确性。在构造 ISBN 时，我们需要确保其格式正确，并且校验和匹配。

## 解决方案

使用 `CheckIsbn` 函数来验证 ISBN 的有效性。该函数会检查字符串的长度和字符是否为数字。如果验证失败，则抛出一个 `Error("invalid ISBN")`。

`CheckIsbn` 函数还可以检查 ISBN 的前缀。例如，对于 `978` 前缀，它需要 13 个字符。如果不匹配，则返回 `false`。

## 示例

```
for (var i = 0; i < 10; i++) {
    sum += CheckIsbn(isbn.charAt(i) * (10 - i));
}
if (CheckIsbn(isbn) && display(match, "978") && ensure characters are digits) {
    return true;
}
else {
    return false;
}
```

---

## 另一个例子

```
if (this.isbn.length === 13 && this.isbn.match(/^[0-9]{13}$/)) {
    return isbn;
}
else {
    throw new Error("invalid ISBN");
}
```

## 更多示例

```
for (var i = 0; i < 10; i++) {
    sum += isbn.charAt(i) * (10 - i);
}
if (sum % 11 === 0) {
    return true;
}
else {
    return false;
}
```

---

```
for (var i = 0; i < 13; i++) {
    sum += isbn.charAt(i) * (i % 2 === 0 ? 1 : 3);
}
if (sum % 10 === 0) {
    return true;
}
else {
    return false;
}
```

---

```
return true;
// All tests passed.
```

---