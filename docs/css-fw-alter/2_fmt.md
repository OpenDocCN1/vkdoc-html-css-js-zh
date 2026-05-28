# 二、使用框架构建登录页面

## 安装骨架

要开始使用，请访问 Skeleton 网站 [`getskeleton.com/`](http://getskeleton.com/)。你会看到一个下载按钮，它在图 2-1 中用红框标出。

![A450102_1_En_2_Fig1_HTML.jpg](img/A450102_1_En_2_Fig1_HTML.jpg)
图 2-1 Skeleton 下载页面

点击下载以获取 Skeleton 的 `.zip` 文件。解压文件后，你将看到如图 2-2 所示的文件结构。

![A450102_1_En_2_Fig2_HTML.jpg](img/A450102_1_En_2_Fig2_HTML.jpg)
图 2-2 Skeleton 框架的内容

`css` 文件夹用于保存你的 CSS 文件。默认情况下，该文件夹包含 `Normalize` 和 `Skeleton` 样式表。`Normalize.css` 是一个小型 CSS 文件，它能提供更好的跨浏览器一致性，使 HTML 元素的默认样式更统一、更符合现代标准。它只针对需要标准化的样式进行精确调整。你可以在官方网站 [`necolas.github.io/normalize.css/`](https://necolas.github.io/normalize.css/) 上找到更多关于 `Normalize` 的信息。

你还可以看到 `images` 文件夹，用于存储你的图片。默认情况下，该文件夹包含 Skeleton 的 `favicon` 图片。

`index.html` 文件是你的默认网页。当你在 Notepad++ 或任何编辑器中编辑该页面时，你会看到如代码清单 2-1 所示的代码。

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <!-- Basic Page Needs ––––––––––––––––––––––––––––––––––– -->
  <meta charset="utf-8">
  <title>Your page title here :)</title>
  <meta name="description" content="">
  <meta name="author" content="">
  <!-- Mobile Specific Metas –––––––––––––––––––––––––––––– -->
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <!—FONT ––––––––––––––––––––––––––––––––––––––––––––––-–– -->
  <link href="//fonts.googleapis.com/css?family=Raleway:400,300,600" rel="stylesheet" type="text/css">
  <!—CSS ––––––––-––––––––––––––––––––––––––––––––––––––––– -->
  <link rel="stylesheet" href="css/normalize.css">
  <link rel="stylesheet" href="css/skeleton.css">
  <!—Favicon –––––––––––––––––––––––––––––––––––––––––––––– -->
  <link rel="icon" type="image/png" href="img/favicon.png">
</head>
<body>
  <!-- Primary Page Layout –––––––––––––––––––––––––––––––– -->
  <div class="container">
    <div class="row">
      <div class="one-half column" style="margin-top: 25%">
        <h4>Basic Page</h4>
        <p>This index.html page is a placeholder with the CSS, font and favicon. It's just waiting for you to add some content! If you need some help hit up the <a href="http://www.getskeleton.com">Skeleton documentation</a>.</p>
      </div>
    </div>
  </div>
<!-- End Document ––––––––––––––––––––––––––––––––––––––––– -->
</body>
</html>
```

代码清单 2-1 基本的 Skeleton 示例

现在，点击 `index.html` 文件以在浏览器中显示网页，如图 2-3 所示。

![A450102_1_En_2_Fig3_HTML.jpg](img/A450102_1_En_2_Fig3_HTML.jpg)
图 2-3 浏览器中的 Skeleton 基本示例

## 骨架网格系统

## Skeleton 网格系统

像大多数其他框架一样，Skeleton 也有自己的网格系统。它本质上是一个最大宽度为 960px 的 12 列网格。这是一个响应式网格，会根据浏览器/设备的大小自动调整。请查看代码片段（清单 2-2）以了解网格系统的工作原理。

### 代码示例

```html
<body>
  <div class="container">
    <!-- columns should be the immediate child of a .row -->
    <div class="row">
      <div style="text-align:center; border: 1px solid black;" class="one column">One</div>
      <div style="text-align:center; border: 1px solid black;" class="eleven columns">Eleven</div>
    </div>
    <br>
    <!-- just use a number and class 'column' or 'columns' -->
    <div class="row">
      <div style="text-align:center; border: 1px solid black;" class="two columns">Two</div>
      <div style="text-align:center; border: 1px solid black;" class="ten columns">Ten</div>
    </div>
    <br>
    <div class="row">
      <div style="text-align:center; border: 1px solid black;" class="three columns">Three</div>
      <div style="text-align:center; border: 1px solid black;" class="nine columns">Nine</div>
    </div>
    <br>
    <div class="row">
      <div style="text-align:center; border: 1px solid black;" class="four columns">Four</div>
      <div style="text-align:center; border: 1px solid black;" class="eight columns">Ten</div>
    </div>
    <br>
    <div class="row">
      <div style="text-align:center; border: 1px solid black;" class="five columns">Five</div>
      <div style="text-align:center; border: 1px solid black;" class="seven columns">Seven</div>
    </div>
    <br>
    <div class="row">
      <div style="text-align:center; border: 1px solid black;" class="six columns">Six</div>
      <div style="text-align:center; border: 1px solid black;" class="six columns">Six</div>
    </div>
    <br>
    <div class="row">
      <div style="text-align:center; border: 1px solid black;" class="seven columns">Seven</div>
      <div style="text-align:center; border: 1px solid black;" class="five columns">Five</div>
    </div>
    <br>
    <div class="row">
      <div style="text-align:center; border: 1px solid black;" class="eight columns">Eight</div>
      <div style="text-align:center; border: 1px solid black;" class="four columns">Four</div>
    </div>
    <br>
    <div class="row">
      <div style="text-align:center; border: 1px solid black;" class="nine columns">Nine</div>
      <div style="text-align:center; border: 1px solid black;" class="three columns">Three</div>
    </div>
    <br>
    <div class="row">
      <div style="text-align:center; border: 1px solid black;" class="ten columns">Ten</div>
      <div style="text-align:center; border: 1px solid black;" class="two columns">Two</div>
    </div>
    <br>
    <div class="row">
      <div style="text-align:center; border: 1px solid black;" class="eleven columns">Eleven</div>
      <div style="text-align:center; border: 1px solid black;" class="one columns">One</div>
    </div>
    <br>
  </div>
</body>
```

**清单 2-2** Skeleton 网格系统演示

### 代码解释

在清单 2-2 中，你定义了一个 `<body>` 元素，其中包含一个带有 `container` 类的 `<div>`。在该元素内部，你定义了带有 `row` 类的 `<div>`。在这个 `<div>` 内，你定义了两个 `<div>`，一个列宽为一列，另一个列宽为十一列。请记住，带有列类的 `<div>` 应该是带有 `row` 类的 `<div>` 的直接子元素。

要定义一列，你使用 `one column` 类。类似地，要定义十一列，你使用 `eleven columns` 类。对于两列，类名是 `two columns`。请注意，你为每一列使用了内联 CSS 样式 `<style="text-align:center; border: 1px solid black;">`，以将文本居中对齐并为每列添加 1px 的黑色边框。你使用 `<br>` 元素来设置每行之间的间距。

基本上，清单 2-2 中的代码定义了具有 `<div>` 类的不同行，并定义了不同宽度的列。图 2-4 显示了代码执行后的输出。

![A450102_1_En_2_Fig4_HTML.jpg](img/A450102_1_En_2_Fig4_HTML.jpg)

**图 2-4** Skeleton 网格系统演示

### 总结

如图 2-4 所示，第一行定义了两个列，宽度分别为一列和十一列。第二行定义了两个列，宽度分别为两列和十列。类似地，你定义了十一行，每行都有两个宽度不同的列。现在你对 Skeleton 中网格系统的工作原理有了概念。

## 骨骼属性概述

Let’s take a look at some attributes of the Skeleton framework.

*   Skeleton 的排版基础是 `Raleway`，一个基于 `Google` 的排版。字体大小默认为 `HTML` 的字体大小，版式保留类似于 `HTML` 基本版式的属性，如锚点、粗体、强调和下划线。

*   骨架中的按钮可以使用 `button-primary` 类创建，这很容易区分。如果不想要增强按钮，也可以选择标准按钮。为此，您只需使用 `button` 类，而不是 `button-primary`。您还可以使用 `<button>` 元素定义一个按钮，或者在 `button` 或 `button-primary` 类中使用一个锚标记 `<a>`。

*   使用 `<pre>` 和 `<code>` 元素可以创建代码块。对于几个代码块，可以在一个 `<pre>` 元素中使用多个 `<code>` 元素。

*   代码样式可以通过使用 `code` 类来设置。对于几个代码块，可以在一个 `<pre>` 元素中使用 `<code>` 元素。

*   Skeleton 中的表格类似于 `HTML` 表格，其中使用了 `<table>` 和 `<tr>` 元素。与 `HTML` 类似，使用 `<tr>` 定义表格行，`<td>` 定义表格数据，`<th>` 定义表格标题；您将所有内容包装在 `<table>` 主元素中。

*   Skeleton 使用以最小宽度为目标的移动优先查询。查询之外的样式适用于所有设备。这样做是为了防止手机和平板电脑等小型设备解析大量未使用的 `CSS`。框架根据设备大小使用以下媒体查询大小:
    *   手机: `400px`
    *   Phablet: `550px`
    *   平板电脑: `750px`
    *   桌面: `1000px`
    *   HD 桌上型电脑: `1200px`

Skeleton also comes with many helper classes that can be used to limit the elements within a container, float the element to the left or right, and clear the floats on both sides.

## 用框架构建登陆网页

In this section, you will create a landing page for a freelance portal called `RemoteDesk`. The landing page shows the various things you can do on the freelance portal. You will design a web page that shows several aspects of the freelance portal along with company information and other basic features. We will divide the process of building the web page into six Steps. After these six Steps, you will have a complete landing page.

### 步骤 1:定义内容区域

You will define the `<html>` tags and then move on to include the necessary links for Skeleton and Normalize (included by default in Skeleton) and the custom style sheet in the `<head>` tags (more about that in the code explanation). Then you will define the `<body>` section after the `<head>` tags and within the `<html>` tags. Inside the `<body>` tags, you will define the `<div>` class with the necessary rows and columns with the required content. Essentially, each content area will be encapsulated in an element with the `row` class. Depending on the content, you will divide the area into sections using elements with the `columns` class in that section’s parent row. Let’s look at Listing 2-3.

```html
<html>
    <head>
        <!-- Step1: Include the necessary style and heading-->
        <meta name="viewport" content="width=device-width, initial-scale=1.0,maximum-scale=1.0"/>
        <link href="https://fonts.googleapis.com/css?family=Source+Sans+Pro" rel="stylesheet">
        <link href="css/normalize.css" rel="stylesheet" type="text/css"/>
        <link href="css/skeleton.css" rel="stylesheet" type="text/css"/>
        <link href="css/style.css"  rel="stylesheet" type="text/css"/>
        <title>Best Freelance management app</title>
        <!-- end of Step 1-->
    </head>
    <body class="container">
        <!-- Navigation area -->
        <div class="row">
            <div class="two columns logo">RemoteDesk.com</div>
            <div class="eight columns">&nbsp;</div>
            <div class="two columns">
                <a class="button button-primary" href="#">Sign up</a>
            </div>
        </div>
        <!-- Navigation area ends -->
    </body>
</html>
```

Listing 2-3: Defining the Content Area

As you can see in Listing 2-3, you define the viewport size inside the `head` section. A viewport controls the way a web page is displayed on a mobile device. If you do not use a viewport, a mobile device will render the page in a typical desktop screen width. Setting a viewport helps you exercise control over a page’s width and scaling on varied devices. You can find more about viewports at [`developers.google.com/speed/docs/insights/ConfigureViewport`](https://developers.google.com/speed/docs/insights/ConfigureViewport).

Then, by default in Skeleton, you set the links for Normalize and Skeleton. Remember that if the path to your files is different, you need to specify so. For now, they should be in the root folder, so the default path is good to go. Then you define the path for the custom style sheet called as `style.css`, which you will place in the `css` folder.

You define a `<body>` tag and assign the `container` class to it. The `container` is the main centered wrapper. You define a `<div>` element and assign a `row` class to it. Inside that `<div>` element, you define three `<div>`s. The first `<div>` contains the content `RemoteDesk.com` and spans two columns. The next `<div>` spans eight columns, and you assign the `&nbsp;` value between the `<div>` tags. Essentially, `&nbsp` creates a nonbreaking space. It is used in programming and web design to create a space in a line that cannot be broken with word wrap. Using it will help create multiple spaces that are visible on a web page and not only in the source code. (We are using this because there are no offset classes in Skeleton compared to other frameworks like Bootstrap and Foundation.) The third `<div>` spans two columns and contains the Sign Up button, which we create using the `button-primary` class.

Figure 2-5 shows the output of the code.

![A450102_1_En_2_Fig5_HTML.jpg](img/A450102_1_En_2_Fig5_HTML.jpg)

Figure 2-5: The output of the content area

### 步骤 2:完成标签内容

## 插入头部图片

现在，你将在 `<body>` 标签内创建其余内容。你将从插入一张图片开始，如清单 2-4 所示。（请参考代码包中的完整代码以了解各个元素的定位；我们在代码包中分步提供了代码，以便你更好地理解每个步骤。最后，`index.html` 包含了整个着陆页的代码。）

```html
<div class="row masthead"></div>
```

**清单 2-4** 插入头部图片

你刚刚使用了 `row` 类，并为其分配了一个名为 `masthead` 的类。图片在样式表 `Style.css` 中定义。所引用的图片 `masthead.png` 位于你将保存所有图片的 `images` 文件夹中。

在 `style.css` 中，你定义了如清单 2-5 所示的代码。

```css
.masthead{
    background: url("img/masthead.png") no-repeat center;
    height: 462px;
    background-size: cover;
}
```

**清单 2-5** 插入头部图片

请记住，清单 2-5 中显示的代码位于名为 `style.css` 的自定义 CSS 样式表中。你将高度设置为 `462px` 并居中图片。你还为 `background-size` 属性分配了值 `cover`；这会缩放背景图像，使其尽可能大，从而完全覆盖背景区域。

现在执行 `index.html` 页面，你将得到如图 2-6 所示的输出。

![A450102_1_En_2_Fig6_HTML.jpg](img/A450102_1_En_2_Fig6_HTML.jpg)

**图 2-6** 头部图片的输出

### 步骤 3: 定义自由职业门户

接下来，您将创建页面的后续部分，用于定义自由职业者门户的功能。要创建此部分，让我们查看代码清单 2-6 中的代码片段。

```html
<div class="row rewardingContent">
    <div class="six columns">
        <h3>Plenty of rewarding projects</h3>
        <p>RemoteDesk is a great place to find more clients, and to run and grow your own freelance business.</p>
        <ul>
            <li><strong>Freedom to work on ideal projects.</strong> On RemoteDesk, you run your own business and choose your own clients and projects. Just complete your profile and we’ll highlight ideal jobs. Also search projects, and respond to client invitations.</li>
            <li><strong>Wide variety and high pay.</strong> Clients are now posting jobs in hundreds of skill categories, paying top price for great work.</li>
            <li><strong>More and more success.</strong> The greater the success you have on projects, the more likely you are to get hired by clients that use Upwork.</li>
        </ul>
    </div>
    <div class="six columns">
        <img src="img/medal.png" class="rewardingImg"/>
    </div>
</div>
```

### 代码清单 2-6：定义“奖励”部分的内容区域

在代码清单 2-6 中，您创建了另一个带有 `row` 类的 `div`。您还为其分配了一个自定义类 `rewardingContent`。然后，您将页面的这一部分划分为两行，每行宽度为六列。带有 `row` 类的 `div` 内的第一个 `div` 使用 `six columns` 类分配了六列的宽度。您在其中分配了一个标题。您使用 `<ul>` 标签创建了一个列表，并使用 `<li>` 标签定义了列表项。然后，您创建了第二个跨越六列的 `div`，在其中使用 `<img>` 标签插入了一张图片。您还为其添加了一个自定义类 `rewardingImg`。

接下来，您可以在 `style.css` 样式表中查看如何处理自定义类 `rewardingContent` 和 `rewardingImg`，如代码清单 2-7 所示。

```css
.rewardingContent {
    margin-top:10px;
}
.rewardingImg {
    width: 75%;
    margin: 0 auto;
    display: block;
}
```

### 代码清单 2-7：定义与“奖励”部分相关的样式

在此代码中，您为 `rewardingContent` 类添加了一个 `margin`，以设置边框周围的空白。您对 `rewardingImg` 使用了 `display: block`，使其占据父元素的空间。（您需要使用 `display: block` 属性，因为 Skeleton 没有任何用于响应式图像的实用程序类。）您还为图像定义了宽度，并使用 `margin: 0 auto` 将其居中。

图 2-7 显示了在步骤 3 中创建的页面后续部分。

![A450102_1_En_2_Fig7_HTML.jpg](img/A450102_1_En_2_Fig7_HTML.jpg)

### 图 2-7：“奖励”部分的输出

### 步骤 4：完成各部分

Next, you will create the remaining three sections using the code in `Listing 2-8`.

```html
<div class="row hiredContent">
    <div class="six columns">
        <img src="img/hire-resources-icon.png" class="hiringImg"/>
    </div>
    <div class="six columns">
        <h3>Get hired quickly</h3>
        <p>RemoteDesk makes it easy to connect with clients and begin doing great work.</p>
        <ul>
            <li><strong>Streamlined hiring.</strong> RemoteDesk's sophisticated algorithms highlight projects you're a great fit for.</li>
            <li><strong>Top Rated and Rising Talent programs.</strong> Enjoy higher visibility with the added status of prestigious programs.</li>
            <li><strong>Do substantial work with top clients.</strong> RemoteDesk pricing encourages freelancers to use Upwork for repeat relationships with their clients.</li>
        </ul>
    </div>
</div>
<!-- Hired content end -->
<!-- work efficiency start-->
<div class="row workEfficiency">
    <div class="six columns">
        <h3>Work efficiently, effectively.</h3>
        <p>With Upwork, you have the freedom and flexibility to control when, where, and how you work. Each project includes an online workspace shared by you and your client, allowing you to:</p>
        <ul>
            <li><strong>Send and receive files.</strong> Deliver digital assets in a secure environment.</li>
            <li><strong>Share feedback in real time.</strong> Use Upwork Messages to communicate via text, chat, or video.</li>
            <li><strong>Use our mobile app.</strong> Many features can be accessed on your mobile phone when on the go.</li>
        </ul>
    </div>
    <div class="six columns">
        <img src="img/Messaging.png" class="messagingImg"/>
    </div>
</div>
<!-- Work efficiency end-->
<!-- Get paid section start -->
<div class="row getPaid">
    <div class="six columns"><img src="img/paid.png" class="paidImg"/></div>
    <div class="six columns">
        <h3>Get paid on time</h3>
        <p>All projects include Upwork Payment Protection — helping ensure that you get paid for all work successfully completed through the freelancing website.</p>
        <ul>
            <li><strong>All invoices and payments happen through RemoteDesk.</strong> Count on a simple and streamlined process.</li>
            <li><strong>Hourly and fixed-price projects. For hourly work, submit timesheets through RemoteDesk.</strong> For fixed-price jobs, set milestones and funds are released via Upwork escrow features.</strong>
            <li><strong>Multiple payment options.</strong> Choose a payment method that works best for you, from direct deposit or PayPal to wire transfer and more.</li>
        </ul>
    </div>
</div>
```

## `Listing 2-8` Adding Content to the Remaining Sections

As shown in `Listing 2-8`, you create three rows using three `<div>`s with the `row` class. In the first `<div>` containing the `row` class, you create two `<div>`s each spanning six columns. In the first child `<div>`, you insert an image using the `<img>` tag, and in the second child `<div>`, you create a heading followed by creating a list using the `<ul>` and `<li>` tags. In short, you create the next three content blocks using a similar technique as you did in Step 3.

Similarly, you create a similar structure for the remaining two `<div>`s with the `row` class by inserting two child `<div>`s, each spanning six columns. You also insert an image and create an unordered list similar to the previous `<div>`s.

Then, you define the CSS styles for the custom CSS code in the `style.css` style sheet just like you did in Step 3. `Listing 2-9` shows the CSS style sheet code.

```css
.rewardingContent,.hiredContent,.workEfficiency,.getPaid{
    margin-top:10px;
}
.rewardingImg,.hiringImg,.messagingImg,.paidImg {
    width: 75%;
    margin: 0 auto;
    display: block;
}
```

## `Listing 2-9` Adding Styles for the Remaining Sections

`Figure 2-8` shows the output of the code in Step 4.

![A450102_1_En_2_Fig8_HTML.jpg](img/A450102_1_En_2_Fig8_HTML.jpg)

## `Figure 2-8` Output of the content of the remaining sections

## 步骤 5：设计注册表单

在本步骤中，你将创建一个简单的注册表单。清单 2-10 展示了该表单的代码。

```html
<h3 class="row">Ready to get hired?</h3>
<div class="row quickSignup">
    <div class="five columns"><input type="text" name="fullName" class="fullName u-full-width" id="fullName" placeholder="Enter your full name"/></div>
    <div class="five columns"><input type="text" name="emailId" class="emailId u-full-width" id="emailId" placeholder="Enter your email"/></div>
    <div class="two columns"><a class="button button-primary" href="#">Sign up</a></div>
</div>
```

**清单 2-10** 注册表单

在清单 2-10 中，你创建了一个 `<div>` 并为其分配了 `row` 类。然后在带有 `row` 类的 `<div>` 内添加了三个带有 `columns` 类的 `<div>`。接着在前两个 `<div>` 中添加了用于输入全名和电子邮件的输入框。这里你使用了 Skeleton 提供的一个实用类；例如，`u-full-width` 用于使字段占据容器的全部宽度。最后，你在最后一个 `<div>` 中放置了一个注册按钮。

图 2-9 展示了该表单的输出效果。

![A450102_1_En_2_Fig9_HTML.jpg](img/A450102_1_En_2_Fig9_HTML.jpg)

**图 2-9** 注册表单的输出效果

## 步骤 6：创建页脚

最后，你将创建页脚。清单 2-11 展示了页脚部分的代码。

```html
<div class="row footer">
    <div class="four columns">
        <h4>Company Info</h4>
        <a class="column">About us</a>
        <a class="column">Customer Stories</a>
        <a class="column">Press</a>
        <a class="column">Career</a>
        <a class="column">RemoteDesk Blog</a>
        <a class="column">Terms of service</a>
        <a class="column">Privacy Policy</a>
    </div>
    <div class="four columns">
        <h4>Additional Services</h4>
        <a class="column">Enterprise Solutions</a>
        <a class="column">Enterprise Summit</a>
        <a class="column">Business resources</a>
    </div>
    <div class="four columns">
        <h4>Browse</h4>
        <a class="column">Freelancers by skills</a>
        <a class="column">Freelancers by region</a>
        <a class="column">Find Jobs</a>
        <a class="column">Hiring Resources</a>
    </div>
</div>
```

**清单 2-11** 页脚

在清单 2-11 中，你创建了一个 `<div>` 标签并为其分配了 `row` 类。在该 `<div>` 内，你创建了三个子 `<div>`，每个都使用 `four columns` 类跨越四列。第一个子 `<div>` 包含用于公司信息的锚点链接 `<a>`。第二个子 `<div>` 包含用于附加服务的锚点链接，而第三个子 `<div>` 包含用于“浏览”部分的锚点链接。

图 2-10 展示了网页的页脚部分。

![A450102_1_En_2_Fig10_HTML.jpg](img/A450102_1_En_2_Fig10_HTML.jpg)

**图 2-10** 页脚部分的输出效果

你刚刚使用 Skeleton 为 RemoteDesk 自由职业者门户设计了一个着陆页！

## 摘要

Skeleton 是一个初学者可以快速上手的简单框架。它拥有简洁明了的代码库。然而，Skeleton 也有其缺点。

*   它缺少其他框架提供的几个 CSS 特性。正因为如此，使用 Skeleton 的开发时间比同类产品要长。
*   12 列流体网格支持的最大宽度为 960 像素。
*   另一个缺点是缺乏社区支持。这个框架的最后一次更新是在三年前。此外，许多“拉”请求和问题仍然是开放的，这意味着所有的差异都还没有随着实质性的更新而得到解决。

因此，当涉及到大型、沉浸式的网站时，Skeleton 显得力不从心。尽管如此，它仍然是一个方便的实用工具，适用于主要针对较小屏幕的 Web 项目。在下一章中，你将使用另一个直观的轻量级框架 Milligram 来设计一个产品页面。