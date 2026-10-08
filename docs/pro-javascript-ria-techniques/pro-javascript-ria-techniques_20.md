# 第 9 章：离线存储——当网络中断时

我们都经历过这种情况。你花了 20 分钟在 Web 邮件客户端中完善一封重要的电子邮件，终于准备发送了——然后砰的一声！——它发送不出去了。你再试一次，仍然不行。然后你意识到网络连接已断开，无法再访问互联网。你该怎么办？此时，我们中的许多人会尝试将文本复制粘贴到计算机本地存储的文档中，这样相对安全。如果必须重启浏览器，或者更糟的是系统崩溃，你应该能够找回它。

这是大多数 RIA 的问题所在：它们的可靠性完全取决于其所运行环境的网络连接和浏览器的可靠性。如果由于某种原因，浏览器和服务器之间的网络连接不可用，我们需要某种方式将重要信息存储在用户计算机或设备上。

# 本地存储方法

在本章中，我们将研究在不同浏览器中可用于本地存储内容的方法。你将看到如何在自己的 RIA 中有效组合使用这些方法来保存数据、回读数据和删除数据，并确保跨浏览器支持。

### 使用 Cookie 存储数据

实际上，自 Web 浏览器早期以来，开发人员就可以通过 cookie 的形式进行本地数据存储。cookie 是存储在用户计算机上并与特定网站域名相关联的小型文本文件，大小上限为 4KB。这增加了一层安全性，因为一个站点只能访问与其域名关联的 cookie。

在大多数 Web 浏览器中，cookie 可以由 Web 服务器设置，也可以通过 JavaScript 设置。它们通常用于存储小量数据，例如用户登录某个网站的 ID 或用户名，以便用户自动登录。

浏览器内部管理 cookie，并且通常通过其设置或偏好窗口提供方法来让用户查看和删除 cookie。用户也可以完全禁用 cookie。然而，实际上，由于如此多的网站依赖 cookie，包括所有需要用户登录的网站，完全禁用 cookie 的情况很少发生。

### 创建 Cookie

每个 cookie 在创建时都会被赋予一个名称，并且任何必要的文本值都存储在其中。当需要该 cookie 时，会根据其名称进行查找并返回值，前提是与该 cookie 关联的域名与执行脚本的网站域名匹配。

每个 cookie 都有一个`expiry`（过期）日期属性，用于定义 cookie 应存储多长时间。

在浏览器自动清除之前，该数据被允许存在。对于许多应用程序，Cookie 的过期时间会被设置为创建时间之后的很多年，这样数据就能保留在用户的机器上，而无需重新创建。

对于某些应用程序，可能需要将值设置为仅在用户访问网站期间有效。这种类型的 Cookie 被称为会话 Cookie，因为一旦浏览器会话结束，该 Cookie 就会被删除。如果没有为 Cookie 分配过期日期，则会创建一个会话 Cookie，这意味着它只会在当前浏览会话期间存在。将 Cookie 的过期日期设置为一个已经过去的日期，将会从浏览器中永久删除该 Cookie。

正如我之前提到的，你必须指定与 Cookie 关联的域名，以提供一定的安全性。托管在其他域名上的网站都不应该能够访问该 Cookie。除此之外，你还可以指定一个`path`值，那么该 Cookie 将仅在你的网站结构中该目录内有效。通常，这个值被设置为指向网站的根目录，以便所有页面都能访问该 Cookie。但是，你可能决定你的 Cookie 仅在你网站的某个特定部分相关；在这种情况下，你需要更改`path`值以指向该目录。

清单 9-1 展示了如何通过 JavaScript 设置 Cookie、读回其值以及删除它。

将代码添加到你的`$`JavaScript 库中（从第 2 章开始），放在实例化该库的文件的最后一行之前。

**清单 9-1.** 设置和读取 Cookie 值

```javascript
// 在 $ 库中为存储相关代码创建一个新的命名空间
$.prototype.Storage = {}

// 创建一个 Cookies 命名空间，用于存储与 Cookie 相关的存储方法
$.prototype.Storage.Cookies = {

  // set 方法在本地机器上设置一个具有给定名称和值的 Cookie
  set: function(input) {
    // 期望输入一个对象字面量，包含 name、value、expiry 和 path 属性
    var name = input.name || "";
    var value = input.value || "";

    // 如果提供了过期日期，则获取其字符串值用于设置 Cookie。
    // 如果没有提供过期日期，则默认设置为 10 年后
    var tenYearsAhead = new Date();
    tenYearsAhead.setFullYear(tenYearsAhead.getFullYear() + 10);

    // 使用输入的过期日期，或默认设置为未来 10 年的日期
    var expiry = (input.expiry ? input.expiry.toUTCString() :
      tenYearsAhead.toUTCString());

    // 如果没有给出路径，则默认设置为站点根目录
    var path = input.path || "/";

    // Cookie 以特殊格式的字符串进行设置。域名将自动分配给正在访问的站点的当前域名
    var cookieFormat = "{name}={value}; expires={expiry}; path={path}";

    // 通过将格式化字符串赋值给 document.cookie 来创建新 Cookie
    document.cookie = $.Utils.replaceText(cookieFormat, {
      // 使用 escape 方法确保非字母数字字符被编码，并且不会破坏生成的格式化 Cookie 字符串
      name: escape(name),
      value: escape(value),
      expiry: expiryDate,
      path: path
    });
  },

  // get 方法通过名称检索之前存储的 Cookie 值
  get: function(name) {
    // document.cookie 是一个字符串，自动包含当前正在访问的站点域和路径下所有有效的 Cookie
    // 使用针对 document.cookie 运行的正则表达式定位 Cookie
    var cookieFinder = new RegExp("(^|;) ?" + name + "=([^;]*)(;|$)");
    var cookie = document.cookie.match(cookieFinder);
    var value = "";

    if (cookie) {
      // 如果找到了 Cookie，则使用正则表达式找到的值
      value = unescape(cookie[2]);
    }

    return value;
  },

  // remove 方法通过名称删除现有的 Cookie
  remove: function(name) {
    // 通过将过期日期重置为当前时间之前的任何时间来删除 Cookie
    var expiryDate = new Date();
    // 将时钟回拨
    expiryDate.setTime(expiryDate.getTime() - 1);

    // 让先前定义的 set 方法重置 cookie 的过期时间，
    // 从而删除该 cookie
    this.set({
      name: escape(name),
      expiry: expiryDate
    });
  }
};
```

清单 9-2 演示了如何在页面自定义的 JavaScript 代码中使用清单 9-1 创建的方法，来创建、查找和删除 cookie。

**清单 9-2.** 使用`$`库创建、查找和删除 Cookie

```javascript
// 创建一个默认将在 10 年后过期的新 cookie
$.Storage.Cookies.set({
  name: "email",
  value: "me@denodell.com"
});

// 输出我们刚才创建的名为 "email" 的 cookie 的值。
// 即使浏览器重启，数据仍会保留
alert($.Storage.Cookies.get("email"));

// 删除 "email" cookie
$.Storage.Cookies.remove("email");

// 输出空字符串，因为该 cookie 已不存在
alert($.Storage.Cookies.get("email"));
```

### Cookie 的缺点

Cookie 提供了一种简单、跨浏览器的方式，可在浏览器中存储少量基于文本的数据以供后续检索。不过，它们也有缺点。

当浏览器请求位于特定域名下的页面或文件时，它也会将与该域名关联的完整 cookie 内容随请求一并发送。

这是设计使然，它允许网页服务器读取用户机器上存储的 cookie，并根据其值个性化响应内容。你在 cookie 中存储的数据越多，这些请求的体积就越大，直到达到 4KB 的最大限制。这可能听起来不多，但请记住，无论所请求文件的类型是什么，cookie 都会被发送。例如，如果你的页面由一个 HTML 文件、两个 CSS 文件、两个 JavaScript 文件和二十张图片组成，那么每个文件请求都会将 cookie 数据发送到服务器，这意味着额外发送给服务器的数据量最多可达 100KB（25 个请求 × 4KB cookie 大小）。

你可能会想，为什么这种机制会存在，因为它可能会导致大量数据无缘无故地被发送到服务器。不幸的是，它是目前唯一有保证的方法，以确保当服务器希望利用这些 cookie 数据来修改图片、脚本、HTML 页面或其他文件时，服务器能够获取到该 cookie。这绝非什么优雅的解决方案，但却是最简便的方法，也是 HTTP 规范在加入 cookie 支持时所采用的方式。

还要记住，许多用户使用的是异步连接，这意味着他们从浏览器到服务器的上传速度远低于从服务器到浏览器的下载速度。因此，传输这些额外 cookie 数据所花费的时间可能会对性能产生重大影响。在第 4 章中，我解释了一种解决此问题的变通方案，即将你的图片和外部资源托管在与 HTML 文件所在域名不同的第二个域名下。尽管如此，当你考虑到这里的主要目标是本地存储数据，以防不可预见的网络连接中断和不幸的浏览器崩溃时，每次 HTML 请求都发送 cookie 数据似乎还是有些多此一举。

### 使用 Internet Explorer 的数据存储

微软在其 IE 浏览器中（从版本 5 开始）引入了自己的方法，允许通过 JavaScript Web 应用将内容存储在用户机器上，这种方式使用了所谓的 `userData` 数据存储。此实现涉及在特定类型的 DOM 元素上使用属性节点来存储数据，通过添加和移除属性来添加和移除数据。数据通过在 DOM 元素上调用 `save()` 方法来持久化，并通过 `load()` 方法进行回读。这些元素是通过 DOM 元素关联的、IE 特有的 `userData` 行为添加的。数据存储本身在 IE 内部实际上是以 XML 为基础结构表示的。

微软对您站点中每个页面施加了 64KB 的基本存储限制，且每个域最多可存储 640KB 数据。其安全限制与 Cookie 存储类似：任何域都无法访问其他域的数据，并且可以通过在数据存储中添加可选的过期时间来在指定时间后移除数据。然而，与 Cookie 不同，过期时间适用于整个数据存储，而非单条数据。此外，不存在仅持续当前会话时长的数据存储概念。依我之见，应忽略过期时间选项，让浏览器或用户手动删除数据。

清单 9-3 展示了如何在 IE 中使用 `userData` 机制来获取、设置和删除数据。

请将此代码添加到您的 `$` JavaScript 库中，位于文件末尾库被实例化之前的位置。

**清单 9-3.** *使用 Internet Explorer 的 `userData` 机制设置、获取和删除数据*

```javascript
// 添加一个 UserData 命名空间，用于存放微软 userData 特有的方法

$.prototype.Storage.UserData = {

    // 引用一个用于存储数据的元素

    storageElement:null,

    // 通过使用不同的名称，数据可以存储在不同的数据存储中。我们
    // 希望所有数据都在同一个地方，因此选择一个名称并固定使用它
    dataStore: "data- store",

    // 在使用任何数据之前，我们需要初始化 DOM 元素
    initialize: function() {
        // 数据存储在 DOM 元素内，因此我们来创建一个以供使用
        this.storageElement = $.Elements.create("span");

        // behavior 属性使 DOM 元素能够加载
        // 数据并将其保存到数据存储中
        this.storageElement.addBehavior('#default#userdata');

        // 我们不希望此元素在页面上可见，因此将其隐藏
        this.storageElement.style.display = 'none';

        // 将新的 DOM 元素添加到页面末尾
        document.body.appendChild(this.storageElement);

        // 从数据存储中加载任何先前存储的数据，将数据填充到
        // 元素的属性中
        this.storageElement.load(this.dataStore);
    },

    // set 方法将给定名称的数据值保存到数据存储中
    set: function(input) {
        // 期望输入为一个对象字面量，包含 name 和 value
        var name = input.name || "";
        var value = input.value || "";

        // 将数据名称和值保存到 DOM 元素
        this.storageElement.setAttribute(name, value);

        // 将 DOM 元素中的当前数据提交到数据存储中
        this.storageElement.save(this.dataStore);
    },

    // get 方法从数据存储中返回给定属性名称的先前存储的值
    get: function(name) {
        // 返回给定名称的属性值，如果不存在则返回空字符串
        return this.storageElement.getAttribute(name) || "";
    },

    // remove 方法从数据存储中永久删除数据名称及其关联值
    remove: function(name) {
        // 从用于存储数据的 DOM 元素中移除给定名称的属性
        this.storageElement.removeAttribute(name);

        // 提交对数据存储所做的更改，以便指定数据被
        // 永久删除
        this.storageElement.save(this.dataStore);
    }
}
```

清单 9-4 展示了如何使用 JavaScript 以及在清单 9-3 中创建的方法在您自己的页面中创建、查找和删除数据。

**清单 9-4.** *在 Internet Explorer 中创建、查找和删除 `userData`*

```javascript
// 初始化 userData 存储
$.Storage.UserData.initialize();

// 将电子邮件地址保存到数据存储中
$.Storage.UserData.set({
    name: "email",
    value: "me@denodell.com"
});

// 输出我们之前创建的 "email" 值。
// 浏览器重启后，数据仍会存在
alert($.Storage.UserData.get("email"));

// 从数据存储中永久删除 "email" 数据值
$.Storage.UserData.remove("email");

// 由于数据已不存在，因此输出空字符串
alert($.Storage.UserData.get("email"));
```

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

# SQLite 离线数据存储

有关 SQLite 支持的 SQL 命令的完整列表，请参阅 [sqlite.org/lang.html](http://www.sqlite.org/lang.html)。

通过使用数据库来存储离线数据，开发者可以在数据库表中表示数据结构，而不是通过文本字符串——而文本字符串是目前唯一跨浏览器可行的数据存储选项。数据库还支持存储二进制数据（例如图像文件），为可能需要存储和随时检索任意类型数据的 Web 应用提供了更灵活的解决方案。

要让所有浏览器都支持通过 JavaScript 访问本地数据库这一标准，还需要一些时间。目前，该 API 和数据库系统仅在桌面版 Safari 3.1 以及 iPhone 上的 Safari 浏览器中可用。当规范更加完善后，其他浏览器也会跟进。

## 第 9 章 ■ 离线存储——当灯光熄灭时

出于安全考虑，每个数据库都被锁定到创建它的域名。每个站点可以根据需要创建任意数量的数据库，并包含任意数量的表和行。目前，在 Safari 3.1 中，每个域名的默认存储空间为 5MB。通过浏览器偏好设置，用户可以为每个域名或全局（适用于所有站点）选择本地数据库大小，最小 1MB，最大 500MB。

作为性能优化措施，对数据库执行的事务和操作使用 SQL 语言编写，并且异步执行。命令执行完成后，会传递一个回调函数来执行。在此期间，其余代码可以继续执行，这意味着大型复杂的数据库事务不会阻止浏览器和 Web 应用正常响应用户的需求。

`Listing 9-9` 演示了如何使用客户端数据库存储 API 在本地机器的 SQLite 数据库中获取、设置和删除文本值。将此代码添加到你的 JavaScript 库中，在库实例化之前。请注意，与客户端数据库存储 API 所能实现的所有功能相比，这只是一个相当有限的用法。

**`Listing 9-9`.** *使用数据库存储 API 获取、设置和删除数据*

```javascript
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

            var command = "INSERT INTO {tableName} (name, value) VALUES ({name}, {value})";

            // 如果数据库中已存在该名称对应的值，则改为执行 SQL 更新命令

            if (value != "") {

                command = "UPDATE {tableName} SET value = {value} WHERE name = {name}";

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

`Listing 9-10` 展示了如何在页面中利用 JavaScript，通过位于用户本地计算机或存储设备上的 SQLite 数据库来存储、检索和删除文本值。

请注意，在检索数据时，你需要使用回调函数。由于数据库是异步操作的，要获知数据何时已被定位，唯一的方法就是告诉该方法在数据找到后应该执行哪些代码。

`清单 9-10.` `使用数据库存储 API 设置、获取和删除数据`

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

如果你的最终用户已安装 Adobe Flash Player 插件（版本 8 或更高），则可以使用另一种数据存储技术。通过一个特别构建的影片文件，你可以通过简单的 API 向 JavaScript 公开所谓的`Flash 本地共享对象`（通常称为`Flash cookie`），以便 JavaScript 在本地存储数据。

- 与浏览器 cookie 不同，本地共享对象（local shared objects）允许每个域在用户机器上存储 100KB 的数据，并且这些值不会随 HTTP 请求发送到服务器。用户可以更改此存储量，尽管许多人选择保留默认设置。正如您所料，作为一种安全措施，从一个域设置的数据无法被任何其他域访问。

我已经构建了必要的 Flash 影片文件，该文件可从本书的详细信息页面（位于 `Apress` 网站（`http://www.apress.com/`））以及本书其余源代码一起获取，也可从我自己的网站（`http://www.denodell.com/`）下载，以供您在自己的 RIA 中使用。请使用第 7 章介绍的 `SWFObject` 组件将 Flash 影片放置在您的页面上。

该 Flash 影片文件公开了三个方法，这些方法可以针对页面上的影片实例进行调用：`get()`、`set()` 和 `remove()`。清单 9-11 显示了要添加到您的 `$` JavaScript 库中以使用 Flash cookie 的代码。将代码添加到 `$` 库文件的末尾，在其实例化之前。

`清单 9-11.` `使用 Flash 本地共享对象获取、设置和删除数据`

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

`清单 9-12.` `使用 Flash 本地共享对象存储、检索和删除数据`

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

- `本地存储 API`：推荐使用，因为它已成为既定标准，使用简单，且在最新浏览器版本中获得了相当广泛的支持。默认提供 5MB 存储空间。

- `客户端数据库存储 API`：一项新兴标准，由于其通用性，若非目前规范仍存在较大变动风险，本应是最优选择。默认提供 5MB 存储空间。

- `Mozilla 全局存储 API`：对于 Firefox 2 和 3 用户来说是一个不错的备选方案，这些用户的浏览器尚不支持本地存储 API，但支持这个非常相似的 API。默认提供 5MB 存储空间。

- `IE 的 userData 机制`：IE 5 至 7 版本不支持上述任何推荐方法，但提供了这种无需任何插件即可在本地存储数据的机制。默认提供 64KB 存储空间。

- `Flash 本地共享对象`：对于浏览器不支持任何更优方法的用户，可以利用无处不在的 Flash Player 插件在本地存储数据，而无需依赖 Cookie。默认提供 100KB 存储空间。

- `Cookie`：绝对是最不推荐的选择。Cookie 文件空间极其有限，而且其数据会随每次 HTTP 请求发送到服务器，Cookie 越大，对连接的阻塞就越严重。默认提供 4KB 存储空间。

根据这些信息，你可以创建三个通用方法——`get()`、`set()`和`remove()`——它们会优先选择最优技术，当首选技术不可用时，则会一直降级到简单的 Cookie。清单 9-13 展示了一个跨浏览器的本地存储 API，你可以使用本章给出的代码将其实现到你的`$` JavaScript 库中。请注意，由于其中一种技术（客户端数据库 API）在检索数据时需要提供回调函数，因此本 API 需要为所有技术都采用这种实践方式。

`清单 9-13.` `跨浏览器本地数据存储 API`

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

## 清单 9-14. 使用跨浏览器 API 获取、设置和删除数据

清单 9-14 展示了如何使用新的跨浏览器离线数据存储 API，在页面上的本地计算机或设备中存储、检索和删除数据。

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

清单 9-14 中的跨浏览器 API 可以实现在 Web 邮件客户端中，使正在撰写的新邮件能够按固定时间间隔自动保存，无需用户干预。

如果网络连接中断，用户可以继续撰写邮件，邮件内容会随着撰写过程被本地存储，直到服务器连接恢复。如果浏览器崩溃，邮件内容也能够恢复——用户不会丢失邮件内容（从而也不会失去耐心）。清单 9-15 展示了一个如何在 HTML 页面中实现此行为的简单示例。

## 清单 9-15. 将正在撰写的电子邮件保存到本地存储

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

# 离线电子邮件存储示例

<!-- 创建用于撰写电子邮件的表单 -->

```html
<form method="post" action="/" id="compose-message">
  <div>
    <label for="to">收件人</label>
    <input type="text" name="to" id="to" />
  </div>
  <div>
    <label for="subject">主题</label>
    <input type="text" name="subject" id="subject" />
  </div>
  <div>
    <label for="message-body">正文</label>
    <textarea id="message-body" name="message-body" rows="10" cols="20"></textarea>
  </div>
  <div>
    <input type="submit" value="保存" />
  </div>
</form>
```

<!-- 创建一个元素，用于后续放置 Flash 存储组件 -->

```html
<div id="flash-storage-element"></div>
```

<!-- 引用 $ JavaScript 库 -->

```html
<script type="text/javascript" src="$.js"></script>
```

<!-- 从 Google CDN 引用 SWFObject -->

```html
<script type="text/javascript" src="http://ajax.googleapis.com/ajax/libs/swfobject/2.1/swfobject.js"></script>
```

<!-- 以下代码以内联方式呈现以保持简洁。在实际的 Web 应用中，应将其存储在外部文件中 -->

```html
<script type="text/javascript">
$.onDomReady(function() {
    // 查找此 Flash 离线存储组件，在线地址为
    // http://www.denodell.com/
    var movieURL = "storage.swf";
    var tagIDToReplace = "flash-storage-element";
    var width = 1;
    var height = 1;
    var flashVersionRequired = "8";
    var parameters = {
        allowScriptAccess: "always"
    };
    var attributes = {
        id: "flash-storage"
    };
    swfobject.embedSWF(movieURL, tagIDToReplace, width, height,
        flashVersionRequired, null, {}, parameters, attributes);

    // 初始化离线数据存储 API，传递页面上 Flash 元素的引用
    // 以允许使用 Flash 进行存储
    $.Storage.initialize(document.getElementById("flash-storage"));

    // 使用上次保存的值填充消息字段，以便当用户在浏览器崩溃后
    // 返回页面时，字段会自动预填上次保存的值
    $.Storage.get("to", function(value) {
        document.getElementById("to").value = value;
    });
    $.Storage.get("subject", function(value) {
        document.getElementById("subject").value = value;
    });
    $.Storage.get("message-body", function(value) {
        document.getElementById("message-body").value = value;
    });

    // 每 30 秒保存一次正在撰写的邮件内容
    var THIRTY_SECONDS = 30000; // 30 秒 = 30000 毫秒
    var saveMessage = window.setInterval(function() {
        // 保存收件人、主题和正文输入字段的值
        // 以防网络断开或浏览器崩溃
        $.Storage.set({
            name: "to",
            value: document.getElementById("to").value
        });
        $.Storage.set({
            name: "subject",
            value: document.getElementById("subject").value
        });
        $.Storage.set({
            name: "message-body",
            value: document.getElementById("message-body").value
        });
    }, THIRTY_SECONDS);

    // 当用户提交表单以将消息保存到服务器时，
    // 移除先前存储的字段值，并停止在表单提交过程中继续保存
    $.Events.add(document.getElementById("compose-message"),
        "submit", function(e) {
            // 阻止 HTML 页面提交表单，以便通过 Ajax 处理数据提交
            e.preventDefault();

            // 停止每 30 秒保存消息字段值
            window.clearInterval(saveMessage);

            // TODO: 使用 Ajax 将表单字段值发送到服务器

            // 消息已发送，移除本地数据
            $.Storage.remove("to");
            $.Storage.remove("subject");
            $.Storage.remove("message-body");

            // TODO: 更改页面以显示邮箱文件夹内容
        });
});
</script>
```

### 使用可复用的离线存储组件

# PersistJS

`PersistJS` [(http://pablotron.org/software/persist-js/)](http://pablotron.org/software/persist-js/) 是一个开源的本地数据存储库，它持续更新以支持更新的浏览器和技术。除了支持我们在本章中探讨的技术外，该库还支持 `Gears`。

`Gears` 是谷歌开发的一个浏览器插件，它提供了一个 `SQLite` 数据库用于离线数据存储。它还提供了一种方法，可以将密集的 JavaScript 例程卸载到最终用户机器上的独立工作进程中，以提升性能。该插件可从 [`gears.google.com/`](http://gears.google.com/) 下载，并支持多种[浏览器](http://gears.google.com/)。它默认内置于谷歌的 Chrome 浏览器中。

## 总结

本章涵盖了通过 Web 应用程序在用户机器上存储数据的不同技术。它提供了一个跨浏览器的 API，用于在本地数据存储中存储、检索和删除数据。使用本地数据存储可以提供更好的用户体验，因为如果网络连接中断或浏览器软件崩溃，数据仍然是安全的。数据可以留在用户计算机上，直到可以发送到服务器，然后被删除。

在下一章中，我们将探讨一些高级 Ajax 技术，用于从存储在 Web 服务器上的非文本文件中提取隐藏信息。
