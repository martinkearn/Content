I'm continuing to explore [Microsoft Cognitive Services](https://www.microsoft.com/cognitive-services). This is a range of 21 public REST APIs, backed by Machine Learning technology which you can use in your applications. They do incredibly cool things and I've blogged about them before a few times:
* [Introducing HowHappy.co.uk](https://blogs.msdn.microsoft.com/martinkearn/2016/03/22/introducing-howhappy-co-uk/)
* [Using the Cognitive Services Emotion API in C# and JavaScript](https://blogs.msdn.microsoft.com/martinkearn/2016/03/07/using-the-project-oxford-emotion-api-in-c-and-javascript/)
* [Machine Learning is for Muggles too!](https://blogs.msdn.microsoft.com/martinkearn/2016/03/01/machine-learning-is-for-muggles-too/)

The one I've been looking at most recently is the [Computer Vision API](https://www.microsoft.com/cognitive-services/en-us/computer-vision-api). Not because I'm especially interested in Computer Vision, but because my boss, [Andrew Spooner](https://twitter.com/andspo) is and if he is, I figured I should look at it too! :)

Computer Vision does lots of amazing things with digital images. It can identify objects in images (including celebrities) and it can read text in images. However, one of the more mundane but useful features is the ability to generate thumbnails. 

Thumbnail generation is definitely not a new idea, but the clever bit is that not only can the API resize an image for you (to any height and width dimensions) but it has a flag called 'Smart Crop' which identifies the 'region of interest' in the image and automatically centers the crop around that so you don't end up accidentally cutting people faces off!

In this article, I'll work through a very simple example of using this API in ASP.NET Core 1. I'm also working on a JavaScript version but I cannot get it working at the moment so I'll update this when I have it.

## How does the Computer Vision API work?
The API works by taking an image (either as a stream or a link to an online image). It also requires a width, height and optionally a boolean indicating whether you need 'smart cropping' or not. 

It will then do its magic and return to you an image smartly cropped to the dimensions you specified.  

The image will be returned as a byte array which you can then either save or post directly to you application's UI.

To get started with the API, have a look at the 