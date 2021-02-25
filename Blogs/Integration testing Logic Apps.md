---
title: Integration testing Logic Apps
author: Martin Kearn
description: This articles talks through a sample script for integration testing Azure Logic Apps with Pester and PowerShell
image: https://github.com/martinkearn/Content/raw/master/Blogs/Images/IntTestingLogicApps.jpg
thumbnail: https://github.com/martinkearn/Content/raw/master/Blogs/Images/IntTestingLogicApps.jpg
type: article
status: draft
published: 2021/02/25 11:00:00
categories: 
  - Pester
  - Testing
  - PowerShell
  - Logic Apps
---

.[Pester](https://pester.dev) and [PowerShell](https://docs.microsoft.com/en-us/powershell/scripting/how-to-use-docs?view=powershell-7.1) are a great way to build integration test scripts for any deployed application. You can see my [Integration testing with Pester and PowerShell](http://martink.me/articles/integration-testing-with-pester-and-powershell) article for detail on what Pester is and how it works.

In this article, we will explore how to use Pester and PowerShell to build integration tests for [Azure Logic Apps](https://azure.microsoft.com/en-gb/services/logic-apps/). There are some tricks and utilities that my team and I have built on a recent project which could be reusable; this article will go through it all and explains how it works.

We will explore:

- How to initiate logic apps via PowerShell and assert that they complete as expected
- How to inspect the results of specific Logic App actions to assert the right values are being produced
- How to identify a specific Logic App run in the history 
- How to deal with Logics Apps which have [Asynchronous Response](https://docs.microsoft.com/en-us/azure/logic-apps/logic-apps-workflow-actions-triggers#run-actions-in-a-synchronous-operation-pattern) enabled

Some reader notes...

> This article assumes basic knowledge of Pester, PowerShell, Azure Logic Apps and the principles of integration testing. See [Integration testing with Pester and PowerShell](http://martink.me/articles/integration-testing-with-pester-and-powershell) or  [Azure Logic Apps](https://azure.microsoft.com/en-gb/services/logic-apps/) if you need refreshers on these technologies.
>
> The sample code that supports this article is at https://github.com/martinkearn/Pester-LogicApp.
>
> This article is based on Logic Apps with HTTP triggers, but the principles of inspecting action results and identifying runs can apply to any Logic App

## A simple Logic App

I did not want this sample to be about the Logic App so I've deliberately chosen a very simple logic which is HTTP triggered, expecting the following `POST` payload:

```json
{
    "location": "Some city",
    "uniqueid": "Some unique identifyer"
}
```

The logic app will then get a weather report for the city specified in the `location` via the [MSN Weather Connector](https://docs.microsoft.com/en-us/connectors/msnweather/) and return it. 

This is the high level flow for the Logic App



## Triggering the Logic App and asserting completion

## Identifying the Logic App in the run history

## Asserting the output of an Action

## In Summary

For more resources:

- Full sample code: https://github.com/martinkearn/Pester-LogicApp
- Pester website: https://pester.dev
- PowerShell docs: https://docs.microsoft.com/en-us/powershell/scripting/how-to-use-docs?view=powershell-7.1
- More articles from me: http://martink.me/articles

