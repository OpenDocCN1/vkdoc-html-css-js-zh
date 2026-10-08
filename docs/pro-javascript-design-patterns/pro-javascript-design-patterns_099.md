# 配置

## 配置属性

### `config.apiUrl`

设置 API 的基础 URL。默认为 `"http://localhost:3000/api"`。

### `config.timeout`

设置请求超时时间（毫秒）。默认为 `5000`。

### `config.retryCount`

设置请求失败时的重试次数。默认为 `3`。

## 配置示例

```javascript
const config = {
  apiUrl: "https://my-api.example.com/v2",
  timeout: 10000,
  retryCount: 5
};

const directory = new PersonDirectory(config);
```
