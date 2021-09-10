---
title: Deploying .net to Azure App Service with Pulumi
author: Martin Kearn
description: Pulumi is an Infrastructure-as-code platform that facilitates deployment of cloud resource and application using standard coding languages such as c# or TypeScript. I used Pulumi to deploy a .net 6 Blazor app and Azure Function to Azure App Service; this is what I learnt.
image: https://github.com/martinkearn/Content/raw/master/Blogs/Images/Blocks.jpg
thumbnail: https://github.com/martinkearn/Content/raw/master/Blogs/Images/Blocks-Thumb.jpg
type: article
status: draft
published: 2021/09/10 14:00:00
categories: 
  - Blazor
  - Azure Functions
  - Pulumi
  - IaC
---

.[Pulumi](https://www.pulumi.com/) is an infrastructure-as-code (IaC) platform that facilitates deployment of cloud resources and applications using familiar programming languages such as C#, Typescript, Python and Go. Pulumi is probably the main current alternative to [Terraform](https://www.terraform.io/) for cloud deployment.

Having never touched Pulumi before, I spent a few days learning about it and seeing if I could deploy a .net 6 Blazor Server application and an Azure Function to Azure App Services (PaaS, not container) using C#. While the Pulumi examples/tutorials are usefull, I found that there was not anything that covered exactly what I wanted to do, which is why I set about creating my own by patching various examples together.

The whole process was remarkably easy, but there were a few areas which I spent time researching, so this article is designed to help anyone else who is new to Pulumi working with .net and Azure.

I'm not going to waste your time trying to explain how Pulumi works, the best way to get that information is simply to use the Pulumi "Getting Started" tutorials which you can find at [Get Started with Azure](https://www.pulumi.com/docs/get-started/azure/). In addition to these simple tutorials, Pulumi provide a bunch of open-source examples on GitHub at https://github.com/pulumi/examples.

Any code I reference in this article can be found on GitHub at https://github.com/martinkearn/Pulumi-Playpen.

## Deployment zip and WEBSITE_RUN_FROM_PACKAGE

One of the simplest ways to publish your .net code to Azure is to publish to a ZIP file and set the Azure App Service to run from that zip file. This approach is outlined in detail here: [Run your app from a ZIP package](https://docs.microsoft.com/en-us/azure/app-service/deploy-run-package#create-a-project-zip-package) or [Run your Azure Functions from a package](https://docs.microsoft.com/en-us/azure/azure-functions/run-functions-from-deployment-package) for Azure Functions.

This approach seems to be the favour approach for most Pulumi examples and follows several high level steps:

1. You need to start with the published output of your application. For .net you can get this by running `dotnet publish` in the CLI, this will output the published application to `{project root folder}\bin\Debug\net6.0\publish`. Please note:
   1. This step is a pre-requisite; Pulumi does not do it for you. Typically you can make this part of your CI pipeline or post-build steps.
   2. The `Debug` can also be `Release` depending on your configuration
   3. The `net6.0` is for .net 6 projects but could also be `net5.0` or `netcoreapp3.1`.
2. The simplest place to put your ZIP in Azure is in an Azure Storage Account Blob Container. This code (which would be in your Pulumi stack) deploys a resource group, storage account and blob container in Azure:

```c#
// Create resource group
var resourceGroup = new ResourceGroup("MyResourceGroup");

// Create storage account
var storageAccount = new StorageAccount("storage", new StorageAccountArgs
{
    ResourceGroupName = resourceGroup.Name,
    Sku = new SkuArgs
    {
        Name = SkuName.Standard_LRS
    },
    Kind = Pulumi.AzureNative.Storage.Kind.StorageV2
});

// Add blob container to storage account
var container = new BlobContainer("deploymentzips", new BlobContainerArgs
{
    AccountName = storageAccount.Name,
    PublicAccess = PublicAccess.None,
    ResourceGroupName = resourceGroup.Name,
});
```

3. You can create the ZIP file and upload it to the blob container in a single step with Pulumi:

```c#
// Create a zip of the publish output and upload it to the blob container
var blob = new Blob($"myapp.zip", new BlobArgs
{
    AccountName = storageAccount.Name,
    ContainerName = container.Name,
    ResourceGroupName = resourceGroup.Name,
    Type = BlobType.Block,
    Source = new FileArchive($"..\\myapp\\bin\\Debug\\net6.0\\publish")
});
```

> **Setting the right path**: It is important to make sure you set the path correctly for the `new FileArchive`. The path should be from the location of the Pulumi stack .cs file. In this example, the application itself is a separate project in the same solution so we use `..\\` to go up one level and then set the path from there

4. We need to create a SAS URL for the ZIP file in storage so that the app service can access it without any addition keys or connection strings. To do this, I created a helper method as follows:

```c#
public static class OutputHelpers
{
    public static Output<string> SignedBlobReadUrl(Blob blob, BlobContainer container, StorageAccount account, ResourceGroup resourceGroup)
    {
        return Output.Tuple(blob.Name, container.Name, account.Name, resourceGroup.Name)
            .Apply(t =>
            {
                (string blobName, string containerName, string accountName, string resourceGroupName) = t;

                var blobSAS = ListStorageAccountServiceSAS.InvokeAsync(new ListStorageAccountServiceSASArgs
                {
                    AccountName = accountName,
                    Protocols = HttpProtocol.Https,
                    SharedAccessStartTime = DateTime.Now.Subtract(new TimeSpan(365, 0, 0, 0)).ToString("yyyy-MM-dd"),
                    SharedAccessExpiryTime = DateTime.Now.AddDays(3650).ToString("yyyy-MM-dd"),
                    Resource = SignedResource.C,
                    ResourceGroupName = resourceGroupName,
                    Permissions = Permissions.R,
                    CanonicalizedResource = "/blob/" + accountName + "/" + containerName,
                    ContentType = "application/json",
                    CacheControl = "max-age=5",
                    ContentDisposition = "inline",
                    ContentEncoding = "deflate",
                });
                return Output.Format($"https://{accountName}.blob.core.windows.net/{containerName}/{blobName}?{blobSAS.Result.ServiceSasToken}");
            });
    }
}
```

This is called in the main stack as follows:

```c#
// Generate SAS url for the function output zip in storage
var deploymentZipBlobSasUrl = OutputHelpers.SignedBlobReadUrl(blob, container, storageAccount, resourceGroup);
```

5. The final step is to set the `WEBSITE_RUN_FROM_PACKAGE` application setting on the Azure App Service to point to the SAS URL for the ZIP in storage. You can do this using the `SiteConfigArgs` property of `WebApp`

```c#
// Rest of the code is ommited deliberatley. See the later sections for how to creat ethe App Service Plan and App Service
SiteConfig = new SiteConfigArgs
{
    AppSettings = new[]
    {
        new NameValuePairArgs{
            Name = "WEBSITE_RUN_FROM_PACKAGE",
            Value = deploymentZipBlobSasUrl,
        },
    },
},
```

## Deploying .net 6 Blazor Server with Pulumi

Deploying a .net 6 Blazor Server app is really very simple once you've done the work outlined in the "Deployment zip and WEBSITE_RUN_FROM_PACKAGE" section.

> Though I use Blazor Server, these steps should apply to any .net application including Web API, Blazor Web Assembly or MVC.

Any .net application is generally hosted as an [Azure App Service](https://docs.microsoft.com/en-us/azure/app-service/overview) and all App Services needs and [App Service plan](https://docs.microsoft.com/en-us/azure/app-service/overview-hosting-plans). 

The following code creates a shared App Service Plan, but you should make sure you set the `Tier` and `Name` to match the SKU you want to use and the SKU that is compatible with the type of App Service you want to use, see more on this in the "Deploying Azure Function" section.

- `Name` maps to the accepted values for the `sku` from the [az appservice plan create CLI](https://docs.microsoft.com/en-us/cli/azure/appservice/plan?view=azure-cli-latest).
- `Tier` maps to the overall tier for the service plan which can be `Free`, `Shared`, `Basic`, `Standard` or `Premium` (I've not been able to find an official reference to this in the Pulumi docs)

```c#
// Create app service plan for app
var appServicePlan = new AppServicePlan("appserviceplan", new AppServicePlanArgs
{
    ResourceGroupName = resourceGroup.Name,
    Sku = new SkuDescriptionArgs
    {
        Tier = "Shared",
        Name = "D1"
    }
});
```

You can now create the App service itself and set it to run from the ZIP you created earlier.

```c#
// Create app service. Set WEBSITE_RUN_FROM_PACKAGE to use the zip in storage
var app = new WebApp($"myappappservice", new WebAppArgs
{
    ResourceGroupName = resourceGroup.Name,
    ServerFarmId = appServicePlan.Id,
    SiteConfig = new SiteConfigArgs
    {
        AppSettings = new[]
        {
            new NameValuePairArgs{
                Name = "WEBSITE_RUN_FROM_PACKAGE",
                Value = deploymentZipBlobSasUrl,
            },
        },
    },
});
```

You should now be able to navigate to https://portal.azure.com/ to see your deployed resource and then go to the actual Blazor app in a browser.

## Deploying Azure Function with Pulumi

Just like Blazor Server, deploying a .net Azure Function is also fairly simple once you've done the work outlined in the "Deployment zip and WEBSITE_RUN_FROM_PACKAGE" section.

Azure Functions deploy to Azure App Service but use some special configurations setting to make them a Function App Service rather than a regular App Service.

Firstly, the app service will need to be set to be one of the service plan types that are compatible with Azure Functions, typically the consumption plan is used which is set by `Tier = Dynamic` and `Name = Y1`.

```c#
// Create app service plan for function app
var appServicePlan = new AppServicePlan("appserviceplan", new AppServicePlanArgs
{
    ResourceGroupName = resourceGroup.Name,
    Sku = new SkuDescriptionArgs
    {
        Tier = "Dynamic",
        Name = "Y1"
    }
});
```

The `name` maps to the accepted values of the `sku` property on [az functionapp plan create CLI](https://docs.microsoft.com/en-us/cli/azure/functionapp/plan?view=azure-cli-latest#az_functionapp_plan_create). However, at the time of writing the accepted values are not documented. 

>  I have raised an issue for this: https://github.com/Azure/azure-cli/issues/19527.

I have not been able to determine exactly what the `Tier` maps to, but I assume that "dynamic" means a consumption plan. The [Pulumi docs](https://www.pulumi.com/docs/reference/pkg/azure-native/web/appserviceplan/#skudescription) are equally vague.

We can now create the App Service itself. Notice that we need to define the `Kind` which is what determines that it is a Function App Service. We also need to add additional application settings to tell the App Service about the function runtime. These settings are for a Function Runtime V3 .net function.

Just like the Blazor app, we use `WEBSITE_RUN_FROM_PACKAGE` to tell the App Service to run from the ZIP we uploaded to Blob Storage.

```c#
// Create function app. Set WEBSITE_RUN_FROM_PACKAGE to use the zip in storage
var app = new WebApp($"myfunctionappservice", new WebAppArgs
{
    Kind = "FunctionApp",
    ResourceGroupName = resourceGroup.Name,
    ServerFarmId = appServicePlan.Id,
    SiteConfig = new SiteConfigArgs
    {
        AppSettings = new[]
        {
            new NameValuePairArgs{
                Name = "AzureWebJobsStorage",
                Value = OutputHelpers.GetConnectionString(resourceGroup.Name, storageAccount.Name),
            },
            new NameValuePairArgs{
                Name = "FUNCTIONS_WORKER_RUNTIME",
                Value = "dotnet",
            },
            new NameValuePairArgs{
                Name = "FUNCTIONS_EXTENSION_VERSION",
                Value = "~3",
            },
            new NameValuePairArgs{
                Name = "WEBSITE_RUN_FROM_PACKAGE",
                Value = deploymentZipBlobSasUrl,
            },
        },
    },
});
```

## In summary

Pulumi is a very nice toolset that lets you provision cloud resource and code using the same programming language you use to writ ethe application itself.

I found that compared to Terraform, the learning curve was much less and I was able to achieve my objectives very quickly compared to the first few days I spent with Terraform.

This article and companying GitHub repo fills the gaps in the Pulumi docs around deploying .net applications to Azure.

Here are some resource that you may find useful.

- [Pulumi's community Slack](https://slack.pulumi.com/)
- [Pulumi's Azure Getting Started tutorial](https://www.pulumi.com/docs/get-started/azure/begin/)
- [Pulumi's Examples GitHub for Azure and C#](https://github.com/pulumi/examples#c-1)
- [martinkearn/Pulumi-Playpen: A playpen for experimenting and learning about Pulumi for Azure. (github.com)](https://github.com/martinkearn/Pulumi-Playpen)
-   More articles from me: http://martink.me/articles

