# Telling the time with ASP.Net Tag Helpers
ASP.Net core has many new features but one of the ones which I'm particularly excited about is the introduction of Tag Helpers. This is simply because it makes my life easier as a developer and makes my code prettier to look at.

In this article we'll look at what Tag helpers are and why they are a good things. We'll look at some of the different types of tag helper sand the built-in ones that are provided with ASP.net and we'll create our own Tag helper to convert [Epoch/Unix time strings](https://en.wikipedia.org/wiki/Unix_time) to human-readable date/time strings.

## What is a Tag Helper and why do we need them?
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

*Verboseness*: The HtmlHelper is actually longer than the resulting Html. This means there is more code to write and more places to go wrong
*Complexity*: HtmlHelpers have their own unique code syntax, which means you have to learn them in order to understand how to use them fully
*Tooling*: All of the tooling investments that have been made in Visual Studio around Html editing (things like css class selection, element completion, intellisense) are ignored for htmlhelpers
*Html*: HtmlHelpers are not standard Html, which means that if the code is contributed to by a non asp.net developer (for exaMple, Designers or Front-end Developers), they will be confused. The example above is obviously a label, but you might see things like `@Html.EditorFor` where the type of Html control rendered depends on the type for the property it is bound to