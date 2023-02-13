---
title: What is new with Sharepoint Columns
author: Martin Kearn
description: What is new with columns in SharePoint 2007
image: https://dummyimage.com/800x600/000/fff&text=placeholder
thumbnail: https://dummyimage.com/200x200/000/fff&text=placeholder
type: article
status: published
published: 2006/04/04 09:40:00
categories: 
  - SharePoint 2007
---

# What is new with Sharepoint Columns
<span style="font-family: Tahoma; font-size: small;">Columns have always been one of the most powerful features in SharePoint as they allow you to slice and dice you data into different views that provide very compelling information to the end users. In MOSS 2007, columns have received a facelift and in this article I’ll aim to tell you what is new and how columns have been improved.</span>

<span style="font-family: Tahoma; font-size: small;">For the sake of clarity, when I’m referring to a column, I am talking about the columns that you would add to a list or library and use to define metadata for a document or list items that are contained within. I’ll also refer to libraries and documents exclusively throughout this article, but that also include lists and list items (just too lazy to write ‘library/list’ or ‘list item/document’ every time!).</span>

<span style="font-family: Tahoma; font-size: small;"><strong><span style="font-size: medium;">What has not changed?
</span></strong>Columns in MOSS 2007 still adopt the same basic principles as in the 2003 suite. A column is still part of a library and when you add a document to that library you have to provide values for the columns. The data you provide acts as the metadata for that document and can be used for many things including:</span>
<ul>
 	<li><span style="font-family: Tahoma; font-size: small;">Views on the library</span></li>
 	<li><span style="font-family: Tahoma; font-size: small;">Webparts</span></li>
 	<li><span style="font-family: Tahoma; font-size: small;">Search queries</span></li>
 	<li><span style="font-family: Tahoma; font-size: small;">Workflow</span></li>
</ul>
<span style="font-family: Tahoma; font-size: small;">Columns still have a variety of types. All of the old types are still there (single line of text, number, date etc) but a few new ones have been added (which I’ll discuss later)
Columns are still the main data source for webparts and views.</span>

<span style="font-family: Tahoma; font-size: small;"><span style="font-size: medium;"><strong>What were the main issues with 2003 columns?</strong>
</span>In the 2003 suite, columns were very useful and were enthusiastically used to build applications and tools based on SharePoint data. However, in my opinion they had several key draw-backs, which were:</span>

<span style="font-family: Tahoma; font-size: small;"><strong>1. No central management</strong>
Columns we not centrally managed across the portal, instead they were managed at the library level. This meant that you never had central control over your corporate metadata taxonomy which is an issue for most organisations, especially those that are concerned with compliance issues such as FOI or SOX.</span>

<span style="font-family: Tahoma; font-size: small;">For example, if you had a company-wide column called ‘Offices’ which you applied to every document library and you suddenly find yourself with new offices; there was no real way of retrospectively adding the new column values to the existing libraries. In practice if you needed to update all of your columns, you either had to manually visit every document library, or write some kind of script that used the object model to update the libraries. Both methods were time consuming and expensive.</span>

<span style="font-family: Tahoma; font-size: small;"><strong>2. Limited range of types</strong>
In 2003, you only had a limited range of types for each column. The types on offer did the job for most scenarios, but not all. You’ll see later in this article how several new types are now available.</span>

<span style="font-family: Tahoma; font-size: small;"><strong>3. Only defined at the list/library level
</strong>In 2003, the columns were defined at the library level which meant that every item in that library had to have the same metadata. This dictated the whole approach around how the lists and libraries were designed.</span>

<span style="font-family: Tahoma; font-size: small;">If you had a document that needed a different set of metadata, you had to have a whole new library just for that document, which clearly does not make sense.
Because of this restrictive approach, it became impossible to build a library structure based on corporate taxonomies; instead they were based on document types.</span>

<span style="font-family: Tahoma; font-size: small;"><strong><span style="font-size: medium;">So what is new?</span>
</strong>OK, so here is the interesting bit…. Unsurprisingly, my take on what is new directly maps to the weak areas in the 2003 platform, here goes:</span>

<span style="font-family: Tahoma; font-size: small;"><strong>1. Column templates</strong>: portal-wide management of column definitions
In MOSS 2007, when you add a column to a library, you now have two choices:</span>
<ul>
 	<li><span style="font-family: Tahoma; font-size: small;">Create a new Colum directly on that library (2003 style)</span></li>
 	<li><span style="font-family: Tahoma; font-size: small;">Add a column template</span></li>
</ul>
<span style="font-family: Tahoma; font-size: small;">This picture shows the interface for adding a column to a library</span>

<img width="565" height="352" style="width: 565px; height: 352px;" src="http://static.flickr.com/122/262230365_92009b0289_o.png" mce_src="http://static.flickr.com/122/262230365_92009b0289_o.png" />

<span style="font-family: Tahoma; font-size: small;">Column templates are centrally controlled columns that are created by Admins at the portal level. Site owners can simply add a column template to his library and it maintains a connection back to the central column template. This means that when the column template is changed in any way, it also changes on any libraries that have used it.</span>

This addresses the scenario around “what happens when I get extra offices”.

<span style="font-family: Tahoma; font-size: small;"><strong>2. New Types
</strong>In MOSS 2007, there are several interesting new types in additional to the 2003 types. The new types are:</span>
<ul>
 	<li><span style="font-family: Tahoma; font-size: small;">Business Data: Allows you to include data from the Business Data Catalogue (BDC). The BDC is a whole new feature which I’ll probably blog about some time soon. It is basically a way of including business data from external applications in your portal.</span></li>
 	<li><span style="font-family: Tahoma; font-size: small;">Audience Targeting: A way of choosing a specific audience, distribution list or sharepoint group to target the document to. Adding this column gives you an audience picker control that you can use to browse the audience, distribution list or sharepoint groups and choose which ones to target the item to.</span></li>
</ul>
<span style="font-family: Tahoma; font-size: small;">This picture shows the different types that are available</span>

<img width="264" height="295" style="width: 264px; height: 295px;" src="http://static.flickr.com/108/262230368_0a7b01118c_o.png" mce_src="http://static.flickr.com/108/262230368_0a7b01118c_o.png" />

<span style="font-family: Tahoma; font-size: small;"><strong>3. Content Types</strong>
Content Types are a whole new feature that allows the behaviour of a piece of content (including templates, columns workflow etc) to be centrally defined and applied to any library in the portal.
This means that libraries can now contain multiple content types and therefore the columns are defined by the content type itself, not the library in which it is placed. </span>

<span style="font-family: Tahoma; font-size: small;">For example, you can have you expense reports in the same library as your reports and they can each have different sets of columns that are driven by the Content Type, not the library (though you can still have library based columns).</span>

<span style="font-family: Tahoma; font-size: small;">This picture shows how columns can be included in Content Types</span>

<img width="514" height="602" style="width: 514px; height: 602px;" src="http://static.flickr.com/87/262230367_abdede77ec_o.png" mce_src="http://static.flickr.com/87/262230367_abdede77ec_o.png" />

<span style="font-family: Tahoma; font-size: small;">This is a huge step forward because it means that we can now design library structures based on organisational rules rather than content-driven rules. I’ve written a whole article explaining Content Types in general, <a href="http://blogs.msdn.com/martinkearn/archive/2006/03/27/561809.aspx">check it out here</a>.</span>

<span style="font-family: Tahoma; font-size: small;"><span style="font-size: medium;"><strong>But wasn’t all this in SharePoint 2001?</strong></span>
A few people have commented on my last post about Content Types and have drawn the obvious comparisons about this stuff in MOSS 2007 going back to some of the functionality on SharePoint 2001. These comments made me chuckle and it is true that we have come ‘full circle’ (thanks for the comments by the way ;) ). </span>

<span style="font-family: Tahoma; font-size: small;">I think the reason that much of this stuff was removed in SharePoint 2003 was because of the changes in the underlying architecture (WebStore to SQL) and there simply not being enough time to develop these features. </span>

<span style="font-family: Tahoma; font-size: small;">It is good for all of us that they are now here in MOSS 2007!</span>

<span style="font-family: Tahoma; font-size: small;">Cheers</span>

<span style="font-family: Tahoma; font-size: small;">PS: I am never quite sure where my audience is at with these articles. If you do not understand anything, please do not hesitate to comment on the blog or ping me at </span><a href="mailto:Martin.Kearn@Microsoft.com"><span style="font-family: Tahoma; font-size: small;">Martin.Kearn@Microsoft.com</span></a><span style="font-family: Tahoma; font-size: small;"> and I’ll do my best to answer your question
