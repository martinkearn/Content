---
title: Dialog Chaining in the Microsoft Bot Framework
author: Martin Kearn
description: How to work with multiple dialogs in Bot Framework v3
image: http://martink.me/images/MartinKearnProfile1.jpg
thumbnail: http://martink.me/images/MartinKearnProfile1.jpg
type: article
status: draft
published: 2018/11/24 09:30:00
categories: 
  - Bots
---

# Dialog Chaining in the Microsoft Bot Framework in
The [Microsoft Bot Framework](https://dev.botframework.com/) is a great way to build chat bot applications that can be published to multiple channels such as Skype, Facebook, Kik and more.

You can write your bots once using C# or NodeJS and the Microsoft Bot Framework has SDKs that support both languages. However, there is not complete parity between the SDKs and one of the areas where they differ is around how Dialogs are chained together.

I was recently working with a customer who needed a simple referancable solution for working with multiple dialogs in C# so I created an example project which is on GitHib: https://github.com/martinkearn/Bot-Dialogs-Example

## What is a Dialog?
A Dialog is an important concept in the Bot Framework and effectively represents what a screen would do within a regular mobile app. 

A dialog represents a distinct piece of the conversational interface. Multiple dialogs can be chained together to bring together mutiple features of a bot and satisfy an end-to-end user journey, this is called a 'Dialog Stack' or 'Dialog Chain'. The users position within the dialog stack is referred to as the context.

One of the great things about the Bot Framework is that the Connector Service stores the state of the dialog stack, this making the bot implementation stateless between requests.

The chained architectural style of bot dialogs means that when a dialog is entered one of three things must occur before the dialog is completed:
1. The context is forwarded to another dialog. This means that another dialog must be competed before the current one can be
2. The context is marked as 'done' meaning that the current dialog is complete and the contxt is passed to the parent dialog
3. The context waits for input from the user

## LuisDialog
LUIS (Language Understanding Intelligent Service) is part of the Microsoft Cognitive Services suite and provide natural language processing for all type of app, but is especially useful in the context of bots.

LUIS allows you to configure a number of intents and it will determine which of your intents the user's utterance (the text they send to the bot) most closley matches. This give you a great way of navigating to different dialogs and dialog branches within your bot.

LUIS will also identify entities within the utterance. Entities are peices of variable information that are pertainenet to the intent. For example if a user utters "what is the weather like in Worcester today", the intent would be to `GetWeatherForecast`, but `Worcester` and `Today` would be idnetified as entities. You can see more baout LUIS in a [Web Hack Wedesday video](https://channel9.msdn.com/Shows/Web-Hack-Wednesday/LUIS-and-HowHappycouk-mashup) I recorded with [Martin Beeby](http://thebeebs.co.uk/).

When you are using [LUIS](https://www.microsoft.com/cognitive-services/en-us/language-understanding-intelligent-service-luis)

## FormFlow
