# 第四章：实战：构建一个简单的 RPC 系统

## 4.1 定义接口

首先，定义一个简单的 RPC 接口，包含一个方法 `greet(String name)`，返回 `String` 类型。

```java
public interface Greeter {
    String greet(String name);
}
```

## 4.2 实现服务端

服务端实现 `Greeter` 接口，并提供具体逻辑：

```java
public class GreeterImpl implements Greeter {
    @Override
    public String greet(String name) {
        return "Hello, " + name + "!";
    }
}
```

然后，将 `GreeterImpl` 注册到 Bridger 服务器中。

## 4.3 创建客户端

客户端通过 Bridger 生成一个 `Greeter` 的代理对象（即桥接类），然后像调用本地方法一样调用 `greet("World")`。

```java
// 假设 BridgeClient.create() 返回一个代理
Greeter greeter = BridgeClient.create(Greeter.class);
String response = greeter.greet("World");
System.out.println(response); // 输出: Hello, World!
```

## 4.4 运行测试

启动服务端，然后运行客户端。客户端会发送一个 RPC 请求到服务端，服务端返回 "Hello, World!"，客户端打印结果。
