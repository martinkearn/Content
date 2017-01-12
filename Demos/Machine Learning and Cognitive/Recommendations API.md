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
Go to `Recommendation UI (Beta)`

Login using the api key

Create a new project and note `Model ID`

Upload catalog & usage files

Create `Recommendations build` and note Build ID

Create `FBT build` and note Build ID

_The builds will take a while so leave them running_

