# 八、全球化和本地化

有机会在近 240 个 Windows 市场中推广您的 Windows 应用。目标受众在文化、地区和语言方面各不相同。您的应用的用户可能位于世界上的任何地方。他们可能说不同的语言，甚至是多种语言。作为应用开发人员，您有必要使您的应用适应多种语言、市场、文化和地区。全球化意味着让您的应用了解文化、语言和地区。本地化是将您的应用的某些方面本地化到它正在运行的文化的能力；例如，日期、数字或货币格式等等。在这一章中，你将看到如何全球化和本地化你的应用的方法。

## 8.1 使用资源字符串

### 问题

当你的 app 在非英语文化中使用，文本不适应新的文化和语言；相反，用户看到的是英文文本。

### 解决办法

为了全球化应用，有必要使用资源字符串来代替静态文本。例如，应用中的任何标签文本都不应该硬编码为静态文本。相反，应该为您需要支持的文化/语言创建资源文件。添加将文本翻译成相应语言的字符串。标签文本也使用资源字符串，而不是硬编码。

### 它是如何工作的

让我们看看资源字符串在应用中的用法，以及如何添加它们。

Open Visual Studio 2015. Select File ➤ New Project ➤ JavaScript ➤ Windows ➤ Universal and select Blank App (Universal Windows) template. This will create a universal app with the necessary files that can run on machines powered by Windows 10.

![A978-1-4842-0719-2_8_Fig1_HTML.jpg](img/A978-1-4842-0719-2_8_Fig1_HTML.jpg)

图 8-1。

Choosing the Blank App (Universal Windows) template in the New Project window

Open the `package.appxmanifest` file from the project in Visual Studio solution explorer. Go to the Application tab and check that the default language is set to `en-US`. When a new project is created, the language is set to `en-US` by default (see Figure 8-2).

![A978-1-4842-0719-2_8_Fig2_HTML.jpg](img/A978-1-4842-0719-2_8_Fig2_HTML.jpg)

图 8-2。

Default language settings in the application manifest

Create a resource folder, as follows:

![A978-1-4842-0719-2_8_Fig3_HTML.jpg](img/A978-1-4842-0719-2_8_Fig3_HTML.jpg)

图 8-3。

# Strings folder in the Windows project

在 Solution Explorer 中，选择 Universal Windows 项目并右键单击它。从上下文菜单中选择 Add ➤ New Folder。

将新文件夹命名为 `strings`。你将在此文件夹内创建不同的文化/语言资源文件（参见图 8-3）。

创建一个子文件夹和一个英语资源文件，步骤如下：

1.  在 `strings` 文件夹（在步骤 3 中创建）中创建一个名为 `en-US` 的文件夹。
2.  右键单击 `en-US` 文件夹，选择 Add ➤ New Item。
3.  从模板中选择 Text File，并将文件命名为 `resources.resjson`。建议在命名资源文件时使用此默认名称。
4.  将文件内容替换为以下内容：

```json
{
    "greeting"          : "Hello World!",
    "_greeting.comment" : "Hello World Text comment",
    "farewell"          : "Goodbye",
    "_farewell.comment" : "A farewell comment."
}
```

资源文件本质上是一个包含键值对的 JSON 文件。这里，`"greeting"` 和 `"farewell"` 标识了将要显示的字符串。其他键——`"_greeting.comment"` 和 `"_farewell.comment"` 是描述字符串本身的注释。为所有字符串提供有意义的注释是一个好习惯。

使用字符串资源标识符执行以下操作：

1.  从 Visual Studio 解决方案资源管理器中打开项目中的 `default.js` 文件。此文件可在项目的 `js` 文件夹中找到。
2.  将以下代码行添加到 `app.onactivated` 函数中：

```javascript
WinJS.Resources.processAll();
```

完成后的代码应如下所示：

```javascript
app.onactivated = function (args) {
    if (args.detail.kind === activation.ActivationKind.launch) {
        if (args.detail.previousExecutionState !== activation.ApplicationExecutionState.terminated) {
            WinJS.Resources.processAll();
        } else {
        }
        args.setPromise(WinJS.UI.processAll());
    }
};
```

要在标记中使用资源字符串，请打开 `default.html` 文件并在 body 中添加以下内容：

```html
<h2>
    <span data-win-res="{textContent: 'greeting'}"></span>
</h2>
<h2>
    <span data-win-res="{textContent: 'farewell'}"></span>
</h2>
```

添加其他语言资源文件：

1.  在项目的 `strings` 文件夹中，添加一个名为 `de-DE` 的文件夹。这是用于德语文化。
2.  在 `de-DE` 文件夹中添加一个资源文件。将资源文件命名为 `resources.resjson`。将文件内容替换为以下代码：

```json
{
    "greeting"          : "Hallo Welt!",
    "_greeting.comment" : " Hello World Text comment.",
    "farewell"          : "Auf Wiedersehen",
    "_farewell.comment" : "A farewell comment."
}
```

3.  在项目的 `strings` 文件夹中添加另一个文件夹，并将其命名为 `fr-FR`。这是用于法语文化。
4.  在 `fr-FR` 文件夹中添加一个资源文件。此资源文件名也应为 `resources.resjson`。将文件内容替换为以下代码：

```json
{
    "greeting"          : "Salut tout le monde",
    "_greeting.comment" : "Hello World Text comment.",
    "farewell"          : "Au revoir",
    "_farewell.comment" : "A farewell comment."
}
```

运行应用程序，步骤如下：

1.  按 F5 构建并运行应用程序。
2.  问候和告别消息将以用户在设备上设置的首选语言显示。图 8-4 显示了使用 Local Machine 选项运行时在 Windows 屏幕上的输出。

![A978-1-4842-0719-2_8_Fig4_HTML.jpg](img/A978-1-4842-0719-2_8_Fig4_HTML.jpg)

图 8-4。英语文化资源字符串显示

3.  更改设备上的语言设置并再次运行应用程序。图 8-5 显示了在设备上将语言设置为法语时显示的相同文本。

![A978-1-4842-0719-2_8_Fig5_HTML.jpg](img/A978-1-4842-0719-2_8_Fig5_HTML.jpg)

图 8-5。法语文化资源字符串显示。

请注意，在 Windows Mobile 上更改语言设置时，您必须重新启动设备。

## 8.2 格式化日期、时间、数字和货币

### 问题

当语言更改为非英语区域性/语言时，日期、时间、数字和货币显示英语区域性设置。

### 解决办法

# 8.3 本地化 WinJS 控件

应用中的静态文本内容可以通过使用资源字符串进行本地化。但是如果您的应用必须显示日期、时间、数字或货币，您就不能使用资源字符串。相反，您需要使用`WinJS.Globalization`名称空间，它提供了日期时间、数字和货币格式助手方法的功能。

## 它是如何工作的

### 格式化日期和时间

以下步骤解释了如何设置日期和时间的格式。

使用`Windows.Globalization.DateTimeFormatting`命名空间中的`DateTimeFormatter`。使用模板格式创建一个`DateTimeFormatter`实例。有关模板格式的列表，您可以查看MSDN文档：[`http://msdn.microsoft.com/en-us/library/windows/apps/windows.globalization.datetimeformatting.datetimeformatter.aspx`](http://msdn.microsoft.com/en-us/library/windows/apps/windows.globalization.datetimeformatting.datetimeformatter.aspx)。

在`default.html`文件的`body`元素内添加以下标记：

```html
<h1>Formatting Demo</h1>
<br />
<h3>Long Date:</h3>
<h3 id="spnDate"></h3>
<br />
<h3>Long Time:</h3>
<h3 id="spnTime"></h3>
```

修改`default.js`中的`onactivated`方法如下：

```javascript
app.onactivated = function (args) {
    if (args.detail.kind === activation.ActivationKind.launch) {
        if (args.detail.previousExecutionState !== activation.ApplicationExecutionState.terminated) {
            var lang = Windows.System.UserProfile.GlobalizationPreferences.languages[0];
            var shortDateFmt = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("longdate", [lang]);
            var shortTimeFmt = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("longtime",[lang]);
            var currentDateTime = new Date();
            var shortDate = shortDateFmt.format(currentDateTime);
            var shortTime = shortTimeFmt.format(currentDateTime);
            document.getElementById("spnDate").innerHTML = shortDate;
            document.getElementById("spnTime").innerHTML = shortTime;
        } else {
            // TODO: This application has been reactivated from suspension.
            // Restore application state here.
        }
        args.setPromise(WinJS.UI.processAll());
    }
};
```

运行应用程序并验证输出（参见图8-6和图8-7）。

![A978-1-4842-0719-2_8_Fig7_HTML.jpg](img/A978-1-4842-0719-2_8_Fig7_HTML.jpg)

图 8-7。法语文化格式的输出

![A978-1-4842-0719-2_8_Fig6_HTML.jpg](img/A978-1-4842-0719-2_8_Fig6_HTML.jpg)

图 8-6。英语文化格式的输出

图8-6显示英语文化格式的日期，图8-7显示法语文化格式的日期。

### 格式化数字和货币

以下步骤解释了如何设置数字和货币的格式：

应使用`NumberFormatting`来显示十进制数、百分比和货币。

在`default.html`中添加以下标记，并追加到`body`：

```html
<h3>Number Format:</h3>
<h3 id="spnCurrency"></h3>
```

修改`default.js`中的`onactivated`方法，并追加以下代码：

```javascript
var userCurrency = Windows.System.UserProfile
                   .GlobalizationPreferences.currencies;
var number = 23456.78
var currFmt = new Windows.Globalization.NumberFormatting
                   .CurrencyFormatter(userCurrency[0],[lang]);
var formattedCurrency = currFmt.format(number);
document.getElementById("spnCurrency").innerHTML =
                   formattedCurrency;
```

按F5运行应用程序。更改手机或平板电脑上的语言设置，以查看数字和货币格式的实际效果（参见图8-8和图8-9）。

![A978-1-4842-0719-2_8_Fig9_HTML.jpg](img/A978-1-4842-0719-2_8_Fig9_HTML.jpg)

图 8-9。法语文化中的日期和货币格式化输出

![A978-1-4842-0719-2_8_Fig8_HTML.jpg](img/A978-1-4842-0719-2_8_Fig8_HTML.jpg)

图 8-8。英语文化中的日期和货币格式化输出

## 问题

当区域性或语言更改时，WinJS 控件不显示本地化字符串。

## 解决办法

通常，像`label`或`title`这样的控件属性被设置在标记中。这有点像对字符串值进行硬编码。因此，当区域性或语言更改时，控件不会适应，并按原样显示硬编码的字符串。通过使用资源文件，可以将 WinJS 控件属性绑定到本地化字符串。WinJS 提供了直接将控件属性绑定到资源文件中的资源键的灵活性。

## 它是如何工作的

下列步骤解释如何本地化 WinJS 控件：

Open Visual Studio 2015. Select `File` ➤ `New Project` ➤ `JavaScript` ➤ `Windows` ➤ `Blank App (Universal Windows)`. This will create a universal app which is a single project that can run on devices powered by Windows 10.

Add resource files to the project (see Recipe 8.1 on how to add resource files).

Add the following string keys:
```json
{
    "ShowMoreTilesTitle"            : "Show more Tiles",
    "_ShowMoreTilesTitle.comment"   : "show more tiles toggle title",
    "VibrateOnCallTitle"            : "Vibrate on Call",
    "_VibrateOnCallTitle.comment"   : "vibrate on call toggle title",
    "ShowArtistTitle"               : "Show artist when playing music",
    "_ShowArtistTitle.comment"      : "Show artist when playing music toggle title",
    "ToggleOn"                      : "Yes",
    "_ToggleOn.comment"             : "show more tiles toggle on label",
    "ToggleOff"                     : "No",
    "_ToggleOff.comment"            : "show more tiles toggle off label"
}
```
Similarly, I have also added strings for other languages.

Add the following code to the `default.js` `onactivated` method:
```javascript
WinJS.UI.processAll().then(function(){
     WinJS.Resources.processAll();
});
```
You need to call `processAll()` on the `WinJS.Resource` class because you will be referring to the resource strings in the markup.

Add the following markup to `default.html` inside the body element:
```html
<div>
    <h1>Controls Localization</h1>
    <br />
    <div class="toggle" data-win-control="WinJS.UI.ToggleSwitch"
         data-win-res="{winControl: {labelOn:'ToggleOn',
                                     labelOff:'ToggleOff',
                                     title:'ShowMoreTilesTitle'}}"
         data-win-options="">
    </div>
    <br />
    <div class="toggle" data-win-control="WinJS.UI.ToggleSwitch"
         data-win-res="{winControl: {labelOn:'ToggleOn',
                                     labelOff:'ToggleOff',
                                     title:'VibrateOnCallTitle'}}"
         data-win-options="">
    </div>
    <br />
    <div class="toggle" data-win-control="WinJS.UI.ToggleSwitch"
         data-win-res="{winControl: {labelOn:'ToggleOn',
                                     labelOff:'ToggleOff',
                                     title:'ShowArtistTitle'}}"
         data-win-options="">
    </div>
</div>
```
Notice that you use `data-win-res` and refer the resource strings by their key names, and assign it to a property of the control. Usually, it is of the following pattern:
```html
data-win-res="{winControl: {propertyName1:'resourceID1',
                            propertyName2:'resourceID2'}}"
```

Press F5 and run the app. Initially, you will see an English string used by the control. Change the Language in the device and then run the app again. Now it will pick up the appropriate resource file and use the appropriate resource strings (see Figure 8-9 and Figure 8-10).

![A978-1-4842-0719-2_8_Fig11_HTML.jpg](img/A978-1-4842-0719-2_8_Fig11_HTML.jpg)

图 8-11。Formatted output in English culture

![A978-1-4842-0719-2_8_Fig10_HTML.jpg](img/A978-1-4842-0719-2_8_Fig10_HTML.jpg)

图 8-10。Formatted output in French culture