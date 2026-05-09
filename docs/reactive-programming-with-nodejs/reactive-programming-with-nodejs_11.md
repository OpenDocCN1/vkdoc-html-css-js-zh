# 不断取用

此处提供了 `take` 函数的三种变体。第一种是简单的 `take` 函数，它会从数据流中接受固定数量的数据块，然后终止数据流。第二种是 `takeUntil` 函数，它会从数据源中接受数据块，直到作为参数传入的函数所定义的条件满足为止（基本上，当该函数返回 `true` 时）。最后是 `takeWhile`，它与 `takeUntil` 非常相似，但条件正好相反：只要条件满足，它就会接受数据块，此后，数据将无法通过此节点。

首先，让我们看看它们的代码：

```
function take(number) {
var counter = 0;
var max = number;
var _fn = function(stream, value, done) {
counter++;
if(counter == max) {
stream.end();
}
return done(null, value);
};
_fn.__fn_type = 'take';
_fn.newInstance = function() {
return take(number);
};
return _fn;
}
function takeUntil(condition) {
var _fn = function(stream, value, done) {
if(condition.length === 1) {
if(condition(value)) {
return stream.end();
}
return done(null, value);
} else {
condition(value, function(err, resp) {
if(!!resp === true) {
return stream.end();
}
return done(err, value);
});
}
};
_fn.__fn_type = 'take';
_fn.newInstance = function() {
return takeUntil(condition);
};
return _fn;
}
function takeWhile(condition) {
var _fn = function(stream, value, done) {
if(condition.length === 1) {
if(!condition(value)) {
return stream.end();
}
return done(null, value);
} else {
condition(value, function(err, resp) {
if(!!resp === false) {
return stream.end();
}
return done(err, value);
});
}
};
_fn.__fn_type = 'take';
_fn.newInstance = function() {
return takeWhile(condition);
};
return _fn;
}
```

它们的代码彼此非常相似，甚至共享同一个函数类型标识符，因为主库可以用相同的方式处理这三种函数。最后，`takeUntil` 和 `takeWhile` 都接受异步和同步函数作为决定何时停止的条件。图 7-1 给出了这三种变体行为方式的示例。

![A418015_1_En_7_Fig1_HTML.jpg](img/A418015_1_En_7_Fig1_HTML.jpg)

**图 7-1.** 大理石图展示了三种 `take` 方法之间的差异

让我们快速查看一下这些方法的代码示例：

```
var cheese = require("./cheese").Cheese;
var _ = require("lodash");
var take = require("./cheese").Utils.take;
var takeUntil = require("./cheese").Utils.takeUntil;
var takeWhile = require("./cheese").Utils.takeWhile;
var ch = new cheese();
var ch1 = new cheese();
var ch2 = new cheese();
ch.fromArray(_.range(100)).then(take(10)).each(function(v) {
console.log("Ch: ", v.toString());
});
ch1.fromArray(_.range(100)).then(takeWhile(function(v) {
// the value needs to be coerced into an integer, because all data that flows through the stream is casted into a String .
return +v  10;
}))
.each(function(v) {
console.log("Ch2: ", v.toString());
})
```

正如你可能已经猜到的那样，这三种变体都做着同样的事情：只取数据流的前 10 个元素，然后关闭它。

