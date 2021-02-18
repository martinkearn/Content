---
title: Integration testing with Pester and PowerShell
author: Martin Kearn
description: Pester is a PowerShell based test framework which makes it very simple to write integration tests. This article gives and overview and some usefull resources.
image: https://github.com/martinkearn/Content/raw/master/Blogs/Images/pester.png
thumbnail: https://github.com/martinkearn/Content/raw/master/Blogs/Images/pesterThumb.png
type: article
status: draft
published: 2021/02/18 11:00:00
categories: 
  - Pester
  - Testing
  - PowerShell
---



I'll admit it ..... despite being an IT veteran of 23+ years, I've never really had to deal with integration testing until recently.

The project I was on used Pester and PowerShell to integration test the deployed system was doing what it was supposed to be doing.

I learnt a lot from this exercise and wanted to share what I learnt here.

## Integration testing; what and why?
Simply put, integration testing is the act of testing a deployed version of your system to ensure it operates as expected to in its "integrated" state along-side other system components. Integration tests catch bugs that affect how code operates as part of the wider deployed system, which are not possible to catch by testing the system units alone (unit testing).

I [asked Twitter](https://twitter.com/MartinKearn/status/1362020458074435586) to give me some tweet-sized examples of what integration testing is and got some great examples. Here are some of my favourites

- [@Toolan](https://twitter.com/toolan) wrote "*Integration testing: Putting everything together in a sandpit and seeing if it plays nicely with its brothers and sisters.*"
- [@devinmau](https://twitter.com/devinmau) wrote "*As a football fan, imagine recruiting 11 of the best strikers in the world for the biggest game of your life. Integration testing helps you avoid that disaster!*"
- There were also some great gifs, including this one from [@Storey247](https://twitter.com/STOREY247). 

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">Unit tests pass… integration tests fail <a href="https://t.co/RJ0EjKMr71">pic.twitter.com/RJ0EjKMr71</a></p>&mdash; ʎǝɹoʇS ǝʌɐᗡ - ʎǝɹoʇS ǝnɹ⊥ (ノಠ益ಠ)ノ彡┻━┻ (@STOREY247) <a href="https://twitter.com/STOREY247/status/1362134686017978370?ref_src=twsrc%5Etfw">February 17, 2021</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

In order to perform integration tests, you'll need a thing that simulates real transactions on your system and exercises known system interfaces, then asserts that the outputs are what you expected them to be.

> Throughput this article, I'll use [WorldClockApi.com](http://worldclockapi.com/) as a system to test; everyone can understand telling the time so it is a nice simple system to base examples on. I have nothing to do with this website. I have no idea who owns or maintains it, but it does seem to work and that is good enough for me.

According to the docs of WorldClockAPI.com, if I call this URI http://worldclockapi.com/api/json/utc/now, I should expect a json response detailing the time and some other date related information.

If I wanted to integration test this system, I would first send a request to http://worldclockapi.com/api/json/utc/now and then assert that the response was what I expected, which in this case is something like this:

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

- Individual tests (or assertions) are represented by [`It` blocks](https://pester.dev/docs/commands/It). In the [AAA pattern](https://www.c-sharpcorner.com/UploadFile/dacca2/fundamental-of-unit-testing-understand-aaa-in-unit-testing/), these are the assertions that we want to actually test are true or false. 

- Each `It` is accompanied with a [`Should` block](https://pester.dev/docs/commands/Should). `Should` is a keyword that defines the assertion and has many operators such as [`Be`](https://pester.dev/docs/commands/Should#be), [`BeGreaterThan`](https://pester.dev/docs/commands/Should#begreaterthan), [`Contain`](https://pester.dev/docs/commands/Should#contain) and many many more

- A [`Describe`](https://pester.dev/docs/commands/Describe) block is a logical grouping of individual tests. In many cases, a single test file (`something.tests.ps1`) may contains a single describe block.
- You can use `BEFOREALL` to arrange/setup the test and `AFTERALL` to tear down afterwards. See [Setup and teardown](https://pester.dev/docs/usage/setup-and-teardown).

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

## Building Pester tests for WorldClockAPI.com

Lets put Pester to use and build an integration test for WorldClockAPI.com, you can find the finished `worldclockapi.test.ps1` file published on GitHub: https://github.com/martinkearn/Pester-WorldClockApi.

The first step is to install Pester, using `Install-Module Pester -Force` in a PowerShell console (with administrator rights). Follow this with `Import-Module Pester -PassThru` to ensure that the output is shown in the console.

Create a file called `worldclockapi.test.ps1` and open it in your favourite PowerShell editor (such as [Visual Studio Code](https://code.visualstudio.com/)).

The first step is to setup the basic test structure and call the WorldClockApi. We do this in a `BeforeAll` section so that we known the response should be there before any tests run. The code within the `BeforeAll` is just basic PowerShell which does a HTTP request to the API to get the response (stored as `$response`) and then converts that response's content to the `$responseContent` object which we can use standard dot notation to drill into and inspect later.

```powershell
Describe 'Test worldclockapi.com' {
    BeforeAll {
        $response = Invoke-WebRequest -Method 'GET' -Uri 'http://worldclockapi.com/api/json/utc/now'
        $responseContent = $response.Content | ConvertFrom-Json
    }
}
```

You can now run this in your PowerShell console using the following command 

`Invoke-Pester -Output Detailed .\worldclockapi.tests.ps1`

There are not `It` blocks yet so there are no tests to pass but as long as there are no errors you are good to continue. The response should look like this.

![Pester result with no tests](https://github.com/martinkearn/Content/raw/master/Blogs/Images/pester-notests.jpg)

Now we have the basic script, we will add a range of tests using the `It` and `Should` keywords. Add the following code beneath the `BeforeAll` closing line. The code is mostly self explanatory. in some of the tests, we have a little bit of PowerShell whihc gets the current data in various formats so we can make assertions against the `$responseContent` object.

```powershell
It "It should respond with 200" {
	$response.StatusCode | Should -Be 200
}

It "It should have a null service response" {
	$responseContent.serviceResponse | Should -BeNullOrEmpty
} 

It "It should be the right day of the week" {
	$dayOfWeek = (Get-Date).DayOfWeek
	$responseContent.dayOfTheWeek | Should -Be $dayOfWeek
}

It "It should be the right year" {
	$year = Get-Date -Format "yyyy"
	$responseContent.currentDateTime | Should -BeLike "*$year*"
}

It "It should be the right month" {
	$month = Get-Date -Format "MM"
	$responseContent.currentDateTime | Should -BeLike "*$month*"
}

# These two tests assume you are running this outside daylight savings (during the winter) .. hacky but good way to showcase the syntax ;)
It "It should not be daylight savings time" {
	$responseContent.isDayLightSavingsTime | Should -Not -Be $true
}

It "It should not be daylight savings time another way" {
	$responseContent.isDayLightSavingsTime | Should -BeFalse
}
```

If you re-run the script now, you should get 7 passing tests, whihc should look like this in your console.

![Pester result with severn passing tests](https://github.com/martinkearn/Content/raw/master/Blogs/Images/pester-severntests.jpg)

## In Summary

Integration testing is an important tool for ensuring that your various system component work well together in their deployed state. It is usefull at the end of the project but also during development as you are building our components.

Pester and PowerShell ae a very flexible way to integration test using simple and well established PowerShell syntax, cmdlets and the easy to follow syntax of Pester.

For more resources:

- Full sample code: https://github.com/martinkearn/Pester-WorldClockApi
- Pester website: https://pester.dev
- Pester GitHub: https://github.com/pester/pester
- PowerShell docs: https://docs.microsoft.com/en-us/powershell/scripting/how-to-use-docs?view=powershell-7.1
- More articles from me: http://martink.me/articles

