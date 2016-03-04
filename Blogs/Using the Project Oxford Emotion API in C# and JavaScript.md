
# Using the Project Oxford Emotion API in C# and JavaScript
Machine Learing is a hot topic at the moment. 

Whats cool about this is that Microsoft has some great tools for making [Machine Learning for muggles too](http://blogs.msdn.com/b/martinkearn/archive/2016/03/01/machine-learning-is-for-muggles-too.aspx), not just Data Scientists.

One of these tools is a set of REST APIs which are collectively called [Project Oxford](https://www.projectoxford.ai/). Project Oxford takes some very clever machine learning algorithms which Microsoft have already applied to very broad sample data sets to provide models (callable via REST APIs) for common scenarios like [Face Recogniton](https://www.projectoxford.ai/face), [Computer Vision](https://www.projectoxford.ai/vision) and [Speech](https://www.projectoxford.ai/vision).

One of the more interesting APIs is the Emotion API which can analyse the emotions shown on a photo. I was very excited about this API when I first heard about it (I know this because I uploaded a selfie to it and it told me I was excited) so I thought I'd set to work on a little sample which shows how to use it in my two favourite languages C# and JavaScript.

## How does the Emotion API work?
The emotion API works by taking an image which contains a face as an input and returns JSON result set which looks something like this:
```
{
    "faceRectangle": 
    {
        "left": 1235,
        "top": 525,
        "width": 677,
        "height": 677
    },
    "scores": 
    {
        "anger": 0.1022928,
        "contempt": 0.579521,
        "disgust": 0.2233283,
        "fear": 0.000002794455,
        "happiness": 0.00103985844,
        "neutral": 0.09139749,
        "sadness": 0.00239139958,
        "surprise": 0.0000263756556
    }
}
```

The JSON above is the result set for the image below which is my 3-year-old daughter, Madison.

![My daughter, Madison](https://raw.githubusercontent.com/martinkearn/Content/master/Blogs/Images/MLProcess.PNG)

As you can see from the data, the API recognised her face (it can actually support up to 64 faces in a single image) and has analysed her emotions in the picture.

Madison's primary emotion was Contempt for which she scored 57%, then Disgust which was 22% and a light smattering of Anger at 10%. In case you are wondering, this is what a 3-year-old with very little respect for her father think when her father tells her she cannot have _another_ piece of chocolate.

As you'll see in the [API Reference](https://dev.projectoxford.ai/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa) for the API