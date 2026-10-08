# 常见问题 FAQ

## Q: 如何导入 PersonDirectory？

A: 您可以使用 `require` 或 `import` 语句：

```javascript
const PersonDirectory = require('person-directory');
// 或者
import PersonDirectory from 'person-directory';
```

## Q: 是否可以同时添加多个人员？

A: 目前不支持批量添加。您需要多次调用 `addPerson()` 方法，每次传入一个人员对象。

## Q: 人员数据存储在哪里？

A: 默认情况下，人员数据存储在内存中。如果您需要持久化存储，可以配置 `apiUrl` 连接后端数据库。

## Q: 如何自定义样式？

A: 您可以通过 CSS 覆盖默认样式。所有元素的类名都以 `pd-` 开头，例如 `.pd-card`、`.pd-button`。
