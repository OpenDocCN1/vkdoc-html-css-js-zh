# 十五、HTML5 表单：使用 HTML5 标签创建表单

接下来说说 HTML5 的 13 个表单创建标签；三个是 HTML5 新版本，十个在 HTML5 和 HTML 传统版本中工作。它们允许开发人员创建不同类型的基于表单的内容，使用带有数据输入(或数据输入)字段的表单容器以及表单中的高级用户界面控件，例如可以使表单具有交互性的按钮和下拉列表。这些更加专门化的表单相关标记允许 HTML5 开发人员使用十几个强大的标记，非常灵活地为他们的文档定义表单。在大多数情况下，表单标记与服务器端处理组件进行对话，因此在这个过程中，我们只讨论客户端表单设计 HTML5 标记组件。对于服务器端处理，一定要在 Apress 网站( [`www.apress.com`](http://www.apress.com) )上找一本关于 PHP、JSF、Drupal、Joomla、AJAJ 或者 AJAX 的书。

在这一章中，您将看到与在 HTML 中实现表单内容相关的表单标签，因为所有这些标签在 HTML5 中都是受支持的，并且在 HTML 的早期版本中也有很多。这些标签包括表单`<form>`，标签`<label>`，输入`<input>`，文本区域`<textarea>`，字段集`<fieldset>`，图例`<legend>`，选择`<select>`，选项`<option>`和选项组`<optgroup>`，按钮`<button>`，数据列表`<datalist>`，密钥生成器`<keygen>`，以及允许您进行内嵌计算的输出`<output>`标签。

## HTML5 表单标签:交互信息

本章涵盖了 HTML5 中支持的 13 个文本相关的表单标签，它们创建交互式表单来收集信息。这些通常被设计成网格结构，甚至类似于表格。这些标签中有十个在 HTML 的旧版本中受支持，其中三个是 HTML5 的新标签。这些表单标签也可以作为第十章中语义标签的子标签，这样基于表单的内容就被封装(或包装)到标准化的语义容器中。表单数据也可以是语义的；可以使用语义搜索来吸收它，因为与表单相关的标签名称描述了由该标签定义的表单元素的作用，因此它代表了内容。

表 15-1 显示了 HTML5 支持的 13 个表单标签；其中 10 种在传统 HTML 版本中受支持。

表 15-1。

Thirteen HTML Forms Design Content Publishing Tags

| HTML 表单创建标签 | HTML 表单创建标签的目的或用途 |
| --- | --- |
| `<form>` | 定义表单 |
| `<input>` | 定义输入(数据字段) |
| `<label>` | 定义输入(字段)标签 |
| `<textarea>` | 定义文本区域(多行输入字段) |
| `<fieldset>` | 定义字段集(一组输入字段) |
| `<legend>` | 定义字段集的图例(标签) |
| `<select>` | 定义一个下拉列表 |
| `<option>` | 定义下拉列表中的选项 |
| `<optgroup>` | 在下拉列表中定义一个选项组 |
| `<button>` | 定义一个按钮 |
| `<datalist>` (HTML5 中的新功能) | 输入控件的预定义选项列表 |
| `<keygen>` (html 5 中的新功能) | 在表单中定义密钥对生成器字段 |
| `<output>` (HTML5 中的新功能) | 定义输出(计算的结果) |

让我们看看这些表单创建标签在逻辑部分中的排列方式，从核心 HTML5 表单标签`<form>`、`<label>`和`<input>`开始。这些为 HTML5 呈现引擎定义了重要的表单特征或组件。因此，这些表单标签也应该被认为是语义搜索标签。因此，为表单正确实现这些标记是很重要的。

### 基本的 HTML 表单创建:表单、标签和输入

定义表单本身的顶级表单标签包括`<form>`标签，它用作表单的父容器，以及`<label>`和`<input>`子标签，它们指定表单标签和数据输入字段。

#### 表单标签:用于创建表单元素的核心标签

标签定义了 HTML 表单结构，用于获取用户数据输入。一个 HTML 表单最起码必须包含这个`<form>`元素，以及一个或多个表 15-1 中的元素。

HTML5 中引入了两个`<form>`元素参数，`autocomplete` 和 `novalidate`，目前仍支持六个 HTML 参数。一种不再支持，如表 15-2 所示。

表 15-2。

九个`<form>`参数用于表单配置

| Html 5 form label parameters | Usage of HTML5 form label parameters |
| --- | --- |
| `Accept` (html 5 is not supported) | Specify a comma-separated list of file types accepted by the server, which is submitted through the file upload process. |
| `autocomplete` (new feature in HTML5) | Specifies to turn on or off the auto-completion function of the form. |
| `novalidate` (new feature in HTML 5) | The specified form should not be validated. |
| `accept-charset` | Specifies the character encoding used for form submission. |
| `action` | Specify where the form data is submitted. |
| `enctype` | Specify how the form data should be encoded. |
| `method` |  |
| `name` | Allows you to specify a form name. |
| `target` | Specify the location where the response is received after the form is submitted (`_blank`, `_parent`, `_self`, `_top`) |

正如你在本章中看到的，所有其他的表单标签都是`<form>`的子标签。以下 HTML 创建一个空表单:

```html
<!DOCTYPE html><html>
<head>
 <title>Empty Exotic Car Preference Form</title>
</head>
<body>
 <section id="exotic car preference form">
  <form action="myForm.asp" method="get" autocomplete="on" novalidate>

    <!-- Your Form Design and Child Tags will be nested in here -->
  </form>

  <p>Form data will be sent to a page on the server called "myForm.asp"</p>
 </section>
</body>
</html>
```

这个`<form>`标签定义了表单数据提交的`myForm.asp`，定义了服务器从客户端表单读取数据的 HTTP `get` 方法，将自动完成特性设置为 `on`，并将 `novalidate` 特性设置为 `true`，方法是在开始的`<form>`标签中包含 `novalidate` 参数本身。

接下来，让我们看看如何使用 input `<input>`标签，从填写表单的人那里获取数据输入。

#### 输入标签:向表单添加数据输入字段

`<input>`标签用作子标签，定义用户输入文本的数据输入区域。这些通常在表单和数据库中称为字段。这个标签有如此多的参数，我将包括两个表:一个包含 HTML5 的新参数，另一个包含 HTML5 不支持的参数。还有那些在 HTML5 和传统 HTML 版本中都有效的参数。在一个在父标签`<form>`中使用两个`<input>`子标签的简短例子之后，我们将讨论参数。

让我们使用`<input>`标签要求用户输入他们喜欢的汽车制造商和型号，如下面的标记所示:

# HTML5 表单输入参数详解

## 表单基础示例

以下是一个包含两个输入字段的异国汽车偏好表单示例：

```html
<!DOCTYPE html><html>
<head>
  <title>My Exotic Car Preference Form with Two Input Fields</title>
</head>
<body>
 <section id="exotic car preference form">
  <form action="myForm.asp" method="get" autocomplete="on" novalidate>
  Manufacturer: <input type="text" name="manufacturer" value="Ferrari"><br>
  A Model Name: <input type="text" name="model-name" value="LaFerrari"><br>
  </form>
  <p>Please enter favorite exotic car manufacturer and model name above!</p>
 </section>
</body>
</html>
```

参数指定文本输入、字段名称和默认值。`autocomplete="on"` 参数告诉浏览器允许预测或猜测用户当前正在键入的数据值。当用户开始在字段中键入内容时，浏览器应该根据先前键入的值或预定义的输入来显示填充字段的选项。当我们在本章稍后讨论用于定义数据元素列表的 `<datalist>` 标签时，您会看到这个预定义输入是如何定义的。它与 `autocomplete` 参数（和函数）一起使用，通过在表单上提供“人工智能”的外观来大大增强用户对表单设计的体验，这为用户提供了关于表单中的字段在寻找什么的反馈。

## HTML5 `<input>` 参数

表 15-3 列出了 19 个新的 HTML5 `<input>` 标签参数。

### 表 15-3：HTML5 `<input>` 参数

| HTML5 输入标记参数 | HTML5 输入标记参数的用法 |
| --- | --- |
| `autocomplete` | 为输入指定自动完成打开或关闭 |
| `autofocus` | 页面加载时为输入指定的自动对焦 |
| `dir` | 为输入指定文本方向 |
| `form` | 指定了输入所属的表单 |
| `formaction` | 处理表单的指定 url |
| `formenctype` | 指定编码（用于提交或图像） |
| `formmethod` | 指定要使用的 HTTP 方法（`get` 或 `post`） |
| `formnovalidate` | 不应验证指定的输入 |
| `formtarget` | 指定显示响应的位置 |
| `height` | 指定了输入元素的高度 |
| `list` | 包含输入选项的指定数据列表 |
| `max` | 指定输入元素的最大值 |
| `min` | 指定输入元素的最小值 |
| `multiple` | 为输入指定了多个值 |
| `pattern` | 指定检查 `<input>` 元素值所依据的“正则表达式” |
| `placeholder` | 指定一个简短的提示，描述要输入到 `<input>` 元素中的预期值 |
| `required` | 将输入字段指定为必填字段 |
| `step` | 为输入指定的法定数字间隔 |
| `width` | 指定了输入元素的宽度 |

## 遗留 HTML `<input>` 参数

表 15-4 列出了 11 个遗留 HTML `<input>` 标签参数。

### 表 15-4：HTML `<input>` 参数

| HTML 输入标记参数 | HTML 输入标记参数的用法 |
| --- | --- |
| `align`（不在 HTML5 中） | 指定图像输入的对齐方式；仅适用于 `type="image"`（左、右、上、中、下） |
| `accept` | 指定服务器接受的文件类型；仅适用于 `type="file"`（音频/类型、视频/类型、图像/类型） |
| `alt` | 指定图像的替换文本；这仅适用于 `type="image"` |
| `checked` | 指定页面加载后预先选择的输入元素；这适用于 `type="checkbox"` 或 `type="radio"`（单选按钮） |
| `disabled` | 指定应该禁用 `<input>` 元素 |
| `maxlength` | 指定输入字段中的最大字符数 |
| `name` | 指定输入字段（元素）名称 |
| `readonly` | 指定输入字段是只读的 |
| `size` | 以字符表示的指定输入字段宽度 |
| `src` | 指定用作提交按钮的图像的 URL；仅用于 `type="image"` |
| `type` | 指定要显示的输入元素的类型 |
| `value` | 指定输入元素默认值 |

HTML5 不支持 `align` 参数，因为它是用 CSS3 实现的。这 11 个参数在 HTML5 和 HTML 的早期（传统）版本中都受支持。

## 添加输入限制与属性

让我们添加一个字符数限制，允许您的用户用来指定汽车品牌和型号名称：

```html
<form action="myForm.asp" method="get" autocomplete="on" novalidate>
 Manufacturer:
 <input type="text" name="manufacturer" value="Ferrari" maxlength="16"><br>
 A Model Name:
 <input type="text" name="model-name" value="LaFerrari" maxlength="24"><br>
</form>
```

这个 `maxlength` 参数允许您控制文本数据字段的宽度，以便您可以有一个紧凑的表单设计，并将用户限制在一个合理的字符数据长度。

让我们将 `autofocus` 添加到第一个数据输入字段，以便用户将光标放在正确的数据字段中，准备输入：

```html
<form action="myForm.asp" method="get" autocomplete="on" novalidate>
  Manufacturer:
  <input type="text" name="manufacturer" value="Ferrari"
         maxlength="16" autofocus><br>
  A Model Name:
  <input type="text" name="model-name" value="LaFerrari" maxlength="24"><br>
</form>
```

现在让我们通过使用 `<form>` 标记中的 `name` 属性和 `<input>` 标记中的 `form` 属性，引用相同的字符，将输入字段“连接”到它们所在的表单：

```html
<form name="carpreferenceform" action="myForm.asp" method="get"
      autocomplete="on" novalidate>
  Manufacturer:
  <input type="text" name="manufacturer" value="Ferrari"
         maxlength="16" form="carpreferenceform" autofocus><br>
  A Model Name:
  <input type="text" name="model-name" value="LaFerrari"
         maxlength="24" form="carpreferenceform"><br>
</form>
```

注意，因为我在我的 `<form>` 标签中有一个 `method="get"`，所以我不必在每个 `<input>` 标签中使用 `formmethod="get"`。出于设计目的，我还可以用像素来定义这些数据字段的宽度和高度：

```html
<form name="carpreferenceform" action="myForm.asp" method="get"
      autocomplete="on" novalidate>
  Manufacturer:
  <input type="text" name="manufacturer" value="Ferrari"   height="24"
         maxlength="16" form="carpreferenceform" autofocus width="128"><br>
  A Model Name:
  <input type="text" name="model-name" value="LaFerrari" height="24"
         maxlength="24" form="carpreferenceform"         width="128"><br>
</form>
```

我不打算在这里详细介绍 `<input>` 元素的 30 个参数，因为其中一些是不言自明的，一些很少使用，还有一些与表单处理的服务器端结合使用，这很可能是指定使用什么参数的不同团队成员！

接下来，我们来看看`<label>`标签。这允许您标记`<input>`元素，尤其是那些使用类型参数的元素，以指定除数据字段之外的内容，例如单选按钮、GUI 按钮或复选框。

## 标签标记:输入元素的固定文本标签定义

鉴于`<input>`标签是一个空标签，不包含任何内容，只包含参数，而`<label>`标签可以在`<label>`和`</label>`开始和结束标签之间包含一个描述性标签。`<label>`标签专门用于定义相关`<input>`元素的标签。这个`<label>`对于用户来说，除了一个文本标签之外，没有任何特别的东西。然而，它确实为触摸屏和鼠标用户提供了用户体验改进，因为如果用户触摸或单击使用此`<label>`元素指定的文本，它将切换由`<input type="control-name">`指定的输入(control)元素。

这个`<label>`标签只有两个参数；`form`参数，你已经看到带有`<input>`标签，它将`<label>`连接到一个`<form>`，以及`for`参数，它使用`id`参数将`<label>`连接到`<input>`本身。

标签的`for`参数应该等于相关元素的`id`属性，以便将它们绑定在一起。这显示在下面的 HTML5 标记示例中:

```html
<!DOCTYPE html><html>
<head>
  <title>My Exotic Car Preference Form using Six Radio Buttons</title>
</head>
<body>
 <section id="exotic car preference form">
  <form name="carform" action="myForm.asp" method="get" novalidate>
   <label for="ferrari">Ferrari</label>
   <input type="radio" name="cartype" form="carform" id="ferrari"
          value="Ferrari Brand Selected" /><br>
   <label for="maserati">Maserati</label>
   <input type="radio" name="cartype" form="carform" id="maserati"
          value="Maserati Brand Selected" /><br>
   <label for="bugatti">Bugatti</label>
   <input type="radio" name=cartype" form="carform" id="bugatti"
          value="Bugatti Brand Selected" /><br>
   <label for="laferrari">Ferrari</label>
   <input type="radio" name="carmodel" form="carform" id="laferrari"
          value="Ferrari La Ferrari Model Selected" /><br>
   <label for="grancabrio">GranCabrio</label>
   <input type="radio" name="carmodel" form="carform" id="grancabrio"
          value="Maserati GranCabrio Model Selected" /><br>
   <label for="chiron">Chiron</label>
   <input type="radio" name=carmodel" form="carform" id="chiron"
          value="Bugatti Chiron Model Selected" /><br>
   <input type="submit" value="Please Submit My Choices" form="carform" />
  </form>                                  
  <p>Please select your favorite exotic car manufacturer and model name</p>
 </section>
</body></html>
```

这次我们使用单选按钮来指定用户选择他们最喜欢的汽车制造商和最喜欢的汽车。单选按钮按名称分组，这样您就不能选择多个，并且分别使用`form`和`for`参数连接到前面描述的表单和输入。您提供的值是当使用`<input type="submit">`标签提交表单时发送到服务器(或电子邮件地址)的内容，该标签用于一个简单的提交按钮。本章稍后将介绍复杂按钮。接下来，让我们看看表单数据输入的更大区域，例如文本区域或段落文本输入以及字段组或字段集。

### HTML 表单内容组:TextArea 或 FieldSet

表 15-1 中接下来的三个标签允许你为文本和数据输入字段集合定义大的数据输入区域。这些标签有十几个参数，HTML5 都支持这些参数。我将它们包括在表 15-5 和 15-6 中，并试图涵盖示例中的所有关键参数。

表 15-6。

Three `<fieldset>` Parameters Used Prior to HTML5

| HTML 字段集标记参数 | HTML 字段集标记参数的用法 |
| --- | --- |
| `disabled` | 指定应禁用文本区域 |
| `form` | 指定文本区域所属的表单 |
| `name` | 指定文本区域(元素)的名称 |

表 15-5。

Twelve HTML5 Parameters for Text Area Configuration

| TextArea 标记参数 | TextArea 标记参数的用法 |
| --- | --- |
| `cols` | 指定文本区域列(字符) |
| `rows` | 指定文本区域行(线) |
| `name` | 指定文本区域(元素)的名称 |
| `disabled` | 指定应禁用文本区域 |
| `readonly` | 指定文本区域是只读的 |
| `autofocus` | 指定页面加载时文本区域自动聚焦 |
| `dir` | 指定请尊重文本区域方向 |
| `form` | 指定文本区域所属的表单 |
| `maxlength` | 指定文本区域最大字符数 |
| `placeholder` | 所需文本的指定描述 |
| `required` | 指定需要文本区域补全 |
| `wrap` | 指定文本区域需要如何换行(选项为硬或软) |

#### TEXTAREA 标签:定义一个文本输入块或段落

`<textarea>`标签允许你定义多行文本输入控件。这些文本区域控件可以容纳无限数量的字符，尽管不建议这样做，并且文本使用固定宽度的字体(如 monospace 或 Courier)呈现。文本区域的大小应该使用`cols`和`rows`参数来指定，正如我在下面的示例标记中所做的那样，我要求用户提供他们最喜欢的汽车品牌和型号的书面描述。我还在示例中使用了几乎所有可能的`<textarea>`标签参数，包括`autofocus`、`required`、`name`、`form`、`placeholder`和`maxlength`。

```html
<!DOCTYPE html><html>
<head>
 <title>Exotic Car Preference Written Description Form</title>
</head>
<body>
 <section id="exotic car preference paragraph form">
  <form name="cardescription" action="myForm.asp" method="get" novalidate>
   <textarea rows="5" cols="250" maxlength="1250" required
              name="myfavoritecar" form="cardescription" autofocus
              placeholder="Write a short paragraph on your favorite car">    Please write a short paragraph on your favorite car and brand in here!    </textarea>

  </form>
  <p>Please write a short paragraph on your favorite car and brand above</p>
 </section>
</body></html>
```

这个`<textarea>`标签的十几个参数列在表 15-5 中，五个遗留参数列在第一部分，七个新的 HTML5 参数列在第二部分。

接下来，让我们看看如何使用

# HTML 表单元素：`<fieldset>` 与 `<legend>`

`<fieldset>` 元素将字段和数据输入控件进行逻辑分组。

## FIELDSET 标签：分组数据字段和输入控件

`<fieldset>` 标签用于将相关的表单数据域和控制元素组合在一个复杂的表单中。

`<fieldset>` 标签在相关元素周围画一个方框，以便在视觉上对它们进行分组。

让我们使用单选按钮控件示例，通过使用这个 `<fieldset>` 元素，将汽车品牌选择和汽车型号选择单选按钮放入它们自己的逻辑部分。

这应该类似于下面的 HTML5 标记：

```html
<!DOCTYPE html><html>
<head>
 <title>Exotic Car Preference Form: 6 Radio Buttons in 2 Field Sets</title>
</head><body>
 <section id="exotic car preference form">
  <form name="carform" action="myForm.asp" method="get" novalidate>
   <fieldset name="carbrands" form="carform">
    <label for="ferrari">Ferrari</label>
    <input type="radio" name="cartype" form="carform" id="ferrari"
           value="Ferrari Brand Selected" /><br>
    <label for="maserati">Maserati</label>
    <input type="radio" name="cartype" form="carform" id="maserati"
           value="Maserati Brand Selected" /><br>
    <label for="bugatti">Bugatti</label>
    <input type="radio" name=cartype" form="carform" id="bugatti"
           value="Bugatti Brand Selected" /><br>
   </fieldset>

   <fieldset name="carmodels" form="carform">
    <label for="laferrari">Ferrari</label>
    <input type="radio" name="carmodel" form="carform" id="laferrari"
           value="Ferrari La Ferrari Model Selected" /><br>
    <label for="grancabrio">GranCabrio</label>
    <input type="radio" name="carmodel" form="carform" id="grancabrio"
           value="Maserati GranCabrio Model Selected" /><br>
    <label for="chiron">Chiron</label>
    <input type="radio" name=carmodel" form="carform" id="chiron"
           value="Bugatti Chiron Model Selected" /><br>
   </fieldset>

    <input type="submit" value="Please Submit My Choices" form="carform" />
  </form>      
 <p>Please select favorite exotic car manufacturer and model name above</p>
 </section>
</body></html>
```

请注意，我已经使用 `form` 和 `name` 参数将所有内容连接在一起，并命名了逻辑字段集。表 15-6 中列出了三个 HTML5 `<fieldset>` 参数。

接下来，让我们看看复杂表单的 `<legend>` 元素。

## 图例标记：向字段集分组添加图例

`<legend>` 标签定义了 `<fieldset>` 元素的标题。它有一个 HTML5 中不再支持的 `align` 参数（见表 15-4），因为你现在需要使用 CSS 来实现这个对齐功能（尽管这个参数在一些试图提供向后兼容性的浏览器中可能仍然有效）。对齐参数值包括顶部、底部、左侧和右侧。

```html
<form name="carform" action="myForm.asp" method="get" novalidate>
 <fieldset name="carbrands" form="carform">
  <legend>Choose Your Favorite Exotic Sports Car Brand:</legend>

   <label for="ferrari">Ferrari</label>
    <input type="radio" name="cartype" form="carform" id="ferrari"
           value="Ferrari Brand Selected" /><br>
   <label for="maserati">Maserati</label>
    <input type="radio" name="cartype" form="carform" id="maserati"
           value="Maserati Brand Selected" /><br>
   <label for="bugatti">Bugatti</label>
    <input type="radio" name=cartype" form="carform" id="bugatti"
           value="Bugatti Brand Selected" /><br>
  </fieldset>
  <fieldset name="carmodels" form="carform">
   <legend>Choose Your Favorite Exotic Sports Car Model:</legend>

    <label for="laferrari">Ferrari</label>
     <input type="radio" name="carmodel" form="carform" id="laferrari"
            value="Ferrari La Ferrari Model Selected" /><br>
    <label for="grancabrio">GranCabrio</label>
     <input type="radio" name="carmodel" form="carform" id="grancabrio"
            value="Maserati GranCabrio Model Selected" /><br>
    <label for="chiron">Chiron</label>
     <input type="radio" name=carmodel" form="carform" id="chiron"
            value="Bugatti Chiron Model Selected" /><br>
  </fieldset>
    <input type="submit" value="Please Submit My Choices" form="carform" />
</form>
```

接下来，让我们看看复杂表单的选项选择标记。

### HTML 表单选项选择：`<select>` 和 `<option>`

HTML5 表单有一些相当复杂的选项，用于选择选项和选项分组，很像应用软件包中的菜单，这使得表单设计成为 HTML5 发布中最先进的领域之一，还有新媒体和交互性，正如您在本书后面看到的。表 15-1 的第三部分显示了用于在 HTML5 表单设计中创建这些选择集（称为下拉列表）的标签。

#### `<select>` 和 `<option>` 标签：定义下拉列表

`<select>` 元素创建下拉列表。在 `<select>` 元素中使用了 `<option>` 标签来定义您希望使用该列表来提供的任何选项。这些类似于单选按钮，因为您只能选择列表中的一个成员。如果要选择多个数据项，请使用一组复选框，其中可以选择多个数据项。

一个`<select>`标签支持七个参数，其中三个是 HTML5 中的新参数，见表 15-7 的顶部。遗留 HTML 版本支持另外四个参数，在表的底部可以看到。

表 15-7。七个 `<select>` 参数用于选择列表

| Html 5 select tag parameters | Usage of HTML5 select tag parameters |
| --- | --- |
| autofocus | Specifies that the selector automatically focuses on page loading. |
| form | Specifies the form to which the form selector belongs. |
| multiple | Specify multiple values for the selector. |
| name | Specify the selector name. |
| size | Define the number of options. |

让我们创建一个制造商和型号选择示例，它在语义标签内的 `<fieldset>` 标签和 `<form>` 标签内使用 `<select>` 和 `<option>` 标签，而不是使用单选按钮为用户提供单一选择选项。

使用 `<form>` 标签中的 `name` 参数，以及每个 `<select>` 标签中的 `form` 参数，将 `<select>` 标签连接到 `<form>` 标签中。

您使用 `<select>` 标签中的 `required` 参数和 `size="4"` 参数来指定选项的数量。您应该在第一个 `<select>` 中设置 `autofocus` 参数，以便它被预先选择使用，最后使用一个 `name` 参数给每个 `<select>` 元素一个唯一的标识。

下面是一个使用 HTML5 标记的例子:

```html
<!DOCTYPE html><html>
<head>
 <title>Exotic Car Selection Form: 8 List Selections in 2 Field Sets</title>
</head>
<body>
 <section id="exotic car preference form">
  <form name="carform" action="myForm.asp" method="get" novalidate>
   <fieldset name="carbrands" form="carform">
    <select form="carform" required name="brandlist" autofocus size="4">
     <option value="ferrari" label="Ferrari">Enzo Ferrari</option>
     <option value="maserati" label="Maserati">Alfieri Maserati</option>
     <option value="bugatti" label="Bugatti">Ettore Isidoro Bugatti</option>
     <option value="lmbo" label="Lamborghini">Ferruccio Lamborghini</option>
    </select>

   </fieldset>
   <fieldset name="carmodels" form="carform">
    <select form="carform" required name="modellist" size="4">
     <option value="laferrari">LaFerrari</option>
     <option value="grancabrio">GranCabrio</option>
     <option value="chiron">Chiron</option>
     <option value="gallardo">Gallardo</option>
    </select>

   </fieldset>
   <input type="submit" value="Please Submit My Selection" form="carform"/>
  </form>       
  <p>Please select favorite exotic car manufacturer and model name above</p>
 </section>
</body></html>
```

还要注意，我使用了一个 `label` 属性来指定一个较短版本的选项文本值。这个较短的版本显示在下拉列表中。还要注意，这还没有在所有 HTML5 平台上的所有浏览器中完全实现，但它被完全支持只是时间问题，因为它的实现是非常符合逻辑的。

接下来，让我们看看选项组或 `<optgroup>` 标签。

### OPTGROUP 标签:分组下拉列表选项

`<optgroup>` 标签将下拉列表中的相关选项分组。如果你有一个很长的选项列表，那么对你的最终用户来说，一组相关的选项更容易理解。让我们将汽车制造商列表选项分组到更贵和更便宜的列表分组中，看看这个 `<optgroup>` 标签是如何使用的。HTML5 标记如下所示:

```html
<select form="carform" required name="brandlist" autofocus size="4">
 <optgroup label="More Affordable Exotic Cars">

  <option value="ferrari" label="Ferrari">Enzo Ferrari</option>
  <option value="maserati" label="Maserati">Alfieri Maserati</option>
 </optgroup>

 <optgroup label="Less Affordable Exotic Cars">

  <option value="bugatti" label="Bugatti">Ettore Isidoro Bugatti</option>
  <option value="lambo" label="Lamborghini">Ferruccio Lamborghini</option>
 </optgroup>

</select>
```

接下来，我们来看看 `<button>` 表单设计标签。

### 按钮标签:创建用户界面按钮

`<button>` 标签定义了可点击的按钮，用于提交或重置表单，或者用于自定义目的。您可以在 `<button>` 元素中放置内容，比如文本或图像。这是 `<button>` 元素和本章前面使用 `<input type="submit">` 元素创建的提交按钮之间的主要区别。

确保总是指定一个 `type` 参数，使用带有 `<button>` 元素的 `submit`、`reset` 或 `button` 值。需要注意的是，不同的 HTML5 浏览器对这个 `<button>` 元素使用不同的默认类型，所以您需要通过指定您希望按钮做什么来“强制解决这个问题”，而不要依赖为您正确设置的默认值！

表 15-8 中列出了八个 HTML5 `<button>` 参数，以及三个传统 HTML 参数。

表 15-8。HTML5 `<button>` 按钮配置参数

| Html 5 button tag parameters | Usage of HTML5 button tag parameters |
| --- | --- |
| autofocus | Specify the button autofocus loading on the page. |
| form | Specify the form to which the button belongs. |
| formaction | Specify the URL to which the form data will be sent. |
| formenctype | Specify the encoding type for the form data. |
| formmethod | Specify the HTTP method to use (get or post). |
| formnovalidate | The specified button should not be verified. |
| formtarget | Specify where to display the response. |
| name | Specify the button element name. |
| disabled | The specified button is disabled. |

让我们用 `<button type="submit">` 和 `<button type="reset">` 标记替换示例中的 `<input type="submit">`:

```markdown
# 新的 HTML5 表单标签: DataList，KeyGen，Output

HTML5 中有三个用于表单设计的新标签。它们允许您定义自动完成的数据列表，为数据安全生成密钥，并输出数据的计算结果。

## DATALIST 标签: 定义每个描述术语子元素

`<datalist>` 标签通过表 15-3 中所示的 `list` 参数指定一个 `<input>` 元素使用的预定义选项列表。`<datalist>` 标签用于在 `<input>` 元素上提供自动完成功能。当提供 `<datalist>` 时，一旦用户开始输入数据，就会看到预定义选项的下拉列表出现。要将 `<input>` 元素 `list` 参数与 `<datalist>` 元素绑定在一起，请在 `<datalist>` 标签中使用 `id` 参数，如以下 HTML5 标记所示：

```html
<input list="italiancars">
<datalist id="italiancars">
  <option value="Ferrari">
  <option value="Maserati">
  <option value="Bugatti">
  <option value="Lamborghini">
</datalist>
```

接下来，让我们看看一个密钥生成器 `<keygen>` 标签。

## KEYGEN 标签: 定义每个描述数据子元素

`<keygen>` 标签允许您指定一个安全密钥对生成器字段，用于保护表单数据。提交表单时，私钥存储在本地，公钥发送到服务器。

下面是一个示例表单和用户名输入，它使用这个 `<keygen>` 标记和一个 `<button>` 标记向服务器提交安全密钥，如下面的 HTML5 标记所示：

```html
<form action="private_keygen.asp" method="get" name="keyform">
 Username: <input type="text" name="user_name">
 Encryption: <keygen name="security_key">
 <button type="submit" value="Submit Secure Key" form="keyform" name="K1"/>
</form>
```

表 15-9 显示了 `<keygen>` 标签支持的六个 HTML5 参数。

**表 15-9。Six `<keygen>` Parameters for Secure Key Generation**

| HTML5 KeyGen 标记参数 | HTML5 KeyGen 标记参数的用法 |
| --- | --- |
| `autofocus` | 页面加载时指定的 `keygen` 自动对焦 |
| `form` | 密钥生成所属的指定表单 |
| `disabled` | 指定的 `keygen` 元素将被禁用 |
| `keytype` | 为密钥指定了安全算法 |
| `name` | 定义 `KeyGen` 元素名称 |
| `challenge` | 指定提交时挑战 `<keygen>` 元素的值 |

接下来，我们来看看输出 `<output>` 标签。

## 输出标签: 定义每个描述数据子元素

`<output>` 标签表示计算的结果，例如使用 `<input>` 数据字段执行的计算，如示例中所做的，或者来自更复杂的 JavaScript 计算。表 15-10 显示了新的 `<output>` 标签（元素）在 HTML5 中支持的三个参数。

**表 15-10。Three `<output>` Parameters for Output Generation**

| HTML5 输出标记参数 | HTML5 输出标记参数的用法 |
| --- | --- |
| `for` | 指定计算结果与该计算中使用的元素之间的关系 |
| `form` | 指定输出所属的表单 |
| `name` | 定义输出元素名称 |

让我们创建一个将两个数字相加的 `<form>` 构造，使用 `<input>` 字段收集数据，`<form>` 标签中的 `oninput` 事件进行简单的 `o.value = inputA + inputB` 计算，使用 `<output>` 标签保存输出。使用 `o.value` (form `oninput`) 和 `name="o"` (output 标记参数) 将和 `<output>` 构造连接在一起。

对此的 HTML5 标记如下所示：

```html
<form name="add2numbers" onsubmit="return false"
      oninput="o.value = parseInt(a.value) + parseInt(b.value)">
      <input name="a" type="number" step="any"> +
      <input name="b" type="number" step="any"> =
      <output name="o"></output>
</form>
```

接下来，我们将开始用 HTML5 定义区域。

## 摘要

本章讨论了 HTML5 和以前版本中的表单标签支持，包括 `<form>`、`<input>`、`<label>`、`<textarea>`、`<fieldset>`、`<legend>`、`<select>`、`<option>`、`</option>`、`<optgroup>`、`<button>`、`<datalist>`、`<keygen>` 和 `<output>` 标签。在下一章中，您将了解 HTML5 标签支持 HTML5 文档和应用中的内容定位，以及使用像素或百分比定义 HTML5 中的区域，从而使您的 HTML 设计更加精确！
```