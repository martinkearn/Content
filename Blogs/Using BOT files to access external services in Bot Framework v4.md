---
Title:Using .BOT files to access external services in Bot Framework v4
Author:Martin Kearn
Keywords:BotFrameworkV4,MSBOT CLIT
---



Title:	Using .BOT files to access external services in Bot Framework v4  
Author:	Martin Kearn  
Keywords:	BotFrameworkV4,MSBOT CLI 

---

# Using .BOT files to access external services in Bot Framework v4

[Bot Framework V4 (BFv4)](https://docs.microsoft.com/en-us/azure/bot-service/?view=azure-bot-service-4.0) was made generally available (GA) as part of the Ignite 2018 announcements (see the [official announcement](https://azure.microsoft.com/en-us/blog/build-conversational-experiences-with-microsoft-bot-framework/)).

This is great news because it is now the 'current' version and all the docs, tooling and SDKs have been refreshed from the preview version we've been using throughout 2018 so far.

I recently wrote about how you can use the new .BOT files to store and manage secrets and configuration information for BFv4 projects. You can see my article here: [Managing secrets with .BOT files in Bot Framework v4](https://blogs.msdn.microsoft.com/martinkearn/2018/09/27/managing-secrets-with-bot-files-in-bot-framework-v4/) 

## Refresher on .BOT files

The .BOT file is a JSON document that is created when you create a BFv4 bot project from an Azure Bot Service.

The .BOT file is used to store mandatory configuration information which the bot code itself needs in order to execute. The idea is that the .BOT file is where you store all your bot configuration data, including secrets to access external services. It is what we'd traditionally use `AppSettings.json` or `DotEnv` for in BFv3. The advantage to a .BOT file being a file is that tools like the Bot Emulator can use them as well as the bot code itself.

Typically, .BOT files are encrypted so that the sensitive data cannot be viewed, my [Managing secrets with .BOT files in Bot Framework v4](https://blogs.msdn.microsoft.com/martinkearn/2018/09/27/managing-secrets-with-bot-files-in-bot-framework-v4/) article goes into this in more detail.

.BOT files are the way you _should_ store configuration data for any external service that is used within your bot code, this includes:

* Azure AI services such as LUIS or QNAMaker
* Azure storage services such as Blob storage or Cosmos DB
* Tooling services like Dispatcher and App Insights
* Generic services; any other service that does not have its own pre-defined type. This could include other AI services such as Face API or your own custom API

You add services and manage the .BOT file via the [MSBot Command Line Tool](https://github.com/Microsoft/botbuilder-tools/tree/master/packages/MSBot).

You can read more about the on the official docs: [Bot File](https://github.com/Microsoft/botbuilder-tools/blob/master/packages/MSBot/docs/bot-file.md).

## Adding a Generic Service

You'll find plenty of samples in the [BotBuilder-Samples repository](https://github.com/Microsoft/BotBuilder-Samples/blob/master/readme.md) for integrating things like Luis, QNAMaker, Cosmos etc using the .BOT file. 

However, I found there was a shortage of examples for using the Generic Service, which is why this article was written.

I have been working on a simple bot that does age verification using the [Cognitive Services Face API](https://azure.microsoft.com/en-us/services/cognitive-services/face/). This is a Microsoft AI service but does not have a special type in the .BOT file so it has to be treated like a Generic Service.

As with any generic service, all I really need to access it is an endpoint and a key. I added this to the .BOT file via the [MSBot Command Line Tool](https://github.com/Microsoft/botbuilder-tools/tree/master/packages/MSBot) as follows:

```
MSBOT connect generic --name "FaceApi" --url "https://uksouth.api.cognitive.microsoft.com/face/v1.0 -keys "{""key"":""mykeyhere""}" --secret "mybotfilesecrethere"
```

This will add something like this to the .BOT file

```json
"services": [
    {
        "type": "generic",
        "name": "FaceApi",
        "url": "https://uksouth.api.cognitive.microsoft.com/face/v1.0",
        "configuration": {
            "key": "xXOpE4UKJ0aCmVTNk989qQ==!R5ouZHhJSzA0XDiq9YzSARdx5+0k/Yxyihi3+dWYPy4UXh1xiL4M6ZuumSYt9fi9"
        },
        "id": "179"
    }
],
```

There are a few things to note here:

* You have to use the `--keys` parameter to store keys as a serialized JSON string of key/value pairs. The fact that it needs to be serialized was a stumbling block for me and it took a while to work out the right format
* You have to add a `--secret` parameter whenever you do anything with a MSBOT file. This is so that sensitive data is encrypted. Notice that the key is encrypted in the JSON above.
* The MSBOT Command Line Tool automatically generates and assigns an ID for the service.

You can now setup your bot code to read and use the details of your generic external service.

## Accessing external services in bot code

There are a few steps for accessing the .BOT file in code which I've only really done in .net. 

However, most of this has been taking by looking through the [BotBuilder-Samples repository](https://github.com/Microsoft/BotBuilder-Samples/blob/master/readme.md)  which has .net and JavaScript examples,

### 1: Define your BotServices class

We're going to use a full .net class called `BotServices` to access our services and expose the required data points as properties in our Bot Code.

In my scenario, I'm working with the Face API so need the Endpoint and Key so I created a `BotServices` class which looks like this

```c#
{
    public BotServices(BotConfiguration botConfiguration)
    {
        foreach (var service in botConfiguration.Services)
        {
            switch (service.Type)
            {
                case ServiceTypes.Generic:
                    {
                        if (service.Name == "FaceApi")
                        {
                            var faceApi = service as GenericService;
                            FaceApiEndpoint = faceApi.Url;
							FaceApiKey = faceApi.Configuration["key"];
                        }

                        break;
                    }
            }
        }
    }

    public string FaceApiEndpoint { get; }

    public string FaceApiKey { get; }
}
```

### 2. Add your BotServices with DI

Because BFv4 is built with .net core 2.0 we can use .net's built-in dependency injection features to add our `BotServices` to the overall `IServiceCollection` for the bot.

If you created your bot with a template, then much of the boiler-plater work around reading the .BOT file has been done for you in `StartUp.cs`, but these are the additions required to add your `BotServices`.

This code should be placed in your `Startup.cs` >  `ConfiguredServices` function. It will read and decrypt the .BOT file then initialize your `BotServices` object and add it as a singleton via dependency injection.

```c#
public void ConfigureServices(IServiceCollection services)
{
    var botFilePath = Configuration.GetSection("botFilePath")?.Value;
    var botFileSecret = Configuration.GetSection("botFileSecret")?.Value;
    var botConfig = BotConfiguration.Load(botFilePath ?? @".\BotConfiguration.bot", botFileSecret);

    // Initialize Bot Connected Services clients.
    var connectedServices = new BotServices(botConfig);
    services.AddSingleton(connectedServices);
```

### 3. Use your BotServices in you main IBot

We now need to make changes to our main `IBot` file to use the `BotServices`.

Declare a private readonly variable of type `BotServices`

```c#
private readonly BotServices _botServices;
```

Update your constructor to initialize your private `_botServices` variable. It should look something like this (other code omitted for brevity).

```c#
public EchoWithCounterBot(BotServices botServices)
{
    _botServices = botServices ?? throw new ArgumentNullException(nameof(botServices));
```

You can now access the properties of you `_botServices` object which contain decrypted data from your .BOT file as follows

```c#
var httpClient = new HttpClient
{
    BaseAddress = new Uri(_botServices.FaceApiEndpoint),
};
httpClient.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", _botServices.FaceApiKey);
```

## Deploying .BOT files to Azure

By default, .BOT files are not included as part of the output of a project and therefore are not deployed when you publish your project.

This can result in a situation where the Azure-hosted version of your bot cannot see the changes you've made to the .BOT file (because it has not been deployed).

To address this you simply need to change the "Copy to Output Directory" file property to `Copy Always` and the .BOT file will be deployed whenever you deploy your project.

I believe this is a bug and have raised an issue in the BotBuilder GitHub repository: https://github.com/Microsoft/botbuilder-dotnet/issues/1004

## In Summary

.BOT files are the preferred way to store configuration data for all external services in BFv4 and it is easy to use with the built in integrate for Microsoft's various Azure services but you can use a Generic Service type to store settings for your own service.

* Review my age verification GitHub sample which this article is based on : https://github.com/martinkearn/Bot-Age-Verification
* Review the BotBuilder-Samples repository for examples of using the .BOT file and BotService in both .net and JS: https://github.com/Microsoft/BotBuilder-Samples/blob/master/readme.md
* Read about encrypting the .BOT file and managing secrets in my article: https://blogs.msdn.microsoft.com/martinkearn/2018/09/27/managing-secrets-with-bot-files-in-bot-framework-v4/
* Read more about the .BOT file itself: https://github.com/Microsoft/botbuilder-tools/blob/master/packages/MSBot/docs/bot-file.md
* Read more about the MSBot command line utility: https://github.com/Microsoft/botbuilder-tools/tree/master/packages/MSBot



