# How Happy
This demo will demonstrate the HowHappy.co.uk website as a basis for showing the emotion API and LUIS API from Microsoft Cogniive Services

### Pre Reqs
* Visit HowHappy.co.uk to wake it up
* Access to a LUIS account with this model imported: https://1drv.ms/u/s!ApiDsT2045uZgYnFTr_oD1R9z0h9NmE

## Show the main How Happy website in action
HowHappy.co.uk

Take a photo of the audience and upload a photo of the crowd

Use the luis prompt to ask for other emotions
* Who is the happiest one here
* Show me all the angry people
* Who is the 3rd most surprised person
* Show me the least happy person

## Explore the emotion API
Go to https://www.microsoft.com/cognitive-services/en-us/emotion-api

Upload the same photo

Explore the JSON result and face rectangles

## Explore the Luis Model
Go to https://www.microsoft.com/cognitive-services/en-us/language-understanding-intelligent-service-luis

Use the built-in weather bot to search for
* how is the weather in birmingham
* Will it rain in london tomorrow

Go to https://www.luis.ai/ via the get started link

Explore the intents, entities

Review labels, show how entities are marked and the intent is labelled

Add a new utterance "who are the angry ones"

Train

Publish > Update published application

Use the Query to add a test query "who are the happy ones"

## Re-query the website
* Who is the happiest one here
* Show me all the angry people
* Who is the 3rd most surprised person
* Show me the least happy person

Explain the results in more depth in context of the intents, entities and emotion api
