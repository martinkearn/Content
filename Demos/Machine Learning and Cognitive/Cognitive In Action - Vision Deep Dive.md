# Cognitive Services Vision APIs
A deep dive demo fo all the APIs and mst of the function in the Cognitive Serviecs Vision category

### Pre Reqs
* A good quality, portable web cam, set as the default camera
* Clone https://github.com/Microsoft/Cognitive-Samples-VideoFrameAnalysis/, set the 'LiveCameraSample' as the default project and run app at least once
* Visual Studio 2015

### Screens and apps
* Edge > Cognitive, HowHappy, Twins, HowOld
* Visual Studio > https://github.com/Microsoft/Cognitive-Samples-VideoFrameAnalysis/
* Windows Camera app

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
1. Go to https://www.microsoft.com/cognitive-services/en-us/computer-vision-api and scroll to the `Analyze video in near real-time` section

2. Play the beach video

3. We'll do more with this later

## Computer Vision > Read text in images
1. Go to https://www.microsoft.com/cognitive-services/en-us/computer-vision-api and scroll to the `Read text in images` section

2. Take a photo of the event lanyard or any printed text

3. Upload and show text and json results

## Computer Vision > Generate Thumbnails
1. Go to https://www.microsoft.com/cognitive-services/en-us/computer-vision-api and scroll to the `Generate a thumbnail` section

2. Use some of the stock portal image

3. Toggle smart cropping on and off

4. Open Skype and Use Smart Thumbnails bot to resize `SmartCropping.jpg` to 400x100 and then 100x400 with smart cropping

## Emotion > Recognize Emotions in Images
1. Go to https://www.microsoft.com/cognitive-services/en-us/emotion-api and scroll to the `Recognize Emotions in Images` section

2. Take a photo of the audience

3. Upload and show the faces

4. Repeat in http://HowHappy.co.uk

## Emotion > Recognize Emotions in Video
1. Go to https://www.microsoft.com/cognitive-services/en-us/emotion-api and scroll to the `Recognize Emotions in Video` section

2. Play the default video in the portal

## Face
1. Go to https://www.microsoft.com/cognitive-services/en-us/face-api and scroll to the `Recognize Emotions in Video` section

2. Take selfie

3. Upload and analyse

4. Show Face Verification, Face Identification, Similar Face Searching, Face Grouping in portal

5. Jake demos with https://www.twinsornot.net/ and http://how-old.net/ see [Cognitive Services in Action](https://github.com/martinkearn/Content/blob/master/Demos/Machine%20Learning%20and%20Cognitive/Cognitive%20Services%20in%20Action.md)

## Video
1. Go to https://www.microsoft.com/cognitive-services/en-us/video-api

2. Use the built-in videos to show Stabilize shaky videos, Detect and track faces, Detect motion, Generate video thumbnails

3. Scroll to the `Analyze video in near real-time` section

4. Show the default video

5. Go to https://www.microsoft.com/cognitive-services/en-US/subscriptions to show keys for Face, Emotion and Computer Vision

6. Get a volenteer to do some faces

7. Open `VideoFrameAnalysis` in Visual Studio
 * Add `e0643cb1a6404b6bbfd7b6fb20f67963` for the face key
 * Add `1dd1f4e23a5743139399788aa30a7153` for the emotion key
 * Add `382f5abd65f74494935027f65a41a4bc` for the computer vision key
 * Set to call api every 1 seconds
 * Save
 * Start camera
 * Cycle through modes (Faces = face api, Emotion = emotion api, Tags = computer vision)
