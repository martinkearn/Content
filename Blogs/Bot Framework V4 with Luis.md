---
title: Bot Framework v4 with Luis
author: Martin Kearn
description: The way you access Luis has changed in Bot Framework V4. Here are some tips based on what I learnt with the preview version in July 2018
image: https://dummyimage.com/800x600/000/fff&text=placeholder
thumbnail: https://dummyimage.com/200x200/000/fff&text=placeholder
type: article
status: published
published: 2018/08/30 09:30:00
categories: 
  - Bots
  - Cognitive Services
  - Luis
---

# Bot Framework v4 with Luis

The [Cognitive Services Language Understanding Intelligence Services (Luis)](https://azure.microsoft.com/en-us/services/cognitive-services/language-understanding-intelligent-service/) should be no stranger to bot developers, it enables the all-important natural language processing which means users can converse with your bot in the way they would with another human; by using natural language.

With Bot Framework V3 (BFv3), there is a well prescribed pattern for using Luis in your bot via the `LuisDialog` which gives you a nicely packaged object to work with, abstracts the Luis API call and handles the top level intents.

Bot Framework V4 (BFv4) is a bit more complex.

I did some work on BFv4 a few weeks ago. I wrote about my main initial observations in [my 'Bot Framework V4: What I learnt in 4 days in July 2018' article](https://blogs.msdn.microsoft.com/martinkearn/2018/07/17/bot-framework-v4-what-i-learnt-in-4-days-in-july-2018/). However, the Luis implementation was something I spent a lot of time on and wanted to drill into it a bit more here.

As with anything I write around emerging technology; this stuff is just a collection of my observations at the time of writing (August 2018). Your mileage may vary.

All the sample code for this article comes from my [Banko bot V4 sample](https://github.com/martinkearn/Bot-V4-Banko) which is a made-up bot based on common banking scenarios. If you want the full sample, please clone from GitHub. I'm happy to accept pull requests if you can think of improvements that remained focused on the job of demonstrating Bot v4 with Luis.

## BFv4 Luis Options; LuisRecognizer or LuisRecognizerMiddleware
In BFv4 there are two patterns for using Luis with capabilities that are built into the SDK.
* `LuisRecognizerMiddleware`: Outlined in [Using LUIS for Language Understanding](https://docs.microsoft.com/en-us/azure/bot-service/bot-builder-howto-v4-luis?view=azure-bot-service-4.0&tabs=cs)
* `LuisRecognizer`: Outlined in [Extract intents and entities using LUISGen](https://docs.microsoft.com/en-us/azure/bot-service/bot-builder-howto-v4-luisgen?view=azure-bot-service-4.0&tabs=cs)

The key difference between these two options is that one is implemented as middleware and the other is not. 

The non-middleware approach gives you a strongly typed .net object to work with whereas the middleware approach is a collection of dictionaries which are must harder to parse and traverse in your code.

When deciding which approach to use, consider that middleware executes on each and every message to your bot. You can see more about how middleware works in the [Middleware](https://docs.microsoft.com/en-us/azure/bot-service/bot-builder-concept-middleware?view=azure-bot-service-4.0) docs.

In some cases, it might make sense for every message to your bot to need natural language processing, but in most cases, Luis is only required for top level intent detection and entity resolution. Once you have the user's intent and initial entities, the bot can then launch into a dialog tree, which typically would not require Luis.

Passing every message through Luis when you don't need to will not only add unnecessary network latency to your bot, but will also become fairly expensive as your bot scales. Luis is a very cheap service for what it does, but there are still costs associated and a typical bot conversation could easily generate 10-20 API calls for just one user.

## LuisRecognizer Implementation
In my Banko scenario, I decided to use `LuisRecognizer` for top level intent and entity detection only. 

The steps for getting this setup is relatively easy and mostly documented in [Extract intents and entities using LUISGen](https://docs.microsoft.com/en-us/azure/bot-service/bot-builder-howto-v4-luisgen?view=azure-bot-service-4.0&tabs=cs) but there are some details missing from the documentation at the moment so hopefully this will fill in the gaps.

### 1: Create a Luis model
Hopefully you already know how to build your Luis model, but if not you just head to http://luis.ai, setup your intents and entities, then train and publish the model.

There are some key things to watch out for:
1. The Key and Endpoint you use both need to be on the same data center otherwise you'll get `401` responses
2. If you have provisioned Luis in Europe, you'll need http://eu.luis.ai to manage the application (I think there is a portal for Asia too but not sure what it is)

### 2: Create a class with LuisGen
This is where we create a .net class based on our Luis model.

You can use the [LUISGen tool](https://github.com/Microsoft/botbuilder-tools/tree/master/LUISGen) to generate classes that make it easier to extract entities from LUIS in your bot's code.

LuisGen is an NPM tool which you install and operate as follows:

```npm install -g luisgen```

```luisgen BankoLuisModel.json -cs Banko.BankoLuisModel -o```

This will give you a c# class which you can use to receive your Luis responses.


### 3: Use a root DialogSet to call Luis and work out intent
In your main bot file, you need to setup a `LuisRecognizer` object which you can use to to get top level intents and entities. You can then either handle them directly or create a `DialogContainer` to handle each one.

In your main bot class (the one that inherits form `IBot`), you can do something like this which gives you the main `LuisRecognizer` object to work with. :

```c#
var luisRecognizerOptions = new LuisRecognizerOptions { Verbose = true };

var luisModel = new LuisModel(
    configuration[Keys.LuisModel],
    configuration[Keys.LuisSubscriptionKey],
    new Uri(configuration[Keys.LuisUriBase]),
    LuisApiVersion.V2);

var LuisRecognizer = new LuisRecognizer(luisModel, luisRecognizerOptions, null);
```

Later on in the main bot code you can do something like this to capture the utterance from the user, call Luis and work out the intent.

```c#
var utterance = dc.Context.Activity.Text?.Trim().ToLowerInvariant();

var luisResult = await LuisRecognizer.Recognize<BankoLuisModel>(utterance, new CancellationToken());

switch (luisResult.TopIntent().intent)
{
    case BankoLuisModel.Intent.Balance:
        //do something to handle the balance intent
        break;
    case BankoLuisModel.Intent.Transfer:
        //do something to handle the transfer intent
        break;
    case BankoLuisModel.Intent.None:
    default:
        await dc.Context.SendActivity($"I dont know what you want to do.");
        await next();
        break;
}
```

### 4: Implement a DialogContainer for each intent
When you've determined the right intent from Luis, you can handle it however you want to. However, I think that `DialogContainer` is probably best practice for most scenarios.

A `DialogContainer` is similar to a `Dialog` in BFv3 and is a way of handling a specific branch of the conversation with a user. The way you progress through a dialog is new compared to BFv3 and uses a series of `WaterfallStep` which are distinct interactions between the bot and user.

You can invoke a `DialogContainer` from your top level intent handler. As an example the switch statement for handling intents may look like this:

```c#
switch (luisResult.TopIntent().intent)
{
    case BankoLuisModel.Intent.Balance:
        await dc.Begin(nameof(BalanceDialogContainer));
        break;
    case BankoLuisModel.Intent.Transfer:
        await dc.Begin(nameof(TransferDialogContainer));
        break;
    case BankoLuisModel.Intent.None:
    default:
        await dc.Context.SendActivity($"I dont know what you want to do.");
        await next();
        break;
}
```

This is a very simple example of a dialog container which simply gives the user a hard-coded balance and exits.

You can see more complete examples of the [BalanceDialogContainer.cs](https://github.com/martinkearn/Bot-V4-Banko/blob/master/DialogContainers/BalanceDialogContainer.cs) and [TransferDialogContainer.cs](https://github.com/martinkearn/Bot-V4-Banko/blob/master/DialogContainers/TransferDialogContainer.cs) from my Banko example to learn how to structure a `DialogContainer`.

```c#
public class BalanceDialogContainer : DialogContainer
{
    public static BalanceDialogContainer Instance { get; } = new BalanceDialogContainer();
    private BalanceDialogContainer() : base(nameof(BalanceDialogContainer))
    {
        this.Dialogs.Add(nameof(BalanceDialogContainer), new WaterfallStep[]
        {
            async (dc, args, next) =>
            {
            	//GetBalance is where you'd get the actual balance from your back-end system, but this is a demo
            	var balance = GetBalance();
                await dc.Context.SendActivity($"You have {balance}. What is next?");
            },
            async (dc, args, next) =>
            {
                await dc.End();
            }
        });
    }
}
```

### 5: Pass Entity as arguments
As well as intent detection, Luis is also commonly used to extract entities from the user's original utterance.

As an example, if a Banko users says _"Transfer £20 from the joint account to martin kearn on saturday"_, Luis could classify this as follows:
* Intent: _"Transfer"_
* Entities: 
    * AccountLabel: _"joint account"_
    * Money: _"£20"_
    * Date: _"Saturday"_ (more on date entities later)
    * Payee: _"martin kearn"_

The `LuisRecognizer` makes it very simple to extract the entities and pass them as an argument to your `DialogContainer` so you can work with them. In the scenario for a money transfer, the code looks like this:

```c#
case BankoLuisModel.Intent.Transfer:
    var dialogArgs = new Dictionary<string, object>();
    dialogArgs.Add(Keys.LuisArgs, luisResult.Entities);
    await dc.Begin(nameof(TransferDialogContainer), dialogArgs);
    break;
```

## Entity Validation
If you do pass entities from your main `IBot` to your `DialogContainer`, you'll want to validate them, convert any entities that have values to the correct type and store them in state so that the rest of your application can use the values.

You may typically want to discard entities that do not have values.

Bot state requires that information is stored as `Dictionary<string,object>` so I find it best to implement a static class which accepts your `_Entities` object from the `LuisRecognizer`, validates and converts each entity and returns a `Dictionary<string,object>` full of entities to be stored in bot state.

In my Banko example, the [LuisValidator.cs](https://github.com/martinkearn/Bot-V4-Banko/blob/master/Helpers/LuisValidator.cs) contains the full details but this snippet should give you the idea. 

This validates that the AccountLabel entity has a value and if it does, it adds the value to a `Dictionary<string,object>` which is returned.

```c#
public static Dictionary<string, object> LuisValidator(BankoLuisModel._Entities entities)
{
    var result = new Dictionary<string, object>();

    // Check AccountLabel
    if (entities?.AccountLabel?.Any() is true)
    {
        var accountLabel = entities.AccountLabel.FirstOrDefault(n => !string.IsNullOrWhiteSpace(n));
        if (accountLabel != null)
        {
            result[Keys.AccountLabel] = accountLabel;
        }
    }

    return result;
}
```

Within the `DialogContainer` you can call the `LuisValidator` and store the results in Bot State. You would typically do this as your first `WaterfallStep`.

```c#
async (dc, args, next) =>
{
    // Initialize state.
    if(args!=null && args.ContainsKey(Keys.LuisArgs))
    {
        // Add any LUIS entities to the active dialog state. Remove any values that don't validate, and convert the remainder to a dictionary.
        var entities = (BankoLuisModel._Entities)args[Keys.LuisArgs];
        dc.ActiveDialog.State = Validators.LuisValidator(entities);
    }
    else
    {
        // Begin without any information collected.
        dc.ActiveDialog.State = new Dictionary<string,object>();
    }

    await next();
}
```

### Resolving date entities

Typically your entities may be simple strings but they could also be more complex types such as DateTime, Money etc.

Luis uses a thing called a 'Resolution' to provide additional data with these kinds of complex entities so that you can resolve the actual values from the words the user said. For example "Saturday" may mean "18th August 2018".

Luis returns date entities to you using Json which looks a little like the following

```json
{
    "entity": "saturday",
    "type": "builtin.datetimeV2.date",
    "startIndex": 32,
    "endIndex": 39,
    "resolution": {
        "values": [
            {
                "timex": "XXXX-WXX-6",
                "type": "date",
                "value": "2018-08-18"
            },
            {
                "timex": "XXXX-WXX-6",
                "type": "date",
                "value": "2018-08-25"
            }
        ]
    }
}
```

Using this data alone, it is hard to boil this down to `DateTime` object you can work with. Fortunately, there are some helpers built into the BotBuilder SDK to help you.

The first thing you need to get is the `Timex` which is a code that can be resolved to a `DateTime` (I have no idea how this works under the hood).  

Luis actually returns several candidate dates in order of likelihood so you may want to implement some logic to determine the correct date (See the [BotBuilder Community DataTypeDisambiguation Dialog for help here](https://github.com/BotBuilderCommunity/botbuilder-community-dotnet/tree/master/libraries/Bot.Builder.Community.Dialogs.DataTypeDisambiguation)), but in this example I've just taken the first one. 

```c#
async (dc, args, next) =>
{
    // Capture Date to state
    if (!dc.ActiveDialog.State.ContainsKey(Keys.Date))
    {
        var answers = args["Resolution"] as List<DateTimeResult.DateTimeResolution>;
        var firstAnswer = answers[0];
        var timex = firstAnswer.Timex;
		var justDate = timex.Substring(0, timex.IndexOf("T"));
		var date = Convert.ToDateTime(justDate);
        dc.ActiveDialog.State[Keys.Date] = date.ToLongDateString();
    }

    await next();
},
```

Once you've implemented the above, you'll have a valid `DateTime` object stored in your bot state which you can use to action the user's request.

For the Banko implementation, I used a helper function to do the `Timex` conversion just to make things a little neater, see [TransferDialogContainer.cs](https://github.com/martinkearn/Bot-V4-Banko/blob/master/DialogContainers/TransferDialogContainer.cs) and [TimexToDateConverter.cs](https://github.com/martinkearn/Bot-V4-Banko/blob/master/Helpers/TimexToDateConverter.cs).

### Resolving currency entities

Luis has a built in entity type for currency which can accurately capture money however the user phrases it, for example all of these would resolve to a currency entity:

* "£20"
* "20.00"
* "twenty pounds"

This is the Json that comes back from Luis for currency

```json
"entity": "£20.50",
"type": "builtin.currency",
"startIndex": 19,
"endIndex": 24,
"resolution": {
    "unit": "Pound",
    "value": "20.5"
}
```

If you have built your Luis c# model using the [LUISGen tool](https://github.com/Microsoft/botbuilder-tools/tree/master/LUISGen), you will have a very useful `Microsoft.Bot.Builder.Ai.LUIS.Money[]` object to work with. 

To the actual amount, you can do a simple validation, much like we did with AccountLabel earlier on.

This is an example of how we can extend the [LuisValidator.cs](https://github.com/martinkearn/Bot-V4-Banko/blob/master/Helpers/LuisValidator.cs) from earlier to validate currency entities and convert to a `Decimal` which is much easier to work with for currency.

```C#
public static Dictionary<string, object> LuisValidator(BankoLuisModel._Entities entities)
{
    var result = new Dictionary<string, object>();

    // Check Money
    if (entities?.money?.Any() is true)
    {
        var number = entities.money.FirstOrDefault().Number;
        if (number != 0.0)
        {
            // LUIS recognizes numbers as doubles. Convert to decimal.
            result[Keys.Money] = Convert.ToDecimal(number);
        }
    }
}
```

This is all great if the user provides the currency in their initial utterance, but if you have to capture it via prompts later, you may have a problem .... more on this in the 'Capturing currency from the user with NumberPrompt' section later.

## Entity Completion via WaterfallStep

If the utterance that gets sent to Luis contains all the required entities, you are good to go with the details above around entity validation. However, no two users are the same and not everyone is going to give you everything you need in one go.

Lets examine the concept of a balance transfer; to do a balance transfer, we need 4 bits of information

* **AccountLabel**: The short name of the account the money is to be transferred from
* **Money**: The amount and currency of the transfer
* **Date**: The date the transfer should take place
* **Payee**: The person or company receiving the money

All of the following are potential utterances which Luis will resolve to the Transfer intent and contain one or more of the required entities

- **"I want to make a transfer"**; the Transfer intent without any entities.
- **"Transfer from the joint account"**; the Transfer intent with the `AccountLabel` entity.
- **"Transfer £20 from the joint account"**; the Transfer intent with the `AccountLabel` and `Money` entities.
- **"Transfer £20 from the joint account on Saturday"**; the Transfer intent with the `AccountLabel`, `Money` and `Date` entities.
- **"Transfer £20 from the joint account to martin kearn on Saturday"**; the Transfer intent with the `AccountLabel`, `Money`, `Date` and `Payee` entities.

If you have used the entity validation approach detailed above, your bot state will contain a `Dictionary<string,object>` containing all the entities that were provided by Luis. However, if you find that that not all your entities are provided, you will need to prompt the user to provide them.

You can use a `WaterfallStep` to prompt the user for a value, capture it and store it in bot state as if it were provided by Luis originally. I find it simplest to implement a different `WaterfallStep` for each message going to or from the user.

The full details of how we can validate, prompt and capture all 4 entities can be found in [TransferDialogContainer.cs](https://github.com/martinkearn/Bot-V4-Banko/blob/master/DialogContainers/TransferDialogContainer.cs) but here is a quick sample for the `AccountLabel` entity.

```c#
async (dc, args, next) =>
{
    // Verify or ask for AccountLabel
    if (dc.ActiveDialog.State.ContainsKey(Keys.AccountLabel))
    {
        await next();
    }
    else
    {
        var promptOptions = new PromptOptions(){RetryPromptString = "Which account do you want to transfer from? For exmaple Joint, Current, Savings etc"};
        await dc.Prompt(Keys.AccountLabel,"Which account?", promptOptions);
    }
},
async (dc, args, next) =>
{
    // Capture AccountLabel to state
    if (!dc.ActiveDialog.State.ContainsKey(Keys.AccountLabel))
    {
        var answer = (string)args["Value"];
        dc.ActiveDialog.State[Keys.AccountLabel] = answer;
    }

    await next();
},
```

You'll note that the we are using built in prompts to capture data from the user. In order for these to work, you'll need to add them, with their validators to the `Dialogs` collection for your `DialogContainer`. To do this you can do something like this at the bottom of the main `DialogContainer` constructor

```c#
// Add the prompts and child dialogs
this.Dialogs.Add(Keys.AccountLabel, new Microsoft.Bot.Builder.Dialogs.TextPrompt());

this.Dialogs.Add(Keys.Money, new Microsoft.Bot.Builder.Dialogs.NumberPrompt<int>(Culture.English, Validators.MoneyValidator));

this.Dialogs.Add(Keys.Date, new Microsoft.Bot.Builder.Dialogs.DateTimePrompt(Culture.English, Validators.DateTimeValidator));

this.Dialogs.Add(Keys.Payee, new Microsoft.Bot.Builder.Dialogs.TextPrompt());

this.Dialogs.Add(Keys.Confirm, new Microsoft.Bot.Builder.Dialogs.ConfirmPrompt(Culture.English));
```

Notice how we're using validators to help the prompt validate the answer given? These can be found in the [Helpers](https://github.com/martinkearn/Bot-V4-Banko/tree/master/Helpers) folder.

### Capturing currency from the user with NumberPrompt

The bot framework provides `Prompt` classes which help you gather specific data types from the user. These are great for entity completion as detailed above, however I encountered an issues with currency which I've not yet been able to resolve.

The best matching `Prompt` for currency is the `NumberPrompt` which captures a number from the user. However this number is returned as an `Int` not a `Double` or `Float` which is required to work with currency.

I've not resolved this issue in my Banko sample, but I suspect that the way you'd tackle this is by creating your own prompt as detailed in [Prompt users for input using your own prompts](https://docs.microsoft.com/en-us/azure/bot-service/bot-builder-primitive-prompts?view=azure-bot-service-4.0&tabs=csharp). I'm open to pull requests on [Banko](https://github.com/martinkearn/Bot-V4-Banko) if anyone wants to write that!? :)

## In Summary

To summarise, there are several options for using Luis with the BFv4 and the right approach will depending on your application. 

For [Banko](https://github.com/martinkearn/Bot-V4-Banko) I elected to use the `LuisRecognizer` because I only wanted to use Luis for top level intent detection and initial entity extraction.

Once you have a Luis response you can use a `DialogContainer` to interact with your user through a series of `WaterfallStep` and `Dialog` objects.

There are some definitive gotchas along the way, but I've tried to capture what I learnt about it in this article, your mileage may vary
