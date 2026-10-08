# JavaScript 异步编程演进：从回调到 `async/await`

## 从回调到 `Promise`

最初的异步代码通常使用回调函数，这可能导致“回调地狱”，使代码难以阅读和维护。`Promise` 的出现提供了一种更清晰的方式来处理异步操作。

```javascript
getMail().then(mail => {
  console.log(mail);
});
```

这样，代码的逻辑更加清晰，并且可以避免回调地狱。

## 迁移到 `async/await`

为了进一步简化代码，我们可以使用 `async/await` 语法，这能让异步代码看起来像同步代码。

```javascript
async function fetchMail() {
  try {
    const mail = await getMail();
    console.log(mail);
  } catch (error) {
    console.error(error);
  }
}
```

## 总结

通过将 `getMail` 方法从回调风格迁移到 `Promise` 和 `async/await` 风格，我们不仅提高了代码的可读性和可维护性，还简化了错误处理流程。这是现代 JavaScript 开发中推荐的做法。

-   只返回处理后的内容，不要重复输出原文，也不要添加额外说明。

## 示例

原文：

进入 `/path/to/xxx` 目录，找到 `xxx.json`。

在表达式 `cvar = avar + bvar` 中，加法运算符（`+`）将 `avar` 与 `bvar` 相加，得到它们的和 `cvar`。

在 `List.of(arg0, arg1, arg2)` 中，`List` 接口的工厂方法 `of()` 接受一系列的元素，返回包含它们的只读列表。

之后我们这样调用 `cmd` 命令：`cmd arg0 arg1 arg2`。

```
if (condVar > someVal) {console.log("xxx")}
```
