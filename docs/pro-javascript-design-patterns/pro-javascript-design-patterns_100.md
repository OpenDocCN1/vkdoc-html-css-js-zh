# 错误处理

## 常见错误

### 网络错误

如果网络连接失败，`addPerson()` 方法会抛出一个 `NetworkError`。您可以使用 `try...catch` 捕获它。

### 数据验证错误

如果 `personData` 缺少必填字段（如 `name`），`addPerson()` 方法会返回 `false`，并设置 `errorMessage` 属性为“缺少姓名”。

## 错误处理示例

```javascript
try {
  const result = directory.addPerson({ name: "李四" });
  if (!result) {
    console.error(directory.errorMessage);
  }
} catch (error) {
  console.error("发生错误：", error);
}
```
