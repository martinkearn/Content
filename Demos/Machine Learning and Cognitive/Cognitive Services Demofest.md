# Cognitive Services Demofest

A set of demos to showcase the latest features of Cognitive Services. This will not necessarily show all the primary features, just things that are new or updated in the past 12-18 months.

This demo is intended to be delivered by Martin Kearn and relies on pre-built, undocumented configurations in Azure etc.

### Pre-Reqs

* Webcam
* Microphone
* Contents of [ML Supporting Files](https://github.com/martinkearn/Content/tree/master/Demos/Machine%20Learning%20and%20Cognitive/ML%20Supporting%20Files) cloned
* Printed text for people to copy (handwriting demo)
* Blanks sheets of paper and pens (handwriting demo)
* Event lanyard (OCR demo)
* Zooming software
* Black Radley app installed and configured
* Photos of young person and old person

### Assumptions

* All logins using @Microsoft.com account



## Computer Vision

_Computers can see things_

### Analyze an image (Computer Vision)

* http://aka.ms/CognitiveServices > Vision > Computer Vision > Analyse an image
* Photograph audience with webcam
* Upload to ‘Analyse an image’
* Explore results (faces, description, colours)
* Upload `SexyBill.jpg`
* Note ‘racy' score

### Printed Text (Computer Vision)

* http://aka.ms/CognitiveServices > Vision > Computer Vision > Read text in images
* Photograph event lanyard with webcam
* Upload
* Explore results
* Explore JSON

### Handwritten Text (Computer Vision)

- http://aka.ms/CognitiveServices > Vision > Computer Vision > Read handwritten text from images
- Have someone hand write some pre-prepared sentences on paper
- Photograph with webcam
- Upload
- Explore results
- Explore JSON

### Car Registrations (Custom Vision)

Talking points

* Had Custom Vision for a year or two

* Ability to train with your own photos

* Now have object detection - recognize a specific object within an image

* Built a model to recognize vehicle registrations

* Talk around you could use this in conjunction with smart cropping and OCR to create a registration extraction flow


Demo

* http://aka.ms/CognitiveServices > Vision > Custom Vision > Getting Started > Sign In (@Microsoft.com)
* Vehicle Registration model
* Explore images and tags
* Upload `Registration-Untagged-1.jpg` and tag registration
* Train > Observe results
* Test with `Registration-test-1.jpg`, `Registration-test-2.jpg`

### BananaLens

_Showcase the Custom Vision API in the context of the hack we did with Sainsburys_

Talking Points

* Make sure the topic has been introduced via slides first

Demo:

- http://aka.ms/CognitiveServices > Vision > Custom Vision > Getting Started > Sign In (@Microsoft.com)
- Bananas-Colour model
- Explore images and tags
- Test `Banana Colour 2.jpg`, `Banana Colour 4.jpg`
- Bananas-Defect model
- Test `Banana Crown Rot.jpg`

## Museum of Microsoft Technology

_Use the Black Radley Intelligent exhibit app to show the face API and face lists in action in a real-world scenario_

Talking points

* Face API with Face List for recognition and tracking
* Audio could now be improved with features I'll talk about later
* Imagine you're in a museum of Microsoft technology and you are looking at the 'Surface Pro 3' exhibit

Demo:

* Get a volunteer that thinks they look less than 30
* Run the app, wait for it to onboard
* Have the 30 year old person approach > age appropriate audio starts
* Have them walk away > audio stops
* Repeat for older person (in person or photo)
* Show child's photo

## Speech API

_Use the Cognitive portal to show the various Speech API services_

### Speech-to-text

* http://aka.ms/CognitiveServices > Vision > Speech > Speech-to-Text > Speech transcription
* `Start Recording` and speak the description to the Speech transcription section
* Also play a sample

### Custom Speech

Talking points

* Custom acoustic environments (airports, hospitals, railway stations, drive-throughs)
* Custom terminology (scientific, technical, legal, medical etc)

Demo:

* http://aka.ms/CognitiveServices > Vision > Speech > Speech-to-Text > Custom Speech service: Speech transcription with custom model
* Play `sample sentance 1` and note differences between the baseline and custom speech
* Play `Sample sentance 4` for custom terminology

### Text-to-speech

Talking points

* 75 voices over 45 languages
* Male and female voices
* Parameters to adjust pitch, volume, pronunciation and rhythm
* Custom voice fonts (for characters, celebrities etc)

Demo:

* http://aka.ms/CognitiveServices > Vision > Speech > Text-to-speech > Bring natural voice to your apps
* Play the various samples
* http://aka.ms/CognitiveServices > Vision > Speech > Text-to-speech > Text to Speech with custom voice models
* Set quality to `Basic` and play `sample voice 2`
* Set quality to `Standard` and play `sample voice 2`
* Set quality to `High quality ` and play `sample voice 2` (this is a custom font)

## Translator App

_Show the Microsoft translation app in use in a live scenario_

Talking points:

* App available on any platform
* Translates between 60 languages
* Text, Speech, Vision
* Conversation mode enables live multi-lingual translation

Demo:

* Get someone who speaks another language (failed that, use German on the iPhone)
* Launch Translator app on PC > Conversation tab > Start Conversation
* Martin, English
* Launch translator app on phone > Conversation icon > Join by scanning QR
* Have a voice or text conversation
  * If talking to myself try to say "I have a guinea pig" / "Ich habe ein Meerschweinchen"

## Banko LUIS

_Use a typical banking scenario to show how Luis works_

Talking Points

* Intent
* Entities

Demo

### Access and Initial Tour

* http://Eu.luis.ai > @microsoft.com account > Banko
  * Talk about the EU domain name

* Introduce basic UI



### Balance Intent

* Using test UI, ask `What is my balance`

* Inspect result

* Explore “balance” intent and utterances



### Transfer Intent

* Explore each of the entities

* Look at “balance” intent and explore utterances

* Switch between entities view on/off

* Ask `can I move £2. 50 from savings to david kearn on saturday` Inspect result

* Explore raw Json
  * Manage > Keys & Endpoints > Starter key > Open in new browser tab > paste utterance after `q=` in address



### Banko Bot (if available)

* `what is my balance`
* `Send Amy Kearn £20`

* `Please Send Amy Kearn £20 rom the savings account on Saturday`

## Named Entity Recognition

_Show the new named Entity recognition feature in Text Analytics service_

Talking Points

* Text Analytics been around for a while
* Sentiment, key phrase, language
* Entity Recognition is a new feature
* Entity is basically anything that has a page on Wikipedia

Demo

### Entity recognition & sentiment

* http://aka.ms/CognitiveServices > Language > Text Analytics
* “I enjoyed going to [London](https://en.wikipedia.org/wiki/London) and looking around the [Excel](https://en.wikipedia.org/wiki/ExCeL_London)” (Make sure Excel is capitalized)
  * London and Excel both picked up as named entities
  * Disambiguated between “London excel” and “Microsoft excel”
  * High sentiment score

* “My least favourite application is Excel, I really cannot understand those formulas”
  * This time, it chose Microsoft Excel
  * Low sentiment score

### Language Detection

* Now use the translator app to translate “My least favourite application is Excel, I really cannot understand those formulas” to another language and paste in
  * Language detection
  * Still does entity recognition

## Brexit Bot

_Show how easy it is to create an FAQ bot from an existing FAQ data source_

Talking Points

* QNAMaker been around for a while
* Like Luis, it is continually being improved and updated

Demo

### Create knowledge base

* http://aka.ms/CognitiveServices > Knowledge > QnA Maker > Trial > Sign In > @Microsoft.com account

* Create a knowledge base
* Use existing `BrexitFAQ` azure service

* Add BBC data source https://www.bbc.co.uk/news/uk-politics-32810887
* Add professional chit-chat
* `Create your KB`
* Look at BBC page whilst it is creating
* Show all QNA pairs



### Test

* `What is Brexit` (what does Brexit mean)

* `What is chequers` (What is the Chequers Plan?)

* `What is the issue with Northern Ireland` (Why is Northern Ireland such a sticking point?)

* `How will brexit affect the NHS` (What impact will leaving the EU have on the NHS?)

* Ask the audience for questions on Brexit



### Settings & Publish

* Explore the settings

* Publish

* Show endpoint

* Use the CURL example in the command line to show the JSON that is returned

## JFK Cognitive Search

_Use the JFK demo as a way to showcase Cognitive Search_

Talking Points

* In 2017, thousands of files, photographs and documents were released in relation to the JFK assassination in 1963
* Microsoft created a Cognitive Search application to help people explore the documents

Demo

* Go to https://jfk-demo.azurewebsites.net
* Search for “Oswald”
* Note OCR
* Note Face
  * Uses the [Cognitive Services Vision API](https://azure.microsoft.com/en-us/services/cognitive-services/computer-vision/) to extract text information from the image via OCR, handwriting, and image captioning,
  * Applies Named Entity Recognition to extract named entities from the documents,
* Look at Graph

## Cognitive Labs

http://aka.ms/CognitiveServices > Scroll Down > Cognitive Services Directory > Explore Cognitive Services Labs

* Quick description of each one

* Personality Chat > Demo

* Custom Decision

* Anomaly Finder

## Sports Shop Recommendations

_Use the Cortana Intelligence Recommendations Solution to show recommendations with a sample web site_

Talking Points

- Item to item recommendations. i.e., what amazon calls "Customers who liked this product also liked these other products"
- Personalized user recommendations

Demo

* Go to <http://aka.ms/SportsShop>

* Open the links to the Recommendations solution from the footer
* Go back to <http://aka.ms/SportsShop>
* Use the website to explore sports wear items and their recommendations
  * Item (Choose from featured items on home page): Outfit, You May Like, Cheaper, Accessories
  * Cart: You may like, Free shipping over $60 threshold
  * User (use 00012): Based on your history