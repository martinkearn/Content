---
title: Deploy your bot | Microsoft Docs
description: Deploy your bot to the Azure cloud.
keywords: deploy bot, azure deploy bot, publish bot
author: ivorb
ms.author: v-ivorb
manager: kamrani
ms.topic: get-started-article
ms.service: bot-service
ms.subservice: abs
ms.date: 02/13/2019
---

# Deploy your bot

[!INCLUDE [pre-release-label](./includes/pre-release-label.md)]

After you have created your bot and tested it locally, you can deploy it to Azure to make it accessible from anywhere. Deploying your bot to Azure will involve paying for the services you use. The [billing and cost management](https://docs.microsoft.com/en-us/azure/billing/) article helps you understand Azure billing, monitor usage and costs, and manage your account and subscriptions.

In this article, we'll show you how to deploy C# and JavaScript bots to Azure. It would be useful to read this article before following the steps, so that you fully understand what is involved in deploying a bot.

## Prerequisites

- Install latest version of the [msbot](https://github.com/Microsoft/botbuilder-tools/tree/master/packages/MSBot) tool.
- A [CSharp](./dotnet/bot-builder-dotnet-sdk-quickstart.md) or [JavaScript](./javascript/bot-builder-javascript-quickstart.md) bot that you have developed on your local machine.

## 1. Prepare for deployment
The deployment process requires a target Web App Bot in Azure so that your local bot can be deployed into it. The target Web App Bot and the resources that are provisioned with it in Azure are used by your local bot for deployment. This is necessary because your local bot does not have all the required Azure resources provisioned. When you create a target Web App bot, the following resources are provisioned for you:
-	Web App Bot – you will use this bot to deploy your local bot into.
-	App Service Plan - provides the resources that an App Service app needs to run.
-	App Service - service for hosting web applications
-	Storage account - contains all your Azure Storage data objects: blobs, files, queues, tables, and disks.

During the creation of the target Web App Bot, an app ID and password are also generated for your bot. In Azure, the app ID and password support [service authentication and authorization](https://docs.microsoft.com/azure/app-service/overview-authentication-authorization). You will retrieve some of this information for use in your local bot code.

> [!IMPORTANT]
> The language for the bot template for the service must match the language your bot is written in.

Creating a new Web App Bot is optional if you have already created a bot in Azure that you'd like to use.

1. Log in to the [Azure portal](https://portal.azure.com).
1. Click **Create new resource** link found on the upper left-hand corner of the Azure portal, then select **AI + Machine Learning > Web App bot**.
1. A new blade will open with information about the Web App Bot.
1. In the **Bot Service** blade, provide the requested information about your bot.
1. Click **Create** to create the service and deploy the bot to the cloud. This process may take several minutes.

### Download the source code and extract the .bot file
After creating the target Web App Bot, you need to download the bot code from the Azure portal to your local machine. The reason for downloading code is to get the service references that are in the [.bot file](./v4sdk/bot-file-basics.md). These service reference are for Web App Bot, App Service Plan, App Service, and Storage Account.

1. In the **Bot Management** section, click **Build**.
1. Click on **Download Bot source code** link in the right-pane.
1. Follow the prompts to download the code, and then unzip the folder.
1. Identify the .bot file and copy it the root of your existing bot project in the same directory as your existing .bot file

### Get the .bot file secret and path from App Settings in Azure
The .bot you have downloaded from the Azure Web App Bot is partially encrypted. You will need to obtain the secret to be able to develop with this .bot file locally
1. In the Azure portal, open the **Web App Bot** resource for your bot.
1. Open the bot's **Application Settings**.
1. In the **Application Settings** window, scroll down to **Application settings**.
1. Locate the **botFilePath** and copy it's value.
1. Locate the **botFileSecret** and copy it's value.

### Update the local app settings for the new .bot file secret and path
You must now edit your project to reference the new .bot file and it's secret.
1. In the existing bot project, open your settings file. For C# this will be `appsettings.json`, for JavaScript this will be `.env`
1. Set the value of `botFilePath` to reflect the **botFilePath** you copied earlier from the App Settings in Azure
1. Set the value of `botFileSecret` to be the **botFileSecret** you copied earlier from the App Settings in Azure
1. Save the settings file

> [!IMPORTANT]
> The bot secret can decrypt the .bot file which may contain multiple secrets and should be protected for production use. Use the standard approach for your language to protect teh contents of your settings file

### Setup a repository

To support continuous deployment, create a git repository using your favorite git source control provider. Commit your code into the repository.

Make sure that your repository root has the correct files, as described under [prepare your repository](https://docs.microsoft.com/azure/app-service/deploy-continuous-deployment#prepare-your-repository).

## 2. Deploy using Azure Deployment Center

Now, you need to upload your bot code to Azure. Follow instructions in the [Continuous deployment to Azure App Service](https://docs.microsoft.com/azure/app-service/deploy-continuous-deployment) topic.

Note that it is recommended to build using `App Service Kudu build server`.

Once you've configured continuous deployment, changes you commit to your repo are published. However, if you add services to your bot, you will need to add entries for these to your .bot file via the `msbot cli` tool.

## 3. Test your deployment

Wait for a few seconds after a successful deployment and optionally restart your Web App to clear any cache. Go back to your Web App Bot blade and test using the Web Chat provided in the Azure portal.

## Additional resources

- [How to investigate common issues with continuous deployment](https://github.com/projectkudu/kudu/wiki/Investigating-continuous-deployment)
