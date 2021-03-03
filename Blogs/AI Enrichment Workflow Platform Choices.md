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

## Azure Cognitive Search

Arguably, Azure Cognitive Search is the natural choice for building a knowledge mining application on Azure. 

Cognitive Search is a system that will index content from a Data Source. This can be binary data such as files on a storage account, relational data in a database or non-relational data stored in something like Azure Cosmos Database.

Through the indexing pipeline, Cognitive Search can use [AI enrichment skills](https://docs.microsoft.com/en-gb/azure/search/cognitive-search-concept-intro) to pass the data to a range of Cognitive Services to be "enriched" and then store the resulting enrichments alongside the search document. When users do a search using the front-end attached to Cognitive Search, they will be searching on the enriched data as well as the origional content.

> "Data" can also mean cracking open the contents of a binary file such as a Word doucment and enriching the text within it

Cognitive Search also has a range of built-in skills which can transform the content as it is indexed, this includes operations like splitting text or shaping data into an expected schema.

Cognitive Search has built-in connections to some Cognitive Services to do things like:

- [Entity extraction with the Text Analytics service](https://docs.microsoft.com/en-gb/azure/search/cognitive-search-skill-entity-recognition)
- [Image Analysis with the Computer Vision service](https://docs.microsoft.com/en-gb/azure/search/cognitive-search-skill-image-analysis)
- [Language detection with the Text Analytics service](https://docs.microsoft.com/en-gb/azure/search/cognitive-search-skill-language-detection)
- [Optical character recognition (OCR) with the Computer Vision service](https://docs.microsoft.com/en-gb/azure/search/cognitive-search-skill-ocr)

If the service you want to use is not available as a cognitive search skill, you can create custom skills which either work with a [web API](https://docs.microsoft.com/en-gb/azure/search/cognitive-search-custom-skill-web-api) (which could be any other Cognitive Service) or an [Azure Machine Learning](https://docs.microsoft.com/en-gb/azure/search/cognitive-search-aml-skill) model.

Cognitive Search applications are built largely by configuring the Cognitive Search pipeline (Data Sources, Indexers, Skillsets) which is undertaken through the portal for most scenarios, but there are some things that can only be done through API calls to the Cognitive Search configuration APIs. You also need to build or connect your front-end to the search APIs provided by the service. all though the service does provide some very basic HTML templates.

There are also accelerator solutions you can use as a starting point:

- [Knowledge Mining Solution Accelerator](https://docs.microsoft.com/en-us/samples/azure-samples/azure-search-knowledge-mining/azure-search-knowledge-mining/)
- [COVID-19 Search App](https://github.com/liamca/covid19search)
- [The JFK Files](https://github.com/Microsoft/AzureSearch_JFK_Files), see https://jfk-demo.azurewebsites.net

## Azure Logic Apps

## Azure Durable Functions

## Platform Choice Considerations

## In Summary

- 
- More articles from me: http://martink.me/articles

