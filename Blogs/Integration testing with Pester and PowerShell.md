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

OK, I'll admit it ..... depsite being an IT veteran of 23+years, I've never really had to deal with integration testing until recently. The project I was on used Pester and PowerShell to test the live system was doing what it was supposed to be doing.

I learnt a lot from this excersise and wanted to share what I learnt here.

## Integration testing; what and why?
Simply put, integration testing is the act of testing the deployed version of your system to ensure it operates as expected to in its "integrated" state along-side other system components. Integration tests catch bugs that affect how code operates as part of the wider deployed system, whihc it is not possible to catch by testing the system units (unit testing) alone.

In order to perform integration tests, you'll need a thing that simulates real transactions on your system and exercises known system interfaces, then asserts that the outputs are what you expected them to be.

> Throughput this article, I'll use [WorldClockApi.com](http://worldclockapi.com/) as a system to test; everyone can understand telling the time so it is a nice simple system to base examples on. I have nothing to do with this website. I have no idea who owns or maintains it, but it does seem to work.

According to the docs of WorldClockAPI, if I call this URI http://worldclockapi.com/api/json/utc/now, I should expect a json response detailing the time and some other date related details.

If I wanted to integration test this system, I would first send a request to tha http://worldclockapi.com/api/json/utc/now and then assert that the response was what I expected, which in this case is something like this

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

Fundamentally, that is all you need to know about integration testing. It is a way of testing that your system works in real life with other systems it depends on.