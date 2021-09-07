---
title: Durable Functions with Retry Timeout and Callback
author: Martin Kearn
description: How to use Durable Functions to call external systems with retry and timeout loops, including waiting for the external system to call back and rehydrate the function.
image: https://raw.githubusercontent.com/martinkearn/Content/master/Blogs/Images/Retry.jpg
thumbnail: https://raw.githubusercontent.com/martinkearn/Content/master/Blogs/Images/Retry_thumb.jpg
type: article
status: draft
published: 2021/09/07 16:00:00
categories: 
  - Durable Functions
  - Azure Functions
  - Serverless
---

.Azure Durable Functions are a platform for building code-based workflows built on top of the Azure Functions runtime.

Like any workflow system, it is common for Durable Functions to need to call external systems and handle retry and timeout scenarios. It is also common for the external systems to carry out long running tasks which need to call back to the Durable Function when they are done.

In this article, we'll discuss patterns and a sample for achieving all of these features with Durable Functions, based on a real-world customer scenario.

The article assume a working knowledge of Durable Functions; if you don't already have that, you may benefit from reading [Durable Functions Overview](https://docs.microsoft.com/en-us/azure/azure-functions/durable/durable-functions-overview?tabs=csharp) first.

There is a working companion sample that goes with this article available at GitHub:  [Durable-Function-Retry-Timeout-Callback](https://github.com/martinkearn/Durable-Function-Retry-Timeout-Callback)

## The scenario

The customer scenario that lead to this article was a Durable Function which needed to execute jobs on [Azure Databricks](https://docs.microsoft.com/en-us/azure/databricks/). In this particular scenario, the Databricks jobs were based on user-driven job logic and configurations, so the chances on users defining incorrect data which might trigger an error code from Databricks was relatively high. 

Because of the long running nature of the Databricks jobs, the system was designed so that Databricks itself calls back to the Durable Function when the job has completed rather than the Durable Function polling Databricks for status.

The customer wanted the Durable Function system to work with a set of configuration values for each job that defined:

- Timeout limit: A setting in seconds that defines how long should be given between the job being sent to Databricks and the call-back arriving back at the Durable Function.
- Retry limit: A setting that defines how many times a job should be retried if it either timeout or has an error when initially requested

Durable Functions are very well suited to these kinds of requirements because Durable Function allows for long-running operations (see [Human interaction and timeouts in Durable Functions](https://docs.microsoft.com/en-us/azure/azure-functions/durable/durable-functions-phone-verification?tabs=csharp)). Durable Functions also allows complex retry logic to be coded using standard code conventions such as `do/while` loops and `try/catch` blocks.

## Handling failures

## Call-back and timeout

## Durable Entities for recording status

## In summary

- GitHub repo: [github.com/martinkearn/Durable-Function-Retry-Timeout-Callback](https://github.com/martinkearn/Durable-Function-Retry-Timeout-Callback)
-   More articles from me: http://martink.me/articles

