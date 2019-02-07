---
title: Tune modernise and optimise your website
author: Martin Kearn
description: Web performance is very importance for lots of reasons. In this article I'll summarise why it is important and how you can fix it
image: https://dummyimage.com/800x600/000/fff&text=placeholder
thumbnail: https://dummyimage.com/200x200/000/fff&text=placeholder
type: article
status: published
published: 2015/01/29 09:30:00
categories: 
  - Web Performance
---

# Tune modernise and optimise your website

These days it is fairly easy to build and publish a website. In fact, if you are using Visual Studio, ASP.net and Azure you can get a modern, responsive site published in [less than 3 minutes](http://blogs.msdn.com/b/martinkearn/archive/2014/12/16/how-to-create-and-publish-a-website-to-azure-in-3-minutes.aspx). However that is only half of the story; the real challenge is tuning and optimising your site so that it is lightening fast, cross browser and backward compatible.

On Wednesday 4th January, my chum Bianca Furtuna and I will present a short talk at [Tech Days Online](https://msdn.microsoft.com/en-gb/dn877971.aspx) called 'Tune, Modernise and Debug Web apps with IE'. In the talk, we'll cover lots of tips and tricks on how to make you website compatible, fast and well tuned, primarily using IE F12 developer tools but also looking at other tools and techniques too.

In this article, I'll aim to summarise some of the main points and resources, but if you are interested in this stuff, I recommend you attend the free online talk live as part of [Tech Days Online](https://msdn.microsoft.com/en-gb/dn877971.aspx).

### IE F12 Developer Tools

Like most modern browsers, IE has a broad array of handy developer tools that you can use to explore and debug your site directly within the browser. You can access these tools by going to any site and hitting F12.

The IE dev tools are frequently updated and operate on a separate cadence from IE itself. This means that the tools are always up to date with the latest standard and developments.

The current version of the F12 developer tools have the following features

*   **DOM Explorer**: A handy utility that lets you view the underlying DOM of your page. Think of it like 'view source' on steroids. You can either explore the full HTML or use the selector to pick certain elements on the page and jump straight to them.
*   **Console**: The console is great to executing and inspecting JavaScript that runs within the site
*   **Debugger**: You can add breakpoints, watches and locals to any JavaScript that the site has loaded. You can choose to exclude specific files that you do not control (JQuery etc)
*   **Network**: The network tool lets you visualise the full network trace HTTP requests for the current page complete with timings and data sizes
*   **UI Responsiveness**: This tools give you very low level detail on the CPU and visual throughput required to load your page. You can see how hardware resources are split amongst Scripting, GC, Styling, Rendering and Image Decoding
*   **Profiler**: The profile lets you profile the JavaScript in your page so you can see which functions are taking the most time to execute
*   **Memory**: The memory tool gives you deep insight into how your page is using device memory. You can take multiple snap-shots and compare them
*   **Emulation**: The emulation tool lets you emulate different document modes, user agent strings and device types as well as orientations and GPS location.

### IE Compatibility & Feature Detection

There is a big problem at the moment where websites look for specific versions of browsers and/or operating systems and serve up targeted content. The origins of this practice goes back several years to a time when the major browsers all behaved differently so you'd have to implement specific code for specific browser versions.

This JavaScript code is an example of how developers would do this sort of thing:

`if (navigator.userAgent.indexOf(‘MSIE’) > 0)  
{  
//serve up an old version of your site  
//make your IE users sad  
//give IE a bad reputation  
}  
`

Fortunately, there is a much better way to do this sort of thing and it is called 'feature detection'. Feature detection involves checking if a specific feature exists; if it does use it, if it does not use an alternative.

[Modernizr](http://modernizr.com/) is probably the standard and most common way to do feature detection. Modernizr is a JavaScript library that allows very easy feature detection in either JavaScript or CSS.

This is an example of Modernizr detecting SVG support and falling back to PNG if SVG's are not supported

```
<img id="logo" src="http://blogs.msdn.com/Content/logo.svg" /> 

<script>  
//if SVG files are not supported, use a PNG instead  
if (!Modernizr.svg)  
{  
document.getElementById('logo').src = '/Content/logo.png';  
}  
</script>
```

Modernizr also works in CSS. The framework adds a set of CSS classes to the HTML document indicating support or non-support of features. If a feature is not support you'll find a class with 'no-' as a prefix. For example 'no-opacity' indicates that opacity is not supported.

 ```
 <style>  
/*set the opacity if we can*/  
.btn-lg {  
opacity: 0.5;  
}  

/*if opacity is not supported, set the background colour to a light shade of blue*/  
.no-opacity .btn-lg {  
background-color: lightblue;  
border-color: lightblue;  
}  
</style>  

<div class="btn-lg"/>
```

#### Polyfills

A common technique for handling unsupported standard is to use Polyfills. A polyfills is a piece of JavaScript that implements a modern feature on older browsers. There are many polyfills available for just about every modern HTML standard, you can see a fairly comprehensive list here: [https](https://github.com/Modernizr/Modernizr/wiki/HTML5-Cross-browser-Polyfills)[://](https://github.com/Modernizr/Modernizr/wiki/HTML5-Cross-browser-Polyfills)[github.com/Modernizr/Modernizr/wiki/HTML5-Cross-browser-Polyfills](https://github.com/Modernizr/Modernizr/wiki/HTML5-Cross-browser-Polyfills).

Polyfills are commonly used with Modernizr to implement alternative techniques if features are not supported. 

#### CanIUse.com

Another great tool for helping you understand which features are supported (or not) in your target browsers is [CanIUse.com](http://caniuse.com/).

CanIUse.com lists all current standard across HTML, CSS and JavaScript and tells you the level of support each major browser / browser version has. For example I can see that [FormValidation](http://caniuse.com/#feat=form-validation) is well supported across all major browser apart form IOS Safari and Opera Mini.

### Modern.IE

If you are targeting any version of IE, then [Modern.IE](https://www.modern.ie/en-gb) is an invaluable resource. Modern.IE is a Microsoft operated website which has two main goals:

1.  <div>Providing tools for testing and optimising your site with IE</div>

2.  <div>Giving you detailed information on the status of IE in terms of standards support. This is similar to [CanIUse.com](http://caniuse.com/) but more detailed and focussed on IE specifically.</div>

 The tools section of Modern.IE contains some killer tools:

*   **Virtual machines**: Spin-up a virtual machine with all the common Windows / IE combinations going all the way back to Windows XP and IE6\. Invaluable if you need to support older versions of IE
*   **Site scan**: Scams your site for common coding errors and use of IE specific features such as live tiles
*   **Remote IE**: An Azure-based virtual machine which enables you to test compatibility on Windows 10 and the very latest version of IE
*   **Compatibility Report**: A report that highlights areas where your site has code that is no longer supported by IE
*   **Browser screen shots**: A tool which will take a screen shots of your site on all of the major browser including Firefox, Safari on IOS, Android browsers and more.

### Performance Optimisation

Performance is a really big deal and can make a massive difference to the usability of your site and even your search engine optimisation. The vast majority of websites are missing a few key tricks that will make a huge impact on how quickly the site renders on a range of devices.

Most performance gains can be made on the front end of a site (i.e. what happens after the response has been received by the server). There are certainly things you can do on the server side, but if you are time constrained, the font-end is where you'll make the biggest impact on your site's performance.

#### Performance Optimisation Tools & Resources

There are lots of great tools that will help you get insight to areas where your site can be improved. The 'site scan' tool in [Modern.IE](https://www.modern.ie/en-gb) is a great place to look for IE-specific tips. There are two other key tools in this area:

*   [YSlow](http://yslow.org/): This is a tool based on Yahoo's rule set for high performance websites. It is available as a browser plug-in for most of the main browsers apart from IE. My personal preference is the Google Chrome plug-in
*   [Google Page Speed Insights](https://developers.google.com/speed/pagespeed/insights/). This is a similar tool to YSlow in that it checks your site against Google set of rules for high speed websites. Page Speed insights is available either [online](https://developers.google.com/speed/pagespeed/insights/) or via the Google Chrome browser F12 developer tools

<div>Both of the tools above will give you suggestions on how your site can be optimised as well as examples on how you might implement the suggestions, but in my experience it boils down to three main principles:</div>

1.  Reduce the number of requests
2.  Reduce the size of each request
3.  Bring the data closer to the users

<div>There are several easy things you can do to reduce the number of requests:</div>

*   **Bundle your CSS and JavaScript files**. This is the concept of combining multiple files into one, thus reducing the amount of requests that are needed. If you are using ASP.net there is built in functionality for this or you can use the [Visual Studio Web Essentials](http://vswebessentials.com/) tool kit
*   **Add expiration headers to your static files**. Expiration headers enable browser caching which means that so long as the content is cached in the browser, it will not be re-requested. If you are using ASP.net you can enable this by following this ['Cache busting in ASP.net'](http://madskristensen.net/post/cache-busting-in-aspnet) article by Mads Kristensen.
*   **Move all of your JavaScript to the bottom of the page or use the async/defer attributes**. This will allow the page to render before the JavaScript is fully parsed, thus giving the impression of better performance.
*   **Reduce images requests with sprites and inline images**. CSS sprites are a great way to have a single image contain multiple graphics which are positioned via CSS. With newer browsers, it is also possible to embed the bytes of an image directly into the CSS (as base 64), thus removing the need to separately request an image at all. If you are using ASP.net the [Visual Studio Web Essentials](http://vswebessentials.com/) tool kit has quick ways to do both sprites and embedded images

<div>Once you have optimised the number of requests needed to load your page, you can look to reduce the size of each request. There are lots of ways to do this but common techniques include:</div>

*   **Compress text based files with GZip**. All static, text based files can be compressed with GZip. If you are using ASP.net, you can do this by enabling ['doStaticCompression' in your Web.Config](https://msdn.microsoft.com/en-us/library/ms525654(v=vs.90).aspx).
*   **Minify your CSS and JavaScript files**. This is the concept of removing all the whitespace from your CSS and JS files, thus making them smaller. If you are using ASP.net there is built in bundling functionality for this or you can use the [Visual Studio Web Essentials](http://vswebessentials.com/) tool kit
*   **Use fonts instead of images**: Most sites make use of icons such as social media icons or icons for buttons etc. Each of those images is a new request and an extra bit of data that must be downloaded. The same capability can be implemented in a more lightweight and generally better quality way using fonts instead. [Font Awesome](http://fortawesome.github.io/Font-Awesome/) and [GlyphIcons](http://glyphicons.com/) are both superb resources for font-based icons.
*   **Put your images on a diet**. If you must use images, make sure you loosely compress them. There are several tools for doing this including [Yahoo Smush.It](http://www.smushit.com/) and [Kraken.io](https://kraken.io/). However if you are using ASP.net, the [Visual Studio Web Essentials](http://vswebessentials.com/) tool kit has a great feature called 'optimise images' which does this for you

For those of you lucky enough to be using ASP.net, I highly recommend the [Performance Optimize Your ASP.NET Web App](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/DEV-B418#fbid=) talk from Mads Kristensen at TechEd North America 2014\. Mads is one of the key people behind [Visual Studio Web Essentials](http://vswebessentials.com/) and this talk is jam packed full of tips and trick for squeezing every last drop of performance form your ASP.net site.

#### Bring the data closer to the users

The final technique for getting super speed performance on your website is to bring the data closer to the users. Depending on the target geography of your site, this may be as simple as making sure you are hosting your site in the best region for your users. With 16 data centres world-wide, hosting your site on [Azure websites](http://azure.microsoft.com/en-gb/services/websites/) is a great way to do this.

If your site targets a multi region or global user base, then you can use Content Delivery Networks (CDNs) to distribute static content around the globe so that users can download from their nearest data centre. [Azure Storage](http://azure.microsoft.com/en-gb/services/storage/) is a great, cost effective way to do this.

### In Summary

There are plenty of tools out there to help you make sure your website is tuned, lightening fast and compatible with as many browsers as possible, so stop reading this and get on with it! :)

[The table styler app](https://divtable.com/table-styler/) is great to layout website sections on the page!