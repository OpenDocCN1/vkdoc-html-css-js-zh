# 核心原则

## 接口与实现

Java 使用接口以一种典型的方式来实现大多数面向对象语言的机制，因此我们将从这里开始。

这里有一个来自 `java.io` 包中的接口示例：`DataOutput`。

- `void writeBoolean(boolean value)` 抛出 `IOException`
- `void writeByte(int value)` 抛出 `IOException`
- `void writeChar(int value)` 抛出 `IOException`
- `void writeInt(int value)` 抛出 `IOException`
- `void writeDouble(int value)` 抛出 `IOException`

---

它是一个方法列表，一个类应该通过这些方法和参数以及异常来实现。每个方法看起来都类似于一个方法声明，唯一不同的是它以分号结束，而不是一对花括号。

创建使用此接口的类需要实现 `DataOutput` 中的所有方法。一个实现 `DataOutput` 的公共类 `DataOutputStream` 扩展了 `FilterOutputStream` 并实现了 `DataOutput`。

---

`public class DataOutputStream extends FilterOutputStream implements DataOutput`：

- `public final void writeBoolean(boolean v)` 抛出 `IOException`
- `public final void writeByte(int v)` 抛出 `IOException`
- `public final void writeShort(int v)` 抛出 `IOException`
- `public final void writeChar(int v)` 抛出 `IOException`
- `public final void writeInt(int v)` 抛出 `IOException`

---

---

`public class RandomAccessFile implements DataOutput`：

- `public final void writeBoolean(boolean v)` 抛出 `IOException`
- `public final void writeByte(int v)` 抛出 `IOException`
- `public final void writeShort(int v)` 抛出 `IOException`
- `public final void writeChar(int v)` 抛出 `IOException`
- `public final void writeDouble(int v)` 抛出 `IOException`

---

## 无需方法声明

---

