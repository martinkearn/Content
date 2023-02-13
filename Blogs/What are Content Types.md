---
title: What are Content Types
author: Martin Kearn
description: An overview of Content Types in SharePoin 2007
image: https://dummyimage.com/800x600/000/fff&text=placeholder
thumbnail: https://dummyimage.com/200x200/000/fff&text=placeholder
type: article
status: published
published: 2006/03/27 09:40:00
categories: 
  - SharePoint 2007
---

# What are Content Types

<span lang="EN-GB" style="font-size: 12pt; line-height: 115%; font-family: Calibri; mso-fareast-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman'; mso-ansi-language: EN-GB; mso-ascii-theme-font: minor-latin; mso-fareast-theme-font: minor-fareast; mso-hansi-theme-font: minor-latin; mso-bidi-theme-font: minor-bidi; mso-fareast-language: EN-US; mso-bidi-language: AR-SA;"><span style="font-family: Tahoma; font-size: small;"></span></span>For me, one of the coolest new features of SharePoint Server 2007 (or MOSS) is Content Types. In this article, I’ll aim to give a brief overview of what they are and why you should be getting excited about them (especially if you are from a SharePoint 2003 background).  <span lang="EN-GB" style="font-size: 12pt; line-height: 115%; font-family: Calibri; mso-fareast-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman'; mso-ansi-language: EN-GB; mso-ascii-theme-font: minor-latin; mso-fareast-theme-font: minor-fareast; mso-hansi-theme-font: minor-latin; mso-bidi-theme-font: minor-bidi; mso-fareast-language: EN-US; mso-bidi-language: AR-SA;"><span style="font-family: Tahoma; font-size: small;"></span></span>It is worth noting that this article is focused on the implications of Content Types in a document management scenario. Content Types are used throughout MOSS and have benefits throughout, but it was too much to cover in one article, so I decided to focus on DM.<span lang="EN-GB" style="font-size: 12pt; line-height: 115%; font-family: Calibri; mso-fareast-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman'; mso-ansi-language: EN-GB; mso-ascii-theme-font: minor-latin; mso-fareast-theme-font: minor-fareast; mso-hansi-theme-font: minor-latin; mso-bidi-theme-font: minor-bidi; mso-fareast-language: EN-US; mso-bidi-language: AR-SA;"><span style="font-family: Tahoma; font-size: small;"></span></span> **What is a Content Type then?** A content type is an object that is stored within MOSS that defines several elements of a piece of content, including:

*   Document Template that the content will be based on
*   Columns that the content will have associated with it (metadata)
*   Workflows that the content will use
*   Information Management policies that apply to the content
*   Conversion types for the content

_This picture shows the site-wide admin interface for managing content types_ ![](http://static.flickr.com/87/262232384_3d55832e64_o.png) Every piece of list or library content in MOSS is created from a content type. There are a load of out-of-the-box content types like ‘Blank Document’ or ‘Announcement’ and you can create your own (without any code or customisation, it is done through the admin UI). **So how do you apply them?** Content Types are created centrally and then they can be applied to lists and libraries throughout your site. <as type="" content="" an="" administrator="" may="" decide="" that="" document="" should="" have="" you="" can="" simply="" add="" it="" to="" and="" expenses="" report="" then="" becomes="" available="" in="" the="" new="" item="" menu="" on="" your="" library="" p=""></as> _This picture shows the list-based interface for adding content types to a list or library_ ![](http://static.flickr.com/102/262232375_0d6d5c6689_o.png) **What does this mean for users?** This means that users can now create an expense report by simply choosing ‘expense report’ from the new item button on the library. Their new expense report will be created from the central content type with the document template and all the columns and workflow that are associated with the content type. _This pictures show the net result of content types; the ability for users to choose what type of content they wish to create_ ![](http://static.flickr.com/109/262232379_c16fed9cd8_o.png) The important thing to note here is that the content will inherit all of the columns and workflow that comes with the content type – even if the library does not. For example, if you have a standard document library that just has the ‘blank document’ content type associated with it (which is the default) it will not have any columns or workflows. However if you add the ‘expense report’ content type to the library, items created from that content type will have all of the columns and workflows that are required by the content type. These columns can be used in the same way as list-based columns. **Why should you get excited?** The easiest way to demonstrate this is to compare SharePoint Server 2007 against SharePoint Portal Server 2003 functionality. 

Document Templates:

*   In 2003 you needed some third party or community supported code to use anything more than the single template that is defined for each library
*   In 2007 you can choose to include multiple content types (each with their own template) within a library using simple configuration

Metadata (Columns):

*   In 2003, all items in a library have to use the same metadata columns. This restricts your library design to specific types of content rather than administrative or logical libraries structures
*   In 2007, the content in a library can be whatever type you like without compromising the metadata of that content because each content type brings it’s own set of columns

Workflow:

*   OK, so 2003 did not have workflow. But it did have Document Library Event Handlers which were used for workflow. Again these event handlers applied to the entire content of a library, not specific documents
*   In 2007, the content brings the workflow that it needs with it, regardless of where it is stored. Work flows are included as part of the contenyt type, not the library (allthough they can also be set on thr library)

Search:

*   In 2003, there was no easy way of differentiating different types of content other than relying on column values being filled out
*   In 2007, you can search on ‘where content type = x’, i.e. ‘give me all the expense reports written by Jo Blogs’

This was only a brief summary, I hope it was useful. Please comment if you need clarification, found it usefull, good but do this different etc ….
