# 第 3 章 ■ API 可用性与消息传递

`port.postMessage("...");`

`//end-region`

`![](img/index-122_1.jpg)`

***图 3-23.** 审查弹出窗口：PSandES 扩展* 清单 3-12 包含了事件脚本的相应代码。请注意，与之前的示例（见清单 3-10）相比，此处未使用 `onMessageExternal` 事件。由于消息传递是在扩展程序内部进行的，因此使用的是 `onMessage` 事件。如果使用长连接（通过 `chrome.runtime.connect` 方法，见清单 3-11），则事件脚本的代码需要为 `chrome.runtime.onConnect` 事件提供一个监听函数。这在清单 3-12 的注释部分已经展示。图 3-18 包含了事件脚本对应的日志。

***清单 3-13.*** 第 [3](http://dx.doi.org/10.1007/978-1-4842-1775-7_3) 章 /CSandES/ manifest.json

```
{
  "manifest_version" : 2,
  "name" : "Communication Demo: content-script and event-script",
  "description" : "Shows communication b/w content-script and event-script",
  "version" : "1.2",
  "background" : {
    "scripts" : ["event_script.js"],
    "persistent" : false
  },
  "content_scripts" : 
```

`![`

***图 3-24.** 后台页面控制台面板*

### 弹出窗口脚本与内容脚本

现在，让我们来看看弹出窗口脚本如何与内容脚本通信。

为了演示这一点，在练习文件文件夹中包含了一个 PSandCS 扩展。清单 3-14 到 3-16 包含了该扩展的相关代码。你可以在浏览器中加载此扩展进行测试。图 3-20 和图 3-21 分别包含了该扩展内容脚本和弹出窗口脚本的日志。

> **注意：** 如果扩展的浏览器操作不包含弹出窗口，则可以使用事件脚本为 `chrome.browserAction.onClicked` 事件提供监听函数。

使用此监听函数，你可以调用 `chrome.tabs.sendMessage`（或 `chrome.tabs.connect`）方法。这种方法不需要使用 tabs API，因为监听函数会将活动标签页作为其参数接收。但仍然需要 `activeTab` 权限，以便与活动标签页的内容进行交互——例如，通过 `chrome.tabs.executeScript` 方法。

正如你从清单文件（清单 3-14）中看到的，此扩展将内容脚本注入到属于 example.org 主机的已访问网页中。请注意 `browser_action` 属性及其弹出窗口组件的使用。弹出窗口包含一个可点击的元素（见图 3-21）——ID 为 `send_message` 的“发送消息”。该元素的点击事件绑定了一个监听函数。清单 3-15 包含了该监听函数的相应代码。

***清单 3-14.*** 第 [3](http://dx.doi.org/10.1007/978-1-4842-1775-7_3) 章 /PSandCS/ manifest.json

```
{
  "manifest_version" : 2,
  "name" : "Communication Demo: popup-script and content-script",
  "description" : "Shows communication b/w popup-script and content-script",
  "version" : "1.2",
  "content_scripts" : [
    {
      "matches" : ["*://www.example.org/*"],
      "js" : ["content_script.js"]
    }
  ],
  "browser_action" : {
    "default_title" : "Communication Demo: popup-script and content...",
    "default_icon" : "icon.png",
    "default_popup" : "popup.html"
  }
}
```

在清单 3-15 中，请注意使用 `chrome.tabs.query` 方法获取（当前）活动标签页。如前所述，使用 `chrome.tabs.sendMessage` 方法向内容脚本发送消息。该方法接受以下参数：

- `tabID`
- `message`
- `responseCallback`

`tabID` 是要向其发送消息的标签页的 ID。`message` 参数是要发送的消息。在本例中，它是一个字符串 `"Test message X"`。与之前讨论的扩展类似，`responseCallback` 是一个回调函数，供消息接收方使用。

`![](img/index-125_1.jpg)`

***图 3-25.** 审查弹出窗口：PSandES 扩展*

***清单 3-15.*** 第 [3](http://dx.doi.org/10.1007/978-1-4842-1775-7_3) 章 /PSandCS/ popup_script.js

```
//region {variables and functions}
var sendMessageButtonID = "send_message";
var greeting = "Hello World!";
var message = "Test message X";

function responseCallback(responseObject) {
  console.log("Message '" + responseObject.message +
    "' from Sender '" + responseObject.sender + "'");
}
//end-region

//region {calls}
console.log(greeting);
document.addEventListener("DOMContentLoaded",function(dcle){
  var buttonID = document.getElementById(sendMessageButtonID);
  buttonID.addEventListener("click",function(ce) {
    chrome.tabs.query({"active":true},function(tabs) {
      chrome.tabs.sendMessage(tabs[0].id,message,responseCallback);
    });
  });
});
//end-region
```

最后，内容脚本包含接收发送消息的代码。为此，使用了 `chrome.runtime.onMessage` 事件的监听函数。在清单 3-16 中，通过调用 `console.log` 将接收到的消息记录到控制台。此日志对应的输出显示在图 3-20 中。请注意，此扩展中没有使用注入的按钮元素。你可以自由扩展此扩展（用于实验等），方法是利用现有代码。

***清单 3-16.*** 第 [3](http://dx.doi.org/10.1007/978-1-4842-1775-7_3) 章 /PSandCS/ content_script.js

```
//region {variables and functions}
var consoleGreeting = "Hello World!";
var responseObject = {
  message : "Test message Y",
  sender : "content_script.js"
};

function GetFormattedMessageString(message,sender) {
  return "Message '" + message + "' from Sender '" + sender.id + "'";
}

function createButton() {
  var button = document.createElement("button");
  button.style.width = "70px";
  button.style.height = "40px";
  button.style.position = "fixed";
  button.style.top = "10px";
  button.style.right = "10px";
  button.innerText = "Send Message";
  document.body.appendChild(button);
  return button;
}
//end-region

//region {calls}
console.log(consoleGreeting);
var button = createButton();
chrome.runtime.onMessage.addListener(function(message,sender,sendResponse) {
  //Will get called from the script where sendResponse is defined
  sendResponse(responseObject);
  console.log(GetFormattedMessageString(message,sender));
});
//end-region
```

### 使用长连接

要创建与内容脚本通信的长连接，可以使用 `chrome.tabs.connect` 方法。该方法将要连接到的标签页的 ID 作为其唯一参数。对于当前扩展，在指定标签页中运行的每个内容脚本都会触发相应的 `chrome.runtime.onConnect` 事件。

`![](img/index-127_1.jpg)`

再次强调，由于连接的两端都可以访问端口对象——可以使用 `port.postMessage` 方法和相应的 `port.onMessage` 事件进行通信。这与“网页脚本与事件脚本”主题中的实现方式类似。

***图 3-26.** API 演示：Alarms API*

### 内容脚本与网页脚本

如前所述，内容脚本无法访问运行时环境。



