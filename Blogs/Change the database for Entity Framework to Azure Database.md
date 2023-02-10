---
title: Change the database for Entity Framework to Azure Database
author: Martin Kearn
description: How to change the database for Entity Framework to Azure Database
image: https://dummyimage.com/800x600/000/fff&text=placeholder
thumbnail: https://dummyimage.com/200x200/000/fff&text=placeholder
type: article
status: published
published: 2015/03/09 09:30:00
categories: 
  - Entity Framework
  - SQL
---

# Change the database for Entity Framework to Azure Database

I spent a little time trying to change the database that Entity Framework uses so that I could developer against an Azure database rather than the local default one. It is fairly simple, but took me a while to figure out so I thought I'd capture it....

By default, Visual Studio creates an MDF file in the 'app_data' folder in your project; this is where Entity Framework stores the data that gets generated from your application. The MDF file will be named after the data context you created when you first setup your project or controller, if you followed all the default suggestions, it will be something like 'WebApplication1Context-[datestamp].mdf'. If you double click this file (it is hidden so you'll have to 'show all files'), you'll be able to explore the database.

I quite often need to change the location of the database to something that is not on my machine such as an Azure Database. This is very easy to do, simply open your Web.Config, under Configuration > ConnectionStrings you'll find an entry that is named after your data context ('WebApplication1Context' by default) and change the 'connectionString' value of this to be whatever you need.

I personally like to use an Azure database because I know I can get to it from anywhere.

### How to use an Azure database for local development

It is very easy to get the connection string for an Azure database, follow these steps:

1.  Login to [https://manage.windowsazure.com](https://manage.windowsazure.com)
2.  Go to 'SQL Databases' and open the database you want to access
3.  Go to the 'Dashboard' tab
4.  Click 'Show connection strings'
5.  Copy the 'ADO.net' one into your Web.Config and make sure you set the password where it says '{your_password_here}'

If you are using an azure database, you'll also need to configure azure to allow access from your PC, to do this, follow these steps:

1.  Click the 'Firewall rules' link in the connection strings dialog from the previous step
2.  Click 'Add to the allowed ip addresses'
3.  Click 'Save'
4.  Note that if your client IP address changes, you'll need to do this step again to allow the new IP address

### Why do this?

There are lots of reason why you might need to do this, here are just a few:

*   There is a team of you all working against the same database and it is important that you all have the same data
*   You roam between multiple machines and want to use the same data on all of them
*   You plan to publish to Azure so want to use it from the start

<div>I hope that saves someone 20 minutes! :)</div>
