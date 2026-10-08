# 静态工厂模式（创建或检索对象实例）

```
// 私有静态方法
```

`this.getCONSTANTS()`

```
return CONSTANTS
```

```
//
```

```
// 返回构造函数。
```

`return function(constructor Argument)`

```
//
```

```
//
```

# 如果你有大量常量并且不想为每个常量都创建一个访问器方法

## 你可以创建一个通用的访问器方法。

```javascript
var Class = getClass()
// 私有静态属性
var constants = CONSTANTS(100, 200)
//
// 私有静态方法
this.getConstant(name)
return constants(name)
//
// 返回构造函数。
return function(constructor Argument)
//
//
```

# 然后你可以通过调用单一的访问器来获取一个常量

`Class.getConstant(CONSTANTS.CONSTANT_A)`
