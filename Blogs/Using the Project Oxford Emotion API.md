---
title: Using the Project Oxford Emotion API
author: Martin Kearn
description: One of the more interesting Cognitive APIs is the Emotion API which can analyse the emotions shown on a photo. I was very excited about this API when I first heard about it. I know this because I uploaded a selfie to it and it told me I was excited. So I thought I'd set to work on a little sample which shows how to use the Emotion API in my two favourite languages C# and JavaScript
image: https://dummyimage.com/800x600/000/fff&text=placeholder
thumbnail: https://dummyimage.com/200x200/000/fff&text=placeholder
type: article
status: published
published: 2016/05/07 09:40:00
categories: 
  - Cognitive Services
  - Emotion API
---

# Using the Project Oxford Emotion API
Machine Learing is a hot topic at the moment and Microsoft has some great tools for making [Machine Learning accessible to developers](http://blogs.msdn.com/b/martinkearn/archive/2016/03/01/machine-learning-is-for-muggles-too.aspx), not just Data Scientists.

One of these tools is a set of REST APIs which are collectively called [Project Oxford](https://www.projectoxford.ai/) and/or [Cortana Analytics](https://www.microsoft.com/en-gb/server-cloud/cortana-analytics-suite/overview.aspx). These services takes some very clever Machine Learning algorithms which Microsoft have already applied to very broad sample data sets to provide models which are callable via REST APIs. These map to common machine Learning scenarios like [Face Recogniton](https://www.projectoxford.ai/face), [Computer Vision](https://www.projectoxford.ai/vision) and [Speech](https://www.projectoxford.ai/vision).

One of the more interesting APIs is the [Emotion API](https://www.projectoxford.ai/emotion) which can analyse the emotions shown on a photo. I was very excited about this API when I first heard about it. I know this because I uploaded a selfie to it and it told me I was excited. So I thought I'd set to work on a little sample which shows how to use the Emotion API in my two favourite languages C# and JavaScript.

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

![My daughter, Madison](https://raw.githubusercontent.com/martinkearn/Content/master/Blogs/Images/Emotion_Thumb.jpg)

As you can see from the data, the API recognised her face (it can actually support up to 64 faces in a single image) and has analysed her emotions in the picture.

Madison's primary emotion was _contempt_ for which she scored 57%, then _disgust_ which was 22% and a light smattering of _anger_ at 10%. In case you are wondering, this is what a 3-year-old with very little respect for her father thinks when she it told that she cannot have _another_ piece of chocolate!

I'll spend the rest of the article outlining how you work with the API. You can find code samples in both C# and JavaScript in my [corresponding GitHub repo](https://github.com/martinkearn/Project-Oxford-Emotion-API-sample). If anyone wants to implement the API in another language or technology (Windows UWP, IOS, Android, Office add-ins, PHP, Node, Powershell, Whatever-floats-yer-boat.js), I'm very open to pull requests! :)

## Step 1: Get your key
As with almost any API, your first task is to obtain an API key. This is really easy to do for all Project Oxford APIs.

You simply go to the API's home page (for the Emotion API it is https://www.projectoxford.ai/emotion) and click the 'Try for free' button. It will ask you to sign in with a Microsoft account (Hotmail.com, Outlook.com, Live.co.uk etc).

Once you are signed in it will ask you to 'request new keys' and subscribe to the API. Most of the Project Oxford APIs have a free pricing tier which permits a number of requests per month for free. Typically it is 10,000 but can vary.

Once subscribed, you will be able to view your key. In most scenarios you only need your 'primary key' which will look something like this `1dd1y4e23c5743139329688ba30a7153`

## Step 2: Make a request
You need to pass your API key as an `Ocp-Apim-Subscription-Key` header with your request.

You'll also need to pass the image in the body. As you'll see in the [API Reference](https://dev.projectoxford.ai/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa), the image is accepted in a `application/json` format body (for a POST request) which is a URL to the image. The JSON in this case looks like this
```
{ "url": "http://example.com/picture.jpg" }
```
The API also accepts a binary image file in `application/octet-stream` format. This is a more typical scenario where a user has uploaded an image to your application for a form or some kind of file picker.

##Step 3. Do something awesome with the JSON
Once you have a JSON response, you can now use the data in your application as you would any other JSON result set.

##Code samples
As mentioned at the top, I have implemented the Emotion API with both the URL and the File request options in both C# (via ASP.Net Core 1.0) and JavaScript with JQuery. You can see these implementations in my [Project-Oxford-Emotion-API-sample Github Repo](https://github.com/martinkearn/Project-Oxford-Emotion-API-sample).

For completeness, here are the main sections of code that you';ll need

### JavaScript with JQuery File/Octet-Stream Sample
This is a full HTML file. You should be able to copy and paste this into a HTML file and it will work
```
<html>

<head>
    <script src="http://ajax.googleapis.com/ajax/libs/jquery/1.9.0/jquery.min.js"></script>
</head>

<body>
    <p>This example shows how to post an image to the Project Oxford Emotion API using a binary file</p>
    <input type="file" id="filename" name="filename">
    <button id="btn">Click here</button>
    <p id="response"></p>

    <script type="text/javascript">
        //apiKey: Replace this with your own Project Oxford Emotion API key, please do not use my key. I inlcude it here so you can get up and running quickly but you can get your own key for free at https://www.projectoxford.ai/emotion 
        var apiKey = "1dd1f4e23a5743139399788aa30a7153";
        
        //apiUrl: The base URL for the API. Find out what this is for other APIs via the API documentation
        var apiUrl = "https://api.projectoxford.ai/emotion/v1.0/recognize";
        
        $('#btn').click(function () {
            //file: The file that will be sent to the api
            var file = document.getElementById('filename').files[0];
        
            CallAPI(file, apiUrl, apiKey);
        });
        
        function CallAPI(file, apiUrl, apiKey)
        {
            $.ajax({
                url: apiUrl,
                beforeSend: function (xhrObj) {
                    xhrObj.setRequestHeader("Content-Type", "application/octet-stream");
                    xhrObj.setRequestHeader("Ocp-Apim-Subscription-Key", apiKey);
                },
                type: "POST",
                data: file,
                processData: false
            })
                .done(function (response) {
                    ProcessResult(response);
                })
                .fail(function (error) {
                    $("#response").text(error.getAllResponseHeaders());
                });
        }
        
        function ProcessResult(response)
        {
            var data = JSON.stringify(response);
            $("#response").text(data);
        }
    </script>
</body>

</html>

```

###C# File/Octet-Stream sample (ASP.NET Core 1.0)
This is a C# example using ASP.NET Core 1.0 MVC. This is not the full code, but an extract from the main 'Home' controller. See the [GitHub Repository](https://github.com/martinkearn/Project-Oxford-Emotion-API-sample) for the full, working sample
```
public class HomeController : Controller
{
    //_apiKey: Replace this with your own Project Oxford Emotion API key, please do not use my key. I inlcude it here so you can get up and running quickly but you can get your own key for free at https://www.projectoxford.ai/emotion 
    public const string _apiKey = "1dd1f4e23a5743139399788aa30a7153";

    //_apiUrl: The base URL for the API. Find out what this is for other APIs via the API documentation
    public const string _apiUrl = "https://api.projectoxford.ai/emotion/v1.0/recognize";

    // POST: Home/FileExample
    [HttpPost]
    [ValidateAntiForgeryToken]
    public async Task<IActionResult> FileExample(IFormFile file)
    {
        using (var httpClient = new HttpClient())
        {
            //setup HttpClient
            httpClient.BaseAddress = new Uri(_apiUrl);
            httpClient.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", _apiKey);
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/octet-stream"));

            //setup data object
            HttpContent content = new StreamContent(file.OpenReadStream());
            content.Headers.ContentType = new MediaTypeWithQualityHeaderValue("application/octet-stream");

            //make request
            var response = await httpClient.PostAsync(_apiUrl, content);

            //read response and write to view
            var responseContent = await response.Content.ReadAsStringAsync();
            ViewData["Result"] = responseContent;
        }

        return View();
    }
}
``` 

## In Summary
The Project Oxford APIs are very simple to get started with and use in your applications. Hopefully the sample in this article and the accompanying [GitHub Repository](https://github.com/martinkearn/Project-Oxford-Emotion-API-sample) will help you get up and running.

I plan to do similar articles on other Project Oxford and Cortana Analytic APIs. Please use the comments if there are particular APIs you'd like to see examples of.

### Resources
* [Emotion API](https://www.projectoxford.ai/emotion)
* [Project Oxford](https://www.projectoxford.ai/)
* [Cortana Analytics](https://www.microsoft.com/en-gb/server-cloud/cortana-analytics-suite/overview.aspx)
* [Machine Learning is for muggles too - my introductory article on Machine Learning](http://blogs.msdn.com/b/martinkearn/archive/2016/03/01/machine-learning-is-for-muggles-too.aspx)
* [Project-Oxford-Emotion-API-sample GitHub Repository](https://github.com/martinkearn/Project-Oxford-Emotion-API-sample)
