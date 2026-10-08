# 4. 使用流迁移大量数据

在前一章中，我们确保了用户会喜欢我们的 CLI，并发布了第一个版本。现在我们来探讨一个更高级的话题：流。流是 Node.js 中处理大量数据的一个非常强大的特性。使用传统的缓冲方式，我们很快就会遇到内存问题，因为所有数据无法全部装入计算机内存。而流使我们能够分片处理数据。Node.js 流的工作方式类似于终端中的 Unix 流，你可以使用管道符号（`|`）将数据从生产者传输到消费者。我们将通过探索 `cat` 命令来了解管道的工作原理。随后，我们将创建自己的流并将其集成到命令行客户端中。

## cat 命令

`cat` 程序将文件内容输出到 `stdout`。在这个例子中，我将 `cat` 程序的输出通过管道传给 `tr`，将 `package.json` 中的所有字母转换为大写：

```
$ cat sourcecode/first-release/package.json | tr 'a-z' 'A-Z'
```

它适用于 `stdout` 上的所有输出：

```
$ echo "i shout?" | tr 'a-z' 'A-Z'
```

Unix 和 Node.js 中的流使我们能够组合出只做好一件事的小型程序或模块，从而完成任务。它们处理背压，这意味着当快速生产者连接到慢速消费者时，生产者会自动减速。

Node 中最常用的流是 `Readable`、`Writable` 和 `Transform` 流。它们是可用于构建自定义流的基类。其他流类型，例如 `Duplex` 流和 `Passthrough` 流，本书不涉及。`Readable` 流用于读取输入数据，`Transform` 流通常用于修改数据块，而 `Writable` 流接收数据并将其写入某处（例如文件）。

今天我们将编写一个命令，使用流将 CSV 文件中的数据通过管道传输到 CouchDB/PouchDB 中。我们也可以编写一个导入器来从数据库（例如 Postgres 或 MongoDB）迁移数据，但使用纯 CSV 文件则无需安装新的数据库。该原理同样适用于文件和数据库这两种数据源。在本章末尾，我将提供一个示例链接，展示如何构建 Node.js 流管道，将数据从 MongoDB 迁移到 CouchDB/PouchDB。

## 第一个流

构建流有时会有点棘手。我们首先在模块的根目录下创建文件 `stream-example.js`。

我们的第一个迭代版本会将 CSV 内容输出到 `stdout`。我们将用它来学习流的工作原理，并在此基础上构建导入器。开发时，我们需要一个 CSV 文件；可以将其保存到 `test/fixtures/test.csv`：

```
time;location
march;austin,us
april;boston,us
october;bristol,uk
february;hermigua,es
march;hermigua,es
april;havana,cu
```

幸运的是，我们不需要自己编写流式 CSV 解析器：

```
$ npm install --save csv-parse
```

我们引用 `fs` 和 `util` 模块。`fs` 模块用于从磁盘读取 CSV 文件。我们还需要引用 CSV 解析器：

```
const parse = require('csv-parse');
const fs = require('fs');
const Transform = require('stream').Transform;
```

我们的自定义流名为 `MyTransformStream`，继承自 `stream.Transform`。我们将流设置为 `objectMode`，以便能够处理来自 CSV 解析器的 JSON 输入：

```
class MyTransformStream extends Transform {
constructor (options = { objectMode: true }) {
options.objectMode = true;
super(options);
}
}
```

`Transform` 流必须实现一个方法：`_transform`。该方法是我们刚刚创建的类的一部分，在处理每个数据块时被调用。在 `_transform` 方法中，我们可以将数据块转换为新的内容。转换后的数据随后通过 `this.push` 推送给下一个消费者。处理完成后，我们调用 `done` 回调来表明该数据块处理完毕。目前，我们只是想看看数据块的样子：

```
_transform (chunk, encoding, done) {
console.log('chunk: ', chunk);
this.push(chunk);
done();
}
```

最后一步，我们需要将 CSV 文件通过管道传给 CSV 解析器，再将解析后的输出传给我们的自定义流：

```
const opts = {comment: '#', delimiter: ';', columns: true};
const parser = parse(opts);
const input = fs.createReadStream(__dirname + '/test/fixtures/test.csv');
input
.pipe(parser)
.pipe(new MyTransformStream());
```

现在运行 `node streams-example.js`，我们会得到以下输出：

```
$ node streams-example.js
chunk:  { time: 'march', location: 'austin,us' }
chunk:  { time: 'april', location: 'boston,us' }
chunk:  { time: 'october', location: 'bristol,uk' }
chunk:  { time: 'february', location: 'hermigua,es' }
chunk:  { time: 'march', location: 'hermigua,es' }
chunk:  { time: 'april', location: 'havana,cu' }
```

每个数据块都是一个 JSON 对象。每次我们调用 `done` 并且仍有输入产生时，`_transform` 会被下一个数据块再次调用。我们现在可以将每个数据块通过 HTTP 请求发往 CouchDB/HTTP API。这样内存占用会非常低，但也会发送大量 HTTP 请求，整个迁移过程会耗时很长。一个折中的好办法是缓冲少量数据块，然后通过 CouchDB/PouchDB 的批量 API 进行提交。这样既能避免缓存所有现有数据导致内存溢出，也能因为减少 HTTP 请求数量而更快完成导入。

**注意**

本节的代码可以在 `sourcecode/streams/streams-example.js` 找到。

## 转换流与可写流

接下来要创建的流，我们将在 lounger 的根目录中新建 `streams-bulk-example.js` 文件。该文件将从 CSV 解析流中获取对象并进行缓冲。当达到指定限制时，它会将缓冲的对象传递给下一个消费者。传递给下一个消费者的结果已准备好用于提交至 `CouchDB`/`PouchDB` 的批量文档 API 端点。`CouchDB`/`PouchDB` 的批量 API 接受一个被 `{"docs": []}` 包裹的对象数组。

### 创建包裹转换流

我们从开发 `Transform` 流所需的相同模块集合开始：

```
const parse = require('csv-parse');
const fs = require('fs');
const Transform = require('stream').Transform;
```

我们的流将被命名为 `TransformToBulkDocs`，它将接收一个对象作为选项。通过选项，我们可以指定要缓冲的文档数量：

```
class TransformToBulkDocs extends Transform {
constructor (options = { objectMode: true, bufferedDocCount: 200 }) {
options.objectMode = true;
super(options);
```

用于 `this.buffer` 的空数组将成为我们的缓冲器：

```
this.buffer = [];
this.bufferedDocCount = options.bufferedDocCount;
```

以下是不包含 `_transform` 方法的完整类：

# 自定义 Node.js 流：Transform 与 Writable

```javascript
class TransformToBulkDocs extends Transform {
constructor (options = { objectMode: true, bufferedDocCount: 200 }) {
options.objectMode = true;
super(options);
this.buffer = [];
this.bufferedDocCount = options.bufferedDocCount;
}
}
```

在 `_transform` 方法中，我们将每个数据块添加到缓冲器中：

```javascript
_transform (chunk, encoding, done) {
this.buffer.push(chunk);
```

如果缓冲器增长到足够大，我们调用 `this.push` 方法，该方法继承自基础 `Transform` 流。`this.push(args)` 告知 Node 我们希望将 `args` 传递给流管道中的下一个消费者。然后我们清空缓冲器，以便接收可能到达的新数据：

```javascript
if (this.buffer.length >= this.bufferedDocCount) {
this.push({docs: this.buffer});
this.buffer = [];
}
done();
}
```

以下是完整的 `_transform` 方法：

```javascript
_transform (chunk, encoding, done) {
this.buffer.push(chunk);
if (this.buffer.length >= this.bufferedDocCount) {
this.push({docs: this.buffer});
this.buffer = [];
}
done();
}
```

我们文件的最后一部分与第一个流示例几乎相同：

```javascript
const opts = {comment: '#', delimiter: ';', columns: true};
const parser = parse(opts);
const input = fs.createReadStream(__dirname + '/test/fixtures/test.csv');
input
.pipe(parser)
.pipe(new TransformToBulkDocs());
```

我们向代码中添加一个临时的 `console.log` 来验证是否正常工作：

```javascript
if (this.buffer.length >= this.bufferedDocCount) {
this.push({docs: this.buffer});
console.log({docs: this.buffer});
this.buffer = [];
}
```

当我们运行 `node streams-bulk-example.js` 时，发现它没有生效！为什么？

问题在于我们没有足够的文档来达到默认的 200 个文档计数。同样的道理也适用于一组中剩余的文档。如果我们初始有 250 个文档作为输入，前 200 个会被推送到下一个消费者，但剩下的 50 个会丢失。幸运的是，Node.js 开发者意识到了这个问题，并提供了 `_flush` 方法。与 `_transform` 方法不同，`_flush` 方法并非实现 `Transform` 流的必需方法。相反，我们可以在需要时才选择实现它。

`Transform` 类的 `_flush` 方法会在流消费完所有数据之后、但在流触发标志流结束的 `end` 事件之前，在最后时刻被调用。`_flush` 会在最后时刻被调用，如果此时我们仍有少量缓冲的文档，我们可以将它们推送给下一个消费者：

```javascript
_flush (done) {
this.buffer.length && this.push({docs: this.buffer});
done();
}
```

这就是我们第一个自定义流！别忘了移除我们添加的 `console.log` 调用！

管道中的下一个消费者将接收收集到的文档，并将它们提交给 `CouchDB`/`PouchDB` 的批量文档端点。由于流能够处理背压，其他流会等待我们成功将文档添加到 `CouchDB`/`PouchDB` 后，再继续沿着管道向我们传递数据。

接下来我们要构建的流将接收来自 `Transform` 流的数据，并将其写入数据库。我们将在下一步添加该流。

## 将结果通过管道传输至可写流

现在，我们将数据通过管道传输至一个 `Writable` 流，并将其添加到 `streams-bulk-example.js` 文件中。`Writable` 流可以接收来自消费者的数据，并将其导向新位置。一个 `Writable` 文件流会将数据分块写入硬盘。

```javascript
const Writable = require('stream').Writable;
```

我们的 `Writable` 流需要知道数据存储的位置，因此需要向其传递数据库 URL。由于流的每个方法都会针对每个数据块被调用，我们需要将传递进来的 URL 存储为 `this.url`：

```javascript
class CouchBulkImporter extends Writable {
  constructor (options) {
    if (!options) {
      options = {};
    }
    if (!options.url) {
      const msg = [
        'options.url must be set',
        'example:',
        "new CouchBulkImporter({url: 'http://localhost:5984/baseball'})"
      ].join('\n')
      throw new Error(msg);
    }
    options.objectMode = true;
    super(options);
    // sanitise url, remove trailing slash
    this.url = options.url.replace(/\/$/, '');
  }
}
```

要实现 `Writable` 流的子类，我们需要实现 `_write` 方法。与 `Transform` 流的 `_transform` 方法类似，`_write` 方法会在每个从前一个生产者传递到该流的数据块被调用时执行。在我们的例子中，我们通过 `request` 将 JSON 数据块以 JSON 格式发送到数据库。在成功将数据发送到 `/_bulk_docs` API 端点后，我们调用 `done` 回调来表明已准备好接收新的数据块：

```javascript
_write (chunk, enc, done) {
  request({
    json: true,
    uri: this.url + '/_bulk_docs',
    method: 'POST',
    body: chunk
  }, function (err, res, body) {
    if (err) {
      return done(err);
    }
    if (!/²../.test(res.statusCode)) {
      const msg = 'CouchDB server answered: \n Status: ' +
        res.statusCode + '\n Body: ' + JSON.stringify(body);
      return done(new Error(msg));
    }
    done();
  });
}
```

我们还需要在文件中引入 `request`：

```javascript
const request = require('request');
```

要使用该流，我们必须将数据通过管道传输进去。我们更新 `streams-bulk-example.js` 的最后部分：

```javascript
input
  .pipe(parser)
  .pipe(new TransformToBulkDocs())
  .pipe(new CouchBulkImporter({url: 'http://127.0.0.1:5984/travel'}));
```

在创建数据库 `travel` 并运行脚本后，我们导入 CSV 文件：

```bash
$ curl -X PUT http://localhost:5984/travel
{"ok":true}
$ node streams-bulk-example.js
$ curl http://localhost:5984/travel/_all_docs
{"total_rows":6,"offset":0,"rows":[{"id":"3444bf7c-65c0-438f-f8e8-7f55124f1736","key":"3444bf7c-65c0-438f-f8e8-7f55124f1736","value":{"rev":"1-37fcd2e5b40939805b8e043da44f9b1d"}},{"id":"568c3b0f-78fe-43a2-9eac-06620cfaa595","key":"568c3b0f-78fe-43a2-9eac-06620cfaa595","value":{"rev":"1-e047788bac9aada0564fc928642d3960"}},{"id":"5d170e63-d845-4e45-f760-97e30cbc4b21","key":"5d170e63-d845-4e45-f760-97e30cbc4b21","value":{"rev":"1-6f1794e4eb24b60665fe02c2624d53eb"}},{"id":"9cd34a61-8a48-4b88-afbf-7fd6e3c9cf42","key":"9cd34a61-8a48-4b88-afbf-7fd6e3c9cf42","value":{"rev":"1-747b11a103cc0fe31d1c546ef70d69c0"}},{"id":"cc7da504-438a-40c1-842b-4722b63c9a37","key":"cc7da504-438a-40c1-842b-4722b63c9a37","value":{"rev":"1-0f9e7add8b7fbb773dc7d3081475d855"}},{"id":"f09811c5-43ec-4a6d-bd3b-b34b3674676d","key":"f09811c5-43ec-4a6d-bd3b-b34b3674676d","value":{"rev":"1-d677c5bbbee1bbbe45f6128a9cf1fe8d"}}]}
```

我们可以通过使用数据库自动分配给每个文档的 ID 来访问单个文档：

```bash
$ curl http://localhost:5984/travel/3444bf7c-65c0-438f-f8e8-7f55124f1736
{"time":"february","location":"hermigua,es","_id":"3444bf7c-65c0-438f-f8e8-7f55124f1736","_rev":"1-37fcd2e5b40939805b8e043da44f9b1d"}
```

看起来很棒！看来我们已经万事俱备，可以在命令行客户端中使用这些底层流处理函数了。

> **注意：** 本节代码位于 `sourcecode/streams/streams-bulk-example.js`。

## 流式导入命令

在本章的剩余部分，我们将复用刚刚创建的自定义流实现。在实际项目中，我会为我们的两个流创建两个模块，使其可以在多个项目中复用。现在，我们可以将`CouchBulkImporter`和`TransformToBulkDocs`流的代码复制到`lib/csv.js`中，该文件将作为我们导入命令的核心：

```javascript
const parse = require('csv-parse');
const fs = require('fs');
const Transform = require('stream').Transform;
const util = require('util');
const Writable = require('stream').Writable;
const request = require('request');
const lounger = require('./lounger.js');

class TransformToBulkDocs extends Transform {
  constructor (options) {
    if (!options) {
      options = {};
    }
    if (!options.bufferedDocCount) {
      options.bufferedDocCount = 200;
    }
    options.objectMode = true;
    super(options);
    this.buffer = [];
    this.bufferedDocCount = options.bufferedDocCount;
  }

  _transform (chunk, encoding, done) {
    this.buffer.push(chunk);
    if (this.buffer.length >= this.bufferedDocCount) {
      this.push({docs: this.buffer});
      this.buffer = [];
    }
    done();
  }

  _flush (done) {
    this.buffer.length && this.push({docs: this.buffer});
    done();
  }
}

class CouchBulkImporter extends Writable {
  constructor (options) {
    if (!options) {
      options = {};
    }
    if (!options.url) {
      const msg = [
        'options.url must be set',
        'example:',
        "new CouchBulkImporter({url: 'http://localhost:5984/baseball'})"
      ].join('\n');
      throw new Error(msg);
    }
    options.objectMode = true;
    super(options);
    // sanitize url, remove trailing slash
    this.url = options.url.replace(/\/$/, '');
  }

  _write (chunk, enc, done) {
    request({
      json: true,
      uri: this.url + '/_bulk_docs',
      method: 'POST',
      body: chunk
    }, function (err, res, body) {
      if (err) {
        return done(err);
      }
      if (!/²../.test(res.statusCode)) {
        const msg = 'CouchDB server answered: \n Status: ' +
          res.statusCode + '\n Body: ' + JSON.stringify(body);
        return done(new Error(msg));
      }
      done();
    });
  }
}
```

### 设计命令行

让我们思考一下即将构建的命令。CSV 文件可以有不同的分隔符；有些使用分号作为分隔符，而有些则使用逗号或制表符。表示注释的符号也可能不同。在我们之前的实现中，我们使用了固定值：

```javascript
const opts = {comment: '#', delimiter: ';', columns: true};
```

对于实际应用场景，分隔符和注释符号必须是可配置的。CSV 输入通常来自文件。

以下是一个可能的 CLI 示例：

```bash
$ lounger csv transfer   [--delimiter=;] [--comment=#] [--chunksize=200]
```

`csv`命令具有良好的扩展性，未来可以承载所有与 CSV 相关的命令。该命令读起来非常顺畅且易于记忆：`lounger csv transfer <file> <database>` 读起来几乎就像 `lounger [do] csv transfer [from] <file> [to] <database>`。合理的默认值帮助我们完全避免传递可选修饰符，但如果需要修改它们，我们可以更改导入的每个重要方面。

## 自动创建数据库

我不确定你是否注意到，在我们之前使用流的时候，必须提前通过 `curl` 创建目标数据库。如果我们的 CLI 用户不必自行创建目标数据库，那将非常方便。我们的目标是帮助他们尽可能快速、轻松地完成任务，因此我们应该在必要时自动创建数据库。接下来，我们将添加一个辅助函数来创建数据库。

### 创建目标数据库

为了让用户使用起来更便捷，我们将添加一个创建目标数据库的函数。这样一来，可以确保命令工作流不会遇到重大障碍。`createTargetDatabase` 函数是一个辅助函数，它将 `request` 封装成 Promise。如果数据库已创建（HTTP 状态码 201 或 200），或者数据库已存在（HTTP 状态码 412），则 Promise 将会被 resolve；其他所有状态都会导致 Promise 被 reject：

```javascript
function createTargetDatabase (url) {
return new Promise((resolve, reject) => {
request({
json: true,
uri: url,
method: 'PUT',
body: {}
}, function (er, res, body) {
if (er && (er.code === 'ECONNREFUSED' || er.code === 'ENOTFOUND')) {
const err = new Error(
'无法连接到 ' + url + '。请检查数据库是否在运行'
);
err.type = 'EUSAGE';
return reject(err);
}
if (er) {
return reject(er);
}
const code = res.statusCode;
if (code !== 200 && code !== 201 && code !== 412) {
const msg = 'CouchDB 服务器响应：\n 状态：' +
res.statusCode + '\n 正文：' + JSON.stringify(body);
return reject(new Error(msg));
}
resolve();
});
});
}
```

如果出现 `ECONNREFUSED` 或 `ENOTFOUND` 错误，我们可以合理地推断数据库当前处于离线状态，并请用户检查数据库是否可用。我再怎么强调`正确处理错误`的重要性也不为过。来看这个例子，我们收到了 `ECONNREFUSED` 错误：

```
$ ./bin/lounger-cli csv transfer test/fixtures/test.csv http://127.0.0.1:1337/testimport
ERR! connect ECONNREFUSED 127.0.0.1:5984
ERR! Error: connect ECONNREFUSED 127.0.0.1:5984
ERR!     at Object.exports._errnoException (util.js:870:11)
ERR!     at exports._exceptionWithHostPort (util.js:893:20)
ERR!     at TCPConnectWrap.afterConnect [as oncomplete] (net.js:1063:14)
ERR!
ERR!
ERR! lounger: 1.0.0 node: v9.1.0
ERR! 请在 http://example.com/lounger/issues 上提交包含此日志的 issue
```

根据用户之前使用 Node.js 的经验，他们可能会非常困惑。他们唯一能继续下去的方法就是去问搜索引擎或者提交一个 issue。收到 issue 后，我们乏味的工作就是关闭它，并告诉他们可能是 URL 拼写错误了。

### 实现主要 CLI 函数

在编写 `createTargetDatabase` 函数之后，我们应该已经准备好了所有辅助函数。像往常一样，我们通过实现 API 命令来开始实现主要的 CLI 函数，然后我们将用 CLI 函数来包装它。

分隔符和注释选项在配置文件中定义，或者通过命令行传递。要知道它们的值，我们需要与 `lounger.config` 交互。要访问 `lounger.config`，我们需要引入它：

```javascript
const lounger = require('./lounger.js');
```

主要的 API 函数会检查是否提供了所有必要的参数，如果配置文件中或命令行没有传入配置，则应用默认值。如果数据库尚不存在，我们会创建它，并将任务委托给 `importFromCsvFile` 辅助函数：

```javascript
exports.api = {
transfer: bulkdocsImport
};
function bulkdocsImport (file, targetDb) {
return new Promise((resolve, reject) => {
const opts = {};
if (!file && !targetDb) {
return reject(new Error('缺少 file 和/或 targetDb 参数'));
}
opts.delimiter = lounger.config.get('delimiter') || ';';
opts.comment = lounger.config.get('comment') || '#';
opts.chunksize = lounger.config.get('chunksize') || 200;
createTargetDatabase(targetDb)
.then(() => {
return importFromCsvFile(file, targetDb, opts);
}).catch(reject);
});
}
```

`importFromCsvFile` 函数接收源 CSV 文件、URL 和选项，并创建流管道。与我们之前在 `streams-example.js` 中的代码相比，主要区别在于我们添加了适当的错误处理来捕获所有错误：

```javascript
function importFromCsvFile (file, url, opts) {
return new Promise((resolve, reject) => {
const options = {comment: opts.comment, delimiter: opts.delimiter, columns: true};
const parser = parse(options);
const input = fs.createReadStream(file);
input
.pipe(parser)
.on('error', reject)
.pipe(new TransformToBulkDocs({bufferedDocCount: opts.chunksize}))
.on('error', reject)
.pipe(new CouchBulkImporter({url: url}))
.on('error', reject);
});
}
```

CLI 函数最终包装了我们的 API 方法，并增加了友好的错误信息：

```javascript
exports.cli = importCli;
function importCli (cmd, file, target) {
return new Promise((resolve, reject) => {
if (!cmd || cmd !== 'transfer' || !file || !target) {
const err = new Error(
'使用方法：lounger csv transfer   [--delimiter=;] [--comment=#] [--chunksize=200]'
);
err.type = 'EUSAGE';
return reject(err);
}
return bulkdocsImport(file, target).catch(reject);
});
}
```

我们引入了三个新选项：`delimiter`、`comment` 和 `chunksize`。`lounger.config` 允许我们的用户通过使用配置文件来设置默认值。此外，我们还需要确保这些选项能在命令行上被解析。在 `bin/lounger-cli` 中，我们需要向 `nopt` 注册我们的可选参数：

```javascript
const parsed = nopt({
'json': [Boolean],
'delimiter': [String],
'comment': [String],
'chunksize': [Number]
}, {'j': '--json'}, process.argv, 2);
```

搞定了！我们创建了一个能够将大量数据流式传输到数据库的命令。

如果你对一个能将数据从 MongoDB 流式传输到 CouchDB 的流管道感兴趣，可以查看 [`https://github.com/robertkowalski/couchbulkimporter/blob/master/examples/mongo.js`](https://github.com/robertkowalski/couchbulkimporter/blob/master/examples/mongo.js)。

注意，本节代码位于 `sourcecode/streams`。欢迎使用！

## 总结

通过流，我们找到了一种处理无法加载到内存中的大量数据的方法。基于流的概念，我们构建了一个能够处理海量数据的导入器。作为最后一步，我们将所有内容集成到了命令行客户端。希望你喜欢这段关于流的探险之旅！
