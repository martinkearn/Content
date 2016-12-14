# Web Hack Wednesday - LUIS - Code How Happy Lite
This is the demo script for the third demo from the LUIS Web Hack Wednesday episode.

### Pre Reqs
* The snippets contained [here](https://github.com/martinkearn/Content/tree/master/Demos/Machine%20Learning%20and%20Cognitive/HowHappyLiteSnippets) imported into Visual Studio
* ASP.Net Core 1.0.0 with tooling preview 2 or higher

## New Project
Open Visual Studio

File > New Project > Visual C# > Web > ASP.NET Core Web Application (.NET Core)
* Web Application template
 
## Index.cshtml
Clear everything in Views/Home/Index.cshtml

Add the snippet `hhlindex`

Briefly explore the code
* A bit of style to draw the rectangles
* A form that posts the image and luis query to the MVC controller
* A DIV to display the results

## Site.js
Clear everything in wwwroot\js\site.js

Add the snippet `hhljs`

Breifly explore the code
* `SubmitForm` Handles form submit event via Ajax
* `ShowImage` Paints the uploaded image
* `ProcessResult` Uses the JSON received back from MVC controller to paint rectangles and data over image

## Add Face Model
Create a `Models` folder

Create a class called `Face.cs`

Remove the default `Face` class

Add `hhlfacemodel` snippet below `namespace hhl.Models {`

Explore the model a little:
* Face contains the bounds of a rectangle highlighting the face in the orginal image
* Score for each emotion

## Add LuisResult C# Model
Create a `Models` folder

Create a class called `LuisResult.cs`

Remove the default `LuisResult` class

Add `hhlluisresultmodel` snippet below `namespace hhl.Models {`

Explore the model a little:
* Intent is each intent in the model with a score for how highly the text scores for that intent
* Entity are the entities within the model

## Update the Controller
Open Controllers\Home

Add `hhlc1` snippet
* Add the main `Result` action
* Gets the file and luis query from submitted form

Add `hhlc2` snippet below `var file...`
* Resolve all using statements using the first option in the lightbulb
* This uses http client to make the API call to Cognitive Services Emotion API

Add `hhlc3` below `var responseString...`
* Resolve all using statements using the first option in the lightbulb
* Converts JSON received from CS api to the faces model

Add `hhlc4` below the closing `}` for the emotion `httpclient`
* This uses httpclient to call into luis

Add `hhlc5` below `luisResult = JsonConvert...`
* This gets the emotion from teh luis result and sorts the faces based on that emotion

Add `hhlc6` below the closing `}` for teh luid httpclient
* This returns the sorted list of faces as json

## Run it
* Who is the happiest one here
* Show me all the angry people
* Who is the 3rd most surprised person
* Show me the least happy perso
