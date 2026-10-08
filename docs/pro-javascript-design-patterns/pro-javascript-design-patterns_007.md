# 排版后的文本

## 消息

### 数据格式

**请求** `POST /api/xxx`，请求参数如下：

- `name`：字符串类型，必填
- `age`：整数类型，可选

**响应** `200 OK`，响应体为 JSON 格式：

- `code`：状态码
- `message`：提示信息
- `data`：具体数据对象

## 示例

请求示例：

```
POST /api/xxx
Content-Type: application/json

{
  "name": "张三",
  "age": 25
}
```

响应示例：

```
HTTP/1.1 200 OK
Content-Type: application/json

{
  "code": 0,
  "message": "成功",
  "data": {}
}
```

## 终端

### 命令列表

- `cmd arg0 arg1 arg2`：执行指定命令
- `ls -la`：列出当前目录文件
- `cd /path/to/xxx`：进入指定目录
- `ping example.com`：测试网络连通性

## 参数说明

- `arg0`：必填参数
- `arg1`：可选参数
- `arg2`：默认值为 `default`

## 代码

# 函数示例

```python
def func_name(param1, param2):
    result = param1 + param2
    return result
```

## 类示例

```java
public class ClassName {
    private String name;
    
    public ClassName(String name) {
        this.name = name;
    }
}
```

## 方法调用

```javascript
ClassName.funcName(var1 + var2, "cmd arg0 arg1")
```

## 描述

### 多边形

- `Intrinsic` 和 `Extrinsic` 状态
- 使用 `Factory` 实例化
- `Extrinsic` 状态封装在 `Manager` 中
- 管理 `Extrinsic` 状态

## 示例

```java
String name = "John";
int age = 30;
System.out.println("Name: " + name + ", Age: " + age);
```

## 附录

### 相关链接

- [官方网站](https://example.com)
- [API 文档](https://api.example.com/docs)

### 版本信息

- 版本：`v1.0.0`
- 更新日期：`2023-10-01`

### 注意事项
