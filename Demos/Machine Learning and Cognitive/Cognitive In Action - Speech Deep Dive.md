# Cognitive Services Speech APIs
A deep dive demo of all the APIs and most of the functions in the Cognitive Services Speech category

Takes about 20 minutes.

### Pre Reqs
* A good quality, portable microphone

### Screens and apps
* Edge > Cognitive

## Bing Speech > Speech Recognition
1. Go to https://www.microsoft.com/cognitive-services/en-us/speech-api

2. Set the language to `English - GB`

3. Record the text in the paragraph above the input

4. Show again using a sample

## Bing Speech > Text to Speech
1. Go to https://www.microsoft.com/cognitive-services/en-us/speech-api and scroll to `Text to Speech`

2. Set language to `English GB` and `Susan`

3. Copy this text `Two GUID's walk into a bar .... GUID one says "a wine, a beer and a nice glass of champagne".......the barman says "That's a strange order!" ......GUID two says "Ignore him, he's random"`
  * Play again with `George`
  * Note punctuations and pauses
  * Also note that it doe snot recgnise `GUID` properly
  
## Custom Recognition
Talking points:
* Allows definition of custom words and phrases (like model numbers or domain specific terminology)
* Custom acoustic models
* Still acessible via Cognitive REST apis
* Private preview

## Speaker Recognition
1. Go to https://www.microsoft.com/cognitive-services/en-us/speech-api and scroll to `Speaker Verification`

2. Enroll in the application by repeating the phrase as requested

3. Get someone else to try the phrase
  * Notice the rejection result
  
4. I try again
  * Notice the acceptance result
  
5. Scroll down to `Speaker Identification`

6. Play various president clips from portal and watch them be identified

7. Upload a new clip of President Obama
  * Play `obama-cuba-speech.wav` in media player, Groove, whatever
  * Upload to portal and see that Obama is correctly identified
  * http://www.obamadownloads.com/obama-mp3s.html
  * http://audio.online-convert.com/convert-to-wav
