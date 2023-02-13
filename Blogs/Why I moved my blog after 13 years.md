---
title: Why I moved my blog after 13 years
author: Martin Kearn
description: I've had a blog on MSDN.Microsoft.com ever since I started at Microsoft in 2005 and I've just recently migrated to my own website for lots of reasons. This article explains why.
image: http://martink.me/images/MartinKearnProfile1.jpg
thumbnail: http://martink.me/images/MartinKearnProfile1.jpg
type: article
status: published
published: 2018/11/28 15:40:00
categories: 
  - Personal
  - GitHub
  - Writing
  - Productivity
---

# Why I moved my blog after 13 years

I started work at Microsoft in 2005. Some of you may not have even started school at that time, which makes me feel either very old or very experienced depending on my mood!

I have maintained a Wordpress blog at https://blogs.msdn.microsoft.com/martinkearn/ ever since I started at Microsoft and have published many posts there over the years covering SharePoint, Office, Windows, Azure, AI, Bots and a bunch of other stuff. 

My most popular post has always been about [configuring Kerberos for SharePoint 2007](https://blogs.msdn.microsoft.com/martinkearn/2007/04/23/configuring-kerberos-for-sharepoint-2007-part-1-base-configuration-for-sharepoint/) which I published in 2007 and has had over 225,000 views. I have no idea why, I was completely blagging it and have never really understood Kerberos to this day! 

In this article I'l explain why I decided to move my blog, how the new system works and why I think it is better.

## Giving my articles a new home

I've just finished the initial phase of a project to migrate my blog to my own website: http://martink.me/articles which is hopefully where you are reading this now.

I've migrated the past year's worth of content and will continue to migrate the historical content over the coming weeks.

Going forward all new content will be hosted on [MartinK.me](http://martink.me).

## Why?

So why did I move away from a Wordpress blog hosted on one of the worlds most well known domains (Microsoft.com)?

There are three big reasons why I moved my blog:

1. The way we consume blogs and long-form content in general has changed
2. I author on GitHub
3. I needed a playpen

### How we consume content

When I started my MSDN blog in 2005, most blogs were exposed as RSS feeds. You'd subscribe to feeds you are interested in via an RSS viewer application. When an article was published, the RSS viewer would automatically download the content to your device and alert you so you can read it.

I don't think people consume content like this any more. 

The way people subscribe to content now is via Twitter, YouTube, Facebook, LinkedIn etc or they discover content via Search. 

In recent years, I've generally published an article and then tweet'd it so that my followers hear about it. I've then relied on Google and Bing to make it available to folks that are searching for answers that my articles may contain.

A lot of the technical features that an blog system provides are now redundant which means that blog content is really nothing more than regular web content now. This aligns well with SEO guidelines from all major search engines that you should make sure your website has rich, impactful content.

I've always felt that my articles are slightly disconnected from the rest of my content which includes details of talks, presentations, contact details etc. I've wanted them to be in a single location for a while and now they are.

### I author on GitHub 

For the past few years, I've used [GitHub](https://github.com/martinkearn/Content/tree/master/Blogs) as my main authoring repository for all my blog articles. I do this for several reasons:

* I like to author in Markdown using [Typora](https://typora.io/) and GitHub is a natural place to store Markdown files
* GitHub gives me excellent version control
* GitHub lets readers submit issues and pull requests to my content, making it 'open source', which seems like a good thing to do
* GitHub has an excellent API which make accessing the content programmatically very easy

All of the content beneath http://martink.me/articles is written and maintained via my [GitHub Content Repository](https://github.com/martinkearn/Content/tree/master/Blogs). 

I have to credit my friend [Martin Beeby](https://thebeebs.co.uk/) who originally came up with the idea of using GitHub as a sort of CMS for blog content. I've not used any of his code, but have stolen his idea.

### My Serverless Playpen

Writing in Markdown is great from an authoring perspective, but at some point the article needs to end up as a HTML document which is readable in a browser

To address this, I built a serverless system for exporting content from GitHub and making it available on my website. I'm planning a much more in depth article (or series) on how I built this system, but in general it follows these steps:

1. GitHub fires a webhook on each pull request to my Content repoitory. I wrote about this in my [Processing Github Webhooks with Azure Functions](http://martink.me/articles/processing-github-webhooks-with-azure-functions) article
2. The webhook triggers an Azure Logic app which uses a combination of built-in functionality and Azure Functions to the following when content is added or modified
   1. Extract the YAML header from my Markdown file and store the data in an Azure Storage Table
   2. Extract the Markdown document body, convert to HTML and store a HTML file in Azure Storage Blobs Container
   3. Remove the above if content is deleted from GitHub
3. My website uses a repository pattern to combine the Azure Blob and Table data to give you the HTML document you are reading right now

I'll write more on this in a future article.

## In Summary

The way we consume long-form content has changed and a lot of traditional blog functionality is no longer needed.

Going forward, all my new articles will be hosted on <http://Martink.me>. I'll slowly migrate historical articles too.

The underlying system for my new GitHub-based content management system has an interesting serverless architecture which I'll write about in the future.

