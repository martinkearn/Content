---
title: What is new in Logic Apps v2
author: Martin Kearn
description: There is a new version of Azure Logic App in preview. I spent a few days playing around with it to see what was new. This article summarises what I learnt and the highlights from my perspective.
image: https://github.com/martinkearn/Content/raw/master/Blogs/Images/LogicAppPreview.jpg
thumbnail: https://github.com/martinkearn/Content/raw/master/Blogs/Images/LogicAppPreview-thumb.jpg
type: article
status: draft
published: 2021/04/01 16:00:00
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

![Visual Studio Code with the Azure Logic Apps (preview) extension](https://github.com/martinkearn/Content/raw/master/Blogs/Images/VSCodeLogicAppExtension.jpg)

This extension currently has a few pre-requisites which are:

- [Visual Studio Code 1.30.1 (January 2019) or higher](https://code.visualstudio.com/)
- [Azure Account extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account)
- [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
- [Azure Functions Core Tools 3.0.3245 or later](https://github.com/Azure/azure-functions-core-tools/releases/tag/3.0.3245)
- [Azure Storage Emulator](https://docs.microsoft.com/azure/storage/common/storage-use-emulator) 

I'm a Windows user so have no been able to check but my understanding is that there are some addition hoops you have to jump through on Mac or Linux operating systems.

The [Create stateful and stateless workflows in Visual Studio Code with the Azure Logic Apps (Preview) extension](https://docs.microsoft.com/en-us/azure/logic-apps/create-stateful-stateless-workflows-visual-studio-code) document talks through the "getting started" process and will get you up and running so I wont cover that here. I will cover what I think are some of the highlights that this local development experience unlocks compared to V1.

**Development Lifecycle**; Firstly, not only is it really nice to be able to use and IDE rather than a browser to develop the Logic App, but it actually makes the whole development lifecycle so much simpler, especially if you are working in a team. Because you are editing a locally stored set of files, you can use source control and all the great advantages hat come with that. 

**Running & Debugging**; You can run and more importantly debug your Logic App directly from Visual Studio. This is made possible by the switch to using the Azure Functions runtime which already has a great local debug experience. You can also set breakpoints within the workflow JSON itself so you can see exactly what is going on in your Logic App (currently this only applies to Actions, not Triggers). In order to make local debugging possible, you will need a few additional tools:

- [NGrok](https://dashboard.ngrok.com/get-started/setup) (or similar) which tunnels public endpoints to your localhost
- [PostMan](https://www.postman.com/) (or similar) which is a tool that can generate HTTP requests

**Designer View**; You'd normally associate VSCode with text editing and whilst you can manually edit the workflow and related files in VS Code, the extension also give you that familiar visual designer that you are used to seeing on the Azure Portal. The designer itself has been tweaked to improve usability compared to V1 but the basic principle of how it works is the same. As far as I can tell there is no difference between the Portal and VS Code designers in terms of feature parity.

**Node or C#**; When you create a Logic App using the VSCode extension, it creates a Node project. I've not been able to determine the implications of this other than it must use the Node version of the Azure Functions runtime and presumably, any inline code will be JavaScript. However, you can change this to use C# if you prefer; you can convert the project to "use NuGet-based Logic App project". This will generate a `.csproj` file and manage dependencies via NuGet instead. I believe that certain capabilities are only possible with C# Logic Apps, such as [built-in connector authoring](https://docs.microsoft.com/en-us/azure/logic-apps/create-stateful-stateless-workflows-visual-studio-code#enable-built-in-connector-authoring).

## "Run anywhere" and custom connectors

One of the other benefits that the move to the Azure Functions runtime brings is flexibility on hosting models; you can run your V2 Logic App anywhere an Azure Function can run, this includes:

- Any network topology
- Any storage account type
- Custom domains and private endpoints
- Docker Containers
- App Service

One question that may arise with this "run anywhere" model is regarding how connectors works. Connectors are essentially API definitions that access API hosted on Azure. In Logic Apps v2 , there are "built-in" and "azure" connectors. 

**Built-in connectors** are hosted in the same process as the Logic App runtime and provide higher throughput, low latency, and local connectivity. There are built-in connectors for things that have always been built-in to Logic apps such as Control, Data Operations and Requests; however we also have things like Service Bus, SQL Server and Azure Functions.

The interesting thing about built-in connectors is that you can create your own using the Logic Apps connector extensibility model. This mean you can have a built-in connector for any service you require and have that packaged and deployed with your Logic App. Read [Azure Logic Apps Running Anywhere: Built-in connector extensibility](https://techcommunity.microsoft.com/t5/integrations-on-azure/azure-logic-apps-running-anywhere-built-in-connector/ba-p/1921272) for a deeper dive on this.

![Azure Logic App standard built-in connectors](https://github.com/martinkearn/Content/raw/master/Blogs/Images/LogicAppPreviewBuiltInConnectors.jpg)

**Azure Connectors** are the same as they always have been and connect you to a wide range of third party service created by Microsoft and others (Outlook.com, Twitter, OneDrive, Dropbox are just a few popular examples). It seems that all the connectors from V1 are also supported on V2.

![Logic App Azure Connectors](https://github.com/martinkearn/Content/raw/master/Blogs/Images/LogicAppPreviewAzureConnectors.jpg)

## Deployment options

It is possible to deploy your Logic App directly from the the [Azure Logic Apps for Visual Studio Code (Preview)](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurelogicapps) extension, just like you may have seen with other Azure publishing features in Visual Studio and Visual Studio Code.

When you publish to Azure, you create a "Logic App resource" on Azure. This is a resource that contains top level settings for your Logic App and one or more workflows. This is a change from V1 because the "Logic App" was both the resource and the workflow combined, but in V2, there is a distinction between the Azure Resource and the workflow that runs inside it; a single Logic App Resource can have multiple workflows. This is very similar to the way you would have a Function App Resource which contains multiple Functions.

Just like Azure Functions, the Application Settings required by your Logic App (`local.settings.json`) map to the Application Settings section of the Logic App Resource. This is a more standard way to manage settings compared to the bespoke model from Logic Apps V1.

Just like Azure Functions, you can choose from an Premium or App Service (dedicated) hosting model. These models directly to relate to the equivalent Azure Functions hosting models, read more at [Azure Functions hosting options](https://docs.microsoft.com/en-us/azure/azure-functions/functions-scale). Logic Apps do not currently support the entry-level "consumption" hosting plan offered with Azure Functions.

![Logic App Preview Resource](https://github.com/martinkearn/Content/raw/master/Blogs/Images/LogicAppsPreviewResource.jpg)

The story around deployment infrastructure-as-code (IaC) technologies such as Terraform or Pullumi is less clear at this stage as the preview docs do not specifically talk about it. Certainly it does not look like there is a Terraform provider for the new Logic App Resource (an update equivalent of [azurerm_logic_app_workflow](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/logic_app_workflow)), so doing a full deployment including the workflow itself is not going to be simple at this time.

Just like everything on Azure, it is possible to export the Logic App Resource and workflow definition as an ARM template is the same way you could do for V1. This enables deployment using something like an Azure DevOps pipeline.

You can read more about deploying the ARM template as follows:

- Deploying with Azure DevOps: https://github.com/Azure/logicapps/tree/master/azure-devops-sample
- Deploying with GitHub actions: https://github.com/Azure/logicapps/tree/master/github-sample

## Stateful and Stateless options

Logic Apps v1 were stateful. This means that each action, trigger and step of the workflow was internally persisted. This meant that the workflow could be long running and were highly durable. Stateful workflows offer high levels of resiliency in the case of outages and can be easily re-run.

Logic Apps V2 offers two workflow types, stateful and stateless.

Stateful workflows are basically the same as they were in V1, but stateless is a new concept for V2.

Stateless workflows only store "state" in memory and are not durable in eth same way that stateful workflows are. As a result, stateless workflows have shorter runs that are typically no longer than 5 minutes, faster performance with quicker response times, higher throughput, and reduced running costs because the run details and history aren't kept in external storage.

Read more at [Stateful and stateless workflows](https://docs.microsoft.com/en-us/azure/logic-apps/logic-apps-overview-preview#stateful-and-stateless-workflows).

## Other observations

As well as the "big ticket" items I've already talked about, there are a few smaller observations I've made around Logic Apps V1.

Because V2 is built on Azure Functions, it gets all the networking options from Functions. This means you can make a Logic App part of a VPN, which was a friction point with V1.

There seems to be tight, built-in integration with Applications Insights for monitoring and logging.

Connections, actions, triggers seem the same as they were in V1. This means that the vast array of third party integrations should all still work with V2.

## In Summary

The move to using the Functions Runtime as the base for Logic Apps v2 is a very smart move in my opinion; it enables a lot of great features "for free" and bring the Azure serverless story together more.

Local development in Visual Studio Code is a big win as we now have a proper development experience which can be manage by git (or some other source control) just like any other code artefact.

The introduction of the the stateless Logic App type will enable niche scenario that require super fast, highly scalable Logic Apps.

For further reading, I recommend:

- [Overview: Azure Logic Apps Preview](https://docs.microsoft.com/en-gb/azure/logic-apps/logic-apps-overview-preview)
- [Create stateful and stateless workflows in Visual Studio Code with the Azure Logic Apps (Preview) extension](https://docs.microsoft.com/en-gb/azure/logic-apps/create-stateful-stateless-workflows-visual-studio-code)
- [Azure Logic Apps Running Anywhere - Runtime Deep Dive](https://techcommunity.microsoft.com/t5/integrations-on-azure/azure-logic-apps-running-anywhere-runtime-deep-dive/ba-p/1835564)
- [Azure Logic Apps Running Anywhere: Built-in connector extensibility](https://techcommunity.microsoft.com/t5/integrations-on-azure/azure-logic-apps-running-anywhere-built-in-connector/ba-p/1921272)
- [Azure Logic Apps Running Anywhere - Built-In Service Bus Trigger: batching and session handling](https://techcommunity.microsoft.com/t5/integrations-on-azure/azure-logic-apps-running-anywhere-built-in-service-bus-trigger/ba-p/2079995)
- [Azure Logic Apps Running Anywhere – Monitor with Application Insights – part 1](https://techcommunity.microsoft.com/t5/integrations-on-azure/azure-logic-apps-running-anywhere-monitor-with-application/ba-p/1877849) and [Azure Logic Apps Running Anywhere – Monitor with Application Insights – part 2](https://techcommunity.microsoft.com/t5/integrations-on-azure/azure-logic-apps-running-anywhere-monitor-with-application/ba-p/2003332)
-   More articles from me: http://martink.me/articles

