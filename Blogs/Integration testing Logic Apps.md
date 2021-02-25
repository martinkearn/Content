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

## Sample code structure

The sample code accompanying this article is stored at https://github.com/martinkearn/Pester-LogicApp.

The repo has the following key files:

- **`/Logic App Template/`** - This folder contains the Logic App ARM template so you can deploy it to your own azure subscription
- **`/PS/sample.env`** - Take a copy of this file named `.env` and update it to include details for your deployed Logic App. See the Environment Variables section below
- **`/PS/LogicApp.tests.ps1`** - This is the main Pester test script. You can execute this script in a PowerShell console using `Invoke-Pester -Output Detailed LogicApp.tests.ps1`
- **`/PS/Utilities.ps1`** - This is a utility PowerShell file which contains functions to wait for async logic apps to complete, get the run history of a completed logic app and to get the output of a specific action.

## A simple weather Logic App

I did not want this sample to be about the Logic App, so I've deliberately chosen a very simple logic app which is HTTP triggered, expecting the following `POST` payload:

```json
{
    "location": "Some city",
    "uniqueid": "Some unique identifyer"
}
```

The logic app will then get a weather report for the city specified in the `location` via the [MSN Weather Connector](https://docs.microsoft.com/en-us/connectors/msnweather/) and return it. 

![A simple logic app which gets a weather report for a given city](https://github.com/martinkearn/Content/raw/master/Blogs/Images/WeatherLogicAppOverview.jpg)

You can deploy the logic app yourself following the steps in https://github.com/martinkearn/Pester-LogicApp/blob/main/README.md#setup--usage.

## Environment Variables

For our test to be able to access the Logic App, it needs to know the full trigger URI which includes a secret. 

Of course, we cannot store the secret as plain text in the test script so we are going to use environment variables to store and access it. You could also use something like Terraform state here.

For local development purposes, it is handy to store variables in an `.env` file which you can read using the [Set-PsEnv PowerShell Module](https://github.com/rajivharris/Set-PsEnv). This makes any of the variables defined in your `.env` file available via the standard PowerShell syntax for accessing environment variables such as `$env:LOGICAPPURI`.

You can use Pester's `BeforeDiscovery` utility to load the `.env` file as follows:

```powershell
BeforeDiscovery {
    # Load environment variables from local .env file by using Set-PsEnv
    if (-not (Get-Module -ListAvailable -Name Set-PsEnv)) {
        Install-Module -Name Set-PsEnv -Force
    }
    Set-PsEnv 
}
```

We can now optionally build a Pester context section which asserts that the expected variables are present and not null. 

```powershell
Describe "Logic App Integration Tests" {
    Context "Host machine has the environment variables setup correctly" {
        It "Expects LOGICAPPURI environment variable to be setup" {
            $env:LOGICAPPURI | Should -Not -BeNullOrEmpty
        }
        It "Expects LOGICAPPNAME environment variable to be setup" {
            $env:LOGICAPPNAME | Should -Not -BeNullOrEmpty
        }
        It "Expects RESOURCEGROUPNAME environment variable to be setup" {
            $env:RESOURCEGROUPNAME | Should -Not -BeNullOrEmpty
        }
    }
}
```

If these tests pass, we can be sure that the script has everything it needs to access the logic app.

## Triggering the Logic App and asserting completion

Before we trigger the Logic App, we need to establish a unique identifier for this test execution so that we can identify which Logic App run is related to the test later on.

To do this, we'll use Pester's `BeforeAll` section which is code that runs before any other code in the script. This can be used to setup a variable called `$uniqueid` with a new guid which will be available throughout the script.

We also use `BeforeAll` to include the `Utilities.ps1` file which we'll cover later.

```powershell
BeforeAll { 
    . $PSScriptRoot/Utilities.ps1
    $uniqueId = New-Guid
}
```

Now we have a unique identifier for the test, we can trigger the Logic App. This example uses a HTTP-triggered Logic App so we can execute a simple HTTP request passing the required body details.

Logic Apps can operate with [Asynchronous Response](https://docs.microsoft.com/en-us/azure/logic-apps/logic-apps-workflow-actions-triggers#run-actions-in-a-synchronous-operation-pattern) enabled, which is usefull and recommended for long running Logic Apps. In this mode, the Logic App will initially respond with a `202/Accepted` response which includes a header called `location`. The `location` header contains a URI which can be used to obtain final status for the Logic App. You can then poll this URI looking for a `200/OK` response with the response payload.

Without Asynchronous Response enabled, we can expect the Logic App to return `200/OK` with the response payload straight away.

In order to support both scenarios, we trigger the Logic App using Pester's `BeforeAll` section (in context of the "Trigger the logic app" context) using a simple `Invoke-WebRequest`. We then look for the presence of the `location` header which will indicate whether to expect a `202` or `200` response.

In the case of a `202` response, we can use one of the utility functions called `Wait-ForLogicAppToComplete` which basically keeps polling the status URI provided in the `location` header looking for a `200` response and returning it when it is found.

This is the code for the `Wait-ForLogicAppToComplete` utility function:

```powershell
function Wait-ForLogicAppToComplete {
    param (
        [parameter(Mandatory = $true)] [string] $LogicAppUri,
        [parameter(Mandatory = $true)] [int] $TimeoutMinutes
    )
    $startDate = Get-Date
    do {
        $response = Invoke-WebRequest -Method GET -Uri $LogicAppUri
        if ($response.StatusCode -ge 500) {
            throw "LogicApp GET $LogicAppUri returned error status code $($response.StatusCode)"
        }
        if ($response.StatusCode -eq 200) {
            return $response
        }
        Start-Sleep -s 10
    } while ($startDate.AddMinutes($TimeoutMinutes) -gt (Get-Date))
}
```

Regardless of whether the Logic App is using Asynchronous Response or not, it will eventually result in a 200 status code, which we can assert for using Pester. The final code for the triggering the logic app and checking it completed successfully is as follows:

```powershell
Context "Trigger the logic app" {
    BeforeAll {
        $postBody = @{
            location = "London"
            uniqueid = "$uniqueId"
        } | ConvertTo-Json
        $response = Invoke-WebRequest -Method POST -Uri $env:LOGICAPPURI -Body $postBody -ContentType "application/json"
    }
    It "Has sucessfully completed" {
        if ($response.Headers.Location -ne $null)
        {
            $response.StatusCode | Should -Be 202
            $response = Wait-ForLogicAppToComplete -LogicappUri $response.Headers.Location -TimeoutMinutes 10
        }
        $response.StatusCode | Should -Be 200
    }
}
```

Following this test, we know that the Logic App has been triggered and successfully completed.

# TO DO - split sample code into two utilities: 1 which gets a logic app run by unique id, one which gets an action output

## Identifying the Logic App in the run history

## Asserting the output of an Action

## In Summary

For more resources:

- Full sample code: https://github.com/martinkearn/Pester-LogicApp
- Pester website: https://pester.dev
- PowerShell docs: https://docs.microsoft.com/en-us/powershell/scripting/how-to-use-docs?view=powershell-7.1
- More articles from me: http://martink.me/articles

