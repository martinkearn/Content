# Managing secrets with .BOT files in Bot Framework v4

[Bot Framework V4 (BFv4)](https://docs.microsoft.com/en-us/azure/bot-service/?view=azure-bot-service-4.0) was made generally available (GA) as part of the Ignite 2018 announcements (see the [official announcement](https://azure.microsoft.com/en-us/blog/build-conversational-experiences-with-microsoft-bot-framework/)).

This is great news because it is now the 'current' version and all the docs, tooling and SDKs have been refreshed from the preview version we've been using since Build.

One of the things that is new in the GA version is the inclusion and dependency on .BOT files to manage secrets as well as other settings to make various tools work consistently.

It took me a while to figure this out so I thought I'd capture it here in case someone else is searching for the same thing.

## What is a .BOT file?

The .BOT file is not new and ever since BFv4 has been around, we've seen these files.

Up until now, they have been mainly used for storing configuration settings to use with the Bot Emulator to test your bot locally. They have always been able to store other things but I've never used it for that until now.

In BFv4 GA, the .BOT file is used to store mandatory configuration information, including secrets (App ID, Storage Connection string etc) which the bot code itself needs in order to execute. 

The idea is that the .BOT file is where you store all your bot configuration data, including secrets from things like LUIS or QNAMaker. It is what we'd traditionally use `AppSettings.json` or `DevEnv` for in BFv3. 

You can read more about the on the official docs: [Bot File](https://github.com/Microsoft/botbuilder-tools/blob/master/packages/MSBot/docs/bot-file.md).

When I first looked at this, I had a few questions ....

* So if .BOT files contain secrets ...
* And .BOT files are needed by the bot run-time ...
* How do I safely store them in my source control without committing my secrets?

The answer is to use the [MSBot Command Line Tool](https://github.com/Microsoft/botbuilder-tools/tree/master/packages/MSBot) to encrypt the secrets in the .BOT file

## The MSBot tool & encryption

## Storing the secret secretly

## Publishing to Azure

## Use the Azure template and it is done for you



