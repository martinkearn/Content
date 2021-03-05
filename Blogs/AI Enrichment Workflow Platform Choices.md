---
title: AI enrichment pipeline platform choices and considerations
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

## What is Azure Cognitive Search?

Arguably, Azure Cognitive Search is the natural choice for building a knowledge mining application on Azure. 

Cognitive Search is a system that will index content from a Data Source. This can be binary data such as files on a storage account or data in a database.

Through the indexing pipeline, Cognitive Search can use [AI enrichment skills](https://docs.microsoft.com/en-gb/azure/search/cognitive-search-concept-intro) to pass the data to a range of Cognitive Services to be "enriched" and then store the resulting enrichments alongside the search document. When users do a search using the front-end attached to Cognitive Search, they will be searching on the enriched data as well as the origional content.

> "Data" can also mean cracking open the contents of a binary file such as a Word document and enriching the text within it

Cognitive Search has built-in connections to some Cognitive Services to do things like:

- [Entity extraction with the Text Analytics service](https://docs.microsoft.com/en-gb/azure/search/cognitive-search-skill-entity-recognition)
- [Image Analysis with the Computer Vision service](https://docs.microsoft.com/en-gb/azure/search/cognitive-search-skill-image-analysis)
- [Language detection with the Text Analytics service](https://docs.microsoft.com/en-gb/azure/search/cognitive-search-skill-language-detection)
- [Optical character recognition (OCR) with the Computer Vision service](https://docs.microsoft.com/en-gb/azure/search/cognitive-search-skill-ocr)

In addition to the AI enrichment skills, Cognitive Search has a range of built-in skills which can transform the content as it is indexed, this includes operations like splitting text or shaping data into an expected schema.

If the operation you need is not available as a skill, you can create custom skills which either work with a [web API](https://docs.microsoft.com/en-gb/azure/search/cognitive-search-custom-skill-web-api) (which could be any other Cognitive Service or a custom API/Azure Function) or an [Azure Machine Learning](https://docs.microsoft.com/en-gb/azure/search/cognitive-search-aml-skill) model.

Cognitive Search applications are built largely by configuring the Cognitive Search pipeline (Data Sources, Indexers, Skillsets) which is undertaken through the portal for most scenarios, but there are some things that can only be done through API calls to the Cognitive Search configuration APIs. You also need to build or connect your front-end to the search APIs provided by the service. all though the service does provide some very basic HTML templates.

The Cognitive Search pipline is largely a linear pipeline where skills are executed one after the other. There are few opportunities for complex conditional logic or branching. This would have to be achieved by some level of pre-processing or custom skills.

The Cognitive Search pipline has a [maximum processing timeout of 230 seconds](https://docs.microsoft.com/en-us/azure/search/cognitive-search-custom-skill-web-api#skill-parameters) for each custom web API skill request. This make it unsuitable for long running indexing operations.

There are also accelerator solutions you can use as a starting point:

- [Knowledge Mining Solution Accelerator](https://docs.microsoft.com/en-us/samples/azure-samples/azure-search-knowledge-mining/azure-search-knowledge-mining/)
- [COVID-19 Search App](https://github.com/liamca/covid19search)
- [The JFK Files](https://github.com/Microsoft/AzureSearch_JFK_Files), see https://jfk-demo.azurewebsites.net

## What is Azure Logic Apps?

[Azure Logic Apps](https://azure.microsoft.com/en-gb/services/logic-apps/) is a declarative workflow platform which features [100's of connectors](https://docs.microsoft.com/en-us/connectors/) to external services including services on Azure, Microsoft 365 and the broader Microsoft SaaS ecosystem as well as popular external services such as Twitter, Dropbox, GitHub, Google Docs, Salesforce and many more.

Included in the long list of connectors are connectors for most of the popular Cognitive Services including Video Indexer, Computer Vision, Translator and Text Analytics (plus more). You can also call any HTTP endpoint via the built-in HTTP connector which enables Logic Apps to call Cognitive Services that do not have a connector, custom Azure Functions or any HTTP endpoint.

Logic Apps also feature a wide range of [triggers](https://docs.microsoft.com/en-us/azure/connectors/apis-list#triggers-and-action-types) which is how the logic app gets started. For example, a Logic App can trigger in response to a Http request, a service bus message, a file in Azure storage or on a schedule.

Logic Apps feature rich branching and conditional logic which enables the provision of complex workflows. The workflow itself is created using a visual designer via the Azure Portal (the [Logic Apps Preview](https://docs.microsoft.com/en-us/azure/logic-apps/logic-apps-overview-preview) also has a Visual Studio Code editing extension).

The rich range of connectors and conditional capabilities make Logic Apps a great choice for complex enrichment pipelines which produce enrichment data which can easily be exported to a downstream system which a front end can interact with.

Logic Apps do not have any built-in incremental indexing capabilities. This means unlike Cognitive Search which can update its enrichments from a given data source many times, Logic Apps would need to be entirely re-run and the export process carefully controlled to achieve the same. This makes Logic Apps suitable for 'one shot' content which rarely changed but less applicable to data sources that feature regular changing or updating content

Logic Apps are source controlled and deployed by exporting an [Azure Resource Manager (ARM) template](https://docs.microsoft.com/en-us/azure/logic-apps/logic-apps-azure-resource-manager-templates-overview). This can be deployed using Infrastructure-as-code (IaC) tools such as Terraform, however the introduction of variables from Terraform (for example, injecting the ID of a Video Indexer service deployed by Terraform) is a tricky process and prone to error.

Logic Apps do not have code in the traditional sense, but the definition is a JSON document. The fact that the entire definition is JSON make source control and merge conflict resolution problematic when large teams are working concurrently on the same Logic App.

## Azure Durable Functions

An [Azure Function](https://docs.microsoft.com/en-us/azure/azure-functions/functions-overview) at its simplest level is a piece of code that lives in Azure and can be triggered via a range of [bindings](https://docs.microsoft.com/en-us/azure/azure-functions/functions-triggers-bindings?tabs=csharp) and can output its results as a to other systems as an output bindings (including HTTP request and response). 

Functions are a central tenant of Azure's serverless offering and removes the need to worry about servers or infrastructure. Customers pay for Functions according to consumption based on number of executions, execution time, and memory used.

Functions support multiple programming languages including C#, Java, JavaScript, TypeScript and Python.

[Azure Durable Functions](https://docs.microsoft.com/en-us/azure/azure-functions/durable/durable-functions-overview?tabs=csharp) is an extension to the Functions runtime which makes Functions stateful and long-running. Durable Functions use an [Orchestrator Function](https://docs.microsoft.com/en-us/azure/azure-functions/durable/durable-functions-types-features-overview#orchestrator-functions) which is used to orchestrate calls to 'child functions' (known as [Activity Functions](https://docs.microsoft.com/en-us/azure/azure-functions/durable/durable-functions-types-features-overview#activity-functions)) and can store state while the Function is waiting for external input or long running operations to complete.

Durable Functions have a concept of [Durable Entities](https://docs.microsoft.com/en-us/azure/azure-functions/durable/durable-functions-entities?tabs=csharp) which are small pieces of state which can be used to track progress or data points for the overall Durable Function orchestration.

Queue management, state persistence and serialisation are all managed by the Durable Function runtime, see [Data persistence and serialization in Durable Functions](https://docs.microsoft.com/en-us/azure/azure-functions/durable/durable-functions-serialization-and-persistence?tabs=csharp).

Durable Functions can support several popular architectural patterns, including:

- [Function chaining](https://docs.microsoft.com/en-us/azure/azure-functions/durable/durable-functions-overview?tabs=csharp#chaining)
- [Fan-out/fan-in](https://docs.microsoft.com/en-us/azure/azure-functions/durable/durable-functions-overview?tabs=csharp#fan-in-out)
- [Async HTTP APIs](https://docs.microsoft.com/en-us/azure/azure-functions/durable/durable-functions-overview?tabs=csharp#async-http)
- [Monitoring](https://docs.microsoft.com/en-us/azure/azure-functions/durable/durable-functions-overview?tabs=csharp#monitoring)
- [Human interaction](https://docs.microsoft.com/en-us/azure/azure-functions/durable/durable-functions-overview?tabs=csharp#human)
- [Aggregator (stateful entities)](https://docs.microsoft.com/en-us/azure/azure-functions/durable/durable-functions-overview?tabs=csharp#aggregator)

Durable Functions make a great choice for AI-enrichment workflows where the workflow is particularly complex, there is a preference of code over configuration or declarative mark-up or where other workflow platforms do not offer the required functionality and would need custom code (typically Functions) to be meet the requirements.

Because Durable Functions are 'just' a Function App they can be easily source controlled, CI/CD'd and deployed using tool like Terraform. The code lives in a repository and can benefit from all the advantages of Git for collaborative development.

Durable Functions have no built-in observability in terms of where items are in the overall workflow. However, because they are simply code, tools like Application Insights and/or Log Analytics can easily be integrated. Dashboards can be created to give the required views on the data.

Like Logic Apps, Durable Functions do not have any built-in incremental indexing capabilities (like Cognitive Search does).

It would be possible to build an equivalent Durable Function workflow using regular Functions, a queueing technology such as Service Bus and a storage technology such as Cosmos database. However, you would be losing some of the great advantages that Durable Functions offer around built-in queueing and state management.

## Platform Choice Considerations

When choosing the workflow platform, there are many factors that should be considered; each platform has strengths and weaknesses. 

This section will discuss some of the key differences of each platform in the context of each main consideration.

> For improved readability, Logic Apps will be referred to as "LA", Cognitive Search as "CS" and Durable Functions as "DF" throughout this section.

### Code vs config vs declarative mark-up

> TLDR: You will need definitely developers to build DF workflows which are 100% code based. You will probably also need developers for CS and LA which are json and configuration based.

Each of the three platforms have a very distinct approach in how applications are created. This could be a critical consideration and may force a particular route depending on the technical expertise you have at your disposal.

DF is a code-based platform. The advantage is that you have ultimate control and flexibility on the solution, but it also means that you have to code features that LA and CS give you by default. The code can be written in a choice of languages including C# (generally the default and the language where you'll find the most community support), JavaScript, Python and PowerShell. In order to build a workflow platform with DF, you will need developers that can write code using one of these languages.

LA is built using declarative mark-up which is configured via the portal and exports as JSON. To build complex workflows with LA, you will need developers that are competent editing JSON files. There is also an additional burden around IaC technologies such as Terraform with LA (see the IaC, CI/CD and deployment section below). For most complex workflows, there will be occasions where you will need custom functionality which typicaly takes the form of an Azure Function (custom code) which is called by the LA; LA's are rarely completely free of code.

CS is configuration based, there is no 'code' technically speaking. The configuration can be mostly undertaken via the Azure Portal but there are areas where the configuration must be undertaken by posting to API with often very complex JSON payloads. Just like LA, CS often requires the creation of a custom skills which typicaly involved an Azure Function (custom code).

Both LA and CS require a learning curve and may be harder to get started with than DF for most developers.

### Triggering and incremental indexing

> TLDR: CS is best suited to content that frequently changes and needs to be fresh. All three are suited to static content that infrequently changes.

One of the key advantages that CS has over both DF and LA is that CS is a search system and so it has a range of options around how it indexes and re-indexes the content.

CS can perform incremental indexing on a scheduled basis, which means your end results are always "fresh" and up to date with the latest source data and the enrichments that apply to it. See [How to schedule indexers in Azure Cognitive Search](https://docs.microsoft.com/en-gb/azure/search/search-howto-schedule-indexers).

Neither LA or DF have an in-built indexing capabilities. Their workflows are simply triggered in response to an external event such as a HTTP request, a file arriving in storage or a service bus message. They will then execute the workflow, produce the output and stop; they will not re-index that file again until they are re-triggered. It is possible to build re-processing systems that work for both LA and DF, but it is something you have to build and is not provided by the platform.

CS is a great choice if your source data changes frequently and you require up-to date views on it, both LA and DF are more applicable to static content that is unlikely to change and/or the customer does not have requirements around the "freshness" of the enrichments.

### Do the built-in skills and connectors cover your requirements?

> TLDR: All three platforms will likely require custom code. If you are doing any code, there is an argument that says an all-code solution will be simpler to build, deploy and maintain; which points to DF

A key advantage that both LA and CS have over DF is that a lot of capabilities are provided by the service and can be used without code. For LA this includes the vast range of connectors and built-in actions, for CS this includes all the built in skills.

Most AI-enrichment pipelines will likely make use of one or more of the built-in enrichment connections/skills. However, the "no code" paradigm breaks down when you need to do something that there is no built-in connector/skill for, for example:

- CS does not have a Video Indexer skill
- LA (probably) does not have a connector for querying your custom eDiscovery system.

LA certainly has more built in capability than CS but there will usually be at least one point where there is no 'thing' for the functionality you need. In this scenario, you have to use a custom skill for CS or call out to a custom API for LA (usually an Azure Function), which means you are having to write code.

If you are writing code as part of your system, you will require the same skills that you need for DF, in addition to the product specific skills required around CS or LA. On this basis, DF often requires a narrower, more abundant and more affordable skills range to build a typical workflow.

Most C# developers will be able to competently build a DF workflow, but that may not be the case for LA and CS.

### Long running enrichments & scale

> TLDR: CS is not suitable for long running enrichments (over 230 seconds) such as Video Indexer. LA is better but also has limits, DF is theoretically unlimited 

Some types of enrichments may take a very long time. The classic example of this is using Video Indexer to index a very large video file which could take hours to complete.

CS has a [230 second maximum timeout](https://docs.microsoft.com/en-us/azure/search/cognitive-search-custom-skill-web-api#skill-parameters) for custom web API skills. This means it cannot be used for these kind of long running operations. You can work around this by having some kind of pre-processing before the file gets indexed (for example a LA which does the video indexer enrichment and places a json file in the data source that CS indexes), but this can get quite complicated and architecturally muddled.

If you have enrichment operations which will get anywhere close to exceeding the 230 limit of CS (such as Video Indexer or Forms Recognizer), this points in the direction of LA or DF as a better fit.

[LA also has limits](https://docs.microsoft.com/en-us/azure/logic-apps/logic-apps-limits-and-config), the key ones are as follows:

- Run duration: maximum 90 days
- Action executions per minute: 100,000 executions (default), 300,000 executions (maximum in high throughput mode)

DF is theoretically limitless in terms of durations of execution and number of executions.

### IaC, CI/CD and deployment

### Collaborative development experience and tooling

### Monitoring, observability and troubleshooting

### Workflow complexity and branching

### Architectural simplicity (LA requires lots of components, DF is one function app, CS or one search index)

### Versioning

### Portability and containers

### Cost

### Unit and integration testing



## In Summary

There is no "best choice" for which platform will work best for your AI-enrichment pipline. Each of the platforms listed in this article have their own distinct advantages and disadvantages and it may be that some combination of some/all of them works best for your scenario.

For some companies, the development skills and experience may be critical, but for others the freshness of the content could be a driving factor; all these consideration must be taken into account when choosing the right workflow platform for your scenario.

- 
- More articles from me: http://martink.me/articles

