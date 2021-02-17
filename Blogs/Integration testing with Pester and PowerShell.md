---
title: Integration testing with Pester and PowerShell
author: Martin Kearn
description: Pester is a PowerShell based test framework which makes it very simple to write integration tests. This article gives and overview and some usefull resources.
image: https://github.com/martinkearn/Content/raw/master/Blogs/Images/pester.png
thumbnail: https://github.com/martinkearn/Content/raw/master/Blogs/Images/pesterThumb.png
type: article
status: draft
published: 2021/02/17 09:00:00
categories: 
  - Pester
  - Testing
  - PowerShell
---

OK, I'll admit it ..... despite being an IT veteran of 23+ years, I've never really had to deal with integration testing until recently.

The project I was on used Pester and PowerShell to integration test the deployed system was doing what it was supposed to be doing.

I learnt a lot from this exercise and wanted to share what I learnt here.

## Integration testing; what and why?
Simply put, integration testing is the act of testing a deployed version of your system to ensure it operates as expected to in its "integrated" state along-side other system components. Integration tests catch bugs that affect how code operates as part of the wider deployed system, which are not possible to catch by testing the system units alone (unit testing) .

In order to perform integration tests, you'll need a thing that simulates real transactions on your system and exercises known system interfaces, then asserts that the outputs are what you expected them to be.

> Throughput this article, I'll use [WorldClockApi.com](http://worldclockapi.com/) as a system to test; everyone can understand telling the time so it is a nice simple system to base examples on. I have nothing to do with this website. I have no idea who owns or maintains it, but it does seem to work and that is good enough for me.

According to the docs of WorldClockAPI.com, if I call this URI http://worldclockapi.com/api/json/utc/now, I should expect a json response detailing the time and some other date related information.

If I wanted to integration test this system, I would first send a request to tha http://worldclockapi.com/api/json/utc/now and then assert that the response was what I expected, which in this case is something like this:

```json
{
    "$id": "1",
    "currentDateTime": "2021-02-17T12:57Z",
    "utcOffset": "00:00:00",
    "isDayLightSavingsTime": false,
    "dayOfTheWeek": "Wednesday",
    "timeZoneName": "UTC",
    "currentFileTime": 132580402538262368,
    "ordinalDate": "2021-48",
    "serviceResponse": null
}
```

If the response is what I expected, then I can assume the system is working correctly in it's integrated state and all is well.

Fundamentally, that is all you need to know about integration testing; it is a way of testing that your system works in real life with other systems it depends on.

## Pester - a PowerShell testing framework

[Pester](https://pester.dev/) is a PowerShell based testing framework which has a very elegant syntax for setting up tests and asserting the results. It follows an English language way of describing assertions.

Because it is based on PowerShell, you can use all of the goodness of PowerShell to call systems via either built-in cmdlets or libraries. Pester tests are written as PowerShell scripts (*.PS1) which can be easily embedded into your CI pipeline.

You can see the official overview of Pester at the [Quick Start](https://pester.dev/docs/quick-start) docs, but here are some of the key principles....

- Individual tests (or assertions) are represented by [`IT` blocks](https://pester.dev/docs/commands/It). In the AAA pattern, these are the assertions that we want to actually test ae true or false. 

- Each `IT` is accompanied with a [`SHOULD` block](https://pester.dev/docs/commands/Should). `SHOULD` is a keyword that defines the assertion and has many operators such as [`BE`](https://pester.dev/docs/commands/Should#be), [`BEGREATERTHAN`](https://pester.dev/docs/commands/Should#begreaterthan), [`CONTAIN`](https://pester.dev/docs/commands/Should#contain) and many many more

- A [`DESCRIBE`](https://pester.dev/docs/commands/Describe) block is a logical grouping of individual tests. In many cases, a single test file (`something.tests.ps1`) may contains a single describe block.
- You can use [`BEFOREALL`](https://pester.dev/docs/commands/BeforeAll) to arrange/setup the test and [`AFTERALL`](https://pester.dev/docs/commands/AfterAll) to tear down afterwards

This is a very basic example which asserts that the variable `$number` is equal to 1.

```powershell
Describe "Number tests" {
    BeforeAll {
		$number = 1
    }
    It "Number is equal to 1" {
    	$number | Should -Be 1
    }
}
```



