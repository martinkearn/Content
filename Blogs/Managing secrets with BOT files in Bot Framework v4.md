---
title: Managing secrets with BOT files in Bot Framework v4
author: Martin Kearn
description: How to securely manage secrets in Bot Framework V4 projects using the all new .BOT file
image: https://dummyimage.com/800x600/000/fff&text=placeholder
thumbnail: https://dummyimage.com/200x200/000/fff&text=placeholder
type: article
status: published
published: 2018/09/27 09:00:00
categories: 
  - Bots
---
# Managing secrets with BOT files in Bot Framework v4

[Bot Framework V4 (BFv4)](https://docs.microsoft.com/en-us/azure/bot-service/?view=azure-bot-service-4.0) was made generally available (GA) as part of the Ignite 2018 announcements (see the [official announcement](https://azure.microsoft.com/en-us/blog/build-conversational-experiences-with-microsoft-bot-framework/)).

This is great news because it is now the 'current' version and all the docs, tooling and SDKs have been refreshed from the preview version we've been using since Build.

One of the things that is new in the GA version is the inclusion and dependency on .BOT files to manage secrets as well as other settings to make various tools work consistently.

It took me a while to figure this out so I thought I'd capture it here in case someone else is searching for the same thing.

## What is a .BOT file?

The .BOT file is not new and ever since BFv4 has been around, we've seen these files.

Up until now, they have been mainly used for storing configuration settings to use with the Bot Emulator to test your bot locally. 

In BFv4 GA, the .BOT file is used to store mandatory configuration information, including secrets (App ID, Storage Connection string etc) which the bot code itself needs in order to execute. 

The idea is that the .BOT file is where you store all your bot configuration data, including secrets from things like LUIS or QNAMaker. It is what we'd traditionally use `AppSettings.json` or `DotEnv` for in BFv3. 

You can read more about the on the official docs: [Bot File](https://github.com/Microsoft/botbuilder-tools/blob/master/packages/MSBot/docs/bot-file.md).

When I first looked at this, I had a few questions ....

* So if .BOT files contain secrets ...
* And .BOT files are needed by the bot run-time ...
* How do I safely store them in my source control without committing my secrets?

The answer is to use the [MSBot Command Line Tool](https://github.com/Microsoft/botbuilder-tools/tree/master/packages/MSBot) to encrypt the secrets in the .BOT file

## The MSBot tool & encryption

The [MSBot Command Line Tool](https://github.com/Microsoft/botbuilder-tools/tree/master/packages/MSBot) is a tool built as part of the overall BFv4 release which helps you manage .BOT files.

MSBot is an NPM package which you install as follows:

`npm install -g msbot`

MSBot facilitates many things but the primary concern for this article is that MSBot can encrypt the keys contained in the .BOT file (not the entire file) and give you a secret which is used by the bot run-time to decrypt the keys.

To encrypt a decrypted BOT use this:

`msbot secret --new`

To clear existing secrets and decrypt use this:

`msbot secret -b my.bot --secret OLDSECRET --clear`

For full details on using MSBot for encrypting your secrets within a .BOT file see [MSBot > Bot Secrets](https://github.com/Microsoft/botbuilder-tools/blob/master/packages/MSBot/docs/bot-file-encryption.md).

## Storing the secret secretly

When you use MSBot to encrypt the secrets within a .BOT file, you are given a secret which is required to decrypt the file and access the secret keys within.

It is important that the secret from MSBot is not contained within source control; the way you handle this depends on platform.

### .net

For .net it is best practice to use the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets/) extension which adds a Visual Studio extension to expose `secrets.json`. 

You can install the extension via NuGet.

When the extension is installed, you can right-click on the project in Visual Studio and go to `Manage User Secrets` which opens `secrets.json`. 

This is a file which is stored outside your project structure on your local machine's hard drive (and therefore your source control). 

You can then add your MSBot secret and file path as follows. 

```json
  {
    "botFilePath": "./YourBotFile.bot",
    "botFileSecret": "Your MSBot secret, something like zOku/rKiFRa/ISohbv3/1O6k7rhGEsXdV+lAO/8mVBU="
  }
```

In order for your bot code to access the local `secrets.json` file during development, you need to add this to the `Startup` constructor in `Startup.cs` (more on how this works in production/Azure later).

```c#
if (env.IsDevelopment())
{
    builder.AddUserSecrets<Startup>(false);
}
```


You can then access secrets in your code the way you would do for any typical `AppSettings.json` file. The default bot templates will use something like this in the `ConfigureServices` method in `Startup.cs`.

```c#
  var secretKey = Configuration.GetSection("botFileSecret")?.Value;
  var botFilePath = Configuration.GetSection("botFilePath")?.Value;
```

See [Safe storage of app secrets in development in ASP.NET Core](https://docs.microsoft.com/en-us/aspnet/core/security/app-secrets?view=aspnetcore-2.1&tabs=windows) for full details on the pattern for managing secrets in .net Core 2.

### JavaScript/Node/TypeScript

For Node/TypeScript, secrets are managed via the `DotEnv` NPM package which is installed as follows:

`npm install -g dotenv`

You can create a `.env` file which contains the secrets as follows:

```json
  {
    "botFilePath": "./YourBotFile.bot",
    "botFileSecret": "Your MSBot secret, something like zOku/rKiFRa/ISohbv3/1O6k7rhGEsXdV+lAO/8mVBU="
  }
```

You can then read the secrets as follows:

```javascript
  // Import required bot configuration.
  const { BotConfiguration } = require('botframework-config');

  // Read botFilePath and botFileSecret from .env file.
  const ENV_FILE = path.join(__dirname, '.env');
  const env = require('dotenv').config({ path: ENV_FILE });

  // Get the .bot file path
  const BOT_FILE = path.join(__dirname, (process.env.botFilePath || ''));
  let botConfig = BotConfiguration.loadSync(BOT_FILE, process.env.botFileSecret);
```

There are many tutorials for using `DotEnv` to manage secrets in Node, simply [do a search for "managing secrets with dotenv"](https://www.bing.com/search?q=managing+secrets+with+dotenv&form=EDGEAR&qs=PF&cvid=6d694ba619b4463191cf2d0d46baf873&cc=GB&setlang=en-US) to see more detail.

## Publishing to Azure

As we know, BFv4 now uses the .BOT file and the secrets contained within as part of its run time, so how does Azure access the .BOT file if it is encrypted and the secret to decrypt it is not in source control or published to Azure?....

One of the coolest things about Azure App Service (which is where bots get published) is that the `Application Settings` in the service replicate local configuration settings, regardless of which platform you are using.

This means that when you access configuration settings in code, Azure will place the value from its own Application Settings and avoid the need for files like `secrets.json` or `.env`.

All you need to do as a developer is make sure that secret and path to your .BOT file are stored in Azure App Service Application Settings to match your local `secrets.json` or `.env`. files.

The settings are as follows:

* **botFilePath**: value will look something like `./YourBotFile.bot`
* **botFileSecret**: value will look something like `zOku/rKiFRa/ISohbv3/1O6k7rhGEsXdV+lAO/8mVBU=`

### Making sure your .BOT file gets deployed in .net projects

In .net bot projects, if you make changes to your .BOT file, it will not get deployed by default when you publish to Azure and your production bot will always use the initial .BOT file

The reason for this is that the .BOT file is marked as `Do not copy` by default which means that it is not treated as content for the application and so never gets deployed. 

To make sure your .BOT file is deployed whenever you publish, set the 'Copy To Output Directory' to `Copy Always` on the file properties of your .BOT file in Visual Studio.

There is a [Stack Overflow post](https://stackoverflow.com/questions/52610698/bot-file-not-getting-deployed-to-azure-bot-service-v4) on this which provides details.

## Use the Azure template and it is done for you

Well done for getting this far through the article.

You now understand what a .BOT file does, how to use MSBot tool to safely encrypt secrets within .BOT file and then store the generated secret in a safe way outside your source control.

You also understand how configuration settings are accessed in Azure.

This next bit might be annoying if you've read this entire article ..... if you create your Azure Bot Service from Azure, all of this is done for you apart from the 'Storing the secret secretly' bit. 

An Azure Bot Service created via Azure will have:

* A .BOT file which contains the encrypted keys, connection strings etc which are required to access Storage and other things needed to make a bot work
* The secret and path to your .BOT file is stored by default in Application Settings in your Azure App Service
* The secret and path to your .BOT file is also stored in `AppSettings.json` for .net or an `.env` file for Node. However this file should not be committed to source control so make sure you follow the steps in the 'Storing the secret secretly' bit

## In Summary
To summarise this article, here are the key take-away points:

* .BOT files are the right way to store secrets and other configuration settings for bots in BFv4.
* .BOT files are language independent and the same approach is used for .net, Node etc
* .BOT files can and should have their secrets contained within encrypted via the MSBot tool
* Make sure you protect the secret to access your .BOT file and do not commit it to source control. Use the techniques that you would normally use to manage secrets for the language you are using (`secrets.json` for .net or `DotEnv` for Node).
* If you create your BotService via Azure and use the started code it generates for you, all of this is setup for you, you just need to protect your MSBot secret and file path

For further reading, I'd recommend:

* Read more about the MSBot tool: https://github.com/Microsoft/botbuilder-tools/tree/master/packages/MSBot
* Read more about .BOT files: https://github.com/Microsoft/botbuilder-tools/blob/master/packages/MSBot/docs/bot-file.md
* Read more about using MSBot to manage secrets in .BOT files: https://github.com/Microsoft/botbuilder-tools/blob/master/packages/MSBot/docs/bot-file-encryption.md
* Read more about BFv4: https://docs.microsoft.com/en-us/azure/bot-service/?view=azure-bot-service-4.0
