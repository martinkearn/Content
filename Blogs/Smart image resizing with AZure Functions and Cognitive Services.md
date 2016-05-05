# Smart image resizing with Azure Functions and Cognitive Services Computer Vision
A few days ago I blogged about [Generating thumbnails with the Cognitive Services Computer Vision API in C# and JavaScript](https://blogs.msdn.microsoft.com/martinkearn/2016/05/03/using-the-microsoft-cognitive-computer-vision-api-in-c-and-javascript/). The article talked about how we can use the [Computer Vision API](https://www.microsoft.com/cognitive-services/en-us/computer-vision-api) to do _smart_ image resizing in C# and JavaScript.

Today I'm going to progress this idea with [Azure Functions](https://azure.microsoft.com/en-us/services/functions/). We'll explore how to automatically create thumbnail images using the Computer Vision API whenever a new image is uploaded to an Azure Storage account. If you have not already seen my [other article on the Computer Vision API](https://blogs.msdn.microsoft.com/martinkearn/2016/05/03/using-the-microsoft-cognitive-computer-vision-api-in-c-and-javascript/), I suggest you take a look at that first.

Using Azure Storage for static web content storage makes sense in lots of ways; it is very cheap, very fast and very easy to work with using the Azure Cloud Explorer tool in Visual Studio. However in this context, Azure Storage gives us an extra benefit of built-in hooks into Azure Functions.

Azure Functions are one of the newest features in the Azure world and they are simply a way to execute C# or JavaScript (Node.js) code in the cloud. They can be triggered on a timer schedule or in response to a web hook, http request, push notification or one of many other types of 'bindings'. If you have ever used Azure Web Jobs, Functions is like web jobs plus.

The third piece of technology in the mix here is the Microsoft Cognitive Services Computer Vision API. Computer Vision does lots of amazing things with digital images, but one of the more mundane features is the ability to generate thumbnails. I discussed this in more detail [here](https://blogs.msdn.microsoft.com/martinkearn/2016/05/03/using-the-microsoft-cognitive-computer-vision-api-in-c-and-javascript/).

## Get Setup
We'll use these three technologies to build a smart thumbnail generation solution. There are a few things you'll need setup if you want to follow along, they are:
* An Azure Subscription. [Get a trial here](https://azure.microsoft.com/en-gb/pricing/free-trial/)
* A key for the Cognitive Service Computer Vision API. See [this article](https://blogs.msdn.microsoft.com/martinkearn/2016/05/03/using-the-microsoft-cognitive-computer-vision-api-in-c-and-javascript/) for details on how to get that
* Any version of [Visual Studio 2015](https://www.visualstudio.com/) (The Community Edition is free by the way)
* [Azure SDK for Visual Studio 2015](https://azure.microsoft.com/en-gb/downloads/)
* Some images to play with

## Create an Azure Function App
Functions are very easy to get started with. You simply go to the [Azure Portal](http://portal.azure.com/) and go to New > Web + Mobile > Function App.

You'll be asked about resource groups, app service plans etc. You'll also be asked to create or select a Storage Account, this is important because your Function App needs somewhere to put log information, but having a storage account is especially useful for our solution because we need somewhere to upload images and thumbnails.

When you are finished you'll be presented with a rather bewildering screen (if you cannot find it, just look in your 'web apps' section). Function Apps are a collection of Functions. Functions are built from templates that represent that various triggers and binding that functions support. The home screen offers you a wizard-style experience based 'pre-made' functions, however I find it easier to look at the full list of the types of Function you can create. You can see this by clicking `New Function`.

You'll see a list of the following templates in both C# and Node (JavaScript):
* BlobTrigger
* Empty
* EventHubTrigger
* GenericWebHook
* GitHubWebHook
* HttpTrigger
* QueueTrigger
* ServiceBusTrigger
* TimerTrigger (_TIP: You'll have to scroll down to see this one - easily missed_)

You can also see a bunch of 'experimental' templates in other languages such as F#, PHP, Phython, Powershell and Bash as well as specific scenarios like Image Resize (which is where I stole the idea for this example from).

To follow along with the smart image re-size scenario in this article, choose `BlobTrigger - C#` and use these settings:
* *Name*: Leave as default
* *Path*: `originals/{name}`. This corresponds to a Blob container called 'originals' (we'll create this later) and the `{name}` parameter is used to identify the file name of the incoming blob
* *Storage Account Connection*: Use the `+ new` button to choose the storage account you create at the start.

When the function is created, avoid the temptation to start coding. We need to do some configuration in our Storage account first.

## Create your Blob Containers

## Implement the CSP (C\# script)

## Resources
* My article on [Generating thumbnails with the Cognitive Services Computer Vision API in C# and JavaScript](https://blogs.msdn.microsoft.com/martinkearn/2016/05/03/using-the-microsoft-cognitive-computer-vision-api-in-c-and-javascript/)
* [Azure Functions](https://azure.microsoft.com/en-us/services/functions/)