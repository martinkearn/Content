
# Building HowHappy Lite
This demo goes from a new project to a lite version of the How Happy application using various snippets

### Pre Reqs
* The snippets contained [here](https://github.com/martinkearn/Content/tree/master/Demos/Machine%20Learning%20and%20Cognitive/HowHappyLiteSnippets) imported into Visual Studio
* ASP.Net Core 1.0.0 with tooling preview 2 or higher
* A picture of a group of people (see supporting files if you can't take one)

## Show the main How Happy website in action
HowHappy.co.uk

Upload a photo of the crowd

Explore other emotions

## New Project
Open Visual Studio

File > New Project > Visual C# > Web > ASP.NET Core Web Application (.NET Core)
* Web Application template
 
## Index.cshtml
Clear everything in Views/Home/Index.cshtml

Add the snippet `hhlindex`

Breifly explore the code
* A bit of style to draw the rectangles
* A form that posts the image to the MVC controller
* A DIV to display the results

## Site.js
Clear everything in wwwroot\js\site.js

Add the snippet `hhl-js`

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

## Controller
Open Controllers\Home

Add `hhlc1` snippet
* Add the main `Result` action
* Gets the file from submitted form

Add `hhlc2` snippet below `var file...`
* Resolve all using statements using the first option in the lightbulb
* This uses http client to make the API call to Cognitive Services

Add `hhlc3` below `var responseString...`
* Resolve all using statements using the first option in the lightbulb
* Converts JSON received from CS api to the faces model
* Orders the faces by happiness

Add `hhlc4` above the closing `}`
* This returns the `Faces` object as json to the view
