# Cognitive Services Language APIs
A deep dive demo of all the APIs and mst of the function in the Cognitive Services Language category

Takes about 12 minutes, including the LUIS video.

### Pre Reqs
* Windows Translator app installed
* Webcam
* An object with some non-english printed text

### Screens and apps
* Edge > Cognitive, HowHappy

## Bing Spell Check
1. Go to https://www.microsoft.com/cognitive-services/en-us/bing-spell-check-api

2. Scroll down each of the main features and epxlain them
  * Word Breaks
  * Slang
  * Names (famous names)
  * Homonyms ('four' used instead of 'for' as an exmaple)
  * Brands
  
3. Scroll up and show the API using each of the samples

## Language Understanding
1. Play video for slides

2. Go to https://www.microsoft.com/cognitive-services/en-us/language-understanding-intelligent-service-luis

3. Go to http://HowHappy.co.uk and ask these questions with audience photo
 * Who is the happiest one here
 * Show me all the angry people
 * Who is the 3rd most surprised person
 * Show me the least happy person

## Linguistic Analysis
1. Go to https://www.microsoft.com/cognitive-services/en-us/linguistic-analysis-api

2. Talk to each feature
 * Sentence separation and tokenization - break text into sentances
 * Part-of-speech tagging - find the nouns (entities, persons, places, things, etc.), verbs (actions, changes of state)
 * Constituency parsing - Determine the internal structure and meaning of a sentence (entities, purpose, etc.) by breaking it into labeled phrases
  * LUIS is based on this API
  
## Text Analytics
1. Go to https://www.microsoft.com/cognitive-services/en-us/text-analytics-api

2. Analyse the text "I think that Future Decoded is possibly the best technology conference ever, at least the AI track certainly is"
 * Senitment anlaysis - 81% sentiment
 * Tey Phrase Extraction - best technology, conference, AI track
 * Language detection - English (confidence: 100%) 
 
3. Scroll down to `Topic Detection`
 * Detects topics in a corpus of text
 * Requires a minimum of 100 text records to be submitted
 
## Translator
1. Go to https://www.microsoft.com/cognitive-services/en-us/translator-api

2. Talk through each of the main functions
  * Language Detection
  * Translation
  * Text-to-speech
  * Custom translation system
  * Collaborative Translation Framework (CTF)
  
3. Open the Windows Translator app

4. Choose French > English

5. Point the camera at the white 'Biscuits Fins De Montmarte' leaflet
 * The app translate the french text to english and overlays
 
6. Switch langauges so it is English > French

7. Use audio to translate the phrase "I promise i did not eat all of those nice french biscuits on my own"
 * Play the english and french audio files
 
## Web Language Model
Talking points:
 * Language models trained on massive amount sof web data
 * Trained on Bing en-US data
 
1. Scroll down the list an talk to each feature
 * Joint porbabilities - predicte when words are used together i.e. "natural language processing"
 * Conditional probabilities - Given a sequence of words, calculate how often a particular word tends to follow.
 * Next word completions - Given a sequence of words, get the list of words most likely to follow.
 
2. Scroll up and use the sample texts in the portal app
