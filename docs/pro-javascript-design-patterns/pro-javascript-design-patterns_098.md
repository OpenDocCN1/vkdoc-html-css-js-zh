# API 参考

## 构造函数

### `new PersonDirectory()`

创建一个新的 `PersonDirectory` 实例。

### `new PersonDirectory(config)`

使用配置对象 `config` 创建一个新的 `PersonDirectory` 实例。配置对象可以包含以下属性：
- `data`（可选）：初始人员数据数组。
- `currentPage`（可选）：当前页码，默认为 `1`。
- `pageSize`（可选）：每页显示的人员数量，默认为 `10`。

### `new PersonDirectory(config, parent)`

创建一个新的 `PersonDirectory` 实例，并将其挂载到 `parent` 元素下。

## 方法

### `addPerson(personData)`

添加一个新的人员。`personData` 应是一个包含 `name`、`age` 和 `email` 属性的对象。成功时返回 `true`，失败时返回 `false`。

### `getAllPersons()`

返回所有人员的数组。每个人员对象包含 `name`、`age` 和 `email` 属性。如果没有人员，则返回空数组。

### `findPersonByName(name)`

根据姓名查找人员。如果找到，返回该人员对象；否则返回 `null`。

### `deletePerson(name)`

根据姓名删除人员。成功时返回 `true`，失败时返回 `false`。

## 事件

### `personAdded`

当添加一个新的人员时触发。回调函数接收新添加的人员对象作为参数。

### `personDeleted`

当删除一个人员时触发。回调函数接收被删除的人员对象作为参数。
