# Pro Android 4（专业 Android 4 开发）

版权所有 © 2012，Satya Komatineni 与 Dave MacLean

本作品受版权保护。出版方保留所有权利，涉及材料的全部或部分内容，具体包括翻译、重印、插图复用、朗诵、广播、微缩胶片或其他任何物理形式的复制，以及信息存储与检索的传输、电子改编、计算机软件，或采用目前已知或未来开发的类似或不同方法。法律保留的例外情况包括：与评论或学术分析相关的简短摘录，或专门为输入计算机系统并执行而提供的材料，仅供购买者个人使用。未经出版方所在地现行版权法许可，不得复制本出版物或其任何部分，且必须始终从 Springer 获得使用许可。使用许可可通过 Copyright Clearance Center 的 RightsLink 获取。违反行为将根据相应版权法被起诉。

ISBN-13（平装版）：978-1-4302-3930-7

ISBN-13（电子版）：978-1-4302-3931-4

本书中可能出现商标名称、徽标和图像。我们不会在每个商标名称、徽标或图像处添加商标符号，而是仅以编辑方式使用这些名称、徽标和图像，以维护商标所有者的利益，无意侵犯商标权。

Android 机器人（01 / Android Robot）的图像源自 Google 创作并共享的作品，并根据 Creative Commons 3.0 署名许可条款使用。Android 以及所有基于 Android 和 Google 的标记均为 Google Inc. 在美国及其他国家的商标或注册商标。Apress Media, L.L.C. 与 Google Inc. 无关联关系，本书编写未经 Google Inc. 认可。

本出版物中使用的商品名称、商标、服务标志及类似术语，即使未明确标识，也不应被视为对其是否受专有权利保护的立场表达。

尽管本书中的建议和信息在出版时被认为是真实准确的，但作者、编辑及出版方均不对可能存在的任何错误或遗漏承担法律责任。出版方不对本书所含材料提供任何明示或暗示的保证。

总裁兼出版人：Paul Manning  
首席编辑：Matthew Moodie  
技术审校：Eric Franchomme、Dylan Phillips、Michael Nguyen、Karim Varela  
编辑委员会：Steve Anglin、Ewan Buckingham、Gary Cornell、Louise Corrigan、Morgan Ertel、Jonathan Gennick、Jonathan Hassell、Robert Hutchinson、Michelle Lowman、James Markham、Matthew Moodie、Jeff Olson、Jeffrey Pepper、Douglas Pundick、Ben Renow-Clarke、Dominic Shakeshaft、Gwenan Spearing、Matt Wade、Tom Welsh  
协调编辑：Corbin Collins  
文字编辑：Tiffany Taylor  
排版：MacPS, LLC  
索引编制：BIM Indexing & Proofreading Services  
艺术设计：SPi Global  
封面设计：Anna Ishchenko

全球图书贸易发行由 Springer Science+Business Media New York 负责，地址：233 Spring Street, 6th Floor, New York, NY 10013。电话：1-800-SPRINGER，传真：(201) 348-4505，电子邮件：`orders-ny@springer-sbm.com`，或访问：`www.springeronline.com`。

关于翻译事宜，请发送电子邮件至：`rights@apress.com`，或访问：`www.apress.com`。

Apress 及 friends of ED 系列图书可批量购买用于学术、企业或促销用途。大多数图书也提供电子版及许可。更多信息，请参考我们的特别批量销售–电子书授权网页：`www.apress.com/bulk-sales`。

作者在本文中引用的任何源代码或其他补充材料，读者可在 `www.apress.com` 获取。关于如何查找本书源代码的详细信息，请访问 `www.apress.com/source-code/`。

*献给我的父亲，是他的放手让我走上了自己的道路。 ——Satya Komatineni*

*献给我的妻子 Rosie，她激励我追寻梦想。献给我的儿子 Mike，他让生活每天充满趣味。也献给我的朋友们 Bigham 一家（Dale、Heather、Eric 和 Lizzie），他们非凡的慷慨助力让这一切成为可能。 ——Dave MacLean*



## 目录一览

目录

关于作者

关于技术审校

致谢

前言

![images](img/square.jpg)第 1 章：Android 计算平台简介

![images](img/square.jpg)第 2 章：搭建开发环境

![images](img/square.jpg)第 3 章：理解 Android 资源

![images](img/square.jpg)第 4 章：理解内容提供者

![images](img/square.jpg)第 5 章：理解 Intent

![images](img/square.jpg)第 6 章：构建用户界面与使用控件

![images](img/square.jpg)第 7 章：使用菜单

![images](img/square.jpg)第 8 章：为平板电脑及更多设备使用 Fragment

![images](img/square.jpg)第 9 章：使用对话框

![images](img/square.jpg)第 10 章：探索 ActionBar

![images](img/square.jpg)第 11 章：高级调试与分析

![images](img/square.jpg)第 12 章：响应配置变更

![images](img/square.jpg)第 13 章：使用偏好设置与保存状态

![images](img/square.jpg)第 14 章：探索安全与权限

![images](img/square.jpg)第 15 章：构建与使用服务

![images](img/square.jpg)第 16 章：探索包

![images](img/square.jpg)第 17 章：探索 Handler

![images](img/square.jpg)第 18 章：探索 AsyncTask

![images](img/square.jpg)第 19 章：广播接收器与长时间运行的服务

![images](img/square.jpg)第 20 章：探索闹钟管理器

![images](img/square.jpg)第 21 章：探索 2D 动画

![images](img/square.jpg)第 22 章：探索地图与基于位置的服务

![images](img/square.jpg)第 23 章：使用电话 API

![images](img/square.jpg)第 24 章：理解媒体框架

![images](img/square.jpg)第 25 章：主屏幕小部件

![images](img/square.jpg)第 26 章：探索列表小部件

![images](img/square.jpg)第 27 章：触摸屏

![images](img/square.jpg)第 28 章：实现拖放

![images](img/square.jpg)第 29 章：使用传感器

![images](img/square.jpg)第 30 章：探索联系人 API

![images](img/square.jpg)第 31 章：部署你的应用：Android Market 及其它

索引

## 目录

目录一览

关于作者

关于技术审校

致谢

前言

![images](img/square.jpg)第 1 章：Android 计算平台简介

新型个人电脑的新平台

Android 早期历史

深入 Dalvik 虚拟机

理解 Android 软件栈

使用 Android SDK 开发终端用户应用

Android 模拟器

Android UI

Android 基础组件

高级 UI 概念

Android 服务组件

Android 媒体与电话组件

Android Java 包

利用 Android 源代码

在线浏览 Android 源码

使用 Git 下载 Android 源代码

本书中的示例项目

总结

![images](img/square.jpg)第 2 章：搭建开发环境

搭建你的环境

下载 JDK6

下载 Eclipse 3.6

下载 Android SDK

工具窗口

安装 Android 开发工具 (ADT)

学习基础组件

视图

Activity

Fragment

Intent

内容提供者

服务

AndroidManifest.xml

Android 虚拟设备

Hello World

Android 虚拟设备

在真实设备上运行

探索 Android 应用的结构

检查应用生命周期

简单调试

启动模拟器

参考

总结

面试问题

![images](img/square.jpg)第 3 章：理解 Android 资源

理解资源

字符串资源

布局资源

资源引用语法

定义你自己的资源 ID 以备后用

编译与未编译的 Android 资源

列举关键的 Android 资源

使用任意 XML 资源文件

使用原始资源

使用资产文件

回顾资源目录结构

资源与配置变更

参考 URL

总结

面试问题

![images](img/square.jpg)第 4 章：理解内容提供者

探索 Android 内置的内容提供者

探索模拟器和可用设备上的数据库

快速 SQLite 入门

内容提供者的架构



Android 内容 URI 的结构

Android MIME 类型的结构

使用 URI 读取数据

使用 Android 游标

使用 `where` 子句

插入记录

向内容提供者添加文件

更新与删除

实现内容提供者

规划数据库

扩展 `ContentProvider`

履行 MIME 类型契约

实现 `query` 方法

实现 `insert` 方法

实现 `update` 方法

实现 `delete` 方法

使用 `UriMatcher` 解析 URI

使用投影映射

注册提供者

实践图书提供者

添加图书

删除图书

统计图书数量

显示图书列表

资源

总结

面试问题

![images](img/square.jpg)第 5 章：理解 Intent

Android Intent 基础

Android 中可用的 Intent

探索 Intent 的组成

Intent 与数据 URI

通用动作

使用额外信息

使用组件直接调用 Activity

理解 Intent 类别

将 Intent 解析为其组件的规则

实践 `ACTION_PICK`

实践 `GET_CONTENT` 动作

引入待定 Intent

资源

总结

面试问题

![images](img/square.jpg)第 6 章：构建用户界面与使用控件

Android 中的 UI 开发

完全通过代码构建 UI

完全通过 XML 构建 UI

通过 XML 与代码结合构建 UI

理解 Android 的常用控件

文本控件

按钮控件

`ImageView` 控件

日期和时间控件

`MapView` 控件

理解适配器

认识 `SimpleCursorAdapter`

认识 `ArrayAdapter`

在 `AdapterView` 中使用适配器

基本列表控件：`ListView`

`GridView` 控件

`Spinner` 控件

`Gallery` 控件

创建自定义适配器

Android 中的其他控件

样式与主题

使用样式

使用主题

理解布局管理器

`LinearLayout` 布局管理器

`TableLayout` 布局管理器

`RelativeLayout` 布局管理器

`FrameLayout` 布局管理器



网格布局管理器

自定义适用于各种设备配置的布局

参考资料

总结

面试题

![images](img/square.jpg)第 7 章：使用菜单

理解 Android 菜单

创建菜单

使用菜单组

响应菜单项

使用其他菜单类型

扩展菜单

使用图标菜单

使用子菜单

使用上下文菜单

使用替代菜单

动态菜单

通过 XML 文件加载菜单

XML 菜单资源文件的结构

填充 XML 菜单资源文件

响应基于 XML 的菜单项

4.0 中的弹出菜单

其他 XML 菜单标签简介

资源文件

总结

面试题

![images](img/square.jpg)第 8 章：面向平板电脑及其他设备的 Fragment

什么是 Fragment？

何时使用 Fragment

Fragment 的结构

Fragment 的生命周期

展示生命周期的 Fragment 示例应用

FragmentTransactions 与 Fragment 返回栈

Fragment 事务转换与动画

FragmentManager

引用 Fragment 时的注意事项

保存 Fragment 状态

ListFragments 与 <fragment>

在需要时调用单独的 Activity

Fragment 的持久化

与 Fragment 通信

使用 startActivity() 和 setTargetFragment()

使用 ObjectAnimator 自定义动画

参考资料

总结

面试题

![images](img/square.jpg)第 9 章：使用对话框

在 Android 中使用对话框

理解对话框 Fragment

DialogFragment 基础

DialogFragment 示例应用

使用 Toast

旧版 Android 的对话框 Fragment

参考资料

总结

面试题

![images](img/square.jpg)第 10 章：探索 ActionBar

ActionBar 剖析

带标签导航的 Action Bar Activity

实现基础 Activity 类

为 Action Bar 分配统一行为

实现标签监听器

实现带标签的 Action Bar Activity

可滚动的调试文本视图布局

Action Bar 与菜单交互

Android 清单文件

检查带标签的 Action Bar Activity

列表导航 Action Bar Activity




## 第 11 章：高级调试与分析

### 启用高级调试
## 调试透视视图
## DDMS 透视视图
## 层级查看透视视图
### 像素完美视图
### Traceview
### adb 命令
### 模拟器控制台
### StrictMode
### StrictMode 策略
### 关闭 StrictMode
### 旧版 Android 中的 StrictMode
### StrictMode 练习
### 参考资料
### 总结
### 面试问题

## 第 12 章：响应配置变更

### 配置变更过程
### Activity 的销毁/创建循环
### Fragment 的销毁/创建循环
### 使用 FragmentManager 保存 Fragment 状态
### 在 Fragment 上使用 setRetainInstance
### 已弃用的配置变更方法
### 参考资料
### 总结
### 面试问题

## 第 13 章：使用首选项与保存状态

### 探索首选项框架
### 理解 ListPreference
### 理解 CheckBoxPreference
### 理解 EditTextPreference
### 理解 RingtonePreference 和 MultiSelectListPreference
### 组织首选项
### 使用 PreferenceCategory
### 创建带依赖关系的子首选项
### 使用标头的首选项
### 以编程方式操作首选项
### 使用首选项保存状态
### 使用 DialogPreference
### 参考资料
### 总结
### 面试问题

## 第 14 章：探索安全与权限

#### 创建 Spinner 适配器
#### 创建列表监听器
#### 设置列表操作栏
## 修改 BaseActionBarActivity
## 修改 AndroidManifest.xml
#### 检查列表操作栏 Activity
### 标准导航操作栏 Activity
#### 设置标准导航操作栏 Activity
## 修改 BaseActionBarActivity
## 修改 AndroidManifest.xml
#### 检查标准操作栏 Activity
### 操作栏与搜索视图
#### 将搜索视图小部件定义为菜单项
## 投射搜索结果 Activity
#### 通过可搜索的 XML 文件自定义搜索
#### 在清单文件中定义搜索结果 Activity
#### 为搜索视图小部件指定搜索目标
### 操作栏与 Fragment
### 参考资料
### 总结
### 面试问题



理解 Android 安全模型

安全概念概述

签署应用以进行部署

执行运行时安全检查

理解进程边界上的安全性

声明和使用权限

理解并使用自定义权限

理解并使用 URI 权限

参考资料

总结

面试问题

![images](img/square.jpg)第 15 章：构建与使用服务

使用 HTTP 服务

使用 HttpClient 发起 HTTP GET 请求

使用 HttpClient 发起 HTTP POST 请求（一个多部分示例）

SOAP、JSON 与 XML 解析器

处理异常

解决多线程问题

超时趣谈

使用 HttpURLConnection

使用 AndroidHttpClient

使用后台线程（AsyncTask）

使用 DownloadManager 获取文件

使用 Android 服务

理解 Android 中的服务

理解本地服务

理解 AIDL 服务

在 AIDL 中定义服务接口

实现 AIDL 接口

从客户端应用调用服务

向服务传递复杂类型

参考资料

总结

面试问题

![images](img/square.jpg)第 16 章：探索包

包与进程

包规范的详细信息

将包名转换为进程名

列出已安装的包

通过包浏览器删除包

重新审视包签名过程

理解数字签名：场景 1

理解数字签名：场景 2

理解数字签名的一种模式

那么如何进行数字签名？

签名过程的影响

在包之间共享数据

共享用户 ID 的本质

共享数据的代码模式

库项目

什么是库项目？

库项目的断言

创建库项目

创建使用库的 Android 项目

使用库项目的注意事项

参考资料

总结

面试问题

![images](img/square.jpg)第 17 章：探索处理器

Android 组件与线程化

Activity 在主线程上运行

广播接收器在主线程上运行

服务在主线程上运行

内容提供者在主线程上运行



主线程单一线程的含义

线程池、内容提供者及外部服务组件

线程实用工具：发现你的线程

处理器

占用主线程的含义

使用处理器延迟主线程的工作

用于延迟工作的处理器源代码示例

构建合适的消息对象

将消息对象发送到队列

响应 `handleMessage` 回调

使用工作线程

从菜单中调用工作线程

工作线程与主线程之间的通信

组件与进程的生命周期

Activity 生命周期

Service 生命周期

Receiver 生命周期

Provider 生命周期

参考资料

总结

面试题

![images](img/square.jpg)第 18 章：探索 AsyncTask

实现一个简单的 AsyncTask

理解 AsyncTask 中的泛型

子类化异步任务

实现你的第一个异步任务

调用异步任务

`onPreExecute()` 回调与进度对话框

`doInBackground()` 方法

触发 `onProgressUpdate()`

`onPostExecute()` 方法

升级到确定性进度对话框

异步任务的本质

设备旋转与 AsyncTask

生命周期方法与 AsyncTask

参考资料

总结

面试题

![images](img/square.jpg)第 19 章：广播接收器与长时间运行的服务

广播接收器

发送广播

编写一个简单的接收器：示例代码

在清单文件中注册接收器

容纳多个接收器

跨进程接收器

从接收器中使用通知

通过通知管理器监控通知

发送通知

在广播接收器中启动 Activity

长时间运行的接收器与服务

长时间运行的广播接收器协议

IntentService

IntentService 源代码

为广播接收器扩展 IntentService

长时间运行的广播服务抽象

一个长时间运行的接收器

使用 `LightedGreenRoom` 抽象唤醒锁

长时间运行的服务实现

非粘性服务的细节

粘性服务的细节

非粘性服务的一种变体：重新传递 Intent

在 `OnStartCommand` 中指定服务标志

选择合适的粘性策略

从两个位置控制唤醒锁



长时间运行服务的实现

测试长时间运行的服务

你的职责

框架的职责

关于项目下载文件的几点说明

参考资料

章节摘要

面试题

![images](img/square.jpg)第 20 章：探索闹钟管理器

闹钟管理器基础：设置一个简单的闹钟

获取闹钟管理器访问权限

设置闹钟时间

为闹钟创建接收器

创建适用于闹钟的 PendingIntent

设置闹钟

测试项目

探索闹钟管理器备选方案

重复触发闹钟

取消闹钟

处理多个闹钟

触发闹钟时的意图优先性

闹钟的持久性

闹钟管理器谓词

参考资料

章节摘要

面试题

![images](img/square.jpg)第 21 章：探索 2D 动画

逐帧动画

规划逐帧动画

创建活动

为活动添加动画

布局动画

基础补间动画类型

规划布局动画测试工具

创建活动与 ListView

为 ListView 添加动画

使用插值器

视图动画

理解视图动画

添加动画

在 2D 中使用 Camera 提供深度感知

探索 AnimationListener 类

关于变换矩阵的说明

属性动画：新的动画 API

属性动画

规划属性动画测试平台

使用对象动画器实现基础视图动画

使用 AnimatorSet 实现顺序动画

使用 AnimationSetBuilder 设置动画关系

使用 XML 加载动画器

使用 PropertyValuesHolder

视图属性动画

类型估值器

关键帧

布局过渡

资源

章节摘要

面试题

![images](img/square.jpg)第 22 章：探索地图与基于位置的服务

理解地图包

从 Google 获取地图 API 密钥

理解 MapView 与 MapActivity

使用叠加层添加标记

理解位置包

在 Android 中使用地理编码

在后台线程中进行地理编码

理解 LocationManager 服务



使用 MyLocationOverlay 显示您的位置

使用临近警报

参考资料

总结

面试题

![images](img/square.jpg) 第 23 章：使用电话 API

使用 SMS

发送 SMS 消息

监控传入的 SMS 消息

使用 SMS 文件夹

发送电子邮件

使用电话管理器

会话发起协议 (SIP)

试验 SipDemo

android.net.sip 包

参考资料

总结

面试题

![images](img/square.jpg) 第 24 章：理解媒体框架

使用媒体 API

使用 SD 卡

播放媒体

播放音频内容

播放视频内容

录制媒体

探索使用 MediaRecorder 进行音频录制

使用 AudioRecord 录制音频

探索视频录制

探索 MediaStore 类

使用 Intent 录制音频

将媒体内容添加到媒体存储

触发整个 SD 卡的媒体扫描

参考资料

总结

面试题

![images](img/square.jpg) 第 25 章：主屏幕小部件

主屏幕小部件的架构

什么是主屏幕小部件？

主屏幕小部件的用户体验

小部件的生命周期

一个示例小部件应用

定义小部件提供者

定义小部件尺寸

小部件布局相关文件

实现小部件提供者

实现小部件模型

实现小部件配置活动

小部件预览工具

小部件的限制与扩展

基于集合的小部件

资源

总结

面试题

![images](img/square.jpg) 第 26 章：探索列表小部件

关于远程视图的快速说明

在远程视图中使用列表

准备远程布局

加载远程布局

设置 RemoteViewsService

设置 RemoteViewsFactory

设置 onClick 事件

响应 onClick 事件

工作示例：测试主屏幕列表小部件

创建测试小部件提供者

创建远程视图工厂

编写远程视图服务代码

主小部件布局文件

小部件提供者元数据

AndroidManifest.xml

测试列表小部件



