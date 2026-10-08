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

献给我的父亲，是他的放手让我走上了自己的道路。 ——Satya Komatineni

献给我的妻子 Rosie，她激励我追寻梦想。献给我的儿子 Mike，他让生活每天充满趣味。也献给我的朋友们 Bigham 一家（Dale、Heather、Eric 和 Lizzie），他们非凡的慷慨助力让这一切成为可能。 ——Dave MacLean

## 目录一览

- 目录
- 关于作者
- 关于技术审校
- 致谢
- 前言
- 第 1 章：Android 计算平台简介
- 第 2 章：搭建开发环境
- 第 3 章：理解 Android 资源
- 第 4 章：理解内容提供者
- 第 5 章：理解 Intent
- 第 6 章：构建用户界面与使用控件
- 第 7 章：使用菜单
- 第 8 章：为平板电脑及更多设备使用 Fragment
- 第 9 章：使用对话框
- 第 10 章：探索 ActionBar
- 第 11 章：高级调试与分析
- 第 12 章：响应配置变更
- 第 13 章：使用偏好设置与保存状态
- 第 14 章：探索安全与权限
- 第 15 章：构建与使用服务
- 第 16 章：探索包
- 第 17 章：探索 Handler
- 第 18 章：探索 AsyncTask
- 第 19 章：广播接收器与长时间运行的服务
- 第 20 章：探索闹钟管理器
- 第 21 章：探索 2D 动画
- 第 22 章：探索地图与基于位置的服务
- 第 23 章：使用电话 API
- 第 24 章：理解媒体框架
- 第 25 章：主屏幕小部件
- 第 26 章：探索列表小部件
- 第 27 章：触摸屏
- 第 28 章：实现拖放
- 第 29 章：使用传感器
- 第 30 章：探索联系人 API
- 第 31 章：部署你的应用：Android Market 及其它
- 索引

## 目录

- 目录一览
- 关于作者
- 关于技术审校
- 致谢
- 前言
- 第 1 章：Android 计算平台简介
  - 新型个人电脑的新平台
  - Android 早期历史
  - 深入 Dalvik 虚拟机
  - 理解 Android 软件栈
  - 使用 Android SDK 开发终端用户应用
  - Android 模拟器
  - Android UI
  - Android 基础组件
  - 高级 UI 概念
  - Android 服务组件
  - Android 媒体与电话组件
  - Android Java 包
  - 利用 Android 源代码
  - 在线浏览 Android 源码
  - 使用 Git 下载 Android 源代码
  - 本书中的示例项目
  - 总结
- 第 2 章：搭建开发环境
  - 搭建你的环境
  - 下载 JDK6
  - 下载 Eclipse 3.6
  - 下载 Android SDK
  - 工具窗口
  - 安装 Android 开发工具 (ADT)
  - 学习基础组件
  - 视图
  - Activity
  - Fragment
  - Intent
  - 内容提供者
  - 服务
  - AndroidManifest.xml
  - Android 虚拟设备
