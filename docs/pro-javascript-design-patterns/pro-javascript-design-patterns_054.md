# 第六章：Bridger 的限制与注意事项

尽管 `Bridger` 提供了极大的便利，但它并非万能。例如，`BridgeClass` 并不是一个已经实现好的类，它只是 `Bridger` 的概念工具。你可以将整个过程理解为：

- 框架自动为你生成 `GreeterBridge` 类。
- 客户端通过 `GreeterBridge` 调用方法，但底层路由到 `GreeterImpl`。

此外，`Bridger` 的目的是简化远程调用，但在一些复杂场景下，比如需要细粒度的错误处理或流式传输，你可能需要直接使用 gRPC 或自定义协议。

---

> 代码块示例

```java
var public Bridge = function() { ... }

var secret = "private"

this.publicGreet = function() { ... }

this.secretGreet = function() { ... }

var BridgeClass = function(a, b, c) {
    this.a = a;
    this.b = b;
    this.c = c;
}

var BridgeClass2 = function(d) { this.d = d; }

var BridgeClass3 = function(a, b, c, d) {
    this.one = new BridgeClass(a, b, c);
    this.two = new BridgeClass2(d);
}
```

这看起来很像一个适配器（`Adapter`）模式。但 `Bridger` 的实现方式更贴近于框架自身的约定。

> 公平地说，`Bridger` 并不是一个已经实现的库，它只是本文中用于讲解的概念工具。它被引入是为了帮助开发者理解 RPC 的抽象层。

有人可能会认为，`Bridger` 只是为方便而引入的，它并不是一个真正的类。但通过桥接类，你可以有效地将多个类组合在一起，从而无需侵入 Dirty Water（指底层实现）的细节。事实上，你可以将 `Bridger` 视为一层轻量的抽象，它提供了对接口和实现的清晰分离。

---
