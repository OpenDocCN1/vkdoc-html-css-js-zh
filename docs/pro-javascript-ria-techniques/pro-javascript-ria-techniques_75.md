# 离线电子邮件存储示例

<!-- 创建用于撰写电子邮件的表单 -->

```html
<form method="post" action="/" id="compose-message">
  <div>
    <label for="to">收件人</label>
    <input type="text" name="to" id="to" />
  </div>
  <div>
    <label for="subject">主题</label>
    <input type="text" name="subject" id="subject" />
  </div>
  <div>
    <label for="message-body">正文</label>
    <textarea id="message-body" name="message-body" rows="10" cols="20"></textarea>
  </div>
  <div>
    <input type="submit" value="保存" />
  </div>
</form>
```

<!-- 创建一个元素，用于后续放置 Flash 存储组件 -->

```html
<div id="flash-storage-element"></div>
```

<!-- 引用 $ JavaScript 库 -->

```html
<script type="text/javascript" src="$.js"></script>
```

<!-- 从 Google CDN 引用 SWFObject -->

```html
<script type="text/javascript" src="http://ajax.googleapis.com/ajax/libs/swfobject/2.1/swfobject.js"></script>
```

<!-- 以下代码以内联方式呈现以保持简洁。在实际的 Web 应用中，应将其存储在外部文件中 -->

```html
<script type="text/javascript">
$.onDomReady(function() {
    // 查找此 Flash 离线存储组件，在线地址为
    // http://www.denodell.com/
    var movieURL = "storage.swf";
    var tagIDToReplace = "flash-storage-element";
    var width = 1;
    var height = 1;
    var flashVersionRequired = "8";
    var parameters = {
        allowScriptAccess: "always"
    };
    var attributes = {
        id: "flash-storage"
    };
    swfobject.embedSWF(movieURL, tagIDToReplace, width, height,
        flashVersionRequired, null, {}, parameters, attributes);

    // 初始化离线数据存储 API，传递页面上 Flash 元素的引用
    // 以允许使用 Flash 进行存储
    $.Storage.initialize(document.getElementById("flash-storage"));

    // 使用上次保存的值填充消息字段，以便当用户在浏览器崩溃后
    // 返回页面时，字段会自动预填上次保存的值
    $.Storage.get("to", function(value) {
        document.getElementById("to").value = value;
    });
    $.Storage.get("subject", function(value) {
        document.getElementById("subject").value = value;
    });
    $.Storage.get("message-body", function(value) {
        document.getElementById("message-body").value = value;
    });

    // 每 30 秒保存一次正在撰写的邮件内容
    var THIRTY_SECONDS = 30000; // 30 秒 = 30000 毫秒
    var saveMessage = window.setInterval(function() {
        // 保存收件人、主题和正文输入字段的值
        // 以防网络断开或浏览器崩溃
        $.Storage.set({
            name: "to",
            value: document.getElementById("to").value
        });
        $.Storage.set({
            name: "subject",
            value: document.getElementById("subject").value
        });
        $.Storage.set({
            name: "message-body",
            value: document.getElementById("message-body").value
        });
    }, THIRTY_SECONDS);

    // 当用户提交表单以将消息保存到服务器时，
    // 移除先前存储的字段值，并停止在表单提交过程中继续保存
    $.Events.add(document.getElementById("compose-message"),
        "submit", function(e) {
            // 阻止 HTML 页面提交表单，以便通过 Ajax 处理数据提交
            e.preventDefault();

            // 停止每 30 秒保存消息字段值
            window.clearInterval(saveMessage);

            // TODO: 使用 Ajax 将表单字段值发送到服务器

            // 消息已发送，移除本地数据
            $.Storage.remove("to");
            $.Storage.remove("subject");
            $.Storage.remove("message-body");

            // TODO: 更改页面以显示邮箱文件夹内容
        });
});
</script>
```

### 使用可复用的离线存储组件



