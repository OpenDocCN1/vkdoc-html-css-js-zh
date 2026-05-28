# 第 9 章 ■ 离线存储——当灯火熄灭时

`userData`技术提供了一种实用机制，用于离线存储比使用 Cookie 更大容量的文本数据。与 Cookie 不同，这些数据不会随服务器请求传递，从而实现了永久数据存储的优势且无需额外开销。IE 5 及以上版本支持该技术，不过 IE 8 现已支持一种基于 WHATWG 本地存储 API 的新技术，可提供更大的存储潜力。

## 数据存储 API 介绍

如第 7 章所述，WHATWG 组织一直积极推动 HTML 5 标准的制定——这是与富互联网应用构建相关的语言更新版本。该标准的部分功能已开始被最新浏览器版本支持。我们关注的是其中一组用于实现离线存储的 API，可通过 JavaScript 访问。接下来逐一介绍各数据存储 API。

### 本地存储 API

第一个 API 称为本地存储。默认情况下，多数浏览器为各站点分配 5MB 的本地存储空间，不过用户可在各支持浏览器的设置中调整此配置。这比使用 Cookie 或 IE 特有的`userData`机制可用的存储空间要大得多。与这两类方法相同，本地存储 API 会将存储数据与当前网站运行的域名关联，为保存数据提供必需的安全保障。

使用该 API 时，无法像 Cookie 那样限制同一网站不同目录下的数据访问权限，更重要的是，无法为数据设置过期时间。这意味着数据存续期将由浏览器及其支持的用户设置控制，最终用户可据此管理以自己名义保存的数据。与目前讨论的所有方法一样，数据仅能以字符串形式存储。

截至撰写本文时，IE 8 及以上版本、Safari 3.1 及以上版本、Google Chrome 2 及以上版本、Firefox 3.5 及以上版本均支持本地存储 API。

清单 9-5 展示了在 JavaScript 库中实现本地存储 API 的简洁性。请在文件末尾实例化库之前，将这段代码添加到库中。

**清单 9-5.** *使用本地存储 API 设置、获取和删除数据*

```
// 添加 LocalStorage 命名空间以集中管理本地存储 API 代码
$.prototype.Storage.LocalStorage = {

  // set 方法通过本地存储 API 存储指定名称的值
  set: function(input) {

    // 期望输入对象字面量，包含要设置的名称和值
    var name = input.name || "";
    var value = input.value || "";

    // 使用顶层 localStorage 对象保存数据
    localStorage.setItem(name, value);
  },

  // get 方法通过名称检索之前存储的值
  get: function(name) {

    // 如果请求的项目不存在则返回空字符串；否则
    // 从 localStorage 对象获取值
    return localStorage.getItem(name) || "";
  },

  // remove 方法从 localStorage 对象中删除之前存储的值
  remove: function(name) {

    // 从 localStorage 中移除项目
    localStorage.removeItem(name);
  }
}
```

清单 9-6 展示了如何在页面 JavaScript 中使用清单 9-5 定义的方法，在本地计算机上存储、检索和删除数据。

**清单 9-6.** *使用本地存储 API 存储、检索和移除数据*

```
// 保存电子邮件地址
$.Storage.LocalStorage.set({
  name: "email",
  value: "me@denodell.com"
});

// 输出之前保存的"email"值
// 浏览器重启后数据仍然存在
alert($.Storage.LocalStorage.get("email"));

// 永久删除"email"数据值
$.Storage.LocalStorage.remove("email");

// 由于"email"数据已不存在，输出空字符串
```

`alert($.Storage.LocalStorage.get("email"));`

### Mozilla 全局存储 API

在 WHATWG 定义本地存储 API 之前，Mozilla 开发者为了实现相同目的——在用户本地计算机或设备上存储数据，曾自行实现了一个类似但明显不同的存储 API。他们将其称为 *DOM 存储*或*全局存储*，该 API 提供的方法在存储、检索和删除数据的方式上与本地存储 API 几乎相同。其默认数据大小限制同样是 5MB。该技术与本地存储 API 的唯一实现差异在于：全局存储对象必须指定数据的存储域，尽管实际上该域必须与当前代码运行的域相同。

**316**

第 9 章■离线存储——当灯光熄灭时

`代码清单 9-7` 演示了如何使用 Mozilla 全局存储 API 在本地计算机或设备上存储、检索和删除数据。请将此代码添加到您的 `$` JavaScript 库中（位于库文件末尾实例化之前的位置）。

**代码清单 9-7.** *使用全局存储 API 设置、获取和删除数据*

```
// 添加 GlobalStorage 命名空间以包含所有全局存储 API 相关方法

$.prototype.Storage.GlobalStorage = {

  dataStore: null,

  // 若浏览器支持全局存储 API，initialize 方法用于定位要使用的数据存储

  initialize: function() {

    // 数据存储本身是 globalStorage 数组的一个索引，该索引始终为当前站点域名

    if (globalStorage) {

      this.dataStore = globalStorage[location.host];

    }

  },

  // set 方法将给定名称的值存储到全局存储 API 中

  set: function(input) {

    // 期望输入为对象字面量，包含要设置的 name 和 value

    var name = input.name || "";

    var value = input.value || "";

    // 使用 globalStorage 提供的数据存储保存数据

    this.dataStore.setItem(name, value);

  },

  // get 方法通过名称检索之前存储的值

  get: function(name) {

    // 若请求的项目不存在则返回空字符串；否则从数据存储中定位

    return this.dataStore.getItem(name) || "";

  },

  // remove 方法通过名称从数据存储中永久删除之前存储的值

  remove: function(name) {

    // 使用全局存储 API 的 removeItem 方法从数据存储中删除项目

    this.dataStore.removeItem(name);

  }

}
```

**317**

第 9 章■离线存储——当灯光熄灭时

`代码清单 9-8` 演示了如何在您自己的页面中通过 JavaScript 使用这些方法，通过全局存储 API 在本地计算机或设备上存储、检索和删除数据。

**代码清单 9-8.** *使用全局存储 API 存储、检索和删除数据*

```
// 初始化 GlobalStorage 以供使用

$.Storage.GlobalStorage.initialize();

// 保存一个电子邮件地址

$.Storage.GlobalStorage.set({

  name: "email",

  value: "me@denodell.com"

});

// 输出我们之前创建的 "email" 值。
// 即使浏览器重启，数据依然存在

alert($.Storage.GlobalStorage.get("email"));

// 永久删除 "email" 数据值

$.Storage.GlobalStorage.remove("email");

// 由于 "email" 数据已不存在，输出空字符串

alert($.Storage.GlobalStorage.get("email"));
```

### 客户端数据库存储 API

新兴的 HTML 5 推荐标准中包含第二个存储实现 API，其功能比我们之前探讨的本地存储 API 更加完善。客户端数据库存储 API 允许 Web 开发者在浏览器内实现的 SQLite 数据库中存储数据。

**注意** SQLite 是一个开源软件库，实现了一个无需服务器或任何预配置即可使用的数据库。使用标准结构化查询语言 (SQL) 命令与数据库通信，根据需要保存和检索数据。请参阅项目网站 (http://www.sqlite.org/lang.html)。