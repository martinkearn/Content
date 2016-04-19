# Building HowHappy with the Emotion API
This demo will show how HowHappy.co.uk was created using ASP.Net core 1.0, JavaScript and the Microsoft Cognitive Services Emotion API

### Pre Reqs
* Visual Studio 2015
* ASP.NET Core 1.0 RC1 or later
* Copy of https://github.com/martinkearn/How-Happy GitHub Repo
* Visit HowHappy.co.uk to wake it up

## Explore the site
Open HowHappy.co.uk
* People can go to the website themselves, it is live

Upload `audience_2500.jpg` or `a-justin-bieber-concert-summed-up-in-one-picture-21677-1283366665-16.jpg`

Show the happiest/saddest
* Zoom in
* Click on someone to show score breakdown (#23 is s good one)

Flick through the other emotions

## Index.cshtml
Explore the simple HTML form (using bootstrap)

ASP.NET's new form tag helper

Input for file selection
* Standard input

JavaScript file (site.js)
* asp-append-version

## Home Controller > Result
Use of Session to store emotion data
* This enables change emotion without re-calling the api
* New way to work with Session in ASP.Net core: `byte[]` in and out
* `Microsoft.AspNet.Session"` nuget package required

GetEmotionData
* Coginitive service api call
* Uses HttpClient (a nuget package)
* Ocp-Apim-Subscription-Key
* Stream file content

GetFaces
* Deserialise JSON to .net class
* Use JSON.net (nuget package)
* Use http://json2csharp.com/ to generate c# classes
* Add display scores

Set stuff based on emotion
* Theme colour
* Icon
* Order of list

## Index.cshtml for result
Tag helpers
* Select - bound to Model.Emotions which is a select list

JavaScript for UI

Bootstrap tooltip and pop-over classes

