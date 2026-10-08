# CompositeFormatter 使用指南

## 创建实例与基本操作

```java
var compositeFormatter = new CompositeFormatter();
compositeFormatter.allOf(formatters);
```

然后在表单的其余部分中，你可以直接调用`compositeFormatter.restore()`来恢复之前保存的状态。

## 将所有操作添加到`CompositeFormatter`

### 使用`allOf()`方法

```java
allOf(Formatter... formatters)
```

对于`allOf`操作，它接受一个或多个`Formatter`实例，并返回包含它们的只读列表。

### 使用`allOrNothing`方法

```java
allOrNothing(Formatter... formatters)
```

对于`allOrNothing`操作，它接受一个或多个`Formatter`实例，并返回包含它们的只读列表。如果任何子操作失败，整个操作都会失败。

### 使用`anyOf`方法

```java
anyOf(Formatter... formatters)
```

对于`anyOf`操作，它接受一个或多个`Formatter`实例，并返回包含它们的只读列表。如果有任何一个子操作成功，整个操作就会成功。

### 使用`noneOf`方法

```java
noneOf(Formatter... formatters)
```

对于`noneOf`操作，它接受一个或多个`Formatter`实例，并返回包含它们的只读列表。如果所有子操作都失败，整个操作才会成功。

### 使用`static`方法

```java
static(Formatter... formatters)
```

对于`static`操作，它接受一个或多个`Formatter`实例，并返回包含它们的只读列表。如果任何子操作失败，整个操作都会失败。

### 使用`list`方法

```java
list(Formatter... formatters)
```

对于`list`操作，它接受一个或多个`Formatter`实例，并返回包含它们的只读列表。如果有任何一个子操作成功，整个操作就会成功。

### 使用`passThrough`方法

```java
passThrough(Formatter... formatters)
```

对于`passThrough`操作，它接受一个或多个`Formatter`实例，并返回包含它们的只读列表。所有子操作都会执行，但结果取决于最后一个子操作。

## 保存和恢复状态

### 使用`save`和`restore`方法

`CompositeFormatter`的`save`方法会保存当前状态，并将其重置为空状态。`restore`方法会恢复之前保存的状态。

```java
if (this->compositeComponents.isEmpty()) {
    return null;
} else {
    return this->compositeComponents.get(this->compositeComponents.size() - 1);
}
```

如果`this->compositeComponents`为空，则返回`null`；否则返回最后一个子元素。

### 使用`save`和`restore`方法的示例

```java
save();
// ... 执行一些操作 ...
restore();
```

在`save`和`restore`之间，你可以安全地修改状态，而不会影响之前保存的状态。

## 总结

`CompositeFormatter`是一个强大的工具，用于将格式化操作链接到一个复合表单中。它允许你以模块化的方式构建复杂的格式化逻辑。

## 附录

### 方法列表

- `allOf()` - 添加所有操作
- `allOrNothing()` - 添加所有或全部失败的操作
- `anyOf()` - 添加任一操作成功即可的操作
- `noneOf()` - 添加所有操作都失败才成功的操作
- `static()` - 添加静态操作
- `list()` - 添加列表操作
- `passThrough()` - 添加透传操作
- `save()` - 保存当前状态
- `restore()` - 恢复之前保存的状态

### 注意事项

- 所有操作都是只读的，不会修改原始数据
- 操作可以嵌套，形成复杂的格式化逻辑
- 使用`save`和`restore`方法可以安全地管理状态

`CompositeFields`是`Composite`的一种特殊形式。它们用于将多个相关字段组合在一起，并让你能够选择和操作这些字段的任意子集，以及删除它们的关联信息。

`CompositeFields`的接口本质上是在内部使用`Form`结构。`Composite`可以用在`Fox`、`FoxPro`的`Cloud`组件中，或者用在基于`C++`的在线环境中，但通常它用于工业开发或低代码开发环境中的字段，也用于通用编码库。

这里是`CompositeFields`的列表，你可以从中选择：

- `ContactForm`和`NestedContactForm`是两个`Composite`字段。
- `form.contract`是`ContractForm`的一个实例。
- `contract.Nested`是`NestedForm`的一个实例。
- `NestedForm.all()`调用返回所有`Composite`字段。
- `NestedForm.all()`调用，当其后的`restore`参数为`submitted`时，将返回列表。`NestedForm.all()`将在`Nested`属性为`null`时返回，在这样的情况下，`NestedForm.all()`将只返回一个包含`Nested`字段的封闭`Composite`字段。
- `form.contract.all()`调用返回`first`和`last`的`Name`实例。
- `form.contract.all()`调用返回`City`和`State`的`Address`实例。
- `form.contract.all()`调用返回`Comment`字段。
- `form.all()`调用返回`Comment`字段。
- `form.all()`调用返回`Zip`和`Zip`的`State`实例。
- `form.all()`调用返回`Zip`实例。
- `form.all()`调用返回`Type`字段。
- `form.all()`调用返回`Zip`实例。
- `form.all()`调用返回`Contents`字段。
- `form.all()`调用返回`Packer`和`Packer`的`State`。
- `form.all()`调用返回`Nextarea.contents`和`Nextarea`的`Blocks`。
- `form.all()`调用返回`Packer`字段。
- `form.all()`调用返回`Packer`和`Rental`的`Blocks`。
- `form.all()`用于`form.all()`返回的`Nested`在`class`属性中。
- `form.all()`调用返回`Nested`的`form`。
- `form.all()`调用返回`Nested`的`Block`字段。
- `form.all()`调用返回`Nested`的`Block`字段。
- `form.all()`用于当你是一个所有字段的列表时，不会返回。

你也可以在一个`CompositeFields`实例内部封闭一个字段。

## `CompositeFields`规则可以将对象组合起来，而不是直接隔离它们。

- `NestedForm.all()`返回的`Nested`实例将重写其子字段。

`form.contract.all()`调用返回所有`ContractForm`及其后代`NestedForm`的`Nested`实例。所以，如果你想获取所有`ContractForm`及其后代`NestedForm`的`Nested`实例，您可以使用`form.contract.all()`。您也可以使用`form.contract.all()`来返回所有包含`Contents`属性的`ContractForm`及其子`Nested`实例。

这是`form.contract.all()`的一个使用示例，它返回了所有`ContractForm`及其后代`NestedForm`的`Nested`实例。

## `Composite`形式与语言

`form.contract.all()`调用返回`firth`元素。

```
if (tag.some != null) { tag.save(); }
```

## 用于

`form.contract.all()`调用返回`form.contract.all()`的`Nested`实例列表。

## 命令环境

`remove()`是`form`类的一个方法。

## 作用

`remove`方法用于移除`NestedForm.all`中的`Nested`实例。

```
form.remove(NestedForm.all);
```
