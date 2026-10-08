# 异步事件流

标题也可以是“异步事件的流”。无论如何，本章这一节的目标是将我目前讨论的所有内容串联起来。

在第 3 章中，我展示了一些处理这类情况的基础代码，它们确实能工作。但由于 Node 已经提供了原生流，因此有必要概述所有选项，这样在后续章节中，你可以从这些不同的选项中汲取灵感，创建出优秀的库。

为了演示，我将使用一个名为 `event-stream` 的外部模块（可在 [`https://www.npmjs.com/package/event-stream`](https://www.npmjs.com/package/event-stream) 获取）。但别担心，它的一切基础都是 Node 的内置流，因此其原理依然适用。

该库本质上提供了一套辅助方法，让你能够操作和处理流，而这些流又可以轻松地处理异步任务。如果你还记得前几页的内容，`SaveUserToDB` 流无法被设置为 `Duplex` 流，相反，由于对数据库的异步调用，我不得不使用 `Transform` 流。

借助 `event-stream` 模块，修复这个问题不仅简单，而且实际上有几种方法可以实现。让我们先看看该库提供的所有辅助方法。

## `through`

该方法是对之前问题的解决方案。它创建了一个 `Duplex` 流，我们可以控制其中的数据流，从而让你将异步过程转化为同步过程。它接受两个可选参数：一个 `write` 函数和一个 `end` 函数。你会注意到没有 `read` 方法，这是因为它是一种特殊的 `Duplex`，具有预定义的 `read` 方法。它会重新发射你在 `write` 方法中通过 `data` 事件发射的任何内容。

让我演示一下。以下是 `SaveUserToDB` 流的重写方式：

```javascript
var es = require("event-stream");

var save = es.through(function write(data) {
    var self = this;
    this.pause();
    //You need to pause the stream, to make sure the saving operation is done
    UserModel.create(data, function(err, obj) {
        if(err) {
            return self.emit('error', err);
        }
        self.resume();
        //you can then resume the stream and emit the new object
        self.emit('data', obj);
    });
}, function end() {
    this.emit('end');
});
```

## `map`

该方法接受两个参数：实际数据和必须在函数末尾调用的回调函数。

我在前一章已经介绍了 `map` 函数的作用，因此在这种情况下，只需理解这个特定的 `map` 是一个自定义的 `through` 函数，它会重新发射你作为接收回调函数第二个参数传递的任何内容。了解这一点后，你可以再次重新实现 `SaveUserToDB` 流，如下所示：

```javascript
var es = require("event-stream");

var save = es.map(function(data, callback) {
    var self = this;
    UserModel.create(data, function(err, obj) {
        if(err) {
            //the first argument of the callback is the error returned
            return callback(err);
        }
        callback(null, obj);
    });
});
```

## `split`

该方法的工作方式与 `String#split` 方法类似，但其默认值是换行符，而不是空字符串。它不是分割单个字符串，而是将一个流分解并重新组装，以便结果分割的每个部分都是一个数据块。

一个简单的例子是，如果你想处理文本文件中的每个单词，可以这样做：

```javascript
var es = require("event-stream");
var fs = require("fs");

fs.createReadStream('myfile.txt')
    .pipe(es.split(' '))
    .pipe(es.map(function(word, cb) {
        cb(null, word.toUpperCase());
    }));
```

显然，你不需要仅仅为了将文档分割成单词而这样做，但这仍然可以作为示例。

## `join`

这个方法有点令人困惑，因为名称暗示它是 `split` 的反向操作，可以撤销你对它所做的任何操作。

该方法在每个发出的数据块后添加一个分隔符（它接受的唯一属性）。因此，实际上，对于每一块数据，它会发射两个新块：分隔符和实际数据。所以你可以这样做：

```javascript
var es = require("event-stream");
var fs = require("fs");

var writer = fs.createWriteStream('./output-join.txt');

fs.createReadStream('./event-stream-join.js')
    .pipe(es.split(" "))
    .pipe(es.map(function(str, done) {
        done(null, str.toUpperCase());
    }))
    .pipe(es.join("\n"))
    .pipe(writer);
```

实际上，上面的代码将文本文件分割成单词，将每个单词转换为大写，然后将每个单词保存到文件中，但末尾带有一个换行符。

**提示**

如果在 `join` 方法中传入一个 `String` 而不是函数，那么该函数将在流结束时被调用，它接收的唯一参数将是所有连接成单个 `String` 的数据块列表。

## `merge`

该方法负责合并多个流。这意味着它将合并每个传入流的结果数据，一旦所有流都发出了 `end` 事件，它就会结束。

一个简单的例子如下：

```javascript
var es = require("event-stream");
var fs = require("fs");

es.merge([
    fs.createReadStream('input1.txt'),
    fs.createReadStream('input2.txt')
]).pipe(fs.createWriteStream('output.log'));
```

注意，合并的流之间不保证顺序，因此保存的数据不会先包含一个文件的内容，然后再包含第二个文件的内容。两个文件的内容可能交织在一起（取决于每个文件内容的长度）。

## `readable`

正如方法名称所示，它创建一个 `Readable` 流，该流与一个异步函数配合作为数据源。换句话说，接收的函数（接受的唯一参数）接受两个参数：一个计数器和一个回调。该函数必须始终调用回调以推进数据流，并且只能调用一次；后续调用将无效。

然而，它可以在一次调用中使用 `data` 事件发射多个数据块。以下是你的 `read` 函数的基本结构：

```javascript
var es = require("event-stream");

es.readable(function(count, callback) {
```

if(hasEnded) { //for some reason the stream is no longer reading

this.emit('end');

}

//... your code here

this.emit('data', chunk); //you can do this as many times as you need to.

callback();

});

```

**注意**

在前面的代码示例中，变量`hasEnded`仅仅是某种指示流已结束的逻辑条件的占位符。你需要将其替换为对业务逻辑有意义的任何内容。

该模块还提供了其他方法，如`duplex`、`child`、`wait`、`parse`等，但主要方法就是上面这些。如果你想了解更多，可以查看该模块 npm 页面上的完整文档（可在[`https://www.npmjs.com/package/event-stream`](https://www.npmjs.com/package/event-stream)获取）。

## 结论

正如你所见，该模块提供的辅助方法与第 3 章中描述的方法非常相似，但同时也有所不同，因为它们是创建新的流，而不是修改流经某个流的数据。

这个模块和之前讨论的可观察流（Observable Streams）将用于创建本书响应式库的最终版本。

## 总结

希望本章能让你对 Node.js 中一个常被误解且常被遗忘的特性有所了解。

内置流将成为在后端处理响应式代码的绝佳工具，正如你在前几章中所见。所以现在你知道了如何使用它们，就可以开始自己动手实践了！

下一章将涵盖目前专门处理 JavaScript（尤其是 Node.js）中响应式编程的主要库。

**脚注**  
\[1\] （当然，这只是一个粗略的近似，因为要得到精确的数字，还需要考虑并行函数所增加的时间。）
