# CSS 预处理器入门

## 使用 Sass、Compass 和 Less

![image](img/FM-00.jpg)

安尼鲁德·普拉布

![image](img/FM-01.jpg)

**CSS 预处理器入门：使用 SASS、Compass.js 和 Less.js**

版权所有 © 2015，安尼鲁德·普拉布

本作品受版权保护。出版商保留所有权利，无论是全部还是部分材料，特别是翻译、重印、重用插图、朗诵、广播、以缩微胶片或任何其他物理方式复制，以及传输或信息存储和检索、电子改编、计算机软件，或现在已知或未来开发的任何类似或不同方法的权利。此法律保留的例外情况是：与评论或学术分析相关的简短摘录，或专门为输入和执行于计算机系统而提供的材料，仅供作品购买者独占使用。仅在出版商所在地现行版权法的条款下，才允许复制本出版物或其部分内容，并且必须始终从 Springer 获得使用许可。使用许可可通过 Copyright Clearance Center 的 RightsLink 获取。违反者将根据相应的版权法受到起诉。

ISBN-13 (平装): 978-1-4842-1348-3

ISBN-13 (电子版): 978-1-4842-1347-6

本书中可能出现商标名称、徽标和图像。对于每次出现的商标名称、徽标或图像，我们不会使用商标符号，而是仅以编辑方式使用这些名称、徽标和图像，以利于商标所有者，无意侵犯商标权。

本出版物中对商品名称、商标、服务标志和类似术语的使用，即使未明确标明，也不应被视为对其是否受所有权保护的观点的表达。

尽管本书中的建议和信息在出版时被认为是真实准确的，但作者、编辑和出版商均不对可能出现的任何错误或遗漏承担法律责任。出版商对本书所含内容不作任何明示或暗示的担保。

常务董事：韦尔莫德·施帕尔

采编编辑：塞莱斯廷·苏雷什·约翰

开发编辑：马修·穆迪

技术审校：洛凯什·艾耶

编辑委员会：史蒂夫·安格林、普拉米拉·巴兰、路易丝·科里根、詹姆斯·德沃尔夫、乔纳森·根尼克、罗伯特·哈钦森、塞莱斯廷·苏雷什·约翰、米歇尔·洛曼、詹姆斯·马卡姆、苏珊·麦克德莫特、马修·穆迪、杰弗里·佩珀、道格拉斯·庞迪克、本·雷诺-克拉克、格温南·斯皮林

协调编辑：丽塔·费尔南多

文字编辑：凯齐娅·恩兹利

排版：SPi Global

索引编制：SPi Global

全球图书贸易由 Springer Science+Business Media New York, 233 Spring Street, 6th Floor, New York, NY 10013 负责发行。电话：1-800-SPRINGER，传真：(201) 348-4505，电子邮件：orders-ny@springer-sbm.com，或访问 [www.springer.com](http://www.springer.com)。Apress Media, LLC 是加利福尼亚州的有限责任公司，其唯一成员（所有者）是 Springer Science + Business Media Finance Inc (SSBM Finance Inc)。SSBM Finance Inc 是特拉华州的一家公司。

有关翻译信息，请发送电子邮件至 rights@apress.com，或访问 [www.apress.com](http://www.apress.com)。

Apress 及 friends of ED 的书籍可批量购买用于学术、企业或促销目的。大多数图书也提供电子书版本和许可证。如需更多信息，请参阅我们的特殊批量销售 – 电子书许可网页：[www.apress.com/bulk-sales](http://www.apress.com/bulk-sales)。

作者在本文中引用的任何源代码或其他补充材料，读者可在 [www.apress.com/](http://www.apress.com/) 获取。有关如何找到本书源代码的详细信息，请访问 [www.apress.com/source-code/](http://www.apress.com/source-code/)。

## 内容概要

关于作者

关于技术审校

致谢

引言

![image](img/sq1.jpg) 第 1 章：预处理器简介

![image](img/sq1.jpg) 第 2 章：Sass 入门

![image](img/sq1.jpg) 第 3 章：高级 Sass

![image](img/sq1.jpg) 第 4 章：使用 Less 进行开发

![image](img/sq1.jpg) 第 5 章：Compass 入门

索引

## 目录

关于作者

关于技术审校

致谢

引言

![image](img/sq1.jpg) 第 1 章：预处理器简介

什么是预处理器？

为什么要使用预处理器？

关于 CSS 预处理器的误解

你需要成为命令行专家

你需要改变编写 CSS 的方式

为什么不直接写 CSS？

已知的预处理器有哪些？

Sass

Less

Compass

预处理器的特性

变量

嵌套

导入

Mixins

扩展/继承

运算符

CSS 预处理器的问题

Mixins 的问题

扩展选择器或使用 Mixins 可能妨碍维护

小结

![image](img/sq1.jpg) 第 2 章：Sass 入门

安装 Sass

检查 Sass 的其他版本

命令行的替代方案

编译 sass 文件

使用应用程序避免命令行：使用 Scout

自动化工作流程

变量

数据类型

变量的默认值

Sass 中的样式嵌套

引用父选择器：&

在 SCSS 中使用组合器

注释

属性嵌套

插值

占位符选择器

Sass 的逻辑能力

Sass 中的数学运算

括号

使用变量进行计算

控制指令

小结

# 第三章：高级 Sass

- `@import`
- `Partials`（局部文件）
- `Nested @import`（嵌套导入）
- `Plain CSS Imports`（纯 CSS 导入）
- `@media`
- `@extend`
- `@extend` 幕后原理
- 何时使用选择器继承
- 占位符选择器
- `@extend` 的更多用法
- `@at-root`
- Mixin 指令
- Mixins 与类
- 使用 CSS 选择器的 Mixins
- 向 Mixins 传递参数
- 在 Mixins 中使用内容块
- 函数指令
- 数值函数
- 颜色函数
- 列表函数
- 用户自定义函数
- 输出样式
  - `nested`（嵌套）
  - `expanded`（展开）
  - `compact`（紧凑）
  - `compressed`（压缩）
- 构建你自己的网格系统
- 本章小结
