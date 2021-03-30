---
title: What is new in Logic Apps v2
author: Martin Kearn
description: There is a new version of Azure Logic App in preview. I spent a few days playing around with it to see what was new. This article summarises what I learnt and the highlights from my perspective.
image: https://github.com/martinkearn/Content/raw/master/Blogs/Images/LogicAppPreview.jpg
thumbnail: https://github.com/martinkearn/Content/raw/master/Blogs/Images/LogicAppPreview-thumb.jpg
type: article
status: draft
published: 2021/03/30 07:00:00
categories: 
  - Logic Apps
  - Serverless
---

.Azure Logic Apps are a no-code/low-code way of building powerful workflows that integrate with thousands of cloud services and data sources.

There is a new version on its way which is a fairly significant change in terms of the way Logic Apps are built, managed and hosted (we'll call it "V2" but it is also called "Preview").

In April 2021, I spent a few day getting used to V2; this article captures some of the highlights and key changes compared to the origional version. 

This article is based mainly on [Overview: Azure Logic Apps Preview](https://docs.microsoft.com/en-gb/azure/logic-apps/logic-apps-overview-preview) which is the official preview page as it was in April 2021. I will attempt to keep this article up to date as V2 gets to be generally available, but please remember that everything written here is based on the preview version and could change.

I'll assume you already have some experience with V1 of Logic Apps and understand the basic concepts such as triggers, actions, API connections, ARM template definition etc. If you do not understand these things, it may be better for you to start with the [official documentation](https://docs.microsoft.com/en-gb/azure/logic-apps/logic-apps-overview-preview) or wait until V2 is generally available when the documentation will be more complete.

## Built on the Functions Runtime

Logic Apps V2 has been rebuilt as an extension to the Azure Functions runtime.

This is the big change in V2 which enables a lot of other improvements around developer productivity. This means that the Logic App executes as an Azure Function would and requires only the Azure Functions runtime to work, wherever it is hosted. This is very much the same model used by Durable Functions.

This also means that Logic Apps can now take advantage of Azure Function's triggers, bindings and network topology.

There is a very good, detailed write-up about the new run-time model at [Azure Logic Apps Running Anywhere – Runtime Deep Dive](https://techcommunity.microsoft.com/t5/integrations-on-azure/azure-logic-apps-running-anywhere-runtime-deep-dive/ba-p/1835564), which a lot of this summary information is taken from.

Because Logic Apps are Azure Functions now, you can create a Logic App code project which is stored on your machine and looks very similar to an Azure Functions code project. The code project includes the following files:

- **Workflow Definition File (workflow.json)**. A project can contain multiple workflow definition files representing each workflow in the app (a single app can have multiple workflows, just like a single Function app has multiple functions). These files are the large JSON file which represent the structure, actions, triggers etc for the Logic App. As far as I can tell, the schema is unchanged from V1
- **Connection Definition File (connections.json)**. A project contains a single connection definition file. This is a new JSON file which defines details of all the API connections that the workflow.json files need. This is a change from V1 where connections were defined as part of the workflow itself. Having them as a separate file will make it easier to inject environment-specific values during deployment.
- **Host Configuration file (host.json)**. If you have ever developed an Azure Function, this file will be familiar. This file controls global configuration options that affect all workflows in the for a Logic App. This file is exactly the same schema as you would find in an Azure Function app. See [host.json reference for Azure Functions 2.x and later](https://docs.microsoft.com/en-us/azure/azure-functions/functions-host-json)
- **Settings file (local.settings.json)**. Again, this file is exactly as you would find in an Azure Functions project. This is a local file which contains secrets and settings that the application needs to run (database connection strings, API keys etc). It would typically mirror the settings form the deployed Azure Logic App resource (Application Settings), just like a Function. This is conceptually similar to `appsettings.json` from ASP.net applications. As this file contains secrets, it is not intended to be source controlled. See [App settings reference for Azure Functions > Local settings file](https://docs.microsoft.com/en-us/azure/azure-functions/functions-run-local?tabs=windows%2Ccsharp%2Cbash#local-settings-file).

Because these are all tangible files that can be created and managed locally, it means they can be included in a source control system such as Git and managed in the same way that any other code artefact is managed; this includes pull requests, branches, merge conflicts etc. 

This is a significant improvement in Logic Apps V2 because previously, the "code" always lived in Azure and you could only take an exported point-in-time copy to put in your source control system. During the export process, V1 used to change the structure of the JSON file each time it was exported which made merge conflicts almost impossible to resolve. All of these issues should go away now that the "code" can be managed as a locally stored collection of files which are deployed to Azure as required.

## Visual Studio Code for local development and debugging

- Extension & pre-reqs
- NGrok
- Code view vs designer view
- Breakpoints
- Node or C#

## Hosted as an App Service or Container

- Deploys to a "logic app resource" (something like a Function app) which can contain several "workflows"
- Deploy to Docker container
- Settings

## Stateful and Stateless options

## Better networking options

- because it is built on functions, it gets all the networking options form functions

## Application Insights for monitoring and tracing

## Connections, actions, triggers seem the same

- New built in connectors (Azure Service Bus, Azure Event Hubs, SQL Server, and MQ)

## In Summary

The move to using the Functions Runtime as the base for Logic Apps v2 is a very smart move in my opinion; it enables a lot of great features "for free" and bring the Azure serverless story together more.

Local development in Visual Studio Code is a bug win as we now have a proper development experience whihc can be manage by git (or some other source control) just like any other code artefact.

For further reading, I recommend:

- [Overview: Azure Logic Apps Preview](https://docs.microsoft.com/en-gb/azure/logic-apps/logic-apps-overview-preview)
- [Create stateful and stateless workflows in Visual Studio Code with the Azure Logic Apps (Preview) extension](https://docs.microsoft.com/en-gb/azure/logic-apps/create-stateful-stateless-workflows-visual-studio-code)
- [Azure Logic Apps Running Anywhere - Runtime Deep Dive](https://techcommunity.microsoft.com/t5/integrations-on-azure/azure-logic-apps-running-anywhere-runtime-deep-dive/ba-p/1835564)
-   More articles from me: http://martink.me/articles

