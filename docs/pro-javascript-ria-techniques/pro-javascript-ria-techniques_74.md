# SQLite 离线数据存储

有关 SQLite 支持的 SQL 命令的完整列表，请参阅 [sqlite.org/lang.html](http://www.sqlite.org/lang.html)。

通过使用数据库来存储离线数据，开发者可以在数据库表中表示数据结构，而不是通过文本字符串——而文本字符串是目前唯一跨浏览器可行的数据存储选项。数据库还支持存储二进制数据（例如图像文件），为可能需要存储和随时检索任意类型数据的 Web 应用提供了更灵活的解决方案。

要让所有浏览器都支持通过 JavaScript 访问本地数据库这一标准，还需要一些时间。目前，该 API 和数据库系统仅在桌面版 Safari 3.1 以及 iPhone 上的 Safari 浏览器中可用。当规范更加完善后，其他浏览器也会跟进。

**318**

## 第 9 章 ■ 离线存储——当灯光熄灭时

出于安全考虑，每个数据库都被锁定到创建它的域名。每个站点可以根据需要创建任意数量的数据库，并包含任意数量的表和行。目前，在 Safari 3.1 中，每个域名的默认存储空间为 5MB。通过浏览器偏好设置，用户可以为每个域名或全局（适用于所有站点）选择本地数据库大小，最小 1MB，最大 500MB。

作为性能优化措施，对数据库执行的事务和操作使用 SQL 语言编写，并且异步执行。命令执行完成后，会传递一个回调函数来执行。在此期间，其余代码可以继续执行，这意味着大型复杂的数据库事务不会阻止浏览器和 Web 应用正常响应用户的需求。

`Listing 9-9` 演示了如何使用客户端数据库存储 API 在本地机器的 SQLite 数据库中获取、设置和删除文本值。将此代码添加到你的 JavaScript 库中，在库实例化之前。请注意，与客户端数据库存储 API 所能实现的所有功能相比，这只是一个相当有限的用法。

**`Listing 9-9`.** *使用数据库存储 API 获取、设置和删除数据*

```
// 添加一个 DBStorage 命名空间来包含所有客户端数据库存储相关方法

$.prototype.Storage.DBStorage = {

    // 每个域名可以有多个数据库，但在此示例中我们定义一个供整个应用使用

    databaseName: "data_store",

    databaseDesc: "Data store",

    // 指定我们使用的 SQLite 数据库版本，以防未来版本更改方法

    sqlLiteDBVersion: "1.0",

    // 5MB 存储空间 = 5120 字节。这是 SQLite 数据库的默认最大大小

    FIVE_MB: 5120,

    // 定义一个用于存储数据的数据库表名

    tableName: "data-store",

    // 定义一个属性来存储数据库引用

    database: null,

    // initialize 方法在数据库中创建表来存储名称和值数据（如果表不存在）

    // name 成为主键

    initialize: function() {

        // 打开 SQLite 数据库

        this.database = openDatabase(this.databaseName,

            this.sqlLiteDBVersion, this.databaseDesc, this.FIVE_MB);

        // 构建用于在数据库中创建表的 SQL 命令

        var command = "CREATE TABLE IF NOT EXISTS {tableName} (name " +

            "TEXT UNIQUE NOT NULL PRIMARY KEY, value TEXT NOT NULL)";

        command = $.Utils.replaceText(command, {

            tableName: this.tableName

        });

        // 执行 SQL 命令

        this.execute(command);

    },

    // execute 方法对数据库执行给定的 SQL 命令，

    // 在命令完成后执行可选的回调函数，并将事务结果传递给回调函数

    execute: function(command, callback) {

        callback = callback || function() {};

    }
```

**319**



```javascript
// 执行所提供的 SQL 命令，然后执行回调函数
this.database.transaction(function(db) {
    db.executeSql(command, [], callback);
});
},

// get 方法根据名称键在数据库中执行查找，并将找到的值（若有）传入所提供的回调函数
get: function(name, callback) {
    // 生成通过名称在数据库中定位值的命令
    var command = "SELECT value FROM {tableName} WHERE name = {name}";
    command = $.Utils.replaceText(command, {
        tableName: this.tableName,
        name: name
    });

    // 执行 SQL 命令
    this.execute(command, function(db, result) {
        var value = "";

        // 在返回的 SQL 数据的第一行中定位值
        if (result.rows.length > 0) {
            value = result.rows.item(0)['value'];
        }

        // 执行回调方法，传入找到的值（若有）
        callback(value);
    });

    // 返回空值，以防有调用方法期望从此方法获取返回值——按理说不应有代码期望此返回值，
    // 但为保险起见，我们在此提供返回值
    return null;
},

// set 方法将值按名称存储到数据库中
set: function(input) {
    // 期望输入为一个对象字面量，包含要设置的名称和值
    var name = input.name || "";
    var value = input.value || "";
    var self = this;

    // 检查数据库中是否已存在该名称对应的值
    this.get(name, function(value) {
        // 默认情况下，我们将值插入数据库，因此指定执行插入操作的命令
        var command = "INSERT INTO {tableName} (name, value) VALUES ➥ ({name}, {value})";

        // 如果数据库中已存在该名称对应的值，则改为执行 SQL 更新命令
        if (value != "") {
            command = "UPDATE {tableName} SET value = {value} WHERE ➥ name = {name}";
        }

        command = $.Utils.replaceText(command, {
            tableName: self.tableName,
            name: name,
            value: value
        });

        // 执行 SQL 命令，将数据保存到数据库中
        this.execute(command);
    });
},

// remove 方法从数据库中删除名称及对应的值
remove: function(name) {
    // 生成从数据库中删除值的 SQL 命令
    var command = "DELETE FROM {tableName} WHERE name = {name}";
    command = $.Utils.replaceText(command, {
        tableName: this.tableName,
        name: name
    });

    // 执行命令，从数据库中移除该条目
    this.execute(command);
}
}
```

清单 9-10 展示了如何在页面中利用 JavaScript，通过位于用户本地计算机或存储设备上的 SQLite 数据库来存储、检索和删除文本值。

请注意，在检索数据时，你需要使用回调函数。由于数据库是异步操作的，要获知数据何时已被定位，唯一的方法就是告诉该方法在数据找到后应该执行哪些代码。

**清单 9-10.** *使用数据库存储 API 设置、获取和删除数据*

```javascript
// 初始化客户端数据库
$.Storage.DBStorage.initialize();

// 保存一个电子邮件地址
$.Storage.DBStorage.set({
    name: "email",
    value: "me@denodell.com"
});

// 输出我们之前创建的"email"值。
// 浏览器重启后，该数据仍将保留
$.Storage.DBStorage.get("email", function(value) {
    alert(value);
});

// 永久删除"email"数据值
$.Storage.DBStorage.remove("email");

// 由于"email"数据已不复存在，因此输出空字符串
$.Storage.DBStorage.get("email", function(value) {
    alert(value);
});
```

### 使用 Flash 共享对象存储数据

如果你的最终用户已安装 Adobe Flash Player 插件（版本 8 或更高），则可以使用另一种数据存储技术。通过一个特别构建的影片文件，你可以通过简单的 API 向 JavaScript 公开所谓的*Flash 本地共享对象*（通常称为*Flash cookie*），以便 JavaScript 在本地存储数据。


- 与浏览器 cookie 不同，本地共享对象（local shared objects）允许每个域在用户机器上存储 100KB 的数据，并且这些值不会随 HTTP 请求发送到服务器。用户可以更改此存储量，尽管许多人选择保留默认设置。正如您所料，作为一种安全措施，从一个域设置的数据无法被任何其他域访问。

我已经构建了必要的 Flash 影片文件，该文件可从本书的详细信息页面（位于 `Apress` 网站（`http://www.apress.com/`））以及本书其余源代码一起获取，也可从我自己的网站（`http://www.denodell.com/`）下载，以供您在自己的 RIA 中使用。请使用第 7 章介绍的 `SWFObject` 组件将 Flash 影片放置在您的页面上。

该 Flash 影片文件公开了三个方法，这些方法可以针对页面上的影片实例进行调用：`get()`、`set()` 和 `remove()`。清单 9-11 显示了要添加到您的 `$` JavaScript 库中以使用 Flash cookie 的代码。将代码添加到 `$` 库文件的末尾，在其实例化之前。

**清单 9-11.** *使用 Flash 本地共享对象获取、设置和删除数据*

```javascript
// 创建一个 Flash 命名空间，用于包含所有与 Flash cookie 相关的存储方法
$.prototype.Storage.Flash = {

    // 用于存储页面上 Flash 影片元素引用的对象
    flashComponent: null,

    // 初始化方法将内部的 flashComponent 对象设置为从页面传入的对象
    // 使用 SWFObject 来加载页面上的影片。
    // 第 7 章展示了如何使用 SWFObject 嵌入 Flash 影片
    initialize: function(flashComponent) {
        this.flashComponent = flashComponent;
    },

    // set 方法通过名称将一个值存储到 Flash 共享对象中
    set: function(input) {
        // 期望输入一个对象字面量，包含要设置的名称和值
        var name = input.name || "";
        var value = input.value || "";
        // 使用 Flash 影片对象内的 set 方法保存数据
        this.flashComponent.set(name, value);
    },

    // get 方法通过名称检索先前存储的值
    get: function(name) {
        // 如果请求的值不存在，则返回空字符串
        return this.flashComponent.get(name) || "";
    },

    // remove 方法从 Flash 共享对象中删除给定名称的值
    remove: function(name) {
        this.flashComponent.remove(name);
    }
}
```

清单 9-12 显示了如何使用清单 9-11 中的方法，通过 JavaScript 使用 Flash 本地共享对象在本地计算机或设备上存储、检索和删除数据。

**清单 9-12.** *使用 Flash 本地共享对象存储、检索和删除数据*

```javascript
// 通过引用页面上包含 Flash 存储文件的 DOM 元素来初始化 Flash 共享对象
// 您可以在我网站上找到此文件：[`www.denodell.com/`](http://www.denodell.com/)
$.Storage.Flash.initialize(document.getElementById("flash-object"));

// 保存一个电子邮件地址
$.Storage.Flash.set({
    name: "email",
    value: "me@denodell.com"
});

// 输出我们之前创建的 "email" 值。
// 浏览器重启后，数据仍然存在
alert($.Storage.Flash.get("email"));

// 永久删除 "email" 数据值
$.Storage.Flash.remove("email");

// 输出空字符串，因为 "email" 数据不再存在
alert($.Storage.Flash.get("email"));
```

### 创建跨浏览器本地数据存储 API

到目前为止，在本章中，您已经了解了如何使用几种不同的技术从本地用户计算机或设备存储和检索文本字符串。每种技术在允许存储的数据量以及不同浏览器对其支持级别方面各不相同。为了在您的 RIA 中处理本地存储，您可以设置一个用于跨浏览器支持的单一 API，该 API 包含所有可用的技术，并使用其中最佳的一种。



## 第 9 章：离线存储——当网络中断时

首先，让我们按优先顺序整理不同的存储技术，从存储空间最大且最符合标准实践的技术开始，到存储空间最小且最不推荐的技术结束：

- **本地存储 API**：推荐使用，因为它已成为既定标准，使用简单，且在最新浏览器版本中获得了相当广泛的支持。默认提供 5MB 存储空间。
- **客户端数据库存储 API**：一项新兴标准，由于其通用性，若非目前规范仍存在较大变动风险，本应是最优选择。默认提供 5MB 存储空间。
- **Mozilla 全局存储 API**：对于 Firefox 2 和 3 用户来说是一个不错的备选方案，这些用户的浏览器尚不支持本地存储 API，但支持这个非常相似的 API。默认提供 5MB 存储空间。
- **IE 的`userData`机制**：IE 5 至 7 版本不支持上述任何推荐方法，但提供了这种无需任何插件即可在本地存储数据的机制。默认提供 64KB 存储空间。
- **Flash 本地共享对象**：对于浏览器不支持任何更优方法的用户，可以利用无处不在的 Flash Player 插件在本地存储数据，而无需依赖 Cookie。默认提供 100KB 存储空间。
- **Cookie**：绝对是最不推荐的选择。Cookie 文件空间极其有限，而且其数据会随每次 HTTP 请求发送到服务器，Cookie 越大，对连接的阻塞就越严重。默认提供 4KB 存储空间。

根据这些信息，你可以创建三个通用方法——`get()`、`set()`和`remove()`——它们会优先选择最优技术，当首选技术不可用时，则会一直降级到简单的 Cookie。清单 9-13 展示了一个跨浏览器的本地存储 API，你可以使用本章给出的代码将其实现到你的`$` JavaScript 库中。请注意，由于其中一种技术（客户端数据库 API）在检索数据时需要提供回调函数，因此本 API 需要为所有技术都采用这种实践方式。

**清单 9-13.** 跨浏览器本地数据存储 API

```
// 存储对技术代码对象的引用，通过初始化方法设置
$.prototype.Storage.dataStore = null;

// 初始化方法选择用于本地数据存储的技术。它接受一个参数来指定页面上的 Flash 元素，
// 以便支持 Flash 共享对象。如果未提供此参数，则不会尝试使用 Flash 来存储数据
$.prototype.Storage.initialize = function(flashElement) {
    // 按优先顺序依次尝试
    if (window.localStorage) {
        // 如果本地存储 API 可用，将 dataStore 指向我们之前创建的本地存储 API 命名空间
        this.dataStore = $.Storage.LocalStorage;
    } else if (window.openDatabase) {
        // 如果支持客户端数据库 API，将 dataStore 分配给 DBStorage 命名空间并初始化
        this.dataStore = $.Storage.DBStorage;
        this.dataStore.initialize();
    } else if (window.globalStorage) {
        // 如果支持全局存储 API，将 dataStore 指向相应的命名空间
        this.dataStore = $.Storage.GlobalStorage;
        this.dataStore.initialize();
    } else if (window.ActiveXObject) {
        // 如果存在 Internet Explorer 的 userData 机制，则初始化该机制以供使用
        this.dataStore = $.Storage.UserData;
        this.dataStore.initialize();
    } else if (flashElement) {
        // 如果支持 Flash 8，则将 dataStore 设置为使用 Flash 共享对象
        this.dataStore = $.Storage.Flash;
        this.dataStore.initialize(flashElement);
    } else {
        // 如果所有方法都失败，则使用 Cookie
        this.dataStore = $.Storage.Cookies;
    }
};

// get 方法检索先前存储的值，并将其传递给
```


```javascript
// 指定的回调函数。由于在客户端数据库 API 中使用了回调技术，
// 我们必须在整个过程中都使用它

$.prototype.Storage.get = function(name, callback) {
  // 实际上，只有客户端数据库 API 这一个对象将回调参数
  // 作为数据检索过程的一部分。dataStore 中指定的其他命名空间
  // 会忽略该参数
  var value = this.dataStore.get(name, callback);

  // 如果返回了一个值（客户端数据库 API 不会返回值），
  // 则执行回调函数，并将找到的值传递给它。
  if (value && callback) {
    callback(value);
  }
};

// set 方法用于将值存储到指定的名称下
$.prototype.Storage.set = function(name, value) {
  // 所有不同的技术都接受相同的输入来设置数据，因此这个
  // 通用方法相当简单
  this.dataStore.set(name, value);
};

// remove 方法通过指定的名称永久删除数据
$.prototype.Storage.remove = function(name) {
  // 每种技术都接受相同的输入来删除数据
  this.dataStore.remove(name);
};
```

清单 9-14 展示了如何使用新的跨浏览器离线数据存储 API，在页面上的本地计算机或设备中存储、检索和删除数据。

**清单 9-14.** *使用跨浏览器 API 获取、设置和删除数据*

```javascript
// 执行代码，决定在当前浏览器中使用哪种技术。
// 在此情况下，没有向 initialize 方法提供参数，
// 这意味着不会使用 Flash 来存储数据
$.Storage.initialize();

// 使用先前选择的技术保存电子邮件地址
$.Storage.set({
  name: "email",
  value: "me@denodell.com"
});

// 输出之前保存的"email"值。
// 即使重启浏览器，数据仍会保留
$.Storage.get("email", function(value) {
  alert(value);
});

// 永久删除"email"数据值
$.Storage.remove("email");

// 由于"email"数据已不存在，输出空字符串
$.Storage.get("email", function(value) {
  alert(value);
});
```

正如在前几章中所见，从本地数据源存储和检索数据比从远程 Web 服务器访问数据更快。这就是为什么需要确保文件被缓存到浏览器中，从而只需下载一次。通过让 Web 应用程序将用户数据存储在本地数据存储中，并从那里访问数据，你可以在后台执行 Ajax 操作，并允许用户仅与数据存储中的预存数据进行交互，从而提升应用程序的感知响应速度。这样一来，用户就无需单击按钮通过 Ajax 提交表单，然后等待服务器响应才能进行 Web 应用程序的下一步操作。

清单 9-13 中的跨浏览器 API 可以实现在 Web 邮件客户端中，使正在撰写的新邮件能够按固定时间间隔自动保存，无需用户干预。

如果网络连接中断，用户可以继续撰写邮件，邮件内容会随着撰写过程被本地存储，直到服务器连接恢复。如果浏览器崩溃，邮件内容也能够恢复——用户不会丢失邮件内容（从而也不会失去耐心）。清单 9-15 展示了一个如何在 HTML 页面中实现此行为的简单示例。

**清单 9-15.** *将正在撰写的电子邮件保存到本地存储*

```html
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN"
  "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
<html lang="en" xml:lang="en" xmlns="http://www.w3.org/1999/xhtml">
<head>
  <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
  <title>离线电子邮件存储示例</title>
</head>
<body>
```



