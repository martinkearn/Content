
# Emotions, Faces, Monkey Detection and Book Recommendations
This demo shows some of the capabilities of Cognitive Services by using the live demo site and some of the spin-off sites created to show-case the APIs

### Pre-reqs
* Have easy access to the supporting files folder
* A high quality webcam on a cable
* The camera app open
* Picture of Madison open
* Props for computer vision api: Xamarin Stuffed Monkey, Pens, Coffee Cup, Phone etc

## Key Steps
1. Computer Vision Portal

2. Emotion Portal Maddie

3. Live Camera sample

4. Twins / How Old

5. How Happy
* Who is the happiest one here
* Show me all the angry people
* Who is the 3rd most surprised person
* Show me the least happy person

6. Sentimental Office App

7. Recommendations - books website

## Computer Vision API
Go to https://azure.microsoft.com/en-gb/services/cognitive-services/computer-vision/

Show built-in pictures

_More on this later_

## Emotion API
Show the [Emotion.jpg](https://github.com/martinkearn/Content/blob/master/Demos/Project%20Oxford/Supporting%20Files/Emotion.jpg)

Go to https://azure.microsoft.com/en-gb/services/cognitive-services/emotion/

Scroll down the Emotion API page and use the live demo section to analyse photo

The primary emotions here are:
* 61% contempt
* 20% disgust
* 9% anger

_This is what happens when you tell a three-year-old who has no respect for her father that she cannot have another piece of chocolate_

## LiveCameraSample Windows App
_You can use any of the computer vision apis with video by uploading specific frames. This is a Windows UWP app that does exactly that_

Open Visual Studio > VideoFrameAnalysis solution

Run the app

Start camera

Cycle through all modes:
* Faces: Gender, age and camera angle
* Emotions: Emotion
* Emotion with face detect: (need to research what this does)
* Tags: Uses Computer Vision api to tag the image (show it props like Xamarin stuffed money, pens, coffee cup)
* Celebrities: Point camera at the Celebs.pdf

## TwinsOrNot.net
_This demo only really works for Martin Kearn because he apparently looks like Jake Gyllenhaal_

Go to <https://www.twinsornot.net>

Upload
* [Twins_Martin.jpg](https://github.com/martinkearn/Content/blob/master/Demos/Project%20Oxford/Supporting%20Files/Twins_Martin.jpg)
* [Jake.jpg ](https://raw.githubusercontent.com/martinkearn/Content/master/Demos/Machine%20Learning/Supporting%20Files/Jake.jpg)
* Should get a result of around 90% - #besties

## HowOld.net
_It is not all upside for Martin, apparently he looks 6 years older than he is, despite looking like Jake Gyllenhall_

Go to <http://how-old.net>

Upload [HowOld_Martin.jpg](https://github.com/martinkearn/Content/blob/master/Demos/Project%20Oxford/Supporting%20Files/HowOld_Martin.jpg)
* Result should be 42 (actual age 36)

_It is not that bad though, at least Jake looks old too_

Upload [Jake.jpg](https://raw.githubusercontent.com/martinkearn/Content/master/Demos/Machine%20Learning/Supporting%20Files/Jake.jpg)
* Result should be 43 (actual age 35)

## HowHappy.co.uk
Show the main How Happy website in action
* Take a photo of the audience and upload a photo of the crowd
* Use the luis prompt to ask for other emotions
    * Who is the happiest one here
    * Show me all the angry people
    * Who is the 3rd most surprised person
    * Show me the least happy person

Explore the Luis Model
* Go to https://www.luis.ai/ via the get started link
* Explore the intents, entities
* Review labels, show how entities are marked and the intent is labelled
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
Open the Cognitive Services website

Navigate to `Knowledge > Recommendations`

Talk to
* Frequently Bought Together (FBT) recommendations
* Item to item recommendations. i.e., what amazon calls "Customers who liked this product also liked these other products"
* Personalized user recommendations

Go to http://recommendationsapi.azurewebsites.net/

Use the app to explore books and their recommendations
