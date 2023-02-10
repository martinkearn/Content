---
title: AI enrichment pipeline platform choices and considerations
author: Martin Kearn
description: There are many scenarios where and AI-based enrichment pipeline is applicable; anywhere when you need to enrich data with insight from AI services. Azure offers three main ways to do this, Logic Apps, Cognitive Search and Durable Functions. This articles compares and contrasts these platforms to help you decide which one suits your requirements
image: https://github.com/martinkearn/Content/raw/master/Blogs/Images/EnrichmentPipline.png
thumbnail: https://github.com/martinkearn/Content/raw/master/Blogs/Images/EnrichmentPipline_thumb.png
type: article
status: published
published: 2021/03/08 07:00:00
categories: 
  - Durable Functions
  - Azure Functions
  - Logic Apps
  - Cognitive Services
---
.An AI enrichment pipeline is something that creates additional insights on data by processing it through AI services before submitting it to a final data store or index. This type of system is also known as knowledge mining and the process of getting this additional data from AI services is called "enrichment".

The data could be anything from a database, images, videos, documents or even just JSON. In this context, the term \"AI services\" refers mainly to [Azure Cognitive Services](https://docs.microsoft.com/en-us/azure/cognitive-services/) but in theory these services could be substituted with any AI service that provides enrichment including AI services by other vendors and home-grown APIs or machine learning systems.

![AI enrichment pipeline](https://github.com/martinkearn/Content/raw/master/Blogs/Images/AIEnrichmentPipeline.jpg)

There are many, many use cases for Cognitive Services in AI enrichment pipelines, here are some of the more common ones:

-   Getting insights from videos using [Video Indexer](https://docs.microsoft.com/en-us/azure/media-services/video-indexer/)

-   Describing an image, including tags and other metadata from images using [Computer Vision](https://docs.microsoft.com/en-us/azure/cognitive-services/computer-vision/) or [Custom Vision](https://docs.microsoft.com/en-us/azure/cognitive-services/custom-vision-service/)

-   Translating text content from documents or metadata with [Translator](https://docs.microsoft.com/en-us/azure/cognitive-services/translator/)

-   Identifying faces from a trained list of known faces with the [Face](https://docs.microsoft.com/en-us/azure/cognitive-services/face/)

-   Analysing audio recording of phone calls for intent and entity extraction using [Language Understanding](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/)

-   Transcribing voice with [Speech Service](https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/)

-   Extracting data from forms, receipts and invoices using [Form Recogniser](https://docs.microsoft.com/en-us/azure/cognitive-services/form-recognizer/)

-   Understanding sentiment, keywords and phrases from text using [Text Analytics](https://docs.microsoft.com/en-us/azure/cognitive-services/text-analytics/)

-   Moderating content for adult or racy content with [Content Moderator](https://docs.microsoft.com/en-us/azure/cognitive-services/content-moderator/)

At the time or writing there are 16 different cognitive service with 100\'s of functions across them. Get the broad overview at [What are Azure Cognitive Services?](https://docs.microsoft.com/en-us/azure/cognitive-services/what-are-cognitive-services)

In the context of Azure, there are a few options for how you architect an AI enrichment pipeline system, the three main ones are:

-   [Azure Logic Apps](https://docs.microsoft.com/en-us/azure/logic-apps/logic-apps-overview)

-   [Azure Cognitive Search](https://docs.microsoft.com/en-gb/azure/search/search-what-is-azure-search)

-   [Azure Durable Functions](https://docs.microsoft.com/en-us/azure/azure-functions/durable/durable-functions-overview?tabs=csharp)

Microsoft Flow is also worth a mention but it is really an end user variation of Logic Apps and shares much of the functionality with Logic apps so is not covered in this article.

This article seeks to provide an overview of these options to help you understand the pros and cons of each and determine which is most suitable for your application.

The knowledge gained here is built from the experience of building a real-world knowledge mining system which takes millions of videos, digital text files and images, adds business critical enrichments via Cognitive Services before exporting to a case management system.

## What is Azure Cognitive Search?

Arguably, Azure Cognitive Search is the natural choice for building a knowledge mining application on Azure.

Cognitive Search is a system that will index content from a Data Source. This can be binary data such as files on a storage account or data in a database.

Through the indexing pipeline, Cognitive Search can use [AI enrichment skills](https://docs.microsoft.com/en-gb/azure/search/cognitive-search-concept-intro) to pass the data to a range of Cognitive Services to be \"enriched\" and then store the resulting enrichments alongside the search document. When users do a search using the front-end attached to Cognitive Search, they will be searching on the enriched data as well as the original content.

> \"Data\" can also mean cracking open the contents of a binary file such as a Word document and enriching the text within it.

Cognitive Search has built-in connections to various Cognitive Service, a few examples are below:

-   [Entity extraction with the Text Analytics service](https://docs.microsoft.com/en-gb/azure/search/cognitive-search-skill-entity-recognition)

-   [Image Analysis with the Computer Vision service](https://docs.microsoft.com/en-gb/azure/search/cognitive-search-skill-image-analysis)

-   [Language detection with the Text Analytics service](https://docs.microsoft.com/en-gb/azure/search/cognitive-search-skill-language-detection)

-   [Optical character recognition (OCR) with the Computer Vision service](https://docs.microsoft.com/en-gb/azure/search/cognitive-search-skill-ocr)

In addition to the AI enrichment skills, Cognitive Search has a range of built-in skills which can transform the content as it is indexed, this includes operations like splitting text or shaping data into an expected schema.

If the operation you need is not available as a skill, you can create custom skills which either work with a [web API](https://docs.microsoft.com/en-gb/azure/search/cognitive-search-custom-skill-web-api) (which could be any other Cognitive Service or a custom API/Azure Function) or an [Azure Machine Learning](https://docs.microsoft.com/en-gb/azure/search/cognitive-search-aml-skill) model.

Cognitive Search applications are built largely by configuring the Cognitive Search pipeline (Data Sources, Indexers, Skillsets) which is undertaken through the portal for most scenarios, but there are some things that can only be done through API calls to the Cognitive Search configuration APIs. You also need to build or connect your front-end to the search APIs provided by the service; although the service does provide some very basic HTML templates.

The Cognitive Search pipeline is largely a linear pipeline where skills are executed one after the other. There are few opportunities for complex conditional logic or branching. This would have to be achieved by some level of pre-processing or custom skills.

The Cognitive Search pipeline has a [maximum processing timeout of 230 seconds](https://docs.microsoft.com/en-us/azure/search/cognitive-search-custom-skill-web-api#skill-parameters) for each custom web API skill request. This makes it unsuitable for long running indexing operations.

Additionally, the CS pipeline is a one-way atomic transaction. State is not saved until the end of the pipeline, which makes it quite brittle to failures within the pipeline. This is especially applicable if you have custom skills.

There are also accelerator solutions you can use as a starting point:

-   [Knowledge Mining Solution Accelerator](https://docs.microsoft.com/en-us/samples/azure-samples/azure-search-knowledge-mining/azure-search-knowledge-mining/)

-   [COVID-19 Search App](https://github.com/liamca/covid19search)

-   [The JFK Files](https://github.com/Microsoft/AzureSearch_JFK_Files), see <https://jfk-demo.azurewebsites.net>

## What is Azure Logic Apps?

[Azure Logic Apps](https://azure.microsoft.com/en-gb/services/logic-apps/) is a JSON-based workflow platform which features [100\'s of connectors](https://docs.microsoft.com/en-us/connectors/) to external services including services on Azure, Microsoft 365 and the broader Microsoft SaaS ecosystem as well as popular external services such as Twitter, Dropbox, GitHub, Google Docs, Salesforce and many more.

Included in the long list of connectors are connectors for most of the popular Cognitive Services including Video Indexer, Computer Vision, Translator and Text Analytics (plus more). You can also call any HTTP endpoint via the built-in HTTP connector which enables Logic Apps to call Cognitive Services that do not have a connector, custom Azure Functions or any HTTP endpoint. It is also possible to create your own custom connectors, see [Custom Connectors](https://docs.microsoft.com/en-us/connectors/custom-connectors/).

Logic Apps also feature a wide range of [triggers](https://docs.microsoft.com/en-us/azure/connectors/apis-list#triggers-and-action-types) which is how the logic app gets started. For example, a Logic App can trigger in response to a HTTP request, a service bus message, a file in Azure storage or on a schedule.

Logic Apps feature rich branching and conditional logic which enables the provision of complex workflows. The workflow itself is created using a visual designer via the Azure Portal (the [Logic Apps Preview](https://docs.microsoft.com/en-us/azure/logic-apps/logic-apps-overview-preview) also has a Visual Studio Code editing extension).

The rich range of connectors and conditional capabilities make Logic Apps a great choice for complex enrichment pipelines which produce enrichment data which can easily be exported to a downstream system which a front end can interact with.

Logic Apps do not have any built-in incremental indexing capabilities. This means unlike Cognitive Search which can update its enrichments from a given data source many times, Logic Apps would need to be entirely re-run and the export process carefully controlled to achieve the same. This makes Logic Apps suitable for \'one shot\' content which rarely changed but less applicable to data sources that feature regular changing or updating content

Logic Apps are source controlled and deployed by exporting an [Azure Resource Manager (ARM) template](https://docs.microsoft.com/en-us/azure/logic-apps/logic-apps-azure-resource-manager-templates-overview). This can be deployed using Infrastructure-as-code (IaC) tools such as Terraform, however the introduction of variables from Terraform (for example, injecting the ID of a Video Indexer service deployed by Terraform) is a manual process which does not facilitate iterative development easily and so is prone to error.

Logic Apps are classed as "low to no code". The definition is a JSON document which makes source control and merge conflict resolution problematic when large teams are working concurrently on the same Logic App.

## Azure Durable Functions

An [Azure Function](https://docs.microsoft.com/en-us/azure/azure-functions/functions-overview) at its simplest level is a piece of code that lives in Azure and can be triggered via a range of [bindings](https://docs.microsoft.com/en-us/azure/azure-functions/functions-triggers-bindings?tabs=csharp) and can output its results to other systems as an output binding (including HTTP request and response).

Functions are a central tenant of Azure\'s serverless offering and remove the need to worry about servers or infrastructure. Customers pay for Functions according to consumption based on number of executions, execution time, and memory used.

Functions support multiple programming languages including C\#, Java, JavaScript, TypeScript, Python and PowerShell.

[Azure Durable Functions](https://docs.microsoft.com/en-us/azure/azure-functions/durable/durable-functions-overview?tabs=csharp) is an extension to the Functions runtime which makes Functions stateful and long-running (unlike standard Functions which are stateless and have execution timeouts).

Durable Functions use an [Orchestrator Function](https://docs.microsoft.com/en-us/azure/azure-functions/durable/durable-functions-types-features-overview#orchestrator-functions) which is used to orchestrate calls to other [Activity Functions](https://docs.microsoft.com/en-us/azure/azure-functions/durable/durable-functions-types-features-overview#activity-functions), Entity Functions and other Orchestration functions. DF can store state while the Function is waiting for external input or long running operations to complete.

Durable Functions have a concept of [Durable Entities](https://docs.microsoft.com/en-us/azure/azure-functions/durable/durable-functions-entities?tabs=csharp) which are small pieces of state which can be used to track progress or data points for the overall Durable Function orchestration.

Queue management, state persistence and serialisation are all managed by the Durable Function runtime, see [Data persistence and serialization in Durable Functions](https://docs.microsoft.com/en-us/azure/azure-functions/durable/durable-functions-serialization-and-persistence?tabs=csharp).

Durable Functions can support several popular architectural patterns, including:

-   [Function chaining](https://docs.microsoft.com/en-us/azure/azure-functions/durable/durable-functions-overview?tabs=csharp#chaining)

-   [Fan-out/fan-in](https://docs.microsoft.com/en-us/azure/azure-functions/durable/durable-functions-overview?tabs=csharp#fan-in-out)

-   [Async HTTP APIs](https://docs.microsoft.com/en-us/azure/azure-functions/durable/durable-functions-overview?tabs=csharp#async-http)

-   [Monitoring](https://docs.microsoft.com/en-us/azure/azure-functions/durable/durable-functions-overview?tabs=csharp#monitoring)

-   [Human interaction](https://docs.microsoft.com/en-us/azure/azure-functions/durable/durable-functions-overview?tabs=csharp#human)

-   [Aggregator (stateful entities)](https://docs.microsoft.com/en-us/azure/azure-functions/durable/durable-functions-overview?tabs=csharp#aggregator)

Durable Functions make a great choice for AI-enrichment workflows where the workflow is particularly complex, there is a preference of code over configuration/declarative mark-up or where other workflow platforms do not offer the required functionality and would need custom code (typically Functions) to be meet the requirements.

Because Durable Functions are \'just\' a Function App they can be easily source controlled, subject to continuous integration and deployment pipelines and deployed using tool like Terraform. The code lives in a repository and can benefit from all the advantages of Git for collaborative development. It is not abstracted behind an intermediate language; once you understand the DF tooling and concepts, the rest is just native code (no json/yaml etc).

Durable Functions have no built-in administrative observability in terms of where items are in the overall workflow. However, because these actions are in essence code, they allow integration into other Azure monitoring tools such as Azure Log Analytics and Azure Application Insights. Dashboards can be created to give the required views on the data.

Like Logic Apps, Durable Functions do not have any built-in incremental indexing capabilities (like Cognitive Search does).

It would be possible to achieve some of the Durable Function workflow features using regular Functions, a queueing technology such as Service Bus and a storage technology such as Cosmos database. However, you would be missing out on some of the great advantages that Durable Functions offer around built-in queueing and state management unless you built those too.

## Platform Choice Considerations

When choosing the workflow platform, there are many factors that should be considered; each platform has strengths and weaknesses.

This section will discuss some of the key differences of each platform in the context of each main consideration.

> For improved readability, Logic Apps will be referred to as \"LA\", Cognitive Search as \"CS\" and Durable Functions as \"DF\" throughout this section.

### Code vs config vs declarative mark-up

> TLDR: You will need definitely developers to build DF workflows which are 100% code based. You will probably also need developers for CS and LA which are json and configuration based.

Each of the three platforms have a very distinct approach in how applications are created. This could be a critical consideration and may force a particular route depending on the technical expertise you have at your disposal.

DF is a code-based platform. The advantage is that you have ultimate control and flexibility on the solution, but it also means that you have to code features that LA and CS give you by default. The code can be written in a choice of languages including C\# (generally the default and the language where you\'ll find the most community support), JavaScript, Python and PowerShell. In order to build a workflow platform with DF, you will need developers that can write code using one of these languages.

LA is built using declarative mark-up which is configured via the portal and exports as JSON. To build complex workflows with LA, you will need developers that are competent editing JSON files. There is also an additional burden around IaC technologies such as Terraform with LA (see the IaC, CI/CD and deployment section below). For most complex workflows, there will be occasions where you will need custom functionality which typically takes the form of an Azure Function (custom code) which is called by the LA; LA\'s are rarely completely free of code.

CS is configuration based, there is no \'code\' technically speaking. The configuration can be mostly undertaken via the Azure Portal but there are areas where the configuration must be undertaken by posting to API with often very complex JSON payloads. Just like LA, CS often requires the creation of a custom skills which typically involved an Azure Function (custom code).

Both LA and CS require a learning curve and may be harder to get started with than DF for most developers.

### Triggering and incremental indexing

> TLDR: CS is best suited to content that frequently changes and needs to be fresh. All three are suited to static content that infrequently changes.

One of the key advantages that CS has over both DF and LA is that CS is a search system and so it has a range of options around how it indexes and re-indexes the content.

CS can perform incremental indexing on a scheduled basis, which means your end results are always \"fresh\" and up to date with the latest source data and the enrichments that apply to it. See [How to schedule indexers in Azure Cognitive Search](https://docs.microsoft.com/en-gb/azure/search/search-howto-schedule-indexers).

Neither LA or DF have an in-built indexing capabilities. Their workflows are simply triggered in response to an external event such as a HTTP request, a file arriving in storage or a service bus message. They will then execute the workflow, produce the output and stop; they will not re-index that file again until they are re-triggered. It is possible to build re-processing systems that work for both LA and DF, but it is something you have to build and is not provided by the platform.

CS is a great choice if your source data changes frequently and you require up-to date views on it, both LA and DF are more applicable to static content that is unlikely to change and/or the customer does not have requirements around the \"freshness\" of the enrichments.

### Do the built-in skills and connectors cover your requirements?

> TLDR: All three platforms will likely require custom code. If you are doing any code at all, there are benefits in terms of the overall simplicity of the solution if it is entirely code-based like it is with DF.

A key advantage that both LA and CS have over DF is that a lot of capabilities are provided by the service and can be used without code. For LA this includes the vast range of connectors and built-in actions, for CS this includes all the built in skills.

Most AI-enrichment pipelines will likely make use of one or more of the built-in enrichment connections/skills. However, the \"no code\" paradigm breaks down when you need to do something that there is no built-in connector/skill for, for example:

-   CS does not have a Video Indexer skill

-   LA (probably) does not have a connector for querying your custom eDiscovery system.

LA certainly has more built in functionality than CS but there will usually be at least one point where there is no \'thing\' for the functionality you need. In this scenario, you have to use a custom skill for CS or call out to a custom API for LA (usually an Azure Function), which means you are having to write code.

If you are writing code as part of your system, you will require the same skills that you need for DF, in addition to the product specific skills required around CS or LA. On this basis, DF often requires a narrower, more abundant and more affordable skills range to build a typical workflow.

Most C\# developers will be able to competently build a DF workflow with only minimal upskilling on some of the DF concepts. However, both CS and LA require more familiarization with the concepts of the technology, tooling and development processes.

### Long running enrichments & scale

> TLDR: CS is not suitable for long running enrichments (over 230 seconds) such as Video Indexer. LA is better but also has limits, DF is theoretically unlimited

Some types of enrichments may take a very long time. The classic example of this is using Video Indexer to index a very large video file which could take hours to complete.

CS has a [230 second maximum timeout](https://docs.microsoft.com/en-us/azure/search/cognitive-search-custom-skill-web-api#skill-parameters) for custom web API skills. This means it cannot be used for these kind of long running operations. You can work around this by having some kind of pre-processing before the file gets indexed (for example a LA which does the video indexer enrichment and places a json file in the data source that CS indexes), but this can get quite complicated and architecturally muddled.

If you have enrichment operations which will get anywhere close to exceeding the 230 limit of CS (such as Video Indexer or Forms Recognizer), this points in the direction of LA or DF as a better fit.

[LA also has limits](https://docs.microsoft.com/en-us/azure/logic-apps/logic-apps-limits-and-config), the key ones are as follows:

-   Run duration: maximum 90 days

-   Action executions per minute: 100,000 executions (default), 300,000 executions (maximum in high throughput mode)

DF is theoretically limitless in terms of durations of execution and number of executions.

### IaC and Deployment

> TLDR: The service for all three platforms can be easily deployed with most IaC providers. However the workflow definition for LA and configuration for CS are both complex to deploy, require specific processes and scripting to get right and will be error prone. DF is very simple to deploy both the service and the application code using IaC.

Any modern, production application is generally deployed using an infrastructure-as-code (IaC) provider such as [Terraform](https://www.terraform.io/), [Pulumi](https://www.pulumi.com/) or [Azure Resource Manager](https://azure.microsoft.com/en-gb/features/resource-manager/). These technologies allow you to write \"code\" files which deploy infrastructure and application code running on that infrastructure according to the exact specification you define.

The benefit of IaC technologies is that your application is entirely repeatable and can be deployed to new environments very easily.

With any mainstream IaC provider, it is relatively simple to deploy the core infrastructure to support each of the workflow platforms, however deploying your application \"code\" is where the choice become more challenging.

LA in particular has a very complex deployment process. When you develop/configure an LA workflow, you export a JSON ARM template which will contain hard-coded references to services. In the case of AI enrichment, it will contain hard-coded references to Cognitive Services and other APIs which the workflow uses. The fact that these references are hard coded means that the same ARM template will not work on a separate deployment, because the exact resource IDs will not be present.

Some of this complication can be mitigated by using parameters and injecting values into those parameters during deployment, however this process is complex and highly error prone because the version of the ARM template that is used by the IaC provider will always be different to what the Azure portal exports, so there is always manual \"copy and paste\" activity to ensure that the IaC parameters are setup correctly.

Some of these issues may be resolve able by customer tooling, scripts or alternative IaC platforms such as Pulumi.

The CS service can be deployed via any IaC provider. However, because CS is configuration based it is generally configured by a series of API requests which can be encapsulated in a script file with embedded curl requests or you have to build out some other script to deploy the configuration via PowerShell or similar. This means you have to manually build up the deployment and tear down code via a series of API requests.

Contrastingly, DF is very simple to deploy because it is just simple application code which can be built and deployed with appropriate application settings from something like Azure Key Vault generated via deployment. There are well documented patterns and practices for injecting application settings via IaC deployment. There are no special processes or considerations in deploying DF.

### Collaborative development experience

> TLDR: DF is very simple, it is just code so source control systems (like git) can do a great job of managing concurrent developers. CS is also relatively simple if you manage your API JSON payloads as JSON files in source control. LA is very complex and concurrent development is not something that is easy to get right.

Unless you have development team of 1, you are likely going to need some processes for managing multiple developers working on the \"code\" for your application.

Typically this involves using a git repository with branches, pull requests, code reviews and a host of other collaborative development tools. If you are using a git-based repository, there are some gotchas with some of the workflow platforms we are discussing in this article.

DF is very simple; the application is pure code which means it lives in your git repository and you can manage it the same way you would for any code. Git does a great job of managing merge conflicts that occur though collaborative development.

CS is also relatively simple as you will likely store the JSON API request payloads required to configure the application in source control and manage them just like normal code files. You could also use tools like Postman to build up collections of requests.

LA is a little more complex. The development process for LA is to make changes to you LA via the portal, export the ARM template (a JSON file) and store that file in Git. If you are using IaC you will also need to edit the file to override the hard-coded values for services that your LA uses with the IaC injected ones. A major gotcha with LA is the sequence and layout of the code in the JSON can change from one export to another; meaning that if you export the same LA twice without changes you could get two different JSON files. This becomes a major problem when you have multiple people working on the LA concurrently, not only is it very hard to manage merge conflicts in JSON files, but there will almost always be merge conflicts because of the way the file gets re-structured on each export.

The problem above is amplified by the fact that you have a single JSON file per LA and that file tends to be very large so managing merge conflicts gets very tricky.

The reality with LA is that it is only really practical for one person to work on the LA at any given time and you have to devise offline check-out type systems and processes to ensure this happens.

### Monitoring, observability and troubleshooting

> TLDR: All three platforms offer integration with Azure Monitor for logging, dashboards and application insights. LA has visual monitoring via its portal where you can see the low level details of each run in a visual way, which can be very useful, especially during development.

It is important for a production application that you can monitor the system and see what is happening, especially when things go wrong.

All three platforms can \"plug in\" to Azure Monitor to give you out-of-the-box insights on the application health. From here you can build dashboards and reports.

However there will be parts of your application\'s workflow which you want to specifically report on by adding events to the Azure Monitor Logs. For example if a file has been successfully validated before being enriched, you want want to log that so administrators can see how far the file got through the workflow.

DF uses the underlying Functions runtime to easily integration with Application Insights (part of Azure Monitor). You can use the `ILogger` interface to log custom events which will bubble up through Application Insights. See [How to configure monitoring for Azure Functions](https://docs.microsoft.com/en-us/azure/azure-functions/configure-monitoring?tabs=v2).

Because CS is essentially just a service, the default Azure Monitoring integration is generally enough to get visibility of wat is happening in the pipeline.

LA also uses Azure Monitor for basic monitoring (see [Set up Azure Monitor logs and collect diagnostics data for Azure Logic Apps](https://docs.microsoft.com/en-us/azure/logic-apps/monitor-logic-apps-log-analytics#install-logic-apps-management-solution)), but has a big advantage over the other two platform with the visual run history that you get with the LA portal. You can dive into a specific run and see exactly what happened in a visual way.

### Unit and Integration Testing

> TLDR: True unit testing is only really possible with DF as that is the only platform that has \'units\' of code that can be tested. For CS and LA, you will need to lean more on integration testing to assert that the applications is behaving as it should do.

Testing is an important part of any production application and AI enrichment pipelines are no different.

Unit testing is the process of testing units of functionality to ensure they behave as expected. With DF, this is very simple; you write unit tests using whichever testing framework you prefer in the way you normally would do for code.

CS is difficult conceptually because there are no \'units\' as such because it is entirely configuration based, so it is hard to really use unit testing as a concept.

LA is slightly more problematic because there is \'code\' in the sense that the application can take different paths, use formulas to compute values and have expected results from actions. However, it is not really possible to unit test specific parts of a LA. You can test the workflow as a whole (see integration testing later) but you cannot really test the actions within a logic app because they will always depends on the other actions around them and the workflow as a whole.

LA does have the capability to test workflows using mock data, which can be useful, see [Test logic apps with mock data by setting up static results](https://docs.microsoft.com/en-us/azure/logic-apps/test-logic-apps-mock-data-static-results).

In terms of integration testing, all three platforms can be equally testing by simulating inputs and asserting expected outputs using which ever integration testing approach you prefer. You can read more at [Integration testing with Pester and PowerShell](https://martink.me/articles/integration-testing-with-pester-and-powershell).

### Execution cost and Billing

> TLDR: DF can be the cheaper option of the 3. However, the platform costs are generally dwarfed by the AI services costs in AI enrichment pipelines and these costs will be the same regardless of platform choice.

Cost is always a factor whenever any application is being created. The three platforms we are discussing all have different billing models and in some scenarios there could be a distinct cost different between them.

Before we dive into the billing models, it is worth calling out that whichever workflow platform you choose, the overall cost of an AI enrichment system will generally be dominated by the AI service themselves (i.e. the Cognitive Services you are using for the enrichments). The platform costs are generally very small compared to the overall system cost, especially if you are suing expensive AI services like Video Indexer.

DF follows the same billing model as Azure Functions which is consumption based which is per-second resource consumption and executions. Consumption plan pricing includes a monthly free grant of 1 million requests and 400,000 GBs of resource consumption per month per subscription in pay-as-you-go pricing across all function apps in that subscription. If you functions are relatively efficient on resources and short running, DF can be a very affordable option. See [Azure functions pricing](https://azure.microsoft.com/en-gb/pricing/details/functions/).

LA is also a pay-for-use billing model based on the usage of connectors and actions. This means that the larger the LA is (i.e. the more actions and connectors it has), the more expensive it will be per run. See [Azure Logic Apps pricing](https://azure.microsoft.com/en-gb/pricing/details/logic-apps/).

Very generally speaking, with a like-for-like functional comparison DF can be cheaper to operate than LA.

CS has various pricing tiers which each offer increasing quotas for things like indexes and document cracking. Like LA, CS costs will increase the more content you index and the more indexes you maintain. See [Azure Cognitive Search pricing](https://azure.microsoft.com/en-us/pricing/details/search/) and [How to estimate and manage costs of an Azure Cognitive Search service](https://docs.microsoft.com/en-us/azure/search/search-sku-manage-costs).

## In Summary

There is no best choice for which platform will work best for your AI-enrichment pipeline. Each of the platforms listed in this article have their own distinct advantages and disadvantages and it may be that some combination of some/all of them works best for your scenario.

For some companies, the development skills and experience may be critical, but for others the freshness of the content could be a driving factor; all these consideration must be taken into account when choosing the right workflow platform for your scenario.

For further reading, I recommend:

-   [Azure Logic Apps](https://docs.microsoft.com/en-us/azure/logic-apps/logic-apps-overview)

-   [Azure Cognitive Search](https://docs.microsoft.com/en-gb/azure/search/search-what-is-azure-search)

-   [Azure Durable Functions](https://docs.microsoft.com/en-us/azure/azure-functions/durable/durable-functions-overview?tabs=csharp)

-   More articles from me: http://martink.me/articles

