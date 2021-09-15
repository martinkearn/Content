# Deploy an Azure Function with Pulumi

These are the steps to get a Pulumi stack setup and ready to go (windows, .net , azure)

### Pre-Reqs

- Clone [martinkearn/Pulumi-Playpen: A playpen for experimenting and learning about Pulumi for Azure. (github.com)](https://github.com/martinkearn/Pulumi-Playpen)
- Make a branch just for the demo
- To set the scene in story of story telling, complete [PulumiSetupWindows.md](./PulumiSetupWindows.md)

## Review Solution

In Visual Studio, review the solution

1. CSharpFunctionDemo.Function is a typical HTTP azure function project
2. CSharpFunctionDemo.Infrastructure is the result of `pulumi new azure-csharp` with a few minor modifications
3. I've added a Helper class which contains some code that will make the demo smoother for creating a SAS token, getting connection string and getting a primary storage key

## Deploy the Function code as a zip to storage

Functions (like all .net application) can be deployed as a ZIP file in storage and the app service can run from the ZIP file. This is typicaly approach with most IaC platforms.

1. `cd C:\git\Pulumi-Playpen\CSharpFunctionDemo\function`
2. `dotnet publish`
3. Add code to create a blob container to `MyStack.cs`

```c#
// Add blob container to storage account
var container = new BlobContainer("functionzips", new BlobContainerArgs
{
    AccountName = storageAccount.Name,
    PublicAccess = PublicAccess.None,
    ResourceGroupName = resourceGroup.Name,
});
```

4. Add code to create a blob in the container which is a zip of the function's published code

```c#
// Create a zip of the function's publish output and upload it to the blob container
var blob = new Blob($"function.zip", new BlobArgs
{
    AccountName = storageAccount.Name,
    ContainerName = container.Name,
    ResourceGroupName = resourceGroup.Name,
    Type = BlobType.Block,
    Source = new FileArchive($"..\\Function\\bin\\Debug\\netcoreapp3.1\\publish")
});
```

5. `cd ..\Infrastructure`
6. `pulumi up`, follow steps to create a stack, then choose `yes` to perform update
7. Go to Azure portal and explore the function.zip file in storage

## Deploy the Function App Service

1. Add code to create App Service Plan

```c#
// Create app service plan for function app
var appServicePlan = new AppServicePlan("appserviceplan", new AppServicePlanArgs
{
    ResourceGroupName = resourceGroup.Name,
    // Consumption plan SKU
    Sku = new SkuDescriptionArgs
    {
        Tier = "Dynamic",
        Name = "Y1"
    }
});
```

2. Add the code to create the app service

```c#
// Create function app. Set WEBSITE_RUN_FROM_PACKAGE to use the zip in storage
var app = new WebApp($"appservice", new WebAppArgs
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
                Value = OutputHelpers.SignedBlobReadUrl(blob, container, storageAccount, resourceGroup),
            },
        },
    }
});
```

3. `pulumi up`, then choose `yes`
4. Go to Azure portal and show the function's app settings > run from package
5. Hit the endpoint in the browser

