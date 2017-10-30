# Recommendations API
This demo will show how to work with the Cognitive Recommendations API.

### Pre-reqs
* Edge
    * Cognitive Home
* Notepad open with API key in it: 2ac6eeb97db243c6a735797b1b396308
* `books_catalog_large.txt` and `books_usage_large.txt` avaliable
* Visual Studio open with this repo: https://github.com/martinkearn/Cognitive-Samples/tree/master/Recommendations/ASP.NET%20Core%201.0%20C%23

## Review Cognitive Recommendations website
Open the Cognitive Services website

Navigate to `Knowledge > Recommendations`

Talk to
* Frequently Bought Together (FBT) recommendations
* Item to item recommendations. i.e., what amazon calls "Customers who liked this product also liked these other products"
* Personalized user recommendations

## Examine the data and its schema
Examine our books catalog file `books_catalog_large.txt`

Examine our books usage file `books_usage_large.txt`

## Examine Recommendations UI project
Go to `Recommendation UI (Beta)` on the Recommendations API website

Login using the api key: `2ac6eeb97db243c6a735797b1b396308`

Open `Books` project

Explore the UI
* Catalog upload
* Usage upload
* FBT Build with ID
* Score build
* Recommendations build with ID
* Model ID

_Uploads and builds take a long time so we won't do this in the demo_

## Examine and use the API
Navigate to `API reference` on the Recommendations API website.

Go to `API Reference > Get Item to item recommendation > Open API Testing Console`

Use these `Query parameters`
* Model ID = bb2afe4c-e122-47ab-b739-da2004043481 (from recommendations ui)
* itemIds = 20442203 (from catalog text file)
* numberOfResults = 5
* minimalScore = 0
* buildId = 1601293 (from recommendations UI - default to active)
* Final URI: https://westus.api.cognitive.microsoft.com/recommendations/v4.0/models/61b5f30d-de8a-4a9c-b026-058081095ef9/recommend/item?itemIds=20442203&numberOfResults=5&minimalScore=0&buildId=1600480

Use these `Headers`
* Ocp-Apim-Subscription-Key = 2ac6eeb97db243c6a735797b1b396308 (API key)

Look for the `200/OK` result

Change the existing request to FBT by changing the `Build ID` query paremeter to `1601294`

## Inspect the Books App and Code
Go to http://recommendationsapi.azurewebsites.net/

Use the app to explore books and their recommendations

Go to the GitHub (linked from the Book Recommendation site)

Go to Cognitive-Samples/Recommendations/ASP.NET Core 1.0 C#/Recommendations/Repositories/RecommendationsRepository.cs 

Explore the `RecommendationsRepository` to show simple `HttpClient` calls



