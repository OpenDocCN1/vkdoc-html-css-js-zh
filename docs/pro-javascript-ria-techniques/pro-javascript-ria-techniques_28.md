# Page Performance

## Viewing the Performance of a Page

## Identifying Potential Bottlenecks in Performance

### `Engine Horsepower`

### `User’s Upload Speeds`

## `Parallel Connection Limits`

### `Choice of Image Format`

### `Unnecessary Content`

### `Cache Settings`

### `Uncompressed Text Files and Extra Whitespace`

## `Summary`

## `Performance Tweaking`

### `Is Performance Really an Issue?`

### `Tweaking Your Web Server for Performance`

#### `Use Separate Domain Names for External Assets`

#### `Use a Content Delivery Network`

#### `Send HTML to the Browser in Chunks`

##### `Flushing the Buffer with PHP`

##### `Flushing the Buffer with ASP.Net/C#`

##### `Flushing the Buffer with JSP`

#### `Customize HTTP Headers to Force Browser Caching`

#### `Compress the Output from the Server`

##### `Enabling Compression in Apache Web Server`

##### `Enabling Compression in IIS`

##### `Enabling Compression in Tomcat`

### `Tweaking HTML for Performance`

#### `Shrink Your HTML File Size with HTML Tidy`

#### `Reference JavaScript Files at the End of Your HTML`

#### `Reduce the Number of HTTP Requests`

##### `Good Division and Structure`

##### `Combine Files`

#### `Don’t Load Every Asset from Your Home Page`

#### `Reduce Domain Name Lookups`

#### `Split Components Across Domains`

#### `Avoid Linking to Redirects`

#### `Reduce the Number of HTML Elements`

#### `Don’t Link to Nonexistent Files`

#### `Reduce the Size of HTTP Cookies`

### `Tweaking Your Style Sheets for Performance`

#### `Shrink Your CSS File Size with CSSTidy`

#### `Don’t Use the @import Command`

#### `Speed Up Table Layouts`

#### `Avoid CSS Filters and Expressions in IE`

#### `Use Shorthand Values`

##### `Colors`

##### `Margins and Padding`

##### `Borders`

##### `Backgrounds`

##### `Fonts`

##### `Lists`

#### `Use the CSS Sprite Technique`

#### `Avoid Inefficient CSS Selectors`

### `Tweaking Your Images for Performance`

#### `Understand Image File Formats`

##### `GIF Format`

##### `JPEG Format`

##### `PNG Format`

#### `Optimize PNG Images`

#### `Don’t Forget the Favicon`

### `Tweaking Your JavaScript for Performance`

#### `Shrink Your JavaScript File Using Dojo ShrinkSafe`

# 目录

## 通过 CDN 访问 JavaScript 库

## 时机至关重要

## 提升核心 JavaScript 性能

-   使用记忆化函数
-   使用高效的字符串拼接
-   使用正则表达式
-   更快速、高效地循环
-   即将推出：后台 Web Worker 进程

## 提升 Ajax 性能

-   使用 JSON 格式的响应
-   结合`<script>`标签使用 JSONp
-   合并服务器请求与响应

## 提升 DOM 性能

-   最小化 DOM 访问
-   使用 DocumentFragment 对象
-   堵塞内存泄漏
-   使用事件委托
-   更新 CSS 时更改类，而非样式
-   复制现有节点而非创建新节点
-   以 HTML 字符串形式将元素追加到页面

## 总结
