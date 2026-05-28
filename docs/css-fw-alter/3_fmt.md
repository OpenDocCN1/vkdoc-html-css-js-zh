# 三、使用 Milligram 建立产品页面

## 安装 Milligram

There are different ways you can install Milligram. In this section, you will learn how to install Milligram by downloading the Milligram files. Go to the Milligram web site at `milligram.io/` and click the Download Milligram button, as highlighted in Figure 3-1. The zip file will be downloaded.

![A450102_1_En_3_Fig1_HTML.jpg](img/A450102_1_En_3_Fig1_HTML.jpg)
Figure 3-1 Milligram download page

Figure 3-2 shows the file structure.

![A450102_1_En_3_Fig2_HTML.jpg](img/A450102_1_En_3_Fig2_HTML.jpg)
Figure 3-2 Content of the Milligram framework

The CSS files (both the usual ones as well as the minified versions) are present in the `dist` folder. Figure 3-3 shows the file structure.

![A450102_1_En_3_Fig3_HTML.jpg](img/A450102_1_En_3_Fig3_HTML.jpg)
Figure 3-3 File structure of Milligram framework (source: Milligram)

You can also download the Milligram files or install it using Bower, NPM, or Yarn. For installation through Bower, NPM, and Yarn, you need to use the following commands from the command-line prompt:

```
$ bower install milligram
$ npm install milligram
$ yarn add milligram
```

Once you download Milligram, add the tags for the files in the head section of your HTML code. There is a preferred way of using Milligram that we will be showing in this chapter, which is to use a content delivery network (CDN). A CDN is basically a system of distributed networks delivering web pages and other web content according to the geographic location of the users, the source of the web pages, and the location of the CDN server. There are many benefits of using CDN.

*   降低服务器负载
*   实现快速内容交付
*   确保高可用性
*   促进高并发网络主干容量
*   更好地控制资产交付

You can add the CDN code for Milligram using the following lines of code:

```
<link href="https://fonts.googleapis.com/css?family=Roboto" rel="stylesheet">
<link href="https://cdnjs.cloudflare.com/ajax/libs/milligram/1.3.0/milligram.min.css" rel="stylesheet" type="text/css"/>
```

The first line of code is the CDN link for the Google Roboto font. The second line of code is for the Milligram CSS minified file.

## Milligram 框架概述

In this section, you will get an overview of the various attributes of Milligram before you learn how to build a product page with the framework.

*   在字体设计上，Milligram 坚持 CSS3 的 `rem` 单位思想，为根元素定义了一个单一的字体大小，然后所有其他的 `rem` 单位都是该根元素的一个百分比，从而提供了更容易的可维护性和更简洁的代码。顺便说一下，Milligram 使用 `Roboto` 字体系列作为其排版的默认字体。
*   以 Milligram 为单位的块引号被引用在 `>` 标记之间。在 `<code>` 标签之间定义了一个代码元素。如果你需要一个块，在 `<pre>` 标签中包装元素。
*   clearfix 实用程序与 `clearfix` 类一起使用，而 float 由 `float-left` 和 `float-right` 类定义，这取决于您是否希望分别向左和向右浮动。
*   与 HTML 类似，以 Milligram 为单位的列表在 `<ul>`、`<ol>` 和 `<dl>` 标签中分别定义为有序列表、无序列表和描述列表。每个列表项都被包装在类似 HTML 的 `<li>` 标签之间。
*   按钮由 `button` 类定义，带有锚标记。
*   以 Milligram 为单位的表格类似于 HTML 表格，其中使用了 `<table>` 和 `<tr>` 元素。与 HTML 类似，使用 `<tr>` 定义表格行，`<td>` 定义表格数据，`<th>` 定义表格标题，然后将所有内容包装在主 `<table>` 元素中。
*   和 Skeleton 一样，Milligram 使用以最小宽度为目标的移动优先查询。查询之外的样式适用于所有设备。这样做是为了防止手机和平板电脑等小型设备解析大量未使用的 CSS。Milligram 根据设备大小使用以下媒体查询大小:
    *   比移动设备/屏幕大: `40rem` (`640` 像素)
    *   大于平板设备/屏幕: `80rem` (`1280` 像素)
    *   大于桌面设备/屏幕: `120rem` (`1920` 像素)

## Milligram 网格系统

# Milligram 网格系统

Milligram 中的网格使用 CSS Flexible Box Layout 模块标准，其中网格是流动的，会在较小尺寸下根据浏览器缩小。整个网格系统是响应式的，最大宽度为 112rem（1120px）。请参阅清单 3-1 以了解 Milligram 中的网格系统。

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Grid system</title>
  <meta name="description" content="">
  <meta name="author" content="">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link href="https://cdnjs.cloudflare.com/ajax/libs/milligram/1.3.0/milligram.min.css" rel="stylesheet" type="text/css">
  <link href="https://fonts.googleapis.com/css?family=Roboto" rel="stylesheet">
</head>
<br>
<div class="container">
  <div class="row">
    <div style="text-align:center; border: 1px solid black;" class="column">One</div>
    <div style="text-align:center; border: 1px solid black;" class="column">Two</div>
    <div style="text-align:center; border: 1px solid black;" class="column">Three</div>
    <div style="text-align:center; border: 1px solid black;" class="column">Four</div>
  </div>
  <br>
  <div class="row">
    <div style="text-align:center; border: 1px solid black;" class="column">One</div>
    <div style="text-align:center; border: 1px solid black;" class="column column-50 column-offset-25">Two</div>
  </div>
</div>
</html>
```

**清单 3-1** Milligram 中的网格系统

在代码中，你使用了 Google 字体和 Milligram 压缩 CSS 文件的 CDN 链接。你还定义了视口。与 Skeleton 类似，整个代码被包装在一个带有 `container` 类的 `<div>` 元素中。所有列都在一个 `row` 中定义，就像许多基于网格的框架一样。然而，Milligram 与其他框架的不同之处在于，你可以在一个 `row` 中添加任意数量的列。你不受像许多流行框架那样 12 列的限制。

在清单 3-1 中，你首先在带有 `container` 类的 `<div>` 内定义了一个 `row`。然后，你使用 `column` 类在该行中定义了四个列。你使用内联样式为每个列分配边框并将文本居中对齐。然后你定义了另一个 `row` 和一个使用 `column` 类的列。接着你在该行中定义了另一个列，并使用了 `column-50` 类以及 `column-offset-25` 类。`column-50` 类的作用是为该列分配 50% 的宽度，这将在父 `row` 内分配 50% 的列内容。`column-offset-25` 类将该列向右移动父 `row` 列空间的 25%。

图 3-4 显示了代码的输出。

![A450102_1_En_3_Fig4_HTML.jpg](img/A450102_1_En_3_Fig4_HTML.jpg)

**图 3-4** Milligram 网格系统的实现

在图 3-4 中，你可以看到第一行中有四个列，分别命名为 One、Two、Three 和 Four。第二行有一个名为 One 的列和一个名为 Two 的列，后者偏移了 25% 并占据了行宽的 50%。

## 用 Milligram 建立产品页面

现在你对 Milligram 有了一个大致的了解，你将学习如何使用 Milligram 创建一个产品页面。产品页面包含有关虚拟专用网络（VPN）的信息及其功能和定价。

### 步骤 1:定义标题

## 构建安全VPN产品页面

让我们从代码清单 3-2 开始，构建安全VPN产品页面的第一步。

```html
<!DOCTYPE html>
<html>
    <head>
        <title>Secure VPN</title>
        <!--include milligram via cdn-->
        <meta name="viewport" content="width=device-width, initial-scale=1">
        <link href="https://cdnjs.cloudflare.com/ajax/libs/milligram/1.3.0/milligram.min.css" rel="stylesheet" type="text/css">
        <link href="https://fonts.googleapis.com/css?family=Roboto" rel="stylesheet">
        <link href="css/style.css" rel="stylesheet" type="text/css">
    </head>
    <body class="container">
        <div class="row contactArea">
            <!--Div with width of 50% and offset from left 50%-->
            <div class="column column-50 column-offset-50">
                <div class="contactColumn">Your IP: 115.166.129.152</div>
                <div class="contactColumn">Your Location: Unknown</div>
                <div class="contactColumn">Your Status: UNPROTECTED</div>
            </div>
        </div>
    </body>
</html>
```

**代码清单 3-2 定义头部**

在代码清单 3-2 中，你定义了视口并添加了字体和 Milligram CSS 压缩文件的 CDN 链接。然后，你引入了自定义 CSS 文件 `style.css` 的链接。在 `<head>` 标签中定义好链接后，你创建了一个带有 `container` 类的 `<body>` 元素。接着，你定义了一个带有 `row` 类的 `<div>`，并为其分配了另一个名为 `contactArea` 的自定义类，你将使用它来定义自定义 CSS 代码。接下来，你使用 `column-50` 和 `column-offset-50` 类，在父级 `row` 内定义了一个宽度为 50% 的 `<div>`，并在该行内将其偏移了 50%。在这个 `<div>` 内，你创建了三个 `<div>`，其中定义了包含 IP 地址、位置和状态的内容。然后，你在自定义的 `style.css` 文件中使用 `contactArea` 和 `contactColumn` 类定义了自定义 CSS 代码。

代码清单 3-3 展示了 `style.css` 自定义 CSS 文件中的代码，该文件使用 `contactArea` 和 `contactColumn` 类为对应的 `<div>` 定义了样式。

```css
.contactArea{
    background: #666;
    color: #fff;
    text-align: right
}
.contactColumn{
    font-size: 12px;
    display: inline-block;
    margin-right: 10px;
}
```

**代码清单 3-3 定义头部的CSS**

在代码清单 3-3 中，你为 `contactArea` 类定义了灰色背景和白色文字颜色，并将文本右对齐。对于由 `contactArea` 类定义的 `<div>`，你将字体大小定义为 `12px` 并使用了外边距。你使用了 `display: inline-block` 属性，它本质上创建了一个填充浏览器宽度并换行的方框网格。在这里，它有助于头部的内容块保留其块级特性，并帮助它们彼此相邻显示，而无需使用 `float` 属性。

图 3-5 展示了代码的输出结果。

![A450102_1_En_3_Fig5_HTML.jpg](img/A450102_1_En_3_Fig5_HTML.jpg)

**图 3-5 头部区域的输出**

在图 3-5 中，你可以看到元素向右浮动，显示了 IP 地址和其余内容。

### 步骤 2:定义导航

Let’s now look at the code in Listing 3-4 to proceed with step 2.

```html
<div class="navigation row">
    <div class="column column-25 logo">
        <img src="img/logo.png"/>
    </div>
    <div class="column column-50 column-offset-25">
        <a>Home</a>
        <a>Pricing</a>
        <a>Support</a>
        <a>Login</a>
    </div>
</div>
```

**Listing 3-4** Defining the Navigation

In Listing 3-4, you create a `<div>` and assign a `row` class to it. You also assign a custom `navigation` class to it, wherein you will define the custom CSS code in the `style.css` style sheet. You then create a `<div>` within the `<div>` with the `row` class and assign a column width of 25 percent to it by using the `column-25` class. You also add a `logo` custom class to it. You then insert an image for that `<div>` using the `<img>` element. The path to the images is set to the `images` folder, with the `logo.png` as the image name. Within the same row, you create another `<div>` and assign a column width of 50 percent to it using the `column-50` class for that parent row, and you offset that column by 25 percent to the right. You define the content in anchor link, `<a>`, tags.

Listing 3-5 shows the custom CSS code linked to the code in Listing 3-4.

```css
.logo {
    text-align: left;
}
.logo img {
    width: 25%;
    margin: 10px 0;
}
.navigation{
    background: #ffc400;
    text-align: right;
    padding: 10px 0;
    font-weight: bold;
}
.navigation a{
    color: #000;
    padding: 5px;
    border: 2px solid #000;
}
```

**Listing 3-5** Defining the CSS for the Navigation

As you can see in Listing 3-5, you define an image width of 25 percent and set a margin for it. In the `navigation` class, you define dark orange as the background color and align the text to the right. You set the padding and define the bold font weight for it. To the anchor links containing the Home, Pricing, Support, and Login links, you assign the black color and a black border with a padding of `5px`.

Figure 3-6 shows the output of the code. In Figure 3-6, you can see the orange background and the links in the anchor tags (i.e., Home, Pricing, Support, and Login) to the right of the screen. You can also see the logo on the left of the screen.

![A450102_1_En_3_Fig6_HTML.jpg](img/A450102_1_En_3_Fig6_HTML.jpg)

**Figure 3-6** Output of the navigation

## 步骤 3:定义横幅区域

Let’s look at the code in Listing 3-6 to see the next step in the coding process for the secure VPN product page.

## Listing 3-6: HTML for the Banner Area

```html
<section class="mastHead row">
    <div class="column column-60">
        <h2>Secure your data. Protect your privacy</h2>
        <h4>Protect your IP address and surf the web anonymously</h4>
    </div>
</section>
```

In Listing 3-6, you define the `<section>` tags and assign the `row` class as well as the custom `mastHead` class to it. Inside that row, you define a `<div>` with a column of 60 percent width for the row for the `<section>` tag.

Listing 3-7 shows the corresponding code for the `mastHead` class in the custom `style.css` style sheet.

## Listing 3-7: CSS for the Banner Area

```css
.mastHead {
    height: 450px;
    overflow: hidden;
    background: #ffc400;
    color: #000;
}

.mastHead h1,.mastHead h2,.mastHead h3,.mastHead h4,.mastHead h5,.mastHead h6{
    color: #000
}
```

What you have done is set the height of the `<section>` with the `mastHead` class to `450px` and set the background to dark orange, the same color as in step 2. Then, you define black color to the content in that section. You also set the color of all the headings in that section to black with the `mastHead` class.

Now you will split the sprite image shown in Figure 3-7 into three parts for the App Store, Google Play, and Windows Phone Store. The rest of the image for the Mac App Store and Windows PC will not be displayed on the page.

![A450102_1_En_3_Fig7_HTML.jpg](img/A450102_1_En_3_Fig7_HTML.jpg)
Figure 3-7: Sprite image for store icons

You define the code for the unordered list in Listing 3-8 within which you assign a column width of 60 percent, after the headings.

## Listing 3-8: Adding Store Information to the Banner Area

```html
<section class="mastHead row">
    <div class="column column-60">
        <h2>Secure your data. Protect your privacy</h2>
        <h4>Protect your IP address and surf the web anonymously</h4>
        <ul class="srote-badges">
            <li><a class="store-ios" title="Available on the App Store"></a></li>
            <li><a class="store-android" title="Get it on Google Play"></a></li>
            <li><a class="store-winphone" title="Download from Windows Phone Store"></a></li>
        </ul>
    </div>
</section>
```

As you can see from the code in Listing 3-8, you define the unordered list and assign the custom `srote-badges` class to it. You define the list in the anchor tags and assign the `store-ios`, `store-android`, and `store-winphone` custom classes to it.

The corresponding custom CSS code in the `style.css` style sheet for the unordered list will look like Listing 3-9.

## Listing 3-9: Adding the CSS for Store Icons

```css
ul.srote-badges{
    list-style: none;
}

ul.srote-badges li a, .srote-badges a {
    display: inline-block;
    background: url(img/store-badges-70x245.png) no-repeat 0 0 #fff;
    width: 245px;
    height: 70px;
    border-radius: 4px;
}

.srote-badges a.store-ios {
    background-position: 0 0;
}

.srote-badges a.store-android {
    background-position: 0 -70px;
}

.srote-badges a.store-winphone {
    background-position: 0 -140px;
}
```

In Listing 3-9, you set `list-style` as `none` to remove the bullets. Further, you set the background as the sprite image by assigning the link to that image. Then, you define the width and height for it. You also assign a `border-radius` setting of `4px` to the image. Next, you split the image into the first three parts and set the `background-position` to `0` for the first part, `-70px` for the next part, and `-140px` for the third. The rest of the image cannot be seen.

Next you define an Android phone image, as shown in Figure 3-8, to the right using another `<div>` within the same section class.

![A450102_1_En_3_Fig8_HTML.jpg](img/A450102_1_En_3_Fig8_HTML.jpg)
Figure 3-8: Application image for the banner area

You assign a column width of 40 percent for the parent `<section>` tag and add the `mastHeadImage` class to it. Listing 3-10 shows the code within the entire `<section>` tags after incorporating everything from the sprite images into this Android image.

## Listing 3-10: Adding the Application Image to the Banner Area

```html
<section class="mastHead row">
    <div class="column column-60">
        <h2>Secure your data. Protect your privacy</h2>
        <h4>Protect your IP address and surf the web anonymously</h4>
        <ul class="srote-badges">
            <li><a class="store-ios" title="Available on the App Store"></a></li>
            <li><a class="store-android" title="Get it on Google Play"></a></li>
            <li><a class="store-winphone" title="Download from Windows Phone Store"></a></li>
        </ul>
    </div>
    <div class="column column-40 mastHeadImage"></div>
</section>
```

Listing 3-11 shows the corresponding custom CSS code for the `<div>` element with the last `mastHeadImage` class.

## Listing 3-11: Adding the CSS for the Application Image in the Banner Area

```css
.mastHeadImage{
    background: url("img/android-device1.png") no-repeat;
    background-size: cover;
    background-position: 0 15px;
}
```

In Listing 3-11, you refer to the background and assign the image link. Then you set `background-size` to `cover` and set `background-position` as `15px`.

Figure 3-9 shows the output of the entire code so far.

![A450102_1_En_3_Fig9_HTML.jpg](img/A450102_1_En_3_Fig9_HTML.jpg)
Figure 3-9: Code output so far

### 步骤 4:设计内容区域

Moving Forward, you will design the content area. `Listing 3-12` shows how to proceed with building the “benefits” section.

```html
<section class="info">
    <div class="row">
        <h3 class="column">BENEFITS OF USING VPN IN TOUCH</h3>
    </div>
    <div class="row">
        <div class="column column-50">
            <h4>Unblock Websites</h4>
            <p>Bypass internet restriction and access to any websites: Unblock Facebook, Unblock Youtube.</p>
        </div>
        <div class="column column-50">
            <h4>Secure Your Data</h4>
            <p>Encrypt your private data before sending it from your computer, smartphone or tablet over the internet.</p>
        </div>
    </div>
    <div class="row">
        <div class="column column-50">
            <h4>Bypass content restrictions</h4>
            <p>Watch Netflix and BBC iPlayer, no matter where you are. Use Skype, Viber and all Voip services without restrictions.</p>
        </div>
        <div class="column column-50">
            <h4>Protect Your Privacy</h4>
            <p>Hide your IP address, protect your online identity while browsing and surf the web anonymously.</p>
        </div>
    </div>
    <div class="row">
        <div class="column column-50">
            <h4>Wifi Hotspot Security</h4>
            <p>Prevent sniffers and hackers from stealing your private data while using public hotspots.</p>
        </div>
        <div class="column column-50">
            <h4>Data Saving and Ad Blocker on Mobile</h4>
            <p>Save more bandwidth on your mobile 3G/4G data plan. Clear your mobile screen of obtrusive ads with Ad Blocking mode.</p>
        </div>
    </div>
</section>
```

`Listing 3-12` HTML for the Content Area

In `Listing 3-12`, you use a `<section>` tag and enclose a `<div>` with a `row` class. Within that parent row, you use the `<h3>` heading to define the content for the level 3 heading. After that `<div>`, you create a `<div>` with the `row` class. You create two `<div>`s each with a column width of 50 percent of the parent `<div>` using the `column-50` class. You define a level 4 `<h4>` and a paragraph element, `<p>`, with their respective content within each child `<div>`. Repeat the process three more times, wherein you create two `<div>`s within a parent `<div>` with a `row` class. Similarly, define `<h4>` and `<p>` under each child `<div>` with their respective content. `Figure 3-10` shows the output of the code.

![A450102_1_En_3_Fig10_HTML.jpg](img/A450102_1_En_3_Fig10_HTML.jpg)

`Figure 3-10` Output of the content area

### 步骤 5:创建定价区域

Next, you will create a pricing table wherein you will list the subscription price for the monthly, half-yearly, and yearly timeline. Let’s look at the code in Listing 3-13.

```html
<section class="pricingInfo">
    <div class="row">
        <h3 class="column">Pricing Overview</h3>
    </div>
    <div class="row">
        <div class="column">
            <table>
                <tbody>
                    <tr>
                        <td><strong>pricing</strong></td>
                        <td>1 Month</td>
                        <td>6 Months</td>
                        <td>1 Year</td>
                    </tr>
                    <tr>
                        <td><strong>Price</strong></td>
                        <td>$9.98/month</td>
                        <td>$2.99/month</td>
                        <td>$2.49/month</td>
                    </tr>
                    <tr>
                        <td><strong>Save</strong></td>
                        <td>0%</td>
                        <td>50%</td>
                        <td>75%</td>
                    </tr>
                </tbody>
            </table>
        </div>
    </div>
</section>
```

Listing 3-13: HTML for the Pricing Area

In Listing 3-13, you define the `<section>` tags within which you define the tables. Initially, you define a `<div>` with a `row` class where you use a heading of `<h3>` with the `column` class to define the content, i.e., Pricing Overview. Then, you create a `<div>` with the `row` class after the preceding `<div>` and assign another `<div>` with the `column` class within it. Moving forward, you define the table headings and the table rows with the list of items in the table, which is quite similar to the way you create tables in HTML. Place the content within the `<section>` tags. Figure 3-11 shows the output of the code.

![A450102_1_En_3_Fig11_HTML.jpg](img/A450102_1_En_3_Fig11_HTML.jpg)

Figure 3-11: Output of the pricing area

### 步骤 6: 创建页脚

Finally, you will create a footer for your product page. Let’s look at the code in Listing 3-14 to understand how you design the footer of the web page.

```html
<footer>
    <div class="row">
        <div class="column column-25 logo">
            <img src="img/logo.png"/>
            <p>&copy; Copyright 2017</p>
            <p>All rights reserved</p>
        </div>
        <div class="column column-25">
            <h6>LEARN MORE</h6>
            <a>Pricing</a><br>
            <a>How To Setup</a><br>
            <a>Servers</a><br>
            <a>Blog</a><br>
            <a>FAQ</a><br>
        </div>
        <div class="column column-25">
            <h6>LEGAL</h6>
            <a href="/legal#terms">Terms & Conditions</a><br>
            <a href="/legal#privacy">Privacy Policy</a><br>
            <a href="/legal#refund">Refund Policy</a><br>
        </div>
    </div>
</footer>
```

Listing 3-14 HTML for the Footer

In Listing 3-14, you define the footer content within the `<footer>` tags. Inside the `<footer>` tags, you initially define a `<div>` with the `row` class. Then, you create three child `<div>`s each with a `column` with a width of 25 percent of the parent row so that each child `<div>` takes a quarter of the parent row space. In the first child `<div>`, you assign a `logo` class to it. You then insert the logo image with the `<img>` tag. Then you enter the copyright information with the `<p>` tags. For this first child `<div>`, you define the custom CSS code in the `style.css` file, as shown in Listing 3-15.

```css
footer{
    color:#fff;
    background: #666;
    padding: 10px 0;
}
footer .logo img,footer .logo p{
    margin-left: 10px;
    display: block;
}
footer .logo p{
    margin-bottom: 0;
}
footer a{
    color: #fff;
}
footer h6{
    font-weight: bold;
    border-bottom: 1px solid #fff;
}
```

Listing 3-15 CSS for the Footer

As you can see in Listing 3-15, you define the color, padding, and background for the `<footer>`. You define the left margin space and `display: block;` property for the footer and the image with the `logo`. Then, you assign the color to the anchors in the footer followed by defining the bold font and solid border for the footer and `<h6>` heading. Back in Listing 3-14, you define the second child `<div>` and define the `<h6>` heading with the content, along with the links, which you define in the anchor tags. The third child `<div>` contains the conditions and policy links defined between the `<h6>` and anchor `<a>` tags. That sums up the code. Figure 3-12 shows the snapshot of the footer part of the code.

![A450102_1_En_3_Fig12_HTML.jpg](img/A450102_1_En_3_Fig12_HTML.jpg)

Figure 3-12 Output of the footer area

## 摘要

In this chapter, you designed a page for a secure VPN product. Milligram is an awesome utility for lightweight projects. However, there are some constraints such as the maximum device size of 1120px and a lack of utility classes, which are required for massive immersive projects. Nevertheless, Milligram is an intuitive framework that doesn’t come with the bulk of huge frameworks and is especially helpful when you want to build simple mobile web pages. In the next chapter, you will look at another engaging framework, UIkit, which is quite useful for lightweight web projects.