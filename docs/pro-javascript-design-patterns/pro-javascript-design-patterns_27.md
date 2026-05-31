# 结论

通过使用 `CheckIsbn` 方法，我们能够确保 ISBN 是有效的字符串，包含正确数量的数字和正确的校验和。这样，我们就可以在构造函数中使用该方法来验证 ISBN，并抛出错误以保护内部数据。我们还可以创建访问器和修改器方法来获取和设置每个属性的值。访问器方法通常被命名为 `getAttributeName`，修改器方法被命名为 `setAttributeName`，用于设置属性的值。

## 最终解决方案

`this.isbn` 被设置为 `isbn.replace(/[⁰-9]/g, "")`。如果 `this.isbn` 的长度不等于 13，或者 `this.isbn.match(/^[0-9]{13}$/)` 不匹配，则抛出新的 `Error("invalid ISBN")`。否则，继续执行构造函数。

`isbn` 被设置为 `isbn.replace(/[⁰-9]/g, "")`。如果 `isbn.length` 不等于 13，则抛出新的 `Error("invalid ISBN")`。如果 `isbn.match(/^[0-9]{13}$/)` 不匹配，则抛出新的 `Error("invalid ISBN")`。否则，返回 `isbn`。

---

