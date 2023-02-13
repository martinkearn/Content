---
title: Is Office the ultimate app platform
author: Martin Kearn
description: Office is now available as an 'app' on all sorts of different devices including iOS, Android, Windows, MAC, Web Browsers and of course the classic Windows desktop
image: https://dummyimage.com/800x600/000/fff&text=placeholder
thumbnail: https://dummyimage.com/200x200/000/fff&text=placeholder
type: article
status: published
published: 2016/02/03 09:40:00
categories: 
  - Office
  - Apps
---

# Is Office the ultimate app platform?
We all know what an app store is, don't we? We use them to fill our phones and tablets with all those lovely little apps. App stores are our 'go to' place to get the latest functionality for our device.

Also, we all know what Office is don't we?... Office is one of Microsoft's oldest products and has been around since 1990, that makes it 26 years old. As I write this article, I work with people who are younger than Office!

Office is now available as an 'app' on all sorts of different devices including iOS, Android, Windows, MAC, Web Browsers and of course the classic Windows desktop.

## Did you know that Office is quite popular?
[More than 1.2 billion people use Microsoft Office](http://news.microsoft.com/bythenumbers/planet-office), by conservative estimates that is around twice as many people than use IOS.

On average, Office users spend 2-3 hours every day working inside Office applications. Can you think of any other platform (including web browsers) that get anywhere near that level of usage on a daily basis?

Microsoft Word, Excel, PowerPoint and Outlook consistently feature in the top 5 productivity apps for IOS and Android devices according to [App Annie](https://www.appannie.com/apps/ios/matrix/productivity/?device=ipad&date=2016-01-26)

In short, Office has a staggering user base across all sorts of devices and platforms. It gets more eyeballs than any other platform.

## Did you know that Office has an app store?
Office does indeed have an app store.

Office has had an app store ever since the 2013 suite was released. The Office app store provides add-ins which enhance the capabilities of Office. One of my favourite examples is the Wikipedia add-in which lets you search for Wikipedia and insert content from Wikipedia directly from within Office, but there are many more highlights including Bing Translator, LinkedIn, Collins Dictionary, Nimble CRM, PayPal, Evernote, Docusign and many more.

The app store spans all Office products including SharePoint and Office 365 and you'll get a filtered view on the store depending on how you access it. For example, in Word you'll only see add-ins that are suitable for use in Word or you can see the whole store at [store.office.com](https://store.office.com/). 

Like any app store, some are free, some have trials and in-app purchases. Some tie into a back-end services and some are paid-to-download. Like any store, there are great add-ins, good add-ins and not-so-good add-ins, but you’re sure to find something in there that makes those 2-3 hours a day you spend in Office more productive.

There are various types of add-in:
* **Task Pane**: Sits in the task pane near where you would normally expect to find the spell check
* **Content**: Inserted into an Office document in the same way a chart, picture or table might be. It is a piece of content within the document
* **Command**: Puts a button in the Office or SharePoint menu bar. This button can execute your code to do whatever you need it to
* **Mail**: Sits within Outlook and provides a contextual integration with your emails, or calendar appointments
* **SharePoint Full Page**: Occupies a full page in SharePoint which adopts the SharePoint look and feel and 'chrome'
* **SharePoint Part**: A small nugget of functionality within a SharePoint site. Parts can be moved around and managed within the context of the SharePoint page

You can find out more about the various types of Office add-ins at the [Office Developer Center](https://msdn.microsoft.com/en-us/library/office/jj220082.aspx)

## Did you know that Office 365 has a massive unified REST api?
[Office 365](https://products.office.com/en-us/business/explore-office-365-for-business) is Microsoft's cloud offering that compliments the Office client app products. Office 365 is based on a combination of Sharepoint Server, Exchange Server, Skype For Business, Yammer and OneDrive for Business. 

The whole of Office 365 is accessible to developers via a [single unified API](http://dev.office.com/officegraph). It’s a REST API, meaning you can call into it regardless of the technology, platform or language you are using. If your device can call HTTP, it can call the Office 365 API.

The Office 365 API is implemented as a 'graph' API. This means that it is very easy to traverse between entities in your code. For example you could take a user, then find the groups they are a member of, then look at a group, then look at another one of its members, then look at files they have access to and so on. 

If you’ve ever done any development against Facebook's APIs, it’s the same idea.

## Did you know that Office apps are written in Web Technologies?
One of the best things about the Office developer platform is that the underlying technology is pure web development; arguably the most popular and well understood form of development on the planet.  

In 2007, Jeff Atwood wrote an article called ["The Principle of Least Power"](http://blog.codinghorror.com/the-principle-of-least-power/#%20) in which he made this increasingly true statement:
> *"any application that can be written in JavaScript, will eventually be written in JavaScript"*. Atwoods Law, Jeff Atwood, 2007

Add-ins in both Office and SharePoint are effectively iFrames to hosted web applications written in HTML and JavaScript. The web applications use a JavaScript library called [Office.js](https://msdn.microsoft.com/en-us/library/office/fp142185.aspx) or the [Office 365 Graph API](http://dev.office.com/officegraph) to get data in and out of Office.

Add-ins can be written in any web back-end stack (i.e. ASP.NET, PHP, Node.js, Python, Whatever), any front-end stack (plain HTML/CSS/JS, Angular, React, Knockout, Whatever) and can be hosted on any service provider (i.e. your own server, Rackspace, Azure, Amazon web services, Whatever). Ultimately anything that can produce HTML and optionally JavaScript is fine.

The only skills you need to be able to write an Office add-in are:
* Basic HTML, CSS and JavaScript knowledge
* Somewhere to host your web application
* A willingness to understand the Office APIs

Also, because the apps are web applications, they can be easily updated and maintained using your regular web workflow.

## All things considered ...
So when we add all these factors up, we have to ponder whether Office really is the ultimate app platform? Let’s consider some facts...

* Office is arguably the most used software on the planet. It receives more 'eyeballs' on a daily basis than any other app platform
* There is a simple extension and connection model which lets you do very powerful things directly within the Office user interface
* It is just web development. There is no special language to learn, no weird and wonderful platform-specific tricks, just your regular web application with a few simple extensions

Convinced? ... get started at **[Dev.Office.Com](http://dev.office.com/)**

### Resources
* [Dev.Office.Com](http://dev.office.com/)
* [Introduction to Office 365 Development](https://mva.microsoft.com/en-US/training-courses/introduction-to-office-365-development-8329?l=iVSJ4Uay_1504984382)
* [Office 365 Developer Kick Off](https://channel9.msdn.com/events/TechEd/Europe/2014/DEV-B207)
* [Office Developer Center](https://msdn.microsoft.com/en-us/library/office/jj220082.aspx)
