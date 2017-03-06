Anyone that has ever shopped on Amazon or listened to music on Spotify will be familar with the concept of recomendations where the user is offered recommendations of items based on browsing history, purchase history and other indicators of how the user has interacted with a product, artist etc.

Recommendation often take one of these forms:
    * **Frequently Bought Together**: Items that are frequent bought or used in conjunction with each other. For example on Amazon if you look at a [SONOS Play:1](https://www.amazon.co.uk/SONOS-PLAY-Smart-Wireless-Speaker/dp/B00FMS1KJK) speaker, it will tell you that [Flexson Desk Stand for Sonos PLAY:1](https://www.amazon.co.uk/Flexson-Desk-Stand-Sonos-PLAY/dp/B00YNGML8Q) is frequently bought with the speaker. Amazon show this under the 'Frequently Bought Together' and 'Customers Who Bought This Item Also Bought' section for every product page.
    * **Item-to-item Recommendations**: Items that are closely related to each other based on generic historical user interactions such as click-through, add-to-cart, download or any other kind of usage. On Spotify if you look at any Artists, you'll see 'Related Artists'
    * **Personalised Recommendations**: These are like item-to-item recommendations but the recomendatiosn are based on a user's personal interactions. In Amazon, you'll see this type of recommendation labelled as 'Related to items you've viewed' or 'Inspired by your purchases' on the home page.

Recommendations are a proven way to boost conversions, discoverability and user satisfaction for any digital service.

## The Cognitive Services Recommendations API
As part of the [Cognitive Services](https://www.microsoft.com/cognitive-services) suite of AI, Microsoft provide the [Recommendations API](https://www.microsoft.com/cognitive-services/en-us/recommendations-api) which enables recommendations on your own catalog based on your own usage.

The API is free to use for up to 10,000 transactions a month and relatively inexpesnive after that.

## Your Catalog & Features
To use the the API, you first need to provide a catalog which is a CSV file containing your items. These don't have to be products in an ecommerce store, they can be songs, files or anything that a user will 'use' or interact with.

As a minimum, you'll need to provide an `Item ID`, `Item Name` and `Item Category` (see the [full catalog schema here](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f316efeda5650db055a3e1)). 

You can optionally provide a list of 'features' which are additional categorical data points about the item. Features are used by the machine learning algorithms behind the API to make usage connections so the more features you can provide, the better (limit of 20). For example, if you are providing recommendation on bands, you could provide the music genre as the category and also provide location, year established, number of members, last gig location as features. Features are defined as key/value pairs in the CSV file. 

Features are used by the model when there is not enough transaction data to provide recommendations on transaction information alone. So features will have the greatest impact on “cold items” – items with few transactions. If all your items have sufficient transaction information you may not need to enrich your model with features.

As an example, the catalog entry for Metallica might look like this where '1234' is some unique ID

`1234,Metallica,Metal,location=San Fransicso,yearestalished=1981,numberofmembers=4,lastgiglocation=Mexico City`

If you are planning to use features, please take note of the advice in the 'Building your model' setcion.

You can get an exmaple catalog containing data about books, cinlduing feature data from http://aka.ms/RecoSampleData

You can upload your catalog file either through the [Recommendations UI tool](http://recommendations-portal.azurewebsites.net/) or the [API Upload Catalog Function](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f316efeda5650db055a3e1).

## Your Usage Data
Once you've uploaded your catalog, you can then upload your usage data. 

Usage data describes transactions where users have interacted with catalog items. This could be downloading a song, viewing a details page, adding to cart, purchasing etc. 

Usage data files have to contain the following data points (see the [full usage schema here](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f316efeda5650db055a3e2)). :
    * `UserID`: Some unique value that idetifies a user
    * `ItemID`: The id of the item that have used. This corrleates to the item ID's in your catalog
    * `Time`: A date/time stamp for the time when the usage occured.

You can also optionally defined the `Event` which indicates the type of usage that occured. Events can be one of the following and defaults to `Purchase` if not defined:
    * Click
    * RecommendationClick
    * AddShopCart
    * RemoveShopCart
    * Purchase

As with the catalog file, the usage data takes as CSV format. For example, if someone had viewed the artost details page for Metallica, the usage record might look like this (where 9876 is a unique user id)

`9876,1234,2017/03/06T12:45:00,Click` 

The quality of your model is heavily dependent on the quality and quantity of your data. The system learns when users buy different items (We call this co-occurrences).

A good rule of thumb is to have 20+ usage transactions per catalog item. So if you had 10,000 items in your catalog, you should have at least 20 times that number of transactions or about 200,000 transactions. If you do not have that level of usage, you may need consider alowing `cold items`. See the 'Building your model' section of this article for more detail.

You can upload usage files either via the [Recommendations UI tool](http://recommendations-portal.azurewebsites.net/) or the [API Upload usage file Function](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f316efeda5650db055a3e2) or you can post usage events directly from your application as they occur via the [API Upload usage event function](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/577d91f77270320f24da2592).

The model will take all usage files and events into account when performing a build.

## Building your model
Once you catalog and usage files\events have been uploaded, you can start a build. A build is the process where the machine learning engine analyses the data and works out the recommendations for each of your items.

There are three main types of build:
    * **Recommendation build**: Determines recomendations based on usage. Split into two sub-builds:
        * **Item-to-item (or ITI)**: Given an item or a list of items, it will predict other items that are likely to be of high interest to customers that have interacted with the original set of items. 
        *  **User-to-item (or U2I)**: Given a user id (and optionally a list of items) this option will predict a list of items that are likely to be of high interest to the given user (and its additional choice of items). The U2I recommendations are based on the history of items that were of interest to the user. 
    * **Frequently-Bought-Together build (or FBT)**: Counts the number of times two or three different products co-occur together, and then sorts the sets based on a similarity function (Co-occurrences, Jaccard, Lift). Given an item, and FBT build returns other items that are likely to occur in the same transaction. 
    * **Rank build**: A rank build is a technical build that allows you to learn about the usefulness of your features.

As inidicated in the 'Your Usage Data' setcion, builds require around 20+ usage events per catalog item to get recommendations. Some items may not have that level of usage data, these are referred to as `cold items`. If you have a large number of cold items in your data, you may want to explore the `AllowColdItemPlacement`, `UseFeaturesInModel` and `ModelingFeatureList` parameters when creating your build (see [API Create/Trigger build function](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f30d77eda5650db055a3d0) for the full list of parameters). These options along with many others are only avaliabel when creating a build via the API.


## Using your model

## ASP.net exmaple

## Microsoft Bot Framework example
