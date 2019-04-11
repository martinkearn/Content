---
title: Bot Framework v4.3 Template
author: Martin Kearn
description: There is no official template for starting a Bot Framework V4.3 project. So colleagues and I created one. This articles outlines what it does and how to use it.
image: https://github.com/martinkearn/Content/raw/master/Blogs/Images/BotVSIX.JPG
thumbnail: https://github.com/martinkearn/Content/raw/master/Blogs/Images/BotVSIX_thumb.JPG
type: article
status: published
published: 2019/04/11 12:00:00
categories: 
  - Bot Framework
  - Bots
  - Visual Studio
---

# Bot Framework v4.3 Starter Template

The latest version of the Bot Framework is v4.3 and it was released in March 2019. You can read about the exciting new features in the [Conversational AI updates for March 2019 Article](https://azure.microsoft.com/en-gb/blog/conversational-ai-updates-for-march-2019/) from the Bot Framework product team.

At the time of writing (11th April 2019), the [templates](https://marketplace.visualstudio.com/items?itemName=BotBuilder.botbuilderv4), [documentation](https://docs.microsoft.com/en-us/azure/bot-service/?view=azure-bot-service-4.0) and [samples](https://github.com/Microsoft/BotBuilder-Samples/blob/master/README.md) had not yet been updated to reflect the new v4.3 patterns and practices.

There are new samples being produced in the [Samples Work in Progress branch](https://github.com/Microsoft/BotBuilder-Samples/tree/samples-work-in-progress). This features many patterns and practices which are new to v4.3.

There is not yet a v4.3 template or a project type that is suited to starting a new project with v4.3 patterns, therefore it is difficult to create a new bot project from scratch.

## Introducing the Starter Bot template

Myself, [Ibrahim Kivanc](https://github.com/ikivanc) and [Martin Simecek](https://github.com/msimecek) have been working on a project and associated VSIX which can be used as a great starting point for a .net Bot Framework v4.3 project that uses the latest patterns and practices.

We have looking through the [Samples Work in Progress branch](https://github.com/Microsoft/BotBuilder-Samples/tree/samples-work-in-progress) to determine common patterns shown in every sample and features which we think will be relevant to almost every bot project. 

We've rolled them all into the [Bot Starter Template for v4.3](https://github.com/martinkearn/Bot-Starter-Template)

The Starter Bot includes the following: 

- Bot project with v4.3.2 `Microsoft.Bot.Builder.*` NuGet packages
- Up-to-date patterns around `StartUp.cs`, `Program.cs`, `BotController.cs`, the main `ActivityHandler` architecture and `dialog` constrctors
- Basic dialog system with a root dialog and multiple child dialogs
- Global state being updated and used across `ActivityHandler` and `Dialog`s
- Passing objects to dialogs on construction
- Strings using RESX files
- A placeholder (commented out) example of using Dispatch, Luis and QNAMaker

## VSIX

You can get the VSIX for the Starter Bot project [here](https://github.com/martinkearn/Bot-v4.3-Template/raw/master/vsix/StarterBot.vsix).



