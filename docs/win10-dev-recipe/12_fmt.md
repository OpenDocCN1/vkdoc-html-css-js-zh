# 十二、Windows 应用中的位置和地图

构建 Windows 应用的开发人员可以利用 Windows 设备的功能将位置和地图集成到他们的应用中。在这一章中，你将看到如何在通用 Windows 运行时应用中使用位置 API 的方法。

## 12.1 获取当前位置

### 问题

你不知道你在哪里。Windows 应用需要确定并显示您的当前位置。

### 解决办法

使用在`Windows.Devices.Geolocation`命名空间中定义的`Geolocator`类的`getGeopositionAsync`方法，从 Windows 10 支持的设备中获取当前位置。

### 它是如何工作的

Create a new Universal Windows project using the Universal Windows template, which can be found under the `JavaScript ➤ Windows ➤ Universal` node of the New Project dialog in Microsoft Visual Studio 2015\. This creates a single project in the Visual Studio Solution with the necessary files in it to start with. The first step to integrate the location functionality in the app is to declare the Location capability in the `package.appxmanifest` file of the project. From Visual Studio Solution Explorer, double-click the `package.appxmanifest` file. In the GUI designer, click the Capabilities tab and select Location, as shown in Figure 12-1.

![A978-1-4842-0719-2_12_Fig1_HTML.jpg](img/A978-1-4842-0719-2_12_Fig1_HTML.jpg)
图 12-1。

Capabilities tab in Visual Studio

Open the project `default.html` file and add the following code under the body tag of each file:

```html
<div>
    <p>Get the Current Location</p>
    <br />
    <button id="btnLocation">Get Location</button> <br />
</div>
<br />
<div>
    <table>
        <tr>
            <td>
                Latitude
            </td>
            <td >
                <div id="latitude">
                </div>
            </td>
        </tr>
        <tr>
            <td>
                Longitude
            </td>
            <td>
                <div id="longitude">
                </div>
            </td>
        </tr>
        <tr>
            <td>
                Accuracy
            </td>
            <td>
                <div id="accuracy">
                </div>
            </td>
        </tr>
    </table>
</div>
```

Open `default.js` (`/js/default.js`) in the project and replace the code in the file with the following:

```javascript
// For an introduction to the Blank template, see the following documentation:
//
// http://go.microsoft.com/fwlink/?LinkID=392286
(function () {
    "use strict";
    var app = WinJS.Application;
    var activation = Windows.ApplicationModel.Activation;
    app.onactivated = function (args) {
        if (args.detail.kind === activation.ActivationKind.launch) {
            if (args.detail.previousExecutionState !== activation.ApplicationExecutionState.terminated) {
            } else {
            }
            args.setPromise(WinJS.UI.processAll().
               done(function ()
               {
                   // Add an event handler to the button.
                   document.querySelector("#btnLocation").addEventListener("click",
                       getLocation);
               }));
        }
    };
    var geolocation = null;
    function getLocation()
    {
        if (geolocation == null) {
            geolocation = new Windows.Devices.Geolocation.Geolocator();
        }
        if (geolocation != null) {
            geolocation.getGeopositionAsync().then(getPosition);
        }
    }
    function getPosition(position)
    {
        document.getElementById('latitude').innerHTML = position.coordinate.point.position.latitude;
        document.getElementById('longitude').innerHTML = position.coordinate.point.position.longitude;
        document.getElementById('accuracy').innerHTML = position.coordinate.accuracy;
    }
    app.oncheckpoint = function (args) {
    };
    app.start();
})();
```

The `document.querySelector` is used to add the click event handler for the `btnLocation` element. Upon click of the button, the `getLocation` method is called.

To get the current location information, create an instance of the `Geolocator` class defined in the `Windows.Devices.Geolocation` namespace and then invoke the `getGeopositionAsync` method of the `Geolocator` class. Once the location is retrieved, an action method needs to be defined to perform a certain set of actions. The `getPosition` method, as shown in the preceding code snippet, takes care of this. The parameter `position` is of type `Geolocation`, which can be used to get the current location information like latitude, longitude, accuracy, and so forth.

The next step is to build the project and run it in the emulator or local machine.

Choose the Build menu and select Build Solution from Visual Studio to build the project. Select Mobile Emulator from the Run drop-down menu in the Visual Studio standard toolbar.

When you run the app for the first time on Windows, you are prompted to confirm if it is OK to use your location. Click Allow so that the app can use the location.

In the application, click the Get Location button to display the current location’s latitude and longitude, as shown in Figure 12-2.

![A978-1-4842-0719-2_12_Fig2_HTML.jpg](img/A978-1-4842-0719-2_12_Fig2_HTML.jpg)

图 12-2。

Current location in Windows Mobile app

When you click the app on the local machine, you are prompted with a message, as shown in Figure 12-3. Click the Allow button. This displays the current location (see Figure 12-4).

![A978-1-4842-0719-2_12_Fig4_HTML.jpg](img/A978-1-4842-0719-2_12_Fig4_HTML.jpg)

图 12-4。

Current location in a Windows desktop family app

![A978-1-4842-0719-2_12_Fig3_HTML.jpg](img/A978-1-4842-0719-2_12_Fig3_HTML.jpg)

图 12-3。

Allow current location prompt in Windows desktop family app

> **Note**
> 确保在您的 Windows 设备中启用了定位服务，以获取当前位置。

## 12.2 响应地理定位器位置更新

### 问题

您希望经常检查应用中的位置是否有任何变化。

### 解决办法

使用在`Windows.Devices.Geolocation`名称空间中定义的`Geolocator`类的`getGeopositionAsync`方法。订阅`PositionChanged`和`LocationChanged`事件，跟踪位置变化，并通过 Windows Mobile 或桌面系列应用做出响应。

### 它是如何工作的

# 创建通用 Windows 项目

使用通用 Windows 模板创建一个新的通用 Windows 项目。该模板可以在 Microsoft Visual Studio 2015 的“新建项目”对话框中，通过 JavaScript ➤ Windows ➤ Universal 节点找到。这将在 Visual Studio 解决方案中创建一个包含必要起始文件的单一项目。

在项目的 `package.appxmanifest` 文件中启用“位置”功能。

从 Visual Studio 解决方案资源管理器中打开 `default.html` 文件，然后在每个文件的 `<body>` 标签下添加以下代码。

```html
<div>
    <button id="start">Start Tracking</button><br />
    <br />
    <button id="stop">Stop Tracking</button><br />
</div>
<br />
<div>
    <table>
        <tr>
            <td>
                Latitude
            </td>
            <td>
                <div id="latitude">
                </div>
            </td>
        </tr>
        <tr>
            <td>
                Longitude
            </td>
            <td>
                <div id="longitude">
                </div>
            </td>
        </tr>
        <tr>
            <td>
                Accuracy
            </td>
            <td>
                <div id="accuracy">
                </div>
            </td>
        </tr>
        <tr>
            <td>
                <div id="Status"></div>
            </td>
        </tr>
    </table>
</div>
```

前面的 HTML 代码与食谱 12.1 中的代码类似，但包含一个用于显示状态的额外 `<div>` 标签。

从 Visual Studio 解决方案资源管理器中打开项目中的 `default.js` (`/js/default.js`) 文件，并用以下代码替换文件中的内容：

```javascript
(function () {
    "use strict";
    var app = WinJS.Application;
    var activation = Windows.ApplicationModel.Activation;
    app.onactivated = function (args) {
        if (args.detail.kind === activation.ActivationKind.launch) {
            if (args.detail.previousExecutionState !== activation.ApplicationExecutionState.terminated) {
            } else {
            }
            args.setPromise(WinJS.UI.processAll().
                 done(function () {
                     // Add an event handler to the button.
                     document.querySelector("#start").addEventListener("click",
                         Starttracking);
                     // Add an event handler to the button.
                     document.querySelector("#stop").addEventListener("click",
                         Stoptracking);
                 }));
        }
    };
    var geolocation = null;
    // Start tracking
    function Starttracking() {
        if (geolocation == null)
        {
            geolocation = new Windows.Devices.Geolocation.Geolocator();
            geolocation.reportInterval = 100;
        }
        if (geolocation != null)
        {
            geolocation.addEventListener("positionchanged", onPositionChanged);
            geolocation.addEventListener("statuschanged", onStatusChanged);
        }
    }
    // On change of location position , update the UI
    function onPositionChanged(args) {
        document.getElementById('latitude').innerHTML = args.position.coordinate.point.position.latitude;
        document.getElementById('longitude').innerHTML = args.position.coordinate.point.position.longitude;
        document.getElementById('accuracy').innerHTML = args.position.coordinate.accuracy;
    }
    // Stop the tracking
    function Stoptracking()
    {
        if (geolocation != null) {
            geolocation.removeEventListener("positionchanged", onPositionChanged);
        }
    }
    // event handler for the Status Changed method.
    function onStatusChanged(args) {
        var Status = args.status;
        document.getElementById('Status').innerHTML =
            getStatus(Status);
    }
    // Gets the status
    function getStatus(Status) {
        switch (Status) {
            case Windows.Devices.Geolocation.PositionStatus.ready:
                return "Ready";
                break;
            case Windows.Devices.Geolocation.PositionStatus.initializing:
                return "Initializing";
                break;
            case Windows.Devices.Geolocation.PositionStatus.disabled:
                return "Location is disabled . Check the Location settings in your device or Appxmanifest file";
                break;
            case Windows.Devices.Geolocation.PositionStatus.notInitialized:
                return "Not Initialized";
            default:
                return "Status us unknown";
        }
    }
    app.oncheckpoint = function (args) {
    };
    app.start();
})();
```

## 代码解释

第一步是为“开始跟踪”和“停止跟踪”按钮添加点击事件处理程序。使用 `document.querySelector` 来添加事件监听器。

```javascript
document.querySelector("#start").addEventListener("click",Starttracking);
document.querySelector("#stop").addEventListener("click",Stoptracking);
```

在 `Starttracking` 方法中创建了 `Geolocator` 类的一个新实例，并设置了 `reportInterval`。`reportInterval` 定义了位置更新之间的最小时间间隔（以毫秒为单位）。

```javascript
geolocation = new Windows.Devices.Geolocation.Geolocator();
geolocation.reportInterval = 100;
```

`positionchanged` 和 `statuschanged` 事件监听器被添加到 `geolocation` 实例：

```javascript
geolocation.addEventListener("positionchanged", onPositionChanged);
geolocation.addEventListener("statuschanged", onStatusChanged);
```

当位置发生变化时，会触发 `positionchanged` 事件。当 `Geolocator` 提供更新位置的能力发生变化时（例如，位置被禁用或初始化等），会触发 `statuschanged` 事件。

`getStatus` 方法根据 `Windows.Devices.Geolocation.PositionStatus` 返回相应的消息。

当不需要跟踪位置时，只需移除 `positionchanged` 事件监听器，如下所示：

```javascript
geolocation.removeEventListener("positionchanged", onPositionChanged);
```

现在，在模拟器中构建并运行项目。

在应用中，单击“开始跟踪”按钮。该应用通过 `Geolocator` 的 `Onpositionchanged` 事件订阅位置更新，并显示位置信息（参见图 12-5）。

![A978-1-4842-0719-2_12_Fig5_HTML.jpg](img/A978-1-4842-0719-2_12_Fig5_HTML.jpg)

图 12-5。

Location updates using the Windows Mobile emulator’s additional tools When the app is run on Windows desktop using the `Local Machine` option, you should see the screen shown in Figure 12-6.

![A978-1-4842-0719-2_12_Fig6_HTML.jpg](img/A978-1-4842-0719-2_12_Fig6_HTML.jpg)

图 12-6。

Location updates in Windows desktop using `Local Machine` option

**Note**

Windows Mobile 模拟器提供了额外的工具，让开发者在开发过程中测试基于位置的应用，如图 12-5 所示。

## 12.3 用 HTML5 检测用户的位置

### 问题

你不知道你在哪里。Windows 应用需要确定并显示您的当前位置。

### 解决办法

除了 Windows 运行时 API，WinJS 应用还可以利用 W3C 地理位置 API，通过使用 Windows 应用中的 `HTML5` 来检测用户的当前位置。

### 它是如何工作的

# 使用 HTML5 在 Windows Universal 应用中获取地理位置

## 创建项目与声明权限

使用 Universal Windows 模板创建一个新的 Universal Windows 项目。该模板可在 Microsoft Visual Studio 2015 的“新建项目”对话框的 JavaScript ➤ Windows ➤ Universal 节点下找到。此操作将在 Visual Studio 解决方案中创建一个包含必要起始文件的项目。

集成地理位置功能的第一步是在 Windows 应用的 `package.appxmanifest` 文件中声明 Location 权限。在 Visual Studio 解决方案资源管理器中，双击 `package.appxmanifest` 文件。在图形设计器中，单击“功能”选项卡并选择“位置”。

## 添加用户界面

打开 Windows 应用的 `default.html` 文件，并在文件的 `<body>` 标签下添加以下代码：

```html
<h1>Current Location using HTML5</h1>
<button id="btnLocation">Get Location</button> <br />
<label>Latitude</label> <div id="latitude"></div><br />
<label>Longitude</label> <div id="longitude"> </div><br />
<div id="status"> </div><br />
```

## 编写 JavaScript 逻辑

打开项目中的 `default.js` (`/js/default.js`) 文件。将文件中的代码替换为以下内容：

```javascript
(function () {
    "use strict";
    var app = WinJS.Application;
    var activation = Windows.ApplicationModel.Activation;
    app.onactivated = function (args) {
        if (args.detail.kind === activation.ActivationKind.launch) {
            if (args.detail.previousExecutionState !== activation.ApplicationExecutionState.terminated) {
            } else {
            }
            args.setPromise(WinJS.UI.processAll().
                 done(function () {
                     // Add an event handler to the button.
                     document.querySelector("#btnLocation").addEventListener("click",
                         GetLocation);
                 }));
        }
    };
    var nav = null;
    function GetLocation() {
        if (nav == null) {
            nav = window.navigator;
        }
        var geoloc = nav.geolocation;
        if (geoloc != null) {
            geoloc.getCurrentPosition(Onsuccess, Onerror);
        }
    }
    // after getting the location information
    function Onsuccess(position) {
        document.getElementById("latitude").innerHTML =
            position.coords.latitude;
        document.getElementById("longitude").innerHTML =
            position.coords.longitude;
    }
    // On error when trying to get the location
    function Onerror(error) {
        var errorMessage = "";
        switch (error.code) {
            case error.PERMISSION_DENIED:
                errorMessage = "Location is disabled";
                break;
            case error.POSITION_UNAVAILABLE:
                errorMessage = "Data unavailable";
                break;
            case error.TIMEOUT:
                errorMessage = "Timeout error";
                break;
            default:
                break;
        }
        document.getElementById("status").innerHTML = errorMessage;
    }
    app.oncheckpoint = function (args) {
    };
    app.start();
})();
```

## 添加事件处理与位置获取

为“Get Location”按钮添加点击事件处理程序。`document.querySelector` 用于获取按钮控件，`addEventListener` 方法用于添加点击事件并将其映射到 `GetLocation` 方法。

```javascript
document.querySelector("#btnLocation").addEventListener("click",
                         GetLocation);
```

`window.navigator.geolocation` 类的 `getCurrentPosition` 方法用于获取用户的当前位置。

```javascript
if (nav == null) {
    nav = window.navigator;
}
var geoloc = nav.geolocation;
if (geoloc != null) {
    geoloc.getCurrentPosition(Onsuccess, Onerror);
}
```

成功获取当前位置后，控制权将传递给 `Onsuccess` 方法以处理坐标并显示。如果出错，则会调用 `Onerror` 方法，在获取位置信息出现问题时显示错误消息。

## 运行与测试

在本地计算机上生成并运行项目。

在应用中单击“Get Location”按钮，如图 12-7 所示。应用会立即提示用户允许其使用位置 API。单击“允许”后，应立即看到当前位置的纬度和经度。

![A978-1-4842-0719-2_12_Fig7_HTML.jpg](img/A978-1-4842-0719-2_12_Fig7_HTML.jpg)
图 12-7。

`Current location in Windows Store app using HTML5`   Note

`W3C` 地理定位 API 目前仅适用于 `Windows` 桌面设备系列应用。当您在 `Windows Mobile` 仿真程序上运行相同的时，状态显示为禁用。

## 12.4 使用 `HTML5` 检测位置更新

### 问题

您希望经常检查应用中的位置是否有任何变化。

### 解决办法

您可以使用 `HTML5` 中的 `W3C` 地理定位 API 来检测 `Windows` 应用中的位置变化。

### 它是如何工作的

# 集成HTML5位置功能到通用Windows应用

## 创建项目
使用通用Windows模板创建一个新的通用Windows项目。该模板可在Microsoft Visual Studio 2015的“新建项目”对话框的“JavaScript ➤ Windows ➤ Universal”节点下找到。这将在Visual Studio解决方案中创建一个包含必要起始文件的单一项目。

## 声明位置能力
将位置功能集成到应用中的第一步，是在Windows应用的`package.appxmanifest`文件中声明位置能力。从Visual Studio解决方案资源管理器中，双击`package.appxmanifest`文件。在图形设计器中，单击“功能”选项卡并勾选“位置”。

## 添加HTML代码
打开Windows应用的`default.html`文件，并在文件的`<body>`标签下添加以下代码：
```html
<h1>Current Location using HTML5</h1>
<button id="btnstart">Start</button>
<button id="btnstop">Stop</button>
<br />
<label>Latitude</label>
<div id="latitude"></div><br />
<label>Longitude</label>
<div id="longitude"> </div><br />
<div id="status"> </div><br />
```

## 实现JavaScript逻辑
打开Windows项目中的`default.js`（`/js/default.js`）文件，并用以下代码替换文件中的内容：
```javascript
(function () {
    "use strict";
    var app = WinJS.Application;
    var activation = Windows.ApplicationModel.Activation;
    app.onactivated = function (args) {
        if (args.detail.kind === activation.ActivationKind.launch) {
            if (args.detail.previousExecutionState !== activation.ApplicationExecutionState.terminated) {
            } else {
            }
            args.setPromise(WinJS.UI.processAll().
                done(function () {
                    document.querySelector("#btnstart").addEventListener("click",
                        starttracking);
                    document.querySelector("#btnstop").addEventListener("click",
                        stoptracking);
                }));
        }
    };
    var geolocation = null;
    var positionInstance;
    // on click of the start tracking
    function starttracking() {
        if (geolocation == null) {
            geolocation = window.navigator.geolocation;
        }
        if (geolocation != null) {
            positionInstance = geolocation.watchPosition(onsuccess, onerror);
        }
    }
    // on click of the stop tracking button
    function stoptracking() {
        geolocation.clearWatch(positionInstance);
    }
    // on success of getting the location
    function onsuccess(pos) {
        document.getElementById('latitude').innerHTML = pos.coords.latitude;
        document.getElementById('longitude').innerHTML = pos.coords.longitude;
    }
    // On error when trying to get the location
    function Onerror(error) {
        var errorMessage = "";
        switch (error.code) {
            case error.PERMISSION_DENIED:
                errorMessage = "Location is disabled";
                break;
            case error.POSITION_UNAVAILABLE:
                errorMessage = "Data unavailable";
                break;
            case error.TIMEOUT:
                errorMessage = "Timeout error";
                break;
            default:
                break;
        }
        document.getElementById("status").innerHTML = errorMessage;
    }
    app.oncheckpoint = function (args) {
    };
    app.start();
})();
```

## 添加按钮事件处理
为“开始跟踪”和“停止跟踪”按钮添加点击事件处理程序。`document.querySelector`用于获取按钮控件，`addEventListener`方法用于向跟踪按钮添加点击事件。跟踪由`window.navigator.geolocation`类的`watchPosition`方法处理，如下所示：
```javascript
if (geolocation == null) {
    geolocation = window.navigator.geolocation;
}
if (geolocation != null) {
    positionInstance = geolocation.watchPosition(onsuccess, onerror);
}
```
当坐标被检索到时，`Onsuccess`方法被调用，用于处理结果并显示它。当获取位置时出现任何问题，`Onerror`方法会使用相应的错误码被调用，开发者可以利用该错误码为每个错误显示用户友好的消息。要停止跟踪，需要调用地理定位的`clearWatch`方法，并提供最初通过`watchPosition`函数检索到的`watchid`参数。
```javascript
geolocation.clearWatch(positionInstance);
```

## 运行与测试
在本地计算机上生成并运行项目。

在应用中，单击“开始”按钮。应用会立即提示用户允许使用位置API。单击“允许”后，应立即看到当前位置的纬度和经度，并且位置跟踪开始，如图12-8所示。

![A978-1-4842-0719-2_12_Fig8_HTML.jpg](img/A978-1-4842-0719-2_12_Fig8_HTML.jpg)

图 12-8。

Detect location updates in Windows Store app using HTML5   Note

W3C 地理定位 API 目前仅适用于 Windows 桌面设备系列。Windows 10 Mobile 不支持它。

# 12.5 在内置地图应用中显示地图

## 问题

您希望在内置的地图应用中显示地图并标出位置。

## 解决办法

使用通用 Windows 应用中的`bingmaps:` URI 方案在内置地图应用中显示地图。

## 它是如何工作的

Create a new Universal Windows project using the Universal Windows template, which can be found under JavaScript ➤ Windows ➤ Universal node of the New Project dialog in Microsoft Visual Studio 2015\. This creates a single project in the Visual Studio Solution with the necessary files in it to start with.   Open the `default.html` file from the project, and add the following code under the body tag of the file: `<h1>Display Map</h1>` `<button id="btnDisplayMap">Display Map</button> <br />`   Open `default.js` (`/js/default.js`) in the project and replace the code in the file with the following:

```javascript
(function () {
    "use strict";
    var app = WinJS.Application;
    var activation = Windows.ApplicationModel.Activation;
    app.onactivated = function (args) {
        if (args.detail.kind === activation.ActivationKind.launch) {
            if (args.detail.previousExecutionState !== activation.ApplicationExecutionState.terminated) {
            } else {
            }
            args.setPromise(WinJS.UI.processAll().
                done(function () {
                    // Add an event handler to the button.
                    document.querySelector("#btnDisplayMap").addEventListener("click",
                        DisplayMap);
                }));
        }
    };
    // Method to display the Built-in Map.
    function DisplayMap() {
        var latitude = "12.917264";
        var longitude = "77.634786";
        var uri = "bingmaps:?cp="+ latitude + "∼" + longitude + "lvl=10";
        Windows.System.Launcher.launchUriAsync(new Windows.Foundation.Uri(uri));
    }
    app.oncheckpoint = function (args) {
    };
    app.start();
})();
```

URI 计划可用于从您的 Windows 应用启动地图应用。`LaunchUriAsync`方法通常用于使用 URI 方案从 Windows 应用商店应用启动另一个应用。在这种情况下，`bingmaps:` URI 方案用于启动地图应用。

Note

使用`LaunchUriAsync`方法时，用户将被带到设备上的另一个应用，用户必须在使用地图应用后手动返回到您的应用。

开发者可以向 URI 方案提供适当的参数来显示位置，甚至在地图上显示路线。例如，以下 URI 方案打开 Bing 地图应用，并显示一幅以印度班加罗尔市为中心的地图:

`bingmaps:?cp=12.917264∼77.634786`

开发人员可以使用表 12-1 中所示的一些参数以及`bingmaps:` URI 方案。

表 12-1。

Examples of the Different Parameters Used with the `bingmaps:` URI

| 参数 | 例子 |
| --- | --- |
| 中心点 | `cp=40.726966∼-74.006076` |
| 边界框 | `bb=39.719∼-74.52∼41.71∼-73.5` |
| q(查询术语或搜索术语) | `q=Mexican restaurant` |
| 缩放级别 | `lvl=10.50` |
| trfc(指定在地图中包含交通信息) | `trfc=1` |
| 路由协议 | `rtp=adr. One%20Microsoft%20Way%20Redmond%20WA%2098052` |

Note

必应地图包含了很多参数；表 12-1 只显示了其中的几个。关于必应地图参数的更多信息可以在 [`http://msdn.microsoft.com/en-us/library/windows/apps/xaml/jj635237.aspx`](http://msdn.microsoft.com/en-us/library/windows/apps/xaml/jj635237.aspx) 找到。

![A978-1-4842-0719-2_12_Fig9_HTML.jpg](img/A978-1-4842-0719-2_12_Fig9_HTML.jpg)

图 12-9。

Built-in maps in the Windows Mobile app Now, build the project and run it. Click the Display Map button on the screen. If you are running the Windows app on the Windows Mobile emulator, you should see the built-in map with the location being plotted, as shown in Figure 12-9.  

如果应用使用本地机器选项在 Windows 桌面上运行，地图应用会显示位置(参见图 12-10 )。

![A978-1-4842-0719-2_12_Fig10_HTML.jpg](img/A978-1-4842-0719-2_12_Fig10_HTML.jpg)

图 12-10。

Built-in maps in Windows desktop family Note

URI 方案是 Windows Phone 8.1 和更早版本中可用的 MapsTask 启动器的替代品。

# 12.6 在内置地图应用中显示方向

## 问题

您希望在 Windows 设备的内置地图应用中显示从一个位置到另一个位置的方向或路线。

## 解决办法

带有`rtp`参数的`bingmaps:` URI 方案可以在您的 Windows 应用中使用，以在内置的地图应用中显示地图，以及显示从一个位置到另一个位置的点对点驾驶方向。

## 它是如何工作的

# 12.7 Windows 应用商店应用中的 Bing 地图控件

## 问题

您希望在 Windows 应用中使用地图，而不是启动内置地图应用。

## 解决办法

使用 Bing Maps AJAX Control 7.0 在使用 WinJS 开发的通用 Windows 应用中添加地图功能。

## 它是如何工作的

Bing 地图是微软提供的在线地图服务，允许用户使用微软的地图解决方案并利用 Bing 地图的各种功能。Bing Maps AJAX Control 7.0 和 Bing Maps REST 服务为开发人员提供了独特的机会，可以轻松地将位置和搜索功能纳入他们的移动和 web 应用。

要使用 Bing Maps AJAX Control 7.0，请按照下列步骤操作:

1.  从 [`http://go.microsoft.com/fwlink/?LinkID=322092`](http://go.microsoft.com/fwlink/?LinkID=322092) 下载并安装适用于 Windows 应用商店应用的 Bing Maps SDK（适用于 Windows 8.1，也适用于 Windows 10）。
2.  获取 Bing Maps 密钥。访问 Bing Maps 账户中心 [`https://www.bingmapsportal.com`](https://www.bingmapsportal.com) 并为应用程序创建一个密钥。您必须在 Windows 应用商店应用中使用此密钥。
3.  在 Microsoft Visual Studio 2013 中使用 Windows 应用商店应用模板创建一个新项目，这将创建一个 Windows 应用商店应用。
4.  将 Bing Maps for JavaScript 的引用添加到项目中。在解决方案资源管理器中，右键单击项目引用，选择“添加引用”，然后选择“Bing Maps for JavaScript”。单击“确定”，如图 12-13 所示。

## 创建项目并添加代码

使用 Universal Windows 模板创建一个新的通用 Windows 项目，该模板可以在 Microsoft Visual Studio 2015 的“新建项目”对话框的 JavaScript ➤ Windows ➤ Universal 节点下找到。这将在 Visual Studio 解决方案中创建一个包含必要文件的单个项目。

使用 Visual Studio 解决方案资源管理器打开项目中的 `default.html` 文件，并在每个文件的 `body` 标签下添加以下代码：
```html
<h1>Display Map</h1>
<button id="btnDisplayRoute">Display Route</button> <br />
```

打开项目中的 `default.js` (`/js/default.js`) 文件，并将文件中的代码替换为以下内容：
```javascript
(function () {
    "use strict";
    var app = WinJS.Application;
    var activation = Windows.ApplicationModel.Activation;
    app.onactivated = function (args) {
        if (args.detail.kind === activation.ActivationKind.launch) {
            if (args.detail.previousExecutionState !== activation.ApplicationExecutionState.terminated) {
            } else {
            }
            args.setPromise(WinJS.UI.processAll().
                 done(function () {
                     // Add an event handler to the button.
                     document.querySelector("#btnDisplayRoute").addEventListener("click",
                         DisplayRoute);
                 }));
        }
    };
    function DisplayRoute() {
        var fromAddress = "adr.HSR Layout 5th sector, Bangalore";
        var toAddress = "adr.Microsoft India,Signature Building,Bangalore";
        var uri = "bingmaps:?rtp=" + fromAddress + "∼"+ toAddress;
        Windows.System.Launcher.launchUriAsync(new Windows.Foundation.Uri(uri));
        //Windows.Services.Maps.MapManager.showDownloadedMapsUI()
    }
    app.oncheckpoint = function (args) {
    };
    app.start();
})();
```

## 使用 URI 方案启动地图

URI 方案可用于从 Windows 应用中启动地图应用。`LaunchUriAsync` 方法通常用于使用 URI 方案从 Windows 应用启动另一个应用。在这种情况下，带有 `rtp` 参数的 `bingmaps:` URI 方案用于启动内置地图，然后显示从指定地址到指定位置的行驶方向。

> **Note**
> 使用 `LaunchUriAsync` 方法时，用户被带到设备上的另一个 app。用户在使用地图应用后，必须手动返回到您的应用。

开发人员可以将 `trfc=1` 参数与 URI 一起显示交通信息。
```javascript
"bingmaps:?rtp=adr.HSR Layout 5th sector, Bangalore∼adr.Microsoft India,Signature Building,Bangalore&trfc=1";
```
在前面的 URI 方案中，`adr.` 定义了地址。`rtp` 需要两个路径点来寻找路线。提供 `trfc` 参数也是为了显示交通信息。

![A978-1-4842-0719-2_12_Fig11_HTML.jpg](img/A978-1-4842-0719-2_12_Fig11_HTML.jpg)
图 12-11。Windows Mobile 中的路线指引

构建项目并运行它。单击屏幕上的“Display Map”按钮。如果您在 Windows Mobile 模拟器上运行 Windows 应用，您应该会看到内置地图并绘制位置，如图 12-11 所示。

如果 Windows 应用在桌面系列上运行，地图应用用于显示位置，如图 12-12 所示。

![A978-1-4842-0719-2_12_Fig12_HTML.jpg](img/A978-1-4842-0719-2_12_Fig12_HTML.jpg)
图 12-12。Windows 应用中的路线指引

> **Note**
> URI 方案是 Windows Phone 8 中 `MapsDirectionsTask` 启动器的替代品。

![A978-1-4842-0719-2_12_Fig13_HTML.jpg](img/A978-1-4842-0719-2_12_Fig13_HTML.jpg)

图 12-13。

Reference Manager in Visual Studio for Bing Maps

Open the `default.html` file in the project and add the following code in the body tag of the file: `<div id="myMap"></div>`

In the `default.html` file, also add the reference to the Bing Maps JavaScript files:
```html
<!-- Bing Maps references -->
<script type="text/javascript" src="ms-appx:///Bing.Maps.JavaScript//js/veapicore.js"></script>
<script type="text/javascript" src="ms-appx:///Bing.Maps.JavaScript//js/veapimodules.js"></script>
```

Open `default.js` (`/js/default.js`) and replace the code in the file with the following:
```javascript
(function () {
    "use strict";
    var app = WinJS.Application;
    var activation = Windows.ApplicationModel.Activation;
    app.onactivated = function (args) {
        if (args.detail.kind === activation.ActivationKind.launch) {
            if (args.detail.previousExecutionState !== activation.ApplicationExecutionState.terminated) {
            } else {
            }
            args.setPromise(WinJS.UI.processAll().
                done(function () {
                    Microsoft.Maps.loadModule('Microsoft.Maps.Map', { callback: GetMap });
                }));
        }
    };
    var map;
    function GetMap() {
        //   Microsoft.Maps.loadModule('Microsoft.Maps.Map', { callback: GetMap });
        var loc = new Microsoft.Maps.Location(13.0220, 77.4908);
        // Initialize the map
        map = new Microsoft.Maps.Map(document.getElementById("myMap"), {
            credentials: "Bing Map Key",
            zoom: 10
        });
        var pin = new Microsoft.Maps.Pushpin(loc);
        map.entities.push(pin);
        // Center the map on the location
        map.setView({ center: loc, zoom: 10 });
    }
    app.oncheckpoint = function (args) {
    };
    app.start();
})();
```

The `loadModule` of the `Microsoft.Maps` function needs to be called first to load the map. `loadModule` has an optional culture parameter that can be used to identify the localized language and the region. The `GetMap` callback function is specified for `loadModule`.
```javascript
Microsoft.Maps.loadModule('Microsoft.Maps.Map', { callback: GetMap });
```

An instance of the `Microsoft.Maps.Map` is created by specifying the container `div` element and the Bing Map API key.
```javascript
// Initialize the map
map = new Microsoft.Maps.Map(document.getElementById("myMap"), {
    credentials: "Bing Map API Key",
});
```

The location on the map where the pushpin is to be added is identified using the `map.entities.push` method.
```javascript
var loc = new Microsoft.Maps.Location(13.0220, 77.4908);
var pin = new Microsoft.Maps.Pushpin(loc);
map.entities.push(pin);
```

Finally, the map is displayed by setting the location at the center and with a zoom level value of 10.

Build the application and run it in the Windows emulator. You should be able to view the map and the pushpin added to the map, as shown in Figure 12-14.

![A978-1-4842-0719-2_12_Fig14_HTML.jpg](img/A978-1-4842-0719-2_12_Fig14_HTML.jpg)

图 12-14。

## Bing Maps in Windows app

Bing Maps AJAX 控件提供了额外的 API，允许开发人员集成额外的地图功能，如显示交通信息、方向等。

目前，WinJS 库没有在 SDK 中提供现成的地图控件，因此 Bing Maps AJAX 控件是一个很好的替代解决方案。