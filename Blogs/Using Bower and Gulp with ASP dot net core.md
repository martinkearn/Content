---
title: Using Bower and Gulp with ASP dot net core
author: Martin Kearn
description: This article will talk you through the basics of how Bower works in the context of ASP.net 5.0 and how you can integrate it into your workflow
image: https://dummyimage.com/800x600/000/fff&text=placeholder
thumbnail: https://dummyimage.com/200x200/000/fff&text=placeholder
type: article
status: published
published: 2015/06/29 09:30:00
categories: 
  - ASP.net core
  - Bower
  - Gulp
---

# Using Bower and Gulp with ASP.net core

One of the things that may be slightly new and unclear to you when you first start looking at ASP.net 5.0 is Bower. If you've only ever done .net development inside Visual Studio, Bower is another big, new, slightly scary thing to learn.

This article will talk you through the basics of how Bower works in the context of ASP.net 5.0 and how you can integrate it into your workflow.

We'll use [Font Awesome](http://fortawesome.github.io/Font-Awesome/) as an example of a front-end package because this is a great framework that incorporates CSS and font files and I know I personally use it on almost every web project i start.

## What is Bower?

Bower is a package manager for front-end frameworks.

[![](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/56/73/3716.bower-logo.png)](http://bower.io/)

Bower is similar conceptually to NuGet, but it focused on management of front-end packages such as JQuery, Angular, Font Awesome, Bootstrap, Modernizr etc

In the current version of ASP.net (>4.6), Microsoft does a load of work to make NuGet packages from the latest Bower packages and you get everything through NuGet, but this will stop in ASP.net 5 and we'll just use Bower directly for front-end stuff and NuGet for back-end stuff.

You can find out more at [http://bower.io/](http://bower.io/).

## How to add Bower packages in ASP.net 5.0?

Before you add a Bower package, you need to work out what it is called on Bower as it is not always the name you'd expect. For example [Font Awesome](http://fortawesome.github.io/Font-Awesome/) is 'components-font-awesome' on Bower. You can find package using the Bower search page which is [http://bower.io/search/](http://bower.io/search/)

Once you have your Bower package name, open Bower.json from the root of your project and simply add the package to the existing list of dependencies. If you are using Visual Studio, you'll get intellisense to help you select the package and the version you want. Remember this is a JSON file you'll need to respect JSON syntax.

This is what the default Bower.json looks like after I've added Font Awesome:

[![](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/56/73/1122.Bower-json.PNG)](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/56/73/1122.Bower-json.PNG)

When you save Bower.json, you'll see in the Output windows that the package gets automatically added to your solution. However, this is not the end of the story, there are a few more steps before you can actually start using your front-end package.

## Using Gulp to add Bower packages to your WWWRoot

Adding a package in Bower.json only downloads your package, you now need to use [Gulp](http://gulpjs.com/) (or whichever task manager you are using, [Grunt](http://gruntjs.com/) is another popular one right now) to add the package to the WWWRoot folder in your project.

[![](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/56/73/4370.Gulp.jpg)](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/56/73/4370.Gulp.jpg)

In ASP.net 5.0, WWWRoot is a new folder that contains all your static resources such as CSS, JS, fonts and images. This folder gets deployed as-is to your web server and the idea of separating it out like that is to help you separate your front-end resources from back-end code.

By default, Gulp is used to manage the build of your project and like Bower, you edit the Gulp configuration in a file called Gulp.js. Unlike Bower, Gulp is a JavaScript file and there is a huge amount of flexibility on what you can actually do with it. Checkout the Gulp GitHub page for more details: [https://github.com/gulpjs/gulp/blob/master/docs/getting-started.md](https://github.com/gulpjs/gulp/blob/master/docs/getting-started.md).

For now, we will continue our example of adding Font Awesome. To add Font Awesome, simply add a reference to the package you installed via Bower as one of the items in the 'bower' array in Gulp.js. Notice that you can use wildcards and file extensions to define exactly what gets deployed. When you are finished, your Gulp.js will look something like this:

[![](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/56/73/6114.Gulp-js.PNG)](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/56/73/6114.Gulp-js.PNG)

 But we are not done yet! ... we now need to set how we want gulp to run.

## Controlling when Gulp executes

By default Gulp does not automatically execute, which means your Bower packages do not automatically end up in the WWWRoot folder where you can reference them.

We need to set Gulp to execute after each project build, to do this we use a new interface called the Task Runner Explorer.

Right click on Gulp.js and choose Task Runner Explorer.

Since we want to control the 'copy' task, we can right click on 'copy' and set the bindings to 'after build'. This means that the Gulp file will execute after each build. If you build your project now, you should see the font awesome files appear in your WWWRoot folder (in a folder called 'Lib').

You can now reference your files in the same way as normal in your HTML, for example, to reference the Font Awesome CSS file, you'd use something like this:

[![](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/56/73/0410.RefFontAwesome.PNG)](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/56/73/0410.RefFontAwesome.PNG)

## In Summary

Lets be honest, this process is hard work! ..... much more work that it used to be with NuGet. However this is the way the rest of the web developer world does things and aligning with the rest of the world is surely better in the long run.

You also have much more control of exactly what gets deployed to where using Bower and Gulp.

Enjoy! :)