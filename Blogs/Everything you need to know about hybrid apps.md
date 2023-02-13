---
title: Everything you need to know about hybrid apps
author: Martin Kearn
description: The term ‘hybrid app’ is a fairly loaded term these days; it means different things to different people. For me, it means a mobile app that in some way uses a combination of native and web technologies.
image: https://dummyimage.com/800x600/000/fff&text=placeholder
thumbnail: https://dummyimage.com/200x200/000/fff&text=placeholder
type: article
status: published
published: 2016/01/14 09:40:00
categories: 
  - Apps
  - Windows UWP
  - ManifoldJS
---

# Everything you need to know about hybrid apps

This week is all about hybrid apps for me. On Wednesday, we released the first in a [weekly series of videos called ‘Web Hack Wednesday’](https://channel9.msdn.com/Shows/Web-Hack-Wednesday), this week's edition is on [Hybrid apps](https://channel9.msdn.com/Shows/Web-Hack-Wednesday/Hybrid-Apps). On Friday, I am speaking at [NDC London](http://ndc-london.com/) about ‘[The murky world of hybrid apps](http://ndc-london.com/talk/the-murky-world-of-hybrid-web-apps/)’. I wanted to write an article which summarises some of the key points around hybrid apps. The videos and resources listed at the bottom will give you more detail, but I thought it made sense to cover some of the basics here.

## So what is a 'Hybrid App'?

The term ‘hybrid app’ is a fairly loaded term these days; it means different things to different people. For me, it means a mobile app that in some way uses a combination of native and web technologies. There are two distinct types of hybrid app: **Hosted Hybrid**. This is where a large proportion of the app comes from a web application that is hosted in the cloud somewhere (on a web server). The HTML, JS and CSS that make-up the app is served from a web server in the same way that they would be through a browser. This web app is then wrapped in a native app that contains ‘web view’ control. The native part of the app may also contain metadata, images and possibly some native code. **Native Hybrid**. This is simply a native app that runs locally on the device, which happens to be written using web technologies and languages (HTML, JS, CSS). Windows has full support for writing native apps in HTML, JS and CSS. Other app platforms like Android and IOS rely on tools like [Apache Cordova](http://cordova.apache.org/) to act as a middle ware which bridges the gap between JavaScript and the operating system's native APIs. In either case, the code runs locally on the device. We also have Native or 'packaged' apps (which run natively on the device and are written in a native language) and plain old web apps which are accessed via a browser. [![](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/56/73/2477.AppStrategyOptions.PNG)](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/56/73/2477.AppStrategyOptions.PNG)

## When to go hybrid (or not)

The decision on whether to go with hybrid or not is a complex one. There are many conflicting priorities and concerns but here are some of the key considerations for and against: Look at hybrid if ...

*   Your web site/app is modern, responsive, touch enabled and fast. Ideally it will be developed to be an 'web app' not 'web site'. See the resources at the bottom for a list of Web App UI frameworks
*   You want to maintain a single developer skill set and workflow
*   You want broad reach but do not have the budget to build native store apps for every platform
*   You care about TCO

<div>Avoid hybrid if ...</div>

<div>

*   Your web site/app is not very nice
*   You need the very best user experience without compromise and you have the budget to support that

<div>I think this summarises it all very well:</div>

<div>[![](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/56/73/8321.ShitInShitOut-Hybrid.PNG)](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/56/73/8321.ShitInShitOut-Hybrid.PNG)</div>

<div>These are just some of the decision points you should consider when making the decision, others include:</div>

<div>

*   Does you app need local device capabilities?
*   How important is offline? (you can do this with web apps, but it is harder than fully native. Details coming later in article)
*   What are you thoughts on design? Consistent for all your apps or platform conventions?
*   Do you need to get something out quickly (time to market)?
*   What is your revenue model? Will you rely on in-app purchases, store subscriptions or fees for the app itself?
*   Does your app contain content that is prohibited from the app stores?

## How to build hybrid?

So you've decided to go ahead with a hybrid app? Thankfully, there are a bunch of tools that will help you get started. One of the key technologies is a [W3C standard called the 'Web App Manifest'](http://blogs.msdn.com/controlpanel/blogs/posteditor.aspx/w3.org/TR/appmanifest). This is a working draft standard that refers to a manifest file which provides a bunch of metadata about the web site. This will help inform your operating system and the app stores about the app. It will also contain links to resources like icons which are required to add native icons/tiles for your app. W3C describe the specification as follows: _"This specification defines a JSON-based manifest that provides developers with a centralized place to put metadata associated with a web application. This includes, but is not limited to, the web application's name, links to icons, as well as the preferred URL to open when a user launches the web application. The manifest also allows developers to declare a default orientation for their web application, as well as providing the ability to set the display mode for the application (e.g., in fullscreen). Additionally, the manifest allows a developer to "scope" a web application to a URL. This restricts the URLs to which the manifest is applied and provides a means to "deep link" into a web application from other applications. Using this metadata, user agents can provide developers with means to create user experiences that are more comparable to that of a native application."_ If you have a web app manifest file, you can use an open-source tool developed by Microsoft called [ManifoldJS](http://manifoldjs.com/) to create hybrid apps on all the main app platforms (Windows, IOS, Android and a bunch of browsers). You can also use ManifoldJS to help create your web app manifest. [![](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/56/73/1854.Manifold_Primary.png)](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/56/73/1854.Manifold_Primary.png) If you are interested in Windows 10 and the Universal Windows Platform. We have some special capabilities in Windows that allow you to take hybrid apps one step further. One of the 'Windows Bridges' is something called [Project Westminster](https://blogs.windows.com/buildingapps/2015/07/06/project-westminster-in-a-nutshell/) which allows JavaScript files that are hosted on a web server to have full access to the Windows Runtime. Yes, full access to the Windows Runtime API! This means that hybrid apps with hosted JavaScript files can pretty much do anything that a native/package Windows app can do. This includes tiles, notifications, use of hardware like camera, GPS etc. #Wow! It looks a bit like this: [![](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/56/73/8032.WestminsterAppArchitecture.PNG)](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/56/73/8032.WestminsterAppArchitecture.PNG)

## How to make Hybrid apps awesome

The goal with any app is to provide a great user experience and hybrid apps are no different. A well designed hybrid app can be a superb experience and often better than most native apps. Here are 5 top tips for making hybrid apps awesome. **1) Be Responsive**. Your app should adapt to the viewport that is being used. This could be 4" or 80". Responsive web design is a must have for this. **2) Be Fast**. One of the top complaints from people around hybrid apps is that they are slow. But they don't have to be. Checkout [YSlow](http://yslow.org/) and [Google Pagespeed Insights](https://developers.google.com/speed/pagespeed/insights/) to make sure your web app is sports-car quick **3) Offline**. You might think that hybrid apps cannot work offline ... wrong, check out the [FT web app](http://app.ft.com/index_page/home). There are three things that can help you make your hybrid app work offline [App Cache Manifest](http://www.html5rocks.com/en/tutorials/appcache/beginner/), [IndexDb](http://www.w3.org/TR/IndexedDB/) and [Local Storage](http://www.w3schools.com/HTML/html5_webstorage.asp) **4) Design as an app**. UI makes a big difference. If your app looks like a website, users will think it is a website and rate it negatively. Avoid 'web' UI conventions like banner adverts, footers, on-page or complex main navigation bars. There are a bunch of UI frameworks that can help you design your HTML and CSS to be more app-like. See the resources at the bottom for links **5) Platform Integration**. At the very least, you should go to the effort of having the right tiles/icons for your app. But if you can, take it a step further with [Apache Cordova](http://cordova.apache.org/) and/or [Project Westminster for Windows](https://dev.windows.com/en-us/uwp-bridges/project-westminster)

## Resources

I've mentioned a tonne of resources in this article, here they are for convenience. [Web Hack Wednesday 'Hybrid app' episode](https://channel9.msdn.com/Shows/Web-Hack-Wednesday/Hybrid-Apps): A video that me and my chum [Martin Beeby](https://twitter.com/thebeebs) recorded about hybrid apps Murky world of hybrid apps [Slides](https://onedrive.live.com/redir?resid=999BE3B43DB18398!1796678&authkey=!AI3u07N-v89k3CU&ithint=file%2cpptx) or [Video](https://onedrive.live.com/redir?resid=999BE3B43DB18398!1796679&authkey=!ANC9pWOi0uE5hyk&ithint=video%2cmp4): The slides and a video recording from a practice run for my talk at [NDC London](http://ndc-london.com/) (called ‘[The murky world of hybrid apps](http://ndc-london.com/talk/the-murky-world-of-hybrid-web-apps/)’) on Friday 15th Jan 2016. [W3C Web App manifest](http://www.w3.org/TR/appmanifest/): A standard for defining metadata and resources for hybrid apps [ManifoldJS](http://manifoldjs.com/): Open source tool from Microsoft for creating a load of hybrid apps based on the W3C Web App Manifest [This Here Web](http://www.thishereweb.com/): A neat website by my chum Jeff Bertoft who is the guy behind ManifoldJS. [Project Westminster](https://blogs.windows.com/buildingapps/2015/07/06/project-westminster-in-a-nutshell/): A way of accessing the full Widows runtime form a hosted JavaScript file [Apache Cordova](http://cordova.apache.org/): An open-source tool for writing HTML, CSS and JavaScript and having that work with local operating system APIs. For IOS, Android, Windows and more [YSlow](http://yslow.org/): A tool for checking your website performance based on Yahoo's YSlow ruleset [Google Page Speed Insights](https://developers.google.com/speed/pagespeed/insights/): A tool for checking your website performance based on Google rule set [App Cache Manifest](http://www.html5rocks.com/en/tutorials/appcache/beginner/): A manifest which allows a HTML pages to be cached [IndexDb](http://www.w3.org/TR/IndexedDB/) and [Local Storage](http://www.w3schools.com/HTML/html5_webstorage.asp): Standard for allow web pages to store data locally [Bootstrap Ratchet](http://goratchet.com/): An app UI framework from the guys behind Bootstrap [Zurb Foundation for Apps](http://foundation.zurb.com/apps.html): An app UI framework from Zurb [Google Material Design](http://www.google.com/design/spec/material-design/introduction.html): A design framework/rule set fro Google [Ionic](http://ionicframework.com/): An app ui framework that uses Angular I hope that is useful to you. Please let me know one way or another via the comments.</div>

</div>
