# Cognitive Services in Action
This demo shows some of the capabilities of Cognitive Services by using the live demo site and some of the spin-off sites created to show-case the APIs

### Pre-reqs
* Have easy access to the supporting files folder
* A high quality webcam on a cable
* The camera app open
* Picture of Madison open
* Props for computer vision api: Xamarin Stuffed Monkey, Pens, Coffee Cup, Phone etc
* Have the [Black Radley Intelligent Exhibit Windows 10 app](https://github.com/martinkearn/dinmore/tree/master/Client) installed and working. Have the PC that will run the demo configured as an 'exhibit'

## Computer Vision API
_Show the computer vision 'Analyse' feature via the main portal'_

Go to https://azure.microsoft.com/en-gb/services/cognitive-services/computer-vision/

Using 'Analyse an image'
* Show built-in pictures
* Take new photo and upload

## Custom Vision
_Use the custom vision API to train on items that Americans often call by the wrong name..... giving something back_

Show portal at https://azure.microsoft.com/en-gb/services/cognitive-services/custom-vision-service/

Go to https://www.customvision.ai/

Create new projects 'Americanisnts'

Upload images from ML Supporting Files\Custom Vision\Training Images

Tag the images

Train

Quick test with new images from ML Supporting Files\Custom Vision\Test Images 

## Emotion API
_Use a picture of Maddie to show the emotion API via the portal_

Show the [Emotion.jpg](https://github.com/martinkearn/Content/blob/master/Demos/Project%20Oxford/Supporting%20Files/Emotion.jpg)

Go to https://azure.microsoft.com/en-gb/services/cognitive-services/emotion/

Scroll down the Emotion API page and use the live demo section to analyse photo

The primary emotions here are:
* 61% contempt
* 20% disgust
* 9% anger

_This is what happens when you tell a three-year-old who has no respect for her father that she cannot have another piece of chocolate_

## Face API with Black Radley
_Use the Black Radley Intelligent exhibit app to show the face API in action ina real-world scenario. _

Run the app and how how it recognizes me as a 30-something-year-old (or even better get someone from the crowd) and gives me a voice description.

Walk away and see how the audio stops

Show a picture of an older person (or get one from the crowd) and not a different audio description.

Show a picture of a child and note a different audio description

## HowHappy.co.uk
_Show the HowHappy website which uses the emotion API and LUIS to sort faces in a photo by their emotion_

Show the main How Happy website in action
* Take a photo of the audience and upload a photo of the crowd
* Use the Luis prompt to ask for other emotions
    * Who is the happiest one here
    * Show me all the angry people
    * Who is the 3rd most surprised person
    * Show me the least happy person

Explore the Luis Model
* Go to https://www.luis.ai/ via the get started link
* Explore the intents, entities
* Review labels, show how entities are marked and the intent is labeled
* Add a new utterance "who are the angry ones"
* Train
* Publish > Update published application
* Use the Query to add a test query "who are the happy ones"

Re-query the website
* Who is the happiest one here
* Show me all the angry people
* Who is the 3rd most surprised person
* Show me the least happy person

Explain the results in more depth in context of the intents, entities and emotion api

## Sentimental
_This is an Office add-in that uses the Text Analytics API to do sentiment and key phrase analysis on text in Office documents. The add-in is called 'Sentimental' and you can get it from the [Office Store](https://store.office.com/sentimental-WA104379510.aspx?assetid=WA104379510&sourcecorrid=755ae580-2491-436f-8471-7888c38149d7&searchapppos=0)_

Open Excel

Install or activate Sentimental

Write `I love Office, it rocks` in a cell

Analyse

Insert score and key phrases

## Recommendations
_Use the Cortana Intelligence Recommendations Solution to show recommendations with a sample web site_

Open the Recommendations solution from https://gallery.cortanaintelligence.com/Tutorial/Recommendations-Solution

Talk to
* Item to item recommendations. i.e., what amazon calls "Customers who liked this product also liked these other products"
* Personalized user recommendations

Go to http://aka.ms/SportsShop

Use the app to explore sports wear items and their recommendations
* Item (http://sportswearshop.azurewebsites.net/Home/CatalogItem/27549): Outfit, You May Like, Cheaper
* Cart: You may like, Free shipping over $60 threshold
* User (use 00012): Based on your history
