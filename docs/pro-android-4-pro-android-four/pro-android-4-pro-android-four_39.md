# 第 4 章 理解内容提供者

Android 使用一种称为内容提供者的概念来将数据抽象为服务。内容提供者的这个想法使得数据源看起来像是支持 REST 的数据提供者，例如网站。从这个意义上说，内容提供者是数据的封装器。Android 设备上的 SQLite 数据库就是一个可以封装到内容提供者中的数据源示例。

**注意：** REST 代表表述性状态转移。当你在 Web 浏览器中输入一个 URL，Web 服务器以 HTML 作为响应时，你实际上已经在 Web 服务器上执行了一个基于 REST 的“查询”操作。REST 通常也与（SOAP——简单对象访问协议）Web 服务进行对比。你可以在以下维基百科条目中阅读更多关于 REST 的内容：[`http://en.wikipedia.org/wiki/Representational_State_Transfer`](http://en.wikipedia.org/wiki/Representational_State_Transfer)。

要从内容提供者检索数据或向内容提供者保存数据，你需要使用一组类似 REST 的 URI。例如，如果你要从一个封装了图书数据库的内容提供者中检索一组图书，你需要使用如下 URI：

`content://com.android.book.BookProvider/books`

要从该图书数据库中检索一本特定的书（第 23 本），你需要使用如下 URI：

`content://com.android.book.BookProvider/books/23`

你将在本章中看到这些 URI 如何对应到底层的数据库访问机制。设备上的任何应用程序都可以利用这些 URI 来访问和操作数据。因此，内容提供者在应用程序之间共享数据方面扮演着重要角色。

严格来说，内容提供者的职责更多地在于封装机制而非数据访问机制。你需要一个实际的数据访问机制，例如 SQLite 或网络访问，才能访问到底层的数据源。因此，仅当你希望对外共享数据或在应用程序之间共享数据时，才需要内容提供者这一抽象层。对于内部数据访问，应用程序可以使用任何它认为合适的数据存储/访问机制，例如：

-   `SharedPreferences`：一组你可以持久化以存储应用程序偏好的键/值对。

-   文件：应用程序内部的文件，你可以将其存储在可移除的存储介质上。

-   `SQLite`：SQLite 数据库，每个数据库对于创建它的包来说是私有的。

-   网络：一种允许你通过 HTTP 服务从外部检索或存储数据的机制。

**注意：** 尽管 Android 允许使用多种数据访问机制，但本章重点介绍 SQLite 和内容提供者抽象，因为内容提供者是数据共享的基础，与其他 UI 框架相比，这在 Android 框架中要普遍得多。我们将在第 11 章中介绍网络方法，并在第 9 章中介绍偏好设置机制。

### 探索 Android 内置的内容提供者

Android 带有许多内置的内容提供者，这些提供者在 SDK 的 `android.provider` Java 包中有文档说明。你可以在此处查看这些提供者的列表：

`http://developer.android.com/reference/android/provider/package-summary.html`

这些提供者包括，例如，联系人（Contacts）和媒体存储（Media Store）。这些 SQLite 数据库通常具有 `.db` 扩展名，并且只能从实现包内部访问。任何在该包之外的访问都必须通过内容提供者接口进行。

#### 在模拟器和可用设备上探索数据库

由于 Android 中的许多内容提供程序都使用 SQLite 数据库（`www.sqlite.org/`），您可以使用 Android 和 SQLite 提供的工具来检查这些数据库。其中许多工具位于 `\android-sdk-install-directory\tools` 子目录中；其他工具位于 `\android-sdk-install-directory\platform-tools` 中。

**注意：** 请参考第 2 章，了解如何在不同操作系统中定位 `tools` 目录和调用命令窗口。与其余大部分章节一样，本章主要在 Windows 平台上给出示例。在本节中，我们将使用多个命令行工具，您可以重点关注可执行文件或批处理文件的名称，而无需过多关注工具所在的目录。我们已在第 2 章 中介绍了如何在各种平台上设置工具目录的路径。

Android 使用一个名为 Android 调试桥（`adb`）的命令行工具，其位置如下：

`platform-tools\adb.exe`

`adb` 是 Android 工具包中的一种特殊工具，大多数其他工具通过它来访问设备。但是，您必须让模拟器正在运行或有 Android 设备已连接，`adb` 才能正常工作。您可以在命令行中输入以下命令来查看是否有正在运行的设备或模拟器：

`adb devices`

如果模拟器没有运行，您可以在命令行中输入以下命令来启动它：

`emulator.exe @avdname`

参数 `@avdname` 是 Android 虚拟设备（AVD）的名称。（第 2 章介绍了 Android 虚拟设备的必要性以及如何在第 2 章 中创建它们。）要查看您已有的虚拟设备，可以运行以下命令：

`android list avd`

此命令将列出所有可用的 AVD。如果您已经通过 Eclipse Android 开发工具（ADT）开发并运行过任何 Android 应用程序，那么您已经至少配置了一个虚拟设备。上述命令至少会列出这一个。

以下是 `list` 命令的一些输出示例。（根据您的 `tools` 目录位置以及 Android 版本，以下输出内容的路径或版本号可能会有所不同，例如 `i:\android`。）

```
I:\android\tools>android list avd
Available Android Virtual Devices:
    Name: avd
    Path: I:\android\tools\..\avds\avd3
  Target: Google APIs (Google Inc.)
          Based on Android 1.5 (API level 3)
    Skin: HVGA
  Sdcard: 32M
---------
    Name: titanium
    Path: C:\Documents and Settings\Satya\.android\avd\titanium.avd
  Target: Android 1.5 (API level 3)
    Skin: HVGA
```

如上所述，AVD 的详细信息在第 2 章有介绍。

您也可以通过 Eclipse ADT 插件启动模拟器。当您选择在模拟器中运行或调试某个程序时，这会自动发生。模拟器启动并运行后，您可以通过输入以下命令再次测试以获取正在运行的设备列表：

`adb devices`

现在您应该会看到类似如下的输出：

```
List of devices attached
emulator-5554 device
```

您可以在命令行中输入以下命令来查看可以通过 `adb` 运行的众多选项和命令：

`adb help`

# 使用 adb 和 SQLite 探索 Android 数据库

您也可以访问以下网址了解 `adb` 的许多运行时选项：[`http://developer.android.com/guide/developing/tools/adb.html`](http://developer.android.com/guide/developing/tools/adb.html)。

您可以使用 `adb` 在连接的设备上打开一个 shell，方法是输入：

`adb shell`

**注意：** 此 shell 是一个 Unix `ash`，但命令集有限。例如，您可以执行 `ls`，但 `find`、`grep` 和 `awk` 在该 shell 中不可用。

您可以在 shell 提示符下输入以下内容来查看 shell 中可用的命令集：

`#ls /system/bin`

`#` 符号是 shell 的提示符。为简洁起见，在以下示例中我们将省略此提示符。要查看根级目录和文件的列表，您可以在 shell 中输入：

`ls -l`

您需要访问此目录才能查看数据库列表：

`ls /data/data`

此目录包含设备上已安装的软件包列表。让我们通过探索 `com.android.providers.contacts` 软件包来查看一个示例：

`ls /data/data/com.android.providers.contacts/databases`

这将列出一个名为 `contacts.db` 的数据库文件，这是一个 SQLite 数据库。（该文件和路径仍然取决于设备和版本。）

**注意：** 在 Android 中，数据库可能是在首次访问时创建的。这意味着如果您从未访问过联系人应用程序，您可能看不到此文件。

如果包含的 `ash` 中有 `find` 命令，您可以查找所有 `*.db` 文件。但仅靠 `ls` 无法很好地做到这一点。您能做到的最接近的操作是：

`ls -R /data/data/*/databases`

使用此命令，您会注意到 Android 发行版中有以下数据库（再次提醒，请谨慎：根据您的版本，此列表可能有所不同）：

```
alarms.db
contacts.db
downloads.db
internal.db
settings.db
mmssms.db
telephony.db
```

您可以在 `adb` shell 中通过对其中一个数据库调用 `sqlite3` 来操作它，请输入：

`sqlite3 /data/data/com.android.providers.contacts/databases/contacts.db`

您可以通过输入以下内容退出 `sqlite3`：

`sqlite>.exit`

请注意，`adb` 的提示符是 `#`，而 `sqlite3` 的提示符是 `sqlite>`。您可以通过访问 `www.sqlite.org/sqlite.html` 来了解各种 `sqlite3` 命令。不过，我们将在下面列出一些重要的命令，这样您就不必去访问网页了。您可以通过输入以下命令来查看表列表：

`sqlite> .tables`

此命令是以下内容的快捷方式：

```
SELECT name FROM sqlite_master
WHERE type IN ('table','view') AND name NOT LIKE 'sqlite_%'
UNION ALL
SELECT name FROM sqlite_temp_master
WHERE type IN ('table','view')
ORDER BY 1
```

您可能已经猜到，`sqlite_master` 表是一个主表，用于跟踪数据库中的表和视图。以下命令行会打印出 `contacts.db` 中名为 `people` 的表的 `create` 语句：

`.schema people`

这是在 SQLite 中获取表列名的一种方法。它还会打印出列数据类型。在使用内容提供程序时，您应留意这些列类型，因为访问方法依赖于它们。

然而，仅仅为了了解列名及其类型而手动解析这条长长的 `create` 语句是非常繁琐的。有一种变通方法：您可以将 `contacts.db` 拉取到本地计算机上，然后使用任何适用于 SQLite 版本 3 的 GUI 工具来检查该数据库。您可以从操作系统命令提示符中发出以下命令来拉取 `contacts.db` 文件：

`adb pull /data/data/com.android.providers.contacts/databases/contacts.db` ![Image](img/U002.jpg)

`c:/somelocaldir/contacts.db`

我们使用了免费下载的 Sqliteman（[`http://sqliteman.com/`](http://sqliteman.com/)），这是一个适用于 SQLite 数据库的 GUI 工具，运行起来似乎没有问题。我们遇到了几次崩溃，但除此之外，发现该工具完全可以用于探索 Android SQLite 数据库。

## SQLite 快速入门

以下示例 SQL 语句可帮助你快速浏览 SQLite 数据库：

```
//设置工具中显示的列标题
sqlite>.headers on

//从表中选择所有行
select * from table1;

//统计表中的行数
select count(*) from table1;

//选择特定的列集
select col1, col2 from table1;

//选择列中的不重复值
select distinct col1 from table1;

//统计不重复值的数量
select count(col1) from (select distinct col1  from table1);

//分组
select count(*), col1 from table1 group by col1;

//常规内连接
select * from table1 t1, table2 t2
where t1.col1 = t2.col1;

//左外连接
//返回 t1 中的所有数据，即使 t2 中没有匹配行
select * from table t1 left outer join table2 t2
on t1.col1 = t2.col1
where ....
```

## 内容提供者的架构

现在你已经知道如何通过 Android 和 SQLite 工具探索现有的内容提供者。接下来，我们将考察内容提供者的一些架构元素，以及这些内容提供者如何与行业中其他数据访问抽象相关联。

总体而言，内容提供者方法与以下行业抽象有相似之处：

- 网站

- REST

- Web 服务

- 存储过程

设备上的每个内容提供者都像网站一样，通过一个字符串（类似于域名，但称为*授权标识*）向系统注册自己。这个唯一标识的字符串构成了该内容提供者所能提供的一组 URI 的基础。这无异于一个拥有域名的网站通过许多 URL 来公开其文档或内容。

这个授权标识的注册发生在 `AndroidManifest.xml` 文件中。以下是在 `AndroidManifest.xml` 中注册提供者的两个示例：

```
<provider android:name="SomeProvider"
        android:authorities="com.your-company.SomeProvider" />

<provider android:name="NotePadProvider"
   android:authorities="com.google.provider.NotePad"
/>
```

授权标识对于内容提供者来说就像域名。鉴于上述授权标识的注册，这些提供者将受理以该授权标识前缀开头的 URL：

```
content://com.your-company.SomeProvider/
content://com.google.provider.NotePad/
```

你会看到，内容提供者就像一个网站，拥有一个作为起始 URL 的基础域名。

**注意：** Android 提供的提供者可能不包含完整的授权标识名称。目前建议仅对第三方内容提供者使用完整名称。这就是为什么你有时会看到内容提供者使用像 `contacts` 这样的简单单词来引用，而不是 `com.google.android.contacts`（对于第三方提供者而言）。

内容提供者也提供类似 REST 的 URL 来检索或操作数据。对于上述注册，在 `NotePadProvider` 数据库中，用于标识笔记目录或集合的 URI 是：

```
content://com.google.provider.NotePad/Notes
```

用于标识特定笔记的 URI 是：

```
content://com.google.provider.NotePad/Notes/#
```

其中 `#` 是特定笔记的 `id`。以下是一些数据提供者可能接受的 URI 示例：

```
content://media/internal/images
content://media/external/images
content://contacts/people/
content://contacts/people/23
```

请注意，这些提供者的媒体（`content://media`）和联系人（`content://contacts`）没有完整的结构。这是因为它们不是第三方提供者，而是由 Android 控制的。

内容提供者也展现出 Web 服务的特性。内容提供者通过其 URI，将内部数据作为服务公开。然而，内容提供者 URL 的输出并非像基于 SOAP 的 Web 服务调用那样是类型化的数据。这种输出更像是来自 JDBC 语句的结果集。即使如此，与 JDBC 的相似之处也只是概念上的。我们不想给人留下这与 JDBC 的 `ResultSet` 相同的印象。

调用者需要了解返回的行和列的结构。此外，正如你将在本章“Android MIME 类型的结构”部分所见，内容提供者有一个内置机制，允许你确定此 URI 所代表数据的多用途互联网邮件扩展（MIME）类型。

除了类似于网站、REST 和 Web 服务之外，内容提供者的 URI 还类似于数据库中存储过程的名称。存储过程提供基于服务的底层关系数据访问方式。URI 与存储过程相似，因为对内容提供者进行的 URI 调用会返回一个游标。然而，内容提供者与存储过程的不同之处在于，内容提供者中服务调用的输入通常嵌入在 URI 本身中。

我们提供这些比较是为了让你了解内容提供者更广泛的应用范围。

### Android 内容 URI 的结构

我们将内容提供者比作网站，因为它能响应传入的 URI。因此，要从内容提供者检索数据，你只需调用一个 URI。然而，在内容提供者的情况下，检索到的数据以行和列集的形式呈现，由一个 Android `cursor` 对象表示。在此背景下，我们将考察可用于检索数据的 URI 结构。

Android 中的内容 URI 看起来与 HTTP URI 类似，区别在于它们以 `content` 开头，并且具有通用形式：

```
content://*/*/*
```

或

```
content://authority-name/path-segment1/path-segment2/etc…
```

以下是一个示例 URI，用于标识笔记数据库中编号为 23 的笔记：

```
content://com.google.provider.NotePad/notes/23
```

在 `content:` 之后，URI 包含授权标识的唯一标识符，用于在提供者注册表中定位该提供者。在上述示例中，`com.google.provider.NotePad` 是 URI 的授权标识部分。

`/notes/23` 是 URI 的路径部分，每个提供者都各不相同。路径部分中的 `notes` 和 `23` 被称为*路径段*。提供者有责任记录并解释 URI 的路径部分和路径段。

内容提供者的开发者通常通过在提供者实现 Java 包的 Java 类或 Java 接口中声明常量来完成这一点。此外，路径的第一部分可能指向一个对象集合。例如，`/notes` 表示笔记的集合或目录，而 `/23` 指向一个具体的笔记项。

给定这个 URI，提供者应检索出该 URI 所标识的行。提供者还应能够使用任何状态变更方法（`insert`、`update` 或 `delete`）来更改此 URI 处的内容。

### Android MIME 类型结构

正如网站会针对特定 URL 返回 MIME 类型（这允许浏览器调用正确的程序来查看内容），内容提供者还有一项额外职责，即为给定的 URI 返回 MIME 类型。这为数据的查看方式提供了灵活性。知道数据类型后，你可能会有多个程序知晓如何处理该数据。例如，如果你硬盘上有一个文本文件，会有许多编辑器能够显示该文本文件。根据操作系统的不同，它甚至可能让你选择使用哪个编辑器。

MIME 类型在 Android 中的工作方式与在 HTTP 中类似。你向提供者询问它所支持的特定 URI 的 MIME 类型，提供者会返回一个由两部分组成的字符串，该字符串根据标准 Web MIME 约定标识其 MIME 类型。你可以在以下位置找到 MIME 类型标准：

`http://tools.ietf.org/html/rfc2046`

根据 MIME 类型规范，一个 MIME 类型包含两部分：主类型和子类型。以下是一些广为人知的 MIME 类型对的示例：

`text/html`

`text/css`

`text/xml`

`text/vnd.curl`

`application/pdf`

`application/rtf`

`application/vnd.ms-excel`

你可以在互联网号码分配机构（IANA）网站上查看注册的主类型和子类型的完整列表：

`http://www.iana.org/assignments/media-types/`

已注册的主要内容类型有：

`application`

`audio`

`example`

`image`

`message`

`model`

`multipart`

`text`

`video`

这些主要类型中的每一种都有子类型。但如果供应商拥有专有数据格式，子类型名称会以 `vnd` 开头。例如，Microsoft Excel 电子表格由子类型 `vnd.ms-excel` 标识，而 `pdf` 被视为非供应商标准，并且这样表示时不带任何供应商特定的前缀。

有些子类型以 `x-` 开头；这些是非标准的子类型，无需注册。它们被视为两个协作代理之间双边定义的私有值。以下是一些示例：

`application/x-tar`

`audio/x-aiff`

`video/x-msvideo`

Android 遵循类似的约定来定义 MIME 类型。Android MIME 类型中的 `vnd` 表示这些主类型和子类型是非标准的、供应商特定的形式。为了提供唯一性，Android 进一步使用类似于域规范的多个部分来区分主类型和子类型。此外，每种内容类型的 Android MIME 类型有两种形式：一种用于单个记录，另一种用于多条记录。

对于单个记录，MIME 类型如下所示：

`vnd.android.cursor.item/vnd.yourcompanyname.contenttype`

对于记录或行的集合，MIME 类型如下所示：

`vnd.android.cursor.dir/vnd.yourcompanyname.contenttype`

这里有几个示例：

```
// 一条笔记
vnd.android.cursor.item/vnd.google.note
```

```
// 笔记的集合或目录
vnd.android.cursor.dir/vnd.google.note
```

**注意：** 这里的含义是 Android 原生地识别项目目录和单个项目。作为程序员，你的灵活性仅限于子类型。例如，列表控件之类的东西依赖于从游标返回的内容作为这些 MIME 主类型之一。

MIME 类型在 Android 中广泛使用，尤其是在意图中，系统会根据数据的 MIME 类型来确定要调用哪个活动。MIME 类型总是通过内容提供者从其 URI 派生而来。在处理 MIME 类型时，你需要牢记三点：

- 主类型和子类型对于它们所代表的内容必须是唯一的。正如所指出的，主类型在很大程度上是为你决定好的。它主要是一个项目目录或单个项目。在 Android 的上下文中，这些可能不像你想象的那么开放。

- 如果主类型和子类型不是标准的（在谈到特定记录时通常如此），则需要在它们前面加上 `vnd`。

- 它们通常根据你的特定需求进行命名空间划分。

为了重申这一点，通过 Android `游标` 返回的项目集合的主要 MIME 类型应始终为 `vnd.android.cursor.dir`，而通过 Android `游标` 检索到的单个项目的主要 MIME 类型应为 `vnd.android.cursor.item`。在子类型方面，你有更多的回旋余地，例如 `vnd.google.note`；在 `vnd.` 部分之后，你可以自由地将其子类型设置为任何你想要的。

### 使用 URI 读取数据

现在你知道，要从内容提供者检索数据，需要使用该内容提供者提供的 URI。由于内容提供者定义的 URI 对该提供者是唯一的，因此记录这些 URI 并让程序员能够查看和调用它们非常重要。Android 自带的提供者通过定义代表这些 URI 字符串的常量来实现这一点。

请考虑 Android SDK 中辅助类定义的这三个 URI：

`MediaStore.Images.Media.INTERNAL_CONTENT_URI`

`MediaStore.Images.Media.EXTERNAL_CONTENT_URI`

`ContactsContract.Contacts.CONTENT_URI`

对应的文本 URI 字符串如下：

`content://media/internal/images`

`content://media/external/images`

`content://com.android.contacts/contacts/`

给定这些 URI，从联系人提供者检索单行人员记录的代码如下所示：

```
Uri peopleBaseUri = ContactsContract.Contacts.CONTENT_URI;
Uri myPersonUri = Uri.withAppendedPath(peopleBaseUri, "23");

//查询这条记录。
//managedQuery 是 Activity 类的一个方法
Cursor cur = managedQuery(myPersonUri, null, null, null);
```

请注意 `ContactsContract.Contacts.CONTENT_URI` 是如何在 `Contacts` 类中被预定义为常量的。我们将变量命名为 `peopleBaseUri`，以表明如果你的目的是查找人员，则需要使用联系人内容 URI。当然，如果你从概念上将人员视为联系人，也可以将此变量命名为 `contactsBaseUri`。

注意：请参考第 30 章以更详细地了解联系人内容提供者。另请注意，联系人 API 及其相关常量可能会随每个 API 版本而更改。本章已使用 Android 2.2（API 8）及更高版本进行测试。

在这个例子中，代码获取根 URI，向其添加一个特定的人员 ID，然后调用 `managedQuery` 方法。

作为针对此 URI 进行查询的一部分，可以指定排序顺序、要选择的列以及 `where` 子句。在此示例中，这些附加参数均设置为 `null`。

注意：内容提供者应通过实现一组接口或将列名列为常量来列出其支持的列。但是，为列定义常量的类或接口还应通过列命名约定、注释或文档来明确列的类型，因为目前没有正式的方法通过常量来指示列的类型。

列表 4–1 展示了如何基于前面的示例，从联系人内容提供者中检索具有特定列列表的游标。

列表 4–1. 从内容提供者检索游标

```
//使用此接口查看常量
import ContactsContract.Contacts;
...
// 指定要返回哪些列的数组。
string[] projection = new string[] {
    Contacts._ID,
    Contacts.DISPLAY_NAME_PRIMARY
};

Uri mContactsUri = ContactsContract.Contacts.CONTENT_URI;  

// 检索查询的最佳方式；返回一个托管的查询。
Cursor managedCursor = managedQuery( mContactsUri,
                        projection, //要返回哪些列。
                        null,       // WHERE 子句
                        Contacts.DISPLAY_NAME_PRIMARY + " ASC"); // 排序子句。
```

请注意，`projection` 仅仅是一个表示列名的字符串数组。因此，除非你知道这些列是什么，否则你会发现很难创建 `projection`。你应该在与提供 URI 相同的类中查找这些列名，此例中就是 `Contacts` 类。你可以通过查看 `android.provider.ContactsContract.Contacts` 类的 SDK 文档（可从以下网址获取）来详细了解每个列：

`http://developer.android.com/reference/android/provider/ContactsContract.Contacts.html`

让我们重新审视返回的 `cursor`：它包含零条或多条记录。列名、顺序和类型是提供者特定的。但是，返回的每一行都有一个名为 `_id` 的默认列，代表该行的唯一标识符。

## 使用 Android 游标

以下是关于 Android 游标的一些要点：

- 游标是行的集合。

- 在读取任何数据之前，你需要使用 `moveToFirst()`，因为游标起始位置在第一行之前。

- 你需要知道列名。

- 你需要知道列类型。

- 所有字段访问方法都基于列号，因此你必须先将列名转换为列号。

- 游标是随机的（可以向前、向后移动，也可以跳转）。

- 由于游标是随机的，你可以询问它的行数。

Android 游标有许多方法允许你对其进行导航。列表 4–2 展示了如何检查游标是否为空，以及当游标不为空时如何逐行遍历游标。

列表 4–2. 使用 `while` 循环遍历游标

```
if (cur.moveToFirst() == false)
{
   //没有行，游标为空
   return;
}

//游标已指向第一行
//让我们访问几个列
int nameColumnIndex = cur.getColumnIndex(Contacts.DISPLAY_NAME_PRIMARY);
String name = cur.getString(nameColumnIndex);

//现在我们来看看如何循环遍历游标

while(cur.moveToNext())
{
   //游标移动成功
   //访问字段
}
```

列表 4–2 开头的假设是游标位于第一行之前。为了将游标定位到第一行，我们在游标对象上使用 `moveToFirst()` 方法。如果游标为空，此方法返回 `false`。然后我们重复使用 `moveToNext()` 方法来遍历游标。

为了帮助你了解游标的位置，Android 提供了以下方法：

```
isBeforeFirst()
isAfterLast()
isClosed()
```

使用这些方法，你也可以像列表 4–3 那样使用 `for` 循环来遍历游标，而不是使用列表 4–2 中的 `while` 循环。

列表 4–3. 使用 `for` 循环遍历游标

```
//在 for 循环外部先获取你的索引
int nameColumn = cur.getColumnIndex(Contacts.DISPLAY_NAME_PRIMARY);

//现在基于列索引遍历游标
for(cur.moveToFirst();!cur.isAfterLast();cur.moveToNext())
{
   String name = cur.getString(nameColumn);
}
```

列的索引顺序似乎有点随意。因此，我们建议你先从游标中显式获取索引，以避免意外。要查找游标中的行数，Android 在游标对象上提供了一个名为 `getCount()` 的方法。

## 使用 where 子句

内容提供者提供了两种传递 `where` 子句的方式：

- 通过 URI

- 通过结合使用 `string` 子句和一组可替换的字符串数组参数

我们将通过一些示例代码来介绍这两种方法。

### 通过 URI 传递 where 子句

假设你想从 Google 笔记数据库中检索 ID 为 23 的笔记。你会使用代码清单 4-4 中的代码来获取一个包含一条记录的游标，该记录对应 notes 表中的第 23 行。

代码清单 4-4. 通过 URI 传递 SQL where 子句

```java
Activity someActivity;
//..初始化 someActivity
String noteUri = "content://com.google.provider.NotePad/notes/23";
Cursor managedCursor = someActivity.managedQuery( noteUri,
                        projection, //要返回的列
                        null,       // WHERE 子句
                        null); // ORDER BY 子句
```

我们将 `managedQuery` 方法的 `where` 子句参数设为 `null`，因为在这种情况下，我们假设笔记提供程序足够智能，能够识别出我们想要的笔记的 `id`。这个 `id` 嵌入在 URI 本身中。我们将 URI 作为传递 `where` 子句的载体。当你看笔记提供程序如何实现相应的查询方法时，这一点就变得很明显了。以下是该查询方法的一段代码片段：

```java
//从类似 content://.../notes/23 的传入 URI 中检索笔记 ID
int noteId = uri.getPathSegments().get(1);

//请求查询构建器构建查询
//指定表名
queryBuilder.setTables(NOTES_TABLE_NAME);

//使用 noteId 来添加 where 子句
queryBuilder.appendWhere(Notes._ID + "=" + noteId);
```

注意笔记的 ID 是如何从 URI 中提取的。表示传入参数`uri`的`Uri`类有一个方法，用于提取 URI 中根路径`content://com.google.provider.NotePad`之后的部分。这些部分被称为路径段（path segments）；它们是位于`/`分隔符之间的字符串，例如`/seg1/seg3/seg4/`，并且按其位置进行索引。对于这里的 URI，第一个路径段就是 23。然后，我们使用这个 ID 23 来追加到为`QueryBuilder`类指定的`where`子句中。最终，等效的 select 语句将为：

```sql
select * from notes where _id = 23
```

**注意：**`Uri`和`UriMatcher`类用于识别 URI 并从其中提取参数。（我们将在“使用 UriMatcher 识别 URI”一节中进一步介绍`UriMatcher`。）`SQLiteQueryBuilder`是`android.database.sqlite`中的一个辅助类，它允许你构建将由`SQLiteDatabase`在 SQLite 数据库实例上执行的 SQL 查询。

## 使用显式 where 子句

现在你已经了解了如何使用 URI 发送`where`子句，接下来考虑 Android 允许我们发送显式列列表及其对应值作为`where`子句的另一种方法。为了探索这一点，让我们再次看看我们在代码清单 4-4 中使用的`Activity`类的`managedQuery`方法。以下是它的签名：

```java
public final Cursor managedQuery(Uri uri,
   String[] projection,
   String selection,
   String[] selectionArgs,
   String sortOrder)
```

注意名为`selection`的参数，其类型为`String`。这个选择字符串代表一个过滤器（本质上是一个`where`子句），用于声明要返回哪些行，其格式为 SQL `where`子句（不包含`WHERE`本身）。传递`null`将返回给定 URI 的所有行。在选择字符串中，你可以包含`?`，这些`?`将被`selectionArgs`中的值按它们在 selection 中出现的顺序替换。这些值将作为`String`类型绑定。

由于你有两种指定`where`子句的方法，你可能会发现很难确定提供程序是如何使用这些`where`子句的，以及如果同时使用两个`where`子句，哪个`where`子句优先。

例如，你可以使用以下任意一种方法查询 ID 为 23 的笔记：

```java
//URI 方法
managedQuery("content://com.google.provider.NotePad/notes/23"
,null
,null
,null
,null);
```

或者

```java
//显式 where 子句
managedQuery("content://com.google.provider.NotePad/notes"
,null
,"_id=?"
,new String[] {23}
,null);
```

惯例是尽可能通过 URI 使用`where`子句，并将显式选项作为特殊情况使用。

## 插入记录

到目前为止，我们已经讨论了如何使用 URI 从内容提供程序检索数据。现在，让我们将注意力转向插入、更新和删除操作。

**注意：** 在解释内容提供程序时，到目前为止，我们使用了 Google 教程提供的记事本应用程序作为原型示例。但是，并不需要完全熟悉那个应用程序。即使你没有看过该应用程序，也应该能够理解这些示例。不过，我们将在本章后面部分为你提供一个示例提供程序的完整代码。

Android 使用一个名为`android.content.ContentValues`的类来保存待插入的单条记录的值。`ContentValues`是一个键/值对字典，非常类似于列名及其值。插入记录的步骤是：先将记录填充到`ContentValues`中，然后请求`android.content.ContentResolver`使用 URI 插入该记录。

**注意：** 你需要定位到`ContentResolver`，因为在这个抽象层面上，你不是在请求数据库插入记录；而是在请求将记录插入到由 URI 标识的提供程序中。`ContentResolver`负责将 URI 引用解析到正确的提供程序，然后将`ContentValues`对象传递给该特定的提供程序。

以下是准备插入操作时，将单行笔记填充到`ContentValues`中的示例：

```java
ContentValues values = new ContentValues();
values.put("title", "新笔记");
values.put("note","这是一条新笔记");

//values 对象现在已准备好插入
```

你可以通过向`Activity`类请求来获取`ContentResolver`的引用：

```java
ContentResolver contentResolver = activity.getContentResolver();
```

现在，你只需要一个 URI 来告诉`ContentResolver`插入该行。这些 URI 在与`Notes`表对应的类中定义。在记事本示例中，该 URI 为：

```java
Notepad.Notes.CONTENT_URI
```

我们可以使用这个 URI 和我们已经拥有的`ContentValues`来调用插入该行的方法：

```java
Uri uri = contentResolver.insert(Notepad.Notes.CONTENT_URI, values);
```

此调用返回一个指向新插入记录的 URI。返回的 URI 将匹配以下结构：

```java
Notepad.Notes.CONTENT_URI/new_id
```

### 向内容提供程序添加文件

有时，你可能需要将文件存储在数据库中。通常的做法是将文件保存到磁盘，然后更新数据库中指向该文件名的记录。

Android 采用此协议并通过定义保存和检索这些文件的特定程序来自动化该过程。Android 使用一种约定，即在对保留列名`_data`的记录中保存对文件名的引用。

当在表中插入一条记录时，Android 会向调用者返回一个 URI。一旦你使用此机制保存了记录，你还需要接着将文件保存到该位置。为此，Android 允许`ContentResolver`获取数据库记录的 URI，并返回一个可写的输出流。在后台，Android 会分配一个内部文件，并将该文件名的引用存储在`_data`字段中。

如果你要扩展记事本示例以便为给定笔记存储图像，你可以创建一个名为`_data`的附加列，并首先执行插入操作以获取一个 URI。以下代码演示了此协议的这一部分：

```java
ContentValues values = new ContentValues();
values.put("title", "新笔记");
values.put("note","这是一条新笔记");

//使用内容解析器插入记录
ContentResolver contentResolver = activity.getContentResolver();
Uri newUri = contentResolver.insert(Notepad.Notes.CONTENT_URI, values);
```

一旦你获得了记录的 URI，以下代码请求`ContentResolver`获取对文件输出流的引用：

```java
....
//直接使用内容解析器获取输出流
//ContentResolver 隐藏了对 _data 字段的访问，该字段存储了实际的文件引用。
OutputStream outStream = activity.getContentResolver().openOutputStream(newUri);
someSourceBitmap.compress(Bitmap.CompressFormat.JPEG, 50, outStream);
outStream.close();
```

然后，代码使用该输出流进行写入。

### 更新与删除

到目前为止，我们已经讨论了查询和插入操作；更新和删除操作相对直接。执行更新类似于执行插入，其中更改的列值通过`ContentValues`对象传递。以下是`ContentResolver`对象上`update`方法的签名：

### ContentResolver 的 `update` 和 `delete` 方法

```java
int numberOfRowsUpdated =
activity.getContentResolver().update(
    Uri uri,
    ContentValues values,
    String whereClause,
    String[] selectionArgs )
```

`whereClause` 参数用于将更新约束到相关的行。类似地，`delete` 方法的签名是：

```java
int numberOfRowsDeleted =
activity.getContentResolver().delete(
    Uri uri,
    String whereClause,
    String[] selectionArgs )
```

显然，`delete` 方法不需要 `ContentValues` 参数，因为在删除记录时你无需指定要删除的列。

几乎所有来自 `managedQuery` 和 `ContentResolver` 的调用最终都定向到 `provider` 类。了解提供程序如何实现这些方法中的每一个，足以让我们了解客户端如何使用这些方法。在下一节中，我们将从头开始介绍一个示例内容提供程序的实现：名为 `BookProvider`。

### 实现内容提供程序

我们已经讨论了如何与内容提供程序交互以获取数据，但尚未讨论如何编写内容提供程序。要编写内容提供程序，你必须继承 `android.content.ContentProvider` 并实现以下关键方法：

- `query`
- `insert`
- `update`
- `delete`
- `getType`

在实现它们之前，你还需要设置一些内容。我们将通过描述你需要采取的步骤来说明内容提供程序实现的所有细节：

1.  规划你的数据库、URI、列名等，并创建一个元数据类，为所有这些元数据元素定义常量。
2.  继承抽象类 `ContentProvider`。
3.  实现这些方法：`query`、`insert`、`update`、`delete` 和 `getType`。
4.  在清单文件中注册提供程序。

#### 规划数据库

为了探索这个主题，我们将创建一个包含书籍集合的数据库。该书籍数据库只包含一个名为 `books` 的表，其列名为 `name`、`isbn` 和 `author`。这些列名属于元数据。你将在 Java 类中定义这类相关元数据。这个承载元数据的 Java 类 `BookProviderMetaData` 如**列表 4–5** 所示。该元数据类的一些关键元素已突出显示。

**列表 4–5.** *为数据库定义元数据：`BookProviderMetaData` 类*

```java
public class BookProviderMetaData
{
    public static final String AUTHORITY = "com.androidbook.provider.BookProvider";

    public static final String DATABASE_NAME = "book.db";
    public static final int DATABASE_VERSION = 1;
    public static final String BOOKS_TABLE_NAME = "books";

    private BookProviderMetaData() {}

    //描述 BookTable 的内部类
    public static final class BookTableMetaData implements BaseColumns
    {
        private BookTableMetaData() {}
        public static final String TABLE_NAME = "books";

        //URI 和 MIME 类型定义
        public static final Uri CONTENT_URI =
                          Uri.parse("content://" + AUTHORITY + "/books");

        public static final String CONTENT_TYPE =
                    "vnd.android.cursor.dir/vnd.androidbook.book";

        public static final String CONTENT_ITEM_TYPE =
                        "vnd.android.cursor.item/vnd.androidbook.book";

        public static final String DEFAULT_SORT_ORDER = "modified DESC";

        //附加列从这里开始。
        //字符串类型
        public static final String BOOK_NAME = "name";

        //字符串类型
        public static final String BOOK_ISBN = "isbn";

        //字符串类型
        public static final String BOOK_AUTHOR = "author";

        //来自 System.currentTimeMillis() 的整数
        public static final String CREATED_DATE = "created";

        //来自 System.currentTimeMillis() 的整数
        public static final String MODIFIED_DATE = "modified";
    }
}
```

这个 `BookProviderMetaData` 类首先将其授权定义为 `com.androidbook.provider.BookProvider`。我们将使用这个字符串在 Android 清单文件中注册提供程序。该字符串构成了用于此提供程序的 URI 的前半部分。

然后，该类继续将其唯一的一个表 (`books`) 定义为一个内部类 `BookTableMetaData`。`BookTableMetaData` 类接着定义一个用于标识书籍集合的 URI。鉴于上一段中的授权，书籍集合的 URI 将如下所示：

```
content://com.androidbook.provider.BookProvider/books
```

该 URI 由常量 `BookProviderMetaData.BookTableMetaData.CONTENT_URI` 表示。

`BookTableMetaData` 类接着定义书籍集合和单本书籍的 MIME 类型。提供程序实现将使用这些常量来返回传入 URI 的 MIME 类型。

然后，`BookTableMetaData` 定义列集合：`name`、`isbn`、`author`、`created`（创建日期）和 `modified`（最后更新日期）。

**注意：** 你应该通过代码中的注释来指明列的数据类型。

元数据类 `BookTableMetaData` 还继承了 `BaseColumns` 类，该类提供了标准的 `_id` 字段，表示行 ID。有了这些元数据定义，我们就可以着手实现提供程序了。

### 扩展 `ContentProvider`

实现我们的 `BookProvider` 示例内容提供程序需要扩展 `ContentProvider` 类，并重写 `onCreate()` 来创建数据库，然后实现 `query()`、`insert()`、`update()`、`delete()` 和 `getType()` 方法。本节介绍数据库的设置和创建，后续几节将分别介绍 `query()`、`insert()`、`update()`、`delete()` 和 `getType()` 这些独立的方法。代码清单 4-6 提供了该类的完整源代码。该类的重要子节已高亮显示。

查询方法需要指定需要返回的列集合。这类似于 `SELECT` 子句，它需要列名以及它们的 `AS` 别名（有时称为 *同义词*）。Android 使用一个 `Map` 对象（称为 `projection map`）来表示这些列名及其同义词。我们需要设置这个映射，以便稍后在查询方法的实现中使用它。在提供程序实现的代码中（参见代码清单 4-6），你将看到这部分在“投影映射设置”中预先完成。

我们将实现的大多数方法都将 URI 作为输入。尽管此内容提供程序能够响应的所有 URI 都以相同的模式开头，但 URI 的尾部会有所不同——就像网站一样。每个 URI 虽然开头相同，但必须不同才能标识不同的数据或文档。下面是一个例子：

```
Uri1: content://com.androidbook.provider.BookProvider/books
Uri2: content://com.androidbook.provider.BookProvider/books/12
```

图书提供程序需要区分这些 URI。这是一个简单的情况。如果我们的图书提供程序容纳了更多对象而不仅仅是图书，那么就会有更多的 URI 来标识这些对象。

提供程序实现需要一种机制来区分不同的 URI；Android 为此目的使用了一个名为 `UriMatcher` 的类。因此，我们需要设置这个对象，使其包含我们所有的 URI 变体。你将在代码清单 4-6 中创建投影映射的片段之后看到这些代码。我们将在“使用 `UriMatcher` 识别 URI”一节中进一步解释 `UriMatcher` 类。

然后，代码清单 4-6 中的代码重写了 `onCreate()` 方法来促进数据库的创建。然后，源代码实现了 `insert()`、`query()`、`update()`、`getType()` 和 `delete()` 方法。所有这些代码都列在一个清单中，但我们将在单独的小节中解释每个方面。

## 代码清单 4-6. 实现 BookProvider 内容提供程序

```markdown
public class `BookProvider` extends `ContentProvider`
{
    //日志辅助标签。对提供程序没有特殊意义。
    private static final String `TAG` = "BookProvider";

    //设置投影映射
    //投影映射类似于 sql 语句中的 "as"（列别名）结构，
    //可以用来重命名列。
    private static `HashMap<String, String>` `sBooksProjectionMap`;
    static
    {
        `sBooksProjectionMap` = new `HashMap<String, String>();
        `sBooksProjectionMap`.put(`BookTableMetaData._ID`,
                                `BookTableMetaData._ID`);

        //名称，isbn，作者
        `sBooksProjectionMap`.put(`BookTableMetaData.BOOK_NAME`,
                                `BookTableMetaData.BOOK_NAME`);
        `sBooksProjectionMap`.put(`BookTableMetaData.BOOK_ISBN`,
                                `BookTableMetaData.BOOK_ISBN`);
        `sBooksProjectionMap`.put(`BookTableMetaData.BOOK_AUTHOR`,
                                `BookTableMetaData.BOOK_AUTHOR`);

        //创建日期，修改日期
        `sBooksProjectionMap`.put(`BookTableMetaData.CREATED_DATE`,
                                `BookTableMetaData.CREATED_DATE`);
        `sBooksProjectionMap`.put(`BookTableMetaData.MODIFIED_DATE`,
                                `BookTableMetaData.MODIFIED_DATE`);
    }

    //设置 URI
    //提供一种机制来识别所有传入的 URI 模式。
    private static final `UriMatcher` `sUriMatcher`;
    private static final int `INCOMING_BOOK_COLLECTION_URI_INDICATOR` = 1;
    private static final int `INCOMING_SINGLE_BOOK_URI_INDICATOR` = 2;
    static {
        `sUriMatcher` = new `UriMatcher`(`UriMatcher.NO_MATCH`);
        `sUriMatcher`.addURI(`BookProviderMetaData.AUTHORITY`, "books",
                          `INCOMING_BOOK_COLLECTION_URI_INDICATOR`);
        `sUriMatcher`.addURI(`BookProviderMetaData.AUTHORITY`, "books/#",
                          `INCOMING_SINGLE_BOOK_URI_INDICATOR`);
    }

    /**
     * 设置/创建数据库
     * 此类用于帮助打开、创建和升级数据库文件。
     */
    private static class `DatabaseHelper` extends `SQLiteOpenHelper` {

        `DatabaseHelper`(`Context` context) {
            super(context,
                `BookProviderMetaData.DATABASE_NAME`,
                null,
                `BookProviderMetaData.DATABASE_VERSION`);
        }

        @Override
        public void `onCreate`(`SQLiteDatabase` db)
        {
            `Log.d`(`TAG`,"内部 oncreate 被调用");
            db.execSQL("CREATE TABLE " + `BookTableMetaData.TABLE_NAME` + " ("
                    + `BookTableMetaData._ID` + " INTEGER PRIMARY KEY,"
                    + `BookTableMetaData.BOOK_NAME` + " TEXT,"
                    + `BookTableMetaData.BOOK_ISBN` + " TEXT,"
                    + `BookTableMetaData.BOOK_AUTHOR` + " TEXT,"
                    + `BookTableMetaData.CREATED_DATE` + " INTEGER,"
                    + `BookTableMetaData.MODIFIED_DATE` + " INTEGER"
                    + ");");
        }

        @Override
        public void `onUpgrade`(`SQLiteDatabase` db, int oldVersion, int newVersion)
        {
            `Log.d`(`TAG`,"内部 onupgrade 被调用");
            `Log.w`(`TAG`, "正在将数据库从版本 "
                    + oldVersion + " 升级到 "
                    + newVersion + "，这将销毁所有旧数据");
            db.execSQL("DROP TABLE IF EXISTS " +
                     `BookTableMetaData.TABLE_NAME`);
            `onCreate`(db);
        }
    }

    private `DatabaseHelper` `mOpenHelper`;

    //组件创建回调
    @Override
    public boolean `onCreate`()
    {
        `Log.d`(`TAG`,"主 onCreate 被调用");
        `mOpenHelper` = new `DatabaseHelper`(`getContext`());
        return true;
    }

    @Override
    public `Cursor` `query`(`Uri` uri, String[] projection, String selection,
            String[] selectionArgs,  String sortOrder)
    {
        `SQLiteQueryBuilder` qb = new `SQLiteQueryBuilder`();

        switch (`sUriMatcher`.match(uri)) {
        case `INCOMING_BOOK_COLLECTION_URI_INDICATOR`:
            qb.setTables(`BookTableMetaData.TABLE_NAME`);
            qb.setProjectionMap(`sBooksProjectionMap`);
            break;

        case `INCOMING_SINGLE_BOOK_URI_INDICATOR`:
            qb.setTables(`BookTableMetaData.TABLE_NAME`);
            qb.setProjectionMap(`sBooksProjectionMap`);
            qb.appendWhere(`BookTableMetaData._ID` + "="
                        + uri.getPathSegments().get(1));
            break;

        default:
            throw new IllegalArgumentException("未知 URI " + uri);
        }

        // 如果未指定排序顺序，则使用默认排序
        String orderBy;
        if (`TextUtils.isEmpty`(sortOrder)) {
            orderBy = `BookTableMetaData.DEFAULT_SORT_ORDER`;
        } else {
            orderBy = sortOrder;
        }
```

```java
// Get the database and run the query
SQLiteDatabase db = mOpenHelper.getReadableDatabase();
Cursor c = qb.query(db, projection, selection,
                   selectionArgs, null, null, orderBy);

//example of getting a count
int i = c.getCount();

// Tell the cursor what uri to watch,
// so it knows when its source data changes
c.setNotificationUri(getContext().getContentResolver(), uri);
return c;
}

@Override
public String getType(Uri uri)
{
    switch (sUriMatcher.match(uri)) {
    case INCOMING_BOOK_COLLECTION_URI_INDICATOR:
        return BookTableMetaData.CONTENT_TYPE;

    case INCOMING_SINGLE_BOOK_URI_INDICATOR:
        return BookTableMetaData.CONTENT_ITEM_TYPE;

    default:
        throw new IllegalArgumentException("Unknown URI " + uri);
    }
}

@Override
public Uri insert(Uri uri, ContentValues initialValues)
{
    // Validate the requested uri
    if (sUriMatcher.match(uri)
            != INCOMING_BOOK_COLLECTION_URI_INDICATOR)
    {
        throw new IllegalArgumentException("Unknown URI " + uri);
    }

    ContentValues values;
    if (initialValues != null) {
        values = new ContentValues(initialValues);
    } else {
        values = new ContentValues();
    }

    Long now = Long.valueOf(System.currentTimeMillis());

    // Make sure that the fields are all set
    if (values.containsKey(BookTableMetaData.CREATED_DATE) == false)
    {
        values.put(BookTableMetaData.CREATED_DATE, now);
    }

    if (values.containsKey(BookTableMetaData.MODIFIED_DATE) == false)
    {
        values.put(BookTableMetaData.MODIFIED_DATE, now);
    }

    if (values.containsKey(BookTableMetaData.BOOK_NAME) == false)
    {
        throw new SQLException(
           "Failed to insert row because Book Name is needed " + uri);
    }

    if (values.containsKey(BookTableMetaData.BOOK_ISBN) == false) {
        values.put(BookTableMetaData.BOOK_ISBN, "Unknown ISBN");
    }
    if (values.containsKey(BookTableMetaData.BOOK_AUTHOR) == false) {
        values.put(BookTableMetaData.BOOK_AUTHOR, "Unknown Author");
    }

    SQLiteDatabase db = mOpenHelper.getWritableDatabase();
    long rowId = db.insert(BookTableMetaData.TABLE_NAME,
            BookTableMetaData.BOOK_NAME, values);
    if (rowId > 0) {
        Uri insertedBookUri =
            ContentUris.withAppendedId(
                    BookTableMetaData.CONTENT_URI, rowId);
        getContext()
           .getContentResolver()
                .notifyChange(insertedBookUri, null);

        return insertedBookUri;
    }

    throw new SQLException("Failed to insert row into " + uri);
}

@Override
public int delete(Uri uri, String where, String[] whereArgs)
{
    SQLiteDatabase db = mOpenHelper.getWritableDatabase();
    int count;
    switch (sUriMatcher.match(uri)) {
    case INCOMING_BOOK_COLLECTION_URI_INDICATOR:
        count = db.delete(BookTableMetaData.TABLE_NAME,
                where, whereArgs);
        break;

    case INCOMING_SINGLE_BOOK_URI_INDICATOR:
        String rowId = uri.getPathSegments().get(1);
        count = db.delete(BookTableMetaData.TABLE_NAME,
                BookTableMetaData._ID + "=" + rowId
                + (!TextUtils.isEmpty(where) ? " AND (" + where + ')' : ""),
                whereArgs);
        break;

    default:
        throw new IllegalArgumentException("Unknown URI " + uri);
    }

    getContext().getContentResolver().notifyChange(uri, null);
    return count;
}

@Override
public int update(Uri uri, ContentValues values,
        String where, String[] whereArgs)
{
    SQLiteDatabase db = mOpenHelper.getWritableDatabase();
    int count;
    switch (sUriMatcher.match(uri)) {
    case INCOMING_BOOK_COLLECTION_URI_INDICATOR:
        count = db.update(BookTableMetaData.TABLE_NAME,
                values, where, whereArgs);
        break;

    case INCOMING_SINGLE_BOOK_URI_INDICATOR:
        String rowId = uri.getPathSegments().get(1);
        count = db.update(BookTableMetaData.TABLE_NAME,
                values, BookTableMetaData._ID + "=" + rowId
                + (!TextUtils.isEmpty(where) ? " AND (" + where + ')' : ""),
                whereArgs);
        break;

    default:
        throw new IllegalArgumentException("Unknown URI " + uri);
    }

    getContext().getContentResolver().notifyChange(uri, null);
    return count;
}
```

### 履行 MIME 类型契约

`BookProvider` 内容提供程序还必须实现 `getType()` 方法，以便为给定的 URI 返回 MIME 类型。与内容提供程序的许多其他方法一样，此方法会根据传入的 URI 进行重载。因此，`getType()` 方法的首要任务是区分 URI 的类型：它是一个书籍集合还是单本书？

正如我们在上一节中指出的，我们将使用 `UriMatcher` 来解析此 URI 类型。根据此 URI，`BookTableMetaData` 类已定义了要为每个 URI 返回的 MIME 类型常量。你可以在清单 4–6 中查看此方法的实现。

### 实现查询方法

内容提供程序中的 `query` 方法负责根据传入的 URI 和 `where` 子句返回一个行集合。

与其他方法一样，`query` 方法使用 `UriMatcher` 来识别 URI 类型。如果 URI 类型是单项类型，则该方法像这样从传入的 URI 中检索书籍 ID：

1.  它使用 `getPathSegments()` 提取路径段。
2.  它索引到 URI 中以获取第一个路径段，该路径段恰好是书籍 ID。

然后，`query` 方法使用我们在清单 4–6 中预先创建的 `projection`s 来标识返回列。最后，`query` 将游标返回给调用者。在整个过程中，`query` 方法使用 `SQLiteQueryBuilder` 对象来制定和执行查询（参见清单 4–6）。

### 实现插入方法

内容提供程序中的 `insert` 方法负责将记录插入到底层数据库中，然后返回一个指向新创建记录的 URI。

与其他方法一样，`insert` 使用 `UriMatcher` 来识别 URI 类型。代码首先检查 URI 是否指示了正确的集合类型 URI。如果不是，代码会抛出一个异常（参见清单 4–6）。

然后，代码验证可选和必填的列参数。对于某些缺失的列，代码可以替换为默认值。

接下来，代码使用 `SQLiteDatabase` 对象插入新记录并返回新插入的 ID。最后，代码使用从数据库返回的 ID 构造新的 URI。

### 实现更新方法

内容提供程序中的 `update` 方法负责根据传入的列值以及 `where` 子句来更新一条或多条记录。`update` 方法随后会返回过程中更新的行数。

与其他方法类似，`update` 使用 `UriMatcher` 来识别 URI 类型。如果 URI 类型是集合，则直接传递 `where` 子句以影响尽可能多的记录。如果 URI 类型是单条记录类型，则从 URI 中提取图书 ID，并将其作为额外的 `where` 子句指定。最后，代码返回更新的记录数（参见清单 4-6）。另请注意，`notifyChange` 方法如何让你能够向外界宣告该 URI 处的数据已发生变化。在 `insert` 方法中，当插入一条记录时，你也可以通过声明“`…/books`”已更改来实现同样的效果。

### 实现删除方法

内容提供程序中的 `delete` 方法负责根据传入的 `where` 子句来删除一条或多条记录。`delete` 方法随后会返回过程中删除的行数。

与其他方法类似，`delete` 使用 `UriMatcher` 来识别 URI 类型。如果 URI 类型是集合类型，则直接传递 `where` 子句以便删除尽可能多的记录。如果 `where` 子句为 `null`，则会删除所有记录。如果 URI 类型是单条记录类型，则从 URI 中提取图书 ID，并将其作为额外的 `where` 子句指定。最后，代码返回删除的记录数（参见清单 4-6）。

### 使用 UriMatcher 识别 URI

我们已经多次提到 `UriMatcher` 类；现在来深入了解一下。内容提供程序中的几乎所有方法都针对 URI 进行了重载。例如，无论你是要检索单本书还是一个图书列表，都会调用相同的 `query()` 方法。具体由该方法来判断请求的是哪种 URI 类型。Android 的 `UriMatcher` 工具类能帮助你识别 URI 类型。

其工作原理如下：你告诉一个 `UriMatcher` 实例预期要匹配哪些 URI 模式，并为每个模式关联一个唯一编号。一旦这些模式注册完毕，你就可以询问 `UriMatcher`，传入的 URI 是否匹配某个特定模式。

如前所述，我们的 `BookProvider` 内容提供程序有两个 URI 模式：一个用于图书集合，另一个用于单本书。清单 4-7 中的代码使用 `UriMatcher` 注册了这两种模式。它将 `1` 分配给图书集合，将 `2` 分配给单本书（URI 模式本身在 `books` 表的元数据中定义）。

**清单 4-7.** *使用 `UriMatcher` 注册 URI 模式*

```
private static final UriMatcher sUriMatcher;
//为每种 URI 类型定义 ID
private static final int INCOMING_BOOK_COLLECTION_URI_INDICATOR = 1;
private static final int INCOMING_SINGLE_BOOK_URI_INDICATOR = 2;

static {
    sUriMatcher = new UriMatcher(UriMatcher.NO_MATCH);
    //为图书注册模式
    sUriMatcher.addURI(BookProviderMetaData.AUTHORITY
                         , "books"
                         , INCOMING_BOOK_COLLECTION_URI_INDICATOR);
    //为单本书注册模式
    sUriMatcher.addURI(BookProviderMetaData.AUTHORITY
                            , "books/#",
                            INCOMING_SINGLE_BOOK_URI_INDICATOR);
}
```

注册完成后，你就可以看到 `UriMatcher` 在查询方法实现中的作用：

```
switch (sUriMatcher.match(uri)) {
   case INCOMING_BOOK_COLLECTION_URI_INDICATOR:
   ......
   case INCOMING_SINGLE_BOOK_URI_INDICATOR:
   ......
   default:
      throw new IllegalArgumentException("Unknown URI " + uri);
}
```

请注意，`match` 方法返回的是之前注册的相同编号。`UriMatcher` 的构造函数接受一个整数，用于表示根 URI。如果 URL 上既没有路径段也没有权限标识，`UriMatcher` 会返回这个数字。当模式不匹配时，`UriMatcher` 也会返回 `NO_MATCH`。你可以构造一个不带根匹配码的 `UriMatcher`；在这种情况下，Android 会在内部将 `UriMatcher` 初始化为 `NO_MATCH`。因此，清单 4-7 中的代码也可以写成如下形式：

```
static {
        sUriMatcher = new UriMatcher();
        sUriMatcher.addURI(BookProviderMetaData.AUTHORITY
                                 , "books"
                                 , INCOMING_BOOK_COLLECTION_URI_INDICATOR);

        sUriMatcher.addURI(BookProviderMetaData.AUTHORITY
                                , "books/#",
                                INCOMING_SINGLE_BOOK_URI_INDICATOR);
}
```

### 使用投影映射

内容提供程序充当抽象列集与数据库中真实列集之间的中介，然而这些列集可能不同。在构建查询时，你必须在客户端指定的 `where` 子句列与真实的数据库列之间建立映射。你可以借助 `SQLiteQueryBuilder` 类来设置这个*投影映射*。

以下是 Android SDK 文档关于 `QueryBuilder` 类中 `public void setProjectionMap(Map columnMap)` 映射方法的描述：

> 设置查询的投影映射。投影映射将调用者传入查询的列名映射到数据库列名。这对于重命名列以及在进行连接操作时消除列名歧义非常有用。例如，你可以将“name”映射为“people.name”。如果设置了投影映射，它必须包含用户可能请求的所有列名，即使键和值相同。

以下是我们的 `BookProvider` 内容提供程序设置投影映射的方式：

```
sBooksProjectionMap = new HashMap<String, String>();
sBooksProjectionMap.put(BookTableMetaData._ID, BookTableMetaData._ID);

//名称，ISBN，作者
sBooksProjectionMap.put(BookTableMetaData.BOOK_NAME
                                               , BookTableMetaData.BOOK_NAME);
sBooksProjectionMap.put(BookTableMetaData.BOOK_ISBN
                                              , BookTableMetaData.BOOK_ISBN);
sBooksProjectionMap.put(BookTableMetaData.BOOK_AUTHOR
                                              , BookTableMetaData.BOOK_AUTHOR);

//创建日期，修改日期
sBooksProjectionMap.put(BookTableMetaData.CREATED_DATE
                                               , BookTableMetaData.CREATED_DATE);
sBooksProjectionMap.put(BookTableMetaData.MODIFIED_DATE
                                                , BookTableMetaData.MODIFIED_DATE);
```

然后，查询构建器像这样使用变量 `sBooksProjectionMap`：

```
queryBuilder.setTables(BookTableMetaData.TABLE_NAME);
queryBuilder.setProjectionMap(sBooksProjectionMap);
```

#### 注册提供程序

最后，你必须使用 清单 4-8 中的标签结构，在 `AndroidManifest.xml` 文件中注册内容提供程序。

**清单 4-8.** *注册提供程序*

```
<provider android:name=".BookProvider"
   android:authorities="com.androidbook.provider.BookProvider"/>
```

### 使用图书提供程序

现在我们已经有了一个图书提供程序，接下来将向您展示调用该提供程序的示例代码。示例代码包括添加图书、删除图书、获取图书数量，以及最终显示所有图书。

请注意，这些都是示例项目中的代码片段，由于需要额外的依赖文件，因此无法直接编译。但我们认为，这些示例代码对于演示我们所探讨的概念非常有价值。

在本章末尾，我们提供了可下载示例项目的链接，您可以在自己的 Eclipse 环境中使用它进行编译和测试。

#### 添加图书

清单 4–9 中的代码向图书数据库中插入一本新书。

**清单 4–9.** *使用提供程序进行插入操作*

```java
public void addBook(Context context)
{
    String tag = "Exercise BookProvider";
    Log.d(tag,"Adding a book");
    ContentValues cv = new ContentValues();
    cv.put(BookProviderMetaData.BookTableMetaData.BOOK_NAME, "book1");
    cv.put(BookProviderMetaData.BookTableMetaData.BOOK_ISBN, "isbn-1");
    cv.put(BookProviderMetaData.BookTableMetaData.BOOK_AUTHOR, "author-1");

    ContentResolver cr = context.getContentResolver();
    Uri uri = BookProviderMetaData.BookTableMetaData.CONTENT_URI;
    Log.d(tag,"book insert uri:" + uri);
    Uri insertedUri = cr.insert(uri, cv);
    Log.d(tag,"inserted uri:" + insertedUri);
}
```

#### 删除图书

清单 4–10 中的代码从图书数据库中删除最后一条记录。有关 清单 4–10 中 `getCount()` 方法的工作原理，请参见 清单 4–11 的示例。

**清单 4–10.** *使用提供程序进行删除操作*

```java
public void removeBook(Context context)
{
    String tag = "Exercise BookProvider";
    int i = getCount(context); //参见清单 4-11 中的 getCount 函数
    ContentResolver cr = context.getContentResolver();
    Uri uri = BookProviderMetaData.BookTableMetaData.CONTENT_URI;
    Uri delUri = Uri.withAppendedPath(uri, Integer.toString(i));
    Log.d(tag, "Del Uri:" + delUri);
    cr.delete(delUri, null, null);
    Log.d(tag, "New count:" + getCount(context));
}
```

请注意，这只是演示如何使用 URI 进行删除操作的简单示例。获取最后一个 URI 的算法并非在所有情况下都有效。不过，如果您添加了五条记录并逐个从末尾删除，它应该可以正常工作。在实际应用中，您可能希望将记录显示在列表中，并让用户选择要删除的记录，这样您就能知道该记录的准确 URI。

#### 获取图书数量

清单 4–11 中的代码获取数据库游标，并统计游标中的记录数量。

**清单 4–11.** *统计表中的记录数*

```java
private int getCount(Context context)
{
    Uri uri = BookProviderMetaData.BookTableMetaData.CONTENT_URI;
    Activity a = (Activity)context;
    Cursor c = a.managedQuery(uri,
            null, //投影
            null, //选择字符串
            null, //选择参数字符串数组
            null); //排序顺序
    int numberOfRecords = c.getCount();
    c.close();
    return numberOfRecords;
}
```

#### 显示图书列表

清单 4–12 中的代码检索图书数据库中的所有记录。

**清单 4–12.** *显示图书列表*

```markdown
# 第 4 章

## 理解内容提供者

```java
public void showBooks(Context context)
{
    String tag = "Exercise BookProvider";
    Uri uri = BookProviderMetaData.BookTableMetaData.CONTENT_URI;
    Activity a = (Activity)context;
    Cursor c = a.managedQuery(uri,
            null, //投影
            null, //选择字符串
            null, //选择参数字符串数组
            null); //排序顺序

    int iname = c.getColumnIndex(
        BookProviderMetaData.BookTableMetaData.BOOK_NAME);

    int iisbn = c.getColumnIndex(
        BookProviderMetaData.BookTableMetaData.BOOK_ISBN);
    int iauthor = c.getColumnIndex(
         BookProviderMetaData.BookTableMetaData.BOOK_AUTHOR);

    //报告索引
    Log.d(tag,"name,isbn,author:" + iname + iisbn + iauthor);

    //基于索引遍历行
    for(c.moveToFirst();!c.isAfterLast();c.moveToNext())
    {
        //获取值
        String id = c.getString(1);
        String name = c.getString(iname);
        String isbn = c.getString(iisbn);
        String author = c.getString(iauthor);

        //报告或记录行
        StringBuffer cbuf = new StringBuffer(id);
        cbuf.append(",").append(name);
        cbuf.append(",").append(isbn);
        cbuf.append(",").append(author);
        Log.d(tag, cbuf.toString());
    }

    //报告已读取的行数
    int numberOfRecords = c.getCount();
    Log.d(tag,"Num of Records:" + numberOfRecords);

    //关闭游标
    //理想情况下，这应该在
    //finally 块中完成。
    c.close();
}
```

### 参考资料

以下是一些额外的 Android 资源，可帮助您理解本章所涵盖的主题：

*   [`http://developer.android.com/guide/topics/providers/content-providers.html`](http://developer.android.com/guide/topics/providers/content-providers.html)：关于内容提供程序的 Android 文档。

*   [`http://developer.android.com/reference/android/content/ContentProvider.html`](http://developer.android.com/reference/android/content/ContentProvider.html)：`ContentProvider` 的 API 描述，您可以在此了解 `ContentProvider` 的约定。

*   [`http://developer.android.com/reference/android/content/UriMatcher.html`](http://developer.android.com/reference/android/content/UriMatcher.html)：有助于理解 `UriMatcher` 的信息。

*   [`http://developer.android.com/reference/android/database/Cursor.html`](http://developer.android.com/reference/android/database/Cursor.html)：帮助您直接从内容提供程序或数据库读取数据的信息。

*   [`www.sqlite.org/sqlite.html`](http://www.sqlite.org/sqlite.html)：SQLite 的主页，您可以在此了解更多关于 SQLite 的信息，并下载可用于处理 SQLite 数据库的工具。

*   `androidbook.com/proandroid4/projects`：可通过此 URL 访问本章的可下载测试项目。zip 文件的名称为 `ProAndroid4_Ch04_TestProvider.zip`。

### 本章小结

在本章中，您学习了以下内容：

*   什么是内容提供程序

*   如何发现现有的内容提供程序数据库

*   内容 URI、MIME 类型和内容提供程序的性质

*   如何使用 SQLite 构建响应 URI 的提供程序

*   如何跨进程边界向各种应用程序公开数据

*   如何编写新的内容提供程序

*   如何访问现有的内容提供程序

*   如何使用 `URIMatcher` 辅助实现内容提供程序

### 面试问题

以下问题将帮助你巩固本章所学的关于内容提供者的知识：

1.  内容提供者与网站有何相似之处？

2.  你能列举一些系统内置的内容提供者吗？

3.  使用`adb`工具可以做什么？

4.  什么是`AVD`？

5.  如何列出可用的`AVD`？

6.  Android 中有哪些有用的命令行工具？

7.  内容提供者的数据库存放在何处？

8.  探索内容提供者数据库的好方法是什么？

9.  内容提供者的`authority`属性是什么？

10. 内容提供者的 authority 是否可以缩写？

11. 什么是 MIME 类型？它们与内容提供者有何关联？

12. 程序员如何发现访问内容提供者的 URI？

13. 如何使用内容提供者 URI 访问数据？

14. 如何为内容提供者查询提供`where`子句？

15. 如何遍历游标？

16. `ContentValues`类的作用是什么？

17. `ContentResolver`类的作用是什么？

18. 在内容提供者中保存文件的协议是什么？

19. `URIMatcher`是如何工作的？它有什么用途？
