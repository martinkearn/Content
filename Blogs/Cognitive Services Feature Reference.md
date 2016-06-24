Microsoft Cognitive Services is a set of 21 machine learning backed APIs which help you bring all the smarts of machine learning to your app with a simple API call.

There are 21 APIs but each API has several functions, some of which are fairly distinct.

The purpose of this article is to give a reference of all the features available to you and which API they belong to.

Much of the text in this article has been extracted from <https://www.microsoft.com/cognitive-services>

## Computer Vision API
<https://www.microsoft.com/cognitive-services/en-us/computer-vision-api>

Extract rich information from images to categorize and process visual data—and protect your users from unwanted content. 

* **Analyze an image** This feature returns information about visual content found in an image. Use tagging, descriptions and domain-specific models to identify content and label it with confidence. Apply the adult/racy settings to enable automated restriction of adult content. Identify image types and color schemes in pictures.
* **Recognize celebrities** The Celebrity Model is an example of Domain Specific Models. Our new celebrity recognition model recognizes 200K celebrities from business, politics, sports and entertainment around the World. Domain-specific models is a continuously evolving feature within Computer Vision API.
* **Read text in images (OCR)** Optical Character Recognition (OCR) detects text in an image and extracts the recognized words into a machine-readable character stream. Analyze images to detect embedded text, generate character streams and enable searching. Allow users to take photos of text instead of copying to save time and effort.
* **Generate a thumbnail** Generate a high quality storage-efficient thumbnail based on any input image. Use thumbnail generation to modify images to best suit your needs for size, shape and style. Apply smart cropping to generate thumbnails that differ from the aspect ratio of your original image, yet preserve the region of interest. 

## Emotion API
<https://www.microsoft.com/cognitive-services/en-us/computer-vision-api>

Analyze faces to detect a range of feelings and personalize your app's responses. 

* **Recognize Emotions in Images** The Emotion API takes an facial expression in an image as an input, and returns the confidence across a set of emotions for each face in the image, as well as bounding box for the face, using the Face API. If a user has already called the Face API, they can submit the face rectangle as an optional input. The emotions detected are anger, contempt, disgust, fear, happiness, neutral, sadness, and surprise. These emotions are understood to be cross-culturally and universally communicated with particular facial expressions.
* **Recognize Emotions in Video** The Emotion API for Video recognizes the facial expressions of people in a video, and returns an aggregate summary of their emotions. You can use this API to track how a person or a crowd responds to your content over time.
The emotions detected are anger, contempt, disgust, fear, happiness, neutral, sadness, and surprise.

## Face API
<https://www.microsoft.com/cognitive-services/en-us/computer-vision-api>

Detect human faces and compare similar ones, organize people into groups according to visual similarity, and identify previously tagged people in images. 

* **Face Detection** Detect one or more human faces in an image and get back face rectangles for where in the image the faces are, along with face attributes which contain machine learning-based predictions of facial features. After detecting faces, you can take the face rectangle and pass it to the Emotion API to speed up processing. The face attribute features available are: Age, Gender, Pose, Smile, and Facial Hair along with 27 landmarks for each face in the image.
* **Face Verification** Check the likelihood that two faces belong to the same person. The API will return a confidence score about how likely it is that the two faces belong to one person. 
* **Face Identification** Search and identify faces. Tag people and groups with user-provided data and then search those for a match with previously unseen faces.
* **Similar Face Searching** Easily find similar-looking faces. Given a collection of faces and a new face as a query, this API will return a collection of similar faces.
* **Face Grouping** Organize many unidentified faces together into groups, based on their visual similarity.

## Video API
<https://www.microsoft.com/cognitive-services/en-us/computer-vision-api>

Intelligent video processing produces stable video output, detects motion, creates intelligent thumbnails, and detects and tracks faces.

* **Stabilization** Smooth and stabilize shaky video. Given an input video, the service will generate a stabilized version of the video.
* **Face detection and tracking** Detect and track faces in videos. This service will analyze the video inputted and output metadata for faces and location of the faces within the video frames.
* **Motion detection** Detect when motion has occurred in videos with stationary backgrounds. This service will analyze the video inputted and output metadata for which frames motion is detected.
* **Video thumbnail** Provides an automatic motion thumbnail summary for videos to let people see a preview or snapshot quickly. Selection of scenes from a video create a preview in form of a short video.