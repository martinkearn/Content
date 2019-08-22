---
title: Using Microsoft Graph in Logic Apps
author: Martin Kearn
description: The Microsoft Graph is the way you programmatically access data store din Azure Active Directory, Office 365 and a bunch of other Microsoft cloud services. Accessing the data in a Logic App is a very powerful way to use this rich API with no code. This article tells you how
image: https://github.com/martinkearn/Content/raw/master/Blogs/Images/microsoft-graph.png
thumbnail: https://github.com/martinkearn/Content/raw/master/Blogs/Images/microsoft-graph.png
type: article
status: draft
published: 2019/08/22 20:00:00
categories: 
  - Logic Apps
  - Microsoft Graph
  - Azure Active Directory
  - Office 365
  - Serverless
---

# Using Microsoft Graph in Logic Apps

I recently spent some time trying to perform operations in Azure Active Directory and Office 365 via Azure Logic Apps. It was clear that [Microsoft Graph](https://developer.microsoft.com/en-us/graph/) is the way this is done, but it can take a while to get setup and accessible via a Logic App. 

I made mistakes, learnt along the way and share them here for your time-saving enjoyment. This article will give you the background and steps/tips on how to get setup.

I'd like to credit [the 'Logic Apps Active Directory OAuth Authentication for Microsoft Graph' article by Ludvig Falck](https://blog.lfalck.se/logic-app-microsoft-graph-oauth/) which was the most useful resource I found in figuring this all out. 

The article is very good but there are some gaps and the process has moved on a bit, which is why I felt that my own article would be worth writing, but credit should go to Ludvig for pointing me in the right direction.

## What is Microsoft Graph?

[Microsoft Graph](https://developer.microsoft.com/en-us/graph/) is a massively powerful unified graph API which you can use to access data stored in the following Microsoft cloud services:

- Azure Active Directory
- Office 365
- Teams
- SharePoint
- OneDrive
- Many more

You can interactively explore all the capabilities of Graph via the [Graph Explorer](https://developer.microsoft.com/en-us/graph/graph-explorer) and see the API details in the [API Reference](https://docs.microsoft.com/en-us/graph/api/overview?view=graph-rest-1.0).

Graph replaces previously separate APIs such as the Azure Active Directory Graph API.

## What is a Logic App?

An [Azure Logic App](https://azure.microsoft.com/en-us/services/logic-apps/) is a no-code business workflow system which lets you integrate Microsoft and other SaaS services in a visual way.

Logic Apps are incredibly powerful and core tenant of Microsoft's Serverless platform.

## Specialist Connectors vs HTTP Connector vs Functions

One of the first things you'll come up against when first investigating Graph integration with Logic Apps is how to do it as there are a few options:

### Specialist Logic App Connectors

You can use specific [Logic App Connectors](https://docs.microsoft.com/en-us/azure/connectors/apis-list) for things like [Azure AD](https://docs.microsoft.com/en-gb/connectors/azuread/), [Office 365 Users](https://docs.microsoft.com/en-gb/connectors/office365users/) and [SharePoint](https://docs.microsoft.com/en-gb/connectors/sharepointonline/) and even [HTTP with Azure AD](https://docs.microsoft.com/en-us/connectors/webcontents/). 

The connectors are by far the easiest approach because they hide all the authentication complexity. 

However, connectors may not include all the features of the Graph. Two examples I ran into are as follows:

- The [Azure AD](https://docs.microsoft.com/en-gb/connectors/azuread/) connector lets you create a user, but not an user invitation (which is how you create a Guest user). 
- The [HTTP with Azure AD](https://docs.microsoft.com/en-us/connectors/webcontents/) connector works well for `GET` requests against the Graph but I did not manage to get it working for `POST` or `PATCH` requests, I think due to the fact that authorization and permissions is hidden.

In my case, I quickly ran into the limitations of these connectors and could only use them for about 20% of what I needed to do.

### Azure Functions

As a developer, the temptation is always to write code to do something and in the Azure Serverless world, this code would sit in an [Azure Function](https://azure.microsoft.com/en-gb/services/functions).

Functions are great because not only can they contain custom code, but they have built-in bindings and integrations, including [Microsoft Graph Bindings](https://docs.microsoft.com/en-gb/azure/azure-functions/functions-bindings-microsoft-graph). 

However, my view here (and it is a view, not a fact; your mileage may vary) is that when you are building Logic Apps, custom code should be the last resort and if you can do something using Logic App's built-in in connector, triggers and actions then, it makes things a lot simpler and easier to maintain.

### HTTP Connector with Azure AD App Registrations

Logic Apps have a brilliant [HTTP connector](https://docs.microsoft.com/en-us/azure/connectors/connectors-native-http) built-in which provides triggers and actions for receiving and sending HTTP requests.

This connector lets you call into any HTTP API, passing verbs, request body, headers and authorization settings.

In my experience with Graph, it is this HTTP connector in Logic Apps that made it all possible because it let me setup my own Azure AD App Registration which is needed for the Logic App to run as a service and have the right permissions over the Graph. 

This is where I settled for most of the integrations I needed for my logic app. The rest of this article will be focused on how to configure, authorize and use the built-in Logic App HTTP Connector to call the Graph.

## How to set up Microsoft Graph in Azure Logic Apps

For the remainder of this article I'll walk through the high level steps for setting up the Azure AD app and calling it in your Logic app.

I'll make an assumption that you already have a Logic App created and understand the basics of how Logic App actions work. 

If you are not yet at this stage, this article is not for you, I suggest you have a look at some of the [Logic App Quickstarts](https://docs.microsoft.com/en-us/azure/logic-apps/quickstart-create-first-logic-app-workflow).

### Step 1 - Azure AD App Registration

In order to call the Graph, the Logic Apps needs an Azure AD App Registration. This basically sets the  

### Step 2 - API Permissions & Grant Consent

### Step 3 - Client Secret

### Step 4 - Logic App HTTP action



## Further Reading

You may find these articles useful.

- [Microsoft Graph Docs > Get access without a user](https://developer.microsoft.com/en-us/graph/docs/concepts/auth_v2_service)
- [Logic Apps Active Directory OAuth Authentication for Microsoft Graph' article by Ludvig Falck](https://blog.lfalck.se/logic-app-microsoft-graph-oauth/) 