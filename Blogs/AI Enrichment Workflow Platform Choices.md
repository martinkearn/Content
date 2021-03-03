---
title: AI Enrichment Pipelines - Platform choices and considerations
author: Martin Kearn
description: There are many scenarios where and AI-based enrichment pipline is applicable; anywhere when you need to enrich data with insight from AI services. Azure offers three main ways to do this, Logic Apps, Cognitive Search and Durable Functions. This articles compares and contrasts these platforms to help you decide which one suits your requirements
image: https://github.com/martinkearn/Content/raw/master/Blogs/Images/EnrichmentPipline.png
thumbnail: https://github.com/martinkearn/Content/raw/master/Blogs/Images/EnrichmentPipline_thumb.png
type: article
status: draft
published: 2021/03/03 08:00:00
categories: 
  - Durable Functions
  - Azure Functions
  - Cognitive Search
  - Logic Apps
  - Cognitive Services
  - Serverless
---

.An AI enrichment pipline is something that takes data and creates additional insights on that data by processing it through AI services before submitting it to a final data store or index. This type of system is also known as "knowledge mining".

The data could be anything from a database, images, videos, documents or even just JSON. In this context, the term "AI services" refers mainly to [Azure Cognitive Services](https://docs.microsoft.com/en-us/azure/cognitive-services/) but in theory these services could be substituted with any AI service that provides enrichment including AI services by other vendors and home-grown APIs or machine learning systems. 

In terms of Azure Cognitive Services, there are many, many use cases, here are some of the more popular ones used in AI enrichment pipelines:

- Getting insights from videos using [Video Indexer](https://docs.microsoft.com/en-us/azure/media-services/video-indexer/)
- Describing and image, including tags and other metadata from images using [Computer Vision](https://docs.microsoft.com/en-us/azure/cognitive-services/computer-vision/) or [Custom Vision](https://docs.microsoft.com/en-us/azure/cognitive-services/custom-vision-service/)
- Translating text content from documents or metadata with [Translator](https://docs.microsoft.com/en-us/azure/cognitive-services/translator/)
- Identifying faces from a list of known faces with the [Face](https://docs.microsoft.com/en-us/azure/cognitive-services/face/) 
- Analysing audio recording of phone calls for intent and entity extraction using [Language Understanding](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/)
- Transcribing voice with [Speech Service](https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/)
- Extracting data from forms, receipts and invoices using [Form Recogniser](https://docs.microsoft.com/en-us/azure/cognitive-services/form-recognizer/)
- Understanding sentiment, keywords and phrases from text using [Text Analytics](https://docs.microsoft.com/en-us/azure/cognitive-services/text-analytics/)
- Moderating content for adult or racy content with [Content Moderator](https://docs.microsoft.com/en-us/azure/cognitive-services/content-moderator/)
- At the time or writing there are 16 different cognitive service with 100's of functions across them. Get the broad overview at [What are Azure Cognitive Services?](https://docs.microsoft.com/en-us/azure/cognitive-services/what-are-cognitive-services)

In the context of Azure, there are a few options for how you build such a system, the three main ones are

- [Azure Logic Apps](https://docs.microsoft.com/en-us/azure/logic-apps/logic-apps-overview)
- [Azure Cognitive Search](https://docs.microsoft.com/en-gb/azure/search/search-what-is-azure-search)
- [Azure Durable Functions](https://docs.microsoft.com/en-us/azure/azure-functions/durable/durable-functions-overview?tabs=csharp)

This article seeks to provide an overview of these options to help you understand the pros and cons of each and determine which is most suitable for your application.

The knowledge gained here is built from the experience of building a real-world knowledge mining system which takes millions of videos, digital text files and images, adds business critical enrichments via Cognitive Services before exporting to [Relativity](https://www.relativity.com/) which is an eDiscovery and case management system.

## Azure Logic Apps

## Azure Durable Functions

## Azure Cognitive Search

## Platform Choice Considerations

## In Summary

- 
- More articles from me: http://martink.me/articles

