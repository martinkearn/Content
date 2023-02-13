---
title: Using Microsoft Graph in Logic Apps
author: Martin Kearn
description: The Microsoft Graph is the way you programmatically access data stored in Azure Active Directory, Office 365 and a bunch of other Microsoft cloud services. Accessing the data in a Logic App is a very powerful way to use this rich API with no code. This article tells you how.
image: https://github.com/martinkearn/Content/raw/master/Blogs/Images/microsoft-graph.png
thumbnail: https://github.com/martinkearn/Content/raw/master/Blogs/Images/microsoft-graph.png
type: article
status: published
published: 2019/08/29 13:40:00
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

[Microsoft Graph](https://developer.microsoft.com/en-us/graph/) is a massively powerful unified graph API which you can use to access data stored in the following Microsoft cloud services:

- Azure Active Directory
- Office 365
- Teams
- SharePoint
- OneDrive
- Many more

You can interactively explore all the capabilities of Graph via the [Graph Explorer](https://developer.microsoft.com/en-us/graph/graph-explorer) and see the API details in the [API Reference](https://docs.microsoft.com/en-us/graph/api/overview?view=graph-rest-1.0). Graph replaces previously separate APIs such as the Azure Active Directory Graph API.

An [Azure Logic App](https://azure.microsoft.com/en-us/services/logic-apps/) is a no-code business workflow system which lets you integrate Microsoft and other SaaS services in a visual way. Logic Apps are incredibly powerful and core tenant of Microsoft's Serverless platform.

I'd like to credit [the 'Logic Apps Active Directory OAuth Authentication for Microsoft Graph' article by Ludvig Falck](https://blog.lfalck.se/logic-app-microsoft-graph-oauth/) which was the most useful resource I found in figuring this all out. 

The article is very good but there are some gaps and the process has moved on a bit, which is why I felt that my own article would be worth writing, but credit should go to Ludvig for pointing me in the right direction.

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

The end result of this Logic App is that we are going to create a `User` using the [Create a user](https://docs.microsoft.com/en-us/graph/api/user-post-users?view=graph-rest-1.0&tabs=http) API.

### Step 1 - Azure AD App Registration

In order to call the Graph, the Logic Apps needs an Azure AD App Registration.

1. Login to the [Azure Portal](https://portal.azure.com/) and go to `Azure Active Directory`.
2. Go to `App Registrations` and click `New Registration`
3. Enter a name, it does not really matter what this is (I called mine "LogicApp")
4. Choose `Single Tenant`
5. Choose `Web` as the Redirect URI and set the value to https://localhost/myapp (it does not matter what this is, it will not be used by the Logic App).
6. Click `Register`

You'll now have you basic app registration.

### Step 2 - API Permissions & Grant Consent

The next step is to choose the API permissions that the App has over the Graph. 

Best practice is to follow the least privilege principle and only grant the precise permissions that are required by your application. The permissions that are require will depend on which Graph API call you are going to make. The [Microsoft Graph API Reference](https://docs.microsoft.com/en-us/graph/api/overview?view=graph-rest-1.0) has a Permissions section at the top of each API reference page which tells you the exact permissions that are required (you need the `Application` permissions).

If you look at the [Create invitation](https://docs.microsoft.com/en-us/graph/api/invitation-post?view=graph-rest-1.0&tabs=http) reference, you'll see that Applications needs:

- `User.Invite.All`
- `User.ReadWrite.All`
- `Directory.ReadWrite.All`

To add these permissions to your App Registration, follow these steps:

1. Go to `API permissions` for your App Registration
2. Click `Add a permission`
3. Choose `Microsoft Graph`
4. Choose `Application Permissions`. This is because our Logic App runs as a background service (or "daemon") and has no specific logged in user.
5. Find and select the permissions above (there is a handy search bar) then click `Add Permissions`

We now need to grant these permissions to the App Registration as an administrator. If we don't do this, the Logic App will fail.

6. Under the `Grant Consent` section, click `Grant Admin consent for (your directory name)` and click `Yes` to confirm. When complete you'll see a green tick next to all the permissions you added.

### Step 3 - Client Secret

In order for the Logic App to use the App Registration, it will need Client Secret or Certificate.

You can choose to use a certificate for a higher level of assurance, but this article is intended as a simple example and so a client secret is preferred for simplicity. For production systems, ensure you understand the choice of certificate vs client secret.

Use caution when following these steps, because the screen with the secret on is only ever shown once. if you forget or lose it, you'll have to create a new one.

1. Go to `Certificates & secrets` for your App Registration
2. Click `New client secret`
3. Add a description your secret. It does not matter what you chose here, I used "clientsecret"
4. Choose the expiry. It does not really matter which one you choose, but I chose `Never`
5. Click `Add`
6. Make a note of the `value` for your secret. It will only be shown on this page, when you navigate away it will be forever partially hidden

At this stage, the Azure AD App Registration is complete.

### Step 4 - Logic App HTTP action

We now get to the fun bit which is setting up the Logic App. We'll need to copy various value from the App Registration pages in this section so I find it is useful to keep the App Registration page open in a separate browser tab.

We'll now configure the HTTP action in your Logic App to create a guest user (an "Invitation") in Azure Active Directory.

1. In your Logic App click `Add step`

2. Choose the standard `HTTP` action

3. Set the values as follows

   | **Property**    | Value                                                        | Get the value from  / Comment                                |
   | :-------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
   | Method          | POST                                                         | This will depend on the API call you are making. See the `HTTP request` section of the relevant [Microsoft Graph API Reference](https://docs.microsoft.com/en-us/graph/api/invitation-post?view=graph-rest-1.0&tabs=http) |
   | URI             | https://graph.microsoft.com/v1.0/invitations                 | See the `HTTP request` section of the relevant [Microsoft Graph API Reference](https://docs.microsoft.com/en-us/graph/api/invitation-post?view=graph-rest-1.0&tabs=http) |
   | Body            | {"invitedUserEmailAddress": "martinkearn@hotmail.com","inviteRedirectUrl": "http://martink.me/","sendInvitationMessage": "true"} | This is a JSON document. Obviously, you should change the values to suit your own details and in most cases, value will come from the Logic App Dynamic values |
   | Authentication  | Active Directory OAuth                                       | Choosing this will expose all the other properties below     |
   | Tenant          | Something like `7435fef9-1754-4686-aed6-7aaa5eb51bab`        | Get this from Azure AD App Registration > Overview > `Directory (tenant) ID` |
   | Audience        | https://graph.microsoft.com                                  | This is always https://graph.microsoft.com/                  |
   | Client ID       | Something like `e5f5ab05-15dc-4b46-ab09-40302a054114`        | Get this from Azure AD App Registration > Overview > `Application (client) ID` |
   | Credential Type | Secret                                                       | See point earlier about secrets vs certificates              |
   | Secret          | Something like `tKExe[M4M66fjWgr.waluj+iQo*fd6N6`            | This is the value you stored from Step 3 - Client Secret     |

The setting should look similar to this:

![The Http action should look like this](https://github.com/martinkearn/Content/raw/master/Blogs/Images/LogicAppHttpAADInvitationRedacted.JPG)

4. `Save` the Logic App

5. `Run` the Logic App

6. After a few seconds, you'll see the result which will hopefully show each action with a green tick and if you followed the steps exactly, whatever email address you set for `invitedUserEmailAddress` will now have an email inviting them to join your Active Directory as a guest user.

## In Summary

This was a simple example of creating a guest user account in Azure Active Directory via the Microsoft Graph in a Logic App.

You'll see that we wrote zero code here and used only configuration in Azure AD and Logic Apps.

This pattern can be adapted to call any of the Microsoft Graph APIs which makes it a very powerful approach when building a Serverless application.

## Further Reading

You may find these articles useful.

- [Microsoft Graph Docs > Get access without a user](https://developer.microsoft.com/en-us/graph/docs/concepts/auth_v2_service)
- [Logic Apps Active Directory OAuth Authentication for Microsoft Graph' article by Ludvig Falck](https://blog.lfalck.se/logic-app-microsoft-graph-oauth/)
