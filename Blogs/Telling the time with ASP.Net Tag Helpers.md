# Telling the time with ASP.Net Tag Helpers
ASP.Net core has many new features but one of the ones which I'm particularly excited about is the introduction of Tag Helpers. This is simply because it makes my life easier as a developer and makes my code prettier to look at.

In this article we'll look at what Tag Helpers are and why they are a good thing. We'll look at some of the different types of tag helper and the built-in ones that are provided with ASP.net and we'll create our own Tag Helper to convert [Epoch/Unix time strings](https://en.wikipedia.org/wiki/Unix_time) to human-readable date/time strings.

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
Tag helpers do the same thing as HtmlHelpers functionally, but syntactically, they are much better and much nicer to work with.

There are three types of Tag Helper...

### Tag Helpers that add functionality to existing elements
Many of these are included out of the box with ASP.net. For example, there is a `<Form>` tag helper that adds attributes like `asp-action="YourActionHere"` which allows you to set the action the form will submit to.

Here are just a few examples of these:
```
<form asp-action="Create">
<div asp-validation-summary="ValidationSummary.ModelOnly" class="text-danger"></div>
<label asp-for="Age" class="col-md-2 control-label"></label>
<input asp-for="Age" class="form-control" />
<a asp-controller="Home" asp-action="Index">Home</a>
<link rel="stylesheet" href="~/css/site.min.css" asp-append-version="true" />
<script src="~/js/site.js" asp-append-version="true"></script>
```
### Tag Helpers to create new, non-standard elements
These can be useful if you want to do something like `<mytaghelper>whatever</mytaghelper>` and have the content be treated in some way by your server-side code.

### Tag Helpers with no output
These are tag helpers that do not directly render html but allow you to add some kind of logic to your razor mark-up. The built-in `<environment>` tag helper is an example of this. It lets you add different links/scripts based on your environment. For exmaple:
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

My tag helper is going to allow us to add something like `<epoch>299442480</epoch>` to our razor mark-up and have it render as a human-readable date/time.