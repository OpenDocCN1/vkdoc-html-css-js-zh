# 第五章：桥接类的高级用法

## 5.1 多接口桥接

有时，你需要将多个接口桥接到同一个实现中。Bridger 支持多接口桥接，你可以定义多个接口，然后将它们组合到一个桥接类中。

```java
interface ServiceA {
    String a();
}

interface ServiceB {
    String b();
}

// 桥接类
class BridgeAB {
    private ServiceA implA;
    private ServiceB implB;

    public BridgeAB(ServiceA implA, ServiceB implB) {
        this.implA = implA;
        this.implB = implB;
    }

    public String doA() { return implA.a(); }
    public String doB() { return implB.b(); }
}
```
