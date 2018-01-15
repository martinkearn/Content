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

## Custom Vision with Makaton
http://makaton.azurewebsites.net/

## HowHappy.co.uk
_Show the HowHappy website which uses the emotion API and LUIS to sort faces in a photo by their emotion_

Show the main How Happy website in action
* Take a photo of the audience and upload a photo of the crowd
* Use the Luis prompt to ask for other emotions
    * Who is the happiest one here
    * Show me all the angry people
    * Who is the 3rd most surprised person
    * Show me the least happy person

## Recommendations
_Use the Cortana Intelligence Recommendations Solution to show recommendations with a sample web site_

Go to http://aka.ms/SportsShop

Open the links to the Recommendations solution from the footer

Talk to
* Item to item recommendations. i.e., what amazon calls "Customers who liked this product also liked these other products"
* Personalized user recommendations

Go back to http://aka.ms/SportsShop

Use the website to explore sports wear items and their recommendations
* Item (Choose from featured items on home page): Outfit, You May Like, Cheaper, Accessories
* Cart: You may like, Free shipping over $60 threshold
* User (use 00012): Based on your history

## Q&A Maker with Innocent Drinks
_Innocent have a great FAQ section on their website, lets use it to show what Q&A Maker can do_

Visit https://www.innocentdrinks.co.uk/us/contact-us/faqs#faq-content and look at some FAQs

Go to https://qnamaker.ai

Create a new service based on https://www.innocentdrinks.co.uk/us/contact-us/faqs#faq-content

Ask "How are the smoothies made"
* It will give the answer for "How and where are your smoothies made?"
* Ask "Where are the smoothies made" ...it will give the same answer
