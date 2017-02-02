# Cognitive Sevices in Action - All 30 mins
A tour around some of the more popular Cognitive Services

### Pre Reqs
* A good quality, portable web cam, set as the default camera
* Computer Vision 'LiveCameraSample' app built and pinned to Start
* Props for computer vision api: Xamarin Stuffed Monkey, Pens, Coffee Cup, Phone etc
* The Obama clip loaded in Groove
* The GUID joke in notepad
* Windows Translator app installed
* An object with some non-english printed text

## Computer Vision > Analyse an image
Go to https://www.microsoft.com/cognitive-services/en-us/computer-vision-api

Use the webcam to take a photo of lecturn (coffee cups are good)

Upload the photo and analyse
  * Description
  * Tags
  * Adult / racy
  * Categories
  * Colours
  
Repeat with a stock portal image

## Face API
Go to https://www.microsoft.com/cognitive-services/en-us/face-api

Explore the portal
* Face Detection
* Face Verification 
* Face Identification
* Similar Face Searching
* Face Grouping

Take a selfie and upload it using the Face Detection section

Jake demo on https://www.twinsornot.net/

Jake demo on http://how-old.net/ 

## Emotion API
Show the [Emotion.jpg](https://github.com/martinkearn/Content/blob/master/Demos/Project%20Oxford/Supporting%20Files/Emotion.jpg)

Go to https://www.microsoft.com/cognitive-services/en-us/emotion-api

Scroll down the Emotion API page and use the live demo section to analyse photo

The primary emotions here are:
* 61% contempt
* 20% disgust
* 9% anger

_This is what happens when you tell a three-year-old who has no respect for her father that she cannot have another piece of chocolate_

## LiveCameraSample Windows App
_You can use any of the computer vision apis with video by uploading specific frames. This is a Windows UWP app that does exactly that_

Run `LiveCameraSample` app from Start menu

Start camera

Cycle through all modes:
* Faces: Gender, age and camera angle
* Emotions: Emotion
* Emotion with face detect: (need to research what this does)
* Tags: Uses Computer Vision api to tag the image (show it props like Xamarin stuffed money, pens, coffee cup)

## Bing Speech
Go to https://www.microsoft.com/cognitive-services/en-us/speech-api

Set the language to `English - GB`

Record the text in the paragraph above the input

Show again using a sample

Go to `Text to Speech`

Set language to `English GB` and `Susan`

Copy this text `Two GUID's walk into a bar .... GUID one says "a wine, a beer and a nice glass of champagne".......the barman says "That's a strange order!" ......GUID two says "Ignore him, he's random"`
  * Play again with `George`
  * Note punctuations and pauses
  * Also note that it does not recgnise `GUID` properly

## Bing Spell Check
Go to https://www.microsoft.com/cognitive-services/en-us/bing-spell-check-api

Scroll down each of the main features and epxlain them
  * Word Breaks
  * Slang
  * Names (famous names)
  * Homonyms ('four' used instead of 'for' as an exmaple)
  * Brands
  
Scroll up and show the API using each of the samples 

## LUIS
Use slides to explain Intents and Entities

Go to http://HowHappy.co.uk and ask these questions with audience photo or justin beiber photo
 * Who is the happiest one here
 * Show me all the angry people
 * Who is the 3rd most surprised person
 * Show me the least happy person

## Text Analytics
Go to https://www.microsoft.com/cognitive-services/en-us/text-analytics-api

Analyse the text "I think that artificial intelligence is the future and I love it"
 * Senitment anlaysis - 98% sentiment
 * Tey Phrase Extraction - artificial intelligence, future
 * Language detection - English (confidence: 100%) 
 
Show the same in Sentimental office app

## Translator
Go to https://www.microsoft.com/cognitive-services/en-us/translator-api

Talk through each of the main functions
  * Language Detection
  * Translation
  * Text-to-speech
  * Custom translation system
  * Collaborative Translation Framework (CTF)
  
Open the Windows Translator app

Choose French > English

Point the camera at the white 'Biscuits Fins De Montmarte' leaflet
 * The app translate the french text to english and overlays
 
Switch langauges so it is English > French

Use audio to translate the phrase "I promise i did not eat all of those nice french biscuits on my own"
 * Play the english and french audio files

## Q and A maker
Go to https://www.microsoft.com/cognitive-services/en-us/qnamaker

Go to `Get started for free` or https://qnamaker.ai/

Go to My Services > Sonos > Edit
 * Add this URL if it is not pre-created http://www.sonos.com/en-gb/faq#what-music-services-work

Go to settings and show the URL and also show in the browser

Use `test` to ask questions
  * "what music servcie work with sonos"
  * "does spotify work with sonos"
  * "Does sonos work with my tv"
  * "sonos vs bluetooth"

## Recommendations
Show `books_catalog_large` and `books_usage_large`

Talk through http://www.sonos.com/en-gb/faq#what-music-services-work

## Bing apis
Talk through each of the Bing APIs