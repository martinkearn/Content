---
title: Configuring Kerberos for SharePoint 2007 - Excel Services and SQL Analysis Services
author: Martin Kearn
description: Configuring Kerberos for SharePoint 2007 - Excel Services and SQL Analysis Services
image: https://dummyimage.com/800x600/000/fff&text=placeholder
thumbnail: https://dummyimage.com/200x200/000/fff&text=placeholder
type: article
status: published
published: 2007/04/27 09:40:00
categories: 
  - SharePoint 2007
---

.This is the second of my several-part series on how to configure Kerberos for MOSS 2007\. In the [first article](http://blogs.msdn.com/martinkearn/archive/2007/04/23/configuring-kerberos-for-sharepoint-2007-part-1-base-configuration-for-sharepoint.aspx), I outlined the steps that are required in order to get Kerberos working for a basic MOSS installation. In this article I am going to address one of the most common scenarios that actually requires Kerberos in order to work; that is using Excel Services to display data from a SQL Analysis Services Cube (or a normal SQL database) via SharePoint.

**Why does this scenario require Kerberos?**

As outlined in [part 1 of this series](http://blogs.msdn.com/martinkearn/archive/2007/04/23/configuring-kerberos-for-sharepoint-2007-part-1-base-configuration-for-sharepoint.aspx), Kerberos is required whenever something in SharePoint needs to access another service on the user's behalf (i.e. "double hop"), this scenario does exactly that. The spreadsheets that are hosted by Excel Services will need to access a SQL server in order to get the information that is required. This is a double-hop and unless Kerberos is enabled, you will get errors in your Excel Services Webparts.

**Step 1: Configure Kerberos for MOSS**

The first step in configuring this scenario is to get your base MOSS configuration sorted. Follow the steps in my first article to do this ([Configuring Kerberos for SharePoint 2007: Part 1, Base Configuration for SharePoint](http://blogs.msdn.com/martinkearn/archive/2007/04/23/configuring-kerberos-for-sharepoint-2007-part-1-base-configuration-for-sharepoint.aspx)).

**Step 2: Configure Permissions in SQL AS**

This section specifically relates to using SQL Analysis Services, but similar steps will be required for normal SQL or Reporting Services.

In order that users can access your SQL AS cube, you need to configure permissions inside SQL Management Studio. Follow these steps:

*   Open SQL Management Studio
*   Connect to Analysis Services
*   Right-click on the server level and go to properties
*   Go to the Security tab
*   Give the relevant users access. If you want everyone to have access, add 'authenticated users'

This means that user have access to actually read the data from the AS cube

**Step 3: Configure Excel Services for Delegation**

One of the key things that people get caught out with on when attempting this scenario is configuring Excel Services to use Delegation (i.e. to use Kerberos rather than NTLM). This is a setting that you can only set by using STSADM.exe; you cannot set it through the SharePoint Administration pages and it is not well documented. The discussion thread outlines this step: [http://forums.microsoft.com/TechNet/ShowPost.aspx?PostID=1224539&SiteID=17](http://forums.microsoft.com/TechNet/ShowPost.aspx?PostID=1224539&SiteID=17)

Follow these steps:

*   On your SharePoint server, open a command prompt and navigate to c:\program files\common files\microsoft shared\web server extensions\12\bin
*   <div>Run the following command where %SSPNAME% is the name of your Shared Service Provider:</div>

    *   stsadm.exe -o set-ecssecurity -ssp %SSPNAME% -accessmodel delegation
*   <div>Now run the following command:</div>

    *   stsadm.exe -o execadmsvcjobs
*   Now perform and IISRESET

**Step 4: Create your Data Connection file**

Now Excel Services has been configured, you need to make sure that the data connection has the right settings for Kerberos. Typically in this scenario, a data connection file will be created and stored in a SharePoint data connections library. This ensures that you only have to set the data connection up once and use it many times.

There are several key settings that must be in the data connection file in order for Kerberos to work, these include using the FQDN of the SQL server and adding SSPI=Kerberos to the connection string. Follow these steps:

*   Open Excel 2007 (Client)
*   Go to the 'Data' ribbon
*   In the 'Get external data' area click on 'From Other Sources' and choose 'From Analysis Services'
*   Enter the FQDN of your SQL server here (i.e. server.domain.local, not just server), leave the default of 'Use Windows Authentication' and click Next
*   Choose the database and cube that you wish to connect to and click Next. Click Finish on the following screen.
*   Choose to show a pivot table (this is not relevant and will not be used at this stage) and click OK
*   Once the pivot table is displayed, it is a good idea to test it out to make sure you got the right settings
*   Go to the 'Data' ribbon and click 'Properties'
*   Go to the 'Definition' tab of the connection properties dialog
*   Add ';SSPI=Kerberos' (without the ') at the end of the connection string (after MDX Missing Member Mode=Error)
*   Now Export your data connection to SharePoint by clicking 'Export Connection File'
*   Enter the full URL to the Data connection library that you wish to save you data connection to and click 'Save'.
*   <div>You may now close Excel and disregard the spreadsheet (you have got the data connection in SharePoint which is the bit you need)</div>

**Step 5: Configure your site**

Now you have created the data connection, you can go ahead and configure your site.

Generally one of the first things to do is to add an 'SQL Server 2005 Analysis Services Filter' webpart which uses the data connection to provide filters to other webparts on your site. This is one of the first places to test Kerberos. When you add the SQL AS Webpart, you will first need to choose the data connection. Upon doing this the Dimension drop-down should populate with dimensions from SQL. If this works then Kerberos is working! J

**Useful References**

These articles were useful for me when trying to configure this:

[Displaying Workbook using external data with periodic refresh in EWA](http://forums.microsoft.com/TechNet/ShowPost.aspx?PostID=1224539&SiteID=17http://forums.microsoft.com/TechNet/ShowPost.aspx?PostID=1224539&SiteID=17) ... this is a good discussion thread on TechNet that talks about some of the Excel Services settings that are required.

[How to configure SQL Server 2005 Analysis Services to use Kerberos authentication](http://support.microsoft.com/kb/917409) ... a great MSDN article that covers some of the AS and connection string parameters.

[How to use Kerberos authentication in SQL Server](http://support.microsoft.com/kb/319723/) ... a very comprehensive article that covers configuring Kerberos for SQL
