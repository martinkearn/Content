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

* •Talk around you could use this in conjunction with smart cropping and OCR to create a registration extraction flow

  ​    epared sents��s

* 

Demo

* http://aka.ms/CognitiveServices > Vision > Custom Vision > Getting Started > Sign In (@Microsoft.com)
* Vehicle Registration model
* Explore images and tags
* Upload `Registration-Untagged-1.jpg` and tag registration
* Train > Observe results
* Test with `Registration-test-1.jpg`, `Registration-test-2.jpg`

### BananaLens

_Make sure the topic has been introduced via slides first_

- http://aka.ms/CognitiveServices > Vision > Custom Vision > Getting Started > Sign In (@Microsoft.com)
- Bananas-Colour model
- Explore images and tags
- Test `Banana Colour 2.jpg`, `Banana Colour 4.jpg`
- Bananas-Defect model
- Test`Banana Crown Rot.jpg`

