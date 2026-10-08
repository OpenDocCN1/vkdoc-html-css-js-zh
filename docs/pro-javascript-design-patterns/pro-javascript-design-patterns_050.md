# 第三章：Bridger 中的桥接类

## 3.1 桥接类的作用

桥接类（Bridge Class）是一个特殊的类，它负责将接口调用映射到具体的实现。每个桥接类都对应一个接口，并持有该接口的一个私有实现实例。当客户端调用桥接类的方法时，桥接类会将调用委托给它的私有实例。

## 3.2 桥接类的示例

```java
// 定义一个 RPC 接口
interface Greeter {
    String sayHello(String name);
}

// 实现该接口
class GreeterImpl implements Greeter {
    @Override
    public String sayHello(String name) {
        return "Hello, " + name + "!";
    }
}

// 桥接类
class GreeterBridge implements Greeter {
    private Greeter impl;

    public GreeterBridge(Greeter impl) {
        this.impl = impl;
    }

    @Override
    public String sayHello(String name) {
        return impl.sayHello(name);
    }
}
```

然后，客户端直接调用 `GreeterBridge.sayHello(name)`，但底层会路由到 `GreeterImpl.sayHello(name)`。

## 3.3 桥接类与 RPC 实现

在 Bridger 框架中，桥接类的作用类似于一个“代理”。它包装了实际的 RPC 实现，使得客户端可以像调用本地方法一样调用远程服务。从客户端角度，`GreeterBridge` 就是 `Greeter` 的实例，但它的行为是透明的：如果是本地调用，它直接调用 `GreeterImpl`；如果是远程调用，它负责序列化、发送请求、接收响应并反序列化。

在实际应用中，桥接类可能包含更多细节，比如：

- 序列化参数和结果。
- 处理网络异常。
- 负载均衡或重试逻辑。
