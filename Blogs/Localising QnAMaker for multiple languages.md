---
title: Localising QnAMaker for multiple languages
author: Martin Kearn
description: QnAMaker is great unless you need to support multiple languages where there is no built-in solution. In this article, I'll explain how I've implemented localized QnAMaker from a single model with Logic Apps, the Translator Text API and an Azure Function.
image: https://dummyimage.com/800x600/000/fff&text=placeholder
thumbnail: https://dummyimage.com/200x200/000/fff&text=placeholder
type: article
status: draft
published: 2020/01/03 16:30:00
categories: 
- Cognitive Services
- Localisation
- Logic Apps
- Azure Functions
- Serverless
- Bots
- QnAMaker
---

# Localising QnAMaker for multiple languages

QnAMaker is a great service for enabling users to ask 100's of frequently asked questions using natural language. However, if you need to support multiple languages, there are a few different approaches you can take, which are as follows:

1. Support only your main language
2. Create multiple QnAMaker knowledge bases (Kb); one for each language and have your application work out which one to call
3. Keep a single Kb and use translation

I've worked on several bot projects recently which had requirements for multiple languages and in all cases, we used option 3. 

This article will discuss the implementation details of keeping a single QnAMaker Kb in your main language and using translation to support multiple languages. We will cover:

- The benefits of a single QnAMaker kb in your main language.
- Using a Logic App to orchestrate the various APIs calls; why and how.
- How to detect which language the user's question is in.
- How to translate the question to the language of the QnAMaker Kb.
- How to translate the QnAMaker Kb answer to language the question was asked in.

The technology involved includes:

- Cognitive Services QnAMaker 
- Cognitive Service Translator Text
- Azure Logic Apps
- Azure Functions

This article is accompanied by the [Multilingual-QnAMaker GitHub repo](https://github.com/martinkearn/Multilingual-QnAMaker), which contains code samples for all the code referenced.

## The benefit of a single QnAMaker Kb

The approach used in this article advocates a single QnAMaker Kb which is in your customer's main language. Often, this is English and for the purposes of this article, we'll say that English is the main language but French, Spanish and German language support is also required.

Using multiple KB's is a valid approach but here are some reason I found for not taking that approach.

**Maintenance**: QnAMaker can contain up to 2999 knowledge bases, each of which can have up to 25,000 characters of answer text. This is a significant corpus of content and maintaining this across multiple languages is a time consuming human effort, especially if content is regularly updated.

**Cost**: QnAMaker has a cost associated with it, therefore the fewer knowledge bases will be more cost effective.

**Code Complexity**: If you have multiple QnA Kb's for different languages, your application must contain logic to know which Kb to call for answers and it must also store multiple keys and Kb ID's

All of the above issues can be mitigated by having a single QnA Kb in your main language.

## The Translate Text API

The [Cognitive Services Translate Text API](https://azure.microsoft.com/en-us/services/cognitive-services/translator-text-api/) (Translate API) is a rich API that deals with text-based translation to and from [64 languages](https://docs.microsoft.com/en-us/azure/cognitive-services/translator/language-support), many with [neural machine translation](https://www.microsoft.com/en-us/translator/business/machine-translation/#nnt) which is much more natural than traditional machine-learning based translation services.

The Translate API support language detection as well as translation.

The Translate API is available with platform SDKs as well as regular REST API calls. There is also a [Logic App connector](https://docs.microsoft.com/en-us/connectors/translatorv2/) which makes it very easy to integrate the API into an Azure Logic App.

## The Logic App; why and how

[Azure Logic Apps](https://docs.microsoft.com/en-us/azure/logic-apps/logic-apps-overview) are wonderful no-code orchestration tool that lets you connect 1000's of different API and services via connectors.

I'm leaning towards always using Logic Apps whenever multiple API's need to be chained together. It removes code from the application layer and improves performance because the APIs call are all made in the Azure data center.

In this particular scenario, we are going to use a Logic App to chain the following tasks:

1. Post a question in any supported language
2. Detect the question language
3. Translate to English
4. Get the answer from QnAMaker
5. Translate the answer to the question's language
6. Return the QnAMaker JSON response with  the answer translated

It is not within scope of this article to go through the detail of how to use Logic Apps, but we will look at each step, what it does and what the inputs are.

### 1. Pre-create API's

You will need to pre-create an instance of the Translator Text API in the same resource group as the Logic App. Make sure you capture the `key` and `name` which you can get from the keys Azure Portal blade.

We'll also need to pre-create an QnAMaker and capture the following:

- `ConnectionName` (from `Name` in Azure Portal > Keys blade)
- `APIKey` (from `Key1` in Azure Portal > Keys blade)
- `Knowledge Base Id` (From the `?kbid=` in the qnamaker.ai url)
- `Service Host` (From `Host` in qnamker.ai > settings)
- `Endpoint Key` (From `Authorization` in qnamker.ai > settings)

### 2. HTTP Request Payload Schema

You need to define a JSON payload schema for the HTTP POST which triggers the logic app. This only needs to contain the question so something like this is fine. This will give you a dynamic data property called `question` to use later on.

```
{"question":"what is your name?"}
```

**Top Tip**: Make sure you name each logic app connection as you go along so that it is clear which connection you are using when selecting dynamic content. This is especially true when you have several instances of the same connector.

### 3. Variables

We'll use Logic App variables to store variable things throughout the app. 

This is fairly simple and self-explanatory, but be careful to name both the variable and the variable actions appropriately; otherwise it gets very confusing later on.

You'll need to initialize the following variables:

- `QuestionLanguage`, string, no default value

### 4. Detect Question Language

Use the `Microsoft Translator V2` Logic App connection and add the `key` and `name` from earlier. You'll then be able to choose from three actions:

- Detect language
- Get languages
- Translate text

Initially we need to detect the language of the `question` dynamic content from the HTTP request connection and store it in the `QuestionLanguage` variable. This will be used later on when translating English the answer from QnAMaker.

### 5. Translate to English

The next step is to translate the question to English. For this, we use the `Microsoft Translator V2` Logic App connection again but this time the `Translate text` action. 

Set the `Text` to the `question` dynamic content from the HTTP request connection and set the `Target Language` to English (or whatever language your QnA Kb is in). 

This will translate the text to English from whatever language the user asked it in. If the text is already English, the API will simply return the same text.

### 6. Get QnA answer

Next we use the `QnA Maker` Logic App connection to get an answer via the `Generate answer` action. You can use the keys etc from earlier and for the `question` field use the `translated text` dynamic content from the second  `Microsoft Translator V2` connection.

This is the final Logic App in designer view, you can also see the JSON 'code' for it in the [Multilingual-QnAMaker GitHub repo](https://github.com/martinkearn/Multilingual-QnAMaker/blob/master/LogicApp/TranslateQnALogicApp.json) which accompanies this article. 

### 7. Translate the answer

The QnA answer will be in English and we now need to translate the answer to the same language that the user asked the question in.

This is the only part of the Logic App which require custom code. The reason for this is that we are unable to select the first Answer in the array of answers returned by QnAMaker using Logic Apps. There may be a way to do this but I could not find it.

To address this requirement, I have written an Azure Function which takes a QNA response as it's body input as well as a parameter called `translateToLanguageCode` which has a value that represents the language code that the answer should be translated to.

The function returns the QBA JSON payload that was passed in but with the answer translated to eth language which was specified.

It is not within scope of this article to talk through the code of this function, but it is very simple and C# and NodeJS code can be seen at the [Multilingual-QnAMaker GitHub repo](https://github.com/martinkearn/Multilingual-QnAMaker/blob/master/LogicApp/TranslateQnALogicApp.json) which accompanies this article.

### 8. Return the JSON

Finally, the logic app returns the Json (with the translated answer) to the calling applications.

The end result is exactly eth same as if the application had called QnAMaker directly, however the calling application can call in any supported language and have the answer returned in eth language they call in

### The overall Logic App

This is a screen shot of the designer view for the logic app.

![Logic App designer view](https://github.com/martinkearn/Content/raw/master/Blogs/Images/TranslateQnALogicApp%20-%20Designer%20View.PNG)



## In Summary

In summary, the approach outlined in this article is a great way to make sure all user can benefit from the content in your QnAMaker KB in their native language, without having to ensure the overhead of supporting multiple Kb's.

Additionally, this approach is a great example of how a serverless architecture with Logic Apps and Functions can be used in an appropriate way as part of a broader system such as a bot.

## Further Reading

You may find these resources useful.

- [Cognitive Services Product Page](https://azure.microsoft.com/en-us/services/cognitive-services/)
- [Multilingual-QnAMaker GitHub repo](https://github.com/martinkearn/Multilingual-QnAMaker) 
