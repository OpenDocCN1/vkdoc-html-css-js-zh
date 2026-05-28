# 十八、商店和货币化

到目前为止，您应该已经完成了针对 Windows 10 操作系统的通用 Windows 平台应用的开发。您需要构建您的应用，并将其上传到 Windows 应用商店，用户可以在那里搜索和下载应用。在本章中，您将了解在 Windows 应用商店中获得您的应用所需的东西，以及如何利用应用内广告赚钱。

## 18.1 创建一个 Windows 应用开发者帐户

### 问题

您需要构建您的应用并将其上传到 Windows 应用商店。但要做到这一点，您需要首先使用您的开发人员帐户登录 Windows 开发人员中心。在开始上传阶段之前，您需要知道如何创建开发人员帐户。

### 解决办法

在线创建一个开发者帐户。有两种方法可以开始这个过程。

*   直接去 [`http://dev.windows.com`](http://dev.windows.com) 。
*   通过 Visual Studio 访问帐户注册。

#### 通过 Windows 开发中心注册访问帐户

Windows 开发中心是 Windows 应用开发的一站式门户。该门户提供了您开始 Windows 10 应用开发所需的工具。有代码示例、如何开发 Windows 10 应用的教程，当然还有提交应用的条款。可以在 [`http://dev.windows.com`](http://dev.windows.com) 访问 Windows 开发中心。当您导航到此页面时，您会看到一个获取开发人员帐户链接。点按它并按照屏幕指示完成创建您自己的开发人员帐户。开发者帐户允许您将应用(适用于所有 Windows 设备)提交到 Windows 应用商店。它还允许您管理您的应用，并分析您的应用在商店中的表现。

您可以注册以下类别之一:

*   个人:此帐户类型允许您以个人、学生或非法人团体的身份开发和销售应用。这种账户的费用是 19 美元。
*   公司:这种帐户类型适用于拥有注册商业名称的公司，用于开发和销售应用。这种账户的费用是 99 美元。

您需要提供以下信息才能获得帐户:

*   您的联系信息
*   要显示的发布者名称
*   支付方式(VISA/MasterCard/PayPal)

#### 通过 Visual Studio 注册访问帐户

您也可以通过 Visual Studio 注册为开发人员。

在 Visual Studio 中，从项目菜单中选择 Store ➤ Open Developer Account(参见图 18-1 )。

![A978-1-4842-0719-2_18_Fig1_HTML.jpg](img/A978-1-4842-0719-2_18_Fig1_HTML.jpg)

图 18-1。Open a developer account from Visual Studio

此操作会打开一个新的浏览器窗口，并将您直接带到帐户注册页面。你必须按照屏幕上的指示来完成帐户注册。

## 18.2 为 Windows 10 打包一个通用 Windows 平台应用

### 问题

您已经完成了应用的开发。您已经完成了 Windows 开发人员中心的帐户注册。现在，您想要打包您的应用，以便提交到商店。

### 解决办法

要销售或分发 Windows 应用，您需要创建一个应用包或 appxupload 包，这是一个技术术语。使用通用 Windows 平台(UWP)，您可以生成一个包(`.appxupload`)。此包已上载到 Windows 应用商店。一旦应用进入商店，就可以在任何 Windows 10 设备上安装和运行，包括手机、平板电脑、个人电脑等。

### 它是如何工作的

打包 Windows 10 应用需要分两步完成。首先，用某些属性和设置配置包。然后，生成要上传到商店的包。

#### 配置应用包

要创建应用包，您需要首先设置描述您的应用的某些属性和设置。应用属性和设置存储在一个名为应用清单文件的文件中，该文件位于项目的根目录下，名为`package.appxmanifest`。您在清单中设置的一些属性/设置是用于应用切片的图像或应用支持的方向。

应用清单文件是一个 XML 文件。Visual Studio 提供了一个基于 GUI 的清单设计器/编辑器来编辑该文件。使用 GUI 设计器/编辑器，很容易对应用清单进行更改。

以下步骤提供了如何配置软件包的说明:

![A978-1-4842-0719-2_18_Fig2_HTML.jpg](img/A978-1-4842-0719-2_18_Fig2_HTML.jpg)

图 18-2。Application package manifest editing

在解决方案资源管理器中，展开您应用程序的项目节点。双击`package.appxmanifest`文件。Visual Studio 将启动清单设计器/编辑器（参见图 18-2）。

清单文件有几个选项卡，用于配置应用的不同方面。

*   应用:配置应用的显示名称、起始页、默认语言、描述、支持的方向、锁屏通知模式和磁贴更新信息。
*   Visual Assets: Configures your app’s visual assets, such as tile images and logos, the badge logo, and the splash screen (see Figure 18-3).

    ![A978-1-4842-0719-2_18_Fig3_HTML.jpg](img/A978-1-4842-0719-2_18_Fig3_HTML.jpg)

    图 18-3。Visual Assets

*   Capabilities: Any capabilities that your app needs has to be declared in this area of the manifest file (see Figure 18-4).

    ![A978-1-4842-0719-2_18_Fig4_HTML.jpg](img/A978-1-4842-0719-2_18_Fig4_HTML.jpg)

    图 18-4。Capabilities

*   Declarations: Use to add any declarations (for example, protocol or share target) for your app and sets their properties (see Figure 18-5).

    ![A978-1-4842-0719-2_18_Fig5_HTML.jpg](img/A978-1-4842-0719-2_18_Fig5_HTML.jpg)

    图 18-5。Declarations

*   Content URIs: Specifies which pages in the app can be navigated to by a frame, and URIs that can be navigated to when loaded in a web view (see Figure 18-6).

    ![A978-1-4842-0719-2_18_Fig6_HTML.jpg](img/A978-1-4842-0719-2_18_Fig6_HTML.jpg)

    图 18-6。Content URIs

*   Packaging: Sets the package details, such as package name (note: this is not the application name, rather just the package name), package display name, version details, publisher, publisher display name, and package family name (see Figure 18-7).

    ![A978-1-4842-0719-2_18_Fig7_HTML.jpg](img/A978-1-4842-0719-2_18_Fig7_HTML.jpg)

    图 18-7。Packaging

#### 创建应用包

在使用清单文件配置应用包之后，接下来要做的事情是生成或创建包。该包是一个 appxupload 文件。Visual Studio 提供了创建应用包向导，您接下来将使用该向导。按照以下步骤创建包:

## 创建应用包

在 Solution Explorer 中，打开你的通用 Windows 应用项目解决方案。在 Solution Explorer 中右键单击你的项目，从上下文菜单中选择 `Store` ➤ `Create App Packages`（参见图 18-8）。

![A978-1-4842-0719-2_18_Fig8_HTML.jpg](img/A978-1-4842-0719-2_18_Fig8_HTML.jpg)

图 18-8。

`Create App Packages` 向导将被调用。在“创建你的包”对话框中，选择“是”以构建包并上传到 Windows 应用商店（参见图 18-9）。

![A978-1-4842-0719-2_18_Fig9_HTML.jpg](img/A978-1-4842-0719-2_18_Fig9_HTML.jpg)

图 18-9。

`Create App Package` 向导。如果你选择“否”，Visual Studio 将不会创建提交到商店所需的 `.appxupload` 文件。此选项仅在你只想将应用旁加载到内部设备上运行时使用。

接下来，你需要登录到你的 Windows 开发人员中心帐户（参见图 18-10）。

![A978-1-4842-0719-2_18_Fig10_HTML.jpg](img/A978-1-4842-0719-2_18_Fig10_HTML.jpg)

图 18-10。

`Create App Package` 开发人员中心登录。

接下来，你需要为你的包选择一个名称，或者你可以从向导中为你的包预留一个名称。你选择的名称需要在 Windows 应用商店中是唯一的（参见图 18-11）。

![A978-1-4842-0719-2_18_Fig11_HTML.jpg](img/A978-1-4842-0719-2_18_Fig11_HTML.jpg)

图 18-11。

选择应用名称。

接下来，你需要选择并配置包信息：包将保存的输出位置，以及构建的版本和体系结构配置。确保选择所有三个体系结构选项（参见图 18-12）。

![A978-1-4842-0719-2_18_Fig12_HTML.jpg](img/A978-1-4842-0719-2_18_Fig12_HTML.jpg)

图 18-12。

配置包详细信息。

单击“创建”以生成 `appxupload` 包，该包将在选定的输出位置生成。然后你可以将 `appxupload` 包提交到商店。接下来，你将看到“包创建完成”对话框（参见图 18-13）。

![A978-1-4842-0719-2_18_Fig13_HTML.jpg](img/A978-1-4842-0719-2_18_Fig13_HTML.jpg)

图 18-13。

创建完成对话框。

在将应用提交到商店进行认证之前，验证你的应用非常重要。可以使用作为 SDK 一部分安装在你的计算机上的 `Windows App Certification Kit (WACK)` 进行验证。验证可以在你的本地计算机或远程计算机上进行。

要在本地验证，请在“包创建完成”对话框中选择“本地计算机”单选按钮。单击“启动 `Windows App Certification Kit`”按钮。`Windows App Certification Kit` 将执行测试并显示结果。

如果你的应用通过了测试，你就可以将应用提交到商店了。

## 18.3 向 Windows 应用商店提交应用

### 问题

你已经完成了包的创建。已为你的应用生成了一个 `appxupload` 包。你现在需要将应用提交到商店进行认证。

### 解决办法

为你的应用创建 `appxupload` 包后，下一步是将其提交到商店进行认证。认证后，你的应用会列在 Windows 应用商店中，以便用户可以搜索和下载安装。你可以使用 Windows 开发人员中心仪表板将应用包提交到 Windows 应用商店。下面概述了提交的过程。

使用你的开发人员中心帐户凭据登录到 Windows 开发人员中心。访问 `http://dev.windows.com` 并登录。

单击 `Dashboard` 链接。

在仪表板页面上，你会在页面左侧看到 `My Apps` 部分。在此部分下，你在包创建期间预留的应用名称会以“进行中”状态列出。单击该应用名称（参见图 18-14）。

![A978-1-4842-0719-2_18_Fig14_HTML.jpg](img/A978-1-4842-0719-2_18_Fig14_HTML.jpg)

图 18-14。

# Apps listing in Windows Dev Center dashboard

Next, you are presented with the App Overview page. Here you need to click the `Start your submission` button in the `Submissions` section.

Next, you are presented with the submission screen. Here you need to provide information related to pricing, the application properties, the application package, which countries you want your app to be available in, and any other instructions for certification team.

## Pricing and availability

Here you provide the pricing for your app or decide to list it as free. You also have option to choose the countries in which you want to list your app.

## App Properties

Here you provide information such as the category and subcategories to which your app belongs in, as well as the age rating, any hardware preferences, and app declarations.

## Packages

Here you submit the `appxupload` package that was generated by `Visual Studio`.

Once you have followed all the steps in the submission process, you can click the `Submit to the Store` button. Your package will then undergo a workflow. Your app is signed with a certificate, and then the certification team performs certification tests. Once certified by the certification team, your app is published to the Windows Store.

# 18.4 在您的 UWP 应用中使用 Windows 广告中介

## 问题

你已经开发了你的 Windows 10 应用，并且你正在考虑通过运行广告来赚钱。你想与多个广告提供商签约，以便在你的应用中显示他们的广告。你的应用需要一个广告中介控件。

## 解决办法

应用内广告是你从应用中赚钱的方式之一。你可以订阅广告提供商，并在你的应用中运行他们的广告。你的应用中显示的广告的印象会得到报酬。不同的提供商有不同的与广告印象相关的经济学。但是要显示广告，你需要先安装 `Windows Ad Mediator`，这个控件可以帮助你显示来自多个提供商的广告。让我们学习如何从 `Visual Studio` 安装一个 `Ad Mediator` 控件。

1.  在 `Solution Explorer` 中打开你的项目。如果尚未展开，请展开你的项目。
2.  从菜单栏中，选择 `Tools➤ Extensions and Updates`（参见图 18-15）。

![A978-1-4842-0719-2_18_Fig15_HTML.jpg](img/A978-1-4842-0719-2_18_Fig15_HTML.jpg)

图 18-15。

`Extension and Updates` 对话框。
3.  从左侧树中选择 `Online`。在对话框搜索栏中输入 `Windows Ad Mediation`（参见图 18-16）。

![A978-1-4842-0719-2_18_Fig16_HTML.jpg](img/A978-1-4842-0719-2_18_Fig16_HTML.jpg)

图 18-16。

Windows ad mediation。
4.  单击搜索结果中显示的 `Windows ad mediation` 包上的 `Download`。这将启动浏览器并下载可执行文件。下载后，安装 `MSI` 并按照屏幕上的说明操作。安装完成后，重新启动 `Visual Studio`。

现在，您可以在应用中使用 `Windows Ad` 中介控件了。

# 18.5 在应用中显示广告

## 问题

您已经安装了 `Windows Ad Mediation SDK`，现在您希望开始在您的应用中显示广告。你想在你的应用页面上放广告。

## 解决办法

为了在应用页面中显示广告，您需要添加对 `Windows 广告 SDK` 的引用，该 SDK 是作为 `Windows 广告中介` 安装的一部分安装的。一旦将引用添加到 `Windows Ad SDK` 中，您就可以在任何页面上实例化中介控件，并在其中显示广告。以下步骤解释了如何添加中介控件。

1.  在 `Solution Explorer` 中打开你的项目。如果尚未展开，请展开你的项目。
2.  右键单击 `References` 节点，然后从上下文菜单中选择 `Add Reference`。
3.  在 `Reference Manager` 对话框中，选择 `Microsoft Advertising SDK for JavaScript` 并单击 `OK`（参见图 18-17）。

![A978-1-4842-0719-2_18_Fig17_HTML.jpg](img/A978-1-4842-0719-2_18_Fig17_HTML.jpg)

图 18-17。

`Reference Manager` 对话框。
4.  打开 `default.html` 或您希望放置广告的任何其他文件。
5.  在 `<head>` 部分，在项目的 `default.css` 和 `default.js` 的 JavaScript 引用之后，添加对 `ad.js` 的引用。

```html
<!-- Microsoft Advertising required references -->
<script src="/Microsoft.Advertising.JavaScript/ad.js" ></script>
```

6.  修改 `default.html` 文件（或其他适合您项目的 HTML 文件）中的 `<body>` 部分以包含以下内容：

```html
<div id="myAd" style="position: absolute; top: 50px; left: 0px; width: 300px; height: 250px; z-index: 1"
     data-win-control="MicrosoftNSJS.Advertising.AdControl"
     data-win-options="{applicationId: 'd25517cb-12d4-4699-8bdc-52040c712cab', adUnitId: '10043121'}">
</div>
```

7.  编译并运行应用以查看带有广告的效果（参见图 18-18）。

![A978-1-4842-0719-2_18_Fig18_HTML.jpg](img/A978-1-4842-0719-2_18_Fig18_HTML.jpg)

图 18-18。

带有广告的通用 Windows 应用。

**注意**

在前面的代码片段中提供的应用 id 和 ad 单元 id 是 Microsoft 在开发过程中为您提供的测试值。出于测试目的，应用 id 将是相同的，但是有不同的测试广告单元，可以用于测试不同的广告维度。位于 [`https://msdn.microsoft.com/en-US/library/mt313178(v=msads.30).aspx`](https://msdn.microsoft.com/en-US/library/mt313178(v=msads.30).aspx) 的微软文档页面提供了关于测试广告单元 id 的更多信息。

To generate ApplicationID and AdUnitId in your production apps, follow these steps: Start the store submission process in the Windows Dev Center. Make sure that you set the App Category in the App Properties section. Next, select Monetization ➤ Monetize with ads from the options in the section on the left of the page. On the Monetize with ads page, under the Microsoft Advertising ad units section, click Show Options. Enter a name for your ad unit. Select the Ad unit type and the device family where the ad will be shown. Click the Create ad unit button. Now you have created an ad unit. Copy the application id and ad unit id and paste into the ad control in your app. Regenerate the package and use the new package in your submission process.