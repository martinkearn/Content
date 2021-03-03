---
title: AI Enrichment Pipelines: Platform choices and considerations
author: Martin Kearn
description: There are many scenarios where and AI-based enrichment pipline is applicable; anywhere when you need to enrich data with insight from AI services. Azure offers three main ways to do this, Logic Apps, Cognitive Search and Durable Functions. This articles compares and contrasts these platforms to help you decide which one suits your requirements
image: https://github.com/martinkearn/Content/raw/master/Blogs/Images/IntTestingLogicApps.jpg
thumbnail: https://github.com/martinkearn/Content/raw/master/Blogs/Images/IntTestingLogicApps.jpg
type: article
status: draft
published: 2021/03/03 08:00:00
categories: 
  - Durable Functions
  - Functions
  - Cognitive Search
  - Logic Apps
  - Cognitive Services
  - Serverless
---

.An AI enrichment pipline is something that takes data and creates additional insights on that data by processing it through AI services. 

The data could be anything from a database, images, videos, document or even just JSON. In this context the "AI services" refer mainly to [Azure Cognitive Services](https://docs.microsoft.com/en-us/azure/cognitive-services/) but in theory these services could be substituted with any AI service that provides enrichment including AI services by other vendors through to home-grown APIs or machine learning systems. 

This type of system is also known as "knowledge mining".

In the context of Azure, there are a few options for how you build such a system. This article seeks to provide an overview of the various options to help you understand the pros and cons of each and determine which is most suitable for your application.

The knowledge gained here is built from the experience of building a real-world knowledge mining system which takes millions of videos, digital text files and images, adds business critical enrichments via Azure Cognitive Services before exporting to [Relativity](https://www.relativity.com/) which is an eDiscovery and case management system.

## In Summary

- 
- More articles from me: http://martink.me/articles

