---
title: Using App Settings in Azure Web Apps
author: Martin Kearn
description: How to use app settings in Azure web apps
image: https://dummyimage.com/800x600/000/fff&text=placeholder
thumbnail: https://dummyimage.com/200x200/000/fff&text=placeholder
type: article
status: published
published: 2015/10/09 09:30:00
categories: 
  - Azure App Service
  - ASP.net
---

# Using App Settings in Azure Web Apps

I'm on a hackathon today where we are looking at retrofitting some DevOps principles to an existing ASP.net 4.5 web application.

I learnt something about app settings which I did not know so thought I'd share it.

## Settings in ASP.net

In ASP.net, the easiest way to store application settings is in the Web.Config file under the [appSettings](https://msdn.microsoft.com/en-us/library/aa903313(v=vs.71).aspx) section. This is a key/value pair and is great for storing things like API urls, settings and other things that affect your app's behaviour.

In you code, you can easily access the settings from Web.Config via [ConfigurationManager.AppSettings](https://msdn.microsoft.com/en-us/library/system.configuration.configurationmanager.appsettings%28v=vs.110%29.aspx).

ConfigurationManager.AppSettings["keyname"];

## Settings in Azure Web App

If you host your website in an Azure Web App, there is a really neat feature that that lets you add settings at the web app level which overwrite the settings from Web.Config at runtime. You configure these in the 'application settings' menu for your web app in the Azure portal.

If a setting is absent in Azure, then the application will default back to what is in the Web.Config.

This allow you to do really useful things like use a local API for dev/test but use a production API in production without changing your code.

Hope that helps someone