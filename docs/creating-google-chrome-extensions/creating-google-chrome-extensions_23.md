# 排版后的文档

在当前主题讨论的所有事件中，有一个共同点。它们都代表了扩展框架中的一个事件对象。*事件*是一个对象，允许您在发生某些有趣的事情时得到通知。以下是使用`chrome.alarms.onAlarm`事件在闹钟结束时得到通知的示例。

```javascript
chrome.alarms.onAlarm.addListener(function(alarm) {
  if(alarm.name == "A") {/**/}
  else if(alarm.name == "B") {/**/}
  else {/**/}
});
```

## 第二章 ■ 架构概述

与 DOM API 类似，扩展框架中的事件也需要一个监听器函数，该函数使用`addListener`方法注册。不应将其与 DOM API 的`addEventListener`方法混淆。扩展框架中的每个 API（`browserAction`、`pageAction`、`commands`、`tabs`、`alarms`、`bookmarks`等）都会向监听器函数传递不同类型的参数。对于前面列出的示例，监听器接收一个`Alarm`类型的参数。

除了目前讨论的事件之外，扩展框架还提供了一个高级系统，用于根据某些自定义规则的匹配来执行操作。虽然这听起来类似于典型的事件和监听器模型，但并非如此。扩展框架中的这个高级功能被称为声明式事件处理器，并将在同名主题中讨论。

### 重新审视`onCommand`事件

现在，您可以最终重新审视`HelloShortcutKey`扩展。要监听`onCommand`事件，首先需要在扩展文件夹中创建一个事件脚本。事件脚本可以任意命名，只要是一个有效的文件名即可。接下来，您需要在清单中列出该事件脚本。为此，您需要先定义`background`清单属性。

向此属性添加两个属性：`scripts`和`persistent`。如前所述，`scripts`属性接受一个数组作为其值。事件脚本将列在此数组中。对于此扩展，由于您不希望在浏览器打开期间让事件脚本持续存在（您只希望它们基于事件保持活动状态），您需要将`persistent`定义为`false`，如清单 2-4 所示。

当此扩展在浏览器中加载时，与事件脚本对应的*后台页面*将开始监听任何事件。要处理`onCommand`事件，您需要实现其监听器函数（参见清单 2-5）。当定义的快捷键被调用时，*后台页面*将变为活动状态并执行监听器函数。一旦它进入空闲状态（参见图 2-11），它将变为不活动状态，从而也卸载所有关联的脚本（以节省内存和其他系统资源）。

### 声明式事件处理器

在我们开始讨论声明式事件处理器作为基于自定义规则执行操作的高级机制之前，请将注意力转移到`HelloPageAction`扩展以显示页面操作。该扩展位于第二章的练习文件文件夹中。您可以在浏览器中加载它进行测试。

**清单 2-6.** 第二章/HelloPageAction/manifest.json

```json
{
  "manifest_version": 2,
  "name": "HelloPageAction",
  "description": "Extension to demonstrate a Page-Action",
  "version": "1.2",
  "page_action": {
    "default_title": "HelloPageAction",
    "default_icon": "icon.png",
    "default_popup": "popup.html"
  },
  "background": {
    "scripts": ["event_script.js"],
    "persistent": false
  },
  "permissions": [
    "tabs"
  ]
}
```

### `HelloPageAction`扩展

如清单 2-6 所示，除了声明和定义页面操作外，此扩展的清单还包含用于使用事件脚本的`background`清单属性的定义。除此之外，清单还包含用于使用`tabs`功能的`permissions`属性（您将在第三章中了解`permissions`属性）。为了使事件脚本仅在事件基础上活动，`persistent`被定义为`false`。

**清单 2-7.** 第二章/HelloPageAction/event_script.js

```javascript
//region {variables and functions}
var consoleGreeting = "Hello World! - from event_script.js";
var queryInfoForAllTabs = {
  //"active":false,"currentWindow":true
};

function logTabs(tabs) {
  console.group("Tabs");
  console.log(tabs);
  console.groupEnd("Tabs");
}

function queryTabsAndShowPageActions() {
  chrome.tabs.query(
    queryInfoForAllTabs,
    function(tabs) {
      console.log("All tabs length: %s", tabs.length);
      //Output tabs object to the console as a separate visual group
      logTabs(tabs);
      if(tabs.length > 0) {
        for(var i=0; i<tabs.length; i++) {
          chrome.pageAction.show(tabs[i].id);
        }
      }
    }
  );
}
//end-region
```

清单 2-7 和 2-8 包含了此扩展中使用的事件脚本的代码。函数`queryTabsAndShowPageActions`使用了`chrome.tabs.query` API，根据对象`queryInfoForAllTabs`查询标签页，该对象作为第一个参数传递给`query`方法。此对象为空，以便使`query` API 返回所有标签页。这在您的扩展中是必需的，以便在所有标签页上显示页面操作。

`query`方法的第二个参数是一个回调函数，它接收`tabs`作为其第一个（也是唯一一个）参数。因为它是一个数组，所以您可以对其进行迭代。此数组中的每个元素都是`Tab`类型。在 Chrome 扩展框架中，每个标签页都使用此`Tab`类型表示。此类型有许多关联的属性，其中重要的包括`id`、`active`和`url`。

对于此扩展，您需要`id`属性来显示页面操作。页面操作通过调用`chrome.pageAction.show`方法显示在标签页中，该方法接受标签页的`id`作为其唯一参数。因此，在循环`for(var i=0; i<tabs.length; i++)`内部，调用`show`方法来显示页面操作。

**清单 2-8.** 第二章/HelloPageAction/event_script.js

```javascript
//region {calls}
console.log(consoleGreeting);
//Show Page-Actions using the chrome.tabs.query method
//queryTabsAndShowPageActions();
//Show Page-Actions using the onUpdated event
chrome.tabs.onUpdated.addListener(function(tabId,changeInfo,tab) {
  chrome.pageAction.show(tabId);
});
//end-region
```

然而，使用`queryTabsAndShowPageActions`函数有一个缺点。此函数只会被调用一次，即当事件脚本首次在浏览器中加载时。为了在每次打开（或更新）标签页时显示页面操作，这还不够。为此，您需要监听标签页上的一个事件——`onUpdated`事件。

此事件的使用在清单 2-8 中有所展示。对于此扩展，`queryTabsAndShowPageActions`函数调用已被注释掉。通过简单地监听`onUpdated`事件来显示页面操作。这样，每次标签页被更新（即打开或更新）时，监听器函数都会被调用，并为相应的标签页显示一个页面操作，其 ID 为`tabId`（监听器函数接收的参数，如清单 2-8 所示）。

#### `declarativeContent` API



现在我们可以开始讨论声明式事件处理器了。声明式事件处理器提供了一种定义规则的方式，这些规则由声明式条件和动作组成。条件是在浏览器本身中评估的，而不是在驱动扩展框架的 JavaScript 引擎中。这减少了浏览器与（前述）JavaScript 引擎之间的往返延迟。在撰写本书时，有两种声明式事件处理器——`chrome.declarativeContent` 和 `chrome.declarativeWebRequest`。在本书的学习过程中，你不需要处理后者（因为它不适用于稳定版通道上的 Google Chrome 用户）。

## 第 2 章 ■ 架构概览

你可以使用 `chrome.declarativeContent` API 根据网页内容执行操作，而无需读取网页内容的权限（读取和修改网页内容是通过注入到已访问网页的 content scripts 完成的）。在撰写本书时，仅支持两种此类操作——`chrome.declarativeContent.ShowPageAction` 和 `chrome.declarativeContent.SetIcon`。要使用此 API，需要在 manifest 中声明以下权限：`declarativeContent`。

#### 添加和移除规则

作为声明式 API，该 API 允许你在 `onPageChanged` 事件上注册规则，即使用 `chrome.declarativeContent.onPageChanged.*`。其中，`*` 可以表示 `addRules` 或 `removeRules`。`addRules` 方法将规则实例数组作为第一个参数，并将一个可选的回调函数（在完成时调用）作为第二个参数。

```
var rule1 = {
    id : "some_rule_A", //可选，未设置时会自动生成
    priority : 100, //可选，默认值为 100
    conditions : [/*条件*/],
    actions : [/*动作*/]
};
...
var ruleList = [rule1,rule2,...];
chrome.declarativeContent.onPageChanged.addRules(ruleList);
```

要移除规则，请调用 `removeRules` 方法。它接受一个可选的规则标识符数组（例如 `[rule1.id, rule2.id]`）作为第一个参数，以及一个回调函数作为第二个参数。如果该数组为 `undefined`，则会移除该扩展的所有已注册规则。

***清单 2-9.*** 第 2 章/PageActionNotes/event_script.js

```
//region {变量和函数}
var consoleGreeting = "Hello World! - from event_script.js";
var ruleStackOverflowHost = {
    "conditions" : [
        new chrome.declarativeContent.PageStateMatcher({
            "pageUrl" : {
                "hostEquals" : "stackoverflow.com",
                "schemes" : ["http","https"]
            }
        })
    ],
    "actions" : [new chrome.declarativeContent.ShowPageAction()]
};
//end-region
```

> **注意：** 规则在浏览会话之间是持久化的。因此，你应该在扩展安装期间，使用 `runtime.onInstalled` 事件来添加规则。请注意，该事件在扩展更新时也会触发。因此，你应该先清除之前安装的规则，然后再注册新规则。有关规则的更多详细信息，请访问以下网址：<https://developer.chrome.com/extensions/events>。

***清单 2-10.*** 第 2 章/PageActionNotes/event_script.js

```
//region {函数调用}
console.log(consoleGreeting);
chrome.runtime.onInstalled.addListener(function() {
    //替换所有规则
    chrome.declarativeContent.onPageChanged.removeRules(undefined,function() {
        //使用新规则
        chrome.declarativeContent.onPageChanged.addRules(
            [ruleStackOverflowHost]
        );
    });
});
//end-region
```

#### 使用 ShowPageAction

在上一主题“HelloPageAction 扩展”中，演示了如何使用 `chrome.tabs.query` 方法和 `chrome.tabs.onUpdated` 事件显示 Page-Action。现在，你将了解另一种使用声明式事件处理器动作来实现的方法。

> **注意：** 有关此主题的深入介绍，请访问该网址。



