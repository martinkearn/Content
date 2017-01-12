# Recommendations API
This demo will show how to work with the Cognitive Recommendations API.

### Pre-reqs
* Edge
    * Amazon.co.uk - not signed in
    * Cognitive Home
* Notepad open to log keys etc
* Visual Studio open with this repo: https://github.com/martinkearn/Cognitive-Samples/tree/master/Recommendations/ASP.NET%20Core%201.0%20C%23

## Show Recommendations on Amazon.co.uk
Go to Amazon.co.uk

Search for `Forza Horizon 3 (xbox one)`

Show `Frequently bought together`

Show `Customers who bought this item also bought` - we called this "item to item recommendations"

Login to Amazon.co.uk

Go to `Home > Related to products you viewed > More > Your recently viewed items and featured recommendations`

## Review Cognitive Recommendations website
Open the Cognitive Services website

Navigate to `Knowledge > Recommendations`

Talk to
* Frequently Bought Together (FBT) recommendations
* Item to item recommendations. i.e., what amazon calls "Customers who liked this product also liked these other products"
* Personalized user recommendations

## Get an API keys
Open the `Recommendations API` website click `Get started for free`

Sign in with a Microsoft account (live.co.uk)

Show the key and copy to Notepad

## Examine the data and its schema
Examine our books catalog file

Examine our books usage file

Show where to get the sample files
* Documentation > Getting Started Guide > Task 2 Did you bring your data? > Link to OneDrive

Examine catalog schema
* API Ref > Upload a catalog file to a model
		
Examine usage schema
* API Ref > Upload a usage file

## Create a project
Go to `Recommendation UI (Beta)` on the Recommendatiosn API website

Login using the api key

Create a new project and note `Model ID`

Upload catalog & usage files

Create `Recommendations build` and note Build ID

Create `FBT build` and note Build ID

_The builds will take a while so leave them running_

## Examine the API
Navigate to `API reference` on the Recommendations API website.

Look at `Get item-to-item recommendations`
* For regular item-to-item recommendations and FBT

Look at `Get user-to-item recommendations`
* For personalised item recommendations and FBT

Click on `SDK` from the Recommendations API website
* This apps covers build and catalog management which represents most of the other API functions

## Use the API test console
Go to `API Reference > Get Item to item recommendation > Open API Testing Console`

Use these `Query parameters`
* Model ID = 61b5f30d-de8a-4a9c-b026-058081095ef9 (from recommendations ui)
* itemIds = 20442203 (from catalog text file)
* numberOfResults = 5
* minimalScore = 0
* buildId = 1600480 (from recommendations UI - default to active)
* Final URI: https://westus.api.cognitive.microsoft.com/recommendations/v4.0/models/61b5f30d-de8a-4a9c-b026-058081095ef9/recommend/item?itemIds=20442203&numberOfResults=5&minimalScore=0&buildId=1600480

Use these `Headers`
* Ocp-Apim-Subscription-Key = 48905f53e07a46138cc413cd04efb325 (API key)

Look for the `200/OK` result

Change the existing request to FBT by changing the `Build ID` query paremeter to `1600485`

## Inspect the Books App
Run and use the books web app via Visual Studio

Look through code of app
* Books repository to load CSV. 
    * Uses .net core DI repository model
* Recommendations repository
    * GetITIItems - helper for standard HttpClient call using the ITI build id from app settings
    * GetFBTItems - helper for standard HttpClient call using the FBT build id from app settings



