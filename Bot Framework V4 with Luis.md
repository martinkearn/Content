# Bot Framework v4 with Luis
The [Cognitive Services Language Understanding Intelligence Services (Luis)](https://azure.microsoft.com/en-us/services/cognitive-services/language-understanding-intelligent-service/) should be no stranger to bot developers; it enables the all-important natural language processing which means users can 'converse' with your bot in the way they would with another human; by using natural language.

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

### 1: Create a Luis model

### 2: Create a class with LuisGen

### 3: Use a root DialogSet to call Luis and work out intent

### 4: Implement a DialogContainer for each intent

### 5: Pass Entity as arguments

## Entity Validation

### TimexRangeResolver for resolving date entities

### A word about currency

## Entity Completion via WaterfallStep

## In Summary