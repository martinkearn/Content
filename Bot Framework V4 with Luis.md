# Bot Framework v4 with Luis
The [Cognitive Services Language Understanding Intelligence Services (Luis)](https://azure.microsoft.com/en-us/services/cognitive-services/language-understanding-intelligent-service/) should be no stranger to bot developers; it enables the all-important natural language processing which means users can 'converse' with your bot in the way they would with another human; by using natural language.

With Bot Framework V3 (BFv3), there is a well prescribed pattern for using Luis in your bot via the `LuisDialog` which gives you a nicely packaged object to work with, abstracts the Luis API call and handles the top level intents.

Bot Framework V4 (BFv4) is a bit more complex.

I did some work on BFv4 a few weeks ago. I wrote about my main initial observations in [my 'Bot Framework V4: What I learnt in 4 days in July 2018' article](https://blogs.msdn.microsoft.com/martinkearn/2018/07/17/bot-framework-v4-what-i-learnt-in-4-days-in-july-2018/). However the Luis implementation was something I spent a lot of time on and wanted to drill into it a bit more here.

## BFv4 Luis Options; LuisRecognizer or LuisRecognizerMiddleware

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