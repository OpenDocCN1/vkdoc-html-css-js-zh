# 十五、第四章的示例内容

清单 A-1 指定了用于第四章中的练习的初始 HTML 内容。这可以从下载源的`Default_content.cshtml`文件中获得。我把它放在这里，以防你不下载代码就想看。

## Listing A-1. Chapter 4 Sample Content

```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8" />
<title>Chapter 4</title>
</head>
<body>
<header class="intro">
<h1>CSS Demo</h1>
<h2>Introducing the new HTML5 features</h2>
<h3>
Use the new CSS3 features to build some of the most visually
appealing web sites.
</h3>
</header>
<nav>
<ul>
<li><a href="#feature">Feature</a></li>
<li><a href="#other">Article</a></li>
<li><a href="#another">Archives</a></li>
<li><a href="http://www.apress.com">Apress</a></li>
</ul>
</nav>
<div id="contentArea">
<div id="mainContent">
<section class="rounded">
<header>
<h2>Main content area</h2>
</header>
<p>
Lorem ipsum dolor sit amet, consectetur adipisicing elit,
sed do eiusmod tempor incididunt ut labore et dolore magna
aliqua. Ut enim ad minim veniam, quis nostrud exercitation
ullamco laboris nisi ut.
</p>
</section>
<section>
<article class="featuredContent">
<a id="feature"></a>
<header>
<h3>Featured Article</h3>
</header>
<div class="rotateContainer">
<p>This is really cool...</p>
<img class="rotate" id="phone"
src="img/phonebooth.jpg"
alt="phonebooth"
onclick="toggleAnimation()"/>
<br />
<p>
Lorem ipsum dolor sit amet, consectetur adipisicing
elit, sed do eiusmod tempor incididunt ut labore et
dolore magna aliqua. Ut enim ad minim veniam, quis
nostrud exercitation ullamco laboris nisi ut.
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipisicing
elit, sed do eiusmod tempor incididunt ut labore et
dolore magna aliqua. Ut enim ad minim veniam, quis
nostrud exercitation ullamco laboris nisi ut.
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipisicing
elit, sed do eiusmod tempor incididunt ut labore et
dolore magna aliqua. Ut enim ad minim veniam, quis
nostrud exercitation ullamco laboris nisi ut.
</p>
</div>
</article>
<article class="otherContent">
<a id="other"></a>
<header>
<h3>Rounded Borders</h3>
</header>
<div>
<p>Details about rounded corners</p>
<p>
One of the most common features that you'll hear
about is the use of rounded corners and we'll cover
that here. Also, by configuring the div size and
radius properly you can also make circular divs
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipisicing
elit, sed do eiusmod tempor incididunt ut labore et
dolore magna aliqua. Ut enim ad minim veniam, quis
nostrud exercitation ullamco laboris nisi ut.
</p>
</div>
</article>
<article class="otherContent">
<a id="another"></a>
<header>
<h3>Another Interesting Article</h3>
</header>
<div>
<p>More things to say...</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing
elit. Proin luctus tincidunt justo nec tempor.
Aliquam erat volutpat. Fusce facilisis ullamcorper
consequat. Vestibulum non sapien lectus. Nam mi
augue, posuere at tempus vel, dignissim vitae nulla.
Nullam at quam eu sapien mattis ultrices. Quisque
quis leo mi, at lobortis dolor. Nullam scelerisque
facilisis placerat. Fusce a augue erat, malesuada
euismod dui. Duis iaculis risus id felis volutpat
elementum. Fusce blandit iaculis quam a cursus.
Cras varius tincidunt cursus. Morbi justo eros,
adipiscing ac placerat sed, posuere et mi.
Suspendisse vulputate viverra aliquet. Duis non
enim a nibh consequat mollis ac tempor lorem.
Phasellus elit leo, semper eu luctus et, suscipit
at lacus. In hac habitasse platea dictumst. Duis
dignissim justo sit amet nulla pulvinar sodales.
</p>
</div>
</article>
</section>
</div>
<aside id="sidebar">
<h3>Other Titles</h3>
<div id="moon"></div>
<p>
Check out some of the other titles available from Apress.
</p>
</aside>
</div>
</body>
</html>
```

# 书籍推荐

## Pro Office 365 Development
[![Pro Office 365](images\office365.png)](http://www.apress.com/9781430240747)
*Pro Office 365 Development* 是一本实用的、动手实践的指南，教你如何使用 Office 365 平台构建基于云的解决方案。

## Pro Access 2010 Development
[![Pro Access 2010](images\access2010.png)](http://www.apress.com/9781430235781)
*Pro Access 2010 Development* 是开发利用 Access 2010 功能的商业应用程序的基础资源。你将学习如何构建数据库应用程序、创建基于 Web 的数据库、为 Access 应用程序开发宏和 VBA 工具、将 Access 与 SharePoint 集成等等。

## Pro Project Management w/SharePoint 2010
[![Pro Project Management w/SharePoint 2010](images\sharepoint_pm.png)](http://www.apress.com/9781430228295)
本书旨在提供一个实际的案例研究，你可以按照它来使用 SharePoint Server 的开箱即用功能创建一个完整的 PMIS（项目管理信息系统）。

## Office 2010 Workflow
[![Office 2010 Workflow](images\office:workflow.png)](http://www.apress.com/9781430229049)
工作流是连接信息工作者流程、用户和工件的粘合剂——没有它，信息工作者就只是数据和潜力的孤岛。*Office 2010 Workflow* 将指导你实现工作流解决方案。

## Beginning WF
[![Beginning WF](images\beginning_wf.png)](http://www.apress.com/9781430224853)
*Beginning WF* 按功能索引，便于你轻松查找答案，并以易于理解的风格编写，展示了如何将 Microsoft 的 Workflow Foundation (WF) 技术应用于各种应用程序。

---
*最后更新于 2015年3月7日，由 [Mark J. Collins](http://www.thecreativepeople.com/) 提供。*