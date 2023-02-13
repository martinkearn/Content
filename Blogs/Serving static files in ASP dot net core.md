---
title: Serving static files in ASP dot net core
author: Martin Kearn
description: The way you store and serve static files has changed in asp.net core compared to asp.net 4.6. In this article, we'll investigate what has changed
image: https://dummyimage.com/800x600/000/fff&text=placeholder
thumbnail: https://dummyimage.com/200x200/000/fff&text=placeholder
type: article
status: published
published: 2015/06/22 09:40:00
categories: 
  - ASP.net core
---

# Serving static files in ASP dot net core

This is just a little something that took me a while to figure out with ASP.net 5.0 and is a key example of some of the biggest changes in ASP.net 5.0 in terms of these three areas:

*   WWWroot
*   Package management
*   Configuration (what we used to put in web.config)

I'm working on some [hosted apps for Windows 10](http://blogs.windows.com/buildingapps/2015/03/02/a-first-look-at-the-windows-10-universal-app-platform/), which requires me to add a static JSON file ([manifest.json](http://www.w3.org/2008/webapps/manifest/)) to the root of my web application. It seems simple enough, however there are three key things you need to know about how ASP.net 5.0 works which are different to previous versions in order to serve static json files. Kudos to the 'Kind of code' Peter Daukintis for helping work this out! :)

### 1) WWWroot is where you put static files

In ASP.net 5.0, you'll get a 'wwwroot' folder at the root of your project. This folder gets deployed as-is to the web server and the intention is that any static files (CSS, scripts, fonts, images, xml or json) all go in this folder. This makes your project structure much cleaner as you know that anything outside this folder is pure ASP.net code (controllers, classes, views etc). So in order for my manifest.json file to get deployed to my website root, i need to place it at the root of the 'wwwroot' folder in my project, not the root of my overall project as I'd have done in older versions of ASP.net.

[![](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/56/73/5584.wwwroot.PNG)](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/56/73/5584.wwwroot.PNG)

### 2) Make sure you have the StaticFile package

In ASP.net 5.0, depending on which template you use to start with, you'll find that by default there is much less included out of the box and additional features must be enabled by adding packages to the solution. For some project templates, even the ability to serve static files is excluded. To add packages, you need to edit the 'project.json' file at the root of your project. In this file you'll find an array of dependencies, you need to make sure you have 'Microsoft.AspNet.StaticFiles' listed in there. If the package is listed in project.json, Visual Studio will take care of all the Nuget import stuff so you do not need to worry about it (same applies if you remove or edit entries to this file). At the time of writing, the latest version for static files was "Microsoft.AspNet.StaticFiles": "1.0.0-beta4", 

[![](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/56/73/8054.staticfiles.PNG)](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/56/73/8054.staticfiles.PNG)

### 3) Configure Static Files in Startup.cs

You've always had to change your [web.config or configure your web server to enable static JSON files in ASP.net](http://stackoverflow.com/questions/8158193/how-to-allow-download-of-json-file-with-a). However, web.config is not the way you do configuration in ASP.net 5.0\. Instead you configure your application in code in a file called Startup.cs which is an [OWIN start-up class](http://www.asp.net/aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana). This is file that gets executed when the application starts-up. If you chose anything other than 'Empty' for your template when you created the project you'll see some default stuff in here. This is where you need to add JSON as a supported mime-type. However, by default, ASP.net will only serve known MIME types and JSON is not one of them so you have to jump through a few hoops. Firstly you have to add a custom content type provider class that maps .json. For example (you can put this anywhere but i just place it in startup.cs)

```
public class JsonContentTypeProvider : FileExtensionContentTypeProvider
{
 public JsonContentTypeProvider()
 {
 Mappings.Add(".json", "application/json");
 }
}
```

Once you have JsonContentTypeProvider you need to add a line of code to start-up.cs that adds static files to the request pipeline. You may already have something like app.useStaticFiles(); but to include your newly created custom content provider for JSON, you need to edit it to look like this

```
app.UseStaticFiles(new StaticFileOptions()
{
 ContentTypeProvider = new JsonContentTypeProvider()
});
```

### In Summary

You should now be able to serve static JSON files from your ASP.net 5.0 application. You've also learnt about three important changes to ASP.net 5.0 along the way around Web.Config and the new startup.cs, wwwroot and project.json.
