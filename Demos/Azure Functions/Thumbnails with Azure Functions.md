# Thumbnails with Azure Functions
In this demo we'll use Azure functions and the Cognitive Computer Vision API to create smart thumbnails from Azure Storage files

### Pre reqs
* A key for the [Cognitive Services Computer Vision API](https://www.microsoft.com/cognitive-services/en-us/computer-vision-api)
* A copy of [OffCenterMartin.png](https://raw.githubusercontent.com/martinkearn/Content/master/Blogs/Images/OffCenterMartin.png)
* Azure subscription
* Visual Studio 2015 with the Azure SDK
* Delete previous instances of `SmartThumbs` Azure Function
* Postman chrome plug-in open and loaded

## Create an Azure Function
http://portal.azure.com

New > Web + Mobile > Function App
* App name = `SmartThumbs`
* Resource Group = `DemoDump` (should already exist)
* Storage Account = Create new, `smartthumbs`
* Pin and create

Take about 3 minutes to complete, wait for the portal notification
* Portal will redirect to Function app settings when done

## Hello World function (http trigger)
New Function > `HttpTrigger C#`
* Accept default name

Review code

Do request in PostMan
* GET to default URL - show `400/Bad Request` message
* Add `&name=world` either via Params or the query string
* Show logs in azure portal

Create the same thing in Node.js (HttpTrigger Node)
* Review code
* Make request in Postman

## Add containers to Storage Account
Open Visual Studio

Load Cloud Explorer

Locate the `smartthumbs` Storage Account
* In the `DemoDump` resource group

Create two blob containers 
* `originals`
* `thumbs`

## Basic Blog Trigger
Back to Azure Portal

New Function > `BlobTrigger C#`
* Path = `originals/{name}`
* Storage account connection = `smartthumbs`

Put the Azure Portal and Visual Studio side-by-side

Upload an image to the 'originals' container in visual studio

Show the log and the binary data

## Implement 'CopyTo' the thumbs blob
Add an output for the 'thumbs' blob container viathe `Integrate` tab
* Name = `outputBlob`
* Path = `thumbs/{name}`
* Storage account connection = `smartthumbs`

Clear the log

Update the code as follows

```
using System;

public static void Run(Stream myBlob, Stream outputBlob, TraceWriter log)
{
    myBlob.CopyTo(outputBlob);
}
```

Wait for "Compilation succeeded"

Upload another file to 'originals'
* Watch the log and wait for "Function completed"
* Show the blob has been copied to 'thumbs'
* Open the file

## Computer Vision blob trigger
Add and talk through the following code 

```
using System;
using System.Text;
using System.Net.Http;
using System.Net.Http.Headers;

public static void Run(Stream myBlob, Stream outputBlob, TraceWriter log)
{
    int width = 320;
    int height = 320;
    bool smartCropping = true;
    string _apiKey = "382f5abd65f74494935027f65a41a4bc";
    string _apiUrlBase = "https://api.projectoxford.ai/vision/v1.0/generateThumbnail";
    
    using (var httpClient = new HttpClient())
    {
        httpClient.BaseAddress = new Uri(_apiUrlBase);
        httpClient.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", _apiKey);
        using (HttpContent content = new StreamContent(myBlob))
        {
            //get response
            content.Headers.ContentType = new MediaTypeWithQualityHeaderValue("application/octet-stream");
            var uri = $"{_apiUrlBase}?width={width}&height={height}&smartCropping={smartCropping.ToString()}";
            var response = httpClient.PostAsync(uri, content).Result;
            var responseBytes = response.Content.ReadAsByteArrayAsync().Result;
            
            //write to output thumb
            outputBlob.Write(responseBytes, 0, responseBytes.Length);
        }
    }
}
```

Upload `OffCenterMartin.png` to "originals" and show smartly cropped blob
* Show it side-by-side with the original