![](images/978-1-4842-9470-3_CoverFigure.jpg)

ISBN 978-1-4842-9469-7e-ISBN 978-1-4842-9470-3 [https://doi.org/10.1007/978-1-4842-9470-3](https://doi.org/10.1007/978-1-4842-9470-3) © Jennifer Harder 2017, 2023 Standard Apress Trademarked names, logos, and images may appear in this book. Rather than use a trademark symbol with every occurrence of a trademarked name, logo, or image we use the names, logos, and images only in an editorial fashion and to the benefit of the trademark owner, with no intention of infringement of the trademark. The use in this publication of trade names, trademarks, service marks, and similar terms, even if they are not identified as such, is not to be taken as an expression of opinion as to whether or not they are subject to proprietary rights. The publisher, the authors and the editors are safe to assume that the advice and information in this book are believed to be true and accurate at the date of publication. Neither the publisher nor the authors or the editors give a warranty, express or implied, with respect to the material contained herein or for any errors or omissions that may have been made. The publisher remains neutral with regard to jurisdictional claims in published maps and institutional affiliations.

This Apress imprint is published by the registered company APress Media, LLC, part of Springer Nature.

The registered company address is: 1 New York Plaza, New York, NY 10004, U.S.A.

Introduction

Welcome to the first step in an exciting journey called *Enhancing Adobe Acrobat Forms with JavaScript*.

My journey into learning about Adobe software began more than 20 years ago when I started college. I took a two-year graphic communications course in Vancouver, BC. While learning about how to set up documents for print layout using QuarkXPress and later Adobe InDesign, I created PDF files. At that point, I saw the PDF only as a transition from one file format to the next production step, from layout to the printing press. It never crossed my mind what else could be done with PDF files in Adobe Acrobat.

Several years after graduating, while doing freelance work for one of my clients, I began to investigate the features of Acrobat to discover what else the program had to offer. In 2008, I decided to learn more about web design and improve my layout skills in Adobe software. After finishing three certificates in web design at Langara College Continuing Studies, I realized that I wanted to help students learn more about Adobe software. There were times through the journey when I read different computer books and felt, “OK, I’ve finished this tutorial or project, but how does this relate to the real world and what I’m trying to accomplish?” In 2011, I became a teaching assistant at Langara College, and this gave me the opportunity to write my own course on introducing students to Adobe Acrobat. While writing it, I realized there was a lot more that could be said about Acrobat than what I could present in a three-evening course. At that point, I was looking at one icon in the Acrobat menu that perplexed me. It was called JavaScript.

JavaScript in Acrobat? What was that doing there? The only JavaScript that I knew about at that point was through building websites. Back then I had built a few basic template forms using LiveCycle Designer, MS Word, and Acrobat, but I had never used JavaScript in the Acrobat program. I began to wonder how JavaScript could improve my forms.

This is when and how the idea began for developing a book for students on the topic of Acrobat and JavaScript. After years of research, looking at Adobe and Acrobat forums, and studying the questions and concerns users had when trying to add JavaScript to Acrobat, I came to the following conclusions:

*   Users are looking for simple solutions to programming an Acrobat form that they will use in real-world situations. Many are looking for the same answers.

*   When documentation is not written in a simplified manner, the average user becomes intimidated. They will shy away from using the JavaScript menu and eventually give up and ignore the tool. To them, JavaScript coding is like a foreign language, and the average person who has not taken web design lessons does not have a clue what it means or where the code should be inserted, since the form field’s property dialog boxes look nothing like a web page.

*   At some point, it’s important to share with others what you have learned about Acrobat and JavaScript and not keep your thoughts to yourself. That’s what leads to innovative ideas. However, these thoughts need to be organized so that the user can find the solution quickly and be able to comprehend it. And these ideas need to be kept up-to-date.

Shortly after compiling my notes for the first edition of this book, Adobe introduced the latest version: Acrobat DC. I saw that the layout of the program had changed and there was no book to show the user how to add JavaScript in this new format. I had completed the first draft of my Advanced Adobe course and had it approved by my program coordinator at the college. In 2016, I realized I could reach a wider audience if the book was published, so I approached Apress. They saw my vision, and that is how this book came to be in your possession.

In October 2022, Acrobat has with latest releases removed the reference to Document Cloud (DC) from its name; see [`https://helpx.adobe.com/acrobat/using/whats-new.html`](https://helpx.adobe.com/acrobat/using/whats-new.html). This is the reason for the removal of “DC” from the title of the second edition.

Newer versions of Adobe Acrobat also rely on web forms, which is a new topic we will look at in Chapter [20](#449379_2_En_20_Chapter.xhtml).

## Understanding How Acrobat and Its Forms Work with JavaScript

If you are currently using Adobe Acrobat Pro DC or older versions, it’s time to upgrade to the new Adobe Acrobat Pro, which we will be using throughout this book. You can either acquire Adobe Acrobat Pro as a stand-alone program through Adobe or get a Creative Cloud subscription and enjoy all the exciting Adobe programs for a monthly fee. Refer to these links and check if your computer’s operating system meets the system requirements needed for the upgrade:

*   [`https://helpx.adobe.com/creative-cloud/system-requirements.html`](https://helpx.adobe.com/creative-cloud/system-requirements.html)

*   [`https://helpx.adobe.com/acrobat/system-requirements.html`](https://helpx.adobe.com/acrobat/system-requirements.html)

Make sure to follow the online instructions and tutorials for installing and working with Acrobat and Creative Cloud.

If you already have Adobe Acrobat Pro installed on your machine through the Creative Cloud subscription, you may notice that Acrobat Pro in the upper-left edge of the program says “32-bit,” even if the version number is 22.0 or newer, while the most recent version is 64-bit. Even with regular updates your version may not make this switch automatically.

I found that the best way to change this application’s bit from 32 to 64 is to uninstall the application from the Creative Cloud Desktop and then install the program again. Refer to Figure [I-1](#449379_2_En_BookFrontmatter_OnlinePDF.xhtml#Fig1).

![](images/449379_2_En_BookFrontmatter_Fig1_HTML.jpg)

A screenshot of the Creative Cloud desktop window. Under the Apps page is the installed Acrobat up-to-date app, followed by the disabled install option with Adobe Acrobat Pro 64-bit. The dropdown with manage plugins, learn more, and uninstall options is to the right.

Figure I-1

Installing Acrobat again will change it to the 64-bit version

The benefit of changing to the 64-bit version is if you are running a newer computer, you can ensure better performance, security, and usage of memory when opening larger files. As of January 2023, Adobe has been gradually fading out support for the 32-bit system; see [`https://helpx.adobe.com/acrobat/kb/end-of-support-acrobat-reader-32-bit-os.html`](https://helpx.adobe.com/acrobat/kb/end-of-support-acrobat-reader-32-bit-os.html).

If you are new to Adobe Acrobat, I encourage you to first read *Adobe Acrobat Classroom in a Book* by Lisa Fridsma. Her book will give you a basic overview of the new Acrobat features as well as some form basics. However, that book does not go into detail regarding forms when working with JavaScript. I consider my book to be a sequel for intermediate and advanced users to take their forms to the next level.

Adobe Acrobat Pro will allow you to add form fields to any PDF file, as I will explain further in Chapter [1](#449379_2_En_1_Chapter.xhtml). It can even work with pre-existing form fields that were created in Adobe InDesign CC when the file was exported as an interactive PDF. However, it’s important that your client views and interacts with the forms in Acrobat Pro, Standard, or Acrobat Reader. Other PDF readers, and some older browsers, have been known to corrupt the JavaScript programing when saving a document, so keep this in mind when you email the forms to clients. Make sure your clients have the latest version of Acrobat Reader, which is free, and that their browser is up-to-date as well.

Another reason some users may not be able to interact with JavaScript is that they may have disabled the use of JavaScript in the Acrobat Application under Edit ➤ Preferences Categories JavaScript.

See Figure [I-2](#449379_2_En_BookFrontmatter_OnlinePDF.xhtml#Fig2) for how your JavaScript preferences should appear in Adobe Acrobat Pro.

![](images/449379_2_En_BookFrontmatter_Fig2_HTML.jpg)

A screenshot of a window has a list of categories under preferences, in which the JavaScript option is selected. The JavaScript page has enabled and disabled options under JavaScript, security, debugger, and editor. Below are the ok and cancel buttons.

Figure I-2

Acrobat Pro Preferences menu for enabling and disabling JavaScript and security features

The following is an explanation from the Adobe website on some of the settings. See [`https://helpx.adobe.com/acrobat/using/javascripts-pdfs-security-risk.html`](https://helpx.adobe.com/acrobat/using/javascripts-pdfs-security-risk.html) for more information.

*   **Enable Acrobat JavaScript**: Uncheck this to disable JavaScript completely or restrict JavaScript through APIs. This setting by default is enabled.

*   **Enable menu items JavaScript execution privileges**: This enables executing JavaScript by clicking menu items. When off, privileged JavaScript calls can be executed through the menu. Executing nonprivileged JavaScript calls through menu items is not blocked whether this box is checked or not. This setting by default is disabled.

*   **Enable global object security policy**: This allows JavaScript globally through APIs or trusts specific documents containing JavaScript. This setting by default is enabled.

Note that you will not be working with any JavaScript that could create a security risk, so you can leave the JavaScript Security area at the default settings for these chapters as you work with the files you download. However, I will show you how to enable the JavaScript Debugger in Chapter [5](#449379_2_En_5_Chapter.xhtml) using the Preferences dialog box.

In most cases, Adobe Acrobat will autodetect in a PDF where most form fields can be added in a form, but it’s not a perfect science. It’s up to you as the author to edit and test your forms for errors as you build them and add your JavaScript code.

## What to Expect from This Book

*Enhancing Adobe Acrobat Forms with JavaScript* covers up-to-date, real working examples that you can easily download, practice with, and edit to suit your own projects. Using screenshots from Adobe Acrobat Pro, users of previous versions will also be able to utilize these techniques. This book also shows work-arounds and solutions to various form issues you might encounter. JavaScript does not need to be scary. Feel empowered by it and improve your PDF documents!

## What You’ll Learn

You’ll learn how to do the following in this book:

*   Create calculations, rating forms, and QR code stamps using the form elements

*   Understand simplified field notation and basic JavaScript for Acrobat

*   Use buttons for navigation

*   Create complex forms that include drop-down and list boxes in combination with other form fields such as check boxes and radio buttons

*   Use the Action Wizard and JavaScript

*   Improve form navigation and the printing of forms

*   Implement various types of alerts and custom validations to improve client-entered data and avoid errors

*   Get new information on Adobe Sign and how it relates to Acrobat Forms online

Note

You can find the project files for the book at [`www.apress.com/9781484228920`](http://www.apress.com/9781484228920).

Acknowledgments

Because of their patience and advice, I would like to thank the following people, for without them I could never have written this book:

*   My parents, for encouraging me to read large computer textbooks that would one day inspire me to write my own books

*   My dad, for reviewing the first draft before I sent a proposal

*   My program coordinator, Raymond Chow, at Langara College, who gave me the chance to teach evening courses and allowing me to find new and creative ways to teach software

*   My various freelance clients who I have learned from and researched more about forms and form layout through working on their projects

I would also like to thank Spandana Chatterjee and Mark Powers at Apress for showing me how to lay out a professional textbook and pointing out that even when you think you’ve written it all, there’s still more to write. Also, thanks to the technical reviewer for providing encouraging comments and to the rest of the Apress team for being involved in the printing of this book and making my dream a reality again. I am truly grateful and blessed.

About the Author About the Technical Reviewer

# Part IBasic Form Improvements

# 1. A Fundamental Forms Primer

**Chapter Goal:** Creating the right form before you add JavaScript to your PDF fields takes time and careful planning. Therefore, it’s important that you become familiar with each of the form tools and the properties that are associated with them. This chapter provides a quick overview of forms, fields, properties, and tabs that will serve as the basis for future chapters. If you are already comfortable with these topics, feel free to jump ahead to Chapter [2](#449379_2_En_2_Chapter.xhtml).

## Forms Review

Each form field has within it properties that can be accessed by right-clicking the form field. The properties of that form field are organized by tabs. Each form field has slightly different properties and therefore different tabs.

Throughout this book you will be working on lessons with a variety of different PDF forms and documents for a fictitious company called the Tourmaline Mining Corporation.

Each chapter (except for Chapters [1](#449379_2_En_1_Chapter.xhtml) and [5](#449379_2_En_5_Chapter.xhtml)) comes with JavaScript in a `.txt` file and PDF files that you can open and compare. You can either view the final PDF file or use the start PDF file and follow along with the notes in these chapters. You can find the link to these files in the introduction and then locate that specific Chapter Project folder.

Note

Please be aware that the PDF files used with this book should be opened only in Adobe Acrobat Pro, Acrobat Reader, or earlier versions of Acrobat DC and not in your web browser or any other PDF creation/reader program. Other PDF readers have been known to corrupt the JavaScript code within the Acrobat PDF files, and then the calculations fail to work.

Upon opening Acrobat Pro, make sure to check your preferences at Edit ➤Preferences ➤ Forms. Refer to Figure [1-1](#449379_2_En_1_Chapter.xhtml#Fig1). They should be set to the default settings shown in Figure [1-2](#449379_2_En_1_Chapter.xhtml#Fig2).

![](images/449379_2_En_1_Chapter/449379_2_En_1_Fig2_HTML.png)

A screenshot presents a preferences side panel that lists the options under categories with highlighted forms. A dialog box below includes general settings with check boxes, highlight color option, and auto-complete as off. An O K button is at the bottom.

Figure 1-2

Default settings for the Forms tool found in the Preferences

![](images/449379_2_En_1_Chapter/449379_2_En_1_Fig1_HTML.jpg)

A screenshot presents an Acrobat icon on the left and an open button on the right with ellipsis.

Figure 1-1

Open Acrobat Pro using your Creative Cloud Desktop App

In this case, all of the General check boxes are enabled.

*   Automatically calculate field values

*   Automatically adjust tab order when modifying fields

*   Show focus rectangle

*   Show text field overflow indicator

*   Show field preview when creating or editing form fields

*   Automatically detect Form Fields

*   Auto-enable text editing in Prepare Form

Highlight Color’s check is enabled to show border hover color for fields. Light blue is the color for highlighted fields, and red is the highlight color for required fields.

Auto-Complete is set to Off, and there is a description below it explaining what this setting means. Other options in the list are Basic or Advanced.

At this point, if these are your settings, you can click Cancel and Exit or make your settings like mine and click OK. These preferences are not saved with the forms but will help you interact with the forms as you work.

If you are working with Manage Barcode Parameter, refer to this topic later in the chapter and Chapter [19](#449379_2_En_19_Chapter.xhtml).

The book assumes you are familiar with filling in basic forms or have used PDF forms in the past. If you are unsure of how to use the Prepare Form tool and its autodetection of fields in Acrobat Pro, this section provides a refresher.

Draw your form either by hand or create the basic structure in MS Word, Adobe Illustrator, or Adobe InDesign, and then decide what steps you want the form to do and accomplish. Then plan how to execute your goals. Is what you want the form to do possible? Do you need to simplify the form? Or do you need to learn more about the topic of forms to create what you want? Make sure to consult with your team or clients as you build the form before you begin to add interactivity.

Except for the program of Adobe InDesign, you cannot assemble the form’s interactive fields outside of Acrobat, so you need to make a PDF to do that. Until you are ready to make the PDF, continue to assemble the form in your layout program such as MS Word until it looks the way it should. Then save the MS Word file (`.doc` or `.docx`). Then create the PDF. For example, in the case of MS Word, I can use my Acrobat tab to quickly create a PDF or use my File Print tab. Refer to Figure [1-3](#449379_2_En_1_Chapter.xhtml#Fig3).

![](images/449379_2_En_1_Chapter/449379_2_En_1_Fig3_HTML.jpg)

A screenshot of M S Word window that lists the options under Acrobat tab with the print dialog box. It includes the number of copies as 1, printer with Adobe P D F selected, and other printer properties.

Figure 1-3

Use the MS Word Ribbon with the Acrobat tab or the Print settings to create your PDF

Once you have the final PDF, open it in Acrobat Pro and follow these steps to add interactivity:

![](images/449379_2_En_1_Chapter/449379_2_En_1_Fig4_HTML.jpg)

A screenshot of the Acrobat page highlights the tool tab on the left with a search bar, and prepare form tool on the right includes the add tab with a dropdown.

Figure 1-4

Location of Prepare Form tool

1.  Click the Tools tab and click the Prepare Form tool, which is found in the Forms & Signatures section (see Figure [1-4](#449379_2_En_1_Chapter.xhtml#Fig4)).

![](images/449379_2_En_1_Chapter/449379_2_En_1_Fig5_HTML.jpg)

A screenshot presents a page with the highlighted single file on the left panel, that displays a document titled T M C customer survey on the right with a change file option and a start button below. A new file icon is at the bottom with an option to select a file.

Figure 1-5

The Prepare Form tool when you first create a new form

1.  You can use this tool whether you have a PDF Form open or not. While in the tool, choose, in this case, Single File, locate your file (you can change the files if the thumbnail is not correct), and make sure that form field autodetection is on. Do not select “This document requires signatures” as we will look at that option in Chapter [20](#449379_2_En_20_Chapter.xhtml) in more detail (Figure [1-5](#449379_2_En_1_Chapter.xhtml#Fig5)).

![](images/449379_2_En_1_Chapter/449379_2_En_1_Fig6_HTML.jpg)

A screenshot presents the save as P D F dialog box that lists the options under files with, your computer selected. It includes the file options, save in recent folder, and a highlighted button to choose a different folder.

Figure 1-6

Saving the new PDF form in a folder after autodetection is completed

1.  Click Start. Acrobat will scan the file for fields; if it detects any, it will create the field. However, it is not perfect in its detection, so you may have to add, delete, or edit some fields afterward.

2.  Once you have added your fields, save the file as a new PDF (Ctrl/Cmd+S). Choose to save the file on your computer and then select a folder to save the file into. Make sure that the “Convert to” option is disabled. The new PDF is now an interactive form. Refer to Figure [1-6](#449379_2_En_1_Chapter.xhtml#Fig6).

Note

You can now, with the Prepare Form tools, create a PDF form in three more ways; refer to Figure [1-5](#449379_2_En_1_Chapter.xhtml#Fig5): directly from your scanner, as a blank new document using the Create New option, or using the Create a Web form. We’ll look at that option in Chapter [20](#449379_2_En_20_Chapter.xhtml).

You can now begin adding your formatting and actions to the properties of each field. Test it, and ask others to try it on their computer, before you email it to your clients. Always keep a backup on a disk or USB drive in case something happens to your main computer. Also, make a printout of the PDF and all code in case you need to refer to it later for another project. In this book I have placed a copy of the code in the `.txt` files in each chapter for reference.

Once the fields are in the saved form, you can open the fields at any time with the Prepare Form tool; you do not need to run the autodetection again for that form. Refer to Figure [1-7](#449379_2_En_1_Chapter.xhtml#Fig7).

You can exit the Prepare Form area partially by toggling the Preview/Edit button in the upper-right (Figure [1-7](#449379_2_En_1_Chapter.xhtml#Fig7)). To exit the Prepare Form tool completely, you must click the Close button in the upper-right (Figure [1-7](#449379_2_En_1_Chapter.xhtml#Fig7)).

![](images/449379_2_En_1_Chapter/449379_2_En_1_Fig7_HTML.jpg)

A screenshot presents an Adobe Acrobat file page, which has a prepared form titled Tourmaline Mining Corporation Customer Survey with fields for customer's full name, date, company name, and a rating table with 6 rows.

Figure 1-7

The Prepare Form tool and the tools for adding and working with the fields. Note the Preview/Edit toggle and Exit (Close) buttons in the upper right. Also note the view of a form while in Edit mode before switching to Preview

For more information on basic forms or basic form creation, check out the following links before you proceed any further in this book:

*   [`https://helpx.adobe.com/acrobat/using/pdf-forms-basics.html`](https://helpx.adobe.com/acrobat/using/pdf-forms-basics.html)

*   [`https://helpx.adobe.com/acrobat/using/create-form.html`](https://helpx.adobe.com/acrobat/using/create-form.html)

*   [`https://helpx.adobe.com/acrobat/using/creating-distributing-pdf-forms.html`](https://helpx.adobe.com/acrobat/using/creating-distributing-pdf-forms.html)

*   [`https://helpx.adobe.com/acrobat/using/filling-pdf-forms.html`](https://helpx.adobe.com/acrobat/using/filling-pdf-forms.html)

*   [`https://helpx.adobe.com/acrobat/using/pdf-form-field-properties.html`](https://helpx.adobe.com/acrobat/using/pdf-form-field-properties.html)

The Adobe HelpX area for forms has additional links as well, and we will look at some of those options later. But you can use this area as a resource when you need to work with basic forms.

While working in the Prepare Form Tool area, you will come across other form tools in the right-side pane, such as the Align and Distribute options, and in the More drop-down menu (Figure [1-7](#449379_2_En_1_Chapter.xhtml#Fig7)). I will go into more detail about this area and the JavaScript area later in the book. However, you can also refer to the previous web links for more specific details. Refer to Figure [1-8](#449379_2_En_1_Chapter.xhtml#Fig8).

![](images/449379_2_En_1_Chapter/449379_2_En_1_Fig8_HTML.png)

A screenshot of the right panel of the prepare form tool has align, center, match size, and distribute options, a drop-down menu under more, with options under fields such as page 1, page 2 with distribute and track, highlight fields, order tabs, and tab order.

Figure 1-8

Additional options in the right-hand pane of the Prepare Form tool

## Fields Refresher

Fields can be blank and inactive, or they can contain a script that, upon entering or clicking a trigger, sets the action in motion. For example, they can execute a menu item or import form data. In Acrobat Pro, the Prepare Form Menu tool area contains all the field options listed next plus the Selection tool and the newer options Edit Text and Images and Add Text.

Fields can be used in any form, while the Selection tool is just an arrow that allows you to select, size, and move them around. From left to right, let’s look at the tool options in this section more closely; refer to Figure [1-9](#449379_2_En_1_Chapter.xhtml#Fig9).

![](images/449379_2_En_1_Chapter/449379_2_En_1_Fig9_HTML.jpg)

A screenshot depicts a toolbar with a row of icons of 15 tools. Some of them are the selection tool, edit text, add text, text field, check box, drop-down, and image field. Selection tool is active.

Figure 1-9

Editing and Field options for forms

![](images/449379_2_En_1_Chapter/449379_2_En_1_Fig10_HTML.jpg)

A screenshot presents the format settings, which include the font style, font size, alignment, bullets, numbering, and spacing. It has a dialog box with text that reads Tourmaline Mining Corporation Customer Survey.

Figure 1-10

Text change settings in the Prepare Form tool and the selected text with a bounding box

*   **Selection tool**: This arrow allows you select, size, and move fields. It can also be used to edit text boxes when they are added; you double-click with the arrow on them. Refer to the arrow in Figure [1-9](#449379_2_En_1_Chapter.xhtml#Fig9).

*   **Edit Text and Images**: This is not a field option, but it allows you to edit the text or images surrounding the field while remaining in the Prepare Form tool and saves you time rather than having to close the tool and use the Edit PDF tool instead. When text is selected, the right pane’s Format area changes so that you can edit the selected text or image that is surrounded by a bounding box. Images can be scaled and rotated as well. Refer to Figure [1-10](#449379_2_En_1_Chapter.xhtml#Fig10).

![](images/449379_2_En_1_Chapter/449379_2_En_1_Fig12_HTML.jpg)

A screenshot of the text field with the message, text 7\. It has a dialog box with the field name as, text 7, check box for required field, and a highlighted all properties option. The text field icon is on the bottom left.

Figure 1-12

Adding a text field

*   **Add Text**: This is not a field option, but it allows you to add a new text box while remaining in the Prepare Form tool and saves you time rather than having to close the tool and use the Edit PDF tool instead.

    Select the tool and click a location on the page that you want the text to appear and start typing, or when you select the tool, drag out a rectangular bounding box for a paragraph area and start typing. Again, the bounding box will appear around the text, and you can scale and rotate as well as edit the format of the text on the right. Refer to Figures [1-10](#449379_2_En_1_Chapter.xhtml#Fig10) and [1-11](#449379_2_En_1_Chapter.xhtml#Fig11).

    ![](images/449379_2_En_1_Chapter/449379_2_En_1_Fig11_HTML.jpg)

    A screenshot presents a text field with the message Text. The text field icon on the top left has a tool tip that indicates font options under content editing preferences, with highlighted change preferences and learn more tabs.

    Figure 1-11

    Adding text to a page while in the Prepare Form tool and tip message

    Click the Selection tool arrow if you want to exit the editing options.

    Now look at the form fields. These can be added to the form when you first select them and then click the page. Use your mouse to hover over each field to read its description and related name.

*   **Text field**: Type text, words, or numbers into the field. Refer to Figure [1-12](#449379_2_En_1_Chapter.xhtml#Fig12).

![](images/449379_2_En_1_Chapter/449379_2_En_1_Fig13_HTML.jpg)

A screenshot of the text field with the message check. It includes a dialog box with the field name as, Check Box 5, check box for required field, and a highlighted all properties option. The checkbox icon is on the bottom left.

Figure 1-13

Adding a check box

*   **Check box**: Select one or multiple options of an item. Refer to Figure [1-13](#449379_2_En_1_Chapter.xhtml#Fig13).

![](images/449379_2_En_1_Chapter/449379_2_En_1_Fig14_HTML.jpg)

A screenshot of the text field with the message group. It has a dialog box for the radio button choice as Choice 1, with the group name as Group 3, required field, all properties, and a highlighted add another button option. The radio button icon is on the bottom left.

Figure 1-14

Adding a radio button

*   **Radio button**: Select one option from a group: yes or no. These options can be mutually exclusive, and you can have more than one group in a form. You must have at least two radio buttons and can add a second if prompted with the initial message. Refer to Figure [1-14](#449379_2_En_1_Chapter.xhtml#Fig14).

![](images/449379_2_En_1_Chapter/449379_2_En_1_Fig15_HTML.jpg)

A screenshot of the text field with the message, list box 8\. It includes a dialog box with the field name as list box 8, check box for required field, and a highlighted All Properties option. The list box icon is on the bottom left.

Figure 1-15

Adding a list box

*   **List box**: Select one or multiple items in a list of choices. Refer to Figure [1-15](#449379_2_En_1_Chapter.xhtml#Fig15).

![](images/449379_2_En_1_Chapter/449379_2_En_1_Fig16_HTML.jpg)

A screenshot of the text field with the message, dropdown 9\. It has a dialog box with the field name dropdown 9, check box for required field, and a highlighted all properties option. The drop-down list icon is on the bottom left.

Figure 1-16

Adding a drop-down list

*   **Dropdown list**: Select an option or choice from a list. Refer to Figure [1-16](#449379_2_En_1_Chapter.xhtml#Fig16).

![](images/449379_2_En_1_Chapter/449379_2_En_1_Fig17_HTML.jpg)

A screenshot of the text field with the message, button 10\. It includes a dialog box with the field name button 10, check box for required field, and all properties option. The O K button icon is on the bottom left.

Figure 1-17

Adding a button field

*   **Button**: This initiates an action such as reset (Clear), submit, and print. Refer to Figure [1-17](#449379_2_En_1_Chapter.xhtml#Fig17).

![](images/449379_2_En_1_Chapter/449379_2_En_1_Fig18_HTML.jpg)

A screenshot of the field with the message image 11 underscore a f underscore image. It includes a dialog box with the field name image 11 underscore a f underscore image, check box for required field, and all properties option. The image field icon is on the bottom left.

Figure 1-18

Adding an image field

*   **Image field**: This is the same as Button, only with some JavaScript added (more on this topic in Chapter [17](#449379_2_En_17_Chapter.xhtml)). Refer to Figure [1-18](#449379_2_En_1_Chapter.xhtml#Fig18).

![](images/449379_2_En_1_Chapter/449379_2_En_1_Fig19_HTML.jpg)

A screenshot of the field with the message date 12 underscore a f underscore date. It has a dialog box with the field name date 12 underscore a f underscore date, checkbox for required field, and a highlighted All Properties option. The date field icon is on the bottom left.

Figure 1-19

Adding a date field

*   **Date field**: This is the same as a text field but is preformatted to date. It can operate as a date picker. Refer to Figure [1-19](#449379_2_En_1_Chapter.xhtml#Fig19).

![](images/449379_2_En_1_Chapter/449379_2_En_1_Fig20_HTML.jpg)

A screenshot of the field with the message signature 13\. It has a dialog box with the field name signature 13, checkbox for required field, and a highlighted All Properties option. The digital signature icon is on the bottom left.

Figure 1-20

Adding a digital signature

*   **Digital signature**: Electronically sign with your signature. Other Acrobat Form and Signature tools can interact with this field as well. We’ll look at this field in Chapter [19](#449379_2_En_19_Chapter.xhtml) in more detail. Refer to Figure [1-20](#449379_2_En_1_Chapter.xhtml#Fig20).

![](images/449379_2_En_1_Chapter/449379_2_En_1_Fig21_HTML.jpg)

A screenshot of a field with barcode 14\. It has a dialog box with the field name barcode 14, checkbox for required field, and a highlighted All Properties option. The barcode icon is on the bottom left.

Figure 1-21

Adding a barcode

*   **Barcode**: This is for a product barcode reader. You can encode the data that users type in this form. Refer to Figure [1-21](#449379_2_En_1_Chapter.xhtml#Fig21). (Refer to Chapter [19](#449379_2_En_19_Chapter.xhtml) for more details on this field.)

The other two icons in this area are a push pin called “keep tool selected (off) or (on)” so that you can keep applying the same field again, and Field Type Help, which will take you to the Adobe Help forms links we discussed earlier ([`https://helpx.adobe.com/acrobat/using/creating-distributing-pdf-forms.html`](https://helpx.adobe.com/acrobat/using/creating-distributing-pdf-forms.html)). Refer to Figure [1-22](#449379_2_En_1_Chapter.xhtml#Fig22).

![](images/449379_2_En_1_Chapter/449379_2_En_1_Fig22_HTML.jpg)

A screenshot of the push pin icon on the left and an encircled question mark icon on the right.

Figure 1-22

Extra tools on the far right of the fields in the Prepare Form tool

Remember to access these fields as shown in Figure [1-23](#449379_2_En_1_Chapter.xhtml#Fig23). Select Tools ➤ Prepare Form and then choose a PDF document. Click Start, or if the Fields are already present, then the icons will appear at the top just below your navigation icons.

![](images/449379_2_En_1_Chapter/449379_2_En_1_Fig23_HTML.png)

A screenshot presents a prepare form page that lists the options under tools with several navigation icons and a highlighted start button on top.

Figure 1-23

Form tools available when working with a PDF form in Adobe Acrobat Pro. You can access them by going to the Tools tab and selecting the Prepare Form tool

Note

If you require a custom QR code rather than a Barcode field, you can generate one in Adobe InDesign Creative Cloud. QR Codes are like barcodes, and we will look at them more closely in Chapter [3](#449379_2_En_3_Chapter.xhtml).

## Properties Refresher

Each form field contains properties that can be easily accessed while you are in form editing mode. Simply right-click the field you want to edit and choose Properties from the pop-up list (Figure [1-24](#449379_2_En_1_Chapter.xhtml#Fig24)). Then select the property you want to work with in the various tabs that will appear in the dialog box.

![](images/449379_2_En_1_Chapter/449379_2_En_1_Fig24_HTML.jpg)

A screenshot depicts a field box with message, customer's full name, a name field, and a pop-up list that includes highlighted properties, and move up, move down, rename field, set as required field, and add new field options.

Figure 1-24

Right-click a field to reveal its properties

Various properties can be set for each field depending upon which field is chosen because the number of properties varies. The properties are organized into sections using tabs. Refer to Figures [1-25](#449379_2_En_1_Chapter.xhtml#Fig25) through [1-32](#449379_2_En_1_Chapter.xhtml#Fig32). Properties can be typed in, checked, or unchecked. The settings are applied as soon as you exit the field and move to another field in the Properties dialog box or when you click the Close button. However, the settings are not fully saved until you save the PDF file.

To review, to work with the form fields, you must be in the Prepare Form tool in Edit mode. You will know you are in Edit mode because the Preview button toggle is in the upper right, and the name of each field will appear. You can now either edit one field at a time or multiple fields.

Use the Selection tool and either click one field, Shift-click several, or drag a marquee around several and then right-click and choose Properties from the menu.

Note

If you select several fields at once, you may not have access to all tabs depending on the type of fields selected. If you have selected several fields, what you type in the tab properties will apply to all fields selected.

You will now be inside the form’s properties dialog box. Now you can change properties within each tab; when you are done, click the Close button to close the dialog box and save your PDF file to confirm the changes.

The following sections provide a cursory look at the properties associated with the form fields listed earlier in the chapter.

### Text Box Field Properties and Date Field Properties Dialog Boxes

The text field and date field have eight tabs to organize their properties. The only difference between a text field and a date field is that the format category for a date field is preset to Date, while the format category for a text field is preset to None. Note that the heading of the dialog box for both is “Text Field Properties.” Refer to Figure [1-25](#449379_2_En_1_Chapter.xhtml#Fig25).

![](images/449379_2_En_1_Chapter/449379_2_En_1_Fig25_HTML.png)

2 screenshots titled, test field properties. 1\. It lists the options under general tab with name, tooltip, and common properties. 2\. It lists the options under format tab with date selected under category, that displays a calendar.

Figure 1-25

Text field and date properties

### Dropdown Properties Dialog Box

The Dropdown Properties dialog box also has eight tabs to organize the properties. The tabs have the same names as the text field properties and contain many equivalent properties; however, if you compare the Options tab on the Text Field Properties dialog box to the Options tab on the Dropdown Properties dialog box, it will look different because drop-down menus are meant to hold multiple items with export values, while a text field can hold only one default value.

While it is an option, the Calculate tab is rarely used with the drop-down lists. Refer to Figure [1-26](#449379_2_En_1_Chapter.xhtml#Fig26).

![](images/449379_2_En_1_Chapter/449379_2_En_1_Fig26_HTML.jpg)

A screenshot titled dropdown properties that lists the options under general tab with the name 1 S R, tooltip as sale rep helpfulness, and common properties with form field as visible, orientation as 0 degrees. A close button is at the bottom right.

Figure 1-26

Dropdown Properties dialog box

### List Box Properties Dialog Box

List box properties act like drop-down lists. However, there are only six tabs to organize the properties. Like drop-down lists, they can have multiple export values. Unlike drop-down lists, you can select more than one value at a time. Refer to Figure [1-27](#449379_2_En_1_Chapter.xhtml#Fig27).

![](images/449379_2_En_1_Chapter/449379_2_En_1_Fig27_HTML.jpg)

A screenshot titled, list box properties, that lists the options under general tab with the name as quality list, tooltip as quality, and common properties with form field as visible, orientation as 0 degrees.

Figure 1-27

List Box Properties dialog box

### Check Box Properties Dialog Box

The Check Box Properties dialog box has five tabs to organize the properties. A check box can be checked either on or off. You cannot enter text into a check box; however, you can give it a word or number value. Like all other properties, you can alter its appearance and color (via the Appearance and Option tabs). Check boxes can act separately or in groups. Refer to Figure [1-28](#449379_2_En_1_Chapter.xhtml#Fig28).

![](images/449379_2_En_1_Chapter/449379_2_En_1_Fig28_HTML.jpg)

A screenshot titled, check box properties, that lists the options under general tab with the name as satisfied, tooltip as satisfied, and common properties with form field as visible, orientation as 0 degrees.

Figure 1-28

Check Box Properties dialog box

### Radio Button Properties Dialog Box

The Radio Button Properties dialog box, like the Check Box Properties dialog box, has five tabs to organize the properties. A radio button must come in pairs or groups or more than two that can be checked either on or off. While one is on, the others are off. You cannot enter text into a radio button; however, you can give it a word or number value. Like all other properties, you can alter its appearance and color (Appearance and Option tabs). You can have more than one group of radio buttons, but there must always be at least two in any group. Refer to Figure [1-29](#449379_2_En_1_Chapter.xhtml#Fig29).

![](images/449379_2_En_1_Chapter/449379_2_En_1_Fig29_HTML.jpg)

A screenshot titled, radio button properties, that lists the options under general tab with the name as group 1.0, tooltip as choice 1, and common properties with form field as visible, orientation as 0 degrees.

Figure 1-29

Radio Button Properties dialog box

### Button Properties and Image Properties Dialog Box

Button properties and image properties are identical except that image properties have a small bit of code in the Actions tab to allow the importing of an image. See Chapter [17](#449379_2_En_17_Chapter.xhtml) for details. Both contain five tabs, and the tabs each have identical properties. Buttons can have more than two states; however, image buttons operate independently of regular button properties once the JavaScript is applied. Refer to Figure [1-30](#449379_2_En_1_Chapter.xhtml#Fig30).

![](images/449379_2_En_1_Chapter/449379_2_En_1_Fig30_HTML.jpg)

A screenshot titled, button properties, that lists the options under general tab with the name as print 3, tooltip as print, and common properties with form field as visible, orientation as 0 degrees.

Figure 1-30

Button Properties dialog box

### Digital Signature Properties Dialog Box

Digital signatures are used for signing electronic PDF forms with a client’s digital signature, which is stored on their computer. The digital signature field appears like the text field; however, it has only five tabs to organize its properties and is specifically designated for signatures only. Chapter [19](#449379_2_En_19_Chapter.xhtml) offers more details on digital signatures and the tools used in conjunction with it. Refer to Figure [1-31](#449379_2_En_1_Chapter.xhtml#Fig31).

![](images/449379_2_En_1_Chapter/449379_2_En_1_Fig31_HTML.jpg)

A screenshot titled, digital signature properties, that lists the options under general tab with the name as vendor signature, tooltip as vendor signature, and common properties.

Figure 1-31

Digital Signature Properties dialog box

### Barcode Properties Dialog Box

The barcode field properties are organized into five tabs. A barcode’s main purpose is to create a scannable barcode that relates to the information that is entered into the various fields around it. Chapter [19](#449379_2_En_19_Chapter.xhtml) offers more details on barcodes. Refer to Figure [1-32](#449379_2_En_1_Chapter.xhtml#Fig32).

![](images/449379_2_En_1_Chapter/449379_2_En_1_Fig32_HTML.jpg)

A screenshot titled, barcode field properties that lists the options under general tab with the name as barcode 6, tooltip as barcode, and common properties.

Figure 1-32

Barcode Field Properties dialog box

## Tabs Refresher

As mentioned, all form fields have similar tabs, as listed here.

These are the options on the General tab:

*   **Name**: This is the name of the field.

*   **Tooltip**: This adds a type of accessibility text to the field so that people with visual impairments can scan the field and know its purpose. It is also a way to give a helpful hint for those who do not know the purpose of entering text in the field.

*   **Common properties**: This form field indicates whether it is visible, hidden, visible but doesn’t print, or hidden but printable. This is not available to barcodes.

*   **Orientation**: This setting adjusts the angle of the field to 0, 90, 180, or 270 degrees. It is not available to barcodes.

*   **Read only**: You can read the text within but not alter it. This is not available to barcodes; by default they are read-only.

*   **Required**: This field is required to complete the form. An error message may appear if the field is not filled. This is not available to buttons, list boxes, image fields, and barcodes.

These are the options on the Appearance tab (not available to barcodes):

*   **Border and colors**: The following are the settings:
    *   **Border color**: This is the color of the border surrounding the field. Set this to a color or to no color.

    *   **Line thickness**: This is the thickness of the border: thin, medium, or thick.

    *   **Fill color**: This is the fill color of the field. Set it to a color or no color. A color will block the background behind the field.

    *   **Line style**: This is the style of the line going around the field: solid, dashed, beveled, inset, or underlined.

*   **Text**: This is the font size (not available to signatures), text color, and font (not available to check boxes or radio buttons). A size of Auto will change the font size to fit the text box as the user types.

These are the options on the Position tab (available for all fields):

*   **Units**: The following are the units of measurement of the size and position of the fields: points, picas, millimeters, centimeters, and inches.

*   **Position units**: The options are left, right, top, bottom, width, and height.

*   Check “Do not change height and width when changing position” if you do not want the size of the box to alter during movement while adjusting the other position settings in the properties dialog box. Entering numbers in the width and height in the dialog box is disabled. Unchecking that box may cause the form field to scale during movement with the other position settings.

These are the options on the Options tab (not available to digital signatures; these options are only for text and date fields):

*   **Alignment**: This aligns text left, center, or right.

*   **Default value**: This provides temporary or default text for the field before the user types in the field.

*   **Field is used for file selection**: This allows the user to enter a file’s text link info. It is not available for the date field. It is available when “Check spelling” and Multi-line are disabled and “Scroll long text” is enabled.

*   **Password**: This creates *** to mask the actual text. It is not available for the date field. It is available when “Check spelling” and Multi-line are disabled.

*   **Check spelling**: This indicates if there is a spelling error in the user-entered data when checked.

*   **Multi-line**: This allows you to enter more than one line of text in the field. It is not available for the date field.

*   **Scroll long text**: If there is more text than the field can handle, a scroll bar appears.

*   **Allow rich text formatting**: This allows users to make the text bold or italic. It is not available for the date field.

*   **Limit of characters**: When enabled, this limits the number of characters allowed in a field, by a set number.

*   **Comb of characters**: When enabled, this creates a divider between characters so they are easier to read later and compare (see Chapter [4](#449379_2_En_4_Chapter.xhtml)). The characters are spread out. This is good when entering numbers. However, other settings in this tab need to be disabled first before you can set the comb at its amount.

These are the options for list boxes and drop-down lists only:

*   **Item**: Enter the item name or option you want to appear in the list.

*   **Export value**: Enter its export value letter or numbers. It can be filled or left blank.

*   **Item List**: This lists all the items. The highlighted item is selected by default in the list.

*   **Add, Delete, Up, Down buttons**: Add, remove, or alter an item’s order in the list when the sort items option is not selected.

*   **Sort items**: Sort alphabetically or numerically.

*   **Allow user to enter custom text**: Allow the user to enter their own text. This is not available for list boxes.

*   **Check spelling**: This indicates if there is a spelling error when a user enters text. This is not available for list boxes.

*   **Commit selected value immediately**: When selected, the value may interact with another field’s value. Otherwise, the user must wait until they tab out of the field and click another form field. It will not operate with multiple list box selections.

*   **Multiple selection**: This lets you select multiple items in a list box only.

These are the options for check boxes and radio buttons only:

*   **Style**: The styles are check, circle, cross, diamond, square, and star. The size is determined in the Appearance panel by the text area.

*   **Export value**: This is the value of the field. It is for check boxes only.

*   **Radio button choice**: This is the same as the export value. Radio buttons in a group must have the same name on the General tab but can have a different export value on the Options tab.

*   **Check box is checked by default**: This sets the box to be checked when the form opens.

*   **(Radio) button is checked by default**: The box appears filled when the form opens.

*   **(Radio) buttons with the same name and choice are selected in unison**: This is the only way to make two or more radio buttons within the same group appear selected.

These are the options for buttons and image fields:

*   **Layout**: This adds a layout for the icon and label (see Chapter [4](#449379_2_En_4_Chapter.xhtml)).

*   **Advanced button**: This creates a more advanced layout for the Icon button (see Chapter [4](#449379_2_En_4_Chapter.xhtml)).

*   **Behavior alters the states of the button**: The options are none, push, outline, and invert (see Chapter [4](#449379_2_En_4_Chapter.xhtml)).

*   **Icon and label state**:
    *   **State**: This sets how the button appears in up, down, and rollover states.

    *   **Label**: This sets the text name on the button.

*   **Icon**: This is a thumbnail of the chosen icon.
    *   **Choose icon**: This allows you to choose an icon.

    *   **Clear**: This clears the icon from the field.

These are the options for barcodes only:

*   **Symbology**: There are distinct types of barcodes available.

*   **Compress data encoding barcode**: Data is compressed before it is encoded.

*   **Decode condition**: These are custom and manage barcode parameters (see Preferences in Figure [1-2](#449379_2_En_1_Chapter.xhtml#Fig2)), such as how the barcode will be decoded by some device such as a scanner.

*   **Settings**: This is the settings for that barcode.

These are the options on the Actions tab (for all form fields; more info in Chapters [4](#449379_2_En_4_Chapter.xhtml) and [5](#449379_2_En_5_Chapter.xhtml)),

*   **Add an action**: Select what triggers the action when the field is entered.

*   **Select action**: Select what kind of action or event is triggered.

*   **Add button**: This adds the action and opens various dialog boxes to edit actions that upon committing will then display in the Actions area.

*   **Up button**: This moves the action in its order.

*   **Down button**: This moves the action in its order.

*   **Edit button**: This edits the action in the original dialog box or JavaScript Editor.

*   **Delete button**: This deletes the selected action or multiple actions under the trigger.

These are the options on the Format tab (the Text, Date Field, and Dropdown lists; refer to Chapters [6](#449379_2_En_6_Chapter.xhtml) and [7](#449379_2_En_7_Chapter.xhtml) for a detailed explanation):

*   **None**: This is for text and numbers with no true numeric value.

*   **Number**: This formats the numbers with or without decimal places, separator style, currency symbol, symbol location, and negative number style using parentheses and red text.

*   **Percentage**: This formats the percentage with decimal places and separator style.

*   **Date**: This formats the type of date options.

*   **Time**: This formats the type of time options.

*   **Special**: This formats the ZIP code, phone number, Social Security number, and arbitrary mask options.

*   **Custom**: This creates custom option scripts called Format and Key Stroke that you can edit.

These are the options on the Validate tab (Text, Date Field, and Dropdown lists):

*   **Field value is not validated**: The field does not require validation.

*   **Field value is in range**: The options are From a Number, To a Number such as Numbers 1–5.

*   **Run a custom validation script**. The JavaScript script can be edited.

These are the options on the Calculate tab (Text, Date Field, and Dropdown lists):

*   **Value is not calculated**. Use this if you only want the user to enter default text.

*   **Value is the (sum, product, average, minimum, maximum) for the following fields**: Use the Pick button to choose the fields.

*   **Simplified field notation**: The Edit button allows you to enter a script in the JavaScript Editor.

*   **Custom calculation script**: The Edit button allows you to enter it in the JavaScript Editor.

These are the options on the Selection Change tab (list box only):

*   **Do nothing (if no action is required)**. This allows doing nothing. No script is triggered and only selections are made.

*   **Execute this script**: Add a custom script. Edit allows you to enter it in the JavaScript Editor.

These are the options on the Signed tab (Digital Signature field only; refer to Chapter [19](#449379_2_En_19_Chapter.xhtml)):

*   **Nothing happens when signed**: No action happens.

    The following setting or option is compatible only with Acrobat 6.0 and later.

*   **Mark as read-only**: All fields or only certain ones can be selected to trigger or prevent certain fields from becoming read-only when the signature is filled creating further changes to the signed document. Use the Pick button to choose the fields.

*   **This script executes when the field is signed**: The Edit button allows you to enter in the JavaScript Editor.

These are the options on the Value tab (Barcode field only; refer to Chapter [19](#449379_2_En_19_Chapter.xhtml) for more info):

*   **Encoding using tab delimited or XML**: This option is used with the pick button to choose the fields.

*   **Include field names**: This is available when the encoding is set to tab delimited.

*   **Custom calculation script**: Edit allows you to enter in the JavaScript Editor.

*   **“Reference to published form” will appear as a link in the dialog box’s field**: For example it may look something like this line of text “File//C/Users” pointing to a location on your computer.

These are the options found on all tabs and fields:

*   **Locked**: When selected, this prevents any further changes to any form field properties until unlocked. This includes position and scaling.

*   **Close**: This button closes the form field’s Properties dialog box. If you are changing the properties of multiple fields, you can leave the Properties dialog box open. Click each field to change its properties. And then click the Close button when done.

The following are the tabs you’ll find in most of the fields. Figure [1-33](#449379_2_En_1_Chapter.xhtml#Fig33) shows the tabs found in the Text Field Properties dialog box.

![](images/449379_2_En_1_Chapter/449379_2_En_1_Fig33_HTML.jpg)

A screenshot depicts the top region of the text field properties dialog box with the general, appearance, position, options, actions, format, validate, and calculate tabs. General tab is selected.

Figure 1-33

The tabs that contain the properties

Bear in mind that the properties can differ depending upon the type of field chosen. For example, the Options tab properties are different for a text field versus a barcode or a radio button (Figures [1-34](#449379_2_En_1_Chapter.xhtml#Fig34) and [1-35](#449379_2_En_1_Chapter.xhtml#Fig35)).

![](images/449379_2_En_1_Chapter/449379_2_En_1_Fig35_HTML.jpg)

A screenshot of a dialog box titled, radio button properties, that lists the options with button style, radio button choice, and a highlighted close button.

Figure 1-35

Options tab properties for Radio Button Properties dialog box

![](images/449379_2_En_1_Chapter/449379_2_En_1_Fig34_HTML.png)

2 screenshots. On the top, a text field properties dialog box, under options tab, lists alignment, default value, and checkboxes for more options. On the bottom, a barcode field properties dialog box lists the options with symbology, decode condition, and settings.

Figure 1-34

Options tab properties for Text Field and Barcode Field dialog boxes

## Editing Your Form with the More Menu and Right Pane

In the pane on the right are a few more settings we will take a brief look at before we conclude this chapter. To make sure you can see your field clearly, make sure that in the More menu, “Highlight Existing fields” is enabled. Refer to Figure [1-36](#449379_2_En_1_Chapter.xhtml#Fig36).

![](images/449379_2_En_1_Chapter/449379_2_En_1_Fig36_HTML.png)

A screenshot presents the right pane of prepare form tool that lists align, center, match size, and distribute options, and more drop-down menu with fields section. It includes page 1, page 2 with expanded list of field names, highlight existing fields, order tabs, and tab order options.

Figure 1-36

Prepare Form tool options in the pane on the right

### Setting the Field Tab Order Using the Menu Options

The More drop-down menu, found in the pane on the right, works in conjunction with the fields found on the page as well as the same fields shown as a list which can each be selected to access the same field’s properties. Those fields and their selection options appear below the More  menu. You can use the fields in the list to better select overlapping fields on the page.

You can place or duplicate many fields across a page as well as create multiple copies of a field using the More menu and the related dialog boxes while a field is selected. Refer to Figure [1-37](#449379_2_En_1_Chapter.xhtml#Fig37) and Figure [1-38](#449379_2_En_1_Chapter.xhtml#Fig38).

![](images/449379_2_En_1_Chapter/449379_2_En_1_Fig38_HTML.jpg)

A screenshot of the create multiple copies of the fields dialog box with the number of fields, overall size, overall position, and a highlighted O K button. It has 4 field boxes in 2 rows with text, table 1 in the first box.

Figure 1-38

Create Multiple Copies of Fields dialog box and a preview of the selected field with default settings

![](images/449379_2_En_1_Chapter/449379_2_En_1_Fig37_HTML.png)

A screenshot presents a more menu with a highlighted duplicate across pages option. It has a dialog box titled, duplicate field, with options for duplicating the field on all, or on some file pages. An O K button is at the bottom.

Figure 1-37

More menu options for duplicating a selected field

While moving around the form, the order of those fields can become jumbled or out of order as you move from page to page.

At some point, after reviewing the field properties and then previewing the form, using the Tab key to tab through the fields, as a client would do while filling out the form, you may discover that you are randomly moving from field to field not in the order you intended. At this point you need to check and edit the tab order.

To do this from the More drop-down menu, choose Show Tab Numbers. Refer to Figure [1-39](#449379_2_En_1_Chapter.xhtml#Fig39).

![](images/449379_2_En_1_Chapter/449379_2_En_1_Fig39_HTML.jpg)

A screenshot presents a more menu with the submenu of, prepare form for e signing, duplicate across pages, create multiple copies, highlighted show tab numbers, and show field properties.

Figure 1-39

From the More menu, choose Show Tab Numbers

Upon selecting this option, you will receive an information message that explains how to alter the tab order. You can do this manually by dragging and dropping the fields in this lower list on the right. Click OK. You will now be able to see the tab number on the field; it is checked as active in the menu. Refer to Figure [1-40](#449379_2_En_1_Chapter.xhtml#Fig40) and Figure [1-41](#449379_2_En_1_Chapter.xhtml#Fig41).

![](images/449379_2_En_1_Chapter/449379_2_En_1_Fig40_HTML.png)

A screenshot depicts an Adobe Acrobat pop-up dialog box with an information tip to change tab order, and a highlighted O K button. It has a side panel with 10 tab numbers and statements.

Figure 1-40

Activating the Show Tab Numbers settings

Now look at the fields list in the right pane.

This will show you how the fields are in the current order page by page. Refer to Figure [1-41](#449379_2_En_1_Chapter.xhtml#Fig41).

![](images/449379_2_En_1_Chapter/449379_2_En_1_Fig41_HTML.jpg)

A screenshot presents the fields menu that lists the names under pages 1 and 2 with several tabs. Some of them are the customer's full name, date, customer's company name, and final result.

Figure 1-41

Setting field order by page

However, here you can do several things to alter the tab order.

The first menu beside the field’s name lets you choose the following options: Order Tabs by Structure, by Row, by Column, Manually, Unspecified, and Show Tab Numbers. Refer to Figure [1-42](#449379_2_En_1_Chapter.xhtml#Fig42).

![](images/449379_2_En_1_Chapter/449379_2_En_1_Fig42_HTML.jpg)

A screenshot presents the field menu with several submenus. They are order tabs by structure, order tabs by row, order tabs by column, order tabs manually, order tabs unspecified, and show tab numbers. The last 2 are enabled.

Figure 1-42

Field setting for tab order

Currently, in my example, by default it is set to Order Tabs Unspecified with the tab numbers shown; however, I could change this to Manually after clicking OK to the warning message. Refer to Figure [1-43](#449379_2_En_1_Chapter.xhtml#Fig43).

![](images/449379_2_En_1_Chapter/449379_2_En_1_Fig43_HTML.jpg)

A screenshot presents an Adobe Acrobat pop-up dialog box with an information tip. It states that the order of fields listed in the fields panel determines the tab order, and instructions to reset the tab order, with a highlighted O K button.

Figure 1-43

Info alert message that appears and gives instructions about tabs if you change from an Order Tabs Unspecified to Order Tabs Manually

I could then, in the Fields panel, drag one field and alter the order to the tabbing order of my choice. Refer to Figure [1-44](#449379_2_En_1_Chapter.xhtml#Fig44).

![](images/449379_2_En_1_Chapter/449379_2_En_1_Fig44_HTML.jpg)

2 screenshots present the field menu that lists the options under Page 1 with several tabs. It includes the highlighted 3 resources tab with the three field panels on the right.

Figure 1-44

Changing the tab order in the Fields panel

If I found this to be out of order, I could drag the field back in the fields list, or from the menu choose other options from the tab list such as Order Tabs by Row to reset the order. Refer to Figure [1-42](#449379_2_En_1_Chapter.xhtml#Fig42). Ultimately, before you submit a form to a client, it is a good idea to preview your tab order and then save your file.

If, while working in your form, you notice that your fields in the fields pane are not appearing in tab order, check in your next menu (AZ) that the Tab Order option is checked and not Alphabetic Order, which does not show the page number. This does not alter the tab order on the pages, and you may not be able to reorder the fields on this setting. Refer to Figure [1-45](#449379_2_En_1_Chapter.xhtml#Fig45).

![](images/449379_2_En_1_Chapter/449379_2_En_1_Fig45_HTML.jpg)

A screenshot presents the field menu, which has the right-click panel with the highlighted alphabetic order and checked tab order.

Figure 1-45

Make sure Tab Order is selected in the list

Either way, you cannot move a field from page to page unless you select a field and select Edit ➤ Cut and Edit ➤ Paste on a new page.

Likewise, you can also right-click a field and from the pop-up menu move the tab order up or down for a specific field. Other operations include deleting a field. Refer to Figure [1-46](#449379_2_En_1_Chapter.xhtml#Fig46).

![](images/449379_2_En_1_Chapter/449379_2_En_1_Fig46_HTML.jpg)

A screenshot presents the right-click option with several tabs. Some of them are properties, cut, copy, delete, select all, set fields to the same size, where show tab numbers is enabled.

Figure 1-46

Right-clicking a field reveals other form editing options

You can learn more about those settings here:

[`https://helpx.adobe.com/acrobat/using/pdf-form-field-basics.html`](https://helpx.adobe.com/acrobat/using/pdf-form-field-basics.html)

You can delete a selected field or multiple selected fields when you press the Delete key on your keyboard and use Edit ➤ Undo to get the field back again.

### Clearing a Form While Working

While editing and previewing your forms, you may want to reset or clear the content of the fields so that you can make alterations or present a final form to the client. Remember under the More menu to frequently use the Clear Form option before you save and close your final file. Refer to Figure [1-47](#449379_2_En_1_Chapter.xhtml#Fig47).

![](images/449379_2_En_1_Chapter/449379_2_En_1_Fig47_HTML.jpg)

A screenshot presents the more menu with the options to prepare form for E-signing, show tab numbers, and highlight existing fields, along with a highlighted clear form.

Figure 1-47

Clearing form fields using the More menu

Keep in mind, however, that this option is only for when you build the form in Acrobat Pro. Someone who is using Reader will not have the option to reset the form like this. So, it is good to add a button field that can do this for all or selected fields if the client needs to reset or clear the fields. We will look at how to do this in Chapter [2](#449379_2_En_2_Chapter.xhtml).

The More menu also has a few other helpful items, depending upon the type of form work.

For example, we will look at e-signing in Chapter [19](#449379_2_En_19_Chapter.xhtml) and Chapter [20](#449379_2_En_20_Chapter.xhtml). Refer to Figure [1-48](#449379_2_En_1_Chapter.xhtml#Fig48).

![](images/449379_2_En_1_Chapter/449379_2_En_1_Fig48_HTML.jpg)

A screenshot of the menu option to prepare form for E-signing.

Figure 1-48

Preparing a form for e-signing

Though not the topic of this book, if you are specifically importing data to fields or exporting data to another file format for use with a database, refer to Figure [1-49](#449379_2_En_1_Chapter.xhtml#Fig49).

![](images/449379_2_En_1_Chapter/449379_2_En_1_Fig49_HTML.jpg)

A screenshot of the menu has the option to import data, export data, and merge data files into spreadsheet.

Figure 1-49

More menu options for working with data in forms

The following links may be of use on that topic:

[`https://helpx.adobe.com/acrobat/using/collecting-pdf-form-data.html`](https://helpx.adobe.com/acrobat/using/collecting-pdf-form-data.html)

[`https://helpx.adobe.com/acrobat/using/creating-distributing-pdf-forms.html`](https://helpx.adobe.com/acrobat/using/creating-distributing-pdf-forms.html)

These links will also give you some information on Distribute and Tracker, which are not topics discussed in this book. You can find these same items in the Edit ➤ Form Options submenu when a form is first opened before entering the Prepare Form tool. Refer to Figure [1-50](#449379_2_En_1_Chapter.xhtml#Fig50).

![](images/449379_2_En_1_Chapter/449379_2_En_1_Fig50_HTML.jpg)

A screenshot presents the distribute and track buttons with the form options. It includes several submenus, some of which are track, clear form, and import data, where highlight existing fields is enabled.

Figure 1-50

Distribute and Track button form options

Another option in the More Tab area is Set Field Calculation Order, which will be mentioned in Chapter [6](#449379_2_En_6_Chapter.xhtml). Refer to Figure [1-51](#449379_2_En_1_Chapter.xhtml#Fig51).

![](images/449379_2_En_1_Chapter/449379_2_En_1_Fig51_HTML.jpg)

A screenshot of the set field calculation order option.

Figure 1-51

More menu options for Set Field Calculation Order

Later in Chapter [8](#449379_2_En_8_Chapter.xhtml) and the following chapters, we will be looking at the final options in the More menu that pertain to JavaScript for the options Debugger, Debug, All JavaScripts, Document JavaScripts, and Document Actions. Refer to Figure [1-52](#449379_2_En_1_Chapter.xhtml#Fig52).

![](images/449379_2_En_1_Chapter/449379_2_En_1_Fig52_HTML.jpg)

A screenshot of the right-click sub-menus. It includes debugger, debug, all Java Scripts, document Java Scripts, and document actions. Debug is not active.

Figure 1-52

More menu JavaScript options

## Summary

This chapter covered the basics of form fields, their tabs, and the properties within those tabs, as well as how to order tabs and additional settings found in the pane on the right of the Prepare Form tool. The next chapter will be an introduction to the basic actions that you can apply to fields.

# 2. Introduction to Actions

**Chapter Goal:** Now that you have reviewed the basic form properties, you will begin your study of JavaScript by taking a closer look at several types of automatic or preprogrammed actions that can be applied within various tabs within each field. In this chapter, you’ll be working with forms, and you’ll discover how actions can be applied.

## Getting Started

If you want to work along in this lesson or review the final result, download the Chapter [2](#449379_2_En_2_Chapter.xhtml) files from the link mentioned in the introduction. The file with the label “Start” is the file without the code, and the file with the label “End” is the final result. You will also find a folder with original MS Word and PDF files if you would like to edit them plus a folder containing the original scripts (`.txt`) if you would like to add them to your own PDF forms.

Note

To view the properties of a field, you must select the Prepare Form tool. Only then can you right-click or double-click a field to review its properties. If you are creating your form from an original PDF that contains no form fields, refer to the “Forms Review” section in Chapter [1](#449379_2_En_1_Chapter.xhtml).

You can apply actions to all form fields. Actions, as you will see in more detail in Chapter [4](#449379_2_En_4_Chapter.xhtml) and later chapters, can trigger various events such as alert boxes or cause a field to display a final calculation or a button to reset fields in a form. However, some actions work better with certain form fields than others.

For instance, applying an action to a radio button might give you some very select calculations, but if you want to add up those values, using check boxes or text fields would probably be a better option.

If there are fields on the page, you can open and view their properties by clicking the Prepare Form tool and then in edit mode either right-clicking the field and choosing Properties from the menu or double-clicking the field itself. The text fields in these examples contain no information on the Actions tab and are blank. However, as you’ll see shortly, other tabs within the field and other field types do contain information that will cause an event to occur. See Figure [2-1](#449379_2_En_2_Chapter.xhtml#Fig1).

![](images/449379_2_En_2_Chapter/449379_2_En_2_Fig1_HTML.jpg)

A screenshot of the text field properties dialog box. The tab for actions is selected. The action is to execute a menu item and select trigger as mouse up. The actions box is empty. The dialog box is next to the icon for prepare form with an add dropdown.

Figure 2-1

The Actions tab in the Text Field Properties dialog box

Here you can see one of the many areas where you can add actions and a trigger, which I will discuss in more detail shortly.

## Rating Forms Value Averaging and Sum: Working with Text Fields

Let’s look at several ways to use a combination of text boxes, radio buttons, and check boxes to do averaging and summing.

Let’s say you want to create a survey to see if your clients have any concerns about your customer service or interactions. You can create a rating form that helps you determine where you need to improve. On page 1 of the PDF form `TMC_Customer Survey_Start.pdf` example shown in Figure [2-2](#449379_2_En_2_Chapter.xhtml#Fig2), you can see how the form looks unformatted. However, in the final “End” form, the Rating fields in this survey are the text fields that are formatted with a setting of Number rather than None so that the client cannot enter in letters, only numbers.

![](images/449379_2_En_2_Chapter/449379_2_En_2_Fig2_HTML.jpg)

A page of the tourmaline mining corporation. The customer's full name, the date, and the name of the customer's company are the necessary details in the form. There are 6 rating factors and further comments area. Reset comments and print are 2 buttons on the form below the comments area.

Figure 2-2

Example of a customer survey form and the form tools (text field, check box, and radio button) used on this page and other pages

To ensure a client puts the correct information into a field, it is important to limit their options. You can change the formatting by selecting from the “Select format category” drop-down menu.

Figure [2-3](#449379_2_En_2_Chapter.xhtml#Fig3) provides an example of the formatting used on the Format tab of the Text Fields Properties dialog box. In this example, I right-clicked a text field I named “1 Sales Rep Helpfulness” and in the Format tab in the Select Format Category list set it from None to Number. By default, Decimal Places will be set to 2\. However, if you need the numbers not to have decimals as in this example, set it to 0 instead. Otherwise, I left the other lists at the default settings Separator Style (1,234.56), Currency Symbol (None), and Symbol Location (disabled), as well as the Negative Number Style options of “Show parentheses” and “Use red text” disabled. Refer to Figure [2-3](#449379_2_En_2_Chapter.xhtml#Fig3).

![](images/449379_2_En_2_Chapter/449379_2_En_2_Fig3_HTML.png)

A dialog box for text field properties. The format category has the None option selected. In comparison, the separator style is 1,234 dot 56, the format category as number in the dialog box below, and the decimal places are set as 0.

Figure 2-3

The formatting for each of the rating text fields on page 1 of the Customer Survey file

We’ll talk about this Format tab in more detail in Chapter [7](#449379_2_En_7_Chapter.xhtml). However, as you build the form on page 1, note that in the Text Fields Properties dialog box you can click or select the next text field in the column, format it to the same Number setting again, or on the last field in the column, for a variation set Decimal Places to 1–10\. In this case, I left it at 2. Refer to Figure [2-4](#449379_2_En_2_Chapter.xhtml#Fig4).

![](images/449379_2_En_2_Chapter/449379_2_En_2_Fig4_HTML.png)

A snip of the format style of the page from top to bottom depicts 1 sales rep helpfulness, 2 questions answered, 3 resources were provided, 4 safety on site, 5 handling of conflicts, 6 my overall feeling, and 7 final results. The format tab is selected with the format category as number.

Figure 2-4

The formatting for each of the text rating fields on page 1 of the Customer Survey file

### The Validate Tab

On page 1 in the rating text field example, you can limit the client’s options even further by setting a range of numbers in the “Field value is in range” field on the Validate tab. Now the client can only enter numbers that range from 1 to 5\. If they enter a 6 or higher or 0 or lower, a warning will occur, as shown in Figure [2-5](#449379_2_En_2_Chapter.xhtml#Fig5).

![](images/449379_2_En_2_Chapter/449379_2_En_2_Fig5_HTML.png)

A snip of the format style of the page from top to bottom depicts 1 sales rep helpfulness, 2 questions answered, 3 resources were provided, 4 safety on site, 5 handling of conflicts, and 6 my overall feeling. The validate tab is selected with field values ranging from 1 to 5.

Figure 2-5

The Validate tab of each of the text following fields on page 1 with the values set from 1-5 to limit the range input and warning if a number is entered out of range

The user must then click OK to the message and enter the correct number to proceed in the form. In this example, the Final Result field is set to no validation since the user cannot enter a number here as the resulting value may be higher than 5 and it is set to Read Only on the General tab. Refer to Figure [2-6](#449379_2_En_2_Chapter.xhtml#Fig6).

![](images/449379_2_En_2_Chapter/449379_2_En_2_Fig6_HTML.jpg)

A dialog box for text field properties. The radio button for field value not validated is selected. The name and the tooltip in the text field properties are given as the final result. The form field is visible and the orientation is 0 degrees.

Figure 2-6

The Final Result field is set to no validation and read-only

If you inspect the Calculate tab for the Grand Total or Final Result field on page 1 of the project (and as in Figures [2-7](#449379_2_En_2_Chapter.xhtml#Fig7) and [2-8](#449379_2_En_2_Chapter.xhtml#Fig8)), you can see how the field will receive data from other fields using the “Value is the ________ of the following fields,” selecting Average from the list, and then clicking the Pick button to choose various text fields that this field will gather information from.

![](images/449379_2_En_2_Chapter/449379_2_En_2_Fig8_HTML.jpg)

A dialog box for field selection. It has a list of fields for calculations. The various options available in the list are my overall feeling, handling of conflicts on site, and safety on site.

Figure 2-8

The Field Selection dialog box

![](images/449379_2_En_2_Chapter/449379_2_En_2_Fig7_HTML.jpg)

A dialog box for text field properties with calculate tab selected. The value is the average of the following table is selected. The other options are simplified field notation and custom calculation scripts are disabled.

Figure 2-7

The Calculate tab for the final result

In your own work, to access other fields and their data for this final field, click the Pick button to access field selection. Select only the fields you want to calculate. Note which field name is on the current page so you don’t get calculations from other fields on other pages by mistake. When you’re done, click OK. You can also select or deselect all fields. This will allow you to make your selections faster, rather than checking off or on all the fields one at a time. In this example I just wanted to calculate the first six fields in the column. Refer to Figure [2-5](#449379_2_En_2_Chapter.xhtml#Fig5).

Tip

If you have trouble selecting the check box, you can highlight it and press the spacebar on your keyboard. This toggles the check box on or off.

Remember that the Final Result text field is set to read-only on the General tab so that a client cannot alter the final result. Refer to Figure [2-6](#449379_2_En_2_Chapter.xhtml#Fig6).

## The Calculate Tab for the Grand Total Using Sum

As you progress though form calculations, rather than using an average (for example, 1,2,3,4,5,5 = 3.33), which is the average of all numbers, you may instead prefer to add up all the numbers to a collective total of 20\. Different surveys may require different accumulative ratings.

On the final form for reference called `TMC_Customer Survey_End.pdf`, if you go to page 2, the only difference between this form and the one on page 1 is that the Final Result value in the Calculate tab was changed to sum (+) and I used the rating text fields on that page to add up the calculation. Refer to Figure [2-9](#449379_2_En_2_Chapter.xhtml#Fig9).

![](images/449379_2_En_2_Chapter/449379_2_En_2_Fig9_HTML.jpg)

A dialog box of the text field properties. The calculate tab is selected. The radio button for value is the sum of the following fields selected. The final result is 0 dot 0.

Figure 2-9

The Calculate tab for the final result on page 2

Note

I selected fields only on page 2\. If I needed fields from other pages to complete the calculation, such as on page 1 or 3, I would pick these fields as well. Form calculations can be on as many pages within the PDF document as required. In this example, those fields on page 2 can be identified with their name with _2 on the end. However, in your own projects, your text field names may be different than mine.

Alternate Drop-Down Rating

If you skip past page 3 down to page 4 of this example in the “End” document, you can see how, with this same form, you can replace some of the rating text fields with drop-down lists to rate. While this method is similar to text fields, I find it a suitable alternative if you want your client to use very specific values. Also, it eliminates the need for the Format and Validate tabs on each drop-down because the values are already set. To set an export value for a drop-down list, you need to set the Export value for each menu item on the Options tab. I also enabled “Check spelling” and “Commit selected value immediately” so that the calculation would change when selected from the list. Refer to Figures [2-10](#449379_2_En_2_Chapter.xhtml#Fig10) and [2-11](#449379_2_En_2_Chapter.xhtml#Fig11).

![](images/449379_2_En_2_Chapter/449379_2_En_2_Fig11_HTML.jpg)

A page of the rating. There are 6 options for rating and each has a dropdown menu with 5 options for rating them. The final result is 5\. A dialog box on text field properties with calculate tab in view and the value calculated in average for the table option is selected.

Figure 2-11

One of the drop-down fields extended on page 4

![](images/449379_2_En_2_Chapter/449379_2_En_2_Fig10_HTML.jpg)

A dialog box for dropdown properties with an options tab selected. Exceeded option is selected for items, the export value is 5, and the item list includes unprofessional, bad, good, professional, and exceeded. The check spelling and commit selected value immediately are checked.

Figure 2-10

The Dropdown Properties dialog box for the selections on page 4 of the customer survey

On page 4 you can see how by choosing a word that has a numeric value applied to it, the Final Result text field takes that information and adds or averages it to what is already calculated.

## Sum and Averaging Using Check Boxes or Radio Buttons with Text Fields

While text fields and drop-downs are useful for surveys on pages 1, 2, and 4, sometimes radio buttons and check boxes will do a more efficient job. Look at page 3 of the PDF file in your Start and End files, as shown in Figure [2-12](#449379_2_En_2_Chapter.xhtml#Fig12).

![](images/449379_2_En_2_Chapter/449379_2_En_2_Fig12_HTML.jpg)

A page of the tourmaline mining corporation. The customer's full name, the date, and the name of the customer's company are the necessary details in the form. There are 6 rating factors and further comments area. Reset comments and print are 2 buttons on the form below the comments area.

Figure 2-12

Page 3 of the customer survey using radio button and check boxes

### Using Radio Buttons on Page 3 of the Project

On the Options tab, the choices can be set to any number value (1, 2, 3, or even negative numbers if required). Figure [2-13](#449379_2_En_2_Chapter.xhtml#Fig13) displays how this is entered in the dialog box.

![](images/449379_2_En_2_Chapter/449379_2_En_2_Fig13_HTML.jpg)

A dialog box for radio button properties with an options tab is selected. The circle is the button style and the radio button choice selected is 1\. A prompt at the bottom reads to create a set of manually exclusive radio buttons give the fields the same name but different button choices.

Figure 2-13

Radio Button Properties dialog box’s Options tab on page 3 of the customer survey

Radio buttons as mentioned in Chapter [1](#449379_2_En_1_Chapter.xhtml) can all be part of the same group, so their name must be the same to function as one group, but the radio button choice of each button can be different. Refer to Figures [2-13](#449379_2_En_2_Chapter.xhtml#Fig13) and Figure [2-14](#449379_2_En_2_Chapter.xhtml#Fig14).

![](images/449379_2_En_2_Chapter/449379_2_En_2_Fig14_HTML.jpg)

2 dialog boxes for radio button properties with general and options tabs are selected, respectively. The name is inserted as group 1 and the button style is a circle with the radio button choice as 2.

Figure 2-14

Radio Button Properties dialog box’s Options tab on page 3 of the customer survey

However, to complete the calculation, you need to have at the end of the group row a hidden text field to accept the calculate field for that group, which you can set in the Calculate field with a sum. Refer to Figure [2-15](#449379_2_En_2_Chapter.xhtml#Fig15) and Figure [2-16](#449379_2_En_2_Chapter.xhtml#Fig16).

![](images/449379_2_En_2_Chapter/449379_2_En_2_Fig16_HTML.jpg)

A snip of the group's dropdown menu from 1 to 5\. 1 sales rep helpfulness underscore 3 option is highlighted. Group 2 dot 0 and the last option in the menu is 2 questions answered.

Figure 2-16

How the group of buttons appears in the Prepare Form Preview fields on the right side

![](images/449379_2_En_2_Chapter/449379_2_En_2_Fig15_HTML.jpg)

2 dialog boxes for text field properties with calculate and general tabs are selected, respectively. The value is the sum of the options in group 1\. The form field is hidden and the orientation is 0 degrees. The name and tooltip are sales rep helpfulness.

Figure 2-15

Text Field Properties dialog box’s General and Calculate tabs with the radio group selected on page 3 of the customer survey

Each radio group should have a hidden text field. In this case, there are six groups. Remember, to operate correctly, radio buttons must always be in groups of two or more. Refer to Figure [2-17](#449379_2_En_2_Chapter.xhtml#Fig17).

![](images/449379_2_En_2_Chapter/449379_2_En_2_Fig17_HTML.jpg)

A matrix of 5 circle field categories for 6 groups for rating 6 services provided to the customer. The final result is 0.

Figure 2-17

Radio button examples and hidden text fields with tab numbers on

Those text fields are then used to calculate the sum of the radio groups in the Final Result_3 text field on the Calculate tab, by selecting the hidden text boxes. The final result field is also set to Read Only, as in Figure [2-18](#449379_2_En_2_Chapter.xhtml#Fig18).

![](images/449379_2_En_2_Chapter/449379_2_En_2_Fig18_HTML.jpg)

2 dialog boxes for text field properties with calculate and general tabs are selected, respectively. The average value of the field is calculated. The name and the tooltip entered is final result. The form field is visible and the orientation is 0 degrees. The read only option is checked.

Figure 2-18

Text Field Properties dialog box’s Calculate and General tabs on page 3 of the customer survey

The Final Result text box is used to calculate the average rating of the other text hidden boxes linked to the radio button groups.

Note

With radio button calculations, rather than leaving them unchecked, you can set one in each group to “Button is checked by default” on the Options tab if you want to see numbers change automatically in the final text field. For instance, in this case an average may remain at 0 for a bit until more radio buttons are selected. You can also use the option of “Buttons with the same name and choice are selected in unison “ to select two or more radio buttons with the same value. However, keep in mind that may yield unexpected results and not add up correctly. Generally, in most situations radio buttons should operate independently, each with their own values, so keep that property unchecked. Refer to Figure [2-14](#449379_2_En_2_Chapter.xhtml#Fig14).

### Using Check Boxes on Page 3 of the Project

Figure [2-19](#449379_2_En_2_Chapter.xhtml#Fig19) shows how similar values can be entered into the Options tab of the Check Box Properties dialog box.

![](images/449379_2_En_2_Chapter/449379_2_En_2_Fig19_HTML.jpg)

A dialog box of check box properties with an options tab is selected. The box style selected is check and the export value is 1\. The check box style prior to finalizing various options is indicated above the dialog box.

Figure 2-19

The Options tab of the Check Box Properties dialog box on page 3 of the customer survey

Check boxes can be used in a comparable way on the Options tab and given an export value of 1, 2, 3…or even negative numbers for a negative rating.

In this example, you do not need any hidden text boxes, and the Final Result text field (read-only) can sum up the check boxes’ values. See Figure [2-20](#449379_2_En_2_Chapter.xhtml#Fig20).

![](images/449379_2_En_2_Chapter/449379_2_En_2_Fig20_HTML.jpg)

A dialog box for text field properties with calculate tab selected. The sum of the values of the following fields is calculated. 6 boxes are checked that represent the ratings.

Figure 2-20

An example of the Calculate tab with the check boxes selected on page 3 of the PDF and how the check boxes appear not grouped in the Prepare Form Preview Fields on the right-side fields list

Note

This text field also has custom validation script that we will discuss in more detail in Chapter [10](#449379_2_En_10_Chapter.xhtml).

### Turning Check Boxes into Radio Buttons

As we have seen in this example, the check boxes can operate independently and don’t need to be grouped.

However, check boxes can become radio buttons, and in certain situations you may prefer this method over the other when you want a square shape.

To program a check box to appear as a radio button, you can do the following steps. Place two or more check box fields on your page. Refer to Figure [2-21](#449379_2_En_2_Chapter.xhtml#Fig21).

![](images/449379_2_En_2_Chapter/449379_2_En_2_Fig21_HTML.jpg)

A snip of the general check boxes styles is indicated. The fields on page 1 are ticked for check boxes 1 and 2.

Figure 2-21

Adding check boxes to a page

Now in the Check Box Properties dialog box, make sure to name the boxes exactly the same as on the General tab. When you do this, a # and number will be added to the end of the name automatically. You do not add this symbol yourself. However, they are now grouped and will operate as one unit if the export values in the Options tab are the same. Refer to Figure [2-22](#449379_2_En_2_Chapter.xhtml#Fig22).

![](images/449379_2_En_2_Chapter/449379_2_En_2_Fig22_HTML.jpg)

2 dialog boxes for the check box properties. The name is given as check box 1 and the box style selected is check. The export value is yes.

Figure 2-22

Make sure the check box names are the same on the General tab and that the export values are the same on the Options tab

However, to turn the check box into a radio button, you need to change the Export Value field of one of the check boxes. Now when you use the boxes, they will toggle on and off as you click one or the other. Optionally, you can set one to be checked by default. Refer to Figure [2-23](#449379_2_En_2_Chapter.xhtml#Fig23).

![](images/449379_2_En_2_Chapter/449379_2_En_2_Fig23_HTML.jpg)

A dialog box of check box properties. The check option is inserted for the check box style and the export value is no. The second box in the second row is checked.

Figure 2-23

Set a different export value for one of the check boxes to turn them into radio buttons

On your own, try this with number calculations and see how a simple calculation with a text field can be accomplished by changing the export value in the check box to a number instead. See my example in `CheckBox_RadioButton.pdf`, and in the text field set the Calculate tab to “Value is the sum (+) of the following fields” and then pick the Check Box 1 group and make the text field read-only on the General tab. Refer to Figure [2-24](#449379_2_En_2_Chapter.xhtml#Fig24) for Calculate tab settings.

![](images/449379_2_En_2_Chapter/449379_2_En_2_Fig24_HTML.jpg)

A dialog box of text field properties. The calculate tab is selected. The value of the sum of the check box 1 is calculated.

Figure 2-24

Set the Calculate tab for your text field

Tip

If you need to get closer to your check boxes or any form field, remember to use your navigation - and + icons. You can also use the Ctrl/Cmd+ or Ctrl/Cmd- shortcut as well as the View ➤ Zoom menu.

## Basic Action Button Triggers for Reset Buttons and Printing Buttons

There are many basic actions that can be added to buttons, as you’ll see in Chapter [4](#449379_2_En_4_Chapter.xhtml) and later. However, for this lesson let’s focus on two that are used quite frequently in forms: the Reset and Print actions. Review page 1 in the `TMC_Customer Survey_End.pdf` file, if you need to follow along.

### Reset Button

If a client makes a mistake in several fields or wants to clear the entire form rather than highlight and press the Delete or Backspace key for each field, it is helpful to add a Clear or Reset button to your form. Figure [2-25](#449379_2_En_2_Chapter.xhtml#Fig25) displays what this action looks like on the Actions tab for a reset button.

![](images/449379_2_En_2_Chapter/449379_2_En_2_Fig25_HTML.jpg)

A dialog box of button properties. The actions tab is selected. The mouse up option is triggered and the action executes a menu item is selected. The action is to reset a form using the mouse-up option. The reset comments button is beneath the dialog box.

Figure 2-25

Button properties on the Actions tab

All fields or only the comments can be reset by a button; it’s your choice. Try this on your own form.

![](images/449379_2_En_2_Chapter/449379_2_En_2_Fig26_HTML.jpg)

A snip of a list of options in the action buttons. The various options in the dropdown menu are as follows, to open a web link, play a sound, play media, read an article, reset a form, run a javascript, set layer visibility, show or hide a field, and submit a form.

Figure 2-26

Select the action of “Reset a form”

1.  Make sure the select trigger is set to Mouse Up.

2.  Select the action “Reset a Form” from the Select Action menu list. Refer to Figure [2-26](#449379_2_En_2_Chapter.xhtml#Fig26).

![](images/449379_2_En_2_Chapter/449379_2_En_2_Fig27_HTML.jpg)

A highlighted rectangular box with the text add over it.

Figure 2-27

Add the action to the Actions tab

1.  Click the Add button to add the action. For a new action, this will bring you to the Reset a Form dialog box. Refer to Figure [2-27](#449379_2_En_2_Chapter.xhtml#Fig27) and Figure [2-28](#449379_2_En_2_Chapter.xhtml#Fig28).

1.  Or, if you have the reset action selected, click the Edit button at the bottom of the properties box to open the Reset a Form dialog box to select which fields you would like to reset, as in Figure [2-28](#449379_2_En_2_Chapter.xhtml#Fig28).

    ![](images/449379_2_En_2_Chapter/449379_2_En_2_Fig28_HTML.jpg)

    A dialog box of actions to reset a form. It has a list of satisfied, recommended, positive, and not positive options, met goals, and groups 1 to 6 are checked. The deselect all option is active while the select all option is disabled.

    Figure 2-28

    Edit the actions in the Reset a Form dialog box

![](images/449379_2_En_2_Chapter/449379_2_En_2_Fig29_HTML.jpg)

A dialog box for resetting a form. A list of the fields that can be reset are comments, over, handling of conflicts on site, and my overall feeling. The comments option is checked. The buttons to select and deselect all are adjacent to the fields box.

Figure 2-29

The Reset a Form options on the Actions tab

1.  In this example, I chose Deselect All, chose just the Comments field, and then clicked OK and closed the properties dialog box. Refer to Figure [2-29](#449379_2_En_2_Chapter.xhtml#Fig29).

Note

The label name for the Button field is on the Options tab.

### Print Form Button

Likewise, you can also create a Print button, as seen on page 1\. In the Button Properties dialog box, follow the same steps as with the reset button, but this time, from the Select Action list, you can choose “Execute a menu item” and click Add. Refer to Figure [2-30](#449379_2_En_2_Chapter.xhtml#Fig30).

![](images/449379_2_En_2_Chapter/449379_2_En_2_Fig30_HTML.jpg)

A dialog box for button properties. The mouse up trigger is chosen under the actions tab, which is visible. A menu item action is chosen to be performed. There is a print button underneath the dialogue box.

Figure 2-30

Print Form options on the Actions tab

From the Menu Item dialog box, select the File ➤ Print action if you want to create a Print button for the whole document. Then click OK. Refer to Figure [2-31](#449379_2_En_2_Chapter.xhtml#Fig31) and to page 1 of the project’s “End” PDF file to review this action.

![](images/449379_2_En_2_Chapter/449379_2_En_2_Fig31_HTML.jpg)

A menu item dropdown menu. The order that has been emphasized is the file, then print. The border of the O K button is highlighted. The mouse up action is followed by the menu item, the file command, and the print command in the actions box.

Figure 2-31

The File ➤ Print options on the Actions tab

This will open the Print dialog box when you click the button. You can edit or delete this action if required.

However, if you need only a specific page or pages to print in a range, you need to add JavaScript instead. To see a preview of this, refer to pages 2 and 4 of the “End” PDF file and to Figure [2-32](#449379_2_En_2_Chapter.xhtml#Fig32). I’ll discuss this more in later chapters. Though script placement can effect whether the document will print successfully. In this example the script on page 2 allows us to print but if the print Action script is placed on the final page the document will not print and will show an error.

![](images/449379_2_En_2_Chapter/449379_2_En_2_Fig32_HTML.jpg)

2 dialog boxes for button properties and JavaScript Editor. The tab for actions is selected. The Java script action is launched by selecting the mouse-up triggered option. a box where you may change and write JavaScript code to print the right page and add the number of pages to print.

Figure 2-32

Adding JavaScript on the Actions tab and closing the form

When you are done previewing and editing the form, click the Close button in the upper-right corner of the preview to close the Prepare Form tool.

## Summary

As you saw in this chapter, you can use the Prepare Form tool and its respective properties in a variety of ways to create customer rating surveys to suit your needs. By looking through the tabs, you can also see that many of the form fields have similar properties, while others have properties that relate only to that specific field. However, as you saw with the radio buttons and check boxes, there is a way to make one behave like the other for a specific project. You are now starting to add actions to buttons as well. As you progress through the lessons, you will discover how knowing which types of fields and properties to use will be important as the forms become more complex.

# 3. Creating a QR Code Custom Stamp

**Chapter Goal:** In this chapter, you’ll create a custom QR Code stamp with the Adobe Acrobat Stamp tool.

You’ve probably opened and viewed a PDF file that contains stamps and comments as well as customer changes and approvals. The application that made viewing those comments possible was most likely Adobe Acrobat Reader or Acrobat Pro. For many computer users, Reader has become the industry standard simply because it is a freeware program that anyone can download for the macOS or Windows platforms.

When you are reviewing the file with your client, Acrobat Pro allows you to view and add comments to a PDF document.

As mentioned in Chapter [1](#449379_2_En_1_Chapter.xhtml), Acrobat Pro also allows you to create barcodes and QR codes with the Prepare Form tool. However, the information generated in the barcode and QR code applies only to the surrounding form fields and not to specific text elsewhere in the document (see Figure [3-1](#449379_2_En_3_Chapter.xhtml#Fig1)). Currently, with the Comment tool, you cannot generate a custom QR code that you may want to use as a comment or stamp, so this chapter shows you a way to get around this situation.

![](images/449379_2_En_3_Chapter/449379_2_En_3_Fig1_HTML.jpg)

A screenshot of the barcode field properties has 5 tabs, of which the options tab is selected. It has 2 drop-down menus for the symbology, selected as Q R code, and decode conditions as document and handheld scanner. It has a Q R code at the bottom right side of the page.

Figure 3-1

The Barcode Field Properties dialog box with an example of a created QR code from the Options tab’s Symbology list

Note

If you want to work along in this lesson or review the final result, download the Chapter [3](#449379_2_En_3_Chapter.xhtml) files from the link mentioned in the introduction. You will find the original Adobe InDesign, QR images, and PDF files if you would like to edit them. See “QR Code Creation” later in the chapter for more details on this topic.

Remember that to view the properties of a field, you must select the Prepare Form tool. Only then can you right-click or double-click a field to review its properties.

## Customizing Your QR Code Stamp

It’s become popular to place a QR code on business cards, résumés, and newsletters; these codes can be read by smartphones. However, Acrobat only allows you to create a QR code that applies to forms. For your résumé or letterhead, you might want a QR code to contain the URL of your company website or just some text. The solution is to build your own custom QR code stamp that you can place in your online PDFs for clients to view. The following exercise will show you how.

Note

If you plan to use the QR code for professional print material, always place it into the original document (MS Word or Adobe InDesign Creative Cloud 2014 or later) rather than using the Acrobat Stamp tool as I will demonstrate next. While the stamp image will print fine on your home computer, it may not print when sent to some professional printers depending on if the layout requires altering. See details on how to do this in MS Word in the “Final Thoughts” section at the end of the chapter. You can also review the file `QRCode_Resume_Chapter3_End.pdf` as a reference in this chapter.

## QR Code Creation

Either create a QR code using InDesign CC 2014 or later, design software that allows you to generate a QR code, or ask a graphic designer in your company to create one for you. To work with the image, use a program like Adobe Photoshop to create a new document that is grayscale, is 200px width by 200px height, has a resolution of 72 pixels/inch, and is 8-bit. The graphic can also have a color mode of RGB if you are making a colorful stamp in InDesign. Refer to the “Summary” section for a link to steps on generating a QR code in InDesign.

Select Edit ➤ Copy from InDesign and select Edit ➤ Paste in Photoshop and use the application to scale your code on the canvas if it comes in too small or too large. Commit to the change by clicking the check mark in the Options panel. Make sure to use the Layers Panel menu to flatten the Image; then save the file as a JPEG on your computer and click OK. Afterward, test it with your smartphone’s Camera app to make sure it works correctly. See Figure [3-2](#449379_2_En_3_Chapter.xhtml#Fig2).

![](images/449379_2_En_3_Chapter/449379_2_En_3_Fig2_HTML.png)

A screenshot has preset details for untitled 1 object. It has a Q R code on the right side of the page, followed by a window with the layers tab selected and a Q R code for vector smart object. The bottom of the page has a dialog box for J P E G options with image and format options.

Figure 3-2

A generated QR code is added from InDesign to Photoshop and scaled and saved for use as an Acrobat Stamp

Open the PDF file in which you plan to add the QR code in Acrobat Reader or Acrobat Pro via File ➤ Open. Refer to Figure [3-3](#449379_2_En_3_Chapter.xhtml#Fig3).

![](images/449379_2_En_3_Chapter/449379_2_En_3_Fig3_HTML.jpg)

A screenshot for the Adobe Acrobat Pro 64-bit file has 6 options file, edit, view, e-sign, window, and help, of which the file option is selected. It has an icon to open at the bottom of the page.

Figure 3-3

The File menu’s Open item

Locate and select the PDF file you want to open.

Click the Stamp button in the Tools menu in the Share & Review section. Refer to Figure [3-4](#449379_2_En_3_Chapter.xhtml#Fig4).

![](images/449379_2_En_3_Chapter/449379_2_En_3_Fig4_HTML.jpg)

A screenshot has an icon for a stamp, followed by 2 options of home and tools, of which the tools option is selected, followed by a drop-down menu for add.

Figure 3-4

The Stamp tool

Select the Custom Stamps option. Choose Create from the drop-down. Refer to Figure [3-5](#449379_2_En_3_Chapter.xhtml#Fig5).

![](images/449379_2_En_3_Chapter/449379_2_En_3_Fig5_HTML.png)

A screenshot has 3 stamp icons for the stamp, stamps palette, and custom stamps, and an icon for Bates numbering. It has the custom stamps option selected with 2 options of create and manage, of which the create option is selected.

Figure 3-5

The Stamp Tool Cmenu

Create the custom stamp. When you choose this option, you will be presented with the Select Image for Custom Stamp dialog box. Click the Browse button to locate your file. Refer to Figure [3-6](#449379_2_En_3_Chapter.xhtml#Fig6).

![](images/449379_2_En_3_Chapter/449379_2_En_3_Fig6_HTML.jpg)

A dialog box to select an image for the custom stamp. It has a box to browse the file, followed by a box for sample, and 2 buttons for ok and cancel, respectively.

Figure 3-6

The Select Image for Custom Stamp dialog box

In Acrobat Pro, you can browse and use several different file formats including JPEG, TIFF, GIF, and PNG. Review the All Supported formats list to see what your options are. Refer to Figure [3-7](#449379_2_En_3_Chapter.xhtml#Fig7).

![](images/449379_2_En_3_Chapter/449379_2_En_3_Fig7_HTML.png)

A screenshot of a dialog box has the file name as Apress underscore Q R code dot j p g. It has a dropdown for all supported formats on the right side, followed by 2 buttons for open and cancel, respectively.

Figure 3-7

Browse for the QR code and select it

In this case, choose a JPEG graphic of the QR code that you or your graphic designer has already created. You can use my file called `Apress_QRCode.jpg` if you do not have a file.

Click the Open button and you will be returned to the previous dialog box with the file link now shown. Refer to Figure [3-8](#449379_2_En_3_Chapter.xhtml#Fig8).

![](images/449379_2_En_3_Chapter/449379_2_En_3_Fig8_HTML.jpg)

A screenshot of a dialog box to select an image for the custom stamp. It has a box to browse the file, followed by a Q R code for sample, and 2 buttons for ok and cancel, respectively.

Figure 3-8

The Select Image for Custom Stamp dialog box with a QR code visible

If you like how the image looks, click OK. Otherwise, browse for another image. You may need to make minor adjustments in a program like Photoshop for spacing needs to reduce or increase the size. If you do, make sure to keep the shape square and don’t distort the QR code. For example, in newer versions of Photoshop, holding down the Shift key while scaling can cause distortion. For now, use my image and click OK to proceed to the next dialog box.

Before you can use the custom stamp, Acrobat wants you to choose a folder category for your stamp and give it a name. You can either create a new folder by typing a name in or choose from current folders that are available in the Category drop-down list. Refer here to Figure [3-9](#449379_2_En_3_Chapter.xhtml#Fig9).

![](images/449379_2_En_3_Chapter/449379_2_En_3_Fig9_HTML.jpg)

A screenshot of a dialog box to create a custom stamp. It has a Q R Code, a selected box for the image for the custom stamp, a drop-down menu to select the category as Apress Stamp, and a box to enter the name as custom stamp.

Figure 3-9

The Create Custom Stamp dialog box

You have the option to downsample the stamp to reduce the file size. In the case of the QR code, I might uncheck this to preserve the quality. In this case, the file is only about 45KB, which is not large. When you are done, click OK to add that stamp to your collection.

Note

The Help Button link can guide you if you have additional questions about stamp creation and will take you to the following page:

[`https://helpx.adobe.com/acrobat/using/adding-stamp-pdf.html#create_a_custom_stamp`](https://helpx.adobe.com/acrobat/using/adding-stamp-pdf.html#create_a_custom_stamp)

## Using the Stamp Tool

The stamp is created. While still in your Stamp tool, you can go to your file folder in the Stamp drop-down menu and choose your new custom stamp. Refer to Figure [3-10](#449379_2_En_3_Chapter.xhtml#Fig10).

![](images/449379_2_En_3_Chapter/449379_2_En_3_Fig10_HTML.jpg)

A screenshot has 3 stamp icons for the stamp, stamp palette, custom stamps, and an icon for Bates numbering, followed by a button for Approved. It has a list of options, of which the option for an Apress stamp is selected with a Q R code at the right side of the page.

Figure 3-10

Created QR code stamp in the Stamp menu

At this point, a dialog box may appear that will request an initial identity setup. This is so the client will know who made this approval stamp for security reasons. You do not have to fill in all the boxes. When you’re done, click the Complete button. You should do this identity setup only once for your new stamp. If you do not see this box, it may mean that you or someone else has already set this area up. You can check this under Edit ➤ Preferences ➤ Categories: Identity and adjust your information there. Refer to Figure [3-11](#449379_2_En_3_Chapter.xhtml#Fig11) if you need to check your settings.

![](images/449379_2_En_3_Chapter/449379_2_En_3_Fig11_HTML.jpg)

A screenshot of a page has a selected Q R code, followed by a box for identify. It has text boxes to enter the login name, name, title, organization name, organizational unit, and email address.

Figure 3-11

Identity setup for new stamps as found in Preferences

The mouse cursor will now turn into the stamp. You can move it around on the page until you find where you want to place the stamp. Click the mouse button, and the stamp will be set. If you don’t like where it is set, you can move it around. The mouse turns into four arrows. If you hold down the mouse icon on the stamp, you can use the bounding box handles to move the stamp around, rotate (upper circle) it, or scale it. Use Edit ➤ Undo or Ctrl/Cmd+Z to undo a scaling or rotation.

If for some reason you need to delete the stamp later, while in the Stamp tool, go to the Custom Stamps ➤ Manage dialog box, and click Delete to remove the selected stamp from your custom list. Refer to Figure [3-12](#449379_2_En_3_Chapter.xhtml#Fig12).

![](images/449379_2_En_3_Chapter/449379_2_En_3_Fig12_HTML.jpg)

A screenshot has the custom stamps option selected with 2 options create and manage, of which the manage option is selected. It has a box for custom stamps and categories, followed by 3 buttons to create, edit and delete at the bottom, and a Q R code at the right side of the page.

Figure 3-12

The Stamp Tool menu and the Manage Custom Stamps dialog box

Note

There is no undo setting for deleting a custom stamp from this dialog box. If you delete it by mistake, click OK and then add the custom stamp again as well as the folder if it was removed. This dialog box will also allow you to create and edit a currently selected stamp category. Refer to Figure [3-9](#449379_2_En_3_Chapter.xhtml#Fig9). The Stamps Palette subtool can also help you access your custom stamp. Refer to Figure [3-12](#449379_2_En_3_Chapter.xhtml#Fig12).

Custom QR stamps, as in Figure [3-13](#449379_2_En_3_Chapter.xhtml#Fig13), can also have comments attached. You can attach a comment in the Comment tool section.

![](images/449379_2_En_3_Chapter/449379_2_En_3_Fig13_HTML.jpg)

A screenshot has an icon for comment and a box to open at the right side of the page. It has 1 comment with an applied stamp icon that reads, this is a Q R code stamp, followed by a button for the post. There is a comment icon with a stamp icon at the bottom of the page.

Figure 3-13

The Comment tool with Add Stamp subtool

Now your comments list has a stamp comment. Here you can add further information about the stamp by double-clicking in the comment to add text to a comment and then clicking Post. If you need to delete the QR code stamp on the page, just select it and press the Delete button on the keyboard or right-click and select Delete from a pop-up list. You can also use Ctrl/Cmd+Z if you need to undo your deletion.

You can add this QR code stamp to as many places in the document as required. When you’re done, click the Close button to exit the Stamp or Comment tool. Refer to Figure [3-14](#449379_2_En_3_Chapter.xhtml#Fig14).

![](images/449379_2_En_3_Chapter/449379_2_En_3_Fig14_HTML.jpg)

A screenshot of an oval-shaped button with text close written on it.

Figure 3-14

Close your current tool to exit

Tip

The Comment Tool also has an Add Stamp subtool with a drop-down menu that will allow you to access your custom stamp rather than entering the Stamp tool again. Refer to Figure [3-13](#449379_2_En_3_Chapter.xhtml#Fig13).

Then save the file and email the file to a client or post it on your website.

If you need to print the document (File ➤ Print, in the dialog box under Comments & Forms choose either the Document and Stamps or Document and Markups option in the Print dialog box. If you just choose the Document or “Form fields only” option, the QR code stamp will not print. Refer to Figure [3-15](#449379_2_En_3_Chapter.xhtml#Fig15).

![](images/449379_2_En_3_Chapter/449379_2_En_3_Fig15_HTML.jpg)

A screenshot of a box for comments and forms has a drop-down menu selected as a document and stamps. It has a tab to summarize comments at the right side of the box and 2 options of print and cancel at the bottom of the page, of which print is selected.

Figure 3-15

Print settings for stamps. Choosing Document and Stamps will ensure that the QR code stamp prints

## Final Thoughts: QR Code for Professional Printing

If you plan to send your résumé or a newsletter to a company and you want to ensure that the QR code will print regardless of the print document settings, I recommend typing your résumé/newsletter in a program like MS Word and then choosing the location where you want to place your QR code. Then in the menu, choose Insert ➤ Picture ➤ This Device and on your computer locate, select, and insert the file. Refer to Figure [3-16](#449379_2_En_3_Chapter.xhtml#Fig16).

![](images/449379_2_En_3_Chapter/449379_2_En_3_Fig16_HTML.jpg)

A screenshot of the Word main menu has the insert option selected. It has multiple insert options, of which the picture is selected, followed by 3 options for the location of the picture, selected as the device. It has a box at the bottom of the page for the file name, followed by an insert button.

Figure 3-16

Insert an image in MS Word

Locate your JPEG image and then click the Insert button. The image will be inserted. Finally, you would save the file and print your file as a PDF or use the Acrobat tab mentioned in Chapter [1](#449379_2_En_1_Chapter.xhtml). This will ensure that the QR code is embedded in the document and will print with the rest of your résumé.

## Summary

In this chapter, you learned how you can turn a QR code into a custom stamp that you can use in a form for any PDF document. The stamp can also be modified and scaled, and a comment can be added.

Creating the QR code in this manner, rather than just as a form field, allows for a wider range of possibilities.

For more information about how to create a QR code in Adobe InDesign, visit [`https://helpx.adobe.com/indesign/using/generate-qr-code.html`](https://helpx.adobe.com/indesign/using/generate-qr-code.html).

# 4. Buttons, Navigation, Form, and Nonform Actions

**Chapter Goal:** You will learn about actions you can create for buttons that are for forms and nonform activities as well as improving navigation within the document with basic actions.

In Chapter [2](#449379_2_En_2_Chapter.xhtml), you saw a few simple examples of actions you could create with buttons. Now you will focus on a few more. You’ll do the following in this chapter:

*   Reviewing the Actions tab’s properties in various fields and nonform items

*   Looking at built-in triggers and actions that require no coding

*   Applying what you discover to buttons for page navigation

*   Using button icons rather than just text

*   Understanding how a button can become a help icon to show or hide information in an order form

*   Understanding how a check box can show or hide information in combination with a reset button

*   Working with PDF Layers Bookmarks and Buttons

Note

If you want to work along in this lesson or review the final result, download the Chapter [4](#449379_2_En_4_Chapter.xhtml) files from the link in the introduction. The file with the label “Start” is the file without the code, and the file with the label “End” is the final result. You will also find folders with original MS Word, PDF, and related InDesign/Illustrator files if you would like to edit them, and you will find a folder containing the original images of the button icons if you would like to add them to your own PDF forms.

To view the properties of a field, you must select the Prepare Form tool. Only then can you right-click or double-click a field to review its properties.

If you are creating your form from an original PDF that contains no form fields, refer to the “Forms Review” section in Chapter [1](#449379_2_En_1_Chapter.xhtml).

## Creating a Button Icon

Not all buttons have to be just text; you can also have text and an image or just an image that you created in a program like Adobe Photoshop or GIMP. As in the QR code stamp example in Chapter [3](#449379_2_En_3_Chapter.xhtml), the file can be in many formats including JPEG, GIF, or PNG.

In this example, you can refer to the “Start” and “End” PDF files for `Newsletter_1_Navigation`. On page 1 of the End file, look at the Next Page button for reference to its properties. Currently, in the Options tab Layout section, it is set to “Label only,” so only a label name can be typed into the Label field. The icon and advanced areas are therefore disabled, though you can alter the Behavior setting from Invert to None, Push, or Outline. We will discuss how the Behavior setting affects the icon and label states later in the chapter. Refer to Figure [4-1](#449379_2_En_4_Chapter.xhtml#Fig1).

![](images/449379_2_En_4_Chapter/449379_2_En_4_Fig1_HTML.png)

A screenshot of the button properties page has 5 tabs for general, appearance, position, options, and actions, of which the options tab is highlighted. It has a drop-down menu for the layout as label only, behavior as invert, and state as up. The 3 options are enlarged at the bottom of the page.

Figure 4-1

The Options tab with the Choose Icon button disabled

However, if in the Options tab you change the layout to another option that contains the word *Icon*, then you can access the Choose Icon button to locate an image using the Select Image dialog box. After you choose an image, the icon will display with the properties. You will see an example of the TMC_Order Form “End” PDF later in the chapter. Refer to Figure [4-2](#449379_2_En_4_Chapter.xhtml#Fig2).

![](images/449379_2_En_4_Chapter/449379_2_En_4_Fig2_HTML.png)

A screenshot of the button properties page has 5 tabs, of which the options tab is highlighted. It has a drop-down menu for the layout as an icon only and a box for an icon as a question mark. It has a pop-up window to select an image with a browse option for a file and a question mark as a sample.

Figure 4-2

The Options tab with the Choose Icon button enabled and the Select Image dialog box

The Layout list allows you to choose the layout setting of the icon only or display the icon with a label if you want to use the icon in combination with text.

To review, the layout options are as follows:

*   Label only (this setting will not allow you to add an icon)

*   Icon only (you cannot add a label)

*   Icon top, label bottom

*   Label top, icon bottom

*   Icon left, label right

*   Label left, icon right

*   Label over icon

After an icon layout is chosen, you can click the Advanced button. Clicking the Advanced button will open the Icon Placement dialog box, where you can adjust the placement of the icon precisely inside the button. You can set When to Scale (Always, Never, Icon is too big, Icon is too small), Scale (Proportionally or Non-Proportionally), and Fit to bounds. You can also move the two sliders to adjust where the icon should sit and reset everything to original settings (refer to Figure [4-3](#449379_2_En_4_Chapter.xhtml#Fig3)). Click OK to commit the settings, or click Cancel to exit without saving the changes.

![](images/449379_2_En_4_Chapter/449379_2_En_4_Fig3_HTML.jpg)

A screenshot for the icon placement window has a drop-down menu for scaling time selected as always and an option for scale selected as proportionally. It has 2 squares for the button and icon with sliders and an ok button at the bottom of the page.

Figure 4-3

Clicking the Advanced button in the Options tab opens the Icon Placement dialog box

### Example of a Button as a Label Only

Figure [4-4](#449379_2_En_4_Chapter.xhtml#Fig4) shows an example of a button with only a label applied, as found in the Newsletter 1 Navigation “End” file on page 1\. This button’s label says “Next Page” because you want the user to go to the next page when clicking the button. The label is a helpful hint so that the user will know what will happen when the button is clicked. These types of buttons can be used in any document that contains three or more pages.

![](images/449379_2_En_4_Chapter/449379_2_En_4_Fig4_HTML.jpg)

A screenshot of a button with text next page within it.

Figure 4-4

Label-only button

Note

The current state is Up, based on the behavior of the button, which is Invert, or the color inverts when clicked. None creates no color change, and Outline inverts only the edge of the button. These have a state of Up.

A behavior such as Push will give other options of states such as Up, Down, and Rollover; instead of one icon or label, you could now add three. This would cause the button to change as you activated each state. See Figure [4-5](#449379_2_En_4_Chapter.xhtml#Fig5) for some of the options. While the label and icon will change, you will not be able to set a different fill color for each state using the Appearance tab.

![](images/449379_2_En_4_Chapter/449379_2_En_4_Fig5_HTML.jpg)

2 screenshots of the button properties with the options tab selected. 1 has a drop-down menu for behavior as invert, state as up, and label as next page. 2 has behavior as push, state as down, and label as click. It has 3 buttons for the next page, click, and rollover at the bottom of the page.

Figure 4-5

Example of various behaviors and states applied to a button

### Example of a Button Combination of Icon Only

Figure [4-6](#449379_2_En_4_Chapter.xhtml#Fig6) shows an example of a three-button group that has an icon image and some actions applied to them. We will look at this more closely later in the chapter. However, without actions, these buttons would function independently and not interact with each other.

![](images/449379_2_En_4_Chapter/449379_2_En_4_Fig6_HTML.jpg)

A screenshot has 3 elements. It has a question mark icon, a button for close, and a combination of a button and an icon followed by a line of text that reads, the 9-digit code that was mailed to you.

Figure 4-6

Example button group with different show and hide fields applied to each button in the TMC Order Form End PDF file

If you enter each of the button’s properties, you will see, as in Figure [4-7](#449379_2_En_4_Chapter.xhtml#Fig7), various hide and show actions set on the Actions tab, which we will look at later in this chapter in detail.

![](images/449379_2_En_4_Chapter/449379_2_En_4_Fig7_HTML.jpg)

2 screenshots of pages. Page 1 for button properties has 5 tabs, of which the actions tab is selected. It has a drop-down menu to select a trigger as mouse up and action as menu item execution. Page 2 for the show or hide field, has a list of fields, from which the info window is selected.

Figure 4-7

Example button group with show and hide fields applied

All actions in the Actions tab, once selected, can be as follows:

*   Added using the Add button

*   Rearranged using the Up and Down buttons

*   Edited using the Edit button

*   Deleted using the Delete button

Note

With the Show/Hide action, you need to apply the action to each field one at a time. Some actions, such as Reset, allow you to apply the actions to more than one field or collectively. Refer to Figure 2-28 in Chapter [2](#449379_2_En_2_Chapter.xhtml).

## Nonform Properties Actions

The following sections take a quick look at the kinds of properties you can apply actions to that are not part of forms.

### Pages

For this example, refer to the Newsletter 1 Navigation PDF End file or use the Start file for practice; see Figure [4-8](#449379_2_En_4_Chapter.xhtml#Fig8).

![](images/449379_2_En_4_Chapter/449379_2_En_4_Fig8_HTML.jpg)

A screenshot of a p d f newsletter has 3 tabs, of which the navigation tab is selected. It has 4 options for page thumbnails on the left side of the window. The middle panel has a layout format for the newsletter, and the right side of the page has different options for the p d f file.

Figure 4-8

Accessing the page thumbnails

Under View ➤ Show/Hide ➤ Navigation Panes ➤ Page Thumbnails, you will find on the left side of Acrobat all the page thumbnails of your current document.

Select one of the pages such as page 1 in the Thumbnail section, and from the Options drop-down choose Page Properties at the bottom of the options list to access this dialog box. Then click the Actions tab (refer to Figure [4-9](#449379_2_En_4_Chapter.xhtml#Fig9)).

![](images/449379_2_En_4_Chapter/449379_2_En_4_Fig9_HTML.jpg)

A screenshot of the page properties page has 2 tabs for tab order and actions, of which the actions tab is selected. It has 2 drop-down menus to select a trigger as the page opens and an action to execute a menu item, followed by a text box for actions, and an ok button at the bottom of the page.

Figure 4-9

The Page Properties option on the Actions tab

Pages can have actions applied for improved navigation. The trigger, which we will look at in more detail later for forms, can be upon page open or page close. However, not all selected actions, which we will review shortly, may be compatible with page properties, so you may want to take some time to plan what type of action is appropriate, such as executing the Edit ➤ Find dialog box when you arrive on that page or playing a sound. Remember that you can access this area on the left side of Acrobat in the Page Thumbnail section. Alternately, you can right-click a thumbnail when no other menu items are active and choose Page Properties from the list. Refer to Figure [4-10](#449379_2_En_4_Chapter.xhtml#Fig10).

![](images/449379_2_En_4_Chapter/449379_2_En_4_Fig10_HTML.jpg)

A screenshot of a page has 2 tabs for tab order and actions, of which the actions tab is selected. It has 2 drop-down menus to select a trigger as the page opens and an action to execute a menu item, followed by a list of options with a text box for actions.

Figure 4-10

The Page Properties option on the Actions tab with a possible action applied for that specific select page

Once you have chosen an action, you can then click OK to confirm or exit to cancel. Refer to Figure [4-9](#449379_2_En_4_Chapter.xhtml#Fig9).

### Bookmarks

For this example, refer to the Newsletter 1 Navigation End PDF file.

Under View ➤ Show/Hide ➤ Navigation Panes ➤ Bookmarks, you will find on the left side of Acrobat all the bookmarks of your current document (refer to Figure [4-11](#449379_2_En_4_Chapter.xhtml#Fig11)).

![](images/449379_2_En_4_Chapter/449379_2_En_4_Fig11_HTML.jpg)

A screenshot of the bookmarks page has different icons. It has 2 bookmark icons for Chapter 1 and Chapter 2, with Chapter 1 selected.

Figure 4-11

Bookmarks found in the Newsletter file in the Navigation Panes area

In Acrobat Pro, bookmarks are easily created when you highlight text in a document and then click the Add a New Bookmark icon.

In this example, select one of the bookmarks and from the Options menu at the bottom of the list choose Properties. This will allow you to add or view the actions for the bookmark (refer to Figure [4-12](#449379_2_En_4_Chapter.xhtml#Fig12)) when you click the Actions tab.

![](images/449379_2_En_4_Chapter/449379_2_En_4_Fig12_HTML.jpg)

A screenshot of the bookmark properties page has 2 tabs for appearance and actions, of which the actions tab is selected. It has a drop-down menu to select action as executing a menu item, followed by a list of actions within a box for actions.

Figure 4-12

Bookmark properties on the Actions tab

Remember that you can access this area on the left side of Acrobat in the Bookmark Thumbnail section. Alternatively, you can right-click a bookmark and choose Properties from the list.

In this example, the Add action area shows that the bookmark has no trigger. It is assumed that clicking in the bookmark is the trigger. And the bookmark’s main purpose is to navigate to a page. In this case, when the bookmark was created, it was set to a default select action of “Go to a page in this document” for page 1\. The Zoom level was set to Fit Page using the dialog box. Select the action and click the Edit button to see the options and edit, if required, the Zoom options. Refer to Figure [4-13](#449379_2_En_4_Chapter.xhtml#Fig13).

![](images/449379_2_En_4_Chapter/449379_2_En_4_Fig13_HTML.jpg)

A screenshot for going to a specific page in the document has 2 options page number and named destination. The use page number option is selected with a text box to enter the page number, followed by a drop-down menu for Zoom selected as the fit page and destination selected as destination 1.

Figure 4-13

“Go to a page in this document” dialog box

Alternatively, you can set a named destination. You will find an example of this in the file on page 3, and it can be selected by clicking the Browse button. Then select one and click OK to confirm. Look in the Destinations area on the left if you need to locate this option. Select View ➤ Show/Hide ➤ Navigation Panes ➤ Destinations. For details, refer to Figure [4-13](#449379_2_En_4_Chapter.xhtml#Fig13) and Figure [4-14](#449379_2_En_4_Chapter.xhtml#Fig14).

![](images/449379_2_En_4_Chapter/449379_2_En_4_Fig14_HTML.jpg)

A screenshot of the destinations page has different icons, followed by 2 columns for name and page. It has the name selected as destination 1 and the page selected as 3.

Figure 4-14

A named destination in the thumbnail area with a page set

Destinations can be created and named like bookmarks. When some text is highlighted, you click the Create New Destination button. On their own they contain no actions or properties, so they must be used in conjunction with a bookmark for better navigation.

### Web and Custom Hyperlinks

For this example, refer to the Newsletter 1 Navigation PDF End PDF file.

You can access this area under Tools ➤ Edit PDF ➤ Link ➤ Add/Edit Web or Document Link. If a highlighted link already exists on a specific page, it will appear with a box around it. If not, you can select this option from the menu and then drag to highlight a selection of text to apply a hyperlink to. For example, try this in the “Start” file on page 4 and highlight the name of the company while using this tool and setting (refer to Figure [4-15](#449379_2_En_4_Chapter.xhtml#Fig15)).

![](images/449379_2_En_4_Chapter/449379_2_En_4_Fig15_HTML.jpg)

A screenshot of a page has an edit p d f option, followed by an icon for the link and an option to add or edit web or document links. A create link page at the bottom of the window has details of the link appearance and link action, followed by a next button.

Figure 4-15

Accessing link properties when a custom link is chosen

In the Create Link dialog box, you can then choose a link appearance so that the user can see the link’s Link Type, Highlight Style, Line Thickness, Line Style, and Color settings applied. Then set a link action of one of these: Go to page view, Open a file, Open a web page, or Custom Link. Then click Next (see Figure [4-15](#449379_2_En_4_Chapter.xhtml#Fig15)). Based on the action chosen, you will be presented with different choices in the link dialog box. In the case of a custom link, you will be taken to Link Properties, where you can select a custom action from the menu on the Actions tab that will trigger when you click the link. Refer to Figure [4-16](#449379_2_En_4_Chapter.xhtml#Fig16).

![](images/449379_2_En_4_Chapter/449379_2_En_4_Fig16_HTML.jpg)

3 screenshots. 1 for creating a go-to-view page has instructions to select the target view, and set a link to create the link destination. 2 for edit U R L page has a text box to enter a U R L for the link. 3 for the link properties page has the actions tab with a drop-down menu to select an action.

Figure 4-16

Accessing link actions and properties on the Actions tab

In this case, I set a custom link to execute a menu item, clicked Add to display the attachments’ thumbnails (View ➤ Show/Hide ➤ Navigation Panes ➤ Attachments), and clicked OK. The action is now applied. Refer to Figure [4-17](#449379_2_En_4_Chapter.xhtml#Fig17).

![](images/449379_2_En_4_Chapter/449379_2_En_4_Fig17_HTML.jpg)

A screenshot of the link properties page has 2 tabs for appearance ad actions, of which the actions tab is selected. It has a drop-down menu to select an action, followed by a list of actions, and a screenshot of the action box at the bottom of the page with a web link.

Figure 4-17

Accessing link actions and properties on the Actions tab

Or choose an action like “Open a web link,” as shown on page 1\. Refer to Figure [4-17](#449379_2_En_4_Chapter.xhtml#Fig17).

If I needed to edit this custom link while in the Edit PDF Tool on the Add/Edit Link option, I could then right-click the link, choose Properties from the list, select the action to edit, and then delete the old action. Refer to Figure [4-17](#449379_2_En_4_Chapter.xhtml#Fig17) and Figure [4-18](#449379_2_En_4_Chapter.xhtml#Fig18).

![](images/449379_2_En_4_Chapter/449379_2_En_4_Fig18_HTML.jpg)

A screenshot of a page has a line of selected text. It has a list of options, including edit, align, center, distribute, size, current appearance, and properties, of which the properties option is selected.

Figure 4-18

Accessing link actions and properties by right-clicking the link

You can then on the upper-right side click the Close button to exit the Edit PDF tool and save the document.

Alternately, outside of the Edit PDF tool, you can select the link by right-clicking it and choosing Edit Link from the list (refer to Figure [4-19](#449379_2_En_4_Chapter.xhtml#Fig19)).

![](images/449379_2_En_4_Chapter/449379_2_En_4_Fig19_HTML.jpg)

A screenshot of a page has a few lines of text, of which the first line is selected. It has a pop-up window with 2 options, delete link and edit link, of which the edit link option is selected.

Figure 4-19

Accessing the link properties on the Actions tab outside of the Edit PDF tool

Alternatively, you could apply this same custom hyperlink to a graphic that would link to the attachment thumbnails. In this case, I also added a comment sticky note for additional clarification. Refer to Figure [4-20](#449379_2_En_4_Chapter.xhtml#Fig20).

![](images/449379_2_En_4_Chapter/449379_2_En_4_Fig20_HTML.jpg)

A screenshot has a selected map with a question map icon, followed by a comment that reads, look in attachments for the other map. It has a list of attachments at the bottom left side of the page, with map 1 selected, and a list of options on the right side with an open attachment option selected.

Figure 4-20

Adding a Custom Hyperlink action to a graphic so an attachment can be located within the PDF file

Tip

You can use the Help button in the Create Link dialog box if you want more clarification on certain link actions ([`https://helpx.adobe.com/acrobat/using/links-attachments-pdfs.html`](https://helpx.adobe.com/acrobat/using/links-attachments-pdfs.html)). Refer to Figure [4-15](#449379_2_En_4_Chapter.xhtml#Fig15).

### Rich Media Nonform Navigation Buttons

For this example, refer to the Floor Plan Layout “End” PDF file.

Buttons can be added either via the Prepare Form tool or via the Rich Media tool (refer to Figure [4-21](#449379_2_En_4_Chapter.xhtml#Fig21)).

![](images/449379_2_En_4_Chapter/449379_2_En_4_Fig21_HTML.jpg)

A screenshot of a page has an icon for rich media followed by an add option for 3D, button, sound, video, and object. It has a screenshot of button properties with 5 tabs, of which the actions tab is selected. It has 2 drop-down menus to select a trigger and an action, followed by a list of actions.

Figure 4-21

Rich Media tool’s Button Properties dialog box on the Actions tab

Nonform button actions can be used for video media, 3D models, navigation, or later for forms while working with the Rich Media tool. You can select a button with the Select object tool and then right-click the button to access the properties. However, after you close the file and open it again, to access the buttons properties, you will need to go into the Prepare Form tool to edit the button actions because you may alter or lose access after exiting the Rich Media tool, especially if you have later added other form elements. The button properties for rich media are the same as those for forms.

### Layers Basic Actions

For this example, refer to the Floor Plan Layout PDF file.

Under View ➤ Show/Hide ➤ Navigation Panes ➤ Layers, you will find on the left side of Acrobat all the layers of your current document (refer to Figure [4-22](#449379_2_En_4_Chapter.xhtml#Fig22)).

![](images/449379_2_En_4_Chapter/449379_2_En_4_Fig22_HTML.jpg)

A screenshot of a page has an icon for the layers. It has an expanded view of the Chapter 3 floor plan layout with 4 options of vendor numbers, seating layout, title, guides, and grids, of which the title option is highlighted.

Figure 4-22

Access to the layers in the Navigation Panes area

To access the layer properties, you can select a layer, and then from the Options menu, choose Layer Properties from the bottom of the list. Alternately, you can right-click a layer and choose Properties (refer to Figure [4-23](#449379_2_En_4_Chapter.xhtml#Fig23)).

![](images/449379_2_En_4_Chapter/449379_2_En_4_Fig23_HTML.png)

A screenshot of the layer properties page has a text box to enter the layer name, 2 options for intent, a drop-down menu for default states, details of the initial state, including 3 drop-down menus for visibility, print, and export, and a box for creator information.

Figure 4-23

Layer properties as seen in the Floor Plan example when you select a layer and either choose from the Options menu or right-click the layer in the navigation pane

Layer actions, compared to some button navigation setups, can be a bit more complex to build. I recommend using a program like InDesign to create your initial layer layout and export it as an interactive PDF, as you can see in the Creator Info area. However, here are some tips to get you started with the Layer Properties dialog box:

*   Layer actions have some similarities to Acrobat page transitions.

*   Layers have a name and have an intent of view or reference.

*   Layers can have a default state of on or off.

*   They can be locked or unlocked.

*   You can set a Layers Initial State for the following:

    Visibility (On, Never, or Always Visible)

    Print (When Visible, Never, or Always)

    Export (When Visible, Never, or Always)

*   Layers are good for overlays to show, for example, optional layouts of a room or floor. The layers can be separated and exported from a program such as Adobe InDesign when you create an interactive PDF.

*   Unlike other actions, they can be tricky to set up and are best kept to one or two layers.

Later I will show how you can add some buttons to make the layer more interactive in the PDF to show and hide layer visibility.

As you can see, most of these nonform properties can be accessed in the Navigation Panes/thumbnail area in Acrobat. The same is true for layers.

### Other Nonform Actions That Can Affect Sticky Note or Markup Tools Indirectly

One final thing I will point out here and later in Chapter [8](#449379_2_En_8_Chapter.xhtml) with Time Stamps is that nonform or form buttons, when they are programed with custom JavaScript, can be set to alter the appearance of a placed comment or stamp. However, while stamps, comments, markup tools, and measurements do have properties that you can access when you right-click them, they do not have an Actions tab. So, by themselves no action can be applied without some indirect trigger applied first to a button or form field.

## Triggers for Actions

Before you choose an action from the Actions tab, it is important to select a trigger.

Trigger + Select Action = Result You Want

The most common trigger to use in digital forms is Mouse Up, as shown in Figure [4-24](#449379_2_En_4_Chapter.xhtml#Fig24).

![](images/449379_2_En_4_Chapter/449379_2_En_4_Fig24_HTML.jpg)

A screenshot of a page has 5 tabs for general, appearance, position, options, and actions, of which the actions tab is selected. It has a block to add an action with a drop-down menu to select a trigger. It has multiple options, and the mouse-up option is selected.

Figure 4-24

Various action triggers that can be selected

The following describes this and a few other actions you may encounter:

*   **Mouse Up**: When the mouse button is released after a click. This is the most common button trigger because it gives the user one last chance to drag the pointer off the button and not activate the action.

*   **Mouse Down**: When the mouse button is clicked (without being released). In most cases, Mouse Up is the preferred trigger as down does not give the user any time to change their mind. While it might not be as apparent with a button with a check box, this trigger might be too fast.

*   **Mouse Enter**: When the pointer enters the field or play area.

*   **Mouse Exit**: When the pointer exits the field or play area.

*   **On Receive Focus (media clips only)**: When the link area receives focus, either through a mouse action or tabbing. This is also called On Focus.

*   **On Lose Focus (media clips only)**: When the focus moves to a different link area. This is also called On Blur.

Bookmarks and hyperlinks are exceptions; they do not have an available trigger.

It is assumed that you will click the link with your mouse or finger and that the action of going to that page or URL will be executed.

However, pages as mentioned do have a select trigger of upon page open or page close.

Refer to the following Adobe page for more details on additional media clip triggers: [`https://helpx.adobe.com/acrobat/using/applying-actions-scripts-pdfs.html`](https://helpx.adobe.com/acrobat/using/applying-actions-scripts-pdfs.html).

## Choose an Action That Requires No Code

There are many actions you can choose from. As you can see in Figure [4-25](#449379_2_En_4_Chapter.xhtml#Fig25), once you choose a trigger, you can set it to execute a selected action when you click the Add button. Upon exiting the dialog box, the action applied to the button will allow you to move to another page or print all the pages in the document, for example.

![](images/449379_2_En_4_Chapter/449379_2_En_4_Fig25_HTML.jpg)

A screenshot of a page has 5 tabs, of which the actions tab is selected. It has a block to add an action with 2 drop-down menus to select a trigger, of which the mouse-up option is selected, and a drop-down menu to select an action, of which the execute menu item is selected.

Figure 4-25

Various actions that can be selected

You will look at some specific actions shortly. However, take a moment to review some of the select actions that are available, as listed here:

![](images/449379_2_En_4_Chapter/449379_2_En_4_Fig26_HTML.jpg)

A screenshot of a page has options under articles and an icon for article 1\. It has a pop-up window to select an article with a list of article titles, of which article 1 is selected. It has an ok button at the bottom part of the pop-up window.

Figure 4-26

Select Article dialog box and article thumbnail present in PDF

*   **Execute a menu item**: Executes a specified menu command as the action. There are at least 50 menu item options in this area.

*   **Go to a 3D/multimedia view**: Jumps to the specified 3D view. These 3D annotation views must already be present in the document. This is more for working with nonform buttons.

*   **Go to a page view**: Jumps to the specified destination in the current document. Use the scroll bars, mouse, and zoom tools to select the target view and then press the set link button in the dialog box to create the link destination.

*   **Import form data**: Brings in form data from another file and places it in the active form. This select file is in FDF format.

*   **Multimedia operation (Acrobat 9 and later)**: Executes a specified action for a multimedia object or annotation in the file (such as playing a sound file). The multimedia object must be added to the file before you can specify an action for it in the dialog box.

*   **Open a file**: Launches and opens a linked file that is in the same folder as the PDF. If you are distributing a PDF file with a link to another file, the reader needs the native application of that linked file to open it successfully. You may need to add information on opening preferences for the target file such as in the form of a comment.

*   **Open a web link**: Jumps to the specified URL destination on the Internet. You can use HTTP, HTTPS, FTP, and mailto protocols to define your link.

*   **Play a sound**: Plays the specified sound file, in WAV or AIFF format. The sound is embedded into the PDF document in a cross-platform format.

*   **Play media (Acrobat 5, Acrobat 6, and later compatible)**: Plays the specified QuickTime or AVI movie that was created as Acrobat 5 or 6 compatible. The specified movie must be embedded in a PDF document. If no media of this kind is present in the document, then the action will not be created.

*   **Read an article**: Follows an article thread or box in the active document. They are created with the Edit PDF tool. An article must be present in the left pane’s article thumbnails for this action to be executed. Select View ➤ Show/Hide ➤ Navigation Panes ➤ Articles. Refer to Figure [4-26](#449379_2_En_4_Chapter.xhtml#Fig26).

![](images/449379_2_En_4_Chapter/449379_2_En_4_Fig27_HTML.jpg)

A screenshot of an actions page has 2 options for mouse enter and mouse exit, each with an option to show or hide a field. The first option for mouse enter is selected. It has 4 buttons at the bottom side of the page, of which the delete button is selected.

Figure 4-27

Example of setting the order for show and hide a field actions

*   **Reset a form**: Clears previously entered data in a form. You can control the fields that are reset with the Select Fields to Reset dialog box, as shown in Chapter [2](#449379_2_En_2_Chapter.xhtml).

*   **Run a JavaScript**: Runs the specified JavaScript. This requires JavaScript to do some custom action that is entered into the JavaScript Editor.

*   **Set layer visibility**: Determines which layer settings are active. Before you add this action, specify the appropriate layer settings for a current state. This works with the layer properties, as we will see later in the chapter.

*   **Show/hide a field**: Toggles between showing and hiding a field in a PDF document. This option is especially useful in form fields. For example, if you want an object to pop up whenever the pointer is over a button, you can set an action that shows a field on the Mouse Enter trigger and hides a field on Mouse Exit. Note in this case two actions will be present on your Actions tab. You will then need to move the actions up or down in the list as well as edit them one at a time if you need to make corrections. Refer to Figure [4-27](#449379_2_En_4_Chapter.xhtml#Fig27).

![](images/449379_2_En_4_Chapter/449379_2_En_4_Fig28_HTML.jpg)

A screenshot of the submit form selection page. It has a box to enter the U R L for the link, followed by options for export format selected as p d f, field selection as all fields, and data options. It has an ok button at the bottom right side of the page.

Figure 4-28

Submit Form Selections dialog box

*   **Submit a form**: Sends the form data to the specified URL or email. You may need someone in your IT department to help you test this first and choose the correct export format, field selection, and date options (refer to Figure [4-28](#449379_2_En_4_Chapter.xhtml#Fig28)).

It is important to note, in the case of the first option “Execute a menu item,” the additional 50 or more possible variations to the action will appear when the Add button is clicked. These include assorted options for page and document navigation. Included is View ➤ Page Navigation ➤ Go To page, which is a similar setting for bookmarks. Menu items fall into several categories, mostly found in the main menu: File, Edit, View, Bookmarks, Miscellaneous, and Tools.

Figures [4-29](#449379_2_En_4_Chapter.xhtml#Fig29) and [4-30](#449379_2_En_4_Chapter.xhtml#Fig30) illustrate what options are available.

![](images/449379_2_En_4_Chapter/449379_2_En_4_Fig30_HTML.jpg)

2 screenshots of the menu item page. Each page has a list of options, with 2 buttons for ok and cancel at the bottom of the page, of which the ok button is selected.

Figure 4-30

The many different menu options that are available

![](images/449379_2_En_4_Chapter/449379_2_En_4_Fig29_HTML.jpg)

A screenshot of the button properties page has 5 tabs for general, appearance, position, options, and actions, of which the actions tab is selected. It has 2 drop-down menus to select a trigger as mouse up and action as the execution of a menu item. It has a box for actions with a list of actions.

Figure 4-29

An example of an action added to the Actions tab from the selected action “Execute a menu item”

As mentioned, on the Actions tab, more than one action can be added to a trigger. as Also, separate triggers with different actions for each field can be added; just select them and click the Add button. One example is to show and hide items on a form; refer to Figure [4-27](#449379_2_En_4_Chapter.xhtml#Fig27) and Figure [4-31](#449379_2_En_4_Chapter.xhtml#Fig31), depending on the choice of field. A field or button with these actions applied shows some fields and then hides others.

![](images/449379_2_En_4_Chapter/449379_2_En_4_Fig31_HTML.png)

A screenshot of the text field properties page has 8 tabs, of which the actions tab is selected. It has a drop-down menu for triggers selected as mouse exit and actions as show or hide a field. It has a dialog box for the show or hide field with a list of fields, and button 2 is selected.

Figure 4-31

Multiple triggers and actions applied to one field

## Newsletter Navigation with Buttons

Open the Newsletter 1 Navigation End file in the Chapter [4](#449379_2_En_4_Chapter.xhtml) folder. Test the buttons. You can use labels with or without images to enhance the navigation experience. See Figure [4-32](#449379_2_En_4_Chapter.xhtml#Fig32) for the buttons with an example of a navigation action applied.

![](images/449379_2_En_4_Chapter/449379_2_En_4_Fig32_HTML.jpg)

A screenshot of the button properties page has 5 tabs, of which the actions tab is selected. It has a drop-down menu for the trigger as mouse up and action as executing a menu item. It has 2 buttons for the next and previous page, of which the next page button is selected.

Figure 4-32

Applying an action to a button to navigate to another page (see pages 1–3 of the Newsletter 1 Navigation End file)

In your own project, choose an action of “Execute a menu item” and apply one of the following types of navigation to your button:

View ➤ Page Navigation ➤

*   First Page

*   Previous Page

*   Next Page

*   Last Page

*   Go to Page

*   Previous View

*   Next View

As shown in Figure [4-30](#449379_2_En_4_Chapter.xhtml#Fig30), other types of viewing include the following:

*   Page Display

*   Zoom

*   Portfolio

*   Show/Hide of Navigation Panes

*   Full Screen Mode

The Next Page button and the right-pointing arrow icon button in the newsletter document have had the action View ➤ Page Navigation ➤ Next Page applied. The Previous Page button and the left-pointing arrow button have had the action View ➤ Page Navigation ➤ Previous Page applied. Refer to Figure [4-32](#449379_2_En_4_Chapter.xhtml#Fig32).

To navigate to a file in the same folder or alternatively an attachment within the PDF document, see Figure [4-33](#449379_2_En_4_Chapter.xhtml#Fig33) with the Go to Form button on page 4 and view the actions applied.

![](images/449379_2_En_4_Chapter/449379_2_En_4_Fig33_HTML.png)

3 screenshots. Page 1 to specify open preference has 3 options, of which user preference is selected. Page 2 for button properties has a trigger of mouse up, and an action as open a file. Page 3 for button properties has a trigger as mouse up, and action to execute a menu item.

Figure 4-33

Navigate to a file somewhere on your hard drive or within the PDF file in the Attachments pane. Refer to page 4 of the Newsletter 1 Navigation End PDF

With the Go to Form button created, you could do one of the following select actions to open the form:

*   Open a file.

*   Execute View ➤ Show/Hide ➤ Navigation Pane ➤ Attachments.

As mentioned earlier, similar navigation can be a hyperlink or even a bookmark.

In the Newsletter 1 Navigation End PDF, you can alter the URL link on page 1 with the text “New finds in China.” This link can go to a website, or if the text is highlighted, it can be added as a new bookmark that will anchor or jump to that page when clicked. However, if your bookmark name is no longer the same as the section title because you altered the link on the Properties Actions tab, you can rename the bookmark, as you can see in Figure [4-34](#449379_2_En_4_Chapter.xhtml#Fig34), by double-clicking it. The bookmark has now been renamed and matches the URL or section it is jumping to.

![](images/449379_2_En_4_Chapter/449379_2_En_4_Fig34_HTML.png)

A screenshot of the bookmark page has 2 bookmarks for Chapter 1 and new finds in China and buyers in Canada selected. It has a dialog box on the right side of the page with multiple options and the properties option selected.

Figure 4-34

A bookmark can be used as a link to a section within a book or to navigate to a URL

## Form Navigation with a Button as a Helpful Hint

If a client is unsure what to enter in a field, a hint can be created to give information. Open the TMC Order Form “End” PDF example to see how this is done.

This example uses “Show/hide a field” to accomplish this. Refer to the button next to the text Customer Order Code and to Figure [4-35](#449379_2_En_4_Chapter.xhtml#Fig35).

![](images/449379_2_En_4_Chapter/449379_2_En_4_Fig35_HTML.png)

A screenshot of a page has a box to enter the customer order code, followed by a question mark icon and a close button on the right side of the page. It has a screenshot of the button properties with the actions tab selected. It has the trigger as mouse up and action as show or hide a field.

Figure 4-35

Show and hide example with the Actions tab

Here is a breakdown of each of the buttons:

![](images/449379_2_En_4_Chapter/449379_2_En_4_Fig36_HTML.jpg)

A screenshot of a page has a button with a question mark at the top of the page. It has 3 dialog boxes for the show or hide field with multiple options. Box 1 has an info window selected, box 2 has close, and box 3 has an info option selected.

Figure 4-36

The Info button is visible at first glance while other buttons are set to hide

*   **Info button**: Show-Close, Show-Info Window, Hide-Info (Figure [4-36](#449379_2_En_4_Chapter.xhtml#Fig36)).

![](images/449379_2_En_4_Chapter/449379_2_En_4_Fig37_HTML.jpg)

A screenshot of 3 dialog boxes for the show or hide field has multiple options. Box 1 has the option of info selected. Box 2 has the info window option selected and box 3 has the option of close selected.

Figure 4-37

The Close button appears on top of the Info Window button

*   **Close button**: Hide-Close, Hide-Info Window, Show-Info (Figure [4-37](#449379_2_En_4_Chapter.xhtml#Fig37)).

![](images/449379_2_En_4_Chapter/449379_2_En_4_Fig38_HTML.jpg)

A screenshot has a question mark icon with a close button towards the top right part of the page. It has a line of text that reads the 9-digit code that was emailed to you.

Figure 4-38

The Info Window button appears below the Close button

*   **Info Window button**: No actions applied. Leave the Actions tab blank and the Close button visible on top (Figure [4-38](#449379_2_En_4_Chapter.xhtml#Fig38)).

## Adding a Comb of Characters

A *comb of characters* spreads the user-entered text evenly across the width of the text field. If a border color is specified on the Appearance tab, each character entered in the field is separated by lines of that color. This option is available only when no other check box is selected on the Options tab; refer to Figure [4-39](#449379_2_En_4_Chapter.xhtml#Fig39).

![](images/449379_2_En_4_Chapter/449379_2_En_4_Fig39_HTML.jpg)

A screenshot of a page has 2 boxes. Box 1 has text that reads 1, 2, 3, 4, 5, 6, a, b, and c. Box 2 has 9 blocks with entries 1, 2, 3, 4, 5, 6, a, b, and c within the blocks.

Figure 4-39

Without and with a comb of characters

Note

When entering a code, it can be helpful to add a comb of characters properties so that the client knows they have entered the correct number of letters or numbers. Just remember it is one field, so you cannot tab between parts of the comb.

### Before Comb and After Comb

Figure [4-40](#449379_2_En_4_Chapter.xhtml#Fig40) shows how a text field on the Options tab appears before you apply the comb of characters. When you uncheck all other options, only then will the comb of characters be available to alter how many characters will be in it. you can specify a number between 1 and 999.

![](images/449379_2_En_4_Chapter/449379_2_En_4_Fig40_HTML.png)

A screenshot of the text field properties has the options tab selected, with alignment as left and a few checkboxes, of which spelling check and scroll long text box are selected. It has a few check boxes at the bottom of the page with the option of the combination selected as 9 characters.

Figure 4-40

Adding the comb of characters to the text field on the Options tab

So that the comb of characters displays correctly, go to the Appearance tab and add a border color, line thickness, and line style; refer to Figure [4-41](#449379_2_En_4_Chapter.xhtml#Fig41).

![](images/449379_2_En_4_Chapter/449379_2_En_4_Fig41_HTML.jpg)

A screenshot of the text field properties page has the appearance tab selected. It has details of the borders and colors, including border color, line thickness, fill color, and line style. It has details for the text, including font size, text color, and font.

Figure 4-41

Adjusting the appearance of the comb on the Appearance tab

You can also adjust the text options if you feel a certain font size and color are more visible; in my case, I just left it black. In Chapter [7](#449379_2_En_7_Chapter.xhtml) I will show you how you can use a comb of characters with the Format tab.

There are other ways to help clients when they are unsure what to enter into a field, and we will look at them in later chapters.

## More Action Triggers to Show and Hide

Check boxes and buttons can be used in combination to create fields that show or hide. Refer to the TMC Order Form End file to see the final example of what happens when you enable the check box, as shown in Figures [4-42](#449379_2_En_4_Chapter.xhtml#Fig42) and [4-43](#449379_2_En_4_Chapter.xhtml#Fig43).

![](images/449379_2_En_4_Chapter/449379_2_En_4_Fig43_HTML.jpg)

A screenshot of the check box properties page has 5 tabs, of which the tab for actions is selected. It has a drop-down menu for the trigger as mouse up and action as showing or hiding a field. It has a list of actions, followed by 4 buttons, of which the delete button is selected.

Figure 4-43

Show and hide a shipping address using a check box and the Actions tab

![](images/449379_2_En_4_Chapter/449379_2_En_4_Fig42_HTML.jpg)

A screenshot of a page has a checkbox for different shipping address marked as yes. It has a table with 7 rows for first name, last name, address, city, state or province, country, and zip code. It has a bright-colored button at the bottom right side of the page for hide and clear fields.

Figure 4-42

Show and hide a shipping address using a check box

The settings for the check box “Check Box7” actions are on mouse up.

For each of the Show/Hide Text and Button fields, show each field one at a time (Figure [4-44](#449379_2_En_4_Chapter.xhtml#Fig44)).

![](images/449379_2_En_4_Chapter/449379_2_En_4_Fig44_HTML.jpg)

A screenshot of a page has an unchecked checkbox for different shipping address. It has a dialog box for the show or hide field with a list of fields, of which the option for first name underscore 2 is selected, followed by an ok button.

Figure 4-44

When the check box is clicked, some fields in the form will show that they were hidden

Since you must set the show fields one at a time, refer to the following list to see which field should have the Show setting:

*   First Name_2

*   Last Name_2

*   Address_2

*   City_2

*   Province_2

*   Country_2

*   Postal Code_2

*   Hide and Clear Fields Button

### Hide and Clear Fields Button

This button uses a combination of Reset and Show/Hide. If the shipping address is the same, you don’t want to store duplicate data, so you clear the fields that were shown when the check box was checked and hide them again along with the button. Refer to Figure [4-45](#449379_2_En_4_Chapter.xhtml#Fig45).

![](images/449379_2_En_4_Chapter/449379_2_En_4_Fig45_HTML.jpg)

A screenshot of a bright-colored button with text, hide, and clear fields.

Figure 4-45

Reset button

Figure [4-46](#449379_2_En_4_Chapter.xhtml#Fig46) shows the settings for the button actions.

![](images/449379_2_En_4_Chapter/449379_2_En_4_Fig46_HTML.jpg)

A screenshot of the show or hide field page has a list of fields with checkboxes for show and hide on the right side of the page. The hide option is selected, and the option of first name underscore 2 is selected from the fields list. It has an ok button at the bottom part of the page.

Figure 4-46

When the button is clicked, some fields in the form will hide that were shown

Since you must set the hide fields one at a time, refer to the following list to see which field should have the Hide setting:

*   First Name_2

*   Last Name_2

*   Address_2

*   City_2

*   Province_2

*   Country_2

*   Postal Code_2

*   Hide and Clear Fields Button

Finally, reset all the fields that were earlier made visible including “Check Box7” to the previous blank state. The only field you cannot reset is the button; you can only hide it. This reset action is moved up to the top of this list. Refer to Figure [4-47](#449379_2_En_4_Chapter.xhtml#Fig47) and Figure [4-48](#449379_2_En_4_Chapter.xhtml#Fig48).

![](images/449379_2_En_4_Chapter/449379_2_En_4_Fig48_HTML.jpg)

A screenshot of a form reset page has a scroll-down bar for the fields list, of which province 2, postal code 2, last name 2, and first name 2 are selected. It has an ok button at the bottom part of the page.

Figure 4-48

Reset for select fields using the dialog box including the check box using the Reset a Form dialog box

![](images/449379_2_En_4_Chapter/449379_2_En_4_Fig47_HTML.jpg)

A screenshot of the button properties page has 5 tabs, of which the actions tab is selected. It has a drop-down menu to select a trigger as mouse up and action as reset a form. It has a box for actions with a list of actions, and reset a form option is selected.

Figure 4-47

Actions tab for the Reset button that clears the information in the fields and hides them again

## Set Layer Visibility

As mentioned earlier, adding and setting layer visibility in a document can be tricky. One of the examples in this chapter is a file of a floor plan layout, showing how this can be achieved if you have a document with one or more layers. You can use either bookmarks or buttons to show and hide your layers. Refer to Figure [4-49](#449379_2_En_4_Chapter.xhtml#Fig49).

![](images/449379_2_En_4_Chapter/449379_2_En_4_Fig49_HTML.png)

A screenshot of a page has a layers icon with an expanded view for the floor plan layout. It has 4 check boxes for vendor numbers, seating layout, title, and guides and grids, of which the title option is selected.

Figure 4-49

The layers in the floor plan PDF

Note

In the properties of each layer, I have set only the default mode of the title layer to be on when the document opens; the others are set to Off. Remember that to access the properties of a layer, you need to right-click the layer and choose Properties.

You can find more details about adding and working with layers at [`https://helpx.adobe.com/acrobat/using/pdf-layers.html`](https://helpx.adobe.com/acrobat/using/pdf-layers.html).

### Using Bookmarks

If you don’t want to use buttons, you can alternatively use bookmarks to show and hide layers. See Figure [4-50](#449379_2_En_4_Chapter.xhtml#Fig50).

![](images/449379_2_En_4_Chapter/449379_2_En_4_Fig50_HTML.jpg)

A screenshot of the bookmarks page has multiple options. It has 3 bookmarks for show floor plans, vendor numbers, and hide layers, of which the floor plan bookmark is selected. It has a box with multiple options, of which properties option is selected.

Figure 4-50

Setting Bookmark properties

Begin by making sure that the layers you want visible or hidden are on or off to create the correct view for your bookmark.

Once you’ve created a bookmark, you can right-click it and choose Properties from the menu. The properties will appear. Then select the Actions tab, as shown in Figure [4-51](#449379_2_En_4_Chapter.xhtml#Fig51).

![](images/449379_2_En_4_Chapter/449379_2_En_4_Fig51_HTML.jpg)

A screenshot of the bookmark properties page has 2 tabs for appearance and actions, of which the actions tab is selected. It has a drop-down menu to select an action as set layer visibility, followed by an add button. It has a box for actions, followed by 2 buttons for edit and delete.

Figure 4-51

Bookmark action properties

Notice that the action “Go to a page in this document” is already added. However, you will need to add the action of “Set layer visibility.” Upon clicking Add, an info warning will appear reminding you that whatever the current state the layers are in now will be what this bookmark will display. If you do not like your layer visibility, click Cancel and set your layers to the correct state before applying the action and choosing OK. Refer to Figure [4-52](#449379_2_En_4_Chapter.xhtml#Fig52).

![](images/449379_2_En_4_Chapter/449379_2_En_4_Fig52_HTML.jpg)

A screenshot of a page has a warning message from Adobe Acrobat that reads, the target layer states of the selected actions will be set to the current state, followed by a ok and a cancel button. It has a box for actions at the bottom of the page with an action to set layer visibility.

Figure 4-52

Bookmark action warning properties for setting layer visibility on the Actions tab

Now your bookmark should be able to show or hide the layer based on the set state. As with any action, it can be selected and then removed by clicking the Delete button on the Actions tab. You can repeat this action with the other bookmarks as well.

### Using Buttons

As with bookmarks, buttons can be used as well. Refer to Figure [4-53](#449379_2_En_4_Chapter.xhtml#Fig53). However, you may want to hide some buttons and layers to reduce clutter while other layers are visible. Also, you may want to reset all the layers to the off or hidden state with a button. You can review the file to see what the final effect looks like. The following list shows what settings were applied to each button on the Mouse Up trigger. As with the bookmarks, make sure that the layers are in the state you want before you set the layer visibility for each button. Make sure to test your file when you’re done.

![](images/449379_2_En_4_Chapter/449379_2_En_4_Fig53_HTML.jpg)

A screenshot of a page has 3 buttons to show the floor plan, vendor numbers, and hide layers, of which the first button to show the floor plan is selected. It has a list of actions, and the mouse-up action is selected.

Figure 4-53

Example of settings applied to the buttons

![](images/449379_2_En_4_Chapter/449379_2_En_4_Fig54_HTML.jpg)

A screenshot of a page has an alert message from Adobe Acrobat that reads, the target layer of the selected actions will be set to the current state, followed by 2 buttons for ok and cancel, of which the ok button is selected.

Figure 4-54

This is the info alert that appears as the layer visibility is set

*   **Show Floor Plan button**: Set layer visibility. You have the options Show – Show Vendor Number and Show – Hide Layers Button.

*   **Show Vendor Numbers button**: Set layer visibility.

*   **Hide Layer button**: Set layer visibility. You have the options Hide – Show Vendor Number and Hide – Hide Layers Button.

*   In the initial state, the Show Floor Plan button on the General tab is set to Form Field Visible, while the other two buttons are set to Hidden.

*   In the Layers pane, only the title layer is set to on; all other layers are turned off.

*   Only turn on layers that you want visible before adding the action of layer visibility to each button. Click OK to confirm setting. Refer to Figure [4-54](#449379_2_En_4_Chapter.xhtml#Fig54).

Later you can use the up and down buttons in the Button properties dialog box if you want to reorder the actions, placing the Set Layer visibility as the last action. You can see my order in the file `Floor_planLayout_2023_End.pdf`.

In your own projects, once you are finished setting the buttons, return the layers to your default settings, save the file, and close it.

## Summary

This chapter covered a lot of topics concerning actions that can be applied to form fields and nonform items such as layers, bookmarks, buttons, pages, and hyperlinks. As you can see, Acrobat Pro offers a lot of options regarding navigation and controlling how you view or hide fields.

At this point, you haven’t added any custom JavaScript. You’ve only used the settings that come with the Acrobat Actions tab. However, in the next part of the book, you are going to look at running your own JavaScript and how this affects what you input into your form fields.

Before you go any further in this book, take some time to practice these actions on your own and try adding them to a few of your own projects.

# Part IISimplified Field Notation and Basic JavaScript

# 5. Introduction to Simplified Field Notation and JavaScript

**Chapter Goal:** In this chapter and the following chapter in Part II, you will first explore simplified field notation (SFN) used in Excel and how it compares to JavaScript. This will set the groundwork for what you can expect in the rest of the book.

This chapter will begin by identifying where in Acrobat Pro form fields you can enter SFN and JavaScript.

The JavaScript that you will be using in the following chapters is only for the form or navigation within a document and is not as advanced as a programmer’s JavaScript used in software or the Web. That is a whole other topic not discussed in this book. Mostly you will focus on simple math formulas that can be used to improve your forms in Acrobat Pro, as well as ones that clients will use in Reader.

Note

This chapter does not reference any files, so there are none that need to be downloaded for this lesson.

## Getting Started

From the text files `(.txt)` provided in the following lessons, you will copy and paste the code into the correct dialog box areas and make minor modifications to the code.

The following are some common uses for JavaScript in Acrobat Pro:

*   Interaction and addition of watermarks, bookmarks, links, and annotations

*   Automatically filling in form fields

*   Changing the appearance of information in text fields as the information itself changes because of user input in other fields

*   Navigation of the document, such as zooming in and going to specific pages

*   Printing of the document

*   Controlling security settings, signatures, and custom validation

*   Assisting the Action Wizard to speed up tasks

The JavaScript entry can be found in the following fields’ properties tabs and tools:

*   **Text, dates, and drop-down lists**: Actions (Run a JavaScript), Format Custom (Custom Format Script and Custom Keystroke Script), Validate (Run a Custom Validation Script), Calculate (Custom calculation script).

*   **Check boxes, radio buttons, image fields, and buttons**: Actions.

*   **List boxes**: Actions and Selection Change (Execute this script).

*   **Digital signatures**: Actions and Signed (This script executes when field is signed).

*   **Barcodes**: Actions and Value (Custom calculation script).

*   **JavaScript tool**: All JavaScripts (Global), Document JavaScripts, and Document Actions.

*   **Action Wizard**: New Action and New Custom Command subtools will be looked at in Chapter [15](#449379_2_En_15_Chapter.xhtml).

*   Remember the Actions tab is also available for Pages, bookmarks, and hyperlinks, but we will be focusing mainly on the form fields and buttons throughout the remainder of this book.

## Text Field, Date, and Drop-Down List Properties

The following sections provide examples of what JavaScript added to these field tabs looks like.

### Actions Tab

Actions happen upon entering or exiting the field, as shown in Figure [5-1](#449379_2_En_5_Chapter.xhtml#Fig1).

![](images/449379_2_En_5_Chapter/449379_2_En_5_Fig1_HTML.jpg)

A screenshot of the text field properties dialog box that lists the options under actions with the add an action that includes the select trigger and action, and the actions.

Figure 5-1

The Actions tab in the Text Field Properties dialog box

### Format Tab

A custom format allows changes to text color, as shown in Figure [5-2](#449379_2_En_5_Chapter.xhtml#Fig2). A custom keystroke allows events to change within the field when a certain criterion has been reached.

![](images/449379_2_En_5_Chapter/449379_2_En_5_Fig2_HTML.jpg)

A screenshot of the text field properties dialog box that lists the options under format with select format category as custom, custom format script, and custom keystroke script.

Figure 5-2

The Format tab in the Text Field Properties dialog box

### Validate Tab

Similar to a custom format, on the Validate tab certain criteria are checked and must be met before you can proceed, as shown in Figure [5-3](#449379_2_En_5_Chapter.xhtml#Fig3).

![](images/449379_2_En_5_Chapter/449379_2_En_5_Fig3_HTML.jpg)

A screenshot of the text field properties dialog box that lists the options under validate with the three radio buttons for field value is not validated, the field value is in range, and a selected run custom validation script.

Figure 5-3

The Validate tab in the Text Field Properties dialog box

### Calculate Tab

The Calculate tab offers custom calculations that can’t be done with SFN or Value, as shown in Figure [5-4](#449379_2_En_5_Chapter.xhtml#Fig4). This is the only tab where SFN can be entered.

![](images/449379_2_En_5_Chapter/449379_2_En_5_Fig4_HTML.jpg)

A screenshot of the text field properties dialog box that lists the options under calculate with the four radio buttons for value is not calculated, the value of the field, simplified field notation, and a selected custom calculation script.

Figure 5-4

The Calculate tab in the Text Field Properties dialog box

## Check Box, Radio Button, Image Field, and Button Properties

Check boxes, radio buttons, and regular buttons that include image fields can be used as visible or hidden fields, if required, to initiate a calculation, as shown in Figure [5-5](#449379_2_En_5_Chapter.xhtml#Fig5).

![](images/449379_2_En_5_Chapter/449379_2_En_5_Fig5_HTML.jpg)

2 screenshots. On the left, a radio button properties dialog box lists the options under actions with add action and actions. On the right, the JavaScript editor dialog box with the create and edit JavaScripts field.

Figure 5-5

The Actions tab in the Radio Button Properties dialog box is the only tab where you can add actions. The same goes for check boxes and regular buttons

## List Box Properties

Besides the Actions tab, the Selection Change tab is an area for adding a script when you want to change the field that is being selected. In this example, an item selected from a List Box references another field elsewhere in the form. Refer to Figure [5-6](#449379_2_En_5_Chapter.xhtml#Fig6).

![](images/449379_2_En_5_Chapter/449379_2_En_5_Fig6_HTML.jpg)

2 screenshots of the list box properties dialog box. On the top, it lists the options under actions with the add an action and action tabs. On the bottom, it lists the options under the selection change with the radio buttons of do nothing and a selected execute the script options.

Figure 5-6

The Actions and Selection Change tabs

## Digital Signature Properties

You can add actions to a digital signature’s properties either on the Actions or Signed tab. The Signed tab is like the Validate tab in other fields. Validation occurs when some criteria are met, and then the script executes. Refer to Figure [5-7](#449379_2_En_5_Chapter.xhtml#Fig7).

![](images/449379_2_En_5_Chapter/449379_2_En_5_Fig7_HTML.png)

2 screenshots of the digital signature properties dialog box. On the top, it lists the options under actions with the add an action and action tabs. On the bottom, it lists the options under the signed with the highlighted option of this script that executes when the field is signed.

Figure 5-7

The Actions and Signed tabs in the Digital Signature Properties dialog box

## Barcode Properties

As with other fields, you can add actions to barcodes. However, the barcode creates its own value script on the Value tab. Its value depends on what is in the form as a whole in regard to types of fields and their input values. Refer to Figure [5-8](#449379_2_En_5_Chapter.xhtml#Fig8).

![](images/449379_2_En_5_Chapter/449379_2_En_5_Fig8_HTML.png)

2 screenshots of the barcode properties dialog box. On the top, it lists the options under actions with the add an action and action tabs. On the bottom, it lists the options under the value with the highlighted option of the custom calculation script.

Figure 5-8

The Actions and Value tabs in the Barcode Field Properties dialog box

Regardless of what type of field you plan to use, all actions and their edits are accessed through the JavaScript Editor box, which holds the script. You can access this editing area when you click the Edit or Add button. The Add button is generally on the Actions tab, as in Figure [5-9](#449379_2_En_5_Chapter.xhtml#Fig9). The Edit button can also be found on the Actions tab but is on some other tabs as well such as Format, Validate, and Calculate; they all lead to the JavaScript Editor.

![](images/449379_2_En_5_Chapter/449379_2_En_5_Fig9_HTML.jpg)

2 screenshots. On the left, a radio button properties dialog box lists the options under actions with add action and actions. On the right, the JavaScript editor dialog box with the create and edit JavaScripts field.

Figure 5-9

Access to the JavaScript Editor through the Edit or Add button, found on the Actions, Format, Validate, and Calculate tabs

## Global Document JavaScript

Now let’s look at the global document JavaScript. If you have a document open, go to the Tools tab and select the JavaScript tool found in the Customize section. You will now be able to view all the subtools, as shown in Figure [5-10](#449379_2_En_5_Chapter.xhtml#Fig10).

![](images/449379_2_En_5_Chapter/449379_2_En_5_Fig10_HTML.jpg)

A screenshot presents the JavaScript icon with an add button on top. It includes a toolbar at the bottom with the debug, debugger, document JavaScripts, all JavaScripts, and document actions tools.

Figure 5-10

JavaScript tool and its submenu items

Actions that happen globally can affect more than one field. This could include formatting such as a color change to the field or the text being entered into fields. These types of actions will affect many fields throughout the file that access the Document JavaScripts dialog box in one of the field properties tab locations mentioned in this chapter. Fields can call upon a single global function to reduce typing and the size of the document, as shown in Figure [5-11](#449379_2_En_5_Chapter.xhtml#Fig11). You will look at this area in more detail in later chapters.

![](images/449379_2_En_5_Chapter/449379_2_En_5_Fig11_HTML.png)

A screenshot presents the document JavaScript dialog box with the script name and set the select values field, with a selected close button.

Figure 5-11

Document JavaScripts dialog box

Additional actions that are specific to the document can be added using the Document Actions tool, as shown in Figure [5-12](#449379_2_En_5_Chapter.xhtml#Fig12).

![](images/449379_2_En_5_Chapter/449379_2_En_5_Fig12_HTML.jpg)

A screenshot of the document actions dialog box, which has the close, save, and print options of the document, execute this JavaScript field, and a selected O K button.

Figure 5-12

Document Actions dialog box

Alerts can be placed here when these following events happen, which we will look at in Chapter [8](#449379_2_En_8_Chapter.xhtml).

Note

You can’t add more document actions here without advanced scripting knowledge.

You can also view all your JavaScripts at once under the All JavaScripts subtool, which opens the JavaScript Editor. Refer to Figure [5-10](#449379_2_En_5_Chapter.xhtml#Fig10).

If there are errors, you can use Debug and Debugger subtools to help you. However, I find it helps to have some knowledge of JavaScript structure and do the editing in a program such as Notepad++ or Adobe Dreamweaver where there is more room to type. Editing programs like Dreamweaver also show you clearly which line in the code you are on with numbers on the left. Acrobat does not have these visual cues, only a text reference in the lower right of the editor, which I do not find useful for detailed code editing. Refer to Figure [5-13](#449379_2_En_5_Chapter.xhtml#Fig13) and Figure [5-15](#449379_2_En_5_Chapter.xhtml#Fig15).

![](images/449379_2_En_5_Chapter/449379_2_En_5_Fig13_HTML.png)

A screenshot of the JavaScript debugger dialog box, which includes the script of the document with the inspect and view options. The inspect and view drop-down menu lists the tools with selected global variables and scripts.

Figure 5-13

JavaScript Debugger dialog box with settings for viewing and inspection

Note

if you find that your Debugger is not operating, you can check and reset this by selecting Edit ➤ Preferences ➤ JavaScript and selecting Enable JavaScript Debugger after Acrobat is Restarted. As well as the settings of “Enable interactive console” and “Show console on error and messages,” by default, these settings are disabled. Refer to Figure [5-14](#449379_2_En_5_Chapter.xhtml#Fig14) and click OK if you choose to activate these options. Then close the Acrobat Pro Application and then open again. Make sure you have exited the JavaScript tool for the file before you close the application, or the dialog box may open suddenly before the document itself opens. Otherwise, click Cancel to exit and leave the debugger disabled, which may be your preference if you are working with lots of documents that do not contain JavaScript. Refer to Figure [5-14](#449379_2_En_5_Chapter.xhtml#Fig14) to see the preference settings. In my case, I enabled these preference settings before I edited my forms.

![](images/449379_2_En_5_Chapter/449379_2_En_5_Fig14_HTML.jpg)

2 screenshots of the JavaScript debugger dialog box. On the top, it has the disabled interactive console and the console on errors and messages. On the bottom, it has the enabled JavaScript debugger, interactive console, and the console on errors and messages.

Figure 5-14

JavaScript Debugger preferences with settings to enable the Debugger dialog box

Though we will not be using Debugger in this book, the following link can give you some further guidance on the limitations of using Debugger as well as other JavaScript preference settings:

[`https://opensource.adobe.com/dc-acrobat-sdk-docs/library/jsdevguide/JS_Dev_Tools.html#using-the-javascript-debugger-console`](https://opensource.adobe.com/dc-acrobat-sdk-docs/library/jsdevguide/JS_Dev_Tools.html#using-the-javascript-debugger-console)

`W`hen you’ve finished typing in your editing program, copy and paste the code into Acrobat’s JavaScript Editor and run the debugger and test it (Figure [5-15](#449379_2_En_5_Chapter.xhtml#Fig15)).

![](images/449379_2_En_5_Chapter/449379_2_En_5_Fig15_HTML.jpg)

A screenshot presents the notepad on top with the script of the JavaScript document and codes, while the bottom has a JavaScript editor dialog box with the create and edit JavaScript field.

Figure 5-15

How JavaScript appears in an application like Dreamweaver or Notepad++ compared to how the code appears in Acrobat Pro JavaScript Editor (no numbered lines)

Note

The Edit ➤ Preferences ➤ JavaScript area will also allow you to alter your Acrobat JavaScript Editor’s display of its font type and its font size. You can also link to a different external editor if required. However, for this book, select the Use Acrobat JavaScript Editor option, as shown in Figure [5-16](#449379_2_En_5_Chapter.xhtml#Fig16).

![](images/449379_2_En_5_Chapter/449379_2_En_5_Fig16_HTML.jpg)

A screenshot of the JavaScript editor dialog box with the enabled Acrobat JavaScript editor and the disabled Eternal JavaScript editor.

Figure 5-16

JavaScript Editor settings

### Web Links and References

For the full library, refer to version Acrobat SDK 2021:

[`https://opensource.adobe.com/dc-acrobat-sdk-docs/acrobatsdk/index.html`](https://opensource.adobe.com/dc-acrobat-sdk-docs/acrobatsdk/index.html)

`C`urrent known support of JavaScript is called SDK 2021, and you can review the latest JavaScript API Reference Guide online which is currently 1.7.

[`https://opensource.adobe.com/dc-acrobat-sdk-docs/library/jsapiref/index.html`](https://opensource.adobe.com/dc-acrobat-sdk-docs/library/jsapiref/index.html)

`O`utside of Acrobat, you can use the following resources to learn more about JavaScript in general:

[`https://www.w3schools.com/js/`](https://www.w3schools.com/js/)

[`https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide)

`H`owever, keep in mind that the JavaScript used in Acrobat is limited to the application and in this case to forms, so certain declarations and scopes will fail to work. Building a PDF form is not the same as a website, which has more complex interactions. Nevertheless, I do find these links can add clarification when I compare them to the documentation found in the Adobe SDK Resources.

### Regular Forms vs. E-Signature Forms

The e-signature form tools are more recent additions to Acrobat Pro and contain digital signature fields for the Adobe Sign for eventual web form creation. While they look similar, they do not operate the same as regular electronic form fields and do not allow for the addition of JavaScript. For those reasons, we will not be looking at them in-depth until Chapter [20](#449379_2_En_20_Chapter.xhtml) where I will be giving a brief overview as well as resources for working with these fields and newer form-related Acrobat tools. For now, refer to Figure [5-17](#449379_2_En_5_Chapter.xhtml#Fig17).

![](images/449379_2_En_5_Chapter/449379_2_En_5_Fig17_HTML.png)

A screenshot presents the regular and E-signature form fields and editing tools. Each has several tool icons. Some of them are selection, text field, checker box, radio button, and pushpin.

Figure 5-17

Two types of form field tools in Acrobat Pro

You can access the e-signature fields shown in Figure [5-17](#449379_2_En_5_Chapter.xhtml#Fig17) if you open your Prepare Form tool by checking “This Document requires signatures” (Figure [5-18](#449379_2_En_5_Chapter.xhtml#Fig18)) and then clicking Start. For your forms in this book, this option should always remain unchecked; check this option only for Chapter [20](#449379_2_En_20_Chapter.xhtml).

![](images/449379_2_En_5_Chapter/449379_2_En_5_Fig18_HTML.jpg)

A screenshot depicts the page, which has a preview of the single file titled T M C customer survey with a highlighted start button.

Figure 5-18

How to create e-signature forms. For the forms you are working with, keep this unchecked

## JavaScript and Acrobat on the Document Level

In Acrobat Pro, to have a resulting event or action occur in a field in your form, you need to follow this equation:

Trigger + Select Action = Result

In addition to knowing what’s being triggered, you need to know where the results are going to end up. In the case of forms, data, or if a value is going to be taken from a field (text, drop-down, check box, radio button, etc.), the collection of that data is going to be deposited into a final field as the result you want. Whether using a value, simplified field notation, or JavaScript, it’s important to know the exact name of those fields. If you do not know the exact name, you will not get a result, and the form will be useless. To avoid confusion with fields, each must have a unique name.

The exception with naming is radio button groups, which act collectively; however, their groups must have distinct names. With all other fields, there can’t be two or more named Sum1 within a form for them to operate independently; it should be Sum1 and Sum2 and so on.

Follow along with the form examples provided to complete the lessons in the next chapters. The PDFs with the name “End” are the final result.

## Summary

JavaScript can be added in a variety of areas; it all depends on what type of form field you plan to use or if you plan to add the JavaScript to the entire document. In the next chapter, you will see how a knowledge of SFN and JavaScript can improve forms that you use for creating calculations.

# 6. Basic and Complex Calculations

**Chapter Goal:** In the first example of JavaScript calculations in this chapter, we will compare three methods, value, Simplified Field Notation (SFN), and JavaScript, as found on the Calculate tab in the Text and Dropdown List form fields, as shown in Figure [6-1](#449379_2_En_6_Chapter.xhtml#Fig1).

![](images/449379_2_En_6_Chapter/449379_2_En_6_Fig1_HTML.jpg)

A screenshot of a table with 4 columns, for example, object 1, object 2, and total. It has 7 rows for sum, product, subtract, divide, average, minimum, and maximum.

Figure 6-1

Inside the Acrobat Calculate Tab Examples PDF file

While value is generally the most straightforward way of dealing with calculations for beginner form creators, you will soon discover that there are limitations to value, so SFN and JavaScript might be better options. This chapter will show the strengths and weaknesses of each method.

Note

If you want to work along in this lesson or review the final result, download the Chapter [6](#449379_2_En_6_Chapter.xhtml) files from the link in the introduction. The file with the label “Start” is the file without the code, and the file with the label “End” is the final result. You will also find folders with original MS Word, Excel, and PDF files if you would like to edit them, as well as a folder containing the original scripts if you would like to add them to your own PDF forms.

If you are creating your form from an original PDF, refer to the “Forms Review” section in Chapter [1](#449379_2_En_1_Chapter.xhtml).

Remember that to view the properties of a field, you must select the Prepare Form tool; only then can you right-click or double-click a field to review its properties.

## Getting Started

Let’s take the addition or sum example, which is common to all methods regardless of which method or option you choose on the Calculate tab. Refer to the Acrobat Calculate Tab Examples PDF “Start” and “End” files in the folder for this chapter. Refer to Figure [6-2](#449379_2_En_6_Chapter.xhtml#Fig2).

![](images/449379_2_En_6_Chapter/449379_2_En_6_Fig2_HTML.jpg)

A screenshot of the text field properties page has the calculate tab selected, with a check box for the value not calculated. A second screenshot of the format tab has the category as the number, and details of decimal places, separator and negative number style, currency symbol, and location.

Figure 6-2

The Calculate tab is common to all three methods we will discuss shortly, and with numbers you use the Format tab with a “Select format category” setting of Number

Note

If no value has yet been applied to a field on the Calculate tab, the option “Value is not calculated” will be selected. This is OK for text fields or even fields that will contain numbers but are not going to create the final calculation. In this case, the fields are formatted to Number with a decimal place of 0 on the Format tab, but you can set this to 1, 2, or higher if you do not want to round up the number. We will look at this tab more in Chapter [7](#449379_2_En_7_Chapter.xhtml). Refer to Figure [6-2](#449379_2_En_6_Chapter.xhtml#Fig2).

## Sum Value

In this example, I set the calculate properties in the field TotalSum, which is set to “Read-only” on the General tab, to prevent users from typing calculations into this field.

On the Calculate tab, select the “Value is the…” button and pick sum (+) from the drop-down list. Notice in the “End” file there are field names of some of the fields in the form in the gray area (Figure [6-3](#449379_2_En_6_Chapter.xhtml#Fig3)).

![](images/449379_2_En_6_Chapter/449379_2_En_6_Fig3_HTML.jpg)

A screenshot of a checkbox to calculate the value as the sum of the following fields. It has a text box with text that reads, object 1 sum and object 2 sum, followed by a pick button on the right side of the box.

Figure 6-3

Entering the sum value into the Calculate tab

In this example, I then clicked the Pick button and selected from the Field Selection dialog box the form fields I wanted to draw my calculations from, in this case Object 1Sum and Object 2Sum. Then I clicked OK to add them to the list. Refer to Figure [6-4](#449379_2_En_6_Chapter.xhtml#Fig4).

![](images/449379_2_En_6_Chapter/449379_2_En_6_Fig4_HTML.jpg)

A screenshot of the field selection has a scroll-down list to select fields for calculation, followed by an ok button. It has 2 tables at the bottom of the page, with 4 columns for example, object 1, object 2, and total. The entries of columns 1 to 3 of table 1 are selected.

Figure 6-4

Entering the sum value on the Calculate tab from the Field Selection dialog box

In this example and Figure [6-4](#449379_2_En_6_Chapter.xhtml#Fig4), you are getting data or values from two fields: Object 1Sum and Object 2Sum. They add together in the field TotalSum to offer a result.

This works really well for other types of basic calculations from the list such as product (x) multiplication, the average of numbers, and the minimum and maximum of numbers. However, other basic value calculations such as subtraction or division are just not possible using this method. Refer to Figure [6-5](#449379_2_En_6_Chapter.xhtml#Fig5).

![](images/449379_2_En_6_Chapter/449379_2_En_6_Fig5_HTML.jpg)

A screenshot of 2 checkboxes, of which checkbox 1 for the value is selected. It has a list of options, of which the sum option is selected.

Figure 6-5

Field value options are limited by what is available in the list

After you click the Close button in the Properties panel, you can then test your form fields in preview mode. Then return to edit mode if you want to review other fields.

To perform additional calculations, we need to look at the next option.

## Simplified Field Notation

Now let’s look at Simplified Field Notation. Notice on the Calculate tab in Figure [6-6](#449379_2_En_6_Chapter.xhtml#Fig6) that the button called Simplified Field Notation is selected.

These examples are found in the next set on page 1 of the Acrobat Calculate Tab Examples PDF file named with “End.”

![](images/449379_2_En_6_Chapter/449379_2_En_6_Fig6_HTML.jpg)

A screenshot of a page has the checkbox for simplified field notation selected. It has a dialog box for Java Script Editor with an equation to create and edit Java Scripts. There are 2 tables with 4 columns example, objects 1 and 2, and a total with entries of columns 2 to 4 of table 1 selected.

Figure 6-6

Entering the SFN code into the Calculate tab and JavaScript Editor and the result on the form

Note

With SFN and JavaScript you cannot pick or select your fields; you must type the information directly into the JavaScript Editor. Refer to Figures [6-6](#449379_2_En_6_Chapter.xhtml#Fig6) and [6-8](#449379_2_En_6_Chapter.xhtml#Fig8).

The second way of accomplishing this is with SFN, which is common to a program like Microsoft Excel. While this is similar to the value sum example, there are differences. In the previous sum example, you could have spaces between the words in the fields. However, Object 1Sum in SFN must be written as Object1Sum. There can be no breaks using SFN. This rule is very strict. Any break in the name of the field will cause it to not calculate. If you need a break, use an underscore (_), as in Object1Sum_2, or use what is known as the *camel* method where each new word is capitalized but with no breaks (ObjectSum).

In this example, for the field TotalSum_2, I am adding together the two fields Object1Sum_2 and Object2Sum_2\. I was able to enter this text into the JavaScript Editor when I clicked the Edit button in Figure [6-6](#449379_2_En_6_Chapter.xhtml#Fig6) and then clicked OK to exit.

Another difference with SFN is the addition of a plus symbol (+) to show that you are adding these two field values within the field TotalSum_2. Use an asterisk (*) to multiply, a minus (-) to subtract, and a slash (/) to divide. If you’ve ever taken algebra, you’ll be familiar with parentheses: (). Whatever occurs within parentheses happens first before the calculation continues. You can see an example of this in the fields TotalAverage_2 and TotalMoreComplex. Refer to Figure [6-7](#449379_2_En_6_Chapter.xhtml#Fig7).

![](images/449379_2_En_6_Chapter/449379_2_En_6_Fig7_HTML.jpg)

A screenshot of 2 selected checkboxes for simplified field notation, each with different complex equations for object 1 and object 2\. It has an edit button at the right side of each checkbox.

Figure 6-7

Entering the SFN into the Calculate tab for a more complex calculation

To type this code into the editor, you must first click the Edit button. When you’re done entering the code, click OK to exit.

Here you can see (Object1MoreComplex*2) + (Object2MoreComplex-7) will now calculate the final result in the TotalMoreComplex field. Then click Close to exit the Properties dialog box and click Preview to test your fields.

If you put numbers such as 5 and 10 into the fields Object1MoreComplex and Object2MoreComplex, this should be your answer in the field TotalMoreComplex:

*   (5*2) + (10-7) = 13

Note that division works best when you have already entered a number in one of the text fields.

However, as you may notice, calculations such as minimum or maximum are a bit more complicated to figure out using SFN, so I have left these fields without a calculation.

## JavaScript Custom Calculation Script

The final method we will look at is a custom calculation script, which is found in the third section on the Acrobat Calculate Tab Examples page. Figure [6-8](#449379_2_En_6_Chapter.xhtml#Fig8) shows the “Custom calculation script” with the Edit button selected for the field TotalSum_3.

![](images/449379_2_En_6_Chapter/449379_2_En_6_Fig8_HTML.jpg)

A screenshot of a checkbox for the custom calculation script with a few lines of code, followed by a dialog box for Java Script Editor with codes to create and edit Java Scripts. It has 2 tables with columns example, objects 1 and 2, and total, and entries of columns 2 to 4 of table 1 selected.

Figure 6-8

Entering the custom calculation script into the Calculate tab

To type this code into the editor, you must first click the Edit button. Then you can type. When done, click OK to exit. Afterward, you click the Close button on the Properties dialog box, and then you would click the Preview button and test the form.

In the case of JavaScript, you can again see that there are similarities and differences in the way the code is set up. To translate the fields into the language of JavaScript, you need to use variables to hold the fields. Variables act as information containers. In JavaScript, the word `variable` uses the short form `var`. After the word `var` you add a name.

In this example, to keep it simple, let’s use the name `a`. This variable called `a` will now hold whatever value comes into the text field Object1Sum_3\. Once a variable is named the first time, it does not need to have the word `var` attached and can be used anywhere in the script as just `a`. To ensure that the number input will follow through correctly, add `this.getField`, which means “get the data in this field,” which in the case is from the field Object1Sum_3\. Place the field name into parentheses, (), and straight quotes (`"")`. At the end of the variable, use a semicolon (;) to indicate that that is the end of this variable statement.

Use the same steps for the second variable, `b`, which uses the field Object2Sum_3\. They are the two fields that you want to use to calculate the final input for your current field, TotalSum_3.

```
var a = this.getField("Object1Sum_3");
var b = this.getField("Object2Sum_3");
```

This is the final part of the script:

```
event.value = a.value + b.value;
```

This is similar to the SFN example. However, you need to add `.value` on the end of each variable to remind Acrobat that you want the value of each field and the final value to be added in the final field. The final field, TotalSum_3, is represented by `event.value`. In this final field, the event of the addition taking place is the result. It is important to note that the final field does not need a variable name because of the way Acrobat contains the script within the final field. Again, you end this line with a semicolon.

Another variation on this same script could be, if you did not want to separate the variables, to combine them and write this:

```
event.value = this.getField("Object1Sum_3").value + this.getField("Object2Sum_3").value;
```

However, when more variables are involved or you are using the field more than once in a calculation, is this the best way of writing code? Or is it better to organize all your variables in one area to access if you need to change the name of a form field in the code quickly?

For example, which is easier to change?

```
event.value=((this.getField("Object1Sum_3").value+this.getField("Object2Sum_3").value))*((this.getField("Object1Sum_3").value -this.getField("Object2Sum_3").value));
```

Or instead write this:

```
var a = this.getField("Object1Sum_3");
var b = this.getField("Object2Sum_3");
event.value = (a.value + b.value)*(a.value - b.value);
```

Where the naming of variables is concerned, it’s OK to have the names longer than one letter, like `a`. You can give them more meaningful names like `age` or `sum`. In the final example, it would be written like this:

```
event.value = age.value + sum.value;
```

However, make sure to change your variables earlier in the code to `var age` and `var sum` so that the code will function.

The following are a few other things to remember about variables when working in Acrobat Pro:

*   All JavaScript variables must be identified with unique names.

*   Field Names can contain letters, digits, and underscores.

*   Variable names must begin with a letter though it is OK for the field you are referencing to start with a number.

*   Names are case sensitive (`y` and `Y` are different variables).

*   Reserved words (like JavaScript keywords) cannot be used as names. I would not create a variable like `var event` because `event` is a key JavaScript word used in the final event. For example, `event.value = age.value + event.value;` might not function since `event.value` has already been used once.

*   As mentioned, as with SFN, make sure to keep your variable and generally your field names as one word rather than separate words. However, so along as the field names are within the straight quote marks, it is OK to have a space.

*   If you are using modern JavaScript, be aware that you cannot replace the declaration var with let as you will run into an error. Use var only.

*   Caution is required when you switch between value, SFN, and the Custom Calculate Script button options. As you select the alternative option, you will erase the code you just entered, so always keep a backup in your JavaScript code in your `.txt` file as you work. In addition, do not save my “End” file if you accidentally erase the code; just close it without saving changes.

Note

Also keep in mind when you are writing your quotes that surround the field name that you use a program like Notepad++ or Dreamweaver to write the code if you need more room, as mentioned in Chapter [5](#449379_2_En_5_Chapter.xhtml). A program like MS Word may try to substitute the double quote with the quote marks that are used in a conversation, also known as *curly*, when you should be writing “neutral” or straight quotes. This can confuse the Acrobat JavaScript Editor, causing an error and not allowing you exit the dialog box.

At this point, if you need some basic knowledge about the Calculate tab, you can refer to this page:

[`https://helpx.adobe.com/acrobat/using/manage-form-field-properties.html`](https://helpx.adobe.com/acrobat/using/manage-form-field-properties.html)

## Final Thoughts on Working with the Calculate Tab

Let’s review. You have seen three ways in which addition can be done: value, SFN, and a JavaScript custom calculation script. However, which one is right for your project? Do you in all situations need to use Acrobat JavaScript?

In most simple calculation projects, I recommend using value or SFN in various parts of the form. However, if you have been reviewing the file Acrobat Calculate Tab Examples “End” examples, you will see Value and SFN do not equally cover all issues. For Value, you cannot subtract or divide. SFN cannot find minimum or maximum numbers. In this case, you may have to use a combination of both these calculations in your form.

Note

JavaScript can do all of these calculation as well as many others. Also, as you will see in Chapter [12](#449379_2_En_12_Chapter.xhtml), when the JavaScript becomes more complex, it’s not always a good idea to mix values and SFN with JavaScript as this can lead to some fields not responding correctly. Consistent coding methods are crucial for form calculations to run smoothly. See also the “Set Field Calculation Order” section later in this chapter.

For example, in the Field TotalMinimum_3, I used the following custom calculate script:

```
var q = this.getField("Object1Minimum_3");
var r = this.getField("Object2Minimum_3");
event.value=Math.min(q.value,r.value);
```

You can use the JavaScript math formula `Math.min` to get a minimum number similar to the value example. You could use `Math.max` to get the maximum number. There are other math formulas that are not available to us via value or SFN but are with JavaScript:

*   `Math.abs(x)`: Returns the absolute value of `x`.

*   `Math.acos(x)`: Returns the arccosine of `x`, in radians. However, you should enter a number between -1 and 1 to avoid throwing an error. Setting a restriction with the Validate tab may be something you will want to explore further later in this book.

*   `Math.asin(x)`: Returns the arcsine of `x`, in radians. However, you should enter a number between -1 and 1 to avoid throwing an error.

*   `Math.atan(x)`: Returns the arctangent of `x` as a numeric value between -PI/2 and PI/2 radians.

*   `Math.atan2(y,x)`: Returns the arctangent of the quotient of its arguments.

*   `Math.ceil(x)`: Returns `x`, rounded up to the nearest integer.

*   `Math.cos(x)`: Returns the cosine of `x` (`x` is in radians).

*   `Math.exp(x)`: Returns the value of `E`^(`x`), where `E` is Euler’s number.

*   `Math.floor(x)`: Returns `x`, rounded down to the nearest integer.

*   `Math.log(x)`: Returns the natural logarithm (base E) of `x`.

*   `Math.PI`: Gives the formula of ∏.

*   `Math.pow(x,y)`: Returns the value of `x` to the power of `y`.

*   `Math.random()`: Returns a random number between 0 and 1.

*   `Math.round(x)`: Rounds `x` to the nearest integer. Example: Round up a subtotal.

*   `Math.sin(x)`: Returns the sine of `x` (`x` is in radians).

*   `Math.sqrt(x)`: Returns the square root of `x`.

*   `Math.tan(x)`: Returns the tangent of an angle.

And there are many other Math objects you can view and test at the following pages:

```
https://www.w3schools.com/jsref/jsref_obj_math.asp
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math
```

In this case, `x` or `y` would be written as `(x.value)` or `(x.value,y.value)`.

## JavaScript Script Validation for Displaying Zero

One final thing that you could not do in this form without JavaScript is to format the number 0\. Sometimes on a form you do not want the 0 to be present if no calculation has been added to the first two fields; you just want it blank. Refer to Figure [6-9](#449379_2_En_6_Chapter.xhtml#Fig9).

![](images/449379_2_En_6_Chapter/449379_2_En_6_Fig9_HTML.jpg)

A screenshot of 2 tables with columns example, objects 1 and 2, and total. It has a screenshot of the text field properties page with the validate tab selected, followed by 3 checkboxes, of which the checkbox to run a custom validation script is selected with a few lines of code.

Figure 6-9

Adding a validation script into the Validate tab of the Total field. Before and after adding the script

This looks better. Look at the Validate tab of the field TotalSum_3 if you need a reference, and see Figure [6-9](#449379_2_En_6_Chapter.xhtml#Fig9).

Along with the Calculate tab script, you can enter the following into your Validate tab:

```
// Custom Validate script for text field
if (event.value == 0) event.value = "";
```

To start off this script in text field TotalSum_3, a comment explains what we’re doing.

A double slash (//) always starts off a comment. Comments can be written anywhere in JavaScript and will not affect the calculations. Consider them a place for helpful reminders.

This is another type of commenting you can use for longer, paragraph-sized comments:

```
/* This type of comment is for lots of text */
```

However, I generally prefer to use // because of the small space Acrobat provides.

### The Final Line of Code

This is what is known as a *conditional statement*, and you will look at them in detail later. Essentially it is saying, “If no value or only the zero number from the other two fields has been entered (both are blank or both contain zeros), then leave the final value field blank as well.”

```
if (event.value == 0) event.value = "";
```

**One final item:** If you used this script in subtraction or addition with negative numbers and you needed to see the 0 (example: 5-5=0), you might not see the 0\. If you need to see the 0 only when the fields are full, you can use a more complex and stricter validation.

== is not a strict equality operator comparison. A result like 5-5=0 might not work, and 0-0=0 or 0x5=0 might not work because the validation regards 0 and a blank field as the same thing.

=== is a strict equality operator comparison, so now a result like 5-5=0 or 0-0=0 or 0x5=0 will work because the validation regards 0 and a blank field as *not* the same thing and will put a 0 in the results field. You could write the following to keep your 0:

```
if (event.value === 0) event.value = "";
```

However, Acrobat seems to act quirky and not get the intended result where the final field is blank if one value field is added but you have not entered `a` value in the other. Refer to Figure [6-10](#449379_2_En_6_Chapter.xhtml#Fig10).

![](images/449379_2_En_6_Chapter/449379_2_En_6_Fig10_HTML.jpg)

A screenshot of 3 text boxes. Box 1 has a value of 0.00, box 2 has no entry, and box 3 has a value of 0.00.

Figure 6-10

Validation is not acting as it should in these fields

So, here’s the alternate code that you can view in my file `AcrobatCalculateTabExamples_End_option2validate.pdf`:

```
//Sum Validation to insure 0 is not missing from an actual calculation
if (a.value !== a.defaultValue && b.value !== b.defaultValue) {
event.value = a.value*1 + b.value*1 ;
} else {
event.value ="" ;
}
```

This code allows you to have the field blank if no numbers are in the other fields. However, if a Zero value is entered in both fields (&&), it will retain the 0 even if the final result is 0\. In this case, you used !== (strict not equal), which is similar to strict equality operator comparison, ===, but works in reverse and checks the current value against the default. If no value is found initially in either of the other value fields (the default value), the `event.value` will remain blank. The values in this example are multiplied once against themselves and then added together. Notice you used another conditional statement, if/else. Refer to Figure [6-11](#449379_2_En_6_Chapter.xhtml#Fig11).

![](images/449379_2_En_6_Chapter/449379_2_En_6_Fig11_HTML.jpg)

A screenshot of 2 tables with 4 columns, for example, object 1, object 2, and total, both with column 1 entry as a sum. Table 1 has the column 2 entry as 0, with the other columns blank. Table 2 has values 0 for columns 2 and 3 and column 4 as 0.00.

Figure 6-11

Validation is acting as it should in these fields when both fields have a value

For other equality and inequality operators and comparisons refer to the following pages:

[`https://www.w3schools.com/js/js_comparisons.asp`](https://www.w3schools.com/js/js_comparisons.asp)

[`https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators)

Note

In the subtraction example, go to the Validate tab, and change the following code in the PDF field called TotalSubtract_3:

`event.value = a.value*1 + b.value*1;`

to the following:

`event.value = f.value*1 - g.value*1;`

For the code to operate correctly, the Variable values on the Validate tab must match what is on the Calculate tab throughout the entire code, so refer to that field’s tab to compare. For example, in the code, the `a.value` variable must be replaced with `f.value`, and the `b.value` variable must be replaced with `g.value`. The same variable change is true for those variables ending an `a.defaultValue` or b`.defaultValue`.

When you are done viewing the forms, click the Close button in the upper-right corner of the preview to close the Prepare Form tool. Refer to the “Forms Review” section in Chapter [1](#449379_2_En_1_Chapter.xhtml) if you are unsure what this button looks like. Save your copy of the “Start” file only if you made edits.

### Drop-Down Alternatives

As in Chapter [2](#449379_2_En_2_Chapter.xhtml), for this example, you can replace some of the text fields with drop-down lists to calculate. While they’re similar to text fields, I find this method a good alternative if you want your client to use very specific values; it eliminates the need for validation on each drop-down because the values are already set. You should not replace the final text field (Read-Only) with a drop-down. Even though these Validate and Calculate tabs do work with drop-downs, the end result is really only one value. In Chapter [12](#449379_2_En_12_Chapter.xhtml), you will see how to use JavaScript to get more than one result into a drop-down list when required. Refer to Figure [6-12](#449379_2_En_6_Chapter.xhtml#Fig12).

![](images/449379_2_En_6_Chapter/449379_2_En_6_Fig12_HTML.jpg)

A screenshot of a table with 4 columns, for example, object 1, object 2, and total. It has 7 rows for sum, product, subtract, divide, average, minimum, and maximum. The column entries for object 1, object 2, and total are 0.

Figure 6-12

Using drop-down lists to fill in fields for calculations

Note

With drop-downs, I left the “Field value is not validated” field on the Validate tab blank because I was using specific numbers. Unlike text fields, there always is a value and never a blank value. Refer to `AcrobatCalculateTabExamples_End_option2DDown.pdf`, which contains a drop-down list in the file folder for this chapter.

### Set Field Calculation Order

Later, as you work with multiple fields that require calculation, you can use the Option on the right side of the Prepare Form tool to set the field calculation order from the More list. Refer to Figure [6-13](#449379_2_En_6_Chapter.xhtml#Fig13).

![](images/449379_2_En_6_Chapter/449379_2_En_6_Fig13_HTML.jpg)

A screenshot of a tool icon for more, followed by an icon to prepare the form for E-signing and a tab to set the field calculation order. It has a dialog box with a list of options for calculated fields, of which the total sum is selected.

Figure 6-13

Calculated Fields dialog box accessed from the More menu

As more fields contain calculations or are accessed by other fields, being able to set the order by moving a field up or down in the list is crucial if you notice that one of the calculations is not appearing in the order you want. This is also a good way to see if certain fields are missing that should be using the Calculate tab. Keep this in mind, along with Show Tab Number Order, as you build more complicated forms in the following chapters.

## Resetting Your Field of Focus

While working with calculations in a form or any set of form fields, there may be situations where you reset the fields. In addition, you may want the user of the form to, upon clicking the reset button, return to a specific field; oftentimes this is the first field in the form. This can be done using JavaScript. Rather than having all fields inactive and not knowing which field to start typing in, the cursor on reset will be placed in the active field, and the user can begin to type again.

We can see a basic example of this in the file `AcrobatResetField_Focus_End.pdf`. Refer to Figure [6-14](#449379_2_En_6_Chapter.xhtml#Fig14).

![](images/449379_2_En_6_Chapter/449379_2_En_6_Fig14_HTML.jpg)

A screenshot of the Acrobat rest field of focus has a reset button, followed by tabs to enter the first and the last name and a table for the calculation area. The table has 4 columns for the product number, quantity, price, and total, and 7 rows.

Figure 6-14

Reset your field of focus in a form

In this example, I could have used JavaScript to perform calculations; however, in this case I am just using value. This is not relevant to creating a field of focus; instead, refer to the Reset button properties on the Actions tab. Refer to Figure [6-15](#449379_2_En_6_Chapter.xhtml#Fig15).

![](images/449379_2_En_6_Chapter/449379_2_En_6_Fig15_HTML.jpg)

A screenshot has the reset button selected, followed by a dialog box for button properties with the actions tab selected. It has 2 drop-down menus for trigger selected as on-focus and action to run a JavaScript. It has a list of actions, followed by a close button.

Figure 6-15

Reset your field of focus in a form on the Button Properties dialog box’s Actions tab

Here I initially chose the action of Mouse Up – Reset a form, which you can review how to do in Chapters [2](#449379_2_En_2_Chapter.xhtml) and [4](#449379_2_En_4_Chapter.xhtml). However, there is no preprogrammed way to set a field focus, so instead set the trigger to On Focus and then select the action Run a JavaScript and click the Add button. In this example, you can select the action and click the Edit button to review my code. Refer to Figure [6-16](#449379_2_En_6_Chapter.xhtml#Fig16).

![](images/449379_2_En_6_Chapter/449379_2_En_6_Fig16_HTML.jpg)

A screenshot of a dialog box for Java Script Editor has a few lines of code to create and edit Java Scripts. It has a function to get the first name from the field, followed by an ok button at the bottom of the page.

Figure 6-16

Code in the JavaScript editor for the field of focus

```
var fn = this.getField("First Name");
fn.setFocus();
```

Upon clicking OK and exiting the JavaScript Editor, you can close the Properties panel and click the Preview button to test the fields. Then click the Reset button, and the field of focus is the text field called First Name.

For your own projects, try changing the name of the field in the JavaScript code to set a new field of focus.

We will see another example of `setFocus();` in Chapter [10](#449379_2_En_10_Chapter.xhtml), when a text box becomes too full and you need to use another field to keep typing your text.

## Summary

In this chapter, you looked at the three ways you can add calculations to your form fields using the Calculate tab.

*   Value

*   Simplified Field Notation (SFN)

*   Custom calculation script (JavaScript)

While each example became progressively more complex, a broader range of possibilities opened up as you moved toward JavaScript.

You also saw how adding JavaScript to your Validation tab made it possible to see when the Total field was being utilized and when it was not. Giving clients accurate form information updates is important.

Finally, you saw how you can add a field of focus so that when a form is reset, the user knows where to start retyping again.

In the next chapter, you will explore how custom JavaScript can affect the Format tab when it comes to the following:

*   Number

*   Percentage

*   Date

*   Time

# 7. Format Calculations

**Chapter Goal:** In this chapter, you’ll be working mostly with number, percentage, date, and time formats using value, Simplified Field Notation (SFN) and some custom JavaScript. Let’s look at where it works and doesn’t. You’ll also be using some JavaScript to assist you.

Like in Microsoft Excel (see Figure [7-1](#449379_2_En_7_Chapter.xhtml#Fig1)), sometimes you must work with different formats of numbers; you don’t always deal with just 1+1\. The following are other formats you might encounter:

![](images/449379_2_En_7_Chapter/449379_2_En_7_Fig1_HTML.jpg)

A screenshot of the format cells page has 6 tabs, of which the number tab is selected. It has a drop-down menu for the category of which the number is selected. The right side has a box for sample, a box to enter decimal places as 2, and a list of negative numbers.

Figure 7-1

Formatting cells in MS Excel

*   Percentage (98%)

*   Date (June 25, 2023)

*   Time (8:30 a.m.)

The same is true when you work in Acrobat Pro.

Text fields and drop-down lists offer the options shown in Figure [7-2](#449379_2_En_7_Chapter.xhtml#Fig2).

![](images/449379_2_En_7_Chapter/449379_2_En_7_Fig2_HTML.jpg)

A screenshot of the text field properties has 8 tabs, of which the format tab is selected. It has a drop-down menu to select the format category with options of none, number, percentage, date, time, special, and custom, of which the none option is selected.

Figure 7-2

Formatting cells in Acrobat Pro in the properties dialog box

Table [7-1](#449379_2_En_7_Chapter.xhtml#Tab1) compares formatting in Acrobat to Excel.

Table 7-1

Comparison of Acrobat and Excel Formatting Options

| Acrobat | Excel |
| --- | --- |
| None | General, Text |
| Number | Number, Currency, Accounting, Scientific |
| Percentage | Percentage |
| Date | Date |
| Time | Time |
| Special | Special |
| Custom | Custom |
| Possibly accomplished with Custom or using two separate formatted number fields | Fraction |

As you can see, most formatting is similar. None or general is considered a default setting; the field could contain generic numbers or text. However, they would not be used in a calculation.

Note

If you want to work along in this lesson or review the final result, download the Chapter [7](#449379_2_En_7_Chapter.xhtml) files from the link mentioned in the introduction. The file with the label “Start” is the file without the code, and the file with the label “End” is the final result. You will also find a folder with original MS Word and PDF files if you would like to edit them, and a folder containing the original scripts if you would like to add them to your own PDF forms.

Remember that to view the properties of a field, you must select the Prepare Form tool; only then can you right-click or double click a field to review its properties.

## Number Formatting

You can accomplish most of your calculations using a combination of value and SFN. To view, refer to the PDF files Acrobat Format and Calculate Tab Examples. See the file `AcrobatFormatCalcTabExamples_End.pdf` in the file folder for this chapter and look at the fields TotalNumber for addition and TotalNumber_2 for subtraction. Refer to Figure [7-3](#449379_2_En_7_Chapter.xhtml#Fig3) as well as the settings in Figure [7-4](#449379_2_En_7_Chapter.xhtml#Fig4) and Figure [7-5](#449379_2_En_7_Chapter.xhtml#Fig5). Later, for an additional example, refer to the file `InvoiceForm_Example_End.pdf`.

![](images/449379_2_En_7_Chapter/449379_2_En_7_Fig3_HTML.jpg)

A screenshot of a page has a table with 4 columns for the format, quantity 1, quantity 2, and total. It has 1 row for format as the number and a total of 0.00.

Figure 7-3

An example of a field formatted with Number

As mentioned, when the Format tab is set to the Select Format Category setting of Number, you then have the number options of **Decimal Places** (0–10).

**Separator Style** can be a combination of comma and decimal points. Examples include 1,234.56, 1234.56, 1.234,56, 1234,56, and 1’234.56. Refer to Figure [7-4](#449379_2_En_7_Chapter.xhtml#Fig4).

![](images/449379_2_En_7_Chapter/449379_2_En_7_Fig4_HTML.jpg)

A screenshot with the format tab selected, with a drop-down menu for the format category as the number. It has multiple parameters within number options, including decimal places, separator style, currency symbol, location, and negative number style, followed by a box for an example of the format.

Figure 7-4

Settings applied to the fields in the Format tab

**Currency Symbol** by default is set to none. However, from the list you can choose options of dollar ($), euro (€), pound (₤), and other currency symbol options that can then appear before or after the currency (Figure [7-4](#449379_2_En_7_Chapter.xhtml#Fig4)). However, if the currency symbol or noncurrency symbol you need does not show up in the list, you may need to use custom JavaScript to accomplish that. We’ll look at that in Chapter [18](#449379_2_En_18_Chapter.xhtml).

**Symbol Location** from the list sets where the symbol will appear before or after the numbers and with or without a space.

The two **Negative Number Style** options are “Show parentheses” and “Use red text.” These are by default unchecked, and you can see how the example of the current format displays in the properties dialog box. However, when “Show parentheses” is enabled, rather than a negative (-) symbol, the numbers show in parentheses instead. “Use red text” also removes the negative symbol and changes the text color from whatever is set on the Appearance tab, like black, to red only when the number is negative. Refer to Figure [7-4](#449379_2_En_7_Chapter.xhtml#Fig4).

Then on the Calculate tab, as you saw in Chapter [6](#449379_2_En_6_Chapter.xhtml), and Figure [7-5](#449379_2_En_7_Chapter.xhtml#Fig5) you can then decide whether to use value, SFN, or JavaScript (a custom calculation script) to perform the calculations.

![](images/449379_2_En_7_Chapter/449379_2_En_7_Fig5_HTML.jpg)

A screenshot of the text field properties page has 8 tabs, of which the calculate tab is selected. It has a box to calculate the value as the sum of the fields, followed by a box for simplified field notation.

Figure 7-5

Possible settings applied to the Total field whether it is value, SFN, or a custom calculation JavaScript

Remember, for examples of JavaScript, as you saw in the math example in Chapter [6](#449379_2_En_6_Chapter.xhtml), sometimes when doing more scientific calculations such as (`Math.tan(x)`), you can use a custom calculation script or JavaScript to assist you.

## Formatting with a Percentage

Like Number formatting, Percentage works well with the Calculate tab’s value and SFN. To view an example, refer to the “End” file called `AcrobatFormatCalcTabExamples` and refer to the fields Quantity 1Percentage and Quantity 2Percentage for addition. Refer to Figure [7-6](#449379_2_En_7_Chapter.xhtml#Fig6). Later, for additional examples, refer to the files `InvoiceForm_Example_End.pdf` and `_End2_customPercent.pdf`.

![](images/449379_2_En_7_Chapter/449379_2_En_7_Fig6_HTML.jpg)

A screenshot of the text field properties page has 8 tabs, of which the format tab is selected. It has a drop-down menu to select the format category as a percentage, and options of decimal places and separator style within the percentage options, followed by an example of the current format.

Figure 7-6

An example of a text field formatted with Percentage

Just like with the Number format, you can set the decimal places and separator style. The % symbol will always appear after the number. Refer to the “Number Formatting” section in this chapter if you need to review.

In this example, I set the field TotalPercentage (read-only) to a format of percentage as well, but you could leave the format at Number if you prefer. However, if you want the numbers to remain within a range of 0–100, then you need to set the Validate tab (Field value is in range) to match this range and prevent the numbers from going higher than 100 percent when the user types in the other fields. Refer to Figure [7-7](#449379_2_En_7_Chapter.xhtml#Fig7).

![](images/449379_2_En_7_Chapter/449379_2_En_7_Fig7_HTML.jpg)

A screenshot of the text field properties has 8 tabs, of which the validate tab is selected. It has 2 check boxes for field value not validated and the range of the field value, of which the second option is selected, followed by from and to values for the range.

Figure 7-7

Set the Validate tab when you want a percentage range

You can set negative numbers as part of the range as well, such as From: -100 To:100.

However, keep in mind when the user types in 1, Acrobat interprets that to mean 100 percent, so if you want 1 percent, you need to type in **0.01**, and “1%” will appear as the number you entered. This can present an issue when you try to type in **100%** in one field and **100%** in the other and even after setting validation you get “200%” with no warning message. Only when you type **101** into a field, which would appear like “10,100.00%,” might you get a warning from the TotalPercentage field.

### A Workaround for the Percentage

If you are concerned that clients will not understand this formatting, you can add a helpful hint using JavaScript, which you’ll explore later. If you don’t want to add a helpful hint, an alternative is to improve this formatting. Let’s create a custom formatting so that when 1 is pressed, only 1% will be entered rather than 100%.

To do this, you need to create a custom format for each percent field that you will be entering percents into. Refer to Figure [7-8](#449379_2_En_7_Chapter.xhtml#Fig8). Fields that will not have data entered, such as the Total, which has been set to read-only on the General tab, can be left with a format of Percent. You can review these same fields Quantity 1Percentage and Quantity 2Percentage now with the changes in this example in my file `AcrobatFormatCalcTabExamples_End_CustomFormat.pdf`.

![](images/449379_2_En_7_Chapter/449379_2_En_7_Fig8_HTML.jpg)

A screenshot of the text field properties has 8 tabs, of which the format tab is selected. It has a drop-down menu to select the format category as custom, a box for the custom format script, and a custom keystroke script within percentage options.

Figure 7-8

Custom format script rather than percentage added to the Format tab

Note

Your version may read Number, Percentage, or Custom options in the label area, but this does not affect the calculations. Refer to Figure [7-8](#449379_2_En_7_Chapter.xhtml#Fig8).

Here on the Format tab, Custom was chosen for “Select format category.” Under Custom Format Script, enter the following code in the JavaScript Editor by clicking the Edit button (Figure [7-8](#449379_2_En_7_Chapter.xhtml#Fig8)):

```
if (event.value !=="" && !isNaN(event.value)){
event.value = util.printf("%.2f%",event.value*100);
}else{
event.value = "";
}
```

Then click OK to exit.

Then add a custom validation to the Validate tab. Refer to Figure [7-9](#449379_2_En_7_Chapter.xhtml#Fig9).

![](images/449379_2_En_7_Chapter/449379_2_En_7_Fig9_HTML.jpg)

A screenshot of the text field properties has 8 tabs, of which the validate tab is selected. It has 2 checkboxes for field value not validated and to run a custom validation script, of which the second option is selected, followed by a few lines of code for the script.

Figure 7-9

Validation script added to the Validate tab

```
if (event.value !== "") {
event.value = event.value / 100;
}
```

On the Format tab, what you have done in this example is tell the field’s validation to remain blank if nothing is entered. You then tell the custom formatting to disregard items that are not numbers (NaN). Then you ask it to display in the following manner using the previous field calculation: it moves the math decimals over 2 so that you have two zeros following the code and multiply by 100\. Adding the second % right after the `f` in the code ensures that a percent symbol will show after numbers. Leaving the first % out before the .2f creates error. Do not do this since we are creating a custom format. If you leave this second % out of the code after the f, then no percent symbol will display after the numbers.

```
event.value = util.printf("%.2f%",event.value*100);
```

Then the Validate tab (Figure [7-9](#449379_2_En_7_Chapter.xhtml#Fig9)) will be set next so that it understands between the two versions of JavaScript that 1 means 1% rather than 100%.

The Validate tab custom script also ensures that the number is divided by 100 to get the percent in the correct decimals when the field is not blank. Refer to Figure [7-9](#449379_2_En_7_Chapter.xhtml#Fig9).

The following is the result you get when you enter a number:

```
Addition of Percentage:  1.00%  + 1.00% =  2.00%
```

As mentioned earlier, for the final summing field TotalPercentage, you do not need to create any custom script. You can leave it as a percentage format and use either Value or SFN in the Calculate area. Refer to Figure [7-10](#449379_2_En_7_Chapter.xhtml#Fig10).

![](images/449379_2_En_7_Chapter/449379_2_En_7_Fig10_HTML.jpg)

2 screenshots of the text field properties page have 8 tabs, of which the format and calculate tabs are selected. Page 1 has a drop-down menu to select the format category as a percentage and 2 percentage options. Page 2 has the value calculated as the sum of the following fields.

Figure 7-10

Format tab and Calculate tab for the Total field

Note

Within the Custom Format settings and Validate tab is another example (Figure [7-8](#449379_2_En_7_Chapter.xhtml#Fig8) and Figure [7-9](#449379_2_En_7_Chapter.xhtml#Fig9)) of a conditional or if/else statement. If/else statements set a condition. “If” the following conditions are met, do whatever is within the brackets ({ }). The other option, `else`, in the previous code gives the alternative. If `if` does not work, then do the `else`, which is whatever is within its brackets. As you will see, for longer conditions you will also add conditions, `else if`, between the `if` and `else`, when you have more than two options. There can also be conditions within conditions, as you will see later in this chapter.For more on decimal places and separator style formatting, see the “Final Thoughts” section later in this chapter.

In this example, you could set a validate range as well, to further prevent a number within the range of 0 and 100 from being entered with the input fields. Here is how that code could be expanded. See the new entry in bold.

```
//Custom Validation
if (event.value !== "") {
event.value = event.value / 100 ;
if (event.value 1){
app.alert("You must enter a Percentage that is between 0% and 100%");
event.value="";
}
}
```

This code was added to fields Quantity 1Percentage and Quantity 2Percentage. We will talk about `app.alert` later in Chapter [8](#449379_2_En_8_Chapter.xhtml), but essentially this code has a validation to prevent numbers entered outside of the 0% of 100% to be entered, and the field will reset if they are. This was done with the logical disjunction operator (||), which in this case will work only if one or more of the input values is true or out of range, in this case less than 0 or greater than 100\. This is represented as 0 and 1 now that the validation and formatting has been already set. This code is placed within the current `if` statement.

### Reset Fields with JavaScript

However, you will still be able to add up a number like 200% in the final field; if that is what you want to do, that is OK. Or you can also go to the Validate tab of that field and just add this similar portion of the code as I did to the TotalPercentage field:

```
if (event.value 1){
app.alert("You have entered a Percentage whose sum is not between 0% and 100%. The fields will reset");
this.resetForm(["Quantity 1Percentage","Quantity 2Percentage"]);
event.value="";
}
```

I also added another new line of code: `this.resetForm();`. Rather than using the Actions tab’s Reset a Form as I would for a reset button as in Chapters [2](#449379_2_En_2_Chapter.xhtml) and [4](#449379_2_En_4_Chapter.xhtml), you can use JavaScript to reset select fields that are placed between brackets, [], and quotes to reset. The field names in the quotes have spaces, but this is OK as long as they are within the quote marks like this: “`Name of Field`”. Each field is separated by a comma. This code is useful when you do not want to reset the whole form after a warning. This ensures that the user will have to enter the correct numbers this time.

I have left the Subtraction example fields NumberInPercentage,NumberOutPercentage, and TotalPercent_2 without this additional formatting if you want to compare the results with and without this kind of validation. In both examples, the Calculate tab for the total used SFN, but alternatively you could use JavaScript.

## Date Formatting

Date formatting is easy to do for one field. To view, refer to the “End” file of AcrobatFormatCalcTabExamples and refer to the field Quantity_1Date, which was set to the (mmm d, yyyy) format. You can set the date to a variety of options for month (mm), day (dd), and year (yyyy), with dashes for additional options for hours (hh), minutes (MM), AM/PM (tt), and a colon (:), including a custom setting. The format will display in the example area. Refer to Figure [7-11](#449379_2_En_7_Chapter.xhtml#Fig11).

Visit the following page to learn about cFormat string patterns:

[`https://opensource.adobe.com/dc-acrobat-sdk-docs/library/jsapiref/JS_API_AcroJS.html`](https://opensource.adobe.com/dc-acrobat-sdk-docs/library/jsapiref/JS_API_AcroJS.html)

![](images/449379_2_En_7_Chapter/449379_2_En_7_Fig11_HTML.jpg)

A screenshot of the text field properties page has 8 tabs, of which the format tab is selected. It has a drop-down menu to select the format category as date, a list of format for date options, followed by an example of the current format.

Figure 7-11

Selecting a format of date on the Format tab and an option

However, when it comes to adding or subtracting dates from one another, value and SFN do not work well. See the fields Quantity 1Date, Quantity 2Date, and TotalDate.

```
fDate:    March 3, 2015     + 1.00 =    4.2015
```

As you can see, this math makes no sense, and trying to use two fields formatted to date to enter a number into a final text field formatted to Number can often cause errors when using value and SFN; refer to Figure [7-12](#449379_2_En_7_Chapter.xhtml#Fig12).

![](images/449379_2_En_7_Chapter/449379_2_En_7_Fig12_HTML.jpg)

2 screenshots of warning messages. 1 has a warning message that reads, invalid date or time, please ensure that the same exists and matches the format. 2 has a message this format does match the default value please choose or change the default value in the options panel to match the format.

Figure 7-12

Examples of error warnings that happen when you try to add two fields that are formatted with dates or another format using Calculate Values or SFN

Once again, you need to look at JavaScript to assist you.

### Date Formatting with JavaScript

Suppose that you want to create a hotel or timesheet form that deals with check-in and check-out dates for half or full days. Refer to Table [7-2](#449379_2_En_7_Chapter.xhtml#Tab2) to see an example. You can also look at three fields: DateStart, DateEnd, and TotalDate_2.

Table 7-2

Table of Formatted Dates

| Format | Number In | Number Out | Total |
| --- | --- | --- | --- |
| Date: | Mar 3, 2015 | March 9, 2015 | 6 |

The Number In or DateStart field is formatted as a date. Refer to Figure [7-11](#449379_2_En_7_Chapter.xhtml#Fig11). The Number Out or DateEnd field is formatted as Date as well so that it is consistent and easy to enter in the date fields using the Date Picker. With JavaScript you can use two date fields to do a calculation. The format options should be the same, for example, “mmm d, yyyy.” However, the final event field should be a Number format. Refer to Figure [7-13](#449379_2_En_7_Chapter.xhtml#Fig13) and Figure [7-14](#449379_2_En_7_Chapter.xhtml#Fig14).

![](images/449379_2_En_7_Chapter/449379_2_En_7_Fig13_HTML.jpg)

A screenshot of the text field properties page has 8 tabs, of which the format tab is selected. It has a drop-down menu to select the format category as date, followed by a list of formats for the date options. It has an example of the current format at the bottom of the page.

Figure 7-13

The setting for the DateEnd field on the Format tab is the same as the DateStart field

The Total field TotalDate_2 is formatted as Number; with a custom calculation on the Calculation tab, this completes the formula. Refer to Figure [7-14](#449379_2_En_7_Chapter.xhtml#Fig14) and Figure [7-15](#449379_2_En_7_Chapter.xhtml#Fig15).

![](images/449379_2_En_7_Chapter/449379_2_En_7_Fig15_HTML.jpg)

A screenshot of the text field properties page has 8 tabs, of which the calculate tab is selected. It has 4 checkboxes, and the fourth checkbox for the custom calculation script is selected, followed by a few lines of code for the calculation script.

Figure 7-15

Setting for the Total field on the Calculate tab

![](images/449379_2_En_7_Chapter/449379_2_En_7_Fig14_HTML.jpg)

A screenshot of the text field properties page has 8 tabs, of which the format tab is selected. It has a drop-down menu to select the format category as the number, followed by decimal places, separator style, currency symbol, symbol location, and negative number style within the number options.

Figure 7-14

Setting for the Total field on the Format tab

The code is as follows:

```
var sStart = getField("DateStart").value;
var sEnd = getField("DateEnd").value;
var dStart, dEnd, diff, days;
if(sStart.length && sEnd.length) {
dStart = util.scand("mmm d, yyyy", sStart);
dEnd = util.scand("mmm d, yyyy", sEnd);
if ( dEnd < dStart)  {
app.alert ( "The end date must be after start date and not before.");
event.value = "";
this.resetForm(["DateStart"]);
}
else{
//one day is equal to 24hours* 60minutes*60 seconds* 1000 miliseconds = 86,400,000
var oneDay = 86400000;
diff = (dEnd.getTime() - dStart.getTime())/oneDay;
//var days = Math.floor(diff)+1;
var days = Math.round(diff)+1;
event.value = days;
}
}
else
{
event.value = "";
}
```

In this example, I added a few comments to help me with the calculations. This code on the Calculate tab grabs the start date and the end date fields from the variables `sStart` and `sEnd` and gets an input value from each of them. Those values are then added into a conditional statement, which has another conditional `if`/`else` statement inside of it. The outer conditional statement `if` states that the solution will be a number or “else” it will be blank.

However, in the outer `if` statement the calculation is broken down further. We see that the `if` makes sure that the values of start date and end date are indeed numbers. Length helps you get a number, and the length of each variable (`var sStart` and `var sEnd`) is checked against the formatting “mmm d, yyyy.” It’s important that this is the same formatting found in the Number In and Number Out or DateStart and DateEnd fields or this calculation will not work. They are now in the new variables called `dStart` and `dEnd`.

The `util.scand()` part of the code is a function or method that can convert any date string by using the date symbols, but it has to know the exact format of the date for this to work.

In the inner `if`/`else` conditional, first a validation check is made with another alert. This is to ensure that you do not enter a start date after the end date by mistake. This would be considered an incorrect value, and you would get an alert. When you click OK to the warning message, the DateStart Field resets to the last default saved state and resets the field TotalDate_2 to blank, and then you must begin resetting the start date or end date again so that the TotalDate_2 field fills again. Likewise, in your own projects you could add a separate reset button to refresh the entire form. We saw an example of `this.resetForm()` earlier in the chapter.

However, when both dates are correct, then you can proceed to the inner `else` condition.

Once this is done for both fields, the end date and start date are recognized as time using the JavaScript `.getTime()` method. For more information, visit this page:

[`https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/getTime`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/getTime)

With JavaScript there are many get `Date`-related methods available, but, in this example, we want to get the milliseconds rather than just part of a date.

This returns a number in milliseconds (example: 1432672724073), which could be quite long. This is not the exact answer you want. You want to know how many days. Once the `getTime` of the `dateStart` is subtracted from the `getTime` of the `dateEnd`, you then figure out how long one day is. In this case, one day is equal to 24 hours * 60 minutes * 60 seconds * 1000 milliseconds = 86,400,000 milliseconds.

Then we divide a day of milliseconds from the result of the two dates to get our variable `diff` or difference.

We can at this point make a choice between two different static methods.

*   `Math.floor()` rounds down the difference to the nearest day.

*   `Math.round()` rounds up the difference to the nearest day.

Either of these numbers will work fine with the dates if you are accounting using half days; with a hotel, you arrive in the evening and leave in the morning. Therefore, you are not actually staying a whole two days. However, if you need to count full days, then you would write this:

```
var days = Math.round(diff)+1;
```

Otherwise, you can remove the “+1” from the code Giving you 6 days as in the example of Table [7-2](#449379_2_En_7_Chapter.xhtml#Tab2) above rather than 7 full days.

I then complete the inner `else` with this:

```
event.value = days;
```

The result now shows up in the field TotalDate_2 in the form. The files `AcrobatFormatCalcTabExamples_End.pdf` and `Custom Format.pdf` give examples.

You could not have done this calculation in Acrobat without JavaScript.

Note in the code examples I wrote the two variables as follows:

```
var sStart = getField("DateStart").value;
var sEnd = getField("DateEnd").value;
```

Likewise, I could have written it as follows:

```
var sStart = this.getField("DateStart").value;
var sEnd = this.getField("DateEnd").value;
```

And the code would have worked the same. If you want to be stricter in your code, add `this` to `getField`. However, in the lengthy example, you may want to save space and leave it off in other lines if you must recall the variable and field elsewhere in the document. You can add `this` if you find your code is not acting correctly.

## Time Formatting

Time formatting is in many ways similar to date formatting. You can even add time to your date depending on what you intend for the field. However, time unlike the date field has no “time picker,” and you must type the numbers in yourself. So, it is important to choose the correct Time option from the list. To view, see the “End” file AcrobatFormatCalcTabExamples, and refer to the field Quantity 1Time. Refer to Figure [7-16](#449379_2_En_7_Chapter.xhtml#Fig16).

![](images/449379_2_En_7_Chapter/449379_2_En_7_Fig16_HTML.jpg)

A screenshot has a table with 4 columns for the format, quantity 1, 2, and total. It has 2 screenshots for the text field properties with the format tab selected. 1 has the format category selected as time, followed by time options. 2 has the format category as a date followed by date options.

Figure 7-16

Comparison of Time and Date formatting on the Format tab

Either one can also accept a custom date or time format, which can also include hours (HH), minutes (MM), seconds (SS), and an a.m./p.m. indicator (tt).

However, while they’re similar, you can use some SFN rather than JavaScript to do a simple subtraction of time, though I would ultimately recommend using JavaScript, which we will look at in a moment reusing our previous “Date” formatting code.

For example, to see how using a value does not work well in the calculation area, refer to Tables [7-3](#449379_2_En_7_Chapter.xhtml#Tab3) and [7-4](#449379_2_En_7_Chapter.xhtml#Tab4).

Table 7-3

Formatting Total Using Values on the Calculate Tab

| Format | Number In | Number Out | Total |
| --- | --- | --- | --- |
| Date: | 12:15 | 27 | 39.15 |

In the first example, you could refer to the text fields Quality 1Time, Quality 2Time, and TotalTime, and the result makes little sense just like the Date formatting example.

In the next table we try to subtract one time from another to get the total hours using the fields NumberInTime, NumberOutTime, and field TotalTime_2.

Table 7-4

Formatting Total Using SFN on the Calculate Tab

| Format | Number In | Number Out | Total |
| --- | --- | --- | --- |
| Date: | 04:30 | 15:30 | 11:00 |

Now let’s use SFN.

To make this work correctly, let’s say Joe came in at 4:30 a.m. and left at 3:30 p.m. (formatted as 15:30). He therefore stayed a total of 11 hours.

Both the NumberInTime and NumberOutTime fields are formatted this way: HH:MM. Refer to Figure [7-17](#449379_2_En_7_Chapter.xhtml#Fig17).

![](images/449379_2_En_7_Chapter/449379_2_En_7_Fig17_HTML.jpg)

A screenshot of the text field properties page has 8 tabs, of which the format tab is selected. It has a drop-down menu to select the format category as time, followed by a list of formats for the time options.

Figure 7-17

Formatting the in and out fields with time on the Format tab

The TotalTime_2 field is then formatted as a number. Refer to Figure [7-18](#449379_2_En_7_Chapter.xhtml#Fig18).

![](images/449379_2_En_7_Chapter/449379_2_En_7_Fig18_HTML.jpg)

A screenshot of the text field properties page has 8 tabs, of which the format tab is selected. It has a drop-down menu to select the format category as the number, and options for decimal places, separator style, currency symbol, symbol location, and negative number style within number options.

Figure 7-18

Number format for the Total field on the Format tab

The last thing that is done is the SFN on the Calculate tab. Refer to Figure [7-19](#449379_2_En_7_Chapter.xhtml#Fig19).

![](images/449379_2_En_7_Chapter/449379_2_En_7_Fig19_HTML.jpg)

A screenshot of the text field properties page has 8 tabs, of which the calculate tab is selected. It has 3 checkboxes, of which the third is selected for the simplified field notation, followed by an edit button at the right side of the box.

Figure 7-19

Using SFN in the Total field on the Calculate tab

NumberOutTime - NumberInTime gives you the number of hours that work was done in the day.

This works with simple number formats that are in full-hour or half-hour increments, like 4:30 to 15:30, that add up to a full hour. However, that is not how the time is accurately calculated such as when you are working with certain times like subtracting 1:30 a.m. from 12 p.m. You can see what happens to the time using SFN. Now let’s look at an example of how to reuse the Date code we created earlier for Time.

### Time Formatting with JavaScript, a Custom Example

This time with the code, let’s rewrite parts of the code. First, however, I will point out something I discovered. The code that I am going to show you does not work directly with the formatting of time and throws a few errors when I try to modify it. However, you can get around this issue and create a custom time and date using the format of Date and then just modify specific areas of the code.

In this example, make sure that your NumberInTime and NumberOutTime fields are formatted to a custom date of “mm/dd/yyyy HH:MM tt.” Refer to Figure [7-20](#449379_2_En_7_Chapter.xhtml#Fig20).

![](images/449379_2_En_7_Chapter/449379_2_En_7_Fig20_HTML.jpg)

A screenshot of the text field properties page has 8 tabs, of which the format tab is selected. It has a drop-down menu to select the format category as date, followed by a list of formats for date options.

Figure 7-20

Using the Date format to set a custom time

Thankfully, the Date format allows us to add HH for hours, MM for minutes, ss for seconds, and tt for the a.m./p.m. indication.

Make sure that your TotalTime_2 field is formatted to Number. This time, on the Calculate tab, enter this code. Refer to Figure [7-21](#449379_2_En_7_Chapter.xhtml#Fig21).

![](images/449379_2_En_7_Chapter/449379_2_En_7_Fig21_HTML.jpg)

A screenshot of the text field properties page has the calculate tab selected. It has the checkbox for custom calculation script selected, followed by a few lines of code for the calculations script, and an edit button on the right side of the box.

Figure 7-21

Using the Date format to set a custom time

Here is the time script:

```
//Time Script
var sStart = getField("NumberInTime").value;
var sEnd = getField("NumberOutTime").value;
var dStart, dEnd, diff, hours;
if(sStart.length && sEnd.length) {
dStart = util.scand("mm/dd/yyyy HH:MM tt", sStart);
dEnd = util.scand("mm/dd/yyyy HH:MM tt", sEnd);
if ( dEnd < dStart)  {
app.alert ( "The end date must be after start date and not before.");
event.value = "";
this.resetForm(["NumberInTime","NumberOutTime"]);
}
else{
//one day is equal to 24hours* 60minutes*60 seconds* 1000 milliseconds = 86,400,000
// hours is 3,600,000
var oneHour = 3600000;
diff = (dEnd.getTime() - dStart.getTime())/oneHour;
//var hours = Math.floor(diff);
var hours = Math.round(diff);
event.value = hours;
}
}
else
{
event.value = "";
}
```

In this example, I was just rounding the numbers or in this case the hours. I changed the numbers from 86,400,000 to 3,600,000, which would also help me to figure out how many hours there might be in a day or maybe several days since with dates I can use the date picker. Refer to Figure [7-22](#449379_2_En_7_Chapter.xhtml#Fig22) for an example of how this looks.

![](images/449379_2_En_7_Chapter/449379_2_En_7_Fig22_HTML.jpg)

A screenshot of a data field has 4 entries of time, 2 entries with a date and time, and the last entry as 11\. The second and third entries have the same date and different times in the same format.

Figure 7-22

Fields set with a custom date and time

You can also write the variable hours without `Math.round` if you want a more accurate time number when dealing with half hours or other times.

```
var hours = diff;
```

Or if you want to round the nearest half-hour, you could write this:

```
var hours = Math.round(diff*2)/2;
```

You can refer to this code in my file `AcrobatFormatCalcTabExamples_End_CustomFormat.pdf`.

### Comb of Characters for Dates

You can also format your dates using a comb of characters, as found on the Options tab. This can be useful when setting a custom option under the date or even time. However, be aware that depending on the format you choose or create, you need to account for the dashes as well as any spacing. In this case, I added a border on the Appearance tab and on the Options tab set the comb of characters to 10\. Refer to Figure [7-23](#449379_2_En_7_Chapter.xhtml#Fig23) and Figure [7-24](#449379_2_En_7_Chapter.xhtml#Fig24).

![](images/449379_2_En_7_Chapter/449379_2_En_7_Fig24_HTML.jpg)

A screenshot of a page has the options tab selected, followed by a list of checkboxes. It has checkboxes for fields for file selection, password, check spelling, multi-line, scroll long text, allow rich text formatting, limit, and combination, of which the last box is checked as 10 characters.

Figure 7-24

Setting a comb of characters for a date format using the Options tab

![](images/449379_2_En_7_Chapter/449379_2_En_7_Fig23_HTML.jpg)

A screenshot has the format tab selected, followed by a format category selected as date, and an example of the selected format. It has a screenshot of the text field properties with the appearance tab selected, followed by the details of the borders, colors, and text.

Figure 7-23

Setting a comb of characters for a date format using the Format and Appearance tabs

## Special Formatting

You can use the Special formatting option to set your ZIP code, phone number, Social Security number, and arbitrary mask (but not additional JavaScript); we will look at this area again in Chapter [11](#449379_2_En_11_Chapter.xhtml). Refer to Figure [7-25](#449379_2_En_7_Chapter.xhtml#Fig25).

![](images/449379_2_En_7_Chapter/449379_2_En_7_Fig25_HTML.jpg)

A screenshot of the text field properties page has 8 tabs, of which the format tab is selected. It has a drop-down menu to select the format category as special, followed by a list of options under special options, of which the arbitrary mask is selected.

Figure 7-25

Setting for Special formatting

## Final Thoughts

As you can see from the formatting examples covered in this chapter, sometimes you will use value and SFN. However, in some cases, they will not give you the results you want. If the calculation becomes complicated or scientific, you may need to look for a custom JavaScript to assist you or look at how you can reuse your code to solve a problem in another field when one type of formatting does not work.

### Drop-Down Alternatives

As in Chapter [2](#449379_2_En_2_Chapter.xhtml) and Chapter [6](#449379_2_En_6_Chapter.xhtml), there is a drop-down alternative in this chapter’s folder if your clients are required to enter very specific values. I have supplied an example that includes some modified code to work with the drop-down lists and the Total text field’s Calculate tab. Refer to the file `AcrobatFormatCalcTabExamples_End_DDown.pdf`.

### More Information on Adding Decimal Places and Leading Zeros

Earlier in the chapter we looked at some code with added decimal places when it came to percents.

```
if (event.value !=="" && !isNaN(event.value)){
event.value = util.printf("%.2f%",event.value*100);
}else{
event.value = "";
}
```

In that example, we only used that Format code without the validation, and we moved the decimal places over two places after the decimal point, written as (%.2f%). This code by itself would give the number 100.00%. By altering the following section of code:

```
event.value = util.printf("%.1f%",event.value*100);
```

you would then get 100.0% or only one decimal place.

Finally, if you added numbers, before the decimal point, like so:

```
event.value = util.printf("%08.0f%",event.value*100);
```

you would get 00000100% when you enter a 1\. This moves the numbers over to eight places before the decimal, and in this case there are no decimal points. Try this with a similar line of code and observe how the leading zero changes. Keep this in mind with your current percent calculation between the Format and Validate tab code.

```
event.value = event.value / 100 ;
```

So, when you create your own calculations, you will not run into unusual formatting errors.

If you need to use commas as a separator or another separator that was in the drop-down percent separator style list, instead you can write the line of code by adding a comma after the first percentage and then a number like 2 before the dot. You will get 100,00% or with longer numbers 120.000,00%. The second example uses a 0 and will add the comma first like 100,000.00%.

```
event.value = util.printf("%,2.2f%",event.value*100);
or
event.value = util.printf("%,0.2f%",event.value*100);
```

You can use the numbers 0–4 to access separator options in the list for number or percent formats. Just make sure to add the comma before the number choice, as shown in in the previous lines. If you do not add this code as in the earlier percent examples, 1 (decimal place only) appears to be the default format.

## Summary

In this chapter, you focused on using the Format tab while working in either text fields or drop-down lists.

You looked at formatting using the following:

*   None

*   Number

*   Percentage

*   Date

*   Time

*   Special

*   Custom

While Acrobat has some automatic solutions for some formats, in this case you needed to create a calculation using value, SFN, or JavaScript to get the results you wanted. Sometimes you must use a combination of formats.

You also looked at how custom formatting interacted with code on the Validate tab. In Part III, you’ll look at the Validate tab in more detail.

In the next chapter, you’ll look at how you can add various alerts to your document using JavaScript and add some JavaScript to buttons that will create notes and time stamps.

# 8. Various JavaScript Alerts, Notes, and Time Stamps

**Chapter Goal:** In this chapter you’ll work with alerts that may or may not be part of a form. You’ll also create comment notes and add a signature and time stamp.

As you have seen in previous chapters, JavaScript is useful for forms when you want to do calculations.

However, sometimes you want to accomplish things that are non-form-related.

In this chapter, you’ll create an alert that welcomes a person when they open your PDF newsletter or catalog, like the one in Figure [8-1](#449379_2_En_8_Chapter.xhtml#Fig1).

![](images/449379_2_En_8_Chapter/449379_2_En_8_Fig1_HTML.jpg)

A warning dialog box on the Javascript window. The text on the box reads welcome to our Newsletter. An OK button appears at the bottom right.

Figure 8-1

An informational message that appears when someone opens the newsletter PDF file

Note

If you want to work along in this lesson or review the final result, download the Chapter [8](#449379_2_En_8_Chapter.xhtml) files from the link in the introduction. The file with the label “Start” is the file without the code, and the file with the label “End” is the final result. You will also find a folder containing the original scripts if you would like to add them to your own PDF forms.

If you are creating your form from an original PDF, refer to the “Forms Review” section in Chapter [1](#449379_2_En_1_Chapter.xhtml).

Remember that to view the properties of a field, you must select the Prepare Form tool; only then can you right-click or double-click a field to review its properties.

## Alert Types

JavaScript has at least four alert types that you can use to alert your clients to an event. It also has four button types that you can choose from. Figure [8-1](#449379_2_En_8_Chapter.xhtml#Fig1) shows the info or status type. I chose this because I did not want to scare the person with a warning, which this is not. It is more of a greeting.

Table [8-1](#449379_2_En_8_Chapter.xhtml#Tab1) provides a breakdown of the alerts with icon and button type.

Table 8-1

Alert Types

| # | Icon Type | # | Button Type |
| --- | --- | --- | --- |
| 0 | Error![](images/449379_2_En_8_Chapter/449379_2_En_8_Figa_HTML.jpg)An icon for denoting error. It has a cross in the center of a circle. | 0 | ![](images/449379_2_En_8_Chapter/449379_2_En_8_Figb_HTML.jpg)An icon of the OK button. The borders are highlighted |
| 1 | Warning ![](images/449379_2_En_8_Chapter/449379_2_En_8_Figc_HTML.jpg)An icon of the warning message. It has a triangle with an exclamatory mark in the middle. | 1 | ![](images/449379_2_En_8_Chapter/449379_2_En_8_Figd_HTML.jpg)A screenshot comprises the OK and cancel buttons. The OK button is highlighted. |
| 2 | Question ![](images/449379_2_En_8_Chapter/449379_2_En_8_Fige_HTML.jpg)An icon of question. It denotes a question mark inside a circle. | 2 | ![](images/449379_2_En_8_Chapter/449379_2_En_8_Figf_HTML.jpg)A screenshot of the yes and no buttons. The Yes button is highlighted. |
| 3 | Status/Info ![](images/449379_2_En_8_Chapter/449379_2_En_8_Figg_HTML.jpg)An icon of the status info. It has an I inside a circle. | 3 | ![](images/449379_2_En_8_Chapter/449379_2_En_8_Figh_HTML.jpg)A screenshot denotes the yes, no, and cancel buttons. The yes button is highlighted at the borders. |
| 4 | No Icon | 4 | Returns to default ![](images/449379_2_En_8_Chapter/449379_2_En_8_Figi_HTML.jpg)An OK button is highlighted at the borders. |

To achieve this alert example, you need to add it *globally*, which means it is part of the whole document. When the document opens, you want to see this alert.

To do that, you need to go to the JavaScript tool in the Tools menu. Refer to Figures [8-2](#449379_2_En_8_Chapter.xhtml#Fig2) and [8-3](#449379_2_En_8_Chapter.xhtml#Fig3).

![](images/449379_2_En_8_Chapter/449379_2_En_8_Fig2_HTML.jpg)

An icon with 2 curly brackets. A text, Javascript is written at the bottom. An add button with a drop-down menu is placed below the text.

Figure 8-2

The JavaScript tool

## Create a Document JavaScript

Now you need to choose Document JavaScripts from the JavaScript menu (Figure [8-3](#449379_2_En_8_Chapter.xhtml#Fig3)). To view, refer to the `Newsletter_2_End.pdf` in this chapter’s file folder. If want to practice, then use the file `Newsletter_2_Start.pdf`.

![](images/449379_2_En_8_Chapter/449379_2_En_8_Fig3_HTML.jpg)

A screenshot of the menu bar with the following options. debug, debugger, document JavaScripts, all JavaScripts, and document actions.

Figure 8-3

The JavaScript menu

When you first start creating your JavaScript for the document, the Document JavaScripts dialog box will appear blank. No JavaScripts have been created, so you’ll need to follow these steps:

1.  Type in a script name. Refer to Figure [8-4](#449379_2_En_8_Chapter.xhtml#Fig4). In this example, I typed **welcome**.

    ![](images/449379_2_En_8_Chapter/449379_2_En_8_Fig4_HTML.jpg)

    A dialog box of documents Java scripts. There is a blank space to enter the script name. A prompt under the dialog box denotes the script name as welcome.

    Figure 8-4

    The Document JavaScripts dialog box

2.  Click the Add button on the right. Refer to Figure [8-5](#449379_2_En_8_Chapter.xhtml#Fig5).

    ![](images/449379_2_En_8_Chapter/449379_2_En_8_Fig5_HTML.jpg)

    A rectangular button icon with is labeled add on it.

    Figure 8-5

    The Add button

3.  In this example, upon entering the JavaScript Editor dialog box, you can highlight and delete the default text and then type the following script:

    ```
    //Welcome Message on start
    app.alert("Welcome to our Newsletter!",3,0);
    ```

    The first line with the //, as mentioned earlier, is a comment. It has nothing to do with the function of the code; it is just a reminder of what the code is about.

    The real code look like this:

    ```
    app.alert("Welcome to our Newsletter!",3,0);
    ```

    This code is calling up an alert message. Within this `app.alert()`, you can put your custom text (“Welcome to our Newsletter!”) in this example. Then you choose what type of icon and button you want to appear; they are separated by commas. Refer to Figure [8-6](#449379_2_En_8_Chapter.xhtml#Fig6).
    *   3 refers to the Alert Icon type (in this case Status/Info). Refer to Table [8-1](#449379_2_En_8_Chapter.xhtml#Tab1).

    *   0 refers to the Button type (in this case, the OK button).

    ![](images/449379_2_En_8_Chapter/449379_2_En_8_Fig6_HTML.jpg)

    A screenshot of a dialog box to create and edit Javascript with the function welcome. It comprises a Java script editor with a set of code for the welcome message on start. The ok, cancel, and go to buttons are at the bottom.

    Figure 8-6

    The removed code and new code and message typed into the JavaScript Editor

    Depending upon the type of alert, you might choose other options.

4.  Click the OK button in the editor when done typing and click Close to exit the Document JavaScripts dialog box. Click the Close button to exit the JavaScript tool. Then save your work.

5.  After you close the PDF and open it again, the alert should pop up. Click OK to the message, and the document opens.

## Viewer Version and Validation Alert

Let’s say you want to make sure that your customer’s version of Reader is up-to-date enough to accomplish certain tasks.

You might create and add your own Viewer validation script such as `viewerVersion`. Refer to Figure [8-7](#449379_2_En_8_Chapter.xhtml#Fig7). This example is also found in the file `Newsletter_2_End.pdf` under JavaScript Tool Document JavaScripts.

![](images/449379_2_En_8_Chapter/449379_2_En_8_Fig7_HTML.jpg)

A dialog box titled document Javascripts.The script name is highlighted as the viewer version. A rectangular box at the bottom denotes a set of codes for the version of Acrobat. There are 4 buttons to close, add, edit, and delete.

Figure 8-7

Document JavaScript dialog box displaying three scripts used in the Newsletter PDF

```
//What version of Acrobat
if (typeof(app.viewerVersion)!="undefined")
if (app.viewerVersion <7.5){
var msg = "You must use Adobe Acrobat or Adobe Reader version 7.5 or above to complete this form.";
app.alert(msg,1,0);
}
```

This code makes sure that when the document opens in Reader, the code interacts with the client’s software, checks itself against the conditional statement, and warns the client, using 1 (Warning) if the version is earlier than 7.5 to indicate that they’re going to have difficulty completing this form (see Table [8-1](#449379_2_En_8_Chapter.xhtml#Tab1)). The client then knows they need to upgrade. Then click OK, which is (0). See Figure [8-8](#449379_2_En_8_Chapter.xhtml#Fig8).

![](images/449379_2_En_8_Chapter/449379_2_En_8_Fig8_HTML.jpg)

A screenshot of a dialog box of the Javascript window. It explains that Adobe Reader version 7.5 or above should be used to complete the form.

Figure 8-8

Warning that appears so that the viewer knows they need to upgrade their software to complete the form

You could also create and add a `viewerType` validation.

```
if (typeof(app.viewerType)!="undefined")
if (app.viewerType == "Reader")
{
var msg = "You must use Acrobat Standard, Acrobat Pro, or Acrobat Pro Extended to complete this form.";
app.alert(msg,1,0);
}
```

If the client is using Reader and not Acrobat Pro, they might have an issue completing this form because of certain settings that the programmer is aware of. If you are in Acrobat Pro, this warning is ignored.

When done, click OK and save your work. Click the Edit button to inspect the code, and click Cancel if don’t want to make changes. Then click Close to exit the dialog box. For your own actions, when using the Document JavaScripts dialog box, if you make a mistake, you can click the Delete button and create the action again. Refer to Figure [8-7](#449379_2_En_8_Chapter.xhtml#Fig7).

Note

If you think that your client may open their file in a different reader or PDF program, you may need to alter your code to reflect that. Remember that a program such as your browser may corrupt the JavaScript in your forms, so run a test of the alert with a client or co-worker first. Also keep in mind that you don’t set your version too high as most clients will be able to complete their forms with an older version of Acrobat Pro or Reader. Currently, I am using version 23.0\. However, your clients may have version 18.0 and be able to complete the form or view the document with no issues, so adjust this warning accordingly, and don’t set it purposely to cause stress.

## Document Actions

Other types of global alerts may need to be set up in another area. From the JavaScript tool, choose Document Actions. Refer to Figure [8-9](#449379_2_En_8_Chapter.xhtml#Fig9).

![](images/449379_2_En_8_Chapter/449379_2_En_8_Fig9_HTML.jpg)

A screenshot of the menu bar with the following options. debug, debugger, document JavaScripts, all JavaScripts, and document actions.

Figure 8-9

The JavaScript Tool menu

In this `Newsletter_2_End.pdf` file, you can see two document actions. You know this because there is a dot beside each action that has been created. You can edit the selected action by clicking the Edit button. The Edit All button shows all actions currently in the document, not just the document actions. Click Cancel if you need to exit that dialog box and then return to the Document Actions dialog box. Refer to Figure [8-10](#449379_2_En_8_Chapter.xhtml#Fig10).

![](images/449379_2_En_8_Chapter/449379_2_En_8_Fig10_HTML.jpg)

A screenshot of a dialog box titled document actions with a list of the options to close, save, and print a document. The execution of javascript represents a code for app alert.

Figure 8-10

The Document Actions dialog box

### Document Will Close

When the document closes, you might want to add a friendly goodbye to the customer to thank them for taking the time to read the newsletter. Click the Edit button to view or enter the following code:

```
app.alert("Thanks for looking at our Newsletter!",3,0);
```

Click the OK button when you’re done to exit the JavaScript editor. Click OK to exit the document actions if you want to commit any changes you made.

The script is similar to the welcome greeting; however, you need to enter it into this section in order for it to work when the document is closed.

### Document Will Print

You might enter something like a message before the document prints after the client has clicked Print in the File ➤ Print dialog box:

```
var msg = "Hope you found what you were looking for."
app.alert(msg,3,1);
```

Note

In this case, the message was written as a separate variable, `var msg`.

I could have written it as one line:

`app.alert("Hope you found what you were looking for.",3,1);`

The same thing would have happened.

However, it is good to know how to write it the other way if you think your message is lengthy or you might use it more than once. It is easier to access.

The following are the other document options available in this area:

*   **Will Save**: The message appears before saving.

*   **Did Save**: The message appears after saving.

*   **Did Print**: The message appears after document prints.

When you’re done, click the OK button to exit the dialog box, close the JavaScript tool, and save your work.

You will look further into global document JavaScripts throughout the following chapters as you can access this area for various form fields to avoid code repetition.

## Alerts Working with Buttons

Not all alerts have to be global. Sometimes you want an alert to happen only on the push of a button or when you enter text incorrectly into a text box. To view this button, refer to Figure [8-11](#449379_2_En_8_Chapter.xhtml#Fig11) and `Newsletter_2_End.pdf` on page 4.

![](images/449379_2_En_8_Chapter/449379_2_En_8_Fig11_HTML.jpg)

A screenshot of a rectangular button labeled alert message.

Figure 8-11

The alert message button found in the Newsletter PDF

You can create a button either in the Prepare Form area or in the Rich Media area. Refer to Figure [8-12](#449379_2_En_8_Chapter.xhtml#Fig12).

![](images/449379_2_En_8_Chapter/449379_2_En_8_Fig12_HTML.jpg)

A screenshot represents 2 icons labeled prepare form and rich media. Each icon comprises a drop-down menu of add.

Figure 8-12

Buttons are found in either the Prepare Form or Rich Media tools

You can then add an alert action to the button using a Run a JavaScript action in the Button Properties dialog box’s Actions tab. In this example, I used a Trigger setting of Mouse Up. Refer to Figure [8-13](#449379_2_En_8_Chapter.xhtml#Fig13).

![](images/449379_2_En_8_Chapter/449379_2_En_8_Fig13_HTML.jpg)

A screenshot of a dialog box of button properties. It has 5 tabs, where the actions tab is selected. It denotes that mouse up is selected as the trigger for the action to run a Javascript. A Javascript editor at the bottom has a set of code for the message alert button.

Figure 8-13

The Actions tab in the Button Properties dialog box running a JavaScript action. Text is entered into the JavaScript Editor

As before, you set a trigger of Mouse Up and select the action of Run a JavaScript. Then you click the Add button and add the following text:

```
//Message Alert Button
var msg = "This is my first JavaScript Alert Message";
app.alert(msg,3,0);
```

Click OK to exit the editor, click Close to exit the Button Properties screen, click Close to exit the tool, and save your work.

Figure [8-14](#449379_2_En_8_Chapter.xhtml#Fig14) shows the alert the button produced when clicked. You will look at these types of alerts later.

![](images/449379_2_En_8_Chapter/449379_2_En_8_Fig14_HTML.jpg)

A screenshot of a dialog box of the Javascript window. The text reads this is my first Java script alert message.

Figure 8-14

The alert message that was produced when clicked

In Chapter [10](#449379_2_En_10_Chapter.xhtml) we will look at how to create a button that prints a specific page using JavaScript. If you are looking for information on different sounds that alerts play or `app.beep();` There are four types; however, you should know that these options are ignored by macOS and UNIX systems. They are optional, so I do not add them to my code and instead let the default sound play.

## Adding a Comment Note, Signature, and Time Stamp

As you can see, buttons are great for alerts, but you can also use them to add both nonform and form items to a page.

For instance, maybe you want to encourage your clients to add a comment to a specific page once they are done proofing it. You could add a button. See the button on page 2 of the Project_Comment “Start” and “End” PDFs. Refer to Figure [8-15](#449379_2_En_8_Chapter.xhtml#Fig15) and Figure [8-16](#449379_2_En_8_Chapter.xhtml#Fig16).

![](images/449379_2_En_8_Chapter/449379_2_En_8_Fig16_HTML.jpg)

A screenshot of a dialog box of the button properties. It has 4 tabs, where the actions tab is selected. It indicates the trigger as mouse up for the action to run a Javascript.

Figure 8-16

The Run a JavaScript action on the Actions tab for the Button Properties dialog box

![](images/449379_2_En_8_Chapter/449379_2_En_8_Fig15_HTML.jpg)

A screenshot of a rectangular button labeled Add a comment.

Figure 8-15

A button that will add a comment or sticky note to the page

Like before, to do this, you need to select a Mouse Up trigger for the button on the Actions tab and then select Run a JavaScript. Click the Add button. Refer to Figure [8-16](#449379_2_En_8_Chapter.xhtml#Fig16) and Figure [8-17](#449379_2_En_8_Chapter.xhtml#Fig17).

![](images/449379_2_En_8_Chapter/449379_2_En_8_Fig17_HTML.jpg)

A screenshot of the Javascript editor to create and edit the Javascript. It consists of a snippet of code to add a comment.

Figure 8-17

Text entered into the JavaScript Editor

This time you need to type the following:

```
//Add a Comment/Sticky note
var annot = this.addAnnot({
page: this.pageNum,
type: "Text",
rect: [187, 168, 487, 243],
strokeColor: color.yellow
});
```

The code here is written a bit differently. Comments or annotations as they are sometimes known do not have in their properties the ability to add JavaScript directly to them, so an action must be added indirectly, in this case using the click of the button if you want certain custom settings that include color or placement. Here’s what that means. When the button is clicked, it will call upon `addAnnot` or the annotation/comment. A comment sticky note will be added to the page that the button is on, `this.pageNum`.

This will allow you to add text. The type of comment will be set to certain coordinates on the page. This comment will have an icon color of yellow, yet the fill color of the comment may be different. The user can then enter some text in the comments area and post it, and it will display on the page when you open it. Refer to Figure [8-18](#449379_2_En_8_Chapter.xhtml#Fig18).

![](images/449379_2_En_8_Chapter/449379_2_En_8_Fig18_HTML.jpg)

A screenshot indicates a comment of an undefined user with a text, this looks great. A post button is there at the bottom of the dialog box. Another screenshot denotes the comment at the bottom.

Figure 8-18

Resulting comment when client clicks the button and then enters some text

In this example, the client comes in as Undefined. They can, however, change this later in the General Properties of the comment if they right-click and set a new author name. Refer to Figure [8-19](#449379_2_En_8_Chapter.xhtml#Fig19).

![](images/449379_2_En_8_Chapter/449379_2_En_8_Fig19_HTML.jpg)

A set of 2 dialog boxes. The upper one denotes a context menu under the comment and highlights the properties option. The bottom one indicates the sticky note properties with the text boxes to enter author name and subject along with the modification date.

Figure 8-19

Changing the author name for the sticky note after using the Properties dialog box

Likewise, you could add a line of code that would already have the author’s name set. Refer to the bold code in the following listing. Refer to Figure [8-20](#449379_2_En_8_Chapter.xhtml#Fig20) for the result in the comment area.

![](images/449379_2_En_8_Chapter/449379_2_En_8_Fig20_HTML.jpg)

A screenshot represents the author's name, John Smith, along with the indication of time as 2, colon, 43 PM, and the comment icon.

Figure 8-20

Changing the author name for the sticky note after using JavaScript

```
//Add a Comment/Sticky note
var annot = this.addAnnot({
page: this.pageNum,
type: "Text",
author: "John Smith",
rect: [187, 168, 487, 243],
strokeColor: color.yellow
});
```

The `Rect` property, also known as an *array* in this case, refers to rectangular coordinates of the rectangle defining the location of annotation on the page.

Colors can be formatted for `strokeColor`, from `color.yellow` to `color.red` or another known color. You will consider other types of color formatting later in Chapter [10](#449379_2_En_10_Chapter.xhtml). However, I will just point out here that you are not limited to annotation type of sticky notes or to the type Text.

For example, you can change the Type to other options, which include Circle, Stamp, and PolyLine.

You can refer to other annotation options in this online reference guide of resource code and see annotation type and properties for more details and examples of coding to fit your project:

[`https://opensource.adobe.com/dc-acrobat-sdk-docs/library/jsapiref/JS_API_AcroJS.html#annotation`](https://opensource.adobe.com/dc-acrobat-sdk-docs/library/jsapiref/JS_API_AcroJS.html#annotation)

[`https://opensource.adobe.com/dc-acrobat-sdk-docs/library/jsdevguide/JS_Dev_RMA.html?highlight=annotation`](https://opensource.adobe.com/dc-acrobat-sdk-docs/library/jsdevguide/JS_Dev_RMA.html?highlight=annotation)

Tip

Make sure to compare annotation names with the actual annotations in the Comment tool if you are unsure what Type setting to use. For example, in the code a Type setting of StrikeOut would be the same as comment Strike Through. And Caret would be the same as “insert text at cursor,” and “Ink” is the same as using the drawing tool. Be aware that ones like Squiggly do not have direct comparison to the comment cloud and may instead appear as underlined text. For a Cloud type, you can use Polygon instead; however, you will likely have to alter additional properties in the code for it to appear correctly on the page.

### Signature Field and Time Stamp

Another thing that you can do with a button is use it to add form fields and a time stamp. Refer to page 2 of the Project Comment PDF “End” file and Figure [8-21](#449379_2_En_8_Chapter.xhtml#Fig21).

![](images/449379_2_En_8_Chapter/449379_2_En_8_Fig21_HTML.jpg)

A screenshot represents a rectangular button labeled signature field and time stamp.

Figure 8-21

The Signature Field and Time Stamp button

Sometimes you want a button to do two things at once, and this is a way of speeding up the process. Again, with a Trigger setting of Mouse Up, you can select Run a JavaScript on the Actions tab. Refer to Figure [8-22](#449379_2_En_8_Chapter.xhtml#Fig22).

![](images/449379_2_En_8_Chapter/449379_2_En_8_Fig22_HTML.jpg)

2 screenshots of different dialog boxes. The top one denotes the button properties indicating the settings under the actions tab. The mouse is selected as the trigger to run Javascript. The bottom one denotes a snippet of code under the Javascript editor to add a signature field.

Figure 8-22

The JavaScript added for the Time Stamp button using the Actions tab

Both scripts are in the same JavaScript Editor dialog box. Refer to Figure [8-22](#449379_2_En_8_Chapter.xhtml#Fig22) and Figure [8-23](#449379_2_En_8_Chapter.xhtml#Fig23) to see the results when the button is clicked.

![](images/449379_2_En_8_Chapter/449379_2_En_8_Fig23_HTML.jpg)

A screenshot of the signature field. It denotes a rectangular blank space with text above it, that reads, this page was reviewed on January 26, 2023.

Figure 8-23

Resulting Signature field

The signature code in this section reads as follows:

```
//Adding a signature field automatically
var c = this.addField({
cName: "clientSignature",
cFieldType: "signature",
nPageNum: this.pageNum,
oCoords: [35,74,176,112]
})
```

This first section controls where the signature is being placed using `var c` to add a field. It has been given the name `clientSignature` so that it is identifiable because all fields need a distinct name. The type of field is a signature, which is for signatures only and not for the following:

*   *Text*: `cFieldType`: `"text"`

*   *Button*: `cFieldType`: `"button"`

*   *Drop-down*: `cFieldType`: `"combobox"`

*   *List box*: `cFieldType`: `"listbox"`

*   *Check box*: `cFieldType`: `"checkbox"`

*   *Radio button*: `cFieldType`: `"radiobutton"`

Again, I have also set the properties for this page to place this field as well as its coordinates on this page. The coordinates can relate to settings found on the Position tab, which are by default set to Units: Points approximately and may need to be adjusted based on the cFieldType and where you want the field to be placed. Depending on the type of cFieldType, you need to make sure that it has a distinct cName is well if you plan to add more than one type of field as adjust your oCoords: [left, bottom, right, top].

*   You can refer to this link as a reference:

*   [`https://opensource.adobe.com/dc-acrobat-sdk-docs/library/jsdevguide/JS_Dev_AcrobatForms.html`](https://opensource.adobe.com/dc-acrobat-sdk-docs/library/jsdevguide/JS_Dev_AcrobatForms.html)

After `this.addField()` , to format the color of the new field further, as we will explore in more detail in Chapter [10](#449379_2_En_10_Chapter.xhtml), we need to do another step so that the default is no longer transparent. In this example, you next need to the get the field you just created as a new variable and then apply the color formatting in this case to the border. This can all be done in the same JavaScript action. To see this example, refer to the file `Project_Comment_End_2.pdf`.

```
var b = this.getField("clientSignature");
b.strokeColor = color.red;
```

The following is the time stamp code for “This Page was viewed on Jan 26, 2023”:

```
//script that creates the text field
{
var r = [200, 200, 400, 300];
var i = this.pageNum;
var f = this.addField(String("completeDate."+i),"text",i,r);
f.textSize = 10;
f.alignment = "right";
f.textColor = color.blue;
f.fillColor = color.transparent;
f.textfont = font.HelvB;
f.strokeColor = color.transparent;
f.value = String("This page was reviewed on: " + util.printd("mmm dd, yyyy", new Date()));
f.readonly = true;
}
```

Next, the time stamp is actually a text field called completeDate.1, and it is added to the page with today’s date when the user clicks the button. Various properties are tied to the text box that also appear on the same page as the signature field. These include such things as properties for text size, alignment, text color, background fill color, type of font, and stroke or border color. Then the New date is stamped on the page with the date formatting of “mmm dd, yyyy.” And this text field is set to read-only on the General tab using the code. The position or `var r` in this case is an approximate placement in points if compared to the actual tab.

Note

You can format its color to blue or red and choose a different font as well. This text (“This page was reviewed on: Date”) is now locked in, because `readonly` was set to true. If you want this text to be edited in some way later, you could have set it to false. This is what is known as a *Boolean expression*; it can only ever be true or false. You will see a way to set `readonly` for a text field with a check box in Chapter [9](#449379_2_En_9_Chapter.xhtml).

You will look at other reasons to format text and the fields with JavaScript in Chapter [10](#449379_2_En_10_Chapter.xhtml).

Though a more advanced topic, I will just mention as a final note that you can also use buttons to add other nonform items that include watermarks, bookmarks, and hyperlinks. You can find more information on this topic of watermarks at the following page:

[`https://opensource.adobe.com/dc-acrobat-sdk-docs/library/jsdevguide/JS_Dev_PDFCreation.html?highlight=watermark#adding-watermarks-and-backgrounds`](https://opensource.adobe.com/dc-acrobat-sdk-docs/library/jsdevguide/JS_Dev_PDFCreation.html?highlight=watermark#adding-watermarks-and-backgrounds)

## Summary

In this chapter, you added some global alerts to your document and some alerts to buttons as well. You also discovered that you can use buttons to create comment/sticky notes, signature fields, and time stamps. As you can see, alerts are useful for nonform documents and buttons for adding comments.

In the next chapter, you’ll take a look at how alerts can assist you in forms.

# 9. Create Help for Clients with Rollover Text and Alerts

**Chapter Goal:** You’ll learn some helpful ways to add alerts to your client’s forms so that they know when they are entering the correct information to avoid errors.

In Chapter [4](#449379_2_En_4_Chapter.xhtml), you learned that you could create a small help menu with the order form using buttons that hide and show a hint about the customer order code. However, if you need your text to be more detailed, this method might not always be the best solution. Luckily, in Acrobat there are a few other methods you can use that can assist you with creating help for your client. In addition, we will look at how to use JavaScript to hide and show files, duplicate input text in other fields, and use a check box so that a client can set a field or fields to read-only and reset fields as well.

Note

If you want to work along in this lesson or review the final result, download the Chapter [9](#449379_2_En_9_Chapter.xhtml) files from the link in the introduction. The file with the label “Start” is the file without the code, and the file with the label “End” is the final result. You will also find folders containing the original files and scripts if you would like to add them to your own PDF forms.

If you are creating your form from an original PDF, refer to the “Forms Review” section in Chapter [1](#449379_2_En_1_Chapter.xhtml).

Remember that to view the properties of a field, you must select the Prepare Form tool; only then can you right-click or double-click a field to review its properties.

If you want to practice any of the next three examples, you can make a copy of the file `TMC_OrderForm_Start.pdf`.

## The Rollover Method

Let’s look at the rollover method first. Refer to Figure [9-1](#449379_2_En_9_Chapter.xhtml#Fig1).

![](images/449379_2_En_9_Chapter/449379_2_En_9_Fig1_HTML.jpg)

A screenshot has a bright shaded box with the text, what is your full name, followed by a text box to enter the customer's full name. It has another box indicating the data in month, day, and year format, followed by a text box to enter the date.

Figure 9-1

When you roll over a text field, different information appears in the upper red text box to give your client a hint

In this example (see the file `TMC_OrderForm_End_rollover.pdf)`, you can see that when a client moves over a field, the red box in the upper-right corner of the form provides a helpful hint so that they know what to type if they’re not sure. This is useful when a client needs to know how to type in the correct date format. A hint is always welcomed.

As the client moves out of the field, it becomes blank again until the next field is rolled over. The client now knows what to type, and errors are avoided when the form is returned to you.

In this example, when you examine the following fields, they are all controlled by a document JavaScript globally and within these fields:

*   Customer Full Name

*   Date

*   Order Code

*   Customers Company Name

The red help box, txtHelp**,** has no JavaScript internally; however, it is referenced globally by all fields. It is, however, set on the General tab to (Read Only) so that the user cannot alter the text.

First, let’s take a look at the global or document JavaScripts. In the JavaScript tool, select Document JavaScripts. Refer to Figures [9-2](#449379_2_En_9_Chapter.xhtml#Fig2) and [9-3](#449379_2_En_9_Chapter.xhtml#Fig3).

![](images/449379_2_En_9_Chapter/449379_2_En_9_Fig3_HTML.jpg)

A screenshot of a dialog box for document Java Scripts has a text box to enter the script name, followed by 4 options to close, add, edit, and delete. The close option is selected. A few lines of code with the function help exit are at the bottom of the box.

Figure 9-3

Document JavaScripts dialog box

![](images/449379_2_En_9_Chapter/449379_2_En_9_Fig2_HTML.jpg)

A screenshot denotes an icon with open and close curly brackets, followed by the text Java Script, and an add button. Following icons are there at the bottom of the page. Debug, debugger, document Java Scripts, All Java Scripts, and document actions.

Figure 9-2

The JavaScript tool and its subtools

You can see there are two document JavaScripts.

`HelpExit` is a global function that controls those fields that will be accessing the red help field, txtHelp.

As the user exits the field, it returns to a blank state.

```
function HelpExit()
{
var helpText = this.getField("txtHelp");
helpText.value ="";
}
```

The other JavaScript is `helpText`; this function contains an array.

```
var gHelpText = [ // Create a variable, gHelpText
"What's your full name?", //String#0
"mm/dd/yyyy", //String#1
"Your customer code is on the form that was emailed to you.", //String#2
"What’s your Company Name?" //String#3
]
```

An array is written with brackets, [], which you will see more of later. It contains multiple variables or strings within itself, and each is separated by a comma. These variable strings can then be accessed by other fields within this form. In the previous code example, I have used the comments (//) to number them from 0 to 3 so I know their order.

Note

JavaScript seems to support the main type of array: indexed list-like objects, not the associative arrays. These indexed arrays can be nested (in the sense that they can hold an array within an array). In this chapter, you have been looking at the basic index type (array literal), which, even though no actual numbers are in the array, based on the order you can still call on a number or string reference from 0–3\. In Chapters [12](#449379_2_En_12_Chapter.xhtml) and [13](#449379_2_En_13_Chapter.xhtml), you will see examples of more complex nested arrays or list objects and their syntax, which deal with words rather than numbers to do the indexing; these array/objects literals will be nested so that information can flow into other fields. Refer to this page for more details:

[`https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)`.`

Now let’s take a look at one of the fields that needs help: Customer Full Name.

In the case of the four fields that are showing “help message” in the red box, they all have the following two JavaScripts. Refer to Figure [9-4](#449379_2_En_9_Chapter.xhtml#Fig4).

![](images/449379_2_En_9_Chapter/449379_2_En_9_Fig4_HTML.jpg)

A screenshot of a dialog box titled text field properties. It has 8 tabs, of which the actions tab is selected. It has 2 drop-down menus to select trigger as mouse up and action to run a JavaScript. It denotes 2 actions under mouse enter and exit.

Figure 9-4

Selecting Run a JavaScript on Mouse Enter and Mouse Exit on the Actions tab

*   When the mouse or cursor enters, run a JavaScript.

*   When the mouse or cursor exits, run a JavaScript.

### Mouse Enter

The JavaScript for the Mouse Enter setting is as follows:

```
var helpText = this.getField("txtHelp");
helpText.value =gHelpText[0];
```

As the mouse enters the field Customer Full Name, JavaScript is asked to access some text from the document JavaScript `helpText` function array `gHelpText`. With arrays, the number 0 always represents the first item in an array, like so:

```
0= "What's your full name?"
1= "mm/dd/yyyy"
2= "Your customer code is on the form that was emailed to you."
3= "What's your Company Name?"
```

And so on, if there were more items in the array.

For [0], it will access the string “What’s your full name?” and place this text in the field txtHelp.

In the case of the Date field, you can see it is the same code except for the number in the array you accessed. This time the Mouse Enter setting is [1]. For the Order Code field, it’s [2], and for the Customer’s Company Name field, it’s [3].

```
var helpText = this.getField("txtHelp");
helpText.value =gHelpText[1];
```

For your own projects, you could add another string to the document JavaScript if you had an additional field, but when you call on it, you would give it the index number of [4]. That is why I added comments earlier in the code so that when I have a lot of text strings, I can keep track of them.

### Mouse Exit

The JavaScript code for the Mouse Exit is the same for all four fields.

```
HelpExit();
```

When the mouse or cursor exits the red help box, it becomes blank.

Writing global document JavaScript like this can be helpful if for some reason you need to change some code inside this function. Any field that had this function attached would now do the new action rather than you having to go manually through and change each one.

In this case, they all follow the earlier code upon exit, and the red txtHelp box returns to a blank state.

```
function HelpExit()
{
var helpText = this.getField("txtHelp");
helpText.value ="";
}
```

### Longer Array Example (Optional)

The code in the previous example was clean and efficient for the needs of this form. However, if you wanted to break down the actual string of the array into objects in the document JavaScript for gHelpText, you could type the following:

```
var gHelpText = new Array();
//spans
//String#0
gHelpText[0] = new Object();
gHelpText[0].text ="What's your full name?";
//String#1
gHelpText[1]= new Object();
gHelpText[1].text ="mm/dd/yyyy";
//String#2
gHelpText[2] = new Object();
gHelpText[2].text ="Your customer code is on the form that was emailed to you.";
//String#3
gHelpText[3] = new Object();
gHelpText[3].text = "What's your Company Name?";
```

This, while much longer, would be useful if you plan to add some specific formatting of color or alignment to the font using JavaScript, which we will look at in more detail in Chapter [10](#449379_2_En_10_Chapter.xhtml). However, then in each field, when you Mouse Enter, you would have to adjust the code by adding a `.text` property at the end of the last line. See the bold.

```
var helpText = this.getField("txtHelp");
helpText.value = gHelpText[0].text;
```

This ensures that you get the text to appear in the red box txtHelp. If not, the word `[Object]` will appear for all examples. You would also make sure that your number matches the string. Refer to the file `TMC_OrderForm_End_rollover_longArray.pdf` if you need to see an example of this.

### Extra Noncustom JavaScript Check Box Example

As in the form in Chapter [4](#449379_2_En_4_Chapter.xhtml), there are the examples of the Shipping Address hide and show text fields. Review this area if you can’t remember how it works.

### How to Show and Hide Fields with JavaScript

However, if you wanted, you could allow this area to be written with JavaScript instead. Refer to the file `TMC_OrderForm_End_rollover.pdf` and the check box field Check Box7 and the button called Hide and Clear Fields. Refer to Figure [9-5](#449379_2_En_9_Chapter.xhtml#Fig5).

![](images/449379_2_En_9_Chapter/449379_2_En_9_Fig5_HTML.jpg)

A screenshot has an unchecked checkbox for a different shipping address, followed by a button to hide and clear fields. The action under the mouse up indicates to run a javascript.

Figure 9-5

Review of Shipping Address check box and button examples

For the check box, you could go the Actions tab, add the Mouse Up trigger, and run a JavaScript with the following code:

```
//Show Fields and button
this.getField("First Name_2").display = display.visible;
this.getField("Last Name_2").display = display.visible;
this.getField("Address_2").display = display.visible;
this.getField("City_2").display = display.visible;
this.getField("Province_2").display = display.visible;
this.getField("Country_2").display = display.visible;
this.getField("Postal Code_2").display = display.visible;
this.getField("Hide and Clear Fields").display = display.visible;
```

Every field that you want to display, including the button, will appear by setting the display to `= display.visible`.

Then for the button Hide and Clear Fields on the Actions tab on Mouse Up, you can run a JavaScript with the following code:

```
//Reset Fields
this.resetForm(["Check Box7","First Name_2", "Last Name_2", "Address_2", "City_2", "Province_2", "Country_2", "Postal Code_2"]);
// Hide Fields and Button
this.getField("First Name_2").display = display.hidden;
this.getField("Last Name_2").display = display.hidden;
this.getField("Address_2").display = display.hidden;
this.getField("City_2").display = display.hidden;
this.getField("Province_2").display = display.hidden;
this.getField("Country_2").display = display.hidden;
this.getField("Postal Code_2").display = display.hidden;
this.getField("Hide and Clear Fields").display = display.hidden;
```

You have seen the reset code before, and in this case, I also make sure that I reset my check box back to its original state. However, this time in the code I set the display for each of the fields that I want to hide to `display = display.hidden`, including the button. This is a good way to keep the code organized.

Alternatively, rather than having the button at all, you could remove it, and you might just want to use the check box only to, upon unchecking, hide and reset those fields. You can see this alternate option in my file `TMC_OrderForm_End_rollover_2.pdf`.

For just the check box, you could type the following JavaScript code. Notice that I, in this case, organized all my variables at the top to avoid repetition of the field names, to keep organized. On the `if`, I made fields visible, and on the `else`, after the reset, I hid the fields.

```
var checkbox = this.getField("Check Box7");
var fn= this.getField("First Name_2");
var ln = this.getField("Last Name_2");
var address = this.getField("Address_2");
var city = this.getField("City_2");
var province = this.getField("Province_2");
var country = this.getField("Country_2");
var postalCode = this.getField("Postal Code_2");
if (event.target.value !="Off"){
//Show the fields and button
fn.display = display.visible;
ln.display = display.visible;
address.display = display.visible;
city.display = display.visible;
province.display = display.visible;
country.display = display.visible;
postalCode.display = display.visible;
}
else{
//Reset Fields
this.resetForm(["Check Box7","First Name_2", "Last Name_2", "Address_2", "City_2", "Province_2", "Country_2", "Postal Code_2"]);
// Hide Fields and Button
fn.display = display.hidden;
ln.display = display.hidden;
address.display = display.hidden;
city.display = display.hidden;
province.display = display.hidden;
country.display = display.hidden;
postalCode.display = display.hidden;
}
```

Notice how the extra button is no longer required. Instead the check box’s Run a JavaScript included the reset code. Refer to Figure [9-5](#449379_2_En_9_Chapter.xhtml#Fig5). On the `else` condition, I then reset the form fields first before I hid them. In this case you cannot call on the variables only on the fields when resetting, so you must make sure your code is spelled correctly. Remember for your own projects to click OK after you exit the editor. Close the Properties panel and preview and test your form to ensure the correct fields are resetting. Then save your file.

## The Default Text Method

The rollover is a good method for some forms, but on others it may not work well, such as when fields are near the bottom of the page or forms that are two pages or more. As with forms on the Internet, the default text is sometimes found within the actual field. You can add default text to a field in the Options tab. However, when the user clicks that field, the text does not disappear; it remains. You must select the text first and then delete it. This can become awkward and frustrating to the client who just wants to start typing. Refer to Figure [9-6](#449379_2_En_9_Chapter.xhtml#Fig6). On your own, add this text in the Options tab to a blank text field and test it.

![](images/449379_2_En_9_Chapter/449379_2_En_9_Fig6_HTML.jpg)

A screenshot of the text field properties page has 8 tabs, of which the options tab is selected. It has a drop-down menu for alignment as left, a box for the default value, followed by a few checkboxes, of which the checkboxes for spelling check and scrolling long text are selected.

Figure 9-6

Adding default text to the Text field in the Options tab

Instead of adding the default text, you can create a custom format in the Format tab with this script. Refer to Figures [9-7](#449379_2_En_9_Chapter.xhtml#Fig7) and [9-8](#449379_2_En_9_Chapter.xhtml#Fig8) and the file `TMC_OrderForm_End _default_text.pdf`.

![](images/449379_2_En_9_Chapter/449379_2_En_9_Fig8_HTML.jpg)

A screenshot has 2 text boxes for the customer's full name. Box 1 denotes the text, what is your full name. box 2 has an entry, John Smith.

Figure 9-8

Custom default text (top) and the text entered (bottom)

![](images/449379_2_En_9_Chapter/449379_2_En_9_Fig7_HTML.jpg)

A screenshot of the text field properties page has 8 tabs, of which the format tab is selected. It has a drop-down menu to select a format category as custom, followed by a few lines of custom format script, and an edit button at the right side of the box.

Figure 9-7

Adding a custom script to the Format tab

You can refer to the field Customer Full Name for this exercise.

```
// Custom Format script
if (!event.value){
event.value = "What is your full name?";
event.target.textColor = color.gray;
}
else{
event.target.textColor = color.black;
}
```

Figure [9-8](#449379_2_En_9_Chapter.xhtml#Fig8) shows the difference.

In this example, the default text is gray so long as nothing is in the field and will disappear when the field is clicked and the new text is typed in. The new text will be black. I applied this same code to the customer’s Company Name field and just altered the `event.value` to reflect the field’s default text.

```
// Custom Format script
if (!event.value){
event.value = "What is your company name?";
event.target.textColor = color.gray;
}
else{
event.target.textColor = color.black;
}
```

No other JavaScript Actions or Document action needs to be applied to those fields.

For some text boxes, this is a good method. However, for other boxes that have explanations longer than the field or must also be formatted as a date, a number, or a comb of characters, using this custom format may not be ideal. You may in those cases still want to use your Document actions as seen with the rollover method earlier. In this file, I left those fields as they were previously. For now, let’s look at another option for those instances.

Note

`if (!event.value)` becomes a type of Boolean or a true and false statement (if not) when you add the exclamation point. In this case of default text, if there is no value entered in the text field, here is what you should fill it with for now. Without this exclamation point, the default text will not fill in.

## The Alert Method

Is there some other way that the message can move along with the client as they enter fields down the form? Yes, it can be done with alert boxes. Another cool feature of alert boxes is, if you set them up correctly, you can enter text into them, and the text will then be transferred to the correct form field. Refer to Figure [9-9](#449379_2_En_9_Chapter.xhtml#Fig9) and to the file `TMC_OrderForm_End_alert.pdf`.

![](images/449379_2_En_9_Chapter/449379_2_En_9_Fig9_HTML.jpg)

A screenshot of 2 warning messages for Java Script Window. 1\. A line of text that reads, type your first and last name, followed by a box to enter the name and an ok button. 2\. A message with the typed name and instructions to register the name.

Figure 9-9

Text is entered into an alert box and is then entered into the field once the user clicks OK and then tabs out of that field

In this example (see the Alert End PDF file), there is no global document JavaScript, so we can skip that area for now.

Again, there are four text fields:

*   Customer Full Name

*   Date

*   Order Code

*   Customers Company Name

This time there is no red txtHelp field; you will use the alert boxes to do that work as it would be awkward to see the red box lower on the form. Now we will look at each field one at a time and explore the code further.

### Customer’s Full Name

This time you will run only one JavaScript per field. Do not worry about an exit function because the alert box will take care of that for you, as you will see. In this case, use the trigger On Focus, which activates when anyone clicks into the text field. Refer to Figure [9-10](#449379_2_En_9_Chapter.xhtml#Fig10).

![](images/449379_2_En_9_Chapter/449379_2_En_9_Fig10_HTML.jpg)

A screenshot of the text field properties has 8 tabs, of which the actions tab is selected. It has 2 drop-down menus to select the trigger as on-focus and action to run a Java Script, followed by a list of actions, a deleted button, and a close button.

Figure 9-10

Run a JavaScript action in the Actions tab

This is the code that was added to the Run a JavaScript action in the JavaScript Editor, as shown in Figure [9-10](#449379_2_En_9_Chapter.xhtml#Fig10):

```
var t = event.target; // the target field
var cResponse = app.response({
cQuestion: "Type your name First, Last",
cTitle: "Name",
cDefault: "Name not added",
cLabel: "Type your name"});
{
if (cResponse == null){ // if Cancel is selected
app.alert ("You cancelled adding your name.");
cResponse = "";
}
else{
app.alert("You typed your name as: \""+cResponse+"\" \n\n Tab out of the field to register your name",3);
}
}
t.value = cResponse; // places the data from the dialog to the target field
```

In Figure [9-11](#449379_2_En_9_Chapter.xhtml#Fig11), you can see that the alert or `app.response` creates a description with the following properties or parameters of question (`cQuestion`), title (`cTitle`), default text (`cDefault`), and Label (`cLabel`), as well as a space for the user to type their name as requested, replacing the default text.

![](images/449379_2_En_9_Chapter/449379_2_En_9_Fig11_HTML.jpg)

A screenshot of a warning message for the Java Script Window name. It has a line of text to type the first and the last name, followed by a box to enter the name and an ok button.

Figure 9-11

The JavaScript help response message window

Note

Optionally you can also have a parameter of Password (`bPassword`), which if true, would show * or a bullet to mask the user’s response. By default this is left off or set to false.

If the user clicks the Cancel button, a warning or app alert with the following message appears: “You cancelled adding your name.” The text field will go back to blank upon clicking OK to the message. Refer to Figure [9-12](#449379_2_En_9_Chapter.xhtml#Fig12).

![](images/449379_2_En_9_Chapter/449379_2_En_9_Fig12_HTML.jpg)

A screenshot of a warning message from the Java Script window has a message that reads you canceled adding your name. It has an ok button at the bottom right side of the page.

Figure 9-12

The JavaScript warning when the user clicks Cancel in the earlier window

If the user types in their name and clicks OK, they get the following app alert: “You typed your name as: [*client name*]” or the line of code called \““+cResponse+”\”. As shown in Figure [9-13](#449379_2_En_9_Chapter.xhtml#Fig13), they click OK to the message. Once the client tabs out of that field to the next field, their name is confirmed, as shown in the line of code `t.value= cResponse;`.

The 3 in the code in this case refers to the change in icon to status/info while in the previous alert it was not added or the default of 0\. Also, no code number in this and the previous example was used for the button, but it again is 0 or the OK button in both cases. So, in this example, you could instead write 3,0 and gotten the same result. You can review these setting in Chapter [8](#449379_2_En_8_Chapter.xhtml) in the section “Alert Types.” Refer to Figure [9-13](#449379_2_En_9_Chapter.xhtml#Fig13).

![](images/449379_2_En_9_Chapter/449379_2_En_9_Fig13_HTML.jpg)

A screenshot of a warning message from the Java Script window denotes a message that reads you typed your name as Jennifer Harder, tab out of the field to register your name. It has an ok button at the bottom right side of the page.

Figure 9-13

The JavaScript confirmation after the user clicks OK to an earlier window

If your tab order is set up correctly, you should then move on to the next field to continue the process.

Note that the way `app.response` is displayed is a bit different from `app.alert.` With `app.response`, in this case the default buttons are OK and Cancel with no alert icon. Yet it can work together with app alerts to create a chain of alerts for the user.

Let’s move to the next field. Notice how again you use the `if/else` condition to get two possible answers.

### Date

The Date field behaves in a similar fashion. However, remember to make sure this field is formatted to a date that matches the text in this example: “mm/dd/yyyy.” Refer to Figure [9-14](#449379_2_En_9_Chapter.xhtml#Fig14).

![](images/449379_2_En_9_Chapter/449379_2_En_9_Fig14_HTML.jpg)

A screenshot of a dialog box titled text field properties. It has 8 tabs, of which the format tab is selected. The format category for the date is selected as the month, slash, day, slash, year format.

Figure 9-14

The Date Options menu on the Format tab

Now go to the Actions tab and see that I used the same setting as with the customer’s full name, altering the text to suit my needs. Refer to Figures [9-15](#449379_2_En_9_Chapter.xhtml#Fig15) and [9-16](#449379_2_En_9_Chapter.xhtml#Fig16).

![](images/449379_2_En_9_Chapter/449379_2_En_9_Fig16_HTML.jpg)

A screenshot of a dialog box denotes a warning message for the Java Script window date. The message reads, type the date in month, date, and year format, followed by a box to enter the date. It has an ok button at the bottom of the page.

Figure 9-16

JavaScript help message for the Date field

![](images/449379_2_En_9_Chapter/449379_2_En_9_Fig15_HTML.jpg)

A screenshot of a dialog box titled text field properties. It has 8 tabs, of which the actions tab is selected. The trigger is selected as on focus for the action to run a JavaScript. There is an add button on the right side.

Figure 9-15

Run a JavaScript action for the Date field on the Actions tab

The following is the code:

```
var t = event.target; // the target field
var cResponse = app.response({
cQuestion: "Type the Date mm / dd / yyyy",
cTitle: "Date",
cDefault: "Date not added",
cLabel: "Type the Date"});
{
if ( cResponse == null){ // if Cancel is selected
app.alert ("You cancelled adding the date.");
cResponse = "";
}
else{
app.alert("You typed the date as: \""+cResponse+"\" \n\n Tab out of the field to register the date",3);
}
}
t.value = cResponse; // places the data from the dialog to the target field
```

We can see how a similar pattern of events unfolds. In this field, a date must be added. You click OK to the alert and then tab to the next field to confirm the response. Keep in mind, however, that Acrobat in recent years made entering dates much easier with the “date picker,” so you may or may not choose to use this alert. Regardless, additional alerts will still appear if you do not enter the correct date format, and the field will remain blank until you do even if you tab out of the field. You can see my comments on this in the “Final Thoughts” section later in this chapter.

### Customer Order Code

You will construct the customer order code in the same way as the date; only the code will be a bit different. Refer to Figure [9-17](#449379_2_En_9_Chapter.xhtml#Fig17) to see the result.

![](images/449379_2_En_9_Chapter/449379_2_En_9_Fig17_HTML.jpg)

2 screenshots of a warning message from Java Script Window. 1\. The customer order code has a text box to enter the 16-digit code number, followed by an ok button. 2\. The message confirms the code number and denotes the instruction to register the card number.

Figure 9-17

JavaScript help message for customer order code and confirmation message

The customer order code functions similarly, and you could have set the format to Number. However, you may want letters and numbers in your code, so leave the formatting set to None on the Format tab. On the Actions tab with a Trigger setting of On Focus, select Run a JavaScript. Refer to Figure [9-15](#449379_2_En_9_Chapter.xhtml#Fig15).

```
// Customer Code or Credit Card Example
var t = event.target; // the target field
var cResponse = app.response({
cQuestion: "Type your Customer Order Code number using numbers/letters only and without dashes",
cTitle: "Customer Order Code",
cLabel: "Type your 16 digit code number"});
{
if ( cResponse == null){ // if Cancel is selected
app.alert ("You cancelled adding your code.");
cResponse = "";
}
else {
if (cResponse.length != 16){
app.alert("Type the complete number/letters without dashes or there may be more or less than 16 digits.");
cResponse = "";}
else{
app.alert("You typed your Order Code number as: \""+cResponse+"\" \n\n Tab out of the field to register the card number",3);}
}
}
t.value = cResponse; // places the data from the dialog to the target field
```

Notice how the outer `else` conditional statement does its own conditional check of `if/else` to make sure that there are 16 characters inside the field. If there are not, the alert will remind you to type without dashes or that there are not 16 characters typed in and the field will go blank. For your own project, you may decide to adjust the message. Otherwise, if there are 16 characters, they will be confirmed in the field upon tabbing out if it.

### Customer’s Company Name

This code is similar to the customer’s Full Name field and is added to the Actions tab as well. Refer to Figure [9-10](#449379_2_En_9_Chapter.xhtml#Fig10). Refer to Figure [9-18](#449379_2_En_9_Chapter.xhtml#Fig18) to see how the alerts should appear.

![](images/449379_2_En_9_Chapter/449379_2_En_9_Fig18_HTML.jpg)

2 screenshots of a warning message from Java Script Window. 1\. It exhibits a text box to enter the company name, followed by an ok button. 2\. It confirms the entered company name and instructs to tab out of the field to register the company name.

Figure 9-18

JavaScript help message for company name and the confirmation message

```
// Company name
var t = event.target; // the target field
var cResponse = app.response({
cQuestion: "Type your Company’s Name",
cTitle: "Company Name",
cDefault: "Company Name not added",
cLabel: "Type your Company name"});
{
if ( cResponse == null){ // if Cancel is selected
app.alert ("You cancelled adding your Company’s name.");
cResponse = "";
}
else{
app.alert("You typed your Company name as: \""+cResponse+"\" \n\n Tab out of the field to register your Company’s name",3);}
}
t.value = cResponse; // places the data from the dialog to the target field
```

For more details on the `app.response` and its use with `app.alert`, you can refer to the following page: [`https://opensource.adobe.com/dc-acrobat-sdk-docs/library/jsapiref/JS_API_AcroJS.html?highlight=response#response`](https://opensource.adobe.com/dc-acrobat-sdk-docs/library/jsapiref/JS_API_AcroJS.html?highlight=response#response)

## How to Duplicate Text for Multiple Fields and Make Fields Read-Only

In certain situations, you may need text that is entered in one field to repeat itself elsewhere in another field, or you want to allow the customer to set a field on their own to read-only. This could include text, a date, or even the result of a calculation. Let’s first look at how text duplication can be accomplished. Refer to the file `TMC_PartOrderForm_Duplicate_End.pdf` and Figure [9-19](#449379_2_En_9_Chapter.xhtml#Fig19).

![](images/449379_2_En_9_Chapter/449379_2_En_9_Fig19_HTML.jpg)

A screenshot of a company's part order form has text boxes for the customer's full name, date, customer order code, and customer's company name. It has a table with 5 columns for part number, description, quantity, unit price, and amount, followed by a summary of the order.

Figure 9-19

A form with fields that can duplicate text in the parent field

In this example, if I type into the field Customers Full Name, other fields that are set to read-only lower on page 1 and page 2 will fill in with whatever text I typed. Sometimes, when you create two text fields with the same name, this can work so that as you type into one, the other will receive the same text. Refer to Figure [9-20](#449379_2_En_9_Chapter.xhtml#Fig20).

![](images/449379_2_En_9_Chapter/449379_2_En_9_Fig20_HTML.jpg)

A screenshot of the text field properties page has 8 tabs, of which the general tab is selected. It has a text box to enter the name, followed by the settings for common properties, including the form field and orientation.

Figure 9-20

Both fields have the same name on the General tab. But in the fields panel they will have slightly different names to differentiate between the two. However, they are now both set to read-only

Creating text fields like this, I find, is never a good idea because it can lead to errors, especially since one field cannot be set to read-only; because they are linked, they will be either both read-only or both not. Refer to Figure [9-20](#449379_2_En_9_Chapter.xhtml#Fig20).

Fields, to make one read-only and leave the other for input, need to have a distinct name. You can do that if you rename a field as I did. I changed one of the fields from Customers Full Name to Customers Full Name#1\. Refer to Figure [9-21](#449379_2_En_9_Chapter.xhtml#Fig21).

![](images/449379_2_En_9_Chapter/449379_2_En_9_Fig21_HTML.jpg)

2 screenshots of a page. Both select the General tab and denotes the section for the customer's full name and common properties. The common properties exhibit the details of form field and orientation. The name in the second screenshot reads, customer's full name hashtag 1.

Figure 9-21

When both fields have distinct names on the General tab, they can operate independently, and you can set one to read-only

However, we still want the text from the field Customers Full Name to flow into Customers Full Name#1.

In this case, for Customers Full Name#1, you can leave the Format setting on the Format tab at None because this is not really a number, but it will have a value. You can then go to the Calculate tab and look at the following code or custom calculation script that was added. Refer to Figure [9-22](#449379_2_En_9_Chapter.xhtml#Fig22).

![](images/449379_2_En_9_Chapter/449379_2_En_9_Fig22_HTML.jpg)

A screenshot of the text field properties page has 8 tabs, of which the calculate tab is selected. The radio button for the custom calculation script is selected. A few lines of code are there to get the customer's full name.

Figure 9-22

Custom calculation script on the Calculate tab

In the JavaScript Editor, I typed the following:

```
event.value = this.getField("Customers Full Name").value;
```

Now the value of the original field will equal the value of the field that is set to read-only, thus preventing the client from typing into linked or duplicate fields.

You can do this as well, for linked dates or even math calculations. And those linked read-only fields can be on other pages as well, such as the field Customers Full Name#2, which also references the same code using the Calculate tab.

You can see this for the fields Date#1 and Date#2; their code references the Date field. Just make sure all the fields are formatted to the same date format.

```
event.value = this.getField("Date").value;
```

Or you could, with the fields Total#1 and Total#2, link to the Total field to repeat that calculation input in real time. In this case, these fields were formatted to Number.

```
event.value = this.getField("Total").value;
```

### Set Fields to Read-Only with JavaScript

Lastly, if you have a field that you want your client to lock or set to read-only with a check box, you can use the code you tried earlier to show and hide fields. I show this with the Customer Order Code field. Refer to Figure [9-23](#449379_2_En_9_Chapter.xhtml#Fig23).

![](images/449379_2_En_9_Chapter/449379_2_En_9_Fig23_HTML.jpg)

A screenshot has 2 sections to enter the customer order code. Both have 16 blocks with the same order code. The order code at the bottom is checked for making the field read-only.

Figure 9-23

Setting a field to read-only with a check box

For the check box Check Box2 on the Actions tab, with the Mouse Up trigger and the Run a JavaScript setting, you can type the following:

```
if(event.target.value !="Off"){
this.getField("Customer Order Code").readonly=true;
}
else{
this.getField("Customer Order Code").readonly=false;
}
```

This again sets a true and false conditional statement. `If` the event target value is not off (on or checked), then it is true, and it is set to read-only. `Else` it is off and therefore false and not read-only, and the client can edit the field.

In your own projects, you could reference more fields in this `if` and `else` condition if you want to set those fields to read-only as well when the check box is enabled. Just make sure to have them set to true and then false.

## Final Thoughts

As you can see, there are several ways you can help your customers fill out forms. Not all fields must offer help. Some are unmistakable and self-explanatory. However, if your client is not familiar with a word or maybe needs a translation because their English is not the best, this might be a possible work-around. If your form is one page, a rollover may be just fine. However, on forms with two pages or more where the help is spread out, I recommend the alert method. While these forms give you a range of alerts, it is important to find the balance that suits your clients best, so you inform them and do not overwhelm them.

Note

Adobe Acrobat now has (since 2017) a Date pop-up menu. You can use the Date Picker when the text field is formatted to Date or you add a Date field. Adding the alert method to the date is optional since when the user chooses a date, it formats itself automatically. In this case, you may choose to alter or remove the alert code altogether if you find it is disrupting the flow of your form. Refer to Figure [9-24](#449379_2_En_9_Chapter.xhtml#Fig24). However, optionally you could instead use this alert if the field is formatted to Time since there is no actual “time picker,” and a hint on the format in that case would be useful.

![](images/449379_2_En_9_Chapter/449379_2_En_9_Fig24_HTML.jpg)

A screenshot has a drop-down menu to select the date from the calendar. It has a pop-up window for the calendar with the date denoting 01, slash, 13, slash, 2023.

Figure 9-24

The Date Picker pop-up menu that appears in Acrobat when you add a Date field

## Summary

In this chapter, you learned about three possible methods for creating help for your clients when they work with text fields. You saw how to alter code for hiding, showing, and resetting fields. You also saw how to reuse document JavaScript in many fields and how you can apply custom formatting to text fields.

Lastly, we looked at how you can duplicate text for multiple fields and make a field read-only with JavaScript.

In the next chapter, you will see what other options you have to alter the text within your fields, depending upon the numbers or user input that is entered.

# 10. Various Types of Formatting with JavaScript

In the previous chapter, you looked at creating various alerts that could assist your clients in knowing when they entered incorrect information.

**Chapter Goal:** In this chapter, you will explore ways that you can globally and individually alter the appearance of various fields so that they conform to a set format you choose. You will also alter the appearance of the resultant value depending on what radio buttons or check boxes are chosen. The following are other topics you will tackle:

*   Creating buttons that allow their label to run onto more than one line

*   Allowing a client’s text in a multiline text field to flow into another text field when there is not enough room in the current field

*   Using JavaScript and a button to print only one page or a range of pages rather than printing the entire document

*   Creating a basic button for submitting a PDF as an email

## Adding Global Formatting to Text Fields

With forms, formatting font or field colors can be accomplished quite easily using the properties on the Appearance and Options tabs. Refer to Figure [10-1](#449379_2_En_10_Chapter.xhtml#Fig1).

![](images/449379_2_En_10_Chapter/449379_2_En_10_Fig1_HTML.png)

2 screenshots for the text field properties. 1 has the appearance tab expanded with details of borders, colors, and text. 2 has the options tab expanded with a drop-down menu for the alignment, a box for the default value, a few checkboxes, and 2 boxes for the limit and combination characters.

Figure 10-1

The Appearance and Options tabs showing formatting choices

These two tabs are available for most fields though the properties within them vary depending on the field. In examples in this chapter, we are discussing text fields. However, much of what you learn here can also be applied to other field types. You will also learn how to access the properties with JavaScript.

On other occasions, you may want all the fields to have the same color or font. You could select all the fields (marquee drag around all fields) and then right-click Properties and set the Appearance tab with various new properties. Refer to Figure [10-2](#449379_2_En_10_Chapter.xhtml#Fig2).

![](images/449379_2_En_10_Chapter/449379_2_En_10_Fig2_HTML.jpg)

A screenshot of a page for text field properties has details of the borders and colors including border color, line thickness, fill color, and line style, and details of the text, including font size, text color, and font. The right side of the page has 7 tabs highlighted.

Figure 10-2

The Appearance tab with multiple fields highlighted at the same time

However, this might lead to errors, and you might not get the result you intended, especially if you forgot to select something.

Note

If you want to work along in this lesson or review the final result, download the Chapter [10](#449379_2_En_10_Chapter.xhtml) files from the link in the introduction. The file with the label “Start” is the file without the code, and the file with the label “End” is the final result. You will also find a folder containing the original scripts if you would like to add them to your own PDF forms.

If you are creating your form from an original PDF, refer to the “Forms Review” section in Chapter [1](#449379_2_En_1_Chapter.xhtml).

Remember that to view the properties of a field, you must select the Prepare Form tool; only then can you right-click or double-click a field to review its properties.

Using JavaScript, you can change the color quickly. To do things globally, you need to go into your document JavaScripts in the JavaScript tool. See the file `TMC_Customer Survey_End_Part2_global.pdf` in this chapter’s file folder. Refer to Figure [10-3](#449379_2_En_10_Chapter.xhtml#Fig3) for the formatting code.

![](images/449379_2_En_10_Chapter/449379_2_En_10_Fig3_HTML.jpg)

A screenshot of the Java script page has a pop-up window for document Java Scripts. It has details of the script name, fields red, and options of close, add, edit, and delete, followed by a few lines of code to set the font and size of text fields or another field format globally.

Figure 10-3

The JavaScript tool and the Document JavaScripts dialog box

Here is a JavaScript called `fieldsRed`. I added the following code:

```
// Script to set the font and size of text fields or another field format globally
for (var i = 0; i < numFields; i++) {
// Get a reference to the current field
var f = getField(getNthFieldName(i));
// If it’s a text field, set the font and font size or another format like stroke color
if (f.type == "text") {
f.strokeColor = color.red;
}
}
```

To find just the text fields and apply a stroke color of red to them, use a JavaScript `for()` loop. Refer to Figure [10-4](#449379_2_En_10_Chapter.xhtml#Fig4) to see the result.

![](images/449379_2_En_10_Chapter/449379_2_En_10_Fig4_HTML.jpg)

A screenshot of the result page has 2 text boxes for the customer's full name and date.

Figure 10-4

The result on the fields of adding the for() loop to the code

Loops are not seen very often in Acrobat JavaScript unless you want to affect multiple fields. The most commonly used is the `for()` loop. There are other loops used in JavaScript, like `for in`, `for of`, `while`, and `do while`. For the sake of simplification, let’s just focus on the `for()` loop, as shown in the `fieldsRed` code.

This `for()` loop contains three statements.

```
"var i = 0; i < numFields; i++"
```

The variable `var i = 0;` is executed before the loop (the code block) starts. The 0 field, like the array example earlier in Chapter [9](#449379_2_En_9_Chapter.xhtml), is the starting point.

`i < numFields;` defines the condition for running the loop (the code block). `var i` is less than (<) `numFields`. Your form may be very large. You don’t know how many fields there are, so you use `numFields` instead of an actual number. You want the loop to keep on going until you reach the end of the form.

`i++` is executed each time after the loop (the code block) has been executed. This keeps the loop going until the end is reached.

This middle section acts as the control:

```
var f = getField(getNthFieldName(i));
// If it’s a text field, set the font and font size or another format like stroke color
if (f.type == "text") {
f.strokeColor = color.red;
}
```

When the number of fields has been collected, you check using a conditional `if` statement that they are indeed strictly text fields. If they are, then a stoke color of red can be applied to the field. Notice in the document how the button fields didn’t have a red stroke applied to them; they were canceled out during the loop.

To format other fields instead of text, format these lines:

*   `if (f.type == "checkbox")`

*   `if (f.type == "radiobutton")`

*   `if (f.type == "button")`

*   `if (f.type == "signature")`

*   `if (f.type == "combobox")`

*   `if (f.type == "listbox")`

Or with this loop, if you want to change the stroke color to blue, you can change this line:

```
f.strokeColor = color.blue;
```

All the fields that had a red stroke will become blue instantly, and you didn’t have to worry about selecting them all.

## Color Properties

Other colors you could try are defined in Table [10-1](#449379_2_En_10_Chapter.xhtml#Tab1). For the color spaces, T= Transparent, G= Grayscale, RGB=Red, Green, Blue, and CMYK= Cyan Magenta, Yellow, Black. 0=0% and 1 = 100% of the color.

Table 10-1

Color Properties

| Color Object | Keyword | Equivalent JavaScript |
| --- | --- | --- |
| Transparent | `color.transparent` | `[ "T" ]` |
| Black | `color.black` | `[ "G", 0 ] or [ "RGB", 0,0,0 ] or[ "CMYK", 0,0,0,1 ]` |
| White | `color.white` | `[ "G", 1 ] or [ "RGB", 1,1,1]` |
| Red | `color.red` | `[ "RGB", 1,0,0 ]` |
| Green | `color.green` | `[ "RGB", 0,1,0 ]` |
| Blue | `color.blue` | `[ "RGB", 0,0,1 ]` |
| Cyan | `color.cyan` | `[ "CMYK", 1,0,0,0 ]` |
| Magenta | `color.magenta` | `[ "CMYK", 0,1,0,0 ]` |
| Yellow | `color.yellow` | `[ "CMYK", 0,0,1,0 ]` |
| Dark Gray | `color.dkGray` | `[ "G", 0.25 ]` |
| Gray | `color.gray` | `[ "G", 0.5 ]` |
| Light Gray | `color.ltGray` | `[ "G", 0.75 ]` |

Some colors like orange or purple might not have a direct keyword that you can use. In that case, you need to figure out your equivalent JavaScript. For example, for orange, you could use [ “CMYK”, 0,0.5,1,0 ] and in this case set Magenta to 50% and Yellow to 100%. For purple you could write [ “CMYK”, 0.60,0.80,0,0 ], setting Cyan to 60% and Magenta to 80% or try to figure out an “RGB” equivalent using the numbers in a similar way. Keep in mind that for Grayscale, or “G,” lighter colors have a higher number.

## Array and Variable Examples of Formatting Properties

Both fields and annotations (comments) can be affected in various ways by being able to customize the properties globally for a field.

For that example, in the text in an array in Chapter [9](#449379_2_En_9_Chapter.xhtml) you might break down the property you want to target within the PDF document.

```
var gHelpText = new Array();
//spans
//String#0
gHelpText[0] = new Object();
gHelpText[0].text = "What's your full name?";
gHelpText[0].textColor = color.blue;
gHelpText[0].textFont = font.Times;
gHelpText[0].textSize = 16;
gHelpText[0].alignment = "center";
```

However, it would be difficult to have that formatting appear and then be relevant to other parts of the array because the formatting would not pass into the field or fields as, for example, a color, only as text string.

In this case, it would be better to add most of these new properties to the field txtHelp on the Format tab as a custom format targeting itself. Refer to Figure [10-5](#449379_2_En_10_Chapter.xhtml#Fig5). Refer to the file `TMC_OrderForm_End_rollover_format.pdf`.

![](images/449379_2_En_10_Chapter/449379_2_En_10_Fig5_HTML.jpg)

A screenshot of a window for the text field properties has the format tab expanded. It has a drop-down menu for the format category, a list of custom format scripts, and custom keystroke scripts. It has a box with a border at the bottom of the page with text, what is your full name question mark.

Figure 10-5

Text field Properties dialog box’s Format tab and the custom format script

With JavaScript code you can control the following properties for this field: the fill, stroke, border style, and stroke width, as well as the font properties of color, text font, or font family, as well as size and alignment.

```
//Field Fill
event.target.fillColor = color.gray;
event.target.strokeColor = color.red;
event.target.borderStyle = border.b;
event.target.lineWidth = 2;
//Fonts
event.target.textColor = color.white;
event.target.textFont =font.Times;
//fontFamily for Rich Text only
event.target.textSize = 14;
//Rich Text
event.target.richText = true;
event.target.alignment = "center";
```

However, when you want to control more than one field with formatting, then you would come back to adjusting the document JavaScript using the `for ()` loop again to format the fields or, in this case, adding these to the variable `f` as properties.

Here we can see how it could be added to the loop and the result. Refer to Figure [10-6](#449379_2_En_10_Chapter.xhtml#Fig6) and to the file `TMC_Customer Survey_End_Part2_global_format2.pdf`. Again, I use my document JavaScript called fieldsRed but added to the script some more formatting.

![](images/449379_2_En_10_Chapter/449379_2_En_10_Fig6_HTML.jpg)

A screenshot of the document Java Scripts has details of the script name, fields red, and a close tab on the right side of the page. It has a few lines of code to set the font and size of text fields and other formatting, followed by fields, customers' full name as John Smith, and date.

Figure 10-6

The JavaScript tool and the Document JavaScripts dialog box and the fields

```
// Script to set the font and size of text fields and other formatting
for (var i = 0; i < numFields; i++) {
// Get a reference to the current field
var f = getField(getNthFieldName(i));
// If it's a text field, set the font and font size and other formatting
if (f.type == "text") {
//Field Fill
f.fillColor = color.gray;
f.strokeColor = color.red;
f.borderStyle = border.b;
f.lineWidth = 2;
//Fonts
f.textColor = color.white;
f.textFont = font.Times;
//f.fontFamily = ["Times","monospace"];
f.textSize = 14;
f.richText = true;
f.alignment = "center"
}
}
```

Note

We will look at other examples later in this chapter of formatting and break down certain properties in JavaScript, including what rich-text formatting is.

## Multiline Buttons

Another type of formatting that you may want to add to a form is having the text of buttons on two or more lines. Buttons without this type of formatting often have the text on one line when you type the label in the Options panel. There is no automatic way to tell the text to spread onto two lines, as you can see in Figure [10-7](#449379_2_En_10_Chapter.xhtml#Fig7).

![](images/449379_2_En_10_Chapter/449379_2_En_10_Fig7_HTML.jpg)

A screenshot of a text box with a text that reads reset comments.

Figure 10-7

A reset button with the text on one line

The button becomes longer and longer. An option is to make the text smaller. Unfortunately, it then becomes difficult to read. You could create an icon graphic, but then the text won’t be editable if you need to change it quickly. Refer to Figure [10-8](#449379_2_En_10_Chapter.xhtml#Fig8) to see the current properties of the button MyButton, as found in the file `TMC_Customer Survey_Start_Part2_global.pdf`.

![](images/449379_2_En_10_Chapter/449379_2_En_10_Fig8_HTML.jpg)

2 screenshots for the button properties. 1 has the appearance tab expanded with details of borders and colors, and the text, including color, thickness, style, size, and font. 2 has the options tab expanded with a drop-down menu for layout and behavior, and details of icon and label.

Figure 10-8

Button Properties dialog box’s Appearance and Options tabs with the label of Reset Comments

Once again JavaScript has a solution. Refer to Figure [10-9](#449379_2_En_10_Chapter.xhtml#Fig9) and the file `TMC_Customer Survey_End_Part2_global.pdf`.

![](images/449379_2_En_10_Chapter/449379_2_En_10_Fig9_HTML.jpg)

A screenshot has button properties with the actions tab expanded. It has a drop-down menu to select a trigger and action, followed by a list of actions and a text box for reset comments. It has a pop-up window for Java Script Editor with a set of 2-line code to create and edit Java scripts.

Figure 10-9

Running a JavaScript to make the text multiline on the button

Besides resetting the comment field using the action “Reset a form,” this adds a JavaScript to the button on the Mouse Up trigger.

Here is the code in the editor:

```
this.getField("MyButton").buttonSetCaption("Reset\n Comments");
```

Just one line does what you need. Call upon the name of the button, MyButton, on the General tab, and tell it that you want to set a caption the same as the label on the Options tab. To make the words into two lines, use the invisible character, `" \n"`. Another name for this is a *metacharacter*. Basically, this means a new line, so the word *Comments* goes onto the second line. This is handy when the button requires several lines of text.

Note

You may have noticed that you used the metacharacter `\n` in Chapter [9](#449379_2_En_9_Chapter.xhtml) when you were creating alert boxes and needed line breaks. In Chapter [11](#449379_2_En_11_Chapter.xhtml), you will look at some regular expressions where metacharacters can be used with other patterns.

## Multiline Text

Buttons aren’t the only fields that require multiple lines. Text fields that require a lot of text such as comments can be made multiline as well. If you’ve reviewed your form basics, you’ll know that you can make a text field multiline simply by selecting the multiline check box on the Options tab. Refer to Figure [10-10](#449379_2_En_10_Chapter.xhtml#Fig10) to see an example of this.

![](images/449379_2_En_10_Chapter/449379_2_En_10_Fig10_HTML.jpg)

A screenshot of the text field properties with the expanded options tab. It has a drop-down menu for alignment and checkboxes for spelling and multi-line. It has 2 small text boxes with text, this text box is too small, followed by 3 checkboxes for spelling, multi-line, and scroll-long text.

Figure 10-10

On the Options tab, check the Multi-line box so you can type more text and then select “Scroll long text” to get a scroll box

When creating a text comments field, a plus symbol appears at the end of the field once it’s filled with type. Refer to Figure [10-10](#449379_2_En_10_Chapter.xhtml#Fig10).

With comments, if you want to read them on your computer, you might check the “Scroll long text” box.

While this makes it easier to read on the computer, when you go to print, you may not see all of the text that was in the box.

Unfortunately, Acrobat does not have expandable text field boxes. This is a problem if your form requires that a client write a lengthy statement and you have only a small amount of room at the end of the page.

Here is a workaround: create a second page and allow the text to flow into a second comment text field.

Rather than using the Actions tab this time, let’s add a custom format on the Format tab. Refer to Figure [10-11](#449379_2_En_10_Chapter.xhtml#Fig11).

![](images/449379_2_En_10_Chapter/449379_2_En_10_Fig11_HTML.jpg)

A screenshot of the text field properties has the format tab highlighted. It has a drop-down menu to select the format category, followed by 2 boxes for custom format script and custom keystroke script. Each box has an edit tab on the right side of the page.

Figure 10-11

The Custom Keystroke Script settings applied

You can see an example of this in the file `TMC_Customer Survey_End_Part2_global.pdf`.

On page 1, in the first field, Comments1, which already had “Check spelling” and Multi-line enabled on the Options tab but “Scroll long text” disabled (Figure [10-10](#449379_2_En_10_Chapter.xhtml#Fig10)), I put the following code into the Format tab (Figure [10-11](#449379_2_En_10_Chapter.xhtml#Fig11)):

**Custom Format Script**

```
event.target.textColor = ["RGB",0,0,1];
```

I did not have to write this line, but I wanted to show how you can adjust the color of text, as you saw in the help text example earlier. I want this `textColor` to be different than the other fields. I could have just as easily set it on the Appearance tab since it is for only one field. Refer to Figure [10-12](#449379_2_En_10_Chapter.xhtml#Fig12).

![](images/449379_2_En_10_Chapter/449379_2_En_10_Fig12_HTML.jpg)

A screenshot of a text tab has a drop-down menu for the font size selected as 12, a box for the text color selected as blue, and a drop-down menu for the font selected as Helvetica.

Figure 10-12

The Appearance tab showing the set font size, text color, and font

However, later in this chapter and Chapter [11](#449379_2_En_11_Chapter.xhtml), you will see more complex examples where this is not always what you want to do; there, formatting color with JavaScript is a better option.

**The Custom Keystroke Script**

```
if ( event.fieldFull || event.willCommit )
this.getField("Comments2").setFocus();
```

This code allows the user to type in the field. When the script senses that the text field is full or will commit, it moves into the next text box field named Comments2\. The willCommit verifies the current keystroke event before the data is committed. See [h`ttps://opensource.adobe.com/dc-acrobat-sdk-docs/library/jsapiref/JS_API_AcroJS.html#willcommit`](https://opensource.adobe.com/dc-acrobat-sdk-docs/library/jsapiref/JS_API_AcroJS.html#willcommit).

Here again, as in Chapter [6](#449379_2_En_6_Chapter.xhtml), we see another use for `setFocus(),` this time to move the focus into a new field.

If you need a third box for some reason, you can add the following lines in the Comments2 box for the Format Custom Keystroke Script:

```
//Second to 3rd box if required
var t2=this.getField("Comments2");
if (event.fieldFull)
{
this.getField("Comments3").setFocus();
}
```

The variable in this example, `t2`, is optional to write as we are working in the field Comments2 or in this case the “event,” but when working in many fields, you may want to reference it to keep track of which one you are currently in or just make a comment (//).

The final field, Comments3, does not require any JavaScript, and the Format tab can be set to None. Refer to Figure [10-13](#449379_2_En_10_Chapter.xhtml#Fig13).

![](images/449379_2_En_10_Chapter/449379_2_En_10_Fig13_HTML.jpg)

A screenshot of the text field properties page has tabs for general, appearance, position, options, actions, format, validate, and calculate, with the format tab highlighted. It has a drop-down menu to select the format category, with none selected.

Figure 10-13

In the Format tab, the category is set to None

You could then alter your Reset Comments button on the Actions tab to reset all three fields.

This example is a good workaround. One note of caution: only use this flow method for typing directly into the field. I found that pasting text in does not always flow into the second and subsequent boxes. In other situations, you may prefer the client to type into a comment (sticky note) and then print the comment later separately.

## Complex Formatting Using Check Boxes and a Text Field

In Figure [10-14](#449379_2_En_10_Chapter.xhtml#Fig14) you can see how check boxes can be used for addition and subtraction ratings. Like with radio buttons, you don’t need JavaScript to do this. See the file `TMC_Customer Survey_End_Part2b.pdf` on page 1.

![](images/449379_2_En_10_Chapter/449379_2_En_10_Fig14_HTML.jpg)

A screenshot of a page has 12 check boxes for words that describe the overall feeling of the completed project. It has 1 option checked that reads, I did not have a positive experience. The right side has 2 highlighted boxes for the final result, one with a value of 0 and the other with negative 6.

Figure 10-14

A survey where the font style and color of the box changes depending upon the rating

All you need to do is give the check boxes a negative or positive value on the Options tab. Refer to Figure [10-15](#449379_2_En_10_Chapter.xhtml#Fig15).

![](images/449379_2_En_10_Chapter/449379_2_En_10_Fig15_HTML.jpg)

2 screenshots of the check box properties page have tabs, general, appearance, position, options, and actions. 1 and 2 have the options tab highlighted with a drop-down menu for the checkbox style and a box for the export value, which is 1 in the first and negative 6 in the second.

Figure 10-15

Check box properties on the Options tab set to positive and negative export values

On the Calculate tab of the text field Final Result_4, you can use the value of sum (+) to add them all up. Refer to Figure [10-16](#449379_2_En_10_Chapter.xhtml#Fig16).

![](images/449379_2_En_10_Chapter/449379_2_En_10_Fig16_HTML.jpg)

A screenshot of a page has a text box for the final result with a value of 0\. It has a window for the text field properties with 8 tabs and the calculate tab highlighted. It has 2 options, with the second option for the value selected, as the sum of some fields.

Figure 10-16

Set the value to sum (+) on the Calculate tab in the Text Field Properties window

However, sometimes you may want to alert yourself to the fact that the rating is becoming negative.

You can format your appearance of the box field and text once on the Appearance tab without JavaScript. Unfortunately, if the number changes from positive to negative, its appearance will not change without adding a conditional script into the Validate tab section. Refer to Figure [10-17](#449379_2_En_10_Chapter.xhtml#Fig17).

![](images/449379_2_En_10_Chapter/449379_2_En_10_Fig17_HTML.jpg)

A screenshot of the text field properties page has tabs, general, appearance, position, options, actions, format, validate, and calculate, with the validate tab highlighted. It has 2 options for the validation and range of the field value and a few lines of code for the custom validation script.

Figure 10-17

The Validate tab with a custom validation script

Here is the code:

```
//get negative number
if (event.value > 0){
//for text color.black
event.target.textColor =["RGB",0,0,0];
// textFont
event.target.textFont = font.Times;
//Size
event.target.textSize = 11;
//for field color.none
event.target.strokeColor = ["T"];
event.target.fillColor = color.transparent;
//left, center, right
event.target.alignment ="left";
}
else{
event.target.textColor = ["RGB",1,0,0];
// textFont
event.target.textFont = font.Helv;
//Size
event.target.textSize = 14;
//for field color.red
event.target.strokeColor = ["RGB",1,0,0];
//fill of field
event.target.fillColor = color.yellow;
// Borders 0=none, 1=thin, 2=medium, 3 = thick
event.target.lineWidth = 2;
// Border Style border.s= solid, border.b= beveled, border.d = dashed, border.i = inset, border.u = underline
event.target.borderStyle = border.d;
//left, center, right
event.target.alignment ="center";
}
```

While this might seem complex at first glance, in reality it is not. You are merely changing your formatting as the number changes. Refer to Figure [10-18](#449379_2_En_10_Chapter.xhtml#Fig18).

![](images/449379_2_En_10_Chapter/449379_2_En_10_Fig18_HTML.jpg)

A screenshot of a page has text, final result 1, followed by a box for the final result, with value as negative 6\. The box has a dotted border with highlighted text.

Figure 10-18

The text and field formatting changes based on the number value

Here is a simplified breakdown:

```
if (event.value > 0){
// Change my formatting
}
else {
//Change my formatting to something else
}
```

When the number is greater than (>) 0, please change the format of the fonts, color, and field to something defined. When it is less than (<) 0, change it to something else defined.

The format properties that can be affected in text field with examples are as follows:

**Fill color** or `fillColor`: The fill of the field

```
event.target.fillColor = color.transparent;
event.target.fillColor = color.yellow;
```

**Stroke color** or `strokeColor`: The stroke around the field

```
event.target.strokeColor = ["T"]; // transparent
event.target.strokeColor = ["RGB",1,0,0]; // red
```

**Border width** or `lineWidth`: The width of the field

```
// Borders 0=none, 1=thin, 2=medium, 3 = thick
event.target.lineWidth =2;
```

**Border style** or `borderStyle`: The style of the border field

```
// Border Style border.s= solid, border.b= beveled, border.d = dashed, border.i = inset, border.u = underline
event.target.borderStyle = border.d;
```

For text you can then set the following properties.

**Text color** or `textColor`: This affects the color of the text. You could say `color.black` or `["RGB",0,0,0];`.

```
event.target.textColor = ["RGB",0,0,0];
event.target.textColor = ["RGB",1,0,0];
//this is a  red color
```

**Text font** or `textFont`:

```
event.target.textFont =font.Times;
event.target.textFont =font.Helv;
```

Note

For bold italic, italic, or bold font styles, use this:

`event.target.textFont=font.TimesBI;` or `font.HelvBI;`

`event.target.textFont=font.TimesI;` or `font.HelvI;`

`event.target.textFont=font.TimesB;` or `font.HelvB;`

Others are `font.Times` for Times`, font.Cour` for Courier`, font.Symbol` for the Symbol font, and `font.ZapfD` for Zapf Dingbats. Some of these have bold, italic, or bold italic styles when adding the B, I, or BI on the end of the name.

Likewise, you can acquire the font with a font family if you know the exact name. For example, in the context of rich text, you would write like this in order to allow the font to change based on the font available on the user’s computer. In this example, once the code is typed in the document JavaScript, you must have Allow Rich Text Formatting checked on the Options tab, as you will see in Figure [10-19](#449379_2_En_10_Chapter.xhtml#Fig19).

```
f = this.getField("Text1");
style = f.defaultStyle;
// If Times is not found on the user’s system, use a monospace font. The field must be set for Rich Text.
style.fontFamily = ["Times", "monospace" ];
f.defaultStyle = style;
```

**Text size** or `textSize`: To change the size of the text

```
event.target.textSize = 11;
event.target.textSize = 14;
```

**Text alignment** or the alignment within the field

```
//left, center, right
event.target.alignment ="left";
event.target.alignment ="center";
```

You don’t have to use all of these in one field. However, with `if/else` conditionals, you need to make sure you have in most cases an on or off state (when `if` formats like this, then `else` formats like that).

Note you can use this same code to affect radio buttons with values that are entered into a text box, as I did using the field Final Result_3\. I just altered the colors slightly. Refer to the file `TMC_Customer Survey_End_Part2b.pdf` on page 1.

Remember, if you are setting a global variable, as in an earlier example, in the `for()` loop, you do not have to write `event.target`. Instead, after you created the variable, you could then apply the properties such as this:

```
f.alignment ="center";
```

## Rich Text Formatting for Text Fields and Comments

There are other formats as well, but these fall into the category of rich formatting even if you have “Rich formatting” in the text field checked, or you set it in the JavaScript, like so:

```
event.target.richText =true;
```

Some other formatting you can use with some PDF tools, such as annotations or comments, but not form fields without some additional scripting, are the following:

**Font family** or `fontFamily`: Can be used with text fields with rich text, but it also can be used for an array (many fonts listed) when you are trying to make sure that the user font will display similar on all computers as shown earlier. It can also be used for text found in annotations.

```
style.fontFamily = ["Times", "monospace" ];
```

**Font style** or `fontStyle`: There are two styles: normal and italic.

```
example[0].fontStyle = "italic";
```

**Font stretch** or `fontStretch`: There are several styles to choose from: normal (default), ultra-condensed, extra-condensed, condensed, semi-condensed, semi-expanded, expanded, extra-expanded, and ultra-expanded.

```
example[0].fontStretch= "expanded";
```

**Font weight** or `fontWeight`: This takes over for bold fonts. Normal is anything under 700, and bold is anything over 700\. The options are 100, 200, 300, 400, 500, 600, 700, 800, and 900\. The default is 400\. It seems to work best in combination with Font Stretch.

```
example[0].fontWeight= 400;
```

`strikethrough`**:** Draws a line through the words. It can be either true or false.

```
example[0].strikethrough=true;
example[0].strikethrough=false;
```

`subscript`: The text is subscripted, reduced point size, and lower baseline.

```
example[0].subscript=true;
example[0].subscript=false;
```

`superscript`: The text is superscripted, lowered point size, and raised baseline.

```
example[0].superscript=true;
example[0].superscript=false;
```

`underline`: The text is underlined.

```
example[0].underline=true;
example[0].underline=false;
```

`rotation`: This is used to rotate the field or the text orientation within the field. This is not used that often. Make sure that you test it in Reader first before you try this with a client, as it might have unexpected results. Numbers you might use are 0, 45, 90, 180, or 270 depending on the rotation. In a text field if you need to rotate one field, use the setting on the General tab called Orientation.

```
event.target.rotation=0;
event.target.rotation=90;
```

However, there is a way around this rich formatting with a bit of extra code and an array in a text field.

For a text field, you can set Allow Rich Text Formatting in the text field’s Options tab. Refer to Figure [10-19](#449379_2_En_10_Chapter.xhtml#Fig19).

![](images/449379_2_En_10_Chapter/449379_2_En_10_Fig19_HTML.jpg)

A screenshot of the text field properties has 8 tabs, with the options tab highlighted. It has a drop-down menu for the alignment, a box for default value, and a few checkboxes, of which spelling check and rich text formatting are selected.

Figure 10-19

Set the option to allow rich-text formatting using the Options tab

Then on your Format tab, in the Custom Format Script field, add the following JavaScript to get the rich value. Refer to Figure [10-20](#449379_2_En_10_Chapter.xhtml#Fig20).

![](images/449379_2_En_10_Chapter/449379_2_En_10_Fig20_HTML.jpg)

A screenshot of the text field properties has tabs, general, appearance, position, options, actions, format, validate, and calculate, with the format tab highlighted. It has a drop-down menu for format category and options for custom format script, followed by a text box with text, rich format.

Figure 10-20

Custom formatting for the rich text using the Format tab

```
var str = event.value;
if(event.value){
var RichFormat = new Array();
RichFormat [0] = new Object();
RichFormat [0].text = str;
RichFormat [0].fontStyle = "italic";
event.richValue = RichFormat;
}
else{
event.value ="";
}
```

Now on your own try replacing the code fontStyle with other options that you could only create with rich text, such as Underline or Strike Through.

Or try to affect parts of the text by adding to the array.

```
var str = event.value;
if(event.value){
var RichFormat = new Array();
RichFormat [0] = new Object();
RichFormat [0].text = "$";
RichFormat [1] = new Object();
RichFormat [1].text = str + "$";
RichFormat [1].fontStyle = "italic";
event.richValue = RichFormat;
}
else{
event.value ="";
}
```

You can refer to my file for examples, `Rich_Text_example.pdf` and `example2.pdf`.

### Comment Example

You can return at this point to Chapter [8](#449379_2_En_8_Chapter.xhtml) and work on the comment and time stamp example. Much of what you have learned here can be applied to the time stamp, which was a Text field, and you can review the properties formatting that I used on the Actions tab of the button. Refer to my file in this chapter called `Project_Comment_End_format2.pdf`. See page 2.

However, for the comment or annotation on the button on the Actions tab, you could on the Mouse Up trigger update the following Run a JavaScript code, which would add and change the default icon as well as add color, alignment, and some rich-text formatting to preset text in the comment.

```
var annot = this.addAnnot({
page: this.pageNum,
type: "Text",
author: "John Smith",
point: [72,500],
popupRect: [72, 500,6*72,500-2*72],
popupOpen: true,
noteIcon: "Help",
strokeColor: color.yellow,
strokeFill: color.red
});
var spans = new Array();
spans[0] = new Object();
spans[0].text = "You're doing a great job!\r";
spans[0].textColor = color.blue;
spans[0].textSize = 18;
spans[0].textFont = font.Helv;
spans[0].alignment = "left";
spans[1] = new Object();
spans[1].text = "You have learned a lot!\r";
spans[1].textColor = color.red;
spans[1].textSize = 20;
spans[1].alignment = "center";
spans[2] = new Object();
spans[2].text = "This is the end of the comment.";
spans[2].textColor = color.green;
spans[2].fontStyle = "italic";
spans[2].underline = true;
spans[2].alignment = "right";
// Now give the rich field a rich value
annot.richContents = spans;
```

Refer to Figure [10-21](#449379_2_En_10_Chapter.xhtml#Fig21) for how this appears in the comment pane. The code `\r` in the text was used to format the text to the next line. Note that green is not the most legible on some backgrounds, so that is something to keep in mind when using colored text and dealing with clients with sight issues such as color blindness or limited vision. Note that sometimes right alignment text will return to left alignment in the comment after the comment has been moved or compressed.

![](images/449379_2_En_10_Chapter/449379_2_En_10_Fig21_HTML.jpg)

A screenshot of a comment page has a box with text to add a comment. It has comments with 3 lines of differently colored text from John Smith at 10 hours 31 minutes that read, you are doing a great job, you have learned a lot, and an underlined, this is the end of the comment.

Figure 10-21

Custom formatting applied to a comment

You can find other similar annotation examples here:

[`https://opensource.adobe.com/dc-acrobat-sdk-docs/library/jsapiref/JS_API_AcroJS.html#annotation`](https://opensource.adobe.com/dc-acrobat-sdk-docs/library/jsapiref/JS_API_AcroJS.html#annotation)

### Final Thoughts on Formatting with Text

As a final note on formatting with `textFont`, an alternate way of writing the line

```
event.target.textFont=font.Helv;
```

is to put the actual name in quote marks, like this:

```
event.target.textFont="Helvetica";
```

`Here`, the style is added with a dash:

```
event.target.textFont="Helvetica-Bold";
```

Another name for italic is oblique and a style can be written like this:

```
event.target.textFont="Helvetica-Oblique";
```

Two styles are often fused together:

```
event.target.textFont="Helvetica-BoldOblique";
```

Here is a better method for uncommon names:

```
event.target.textFont="MinionPro-Regular";
```

The following gives you access to less traditional styles:

```
event.target.textFont="Arial-Black";
event.target.textFont="ArialNarrow";
```

Not all styles use the dash. Refer to your computer’s control panel area and search *Fonts* for the correct font names. For example, MinionPro-Italic is written as MinionPro-It. You can discover this when you double-click the font to preview it and locate the font name in the upper left of the preview box. Refer to Figure [10-22](#449379_2_En_10_Chapter.xhtml#Fig22).

![](images/449379_2_En_10_Chapter/449379_2_En_10_Fig22_HTML.jpg)

A screenshot of a preview box has, Minion Pro It, open type, at the top, with an A letter font on its left. It is followed by 2 options of print and install, of which the print option is selected. The bottom of the page has a line of text that reads, font name, Minion Pro.

Figure 10-22

Know the exact name of your font before you use it in code

When you enter an incorrect name, you will not get the font or font style you require. Also, be aware that your client’s computer may not have all specialized fonts, so stick to safe names such as Times, Courier, Arial, or Helvetica to avoid text field font issues.

Refer to Chapter [16](#449379_2_En_16_Chapter.xhtml) for a further discussion on the use of `event.target`.

## Silent Printing

What kind of JavaScript is required to print a single page in a document or a range of pages using a Print button in a form?

Refer to Figure [10-23](#449379_2_En_10_Chapter.xhtml#Fig23) to review how to print all pages in a document with a button. Refer to Chapter [2](#449379_2_En_2_Chapter.xhtml) as well to review.

![](images/449379_2_En_10_Chapter/449379_2_En_10_Fig23_HTML.jpg)

A screenshot of the button properties page has 5 tabs, general, appearance, position, options, and actions, of which the actions tab is selected. It has a section to add an action, with drop-down menus to select a trigger and an action, followed by a list of actions.

Figure 10-23

The “Execute a menu item” action on the Actions tab. Select File ➤ Print to print the document

Another feature about this form, in file `TMC_Customer Survey_End_Part2b.pdf`, is silent printing. In most cases, when you want to print a document in Acrobat, you just go to the menu and choose File ➤ Print. As shown in Chapter [2](#449379_2_En_2_Chapter.xhtml), you can do this with a button and the Actions tab. However, this action prints the whole document, which may not be what you want to do. Maybe you want to print out just the form page, which may be one page or a range. Refer to Figure [10-24](#449379_2_En_10_Chapter.xhtml#Fig24).

![](images/449379_2_En_10_Chapter/449379_2_En_10_Fig24_HTML.jpg)

A screenshot of the button properties page has 5 tabs, general, appearance, position, options, and actions, of which the actions tab is selected. It has a section to add an action, with drop-down menus to select a trigger and an action, of which run a Java Script action is selected.

Figure 10-24

Run a JavaScript using a button and the Actions tab to print specific pages

Here is the code you can use to run an action on the Actions tab of the button:

```
// Print current page silently
this.print(false, this.pageNum, this.pageNum);
```

When you add the code to a button, only the page that this button is on will print.

If the user gets the message shown in Figure [10-25](#449379_2_En_10_Chapter.xhtml#Fig25), they just click the Yes button and continue or click No to cancel the print.

![](images/449379_2_En_10_Chapter/449379_2_En_10_Fig25_HTML.jpg)

A screenshot of a message window from Adobe Acrobat. It has a caution message that reads, this document is trying to print, do you want to allow this, followed by 2 options of yes and no, of which yes is selected.

Figure 10-25

Acrobat Alert message that appears regarding printing when the button is clicked

When you need to print this form page and the ones below it, change the code to this:

```
// Print current page silently and the range below it
this.print(false, this.pageNum, this.pageNum+1);
```

If there are more pages in the range, `this.pageNum+1` becomes `this.pageNum+2` and so on. Just make sure that the number of current pages below the button match the range you set or you will get an error when your attempt to print.

Once again, this is like an array:

```
this.pageNum (0) = page 1
this.pageNum+1 = page 2
this.pageNum+2 = page 3
```

and so on.

Note

Be aware that when using this code your client may have their default printer set to Adobe PDF. If that is the case, they will be taken to the Save file as a PDF dialog box and be able to save the file as a PDF to print later using File ➤ Print. If not, they will go directly to the printer. An alternative is to change your setting in the code from false to true. This will allow the Print dialog box to open, and the user can reset the printer and review the page range.

## Options for Submit Button Using Email

One final topic we will discuss in this chapter is how to add a submit button to an Acrobat form for email purposes if you or your clients are comfortable with using Adobe Acrobat to submit a form. Refer to Figure [10-26](#449379_2_En_10_Chapter.xhtml#Fig26). You can see an example of this button in the file `TMC_OrderForm_End_rollover_format_2.pdf`.

![](images/449379_2_En_10_Chapter/449379_2_En_10_Fig26_HTML.jpg)

A screenshot of a highlighted button with a border. It has text, submit, within the button.

Figure 10-26

Send email button called Submit

The following link will also give you some further help on this topic on how to write the code. Refer to the “Doc Methods of mailDoc” section.

[`https://opensource.adobe.com/dc-acrobat-sdk-docs/library/jsapiref/doc.html`](https://opensource.adobe.com/dc-acrobat-sdk-docs/library/jsapiref/doc.html)

I will just point out the basic starting code for such a button. You can also refer to my file, which you will need to replace certain lines with your own code.

In this case, for the submit button, rather than using the action on the Actions tab of the Submit a Form dialog box, you can run a JavaScript, in this case on the trigger of Mouse up,. Refer to Figure [10-27](#449379_2_En_10_Chapter.xhtml#Fig27).

![](images/449379_2_En_10_Chapter/449379_2_En_10_Fig27_HTML.jpg)

A screenshot of the button properties has the actions tab selected. It has a section to add an action, with drop-down menus to select a trigger and an action, of which run a Java Script action is selected. It has a page for Java Script Editor with a few lines of code to create and edit Java Scripts.

Figure 10-27

Button Properties dialog box’s Actions tab and the code in the JavaScript Editor

Here is the code for `this.mailDoc()` and the message window. You would in this case have to edit the emails if you were going to cc someone, as well as edit the subject line and the message.

```
this.mailDoc({
bUI: false,
cTo: "tmcZZZ@gmail.com",
cCC: "JSmith2289@gmail.com",
cSubject: "My Document",
cMsg: "John, attached is my latest form in PDF."
});
```

Note

According to Adobe, on Windows, the client machine must have its default mail program configured to be MAPI enabled to use this method.

You would then click OK to close the JavaScript Editor, close the Properties dialog box, and close the Prepare Form tool. Save your file and test the button.

When the user clicks the Submit button, a dialog box will pop up that has a heading of Send Email. Refer to Figure [10-28](#449379_2_En_10_Chapter.xhtml#Fig28).

![](images/449379_2_En_10_Chapter/449379_2_En_10_Fig28_HTML.jpg)

A screenshot of the send email page. It has 2 options for sending using, default email application or webmail, of which the first option is selected. There is a checkbox to remember the choice. Continue and cancel are at the bottom of the page, and the continue button is selected.

Figure 10-28

Send Email dialog box

The user then chooses what kind of email for the Send Using setting, either the “Default email application (Microsoft Outlook)” setting or Use Webmail, which could be Gmail or some other account that the user could set up and have their choice remembered. Refer to Figure [10-29](#449379_2_En_10_Chapter.xhtml#Fig29).

![](images/449379_2_En_10_Chapter/449379_2_En_10_Fig29_HTML.jpg)

2 screenshots of a page to add a new Gmail account and a webmail account. 1 has a box to enter the email address, followed by an example, and an okay button. 2 has boxes to enter an email address, password, incoming I M A P, outgoing S M T P, and add button.

Figure 10-29

The user can choose and set their own web account using various dialog boxes

Upon adding an email address and clicking Continue (Figure [10-28](#449379_2_En_10_Chapter.xhtml#Fig28)), in the case of the Gmail account, Adobe Acrobat will then ask you to sign into your account, and then a message will be created in the designated drafts folder of the selected account.

With Microsoft, if your account is set up, the user will see a new message set up with the PDF file attached, as well as subject line and message already set. All they need to do is edit the message and send the PDF file.

Keep in mind that not all clients will want to submit their forms using this procedure if only a few signatures are required in the document. In that case, it may be better to use Adobe Sign to create and distribute e-forms for e-signing and request the signing be online, which may be more efficient if you are dealing with multiple clients and the same form for each person. We will talk about e-signatures a bit more in Chapters [19](#449379_2_En_19_Chapter.xhtml) and [20](#449379_2_En_20_Chapter.xhtml).

## Summary

As you can see, there are many useful ways you can customize forms with JavaScript to suit your needs.

*   Customized text

*   Fields

*   Buttons for adding comments, printing, and emailing forms

As you saw in this chapter, there are many options you can use to format form fields and nonform items. Sometimes custom formatting is used, and other times the Validation tab is a better option. You can add JavaScript globally to affect many fields or use the Actions tab to affect just one. I recommend spending some time reviewing this chapter before moving on to Part III. Add some of this JavaScript to one of your own forms and discover how rich text can alter your fields as well as your comments.

For most of Part II, you focused on adding JavaScript to text fields, buttons, and check boxes. However, as you will see in Part III, PDF forms can be complex once you start adding JavaScript to radio buttons, check boxes, drop-down lists, and list boxes.

# Part IIIWorking with More Complex Forms

# 11. Validation with Text Boxes, Alerts, and Radio Buttons

JavaScript in Part III becomes more complex as multiple form fields work in tandem.

As you’ve seen in past examples, to avoid errors, you need to create warnings or alerts so clients will enter the correct information.

**Chapter Goal:** Text boxes are not the only field item that alerts or choices can be applied to. You can also use radio buttons. In this chapter, you’ll learn how.

Note

If you want to work along in this lesson or review the final result, download the Chapter [11](#449379_2_En_11_Chapter.xhtml) files from the link in the introduction. The file with the label “Start” is the file without the code, and the file with the label “End” is the final result. You will also find folders with original MS Word and PDF files if you would like to edit them and a folder containing the original scripts if you would like to add them to your own PDF forms.

If you are creating your form from an original PDF, refer to the “Forms Review” section in Chapter [1](#449379_2_En_1_Chapter.xhtml).

Remember that to view the properties of a field, you must select the Prepare Form tool; only then can you right-click or double-click a field to review its properties.

## Money Transfer Example

The money transfer example is a wonderful way to see how validation can be applied to fields that do not have the Validate tab in their Properties dialog box. Refer to Figure [11-1](#449379_2_En_11_Chapter.xhtml#Fig1).

![](images/449379_2_En_11_Chapter/449379_2_En_11_Fig1_HTML.jpg)

A screenshot of a page has a table with 2 columns for details of account transfer from and to. It has 3 rows. Row 1 has 3 check boxes for checking, savings, and money market pro. Row 2 has account number details of from and to. Row 3 has the name of the account holder of both accounts.

Figure 11-1

In this money transfer example, you can choose to transfer your money to another account

Sometimes you may want to transfer an amount of money into another type of account. See the file `TMC Account Transfer End .pdf`. Refer to Figures [11-2](#449379_2_En_11_Chapter.xhtml#Fig2) and [11-3](#449379_2_En_11_Chapter.xhtml#Fig3).

![](images/449379_2_En_11_Chapter/449379_2_En_11_Fig3_HTML.jpg)

A screenshot of a page has a table with 2 columns for details of the sender and receiver accounts, each with 3 checkboxes and the checkbox for savings selected. It has a pop-up window with a message you cannot transfer funds from savings to savings.

Figure 11-3

Savings to savings account transfer with warning

![](images/449379_2_En_11_Chapter/449379_2_En_11_Fig2_HTML.jpg)

A screenshot of a page has 2 columns for details of the sender and the receiver accounts. It has a row with 3 checkboxes for checking, savings, and money market pro for each account, with the checking box selected.

Figure 11-2

Checking to savings or money market pro is a possible transfer

The choice in Figure [11-2](#449379_2_En_11_Chapter.xhtml#Fig2) gives you no warning, because you are transferring money from checking to savings. However, if you want to transfer from savings to savings, you will get a warning.

This type of transfer gives you a warning alert. You must click OK, and then the radio buttons reset themselves to blank.

You will also get a warning if you tried to transfer from checking to checking and money market pro to money market pro.

Figure [11-4](#449379_2_En_11_Chapter.xhtml#Fig4) shows how the buttons are set up in the form’s edit mode.

![](images/449379_2_En_11_Chapter/449379_2_En_11_Fig4_HTML.jpg)

A screenshot of a page has a table with 2 columns for details of the sender and receiver accounts, each with 3 radio buttons for checking, savings, and money market pro.

Figure 11-4

Radio button layout: the left is the From group, and the right is the To group

As mentioned earlier, radio buttons must always be grouped to work together; they need to have the same name on the General tab. They can have different values or radio button choices on the Options tab, but they need to be part of the same group. Refer to Figure [11-5](#449379_2_En_11_Chapter.xhtml#Fig5).

![](images/449379_2_En_11_Chapter/449379_2_En_11_Fig5_HTML.jpg)

A screenshot has an expanded view for page 1 on the left side. It has radio buttons for from and the to dot group. The right side has an expanded view of page 1 with expanded views for the from and the to group, including options for checking, savings, and m m.

Figure 11-5

The Prepare Form tool shows the grouping of the radio buttons collapsed and expanded, as seen on the right side of the Fields panel

Here you can see the setup for the from.group radio buttons. In this case, I have selected just the one with the choice of checking so that you can see its properties. If you choose savings or mm (money market pro), the only difference would be the radio button choice on the Options tab. Refer to Figure [11-6](#449379_2_En_11_Chapter.xhtml#Fig6).

![](images/449379_2_En_11_Chapter/449379_2_En_11_Fig6_HTML.jpg)

2 screenshots for the radio button properties page, each with 5 tabs for general, appearance, position, options, and actions. 1 has boxes for the name, tooltip, and common properties. 2 has a drop-down menu for the button style and a box for radio button choice.

Figure 11-6

Settings for the radio buttons in the from.group in the General and Options tabs. The only difference is the radio button choice

On the left side, the from.group radio buttons contain no actions. Refer to Figure [11-7](#449379_2_En_11_Chapter.xhtml#Fig7).

![](images/449379_2_En_11_Chapter/449379_2_En_11_Fig7_HTML.jpg)

A screenshot of the radio button properties page has 5 tabs for general, appearance, position, options, and actions, of which the actions tab is selected. It has a box to add an action with drop-down menus to select a trigger and an action.

Figure 11-7

Settings for the radio buttons in the from.group on the Actions tabs. No actions are applied

However, the to.group radio buttons contain an action because they are the second choice that may or may not trigger a warning based on whether its choice matches the first choice. On mouse up, I chose Run a JavaScript. Refer to Figure [11-8](#449379_2_En_11_Chapter.xhtml#Fig8).

![](images/449379_2_En_11_Chapter/449379_2_En_11_Fig8_HTML.jpg)

A screenshot of the radio button properties page has 5 tabs, and the actions tab is selected. It has a box to add an action with drop-down menus to select a trigger and an action. There is a pop-up window for Java Script Editor with a few lines of code to create and edit Java Scripts.

Figure 11-8

Settings for the radio buttons in the to.group on the Actions tab. The code is for a transfer from checking to checking

In my “End” example I typed the following code into the JavaScript Editor:

```
//to.group radio button 1 checking
var rfrom = this.getField("from.group");
var rtrans = event.target;
var msg = "You cannot transfer funds from Checking to Checking";
if (rfrom.value == "checking"){
app.alert(msg);
rfrom.value = "";
rtrans.value = "";
}
```

Here you have two variables: `rfrom` and `rtrans`. The first variable, `var rfrom`, represents the whole radio button group from.group. The second variable, `var rtrans`, represents the current field, which is part of to.group and has a choice of checking. An alert message variable is written and appears if the two choices match exactly:

```
var msg = "You cannot transfer funds from Checking to Checking";
```

Once the alert’s OK button is clicked, the `if` statement also resets the two variables.

```
rfrom.value = "";
rtrans.value = "";
```

They reset to blank as if never checked.

Note

I am using the error icon and OK button in the alert (Figure [11-3](#449379_2_En_11_Chapter.xhtml#Fig3)), as shown in Chapter [8](#449379_2_En_8_Chapter.xhtml). The code for both is 0, so they are left out of the code as default, but you could write `app.alert(msg,0,0);`, which would be the same.

Similar code can be written for the savings and money market pro radio buttons in the to.group, as follows:

**Savings Code**

```
//to.group radio button 2 savings
var rfrom = this.getField("from.group");
var rtrans = event.target;
var msg = "You cannot transfer funds from Savings to Savings";
if (rfrom.value == "savings"){
app.alert(msg);
rfrom.value = "";
rtrans.value = "";
}
```

**Money Market Pro Code**

```
//to.group radio button 3 mm
var rfrom = this.getField("from.group");
var rtrans = event.target;
var msg = "You cannot transfer funds from Money Market Pro to Money Market Pro";
if (rfrom.value == "mm"){
app.alert(msg);
rfrom.value = "";
rtrans.value = "";
}
```

In this case, only the message and the `rfrom` value change.

As you can see, when two fields’ choices match, an alert can be triggered.

Notice, however, that in this file if you were by accident first select the to.group radio button with the same name before the from.group, an alert would not be triggered. This is something to consider if you are not sure what order a client will enter in data. In a case like that, adding similar JavaScript code to the Actions tab (Figure [11-7](#449379_2_En_11_Chapter.xhtml#Fig7)) for each of the radio buttons in from.group would be a good idea.

The following is how the code for checking could appear; you can adapt it for the Savings and Money Market Pro radio buttons by altering the variable names, fields, values, and messages. Refer to the file `TMC_AccountTansfer_End_2.pdf`.

**For the Checking radio button:**

```
//from.group radio button 1 checking
var rto = this.getField("to.group");
var rtrans = event.target;
var msg = "You cannot transfer funds from Checking to Checking";
if (rto.value == "checking"){
app.alert(msg);
rto.value = "";
rtrans.value = "";
}
```

**For the Savings radio button:**

```
//from.group radio button 2 savings
var rto = this.getField("to.group");
var rtrans = event.target;
var msg = "You cannot transfer funds from Savings to Savings";
if (rto.value == "savings"){
app.alert(msg);
rto.value = "";
rtrans.value = "";
}
```

**For the Money Market Pro radio button****:**

```
//from.group radio button 3 mm
var rto = this.getField("to.group");
var rtrans = event.target;
var msg = "You cannot transfer funds from Money Market Pro to Money Market Pro";
if (rto.value == "mm"){
app.alert(msg);
rto.value = "";
rtrans.value = "";
}
```

### Alternate Way to Work with Radio Buttons Without Alerts

Rather than an alert pop-up, another way of preventing the client from clicking the wrong radio button is to have a radio button set to read-only when its counter option is selected. The problem with doing this is that radio buttons operate in a group, so if one is set to read-only, then the others in that group are as well, which is not what you want. The next best option is to show and hide the selected radio button so that the client does not have an opportunity to click it. It is removed entirely, and the client must choose between the remaining two options for the transfer. This example assumes again that your client will work from left to right in the form. Refer to Figure [11-9](#449379_2_En_11_Chapter.xhtml#Fig9).

![](images/449379_2_En_11_Chapter/449379_2_En_11_Fig9_HTML.jpg)

A screenshot of a page has a table with 2 columns for details of the sender and receiver accounts. The sender account has 3 buttons for checking, saving, and money market pro, of which checking is selected. The receiver account has 3 options, with no button for checking.

Figure 11-9

Removing a radio button from the possible choices

Refer to the file `TMC_AccountTransfer_End_show_hide.pdf` to see this in action.

In this example, I removed all the JavaScript from the radio buttons in the group named to.group on the Actions tab and this time applied a code to each of the radio buttons in from.group.

For checking on Mouse Up, I selected Run a JavaScript.

```
//checking
var checkingTo = this.getField("to.group.0");
var savingsTo = this.getField("to.group.1");
var mmTo = this.getField("to.group.2");
if(event.target.value !="Off"){
checkingTo.display = display.hidden;
savingsTo.display = display.visible;
mmTo.display = display.visible;
}
```

You saw similar code in Chapter [9](#449379_2_En_9_Chapter.xhtml) when working with a check box to turn fields on and off. However, turning radio buttons on and off is a bit trickier to control. They like an `if` conditional but not an `else` as they move in a group and do not operate separately on and off. Another radio button will need to set the next condition.

So, in this code for the checking radio button, I set all the variables of each button. You can keep them separate by adding a dot and then a number to each field, much like an array, on each radio button, in this case 0-2.

Then set, in the `if` condition when the radio button is active or not off, what fields you would like to see or hide. In this case, I want to hide the other radio button `checkingTo`, so I set the display to hidden.

I then added the same code to each of the other radio buttons in from.group and altered the display lines of the code. For example, for `savingsTo`, see the changes in the following code in bold:

```
//savings
var checkingTo = this.getField("to.group.0");
var savingsTo = this.getField("to.group.1");
var mmTo = this.getField("to.group.2");
if(event.target.value !="Off"){
checkingTo.display = display.visible;
savingsTo.display = display.hidden;
mmTo.display = display.visible;
}
```

Then for Money Market Pro, when you paste the code in the editor, just change this area of the previous code:

```
if(event.target.value !="Off"){
checkingTo.display = display.visible;
savingsTo.display = display.visible;
mmTo.display = display.hidden;
}
```

Lastly, to reset all the radio buttons to visible, I used a Reset button on the form, along with choosing the action of Reset a Form to reset all fields. Also, I made sure to add the action of “Show/hide a field” and then in the Show/Hide field dialog box made sure that the to.group radio buttons are set to show. Refer to Figure [11-10](#449379_2_En_11_Chapter.xhtml#Fig10).

![](images/449379_2_En_11_Chapter/449379_2_En_11_Fig10_HTML.png)

A screenshot of the button properties page has the actions tab selected, with 2 drop-down menus to select a trigger and an action. It has a show or hide field page with 2 options show and hide, of which show is selected. The left side of the page has a few options, of which to dot group is selected.

Figure 11-10

Showing all radio buttons upon reset using the Actions tab and Show/Hide dialog box

Keep in mind that in this example if your client makes a selection for to.group before from.group, the hidden radio button will still be selected. However, the hiding of the field should cause your client to select one of the other visible options if they see that the wrong selection is missing. Alternatively, for each from.group radio button, in the `if` condition at the bottom, you could add `this.resetForm([“to.group”]);`.

```
if(event.target.value !="Off"){
checkingTo.display = display.visible;
savingsTo.display = display.visible;
mmTo.display = display.hidden;
this.resetForm(["to.group"]);
}
```

This would reset or clear to.group again so that you can now enter the correct option in from.group first. Refer to the file `TMC_AccountTransfer_End_show_hide_2.pdf` if you want to see an example of this.

## Changing the Shipping Price Using Radio Buttons

As you’ve seen in past projects, check boxes and radio buttons can be used to adjust numbers in text boxes. Let’s look at another example of how this can be done with prices. See the file `InvoiceForm_Shipping_Change_End.pdf`, and refer to Figure [11-11](#449379_2_En_11_Chapter.xhtml#Fig11).

![](images/449379_2_En_11_Chapter/449379_2_En_11_Fig11_HTML.jpg)

2 screenshots of an invoice, each with the same values for discount, total, provincial tax, federal tax, shipping charge, and total. The first page has 3 buttons for Loomis, U P S, and Fed Ex, of which Loomis is selected. The second page has the Fed Ex option selected.

Figure 11-11

Invoice form for adjusting the shipping price using radio buttons

These numbers will vary depending on what you input. If you choose a different company like FedEx, the shipping charge changes again. See note on refreshing the fields later in this section.

How does this work?

Most of this form is set up like most invoice forms, in which fields use a combination of value and Simplified Field Notation (SFN) and very little JavaScript. However, this example contains a new field called *discount* and the radio buttons grouped as shipChoice on the lower left.

The field discount is just formatted as a number with the dollar symbol in the currency symbol and has no action applied to it. Refer to Figure [11-12](#449379_2_En_11_Chapter.xhtml#Fig12).

![](images/449379_2_En_11_Chapter/449379_2_En_11_Fig12_HTML.jpg)

A screenshot of the text field properties page has 8 tabs, of which the format tab is selected. It has a drop-down menu to select the format category, decimal places, separator style, currency symbol, symbol location, and negative number style.

Figure 11-12

The discount field is formatted as a number on the Format tab

However, you are going to call upon the discount, Total, and Shipping fields once you start using the radio buttons. The radio buttons make up one group, each with their own choices on the Options tab. Refer to Figure [11-13](#449379_2_En_11_Chapter.xhtml#Fig13).

![](images/449379_2_En_11_Chapter/449379_2_En_11_Fig13_HTML.jpg)

A screenshot of a page has an expanded view for ship choice. It has 3 buttons for Loomis, U P S, and FedEx. It has a page for radio button properties with 5 tabs has the options tab highlighted. It includes a drop-down menu for button styles and a box for radio button choice.

Figure 11-13

Radio group shipChoice and radio button properties on the Options tab

The Fields panel on the right of the Prepare Form tool shows how the radio buttons are grouped. Then you add JavaScript to each radio button on the Actions tab on the Mouse Up trigger. Refer to Figure [11-14](#449379_2_En_11_Chapter.xhtml#Fig14).

![](images/449379_2_En_11_Chapter/449379_2_En_11_Fig14_HTML.jpg)

A screenshot of the radio button properties has 5 tabs for general, appearance, position, options, and actions, with the actions tab selected. It has a box to add an action, a drop-down menu to select a trigger, and a box to select an action.

Figure 11-14

The Radio Button Properties dialog box on the Actions tab, with the action of “Run a JavaScript”

There is similar code for each radio button, shown here:

```
//Shipping Price Charge
var oShip = this.getField("Shipping");
var oSub = this.getField("Total");
var oDiscount = this.getField("discount");
if(oSub.value <200)
var oRate = (oSub.value - oDiscount.value)*.25;
else
var oRate =0;
oShip.value = oRate;
```

The code says that if the Total field before or after the discount is less than $200, then add the shipping charges (that is, Total * .25) to get the shipping price. However, if the price is over $200 or even with the discount (`else` statement), the shipping is 0 dollars, or free.

Note that the Total field has its own calculation that relates to a hidden field called “Sub total.” This was added so that the discount can be added accurately to the shipping charge and grand total, upon refresh of the radio button. Refer to Figure [11-15](#449379_2_En_11_Chapter.xhtml#Fig15).

![](images/449379_2_En_11_Chapter/449379_2_En_11_Fig15_HTML.jpg)

A screenshot of an invoice has attributes for quantity, price, sub-total, discount, taxes, shipping charge, and a column for the grand total which is hidden. It has a page for text field properties to calculate the value, followed by a formula for the simplified field notation for the sub-total.

Figure 11-15

The hidden subTotal text field uses a value sum (+) to add up all the fields. Then the Total field uses those values later and calculates the discount using the radio buttons to get the actual grand total

As you can see, you can apply this JavaScript to the other courier’s radio button on the Actions tab like this:

```
var oShip = this.getField("Shipping");
var oSub = this.getField("Total");
var oDiscount = this.getField("discount");
if(oSub.value <200)
var oRate = (oSub.value - oDiscount.value)*.17;
else
var oRate =0;
oShip.value = oRate;
```

You could change the price from this:

```
var oRate = (oSub.value - oDiscount.value)*.17;
```

to the following:

```
var oRate = (oSub.value - oDiscount.value)*.13;
```

Or you could change the dollar limit from this:

```
if (oSub.value <200)
```

to the following:

```
if (oSub.value <300)
```

depending on the preferences of the courier company.

Note

You could have written the conditional part of the code with the added curly brackets ({ }).

```
if(oSub.value <200){
var oRate = (oSub.value - oDiscount.value)*.17;
}
else{
var oRate =0;
}
oShip.value = oRate;
```

Note

In this case, it was a small and uncomplicated conditional script, and I left some bracket sets out. JavaScript will sometimes let you do that to make code less complicated. However, in other cases, if you leave them out, you may get an error warning, or the script will not function correctly. If you are unsure or want to keep the code accurate, I recommend leaving the brackets in complex examples to avoid confusion. As you can see, the line `oShip.value =oRate;` is outside the conditional statement, but you might not know that without the curly brackets. Refer to my file `InvoiceForm_Shipping_Change_End_2.pdf` if you need to see how this code looks in each of the radio buttons.

One final item you add to each radio button is the ability to reset/refresh the shipping when you click the radio button again rather than having to click another radio button (On Mouse Down ➤ Reset a form). Check this only for the Shipping field. I also in this case added a sticky note comment for the user so that they would be aware that they can refresh the shipping as they work on the form. Refer to Figure [11-16](#449379_2_En_11_Chapter.xhtml#Fig16).

![](images/449379_2_En_11_Chapter/449379_2_En_11_Fig16_HTML.png)

A screenshot of the radio button properties has drop-down menus for trigger and action, followed by a list of actions, of which mouse down is selected. It has a pop-up window to reset a form with check boxes for the list of fields, and shipping is selected.

Figure 11-16

Radio button settings in the Actions tab to refresh the shipping and a Sticky note to let the user they can refresh the numbers using the radio buttons

## Additional Radio Button Example with JavaScript and Text Fields and How to Change Data for Time

The radio button and text box can be also used in another similar example where you might want to switch calculations of time, especially for certain changes with the millisecond conversion of the time between two dates. You can refer to Chapter [7](#449379_2_En_7_Chapter.xhtml) in regard to time formatting and date formatting. However, in this example, look at the file `Acrobat Format Time_Date_radio_End.pdf` and Figure [11-17](#449379_2_En_11_Chapter.xhtml#Fig17).

![](images/449379_2_En_11_Chapter/449379_2_En_11_Fig17_HTML.jpg)

A screenshot of a page titled Acrobat format calculate time examples has a table for time calculation with 4 columns for the format, date in, date out, and total. It has a few radio buttons for time, of which the millisecond option is selected.

Figure 11-17

Radio buttons are used to change the time conversion in the TotalDate field on the right

This document uses a global document JavaScript so that I can control the change using the radio buttons that draw from the input of two fields StartingDate and EndingDate, and the Total is then added to the TotalDate field.

In this example of the document JavaScript, I created a function that I called TimeDateDiff. Refer to Figure [11-18](#449379_2_En_11_Chapter.xhtml#Fig18).

![](images/449379_2_En_11_Chapter/449379_2_En_11_Fig18_HTML.jpg)

A screenshot has an add option for document Java Scripts. It has a pop-up window for Document Java Scripts with a box to enter the script name, marked as time date difference, followed by a set of codes. It has a few options on the right side of the page, and the close option is selected.

Figure 11-18

Adding the document JavaScript to the file

The JavaScript code is as follows:

```
function TimeDateDiff(){
var sStart = getField("StartingDate").value;
var sEnd = getField("EndingDate").value;
var dStart, dEnd, diff;
if(sStart && sEnd) {
dStart = util.scand("mm/dd/yyyy HH:MM:ss tt", sStart);
dEnd = util.scand("mm/dd/yyyy HH:MM:ss tt", sEnd);
if ( dEnd < dStart)  {
app.alert ( "The end date must be after start date and not before.");
event.value = "";
this.resetForm(["StartingDate", "EndingDate"]);
}
else{
var TimeCov = getField("Time").value;
//diff = (dEnd.getTime() - dStart.getTime())/86400000;
diff = (dEnd.getTime() - dStart.getTime())/TimeCov;
event.value = diff;
}
}
else
{
event.value = "";
}
}
```

This is similar to the code in Chapter [7](#449379_2_En_7_Chapter.xhtml). However, this time I am getting my calculation for time from the group of radio buttons called Time. Refer to Figure [11-19](#449379_2_En_11_Chapter.xhtml#Fig19).

![](images/449379_2_En_11_Chapter/449379_2_En_11_Fig19_HTML.jpg)

A screenshot of a page has 6 radio buttons for time, including weeks, days, hours, minutes, seconds, and milliseconds, and the millisecond option is selected. The right side of the page has an expanded view of the time button with 6 different numerical values.

Figure 11-19

Radio button group time and their values

These buttons contain no JavaScript; however, I have set their values on the Options tab for the radio button choice. Refer to Figure [11-20](#449379_2_En_11_Chapter.xhtml#Fig20).

![](images/449379_2_En_11_Chapter/449379_2_En_11_Fig20_HTML.jpg)

A screenshot of the radio button properties page has 5 tabs for general, appearance, position, options, and actions, of which the options tab is selected. It has a drop-down menu to choose the button style, a box to enter the radio button choice, and 2 checkboxes for the button.

Figure 11-20

Radio Button Properties dialog box with a radio button choice; for milliseconds it is set to “Button is checked by default”

In the case of milliseconds, the set to “Button is checked by default,” but for all other radio buttons, I left the setting unchecked.

The value for milliseconds is set to 1\. When we get the calculation of time difference, we will divide by 1 to get all the milliseconds.

Likewise, the other radio buttons have their own radio button choices that will be divided by Seconds (1000), Minutes (60000), Hours (3600000), Days (86400000), and Weeks (604800000).

### Setting Up the Text Fields

In this example, the text fields StartingDate and EndingDate were set on the Format tab with a custom date to match the date setting in the document JavaScript code `mm/dd/yyyy HH:MM:ss tt`. Refer to Figure [11-21](#449379_2_En_11_Chapter.xhtml#Fig21).

![](images/449379_2_En_11_Chapter/449379_2_En_11_Fig21_HTML.jpg)

A screenshot of the test field properties page has 8 tabs, of which the format tab is selected. It has a drop-down menu to select the format category for the date, followed by number options, and a box for the example of the current format.

Figure 11-21

Text Field Properties dialog box custom formatting for dates on the Format tab

The last field, TotalDate, which is set to Read Only, is formatted to Number with 10 decimal places. However, for the custom calculation script on the Calculate tab, I have referenced the document JavaScript function `TimeDateDiff();`. Refer to Figure [11-22](#449379_2_En_11_Chapter.xhtml#Fig22).

![](images/449379_2_En_11_Chapter/449379_2_En_11_Fig22_HTML.png)

2 screenshots for the test field properties have the format and the calculate tab selected. Page 1 has the format category selected as a number, followed by 5 drop-down menus for the number options. Page 2 has 4 checkboxes, of which a custom calculation script is selected for time date difference.

Figure 11-22

Text Field Properties dialog box for the Format and Calculate tabs

Once the function `TimeDateDiff();` is referenced by the field, you can begin to change the two dates as well as the radio buttons.

Note

Besides the warning in the document JavaScript that prevents start dates from being added before end dates, you may also receive an additional warning if your milliseconds are too low for the day or week, but it will still calculate the milliseconds regardless. In your own projects, you may want fewer decimal places or will want to round the numbers up or down as mentioned in Chapter [7](#449379_2_En_7_Chapter.xhtml).

### Check Box Variation with Date Text Fields

As mentioned in Chapter [2](#449379_2_En_2_Chapter.xhtml), check boxes can act as radio buttons if they are given the same name on the General tab. Then the values must be changed on the Options tab.

If you choose to use check boxes instead because that is your preference, then make sure to set up the Options tab as follows and refer to the file `Acrobat Format Time_Date_checkbox_End.pdf`. This time set your numbers in the Export Value. Refer to Figure [11-23](#449379_2_En_11_Chapter.xhtml#Fig23).

![](images/449379_2_En_11_Chapter/449379_2_En_11_Fig23_HTML.jpg)

2 screenshots for the check box properties page with the general and options tab selected. Page 1 has a box with the name time. Page 2 has a drop-down menu for the check box style as check and export value as 1. It has 6 check boxes on the right side of the page, with milliseconds selected.

Figure 11-23

Text Field properties for Format and Calculate tabs

Remember only one check box needs to be set as ”Check box is checked by default.” Otherwise, the code does not change. If the name of your check boxes are the same as your name in the document JavaScript, they will operate just like the radio buttons.

### Notes on Changing Formatting Options for Radio Button, Check Box, and Button Style

In Chapter [9](#449379_2_En_9_Chapter.xhtml), you saw how to format text field code with JavaScript. This is quite easy either using the document JavaScript or using the Format tab to set a custom format. The Format tab is available only for text fields and drop-down lists, so this is not an option you can use for other fields, including list boxes, which I will discuss in Chapter [13](#449379_2_En_13_Chapter.xhtml). You can use the Appearance tab or Options tab to format your button, radio button, and check boxes one at a time, or as you saw in Chapter [10](#449379_2_En_10_Chapter.xhtml), you can use a document JavaScript to format all those similar fields at once. Refer to Figure [11-24](#449379_2_En_11_Chapter.xhtml#Fig24).

![](images/449379_2_En_11_Chapter/449379_2_En_11_Fig24_HTML.jpg)

A screenshot of the radio button properties page has 5 tabs, of which the appearance tab is selected. It has details for the borders and colors, including border color, line thickness, fill color, and line style, and details of the text, including font size, text color, and font.

Figure 11-24

Radio Button Properties dialog box for the Appearance tab

#### Formatting for Buttons and How the Following Properties Can Be Affected

Refer to Chapter [10](#449379_2_En_10_Chapter.xhtml) for exact coding descriptions. Like with text boxes, you can affect `strokeColor, fillColor, lineWidth, borderStyle, textSize,` `textFont``,` and `textColor`. However, you cannot use any rich-text formatting.

Here’s an example of using the document JavaScript:

```
for (var i = 0; i < numFields; i++) {
// Get a reference to the current field
var f = getField(getNthFieldName(i));
// If it's a text field, set the font and font size or set other properties
if (f.type == "button") {
f.strokeColor = color.blue;
f.fillColor = color.red;
f.textColor = color.black;
}
}
```

Or for a single field in the Actions tab, you could write this:

```
var f = this.getField("myButton");
f.strokeColor = color.blue;
f.fillColor = color.red;
f.textColor = color.black;
```

In this case, once the button was clicked (Mouse Up), the formatting could be applied.

Some additional formatting options for rotation, text alignment, and highlight properties can be found here:

```
https://opensource.adobe.com/dc-acrobat-sdk-docs/library/jsdevguide/JS_Dev_AcrobatForms.html#28613
```

#### Formatting for Radio Buttons and Check Boxes

Refer to Chapter [10](#449379_2_En_10_Chapter.xhtml) for the exact coding description. Like with text boxes, you can affect `strokeColor,` `fillColor,` `lineWidth, borderStyle, textSize,` and `textColor`. However, you cannot use any rich-text formatting or affect the `textFont` as it is set to the default of Adobe Pi. You can, however, change the check box or radio button style using a document JavaScript. Refer to Figure [11-25](#449379_2_En_11_Chapter.xhtml#Fig25) and see how the options appear on the Options tab.

![](images/449379_2_En_11_Chapter/449379_2_En_11_Fig25_HTML.jpg)

A screenshot of the check box properties page has 5 tabs for general, appearance, position, options, and actions, of which the options tab is selected. It has a drop-down menu to select the check box style, selected as check.

Figure 11-25

Check Box Properties dialog box on the Options tab

The following style formatting code examples are for a check, circle, cross, diamond, square, and star:

```
//check
f.style = style.ch;
//cross
f.style = style.cr;
//diamond
f.style = style.di;
//circle
f.style = style.ci;
//square
f.style = style.sq;
//star
f.style = style.st;
```

For example, replace f.type with either “radiobutton” or “checkbox” when creating a document JavaScript.

```
for (var i = 0; i < numFields; i++) {
// Get a reference to the current field
var f = getField(getNthFieldName(i));
// If it's a text field, set the font and font size or set other properties
if (f.type == "checkbox") {
f.strokeColor = color.blue;
f.fillColor = color.red;
f.textColor = color.black;
f.style = style.ch;
}
}
```

Note that only the circle radio button is round, while all other styles make it a square shape.

Likewise, as with the button, you could target a single radio button within a group or check box when this code is applied to the Actions tab. This is assuming you have named them correctly on the General tab. They will then change format on the Mouse Up trigger when you click them.

//single radio button for myGroup

```
var f = this.getField("myGroup.0");
f.strokeColor = color.blue;
f.fillColor = color.red;
f.textColor = color.black;
f.style = style.di;
```

//single check box

```
var f = this.getField("myCheckbox");
f.strokeColor = color.blue;
f.fillColor = color.red;
f.textColor = color.black;
f.style = style.di;
```

Additional references to formatting can be found on the following links:

[`https://opensource.adobe.com/dc-acrobat-sdk-docs/library/jsdevguide/JS_Dev_AcrobatForms.html#85887`](https://opensource.adobe.com/dc-acrobat-sdk-docs/library/jsdevguide/JS_Dev_AcrobatForms.html#85887)

[`https://opensource.adobe.com/dc-acrobat-sdk-docs/library/jsdevguide/JS_Dev_AcrobatForms.html#34428`](https://opensource.adobe.com/dc-acrobat-sdk-docs/library/jsdevguide/JS_Dev_AcrobatForms.html#34428)

## Text Field Validation with Regular Expressions

Another feature to try in this form (see the file `InvoiceForm_Shipping_Change_End_defaulttext.pdf`) is a telephone validation script. Refer to Figure [11-26](#449379_2_En_11_Chapter.xhtml#Fig26) and the text field Phone Number.

![](images/449379_2_En_11_Chapter/449379_2_En_11_Fig26_HTML.jpg)

A screenshot of the test field properties has 8 tabs, of which the validate tab is selected. It has the field value not validated option selected, followed by a few lines of code for the custom validation script. There is a window at the bottom of the page that reads invalid telephone number.

Figure 11-26

Telephone text field validation on the Validate tab and the alert that is produced when the input is incorrect

If the client does not type in the text field in the correct way, an alert will happen, and the text will appear red. The correct format is displayed in Figure [11-27](#449379_2_En_11_Chapter.xhtml#Fig27) in black text.

![](images/449379_2_En_11_Chapter/449379_2_En_11_Fig27_HTML.jpg)

A screenshot of a page has a box for a phone number with the number 55, followed by a box for a phone number with a 10-digit number, 999 999 999.

Figure 11-27

Top: Incorrectly entered information appears in red. Bottom: Correctly entered information in the Telephone text field appears in black

After the OK button in the alert is clicked, the text in the text field turns red and will not change back to black until the user types in the correct format.

### Telephone Validation

Here is the JavaScript that is added to the field’s Validate tab under “Run custom validation script” (Figure [11-26](#449379_2_En_11_Chapter.xhtml#Fig26)):

```
var rgexTele = /^[(]{0,1}[0-9]{3}[)]{0,1}[-\s\.]{0,1}[0-9]{3}[-\s\.]{0,1}[0-9]{4}$/;
if(rgexTele.test(event.value)){
event.target.textColor = color.black; //Change text to black
{
else{
event.target.textColor = ["RGB",1,0,0]; //Change text to red
app.alert("Invalid Telephone Number");
}
```

To accomplish this, you use a form of JavaScript called a *regular expression*. You saw a simple example of this when you created your multiline button using `\n`, the new line, and you created a metacharacter in Chapter [10](#449379_2_En_10_Chapter.xhtml). When you combine more than one metacharacter, you create a pattern, and this combination of characters creates a regular expression. This time you are going to use a more complex regular expression.

```
/^[(]{0,1}[0-9]{3}[)]{0,1}[-\s\.]{0,1}[0-9]{3}[-\s\.]{0,1}[0-9]{4}$/
```

This portion of the expression is saying that the code after the quantifiers of {0,1} must match the following:

*   Three digits within parentheses ranging from 0 to 9 in the first section that can be separated by either a dash (-), a space (\s), or a dot (.). These digits can be enclosed in parentheses.

*   Three digits ranging from 0 to 9 in the second section that can be separated by either a dash, a space, or a dot.

*   Four digits ranging from 0 to 9 in the last section.

*   The caret (/^) begins the line of data, while the dollar sign ($/) ends the line of data.

The following examples are acceptable:

*   (123) 456-7890

*   123-456-7890

*   123.456.7890

*   123 456 7890

*   1234567890

This code even allows for no spaces. However, anything more or less than 10 digits is unacceptable, and you cannot use letters.

You can use the format Special Phone Number or Arbitrary Mask if you do not want to write this. Refer to Figure [11-28](#449379_2_En_11_Chapter.xhtml#Fig28).

![](images/449379_2_En_11_Chapter/449379_2_En_11_Fig28_HTML.jpg)

A screenshot of the text field properties page has 8 tabs, of which the format tab is selected. It has a drop-down menu to select the format category as special, followed by a list of special options for zip code, zip code+4, phone number, social security number, and arbitrary mask.

Figure 11-28

The Special Phone format can be used

However, doing so would not have given you the custom number and color formatting, used to show the difference between correct and incorrect. So, leave the format as None and enter the following code on the Validate tab (Figure [11-26](#449379_2_En_11_Chapter.xhtml#Fig26)):

```
var rgexTele = /^[(]{0,1}[0-9]{3}[)]{0,1}[-\s\.]{0,1}[0-9]{3}[-\s\.]{0,1}[0-9]{4}$/;
if(rgexTele.test(event.value)){
event.target.textColor = color.black; //Change text to black
{
else{
event.target.textColor = ["RGB",1,0,0]; //Change text to red
app.alert("Invalid Telephone Number");
}
```

If the input text is tested and matches the regular expression format, leave it as black. If not (else), send out a warning alert and change the text to red until it is corrected and matches the regular expression.

There are a lot of good regular expressions that can be used, and I recommend exploring them; I will provide links later in the chapter.

Note

According to the Adobe documentation, the format setting Special Arbitrary Mask will allow you to use some basic regular expressions such as the following:

`A`: Accepts only letters (A–Z, a–z)

`X`: Accepts spaces and printable characters, including all characters available on a standard keyboard and ANSI characters in the ranges of 32–126 and 128–255

`O`: Accepts alphanumeric characters (A–Z, a–z, and 0–9)

`9`: Accepts only numeric characters (0–9)

For example, a mask setting `AAA-–q#999` accepts the input of BRT--q#123\. The q# in this case do not change and must be exact. Or, if `OOO@XXX`, it accepts the input of v12@2up. However, as you’ll see, regular expressions with JavaScript offer a wider range of possibilities.

For some more examples, you can look at the file `TMC_FieldValidationsEnd.pdf`. Refer to Figure [11-29](#449379_2_En_11_Chapter.xhtml#Fig29).

![](images/449379_2_En_11_Chapter/449379_2_En_11_Fig29_HTML.jpg)

A screen of the page titled field validations with regular expressions. It has a table with 9 rows for the date, date hash 2, name, serial hash, address, email, U R L or web, telephone, and telephone hash 2\. The rows for name, serial hash, and telephone have entries.

Figure 11-29

Text field examples to which you can add regular expressions

### Name Validation

Use this script if you want to have a first and last name in the same field. Refer to the field called Name. The code is entered into the Validate tab. Refer to Figure [11-30](#449379_2_En_11_Chapter.xhtml#Fig30).

![](images/449379_2_En_11_Chapter/449379_2_En_11_Fig30_HTML.jpg)

A screenshot of the text field properties page has 8 tabs, of which the validate tab is selected. It has 3 checkboxes for field value not validated, range of the field value, and the validation script. The third option to run a custom validation script is selected, followed by a few lines of code.

Figure 11-30

Running a custom validation script on the Validate tab

```
// Custom Validation of Name
var re = /^[A-Z][a-z]+ [A-Z][a-z]+$/;
if(event.value !=""){
if (re.test(event.value)== false){
app.alert("That does not appear to be a valid name. I need a first and last name.");
event.rc = false;
}
}
```

Note

The following are basic examples of regular expression metacharacters:

`[abc]`: Find any character between the brackets.

`[A-Z]`: Find any character from uppercase A to uppercase Z.

`[a-z]`: Find any character from lowercase a to lowercase z.

`[A-z]`: Find any character from uppercase A to lowercase z.

`[0-9]`: Find any digit between the brackets.

`.`: Find a single character, except newline or line terminator.

`\d`: Find a digit.

`\n`: Find a newline character.

`\s`: Find a whitespace character.

`\w`: Find a word character.

For more detailed information on regular expressions, visit any of the following:

[`www.w3schools.com/jsref/jsref_obj_regexp.asp`](http://www.w3schools.com/jsref/jsref_obj_regexp.asp)

[`https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions)

[`https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions/Quantifiers`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions/Quantifiers)

In this Name field, the name needs a first and last name that contains uppercase and then lowercase letters, as well as a space between those names like John Smith, as you can see the blank space after the (+) in the code that separates the names. If the user types in JS or just J, they will get the error alert “That does not appear to be a valid name. I need a first and last name.” When they click the OK button on the error alert, the field returns to blank (`event.rc = false;`) and resets the validation. They must try again, and no further information is processed. Or if a correct format text was entered earlier, it will in that case revert to that text after the alert.

The code `event.rc` is used as a type of reset that can be used with the validation of regular expressions. In this case, because it is set to false, the input data is rejected because the formatting is incorrect. If no user input text is in the field, then no error can occur.

Note we will look at some additional formatting on the Format tab that is added to this field later in the chapter; refer to the “Final Thoughts on Formatting Fields” section.

### Account Number Validation

Use this script if you want to create a field where a person must enter a special account number or serial number. Refer to Figures [11-29](#449379_2_En_11_Chapter.xhtml#Fig29) and [11-30](#449379_2_En_11_Chapter.xhtml#Fig30) to see where to enter the code for the field called Serial.

```
//account number
//A6BF-479X-2139
var re = /^[A-Z]\d[A-Z]{2}-\d{3}[A-Z]-\d{4}$/;
//prevent alert if field is blank
if(event.value !=""){
if (re.test(event.value)== false){
app.alert("I'm sorry. That is not a valid account number.\n Example: A6BF-479X-2139");
event.rc = false;
}
}
```

In this field, the code must be an uppercase letter, a digit, two uppercase letters, then a dash, three number digits, uppercase letter, then another dash, and four number digits. This must be correct or an error alert will occur, and the field will be returned blank.

### Email and URL Validation

Use the following script if you want to create a field where a person must enter an email or a URL and search for a word character (\w) and literals (*). Refer to Figures [11-29](#449379_2_En_11_Chapter.xhtml#Fig29) and [11-30](#449379_2_En_11_Chapter.xhtml#Fig30) for where to enter the code in the fields Email and URLWeb.

```
//email
var re = /^\w+([\.-]?\w+)*@\w+([\.-]?\w+)*(\.\w{2,3})+$/;
//prevent alert if field is blank
if(event.value !=""){
if (re.test(event.value)== false){
app.alert("I'm sorry. That is not a valid email address.");
event.rc = false;
}
}
```

As you can see, the code is like the others except that this time you need to use the following format to check for the email names and the @ symbol. So, jennifer@email.com would be correct, but excluding the @ symbol would trigger an alert. If you were creating a validation for a URL website, you would need to adjust the code to perhaps something like this with additional quantifiers in the curly brackets, {}, on all one line:

```
var re = /^(http:\/\/www\.|https:\/\/www\.|http:\/\/|https:\/\/)[a-z0-9]+
([\-\.]{1}[a-z0-9]+)*\.[a-z]{2,5}(:[0-9]{1,5})?(\/.*)?$/;
```

Then you could add an alert to reflect if there was an error. For example, [`http://www.mysite.com`](http://www.mysite.com) is correct, but if part of the URL was left off, then you would get a warning.

```
app.alert("I'm sorry. That is not a valid URL address.");
```

In both examples, quantifiers like {2,3}, {2,5}, and {1,5} are used to help with the expression validation and matching. This example does allow you to exclude the www, but not the http:// or https://.

### Another Phone and Date Example with Two Variables

Use this script if you want to create a field where a person must enter a phone number or date with two options. Refer to Figures [11-29](#449379_2_En_11_Chapter.xhtml#Fig29) and [11-30](#449379_2_En_11_Chapter.xhtml#Fig30) for where to enter the code. The field Telephone is the regular expression we discussed earlier. However, the field Telephone 2 has the new warning alert message and validation, as shown in Figure [11-31](#449379_2_En_11_Chapter.xhtml#Fig31).

![](images/449379_2_En_11_Chapter/449379_2_En_11_Fig31_HTML.jpg)

A screenshot of a warning window has a message that reads, I am sorry, that is not a valid 7 or 10-digit phone number, followed by an okay button.

Figure 11-31

The alert warning when a telephone number is added incorrectly

```
//phone validation alert
var re7Digits = /^(\d{3})-(\d{4})$/;
var re10Digits = /^(\d{3})-(\d{3})-(\d{4})$/;
//prevent alert if field is blank
if(event.value !=""){
if (re7Digits.test(event.value)== false && re10Digits.test(event.value)== false){
app.alert("I'm sorry. That is not a valid 7 or 10 digit phone number. ");
event.rc = false;
}
}
```

This final example deals with the option to enter a 7-digit or 10-digit code with a dash separation. While not as complex as the first phone example, it does show how to have two formatting options inside one validation. By using && (logical conjunction), you are asking for two conditions to be false or true. If true, no alert warning will happen; if false, the alert will happen, and the fields will be set back to blank.

You could also use this example with a Date text field. You can set the variables to something like this for mm/dd/yyyy or mm/dd/yy:

```
var re6Date = /^(\d{1,2})\/(\d{1,2})\/(\d{2})$/;
var re8Date = /^(\d{1,2})\/(\d{1,2})\/(\d{4})$/;
```

Make a few adjustments to the variable names in the conditional statement so they match as well as the text in the `app.alert`. Refer to the field Date 2\. You would have two options (12/31/05 or 12/31/2005) for date formats for a person to enter. However, be aware that if this is something that is going into the database, it might cause errors, so check with the people who oversee the database before you set an alternate date entry. This option also does not do a validation check for number limit such as if you type in a 13th month or check for exact number of days in the month like for a leap year. A closer example using the same formatting would be to write your regular expression variables like this to limit your numbers to 12 months and 31 days as well as the 1900s to 2000s:

```
var re6Date = /^(0[1-9]|1[012])\/(0[1-9]|[12][0-9]|3[01])\/(\d{2})$/;
var re8Date = /^(0[1-9]|1[012])\/(0[1-9]|[12][0-9]|3[01])\/(19|20)\d\d$/;
```

However, keep in mind you could still get away with writing 02/30/2023, which is not an actual date. So, likely further complex validation in the script beyond the topic of this book would need to be considered for this and other complex cases. You can refer to my file `Complex_Validation_Dates.pdf` if you want to explore examples of this date and a suggestion on how to modify it further on your own.

Note

With the updates of Acrobat Pro, you can alternately set your formatting of the date field to Date to access the pop-up menu. Refer to Chapter [1](#449379_2_En_1_Chapter.xhtml) if you need to see the Date field on this form. However, when you have two alternate date options, refer to field Date 2\. You must set the Format tab to None and place the code on the Validate tab. Doing this does not give you access to the pop-up menu. Also, as just mentioned, creating an accurate regular expression for dates to avoid errors can be complex, so that is something to first consider.

One final regular expression that you may encounter on some coding examples is the (g), which is sometimes added onto the end of expressions as a type of flag when doing a global search. While in the previous examples the code works without it, as we are working with only one field, this additional property indicates whether the regular expression should be tested against all possible matches in the string. The file `TMC_FieldValidationsEnd_2.pdf` shows the same examples with the (g) applied to the code for reference.

Here is an example for the first and last name:

```
var re = /^[A-Z][a-z]+ [A-Z][a-z]+$/g;
```

## Final Thoughts on Formatting Fields

As you can see, errors in typing and selection can be avoided if you have the correct warnings and validations in place. You can assist yourself and your clients when you take the guesswork out of a form.

If you are planning to work with the gray default text method from Chapter [9](#449379_2_En_9_Chapter.xhtml) and combine it with the regular expression examples from this chapter, it can easily be done by adding this custom format on the Format tab. (See the examples in the files InvoiceForm_Shipping_Change_End_defaulttext.pdf and `TMC_FieldValidationsEnd.pdf`.) Refer to Figure [11-32](#449379_2_En_11_Chapter.xhtml#Fig32); in this example I am using the Name field.

![](images/449379_2_En_11_Chapter/449379_2_En_11_Fig32_HTML.jpg)

A screenshot of the text field properties page has 8 tabs, of which the format is selected. It has a drop-down menu to select the format category as custom, followed by special options, including a few lines of code for custom format script and custom keystroke script.

Figure 11-32

A custom format script can be added if you want default text using the Format tab

```
if (!event.value){ event.value = "What is your First and Last Name?";
event.target.textColor = color.gray;
}else{
event.target.textColor = color.black;
}
```

Remember to use the validation code for each field from earlier in this chapter if it is not already applied. Refer to Figure [11-33](#449379_2_En_11_Chapter.xhtml#Fig33); in this example I am using the Name field.

![](images/449379_2_En_11_Chapter/449379_2_En_11_Fig33_HTML.jpg)

A screenshot of the text field properties page has 8 tabs, of which the validate tab is selected. It has 3 checkboxes for field value not validated, range of the field value, and custom validation script. The third option to run a custom validation script is selected, followed by a few lines of code.

Figure 11-33

Run the custom validation script on the Validate tab

However, for the telephone validation in this field, a minor adjustment needs to be made to the custom format script to work with three colors rather than two and still get an alert message. Refer to Figure [11-34](#449379_2_En_11_Chapter.xhtml#Fig34).

![](images/449379_2_En_11_Chapter/449379_2_En_11_Fig34_HTML.jpg)

A screenshot of the text field properties has 8 tabs, of which the format tab is selected. It has a drop-down menu to select the format category as custom, followed by customer options that include a few lines of code for custom format script. It has an edit button on the right side of the page.

Figure 11-34

Changing the custom format script on the Format tab

Here is the custom format code:

```
var rgexTele = /^[(]{0,1}[0-9]{3}[)]{0,1}[-\s\.]{0,1}[0-9]{3}[-\s\.]{0,1}[0-9]{4}$/;
if (!event.value){
event.value = "Example: 999-999-9999";
event.target.textColor = color.gray;
}
else if(rgexTele.test(event.value)){
event.target.textColor = color.black;
}
else{
event.target.textColor = ["RGB",1,0,0]; //Change text to red
}
```

Note

Be cautious using this method with the phone number with two variables because it could cause an issue and may not display correctly. Also, don’t use default text when a field is formatted to Date to avoid errors and warnings in Acrobat; when a field is formatted to Date, it cannot accept any other text.

## Summary

In this chapter, you learned several ways to add validation to radio buttons and text fields. You also saw a variety of ways to use the validation in combination with formatting to create fields that will alert clients when they chose or typed incorrect information into a field, as well as how to hide select radio buttons.

Besides warning clients, radio buttons can be used to adjust shipping rates when used in an invoice or order form. They can also be used to refresh or reset a value so your client sees the correct information.

You then looked at some additional formatting that you can apply to buttons, radio buttons, and check boxes using a document JavaScript.

Finally, you also explored a variety of regular expressions that can be used in the Text Field Properties dialog box’s Format and Validation tabs rather than just relying on the formatting that comes with Acrobat, such as Special.

In the next chapter, you will be working with drop-down lists. You’ll see how they can interact with text fields and other drop-down lists.

# 12. Working with Dropdown Lists

So far you have looked at ways to improve text boxes, buttons, radio buttons, and check boxes with JavaScript. However, there are other types of fields that can be dynamic as well.

**Chapter Goal:** You are going to take a look at incorporating JavaScript with drop-down lists, also known as *combo boxes*, so that information will be accurately entered or calculated to avoid clients entering incorrect information.

Note

If you want to work along in this lesson or review the final result, download the Chapter [12](#449379_2_En_12_Chapter.xhtml) files from the link in the introduction. The file with the label “Start” is the file without the code, and the file with the label “End” is the final result. You will also find folders with original MS Word and PDF files if you would like to edit them and a folder containing the original scripts if you would like to add them to your own PDF forms.

If you are creating your form from an original PDF, refer to the “Forms Review” section in Chapter [1](#449379_2_En_1_Chapter.xhtml).

Remember that to view the properties of a field, you must select the Prepare Form tool; only then can you right-click or double-click a field to review its properties.

## Current Skills Request Form

In a small company, an employer may want to ask their employees to submit a form to HR that informs them of the employee’s current skill set. In this example, an employee selects the name of their department from the drop-down list, and their name and some of their information are automatically entered. If their department is not on the list, they can still type that information themselves into the various text fields. See the file `TMC_RequestForm_End.pdf`. Refer to Figure [12-1](#449379_2_En_12_Chapter.xhtml#Fig1).

![](images/449379_2_En_12_Chapter/449379_2_En_12_Fig1_HTML.jpg)

A page presents various text fields. The department name, Accounting, selected from the drop-down, pre-fills the department contact, department title, department e mail, department number, and department phone number.

Figure 12-1

The Current Skills Request Form project

This automatic action has two parts: a document JavaScript and a script attached to the drop-down list. You will look at the document JavaScript first. Refer to Figures [12-2](#449379_2_En_12_Chapter.xhtml#Fig2) and [12-3](#449379_2_En_12_Chapter.xhtml#Fig3).

![](images/449379_2_En_12_Chapter/449379_2_En_12_Fig3_HTML.jpg)

A screenshot of the document Java Scripts dialog box with the script name as, set select values. It has a selected close button. The editor below has a code entered.

Figure 12-3

The Document JavaScripts dialog box with the script SetSelectValues entered with the Add button

![](images/449379_2_En_12_Chapter/449379_2_En_12_Fig2_HTML.jpg)

A screenshot of the JavaScript icon with the add option and a document Java Scripts tool.

Figure 12-2

Enter the JavaScript tool and choose the Document JavaScripts subtool

The document JavaScript I’ve created is called `SetSelectValues`. I entered the following code into the editor:

```
//Global Document SetSelectValues Script
// Place all prepopulation data into a single data structure
var DeptInfo = { Accounting:{ contact: "Kathy Jones",
title: "Chief Officer",
email: "accounting@tmc.com",
deptnum: "tmc1434",
deptphone: "999-999-9922"},
Engineering:{ contact: "Frank R. Smith",
title: "Senior Specialist",
email: "engineering@tmc.com",
deptnum: "tmc1435",
deptphone: "999-999-9921" },
"Graphic Department":{ contact: "Nancy Smith",
title: "Artwork Planner",
email: "graphics@tmc.com",
deptnum: "tmc1436",
deptphone: "999-999-9923" },
ITSupport:{ contact: "Troy Carson",
title: "Official Coder",
email: "it@tmc.com",
deptnum: "tmc1437",
deptphone: "999-999-9927" },
Marketing:{ contact: "Janice Walker",
title: "Marketing Advisor ",
email: "marketing@tmc.com",
deptnum: "tmc1438",
deptphone: "999-999-9925" },
"Mine Site Safety":{ contact: "Rick James",
title: "Safety Officer",
email: "mss@tmc.com",
deptnum: "tmc1439",
deptphone: "999-999-9924"},
};
function SetSelectValues(cDeptName)
{
this.getField("DeptContact").value = DeptInfo[cDeptName].contact;
this.getField("DeptTitle").value = DeptInfo[cDeptName].title;
this.getField("DeptEmail").value = DeptInfo[cDeptName].email;
this.getField("DeptNumber").value = DeptInfo[cDeptName].deptnum;
this.getField("DeptPhone").value = DeptInfo[cDeptName].deptphone;
}
```

`SetSelectValues` is a function that calls upon an array variable called `DeptInfo`. Remember that an array/object literal is a complex variable that can hold multiple pieces of information. In the previous array, there are six departments.

*   Accounting

*   Engineering

*   Graphic Department

*   IT Support

*   Marketing

*   Mine Site Safety

Each department has five text fields that must be filled in.

*   Contact

*   Title

*   Email

*   Department Number

*   Department Phone Number

In a sense, this has become an array within an array, or a *nested array*, and you can continue to add to this array if you had more departments or employees with additional contact information.

Note

This type of array though it is indexed appears like an assosiative array mentioned in Chapter [9](#449379_2_En_9_Chapter.xhtml). In the case of JavaScript, a better term is an *associative syntax* when referring to these object literals within the square brackets, [ ]. In this case, it is using [cDeptName]. Yet JavaScript arrays must rely on numbered indexes, but as objects they can have named indexes. Rather than using indexes or the numbers 0 to 3 to call upon a variable, it uses words to call information or object literals/variables. It is also nested in that it can hold another object literal within an object literal. Later in this chapter, you will see another example that is also nested.

The last part the function `SetSelectValues` must now draw in each of these parts from the associative syntax or object so that when a selection from the drop-down list is chosen, the correct information or value is filtered in the correct order into the text fields.

Notice how each field is calling for an item called `cDeptName`; however, you don’t see it anywhere in the global function as a variable or part of the associative syntax. What is this? `cDeptName` refers to the drop-down field DepartmentNames. Refer to Figure [12-4](#449379_2_En_12_Chapter.xhtml#Fig4).

![](images/449379_2_En_12_Chapter/449379_2_En_12_Fig4_HTML.jpg)

A screenshot of several text fields with department names, department contact, department title, department email, department number, and department phone. The department names is highlighted.

Figure 12-4

The drop-down and text fields in the Prepare Form Tool view

For this drop-down list field, I have checked “Allow user to enter custom text” on the Dropdown Options tab. This way, if a department is not on the list, the user can enter the information themselves. Also check “Commit selected value immediately” to allow the text to fill the other fields as soon as the drop-down selection is released. It is optional to check the “Check spelling” box. In this case, we may not want to sort items alphabetically, so I left it unchecked. Refer to Figure [12-5](#449379_2_En_12_Chapter.xhtml#Fig5).

![](images/449379_2_En_12_Chapter/449379_2_En_12_Fig5_HTML.jpg)

A screenshot of the dropdown properties dialog box that lists the tabs under options with item, export value, item list with a drop-down, and enabled options of allow user to enter custom text, check spelling, and commit selected value immediately.

Figure 12-5

The Dropdown Properties dialog box’s Options tab that is referenced in the document JavaScript

Now, in the same field, go to the Format tab. Refer to Figure [12-6](#449379_2_En_12_Chapter.xhtml#Fig6).

![](images/449379_2_En_12_Chapter/449379_2_En_12_Fig6_HTML.jpg)

A screenshot of dropdown properties dialog box that lists the options under format tab with select format category as custom, custom format script, and custom keystroke script. There are edit buttons for the scripts.

Figure 12-6

The Format tab with a custom setting and a custom keystroke script

Enter the following JavaScript into the Format tab’s custom keystroke script section:

```
// Custom Format Script
if( event.willCommit ){
if(event.value == " "){
this.resetForm(["DeptContact","DeptTitle","DeptEmail","DeptNumber",
"DeptPhone"]);
}
else{
SetSelectValues(event.value);
}
}
```

So, you have created this field and applied JavaScript to the custom keystroke script area (see Figure [12-6](#449379_2_En_12_Chapter.xhtml#Fig6)). The script “will commit” because this was also checked on the Options tab (see Figure [12-5](#449379_2_En_12_Chapter.xhtml#Fig5)) and do one of two things:

*   If you set the drop-down list to blank, all the text fields will become blank or have `" "`. The list will reset these form fields.

*   Otherwise (`else`), it will make a connection with the function `SetSelectValue` found within the document JavaScript so that the information or values will flow through to the text fields.

Without this trigger to the function, the drop-down field would not be able to place any data in the drop-down list. Note that if the selection is set back to blank information in the drop-down list, then the fields will reset themselves immediately to blank because the drop-down list is part of the `willCommit` event. As mentioned earlier, a person who cannot find their information in the list could enter their own data in the blank text areas or below it in other text fields as required, or alternatively you could set these fields to read-only on the General tab.

Note

No JavaScript code was applied to the text fields.

## Parts Order Form

Sometimes a client must search through multiple items and then through multiple subitems to find the exact information or item they are looking for. For instance, in a retail job setting, there may be more than one type of screw, more than one size of T-shirt, or more than one color of product. In just about any job, you can find items that have several variations, and those variations might have different prices depending upon their size and material.

The parts order form is a simplified example of this scenario. See the file `TMC_PartsOrderForm_End.pdf`. Refer to Figure [12-7](#449379_2_En_12_Chapter.xhtml#Fig7).

![](images/449379_2_En_12_Chapter/449379_2_En_12_Fig7_HTML.jpg)

A page with a table that has 7 columns titled hash, label, name, description, price of each, quantity, and item-total, with sub-total, less discount, tax, shipping, and total under the last column, and an additional comments box.

Figure 12-7

Drop-down lists in the parts order form project

This time the form contains two connecting drop-down lists: Name (Row1.item) and Description (Row1.Description). There are at least five of these sets. Refer to Figure [12-8](#449379_2_En_12_Chapter.xhtml#Fig8).

![](images/449379_2_En_12_Chapter/449379_2_En_12_Fig8_HTML.jpg)

A screenshot of the name and description with drop-down lists. The name includes drills, lubricants, gears, and mining, while the description includes small, medium, large, and very large.

Figure 12-8

The two connecting drop-down lists found in the current project

Once the user chooses a name, they can choose a description. Once the description is chosen, a price is then set for the part as the user tabs out. Refer to Figure [12-9](#449379_2_En_12_Chapter.xhtml#Fig9).

![](images/449379_2_En_12_Chapter/449379_2_En_12_Fig9_HTML.jpg)

A table has five columns titled hash, label, name, description, and price of each, with its corresponding attributes of name as drills, description as small, and price as $ 19.95.

Figure 12-9

The Description drop-down list changes the Price field

All the customer has to do is decide how many parts he wants to buy, and the order form prices and total start to fill in. Discounts as well as tax and shipping can later be added as well by the user. We will look at those fields later. Refer to Figure [12-10](#449379_2_En_12_Chapter.xhtml#Fig10).

![](images/449379_2_En_12_Chapter/449379_2_En_12_Fig10_HTML.jpg)

A page with a table that has 7 columns titled hash, label, name, description, price of each, quantity as 3, and item-total of $ 59.85, with the sub-total, less discount, tax, shipping, and total of $ 59.85.

Figure 12-10

When a client enters a quantity, this changes the total price

Let’s look at this in depth. Once again you will need to create some global document JavaScript. Refer to Figure [12-11](#449379_2_En_12_Chapter.xhtml#Fig11).

![](images/449379_2_En_12_Chapter/449379_2_En_12_Fig11_HTML.jpg)

A page with the JavaScript icon at the top, with the add tab and the document JavaScript tool. It includes the document Java Scripts dialog box with the script name as set each value, and set each value with add items and calculate row total. Close button is highlighted.

Figure 12-11

Document JavaScripts dialog box settings that will affect various fields in the project in the dialog box

As you can see, with the addition of another connecting drop-down and related drop-downs on other rows, more JavaScript is required.

The first thing you need to do is create your associative syntax/object literal array and the function `addItems`. Each associative syntax `oArray or Object` is separated with a comma and brackets, [].

```
// Global Document addItems
var oArray = {
Drills: [ ["-","None"], ["Small",19.95], ["Medium",29.95], ["Large",39.95], ["Extra Large",44.95]],
Lubricants: [ ["-","None"], ["Oil",69.95], ["Grease",49.95], ["Super
Smooth",79.95],["Averge",139.95]],
Gears:  [ ["-","None"], ["Small",149.95], ["Medium",159.95], ["Large",219.95], ["Very Large",339.95]],
Mining:[ ["-","None"], ["Hard Hat",39.95], ["Pick",29.95], ["Axe",19.95],["Shovel",49.95]]
};
function SetDescriptionEntries(){
if(event.willCommit){
var cRowName = event.target.name.split(".").shift();
var list = oArray[event.value];
if( (list != null) && (list.length > 0) ){
this.getField(cRowName + ".Description").setItems(list);
}
else{
this.getField(cRowName + ".Description").clearItems();
}
this.getField(cRowName + ".Each").value = 0;
}
}
```

Note

If for some reason on another page you need to add a separate set of drop-down lists, you must create another document JavaScript like `addItems2` and then change the associative syntax/object’s name and the function’s name to avoid a clash or items being called from the wrong object.

In the previous single drop-down example, you had to take the data from the item in the drop-down and copy it to other text fields. Once the array has pasted the item into dropdown-down list (Row1.item), this time you have to copy the subitems to another drop-down list call `Row1.Description`. Again, if no value is found in the name drop-down or items, the description value remains blank or cleared and no choice is available. In this example, for array (`oArray`), the associative syntax is divided into four item names, and part of that syntax will later inhabit fields that have part of their name as “Row” or the variable `cRowName`. In this example the following four items would begin with the cRowName populate the drop-down list that corosponds with it which is Row1.item. They are:

*   Drills

*   Lubricants

*   Gears

*   Mining

Each part of the associative syntax or object for `name` or (item) divides into another drop-down for a subitem `Description` and a text field that contains the price/each item Row1.Each. You will see shortly how this next function is used to separate the price that is called upon.

Then you need to add `SetEachValue` function as a separate document JavaScript (Figure [12-11](#449379_2_En_12_Chapter.xhtml#Fig11)).

```
function SetEachValue(){
if(!event.willCommit){
var cRowName = event.target.name.split(".").shift();
var nSelection = 0;
if(!isNaN(event.changeEx))
nSelection = event.changeEx;
this.getField(cRowName + ".Each").value = nSelection;
}
}
```

The fields have names like `Row1.item`, `Row2.item`, and so on. Using this type of order or schema allows the document JavaScript to easily call each item through the function. Because this type of form is so complex, using ordered and meaningful names is crucial.

If the event will not commit, because of the exclamation point (!) override, then the value is 0\. This takes care of any missing numbers in the object array. If a non-number (NaN) like a word was entered instead, the value will remain 0\. Otherwise, the function `SetDescriptionEnteries (`which we looked at earlier) will enter the value found in the associative syntax for price/each item and change upon each new selection.

The `event.changeEx` is often used with list menus and combo boxes to assist in the changing of the selections. Refer to this link for more details:

[`https://opensource.adobe.com/dc-acrobat-sdk-docs/library/jsapiref/JS_API_AcroJS.html?highlight=changeex#event-properties`](https://opensource.adobe.com/dc-acrobat-sdk-docs/library/jsapiref/JS_API_AcroJS.html?highlight=changeex#event-properties)

The setting for the price of each item was chosen from the `description` drop-down list. The description and price are split into their respective fields using the `split` method, which starts the split at the variable cRowName or item to break down the array into the respective fields, which contain the period in the name `(".")` . You can see this in the methods called `.split()` and `.shift().` The `shift` method removes the first item in the object associative example `["Oil",69.95]` and leaves only the price, which will now go into the price of each item field called Row1.Each.

There are other methods that you can use to add or remove elements or object literals from the associative syntax. Here are a few explained for your reference:

*   `pop()`: Removes the last element from the array and returns the remaining elements (example: `["Oil",69.95]` = `Oil`)

*   `push()`: Adds a new element to the end of an array and gives a new length count

*   `unshift()`: Adds new elements to the beginning an array and gives a new length count

*   `splice()`: Adds/removes elements from an array anywhere

*   `join():` Joins all array elements into a string

Also, because each element is separated by a comma, this helps in the shifting and sorting as well.

For more information on methods and what else they can do, visit these links:

*   [`www.w3schools.com/js/js_array_methods.asp`](http://www.w3schools.com/js/js_array_methods.asp)

*   [`www.w3schools.com/jsref/jsref_split.asp`](http://www.w3schools.com/jsref/jsref_split.asp)

*   [`www.w3schools.com/jsref/jsref_obj_array.asp`](http://www.w3schools.com/jsref/jsref_obj_array.asp)

*   [`https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)

I created the function `SetEachValue` here in the document JavaScript because I will be using it multiple times and I want to be able to call on it as many times as I like depending on how many drop-downs I have. Also, I do not have to rewrite this code for each field, which could lead to errors.

The last document JavaScript function (Figure [12-11](#449379_2_En_12_Chapter.xhtml#Fig11)) is used to calculate the item total (`calculateRowTotal`) of the row. However, this is not the Subtotal or Final Total field, which can be determined only when you have finished adding up multiple items. Refer to the file `TMC_PartsOrderForm_End.pdf`.

```
// Global Document calculateRowTotal
function CalculateRowTotal()
{
var cRowName = event.target.name.split(".").shift();
event.value = this.getField(cRowName + ".Each").value * this.getField(cRowName + ".Qty").value;
}
```

This function once again uses the split and `shift` methods. However, only the price (`.Each`) is multiplied against the quantity field (`.Qty`) so that the Row (`.ItemTotal`) price is now calculated.

Like the `SetEachValue()` function, this will be used multiple times, so that is why you create it in the Document JavaScripts tool. Whenever you have a function that you think you will use many times, it’s good to set it up in the Document JavaScripts tool so that you can call upon it later.

Now you will take a look at the first row of your parts order form to see where it all fits. You will skip the Label field, which is formatted to None since it is not part of the project. Refer to Figure [12-12](#449379_2_En_12_Chapter.xhtml#Fig12).

![](images/449379_2_En_12_Chapter/449379_2_En_12_Fig12_HTML.jpg)

A table with seven columns titled hash, label, name, description, price of each, quantity, and item-total, with its corresponding data in a row, with drop-downs for name and description.

Figure 12-12

First row in the Part Order form

The first drop-down, `Row1.item`, is set up like Figure [12-13](#449379_2_En_12_Chapter.xhtml#Fig13) so that it can reference the associative syntax/object called `oArray`. On the Options tab, you do not put anything in the Item or Export Value fields; these fields need to remain blank and are then filled in with the document JavaScript as you can see in the “End” example.

![](images/449379_2_En_12_Chapter/449379_2_En_12_Fig13_HTML.jpg)

A screenshot of the drop-down properties dialog box that lists the tabs under options tab, with item as a dot, export value, and item list with drills, lubricants, gears, and mining. It has a text field for the name with the text, Row 1 item.

Figure 12-13

Drop-down properties on the Options tab for the first drop-down list

In the Format tab’s Custom Keystroke Script area, you call upon the `SetDescriptionEntries` function, which is found in the document JavaScript’s `addItems script`. This references the associative syntax object `oArray` and fills the next drop-down field of Row1.Description and text field Row1.Each with information. Transferring the data based on what is chosen in the .item drop-down list when required to the `.Description` drop-down list and `.Each field`. Refer to Figure [12-14](#449379_2_En_12_Chapter.xhtml#Fig14).

![](images/449379_2_En_12_Chapter/449379_2_En_12_Fig14_HTML.jpg)

A screenshot of the drop-down properties dialog box that lists the options under format tab with select format category as custom, custom format script, and custom keystroke script. Edit buttons are next to scripts.

Figure 12-14

The Dropdown Properties dialog box’s Format tab with custom keystroke script in the first drop-down list (.item)

```
// Format Custom Keystroke for Name
SetDescriptionEntries();
```

For all the other Name item drop-downs, you can repeat these steps so that on the Options tab it will reference the Format tabs and fill in the drop-down list. To see how you use this same function in the other drop-down fields, refer to Figure [12-15](#449379_2_En_12_Chapter.xhtml#Fig15). In my “End” file, you could right-click the field and then, from the pop-up menu, view the properties.

![](images/449379_2_En_12_Chapter/449379_2_En_12_Fig15_HTML.jpg)

A screenshot of various text fields for the name and description. It includes the right-click menu options with highlighted properties list for row 2 item in name.

Figure 12-15

Drop-down fields requiring same options and format settings, which are accessed by right-clicking the field and choosing Properties from the menu

The description drop-down Row1.Description is left blank, and you do not preset anything on its Options tab. JavaScript will do that work for you. Refer to Figure [12-16](#449379_2_En_12_Chapter.xhtml#Fig16).

![](images/449379_2_En_12_Chapter/449379_2_En_12_Fig16_HTML.jpg)

A screenshot of the description text field with text, row 1 dot description. A dropdown properties dialog box below, with options tab highlighted includes, item, export value, and item list.

Figure 12-16

Dropdown Properties dialog box’s Options tab in the second drop-down list in the first row

The `SetEachValue` function is now called upon on the Format tab’s Custom Keystroke Script area to get the value into the price of each field (.Each) and can be added to the other (.Description) fields. Remember, if the price was left out of the associative syntax or was text, the price will come in as 0 or $0.00. Refer to Figure [12-17](#449379_2_En_12_Chapter.xhtml#Fig17).

![](images/449379_2_En_12_Chapter/449379_2_En_12_Fig17_HTML.jpg)

A screenshot of the drop-down properties dialog box that lists the options under format tab with select format category as custom, custom format script, and custom keystroke script with entry of set each value parentheses semicolon. Edit buttons are next to scripts.

Figure 12-17

The custom keystroke script for the second drop-down list (.Description)

```
// Format Custom Keystroke for Description
SetEachValue();
```

This code is entered into each description drop-down list. Refer to Figure [12-18](#449379_2_En_12_Chapter.xhtml#Fig18).

![](images/449379_2_En_12_Chapter/449379_2_En_12_Fig18_HTML.jpg)

A screenshot of the various text fields of description with row 1, row 2, row 3, row 4, and row 5\. Row 1 dot description is highlighted.

Figure 12-18

Drop-down fields requiring the same options and format settings

The price of each field called Row1.Each has no JavaScript in it. Its settings come from the `SetEachValue` function in the previous drop-down list (.Description). However, the text field has been formatted to Number with two decimal places with a currency symbol. Note that the Currency symbol was not added to the document JavaScript; just the number was. These fields were set to read-only on the General tab. Refer to Figure [12-19](#449379_2_En_12_Chapter.xhtml#Fig19).

![](images/449379_2_En_12_Chapter/449379_2_En_12_Fig19_HTML.png)

A screenshot of drop-down list options for price of each, with text fields, with row 1 each, highlighted. It has a text field properties dialog box that lists options under format tab, with select format category as number, decimal places to 2, separator style, and currency symbol and location.

Figure 12-19

The text field properties on the Format tab are set to Number for (.Each)

The same is true of the quantity column, Row1.Qty. It contains no JavaScript. It just needs to be formatted to Number with no currency symbol and the decimal place set to 0\. This field will be referenced shortly but is not set to Read Only. Refer to Figure [12-20](#449379_2_En_12_Chapter.xhtml#Fig20).

![](images/449379_2_En_12_Chapter/449379_2_En_12_Fig20_HTML.png)

A screenshot of drop-down list options for quantity with text fields, with row 1 quantity selected. It has a text field properties dialog box that lists options under format tab, with select format category as number, decimal places to 0, separator style, currency symbol as none, and location.

Figure 12-20

The Text Field Properties dialog box’s Format tab are set to Number without a currency symbol for (.Qty)

The final field, Row1.itemTotal (formatted to Number with two decimal places and the currency symbol as in Figure [12-19](#449379_2_En_12_Chapter.xhtml#Fig19)), now multiplies the information from the Price of Each and Quantity fields on the Calculate tab as a Custom calculation script. Again, the calculation function was referenced from the document JavaScript section and can be applied to multiple fields. Refer to Figure [12-21](#449379_2_En_12_Chapter.xhtml#Fig21).

![](images/449379_2_En_12_Chapter/449379_2_En_12_Fig21_HTML.png)

A screenshot of drop-down list options of item total with item 1 text field selected. It has a text field properties dialog box that lists options under calculate tab, where calculate row total is entered in custom keystroke script.

Figure 12-21

Text field properties set in the Calculate tab’s custom calculation script for (.itemTotal)

```
// Calculate Script Items
CalculateRowTotal();
```

I set the itemTotal fields to read-only on the General tab.

Now make sure that your fields, SubTotal, Discount, Tax, Shipping, and Total, are all formatted to Number with two decimal places and a currency symbol as in Figure [12-19](#449379_2_En_12_Chapter.xhtml#Fig19). SubTotal and Total should be set to read-only on the General tab so that the client does not overwrite a number. Refer to Figure [12-22](#449379_2_En_12_Chapter.xhtml#Fig22).

![](images/449379_2_En_12_Chapter/449379_2_En_12_Fig22_HTML.jpg)

A page with a table that has 5 rows titled sub-total, less discount, tax, shipping, and total. Sub total and total are set to $ 0.00\. There are various text fields below with text that read sub-total, discount, tax, shipping, and total.

Figure 12-22

Final text fields that need to be calculated in the project in preview and edit modes

In past invoice forms, you used the value or Simplified Field Notation for these fields. Here you might be tempted to use these options to add up your `subTotal` and final `Total` fields. Refer to Figure [12-23](#449379_2_En_12_Chapter.xhtml#Fig23).

![](images/449379_2_En_12_Chapter/449379_2_En_12_Fig23_HTML.jpg)

A screenshot of the text field properties dialog box that lists the options under calculate tab, with the enabled value is the sum of rows, and simplified field notation.

Figure 12-23

On the Calculate tab, in basic situations, the SubTotal and Total calculations do not require JavaScript

While the previous examples do work, I found them to be a bit buggy, and they did not refresh well. We don’t want users of our form to get frustrated and confused as to why their inputs are not working smoothly. In the end, what I discovered was that you need to add or subtract everything with JavaScript, and then the form will run smoothly. In the Subtotal field, I placed the following script into the Calculate tab’s Custom Calculation Script field. Refer to Figure [12-24](#449379_2_En_12_Chapter.xhtml#Fig24).

![](images/449379_2_En_12_Chapter/449379_2_En_12_Fig24_HTML.jpg)

A screenshot of text field properties dialog box that lists the options under calculate tab, with the enabled custom calculation script that has variables for row item totals, and a highlighted close button.

Figure 12-24

Text field properties on the Calculate tab for the SubTotal field

```
//Subtotal Script
var a = this.getField("Row1.itemTotal");
var b = this.getField("Row2.itemTotal");
var c = this.getField("Row3.itemTotal");
var d = this.getField("Row4.itemTotal");
var f = this.getField("Row5.itemTotal");
event.value =a.value + b.value + c.value + d.value + f.value;
```

As you can see, this is like the example you tried in earlier lessons such as Chapter [6](#449379_2_En_6_Chapter.xhtml). For the Total field, refer to Figure [12-25](#449379_2_En_12_Chapter.xhtml#Fig25).

![](images/449379_2_En_12_Chapter/449379_2_En_12_Fig25_HTML.jpg)

A screenshot of text field properties dialog box that lists options under calculate tab, with the enabled custom calculation script that has variables for sub total, discount, tax, and shipping, disabled buttons of value is not calculated, value is the field, and simplified field notation.

Figure 12-25

Text field properties on the Calculate tab for the Total field

Then for the Grand Total or Total field, I used this custom calculation script:

```
//Total Script
var g = this.getField("SubTotal");
var h = this.getField("Discount");
var j = this.getField("Tax");
var k = this.getField("Shipping");
event.value =g.value - h.value + j.value + k.value;
```

This worked the best for me for this basic example.

Remember that if your fields are not tabbing correctly, make sure to also check your Tab Order numbers and Field Calculation order using the Prepare Form More menu as you did in earlier chapters. Testing your form over and over plays an important role in getting calculations right. You’ll also want to add a Reset button for clients that need to reset their complex calculations when they make mistakes.

## Drop-Down Lists with Select Dates and Discounts

A drop-down list formatted to a date could trigger a discount on a specific or current price.

In this example, in the file `Discount_End.pdf`, a user could enter a price and then select a date from a drop-down list to see what the discount percentage is, as well as the price discount. Then they will see what the new final price would be. In this example, you do not need to use document JavaScript. Refer to Figure [12-26](#449379_2_En_12_Chapter.xhtml#Fig26).

![](images/449379_2_En_12_Chapter/449379_2_En_12_Fig26_HTML.jpg)

A page presents the custom price, discount date with a drop-down list, discount in %, price discount, and final price, with its corresponding values of $ 500.00, 03 09 2023, 25.00%, $ 125.00, and $ 375.00.

Figure 12-26

Calculate the Price discount based on a set date

For your own project, this might be useful if you were calculating the price of some items that would normally be a certain price, but on the sale date they would be worth less.

The drop-down list in this case has some set dates added into the drop-down list, and each is given a percentage of an export value on the Options tab. For example, 0.25 is 25 percent. Refer to Figure [12-27](#449379_2_En_12_Chapter.xhtml#Fig27).

![](images/449379_2_En_12_Chapter/449379_2_En_12_Fig27_HTML.jpg)

A screenshot of a text field with discount date and a screenshot of dropdown properties dialog box that lists options tab, with item as 03 09 2023, export value as 0.25, item list with selected 03 09 2023 from the drop-down, and enabled commit selected value immediately.

Figure 12-27

Add item dates and values on the Options tab and choose “Commit selected value immediately”

I then set the category to Date on the Format tab and made sure to choose a similar format to how I typed and added the items on the Options tab. Refer to Figure [12-28](#449379_2_En_12_Chapter.xhtml#Fig28).

![](images/449379_2_En_12_Chapter/449379_2_En_12_Fig28_HTML.jpg)

A screenshot presents the drop-down properties dialog box that lists the options under format tab, with select format category as date, and date format as m m slash d d slash y y y y, from the drop-down list.

Figure 12-28

Format your drop-down field to a Date on the Format tab

Note

Only the text fields can use the date picker and not the drop-down list when formatted to Date, as we are using only a select date from the list.

The next three text fields are set to Read Only. DiscountPercent, which is formatted to Percent. Then the fields PriceDiscount and GrandTotal are both formatted to Number, a currency symbol, and two decimal places. For these three fields, I then set the Calculate tab with a custom calculation script. I am then able to view the export value of the selection in the drop-down list DiscountDate. Refer to Figure [12-29](#449379_2_En_12_Chapter.xhtml#Fig29).

![](images/449379_2_En_12_Chapter/449379_2_En_12_Fig29_HTML.jpg)

A screenshot of text field properties dialog box that lists the options under calculate tab with the enabled custom calculation script. Event dot value = this dot get Field, Discount Date in parentheses, value semicolon, is entered in the script box.

Figure 12-29

Set the custom calculation script on the Calculate tab

For the text field DiscountPercent, you can get the value of the date by calling on the value of DiscountDate.

```
event.value=this.getField("DiscountDate").value;
```

For the text field PriceDiscount, you can multiply the current price by the discount date value.

```
event.value= this.getField("CurrentPrice").value*this.getField("DiscountDate").value;
```

For the text field GrandTotal, you can subtract the price discount from the current price.

```
event.value= this.getField("CurrentPrice").value-this.getField("PriceDiscount").value;
```

## Final Thoughts

When making a complex form with multiple drop-downs, it’s best to plan it out on paper or a drawing program first and decide what you would like to do. You may need an array or an associative syntax with object literals or a global function, but write it all out first.

Once you’ve built the form, similar to what I have done, make sure that number areas are formatted to Number or Custom correctly and then add the JavaScript. Test the form or maybe get others to test it. It’s important to work out all the bugs before your customers use it. The focus here is to avoid client errors and frustration and to get the results you want when working with fields in a form.

### Load a Lengthy Single Drop-Down or List Menu

While not as complex as the previous examples, occasionally you may need to load only a single drop-down list with 30 or more items that don’t affect other fields or menus in the form. See the Load Single Menu folder in this example. Refer to the file `TMC_LoadDropDown_end.pdf`.

Having to enter this into a drop-down or list box menu can be labor intensive: you must select the Options tab, enter an item name, enter an export value, and then click the Add button for each item. Also, you may want the same information in multiple menus that change often (see the file `Time_Sheet_End_multiload.pdf`). To save time for lengthy single menus, I have included these extra files where I have put all my code for the drop-down (combo box) or list box into the global document JavaScript area. I call this function `LoadOptions`. You can view the code on the following pages and will find it in the supplied text file as well in the Scripts folder. As always, make sure that your fields have the same name as what is written in JavaScript so that the connection will work correctly. Refer to Figure [12-30](#449379_2_En_12_Chapter.xhtml#Fig30).

![](images/449379_2_En_12_Chapter/449379_2_En_12_Fig30_HTML.jpg)

A screenshot of a JavaScript icon with the add option and the document JavaScripts tool. It includes the document JavaScript dialog box with the script name as load options, and load options box, and an editor with code. A dropdown list next to it highlights select mineral option.

Figure 12-30

The LoadOptions script in the document JavaScripts dialog box and the referenced drop-down will load the items and values

The following code is added to the editor in the document JavaScripts tool in the file `TMC_LoadDropDown_end.pdf`:

```
// define array of entries and export values for drop-down array
// define array of minerals and abbreviations
var aMinerals = new Array(["Select Mineral", ""],
["Diamond", "DI"],
["Emerald", "EM"],
["Garnet", "GA"],
["Opal", "OP"],
["Ruby", "RB"],
["Sapphire", "SP"]);
// function to load a combo/list box with an array of values
function LoadOptions(oField, aValues) {
var bResult = false;
// load array of values into field object
if(oField.type != "combobox" && oField.type != "listbox") {
app.alert("Load Options function requires a combobox or listbox", 0, 0);
bResult = false;
}
else{
oField.setItems(aValues); // set values
bResult = true;
}
return bResult;
} // end LoadOptions
// load the data - comment out the following two lines after updating when only doing array changes to reset
LoadOptions(this.getField('Dropdown1'), aMinerals);
LoadOptions(this.getField('ListBox1'), aMinerals);
```

The previous example works for loading the drop-down list or list box only within the same form for both list items and export values. If you try to load it into another type of form field, you will get an error alert. These fields are referencing the array called `aMinerals`. Also notice that you can use single straight quotes (`'')` rather than double straight quotes (`"")` if you want to load and get a field. You can check that the `oFiled` and `aValues` of the array are loading and setting if you inspect the field’s Options tab.

If you are not using a list box, you can comment out that line or add more `LoadOptions` functions if other named drop-down fields require the same object literals that may change often. Remember to give all drop-down or list box fields a distinct name.

We will be discussing list boxes in more detail in the next chapter.

### Tips on Changing Custom Formatting for Drop-Down Lists with JavaScript

Like text fields, you can format most drop-down list combo boxes either using the Appearance tab properties or using the Document JavaScript dialog box, Actions tab, or Format tab set to Custom and the custom format script area.

Like with text fields, the properties that are available to this field are the ones mentioned in Chapter [10](#449379_2_En_10_Chapter.xhtml), which include `fillColor`, `strokeColor`, `lineWidth`, `borderStyle`, `textColor`, `textFont`, `textSize`, and `rotation`. You cannot use `fontFamily` as this is for the rich-text field settings or in annotations. Also, you cannot use alignment to, for instance, center the text; by default it will remain left aligned. Though with items with multiple words, you could add spaces in the text or even a `\t` to add a tab space like this in the earlier line of code:

```
["\tDiamond", "DI"],
```

However, after experimenting, I found that this appears to affect only the list box and drop-down list (combo box) when the user has tabbed out, not during selection. The same is true of `\n`. If you try to add a new line, you must tab out of the field for it to appear correctly. You would also need to expand the size of your drop-down’s height to accommodate this.

```
["\tDiamond\nMineral", "DI"],
```

As well, it appears that rich text, even if the user was to enter something custom into the list or if it cannot be formatted, will throw an error in the Debugger if you try to adapt this code in the custom Format area. Refer to this link for more details:

[`https://opensource.adobe.com/dc-acrobat-sdk-docs/library/jsdevguide/JS_Dev_AcrobatForms.html#44065`](https://opensource.adobe.com/dc-acrobat-sdk-docs/library/jsdevguide/JS_Dev_AcrobatForms.html#44065)

## Summary

In this chapter, while working with drop-down list and text boxes, you encountered some complex JavaScript. You learned how nested associative/object literals work like arrays and how you can extract information from those associative syntax functions. You also saw the benefit of document JavaScript and how writing the script in one location saved you time so you didn’t have to write multiple edits in several locations; you could just reference the script or other fields.

In the next chapter, you will take a more detailed look at probably one of the most underused fields: the list box. You’ve just seen, as with drop-down lists, that you can load text into them using document JavaScript. However, they’re not as compact as a drop-down, so what else can you do with them?

# 13. Working with List Boxes

**Chapter Goal:** The last main type of form field you are going to explore is list boxes. In my experience, list boxes are the most underused form field. While they may not be as compact as drop-downs, they are great for helping organize a list of priorities. Let’s explore some ideas for working with list boxes in various projects. Refer to Figure [13-1](#449379_2_En_13_Chapter.xhtml#Fig1).

Note

If you want to work along in this lesson or review the final result, download the Chapter [13](#449379_2_En_13_Chapter.xhtml) files from the link in the introduction. The file with the label “Start” is the file without the code, and the file with the label “End” is the final result. You will also find folders with original MS Word and PDF files if you would like to edit them and a folder containing the original scripts if you would like to add them to your own PDF forms.

If you are creating your form from an original PDF that contains no form fields, refer to the “Forms Review” section in Chapter [1](#449379_2_En_1_Chapter.xhtml).

Remember that to view the properties of a field, you must select the Prepare Form tool; only then can you right-click or double-click a field to review its properties.

## List Box Priority List with Control Buttons

Figure [13-1](#449379_2_En_13_Chapter.xhtml#Fig1) shows list boxes (see the file `TMC_PriorityList_End.pdf`). As you can see, there are list boxes and five buttons. I’ll discuss the lower text field and button later.

![](images/449379_2_En_13_Chapter/449379_2_En_13_Fig1_HTML.jpg)

A rectangular box with a list of 12 phrases has a blank box to its right, with add, delete, clear, up, and down buttons between them. The right box below has 3 phrases added, that are highlighted in the left boxes. Add to list field and add new priority button are between the top and bottom boxes.

Figure 13-1

List box priority project with buttons and a custom form field with a button that allows you to add your own priority to the final list

The Add button transfers the selected phrase one at a time to the right list box. The Delete button deletes a selected phrase from the right list. The Clear button resets the whole form. The Up and Down buttons reorder the selected phrase either up or down. You have complete control over your list of priorities.

Here’s how it works. You start with some global document JavaScript. Refer to Figure [13-2](#449379_2_En_13_Chapter.xhtml#Fig2).

![](images/449379_2_En_13_Chapter/449379_2_En_13_Fig2_HTML.png)

A Java Script icon and an add button. A dialog box on document Java script below has script name inserted as add to priority, with the same entered in text box below. A code for global add to priority script is in the editor window beneath script name. Close button is highlighted.

Figure 13-2

The document JavaScript added to the PDF project using the JavaScript tool and dialog box

This JavaScript added to the editor is called `addToPriority`:

```
//Global addToPriority Script
function addToPriorityList(cEntry){
var OrgLFld = this.getField("priorityList");
var bFound = false;
for(var i=0;i<OrgLFld.numItems;i++){
if(OrgLFld.getItemAt(i,false) == cEntry){
bFound = true;
break;
}
}
if(!bFound){
OrgLFld.insertItemAt({cName:cEntry, nIdx:-1});}
}
```

In this `addToPriorityList` function, you are selecting items from the qualityList list box form field via the (cEntry) and using the buttons to move those items to the priorityList list box form field. Notice that you are using a `for()` loop to loop through all the object literals found with the associative syntax (see Chapters [9](#449379_2_En_9_Chapter.xhtml) and [12](#449379_2_En_12_Chapter.xhtml)) that are found in the qualityList. When an item is found that will be added by the Add button, it will be broken out and inserted into the priorityList, but only once. You can see this with the variable `bFound`; it’s set to true and then false if an attempt is made to enter it again. You can enter it again only if it is cleared or deleted from the priorityList field.

The qualityList list box form field has been set up like this on the Options tab with its Items added, but no Export Value and Sort Items for alphabetical sorting is checked. No script is directly applied to it; it is only called upon.

The same is true for the priorityList; however, on the Options tab priorityList contains no entries and sort is left unchecked so that you can continue to order the entries the way you want in the list. Refer to Figures [13-3](#449379_2_En_13_Chapter.xhtml#Fig3), [13-4](#449379_2_En_13_Chapter.xhtml#Fig4), and [13-5](#449379_2_En_13_Chapter.xhtml#Fig5).

![](images/449379_2_En_13_Chapter/449379_2_En_13_Fig5_HTML.jpg)

A dialog box on list box properties. The options of item, export value, and item list is blank. 3 checkboxes to sort items, multiple selection, and commit selected value immediately are below. 4 buttons, add, delete, up, and down are disabled.

Figure 13-5

The List Box Properties dialog box on the Options tab for priorityList and “Sort items” is unchecked

![](images/449379_2_En_13_Chapter/449379_2_En_13_Fig4_HTML.jpg)

A dialog box on list box properties with tabs, general, appearance, position, options, actions, and selection change, where options is selected. Item name is entered as follow safety policies. An item list is created based on the priority list. Sort items is checked.

Figure 13-4

The List Box Properties dialog box on the Options tab for qualityList with “Sort items” checked

![](images/449379_2_En_13_Chapter/449379_2_En_13_Fig3_HTML.jpg)

A rectangular box with a list of 12 priority tasks and text field, quality list, has a blank box to its right, that has a text field, priority list, at the middle of the box. 4 buttons to add, delete, clear, up, and down are in between the boxes.

Figure 13-3

List boxes and buttons in the Prepare Form’s edit tool

In those fields, I have left “Multiple selection” unchecked as we are selecting one item at a time, and because we are not doing an actual calculation of values, the property “Commit selected value immediately” is left unchecked as well.

Only the buttons contain the actions on the Actions tab on the Mouse up trigger as they are what controls what happens between the two list boxes. Here we see the action of Run a JavaScript. Refer to Figure [13-6](#449379_2_En_13_Chapter.xhtml#Fig6).

![](images/449379_2_En_13_Chapter/449379_2_En_13_Fig6_HTML.jpg)

A dialog box of button properties with actions tab in view. Add an action section has options to select the trigger as mouse up and select the action as run a Java script. 5 buttons of add, delete, clear, up, and down are in the actions section in text fields, and normally.

Figure 13-6

The List Box Properties dialog box on the Actions tab for buttons and how the buttons appear in the Prepare Form view and again normally

### The Add Button

Refer to Figure [13-7](#449379_2_En_13_Chapter.xhtml#Fig7) and the Actions tab.

![](images/449379_2_En_13_Chapter/449379_2_En_13_Fig7_HTML.jpg)

An icon of a highlighted rectangular box with text, add, over it.

Figure 13-7

The Add button adds an item to the list box on the right

```
// Add Button
var OrgLFld = this.getField("qualityList");
if(OrgLFld){
var cEntry = OrgLFld.getItemAt(OrgLFld.currentValueIndices,false);
addToPriorityList(cEntry);
}
```

Notice how it calls on the function `addToPriorityList` using the `cEntry`, gets the items from the `qualityList`, and adds the item to the `priorityList` field only once and cannot duplicate. You will find that the method `currentValueIndices` is often seen in the code for interacting with a list or drop-down/combo box to set the value.

### The Delete Button

Refer to Figure [13-8](#449379_2_En_13_Chapter.xhtml#Fig8) and the Actions tab.

![](images/449379_2_En_13_Chapter/449379_2_En_13_Fig8_HTML.jpg)

An icon of a highlighted rectangular box with text, delete, over it.

Figure 13-8

The Delete button deletes list items from the priority list

```
// Delete Button
var OrgLFld = this.getField("priorityList");
var prev = OrgLFld.currentValueIndices;
if(OrgLFld.currentValueIndices > -1){
OrgLFld.deleteItemAt(OrgLFld.currentValueIndices);
if(OrgLFld.numItems > prev){
OrgLFld.currentValueIndices = prev;}
else if(OrgLFld.numItems > 0){
OrgLFld.currentValueIndices = prev-1;}
}
```

The Delete button’s script looks at what was previously added or just selected. Whatever is currently selected when the Delete button is clicked is removed from the priortyList list box form field.

### The Clear or Reset Button

Refer to Figure [13-9](#449379_2_En_13_Chapter.xhtml#Fig9) and the Actions tab.

![](images/449379_2_En_13_Chapter/449379_2_En_13_Fig9_HTML.jpg)

An icon of a highlighted rectangular box with text, clear, over it.

Figure 13-9

The Clear Button resets/clears the priority list

You could have simply used your actions to reset the form with no JavaScript code. However, sometimes you just want to clear a specific list, and reset alone is not enough.

```
// Clear Button
this.getField("priorityList").clearItems();
```

The `.clearItems()` method acts as a reset for the priortyList, and you saw an example of this code in Chapter [12](#449379_2_En_12_Chapter.xhtml) when we worked with the drop-down list.

### The Up Button

The Up button moves items up in the list. Refer to Figure [13-10](#449379_2_En_13_Chapter.xhtml#Fig10) and the Actions tab.

![](images/449379_2_En_13_Chapter/449379_2_En_13_Fig10_HTML.jpg)

An icon of a highlighted rectangular box with text, up, over it.

Figure 13-10

The Up button moves an item up in the list

```
//Up Button
OrgLFld = this.getField("priorityList");
if(OrgLFld){
var prevIdx = OrgLFld.currentValueIndices;
if(prevIdx > 0){
var curValue = OrgLFld.getItemAt(prevIdx);
OrgLFld.deleteItemAt(prevIdx);
OrgLFld.insertItemAt({cName:curValue, nIdx:prevIdx-1});
OrgLFld.currentValueIndices = prevIdx - 1;
}
}
```

It acts like an organizer or index. For instance, in an array or associative syntax, the first item or object literal is 0, then 1, then 2, and so on. For an item that is a 2 rank in the list to move upward, it must become rank 1 or the highest rank, which is in this case 0\. If the item is greater than zero, it can move upward. By using `prevIdx - 1`, you are telling the button to move this item one back or one higher on the list from its current value. The other methods of getItemAt, deleteItemAt, and insertItemAt also work together and help move the Item upward in the list.

### The Down Button

The Down button works in reverse, moving items down the list. Refer to Figure [13-11](#449379_2_En_13_Chapter.xhtml#Fig11) and the Actions tab.

![](images/449379_2_En_13_Chapter/449379_2_En_13_Fig11_HTML.jpg)

An icon of a highlighted rectangular box with text, down, over it.

Figure 13-11

The Down button moves a priority item down in the list

```
//Down Button
OrgLFld = this.getField("priorityList");
if(OrgLFld){
var prevIdx = OrgLFld.currentValueIndices;
if( (prevIdx >= 0) && (prevIdx < (OrgLFld.numItems -1)) ){
var curValue = OrgLFld.getItemAt(prevIdx);
OrgLFld.deleteItemAt(prevIdx);
OrgLFld.insertItemAt({cName:curValue, nIdx:prevIdx+1});
OrgLFld.currentValueIndices = prevIdx + 1;
}
}
```

This time, if an item is a rank of 2, it will become a rank 3 if that is possible. In the `if` conditional statement, a check is made: is the item greater (>) or equal (=) to zero, and (&&) is it less than (<) but not already the last item in the list. By using `prevIdx + 1`, you are telling the button to move this item one forward or one lower in the priorityList list box until it can go no further downward. Again, you will find similar methods that were used in the Up button, like get, delete, and then insert to insert the item in a new location based on its current value.

The Add New Priority button allows the user to add a custom priority that might not be on the original list. Unlike drop-down lists where you can allow the user to enter a custom text, you cannot add text directly to a list box. Like any list item, it can be deleted. Notice that, as in earlier lessons like Chapter [9](#449379_2_En_9_Chapter.xhtml), you can add a default text custom format script to the text field “customAdd” so that it will always be filled. The same word will not enter twice.

```
if(!event.value){
event.value="Add To List";
event.target.textColor=color.gray;
}
else{
event.target.textColor=color.black;
}
```

The button has the following code and resets the text field only after an addition. Refer to Figure [13-12](#449379_2_En_13_Chapter.xhtml#Fig12).

![](images/449379_2_En_13_Chapter/449379_2_En_13_Fig12_HTML.jpg)

A snip of 2 text fields, to add to list. 2 buttons to add new priority. A dialog box on text field properties with format tab in view, with custom format script under custom format category. A dialog box on button properties with actions tab in view has mouse up trigger and run a Java Script action.

Figure 13-12

This text field allows you to enter a new priority, and the Add New Priority button allows you to add that priority to the list. You can see the Text Field Properties dialog box’s Format tab set to Custom and the Button Field Properties dialog box’s Actions tab

Here is the mouse up “Run a JavaScript” code for the button field AddPlist:

```
var textValue = this.getField("customAdd").value;
var listField = this.getField("priorityList");
if (textValue) {
listField.insertItemAt(textValue, textValue, -1);
}
```

With the same button, the text field customAdd is reset in the form. Refer to Figure [13-13](#449379_2_En_13_Chapter.xhtml#Fig13).

![](images/449379_2_En_13_Chapter/449379_2_En_13_Fig13_HTML.jpg)

A dialog box of reset a form. The select fields to reset options include quality list, priority list, custom add, sales representative name, and date, where custom add is checked. The text field below has input text, talk to manager, to add to list, with Add new priority buttons adjacent to it.

Figure 13-13

The input text in the text field is reset after it is added to the list box. Use the Actions tab in the Reset a Form dialog box

List boxes are useful and creative ways to organize priorities. You will improve upon this list later in the chapter.

## Check Box, Drop-Down, and List Box: Example 1

A single list box can be used in combination with other form field items. See the file `TMC_FreeNewsletterComboEnd_option1.pdf`. Refer to Figure [13-14](#449379_2_En_13_Chapter.xhtml#Fig14).

![](images/449379_2_En_13_Chapter/449379_2_En_13_Fig14_HTML.jpg)

A snip of a checked option with text, yes, send me my free newsletter. Investment option is selected form the drop down menu, that includes 4 relevant topics listed adjacent to it. A disabled reset button is at the bottom.

Figure 13-14

This project allows you to choose a free newsletter after making a selection from the drop-down list. The drop-down appears only if you check the box, and you can access an item from the list only after you make a selection

This example uses a check box to reveal a drop-down list. When an item in the drop-down is selected, it reveals a secondary choice in the list box. Refer to Figure [13-15](#449379_2_En_13_Chapter.xhtml#Fig15).

![](images/449379_2_En_13_Chapter/449379_2_En_13_Fig15_HTML.jpg)

3 snips of the text fields with c h k yes, c b o newsletters, and list titles, written in the center of the boxes. The first field has a text, yes, send me my free newsletter, adjacent to it.

Figure 13-15

Choices of free newsletter fields in the Prepare Form edit view

Here you begin with a global document JavaScript called `SetTitleEntries`. Refer to Figure [13-16](#449379_2_En_13_Chapter.xhtml#Fig16).

![](images/449379_2_En_13_Chapter/449379_2_En_13_Fig16_HTML.jpg)

A Java Script icon with an add option. A dialog ox on document Java Scripts has script name as, set title entries, with the same in the box below. The java scripts to enter the topics related to investment is mentioned in the editor window. Close button is highlighted.

Figure 13-16

The document JavaScript for the free newletter PDF file in the dialog box

I added the following code:

```
//Set Title Enteries
var oArray = {
Investment: [ ["-"], ["How to Make a Million in Mining"],
["Investing Wisely in Jade"], ["Playing the Market with Lead"],
["Investing in Bonds and Silver"]],
Mining: [ ["-"], ["Gold Today"], ["Silver Futures"], ["Copper Watch"],["Stake the Claim"]],
Gold:  [ ["-"], ["Global Annual Gold Festival"],
["Gold Watch"], ["All that Glitters"],
["Fool's Gold"]],
"Tourmaline Mining":  [ ["-"], ["Tourmaline Queen"],
["Waltermelon Gem"], ["Pick and Shovel"],
["Rocks and Gems"]],
};
function SetTitleEntries(){
if(event.willCommit){
var list = oArray[event.value];
if( (list != null) && (list.length > 0) ){
this.getField("ListTitles").setItems(list);
}
else{
this.getField("ListTitles").clearItems();
}
}
}
```

Notice how, as in the previous drop-down examples, you are using an associative syntax/object called `oArray` to contain all your drop-down items in the cboNewsletters drop-down field and subitems that will go into the list box field called ListTitles. Different subselection items appear in the list box based on the selection made in the drop-down. It’s important that the names in the associative syntax match the names in the drop-down that were entered on the Options tab. They are Investment, Mining, Gold, and Tourmaline Mining. Otherwise, errors can result, and these fields will not work correctly. Always double-check your work for errors.

As in Chapter [12](#449379_2_En_12_Chapter.xhtml), when you have a name in your associative syntax that is two words, always put quotes around it, like `Tourmaline Mining`; if it is one word, it needs no quotes, like `Mining`. However, the titles that will appear in the square brackets, [], and later in the list box should be surrounded by quotes regardless. If no item is selected in the drop-down or in this case 0, which is represented by as upper dash (-) in the Options tab, the list box is cleared.

The check box chkYes has a script applied to it to show and hide fields. You could have used the no script action to show and hide as well. However, in some cases you need to write JavaScript to confirm you are getting the results you want when doing on and off actions, in this case when the `event.target.value` is set to Yes on the Options tab. Refer to Figure [13-17](#449379_2_En_13_Chapter.xhtml#Fig17).

The following code is added on Mouse Up on the Actions tab:

```
//Check Box JavaScript
var topicFld = this.getField("cboNewsletters");
var clistT = this.getField("ListTitles");
if (event.target.value == "Yes"){
topicFld.display = display.visible;
clistT.display = display.visible;
}
else {
topicFld.display = display.hidden;
clistT.display = display.hidden;
}
```

![](images/449379_2_En_13_Chapter/449379_2_En_13_Fig17_HTML.jpg)

2 dialog boxes on check box properties, with options and actions tab selected. Check option is selected for checkbox style and yes entered for export value. Mouse up trigger and run a Java script action are selected in the actions tab in view.

Figure 13-17

The Check Box Properties dialog box showing the Options tab Export Value setting and the Actions tab’s Run a JavaScript setting

For the drop-down list cboNewletters, make sure to check “Commit selected value immediately” on the Options tab so that the items for the list box will appear once the selection is made. As mentioned earlier, these item names including the dash (-) were typed in and added when the Drop-down list was first created and checked to make sure their names matched the JavaScript. The items have no Export Value setting.

The final part of the script is a custom keystroke script in the drop-down list on the Format tab. Refer to Figure [13-18](#449379_2_En_13_Chapter.xhtml#Fig18).

![](images/449379_2_En_13_Chapter/449379_2_En_13_Fig18_HTML.png)

2 dialog boxes on dropdown properties, with options and format tabs selected. Commit selected value immediately is checked. Item list has options of investment, mining, gold, and tourmaline mining. Custom option is selected in format category and custom keystroke script is set title entries.

Figure 13-18

The Dropdown Properties dialog box’s Options and Format tabs

```
//dropdown script custom format script
SetTitleEntries();
```

Here it calls upon the document JavaScript to help it populate the list box correctly when a selection is made.

Until called upon, the list box ListTitles will remain blank because the default selection in the drop-down list is the dash (-). Also, no script is applied to the list box; it is called upon only when required. Refer to Figure [13-19](#449379_2_En_13_Chapter.xhtml#Fig19).

![](images/449379_2_En_13_Chapter/449379_2_En_13_Fig19_HTML.jpg)

A dialog box with options tab on view of list box properties. Fields for item, export value, and items list are blank. Sort item, multiple selection, and commit selection value immediately are not checked. Add, delete, up, and down buttons are disabled.

Figure 13-19

The List Box Properties dialog box on the Options tab are left blank

Important Note

Prior to 2006 if you wanted to show or hide a field, you would write this:

`genreFld.hidden = false;`

`genreFld.hidden = true;`

This method has been deprecated and should be written as shown in the following examples.

`genreFld.display = display.visible;`

`genreFld.display = display.hidden;`

`a.display = display.noPrint //field text visible on screen, but does not print`

`a.display = display.noView // field text hidden on screen, but prints`

In this example, if you uncheck the check box and recheck it, the past selection will remain. For your own project, you may want to reset these fields upon unchecking, as shown in other chapters. You use the Actions tab and add an action of Reset a Form. Then in the dialog box select the fields of the check box, drop-down, list and list box to reset them. Finally, click OK to confirm. You can also add a show and hide for select fields to this button as well.

## Check Box, List Box, and Multi-Drop-Down: Example 2

For a variation on this theme, you could try it in reverse order. Previously, you started off with a drop-down list, so now start with a list box. The list box, depending upon which selection is made, reveals various drop-down lists that contain selections. You may prefer one option over the other, so I am showing you both. See the file `TMC_FreeNewsletterComboEnd_option2.pdf`. Refer to Figure [13-20](#449379_2_En_13_Chapter.xhtml#Fig20).

![](images/449379_2_En_13_Chapter/449379_2_En_13_Fig20_HTML.jpg)

3 screenshots of a list box with a checked text, yes send me my free newsletter on top, and an adjacent dropdown list. Investment and mining are selected respectively in 2 lists, with corresponding dropdown of invest and management, and prospect and panning. Third has text fields for the same.

Figure 13-20

The list box and drop-down list options for the second free newsletter project

This time there is no document JavaScript, based on the way that I set up the file. Refer to Figure [13-21](#449379_2_En_13_Chapter.xhtml#Fig21).

![](images/449379_2_En_13_Chapter/449379_2_En_13_Fig21_HTML.jpg)

A Java Script icon with an add option and document Java Scripts icon. A document Java Script dialog box has blank space for the script name and the close button is highlighted along its borders.

Figure 13-21

This project contains no document JavaScript as seen in the dialog box

The check box chkYes is set up as in the Options panel as in the previous example, but it contains this altered script on the Actions tab to show and hide items. Refer to Figure [13-22](#449379_2_En_13_Chapter.xhtml#Fig22).

![](images/449379_2_En_13_Chapter/449379_2_En_13_Fig22_HTML.jpg)

2 dialog boxes of check box properties with options and actions tab expanded. Checkbox style selected is check and export value is yes. The select trigger is mouse up, with action to run a Java script.

Figure 13-22

The check box properties on the Options and Actions tabs

```
//Check Box JavaScript
var listT = this.getField("ListTitle");
var investments = this.getField("Investments");
var mining = this.getField("Mining");
var gold = this.getField("Gold");
if (event.target.value == "Yes"){
listT.display = display.visible;
}
else {
listT.display = display.hidden;
investments.display = display.hidden;
mining.display = display.hidden;
gold.display = display.hidden;
}
```

Notice how there are more items to show or hide depending upon what is happening. You can choose what you want to see or hide using the `if/else` conditional statements. See Part II of this book if you need to review `if/else` statements. However, notice this code does not use the display.visible to make the drop-down lists appear; it only hides them. We’ll look at that next in another field and tab.

Now let’s move on to the list box field called ListTitle.

Unlike the earlier drop-downs, the items in the list box named ListTitle do have export values, as you’ll see shortly. Refer to Figures [13-23](#449379_2_En_13_Chapter.xhtml#Fig23) and [13-24](#449379_2_En_13_Chapter.xhtml#Fig24).

![](images/449379_2_En_13_Chapter/449379_2_En_13_Fig24_HTML.jpg)

A dialog box of list box properties with selection change tab expanded. A Java code is enclosed in the editor window beneath the selected execute this script option.

Figure 13-24

The Selection Change tab in the List Box Properties dialog box

![](images/449379_2_En_13_Chapter/449379_2_En_13_Fig23_HTML.jpg)

A dialog box of list box properties with options tab in view. Item is a dot, export value is none and item list includes investment, mining, and gold. Sort items, multiple selection, and commit selected value immediately are unchecked. Delete and down buttons are enabled.

Figure 13-23

The Options tab in the List Box Properties dialog box

This time the script is added to a different tab. It is controlled on the Selection Change tab when you change from “Do nothing” to “Execute this script” and click Edit to add your script and then OK in the JavaScript Editor to confirm.

List boxes do not have a Validate, Calculate, or Format tab, so the setting Custom Format is not an option; the script must be controlled on the Selection Change tab. This is similar to a custom keystroke.

Execute this script:

```
// Combo Box Selection Change Text Example
var investments = this.getField("Investments");
var mining = this.getField("Mining");
var gold = this.getField("Gold");
if (event.changeEx == "IV"){
investments.display = display.visible;
mining.display = display.hidden;
gold.display = display.hidden;
this.resetForm(["Mining", "Gold"]);
this.getField("chkYes").checkThisBox(0,true);
}
else if (event.changeEx == "MN"){
investments.display = display.hidden;
mining.display = display.visible;
gold.display = display.hidden;
this.resetForm(["Investments", "Gold"]);
this.getField("chkYes").checkThisBox(0,true);
}
else if (event.changeEx == "GD"){
investments.display = display.hidden;
mining.display = display.hidden;
gold.display = display.visible;
this.resetForm(["Investments", "Mining"]);
this.getField("chkYes").checkThisBox(0,true);
}
else if (event.changeEx == "None"){
investments.display = display.hidden;
mining.display = display.hidden;
gold.display = display.hidden;
this.resetForm(["Investments", "Mining", "Gold"]);
this.getField("chkYes").checkThisBox(0,true);
}
```

Once again, you add more `if/else` conditional statements. Notice how this code is all about the hiding or showing drop-downs based on their Export value in the list box and the drop-down list’s name. I also added a `this.resetForm()` to reset select fields within the brackets rather than just using the default “Reset a Form” action that we used on the check boxes in earlier examples and in Chapter [9](#449379_2_En_9_Chapter.xhtml). If a new selection in the list box is made, the other drop-down lists reset themselves and do not keep the recent selection. You can apply this reset method to check boxes as well using a conditional statement if required.

To prevent the check box from becoming unchecked during the selection change, I made sure to add some code to ensure that it remained on or true. In your own projects, you may find situations where the check box, during a reset, sets itself to false or off when a selection is changed. This works well for a single check box.

Refer to Figure [13-25](#449379_2_En_13_Chapter.xhtml#Fig25) and Figure [13-26](#449379_2_En_13_Chapter.xhtml#Fig26) to see examples of list box items and their related Export values that affect the drop-down list.

![](images/449379_2_En_13_Chapter/449379_2_En_13_Fig26_HTML.jpg)

A dialog box of dropdown properties. Item is a dot, export value is blank, and item list has a highlighted dot, invest, and management. Add and up buttons are disabled, while delete and down options are active.

Figure 13-26

The Dropdown Properties dialog box on the Options tab

![](images/449379_2_En_13_Chapter/449379_2_En_13_Fig25_HTML.jpg)

2 dialog boxes of list box properties with options tab. The item names are investment and mining, respectively. The item list consists of investment, mining, and gold, with the corresponding item highlighted. Export values are I V and M N respectively. Delete, up, and down buttons are enabled.

Figure 13-25

List Box Properties dialog box’s Options tab with items and values

This time the drop-down lists have no formatting applied to them; they are only called upon. However, rather than using a document JavaScript and creating an associative syntax, they do contain their items, which I entered using the Options tab with no values preset. These drop-downs are referenced only by the selection change of the list box.

Note

While not required for options 1 and 2 “End” examples, I also added a reset button so that if the check box was not turned off, you could still reset all the form fields, including hiding the list box and drop-down lists, if they remained selected. Refer to the field ResetBtn on the Actions tab if you need to review this.

## Button Slide Show Variation

This last example on a list box could also be used with buttons set to read-only that contain images if you want to create a type of slide show for a client, where graphics show and hide based on selection. You can find a sample of this in the extra slide show project folder in the file called `TMC_MineralIdentifier_End.pdf`. Refer to Figure [13-27](#449379_2_En_13_Chapter.xhtml#Fig27).

![](images/449379_2_En_13_Chapter/449379_2_En_13_Fig27_HTML.jpg)

A list box with 10 shapes of mineral crystals in which Acicular or radiating is highlighted. The mineral image of Acicular is adjacent to it. The various shapes listed are Acicular, bladed, dendritic, equant, gemstone, globular, massive, prismatic, striated, and tabular.

Figure 13-27

Use a list box and buttons to create a changing image list for reference

Explore this file further on your own and format it to suit your needs. For long lists, the order of the JavaScript variables on the Selection Change tab should always match the same order as the options in the Options tab from top to bottom; otherwise, errors are likely to occur. Make sure to check the Sort option on the Options tab if you want the items to be alphabetical. For the buttons in this example, Layout was set to “Icon top, label bottom,” Behavior was set to Invert, and State was set to Up. However, on the General tab I set them each to read-only so their state could not be affected. Refer to Figure [13-28](#449379_2_En_13_Chapter.xhtml#Fig28) for the Option tab setup.

![](images/449379_2_En_13_Chapter/449379_2_En_13_Fig28_HTML.jpg)

A dialog box on button properties with options tab expanded. Icon top, label bottom is selected in the layout drop down. Invert behavior is selected. The label is Acicular and its icons are represented in the box adjacent to the up state, in the icon and label section. Close button is highlighted.

Figure 13-28

Button Properties dialog box’s Options tab

### Extra Example Priority List Improved

Now that you’ve discovered a few things about list boxes, let’s look at how you can improve the priority list discussed earlier in this project. You can see how this interaction might work in Figure [13-29](#449379_2_En_13_Chapter.xhtml#Fig29), and you can add a text box for a description of a speaker’s topic to assist your organization. To see the full code, go to the file `TMC_PriorityList_End_GuestSpeakers.pdf`.

![](images/449379_2_En_13_Chapter/449379_2_En_13_Fig29_HTML.jpg)

2 rectangular boxes with a list of names. Left. John Smith, James Walker, Paul workman, and Maria Sales. Right. John Smith and James Walker. The topic is mining in Northern B C in a text field, below both the boxes. Buttons, add, delete, clear, up, and down are placed at the center of the 2 lists.

Figure 13-29

A list box project with guest speakers and text fields containing their topics

The buttons’ code will remain the same except for the names of the list boxes; speakerList replaces qualityList, and orderTalkList replaces priorityList. However, the document JavaScript will be altered. Now it is called `talkExport`. Refer to Figure [13-30](#449379_2_En_13_Chapter.xhtml#Fig30).

![](images/449379_2_En_13_Chapter/449379_2_En_13_Fig30_HTML.jpg)

A Java Script icon with an add button and document Java Scripts icon. A dialog box of document Java Scripts has script name as talk export, with the same in the box below. Close option is highlighted. The editor window below has java scripts for talk export with function, add to talk list.

Figure 13-30

document JavaScript added to the PDF file as seen in the dialog box

```
function addToTalkList(cEntry){
// Acquire the Distribution List Field
var OrgLFld = this.getField("orderTalkList");
// Make sure entry does not already exist
// by comparing it to all of the existing entries
var bFound = false;
for(var i=0;i<OrgLFld.numItems;i++){
if(OrgLFld.getItemAt(i,false) == cEntry){
bFound = true;
break;
}
}
if(!bFound){
// Insert entry at end of list
OrgLFld.insertItemAt({cName:cEntry, nIdx:-1});
}
}
function GetMasterExport(cEntry){
// Acquire the Master List Field
var OrgLFld = this.getField("speakerList");
for(var i=0;i<OrgLFld.numItems;i++){
// If item matches, then return the export value
if(OrgLFld.getItemAt(i,false) == cEntry){
return OrgLFld.getItemAt(i,true);
}
}
return null;
}
```

You add an extra function script to deal with values that will act as the speakers’ topics. You want to transfer this information as well to the right, so you add the `GetMasterExport` function, which, if everything is correct or true, will help display the value or speaker topic. Otherwise, it will not display.

Figure [13-31](#449379_2_En_13_Chapter.xhtml#Fig31) shows what this looks like in the left list box, called speakerList. Notice how this time each item has an Export value.

![](images/449379_2_En_13_Chapter/449379_2_En_13_Fig31_HTML.jpg)

A dialog box of list box properties with the options tab in view. Item is Paul workman, export value is, safety on the worksite. Item list has names of John Smith, James Walker, Paul workman, and Maria Sales, in which Paul workman is highlighted. Delete, up, and down buttons are enabled.

Figure 13-31

The list box properties on the Options tab for the left list box speakerList

The list box on the right named orderTalkList should remain blank. However, make sure to check “Commit selected value immediately” on the Options tab, or the values will not commit properly in the text box. Refer to Figure [13-32](#449379_2_En_13_Chapter.xhtml#Fig32).

![](images/449379_2_En_13_Chapter/449379_2_En_13_Fig32_HTML.jpg)

A dialog box of list box properties with the options tab in view. The spaces for item, export value, and item list are blank. Sort items is unchecked. Multiple selection is disabled. Commit selected value immediately option is checked. Add, delete, up, and down buttons are disabled.

Figure 13-32

The list box properties on the Options tab for the list box orderTalkList

The final script is found in the list boxes on the Selection Change tab for both this time. Refer to Figure [13-33](#449379_2_En_13_Chapter.xhtml#Fig33).

![](images/449379_2_En_13_Chapter/449379_2_En_13_Fig33_HTML.jpg)

A dialog box of list box properties with selection change tab in view. The option to execute this script is selected, when the list box selection changes, with java script written beneath the options. The topic is path to profits, in a text field below.

Figure 13-33

List Box Properties dialog box, with JavaScript added to the Selection Change tab and the value appearing in the text field

For the left list box speakerList, this is the code:

```
//Selection Change for First List box
if(event.willCommit){
addToTalkList(event.value, this.cLastValEx);
this.cLastValEx = null;
}
else{
this.getField("Topic1").value = event.changeEx;
}
```

This adds the value as text to the text field Topic1 but also controls the ability to move your last selected item and value over by simply being selected and tabbing out without using the Add button. For your project this may or may not be a benefit. If you do not like this option, then simply comment this line out. Now only the add button will add or move the speaker over and not tabbing out of the list box.

```
// addToTalkList(event.value, this.cLastValEx);
```

For the right list box orderTalkList, here is the code on the Selection Change tab:

```
//Selection Change for Second List Box
if(event.willCommit){
if(event.value){
this.getField("Topic2").value = GetMasterExport(event.value);}
else{
this.getField("Topic2").value = "";}
}
```

This adds the value as text to the text field Topic2 after it has been exported to the right.

If you want more interaction, in the extra example I created, you can also add a text field and buttons to this file in case the user wants to add a last-minute guest speaker with their topic. See the file `TMC_PriorityList_End_GuestSpeakers_extra_3.pdf`. Refer to Figure [13-34](#449379_2_En_13_Chapter.xhtml#Fig34).

![](images/449379_2_En_13_Chapter/449379_2_En_13_Fig34_HTML.jpg)

A snip with 2 text fields to fill the speaker name and the speaker topic. Add guest speaker button is adjacent to it. The actions dialog box indicates mouse up to run a java script, that is highlighted.

Figure 13-34

Adding a guest speaker and their topic to the left list box and text field, which can also automatically later be added to the right list box and text field and then reordered. The action is added to the button’s Action tab

In this example, the button AddSpeaker allows you to add the guest speaker to both list boxes. In addition you can reset the text fields SpeakerName and Speaker Topic after input using the action of “Reset a form” and selecting those fields from the related dialog box. For the input, you must add both the Speaker Name (Item) and Speaker Topic (Export Value) before clicking the button; otherwise, nothing will be added to either list box on the left. Once selected, the new speaker and topic are added to the left and right list boxes and text fields. Here is the code that was used on the button’s Actions tab:

```
//Add Custom Item
var textValue = this.getField("SpeakerName").value;
var textValue2 = this.getField("SpeakerTopic").value;
var listField = this.getField("speakerList");
if (textValue,textValue2) {
listField.insertItemAt(textValue, textValue2, -1);
}
```

Note

In the previous example, you would manually need to clear any new entries from the left list box’s Options tab as they are added permanently to the list if you saved the file. On your own, you could create another hidden delete button to help you select and delete entries you no longer require from the Speaker list box.

## Final Thoughts

As you can see, there is no right or wrong way to organize your list boxes. You can also use them to interact with text boxes or even in combination with check boxes, buttons, or radio buttons. Depending upon the form, one way or another list boxes will help you accomplish your goal.

### Hidden Fields

Note

For those of you who are more familiar with JavaScript, buttons and text fields can also be used as hidden object fields if an extra calculation in a form needs a hidden function. While there are no examples in this chapter, things like double-clicking a list item to transfer it or a complex calculation that you don’t want the client to see (see the earlier invoice form example), those fields can be set to hidden on the General tab or set to no color (Transparent) on the Appearance tab to avoid form clutter for the client. Refer to Figure [13-35](#449379_2_En_13_Chapter.xhtml#Fig35).

![](images/449379_2_En_13_Chapter/449379_2_En_13_Fig35_HTML.jpg)

2 dialog boxes on button properties with appearance and general tab in view. The borders and fill colors options are selected. Line thickness as thin and line style as solid are prefilled. The hidden option is selected in the form field, and the name is inserted as Button 1.

Figure 13-35

A hidden button can have a fill and border color of none on the Appearance tab or be set to Hidden on the General tab

### Using List Boxes for a Number Rating

It is possible to use two or more list boxes to add values together in a final text field. However, I find this method to be buggy, and it does not allow for multiple selection values to be added together. List boxes, as mentioned earlier, do not contain a Format tab, so the final text field can control the calculation output of only the value as a true number. When working with specific multiple number selections, a drop-down list or text field should be used instead of a list box.

### Notes on Changing Custom Formatting for List Boxes

As we have seen, the list box is similar to a drop-down or combo box but does not have as many options. You can, however, use the Appearance tab to make similar property changes. List box locations where you can add formatting include the Document JavaScript dialog box, the Actions tab, and the Selection Change tab as there is no Format, Validate, or Calculate tab.

Chapter [10](#449379_2_En_10_Chapter.xhtml) gives more detail on the following properties that can be used to style a list box; they include `fillColor, strokeColor, lineWidth, borderStyle, textColor, textFont, textSize`, and rotation. You cannot use FontFamily as this is for annotations or text fields that support rich text. Also, you cannot use alignment to, for instance, center the text, and by default text will remain left aligned.

You can, as mentioned in Chapter [12](#449379_2_En_12_Chapter.xhtml), use a `\t` for tab and `\n` for newline to break up the text when writing the code, but it will appear this way only once you tab out of the selection. Also, as mentioned, you cannot use rich-text formatting. For more details, you can refer to this page:

[`https://opensource.adobe.com/dc-acrobat-sdk-docs/library/jsdevguide/JS_Dev_AcrobatForms.html#53749`](https://opensource.adobe.com/dc-acrobat-sdk-docs/library/jsdevguide/JS_Dev_AcrobatForms.html#53749)

## Summary

As you can see, list boxes can be used in combination with other types of form fields. You can use them to transfer information from one list box to another or to a drop-down list or a text field. You can even use them with buttons to create a slide show of images.

In the next chapter, you are going to look at a type of drop-down menu that you might not have heard of before in Acrobat or even knew you could create: the pop-up menu.

# 14. Advanced Navigation: The Pop-Up Menu

**Chapter Goal:** You’ll look at the final example on buttons and navigation: a pop-up menu. Pop-ups are a great nonform way to get from one page to another in a document and can act like a table of contents, as shown in Figure [14-1](#449379_2_En_14_Chapter.xhtml#Fig1). This example can be found in the file `GemShow_inDesign_booklet_2023_End.pdf` in this chapter’s download folder.

Note

If you want to work along in this lesson or review the final result, download the Chapter [14](#449379_2_En_14_Chapter.xhtml) files from the link in the introduction. The file with the label “Start” is the file without the code, and the file with the label “End” is the final result. You will also find a folder with original image files and original Adobe InDesign file (`.indd`), if you would like to edit them, as well as a folder containing the original scripts if you would like to add them to your own PDF forms.

Remember that to view the properties of a field, you must select the Prepare Form tool; only then can you right-click or double-click a field to review its properties.

![](images/449379_2_En_14_Chapter/449379_2_En_14_Fig1_HTML.jpg)

A page titled T M C Gem Show 2023, Guide and Information. It has three diamond structures, and the right-click menu highlights the dealers with the selected T M C Gem Show Dealers option.

Figure 14-1

An example of a pop-up menu in the PDF file

## The Pop-Up Menu Example

In this example, you’ll use a similar conditional statement to the `if/else`. This one is easier to write and is called the `switch case break` or `switch` statement. Like the previous example, it works well for running through the associative syntax and its objects within the square brackets ([]). To begin, add a document JavaScript called `CloseWarning. Refer to` Figure [14-2](#449379_2_En_14_Chapter.xhtml#Fig2).

![](images/449379_2_En_14_Chapter/449379_2_En_14_Fig2_HTML.jpg)

A screenshot of the JavaScript icon with the add tab and the document Java Scripts tool. It includes a document JavaScript dialog box with the script name as close warning, and a close warning box and an editor with script below. The close button is highlighted.

Figure 14-2

The document JavaScript with the close warning in the dialog box

Now add the following JavaScript to the editor, or click Edit to review the script in the “End” example:

```
// Global CloseWarning
function CloseWarning(){
var btn = app.alert("Are you sure you want to quit?",2,2);
if (btn == 4){
this.closeDoc();
}
}
```

This JavaScript creates an option in the drop-down menu to close the document. Depending on where you want it to be within the associative syntax, it is based on the number. In this case, I chose 4 because it is the fifth list items in an index of 0–4, but you may have to adjust this in your list. You can also attach an alert message to the button field “btn” on page 1 on the Actions tab by selecting the Mouse Up trigger and Run a JavaScript. Refer to Figure [14-3](#449379_2_En_14_Chapter.xhtml#Fig3) to see the Option and Actions tabs setup.

![](images/449379_2_En_14_Chapter/449379_2_En_14_Fig3_HTML.jpg)

2 screenshots of the button properties dialog box. On the top, it lists the options tab, with layout as icon only, behavior as invert, and icon and label section. On the bottom, it lists the options under actions tab, with add an action section where run a Java Script is selected under action.

Figure 14-3

Button properties in the Options and Actions tabs

The button itself has the following JavaScript and associative syntax:

```
var itemPicked = app.popUpMenu("Information","About BC Lapidary Society", ["Dealers","TMC Gem Show Dealers","BCompany","GCompany","MCompany",
"SCompany"] ,"-","Quiz", "Close Document");
switch(itemPicked){
case "Information":
this.pageNum = 1;
break;
case "About BC Lapidary Society":
this.pageNum = 3;
break;
case "TMC Gem Show Dealers":
this.pageNum = 5;
break;
case "BCompany":
this.pageNum = 6;
break;
case "GCompany":
this.pageNum = 7;
break;
case "MCompany":
this.pageNum = 8;
break;
case "SCompany":
this.pageNum = 9;
break;
case "Quiz":
this.pageNum = 10;
break;
case "Close Document":
CloseWarning();
break;
//optional
default:
app.alert("Not found here.");
break;
}
```

The script helps you navigate `app.popUpMenu`. The associative syntax helps you navigate to the page you want. Remember that 5 is actually page 6\. Like arrays, they start at 0, so page 1 is actually page 0\. So, make sure that you test your pop-up menu as you go along to make sure you can navigate to the page you want. Subitems for example the “Dealers” are placed in brackets ([]), and you must make a horizontal divider using a dash (-). However, do not include the main heading of the submenu or the dash as part of the conditional `switch case break` statement. Each menu item is separated by a comma.

The `switch` statement evaluates an expression, matching the expression’s value against a series of case clauses (menu times) or in this case itemPicked. It executes statements after the first `case` clause with a matching value, until a `break` statement is encountered or in this case the Menu Item that is selected. Refer to this link:

[`https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/switch`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/switch)

Review these lines in the code:

```
case "Close Document":
CloseWarning();
break;
```

This calls upon the document JavaScript function rather than a number, like the others. You want the document to close when the Yes button is clicked and the user chooses the question alert. Refer to Figure [14-4](#449379_2_En_14_Chapter.xhtml#Fig4).

![](images/449379_2_En_14_Chapter/449379_2_En_14_Fig4_HTML.jpg)

A screenshot of the JavaScript warning window with the text, Are you sure you want to quit, and a highlighted yes button.

Figure 14-4

JavaScript alert asking if you want to quit and close the document

For a `switch` statement, you can add a default statement as an option. If nothing is found or selected in the list, an alert comes up:

```
app.alert("Not found here.");
```

You do not have to add this part to the `switch` expression; it’s optional, and you can comment out this line of code if you do not want the alert to appear.

On the final page (see page 12), create a Back button to get back to the top. You can put an item like this on other pages if you want. Refer to Figure [14-5](#449379_2_En_14_Chapter.xhtml#Fig5).

![](images/449379_2_En_14_Chapter/449379_2_En_14_Fig5_HTML.jpg)

A screenshot presents the button properties dialog box that lists the options under actions tab, with add an action, including the select trigger as mouse up, and select action as execute a menu item, and actions section. Back to first page text is on top of the dialog box.

Figure 14-5

Button properties on the Actions tab for the Back to First Page button. Select the menu item View ➤ Page Navigation ➤ First Page

As you can see, the Back Button requires no JavaScript, but it is nice to have for navigation purposes.

Note

Beyond the customization shown with the `app.popUpMenu`, you can’t alter its color or fonts like with `app.alert`; those are hardwired into the application.

## Add a Pop-Up Menu Using a Bookmark

While a button on page 1 is great to attach navigation to, in other situations you may want to access the menu for navigation while not on page 1.

You could create bookmarks as one form of navigation, as we saw in Chapter [4](#449379_2_En_4_Chapter.xhtml). However, you may want to, in this case, attach the navigation you already created to a single bookmark so that when the user clicks it, they can see the navigation anytime as well as get to page 1 quickly. This would remove the clutter of the buttons as well if the client wanted to print the document.

In this case, refer to the file `GemShow_inDesign_booklet_2023_End_2.pdf`.

In this example, I moved all my code on the button to the document JavaScript tool area and placed it into a function I called PopUpMenu in the editor. Refer to Figure [14-6](#449379_2_En_14_Chapter.xhtml#Fig6).

![](images/449379_2_En_14_Chapter/449379_2_En_14_Fig6_HTML.jpg)

A screenshot of the document Java Scripts dialog box with the script name as pop-up menu, and function, pop-up menu highlighted in the box below, and a script field. It has a highlighted close button.

Figure 14-6

Document JavaScript dialog box with new script added

```
function PopUpMenu(){
//Insert Button JavaScript Here
}
```

Click OK to exit the editor, click the Close button to exit the dialog box, and then close the JavaScript tool.

Then in my Bookmarks navigation pane, I created a new bookmark called Popup Menu. Refer to Figure [14-7](#449379_2_En_14_Chapter.xhtml#Fig7).

![](images/449379_2_En_14_Chapter/449379_2_En_14_Fig7_HTML.jpg)

A screenshot of the bookmarks panel, and the navigation pane includes several icons with a highlighted popup menu. Add a new bookmark icon is highlighted on top.

Figure 14-7

New bookmark added to the Bookmarks pane

I then right-clicked the bookmark and adjusted its properties in the Actions tab. Refer to Figure [14-8](#449379_2_En_14_Chapter.xhtml#Fig8).

![](images/449379_2_En_14_Chapter/449379_2_En_14_Fig8_HTML.png)

A screenshot of the bookmark properties dialog box that lists the options under actions with run a Java Script added as an action and actions. It includes a Go to a page in this document pop-up dialog box with an enabled use page number option.

Figure 14-8

Access the Bookmark Properties dialog box’s Actions tab and the “Go to a page in this Document” dialog box

By default, it must be set to “Go to a Page in this document.” You can edit this action and set it to page 1\. Then adjust the zoom level in the dialog box and click OK to confirm (Figure [14-8](#449379_2_En_14_Chapter.xhtml#Fig8)). Then you can click Run a JavaScript and add the reference to the document JavaScript. In the JavaScript Editor, add `PopUpMenu();` and then click OK to exit the editor and then the Bookmark Properties dialog box. Then click OK again. Refer to Figure [14-9](#449379_2_En_14_Chapter.xhtml#Fig9).

![](images/449379_2_En_14_Chapter/449379_2_En_14_Fig9_HTML.jpg)

A screenshot of the JavaScript editor, which has the create and edit JavaScript field with text pop-up menu parentheses semicolon.

Figure 14-9

Function references in the JavaScript Editor for the bookmark on the Actions tab

I then saved my file, and now when the user clicks the bookmark icon, they can be on page 1 and access the drop-down menu to go to a new page. The Bookmarks pane is always on the left no matter what page you are currently on. Refer to Figure [14-10](#449379_2_En_14_Chapter.xhtml#Fig10).

![](images/449379_2_En_14_Chapter/449379_2_En_14_Fig10_HTML.jpg)

A screenshot of the bookmark dialog box, which has the highlighted bookmark of, pop-up menu, and the right-click navigation pane includes information, about B C lapidary society, dealers, quiz, and close document.

Figure 14-10

The bookmark can access the PopupMenu

In other situations, some users may prefer to use app.popUpMenuEx to launch a URL. Again, this type of code would have to rely on placing it in the document JavaScript as a function to globally reference. While the pop-up menu with this type of Array object does hover and initially appear on the page, to retrieve the menu again I find it is difficult; also, the option of right-clicking does not appear to work to bring up that menu again. However, if you would like to review examples of the code and modify it yourself, you can refer to this page:

[`https://opensource.adobe.com/dc-acrobat-sdk-docs/library/jsapiref/JS_API_AcroJS.html?highlight=popupmenu#popupmenuu`](https://opensource.adobe.com/dc-acrobat-sdk-docs/library/jsapiref/JS_API_AcroJS.html?highlight=popupmenu#popupmenuu)

## Final Thoughts

I hope that you have enjoyed these lessons. I have also included the following extra lessons in Part IV:

*   Creating a task using JavaScript using the Action Wizard (Chapter [15](#449379_2_En_15_Chapter.xhtml))

*   Faux multistate check boxes (Chapter [16](#449379_2_En_16_Chapter.xhtml))

*   Five-star rating button (Chapter [16](#449379_2_En_16_Chapter.xhtml))

*   Importing an image into a button (Chapter [17](#449379_2_En_17_Chapter.xhtml))

*   Mass formatting of numbers (Chapter [18](#449379_2_En_18_Chapter.xhtml))

*   Digital signatures and barcodes (Chapter [19](#449379_2_En_19_Chapter.xhtml))

*   E-signature forms and web forms (Chapter [20](#449379_2_En_20_Chapter.xhtml))

In the meantime, practice the previous lessons first to build up your JavaScript knowledge. If you want some extra practice, you can also jump to Part V (Chapter [21](#449379_2_En_21_Chapter.xhtml)) where there are four assignments that reference Chapters [1](#449379_2_En_1_Chapter.xhtml) to [12](#449379_2_En_12_Chapter.xhtml) and [18](#449379_2_En_18_Chapter.xhtml) of this book.

## Summary

In this chapter, you looked at pop-up menus and how they can improve navigation in a document with either a button or a bookmark. You also learned about JavaScript’s `switch case break` statement.

# Part IVBeyond the Basics

# 15. Action Wizard and JavaScript

Part IV contains a collection of extra lessons that I added to the end of the book for those who are more advanced users of JavaScript.

**Chapter Goal:** The Action Wizard is another tool that is available in Adobe Acrobat Pro. With it you can create tasks or repetitive actions that you can save and then later use in multiple PDF files (Figure [15-1](#449379_2_En_15_Chapter.xhtml#Fig1)).

![](images/449379_2_En_15_Chapter/449379_2_En_15_Fig1_HTML.jpg)

A screenshot of the action wizard icon with the add option. It includes sub-tools below such as new action, manage actions, more actions web, new custom command, and manage custom commands.

Figure 15-1

These are subtools you can find in the Action Wizard tool menu

## Working with the Action Wizard

The Action Wizard comes with some actions already available. However, when you click the New Action button in the upper left, the Create New Action dialog box opens, and you can create new actions by adding an action from a choice of available tools like the ones you saw in the Form Properties dialog box’s Actions tab. And just like in that tab, you can create a JavaScript action when you choose Execute JavaScript or Document JavaScripts from either the Go To or More Tools sections.

Note

If you want to work along in this lesson or review the final result, download the Chapter [15](#449379_2_En_15_Chapter.xhtml) files from the link in the introduction. You will find a folder containing the original script and `.sequ` files if you would like to add them to your own Action Wizard files, as I will explain later how to Import when you manage actions. For these examples, you can use my two files with slight code variations named `Watermark - Without PDF Extension.sequ` and `New Watermark-Without PDF Exentsion.sequ` or just build your own from the instructions in this chapter.

Let’s look at an example. Here’s how to automatically add the name of the PDF file in Acrobat Wizard for Acrobat Pro:

![](images/449379_2_En_15_Chapter/449379_2_En_15_Fig11_HTML.jpg)

A screenshot of 2 file name watermark fields, which have the file titled T M C customer survey with add files, execute JavaScript options, and start or completed buttons respectively. It includes the add files drop-down list on the left with the highlighted add files option.

Figure 15-11

The Action Wizard adds a watermark to the file

1.  Open the file you want to apply this action to (File ➤ Open).

2.  Select Tools ➤ Action Wizard ➤ New Action, as shown in Figure [15-2](#449379_2_En_15_Chapter.xhtml#Fig2).

    ![](images/449379_2_En_15_Chapter/449379_2_En_15_Fig2_HTML.jpg)

    A screenshot of the action wizard icon with the add option and the new action tool next to it.

    Figure 15-2

    Select Action Wizard ➤ New Action to begin the project

3.  Select More Tools ➤ Execute JavaScript. Refer to Figure [15-3](#449379_2_En_15_Chapter.xhtml#Fig3) and Figure [15-4](#449379_2_En_15_Chapter.xhtml#Fig4).

    ![](images/449379_2_En_15_Chapter/449379_2_En_15_Fig4_HTML.jpg)

    A screenshot of the drop-down list of more tools with, document Java Scripts and execute JavaScript.

    Figure 15-4

    Selecting More Tools ➤ Execute JavaScript

    ![](images/449379_2_En_15_Chapter/449379_2_En_15_Fig3_HTML.jpg)

    A screenshot of the create a new action window, which has tools to add and action steps to show. The tools drop-down list has selected content, while the action steps have the files to be processed with the currently open p d f file and default options.

    Figure 15-3

    The many Create New Action dialog box selections

4.  Click the Add to Right-Hand Pane button to move the tool to the “Actions steps to show” side. Refer to Figure [15-5](#449379_2_En_15_Chapter.xhtml#Fig5).

    ![](images/449379_2_En_15_Chapter/449379_2_En_15_Fig5_HTML.jpg)

    A screenshot of the drop-down list of more tools with document Java Scripts and highlighted execute JavaScript.

    Figure 15-5

    Selected action and the Add to Right-Hand Pane button

    **Note** For more complex actions with multiple tools, as you work you can use the other button icons on the far right of the dialog box (Figure [15-3](#449379_2_En_15_Chapter.xhtml#Fig3)) to add a panel, add an instruction, add a divider, move an action up or down on the list, and/or remove selected items from the action.

5.  Once the Execute JavaScript option is on the right side, double-click Specified Settings to enter the JavaScript Editor. Note that the Prompt User check box should remain selected. Refer to Figure [15-6](#449379_2_En_15_Chapter.xhtml#Fig6).

    ![](images/449379_2_En_15_Chapter/449379_2_En_15_Fig6_HTML.jpg)

    A screenshot of the action steps to show with currently open file, default option, and highlighted execute JavaScript. It includes the JavaScript editor dialog box with the create and edit JavaScript field, with a highlighted O K button.

    Figure 15-6

    Enter the JavaScript Editor upon opening the specified settings

    The JavaScript Editor appears and allows you to begin to type.

6.  Enter the following code:

    ```
    /* Part of my script was missing see my highlight note */
    this.addWatermarkFromText({
    cText: this.documentFileName.replace(/\.pdf$/i,""),
    nFontSize: 12,
    cFont: "Arial,Bold",
    aColor: color.red,
    nOpacity: 0.4,
    nVertAlign: app.constants.align.bottom,
    nHorizAlign:app.constants.align.left,
    nVertValue: 18
    });
    ```

    This script is using a regular expression, which you saw in Chapter [11](#449379_2_En_11_Chapter.xhtml). You are removing or replacing the `.pdf` extension of the filename with a blank area. The `/i` acts as modifier when doing a case-insensitive match, and then the `.pdf` is to be made blank using the `.replace` method. You are also requesting that the script add the name of this file at the bottom left of the page in a font size of 12 and a vertical value or position of 18\. You’re also making the font red with an opacity of 40 percent. In this case, 1 is equal to 100 percent, and 0.1 would be 10 percent. If you just want the text to be a default of black, you can remove the `aColor` and `nOpacity` lines.

    The color version should look something like Figure [15-11](#449379_2_En_15_Chapter.xhtml#Fig11) but will depend on how you named your file.

    For more information on regular expressions, go to the following pages:

    [`www.w3schools.com/jsref/jsref_obj_regexp.asp`](http://www.w3schools.com/jsref/jsref_obj_regexp.asp)

    [`https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions)

    **Note** If you want to keep the `.pdf` at the end of the watermark, remove the command of `.replace(/\.pdf$/i, "")` and only leave `cText: this.documentFileName`.

    Another variation on the regular expression is to write `this.documentFileName.replace(/\.[^/.]+$/, ""),`.

    This uses metacharacters and quantifiers, which search for the beginning of the string and match the ending. This would match other file extensions that do not end in `.pdf`. For example, this could be part of the action if the intent is to first locate a Word `.doc` file, convert the file to a PDF, and later add the watermark. More file extensions could then be part of the action including those in Excel and some graphic files. However, in the previous example, I set it to a specific file extension to work only with PDFs, but the extension can be in lowercase or capital letters.

    For watermarks, you can control the alignment further.

    `nVertAlign: app.constants.align`. can be top, center, or bottom.

    `nHorizAlign:app.constants.align`. can be left, center, or right.

    You can also use `nHorizValue` and `nVertValue` to shift the position of the watermark on the page.

    `nHorizValue: -50, nVertValue: -50`

    Refer to this link for more examples:

    [`https://opensource.adobe.com/dc-acrobat-sdk-docs/library/jsapiref/doc.html?highlight=addwatermarkfromtext#addwatermarkfromtext`](https://opensource.adobe.com/dc-acrobat-sdk-docs/library/jsapiref/doc.html?highlight=addwatermarkfromtext#addwatermarkfromtext)

7.  Click the OK button to close the JavaScript Editor.

8.  Now click the Save button in the dialog box. Refer to Figure [15-7](#449379_2_En_15_Chapter.xhtml#Fig7).

    ![](images/449379_2_En_15_Chapter/449379_2_En_15_Fig7_HTML.jpg)

    A screenshot of the save button.

    Figure 15-7

    The Save button that allows you to save the action

9.  When the Save Action dialog box opens, give the action a title in the Action Name box and a description in the Save Action dialog box. Refer to Figure [15-8](#449379_2_En_15_Chapter.xhtml#Fig8).

    ![](images/449379_2_En_15_Chapter/449379_2_En_15_Fig8_HTML.jpg)

    A screenshot of the save action dialog box with the action name as file name watermark, without P D F extension, action description as adds a watermark to the page, and a selected save button.

    Figure 15-8

    Add a name and description to the action

10.  Click Save, and the action is added to the Actions List on the right side. Refer to Figure [15-9](#449379_2_En_15_Chapter.xhtml#Fig9).

    ![](images/449379_2_En_15_Chapter/449379_2_En_15_Fig9_HTML.jpg)

    A screenshot of the actions list with the enabled file name watermark.

    Figure 15-9

    The action is added to the Actions List area

    If you want to make adjustments/edits or remove this action, you can do so in the Manage Actions dialog box. Refer to Figure [15-10](#449379_2_En_15_Chapter.xhtml#Fig10).

    ![](images/449379_2_En_15_Chapter/449379_2_En_15_Fig10_HTML.jpg)

    A screenshot of the manage action tool at the top. It has a manage actions dialog box, which has 9 enabled action fields with the highlighted file name watermark without P D F extension.

    Figure 15-10

    The Manage Actions tool and dialog box

    By selecting the action and clicking Edit, you can alter the action if you discover an error while you test it. Other options in this area allow you to rename an action, copy, and remove. However, be careful that when you remove an action that you do not remove a custom action that you might need later by mistake as there is no undo key. Thankfully, default actions cannot be deleted or edited as those buttons will disable when those actions are selected. You can also move actions up and down in the list, as well as export or import an action as a `.sequ` file so that you can share your action files with others or back up your actions in a folder on your computer. You can close this dialog box for now.

11.  To execute, click the action on your right pane. You can add more than one file at this point or select another option from the drop-down list. Then click Start, and then, when the editor appears, this will give you the option to edit the action. Then click OK, and it will add the text to the bottom of the file. If you like what you see, save the file or files to a new location. Refer to Figure [15-11](#449379_2_En_15_Chapter.xhtml#Fig11).

Make sure that your filenames do not contain an underscore if you do not want this to be part of the filename. Also, on your own, if you want to add saving as part of your action, look in the Save & Export tool section. Refer to Figure [15-3](#449379_2_En_15_Chapter.xhtml#Fig3). I left this off so that you could just focus on the script and testing it.

Remember, JavaScript actions can be imported and exported if you want to share your actions with your friends and co-workers. Refer to Figure [15-10](#449379_2_En_15_Chapter.xhtml#Fig10). They are saved as a `.sequ` file, as is the one for this example in the folder. You can also find more actions on the Web by clicking the More Action (Web) button. Refer to Figure 18-1, which will take you to the following link:

[`https://acrobatusers.com/actions-exchange/`](https://acrobatusers.com/actions-exchange/)

### Reuse JavaScript from Chapter [8](#449379_2_En_8_Chapter.xhtml)

By following these same steps, you can use some of the JavaScript code to create a time stamp and add a signature field to multiple documents. You can refer to my file `Add a signature field.sequ` for an example.

```
//Adding a signature field automatically
var c = this.addField({
cName: "clientSignature",
cFieldType: "signature",
nPageNum: this.pageNum,
oCoords: [35,74,176,112]
})
//script that creates the text field
{
var r = [200, 200, 400, 300];
var i = this.pageNum;
var f = this.addField(String("completeDate."+i),"text",i,r);
f.textSize = 10;
f.alignment = "right";
f.textColor = color.blue;
f.fillColor = color.transparent;
f.textfont = font.HelvB;
f.strokeColor = color.transparent;
f.value = String("This page was reviewed on: " + util.printd("mmm dd, yyyy", new Date()));
f.readonly = true;
}
```

Note

You can also access document JavaScripts as well in the More Tools area. Refer to Figure [15-12](#449379_2_En_15_Chapter.xhtml#Fig12). However, remember to also look in the Go To section as there are a few related actions there that include Prepare Form, JavaScript, and other related new tools for e-signing, which we will focus on more in Chapter [20](#449379_2_En_20_Chapter.xhtml).

![](images/449379_2_En_15_Chapter/449379_2_En_15_Fig12_HTML.jpg)

A screenshot of the more tools drop-down list with document Java Scripts and execute JavaScript.

Figure 15-12

The JavaScript options

## Is It a Custom Action or a Custom Command?

In the Action Wizard, custom commands in Figure [15-13](#449379_2_En_15_Chapter.xhtml#Fig13) are single-step and applicable to the current document, while actions as we discussed earlier are multistep sequences and applicable to multiple files. Refer to Figure [15-14](#449379_2_En_15_Chapter.xhtml#Fig14).

![](images/449379_2_En_15_Chapter/449379_2_En_15_Fig14_HTML.jpg)

A screenshot of the new custom command dialog box with customizable commands where execute Java Script is selected, label and tooltip with execute Java Script, command options, and enabled show options every time the command is run.

Figure 15-14

The New Custom Command dialog box

![](images/449379_2_En_15_Chapter/449379_2_En_15_Fig13_HTML.jpg)

A screenshot of the icons of the new custom command and the manage custom command sub-tools.

Figure 15-13

Action and command options

### Create and Manage Custom Commands

Custom commands allow you to preconfigure commands such as for watermarks, headers, or footers to reduce the amount of time each command takes to set up. This saves time for repetitive tasks. In this example, if you execute the JavaScript, you insert the code when you click the Command Options button and select “Show options every time the command is run” or “Don’t show options, options are always the same”. Then click OK after you add a label and tool tip.

The commands are imported or exported as XML under the Manage Custom Commands button; they have similar management options as actions.

Actions and commands can use JavaScript.

For more on the Action Wizard and commands, visit the following link:

[`https://helpx.adobe.com/acrobat/using/action-wizard-acrobat-pro.html`](https://helpx.adobe.com/acrobat/using/action-wizard-acrobat-pro.html)

## Summary

In this chapter, you looked at a tool known as the Action Wizard. This versatile tool allows you to apply your JavaScript actions in multiple documents and saves you time. You can also share these actions or commands with other Acrobat Pro users, and they can use the code as well. Take some time to add a few new actions with your Action Wizard tools to PDF documents.

# 16. Multistate Check Boxes

**Chapter Goal:** When space is limited in a document, it is often helpful to be able to combine multiple states into one field. For example, suppose that you have a form and in it you would like to be able to change the setting from blank to a check symbol (representing yes) or to an X or cross symbol (representing no or wrong). Or maybe you have multiple symbols that represent distinct levels. As the user who fills out the form advances in skill, they require a frequent update to their status in a specific order. In this chapter, you’ll see how to create a type of multistate check box using a button field.

Later you will also look at how to select multiple check boxes at once.

## The Problem of Multistate Check Boxes

If you had lots of room, you could do the following with radio buttons:

Do you like our newsletter? • `Yes` ❍ `No` ❍ `Undecided`

```
• Level 1  ❍ Level 2  ❍ Level 3
```

As you have discovered with Acrobat, radio buttons come in groups, and you can choose only one option. They also appear in one state (`On` • or `Off` ❍); there is no third or fourth option.

You can also use check boxes to save space. Yes could mean checked, and No could be unchecked. However, once again there is no third option, and the person filling in the form may not be sure if not checking means No or Undecided.

Do you like our newsletter? ❏ `Yes` ❏ `No` or ✓ `Yes` ✓ `No (I like it, but...)`.

✓ `Level 1` ✓ `Level 2` ❏ `Level 3`. Here I might want people to know I am Level 2 now, but this takes up a lot of space.

To save space, a more visual clue with a check box would be the following:

Do you like our newsletter?

*   ❏ - This means I have not answered yet

*   ✔ - This means Yes, I do

*   ✘ - This means No, I don’t

*   Or current level

*   ❶

*   ❷

*   ❸

With Acrobat check boxes, you can choose more than one. Unfortunately, they are not able to have three or more states, only on or off.

The best way to achieve a multistate goal is to mimic a check box with a button field and some JavaScript actions. Refer to Figure [16-1](#449379_2_En_16_Chapter.xhtml#Fig1).

![](images/449379_2_En_16_Chapter/449379_2_En_16_Fig1_HTML.jpg)

A screenshot presents the prepare form icon, with the add tab, a checker box, and an O K button.

Figure 16-1

The Prepare Form tool for making a button look like a check box

Note

If you want to work along in this lesson or review the final result, download the Chapter [16](#449379_2_En_16_Chapter.xhtml) files from the link found in the introduction. The file with the label “Start” is the file without the code, and the file with the label “End” is the final result. You will also find folders with original MS Word and PDF files if you would like to edit them and a folder containing the original scripts if you would like to add them to your own PDF forms.

If you are creating your form from an original PDF, refer to the “Forms Review” section in Chapter [1](#449379_2_En_1_Chapter.xhtml).

Remember that to view the properties of a field, you must select the Prepare Form tool; only then can you right-click or double-click a field to review its properties.

Let’s begin by altering the Appearance tab of the button so that it will look like a check box. You can also adjust the size further if you need to fill a certain area. Refer to the file `TMC_mineralchecklist_End.pdf` in this chapter’s file folder. Look at the properties button field called ButtonDiamond. Refer to Figure [16-2](#449379_2_En_16_Chapter.xhtml#Fig2).

![](images/449379_2_En_16_Chapter/449379_2_En_16_Fig2_HTML.jpg)

2 screenshots of the button properties dialog box and the text field at the top with the text button diamond. On the top, appearance tab is open with borders and colors, and text sections. On the bottom, options tab is open with layout as label only selected, behavior, and icon and label section.

Figure 16-2

The button properties in the Appearance and Options tabs

The button has a border to make it look like a check box and currently has no text. The Options tab remains at default Layout setting of Label Only, the Behavior setting of Invert, and the State setting of Up, and you do not give the button a label name; you will let the JavaScript actions on the Actions tab handle this.

The final step in the Actions tab is to choose Mouse Up and the action of Run a JavaScript. Then click Add and enter the following script or in this case select the Action and click Edit to view. Refer to Figure [16-3](#449379_2_En_16_Chapter.xhtml#Fig3).

![](images/449379_2_En_16_Chapter/449379_2_En_16_Fig3_HTML.jpg)

A screenshot of button properties dialog box that lists the options under actions tab, with add an action section including select trigger as mouse up, and select action as run a Java Script, and an actions section.

Figure 16-3

The Mouse Up trigger and the Run a JavaScript action

Review the JavaScript in the editor.

```
if (event.target.buttonGetCaption()=="\u2714"){
event.target.buttonSetCaption("X");
event.target.textColor = color.red;
}
else if (event.target.buttonGetCaption()=="X"){
event.target.buttonSetCaption("");
}
else if (event.target.buttonGetCaption()==""){
event.target.buttonSetCaption("\u2714");
event.target.textColor = color.green;
}
```

When you test it by clicking the button, you see now that you have the states shown in Figure [16-4](#449379_2_En_16_Chapter.xhtml#Fig4), and it looks very much like a check box.

![](images/449379_2_En_16_Chapter/449379_2_En_16_Fig4_HTML.jpg)

A screenshot of 2 rectangular-shaped buttons with a tick and a cross, and a blank rectangular button.

Figure 16-4

Three button states: a check, an X, and a blank that mimics a check box

Three conditions were used in the following `if/else` conditional statements:

*   The symbol or Unicode for the green check shape, `\u2714`

*   The symbol for the red cross or X

*   A blank state this can be written with the straight quotes together (`"")` or a blank space between the straight quotes (`" ")`

The reason for the blank state is that unlike other form fields, Acrobat does not allow you to use a Reset button to reset another button’s field state, so within each button, a cleared state must be created as part of the toggle.

The code moves through a cycle of `buttonGetCaption` to `buttonSetCaption.` After one caption appears (`Get`), you click to set the next caption and so on.

Note

You cannot select or deselect a button field. It doesn’t show up as a field you can reset when you search the Reset a Form dialog box. Refer to Figure [16-5](#449379_2_En_16_Chapter.xhtml#Fig5).

![](images/449379_2_En_16_Chapter/449379_2_En_16_Fig5_HTML.jpg)

A screenshot of the reset form dialog box that lists the options under select fields to reset with several enabled fields and a highlighted O K button.

Figure 16-5

No reset selection for a Button field can be found when you try to add the action on your Actions tab of the Reset a Form dialog box

Note

Throughout this book you have seen examples of `event.value` or `event.target.value` or even `event.target.textColor`. When is it appropriate to use `event.target` or just `event`? In the case of `event.value`, this represents the text field’s new input value, which is always changing, but when we say `event.target.value` or `event.target.textColor`, we are now targeting a property that will now represent the field’s current or set value. This could apply in the case of a radio button or check box where an export value is already set as either on or off and then called upon. For example, we would use the JavaScript code in a specific check box on the Mouse Up trigger if we wanted to change its color on the on or off state.

```
if (event.target.value !="Off"){
event.target.fillColor=color.red;
}
else{
event.target.fillColor=color.green;
}
```

With `event.target.textColor`, you can change the properties using conditional statements. When On (not Off), it is red, and when Off, it is green. However, in another example with a text field where the input text is always changing, you can set the field to Number Format, and on the Validation tab, add the following custom validation code to switch the color of the numbers from black to red if they are negative or positive. Here we use the `event.value`:

```
event.target.textColor = event.value < 0 ? color.red : color.black;
```

The best way to learn when to use the `event.target.value` is by referring to your Acrobat Online Reference and experimenting with different examples in this book.

Continuing on with our project, as each `if/else` condition is met with a click, you toggle to the next state and cycle back to the beginning.

```
event.target.buttonSetCaption("X");
event.target.buttonSetCaption("");
event.target.buttonSetCaption("\u2714");
```

You also differentiate the check from the cross by color.

```
event.target.textColor = color.red;
event.target.textColor = color.green;
```

The blank state required no color because nothing is in the field.

As a final note, let’s see what the code looks like with the level symbols. Refer to the properties of the button field called Button Quartz.

In forms, if you’re just using regular numbers, text fields or drop-downs are fine. However, when using special symbols that have a precise order, this might be a better solution for toggling to the correct choice:

*   ❶ uses `\u2776`

*   ❷ uses `\u2777`

*   ❸ uses `\u2778`

```
if (event.target.buttonGetCaption()=="\u2776"){
event.target.buttonSetCaption("\u2777");
event.target.textColor = color.red;
}
else if (event.target.buttonGetCaption()=="\u2777"){
event.target.buttonSetCaption("\u2778");
event.target.textColor = color.blue;
}
else if (event.target.buttonGetCaption()=="\u2778"){
event.target.buttonSetCaption("");
}
else if (event.target.buttonGetCaption()==""){
event.target.buttonSetCaption("\u2776");
event.target.textColor = color.green;
}
```

On the Appearance tab, adjust the font size to Auto, which will center the symbol better if it appears to be chopped off or not centered on the button. You can also resize the field’s width slightly as required. Refer to Figure [16-6](#449379_2_En_16_Chapter.xhtml#Fig6).

![](images/449379_2_En_16_Chapter/449379_2_En_16_Fig6_HTML.jpg)

A screenshot of the button properties dialog box, which lists the options under appearance tab with borders and colors, and text, with font size set to auto. It includes 3 buttons labeled 1, 2, and 3 in increasing order of size.

Figure 16-6

When the font size is set to Auto, this is how it appears in the button

## Bonus Star Rating Idea

Maybe you want to rate an idea or a product. Figure [16-7](#449379_2_En_16_Chapter.xhtml#Fig7) shows the button field Button1.

![](images/449379_2_En_16_Chapter/449379_2_En_16_Fig7_HTML.jpg)

A screenshot of the rate me button on the left, and the five-star rating icon on the right with 1 shaded star.

Figure 16-7

A button with a five-star rating option

Here is what a star rating button could look like using Unicode symbols of white and black stars:

```
if (event.target.buttonGetCaption()=="\u2605 \u2606 \u2606 \u2606 \u2606") {
event.target.buttonSetCaption("\u2605 \u2605 \u2606 \u2606 \u2606");
event.target.textColor = color.yellow;
//1 star
}
else if (event.target.buttonGetCaption()=="\u2605 \u2605 \u2606 \u2606
\u2606") {
event.target.buttonSetCaption("\u2605 \u2605 \u2605 \u2606 \u2606");
event.target.textColor = color.yellow;
//2 star
}
else if (event.target.buttonGetCaption()=="\u2605 \u2605 \u2605 \u2606
\u2606") {
event.target.buttonSetCaption("\u2605 \u2605 \u2605 \u2605 \u2606");
event.target.textColor = color.yellow;
//3 star
}
else if (event.target.buttonGetCaption()=="\u2605 \u2605 \u2605 \u2605
\u2606") {
event.target.buttonSetCaption("\u2605 \u2605 \u2605 \u2605 \u2605");
event.target.textColor = color.yellow;
// 4 star
}
else if (event.target.buttonGetCaption()=="\u2605 \u2605 \u2605 \u2605
\u2605") {
event.target.buttonSetCaption("Rate Me");
// 5 back to blank
}
else if (event.target.buttonGetCaption()=="Rate Me") {
event.target.buttonSetCaption("\u2605 \u2606 \u2606 \u2606 \u2606");
event.target.textColor = color.yellow;
}
```

Always test your form before submitting it to others.

For more information on the field methods of buttonGetCaption and buttonSetCaption, refer to the following links:

[`https://opensource.adobe.com/dc-acrobat-sdk-docs/library/jsapiref/JS_API_AcroJS.html?highlight=checkthisbox#buttongetcaption`](https://opensource.adobe.com/dc-acrobat-sdk-docs/library/jsapiref/JS_API_AcroJS.html?highlight=checkthisbox#buttongetcaption)

[`https://opensource.adobe.com/dc-acrobat-sdk-docs/library/jsapiref/JS_API_AcroJS.html?highlight=checkthisbox#buttonsetcaption`](https://opensource.adobe.com/dc-acrobat-sdk-docs/library/jsapiref/JS_API_AcroJS.html?highlight=checkthisbox#buttonsetcaption)

For more on Unicode symbols, see the following links for other references:

[`https://en.wikipedia.org/wiki/Dingbat`](https://en.wikipedia.org/wiki/Dingbat)

[`https://en.wikipedia.org/wiki/Star_(glyph)`](https://en.wikipedia.org/wiki/Star_(glyph))

Note that to write Unicode, you may see it written like U+2606 when you look at these links. In your code you would then change it to \u2606\. When changing to a new Unicode symbol, make sure to clear your Options tab’s Label area so that it resets to the blank state if you find the button is not cycling correctly. Not all modern Unicode symbols will display as intended, such as those with letters and numbers in the Unicode, so it’s important to test the button.

Be aware in Acrobat you cannot use Unicode color emojis as they will appear only as bullets. Just use the black and white dingbats with a number Unicode and color them using JavaScript.

## Select All or Deselect All Check Boxes at Once

If you have a lot of check boxes that need to be checked in your form, wouldn’t it be great to check them all at once? Refer to Figure [16-8](#449379_2_En_16_Chapter.xhtml#Fig8) and the following text.

![](images/449379_2_En_16_Chapter/449379_2_En_16_Fig8_HTML.jpg)

A screenshot of the good quality field with three checkboxes below, and the checked select all option to its bottom right.

Figure 16-8

Select all check boxes at once with one main check box

You may have noticed that Acrobat does not have an option to create a check box that will select all the other check boxes in a set. You can use the code included in the file `TMC_mineralchecklist_End_selectAll.pdf` from the downloaded folder for this chapter. The following is the code for the Select All check box on the Actions tab with a Mouse Up trigger:

```
/*Check Box Select All*/
var otherCheckBoxes = ["CheckBox1", "CheckBox2", "CheckBox3"];  // etc.
if (this.getField("Select All").value!="Off") {
for (var i in otherCheckBoxes) {
this.getField(otherCheckBoxes[i]).checkThisBox(0,true);
}
}
else{
for (var i in otherCheckBoxes){ this.getField(otherCheckBoxes[i]).checkThisBox(0,false);
}
}
```

The code states that when the main check box, Select All, is not off (which would be on), then all the other check boxes within its group should be selected.

Each of the check boxes is numbered in order in a type of object array (0, 1, 2, 3) though the field names do not have to start with 0; this is so that the `for…in` loop statement will find all the check boxes so they can be selected or deselected. Refer to Chapters [10](#449379_2_En_10_Chapter.xhtml) and [13](#449379_2_En_13_Chapter.xhtml) for examples of `for()` loops. However, in this example we are using a `for..in`. First we state the variable index (i) and then refer to how many fields in that object otherCheckBoxes appear. The statement iterates over all enumerable string properties of an object.

Make sure that your check box names match the names that are in the associative syntax/object (square brackets, []) called `otherCheckBoxes`. If they do not, they will not select or be (true) and deselect or be (false) based on a 0 index. We saw an example of this `.checkThisBox` in Chapter [13](#449379_2_En_13_Chapter.xhtml).

For more information on the `for..in` loop, refer to the following link:

[`https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...in`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...in)

In addition, if you wanted to expand this example further, you could allow for formatting within the script to affect the stroke color of the check boxes when all are selected by setting the Select All check box to read-only. When the “Select all” is unchecked, then they act independently. Add a few more lines to the script as shown in bold and refer to Figure [16-9](#449379_2_En_16_Chapter.xhtml#Fig9) and the file `TMC_mineralchecklist_End_selectAll_2.pdf`.

![](images/449379_2_En_16_Chapter/449379_2_En_16_Fig9_HTML.jpg)

A screenshot of 2 good-quality fields. Left. 3 checked boxes below and the checked select all option to its top right. Right. 2 checked boxes and an unchecked box below, and a no check select all option to its top right.

Figure 16-9

Select all the check boxes at once with one master check box, and they change to green and read-only, or just check them separately

```
var otherCheckBoxes = ["CheckBox1", "CheckBox2", "CheckBox3"]; // etc.
if (this.getField("Select All").value!="Off") {
for (var i in otherCheckBoxes){
this.getField(otherCheckBoxes[i]).checkThisBox(0,true);
this.getField(otherCheckBoxes[i]).strokeColor = color.green;
this.getField(otherCheckBoxes[i]).readonly = true;
}
event.target.strokeColor = color.green;
}
else{
for (var i in otherCheckBoxes){
this.getField(otherCheckBoxes[i]).checkThisBox(0,false);
this.getField(otherCheckBoxes[i]).strokeColor = color.black;
this.getField(otherCheckBoxes[i]).readonly = false;
}
event.target.strokeColor = color.black;
}
```

Likewise, if you are concerned about resetting the field’s color using a reset button afterward, you can add the following code to a reset button along with Reset a Form on the Actions tab to see the field Reset. Refer to Figure [16-10](#449379_2_En_16_Chapter.xhtml#Fig10).

![](images/449379_2_En_16_Chapter/449379_2_En_16_Fig10_HTML.jpg)

A screenshot of the button properties dialog box that lists the options under actions tab with the add an action, including the select trigger as mouse up, and action as run a Java Script, and actions field, that has a highlighted reset button.

Figure 16-10

Add a script to a reset button when you need to reset the color and set read-only to false as well

```
var otherCheckBoxes = ["CheckBox1", "CheckBox2", "CheckBox3"]; // etc.
if (this.getField("Select All").value =="Off") {
for (var i in otherCheckBoxes){
this.getField(otherCheckBoxes[i]).checkThisBox(0,false);
this.getField(otherCheckBoxes[i]).strokeColor = color.black;
this.getField(otherCheckBoxes[i]).readonly = false;
}
this.getField("Select All").strokeColor = color.black;
}
```

## Summary

In this chapter, you looked at a way to create a type of multistate check box using a button field. You also discovered that a single check box field can select other check boxes when JavaScript is added. With slight alterations to the code, you can cause one type of field to mimic another.

# 17. Importing an Image into a Button

**Chapter Goal:** You’ll create a button that will import an icon image using a script. This could be useful for clients who want to attach an image or photo to the form.

You can do all the steps in this chapter with the latest version of Acrobat Pro with either the button field properties or the Image Field button, shown in Figure [17-1](#449379_2_En_17_Chapter.xhtml#Fig1).

![](images/449379_2_En_17_Chapter/449379_2_En_17_Fig1_HTML.png)

A screenshot of the prepare form tool has a folded piece of paper as a symbol with an add button beneath it. A small O K button, a small image button, and a large blank image field box are next to it.

Figure 17-1

The new Add an Image Field icon in the Prepare Form tool submenu

Note

If you want to work along in this lesson or review the final result, download the Chapter [17](#449379_2_En_17_Chapter.xhtml) files from the link in the introduction. The file with the label “Start” is the file without the code, and the file with the label “End” is the final result. You will also find a folder containing the original scripts and images if you would like to add them to your own PDF forms.

If you are creating your form from an original PDF, refer to the “Forms Review” section in Chapter [1](#449379_2_En_1_Chapter.xhtml).

Remember that to view the properties of a field, you must select the Prepare Form tool; only then can you right-click or double-click a field to review its properties.

## Creating the Button

Insert either a Button icon field or the Add an Image Field icon on your page. Refer to Figure [17-1](#449379_2_En_17_Chapter.xhtml#Fig1).

Make sure the Layout field on the Options tab is set to “Icon only.” Set Behavior to Invert, and set State to Up, as shown in Figure [17-2](#449379_2_En_17_Chapter.xhtml#Fig2).

![](images/449379_2_En_17_Chapter/449379_2_En_17_Fig2_HTML.jpg)

A dialogue box of button's properties. The options tab is selected. Layout as icon only, behavior as invert, state as up in icon and label section, with label and icon blank. Close button is highlighted.

Figure 17-2

Select “Icon only” for the Layout field on the Options tab

Note that it will look the same for the Add an Image Field icon; both will default to button properties. Refer to Figure [17-3](#449379_2_En_17_Chapter.xhtml#Fig3). However, the Image field will have the JavaScript already added on the Actions tab with the Mouse Up trigger.

![](images/449379_2_En_17_Chapter/449379_2_En_17_Fig3_HTML.jpg)

A window of button properties. The actions tab is selected and the mouse up option is selected as the trigger. The selected action is to execute a menu item. The actions area indicates the mouse-up option to run a Java script. The up, down, and edit options are disabled.

Figure 17-3

Run a JavaScript action on the Actions tab

Then add the following code; if it is not already added, click the Edit button to review the following code:

```
// Mouse Up script for a button
event.target.buttonImportIcon();
```

When the user clicks the blank button, the Select Icon box appears. Refer to Figure [17-4](#449379_2_En_17_Chapter.xhtml#Fig4).

![](images/449379_2_En_17_Chapter/449379_2_En_17_Fig4_HTML.jpg)

A dialog box to select an image. The file and the sample areas are blank. A Q R code is present in the sample area in the adjacent select image dialog box. A smaller size of the Q R code is below the first box.

Figure 17-4

When the button is clicked, an image or icon of your choice can be imported

The user can then browse for the image on their computer and, depending on what file formats are available to them in Reader or Pro like (`.png, .jpg,` or `.pdf)`, select the correct image as well as clear the image from the sample area.

Click the OK button, and the image will be imported as an image within the button, or click Cancel to exit without importing the Icon. The user should then save the file so that the image is retained internally within the PDF. You can refer to my file `Tourmaline Mining Corporation_mineralchecklist_End.pdf` if you need to review an example.

Note

The Image field may need to be presized if you know a client will be importing a specific sized image so that it can be seen clearly.

In the past if you did not want the user in Acrobat Reader to enter an image, you could in Acrobat Pro preset a link or path to the image file on the drive using JavaScript, providing that the user had the image in the same file location. This works with other properties such as `buttonSetIcon` or button`ImportIcon`. You would, for example, enter the path like this:

```
event.target.buttonImportIcon("/C/Users/names/Desktop/Test/Apress_QRCode.jpg");
```

This is a more advanced topic; you can find more details on this in the Acrobat online developer guide.

[`https://opensource.adobe.com/dc-acrobat-sdk-docs/library/jsdevguide/JS_Dev_AcrobatForms.html#28613`](https://opensource.adobe.com/dc-acrobat-sdk-docs/library/jsdevguide/JS_Dev_AcrobatForms.html#28613)

However, in recent versions of Acrobat Pro, doing this will throw an error in the Debugger console when the icon is clicked. The error will say doing so could be a security risk and not a trusted function, which requires more advanced privileged methods. So, as a beginner form creator, it is best to let the user enter their own image via the button and save the file, or you can place the image in the button and save the file before sending it a client. For now, just use this code in the button field:

```
event.target.buttonImportIcon();
```

## Summary

In this chapter, you looked at how to import an image into a button. You also discovered the Add an Image Field icon, which is easy to do when the code is already added for you. In addition, you learned about the security risk warning if you add a direct path in the JavaScript directly to the image field.

In the next chapter, we will look at how to work with multiple formatting, specifically for numbers.

# 18. Multiple Formatting

**Chapter Goal:** One of the most laborious tasks that I have found when creating forms in Acrobat is formatting many text fields to Numbers. Wouldn’t it be nice if Acrobat were like MS Excel and you could format the fields all at once? And what if you need to format them all again? Do you want to go through them one at a time? What if you could update them quickly?

Until the day that Acrobat comes up with a solution, there are two possible ways to deal with this problem: do it yourself or call a professional. This chapter discusses both options and shows you how to add custom symbols to both currency and noncurrency numbers.

## The Problem of Multiple Formatting

Accessing the format is straightforward with a single selected text field. You can select each text field or drop-down list field, one at a time, and perform this task, as shown in Figure [18-1](#449379_2_En_18_Chapter.xhtml#Fig1).

![](images/449379_2_En_18_Chapter/449379_2_En_18_Fig1_HTML.jpg)

A dialog box of text field properties. The format tab is selected. The number option is selected in the format category. The decimal places are up to 2\. The symbol location is selected as before with space. The dollar symbol is used for currency.

Figure 18-1

Text field properties on the Format tab

However, when you Shift+click to select multiple text fields and right-click to choose Properties from the pop-up menu, the Format, Validate, and Calculate tabs disappear. Refer to Figure [18-2](#449379_2_En_18_Chapter.xhtml#Fig2).

![](images/449379_2_En_18_Chapter/449379_2_En_18_Fig2_HTML.jpg)

A magnified view of the text field properties dialog box along with 5 tabs of general, appearance, position, options, and actions. The general tab is selected.

Figure 18-2

The Text Field Properties dialog box’s tab area where three tabs have disappeared because multiple fields are selected

So, you’re stuck with the task of formatting them one at a time. Refer to Figure [18-3](#449379_2_En_18_Chapter.xhtml#Fig3).

![](images/449379_2_En_18_Chapter/449379_2_En_18_Fig3_HTML.jpg)

A dialog box of text field properties. The format tab is selected. The drop-down menu in the format category has the option, number highlighted. The various options in the list are none, number, percentage, date, time, special, and custom.

Figure 18-3

The Format tab is available only through single field selection

I can understand why the Acrobat developers might remove the Validate and Calculate tabs, since these areas would contain custom scripts that you would not want to overwrite in a mass conversion. Yet the same could be said for the Actions tab, but it remains. If both fields have slightly different actions, then the Mouse Up trigger shows the word *Varies*, and you cannot edit either action while both fields are selected. Refer to Figure [18-4](#449379_2_En_18_Chapter.xhtml#Fig4).

![](images/449379_2_En_18_Chapter/449379_2_En_18_Fig4_HTML.jpg)

A snip of the actions dialog box. The mouse up title has the text, varies in the box. Up, down, and edit buttons are disabled, delete button is enabled.

Figure 18-4

Text Field Properties dialog box’s Actions tab when two fields with different actions are selected

As in all cases, it is important to give your fields unique names. Refer to Figure [18-5](#449379_2_En_18_Chapter.xhtml#Fig5) and to the file `TestingForm_format_test_End.pdf.`

![](images/449379_2_En_18_Chapter/449379_2_En_18_Fig5_HTML.jpg)

A snip of the general styles of the text fields with 5 rows for addition, subtraction, multiplication, division, and complex examples., 0, 1, and 2.

Figure 18-5

The text fields in form mode with their unique names in Edit mode

Note

If you want to work along in this lesson or review the final result, download the Chapter [18](#449379_2_En_18_Chapter.xhtml) files from the link in the inntroduction. The file with the label “Start” is the file without the code, and the file with the label “End” is the final result. You will also find a folder containing the original scripts if you would like to add them to your own PDF forms.

If you are creating your form from an original PDF, refer to the “Forms Review” section in Chapter [1](#449379_2_En_1_Chapter.xhtml).

Remember that to view the properties of a field, you must select the Prepare Form tool; only then can you right-click or double-click a field to review its properties.

## Option 1: Do It Yourself

As mentioned in previous chapters, when you want to affect something globally, you need to create document JavaScripts. Refer to Figure [18-6](#449379_2_En_18_Chapter.xhtml#Fig6).

![](images/449379_2_En_18_Chapter/449379_2_En_18_Fig6_HTML.jpg)

An icon of 2 curly brackets with an add option beneath it along with document Java Scripts icon. A dialog box of document Java Scripts has script name as custom function, with the same selected in the box below. An editor below has code for function of P F custom format.

Figure 18-6

The Document JavaScripts dialog box used for formatting the numbers

In this case, there are two scripts. CustomFunction makes sure the number has a price ($) sign, and CustomFunctionNoStyle has no price sign.

The following are the scripts and functions used for both.

### Price Script

Here’s the price script:

```
function PFCustom_Format() {
// array of fields to apply custom format and keystroke to
var aFormatFields = new Array("Addition0", "Addition2", "Subtraction0", "Subtraction2", "Multiplication0", "Multiplication2", "Division0", "Division2", "Complex_example0", "Complex_example2");
// define the varibles for the custom number formatting and keystroke
var nDecimal = "2"; // number of decimals
var separStyle = "0"; // separator sytle
var negativeStyle = "0"; // negative display style
var currencyStyle = "0"; // currency style
var symCurrency = "\"$\""; // currency symbol
var symCurrencyPrepend = "true"; // logical to prepend (add before) currency symbol
// build the format string
var strFormat = nDecimal + ", " + separStyle + ", " + negativeStyle + ", " + currencyStyle +", " + symCurrency + ", " + symCurrencyPrepend;
// set the custom format and keystroke for the fields
for (i = 0; i < aFormatFields.length; i++) {
this.getField(aFormatFields[i]).setAction("Format", "AFNumber_Format(" + strFormat + ")");
this.getField(aFormatFields[i]).setAction("Keystroke", "AFNumber_Keystroke("+ strFormat + ")");
} // end for loop
return;
} // end custom format function
```

### No Price Script

Here’s the no price script:

```
function PFCustom_Format2() {
// array of fields to apply custom format and keystroke to
var aFormatFields2 = new Array("Addition1", "Subtraction1", "Multiplication1","Division1", "Complex_example1");
// define the varibles for the custom number formatting and keystroke
var nDecimal = "2"; // number of decimals
var separStyle = "0"; // separator sytle
var negativeStyle = "0"; // negative display style
var currencyStyle = "0"; // currency style
var symCurrency = "\"\""; // currency symbol
var symCurrencyPrepend = "false"; // logical to prepend(add before) currency symbol
// build the format string
var strFormat2 = nDecimal + ", " + separStyle + ", " + negativeStyle + ", " + currencyStyle +", " + symCurrency + ", " + symCurrencyPrepend;
// set the custom format and keystroke for the fields
for (i = 0; i < aFormatFields2.length; i++) {
this.getField(aFormatFields2[i]).setAction("Format", "AFNumber_Format(" + strFormat2 + ")");
this.getField(aFormatFields2[i]).setAction("Keystroke", "AFNumber_Keystroke("+ strFormat2 + ")");
} // end for loop
return;
} // end custom format function
```

The second script shows the areas that are different from the first script in **bold**. I changed the names of some of the variables and the array as a precaution so that the two functions would not become conflicted.

The following variables are all prebuilt into the inner format scripts (`AFNumber_Format` and `AFNumber_Keystroke`) that are controlling this Format tab:

*   `var nDecimal = "2"; // number of decimals`

*   `var separStyle = "0"; // separator style`

*   `var negativeStyle = "0"; // negative display style`

*   `var currencyStyle = "0"; // currency style`

*   `var symCurrency = "\"\""; // currency symbol`

*   `var symCurrencyPrepend = "false"; // logical to prepend (add before) append(after) currency symbol`

`nDecimal` controls the number of decimal places. Refer to Figure [18-7](#449379_2_En_18_Chapter.xhtml#Fig7).

![](images/449379_2_En_18_Chapter/449379_2_En_18_Fig7_HTML.jpg)

A snip of the drop down menu to select the decimal places. 2 is selected.

Figure 18-7

Format the number of decimal places

`separStyle` is the separator style for the 000s and decimal point. Refer to Figure [18-8](#449379_2_En_18_Chapter.xhtml#Fig8).

![](images/449379_2_En_18_Chapter/449379_2_En_18_Fig8_HTML.jpg)

A snip of the drop down menu to select the separator style. It has various styles for the separation of the numbers and decimals. 1 comma 234 point 5 6 is selected.

Figure 18-8

Format the separator style

```
0 ="," Thousands and "." decimal point with comma Example: 1,000.00
1= "." Decimals only with no comma separation Example: 1000.00
2 = "."Decimal point and "," Thousands Example: 1.000,00
3 = "," Thousands Only Example: 1000,00
4 =" ‘ " quote mark and decimal "." Example: 1’000.00
```

`negativeStyle` controls the negative style. Refer to Figure [18-9](#449379_2_En_18_Chapter.xhtml#Fig9).

![](images/449379_2_En_18_Chapter/449379_2_En_18_Fig9_HTML.jpg)

A snip of the negative number style has 2 check boxes to show parenthesis and use red text.

Figure 18-9

Format the negative number style

```
0 = "-" in black
1 = red text for negative number
2 = parentheses black
3 = parentheses red
```

`currencyStyle` refers to currency style set to 0 (in most cases it is unused; it is a reserved feature).

`symCurrency` refers to the type of symbol used for currency. Refer to Figure [18-10](#449379_2_En_18_Chapter.xhtml#Fig10).

![](images/449379_2_En_18_Chapter/449379_2_En_18_Fig10_HTML.jpg)

A snip of the drop down menu to select the currency symbol. The drop-down list has different currency symbols, of which dollar is selected.

Figure 18-10

Format the currency symbols

In this example, you use `\"$\"` for dollar sign and `\"\"` for none. If you had a foreign currency, you might need to look up the Unicode code for how to create the symbol. Some examples are shown here:

*   Euro sign, €: `\u20AC`

*   Pound, £: `\u00A3`

*   Yen, ¥: `\u00A5`

*   Won, ₩: `\u20A9`

See [`https://en.wikipedia.org/wiki/Currency_Symbols_(Unicode_block)`](https://en.wikipedia.org/wiki/Currency_Symbols_(Unicode_block)).

If you want to use a different Unicode code, you may need to look it up and test it; we will look at a noncurrency symbol later in this chapter. Also, the font you choose on the Appearance tab and custom formats can affect how the symbol may appear.

`symCurrencyPrepend` is set either to true ($1.00) to appear before the number or to false (1.00$) to appear after, depending on where you want the symbol to be placed. Adding a space within your `symCurrency` variable with the Unicode symbol will give you the extra before or after space. Refer to Figure [18-11](#449379_2_En_18_Chapter.xhtml#Fig11).

![](images/449379_2_En_18_Chapter/449379_2_En_18_Fig11_HTML.jpg)

A snip for selecting the symbol location. The options in the drop down list are before with space, before with no space, after with space, and after with no space. Before with space is selected.

Figure 18-11

Format the prepend space

Note

While this is visually correct, you may notice while editing your JavaScript that the Format tab will say “After with space” rather than “Before no space.” Do not alter this formatting here; do it only in the document JavaScript, because it will unlink the global formatting to this field. We’ll look at how to relink the format later in the chapter.

In both functions, you separate your associative syntax and object literals in the parentheses, (), of the new array with the fields that you want formatted into two functions, with or without dollar signs. This avoids confusion and keeps it organized. They will later be indexed in the square brackets, as in [i], as required.

Next, the variable string sets the order:

```
var strFormat2 = nDecimal + ", " + separStyle + ", " + negativeStyle + ", " + currencyStyle +", " + symCurrency + ", " + symCurrencyPrepend;
```

The `for()` loop of each function then runs through the arrays, distributing the information in the correct order. Refer to the `for()` loop statements in Chapters [10](#449379_2_En_10_Chapter.xhtml), [13](#449379_2_En_13_Chapter.xhtml), and [16](#449379_2_En_16_Chapter.xhtml).

```
for (i = 0; i < aFormatFields2.length; i++) {
this.getField(aFormatFields2[i]).setAction("Format", "AFNumber_Format(" + strFormat2 + ")");
this.getField(aFormatFields2[i]).setAction("Keystroke", "AFNumber_Keystroke("+ strFormat2 + ")");
}
```

The `.setAction()` method sets the formatting and keystroke for the custom formatting of the fields indexed within the array object.

The final word `return` allows the information to display properly from the associative syntax/objects for the various fields.

If you were to test the form at this point, you might notice a bug. The fields for some reason keep the format of None or do not change after making edits. Refer to Figure [18-12](#449379_2_En_18_Chapter.xhtml#Fig12).

![](images/449379_2_En_18_Chapter/449379_2_En_18_Fig12_HTML.jpg)

A magnified snip of the text field properties dialog box along with tabs of general, appearance, position, options, actions, format, validate, and calculate. The format tab is selected. None option is selected in the format category.

Figure 18-12

The format category is set to a category of None

Why is this happening?

To get the form working, in this case you need to refresh the format. You need to do this just once for each function so that the other fields accessing the associative syntax in their functions will work properly. If you make a major change to the function, always remember that it needs a refresh to affect the other fields. For example, choose a field that you know is part of the function `PFCustom_Format,` which in this case would be a field with the price symbol. An example is the field Addition0\. Choose Custom on the Format tab, not the Number format category. Refer to Figure [18-13](#449379_2_En_18_Chapter.xhtml#Fig13).

![](images/449379_2_En_18_Chapter/449379_2_En_18_Fig13_HTML.png)

A snip of a drop down menu with custom option. A list of options in the drop down are none, number, percentage, date, time, special, and custom. The custom format style has a blank rectangular space, with a highlighted edit button.

Figure 18-13

Choose Custom on the Format tab

In the Custom Format Script box, click the Edit button. Then in the JavaScript Editor, enter the following:

```
PFCustom_Format();
```

Confirm by clicking the OK button to exit. The formatting will then automatically switch to Number. Refer to Figure [18-14](#449379_2_En_18_Chapter.xhtml#Fig14).

![](images/449379_2_En_18_Chapter/449379_2_En_18_Fig14_HTML.jpg)

A dialog box on text field properties along with tabs of general, appearance, position, options, actions, format, validate, and calculate. The format tab is selected. The number category is selected in the select format category option.

Figure 18-14

On the Format tab, the category automatically changes to Number

The following is the other function you may have to apply once to an appropriate field:

```
PFCustom_Format2();
```

Once this is accomplished, close the Properties panel, and all the fields that use these two functions should be set automatically and you don’t have to go into each one. Test your form at this point.

Note while not part of this lesson, I also added some custom JavaScript to my Calculate tab on certain fields to calculate the result similar to what was done in Chapters [5](#449379_2_En_5_Chapter.xhtml) and [6](#449379_2_En_6_Chapter.xhtml).

### Adding a Degree Symbol and Formatting Range

Likewise, you do not have to limit yourself to just using currency symbols; as we saw in Chapter [16](#449379_2_En_16_Chapter.xhtml), there are many Unicode symbols that are available. One symbol that I have found to be useful is the degree symbol. This can appear in two main varieties: you may be inputting a temperature or an angle. In the past, I would just have made sure to add the degree symbol outside the form. But now you can have it as part of the form field when typing in your numbers. The following examples shown next use red text for a negative style.

The following code shows how that could appear as a document JavaScript. Refer to Figure [18-15](#449379_2_En_18_Chapter.xhtml#Fig15).

![](images/449379_2_En_18_Chapter/449379_2_En_18_Fig15_HTML.jpg)

A dialog box of document Java Scripts. The script name is in degree format. Degree format is selected from a list in the box below. An editor has codes for function of degree format in the column beneath. Close button is highlighted.

Figure 18-15

Document JavaScript for degrees in the dialog box

Look at the script `degreeFormat``:`

```
function degreeFormat() {
var aFormatFields = new Array("Degree0", "Degree1", "Degree2");
var nDecimal = "2"; // number of decimals
var separStyle = "1"; // separator sytle
var negativeStyle = "1"; // negative display style
var currencyStyle = "0"; // currency style
var symCurrency = "\"\u00b0\""; // degree symbol
var symCurrencyPrepend = "false"; // logical to prepend(add before) currency symbol
var strFormat = nDecimal + ", " + separStyle + ", " + negativeStyle + ", " + currencyStyle +", " + symCurrency + ", " + symCurrencyPrepend;
for (i = 0; i < aFormatFields.length; i++) {
this.getField(aFormatFields[i]).setAction("Format", "AFNumber_Format(" + strFormat + ")");
this.getField(aFormatFields[i]).setAction("Keystroke", "AFNumber_Keystroke("+ strFormat + ")");
} // end for loop
return;
} // end custom format function
```

Alternatively, if you just want to add the degree symbol to one or two fields, you can write a short version of this in the document JavaScript, as shown in the script `degreeFormat2``.`

```
function degreeFormat2()
{
AFNumber_Format (2,1,1,0,"\u00b0",false);
AFNumber_Keystroke(2,1,1,0,"\u00b0",false);
}
```

In either example, go to the select text field and then set Format to Custom and add the reference to the function as you saw earlier. With the short example, you would need to set each field one at a time, but you might find this easier as you only have to copy and paste this line of code into the Custom Format Script box for each field. Just be aware that if you alter the negative number style to another setting to check your Appearance panel if the colors don’t come in at first as intended when you now enter a negative number as it may be set to the text color of red and you should adjust this back to black. Refer to Figure [18-16](#449379_2_En_18_Chapter.xhtml#Fig16).

![](images/449379_2_En_18_Chapter/449379_2_En_18_Fig16_HTML.jpg)

A dialog box of text field properties, with the format tab selected. The custom format category is selected. The custom format script has degree format 2 code written in the space provided in the custom options. An edit button is next to it.

Figure 18-16

Set the custom JavaScript on the Format tab

Then you can set your custom range to “if less than 0” or “greater than 360” with a warning in the document JavaScript.

```
function customval(){
if (event.value 360 ){
app.beep(0);
app.alert("Invalid entry for degree you must enter a number between 0 and 360.");
event.rc = false;
}
}
```

Note

Adding an `app.beep` method is optional as the alert message will beep regardless.

Then you can add it to each field’s Validate tab as a custom validation script with references to the Function `customval();`. Refer to Figure [18-17](#449379_2_En_18_Chapter.xhtml#Fig17).

![](images/449379_2_En_18_Chapter/449379_2_En_18_Fig17_HTML.jpg)

A dialog box of text field properties with the validate tab expanded. Run custom validation script is enabled, with entry of custom val parentheses semicolon, in the box. Edit button is highlighted. The highlighted rectangular box below has text, 27.00 degrees.

Figure 18-17

Validate tab with custom validation reference

The reason you must do this on the Validate tab is that the option “Field Value is in range” will not be accessible. If a range is not required, then you do not have to apply this to the Validate tab. If incorrect, the field will reset itself after exiting the alert.

You can refer to Figure [18-17](#449379_2_En_18_Chapter.xhtml#Fig17) and my file `Degree_FormatExamples.pdf.`

Note

In my examples, I used a longer description for the various format variables to make them clear to understand. However, you may find online other documentation that writes the variable’s names like this:

`AFNumber_Format(nDec, sepStyle, negStyle, currStyle, strCurrency, bCurrencyPrepend);`

`AFNumber_Keystoke(nDec, sepStyle, negStyle, currStyle, strCurrency, bCurrencyPrepend)`;

Either way of writing the name will work, but make sure that in the end you match up your variable names in the rest of the function.

Other `AF` formats and keystrokes, though not used as often, that you may want to research on your own are `AFDate_Format(cFormat), AFTime_Format(pft), AFPercent``_Format(nDec, sepStyle), and AFSpecial_Format(psf)`.

## Option 2: Contact a Professional

While the Number format example is fast and eliminates errors, it would be nice if there was an even faster way that even included a dialog box. Thankfully, after a lot of research, I found a JavaScript developer who has devoted a lot of time to figuring out this very issue.

His name is Gilad Denneboom (username Try67). He has created an extension for Acrobat on his website that is called “Apply Format to Multiple Text Fields.” You can get it at [`https://www.try67.com/tool/acrobat-apply-format-to-multiple-text-fields`](https://www.try67.com/tool/acrobat-apply-format-to-multiple-text-fields).

Figure [18-18](#449379_2_En_18_Chapter.xhtml#Fig18) shows what it looks like.

![](images/449379_2_En_18_Chapter/449379_2_En_18_Fig18_HTML.jpg)

A dialog box of mass format number fields. The decimal places is selected as 2\. The currency symbol selected is dollars. The other options are the separator style and custom currency symbol.

Figure 18-18

The Mass Format Number Fields dialog box

His formatting also allows you to mass format the following:

*   Percentage

*   Date

*   Time

*   Special

If you think you will be doing a lot of this type of formatting, I think his $60 product is worth it.

He also has other offers such as Acrobat Mass Edit Fields Actions `at` [`https://www.try67.com/tool/acrobat-mass-edit-fields-actions`](https://www.try67.com/tool/acrobat-mass-edit-fields-actions). This one allows you to even apply formatting to areas like the Validate and Calculate tabs through his dialog boxes.

Before you buy, if you have questions about the programs like which is the best product, will it work for your Version of Acrobat Pro, or how to install it, ask him first. He is very helpful; he assisted me on a few questions that I had as I wrote this book. He also often answers questions in the Adobe and Acrobat user forums.

## Summary

In this chapter, you looked at ways to format multiple text fields with Number formatting. While you can form numbers easily for one document, it’s important to find the best option if you must create a lot of forms with numbers. Your choices are to do it yourself or contact a professional.

# 19. Digital Signatures and Barcodes

**Chapter Goal:** I haven’t yet discussed two form fields in great detail in regard to JavaScript: digital signatures and barcodes. Thus, this chapter focuses on them. Let’s first look at how they and other fields relate to security.

Note

If you want to work along in this lesson or review the final result, download the Chapter [19](#449379_2_En_19_Chapter.xhtml) files from the link in the introduction. The file with the label “Start” is the file without the code, and the file with the label “End” is the final result. You will also find folders containing the original files and scripts if you would like to add them to your own PDF forms.

If you are creating your form from an original PDF, refer to the “Forms Review” section in Chapter [1](#449379_2_En_1_Chapter.xhtml).

Remember that to view the properties of a field, you must select the Prepare Form tool; only then can you right-click or double-click a field to review its properties.

## Digital Signatures and Security

Basic security can be added to text fields. See the file `TMC_Password_Access_Example_End.pdf` for an example. Refer to Figures [19-1](#449379_2_En_19_Chapter.xhtml#Fig1) and [19-2](#449379_2_En_19_Chapter.xhtml#Fig2).

![](images/449379_2_En_19_Chapter/449379_2_En_19_Fig2_HTML.jpg)

A snip of password access example table. Password with asterisks has view, submit, and rest buttons. Unlock this field is on top right. Rows 2 to 4 are for file locations 1, 2, and 3 with submit file options. Reset file fields is at the bottom. A warning message window states fields are unlocked.

Figure 19-2

Entering the correct password unlocks the fields and shows buttons. You can also reset all the fields back to read-only

![](images/449379_2_En_19_Chapter/449379_2_En_19_Fig1_HTML.jpg)

A snip of a 3-column table of password access example. The password row has an entry bar with view and submit buttons. Unlock this field option is on top right. Blank rows 2, 3, and 4 are for file locations 1, 2, and 3\. Reset file fields is at the bottom right. Third column is blank.

Figure 19-1

If you don’t know the password, you can’t unlock the fields and show the buttons that allow you to add file location links

Before you enter the password in the Text1Password field and click the Submit button, all text fields except the password field and the check box have a setting of read-only on the General tab.

The Text1Password field has a setting of Password on the Options tab, as shown in Figure [19-3](#449379_2_En_19_Chapter.xhtml#Fig3). This setting will display only a series of asterisks (***); the actual text will remain hidden. No other JavaScript code is directly applied to this field.

![](images/449379_2_En_19_Chapter/449379_2_En_19_Fig3_HTML.jpg)

A window of properties of the text field. The options tab is selected. The alignment selected is left and the default value space is blank. The password and scroll-long text are checked. Other options on the file are disabled.

Figure 19-3

The Options tab has a setting of Password so that the actual text is hidden

Here is the JavaScript code for the Submit button on the Actions tab after selecting a Mouse Up trigger and Run a JavaScript:

```
//Submit Password Button
var b=this.getField("Text1Password");
var c=this.getField("Text2");
var file1=this.getField("SubmitFile");
var btn1=this.getField("Submitfilebtn");
var d=this.getField("Reset");
if(b.value=="password"){
c.readonly =false;
file1.readonly=false;
btn1.display=display.visible;
d.display=display.visible;
app.alert("Fields are Unlocked",3,0);
}
else{
app.alert("Wrong Password");
b.value="";
}
```

Only if you enter the value of **password** will you be able to access the other fields and buttons. Upon clicking the Submit button, if the value is correct, the read-only fields (Text2, SubmitFile.1, SubmitFile.2, SubmitFile.3) will change to be editable, and the hidden buttons (SubmitFilebtn.1, SubmitFilebtn.2, SubmitFilebtn.3, and Reset) will display. You will also get an alert that the fields are unlocked. Note how you do not have to name all the buttons or fields in this example that have a period and number on the end; as long as they have the same starting name before the period, they are included as part of a type of array object.

However, if you enter the wrong value and click Submit, you will get an alert that you entered the wrong password, and the field will reset itself, so you must try again. Refer to Figure [19-4](#449379_2_En_19_Chapter.xhtml#Fig4).

![](images/449379_2_En_19_Chapter/449379_2_En_19_Fig4_HTML.jpg)

A popup window of warning for Java script window. It reads, wrong password, and the O K button has highlighted borders. The password space is blank along with the submit button.

Figure 19-4

The Password field returns to blank if the password is incorrect

However, some people have difficulty entering a password when it is set to asterisks (***). To help them out, you can add a check box so that they can turn on or off the password setting. This will allow them to check whether they entered the password correctly before clicking the Submit button. For the check box field called Check Box1, I entered the following code on the Actions tab and selected the Mouse Up trigger:

```
//Show Hide the password
if(event.target.value !="Off"){
this.getField("Text1Password").password=false;
}
else{
this.getField("Text1Password").password=true;
}
```

Like `.readonly`, `.password` is another property that you can access like a Boolean of true and false. In this case, if `! =` `"Off"` or ON is checked, set it to false to show the actual text. When it’s unchecked, set it to true to show the password setting. Refer to Figure [19-5](#449379_2_En_19_Chapter.xhtml#Fig5).

![](images/449379_2_En_19_Chapter/449379_2_En_19_Fig5_HTML.jpg)

A snip of the window of the check box properties. The actions tab is selected. A bar has entry of the text password, with the view box checked. Asterisks are in the bar below, with the view unchecked.

Figure 19-5

Use a check box action to hide and show a password with the code on the Actions tab

If the password is entered correctly, once the Reset button is visible, you can reset the form, make the text fields read-only, and hide some of the buttons. Refer to Figure [19-6](#449379_2_En_19_Chapter.xhtml#Fig6).

![](images/449379_2_En_19_Chapter/449379_2_En_19_Fig6_HTML.jpg)

A dialog box of button properties. The actions tab is selected. The mouse-up trigger is used to execute a menu item action. Asterisks entered for password, with submit and reset buttons are on top of the box.

Figure 19-6

The Reset button has two actions on the Actions tab

For the Reset button on the mouse up, besides resetting the form’s five text fields and the check box with the action of “Reset a form”, you can add the following code for Run a JavaScript in the editor:

```
//Reset Password Field with check box setting, File Fields and Button
var b=this.getField("Text1Password");
var c=this.getField("Text2");
var file1=this.getField("SubmitFile");
var btn1=this.getField("Submitfilebtn");
var d=this.getField("Reset");
if(b.value==""){
c.readonly =true;
file1.readonly =true;
btn1.display =display.hidden;
app.alert("Fields are Locked again",3,0);
d.display= display.hidden;
b.password = true;
}
```

Not only does this lock (make it read-only), hide, and reset fields and password properties, but you also get an alert message saying that the fields are locked again. Refer to Figure [19-7](#449379_2_En_19_Chapter.xhtml#Fig7).

![](images/449379_2_En_19_Chapter/449379_2_En_19_Fig7_HTML.jpg)

A popup window of javascript warning. The info button indicates that the fields are locked again. The O K button is highlighted at the borders.

Figure 19-7

Alert that appears when the fields have been locked again upon reset

Note

Upon clicking OK in the Alert dialog box, the Reset button hides, and the password resets. These are the last items in the conditional statement that had to be fulfilled. This is always something to keep in mind when you set the order of your code and you notice that it is not activating in the order you expect. In this kind of situation, you may have to move a line of code or an action above or below another on the Actions tab or editor for it to operate as you would expect.

In these examples, we used the word *locked* to refer to a field that was set to read-only. This is not the same as when we lock field properties in the dialog by clicking the Locked check box. Refer to the lower-left corner of Figure [19-6](#449379_2_En_19_Chapter.xhtml#Fig6). When this is checked, you can still enter text into the field, but when you review the properties and tabs, they are grayed out, and you cannot make changes to the properties including the Actions tab. If you find that any of your fields no longer allow you to edit the properties, make sure to unlock the “locked” check box.

While not part of security, you can add this code to the Submit File buttons on the Actions tab for the Mouse Up trigger and the Run a JavaScript setting:

```
Submitfilebtn.1
// Additional Script for Submit a File Button
var file1 = this.getField("SubmitFile.1");
file1.browseForFileToSubmit();
Submitfilebtn.2
var file2 = this.getField("SubmitFile.2");
file2.browseForFileToSubmit();
Submitfilebtn.3
var file3 = this.getField("SubmitFile.3");
file3.browseForFileToSubmit();
```

The text fields that correspond to these buttons have this setting added on the Options tab: “Field is used for file selection.” When the Submit File button is clicked, a dialog box will open, allowing you to enter a text version of the location of a specific file on your computer’s drive. Without these action settings for the buttons and these specific text field settings on the Options tab, the Open dialog box will not appear. Refer to Figure [19-8](#449379_2_En_19_Chapter.xhtml#Fig8).

![](images/449379_2_En_19_Chapter/449379_2_En_19_Fig8_HTML.png)

A dialog box of text field properties. The options tab is expanded and the left alignment is selected. Field for file selection is checked. File location 1, file location 2, and file location 3 and submit buttons present along each space provided to type in the file locations, are on top of dialog.

Figure 19-8

The properties on the Options tab so that this field will work with the Submit File button

Note that the Reset File Fields button in this example resets the text in the three file fields while you work but has no security. Refer to Figure [19-2](#449379_2_En_19_Chapter.xhtml#Fig2).

JavaScript, while it can be used for some basic security, is not always the most secure or reliable solution when it comes to encryption because these areas can be easily accessed if you have Acrobat Pro and have not set additional password security.

### Working with Digital Signatures and Other Adobe Security Tools

In regard to signatures and security, I recommend reading *Adobe Acrobat DC Classroom in a Book* by Lisa Fridsma and Brie Gyncild, especially her coverage of signatures and security. I find it has the most up-to-date PDF information in regard to secure encrypted signatures that should be used in a company that requires professional certification and IDs. Likewise, you can also contact Adobe if you need certificates or any similar security.

While not relevant to this book, some Adobe tools that you should consider learning more about, if security is part of your workflow, are found under Forms & Signatures and Protect & Standardize. These include the Certificates, Fill & Sign, Protect, and Redact tools. There are also other e-signature form options, which we will talk about a bit more in Chapter [20](#449379_2_En_20_Chapter.xhtml). Refer to Figure [19-9](#449379_2_En_19_Chapter.xhtml#Fig9).

![](images/449379_2_En_19_Chapter/449379_2_En_19_Fig9_HTML.jpg)

14 tools indicated are of forms and signatures, and protect and standardize options. Includes, request E sign, fill and sign, prepare a form, certificates, send in bulk, add E sign branding, collect payments, protect, redact, P D F standards, optimize P D F, print production and accessibility.

Figure 19-9

Additional Adobe security tools

Note that while not directly used with JavaScript, if your forms will be for the visually impaired, you may want to use the Accessibility tool to prepare your form fields as well as other reading options. This is a more advanced topic and not required for this book. You can learn more about accessibility, tags, and reading order at [`https://helpx.adobe.com/acrobat/using/create-verify-pdf-accessibility.html`](https://helpx.adobe.com/acrobat/using/create-verify-pdf-accessibility.html). Refer to Figure [19-10](#449379_2_En_19_Chapter.xhtml#Fig10).

![](images/449379_2_En_19_Chapter/449379_2_En_19_Fig10_HTML.png)

A dialog box for setting the reading order from the accessibility menu. The various options for formatting are in a drag and drop format. The options are text, figure, form field, figure, headings, formula, cell, note and artifacts.

Figure 19-10

Accessibility tool displaying some of the tool’s options that relate to form fields

However, in this chapter, because the form does not require high-level security, I’ve included a file (`TMC_Signature_Agreement_End.pdf`) in this chapter’s folder that has JavaScript applied to the digital signatures, which you may want to explore. This file uses a combination of signature fields and buttons to do the following:

*   Make the signature-related text fields read-only when you enter your signature in the field, and then you can click the Submit to Supervisor button.

*   Show or hide fields when buttons are clicked.

*   Reset part or the entire form with the click of a specific button.

In the Digital Signature Properties field, for employee.signature, take a moment to look at the Signed tab for the field, which is available to digital signatures only once you have configured your digital ID. Unlike other text fields, digital signatures do not have Format, Validate, and Calculate tabs, so the only places you can add actions are the Actions and Signed tabs. You can also use the global Document JavaScript area for formatting. Refer to Figures [19-11](#449379_2_En_19_Chapter.xhtml#Fig11), [19-12](#449379_2_En_19_Chapter.xhtml#Fig12), and [19-13](#449379_2_En_19_Chapter.xhtml#Fig13).

![](images/449379_2_En_19_Chapter/449379_2_En_19_Fig13_HTML.jpg)

A snip of the blank field to insert the employee's signature. The signed tab is selected in the digital signature properties dialog box below. This script executes when the field is signed option is selected. The other options are disabled. Close button is highlighted.

Figure 19-13

Digital signature properties on the Signed tab when “This script executes when a field is signed” is selected

![](images/449379_2_En_19_Chapter/449379_2_En_19_Fig12_HTML.jpg)

A snip of the window on digital signature properties. The signed tab is selected. The mark as the read-only option is selected. Just these fields option is selected from the drop-down list.

Figure 19-12

Example of a digital signature on the Signed tab set to “Mark as read-only” when “Just these fields” is selected from the list

![](images/449379_2_En_19_Chapter/449379_2_En_19_Fig11_HTML.jpg)

A screenshot of the tabular format to insert the details of the employees and the supervisor. The details collected are name, info, signature, and date. The submit to supervisor button is beneath the table. An icon of prepare a form with an add option is indicated below the form.

Figure 19-11

Example of a digital signature found in the Prepare form tool

Basically, as one signature is filled in by the employee and you click the Submit to Supervisor button, the next signature becomes available to the next supervisor. The example is the same as showing and hiding using fields and a combination of buttons. Though this is low-level security, it provides a way to make sure files are signed off on in the correct order. It also places a restriction on who can reset what fields. This method is more precise than simply choosing “Mark as read-only.”

The following are some code examples for use with signatures and buttons.

The following can be applied to the digital signature field employee.signature on the Signed tab:

```
//Employee Signature Field
var a = this.getField("employee");
a.readonly = true;
```

The following can be applied to the button employee1.submitForm on the Actions tab as a Mouse Up trigger with Run a JavaScript selected:

```
//Submit to Supervisor button
var a = this.getField("supervisor");
a.display = display.visible;
```

To that button I then added an additional show/hide a field action on mouse up to hide the button named `employee1.resetForm`.

The following can be applied to the button `employee1.resetForm`. At first, I added the JavaScript action where (a) in this case refers to all with the name employee, but be aware it could be other fields as well. So, I commented that line out. Alternatively, you could, as you saw in Chapter [9](#449379_2_En_9_Chapter.xhtml), just set a separate action for the fields you want to hide, or you could change the code to `this.resetForm(["employee"]);` to get all the fields that start with `employee` but not `employee1`. Refer to Figure [19-14](#449379_2_En_19_Chapter.xhtml#Fig14).

```
//Employee Reset Button
var a = this.getField("employee");
//this.resetForm(a);
this.resetForm(["employee"]);
```

The following can be applied on the Actions tab to the button field `supervisor.submitForm`:

```
//Submit to HR Button
var a = this.getField("supervisor");
var b = this.getField("employee1.submitForm");
var c = this.getField("employee1.resetForm");
a.display = display.visible;
b.display = display.hidden;
c.display = display.hidden;
```

The following can be applied on the Actions tab to the button field `supervisor.resetonly`. You can then set only the supervisor fields.

```
//Only Supervisor Button
var a = this.getField("supervisor");
this.resetForm(["supervisor"]);
```

The following can be applied on the Actions tab to the button field `supervisor.resetForm` to reset both the employee and supervisor fields.

```
// Supervisor Reset Button
var a = this.getField("supervisor");
var b = this.getField("employee");
this.resetForm(["employee","supervisor"]);
```

The following can be applied on the Actions tab to the button field `reset`:

```
//Reset the whole form Button
var a = this.getField("supervisor");
var b = this.getField("employee1.submitForm");
var c = this.getField("employee1.resetForm");
var d = this.getField("employee");
a.display = display.hidden;
b.display = display.visible;
c.display = display.visible;
this.resetForm(["employee","supervisor"]);
```

![](images/449379_2_En_19_Chapter/449379_2_En_19_Fig14_HTML.jpg)

A snip of the space provided to insert the signature of the user in Adobe.

Figure 19-14

A Digital Signature field in the Signature Agreement PDF file

Notice how, as in Chapter [13](#449379_2_En_13_Chapter.xhtml), you are using `display.hidden` and `visible` to hide and show certain fields. Read-only (`.readonly`) in the employee signature field is a Boolean that can be set to either true or false and determines whether the field is read-only or not. This script executes only once the field has been signed and can’t be undone by the employee, and then only the supervisor can reset the field.

#### Signature Field Formatting

Like text fields, you can also alter the Appearance properties of the digital signature using the Appearance tab or Document JavaScript dialog box. For example, use `f.type == "signature"` as in Chapter [10](#449379_2_En_10_Chapter.xhtml) or `cFieldType: "signature"` as in Chapter [8](#449379_2_En_8_Chapter.xhtml). You can affect the formatting such as `strokeColor`, `fillColor`, `lineWidth`, `borderStyle`, `textFont`, and `textColor`. You cannot, however, alter `textSize` as that is disabled and set to Auto, so make sure that the field is the size that you want to correctly see the signature. Refer to Chapter [10](#449379_2_En_10_Chapter.xhtml) for more details on those properties.

For more knowledge on the steps of creating a digital ID, refer to the following pages:

[`https://helpx.adobe.com/acrobat/using/digital-ids.html`](https://helpx.adobe.com/acrobat/using/digital-ids.html)

[`https://www.adobe.com/acrobat/how-to/fill-sign-pdf-forms-electronically.html`](https://www.adobe.com/acrobat/how-to/fill-sign-pdf-forms-electronically.html)

This page will also provide more information on the use of signatures in JavaScript:

[`https://opensource.adobe.com/dc-acrobat-sdk-docs/library/jsapiref/JS_API_AcroJS.html?highlight=signature`](https://opensource.adobe.com/dc-acrobat-sdk-docs/library/jsapiref/JS_API_AcroJS.html?highlight=signature)

Caution

In certain situations, after a PDF has been signed by the client or host using a digital ID or certificate, they can lock the entire PDF to prevent changes to the document after signing. While the signer who added the signature can remove it for you, you as the designer may not be able to and you will not be able to edit the form fields using the Prepare Form tool or edit other parts of the locked document. That is why when building forms always keep an unsigned copy separate as back up in case you need to edit the form for future revisions. For more details on this topic and solutions refer to this link:

[`https://helpx.adobe.com/acrobat/kb/edit-signed-PDF.html`](https://helpx.adobe.com/acrobat/kb/edit-signed-PDF.html).

### Create a Security Puzzle

You might find it interesting to set the fields to read-only with radio buttons and create a type of game or puzzle, where only if you select the correct radio buttons can a text field be unlocked from read-only, or in another variation an alert will sound if you duplicate a radio button in a row; or maybe the words on a button can change from locked to unlocked when the correct text is entered. I have put a few examples of this in a file that I call `PDF_Puzzles.pdf`. Refer to Figure [19-15](#449379_2_En_19_Chapter.xhtml#Fig15).

![](images/449379_2_En_19_Chapter/449379_2_En_19_Fig15_HTML.jpg)

9 circular dots indicate, unlock the number option and reset radio button is adjacent to it. Spaces are provided to change text on the buttons with correct number or word. The radio button clash alerts are highlighted with 3 different colors, with a reset radio button.

Figure 19-15

You can create puzzles using form fields and the read-only setting in your JavaScript code

In some examples, the action is added to the Validate tab and in others to the Actions tab.

The first radio button example has only this text applied to the text2 field on the Validate tab.

```
var a= this.getField("Text2");
if(event.value==6){
a.readonly=false;
}
else{
a.readonly=true;
}
```

Then you can add up the sum on the Calculate tab. The reset button just resets the fields. Refer to Figure [19-16](#449379_2_En_19_Chapter.xhtml#Fig16).

![](images/449379_2_En_19_Chapter/449379_2_En_19_Fig16_HTML.png)

2 dialog boxes of properties of the text fields. The variable tab is selected and run custom validation script is selected. The total value for the sum of Group 1, group 2, and Group 3 is calculated.

Figure 19-16

Settings in the text field properties on the Validate and Calculate tabs

Likewise, fields like Text5 and Text6 can also have a script applied to the Validate tab to interact with a specific button to lock or set the button to read-only or unlocked. Refer to Figure [19-17](#449379_2_En_19_Chapter.xhtml#Fig17).

![](images/449379_2_En_19_Chapter/449379_2_En_19_Fig17_HTML.jpg)

4 blank spaces are provided for writing the passwords and the buttons next to them are either to lock or unlock the passwords.

Figure 19-17

Use the text field to lock or unlock based on input

```
var b = this.getField("Button4");
if(event.value==2){
b.readonly=false;
b.textColor=color.green;
b.buttonSetCaption("UnLocked");
}
else{
b.readonly=true;
b.textColor=color.red;
b.buttonSetCaption("Locked");
}
```

As shown in the code in **bold**, simply by editing the first two lines in the second text box it accepts a word rather than a number as the input value.

```
var b = this.getField("Button5");
if(event.value=="password"){
//The rest of the code
```

You can see a repeat of the `if/else` statements, and this can again be applied to radio buttons for alerts. However, this can get progressively complicated to work with. Take time to look at this in the `PDF_Puzzles` file and observe how the code repeats itself but is slightly different for each radio button so that conditions are met and do not clash with each other.

Here is one example of a radio button action:

```
var g = this.getField("Group4");
g.readonly= true;
//conflict correction
var one= this.getField("Group5");
var one1= this.getField("Group6");
var sec = event.target;
var msg="First Color Chosen Already";
if (one.value=="Yellow1"){
app.alert(msg);
one.value="Yellow1";
one.readonly = true;
sec.value="";
sec.readonly = false;
}
//conflict correction 2
else if (one1.value=="Blue1"){
app.alert(msg);
one1.value="Blue1";
one1.readonly = true;
sec.value="";
sec.readonly = false;
}
```

You can look at the other eight radio buttons to see how the code changes on each Actions tab. Finally, you would default reset the radio buttons after the short game and you would also set the Reset Radio 2 button with the following code on the Actions tab:

```
var a = this.getField("Group4");
var b = this.getField("Group5");
var c = this.getField("Group6");
a.readonly=false;
b.readonly=false;
c.readonly=false;
```

I hope this will inspire you to come up with your own PDF security game.

## Barcodes

Finally, in regard to barcodes, this custom JavaScript influences how the barcode is visually displayed when it refers to the various fields in the form. When the barcode is scanned, this information is collected by a software program and entered into a database for later viewing. If you are planning on buying a barcode reader/scanner or fax to use with Acrobat PDF files, contact the company first and ask what types of JavaScript code its reader can decipher in regard to PDF barcodes. The company may be able to supply documentation or allow you to test the scanner before buying. Also, to refer to Adobe’s Barcoded Paper Forms solution, you may have to contact Adobe directly. Refer to Figure [19-18](#449379_2_En_19_Chapter.xhtml#Fig18).

![](images/449379_2_En_19_Chapter/449379_2_En_19_Fig18_HTML.png)

An icon of prepare form with an add option. A warning box by Adobe Acrobat. A window of barcode field properties with value tab in view. The codes in custom calculation script is written in the space provided.

Figure 19-18

Barcode field properties are found in the Prepare Form tool. When you choose the Barcode field, you may get the alert shown here. You can add or edit JavaScript on the Value tab in the custom calculation script field

JavaScript for barcodes is entered on the Actions or Value tabs. Unlike other form fields, the barcode fields do not possess an Appearance tab, and this does not allow for the use of `.type` formatting or duplication using a script. Changes to the type of barcode need to be made in the Options tab symbology; however, if you are considering adding a non-form-related QR code or barcode to a document or form, refer to Chapter [3](#449379_2_En_3_Chapter.xhtml) of this book. Refer to Figure [19-19](#449379_2_En_19_Chapter.xhtml#Fig19).

![](images/449379_2_En_19_Chapter/449379_2_En_19_Fig19_HTML.png)

A dialog box on barcode field properties with options tab in view. The symbology used is to insert Q R code. The decode condition is document or handheld scanner. There is a Q R code with label barcode 1 at the bottom.

Figure 19-19

Barcode Field Properties dialog box’s Options tab with the Symbology option set to QR Code

You can find more information on Acrobat PDF barcodes at [`https://helpx.adobe.com/acrobat/using/pdf-barcode-form-fields.html`](https://helpx.adobe.com/acrobat/using/pdf-barcode-form-fields.html)`.`

## Summary

In this chapter, you looked at basic security with text fields, check boxes, radio buttons, signature fields, and barcodes. Signature fields with JavaScript can be used for basic security; however, they are not the most secure PDF options unless combined with other security IDs and certifications. Using barcodes properly requires a barcode reader or scanner.

In the next chapter, we will look at E-signature forms and web forms and their related tools.

# 20. What Are E-Signature Forms and Web Forms?

**Chapter Goal:** In this chapter, we’ll explore some newer form tools in Acrobat Pro and how they relate to the JavaScript forms in previous chapters.

While this chapter is not specifically about Acrobat Forms using JavaScript, it is important to be aware of other new and optional tools within Acrobat as well as external links that can be used that connect to the application to create various electronic forms. We will look at two related examples and additional resources you can find online from Adobe Acrobat. Refer to Figure [20-1](#449379_2_En_20_Chapter.xhtml#Fig1) and Figure [20-2](#449379_2_En_20_Chapter.xhtml#Fig2).

![](images/449379_2_En_20_Chapter/449379_2_En_20_Fig2_HTML.png)

A screenshot of the window displays the Adobe Acrobat tools under Forms and Signatures. Request E-signatures, Fill and Sign, Prepare Forms, Certificates, Create a Webform, Send in Bulk, Add E-sign Branding, and Collect payments.

Figure 20-2

New form fools in the Tools Forms & Signature section

![](images/449379_2_En_20_Chapter/449379_2_En_20_Fig1_HTML.jpg)

A screenshot of the Adobe Acrobat toolbar with home and tools options. A search bar is below the tools option.

Figure 20-1

Acrobat Pro Tools tab

Note

To complete the assignments in this chapter, be sure to download the Chapter [20](#449379_2_En_20_Chapter.xhtml) folder at the link in the introduction.

You will also be able to locate some of the options discussed in this chapter using the E-Sign drop-down list in the main menu. The look of some of these settings may vary based on Acrobat Pro version you are using. You can refer to the following page for more details. [`https://helpx.adobe.com/acrobat/using/send-for-signature-new-experience-a.html`](https://helpx.adobe.com/acrobat/using/send-for-signature-new-experience-a.html). Refer to Figure [20-3](#449379_2_En_20_Chapter.xhtml#Fig3).

![](images/449379_2_En_20_Chapter/449379_2_En_20_Fig3_HTML.png)

A screenshot of the window presents a dropdown list under the E-sign tool. It lists Request E-signatures, Fill and Sign Yourself, Create a Webform, Send in Bulk, Add E-sign Branding, Collect payments, Prepare Agreement, and Manage All Agreements.

Figure 20-3

Main menu E-Sign drop-down options

## A Brief Look at Request E-Signatures and Resources

E-signatures are a type of digital or electronic signature that you can enable your client to sign online forms. There are situations where a client does not necessarily have to interact with other form fields and just needs to sign the digital signature parts of a documents. In these situations, where signatures outweigh the needs of other fields and you need to collect multiple signatures securely in a form via email, an e-signing form can be a useful tool.

In Chapter [19](#449379_2_En_19_Chapter.xhtml), we briefly looked at the importance of The digital signature field found in the Prepare Form tool. Refer to Figure [20-4](#449379_2_En_20_Chapter.xhtml#Fig4).

![](images/449379_2_En_20_Chapter/449379_2_En_20_Fig4_HTML.png)

A screenshot of the Field Name popup box. It displays an empty field where Signature 1 is to be added. A note reads Add a digital signature, add signature options from the properties view. Includes a checkbox for required field, and an all properties link.

Figure 20-4

Regular forms allow you to add a digital signature

To this type of form field, we can apply JavaScript in the Properties area on the Actions and Signed tabs. Refer to Figure [20-5](#449379_2_En_20_Chapter.xhtml#Fig5).

![](images/449379_2_En_20_Chapter/449379_2_En_20_Fig5_HTML.png)

A screenshot of the Digital Signature Properties dialog box. It displays the options under the signed tab. The option Nothing happens when signed is selected.

Figure 20-5

Digital Signature Properties dialog box with the Signed tab displayed

And as we discovered in previous chapters, other form fields, such as text fields, drop-down lists, radio buttons, and check boxes, can all have actions added in various tabs in that field’s properties dialog box.

However, as mentioned, there are some types of forms that rely more on the collection of digital signatures rather than calculations or just entering text. These are referred to as e-signatures or a request for e-signing.

With the Acrobat forms we used in the past, other tools are often used to complete the signature process, such as Fill & Sign and Certificates. Refer to Figure [20-6](#449379_2_En_20_Chapter.xhtml#Fig6).

![](images/449379_2_En_20_Chapter/449379_2_En_20_Fig6_HTML.jpg)

Two screenshots of the Fill and Sign tool icon with an open button below it, and the Certificates tool icon with an add button below it.

Figure 20-6

Form tools Fill & Sign and Certificates

You can learn more at the following pages:

[`https://helpx.adobe.com/acrobat/using/signing-pdfs.html`](https://helpx.adobe.com/acrobat/using/signing-pdfs.html)

[`https://helpx.adobe.com/acrobat/kb/certificate-signatures.html`](https://helpx.adobe.com/acrobat/kb/certificate-signatures.html)

[`https://helpx.adobe.com/acrobat/using/securing-pdfs-certificates.html`](https://helpx.adobe.com/acrobat/using/securing-pdfs-certificates.html)

However, as the process of form building has become more focused on multiple clients or participant signatures, the Prepare Form tool has added specific types of form fields to build this type of form separate from the traditional Acrobat form that we have been discussing in the book. Refer to Figure [20-7](#449379_2_En_20_Chapter.xhtml#Fig7).

![](images/449379_2_En_20_Chapter/449379_2_En_20_Fig7_HTML.jpg)

A screenshot of the Prepare Form tool icon with an add button below it.

Figure 20-7

Prepare Form tool

After selecting the Prepare Form tool, you enable this type of form and its options when you have a PDF file selected and then check the option “This document requires signatures” before clicking Start. And as in previous examples, make sure that form field auto detection is ON. Refer to Figure [20-8](#449379_2_En_20_Chapter.xhtml#Fig8).

![](images/449379_2_En_20_Chapter/449379_2_En_20_Fig8_HTML.jpg)

A screenshot of a window displays a document to be scanned to begin a file under the Single File option. The option, This document requires signatures, and the start button are selected.

Figure 20-8

Prepare Form tool settings for a single selected file

Note that if some fields are already present in the file, you may be prompted to save the document as a copy first before you continue adding the fields.

However, if you start with a single file and no fields, choose Create New and start with a blank document, or create a new form, you will not be asked to save the file first. You will, in that case, save the file only after you have completed adjusting the form fields. Refer to Figure [20-9](#449379_2_En_20_Chapter.xhtml#Fig9).

![](images/449379_2_En_20_Chapter/449379_2_En_20_Fig9_HTML.jpg)

A screenshot of a window displays a blank space where the document is to be created under the Create New option. The option, This document requires signatures, and the start button are selected.

Figure 20-9

Prepare Form tool settings for a new blank file

Note

The Scanner option is for when you are working directly with a scanned document that is currently on the scanner bed. We will look at the Create a Web Form option in the “What Are Web Forms” section of this chapter. Refer to Figure [20-9](#449379_2_En_20_Chapter.xhtml#Fig9).

In this case, I will use a time sheet that I created in Word and then converted to a PDF and added form fields to. I have supplied the original Word and PDF forms as well, if you want to compare a regular PDF form to one that requires signature. You can open these files named `Timesheet_End.pdf` and `Timesheet_esign_End.pdf` and rename them as required for practice purposes. Open the new files and click the Prepare Form tool to view the fields in each example one at a time. However, for now the focus is on the E-Sign form. Refer to Figure [20-10](#449379_2_En_20_Chapter.xhtml#Fig10).

![](images/449379_2_En_20_Chapter/449379_2_En_20_Fig10_HTML.jpg)

A screenshot of a window presents the Prepare Form tool icon with an Add button below. It displays a sheet titled Tourmaline Mining Corporation Weekly Project Time Sheet. It lists the employee information with the comments, Admin signature, and date fields at the bottom.

Figure 20-10

Prepare Form tool and a PDF time sheet that uses a signature and other fields

The properties for these form fields will be limited solely to signature allocation. You’re not able, in the properties, to add custom JavaScript or any form of action if you build this form from scratch requiring signatures. You can open this dialog box when you right-click a field and choose properties form the the popup list. Refer to Figure [20-11](#449379_2_En_20_Chapter.xhtml#Fig11).

![](images/449379_2_En_20_Chapter/449379_2_En_20_Fig11_HTML.png)

A screenshot of the Text Field Properties dialog box presents the options under the General tab. It displays the name and tooltip as employee's name, field type, participant role, signer's email, and common properties with form field and orientation options. Close button is t the bottom.

Figure 20-11

Text Field Properties dialog box’s General tab

In the Text Field Properties dialog box, you are limited to the General, Appearance, Position, and Options tabs.

There are no settings here for creating a calculation, adding a validation, or adding a custom action to a text field. The properties in the Appearance, Position, and Options tabs are the same as what is found when you create regular Acrobat forms.

However, I will just point out a few key differences on the General tab.

The Field Type list in the figure is set to Text Field from the drop-down list. You can choose some other Text Field type options, which include Text, Signature, Signature Block, Initial, Title, Company, Name, Email, and Date. Refer to Figure [20-12](#449379_2_En_20_Chapter.xhtml#Fig12).

![](images/449379_2_En_20_Chapter/449379_2_En_20_Fig12_HTML.jpg)

A screenshot presents the dropdown list under the field type, where the text option is selected. Other options are, signature, signature block, initial, title, company, name, email, and date.

Figure 20-12

General tab’s Field Type options in the drop-down list

Rather than select one from the list, you can find these same field options icons in the Prepare forms subtools area along with additional icons for drop-down lists, check boxes, and radio buttons, which we will discuss further shortly. Refer to Figure [20-13](#449379_2_En_20_Chapter.xhtml#Fig13).

![](images/449379_2_En_20_Chapter/449379_2_En_20_Fig13_HTML.jpg)

A screenshot presents 17 field options icons under the Prepare forms sub tools.

Figure 20-13

Options for e-signature forms

A related signature field can fall into any of the field type categories, as shown in Figure [20-12](#449379_2_En_20_Chapter.xhtml#Fig12).

However, when you change the field type from Text to another option in the Field Type drop-down list, this will add additional code to the name tag as displayed in field edit view. An example is Employees Name_es_:signature. Refer to Figure [20-14](#449379_2_En_20_Chapter.xhtml#Fig14).

![](images/449379_2_En_20_Chapter/449379_2_En_20_Fig14_HTML.png)

A screenshot of a window presents how the field types are changed from text to other options. Text is changed to the Employees name and the Signature is changed to the Employees name e-signature.

Figure 20-14

Based on the field type, the name of the field changes in edit view

Doing so does not, however, change the name of the field on the General tab, as in Figure [20-11](#449379_2_En_20_Chapter.xhtml#Fig11).

On the General tab, you work with the signer’s Participant Role options such as Anyone, Sender, Signer, Signer 1 through 6, or Prefill to set who needs to sign this field and set the signer’s email (which is optional at this point). Refer to Figure [20-15](#449379_2_En_20_Chapter.xhtml#Fig15).

![](images/449379_2_En_20_Chapter/449379_2_En_20_Fig15_HTML.jpg)

A screenshot presents the dropdown list under the participant role option which is under the General tab. The text anyone is selected. Other options in the list are, Sender, Signer, Signer 1 through 6, and Prefill.

Figure 20-15

General tab for Text Field setting called Participant Role

Other fields that you can set are check boxes, radio buttons, and drop-down lists. On the General tab the field type for these will be grayed out and disabled so that you cannot edit it. Additionally, for radio buttons, you will not be able to change the Participant Role and Signer’s Email settings. Refer to Figure [20-16](#449379_2_En_20_Chapter.xhtml#Fig16).

![](images/449379_2_En_20_Chapter/449379_2_En_20_Fig16_HTML.jpg)

Two screenshots present the options under the General tab with checkboxes, radio buttons, and drop-down list icons on top. It displays the name and tooltip as dropdown 1 in the first, and group 1 and radio 1 respectively in the second, disabled field types, participant role, and signer's email.

Figure 20-16

Check boxes, radio buttons, and drop-down lists have limits on which fields in the General tab can be set

No list box or button, image, or barcode fields are available. In the Prepare Tools area, however, you still have the options to select a field, which you can double-click to edit. You can also edit text and images, as well as add a text box. Refer to Figure [20-17](#449379_2_En_20_Chapter.xhtml#Fig17).

![](images/449379_2_En_20_Chapter/449379_2_En_20_Fig17_HTML.jpg)

A screenshot of the select, edit text and images, and add text box icons.

Figure 20-17

These Prepare Form tool options allow you to modify text in your forms

When you click the page to add a new form field, these same options will appear before entering the properties panel and setting them on the General tab. Refer to Figure [20-18](#449379_2_En_20_Chapter.xhtml#Fig18).

![](images/449379_2_En_20_Chapter/449379_2_En_20_Fig18_HTML.jpg)

A screenshot of a popup box. It displays text fields where the options field name, who need to sign this field, and the signer's email are to be added. A note appears under each option when it is selected. A placeholder is at the bottom left to add a signature block.

Figure 20-18

Options appear when you add a new form field

Once you preview your fields, close the Prepare Forms tool. Then once the file is saved, you can request e-signatures and send the form for a signature. Refer to Figure [20-19](#449379_2_En_20_Chapter.xhtml#Fig19).

![](images/449379_2_En_20_Chapter/449379_2_En_20_Fig19_HTML.jpg)

A screenshot of a preview window displays the options under, request E-signatures, with an open button. It reads, add recipients to e-sign this document, with a text field to add name or email. The button, Specify where to sign is selected.

Figure 20-19

Once a form is complete, you can then start the request for the e-signatures process

This newer tool is found on the Tools tab in the Forms and Signatures area. However, you can also access it while working on the form in the Prepare Form tool in the bottom pane on the right. Refer to Figure [20-20](#449379_2_En_20_Chapter.xhtml#Fig20) and Figure [20-21](#449379_2_En_20_Chapter.xhtml#Fig21).

![](images/449379_2_En_20_Chapter/449379_2_En_20_Fig20_HTML.png)

A screenshot of a list presents the editing options under the following icons. Align, center, match size, and distribute with more tool options. It displays a list under fields option with the Request E-signatures button selected.

Figure 20-20

A request for e-signatures can also be started while in form editing mode

Initially, you will be taken to the Fill & Sign tool. However, once you add a recipient and then click the More Options button, you will be taken to the Request E-Signatures tool. Refer to Figure [20-21](#449379_2_En_20_Chapter.xhtml#Fig21).

![](images/449379_2_En_20_Chapter/449379_2_En_20_Fig21_HTML.png)

A screenshot of a window displays the options under Fill and Sign with the Specify where to sign button selected. It displays a get document signed page with recipients, message, and file fields, in the first stage of adding signers, under request E-signatures.

Figure 20-21

Starting the request for e-signatures on a test file

Depending on the button you clicked, the layout and order of these two options are slightly different, so here are some links to further resources that allow you to continue this process should the appearance in your version of Acrobat Pro be slightly different than mine:

[`https://helpx.adobe.com/acrobat/using/creating-distributing-pdf-forms.html`](https://helpx.adobe.com/acrobat/using/creating-distributing-pdf-forms.html)

[`https://helpx.adobe.com/acrobat/using/send-for-signature.html`](https://helpx.adobe.com/acrobat/using/send-for-signature.html)

Once you click Next (Figure [20-21](#449379_2_En_20_Chapter.xhtml#Fig21)), you will proceed to the next step of specifying where to fill and sign. This same step can be accessed at the button found earlier with the same name “Specify where to sign.” You would then continue to work on the form adjusting the settings for recipients and adding and editing fields as required. Fields can be right-clicked, and you can choose the Edit option from a pop-up menu. Then click the Send button in the right pane to send and track the progress and receive a confirmation notice. This creates a type of web form agreement that you can cancel and delete online on your Adobe Acrobat home page if required or continue to edit online. They can also be saved as a template for other projects. We’ll look at some of those steps regarding editing fields in more detail later in the chapter as we do them online with the web form. What you learn in the next section you can apply to the Request E-Signatures tool as well. Refer to Figure [20-22](#449379_2_En_20_Chapter.xhtml#Fig22).

![](images/449379_2_En_20_Chapter/449379_2_En_20_Fig22_HTML.jpg)

A screenshot displays the second step of signing a document. It presents an employee information sheet with admin signature option, and lists a dropdown under the signature fields that have signature, initials, signature block, and stamp options. A selected send button is at the bottom.

Figure 20-22

Starting the request for e-signatures on a test file and specifying where to fill and sign and review the fields

You can click the Close button in the upper right of this tool if you need to edit it without saving changes.

As mentioned earlier, tools like Fill & Sign and Certificates are generally used with regular Acrobat forms to complete the signature process. However, there are also some new related tools such as Create a Web Form, which we will briefly look at next, as well as other online options such as Send in Bulk, Add E-Sign Branding, and Collect Payments. Refer to Figure [20-23](#449379_2_En_20_Chapter.xhtml#Fig23).

![](images/449379_2_En_20_Chapter/449379_2_En_20_Fig23_HTML.jpg)

A screenshot of the window displays the Adobe Acrobat tools under Forms and Signatures. Request E-signatures, Fill and Sign, Prepare Form, Certificates, Create a Webform, Send in Bulk, Add E-sign Branding, and Collect payments.

Figure 20-23

Review of the tools in the Forms & Signatures section

While working on your form, at any point in time if you discover that this was not the type of form you wanted to create in the Prepare Form tool, use the More drop-down menu on the right to revert to creating an Acrobat form as you did in past chapters. Refer to Figure [20-24](#449379_2_En_20_Chapter.xhtml#Fig24).

![](images/449379_2_En_20_Chapter/449379_2_En_20_Fig24_HTML.jpg)

A screenshot of the Revert to Acrobat form option, under the more tools icon.

Figure 20-24

While using the Prepare Form tool, use the More menu to revert to an Acrobat form

Currently the form is prepared for e-signing. Refer to Figure [20-25](#449379_2_En_20_Chapter.xhtml#Fig25).

![](images/449379_2_En_20_Chapter/449379_2_En_20_Fig25_HTML.jpg)

A screenshot of the Prepare form for E-signing option, under the more tools icon.

Figure 20-25

Use the More Menu to prepare the form for e-signing

However, just be aware that reverting back and forth between an Acrobat form and an e-sign form may remove or hide the ability for you to access custom JavaScript you created earlier. As in this example, you cannot access the Calculation tab, although the calculations in the Total Hours column and the Grand Total hours still function. A button that was in the original Acrobat form was removed, and two warning messages appear before this setting can commit. In the first alert, click Next, and then click OK for the second alert. Refer to Figure [20-26](#449379_2_En_20_Chapter.xhtml#Fig26).

![](images/449379_2_En_20_Chapter/449379_2_En_20_Fig26_HTML.jpg)

Two screenshots of the Adobe Acrobat popup boxes. Both read the warning messages related to converting the file into Adobe Acrobat sign form and removing the unsupported form field types. The buttons Next and O k are highlighted.

Figure 20-26

Warning messages that may appear while converting the form for e-signing

If you do not want to convert to an Acrobat Sign form, click Cancel before the next message. Or you can close the file and not save the changes.

These forms, however, can be later modified as web forms, as we will see in a moment with the next tool.

## What Are Web Forms?

The other tool that involves forms directly is the new Create a Web Form tool. Web forms are like the Request for E-Signature Agreement form we created earlier. They require signatures, and these forms can be embedded on your website or sent as a link in an email so that multiple people can access the form or document. Rather than use Acrobat Pro to build the whole form, we can continue to construct it online on your Acrobat home page. We will now look at a brief overview of the web form creation process. This allows you to add forms to your website and share links to collect fata online. This area can also be accessed through the Prepare Form tool when you choose the Create a Web Form option with no particular form selected. Refer to Figure [20-27](#449379_2_En_20_Chapter.xhtml#Fig27).

![](images/449379_2_En_20_Chapter/449379_2_En_20_Fig27_HTML.jpg)

A screenshot of a window displays the Create a Web Form and Prepare Form icons. It presents the options to create a Web form by adding forms to the user's website and sharing links to collect data online. The Open in Web app button is selected.

Figure 20-27

You can create a web form from scratch using this option in the Prepare Form tool or from the tool itself

If you are logged in to your account, when you click this tool or the button Open in Web App, it takes you to `acrobat.adobe.com`, where you can upload one of your existing forms that you created. Refer to Figure [20-28](#449379_2_En_20_Chapter.xhtml#Fig28).

![](images/449379_2_En_20_Chapter/449379_2_En_20_Fig28_HTML.png)

A page is titled Create a web form and presents the first step of adding files into the web form. It reads the following. Name of the form, Add files, Required participants to complete the form, and Add password.

Figure 20-28

Create a Web Form and related settings

First you can name your form. Refer to Figure [20-29](#449379_2_En_20_Chapter.xhtml#Fig29).

![](images/449379_2_En_20_Chapter/449379_2_En_20_Fig29_HTML.jpg)

A screenshot of the blank field space for adding the name of the form. The text in lighter shade in the field is, time sheet.

Figure 20-29

Start creating a web form by naming it

Then you select a file or files you want to add from your computer. Refer to Figure [20-30](#449379_2_En_20_Chapter.xhtml#Fig30).

![](images/449379_2_En_20_Chapter/449379_2_En_20_Fig30_HTML.jpg)

A screenshot of the blank space for adding files to the form. It has a drag-and-drop or selects files option.

Figure 20-30

Select the files on your computer you want as part of the web form

You can use the files I have supplied in this chapter if you want to practice. First click the “select files” link and then the My Computer tab. Then click “Add a file,” such as a renamed version of my file or your own, from your device to locate a PDF file on your desktop. Click Open and it will be added to the list. Refer to Figure [20-31](#449379_2_En_20_Chapter.xhtml#Fig31).

![](images/449379_2_En_20_Chapter/449379_2_En_20_Fig31_HTML.jpg)

A screenshot of a blank space for selecting files for the form. It has a drag-and-drop files option under my computer tab. It displays a field for custom files with an open button highlighted, and add files option with a selected file.

Figure 20-31

Locate a file(s) and add to the list

The kinds of files that you can add can be PDFs that are only for e-signing with e-signature, but you can also use the regular Acrobat Forms PDF that you have been creating throughout this book as well.

Keep in mind, however, that not all the form fields will operate as they would in the PDF format within Acrobat Pro, so you may need to alter or remove some fields as mentioned earlier in the chapter. An example of this would be the digital signature, which might be turned into an e-sign signature once it is uploaded. I will show an example of this in a moment.

Other types of fields that are removed would be a button, a list box, and an image field. The Barcode field comes in partially as a blank square. So, before you upload a regular Acrobat form, rather than an e-sign form, consider whether those fields are necessary to complete, whether they should be removed, or whether they should be replaced with another field. For example, a List Box field could probably be replaced by a drop-down list. And you would manually make those edits first to a copy of your form. Refer to Figure [20-32](#449379_2_En_20_Chapter.xhtml#Fig32).

![](images/449379_2_En_20_Chapter/449379_2_En_20_Fig32_HTML.jpg)

A screenshot of the O K, barcode, list, and drop-down list icons.

Figure 20-32

The following type of fields, button, barcode, and list box are not available for e-forms except for drop-down lists

Later we will see that an image field, when online, can be replaced if it disappears. Refer to Figure [20-33](#449379_2_En_20_Chapter.xhtml#Fig33).

![](images/449379_2_En_20_Chapter/449379_2_En_20_Fig33_HTML.jpg)

A screenshot of the image field icon.

Figure 20-33

The image field can be added later when you build online but will disappear when first uploaded

Likewise, you can also start with other formats such as Microsoft Word (`.doc` and `.docx`) as well as Excel (`.xls`, `.xlsx`), PowerPoint (`.ppt`, `.pptx`), and other graphic formats (`.html`, `.bmp`, `.jpg`, `.tiff`). Refer to Figure [20-34](#449379_2_En_20_Chapter.xhtml#Fig34).

![](images/449379_2_En_20_Chapter/449379_2_En_20_Fig34_HTML.jpg)

A screenshot of a program code. It reads 2 rows of file formats such as doc, p p t, j p g, h t m l, and tiff, of custom files to create a web form. The entire code is selected.

Figure 20-34

Various file formats can be combined to create a web form

However, keep in mind that other than PDF, the formats will not maintain some of the form fields preset before you enter the online file, and you will need to autodetect them. Autodetect, while helpful, might not in all cases give you the form fields that you need, and you may need to delete or add the ones you require afterward. This is why working initially in Acrobat to build the form may be a better solution if you need to return to your form on your computer desktop for some specific edits. We’ll look at that section in a moment. Refer to Figure [20-35](#449379_2_En_20_Chapter.xhtml#Fig35).

![](images/449379_2_En_20_Chapter/449379_2_En_20_Fig35_HTML.jpg)

A screenshot of the autodetect tool. It reads the following. Form fields were detected in this document. Click this button to place them.

Figure 20-35

Once files are added and uploaded, Acrobat can detect possible field areas

Coming back to the Add step, after the file or files are added in the first step, then you can set your participate action to complete the form and authentication via a password.

*   *Sign*: Signs the form and may also enter data.

*   *Approve*: Reviews and approves the form; may also enter data or sign.

*   *Fill form*: Enters required data and submits the form, but typically doesn’t sign.

*   *CC Copied*: Receives notifications as form completion progresses. This does not appear in the initial list.

Add the number of participants, up to 36, that you will list in order to complete the form with their actions. Add their email addresses for routing, except for the first participant; in this case you will enter their email address during the form completion stage. You can look at the various info buttons if you need more clarification on certain settings as well as password authentication for participants. Refer to Figure [20-36](#449379_2_En_20_Chapter.xhtml#Fig36).

![](images/449379_2_En_20_Chapter/449379_2_En_20_Fig36_HTML.jpg)

A screenshot of the first step of adding additional participants to the web form. It lists a dropdown under actions and displays a popup box with the add participant authentication information.

Figure 20-36

Add additional participants to your web form and decide on their actions as well as their authentication

In addition, you can add an access password if required, but it is unnecessary to view the completed form. Click Next to proceed to the next step. Refer to Figure [20-37](#449379_2_En_20_Chapter.xhtml#Fig37).

![](images/449379_2_En_20_Chapter/449379_2_En_20_Fig37_HTML.jpg)

A screenshot of the window for adding a password to a file. It reads Add an access password option with the next button highlighted.

Figure 20-37

Add a password for certain forms (optional) and click Next to proceed

You will then prepare the document, which is the step where you can add or edit form fields on your online document. Refer to Figure [20-38](#449379_2_En_20_Chapter.xhtml#Fig38).

![](images/449379_2_En_20_Chapter/449379_2_En_20_Fig38_HTML.jpg)

A screenshot of the second step of preparing the web form. It presents a sheet titled Tourmaline Mining Corporation and lists the form field icons for employee information, with a dropdown under the Signature fields.

Figure 20-38

View and prepare your web form

Here you can edit each field. The recipients’ signatures, as shown on the right, indicate who can later sign the document as well as interact with various form fields. The color, assigned to the field in the triangle in the upper right, will indicate which recipient it affects. Refer to Figure [20-39](#449379_2_En_20_Chapter.xhtml#Fig39).

![](images/449379_2_En_20_Chapter/449379_2_En_20_Fig39_HTML.jpg)

A screenshot of the Recipients popup box. It lists a dropdown where the option Web Form Signer is selected.

Figure 20-39

Preview who the current recipients are

In this web form, other property options will become available, similar to what you saw earlier with the Request E-signatures tool while working in Acrobat. You can use text fields so that you can create value calculations. You can review these types of fields in the pane on the right. Refer to Figure [20-40](#449379_2_En_20_Chapter.xhtml#Fig40).

![](images/449379_2_En_20_Chapter/449379_2_En_20_Fig40_HTML.png)

A screenshot of the Recipients popup box displays the options under the Web Form Signer. It lists a dropdown under Signature fields and reads signature, initials, signature block, and stamp.

Figure 20-40

Reviewing various field options on the right pane

So, does this kind of form allow you to use JavaScript?

While it does not appear at first when you upload or add a form that you can program any custom JavaScript or code, once online and in the Prepare Form stage, individual fields can be right-clicked and edited by selecting that option from the pop-up menu, as you would when using the Request for E-Signature tool earlier. You can then set various validation settings as well as conditions that the participant may be required to fill in to complete the form. Refer to Figure [20-41](#449379_2_En_20_Chapter.xhtml#Fig41).

![](images/449379_2_En_20_Chapter/449379_2_En_20_Fig41_HTML.png)

A screenshot of the editing options under the Total Hours field. It reads the following for row 1\. Assigned to, field type, value type, default value, tooltip, validation, conditions, appearance, and tools with the O K button highlighted.

Figure 20-41

Right-clicking a field and choosing Edit will allow you to review the field’s properties

When you choose Edit from the pop-up menu, other options include Copy, Clone, and Delete.

After selecting the Edit option, you can set properties. These might include setting validations or various types of conditions that would cause a field to show, hide, enable, or disable. You can also adjust the appearance of the field on the Tools tab to copy and clone or delete a field option. Refer to Figure [20-42](#449379_2_En_20_Chapter.xhtml#Fig42).

![](images/449379_2_En_20_Chapter/449379_2_En_20_Fig42_HTML.png)

A screenshot of the settings under validations with a dropdown list where none is highlighted. Includes settings for conditions and appearance.

Figure 20-42

Various properties available for text fields

However, during the upload of the Acrobat form in the calculation example, settings may be lost, and you will have to set new calculation conditions for the form to work correctly. I have supplied some links at the end of this section that discuss this code in more detail. Figure [20-43](#449379_2_En_20_Chapter.xhtml#Fig43) shows a sample of how that could look for text fields that you want to add together.

![](images/449379_2_En_20_Chapter/449379_2_En_20_Fig43_HTML.png)

Two screenshots of the field settings under Sat Hours Row 1 and Total Hours Row 1\. It reads the following settings options. Assigned to, field type, value type, default value, tooltip, validation, value range, formula, display as, and number format.

Figure 20-43

Changing the value type from entered value to calculated value and entering a new formula

When I click the formula *f(x)* icon, I can enter a dialog box and begin to create a formula. This area is also available for the validation custom-formula as well. I find this area to be much more detailed than I find in the regular area of Acrobat Pro, and in some ways it appears similar to MS Excel. You can access this area too when using the Request E-Signatures tool. Once you enter the selected fields and functions to build your formula, you can then click the Check Syntax button, which acts as a debugger for the formula, or you can reset the formula to clear it. Refer to Figure [20-44](#449379_2_En_20_Chapter.xhtml#Fig44).

![](images/449379_2_En_20_Chapter/449379_2_En_20_Fig44_HTML.jpg)

Three screenshots of the steps to build a formula in the sheet by selecting the required fields of rows, and functions of date, logic, and math. It has a blank space where the formula with functions is to be entered with a check syntax button highlighted.

Figure 20-44

Adding a formula using the dialog box

Clicking the plus key beside a field or a function adds that item to your formula. There are also additional operators that you can click to add, subtract, divide, and multiply fields as well as other conditions of greater or less than. But you can type in your own text as well.

Once the syntax is checked upon clicking the button and proves to be valid, you can then click OK to exit the dialog box or click Cancel and not save any changes. Refer to Figure [20-45](#449379_2_En_20_Chapter.xhtml#Fig45).

![](images/449379_2_En_20_Chapter/449379_2_En_20_Fig45_HTML.jpg)

A screenshot of how the syntax of the formula is checked by using the check syntax button. It reads the message, The expression is valid if the entered formula is correct, and the O K button is highlighted.

Figure 20-45

Checking the syntax of the formula that it is valid and clicking OK

Note

The lower text area in the dialog box gives some helpful descriptions of some of the functions for date, logic, and math. Refer to Figure [20-46](#449379_2_En_20_Chapter.xhtml#Fig46).

![](images/449379_2_En_20_Chapter/449379_2_En_20_Fig46_HTML.jpg)

A screenshot of the information about the absolute number function. It reads a b s of number, it returns the absolute value of a number. a b s of negative 10 and a b s of 10 will both return 10\. a b s of field 1 will return the a b s value of the value of field 1.

Figure 20-46

The dialog box helps to explain certain formula functions (fx) and their uses

Conditions can also be set for radio buttons, check boxes, and drop-down lists but do not appear to have any properties that allow for special validation formats. However, radio buttons and check boxes can be part of calculated value type. Refer to Figure [20-47](#449379_2_En_20_Chapter.xhtml#Fig47).

![](images/449379_2_En_20_Chapter/449379_2_En_20_Fig47_HTML.png)

Three screenshots of the popup boxes. Each presents the field settings options such as assigned to, value type, default value, tooltip, dropdown options, export values, conditions, appearance, and tools, under Check Box 1, Group 1, and Dropdown 1 with the O K button highlighted.

Figure 20-47

Property options for a check box, radio button group, and drop-down list

Click OK to commit the changes, or click Cancel to exit when you have entered a specific field’s properties.

As mentioned earlier, if your image field is lost during upload, your form can also have an image box added again using the Data Field options on the right side. However, there are no options for list boxes or buttons. This is where you can access more fields such as Text Input, Drop Down, Check Box, and Radio Button. Refer to Figure [20-48](#449379_2_En_20_Chapter.xhtml#Fig48).

![](images/449379_2_En_20_Chapter/449379_2_En_20_Fig48_HTML.jpg)

A screenshot of the dropdown under the data fields option. It reads text input, drop-down, check box, radio button, and image. A preview of the image is displayed with the image height and width of 1.2 inches.

Figure 20-48

Data Field options and the new Image field previewed on the page and other data fields

The following are other fields that you can add afterward and drag into the form.

*   Signature Fields: For signing with Signature, Initials, Signature Block (which includes signature and email), and Signature Stamp. Refer to Figure [20-49](#449379_2_En_20_Chapter.xhtml#Fig49).

    ![](images/449379_2_En_20_Chapter/449379_2_En_20_Fig49_HTML.jpg)

    A screenshot of the dropdown under the Signature fields option. It reads signature, initials, signature block, and stamp. A preview of each signature field option is displayed with their parameters.

    Figure 20-49

    Signature Field options and how they preview on a page

Signer Info Fields: This is an area for the signer to add additional info, which includes Job Title, Company Name, Full Name, Email, and Date settings. Name is the signer, which may already be known, so this may already be set or read-only. In the case of the Date field, this acts as a type of time stamp, for example today’s date. It is not the same as a data field text input field, with validation of date added. Refer to Figure [20-50](#449379_2_En_20_Chapter.xhtml#Fig50).

![](images/449379_2_En_20_Chapter/449379_2_En_20_Fig50_HTML.jpg)

A screenshot of the dropdown under the Signer Info fields option. It reads title, company, name, email, and date. A preview of each signer field option is displayed.

Figure 20-50

Signer Info Fields options and how they preview on a page

Note

When creating an e-form in Acrobat Pro that you plan to upload to publish online, you need to make sure the participant roles are set in the Properties General field correctly. Otherwise, you may notice when you upload the form that some of the fields are dropped. I found this to be the case when I changed the field from Signer to an option like Sender or Prefill. These fields do not appear in your Recipients area in your right pane and are not options for color coordinated fields with a triangle as seen in Figure [20-50](#449379_2_En_20_Chapter.xhtml#Fig50). The option Anyone works correctly. Refer to Figures [20-15](#449379_2_En_20_Chapter.xhtml#Fig15) and [20-39](#449379_2_En_20_Chapter.xhtml#Fig39).

Data Fields: As we have seen so far, data fields include Text Input, Drop Down, Check Box, Radio Button, and Image.

More fields: These are used for File Attachment and Hyperlink (a URL or page within the document). Refer to Figure [20-51](#449379_2_En_20_Chapter.xhtml#Fig51).

![](images/449379_2_En_20_Chapter/449379_2_En_20_Fig51_HTML.jpg)

A screenshot of the dropdown under More fields option, with file attachment and hyperlink. A preview of file attachment option is displayed below along with paperclip icon. The hyperlink option is a popup box with the website, display label, and appearance settings.

Figure 20-51

More Field options and how they preview on a page

Transaction Fields: These include Participation Stamp and Acrobat Sign Transaction Number. Refer to Figure [20-52](#449379_2_En_20_Chapter.xhtml#Fig52).

![](images/449379_2_En_20_Chapter/449379_2_En_20_Fig52_HTML.jpg)

A screenshot of the dropdown under transaction fields option. It reads the participation stamp and transaction number. A preview of the participation stamp with name, signing or approval time, and email settings is displayed, along with the Acrobat sign transaction number.

Figure 20-52

Transaction Fields options and how they preview on a page

After dragging various form fields onto the form, you can then adjust them by either moving or scaling the sides when the cursor turns to arrows. Refer to Figure [20-53](#449379_2_En_20_Chapter.xhtml#Fig53).

![](images/449379_2_En_20_Chapter/449379_2_En_20_Fig53_HTML.png)

A screenshot of how the fields can be altered in their sizes by dragging the cursor. It has a rectangular field box with orientation arrows. A double-headed arrow of different directions and a four-headed arrow are displayed.

Figure 20-53

Fields can be moved or have the size adjusted when the cursor changes to arrows

Multiple fields can also be Shift+clicked, and you can use the Align options in the upper left to align or distribute fields relative to each other or the page as you would in the Acrobat Pro application. Refer to Figure [20-54](#449379_2_En_20_Chapter.xhtml#Fig54).

![](images/449379_2_En_20_Chapter/449379_2_En_20_Fig54_HTML.jpg)

A screenshot of how multiple fields can be selected. It has two rectangular fields which are selected and a list of 9 icons below it for alignment settings of the selected fields, or relative to page. A dropdown with sat hours row 2 is next to it.

Figure 20-54

Multiple selected fields can be aligned using the icons

You can also navigate to a specific form field using the upper-right drop-down list to help you select the field. Refer to Figure [20-55](#449379_2_En_20_Chapter.xhtml#Fig55).

![](images/449379_2_En_20_Chapter/449379_2_En_20_Fig55_HTML.jpg)

A screenshot of a dropdown under the field Sat Hours Row 2\. It reads the date, date underscore 2, dropdown 1, dropdown 2, dropdown 3, dropdown 4, and employees code, where date underscore 2 is selected.

Figure 20-55

Locate a selected field from the drop-down list

At the bottom of this pane on the right you can also reset the fields to the last saved version. Refer to Figure [20-56](#449379_2_En_20_Chapter.xhtml#Fig56).

![](images/449379_2_En_20_Chapter/449379_2_En_20_Fig56_HTML.jpg)

A screenshot of the reset fields popup box. It reads the warning information that resetting the document would make the user loose all the current edits to the document, and if the user wants to reset that document, with the confirm button highlighted.

Figure 20-56

Reset fields if you have made a mistake

In this lower area is also the navigation for the current page. Refer to Figure [20-57](#449379_2_En_20_Chapter.xhtml#Fig57).

![](images/449379_2_En_20_Chapter/449379_2_En_20_Fig57_HTML.jpg)

A screenshot of a dark row displays the navigation icons. It has an upward arrow, down arrow, page numbers, zoom out, and zoom in options.

Figure 20-57

Navigate to other pages in the form or zoom in or out in the form to look at specific fields

If there are any errors when the form is imported, such as not enough values in the drop-down list, you will get a red warning message either across the top or in the Properties dialog box. You must correct this before proceeding with publishing. Figure [20-58](#449379_2_En_20_Chapter.xhtml#Fig58) shows an example of how it might appear in a drop-down list, where drop-down as well as export values are missing and then with that error corrected. Refer to Figure [20-58](#449379_2_En_20_Chapter.xhtml#Fig58).

![](images/449379_2_En_20_Chapter/449379_2_En_20_Fig58_HTML.jpg)

Two screenshots of the Drop Down 1 window. Window 1 displays a blank space for the dropdown options and export value with an error message that there must be at least 2 options and it cannot be blank or contain only spaces. Window 2 displays the dropdown and export value with 4 options.

Figure 20-58

Correcting errors in drop-down list properties for the Options and Export values

A Required field will appear with a star within the field. Refer to Figure [20-59](#449379_2_En_20_Chapter.xhtml#Fig59).

![](images/449379_2_En_20_Chapter/449379_2_En_20_Fig59_HTML.jpg)

A screenshot of the signature field settings option with an asterisk to its top left. It displays a signature 1 popup box with entries for the following. Assigned to, field type, conditions, tools, and delete field options, with the O k button selected.

Figure 20-59

Setting a required check for signature fields

After you prepare the form and add fields, you can publish it by clicking the Create button to proceed to the final step. Refer to Figure [20-60](#449379_2_En_20_Chapter.xhtml#Fig60).

![](images/449379_2_En_20_Chapter/449379_2_En_20_Fig60_HTML.jpg)

A screenshot of three steps to prepare a form. Add, prepare, and publish. There are close and create buttons on top of the Prepare step, where the create button is selected.

Figure 20-60

Click the Create button and publish your form

The form is now ready to publish for use on your website once you have access to the URL for the web form. In addition, you can then copy the iframe HTML or JavaScript code and embed it on your web page. Links can be shared as a URL with others, like by sending an email, or the code can be embedded somewhere on your website. Refer to Figure [20-61](#449379_2_En_20_Chapter.xhtml#Fig61).

![](images/449379_2_En_20_Chapter/449379_2_En_20_Fig61_HTML.jpg)

A page titled Your time sheet web form is ready. It reads the U R Ls of the created sheets with share and embed code options. Recommended actions such as preview this web form, view your web forms, and create another web form and highlighted done button are at the bottom.

Figure 20-61

Reviewing your URL and other recommended actions and click Done

Each participant will receive a PDF copy or email link once the form is completed, and you can track your activity in your Documents list or preview the form you just created. You can then look at other recommended actions at this stage, such as previewing the web form.

Now click Done, and the file is added to your list on your Adobe home page for share and review. Refer to Figure [20-62](#449379_2_En_20_Chapter.xhtml#Fig62).

![](images/449379_2_En_20_Chapter/449379_2_En_20_Fig62_HTML.jpg)

A screenshot of the created timesheet listed under the recent tab of the Adobe home page. It reads the name, shared, opened date and time, and size of the time sheet.

Figure 20-62

The newly created form is added to the Adobe Acrobat home page

As this area is always changing and updating with new features, here are some links and resources that allow you to continue this learning process, as well as references to how to format fields:

*   [`https://www.adobe.com/sign.html`](https://www.adobe.com/sign.html)

*   [`https://www.adobe.com/sign/features/web-forms.html`](https://www.adobe.com/sign/features/web-forms.html)

*   [`https://helpx.adobe.com/sign/config/web-forms.html`](https://helpx.adobe.com/sign/config/web-forms.html)

*   [`https://helpx.adobe.com/sign/power-user/web-form/create.html`](https://helpx.adobe.com/sign/power-user/web-form/create.html)

*   [`https://helpx.adobe.com/sign/power-user/web-form/edit.html`](https://helpx.adobe.com/sign/power-user/web-form/edit.html)

*   [`https://helpx.adobe.com/sign/power-user/web-form/disable.html`](https://helpx.adobe.com/sign/power-user/web-form/disable.html)

*   [`https://helpx.adobe.com/sign/power-user/web-form/hide.html`](https://helpx.adobe.com/sign/power-user/web-form/hide.html)

*   [`https://helpx.adobe.com/sign/power-user/web-form/find-code.html`](https://helpx.adobe.com/sign/power-user/web-form/find-code.html)

*   [`https://helpx.adobe.com/sign/power-user/web-form/url-parameters.html`](https://helpx.adobe.com/sign/power-user/web-form/url-parameters.html)

*   [`https://helpx.adobe.com/sign/using/reference-guides.html`](https://helpx.adobe.com/sign/using/reference-guides.html)

*   [`https://helpx.adobe.com/sign/using/authoring-tools-create-forms.html`](https://helpx.adobe.com/sign/using/authoring-tools-create-forms.html)

*   [`https://helpx.adobe.com/sign/using/field-types.html`](https://helpx.adobe.com/sign/using/field-types.html)

*   [`https://helpx.adobe.com/lu_en/sign/using/automatic-field-detection.html`](https://helpx.adobe.com/lu_en/sign/using/automatic-field-detection.html)

This link will help you get started with your calculation formulas:

*   [`https://helpx.adobe.com/sign/using/calculated-fields.html`](https://helpx.adobe.com/sign/using/calculated-fields.html)

Published forms on the Adobe site can always be returned to and edited again when you click the thumbnail in the list in the home area and then, after editing, click Save and Publish again. Then return to the home page with the Acrobat icon. Refer to Figure [20-63](#449379_2_En_20_Chapter.xhtml#Fig63).

![](images/449379_2_En_20_Chapter/449379_2_En_20_Fig63_HTML.jpg)

A screenshot of editing options under the actions tab of Adobe Acrobat. Get code, edit web form, download p d f, report abuse, download form field data, download individual files, share, hide web form, and add notes. Edit web form is selected. The save button is highlighted at the bottom.

Figure 20-63

Actions for editing the web form

You can close your browser when you are done.

Once you receive an email from Adobe Acrobat Sign, you can then click the URL, and you will be taken to the form. Refer to Figure [20-64](#449379_2_En_20_Chapter.xhtml#Fig64).

![](images/449379_2_En_20_Chapter/449379_2_En_20_Fig64_HTML.jpg)

A screenshot of the link of the U R L that locates the created timesheet to be signed by the users. h t t p colon double forward slash n a 1 dot documents dot adobe dot com forward slash public forward slash e sign widget question mark w i d equals to C B F C I B A A 3 A A A B L.

Figure 20-64

Link in email to view your online form for signing

This is a great way to test and preview the form so that you can see if your fields for calculation as well as signature work as intended. Refer to Figure [20-65](#449379_2_En_20_Chapter.xhtml#Fig65).

![](images/449379_2_En_20_Chapter/449379_2_En_20_Fig65_HTML.jpg)

A screenshot of a timesheet with employee information titled Tourmaline Mining Corporation under Adobe Acrobat Sign page. Please sign timesheet, and next required field as 1, are on top. Admin signature field with click here to sign, is at the bottom.

Figure 20-65

Form ready for signing with Adobe Acrobat Sign settings

If not, then you can open the form again while on the Adobe home page, as mentioned earlier, and then edit the form and publish again. The same link will continue to work as long as the form is online and active.

Note

Keep in mind that these forms are now online, and if you want to make major text edits, you would need to go back to your original PDF on your computer and make your edits there before uploading the form with its fields again.

For details on using options of the Tools such as Send in Bulk, Add E-Sign Branding, and Collect Payments, refer to the following links. Likewise, you will also find additional tools on `acrobat.adobe.com` on the Home tab under Tools. Refer to Figure [20-66](#449379_2_En_20_Chapter.xhtml#Fig66) and Figure [20-67](#449379_2_En_20_Chapter.xhtml#Fig67).

![](images/449379_2_En_20_Chapter/449379_2_En_20_Fig67_HTML.jpg)

A screenshot of the icons under tools tab. Combine files, Create e-sign template, P D F to J P G, and Send in Bulk, with a brief description for each.

Figure 20-67

Additional tools in the Adobe Acrobat online Tools area

![](images/449379_2_En_20_Chapter/449379_2_En_20_Fig66_HTML.jpg)

A screenshot of three icons. Send in bulk, Add E-sign branding, and Collect payments. Each has an add button below it.

Figure 20-66

Additional tools that you can use with clients and your Acrobat forms

Send in Bulk: Send a document to many people at once to sign individually:

*   [`https://helpx.adobe.com/sign/using/pdf-pack-send-in-bulk.html`](https://helpx.adobe.com/sign/using/pdf-pack-send-in-bulk.html)

Add E-Sign Branding: Add your company name and custom URL to e-sign agreements (make sure you are on the Brand settings tab):

*   [`https://helpx.adobe.com/sign/using/upload-logo-to-account.html`](https://helpx.adobe.com/sign/using/upload-logo-to-account.html)

Collect Payment: Set up a Braintree Account to collect payments from the signer.

*   [`https://helpx.adobe.com/sign/using/collect-online-payments.html`](https://helpx.adobe.com/sign/using/collect-online-payments.html)

Online tools will allow you to create e-sign templates as well. This is a reusable template of frequently used form fields. You can create a sign template and use it for sending agreements for signatures. Refer to this link for more details:

[`https://helpx.adobe.com/document-cloud/help/create-template.html`](https://helpx.adobe.com/document-cloud/help/create-template.html)

Note

A web form is considered an agreement, and if you need to know how to remove one, here are links to that information since the removal process is a bit complex. You can, however, if the file is labeled as Agreement, check it and in the Actions pane click Cancel and then Delete to remove it. This is useful if you are just practicing using Request E-Signatures, as we saw earlier in the chapter. Refer to Figure [20-68](#449379_2_En_20_Chapter.xhtml#Fig68).

[`https://helpx.adobe.com/sign/kb/to-delete-a-document-from-manage-Page.html`](https://helpx.adobe.com/sign/kb/to-delete-a-document-from-manage-Page.html)

[`https://helpx.adobe.com/sign/using/sign-cancel-agreement.html`](https://helpx.adobe.com/sign/using/sign-cancel-agreement.html)

[`https://helpx.adobe.com/document-cloud/help/delete-restore-agreement.html`](https://helpx.adobe.com/document-cloud/help/delete-restore-agreement.html)

[`https://community.adobe.com/t5/adobe-acrobat-sign-discussions/looking-for-an-option-to-delete-document-from-adobe-sign-we-can-help-you/td-p/11035932`](https://community.adobe.com/t5/adobe-acrobat-sign-discussions/looking-for-an-option-to-delete-document-from-adobe-sign-we-can-help-you/td-p/11035932)

For the web form, while you currently cannot delete it directly, you can from the Actions pane choose to hide the web form if you no longer want to see it in the recent list or access it.

![](images/449379_2_En_20_Chapter/449379_2_En_20_Fig68_HTML.png)

A screenshot of created timesheet with actions dropdown list to open agreement, edit agreement, cancel, hide agreement, and download p d f. Also includes delete, cancel, hide, and sign web form options. A preview of cancel option has a box to enter the reason and cancel agreement button.

Figure 20-68

Cancel and delete an agreement or hide a web form on the Adobe Acrobat home page using preset actions

Tip

As you saw in Chapter [15](#449379_2_En_15_Chapter.xhtml), you can use the Action Wizard tool to speed up repetitive tasks. The Action Wizard now allows you to add an action tool for the Create a Web Form tool in the Go To section as well as access the other new tools recently added to Acrobat Pro that were mentioned in this chapter, including Request E-Signatures. This may be useful if you must work on multiple files in a folder that require the same actions.

## Summary

This chapter looked at some of the additional options that Acrobat offers for you to complete your forms if they require multiple signatures from participants, specifically online. However, for these forms, custom coding cannot be programed directly into the fields until they are edited online as a web form or you use the Request E-Signatures tool to create an agreement that can be later uploaded online.

In Part V, which consists of Chapter [21](#449379_2_En_21_Chapter.xhtml), you will be putting your knowledge into practice with a collection of extra homework assignments you can try to build up your skills.

# Part VPutting It into Practice

# 21. Homework Assignments

**Chapter Goal:** This chapter is a collection of homework assignments you can try to build up your skills and put your knowledge into practice.

Note

To complete the four assignments in this chapter, be sure to download the Chapter [21](#449379_2_En_21_Chapter.xhtml) folder from the link in the introduction. This folder contains original files and script files as well as “Start” and “End” PDF files for each assignment.

## Homework Assignment 1: Show and Hide

Review Chapters [1](#449379_2_En_1_Chapter.xhtml), [2](#449379_2_En_2_Chapter.xhtml), [4](#449379_2_En_4_Chapter.xhtml), and [9](#449379_2_En_9_Chapter.xhtml).

Use the file `Assignment1_showhide_start.pdf`; the document has two pages. Later you can review the “End” file if you need to compare it to your result.

This assignment will be a review of the show and hide actions. However, this time you will use yes and no radio buttons rather than a check box and a regular button. It’s good to try similar projects in different ways to see which solution you are more comfortable with. Depending on the form you create, one way may be better than another. Remember that radio buttons are always found in groups. Most of the fields have already been created for you. All you need to do is add the correct actions to them.

![](images/449379_2_En_21_Chapter/449379_2_En_21_Fig18_HTML.png)

A screenshot depicts the show or hide field that lists the options under select a field with the highlighted address 2, two radio buttons for show and hide, and a selected O K button.

Figure 21-18

The actions for radio buttons: show or hide. Change the toggle to show or hide a specific field using the dialog box

![](images/449379_2_En_21_Chapter/449379_2_En_21_Fig17_HTML.jpg)

A screenshot depicts the radio button properties dialog box that lists the options under actions with add action and actions, along with several text fields labeled first name 2, last name 2, address 2, city 2, provinces 2, country 2, and postal code 2.

Figure 21-17

The actions for the radio buttons that will affect the text fields on the right

![](images/449379_2_En_21_Chapter/449379_2_En_21_Fig16_HTML.jpg)

A screenshot depicts the radio button properties dialog box that lists the tabs under options with button style, radio button choice, and a selected checker box of the button is checked by default.

Figure 21-16

The Options tab with a choice of No

![](images/449379_2_En_21_Chapter/449379_2_En_21_Fig15_HTML.jpg)

A screenshot depicts the radio button properties dialog box that lists the tabs under options with button style and radio button choice.

Figure 21-15

The Options tab with a choice of Yes

![](images/449379_2_En_21_Chapter/449379_2_En_21_Fig14_HTML.jpg)

A screenshot presents the text that reads the shipping address different, with yes and no radio buttons.

Figure 21-14

Yes and No radio buttons in the upper right of the shipping info area

![](images/449379_2_En_21_Chapter/449379_2_En_21_Fig13_HTML.jpg)

2 screenshots of the text field properties dialog box. On the top, it lists the options under common properties. On the bottom, it lists the tabs under options with alignment and default values.

Figure 21-13

The Country field on the right is set to hidden and read-only on the General tab, and the default value text is added on the Options tab

![](images/449379_2_En_21_Chapter/449379_2_En_21_Fig12_HTML.png)

A screenshot depicts several text fields with first name 2, last name 2, address 2, city 2, province 2, country 2, and postal code 2\. It includes the text field properties dialog box that lists the options under general with name, tooltip, and common properties options.

Figure 21-12

The text fields on the right in the shipping info are set to Hidden

![](images/449379_2_En_21_Chapter/449379_2_En_21_Fig11_HTML.jpg)

A screenshot of the page presents the home address information with the first name, last name, address, city, state, country, and postal code, and the No option is selected in the shipping address information.

Figure 21-11

The lower half of page 2 of Assignment 1, Shipping Info with No selected

![](images/449379_2_En_21_Chapter/449379_2_En_21_Fig10_HTML.jpg)

A screenshot of the page presents the home and shipping address information with the first name, last name, address, city, state, country, and postal code.

Figure 21-10

The lower half of page 2 of Assignment 1, Shipping Info

![](images/449379_2_En_21_Chapter/449379_2_En_21_Fig9_HTML.jpg)

A screenshot of the text field properties dialog box that lists the options under format with select format category and date options.

Figure 21-9

The Format tab with a specific date setting

![](images/449379_2_En_21_Chapter/449379_2_En_21_Fig8_HTML.jpg)

A screenshot of the Assignment 1 Show and Hide page with the customer's full name, date, customer order code, and customer's company name.

Figure 21-8

The upper half of page 2 of Assignment 1

![](images/449379_2_En_21_Chapter/449379_2_En_21_Fig7_HTML.png)

A snippet of the preview button on the left and the edit button on the right.

Figure 21-7

The Preview and Edit buttons

![](images/449379_2_En_21_Chapter/449379_2_En_21_Fig6_HTML.jpg)

A screenshot depicts the actions tab with the selected mouse-up menu, Go to a page in this document on the page 2 menu and zoom level.

Figure 21-6

The final set of actions is added to the Actions tab

![](images/449379_2_En_21_Chapter/449379_2_En_21_Fig5_HTML.png)

A screenshot presents the Create Go to View dialog box with some text that reads Use the scrollbars, mouse, and zoom tools to select the target view. It has a highlighted set link button.

Figure 21-5

Click the Set Link button after you go to page 2

![](images/449379_2_En_21_Chapter/449379_2_En_21_Fig4_HTML.jpg)

A screenshot presents the button properties dialog box that lists the options under actions with add an action. It includes the mouse-up trigger and go to a page view action options.

Figure 21-4

On the Action tab, click the Add button

![](images/449379_2_En_21_Chapter/449379_2_En_21_Fig3_HTML.jpg)

A screenshot presents the button properties dialog box that lists the options under actions with add an action. It includes the select trigger and the select action options.

Figure 21-3

The Button properties on the Actions tab

![](images/449379_2_En_21_Chapter/449379_2_En_21_Fig2_HTML.jpg)

2 screenshots present the button properties dialog box. On the top, it lists the options under general with a name and tooltip. On the bottom, it lists the tabs under options with layout, behavior, icon, and label.

Figure 21-2

The button properties in the General and Options tabs. You can also add tooltip text. The label layout is “Label only.” Set Behavior to Invert, State to Up, and the label to “Go to Form”

![](images/449379_2_En_21_Chapter/449379_2_En_21_Fig1_HTML.png)

A screenshot presents the prepare form icon with the add and go to form options.

Figure 21-1

Add a button to page 1

1.  Go to the Prepare Form tool. Refer to Figure [21-1](#449379_2_En_21_Chapter.xhtml#Fig1).

    On page 1 of the document, add a Go to Form button in the blank area at the bottom of the page. This will help the reader to jump to the page that has the form. Helpful navigation is useful when a document contains many pages and you want the reader to get to a specific page quickly.

2.  Add the following properties to the General and Options tabs. Refer to Figure [21-2](#449379_2_En_21_Chapter.xhtml#Fig2).

3.  For the Actions tab, do the following:

    Select Trigger: Mouse Up

    Select Action: Go to a page view

    Refer to Figure [21-3](#449379_2_En_21_Chapter.xhtml#Fig3).

4.  Now click the Add button. Refer to Figure [21-4](#449379_2_En_21_Chapter.xhtml#Fig4).

5.  While these instructions are on the screen, go to page 2 of the document. Make sure the form is at 75 percent so you can see the entire form. When you are happy with the size, click the Set Link button. Refer to Figure [21-5](#449379_2_En_21_Chapter.xhtml#Fig5).

6.  When you are done, your Actions properties should look like Figure [21-6](#449379_2_En_21_Chapter.xhtml#Fig6).

7.  Click the Close button in the properties dialog box when done.

8.  Click the Preview button in the upper-right area of the Prepare Form tool to make sure the Go to Form button is working. Refer to Figure [21-7](#449379_2_En_21_Chapter.xhtml#Fig7).

9.  To return to editing, click the Edit button. Refer to Figure [21-7](#449379_2_En_21_Chapter.xhtml#Fig7).

    Now you’ll work on page 2\. Refer to Figure [21-8](#449379_2_En_21_Chapter.xhtml#Fig8).

10.  In the upper-right area, make sure the Date field is formatted to mm/dd/yyyy. Otherwise, leave this area of fields alone, and do not apply any additional changes or actions. Refer to Figure [21-9](#449379_2_En_21_Chapter.xhtml#Fig9).

    Let’s look at the next area of the form you want to work on. Refer to Figure [21-10](#449379_2_En_21_Chapter.xhtml#Fig10).

    As in Chapter [4](#449379_2_En_4_Chapter.xhtml), when the No radio button is selected, you want the fields on the right will be hidden. When the Yes radio button is selected, you want the fields on the right to be visible. As in the button example, if you realize afterward that the addresses are the same, you can click the No radio button again to hide all the right-hand fields and reset them to blank. Refer to Figure [21-11](#449379_2_En_21_Chapter.xhtml#Fig11).

11.  The left fields are already set up. Begin with the fields on the right and make sure that their General tab property is set to Form Field: Hidden. Refer to Figure [21-12](#449379_2_En_21_Chapter.xhtml#Fig12).

12.  Only the Country_2 field should be set to Read Only. And there is some Default Value text added using the Options tab, which you can edit if you prefer. Refer to Figure [21-13](#449379_2_En_21_Chapter.xhtml#Fig13).

    You do not need to make any other adjustments to these fields.

    Now focus on the radio button group (Group 1), which will do the show/hide and reset. Refer to Figure [21-14](#449379_2_En_21_Chapter.xhtml#Fig14).

13.  Go to the Options tab of the Yes radio button. The Radio Button choice is Yes. Refer to Figure [21-15](#449379_2_En_21_Chapter.xhtml#Fig15).

14.  Now go to the Options tab of the No radio button. The Radio Button Choice setting is No. Enable the option “Button is checked by default.”

    Refer to Figure [21-16](#449379_2_En_21_Chapter.xhtml#Fig16).

15.  For the Actions tab for Yes and No, refer to Chapter [4](#449379_2_En_4_Chapter.xhtml) to show/hide a field and apply them to the radio buttons for each of the fields on the right. Refer to Figure [21-17](#449379_2_En_21_Chapter.xhtml#Fig17).

    Remember that the Yes radio button will show the fields.

16.  Make sure to add actions to show these fields:
    *   First Name_2

    *   Last Name_2

    *   Address_2

    *   City_2

    *   Provinces_2

    *   Country_2

    *   PostalCode_2

    Refer to Figure [21-18](#449379_2_En_21_Chapter.xhtml#Fig18).

    **Note** You will need to show these and add them one at a time because you cannot select more than one at a time using the dialog box.

The No radio button will hide and reset the form fields.

1.  Make sure to add actions to hide these fields:
    *   First Name_2

    *   Last Name_2

    *   Address_2

    *   City_2

    *   Provinces_2

    *   Country_2

    *   PostalCode_2

    **Note** You will need to hide these and add them one at a time because you cannot select more than one at a time using the dialog box.

2.  Finally, for the No radio button, you will need to select and add the Reset a Form action at the bottom of the list. Refer to Figure [21-19](#449379_2_En_21_Chapter.xhtml#Fig19).

3.  You will then select the items you need to reset, which you can do in one action. Refer to Figure [21-20](#449379_2_En_21_Chapter.xhtml#Fig20).
    *   First Name_2

    *   Last Name_2

    *   Address_2

    *   City_2

    *   Provinces_2

    *   Country_2

    *   PostalCode_2

    *   Group1

    Remember to include your Group 1 Yes and No radio buttons in the reset because they need to refresh if set back to the original settings.

    When you are done, close the properties, preview your results, and make sure the radio buttons are functioning correctly. Refer to Figure [21-7](#449379_2_En_21_Chapter.xhtml#Fig7) if you can’t remember how to do this.

    ![](images/449379_2_En_21_Chapter/449379_2_En_21_Fig20_HTML.jpg)

    A screenshot of the reset a form dialog box that lists the options under select fields to reset with the highlighted provinces 2, and a selected OK button.

    Figure 21-20

    The Actions tab of the Reset a Form dialog box for the No radio button and the field choices that will be reset

    ![](images/449379_2_En_21_Chapter/449379_2_En_21_Fig19_HTML.jpg)

    A screenshot presents the radio button properties dialog box that lists the options under actions, with add an action that includes the select trigger and action options, and actions with a highlighted reset form.

    Figure 21-19

    The Actions tab for the No radio button with a reset added last

    **Note** Alternatively, use Chapter [9](#449379_2_En_9_Chapter.xhtml) to figure out how you would show and hide these same fields on your own with the radio buttons using JavaScript.

The final item is to add a Print this Document button at the bottom of page 2\. Refer to Figure [21-21](#449379_2_En_21_Chapter.xhtml#Fig21).

1.  Make sure to give the button a name of Print on the General tab. Add a label on the Options tab, as you see in Figure [21-21](#449379_2_En_21_Chapter.xhtml#Fig21).

2.  For the Actions tab, choose Execute a Menu Item, click Add, select File ➤ Print, and click OK. Refer to Figure [21-22](#449379_2_En_21_Chapter.xhtml#Fig22).

    This will print both pages. To print just one page, you will need the JavaScript coding from Chapter [10](#449379_2_En_10_Chapter.xhtml).

3.  Test the button. When the user clicks Print, they will go automatically to the Print dialog box.

![](images/449379_2_En_21_Chapter/449379_2_En_21_Fig21_HTML.jpg)

A snippet of the print this document tab.

Figure 21-21

The Print this Document button

![](images/449379_2_En_21_Chapter/449379_2_En_21_Fig22_HTML.jpg)

A screenshot of the button properties that lists the option under actions with the add an action that includes the select trigger and actions. It includes the menu item dialog box with the highlighted file print button.

Figure 21-22

The actions for the Print button

Select File ➤ Save to save the document. You are now finished with Assignment 1\. If you need to compare notes, you can look at my files `Assignment1_showhide_End.pdf` and `Assignment1_showhide_End_2.pdf`.

## Homework Assignment 2: Working with JavaScript to Create Formulas

Review Chapters [5](#449379_2_En_5_Chapter.xhtml), [6](#449379_2_En_6_Chapter.xhtml), [7](#449379_2_En_7_Chapter.xhtml), [10](#449379_2_En_10_Chapter.xhtml), and [18](#449379_2_En_18_Chapter.xhtml).

Use the file `Assignment_2_formulas_Start.pdf` to practice. You can refer to the “End” file if you need to compare afterward.

Refer to Figure [21-23](#449379_2_En_21_Chapter.xhtml#Fig23) to open your form with the Prepare Form tool.

![](images/449379_2_En_21_Chapter/449379_2_En_21_Fig23_HTML.jpg)

A screenshot of the prepare form icon with the add tab.

Figure 21-23

The Prepare Form tool

For this assignment, you will complete five formulas using JavaScript. The actual formulas have been given above each field. The fields have already been created for you and formatted to Number on the Format tab. The fields on the right are also set to read-only on the General tab so that that these fields cannot be tampered with by clients during the calculation. Refer to Figure [21-24](#449379_2_En_21_Chapter.xhtml#Fig24).

![](images/449379_2_En_21_Chapter/449379_2_En_21_Fig24_HTML.jpg)

A screenshot of the page, which includes the common properties of the text field with an area of the circle, the circumference of the circle, the volume of the sphere, the Celsius to Fahrenheit formula, and the Fahrenheit to Celsius formula.

Figure 21-24

Assignment 2 has five formulas you can use to create the calculations for the fields on the right. Note that the fields have been set to read-only

The fields on the left require no extra JavaScript. You will only call them through variables as required, using the fields on the right.

The first formula must be written as follows:

### Area of a Circle *A = π r* ^(*2*)

In the field AreaRow1, enter the following script in the “Custom calculation script” area on the Calculate tab and refer to Figure [21-25](#449379_2_En_21_Chapter.xhtml#Fig25):

![](images/449379_2_En_21_Chapter/449379_2_En_21_Fig25_HTML.jpg)

A screenshot of the text field properties dialog box that lists the options under the calculate with the value of the field, simplified field notation, and the selected custom calculation script.

Figure 21-25

Assignment 2 on the Calculate tab of a Text field

```
var radius1 = this.getField("RadiusRow1");
event.value = Math.PI * Math.pow(radius1.value, 2);
```

The number `2` in `Math.pow(radius1.value, 2);` is equivalent to writing r².

Test it to make sure that the formula is working and then proceed to the other four calculations.

If you’re not sure where to start, here is a hint:

### Field: CircumferenceRow1 Circumference of a Circle C = 2 π r

```
var radius2 = this.getField("RadiusRow1_2");
event.value = 2 * _______ * radius2.value;
```

What value from the previous example should go here to complete the formula?

What does the JavaScript equivalent look like?

### Field: VolumeRow1 Volume of a Sphere V = 4/3 π r³

```
var radius3 = this.getField("RadiusRow1_3");
event.value = (4/3)* _______ * Math.pow(radius3.value, _____);
```

What two values from the previous example should go here to complete the formula?

What does the JavaScript equivalent look like?

### Field: FahrenheitRow1 Celsius to Fahrenheit to Formula. (°C x 9/5) + 32 = °F.

```
var celsius = this.getField("CelsiusRow1");
event.value = (celsius.value* 9/5)+ _______;
```

What value from the previous example should go here to complete the formula?

What does the JavaScript equivalent look like?

### Field: CelsiusRow1_2 Fahrenheit to Celsius Formula. (°F - 32) x 5/9 = °C

```
var fahrenheit = this.getField("FahrenheitRow1_2");
event.value = (fahrenheit.value-_______)*(5/9);
```

What value from the previous example should go here to complete the formula?

What does the JavaScript equivalent look like?

Once you have entered the correct formulas into the Calculation tab area using the JavaScript Editor, click OK and the close the Properties dialog box. Refer to Figure [21-26](#449379_2_En_21_Chapter.xhtml#Fig26).

![](images/449379_2_En_21_Chapter/449379_2_En_21_Fig26_HTML.png)

A screenshot presents the selected radio button of the custom calculation script on the left and the close button on the right.

Figure 21-26

Assignment 2 on the Calculate tab. Select “Custom calculation script” and click the Close button

Preview and test them to make sure they are working. Sometimes you need to save the file, close it, and then open it again so that it takes effect.

If it is functioning correctly, continue to the next part of the assignment.

For the next part of the assignment, you will add some JavaScript to the Validation tab of these three fields. Refer to Chapter [6](#449379_2_En_6_Chapter.xhtml) and Figure [21-27](#449379_2_En_21_Chapter.xhtml#Fig27).

![](images/449379_2_En_21_Chapter/449379_2_En_21_Fig27_HTML.jpg)

A screenshot of the text field properties dialog box that lists the options under validate with the three radio buttons for field value is not validated, the field value is in range, and a selected run custom validation script.

Figure 21-27

The Validate tab with a custom validation script

*   AreaRow1

*   CircumferenceRow1

*   VolumeRow1

Select “Run custom validation script.”

Enter the following code:

```
if (event.value == 0) event.value = "";
```

This is to ensure that the field remains blank if nothing is entered in the radius fields. However, what alternative code could you use to ensure that if 0 was entered in the input field, 0 would show in the read-only field?

Click OK after entering the script in each.

For the fields

*   FahrenheitRow1

*   CelsiusRow1_2

enter this validation script:

```
if (event.value > 0){
event.target.textColor = color.black;
}
else{
event.target.textColor = color.red;
}
```

If the calculation is greater than zero, the text will be black; if less than zero, the text will be red. As mentioned in Chapter [10](#449379_2_En_10_Chapter.xhtml), without JavaScript you could not have given these numbers a two-color option.

Now for these two fields, as well as the two fields CelsiusRow1 and FehrenheitRow1_2, let’s take one more step. Use Chapter [18](#449379_2_En_18_Chapter.xhtml) to figure out how to add a degree symbol to the Format tab as a custom format using document JavaScript. Refer to Figure [21-28](#449379_2_En_21_Chapter.xhtml#Fig28) as a hint.

![](images/449379_2_En_21_Chapter/449379_2_En_21_Fig28_HTML.jpg)

A screenshot of the text field properties dialog box that lists the options under format with the select format category and the custom options. The custom options include the custom format script. It has a validation field with a Fahrenheit value of 32.00 degrees.

Figure 21-28

The Validate tab with a custom validation script applied to the field

Save your file and test it. You have finished Assignment 2.

## Homework Assignment 3: Custom Validation and Regular Expressions

Refer to Chapters [8](#449379_2_En_8_Chapter.xhtml), [9](#449379_2_En_9_Chapter.xhtml), [10](#449379_2_En_10_Chapter.xhtml), and [11](#449379_2_En_11_Chapter.xhtml).

Refer to the file `Assignment_3_validation_Start.pdf` and see the “End” version if you need to review after doing this assignment.

Refer to Figure [21-29](#449379_2_En_21_Chapter.xhtml#Fig29) to open your form with the Prepare Form tool.

![](images/449379_2_En_21_Chapter/449379_2_En_21_Fig29_HTML.jpg)

A screenshot presents the prepare form icon with the add option.

Figure 21-29

Prepare Form tool

This assignment is a review of custom validations and regular expressions. Refer to Figure [21-30](#449379_2_En_21_Chapter.xhtml#Fig30).

![](images/449379_2_En_21_Chapter/449379_2_En_21_Fig30_HTML.jpg)

A page presents a table with five rows labeled employee name, employee serial number, email, website U R L, and telephone number, with the date and reset the fields options.

Figure 21-30

This assignment contains regular expressions and default text on the Format tab

For the Date field, leave the format set to None since you will try two format options. Refer to Figure [21-31](#449379_2_En_21_Chapter.xhtml#Fig31) for the next step.

![](images/449379_2_En_21_Chapter/449379_2_En_21_Fig31_HTML.jpg)

2 screenshots of the text field properties dialog box. On the top, it lists the options under format with the select format category. On the bottom, it lists the options under validate with the three radio buttons for the field value and a selected run custom validation script.

Figure 21-31

Format tab and Validate tab: Run a custom validation script

Instead, add the following validation on the Validate tab, in the “Run custom validation script” area:

```
//Date Validation
var re6Date = /^(\d{1,2})\/(\d{1,2})\/(\d{2})$/;
var re8Date = /^(\d{1,2})\/(\d{1,2})\/(\d{4})$/;
//prevent alert if field is blank
if(event.value !=""){
if(re6Date.test(event.value)== false && re8Date.test(event.value)== false){
app.alert("I'm sorry. That is not a valid format. It must be mm/dd/yy or mm/dd/yyyy. ");
event.rc = false;
}
}
```

Refer to Chapter [11](#449379_2_En_11_Chapter.xhtml) for a review of date validation and formatting.

For the Employee Name field, set the Format tab to Custom. Refer to Figure [21-32](#449379_2_En_21_Chapter.xhtml#Fig32).

![](images/449379_2_En_21_Chapter/449379_2_En_21_Fig32_HTML.jpg)

A screenshot presents the text field with the employee name and a text field properties dialog box that lists the options under format with the select format category and custom options.

Figure 21-32

Apply a custom format category on the Format tab

Enter the following custom format script:

```
// Custom Format script
if (!event.value){
event.value = "What is your First and Last Name?";
event.target.textColor = color.gray;
}
else{
event.target.textColor = color.black;
}
```

Then click the “Run custom validation script” button on the Validate tab and enter the following code. Refer to Figure [21-33](#449379_2_En_21_Chapter.xhtml#Fig33).

![](images/449379_2_En_21_Chapter/449379_2_En_21_Fig33_HTML.jpg)

A screenshot presents the text field properties dialog box that lists the options under validate with the three radio buttons for the field value is not validated, the field value is in range, and a selected run custom validation script.

Figure 21-33

Apply validation on the Validate tab for the Employee Name field

```
// Custom Validation of Name
var re = /^[A-Z][a-z]+ [A-Z][a-z]+$/;
if(event.value !=""){
if (re.test(event.value)== false){
app.alert("That does not appear to be a valid name. I need a first and last name.");
event.rc = false;
}
}
```

Looking at JavaScript, think about how you could modify this code for the other fields on the Format and Validate tabs. This script is covered in Chapters [9](#449379_2_En_9_Chapter.xhtml) and [10](#449379_2_En_10_Chapter.xhtml) (the default method) and Chapter [11](#449379_2_En_11_Chapter.xhtml).

*   Employee Serial Number

*   Email

*   Website URL

*   Telephone Number

For the telephone number, alter the script on the Custom Format tab slightly for three colors since the original had only two colors in the example.

```
// Custom Format script
var rgexTele = /^[(]{0,1}[0-9]{3}[)]{0,1}[-\s\.]{0,1}[0-9]{3}[-\s\.]{0,1}[0-9]{4}$/;
if (!event.value){
event.value = "Example: 999-999-9999";
event.target.textColor = color.gray;
}
else if(rgexTele.test(event.value)){
event.target.textColor = color.black;
}
else{
event.target.textColor = ["RGB",1,0,0]; //Change text to red
}
```

For the Employee Serial Number Format and Validate tab script, refer to Chapter [11](#449379_2_En_11_Chapter.xhtml) (text field validation with regular expressions). In this example, I have altered the code slightly.

```
// Custom Format script Tab
if (!event.value){ event.value = "Example: A6BF-479X-2139";
event.target.textColor = color.gray;
}
else{
event.target.textColor = color.black;
}
// Validate Tab Script account number
//A6BF-479X-2139
var re = /^[A-Z]\d[A-Z]{2}-\d{3}[A-Z]-\d{4}$/;
if(event.value !=""){
if (re.test(event.value)== false)
{
app.alert("I'm sorry. That is not a valid account number.");
event.rc = false;
}} (Comment note: One bracket was missing make sure there are two)
```

For the Email Format tab, make sure to adjust your default script. For the Validate tab’s script, refer to Chapter [11](#449379_2_En_11_Chapter.xhtml). Hint: alter this line of code:

```
var re = /^\w+([\.-]?\w+)*@\w+([\.-]?\w+)*(\.\w{2,3})+$/;
```

In addition, alter your `app.alert()` message.

For the Website URL Validate tab script, refer to Chapter [11](#449379_2_En_11_Chapter.xhtml). Hint: alter this line:

```
var re = /^(http:\/\/www\.|https:\/\/www\.|http:\/\/|https:\/\/)[a-z0-9]+([\-\.]{1}[a-z0-9]+)*\.[a-z]{2,5}(:[0-9]{1,5})?(\/.*)?$/;
```

Alter your `app.altert()` message on the Validate tab and adjust your default message on the Format tab.

For the Telephone Number setting, since you already set up the Format tab, now for the Validate tab script, refer to Chapter [11](#449379_2_En_11_Chapter.xhtml) for correct script; use only the 10-digit example at the beginning of the chapter. Hint: look for this line of code:

```
var rgexTele = /^[(]{0,1}[0-9]{3}[)]{0,1}[-\s\.]{0,1}[0-9]{3}[-\s\.]{0,1}[0-9]{4}$/;
```

Alerts such as the one in Figure [21-34](#449379_2_En_21_Chapter.xhtml#Fig34) should appear if the data is not entered correctly, but make sure the message matches the validation.

![](images/449379_2_En_21_Chapter/449379_2_En_21_Fig34_HTML.jpg)

A screenshot depicts the warning JavaScript pop-up window with an alert message that reads an invalid telephone number, which must be 10 digits.

Figure 21-34

An alert from a custom validation script

For this Validation script, you only need the color of the text to change to black if correct and red if a wrong format.

```
if(rgexTele.test(event.value)){
event.target.textColor = color.black; //Change text to black
}
else
{
event.target.textColor = ["RGB",1,0,0]; //Change text to red
app.alert("Invalid Telephone Number! Must be 10 digits.");
}
```

At the end of the assignment, make sure to add a Reset a Form action to all the fields with a button that has a label like “Reset the fields.”

A final thing to add to this button field Reset is to make it multiline. Refer to Chapter [10](#449379_2_En_10_Chapter.xhtml) for an example. Refer to Figure [21-35](#449379_2_En_21_Chapter.xhtml#Fig35) and Figure [21-36](#449379_2_En_21_Chapter.xhtml#Fig36) for how to set up the button.

![](images/449379_2_En_21_Chapter/449379_2_En_21_Fig36_HTML.png)

2 screenshots. On the top, a button properties dialog box lists the options under actions with add action and actions. On the bottom, reset a form dialog box that lists the options under select fields to rest with the highlighted website U R L.

Figure 21-36

Various properties on the Actions tab for the Reset button. Select Run a JavaScript to make the button multiline and reset fields in the form using the dialog box

![](images/449379_2_En_21_Chapter/449379_2_En_21_Fig35_HTML.jpg)

2 screenshots of the button properties dialog box. On the top, it lists the options under general with the name, tooltip, and common properties. On the bottom, it lists the tabs under options with layout, behavior, icon, and label, with the reset the fields button.

Figure 21-35

Various properties on the General and Options tabs for a button

After you enter text for a label on the Options tab, choose Run a JavaScript and enter this code:

```
this.getField("Reset").buttonSetCaption("Reset\n the Fields");
```

Then reset all the form fields. Click OK and then close the button properties. Refer to Figure [21-35](#449379_2_En_21_Chapter.xhtml#Fig35).

When you’re done, save your file and test it. You have completed Assignment 3.

## Homework Assignment 4: Personal Drop-Down List Menu and Definitions Text Box

Review Chapters [11](#449379_2_En_11_Chapter.xhtml) and [12](#449379_2_En_12_Chapter.xhtml).

Use the file `Assignment_4_multiline_dropdown_Start.pdf` and refer to the “End” file if you need to reference it.

Refer to Figure [21-37](#449379_2_En_21_Chapter.xhtml#Fig37) to open your form with the Prepare Form tool.

![](images/449379_2_En_21_Chapter/449379_2_En_21_Fig37_HTML.jpg)

A screenshot presents the prepare form icon with an add option.

Figure 21-37

The Prepare Form tool

This final assignment is useful when you have a topic but need to see more information about it, such as a definition. A drop-down list can be only single-line unless you want to add a regular expression; however, with a lot of text, a multiline menu is a nice option to have, so we will use a text box instead to display. Refer to Figure [21-38](#449379_2_En_21_Chapter.xhtml#Fig38).

![](images/449379_2_En_21_Chapter/449379_2_En_21_Fig38_HTML.jpg)

A page presents a menu with a drop-down menu for selecting a topic and a blank textbox for the definition.

Figure 21-38

The final assignment is to create a drop-down that has extra information appear in a text box

The drop-down and the text field have already been set up. Refer to Figure [21-39](#449379_2_En_21_Chapter.xhtml#Fig39).

![](images/449379_2_En_21_Chapter/449379_2_En_21_Fig39_HTML.jpg)

A screenshot presents the dropdown properties dialog box that lists the tabs under options with the item, export value, item list, and a selected close button.

Figure 21-39

The Options tab of the Dropdown Properties dialog box

The Options tab offers these options:

*   Diamond

*   Ruby

*   Emerald

*   Sapphire

*   Tourmaline

Leave the export value blank for these items so it will not mess with the JavaScript. The top Item setting of Select a Topic should be selected, and it has an export value of 0\. Remember to check “Commit selected value immediately” so the script will load right away into the text box. Uncheck Check Spelling. Refer to Figure [21-40](#449379_2_En_21_Chapter.xhtml#Fig40).

![](images/449379_2_En_21_Chapter/449379_2_En_21_Fig40_HTML.jpg)

A screenshot presents the options with checker boxes for the sort items, allowing the user to enter custom text, check spelling, and commit selected values immediately.

Figure 21-40

The Options tab of the Dropdown Properties dialog box; select “Commit selected value immediately”

For the Options tab of the text field Text2, make sure it is set to multiline in case the comments become longer. As in Figure [21-41](#449379_2_En_21_Chapter.xhtml#Fig41), you can uncheck Check Spelling because this option is not required. Leave the “Scroll long text” options enabled.

![](images/449379_2_En_21_Chapter/449379_2_En_21_Fig41_HTML.jpg)

A screenshot of the text field properties dialog box that lists the tabs under options with alignment and default values. It includes the options tab with the selected multiline and scroll-long text.

Figure 21-41

The Text Field Properties dialog box on the Options tab with Check Spelling not enabled

On the General tab, give the text field a setting of read-only.

Do not alter anything else in the text field and focus only on the drop-down list named Dropdown1 and on the document JavaScript. Refer to Figure [21-42](#449379_2_En_21_Chapter.xhtml#Fig42).

![](images/449379_2_En_21_Chapter/449379_2_En_21_Fig42_HTML.jpg)

A screenshot of the dropdown properties dialog box that lists the options under format with select format category, custom format script, and custom keystroke script.

Figure 21-42

The Dropdown Properties dialog box’s Format tab with a custom keystroke script

In the drop-down list on the Format tab, set it to Custom and add the following JavaScript to the Custom Keystroke Script area so that the values will either display or reset for the text field:

```
if(event.willCommit)
{
if(event.value == "Select a Topic"){
this.resetForm(["Text2"]);
}
else{
SetFieldValues(event.value);
}
}
```

You don’t need to do anything else to the drop-down list. Close the properties dialog box.

Now exit the Prepare Form tool by clicking the Close button and then select Tools ➤ JavaScript. Refer to Figures [21-43](#449379_2_En_21_Chapter.xhtml#Fig43) and [21-44](#449379_2_En_21_Chapter.xhtml#Fig44).

![](images/449379_2_En_21_Chapter/449379_2_En_21_Fig44_HTML.jpg)

A screenshot of the toolbar with the debug, debugger, document JavaScripts, all JavaScripts, and document actions tools.

Figure 21-44

Select the Document JavaScripts subtool to enter the dialog box

![](images/449379_2_En_21_Chapter/449379_2_En_21_Fig43_HTML.png)

A screenshot presents the Prepare Form icon on the left and the JavaScript icon on the right, with a close button in the middle. Each includes the add option.

Figure 21-43

From the Prepare Form tool, switch to the JavaScript tool

Select the Document JavaScripts subtool. Refer to Figure [21-44](#449379_2_En_21_Chapter.xhtml#Fig44).

Name the script `SetFieldValues` and click the Add button. Refer to Figure [21-45](#449379_2_En_21_Chapter.xhtml#Fig45).

![](images/449379_2_En_21_Chapter/449379_2_En_21_Fig45_HTML.png)

2 screenshots. It has a document JavaScripts dialog box on top with the script name and set field values, and a JavaScript editor dialog box on the bottom with the create and edit JavaScripts field.

Figure 21-45

The Document JavaScripts dialog box with the SetFieldValues script. The text is entered into the JavaScript Editor

Enter only this script:

```
//Global Document SetFieldValues Script
// Place all prepopulation data into a single data structure
var MenuData = { Diamond:{ definition: "The Diamond has a hardness of 10."},
Ruby:{ definition: "The Ruby has a hardness of 9."},
Emerald :{ definition: "The Emerald has a hardness on 8\. "},
Sapphire:{ definition: "The Sapphire has a hardness of 9."},
Tourmaline:{ definition: "Tourmaline has a hardness of 7.5."}};
function SetFieldValues(cDropdown)
{
this.getField("Text2").value = MenuData[cDropdown].definition;
}
```

Once entered, click OK to confirm and click Close to close the Document JavaScript dialog box. Refer to Figure [21-45](#449379_2_En_21_Chapter.xhtml#Fig45).

Close the JavaScript tool and test the drop-down list by selecting a name from the menu.

As you let go of the selection from the drop-down list, the definition text should appear in the text box.

When you select the top item Select a Topic, it should reset the text area to blank.

Make sure to test the drop-down list to confirm that all parts of the menu are functioning correctly.

Refer to Figure [21-46](#449379_2_En_21_Chapter.xhtml#Fig46).

![](images/449379_2_En_21_Chapter/449379_2_En_21_Fig46_HTML.jpg)

A page presents a menu with a drop-down menu with a selected diamond and a blank textbox for the definition with the text that reads the diamond has a hardness of 10.

Figure 21-46

This is what the final assignment should look like for the drop-down list and text field

Save your file. You have completed the final assignment.

Note

If you would like to have an additional written assignment test to review your knowledge as you would in a classroom setting, you can look at my file `Advanced Adobe Acrobat_test.docx` as a reference for creating a test for students.

## Summary

To help apply what you’ve learned throughout the book, this chapter presented you with four assignments. It also added a few variations to the original examples found in earlier chapters so that you can see how to alter your script to match your project.

Index A Acrobat Forms using JavaScript Acrobat Pro Tools tab Actions calculate tab, grand total sum customer survey, radio button/check boxes print button rating fields customer survey end form validate tab reset button sum and average radio buttons turning check boxes/radio buttons using check boxes text fields Action Wizard Actions List Actions steps to show adds a watermark to the file color version create new action custom action/custom command Execute JavaScript or Document JavaScripts JavaScript Editor Manage Actions tool and dialog box metacharacters and quantifiers New Action Save Action Save & Export tool section tool menu Adobe Acrobat home page Adobe Acrobat online Tools area Adobe Acrobat Sign Alert method correct form field customer order code customer’s company name customers full name date field set fields, read-only text duplication, fields read only text fields Alerts button comment/sticky note, adding document document actions closes document dialog box document prints tool menu document JavaScripts, create PDF newsletter/catalog signature field/time stamp types viewer version/validation “Apply Format to Multiple Text Fields” B Barcodes Button Actions tab Add an Image Field icon blank button button properties creation Debugger console Edit button field properties/Image Field button Forms Review Icon only image field icon ButtonDiamond Button field properties /Image Field button Button Quartz C Check Box1 Combo boxes Comment tool Conditional statement Create a Web Form Custom action Custom calculation script Custom command Custom QR Code stamp barcode field properties code creation comment tool customizing code custom stamps dialog box macOS/Windows platforms preferences print settings professional printing stamp drop-down menu Custom validation script D Data Field options Date formatting AcrobatFormatCalcTabExamples cFormat string patterns format tab/option JavaScript SFN Degree symbol Digital signatures accessibility tool Adobe security tools appearance properties button field reset check box action employee.signature fields and buttons fields are locked again formatting “Mark as read-only” password access password on the options tab password resets prepare form tool professional certification and IDs properties field properties on options tab properties on the signed tab Reset button security puzzle show hide password Signature Agreement PDF file submit button on actions tab submit file buttons on actions tab text field properties type of array object value of password wrong password Document Actions tool Document button Drop-down lists combo boxes custom formatting, changing list box menu LoadOptions script parts order form request form select dates/discounts Dropdown Properties dialog box E Email Format tab Employee Name field Employee Serial Number Format E-Signature Agreement E-signatures Acrobat forms Acrobat Pro Adobe site align options barcode fields computer desktop document drop-down lists field type list form JavaScript online document operators options and export values PDF file properties radio buttons recipients’ signatures regular forms signature allocation signature fields traditional Acrobat transaction fields validation formats web form E-Signature tool E-Sign drop-down list E-Sign form F Field Type drop-down list Format calculations decimal places/leading zeros drop-down lists MS excel formatting options number formatting numbers percentage custom format script helpful hint select format category text field formatted validate tab validation script special formatting option time Formatting, JavaScript array/variable, example color properties complex formatting, check boxes/text fields fields multiline buttons multiline text printing submit button, email text fields Forms creative cloud desktop app fields general check boxes menu/right pane clear form option field tab order highlight existing fields tool options MS Word preferences prepare form tool properties barcode button/image check box digital signatures dropdown properties dialog box list box radio button reveal text field right-side pane tabs appearance calculate check boxes/radio buttons format general options position select change signed Forms Review G General tab property GetMasterExport function Global document JavaScript debug/debugger subtools document actions dialog box document level, Acrobat editor settings PDF form regular foems *vs*. e-signature forms subtools H Homework assignments actions properties actions tab button properties custom validation script document button drop-down list format tab general and options tabs instructions JavaScript preview button radio button show and hide actions text fields validate tab validation script I if/else condition J, K JavaScript more tools area options reuse JavaScript calculations custom calculation script form creators resetting field select format category setting SFN sum value validation script calculate tab script drop-down lists field calculation order final line of code total field JavaScript Editor JavaScript equivalent JavaScripts Execute/Document JavaScript tool L List boxes button slide show variation changing custom formatting check box/drop-down check box/multi-drop-down control buttons actions tab add add priority addToPriority clear delete down button PDF project prepare form’s edit tool qualityList sort items up button hidden object fields number rating ListTitle LoadOptions functions Logical disjunction operator (||) M Multiple formatting Acrobat Mass Edit Fields Actions Actions tab associative syntax category automatically changes to Number category of none currency symbols Custom Format Script box CustomFunction custom JavaScript custom range decimal places script degreeFormat degree symbol document JavaScript formatting range mass format number fields negative number style No Price Script Price Script properties from the pop-up menu separator style single field selection text field properties on the Format tab unique names in edit mode validate tab with custom validation reference Multistate check boxes Actions tab Button Quartz button with five-star rating option ButtonDiamond buttonGetCaption and buttonSetCaption event.target.textColor font size Forms Review if/else condition if/else conditional statements mimics Mouse Up and Run a JavaScript action precise order prepare form tool radio buttons select all/deselect all N Nested array Nonform actions buttons label-only button options tab layout placement dialog box show/hide fields hide/clear fields button hint layers newsletter navigation, buttons no code properties bookmarks layers nonform navigation buttons pages web/custom hyperlinks setting layer visibility bookmarks buttons floor plan PDF show and hide triggers No radio button O oArray P, Q Participant role options Parts order form calculate tab changed price field connecting drop-down lists cRowName custom keystroke scrip document JavaScripts drop-down properties event.changeEx oArray oArray/object remove elements/object literals SetEachValue text field properties total price Password resets Pop-up menu bookmarks document example PDF file Prepare Form’s edit tool Prepare Form stage Prepare Forms tool Prepare Form tool R Radio Button Choice Radio buttons Rect property Request E-signatures tool Rich text formatting annotations/comments comment/annotation custom formatting fontStyle PDF tools rotation set options text Rollover method default text document JavaScripts JavaScript tool longer array example mouse enter mouse exit shipping address check box and button show/hide fields, JavaScript text field Run custom validation script S Scanner option Security puzzle Select All/Deselect All Separator style SetDescriptionEntries function SetEachValue function SetSelectValues SetTitleEntries Signature Field options Simplified field notation (SFN) barcodes check boxes/radio buttons/regular buttons digital signature’s properties field tabs actions calculate format validate JavaScript list box properties properties tabs/tools Simplified Field Notation (SFN) Stamp tool Switch case break/switch statement T, U Telephone number setting Text Field Properties dialog box Time formatting calculate tab comb of characters, dates custom example date field format tab number SFN Timesheet_End.pdf Tourmaline Mining Corporation Transaction Fields options V Validation changing shipping price, radio buttons formatting fields money transfer example properties dialog box radio buttons radio buttons, without alerts savings account transfer savings/money market pro transfer to another account radio button/text box change time conversion check box variation check button formatting options JavaScript code setting up text fields text field account number date example, two variables email/URL validation incorrect input name validation telephone W, X, Y, Z Web Form option