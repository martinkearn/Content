
# Deployment in the Web App Service
A demo that shows dpeloyment with the Azure Web App Service. Specifically continuous integration and dpeloyment slots

## Create a new web app

Create new web app
* Default service plan
* http://mswebdayscale.azurewebsites.net
* Create resource group

Look at default placeholder site

*NOTE: A blank site is all well and good but I happen to have a lovely project in my GitHub already, how can I deploy that?*
 
## Setup Continuous deployment from GITHub
Configure continuous integration into our site
* From: https://github.com/martinkearn/MSWebDayScale.git
* Authorize account
* Choose project 
* Choose branch

Sync and wait for the deployment to happen
	
Access the live site see that it has been deployed

## Commit a change and watch it auto deploy
Change header in Visual Studio (change to Droitwich Spa)

Commit & Sync (push) in git

See it get automatically deployed to the staging site

Refresh browser and see new header

## Redeploy old deployment
Redeploy

Refresh browser and see old header

## Create a deployment slot
Look at live site and note URL `http://mswebdayscale.azurewebsites.net/`

Create a new deployment slot called 'beta'

Clone existing

Look at placeholder azure site and note url `http://mswebdayscale-beta.azurewebsites.net/`

## Change the site
Change the register button to magenta (to see if we get better conversions)

Publish to beta deployment slot
* Import publishing profile
* Publish from VS to Beta site

Show Beta site with old header

Show live site with 'ASP.net' header

Swap
* See that the headers for the two URLs have swapped