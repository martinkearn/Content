---
title: Why I moved my blog after 13 years
author: Martin Kearn
description: I've had a blog on MSDN.Microsoft.com ever since I started at Microsoft in 2005 and I've just recently migrated to my own website for lots of reasons
image: http://martink.me/images/MartinKearnProfile1.jpg
thumbnail: http://martink.me/images/MartinKearnProfile1.jpg
type: article
status: draft
published: 2018/11/27 09:30:00
categories: 
  - Personal
  - GitHub
  - Writing
---

# Why I moved my blog after 13 years

I started work at Microsoft in 2005, some of you may not have even started school at that time, which makes me feel either very old or very experienced depending on my mood!

I've maintained a Wordpress blog at https://blogs.msdn.microsoft.com/martinkearn/ ever since I started at Microsoft and have published many posts there over the years covering SharePoint, Office, Windows, Azure, AI, Bots and a bunch of other stuff. 

My most popular post has always been [Configuring Kerberos for SharePoint 2007: Part 1 – Base Configuration for SharePoint](https://blogs.msdn.microsoft.com/martinkearn/2007/04/23/configuring-kerberos-for-sharepoint-2007-part-1-base-configuration-for-sharepoint/) which I published in 2007 and has had over 225,000 views. I have no idea why, I was completely blagging it and have never really understood Kerberos to this day! 

## Giving my articles a new home

I've just finished the initial phase of a project to migrate my blog to my own website: http://martink.me/articles which is hopefully where you are reading this now.

I've migrated the past year's worth of content and will continue to migrate the historical content over the coming weeks.

## Why?

This brings me to the main point of the article; to explain why I made this decision and to provide a reference for a series of articles I may write regarding how my new blog system works.

There are three big reasons why I moved my blog

1. The way we consume blogs and long-form content in general has changed
2. I author on GitHub
3. A playpen

### How we consume content

When I started my MSDN blog in 2005, most blogs were exposed as RSS feeds. You'd subscribe to feeds you are interested in via an RSS viewer application. When an article was published, the RSS viewer would automatically download the content to your device and alert you so you can read it.

I don't think people consume content like this any more. 

The way people 'subscribe' to content now is via Twitter, YouTube, Facebook, LinkedIn etc or they discover content via Search. 

In recent years, I've generally published an article and then tweet'd it so that my followers know it is there. I've then relied on Google and Bing to make it available to folks that are searching for answers that my articles may contain.

A lot of the technical features that an actual blog system provides are now redundant which means that blog content is really nothing more than regular web content now. This aligns well with SEO guidelines from all major search engines that you should make sure your website has rich, impactful content.

I've always felt that my articles are slightly disconnected from the rest of my content which includes details of talks, presentations, contact details etc. I've wanted them to be in a single location for a while and now they are.

### Why I author on GitHub 

For the past few years, I've used [GitHub](https://github.com/martinkearn/Content/tree/master/Blogs) as my main authoring repository for all my blog articles. I do this for several reasons:

* I like to author in Markdown using [Typora](https://typora.io/) and GitHub is a natural place to store Markdown files
* GitHub gives me excellent version control
* GitHub lets readers submit issues and pull requests to my content, making it 'open source', which seems like a good thing to do
* GitHub has an excellent API which make accessing the content very easy

All of the content beneath http://martink.me/articles is written and maintained via my [GitHub Content Repository](https://github.com/martinkearn/Content/tree/master/Blogs). 

### My Serverless Playpen

