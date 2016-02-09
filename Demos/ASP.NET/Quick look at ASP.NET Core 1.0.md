
# A quick look at ASP.NET Core 1.0
In this demo, we'll look at some of the main changes to the project structure and tooling for an ASP.NET Core 1.0 project in the context of Visual Studio

### Pre reqs
* Visual Studio 2015
* ASP.NET Core 1.0 RC1 from https://get.asp.net/

## File > New Project
File > C# > Web > ASP.NET Web Application

Choose Web Application from the ASP.NET 5 templates
* Note that Authentication and 'Host in the cloud' are both options

When the solution loads, note the Welcome page

Note that Visual Studio is 'restoring packages'
* These are Nuget and NPM packages required by the template

Explore References and Dependencies

## Nuget and Project.json
Edit project.json

Add `"Microsoft.AspNet.Mvc.WebApiCompatShim": "6.0.0-rc1-final"`
* Show intelisense
* Show the 'restoring packages' UI on save

## NPM and Packages.json
Expose all files using the `Show all files` button in Solution Explorer

Show how the list correlates with `Dependencies > NPM` in solution explorer

Ask the audience for any noun and show how it looks up to NPM and installs it
* Use `"answer42": "1.0.3"` as a backup
* https://www.npmjs.com/package/answer42

Show 'restoring packages' again

## Bower and Bower.json
Add `"Font-Awesome": "4.5.0"`

Show 'restoring packages' again

## WWWroot
All static files (like the ones deployed by Bower) are stored in WWWRoot

Show `/WWWRoot/lib/Font-Awesome`

## Gulp
Explore `GulpFile.js`

Show the various Gulp plug-ins as NPM packages
* Show their NPM dependencies

## Startup.cs
Controls the ASP.NET pipeline

Middleware replaces many web.config settings

## Controller
Show account controller

Dependency injection

Much the same at a C# level