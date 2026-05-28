# 定义模型

```python
class LinearModel(paddle.nn.Layer):
    def __init__(self):
        super(LinearModel, self).__init__()
        self.linear = paddle.nn.Linear(1, 1)

    def forward(self, x):
        return self.linear(x)
```