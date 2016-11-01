# Cognitive Services Vision APIs
A deep dive demo fo all the APIs and mst of the function in the Cognitive Serviecs Vision category

### Pre Reqs
* A good quality, portable web cam, set as the default camera
* Clone https://github.com/Microsoft/Cognitive-Samples-VideoFrameAnalysis/, add Face, Emotion and Computer Vision keys to app.config, set the 'LiveCameraSample' as the default project and run app at least once
* Visual Studio 2015

## Computer Vision > Analyse
1. Go to https://www.microsoft.com/cognitive-services/en-us/computer-vision-api

2. Upload the photo and analyse
  * Description
  * Tags
  * Adult / racy
  * Categories
  * Colours
  
3. Repeat with a stock portal image
 
## Computer Vision > Recognise Celebs
1. Go to https://www.microsoft.com/cognitive-services/en-us/computer-vision-api and scroll to the `Recognize celebrities` section

2. Search for `Bill Gates`

3. Explore JSON

4. Choose a different Bill Gates photo

5. Repeat for `Donald Trump` (or ask for suggestions)

## Computer Vision > Analyze video in near real-time
Talking points
* Use any of the computer vision APIs with videos by splitting the frames
* We'll use a Windows UWP app to show this in action

---

1. Go to https://www.microsoft.com/cognitive-services/en-us/computer-vision-api and scroll to the `Analyze video in near real-time` section

2. Play the beach video

3. Go to https://www.microsoft.com/cognitive-services/en-US/subscriptions to show keys for Face, Emotion and Computer Vision

3. Open `VideoFrameAnalysis` in Visual Studio
