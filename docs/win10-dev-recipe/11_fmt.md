# 十一、后台任务

在任何应用中，您可能必须在后台静默执行某些任务。通用 Windows 平台提供了后台任务功能，允许您在后台运行代码。后台任务只不过是操作系统在后台运行的 JavaScript 文件。后台任务通常根据您可以订阅的系统触发器运行。需要在应用任务中声明一个后台任务。后台任务可以通过某些事件向应用报告进度、完成和取消。

## 11.1 后台任务的系统事件触发器

### 问题

您决定在应用中实现一个后台任务。您希望了解可以订阅的各种系统事件触发器，并在触发器发生时运行后台任务。

### 解决办法

系统事件通过一个名为`SystemTrigger`的全系统触发对象可用。当创建一个新的后台任务时，您需要让操作系统知道您想要订阅哪个系统触发器。您创建了一个新的`SystemTrigger`实例，并在构造函数中传递了触发器类型。触发类型在`SystemTriggerType`枚举中定义。表 11-1 列出了触发器及其描述。

表 11-1。

System Trigger Types

| 触发器名称 | 描述 |
| --- | --- |
| 互联网可用 | 互联网变得可用 |
| 网络状态改变 | 网络发生了变化 |
| OnlineIdConnectedStateChange | 与帐户更改相关联的在线 id |
| SMS 接收器 | 接收到新的 SMS 消息 |
| 时区变更 | 设备上的时区发生变化 |

## 11.2 创建和注册后台任务

### 问题

您希望为您的应用创建一个后台任务，并将其注册到操作系统。

### 解决办法

使用在`Windows.ApplicationModel.Background`名称空间中找到的`BackgroundTaskBuilder`创建一个新的后台任务并注册它。

### 它是如何工作的

Open Visual Studio 2015 Community edition. Select File ➤ New Project. In the New Project dialog window, select Templates ➤ JavaScript ➤ Windows ➤ Universal from the Installed Templates section (see Figure 11-1). Select Blank App (Universal Windows) from the available project templates. Provide a name and location for the app and click OK.

![A978-1-4842-0719-2_11_Fig1_HTML.jpg](img/A978-1-4842-0719-2_11_Fig1_HTML.jpg)

图 11-1。

New Project dialog

Visual Studio prepares the project, which will look like what’s shown in Figure 11-2 once done.

![A978-1-4842-0719-2_11_Fig2_HTML.jpg](img/A978-1-4842-0719-2_11_Fig2_HTML.jpg)

图 11-2。

Solution Explorer

Create a new JavaScript file in your `js` folder. Let’s name the file `mytask.js`. This `js` file contains the logic for the background work/task that you want to perform. You will use the file name of this `js` file to register the task, so keep note of the file name.

Here is the skeleton code of the `mytask.js` file:

```javascript
(function () {
    "use strict";
    //gets the current instance of the background task
    var backgroundTaskInstance = Windows.UI.WebUI.WebUIBackgroundTaskInstance.current;
    var canceled = false,
        settings = Windows.Storage.ApplicationData.current.localSettings,
        key = backgroundTaskInstance.task.name;
    backgroundTaskInstance.addEventListener("canceled", OnCanceled);
    //checking if the background task is cancelled by the user.
    if (!canceled) {
        doWork();
    }
    else {
        settings.values[key] = "Canceled";
        close();
    }
    //the function which performs the main work as part of our background task
    function doWork() {
        settings.values[key] = "Starting"
        //Your code to do work in the background
        //...
        settings.values[key] = "Succeeded";
        settings.values[key] = "Closing";
        //call close when task is done
        close();
    }
    function OnCanceled(sender, reason) {
        canceled = true;
    }
})();
```

让我们检查一下代码。使用`WebUIBackgroundTaskInstance.current`属性获得后台任务的一个实例。然后，将取消的事件处理程序附加到后台任务的取消。然后定义一个`doWork()`函数，它作为后台任务的一部分完成实际工作。您使用`localSettings`并更新任务的状态，以便主应用可以读取相同的状态并在 UI 上执行任何更新。注意对`close()`函数的调用；后台任务应该在完成工作后调用这个函数。接下来，注册刚刚创建的任务。

In `default.js`, once the app has been activated, register the `mytask.js` background task that you just created. The following are the steps involved in registering a task.

Iterate over all the tasks using the `BackgroundTaskRegistration.allTasks` property. It is important to check if your task is already registered or not. If you don’t check for this and register blindly, the background task is registered multiple times, which may lead to unexpected results. Here is the code snippet to do this:

```javascript
var task = null,
    taskRegistered = false,
    background = Windows.ApplicationModel.Background,
    iter = background.BackgroundTaskRegistration.allTasks.first();

while (iter.hasCurrent) {
    task = iter.current.value;
    if (task.name === exampleTaskName) {
        taskRegistered = true;
        break;
    }
    iter.moveNext();
}
```

If the task is not already registered, perform the registration by using the `BackgroundTaskBuilder` class. You need to set the system trigger on which the task should be invoked. In this example, you listen to `timeZoneChange` as the trigger for the background task. Here is the code snippet:

```javascript
if (taskRegistered != true) {
    var builder = new Windows.ApplicationModel.Background.BackgroundTaskBuilder();
    var trigger = new Windows.ApplicationModel.Background.SystemTrigger(
        Windows.ApplicationModel.Background.SystemTriggerType.timeZoneChange, false);
    builder.name = exampleTaskName;
    builder.taskEntryPoint = "js\\mytask.js";
    builder.setTrigger(trigger);
    task = builder.register();
}
```

Next, you need to handle the task completion. Add a completed event handler on the task itself. Here is the code snippet:

```javascript
task.addEventListener("completed", function (args) {
    var settings = Windows.Storage.ApplicationData.current.localSettings;
    var key = args.target.name;
    settings.values[key] = "Completed";
});
```

Here is the complete code snippet for registering the background task:

```javascript
app.onactivated = function (args) {
    if (args.detail.kind === activation.ActivationKind.launch) {
        if (args.detail.previousExecutionState !==
            activation.ApplicationExecutionState.terminated) {
            var task = null,
                taskRegistered = false,
                background = Windows.ApplicationModel.Background,
                iter = background.BackgroundTaskRegistration.allTasks.first();

            while (iter.hasCurrent) {
                task = iter.current.value;
                if (task.name === exampleTaskName) {
                    taskRegistered = true;
                    break;
                }
                iter.moveNext();
            }

            if (taskRegistered != true) {
                var builder = new Windows.ApplicationModel.Background.BackgroundTaskBuilder();
                var trigger = new Windows.ApplicationModel.Background.SystemTrigger(
                    Windows.ApplicationModel.Background.SystemTriggerType.timeZoneChange, false);
                builder.name = exampleTaskName;
                builder.taskEntryPoint = "js\\mytask.js";
                builder.setTrigger(trigger);
                task = builder.register();
            }

            task.addEventListener("completed", function (args) {
                var settings = Windows.Storage.ApplicationData.current.localSettings;
                var key = args.target.name;
                settings.values[key] = "Completed";
            });
        } else {
        }
        args.setPromise(WinJS.UI.processAll());
    }
};
```

Next, you need to add a declaration for the background task in the application manifest file. Open the manifest file by double-clicking `package.appxmanifest`, which is in the root of the application.

Click the **Declarations** tab. Select **Background Tasks** from the available declarations drop-down list. Click the **Add** button to add a declaration.

In the **Supported task types** section, select **System event**.

In the **App settings** section, under the **Start page** entry, add `js\mytask.js` as the value.

Save the package manifest file.

使用前面的代码，您已经创建了一个自定义后台任务，并将其注册到操作系统中。您使用系统触发事件，即时区更改事件来运行任务。按 F5 运行应用，然后更改您系统的时区设置。时区一改变，就会触发后台任务。

## 11.3 设置运行后台任务的条件

### 问题

您希望创建并注册一个自定义后台任务。但是您希望后台任务仅在满足特定条件时运行；例如，用户在场或用户不在场，等等。

### 解决办法

后台任务仅在触发 set 触发器时运行。正如在前面的配方中看到的，您在创建任务时提供了触发器。如果您的任务需要满足某个条件，即使在系统触发器被触发之后，您也可以创建一个系统条件，并在任务注册期间将该条件提供给任务生成器。使用`SystemConditionType`枚举提供条件。表 11-2 描述了 UWP 上可用的系统条件类型。

### 表 11-2 System Condition Types

| 触发器名称 | 价值 | 描述 |
| --- | --- | --- |
| 病人 | Zero | 不是有效的条件类型 |
| 用户呈现 | one | 只有当用户在场时，任务才能运行 |
| 用户不存在 | Two | 只有当用户不在场时，任务才能运行 |
| 互联网可用 | three | 任务只能在 Internet 可用时运行 |
| 互联网不可用 | four | 任务只能在互联网不可用时运行 |
| 会话连接 | five | 仅当连接了用户会话时，任务才能运行 |
| 会话断开连接 | six | 仅当用户会话断开时，任务才能运行 |
| 免费网络可用 | seven | 任务只能在免费网络(非计量)可用时运行 |
| 背景工作成本 | eight | 仅当后台工作的成本较低时，任务才能运行 |

### 它是如何工作的

让我们学习如何为任务设置系统条件。

创建一个`SystemCondition`对象。在注册任务之前，需要构建后台任务运行所需应用的条件。您需要创建一个`SystemCondition`对象来表示该条件。`SystemCondition`对象的构造函数需要`SystemConditionType`枚举值，这是在运行任务之前需要满足的条件。以下是提供条件的代码片段：`var internetConditionType = Windows.ApplicationModel.Background.SystemConditionType.InternetAvailable;` `var internetCondition = new Windows.ApplicationModel.Background.SystemCondition(internetConditionType);`

将`SystemCondition`对象添加到后台任务。构建系统条件后，下一步是将其添加到任务生成器。`BackgroundTaskBuilder`提供了一个`AddCondition()`方法，用于设置条件。以下是添加条件的代码片段：`taskBuilder.AddCondition(internetCondition);`

接下来，您使用`TaskBuilder`的`Register()`方法注册任务。以下是任务注册的代码片段：`var task = builder.Register();`

## 11.4 监控后台任务进度和完成情况

### 问题

您有一个由您的应用创建和注册的后台任务。您希望在应用中监控任务的进度和完成情况。

### 解决办法

在系统中注册的任务会触发进度和完成事件。您的应用需要提供事件处理程序，并订阅任务公开的事件。

### 它是如何工作的

处理任务完成。首先，您需要创建一个函数来附加到后台任务的`Completed`事件。此函数接受一个`BackgroundTaskCompletedEventArgs`类型的参数。以下是该函数的框架：
```javascript
function onCompleted(args){
    //code to deal with the completion of the task
}
```
接下来，您需要将该函数注册到后台任务。以下是执行此操作的代码片段：
```javascript
task = builder.register();
task.addEventListener("completed",onCompleted);
```
将任何需要在任务完成时执行的代码放在`onCompleted`函数中。

处理任务进度。与完成事件类似，任务也会触发进度事件。您需要编写一个函数来附加到后台任务的进度事件。通过订阅进度事件，每当任务触发进度事件时，附加到该事件的函数就会被调用。任何进度报告例程都可以写在该函数中。该函数接受两个参数：一个`IBackgroundTaskRegistration`对象和一个`BackgroundTaskProgressEventArgs`对象。以下是处理进度事件的函数框架：
```javascript
function onProgress(task, args){
    //add code to perform progress related routine here
}
```
接下来，您需要将该函数注册到后台任务的进度事件。以下是注册该函数的代码片段：
```javascript
task = builder.register();
task.addEventListener("progress", onProgress);
```