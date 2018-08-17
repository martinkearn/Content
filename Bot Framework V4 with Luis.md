# Bot Framework v4 with Luis
The [Cognitive Services Language Understanding Intelligence Services (Luis)](https://azure.microsoft.com/en-us/services/cognitive-services/language-understanding-intelligent-service/) should be no stranger to bot developers; it enables the all-important natural language processing which means users can converse with your bot in the way they would with another human; by using natural language.

With Bot Framework V3 (BFv3), there is a well prescribed pattern for using Luis in your bot via the `LuisDialog` which gives you a nicely packaged object to work with, abstracts the Luis API call and handles the top level intents.

Bot Framework V4 (BFv4) is a bit more complex.

I did some work on BFv4 a few weeks ago. I wrote about my main initial observations in [my 'Bot Framework V4: What I learnt in 4 days in July 2018' article](https://blogs.msdn.microsoft.com/martinkearn/2018/07/17/bot-framework-v4-what-i-learnt-in-4-days-in-july-2018/). However the Luis implementation was something I spent a lot of time on and wanted to drill into it a bit more here.

As with anything I write around emerging technology; this stuff is just a collection of my observations at the time of writing (August 2018). Your mileage may vary.

## BFv4 Luis Options; LuisRecognizer or LuisRecognizerMiddleware
In BFv4 there are two patterns for using Luis with capabilities that are built into the SDK.
* `LuisRecognizerMiddleware`: Outlined in [Using LUIS for Language Understanding](https://docs.microsoft.com/en-us/azure/bot-service/bot-builder-howto-v4-luis?view=azure-bot-service-4.0&tabs=cs)
* `LuisRecognizer`: Outlined in [Extract intents and entities using LUISGen](https://docs.microsoft.com/en-us/azure/bot-service/bot-builder-howto-v4-luisgen?view=azure-bot-service-4.0&tabs=cs)

The key difference between these two options is that one is implemented as middleware and the other is not. The non-middleware approach also gives you a strongly typed .net object to work with whereas the middleware approach is a collection of dictionaries which are must harder to parse and traverse in your code.

**Key Fact**: Middleware executes on each and every message to your bot. You can see more about how middleware works in the [Middleware](https://docs.microsoft.com/en-us/azure/bot-service/bot-builder-concept-middleware?view=azure-bot-service-4.0) docs.

In some cases, it might make sense for every message to your bot to need natural language processing, but in most cases, Luis is only required for top level intent detection and entity resolution. Once you have the user's intent and initial entities, the bot can then launch into a dialog tree, which typically would not require Luis.

Passing every message through Luis when you don't need to will not only add unnecessary network latency to your bot, but will also become fairly expensive as your bot scales. Luis is a very cheap service for what it does, but there are still costs associated and a typical bot conversation could easily generate 10-20 API calls for just one user.

## LuisRecognizer Implementation
In my scenario, I chose to use `LuisRecognizer` for top level intent and entity detection only. The steps for getting this setup is relatively easy and mostly documented in [Extract intents and entities using LUISGen](https://docs.microsoft.com/en-us/azure/bot-service/bot-builder-howto-v4-luisgen?view=azure-bot-service-4.0&tabs=cs) but there are some details missing from the documentation at the moment so hopefully this will fill in the gaps.

### 1: Create a Luis model
Hopefully you already know how to build your Luis model, but if not you just head to http://luis.ai, setup your intents and entities, then train and publish the model.

There are some key things to watch out for:
1. The Key and Endpoint you use both need to be on the same data center otherwise you'll get `401` responses
2. If you have provisioned Luis in Europe, you'll need http://eu.luis.ai to manage the application (I think there is a portal for Asia too but not sure what it is)

### 2: Create a class with LuisGen
This is where it starts to get interesting and we create a .net class based on our Luis model.

You can use the [LUISGen tool](https://github.com/Microsoft/botbuilder-tools/tree/master/LUISGen) to generate classes that make it easier to extract entities from LUIS in your bot's code.

LuisGen is an NPM tool which you install and operate as follows

```npm install -g luisgen```

```luisgen BankoLuisModel.json -cs Banko.BankoLuisModel -o```

This will give you a c# class which you can use to receive your Luis responses.


### 3: Use a root DialogSet to call Luis and work out intent
> NOTE: At this point, I'll mention that all the sample code for this article from this point onwards comes from my [Banko bot V4 sample](https://github.com/martinkearn/Bot-V4-Banko). If you want the full sample, please clone that from GitHub.

In your main bot file, you need to setup a `LuisRecognizer` object which you can use to to get top level intents and entities. You can then either handle them directly or spin up a `DialogContainer` to handle each one.

In your main bot class (the one that inherits form `IBot`), you can do something like this:

```
var luisRecognizerOptions = new LuisRecognizerOptions { Verbose = true };

var luisModel = new LuisModel(
    configuration[Keys.LuisModel],
    configuration[Keys.LuisSubscriptionKey],
    new Uri(configuration[Keys.LuisUriBase]),
    LuisApiVersion.V2);

var LuisRecognizer = new LuisRecognizer(luisModel, luisRecognizerOptions, null);
```

This gives you the main `LuisRecognizer` object to work with. Later on in the main bot code you can do something like this to capture the utterance from the user, call Luis and work out the intent.

```
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

A `DialogContainer` is similar to a `Dialog` in BFv3 and is essentially a way of handling a specific branch of the conversation with a user. The way you progress through a dialog is new compared to BFv3 and uses a series `WaterfallStep` which are distinct interactions between the bot and user.

This is a very simple example of a dialog container which simply says "OK, we're done here. What is next?" and exits. You can see more complete examples of the [BalanceDialogContainer.cs](https://github.com/martinkearn/Bot-V4-Banko/blob/master/DialogContainers/BalanceDialogContainer.cs) and [TransferDialogContainer.cs](https://github.com/martinkearn/Bot-V4-Banko/blob/master/DialogContainers/TransferDialogContainer.cs) from my banko example to learn how to structure a `DialogContainer`.

```
public class BalanceDialogContainer : DialogContainer
{
    public static BalanceDialogContainer Instance { get; } = new BalanceDialogContainer();
    private BalanceDialogContainer() : base(nameof(BalanceDialogContainer))
    {
        this.Dialogs.Add(nameof(BalanceDialogContainer), new WaterfallStep[]
        {
            async (dc, args, next) =>
            {
                await dc.Context.SendActivity("OK, we're done here. What is next?");
            },
            async (dc, args, next) =>
            {
                await dc.End();
            }
        });
    }
}
```

You can invoke a `DialogContainer` from your top level intent handler. As an example the switch statement for handling intents may look like this

```
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

```
case BankoLuisModel.Intent.Transfer:
    var dialogArgs = new Dictionary<string, object>();
    dialogArgs.Add(Keys.LuisArgs, luisResult.Entities);
    await dc.Begin(nameof(TransferDialogContainer), dialogArgs);
    break;
```

## Entity Validation
If you do pass entities from your main `IBot` to your `DialogContainer`, you'll want to validate them, convert any entities that have values to the correct type and store them in state so that the rest of your application can use the values.

You may typically want to discard entities that do not have values.

Bot state requires that information is stored as `Dictionary<string, object>` so I find it best to implement a static class which accepts your `_Entities` object from the `LuisRecognizer`, validates and converts each entity and returns a `Dictionary<string, object>` full of entities to be stored in bot state. 

In my Banko example, the [LuisValidator.cs](https://github.com/martinkearn/Bot-V4-Banko/blob/master/Helpers/LuisValidator.cs) contains the full details but this snippet should give you the idea. This validates that the AccountLabel entity has a value and if it does it adds the value to a `Dictionary<string, object>` which is returned and stored in Bot state.

```
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

```
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
},

Typically your entities may be simple strings but they could also be more complex types such as DateTime, Money etc.

Luis uses a thing called a 'Resolution' to provide additional data with these kinds of complex entities so that you can resolve the actual values from the words the user said. For example "Saturday" may mean "18th August 2018". 

For these kind of entities you need special validators to get the data you'll need for your bot code.

### TimexRangeResolver for resolving date entities

### A word about currency

## Entity Completion via WaterfallStep

## In Summary