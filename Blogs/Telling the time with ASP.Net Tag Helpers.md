---
title: Telling the time with ASP.Net Tag Helpers
author: Martin Kearn
description: ASP.Net core has many new features but one of the ones which I'm particularly excited about is the introduction of Tag Helpers. This is simply because it makes my life easier as a developer and makes my code prettier to look at
image: https://dummyimage.com/800x600/000/fff&text=placeholder
thumbnail: https://dummyimage.com/200x200/000/fff&text=placeholder
type: article
status: published
published: 2016/04/27 09:40:00
categories: 
  - ASP.net
  - Tag Helpers
---

# Telling the time with ASP.Net Tag Helpers
ASP.Net core has many new features but one of the ones which I'm particularly excited about is the introduction of Tag Helpers. This is simply because it makes my life easier as a developer and makes my code prettier to look at.

In this article we'll look at what Tag Helpers are and why they are a good thing. We'll look at some of the different types of Tag Helper and the built-in ones that are provided with ASP.net and we'll create our own Tag Helper to convert [Epoch/Unix time strings](https://en.wikipedia.org/wiki/Unix_time) to human-readable date/time strings.

## What was wrong with HtmlHelpers?
Tag Helpers are a new way to do what we used to do with HtmlHelpers; render small snippets of server-generated output in our Razor views.

If you cast your mind back to ASP.net 4.x, you'll have probably written some razor mark-up like this

```
@Html.LabelFor(model => model.FirstName, htmlAttributes: new { @class = "control-label col-md-2" })
```

This code will generate a simple HTML label who's inner text is bound to the `model.FirstName` property in the view model which has underlying code looks like this
```
<label class="control-label col-md-2" for="FirstName">FirstName</label>
```

You'll notice that there is a big difference between the razor mark-up and the HTML that gets rendered for what seems to be a fairly simple, common-place element. There are a few key problems here:

* *Verboseness*: The HtmlHelper is actually longer than the resulting Html. This means there is more code to write and more places to go wrong
* *Complexity*: HtmlHelpers have their own unique code syntax, which means you have to learn them in order to understand how to use them fully
* *Tooling*: All of the tooling investments that have been made in Visual Studio around Html editing (things like css class selection, element completion, intellisense) are ignored for HtmlHelpers
* *Html*: HtmlHelpers are not standard Html, which means that if the code is contributed to by a non asp.net developer (for example designers or front-end developers), they will be confused. The example above is obviously a label, but you might see things like `@Html.EditorFor` where the type of Html control rendered depends on the type for the property it is bound to, which is even more confusing

## How do Tag Helpers make things better?
Tag Helpers do the same thing as HtmlHelpers functionally, but syntactically, they are much better and much nicer to work with.

There are three types of Tag Helper...

### Tag Helpers that add functionality to existing elements
Many of these are included out of the box with ASP.net. For example, there is a `<Form>` Tag Helper that adds attributes like `asp-action="YourActionHere"` which allows you to set the action the form will submit to.

Here are just a few examples of these:
```
<form asp-action="Create">
<div asp-validation-summary="ValidationSummary.ModelOnly"></div>
<label asp-for="Age" class="control-label"></label>
<input asp-for="Age" class="form-control" />
<a asp-controller="Home" asp-action="Index">Home</a>
<link href="~/css/site.min.css" asp-append-version="true" />
<script src="~/js/site.js" asp-append-version="true"></script>
```
### Tag Helpers to create new, non-standard elements
These can be useful if you want to do something like `<mytaghelper>whatever</mytaghelper>` and have the content be treated in some way by your server-side code.

### Tag Helpers with no output
These are Tag Helpers that do not directly render html but allow you to add some kind of logic to your razor mark-up. The built-in `<environment>` Tag Helper is an example of this. It lets you add different links/scripts based on your environment. For example:
```
<environment names="Development">
    <link rel="stylesheet" href="~/lib/bootstrap/dist/css/bootstrap.css" />
    <link rel="stylesheet" href="~/css/site.css" />
</environment>
<environment names="Staging,Production">
    <link rel="stylesheet" href="https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.min.css"
            asp-fallback-href="~/lib/bootstrap/dist/css/bootstrap.min.css"
            asp-fallback-test-class="sr-only" asp-fallback-test-property="position" asp-fallback-test-value="absolute" />
    <link rel="stylesheet" href="~/css/site.min.css" asp-append-version="true" />
</environment>
```

## Building a Tag Helper
We are going to build a Tag Helper that uses its own tag to convert [EPOCH time (or 'Unix'/'Posix' time)](https://en.wikipedia.org/wiki/Unix_time) to human readable time.

EPOCH time is an integer that represents the number of seconds between 00:00:00 Coordinated Universal Time (UTC), 1st January 1970 and a date/time. For example, I was born at 18:28 on 28th June 1979 which is '299442480' as an EPOCH.

My Tag Helper is going to allow us to add something like `<epoch>299442480</epoch>` to our razor mark-up and have it render as a human-readable date/time.

### 1. Create a 'web application' project
I strongly suggest you use the 'web application' project template as a starter. This will already have all the packages and configuration you need to use Tag Helpers.

In this example, the project is called 'epoch'.

### 2. Add your class
Create a folder at the root of your project called 'TagHelpers'. 

In that folder add a class which inherits from `TagHelper`. This is what makes it a Tag Helper rather than a standard class. By convention your file (and class) name should be post-foxed with 'TagHelper'. For our demo we'll use 'EpochTaghelper'. Your class should look something like this:

```
namespace epoch.TagHelpers
{
    public class EpochTagHelper : TagHelper
    {
    }
}
```

### 3. Implement ProcessAsync
Tag Helpers have two functions where the main code for the Tag Helper is executed, `Process` and `ProcessAsync`. We'll use the Async one.

Add this code to your `EpochTagHelper` class

```
public override async Task ProcessAsync(TagHelperContext context, TagHelperOutput output)
{
    output.Content.SetContent("Hello world");
}
```

You should build the project at this point to ensure all is well.

We'll stop the code for now and see if we can get "hello world" rendered in our Views.

### 4. _ViewImports.cshtml
_ViewImports.cshtml is a neat file that allows you to add Tag Helpers and `using` statements to all your views rather than having to add to them all individually.

It is located in /Views/_ViewImports.cshtml. If you open this file, you'll see that you already have a line referencing the default Tag Helpers. To add your own Tag Helper, simply add its namespace using the `@addTagHelper`, in our example you need to add

```
@addTagHelper "*, epoch"
```

### 5. Add the mark-up
We are now getting to the fun bit, we can add our Tag Helper mark-up to a view.

Open the /Views/Home/Index.cshtml view and add this code at the top. You'll notice it goes a teal colour when you add it.

```
<epoch>1461483059</epoch>
```

If you run the project, you should find that "hello world" is displayed where you added the Tag Helper. This means it is working :)

### 6. Epoch logic
Lets now implement some logic in our Tag Helper to convert the epoch date to a human readable date.

Go back to /TagHelpers/EpochTagHelper.cs and add this code to your `ProcessAsync` method.

```
//Get child content (the content within the tags)
var childContentRaw = (await output.GetChildContentAsync()).GetContent();

//Check the child content is a double
double childContent;
bool isDouble = Double.TryParse(childContentRaw, out childContent);
if (isDouble)
{
    //Establish a base Epoch dateTime
    var epoch = new DateTime(1970, 1, 1, 0, 0, 0, DateTimeKind.Utc);

    //calculate the passed in date in DateTime format
    var display = epoch.AddSeconds(Convert.ToDouble(childContent));

    //display output
    output.Content.SetContent(display.ToString());
}
else {
    //if it is not a double, just display the raw child content
    output.Content.SetContent(childContentRaw);
}
```
The logic of this code should be fairly self-explanatory, but there are a few key bits:
* `(await output.GetChildContentAsync()).GetContent()` is how you get the inner text for your Tag Helper. In our case, this is how we get the Epoch string of "1461483059" contained within the `<epoch>` tag
* `output.Content.SetContent(display.ToString());` is how we set the output of the Tag Helper. In our case this is where we put the human readable date/time

If you re-run your application now, you should find is displays the converted, human-readable date/time.

### 7. Add a parameter
The output the previous step is great, but it would be nice to be able to set a date formatter in my razor mark-up to dictate how the date/time is formatted using [C# Custom Date and Time Format Strings](https://msdn.microsoft.com/en-us/library/8kb3ddd4(v=vs.110).aspx).

To do this, we can add a public property to our Tag Helper class. Go back to /TagHelpers/EpochTagHelper.cs and add this public property above the `processAsync` method

```
public string Formatter { get; set; }
```

You can now reference this as the formatter for the `output.Content.SetContent` line. Update it so it looks like this

```
//display output
output.Content.SetContent(display.ToString(Formatter));
```

We now need to update our mark-up to use this property. Go to /Views/Home/Index.cshtml and update your `<epoch>` tag to look like this

```
<epoch Formatter="dd MMM yyy">1461483059</epoch>
```

If you re-run the project, you'll see that the date has been formatted in a more readable way.

## In Summary
Tag Helpers are agreat way to clean up your razor mark-up whilst still maintaining the power of having server-side code bubbling up to your mark-up.

You can find out more using these resources:
* [The GitHub repository containing this code sample](https://github.com/martinkearn/EpochTagHelper)
* [Docs.Asp.Net tutorial on Tag Helpers](https://docs.asp.net/en/latest/mvc/views/tag-helpers/authoring.html)
