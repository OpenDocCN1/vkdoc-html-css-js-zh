## 索引

### A

Android Virtual Device Manager（安卓虚拟设备管理器），198，199

`apply()` 函数，61

`axis()` 函数，55

### B

平衡性能

实际应用

监控 Web 性能，192

分享发现，201–202

站点检测，192–193

测试实验室基准测试（*参见* 测试实验室基准测试）可读性、模块化和设计，175–176

“焦土式”性能优化（参见 “焦土式”性能优化）

### C

Carakan，6

Chakra，7

`comparePerfMetricsbyURL()` 函数，126

条件编译，7

`constructScriptTag()` 函数，112，113，124

`createDataFrameByURL()` 函数，54，61

### D, E

数据可视化，9，10

`document.createElement()` 函数，112

### F

Firebug

过滤结果，15，16

浏览器内工具，13

安装，13

网络监控，15

瀑布图，15

Firefox 内存窗口，98

### G, H, I

垃圾回收，97

Gecko，6

`getDFByBrowser()` 函数，145

### J, K

JavaScript 基准测试与日志记录，65

`appendToFile()` 函数，77

# 架构

- 临时代码，67
- 关联数组，67
- 代码执行，65
- 决策树，66
- 高层序列图，67
- 元数据，68
- 对象图，68
- `perfLogger`，67
- `savePerfData` 流程，67
- `stopTimeLogging()` 函数，67
- `testresult` 对象，68
- 工作流，运行时计算，65，66

`by()` 函数，80
`calculateResults()` 函数，69
`cleanCommas()` 函数，77
完整 R 脚本，82
数据框，80，81
数据保存，70
调试信息，69
`formatDebugInfo()` 函数，70
`formatNewLog()` 函数，78
开源，82
`page_render`，79，81
公共 API 打造完成的库函数，73–75
- `logBenchmark()` 函数，72
- 传入的属性，71
- `startTimeLogging()` 函数，71
- `stopTimeLogging()` 函数，71
远程日志记录，75
`runtimePerformance.R` 文档，80
`runtimeperf_results.txt` 文件，79
`saveLog()` 函数，76
`savePerfData.php` 文件，78
自执行函数，68
`setResultsMetaData()` 方法，69
`JavaScriptCore`，6
JQuery *vs*. Javascript 编写时效率，151

## DOM 访问

- 基准测试结果，160
- 基准测试计时数据，159
- `JQueryDOM` 函数，158，160
- `JSDOM` 函数，158
- `populateArray` 函数，157–158

## 循环

- 平均基准测试结果图表，154，155
- 创建变量，153
- 数据框创建，153
- DRY 原则概念，155–157
- `jquerycomparison.html` 页面，151
- `JQueryEach` 函数，152
- `JSForLoop` 函数，152，153
- `populateArray` 函数，151–152
- R 代码，154

# L

## 懒加载

- 反模式，122
- 核心算法模式，122

### CSS

- 平均加载时间，133
- 平均页面加载时间（使用 `perfLogger`），135
- 平均页面渲染时间，134
- 平均页面渲染时间（使用 `perfLogger`），135
- `constructTag` 函数，129–130
- `fetch` 函数，129
- `lazyloadcss.html`，129
- `processURLs` 函数，130–131
- WebPagetest 摘要结果，132
- WebPagetest 瀑布视图，132
- 定义，121
- 图片，136
- 实现，121
- 惰性初始化模式，122

### 脚本

- 平均加载时间，127
- 平均渲染时间，126
- 回调函数，123–124
- `comparePerfMetricsbyURL()` 函数，125
- `lazyloadscript.html`，123
- `meanRenderTimes` 和 `meanLeadTimes` 数据框，125–126
- `perfLogger.showPerformanceMetrics()` 函数，123，125
- `remoteLoader.loadJS` 函数，123
- WebPagetest 摘要结果，128
- WebPagetest 瀑布视图，128
- `window.addEventListener()`，123
- `window.attachEvent` 函数，123
- 序列图，121
- 值持有者模式，122
- 虚拟代理模式，122

`legend()` 函数，58
`line()` 函数，56
`loadtime` 列，57–58

# M, N, O

## Meenan, Patrick (WebPagetest)

- 代理架构，63–64
- 应用场景，64
- 平均测试时间，62
- 回调方法，63
- 挑战，62
- 创建灵感，61
- WebPagetest LLC 的目标，62
- HTTP 归档，63
- 内存分析器，64
- 开源项目，62
- 测试流程，63
- 测试结果存储时间，62

## 压缩

- 分析与可视化，27，28
- Closure Compiler，24–27
- 定义，23
- Minify，23–24，26
- 瀑布图，26
- YUI 压缩器，24

# P, Q

## 页面渲染瓶颈

- 带注解的浏览器架构，109, 110
- `async` 属性，114, 115
- 瀑布图，117, 118
- WebPagetest 结果，115, 116
- 平均加载计时器，119, 120
- 平均渲染时间，119, 120
- 基准文件，114
- 瀑布图，117
- WebPagetest 结果，115, 116
- 面向客户端的接口，109
- `comparePerfMetricsbyURL`，118
- 数据帧，119
- DOM 元素，110
- `getDFByURL()` 函数，118
- 网络层，109–110
- `perfLogger` 指标，118
- 渲染引擎工作流程，110, 111
- R 函数，118
- 脚本加载
  - 数组，113
  - `constructScriptTag()` 函数，112, 113
  - `document.createElement()` 函数，112
  - `instanceof` 操作符，113
  - `loadJS()` 函数，112
  - `remoteLoader` 函数，112–114
  - `src` 属性，112
  - 瀑布图，117
  - WebPagetest 结果，116
- 脚本标签，解析，111
- 分词，111
- UI 层，109
- 解析与渲染
  - 浏览器架构，4, 5
  - JavaScript 引擎，6–7
  - 渲染引擎，6
- `paste()` 函数，53
- Perflogger
  - `jsonConcat()` 函数，89, 90
  - `logToServer()`，89–91
  - 公有函数，90–91
  - 自执行函数，88
  - `TestResults` 对象，88, 89
  - 更新，90
- `perfLogger` 库，109
- `plot()` 函数，55, 58
- `PlotResultsofTestsByBrowser` 函数，156
- Presto，6

# R

## `R`

- `apply()` 函数，41
- `barplot` 函数，38
- 图表绘制，34–37
- 数据帧，38
- `dev.off()`，39
- `getPercentImproved` 函数，40–41
- 图形参数，38
- 安装与运行，29–30
- `mincompare`，38
- `opar` 变量，38
- `par()` 函数，38–39
- 传入向量，40
- `percentVector`，40
- 入门
  - 缺陷积压，33
  - 函数，33
  - 循环，33
  - `read.table()` 函数，32
  - 变量与数据类型，31–32
- 统计计算，28
- 工作流程，28

`rel` 属性，136
R 语言，11
运行时性能，139

## 缓存变量与属性

- 临时计时数据捕获，141, 144
- 基准测试结果，145–147, 150
- `cache_locationcomparison.html`，140, 143
- `cacheLoc` 函数，143
- 数据帧，150
- 文档位置，140
- 全局作用域，140
- 局部变量创建，140
- 循环终止条件，147, 148
- `perfLogger.logBenchmark`，142
- `perfLogger.startTimeLogging`，148
- `perfLogger.stopTimeLogging`，141
- `populateArray`，141
- `tempArray`，141
- 测试输出，142
- 测试结果，149
- `uncachedLoc()` 函数，142, 145

## DOM 访问

- 节点添加，队列，166–168
- 队列变化，DOM 元素，164–166

## S

- `eval` 的真实代价，161
  - 基准测试函数，162, 163
  - 代码注入，161
  - `evalAverage()` 函数，162
  - `getAvg` 函数，161
  - HTML 骨架结构，161
  - 解释器，161
  - `perfLogger.logBenchmark`，162
- JQuery `vs.` Javascript（参见 JQuery `vs.` Javascript）
- 嵌套循环基准测试函数结果，170
  - 代码执行，168
  - 递增深度图表，171–173
  - 脚本标签与 `tempArray` 变量创建，168
  - 慢脚本警告，169–170
- 数字，139

### 焦土性能

- Closure 编译器
  - `Advanced` 模式, 183, 191
  - `basic skeletal HTML structure`, 183
  - `benchmarkobjects.html`, 183
  - `completed page`, 184–185
  - `debugging`, 191
  - `favoriteList`, 184
  - `JavaScript error`, 187
  - `printInfo()`, 183
  - `running perfLogger`, 187
  - `Simple` 模式, 183
  - `test file`, 187–190
  - `testUserObject` 函数, 184
  - `UI`, 185, 186
  - `URLs` 测试, 190
  - `user object`, 183
  - `video` 函数, 183
  - `web performance results`, 191

### `内联函数`

- `basic skeletal HTML structure`, 177
- `coalescing functionality`, 182
- `getAvg()` 函数, 178
- `getSum()` 函数, 178
- `PlotResultsofTestsByBrowser()` 函数, 181
- `simpleMath` 函数, 178
- `test page`, 179–181
- `unwoundfunction()` 函数, 177
- `usingfunctions()` 函数, 178
- `usingobjects` 函数, 179

### `会话/网站放弃`, 8

### `SpiderMonkey`, 6

### `src` 属性, 136

### `statusCode` 节点, 48

## T

### 测试实验室基准

- `Android SDK`
  - `Device Manager`, 198–200
  - `download page`, 196, 197
  - `emulator`, 200, 201
  - `Manager`, 197, 198
- `browser support matrix`, 193, 194
- `iOS simulator`, 196
- `Keynote Device Anywhere`, 196
- `Virtual Box` 下载页面, 195
- `homepage`, 194, 195
- `with multiple VMs`, 195, 196

### `线程`, 3

### `传输控制协议 (TCP)`, 3

### `Trident`, 6

## U, V

### `useAppendChild` 函数, 166

## W, X

### `W3C 网络性能`, 83

### `Webkit`, 6

### `WebPagetest`

- `Auth` 标签, 20
- `Block` 标签, 21
- `code repository`, 19
- `document.onload` 事件, 19
- `performance optimization checklist`, 22
- `pie charts`, 22
- `Preserve original User Agent string` 选项, 20
- `public web site`, 19
- `results page`, 21
- `Script` 标签, 20
- `setUserAgent` 命令, 21
- `SSL certification errors`, 19
- `Video` 标签, 21
- `waterfall charts`, 22

### `网络性能`

- `definition`, 1
- `network transactions diagram`, 2
- `optimizations lazy loading` (参见 懒加载)
- `page render bottlenecks` (参见 页面渲染瓶颈)
- `parsing and rendering` (参见 解析与渲染)
- `runtime performance`, 8
- `site abandonment`, 8

### `TCP/IP 模型`

- `Application layer`, 3
- `communication`, 3
- `HTTP requests`, 5
- `HTTP Response`, 4
- `Internet layer`, 3
- `Network Access layer`, 3
- `specification`, 3
- `total page size`, 4
- `Transport layer`, 3
- `user experience`, 8

### `Window 性能`

### `数据可视化`

- `average perceived load time`, 浏览器, 106
- `avgTimeBreakdownInRequest`, 102
- `Avg time`, HTTP 请求, 103, 104
- `data.frame()` 函数, 103
- `exponential notation`, 103
- `frequency distribution`, 102
- `getDFByBrowser()`, 105
- `grep()` 函数, 105
- `HTTP transaction process`, 101
- `loadtime_bybrowser`, 105
- `negative numbers`, 103
- `updated R file`, 106
- `high-resolution time`, 98–101
- `logging functionality updation`, 93
- `memory object`, 96–98
- `Navigation object`, 94–96

### `对象`

- `Chrome` 20 测试版, 84
- `JavaScript console`, 83
- `perfLogger` (参见 Perflogger)
- `timing`, 84
- `W3C web performance`, 83

### `万维网联盟 (W3C)`, 11

### `wpochart` 变量, 53

### `wpologs` 数据框, 53

### `wpt_credentials_urls.php`, 47

### `$wpt_response` 变量, 47

### `WPTRunner`

#### 架构

- `directory structure`, 45
- `finished sequence diagram`, 44, 45
- `first iteration sequence diagram`, 44
- `process_wpt_response`, 43
- `sequence diagram`, 43
- `webpagetest_responses.txt`, 44
- `wpo_log.txt`, 44

#### `charting with R`, 53

#### `completed process_wpt_response` 文件, 52–53

#### `data parsing`, 54–55

#### `interview (Meenan, Patrick)` (参见 Meenan, Patrick (WebPagetest))

#### `parsing test results`

- `appendToFile`, 51
- `fileio()` 函数, 49
- `firstView` 节点, 51
- `formatted result`（格式化结果），49
- `formatWPOLog()` 函数，51–52
- 循环（looping），50–51
- `$newline`，51
- `readCSVurls()` 函数，49
- `repeatView` 节点，51
- `statusCode` 节点，49–51
- `$xml` 对象转换，50

#### 绘图（plotting）

- 加载时间，55–57
- 有效负载与 HTTP 请求数量，57–61

#### 共享配置文件 API 密钥，45

`appendToFile()` 函数，46

`fileio.php`，46

URL 存储，45

WebPagetest API 访问，46–49

`wpt_credentials_urls.php`，46

### ![Images](img/square.jpg) Y, Z

YSlow

- 组件部分，17
- 实体标签（etags），17
- 浏览器内工具，16
- 安装，16，17
- 与页面速度测试，18，19
- 结果屏幕，17，18
- 规则集，17
- 屏幕——统计信息，18
- 启动屏幕，17
- 子导航栏，17