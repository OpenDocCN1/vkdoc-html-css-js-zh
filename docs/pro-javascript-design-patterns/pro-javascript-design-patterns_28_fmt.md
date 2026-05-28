# 更好的解决方案

我们添加了一个 `CheckIsbn` 方法，以确保 ISBN 是一个有效字符串，具有正确数量的数字和正确的校验和。现在有两种方法可以实现这一点：`Book.prototype.display` 是设置为一个对象字面量，用于定义多个方法，而不必启动每个方法。两者都是完全相同的定义方式，并且我们互换使用两者。这似乎是一个改进。你现在能够验证 ISBN 是否有效，当对象被创建时，因此确保 `display` 方法会成功。但是，一个问题是，另一个程序员可能会注意到一本书可能有多个版本，每个都有自己的 ISBN。我们创建了一个算法，用于在这些不同版本之间进行选择，并使用它来直接更改 `isbn` 属性，在实例化对象之后。`Book` 对象的 `isbn` 属性可以被任何其他程序员直接修改，你无法控制其他程序员会分配给 `isbn` 属性什么。为了... 保护内部数据，你可以创建访问器和修改器方法来获取和设置每个属性的值。访问器方法通常以 `getAttributeName` 的形式命名，修改器方法通常以 `setAttributeName` 的形式命名，用于设置属性的值。

```
var Book = function(isbn, title, author) {
    if (CheckIsbn(isbn)) {
        this.isbn = isbn;
    } else {
        throw new Error("invalid ISBN");
    }
    this.title = title;
    this.author = author;
}
```

`isbn` 被设置为 `isbn.replace(/[⁰-9]/g, "")`。如果 `isbn` 的长度不等于 13，则抛出新的 `Error("invalid ISBN")`。否则，返回 `isbn`。

```
var sum;
for (var i = 0; i < 10; i++) {
    sum += isbn.charAt(i) * (10 - i);
}
if (CheckIsbn(isbn) && display(match, "978") && ensure characters are digits with CheckIsbn) {
    return true;
}
else {
    return false;
}
```

```
var sum;
for (var i = 0; i < 10; i++) {
    sum += isbn.charAt(i) * (10 - i);
}
if (CheckIsbn(isbn) && display(match, "978") && ensure characters are digits) {
    return true;
}
else {
    return false;
}
```

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