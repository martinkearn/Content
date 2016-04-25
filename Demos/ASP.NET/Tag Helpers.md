# ASP.net Tag Helpers
A demo that shows the built-in tag helpers and how they compare to teh old 4.x Htmlhelpers. We also create a new tag helper that converts Epoch date/timne to human readabledate time.

Tag Helpers are an update to what we used to use HtmlHelpers for; a way of rendering a server-side output within your razor mark-up. 

Htmlhelpers were messy and limited, but Tag Helpers are much more aligned to standard HTML which means that tooling and integration becomes a lot easier.

Three types of Tag Helper:
* Target existing elements and make changes. For example, adding something to an <input
* Define new elements such as <foo> that do their own thing
* Define elements with no output like the built-in <environment>

### Pre-reqs
* Have the default 4.x template with a person scafolded open: https://github.com/martinkearn/DefaultWebFrameworks/tree/master/ASPNet4.5.2/ASPNet452
* Have the default Core 1 template with a person scafolded open: https://github.com/martinkearn/DefaultWebFrameworks/tree/master/ASPNetCoreRC1/ASPNetCoreRC1
* Postman to show the SO API and Epoch time

## Built-in tag helpers
Open the ASP.net core 1 project with the Person scafolded (or create it)

Open Views > People > Create.cshtml page

Show `<form asp-action`
* Example of decorating existing HTML flags with asp.net functions

Show `<div asp-validation-summary`
* Choose how the form shoudl be validated

Show `<label asp-for`

Show `<input `asp-for`

Show `<a asp-action`

Open Views > Shared > _Layout.cshtml

Show `<environment>`
* Example of new tag
* Uses the `environment` environmental variable to differnetaiet where to get CSS. 
* Can be set differently on hosting provider (i.e. azure) 

Show `<link>`
* asp-fallback-href
* asp-append-version

## Compare to 4.x html helpers
Show how we used to do this in 4.x .... by using Htmlhelpers and point out how ugly this is

Open the ASP.net 4.x project with Person scafolded (or create it)

Show `@using (Html.BeginForm())`

Show `@Html.LabelFor(model => model.FirstName, htmlAttributes: new { @class = "control-label col-md-2" })`

Show `@Html.EditorFor(model => model.FirstName, new { htmlAttributes = new { @class = "form-control" } })`

Show `@Html.ActionLink("Back to List", "Index")`

## Create new EpochTagHelper
_Now we are going to create a tag helper to convert epoch time to human readable time_

Explain Epoch time using https://en.wikipedia.org/wiki/Unix_time and the stack overflow API
* In Postman, do a GET to https://api.stackexchange.com/2.2/questions?order=desc&sort=activity&tagged=asp.net-mvc6&site=stackoverflow 

Create a new ASP.net Core 1 project
* Use the 'Empty' template
* Don't need authnetication but does not matter either way

Create new folder called `TagHelpers`

Add new class called `EpochTagHelper`

Set the class to inherit from `TagHelper`
* Resolve using statement

Add the 'ProcessAsync` function as follows:
```
public override async Task ProcessAsync(TagHelperContext context, TagHelperOutput output)
{
    output.Content.SetContent("fictional race of small, mammaloid bipeds");
}
```

## View Imports
Open Views > _ViewImports.cshtml

Add `@addTagHelper "*, epoch"`

Point out that `@addTagHelper "*, Microsoft.AspNet.Mvc.TagHelpers"` is already there and would not be in an empty project

Open `project.json` to show that `Microsoft.AspNet.Mvc.TagHelpers` is already added as a nuget package
* You'd have to manually add this to an empty project

## Basic Markup
Open Views > Home > Index.cshtml

Create a `<h1>` at the top

In the `<h1>` add `<epoch></epoch>`
* Note the green code highlighting

Run the project and show the tag helper

## Do the Epoch date conversion
Open TagHelpers > EpochTagHelper.cs

Add the folloiwng code with in the `ProcessAsync` function
```
var childContentRaw = (await output.GetChildContentAsync()).GetContent();

var epoch = new DateTime(1970, 1, 1, 0, 0, 0, DateTimeKind.Utc);

var display = epoch.AddSeconds(Convert.ToDouble(childContentRaw));

output.Content.SetContent(display.ToString());
```

Build the solution

Go back to Index.cshtml

Add `299379600` within the `<epoch>` tag to look like this
```
<epoch>299379600</epoch>
```

Re-run and show that the page renders a human readable date
*The exact Epoch that i was born!

## Extend to support date format
_What if we wanted to have better control over the formatting of the date ... for example show it as a short date string?_

Open TagHelpers > EpochTagHelper.cs

Add a public property as follows
```
public string Formatter { get; set; }
```

Add `Formatter` to the `ToString()` method on the final line so it looks like this
```
output.Content.SetContent(displayDateTime.ToString(Formatter));
```

Build the solution

Go back to Index.cshtml

Update the `<epoch>` to include a date formatter
```
<epoch Formatter="dd MMM yyy">299379600</epoch>
```

Re-run and show nicely formatted date
