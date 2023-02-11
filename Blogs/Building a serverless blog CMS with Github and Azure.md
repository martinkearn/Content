---
title: Building a blog CMS with Github and Azure Serverless
author: Martin Kearn
description: All the articles on Martink.Me are authored on GitHub. I use GitHub API, Webhooks, Azure Functions and an Azure Logic App to get them to end up on my website. I've basically created a Content Management System on GitHub. In this article I'll go through how the system is setup and how you can build your own version.
image: https://github.com/martinkearn/Content/raw/master/Blogs/Images/GitHubCMS.jpg
thumbnail: https://github.com/martinkearn/Content/raw/master/Blogs/Images/GitHubCMS_thumb.jpg
type: article
status: draft
published: 2020/01/08 19:40:00
categories: 
  - GitHub
  - Azure Functions
  - Logic Apps
---

# Building blog CMS with Github and Azure Serverless

I started writing this article in January 2019; ever since then, my blog (http://martink.me/articles) has been running on a content management system I designed myself using GitHub and Azure Serverless (Logic Apps, Functions).

I've been meaning to write about how the systems was built ever since then, but never got around to it .... until now when a colleague asked about cheap/free blog platforms.

There are many blogging platforms avaliable whch provide varying levels of CMS capability. However, I never quite managed to find one which suited my needs which are:
- Free
- Authoring in Markdown
- Open Source content (meaning people can submit issues, pull requests etc for my content if they spot mistakes - just like code)
- Integrates into my exsiting website without taking it over

The basic premise of my system is that I write articles on GitHub in Markdown. You can can see the raw content at https://github.com/martinkearn/Content/tree/master/Blogs. I use all the features of Git to manage the production of my content from both myself and anyone else who cares to contribute (commits, branhing, pull requests etc).

Every artcle has a YAML front-matter section at the start which describes the metadata for the article (Title, Description, Tags, Publication date, Images, Status etc). YAML is an easy way to author metadta which can be easily extracted later on in the process 
