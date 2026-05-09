# 添加按钮：浏览器操作

现在，你需要为 Google Chrome 工具栏中可点击的按钮添加代码。这也被称为*浏览器操作*按钮，或简称为浏览器操作。为此，你需要在清单中添加另一个名为 `browser_action` 的属性。`browser_action` 属性接受一个对象值（即 `{}`），该对象由以下（字符串）键组成：`default_title`、`default_icon` 和 `default_popup`。

如清单 1-1 所示，每个键都接受一个字符串值。`default_title` 键代表浏览器操作的（字符串）工具提示。`default_icon` 键代表要用作图标的 PNG 图像资源的（相对）路径。同样地，`default_popup` 代表要用作弹窗（视图）的 HTML 文件的（相对）路径。

**清单 1-1.** 第 1 章 /ShowTime/manifest.json

```
{
  "manifest_version": 2,
  "name": "ShowTime",
  "description": "显示当前时间和日期的扩展程序",
  "version": "1.2",
  "browser_action": {
    "default_title": "ShowTime",
```


