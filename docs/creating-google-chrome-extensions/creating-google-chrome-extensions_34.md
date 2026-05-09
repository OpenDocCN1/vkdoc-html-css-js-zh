# 使用书签 API

要搜索书签，请使用`chrome.bookmarks.search`方法。此方法接受一个查询和一个回调函数作为参数。查询可以使用字符串或对象进行。在清单 3-24 中，查询使用了字符串。如果使用对象，则需要定义以下属性：`query`、`url`和`title`。

***图 3-35.** API 演示：存储 API*

#### 使用书签层次结构

书签 API 提供了`chrome.bookmarks.getTree`方法来检索整个书签层次结构。此方法接受一个函数作为其唯一参数。书签树作为数组传递给此方法。与该 API 对应的用法可以在以下代码片段中看到。

请注意，树是此传递数组的第一个元素。由于此树使用节点表示，因此使用`children`属性来访问嵌套文件夹。

在这种情况下，文件夹是`Bookmarks Bar`和`Other Bookmarks`。通过遍历嵌套的`BookmarkTreeNode`项（例如，使用`folder.children.forEach`调用），您可以访问层次结构中的所有书签。

```
chrome.bookmarks.getTree(function(bookmarkTreeAsArray) {
    var bookmarkTree = bookmarkTreeAsArray[0];
    var folders = [];
    if(bookmarkTree.children) {
        bookmarkTree.children.forEach(function(node) {
            if(node.children.length > 0) folders.push(node);
        });
    }
    if(folders.length > 0) {
        folders.forEach(function(folder) {
            folder.children.forEach(function(bookmarkTreeNode) {
                if(bookmarkTreeNode.url) {
                    /*use the node*/
                }
            });
        });
    }
});
```

#### 相关事件

书签 API 还提供了一些事件，这些事件在书签树发生特定变化时触发。以下列表包含这些事件及其描述。请注意，正如您在之前的示例中所看到的，要监听这些事件，您需要提供监听器函数。它们通过调用事件的`addListener`来添加。要了解更多关于这些事件的信息，请访问 https://developer.chrome.com/extensions/bookmarks。

- `onCreated` — 在创建书签或文件夹时触发
- `onRemoved` — 在删除书签或文件夹时触发
- `onChanged` — 在书签或文件夹发生变化时触发
- `onMoved` — 在书签或文件夹移动到不同的父文件夹时触发
- `onChildrenReordered` — 在文件夹的子项因 UI 中排序而改变顺序时触发
- `onImportBegan` — 在书签导入会话开始时触发
- `onImportEnded` — 在书签导入会话结束时触发

#### 下载 API

下载 API（即`chrome.downloads`API）用于以编程方式启动、监控、操作和搜索下载。它使用`downloads`权限。

清单 3-25 包含来自 DownloadsAPI 扩展的代码，该扩展位于第 3 章的 Exercise Files 文件夹中。

***清单 3-25.*** 第 3 章/DownloadsAPI/event_script.js

```
//region {variables and functions}
var greeting = "Hello World!";
var downloadOptions = {
    "url" : "http://www.apress.com/downloadable/download/sample/sample_id/1456/",
    "saveAs" : true
};
//end-region

//region {calls}
console.log(greeting);
chrome.downloads.download(downloadOptions,function(downloadId) {
    console.log(downloadId);
    /*
    chrome.downloads.pause(downloadId,function() {
        if(!chrome.runtime.lastError) console.log("pause");
    });
    */
});
chrome.downloads.onCreated.addListener(function(downloadItem) {
    console.log("onCreated:");
    console.log(downloadItem);
});
chrome.downloads.onErased.addListener(function(downloadId) {
    console.log("onErased:");
    console.log(downloadId);
});
chrome.downloads.onChanged.addListener(function(downloadDelta) {
    console.log("onChanged:");
    console.log(downloadDelta);
});
//end-region
```

#### 下载文件



要下载文件，请使用`chrome.downloads.download`方法。该方法接受两个参数——`options`（类型为`object`）和`callback`（类型为`function`）。对应于`DownloadItem`的`id`将被传递到回调函数中。`options`对象描述了下载内容及其方式。它支持以下属性。

- `url`（`string`）
- `filename`（`string`）
- `saveAs`（`Boolean`）
- `method`（`"GET"`或`"POST"`字符串）
- `headers`（`array`）
- `body`（`string`）

> **注意**：`DownloadItem`是与下载 API 关联的一种类型。该类型包含一长串属性（详情请访问[`developer.chrome.com/extensions/downloads#type-DownloadItem`](https://developer.chrome.com/extensions/downloads#type-DownloadItem)）。一些重要的属性包括`id`、`filename`、`mime`、`startTime`、`endTime`、`bytesReceived`和`totalBytes`。

---

## 第 3 章 ■ API 可用性与消息传递

请注意，如果要下载的 URL 使用 HTTPS 协议，则应使用`method`、`headers`和`body`属性。现在，让我们看一个使用`download`方法的示例。在此示例中，我们将下载位于 [`www.apress.com/9781430250531`](http://www.apress.com/9781430250531) 的源代码文件。要下载的文件的确切 URL 显示在 清单 3-25 的`downloadOptions`对象中。

* **图 3-36.** Tabs API：使用`captureVisibleTab`方法

如您所见，调用了`download`方法，并将`downloadOptions`对象作为第一个参数传递。`downloadOptions`对象使用`url`和`saveAs`属性定义。显而易见，`url`是要下载的 URL。将`saveAs`属性设置为`true`是为了使用文件选择器，允许用户选择文件名。请注意图 3-29 中的“下载者”部分，它指向了发起下载的扩展。

---

## 第 3 章 ■ API 可用性与消息传递

#### 取消或恢复下载

一旦获得了`DownloadItem`对应的`id`（该`id`已传递给`download`方法的回调函数），您就可以轻松地取消、恢复甚至暂停下载。相应的调用非常简单：

- `chrome.downloads.cancel(integer downloadId, function callback)`
- `chrome.downloads.resume(integer downloadId, function callback)`
- `chrome.downloads.pause(integer downloadId, function callback)`

#### 打开下载

如果`DownloadItem`已完成，请使用`open`方法打开下载的文件。如果未完成，`open`方法会通过`chrome.runtime.lastError`返回错误。

此方法只需要一个参数，即`downloadId`。请注意，此方法需要额外的权限：`downloads.open`。

若仅需在文件管理器中显示已下载文件所在的文件夹，需要调用以下方法：`chrome.downloads.show(integer downloadId)`。

此外，如果唯一意图是在文件管理器中显示默认的“下载”文件夹，则需要调用`chrome.downloads.showDefaultFolder()`方法。

#### 删除下载

如果`DownloadItem`已完成，请使用`chrome.downloads.removeFile`方法删除已下载的文件。该方法接受两个参数——`downloadId`和`callback`。但是，如果只需要从历史记录中清除下载记录（而不删除已下载的文件），则应改用`chrome.downloads.erase`方法。

此方法接受一个`query`对象和一个`callback`函数作为其参数。`query`对象可以接受一长串属性。一些重要的属性包括`id`、`startedBefore`、`startedAfter`、`endedBefore`、`endedAfter`、`filenameRegex`和`urlRegex`。要查看完整列表，请访问 [`developer.chrome.com/extensions/downloads#method-erase`](https://developer.chrome.com/extensions/downloads#method-erase) 。

#### 相关事件



下载 API 提供了一些有用的事件，例如`onCreated`、`onChanged`等，可用于在下载开始或对应的`DownloadItem`属性发生变化时提供回调函数。以下列出这些事件及其说明。请注意，你还可以参考清单 3-25 了解这些事件及其监听函数的具体用法。

* `onCreated`—当下载开始时，使用`DownloadItem`对象触发该事件
* `onErased`—当下载从历史记录中删除时，使用`downloadId`触发该事件
* `onChanged`—当`DownloadItem`的任何属性（`bytesReceived`和`estimatedEndTime`除外）发生变化时触发该事件

### History API

历史 API（即`chrome.history` API）用于与浏览器记录的已访问页面进行交互。通过此 API，你可以添加、删除和查询浏览器历史记录中的 URL。它需要使用历史权限。清单 3-26 包含了来自第三章练习文件文件夹中 HistoryAPI 扩展的代码。

***清单 3-26.*** 第 3 章/HistoryAPI/ event_script.js

```
//region {变量和函数}

var greeting = "Hello World!";

var tenMinutesAsMilliseconds = 10 * 60 * 1000;

//getTime 返回自纪元以来的毫秒数

var currentTimeAsMilliseconds = new Date().getTime();

//使用 "text" 查询过去一小时内过滤历史记录

var query = {

"text" : "apress",

"startTime" : currentTimeAsMilliseconds - 6 * tenMinutesAsMilliseconds,

"endTime" : currentTimeAsMilliseconds,

"maxResults" : 10

};

//end-region

//region {调用}

console.log(greeting);

chrome.history.search(query,function(results) {

results.forEach(function(result) {

//result 是 HistoryItem 类型

console.log(result);

});

});

chrome.history.getVisits({"url" : "http://www.example.

org"},function(results) {

results.forEach(function(result) {

//result 是 VisitItem 类型

console.log(result);

});

});

chrome.history.addUrl({"url" : "http://www.example.org"},function() {

console.log("addUrl");

});

chrome.history.deleteUrl({"url" : "http://www.example.org"},function() {

console.log("deleteUrl");

});

/*

chrome.history.deleteAll(function() {

console.log("deleteAll");

});

*/

//end-region
```

要使用查询搜索历史记录，请使用`chrome.history.search`方法。该方法接受两个参数——`query`（`object`类型）和`callback`（`function`类型）。

`query`对象支持以下属性：

* `text`—向历史服务提交的自由文本查询。留空以检索所有页面。
* `startTime`—将结果限制为此日期之后访问的页面，以自纪元以来的毫秒数表示。
* `endTime`—将结果限制为此日期之前访问的页面。以自纪元以来的毫秒数表示。
* `maxResults`—要检索的最大结果数。默认值为 100。

请注意，`callback`函数接收一个`HistoryItem`结果数组。这里，`HistoryItem`是一个封装历史查询单个结果的对象。它支持以下有用属性——`id`、`url`、`title`、`lastVisitTime`、`visitCount`等。

要检索关于某个 URL 访问的信息，请使用`chrome.history.getVisits`方法。如清单 3-26 所示，该方法将一个对象（包含`url`属性）作为其第一个参数。第二个参数是一个`callback`函数，该函数接收一个`VisitItem`结果数组。

**注意：** `VisitItem`是一个封装了对某个 URL 单次访问的对象。它由以下属性组成：`id`、`visitId`、`visitTime`、`referringVisitId`和`transition`（类型为`TransitionType`，描述了浏览器如何导航到特定 URL）。你可以访问<https://developer.chrome.com/extensions/history#transition_types>了解更多关于`TransitionType`的信息。



#### 添加和移除网址

要在当前时间向历史记录中添加一个网址，请使用 `chrome.history.addUrl` 方法。

该方法接受两个参数：`details`（类型为 `object`）和 `callback`（类型为 `function`）。`details` 对象仅支持 `url` 属性。类似地，使用 `chrome.history.deleteUrl(object details, function callback)` 方法可从历史记录中移除指定网址的所有出现记录。请注意，要清除所有历史记录项，需使用 `chrome.history.deleteAll` 方法。该方法仅接受一个 `callback` 函数作为参数。

*图 3-37.* XHRAPI：记录响应

#### 关联事件

历史记录 API 支持 `onVisited` 和 `onVisitRemoved` 事件。当访问某个网址时，会触发 `onVisited` 事件，并在相应的监听函数中提供该网址的 `HistoryItem` 数据。请注意，此事件在页面加载完成之前触发。

类似地，当一个或多个网址从历史记录服务中移除时，会触发 `onVisitRemoved` 事件。该事件对应的监听函数接收一个对象。

该对象支持以下属性：

- **allHistory** (`Boolean`) - 如果移除了所有历史记录，则为 `True`。若为 `true`，则 `urls` 将为空。
- **urls** (`array`) - 被移除的 `String` 类型网址数组。

#### 通知 API

您可以使用 `chrome.notifications` API 通过模板创建丰富的通知，并在系统托盘中向用户显示这些通知。它需要使用 `notifications` 权限。*代码清单 3-27* 包含了来自第 3 章练习文件文件夹中 NotificationsAPI 扩展的代码。

***代码清单 3-27.*** 第 3 章 /NotificationsAPI/ event_script.js

```
//region {variables and functions}

var greeting = "Hello World!";

var title = "NotificationsAPI";

var message = "Test message X";

var oneMinuteAsMilliseconds = 1 * 60 * 1000;

//getTime returns the number of milliseconds since the epoch

var currentTimeAsMilliseconds = new Date().getTime();

var notificationId = "myNotification1";

var NOTIFICATION_TEMPLATE_TYPE = {

BASIC : "basic",

IMAGE : "image",

LIST : "list",

PROGRESS : "progress"

};

var myButton1 = {

title : "Click X",

iconUrl : "button.png"

};

var myButton2 = {

title : "Click Y",

iconUrl : "button.png"

};

var myItem1 = {

title : "Item X",

message : "This is item X"

};

var myItem2 = {

title : "Item Y",

message : "This is item Y"

};

var notificationOptions = {

type : NOTIFICATION_TEMPLATE_TYPE.LIST,

iconUrl : "icon.png",

title : title,

message : message,

eventTime : currentTimeAsMilliseconds + oneMinuteAsMilliseconds,

buttons : [myButton1,myButton2],

/*imageUrl : "icon.png",*/

items : [myItem1,myItem2], //comment out for BASIC

/*progress : 0,*/

isClickable : true

};

//end-region

//region {calls}

console.log(greeting);

chrome.notifications.create(notificationId,notificationOptions,

function(id) {

console.log("create: " + id);

}

);

/*

chrome.notifications.clear(notificationId,function(wasCleared) {

console.log("clear: " + wasCleared);

});

*/

/*

chrome.notifications.getAll(function(notifications) {

console.log("getAll:");

console.log(notifications);

});

*/

chrome.notifications.onClicked.addListener(function(id) { //notification-id console.log("onClicked: " + id);

notificationOptions.title = title + " (onClicked)";

chrome.notifications.update(notificationId,notificationOptions,

function(wasUpdated) {

console.log("update: " + wasUpdated);

}

);

});

chrome.notifications.onClosed.addListener(function(notificationId,byUser) {

console.log("onClosed: " + notificationId);

});

chrome.notifications.onButtonClicked.addListener(

function(notificationId,buttonIndex) {

console.log("onButtonClicked: " + buttonIndex);

}

);

//end-region
```

#### 创建和清除通知

要创建一条通知，请使用 `chrome.notifications.create` 方法。该方法接受三个参数：`notificationId`（类型为 `string`）、`notificationOptions`（类型为 `object`）以及一个 `callback` 函数。请注意，`notificationOptions` 对象描述了通知的内容。它支持的完整属性列表可访问 [`developer.chrome.com/apps/notifications#type-NotificationOptions`](https://developer.chrome.com/apps/notifications#type-NotificationOptions)。

`notificationOptions` 对象中一个名为 `type` 的重要属性声明了用于创建通知的模板。其他属性如 `title`、`message`、`buttons`、`imageUrl`、`items`、`progress` 等则定义了模板的具体部分。

图 3-30 至 3-32 包含了不同通知模板的示例。在*代码清单 3-27* 中，请注意用作枚举来选择通知模板的 `NOTIFICATION_TEMPLATE_TYPE` 对象。

您可以通过调用 `chrome.notifications.clear` 方法来清除一条通知，该方法接受 `notificationId` 作为参数。要清除所有通知，请结合使用 `clear` 方法与 `chrome.notifications.getAll` 方法，后者接受一个回调函数作为参数。此回调函数接收一个包含所有通知的对象。

#### 更新通知

要更新一条已有的通知，请使用 `chrome.notifications.update` 方法。该方法接受三个参数：`notificationId`（类型为 `string`）、`notificationOptions`（类型为 `object`）以及一个回调函数。该回调函数接收一个 `Boolean` 参数，指示通知是否已更新。在*代码清单 3-27* 中，请注意 `update` 方法是在 `onClicked` 事件的监听函数内部调用的。

#### 关联事件

通知 API 支持 `onClosed`、`onClicked` 和 `onButtonClicked` 事件。

当通知被关闭时（无论是系统关闭还是用户操作关闭），会触发 `onClosed` 事件。如*代码清单 3-27* 所示，该事件对应的监听函数接收两个参数：(`string`) `notificationId` 和 (`Boolean`) `byUser`。

当用户点击通知中的按钮（见图 3-34）时，会触发 `onButtonClicked` 事件。请注意，按钮是通过 `notificationOptions` 对象中的 `buttons` 属性提供的（请参见*代码清单 3-27*）。此事件的监听函数接收 `notificationId` 和 `buttonIndex`。类似地，当用户点击通知的非按钮区域时，会触发 `onClicked` 事件。在*代码清单 3-27* 中，此事件用于更新通知的 `title` 属性（见图 3-33）。

#### 存储 API

存储 API（即 `chrome.storage` API）用于存储、检索和跟踪用户数据的更改。它需要使用 `storage` 权限。*代码清单 3-28* 包含了来自第 3 章练习文件文件夹中 StorageAPI 扩展的代码。该 API 经过优化，以满足扩展的特定存储需求。它提供了与 `localStorage` API 相同的存储能力，但具有以下关键区别：

- 用户数据可以自动与 Chrome 同步（使用 `chrome.storage.sync` API）。
- 您的扩展内容脚本可以直接访问用户数据，无需*后台页面*。
- 它是异步的，支持批量读写操作，因此比阻塞且串行的 `localStorage` API 更快。
- 用户数据可以存储为对象（而 `localStorage` API 以字符串形式存储数据）。

#### 同步存储与本地存储



## 第 3 章：API 可用性与消息传递

要为您的扩展程序存储用户数据，您可以使用 `storage.sync` 或 `storage.local` API。使用 `storage.sync` 时，只要用户启用了同步功能，存储的数据就会自动同步到用户登录的任何 Chrome 浏览器。

当 Chrome 处于离线状态时，Chrome 会将数据存储在本地。下次浏览器在线时，Chrome 会同步这些数据。即使**用户禁用同步**，`storage.sync` API 仍然会正常工作。在这种情况下，它的行为将与 `storage.local` API 完全相同。

> **注意**：不应存储机密的用户信息！存储区域未加密。

---

## 清单 3-28. 第 [3](http://dx.doi.org/10.1007/978-1-4842-1775-7_3) 章 /StorageAPI/event_script.js

```javascript
//region {variables and functions}

var greeting = "Hello World!";

//end-region

//region {calls}

console.log(greeting);

/*
//single key or a list of keys for items to remove
chrome.storage.sync.remove("color",function() {
    console.log("remove");
    chrome.storage.sync.get("color",function(items) {
        console.log("get");
        console.log(items);
    });
});
*/

chrome.storage.sync.set({"color":"red"},function() {
    console.log("set");
    //string or array of string or object keys
    chrome.storage.sync.get("color",function(items) {
        console.log("get");
        console.log(items);
    });
});

chrome.storage.onChanged.addListener(function(changes,areaName) {
    console.log(changes);
    //"sync","local" or "managed"
    console.log(areaName);
});

//end-region
```

---

#### 设置和获取项目

您可以通过以下 API 调用分别从存储中设置和获取项目：`chrome.storage.sync.set` 和 `chrome.storage.sync.get`。`get` 方法接受字符串、字符串数组或对象键作为其第一个参数，`set` 方法接受一个对象（包含键值对）作为其第一个参数。`get` 方法的第二个参数是一个回调函数，该函数接收一个对象作为其参数。图 3-35 包含了这些方法调用产生的对应日志。

#### 删除项目

您可以通过调用 `chrome.storage.sync.remove` 方法轻松删除存储中的项目。此方法接受一个字符串或字符串数组作为其第一个参数，并可选地接受一个回调函数作为其第二个参数。要从存储中删除所有项目，请使用 `chrome.storage.sync.clear` 方法。

#### 关联事件

存储 API 提供了 `chrome.storage.onChanged` 事件。当一个或多个（存储）项目发生更改时，会触发此事件。对应于此事件的监听器函数接收两个参数：（对象）`changes` 和（字符串）`areaName`。此处，`areaName` 是存储区域的名称（例如 `sync`、`local` 等）。

---

### Tabs API

Tabs API（即 `chrome.tabs` API）用于与浏览器的标签系统进行交互。

您可以使用此 API 在浏览器中创建、修改和重新排列标签页。它需要使用 `tabs` 权限。清单 3-29 包含了来自第 [3 章](http://dx.doi.org/10.1007/978-1-4842-1775-7_3) 练习文件文件夹中的 `TabsAPI` 扩展程序的代码。您已经使用了此 API 中的 `query`、`connect`、`sendMessage`、`executeScript` 和 `insertCSS` 方法。本节将介绍其余的方法。

> **注意**：您可以在扩展程序的清单文件中不声明任何权限的情况下，使用大多数 `chrome.tabs` 方法和事件。但是，如果您需要访问 `tabs.Tab` 的 `url`、`title` 或 `favIconUrl` 属性，则必须在清单中声明 `tabs` 权限。

---

## 清单 3-29. 第 [3](http://dx.doi.org/10.1007/978-1-4842-1775-7_3) 章 /TabsAPI/event_script.js

```javascript
//region {variables and functions}

var greeting = "Hello World!";

var createProperties = {
    url : "http://www.example.org",
    active : false,
};

var updateProperties = {
    pinned : true
};

function getJavaScriptCode(dataUrl) {
    var javascriptCode = "var imgElement = document.createElement('img');";
    javascriptCode += "document.body.appendChild(imgElement);";
```


```javascript
javascriptCode += "imgElement.style.borderTop = '2px dashed silver';";
javascriptCode += "imgElement.src = ";
javascriptCode += "'" + dataUrl + "';";
return javascriptCode;
}

function createAndUpdateTab(tab) {
  chrome.tabs.create(createProperties, function(tab) {
    console.log("create");
    // integer or array of integers
    //chrome.tabs.remove(tab.id);
    /*
    chrome.tabs.duplicate(tab.id,function(tab) {
      console.log("duplicate");
    });
    */
    chrome.tabs.update(tab.id, updateProperties, function(tab) {
      console.log("update");
      //chrome.tabs.reload(tab.id);
      chrome.tabs.getZoom(tab.id, function(zoomFactor) {
        console.log("getZoom");
        console.log(zoomFactor); //1
      });
      /*
      chrome.tabs.setZoom(tab.id,2,function() {
        console.log("setZoom");
      });
      */
    });
  });
}
// end-region
```

## 第 3 章：API 可用性与消息传递

```javascript
//region {calls}
console.log(greeting);

chrome.browserAction.onClicked.addListener(function(tab) {
  chrome.tabs.captureVisibleTab({"format": "png"}, function(dataUrl) {
    // Cannot access a chrome:// URL
    chrome.tabs.executeScript(tab.id, {"code": getJavaScriptCode(dataUrl)});
  });
  //createAndUpdateTab(tab);
});
//end-region
```

#### 创建与删除标签页

要创建一个标签页，请使用 `chrome.tabs.create` 方法。该方法接受两个参数：`createProperties`（`object` 类型）和 `callback`（`function` 类型）。`createProperties` 对象支持以下常用属性：`index`、`url`、`active` 和 `pinned`。`callback` 函数接收一个 `Tab`（`tabs.Tab`）类型的参数。此外，也可以通过调用 `chrome.tabs.duplicate` 方法来复制标签页，如清单 3-29 所示（参见 `createAndUpdateTab` 函数中被注释掉的部分）。

使用 `chrome.tabs.remove` 方法可以关闭一个或多个标签页。该方法将需要关闭的标签页 ID 或 ID 列表作为第一个参数，并支持可选的 `callback` 函数作为第二个参数。

#### 更新标签页

使用 `chrome.tabs.update` 方法可以修改标签页的属性。该方法接受三个参数：`tabId`、`updateProperties` 和 `callback`。你可能已经猜到，`tabId` 是要更新的标签页的 ID。`callback` 函数通过 `tab` 参数接收更新后标签页的详细信息。`updateProperties` 对象指定了需要更新的属性，仅支持以下属性：`url`、`active`、`highlighted`、`pinned`、`muted` 和 `openerTabId`。请注意，你还可以通过调用 `chrome.tabs.reload` 方法来重新加载标签页。该方法将待重新加载的标签页 ID 作为第一个参数，并支持可选的 `callback` 函数作为第二个参数。

***清单 3-30.*** 第 3 章/TabsAPI/manifest.json

```json
{
  "manifest_version": 2,
  "name": "API 演示：tabs API",
  "description": "演示 tabs API 的使用",
  "version": "1.2",
  "background": {
    "scripts": ["event_script.js"],
    "persistent": false
  },
  "browser_action": {
    "default_icon": "icon.png"
  },
  "permissions": [
    "tabs",
    "<all_urls>"
  ]
}
```

> **提示：** 要缩放指定标签页，请使用 `chrome.tabs.setZoom` 方法。此外，要获取指定标签页的当前缩放因子，可使用 `chrome.tabs.getZoom` 方法。

#### 捕获标签页

要捕获指定窗口中当前活动标签页的可见区域，请使用 `chrome.tabs.captureVisibleTab` 方法。该方法接受三个参数：`windowId`、`options` 和 `callback`。`windowId` 参数是可选的，默认为当前窗口。`options` 参数用于指定图像的格式。

`callback` 函数接收 `dataUrl`（字符串）参数——这是一个指向包含被捕获标签页可见区域编码图像数据的 URL。请注意，`dataUrl` 可赋值给 HTML `img` 元素的 `src` 属性，如清单 3-29 所示。对应的图像见图 3-36。同时请注意，此方法需要额外的权限：`<all_urls>`——如清单 3-30 所示。

#### 相关事件

`chrome.tabs` API 提供了与几乎所有可用方法相对应的大量事件。例如，`onCreated`、`onUpdated`、`onRemoved`、`onZoomChange` 等事件。此类事件的完整列表可访问 [`developer.chrome.com/extensions/tabs`](https://developer.chrome.com/extensions/tabs)。

### XHR API

扩展程序可以通过请求跨源权限，与远程服务器通信（使用 `XMLHttpRequest` 对象）。此类权限通过*匹配模式*来申请，这些模式提供了对一个或多个主机的访问权限（参见清单 3-31 中的 `"*://localhost/*"`）。

***清单 3-31.*** 第 3 章/XHRAPI/manifest.json

```json
{
  "manifest_version": 2,
  "name": "API 演示：xhr API",
  "description": "演示 xhr API 的使用",
  "version": "1.2",
  "background": {
    "scripts": ["event_script.js"],
    "persistent": false
  },
  "permissions": [
    "*://localhost/*"
  ]
}
```

一旦申请了主机权限，就可以像普通网页一样使用 XHR API。在清单 3-32 中，请注意 `ucService`（将输入字符串转换为大写的 PHP 脚本）托管在我本地的 HTTP 服务器上。PHP 脚本见清单 3-33。

***清单 3-32.*** 第 3 章/XHRAPI/event_script.js

```javascript
//region {variables and functions}
var greeting = "Hello World!";
var xhr = new XMLHttpRequest();

function onReadyStateChange() {
  if (xhr.readyState == 4) {
    console.log(xhr.responseText);
  }
}

var host = "http://localhost/";
var ucService = host + "Exercise Files/Chapter3/XHRAPI/webpage.php?";
var queryString = "name=" + encodeURIComponent("xhr api");
//end-region

//region {calls}
console.log(greeting);
xhr.onreadystatechange = onReadyStateChange;
xhr.open("GET", ucService + queryString);
xhr.send();
//end-region
```

查询字符串 `"name=xhr api"` 是以静态方式提供的。在您的扩展程序中，可以动态提供，例如来自 Browser-Action 弹出窗口或 Content-UI 等。图 3-37 显示了来自服务器的相应响应。

***清单 3-33.*** 第 3 章/XHRAPI/WebServer/webpage.php

```php
<?php
$raw_input = trim($_GET["name"]);
if(!preg_match("/^[a-zA-Z ]*$/",$raw_input)) {
    echo "";
} else {
    echo strtoupper($raw_input);
}
?>
```

### 本章小结

本章开头讨论了上一章中未涉及的输入组件：多功能框输入、上下文菜单项和 Content-UI。之后，你学习了脚本组件如何利用 Google Chrome 扩展框架提供的消息传递 API 以各种方式进行交互。除此之外，你还学习了普通网页如何与扩展程序进行交互。

接着，你了解了清单文件中的权限字符串的含义，然后学习了如何使用扩展框架提供的许多有用的 API。你还学习了如何在扩展程序中使用 XHR 跨不同源进行通信，这通过在清单文件中使用*匹配模式*权限字符串实现。

在下一章中，你将了解 Chrome 扩展框架提供的其余一些功能，例如主题、替换页面（用于替换新标签页、书签页面等）和选项页面，这些功能可以提升你的扩展在 Chrome 浏览器中的适用性。

---

## **第 4 章：关于扩展的更多内容**



