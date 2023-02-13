---
title: What is the Business Data Catalouge
author: Martin Kearn
description: An introduction to Business Data Catalouge in SharePoint 2007
image: https://dummyimage.com/800x600/000/fff&text=placeholder
thumbnail: https://dummyimage.com/200x200/000/fff&text=placeholder
type: article
status: published
published: 2006/10/05 09:40:00
categories: 
  - SharePoint 2007
---

# What is the Business Data Catalouge

..... In short, the Business Data Catalogue (BDC) is without doubt the most powerful feature in MOSS 2007! (In my opinion)..... I know this is a rather bold statement and hopefully this article will go some way to convincing you of the same!

**So what does BDC actually do?**

The BDC is a feature of MOSS that allows you to connect your business data systems to MOSS and use data from those systems in one of 4 main presentation methods. When I say 'Business Data System' what I'm referring to there is basically anything you can get to via ADO.net (SQL, Oracle, ODBC, and OLEDB) or web services. Throughout this article, we'll use the Adventure Works sample database in SQL 2005 as our 'Business Data System'.

**BDC Webparts**

The easiest presentation method to understand is the BDC webparts. There are several webparts provided out-of-the-box with MOSS which work together to provide various views on your BDC data.

The first one is the BDC List webpart. This webpart will simply display your data in a list form with all of the filtering and sorting capabilities that you'd expect in a list view webpart. It even has a built-in search interface which allows users to perform mini searches on the contents of the list. The scenario here is that you may have a list of 'resellers', including some of the key information that you might have about those resellers such as geography, address phone etc.

The BDC List is nicely complimented by the BDC List Item webpart which, when connected (using standard MOSS webpart connections) to the BDC List will display further details for the item that is selected. The idea is that you click a 'reseller' out of the BDC List of resellers and the BDC List Item webpart displays that reseller's full profile.

![](http://static.flickr.com/112/261445553_9f79b9ae87_o.png) 

Other webparts include related list, which is useful for showing sub-lists of information. In the Adventure Works system, this can be used by having a BDC List displaying all of the product categories and the BDC Related List showing the sub-categories of the category that is selected in the BDC List webpart. Again, this works via webpart connections.

**BDC Search**

BDC applications can be 'indexed' by the standard MOSS index service. This means that users can search across the business systems amongst all of the other data in the MOSS index. Results are presented in the same way as other documents and when the users click on a result, them get sent to an out-of-the-box ASPX page that shows all of the data that is available for that item. The BDC applications are treated as normal content sources and can be managed in exactly the same way (schedules, scopes etc).

This feature is great for integrating CRM systems into SharePoint and allowing users to search across customers and partners and get phone numbers etc. This also has applications if you have products in your BDC system; users could search for the product name and return things like financial data from the BDC system amongst document-based results from elsewhere in MOSS.

**BDC Columns**

One of the new [column types](http://blogs.msdn.com/martinkearn/archive/2006/04/04/568006.aspx) in MOSS is 'Business Data'. If you select a business data column, users will be able to choose an entity from the BDC to store as the value of a column (metadata).

Once chosen, the column will then pull other fields that are related to the chosen BDC entity and display them in your document library or list as normal (read-only) columns. For example, if you have a document that you are writing for a customer, you might select the customer name for the business data column and the system will then display the phone number, address, website, key contact etc for that customer as part of the document metadata.

![](http://static.flickr.com/98/261445551_cbf85b0701_o.png) 

Having these details displayed in your list views and webparts is great, but like all columns, this data also gets stored in the document itself and if you have Office 2007 professional, you can edit and view this data directly from within office.

**BDC Profiles**

Like BDC columns, you can also create 'business data' properties in the MOSS user profile database. This means that the user profile can now be a mixture of MOSS-based, Active Directory and business data information.

The scenario here is that you might hook the BDC up to your HR system and have key fields from the HR system imported into the BDC and displayed as part of the standard user profile. This solves the "where do I store my personnel data" problem that many organisations face; it does not matter where the data is stored ... simply have BDC pull it in from whatever systems it is in today.

**What can I do with my data once I have it in MOSS?**

All of the things I have discussed so far have been about reading the data from your business system and displaying it in various formats. You can also perform actions on this data and use bits of the data as parameters to a URL.

One scenario where Actions are used is performing internet searches on customers or resellers from your BDC system. If users clicked on the 'internet search' action, the system would pass the reseller name into the URL of an internet search engine such as Windows Live Search.

![](http://static.flickr.com/121/261445552_ede8bb14d9_o.png) 

This works by admins configuring markers in the URL that are replaced by BDC properties, for example if you did want to perform an internet search, you'd configure an action that used this URL [http://beta.search.live.com/results.aspx?q={0}](http://beta.search.live.com/results.aspx?q=%7b0%7d) where {0} is replaced by the 'reseller name' of the BDC entity that is action is based on. This works with any URL, for example if (for some weird reason), Windows Live is not your favoured search engine, you could use Google as follows: [http://www.google.co.uk/search?q={0}](http://www.google.co.uk/search?q=%7b0%7d)

The same principle can also be applied to passing an address into Windows Live Local and displaying a map of your customer's office. For this, you would use this URL [http://local.live.com/default.aspx?where1={0}%20{1}&v=2](http://local.live.com/default.aspx?where1=%7b0%7d%20%7b1%7d&v=2) where {0} is the postal code and {1} is the country name of your customer.

**How do I get started?**

By far the easiest and most compelling way to get started with BDC is to install the SQL 2005 Adventure Works sample database, to do this you will need:

*   MOSS Beta2 TR Enterprise Edition (BDC is an enterprise edition only feature) installed
*   SQL 2005 installed with the Adventure Works DW database which you can get from Control Panel > Add/Remove Programs > Change SQL 2005 > Workstation Components > Sample Databases
*   The Adventure Works BDC application definition which is available here: [http://msdn2.microsoft.com/en-us/library/ms494876.aspx](http://msdn2.microsoft.com/en-us/library/ms494876.aspx)

Simply import the BDC application definition file (via Shared Services Admin interface) and away you go!

**How do I use BDC anywhere other than demo-land?**

If you want to use BDC with your own applications and systems (which hopefully, you will! J), you'll need to write an application definition file. This is basically a big XML file which sets out the metadata of the system you are connecting to. You can write it from scratch using the excellent SDK examples which are here [http://msdn2.microsoft.com/en-us/library/ms563661.aspx](http://msdn2.microsoft.com/en-us/library/ms563661.aspx).

There are also two great tools for helping you created your own definition files:

*   Simple command-line tool on CodePlex: [http://www.codeplex.com/Wiki/View.aspx?ProjectName=DBMetadataGenerator](http://www.codeplex.com/Wiki/View.aspx?ProjectName=DBMetadataGenerator)
*   BDC Metaman: [www.bdcmetaman.com](http://www.bdcmetaman.com/)

**So is BDC the "most powerful feature in MOSS 2007"?**

I think it is because when you sit back and think about all those webparts and custom applications we used to write for Sharepoint 2003, I'd say that BDC removes the need to write custom code (C#, VB etc) to achieve the same results in MOSS for most cases. In many cases, you get a much better result with features that would have taken eternity to try and code yourself (search, profiles etc). Added to that, we get all of the 'plumbing' work like security, performance, deployment etc managed for us.

On this basis, I think BDC will significantly reduce the custom code footprint on most MOSS deployments. Reducing the custom code footprint is one of the best ways of improving stability, performance and overall TCO for a MOSS deployment, which is why I feel BDC is the "most powerful feature in MOSS 2007" .... Discuss J

The free [online JavaScript beutifier](https://html-cleaner.com/js/) organizes your scripts. Use it every time before publishing codes.
