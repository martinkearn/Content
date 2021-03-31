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

Logic Apps V2 can be edited in Visual Studio Code using the [Azure Logic Apps for Visual Studio Code (Preview)](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurelogicapps) extension. 

>  Developing in VSCode is just an option. You can also develop in the Azure Portal, just like V1. See [Create stateful and stateless workflows in the Azure portal with Azure Logic Apps Preview](https://docs.microsoft.com/en-us/azure/logic-apps/create-stateful-stateless-workflows-azure-portal)

This extension currently has a few pre-requisites which are:

- [Visual Studio Code 1.30.1 (January 2019) or higher](https://code.visualstudio.com/)
- [Azure Account extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account)
- [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
- [Azure Functions Core Tools 3.0.3245 or later](https://github.com/Azure/azure-functions-core-tools/releases/tag/3.0.3245)
- [Azure Storage Emulator](https://docs.microsoft.com/azure/storage/common/storage-use-emulator) 

I'm a Windows user so have no been able to check but my understanding is that there are some addition hoops you have to jump through on Mac or Linux operating systems.

The [Create stateful and stateless workflows in Visual Studio Code with the Azure Logic Apps (Preview) extension](https://docs.microsoft.com/en-us/azure/logic-apps/create-stateful-stateless-workflows-visual-studio-code) document talks through the "getting started" process and will get you up and running so I wont cover that here. I will cover what I think are some of the highlights that this local development experience unlocks compared to V1.

**Development Lifecycle**; Firstly, not only is it really nice to be able to use and IDE rather than a browser to develop the logic app, but it actually makes the whole development lifecycle so much simpler, especially if you are working in a team. Because you are editing a locally stored set of files, you can use source control and all the great advantages hat come with that. 

**Running & Debugging**; You can run and more importantly debug your logic app directly from Visual Studio. This is made possible by the switch to using the Azure Functions runtime which already has a great local debug experience. You can also set breakpoints within the workflow JSON itself so you can see exactly what is going on in your Logic App (currently this only applies to Actions, not Triggers). In order to make local debugging possible, you will need a few additional tools:

- [NGrok](https://dashboard.ngrok.com/get-started/setup) (or similar) which tunnels public endpoints to your localhost
- [PostMan](https://www.postman.com/) (or similar) which is a tool that can generate HTTP requests

**Designer View**; You'd normally associate VSCode with text editing and whilst you can manually edit the workflow and related files in VS Code, the extension also give you that familiar visual designer that you are used to seeing on the Azure Portal. The designer itself has been tweaked to improve usability compared to V1 but eth basic principle of how it works is the same. As far as I can tell there is no difference between the Portal and VS Code designers in terms of feature parity.

**Node or C#**; When you create a Logic App using the VSCode extension, it creates a Node project. I've not been able to determine the implications of this other than it must use the Node version of the Azure Functions runtime and presumably, any inline code will be JavaScript. However, you can change this to use C# if you prefer; you can convert the project to "use NuGet-based Logic App project". This will generate a `.csproj` file and manage dependencies via NuGet instead. I believe that certain capabilities are only possible with C# Logic Apps, such as [built-in connector authoring](https://docs.microsoft.com/en-us/azure/logic-apps/create-stateful-stateless-workflows-visual-studio-code#enable-built-in-connector-authoring).

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

