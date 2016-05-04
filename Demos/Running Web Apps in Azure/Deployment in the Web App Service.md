
# Deployment in the Web App Service
A demo that shows deployment with the Azure Web App Service. Specifically continuous integration and deployment slots.

### Pre-reqs
* Visual Studio 2015 open with https://github.com/martinkearn/MSWebDayScale 
 browser open to http://portal.azure.com and https://github.com/martinkearn/MSWebDayScale

## Create a new web app
Create new web app
* Default service plan
* http://mswebdayscale.azurewebsites.net or http://fibble.azurewebsites.net
* Create resource group
* Create app service plan (standard)

Look at default placeholder site

*NOTE: A blank site is all well and good but I happen to have a lovely project in my GitHub already, how can I deploy that?*
 
## Setup Continuous deployment from GitHub
Configure continuous integration
* All Settings > Deployment Source
* Authorize account
* Project : MSWebDayScale
* Branch: Master

Wait for the deployment to happen (no intervention required)
	
Access the live site see that it has been deployed

## Commit a change and watch it auto deploy
Change header in Visual Studio ()
* Index.cshtml
* Change to "Droitwich Spa"

Commit & Sync (push) in git

Show the commit in GitHub

See it get automatically deployed in the Azure portal
* Takes about a minute to detect and a further minute to build

Refresh browser and see new header

## Redeploy old deployment
Redeploy

Refresh browser and see old header

## Create a deployment slot
Note URL of site `http://mswebdayscale.azurewebsites.net` or `http://fibble.azurewebsites.net`

Create a new deployment slot called `beta`

Clone existing

Look at placeholder azure site and note that he url has '-beta'
* Have both slots open side-by-side
* Note the beta one has not yet bene deployed

## Deploy the beta slot and swap to production
Open the project in Visual STudio

Change the register button to magenta (to see if we get better conversions)
* Add `style="background-color:magenta;"` to the Register button

Publish to beta deployment slot
* Sign-in via Visual Studio
* Select app service > beta slot
* Publish

Compare two sites
* Show Beta site with magenta button
* Show original site with original grey button

Swap
* See that the headers for the two URLs have swapped

## Traffic routing
All settings > Traffic Routing

Split traffic 50/50 between two slots


