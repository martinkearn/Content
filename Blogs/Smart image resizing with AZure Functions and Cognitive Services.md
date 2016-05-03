# Smart image resizing with Azure Functions and Cognitive Services Computer Vision
Resizing images to generate thumbnails is one of perennial workflow challenges of the web. 

There are many tools and techniques that already do this, but in this article, I'm going to bring together a set of rather cool Microsoft technology to do image resizing in a rather smart, interesting and hands-free way.

We'll cover a few technologies here, they are:
* Microsoft Azure Storage
* Microsoft Azure Functions
* Microsoft Cognitive Services, Computer Vision API

Using Azure Storage for static web content storage makes sense in lots of ways; it is very cheap, very fast and very easy to work with using the Azure Cloud Explorer tool in Visual Studio. However in this context, Azure Storage gives us an extra benefit of built-in hooks into Azure Functions.

Azure Functions are one of the newest features in the Azure world and they are simply a way to execute C# or JavaScript (Node.js) code in the cloud. They can be triggered on a timer schedule or in response to a web hook, http request, push notification or one of many other types of 'bindings'. If you have ever used Azure Web Jobs, Functions is the updated version of them.

The third piece of technology in the mix here is the Microsoft Cognitive Services Computer Vision API. Computer Vision does lots of amazing things with digital images, but one of the more mundane features is the ability to generate thumbnails. However, the clever bit is that not only can the API resize an image for you (to any height and width dimensions) but it has a flag called 'Smart Crop' which identifies the region of interest in the image and automatically centers the crop around that so you don;t end up accidentally cutting people faces off!

We'll use these three technologies to build a smart thumbnail generation solution. There are a few things you'll need setup if you want to follow along, they are:
* An Azure Subscription. [Get a trial here](https://azure.microsoft.com/en-gb/pricing/free-trial/)
* Any version of [Visual Studio 2015](https://www.visualstudio.com/) (The Community Edition is free by the way)
* [Azure SDK for Visual Studio 2015](https://azure.microsoft.com/en-gb/downloads/)
* Some images to play with
