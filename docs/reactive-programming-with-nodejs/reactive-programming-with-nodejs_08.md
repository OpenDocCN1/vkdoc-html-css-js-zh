# JavaScript 响应式编程与 `Streamable` 类

```javascript
function Streamable(triggerCode) {
  this.events = [];
  this.idx = 0;
}

Streamable.prototype.start = function(val) {
  this.idx = 0;
  this.next(val);
};

Streamable.prototype.next = function(val, result) {
  if (this.idx >= this.events.length) return false;
  var currentFn = this.events[this.idx];
  this.idx++;
  currentFn(val);
};

Streamable.prototype.catch = function(errfn) {
  this.error_fn = errfn;
};

Streamable.prototype.then = function(fn) {
  var self = this;
  var newFn = null;
  if (!fn.__type) {
    if (fn.length > 1) {
      newFn = function(val) {
        fn(val, function(err, result) {
          if (err) return self.error_fn(err);
          self.next(result);
        });
      };
    } else {
      newFn = function(val) {
        self.next(fn(val));
      };
    }
  } else {
    if (fn.__type === 'map') {
      newFn = function(val) {
        self.next(fn(val));
      };
    }
  }
  if (newFn) this.events.push(newFn);
  return this;
};
```

`Streamable` 类提供了著名的 `then` 方法，传递给它的每个函数都会被包装成另一个函数（该函数负责调用事件链中的下一个函数），并添加到一个列表中。一旦触发 `trigger` 事件，该类将遍历该列表并依次调用每个函数。

## 辅助 `map` 函数

```javascript
function map(fn) {
  var curried = function(val) {
    return fn(val);
  };
  curried.__type = 'map';
  return curried;
}
```

> **注意**：你可能已经注意到错误处理非常简单；当然，这是为了简化。最终的代码将包含所有必要的错误处理。

## 增强：使用属性定义替代 `getValues` 方法

为了使代码更具声明性，可以用带 `setter` 的属性定义来替代 `getValues` 方法。因此，我们直接移除该方法，并在构造函数中添加以下代码：

```javascript
function ObservableStream() {
  this.values = [];
  this.error_fn = null;
  EE.call(this);
  Object.defineProperty(this, 'data', {
    get: function() {
      var self = this;
      var stream = new Streamable();
      self.on('data', function(v) {
        stream.start(v);
      });
      return stream;
    }
  });
}
```

现在不再需要 `getValues` 方法，你可以直接使用 `data` 属性。

## 使用 `ObservableStream` 的示例

以下示例演示了如何使用上述代码转换和处理 `ObservableStream`：

```javascript
var fs = require("fs");
var dataStream = new ObservableStream();

dataStream.data
  .then(map(function(x) {
    return 'filename_' + x + '.txt';
  }))
  .then(fs.readFile)
  .then(map(function(x) {
    return '' + x;
  }))
  .then(console.log);

var stdin = process.stdin;
stdin.setRawMode(true);
stdin.resume();
stdin.on('data', function(input) {
  var str = input.toString();
  dataStream.add(str);
  if (str == 'x') {
    process.exit();
  }
});
```

上述代码包含两个主要部分。第一部分为 `ObservableStream` 进行设置：实例化它并建立转换链。第二部分代表外部数据源。

我们先介绍第二部分，因为从数据流的角度来看，这里是一切开始的地方。这段代码基本上允许你捕获用户每次按下键盘时触发的事件。利用该事件，将接收到的对象转换为 `String` 并添加到自定义集合中。额外的 `if` 语句用于确保可以在需要时停止监听并关闭程序。

这部分代码可以替换为任何其他实现，关键点在于：当数据到来时，你将其添加到可观察集合中。数据从哪里来并不重要。

现在，回到代码的第一部分，这里设置了处理数据流的函数链。这些函数非常直观，正如本章前面提到的：声明式代码更容易阅读和理解。

处理步骤如下：

1. 使用 `map` 函数将每个按键字符转换为文件名。
2. 将文件名传递给 `fs.readFile` 方法，读取对应文件。
3. 将返回的缓冲区转换为实际的 `String`。
4. 最后，将第 3 步的输出（读取的文件内容）传递给最后一步，通过 `console.log` 打印出来。

是的，逻辑非常简单，但它的美妙之处在于你只关注这些逻辑，而不必关心如何读取文件、处理错误等底层细节。

---

