
In this demo we'll cover the three main package managers used with ASP.NET core; Nuget, NPM and Bower.

### Pre-Reqs

* Visual Studio 2015
* ASP.NET Core 1.0 RC2 or later

## Create a new project

File > New > Project > Visual C# > Web > ASP.NET Core Web Application (.NET Core) > Empty template

Note that references are 'Restoring' on initial project creation (This is Nuget installing the default packages)

## Nuget

Project > Right-click > Manage Nuget Packages

Three installed by default:

* `Microsoft.AspNetCore.Server.IISIntegration`
* `Microsoft.AspNetCore.Server.Kestrel`
* `Microsoft.NETCore.App`

Search for new packages in the 'Browse' section

* Install `Microsoft.EntityFrameworkCore.SqlServer` (need to tick Include prerelease)
* Save and notice dependencies being restored

Open `project.json`

* Notice that `Microsoft.EntityFrameworkCore.SqlServer` has been added
* Install `"Newtonsoft.Json": "8.0.3"`
* Save and notice dependencies being restored

Explore the `References` section

## NPM

Project > Right-click > Add > New Item > Search for `NPM` > Add `package.json`

Add Gulp directly to packages.json

* `"gulp": "^3.9.1"`
* Save and notice dependencies being restored

Explore NPM dependency tree. Dependencies > npm > gulp > all its sub-dependancies

Look on disk. Project > Right-click > Open Folder in File Explorer

Traverse `node_modules`

* node_modules > gulp > node_modules > chalk > node_modules > ansi_styles
* Open index.js in VS code to show JavaScript

Open `package.json` through UI

* Close package.json
* Dependencies > npm > right-click > Open package.json

## Bower

Project > Right-click > Add > New Item > Search for `Bower` > Add `bower.json`

Add Bootstrap through the UI

* `"bootstrap": "^3.3.6"`
* Save and notice dependencies being restored, just like NPM

Explore NPM dependency tree

* Dependencies > bower > bootstrap > jquery

Open `bower.json` via UI

* Close bower.json
* Dependencies > bower > right-click > Open bower.json

Add foundation-sites directly in bower.json

* `"foundation-sites": "^6.2.1"`
* Save and notice dependencies being restored, just like NPM

Look on disk

* Project > Right-click > Open Folder in File Explorer
* wwwroot > lib (wwwroot is the right place to reference in your HTML - no additional steps needed)
* notice only a single version of JQuery despite both Foundation and Bootstrap having dependencies on it

## NPM vs Bower

Add TypeIt and Foundation-sites to NPM

* Dependencies > npm > right-click > open package.json
* `"typeit": "^4.0.0"`
* `"foundation-sites": "^6.2.1"`

Explore `node_modules` on disk to see two copies of JQuery beneath TypeIt and Foundation

* \node_modules\foundation-sites\node_modules\jquery
* \node_modules\typeit\node_modules\jquery
* _NOTE_ This is due to NPM's nested dependency approach