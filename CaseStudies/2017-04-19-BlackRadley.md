---
layout: post
title:  "Black Radley: Intelligent Exhibits"
author: ["Martin Kearn", "Martin Beeby"]
author-link: ["http://martink.me"]
date:   2017-04-19
categories: [Cognitive Services, Windows, Azure]
color: "blue"
excerpt: "Black Radley make museum exhibits more compelling by tailoring audio description based on age, gender and emotional state as well as giving museum's more insight by tracking patrons as they traverse the exhibits of a museum."
language: [English]
verticals: [Public Sector]
---

# Black Radley: Intelligent Exhibits

Microsoft teamed up with Black Radley and the Shrewsbury Museum & Art Gallery to create a solution system that would react to museum patrons as they interact with exhibits based upon their approximate age, gender and emotion state. Additionally the solution provides detailed insight on how patrons traverse the museum, which exhibits they linger at and for how long.

The goal was to address problems museums face around making exhibits more compelling for patrons and better understanding how patrons use the museums. See more in the 'Problem Statement' section.

## Technologies Used

The following technologies were using in this solution:

- [Microsoft Windows 10 IoT Core](https://developer.microsoft.com/en-us/windows/iot) - herein referred to as 'Windows IoT'
- [Raspberry Pi 3](https://www.raspberrypi.org/products/raspberry-pi-3-model-b/) with a USB web cam and speaker - herein referred to as 'RP3'
- [Microsoft Universal Windows Platform (UWP)](https://docs.microsoft.com/en-us/windows/uwp/get-started/universal-application-platform-guide)  - herein referred to as 'UWP App'
- [Microsoft Cognitive Services Face API](https://www.microsoft.com/cognitive-services/en-us/face-api) - herein referred to as 'Face API'
- [Microsoft ASP.NET Core Web API](https://www.asp.net/) - herein referred to as 'Web API'
- [Microsoft Azure App Service](https://azure.microsoft.com/en-us/services/app-service/) - herein referred to as 'App Service'
- [Microsoft Azure Table Storage](https://azure.microsoft.com/en-gb/services/storage/) - herein referred to as 'Table Storage'
- [Microsoft Power BI](https://powerbi.microsoft.com/en-us/) - herein referred to as 'Power BI'
- [Microsoft Excel 2016](https://products.office.com/en-gb/excel) - herein referred to as 'Excel'

## Core Team

- Martin Kearn (Microsoft, @MartinKearn)

- Martin Beeby (Microsoft, @TheBeebs)

- Paul Foster (Microsoft, @PaulFo)

- Joe Collins (Head of Systems and Analysis, Black Radley, @JoeJCollins)

- Richard Wilde (Developer, Black Radley, @Rippo) 

- Ryan O'Neil (Developer, Black Radley, @RyanONeill1970) 

## Source Code

The full solution is open sourced under the [GNU General Public License v3.0](https://choosealicense.com/licenses/gpl-3.0/) license on [GitHub](https://github.com/blackradley/dinmore).

## Partner Profile

[Black Radley](http://www.blackradley.com) are a public services consultancy organisation who work with public services to inspire and invigorate systems and people. Black Radley work closely with museums within the United Kingdom on both technical and commercial projects. 

![Black Radley]({{ site.baseurl }}/images/BlackRadley/BlackRadley.JPG)

For this project, we worked closely with [Shrewsbury Museum & Art Gallery (SMAG)](http://www.shrewsburymuseum.org.uk) who are a provincial museum in Shrewsbury, Shropshire, England. SMAG offers you the chance to discover a mix of the old, the new and the curious all within an extraordinary set of buildings. From a medieval town house to an early Victorian Music Hall, exhibits span more than 750 years of history. 

![Shrewsbury Museum & Art Gallery]({{ site.baseurl }}/images/BlackRadley/SMAG.png)

## Problem Statement

Provincial museums like SMAG are under increasing pressure to be more compelling, profitable and ultimately drive higher attendance from the general public. 

Black Radley have had a desire to make museum exhibits more intelligent for some time. Joe from Black Radley was inspired to explore Microsoft technology having attended the [Hereford Smart Devs community meetup](https://www.meetup.com/Smart-Devs-User-Group/) in February 2017 where Martin Kearn presented on [Microsoft Cognitive Services](http://www.microsoft.com/cognitive).

> "We had attempted this project using Linux and several open source tools and found that setting up our development environment proved to be a challenge. Working with Microsoft and using Visual Studio we were able to accelerate through to a working solution within a couple of days."

<cite>Joe Collins, CTO, Black Radley</cite>

Black Radley saw two primary ways to help museums achieve this goal; 1) make exhibits more compelling 2) better understand patrons

> "Public funding for museums is likely to decline in the foreseeable future.  To maintain their services museums and galleries are trying to attract more visitors and understand the visitors' experience.  This solution provides a tool to help museums enhance the visitor experience and to understand how visitors respond to exhibits."

<cite>Joe Collins, CTO, Black Radley</cite>

### The state of the art for exhibits

Most museum exhibits have a small amount of printed information accompanying them for patrons to read. This information has to be written in a plain, generic way that its accessible for all audiences, therefore it is often very factual and not particularly compelling, especially for younger audiences.

![An exhibit with a printed text description]({{ site.baseurl }}/images/BlackRadley/Skeleton.jpg)

Some exhibits may also have PIR-based, motion triggered audio descriptions/videos/other media which give some information about the exhibit as a patron approaches. Whilst these multi-media descriptions do grab attention and give information in a more compelling way than printed text, they still have to be created in a generic way that is suitable for all audiences. Additionally, the PIR systems are relatively 'dumb' in that they cannot detect when a person walks off. This causes the descriptions to continuing playing until completed, regardless of whether anyone is listening.

![An exhibit with a sensor and audio/video description]({{ site.baseurl }}/images/BlackRadley/SensorAudioVideo.jpg)

### Patron Data

Most museums capture relatively little information around visitor information and activity, such as:

- Patron Age

- Patron Gender

- Which exhibits are the most popular and attract the most interaction

Without this sort of data, it is down to the museum curators to estimate using their experience on how best to lay out the exhibits and make recommendations on which exhibits certain demographics may find more interesting.

> "Museums usually count and segment visitors so they can report to funding bodies and understand their customers.  Many museums conduct a rolling programme of visitor surveys to understand visitor experience.  The data is used to guide design changes to the museum exhibitions and to support funding applications."

<cite>Joe Collins, CTO, Black Radley</cite>

## Solution and Steps

There were two primary goals to the solution

1. Improve exhibits by making them more intelligent

2. Track and report on how patrons interact with exhibits and the museum as a whole

## Prerequisites

- Install [Visual Studio Code](https://code.visualstudio.com/)
- Obtain key for the [Microsoft Cognitive Services Face API](https://www.microsoft.com/cognitive-services/en-us/face-api)
- Obtain [Azure Subscription](https://azure.microsoft.com/en-us/free/) to use the [Microsoft Azure App Service](https://azure.microsoft.com/en-us/services/app-service/) and [Microsoft Azure Table Storage](https://azure.microsoft.com/en-gb/services/storage/)
- Setup [Microsoft Windows 10 IoT Core](https://developer.microsoft.com/en-us/windows/iot) on your Raspberry Pi 3
- Setup account for [Microsoft Power BI](https://powerbi.microsoft.com/en-us/)


## Intelligent Exhibit App & Device

The overall idea of the intelligent exhibit was that we'd place an IoT device with a web cam and speaker on each exhibit. This device would do several things

1. Detect the presence of faces gazing at / interacting with the exhibit

2. Greet the patron/patrons appropriately

3. Take a photo and obtain the rough age, gender and emotion state of each face using Face API

4. Play a suitable audio description to match the age and gender of the patron or play a generic audio description for groups. For example, younger patron may get an amusing description comparing the exhibit to modern culture, where as older people may get a more in depth, factual description

5. Detect when patrons depart and stop the audio description

The IoT device was a Raspberry Pi 3 running Windows IoT Core which runs a UWP app which does the initial face detection, takes the photo and plays the audio descriptions. The Raspberry Pi's are equipped with a web cam and speaker but no screen or other peripherals.

This is a photo of the device and hardware (the screen is for debugging only)

![Intelligent exhibit device and hardware]({{ site.baseurl }}/images/BlackRadley/IntelligentExhibitDeviceHardware.jpg)

For the purposes of the initial workshop, we used a mocked cardboard box to contain the Pi and other hardware when it is in situ with the exhibit, but going forward this would have a much better, more discreet finish.

This is the cardboard casing

![Intelligent exhibit device casing]({{ site.baseurl }}/images/BlackRadley/IntelligentExhibitCasing.jpg)

This is the cardboard casing in situ with an exhibit at SMAG.

![Intelligent exhibit device casing in situe]({{ site.baseurl }}/images/BlackRadley/IntelligentExhibitCasingInSitue.jpg)

## Patron Data Capture and Analysis

In order for museums to gain more insight into how patrons interact with exhibits and traverse the museum, the solution also tracks faces as they are sighted throughout the museum.

When the intelligent exhibit device captures the details of a face, we also store the unique face ID and add it to a list of faces for that day. We store the face data with the time, exhibit location and device details so that we are able to determine whether a face has been seen before in that day and if so at which exhibit and which time.

The solution logs each sighting so that the data can be analysed at a later date. Using this data, we can track key data about each patron, including:

- Approximate age, gender and emotional state for each patron at each sighting

- Which exhibits they visited and in which order

- How long they lingered at each exhibit

These data point are extremely valuable in terms of helping museums understand patrons and therefore offer better services to them

> "Using a device like this takes visitor data collection to a new level, providing museums with much more detailed information about visitor behaviour.  This would allow museums to identify hotpots within the museum and to spot trends as they develop."

<cite>Joe Collins, CTO, Black Radley</cite>

## Technical Delivery

The entire solution is a relatively complex solution involving multiple technologies.

This is the high level architecture which is explained in each of the sub-sections below

![Intelligent Exhibit Architecture]({{ site.baseurl }}/images/BlackRadley/Architecture.JPG)

They key steps of the solution were to create a UWP app that would run on a RP3 device running Windows IoT. This device would be integrated into exhibits and set to capture images when a person (face) is detected. The photo would be send to a proxy Web API which passes the image through the Face API to gather data on emotion, age, gender and other data points before logging the data in Table Storage for later analysise and passing the data back to the UWP app. The UWP app would then tailor the audio description about the exhibit based on the data gained from the API.

### Windows UWP and Windows 10 IoT core

> "Windows 10 IoT Core and UWP provided the fastest method of getting a working initial solution.  It offered a familiar development experience so that our developers could be productive from day one."

<cite>Joe Collins, CTO, Black Radley</cite>

The Windows UWP uses a webcam to capture faces, detect faces, post information to the Face API and display a different audio file based upon the response from the API.

The main entry point for the application is WebcamFaceDetector.xaml inside of App.xaml.

This page has several responsibilities which include.
* Initialize and manage the FaceTracker object
* Start a live webcam capture and display the video stream
* Create a Timer and capture video frames at specific intervals
* Execute face tracking on selected video frames to find human faces within the video stream
* Acquire video frames from the video capture stream and convert it to a format supported by FaceTracker
* Retrieve results from FaceTracker and visualize them
* Call the API to get demographic Information from video frames with faces in.
* Request Audio to be played by the VoicePlayer based upon demographic information.

The FaceDetector is inituialised in the `OnNavigatedTo` method. The FaceDetector class is part of the namespace `Windows.Media.FaceAnalysis` and can be used to detect faces in a SoftwareBitmap (uncompressed bitmap). 
```
if (faceTracker == null)
{
    faceTracker = await FaceTracker.CreateAsync();
    ChangeDetectionState(DetectionStates.Startup);
}
```
We used the this class as a fast, on device, way of establishing the presence of faces. Once faces are detected the proxy Web API (explained in the following section) is used to get more in-depth information. This ensures that our device responds to faces quickly whilst also reducing the amount of network chatter that would be required to process every frame using off-device APIs.

Despite initial reservations about performance, we found that we could actively detect faces from a live video stream whilst keeping the RP3 CPU at well under 50% capacity.

During Initialization, we change the DetectionState to Startup. Throughout the code there are references to the DetectionState. This is a state machine which is used to determine if the face detector should be analyzing frames or not. We required this state machine as we wanted to have a way to stop processing frames whilst we made API calls, we also use it to determine if a patron moves away from an exhibit and pausing the audio.

By calling ChangeDetectionState with DetectionStates.Startup this initialises the Webcam and starts face detection. We also change the DetectionState to WaitingForFaces. 
```
private async void ChangeDetectionState(DetectionStates newState)
{
// Rest of class and switch statement removed for brevity
        case DetectionStates.Startup:
            if (!await StartWebcamStreaming())
            {
                ChangeDetectionState(DetectionStates.Idle);
                break;
            }
            VisualizationCanvas.Children.Clear();
            ChangeDetectionState(DetectionStates.WaitingForFaces);
            break;
// Rest of class and switch statement removed for brevity
}
```
The **StartWebcamStreaming** method Creates a new **MediaCapture** class and ties the output to **CamPreview**, which is a **CaptureElement** XAML control that is visible on the XAML page. We use this for debugging but to preserve system resources you can switch it off using setting **TurnOnDisplay** to false in the resources file. Lastly, we call **RunTimer** which starts a timer that we use to process the video frames for face detection.

```
private async Task<bool> StartWebcamStreaming()
{
// Rest of class and try statement removed for brevity
    mediaCapture = new MediaCapture();
    MediaCaptureInitializationSettings settings = new MediaCaptureInitializationSettings();
    settings.StreamingCaptureMode = StreamingCaptureMode.Video;
    await mediaCapture.InitializeAsync(settings);
    mediaCapture.Failed += MediaCapture_CameraStreamFailed;
    CamPreview.Source = mediaCapture;
    await mediaCapture.StartPreviewAsync();

RunTimer();
// Rest of class and try statement removed for brevity
}
```
The timer uses **timerInterval**, which is set by values retrieved from **TimerIntervalMilliSecs**  in Resources.resw (250ms). On each tick of the timer we call **ProcessCurrentStateAsync**. This method uses the **DetectionState** to determine if faces are present and what to do in various circumstances.

```
private void RunTimer()
{
    frameProcessingTimer = ThreadPoolTimer.CreateTimer(
        new TimerElapsedHandler(ProcessCurrentStateAsync), timerInterval);
}
```
Inside **ProcessCurrentStateAsync** if the **DetectionState** is **DetectionStates.WaitingForFaces** then we process the current Video frame using ```ProcessCurrentVideoFrameAsync()```. If this API returns a face we know that a face has been detected and so we change the **DetectionState** to **DetectionStates.FaceDetectedOnDevice**.

```
case DetectionStates.WaitingForFaces:
    //LogStatusMessage("Waiting for faces", StatusSeverity.Info);
    CurrentState.ApiRequestParameters = await ProcessCurrentVideoFrameAsync();

    if (CurrentState.ApiRequestParameters != null)
    {
        ChangeDetectionState(DetectionStates.FaceDetectedOnDevice);
    }
    break;
```
Inside the **ProcessCurrentStateAsync** if the state is **FaceDetectedOnDevice** then the below logic runs. We check to see if the API was called in the last 5 seconds, this avoids the device getting stuck in a loop if the user leaves and then re-enters the frame.  
```        
case DetectionStates.FaceDetectedOnDevice:
    if (CurrentState.LastImageApiPush.AddMilliseconds(ApiIntervalMs) < DateTimeOffset.UtcNow
        && CurrentState.TimeVideoWasStopped.AddMilliseconds(NumberMillSecsBeforeWePlayAgain) < DateTimeOffset.UtcNow)
    {
        ThreadPoolTimer.CreateTimer(
            new TimerElapsedHandler(HelloAudioHandler),
            TimeSpan.FromMilliseconds(NumberMilliSecsToWaitForHello));

        CurrentState.LastImageApiPush = DateTimeOffset.UtcNow;
        CurrentState.FacesFoundByApi = await PostImageToApiAsync(CurrentState.ApiRequestParameters.Image);

        LogStatusMessage($"Sending faces to api",StatusSeverity.Info);

        ChangeDetectionState(DetectionStates.ApiResponseReceived);
    }
    break;
```

The code above waits a few seconds and then plays a hello audio file (call ```VoicePlayer.PlayIntroduction()```)and then post the image data to the Proxy API using **PostImageToApiAsync**. During this pause and the duration of the audio file we would hope that we have received a response from the Proxy API, with the demographic information we are then able to determine which audio playlist to play. The **DetectionState** is changed to **ApiResponseReceived**.

Inside **ApiResponseReceived** we run some more logic and then if we still have a face we ultimately call the play method of the **VoicePlayer** class. This method takes a **DetectionState** argument and this is used to determine the audio to play. We take age of the oldest face in the image and play the audio file that corresponds with that face.

```
internal void Play(DetectionState currentState)
{
    var avgAge = currentState.FacesFoundByApi.OrderByDescending(x => x.faceAttributes.age).First().faceAttributes.age;
    PlayListGroup playListGroup = GetPlayListGroupByDemographic(avgAge);

    PlayWav(PlayList.List
            .Where(w => w.PlayListGroup == playListGroup).ToList()
        );
}
```
To play the wav file we use the **PlayWav** Method which takes all the Wav files in the playlist group as an argument.

Inside of **PlayWav** we use a **MediaPlayer** and a **MediaPlaybackList** to play our audio. Rather than an exhibit having a single audio file for each demographic group we have multiple audio files per group. We add these to a playlist. That way if a user leaves the frame and walks away from the exhibit we can stop the audio at the end of a sentence rather than randomly during playback. 
```
internal void PlayWav(List<PlayListItem> list)
{
    mediaPlayer.PlaybackSession.PositionChanged += PositionChanged;
    playbackList.Items.Clear();
    foreach (var item in list)
    {
        var mediaSource = MediaSource.CreateFromUri(new Uri($"ms-appx:///{item.Name}"));
        playbackList.Items.Add(new MediaPlaybackItem(mediaSource));
    }
    mediaPlayer.Play();
}
```
To handle the event that the user has left the exhibit. We have a detection case called **WaitingForFacesToDisappear**. When the audio is playing, this is the state that the system should be in.

If it detects that faces disappear than it will call ```VoicePlayer.Stop()``` This method sets a variable called **StopOnNextTrack**.
```
internal void Stop() {
    StopOnNextTrack = true;
    IsCurrentlyPlaying = false;
}
```
When the playlist changes the item, it is playing it calls the **CurrentItemChanged** event. This checks the **StopOnNextTrack** event. If it finds it then it will stop the audio.
```
private void PlaybackList_CurrentItemChanged(MediaPlaybackList sender, CurrentMediaPlaybackItemChangedEventArgs args)
{           
    if (StopOnNextTrack)
    {
        mediaPlayer.Pause();
        IsCurrentlyPlaying = false;
        StopOnNextTrack = false;
    }
}
```
The full code for the UWP app can be seen on [GitHub]( https://github.com/blackradley/dinmore/tree/master/Client/Dinmore.Uwp).

There are naturally some concerns over privacy, however the device makes no record of the images of visitors.  No images are stored so it is not possible to review the records of who visited the exhibit.  It is only possible to determine the estimated age and gender of visitors.  


### Proxy API

Because we potentially needed to use several Cognitive Services APIs, we took the decision to create a custom proxy API which the Intelligent Exhibit app would interface via a single call. This allows the logic of the UWP app to be simple and light weight.

This API was created using ASP.NET Core 1.1 Web API in C# and is contained within the ['server' solution](https://github.com/blackradley/dinmore/tree/master/Server) 

There is a single controllers called `PatronsController` with a single public method called `Post`. This responds to `HttpPost` requests to `<server>/api/patrons`. It accepts three parameters

- **`string device`**: A label for the device that is calling the API. Required to facilitate patron tracking

- **`string exhibit`**: A label for the exhibit where the device is located. Required on the basis that a single exhibit may have multiple devices

- **`bool returnFaceLandmarks`**: Whether to return face landmarks from the Face API. Default to `false`.

- **`string returnFaceAttributes`**: A comma-delimited string of which face attributes should be returned. Defaults to `age,gender,headPose,smile,facialHair,glasses,emotion`. Note that emotion can be included here, this removing the need to use the Cognitive Services Emotion API.

The API also accepts a body containing a byte array which represents an image containing faces. In order for this to work, the client must make the request with the `Content-Type:application/octet-stream` header.

The API uses a repository pattern with [ASP.net Core Dependency Injection](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/dependency-injection) to interact with the Cognitive Services and Azure APIs. The use of patterns was chosen to separate the application logic from the logic required to interact with the third party APIs.

The `Post` function broadly follows this logical flow

1. Cast the request body to a byte array representing the image passed from the client

2. Get or Create a Face List for today (see 'Face Lists' section)

3. Get the faces in the images using the Cognitive Services Face - Detect API (see 'Face Detect API')

4. Enumerate the faces and do the following:

5. Get the best matching face in todays face list (see 'Similar Face Matching API') if there is one, otherwise add this face to the face list as a new face

6. Create a `Patron` object to pass back to the client containing the face data and other data points required by the client

7. Store the `Patron` object in Azure Table Storage (see 'Azure Table Storage')

8. Return the `Patron` to the client

The interactions with the Face API are very simple and there are plenty of examples on-line for how to do this. You can see the full `FaceApiRepository` on [GitHub](https://github.com/blackradley/dinmore/blob/master/Server/dinmore.api/Repositories/FaceApiRepository.cs)

You will also see more detail on the Face API in subsequent sections of this document.

### .NET Core User Secrets and App Settings

In order to avoid putting API keys into the public GitHub repository, we used a tool called User Secrets to store local copies of API keys which can be exposed via the `IOptions<AppSettings>` interface in the code and 'app settings' on the Azure App service.

To use User Secrets the [Microsoft.Extensions.Configuration.UserSecrets NuGet package](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets/1.1.1/) is required.

Once installed, you can store API keys locally on your machine by using this command:

```dotnet user-secrets set AppSettings:EmotionApiKey wa068ba186e145bw9a57cue59bd53799```

`EmotionApiKey` is the name of the app setting as referred to in your code and `wa068ba186e145bw9a57cue59bd53799` is the value (this is a made-up key for the sake of this write-up).

Once stored, the keys can be access in code using the [.net core configuration API](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/configuration) with options and configuration objects. The code looks like this `_appSettings.EmotionApiKey`.

When the application is published to an Azure App Service, the key can be stored in an Application Setting for the App Services which is accessible via the Azure Portal. [This Azure Friday video](https://azure.microsoft.com/en-gb/resources/videos/configuration-and-app-settings-of-azure-web-sites/) has some detail on how to configure this.

One tip which caused problems for us in the solution is that the key name in Azure must also include the `AppSettings` prefix. It should look like this:

![Azure App Settings]({{ site.baseurl }}/images/BlackRadley/AzureAppSettings.jpg)

### Face Detect

The proxy API initially uses the Face API to detect faces within the image passed to it by the client device.

You can read all about this function of the Face API [here](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)

As with the entire solution, a repository pattern has been used to separate concerns between the proxy API and the services it calls. There is a `FaceApiRepository` class which contains a function called `DetectFaces` which performs the task of sending the image to the Face API and parsing results.

This operation is relatively straight forward and follows the following logical flow:

1. The image is sent to the Cognitive API as a POST request to this endpoint `https://westeurope.api.cognitive.microsoft.com/face/v1.0/detect`  
3. The image is sent as binary file in the body of the request
3. The following headers are added to the request

`Ocp-Apim-Subscription-Key:<The API key for the Face API>`

`Content-Type:application/octet-stream`

4. The Face API accepts several request parameters which allow additional data to be returned. By default the proxy API request the Face ID and all attributes

6. The response is parsed as JSON and returned to the API controller

The full code for the `DetectFaces` function can be seen on [GitHub](https://github.com/blackradley/dinmore/blob/master/Server/dinmore.api/Repositories/FaceApiRepository.cs)

### Face Lists

In order to meet the objective of recognising whether a patrons has been seen before, the solution makes use of the Face API Lists feature. A Face List is a list of up to 1000 faces which have been detected using the [Face API Detect function](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) and permanently stored.

Face lists are a pre-requisite to the [Face API Find Similar function](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237).

A design decision was made to create a new face list for each day using the `ddMMyyy` naming schema as it is unlikely that there will be more than 1000 patrons going through a museum in a given day (at least for the museums this solution is targeted towards). On this basis, the first task that is performed is to get or create the face list for the current day.

As with the entire solution, a repository pattern has been used to separate concerns between the proxy API and the services it calls. There is a `FaceApiRepository` class which contains a function called `GetCurrentFaceListId` which checks if there is a face list for today and if not creates it, then returns the ID. This makes use of [Face API Create a Face List function](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524b). The code attempts to create a face list using the date ID detailed above. If one already exists, a `409` status code is return which the code ignores.

The full code for the `GetCurrentFaceListId` function can be seen on [GitHub](https://github.com/blackradley/dinmore/blob/master/Server/dinmore.api/Repositories/FaceApiRepository.cs).

### Similar Face Matching

A core part of the server-side solution is to check whether a face has been seen before. This is critical to the goal of tracking patrons as they traverse the museum.

To do this, the solution performs two operations at the controller level:

1. Find similar faces
2. Add detected faces to today's face list if there are no matches

Once the controller has detected faces, it will enumerate each one and call the `FindSimilarFaces` function of the `FaceApiRepository` class which returns a list of faces that match the detected face from the current face list. The controller will use the best matching face for the next steps of the solution (Table Storage). 

If there are no matches, the controller considers the detected face to be a new face within the context of today's face list and will add the detected face to the face list.

As with the entire solution, a repository pattern has been used to separate concerns between the proxy API and the services it calls. There is a `FaceApiRepository` class which contains a two functions related to this feature which as `FindSimilarFaces` and `AddFaceToFaceList`. The full code for both functions can be seen on [GitHub](https://github.com/blackradley/dinmore/blob/master/Server/dinmore.api/Repositories/FaceApiRepository.cs)

The face repository makes use of [Face API Find Similar function](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237) and [Face API Add a Face to a Face List function](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250).

### Azure Table Storage

Once the controller has done all the work in detecting faces and storing them in the face list, it creates a `Patron` object which is what gets returned to the client.

The `Patron` is also stored so that it can be queried later on, this fulfilling the objective of tracking patrons as they traverse the museum.

There were several options around the best storage mechanism, including:
* [Azure SQL Database](https://azure.microsoft.com/en-us/services/sql-database/?v=16.50)
* [Azure Table Storage](https://azure.microsoft.com/en-us/services/storage/tables/)
* [Azure Blob Storage](https://azure.microsoft.com/en-us/services/storage/blobs/) (store the `Patron` as a JSON document)
* [Cosmos DB](https://azure.microsoft.com/en-us/services/cosmos-db/) (store the `Patron` as a JSON document)

One of the key criteria for the solution was that museum staff can easily analyse data in Excel 2016 without any specific technical knowledge or custom code. This requirement means that only SQL Database and Table Storage were valid options since these can both be easily imported into Excel as a Data Connection. Table Storage was ultimately chosen because of the following reasons:

* Schema can be easily changed
* Cost effective compared to Azure SQL Databases
* Azure Table Storage offer better performance at scale compared to SQL Databases

In order to facilitate interaction between the proxy API controller and Table Storage, a repository was created called `StoreRepository`. The repository has a single function called `StoreTable` which casts a `List<Patron>` to a model called `PatronSighting` which is simply inserted into Table Storage. The repository has a dependency on the [Windows Azure Storage Nuget Package](https://www.nuget.org/packages/WindowsAzure.Storage) which makes it very simple to interact with Azure Storage from C# code.

You can see the full code for the `StoreRepository` on [GitHub](https://github.com/blackradley/dinmore/blob/master/Server/dinmore.api/Repositories/StoreRepository.cs)

As well as using Excel to query the data, we found that [Azure Storage Explorer](http://storageexplorer.com/) was a superb tool for exploring the storage repository and seeing data coming in from the API.

### Excel 2016 for data analysis

The ability to easily analyse the data stored in Table Storage was a key requirement.

It is very simple to connect Excel to Table Storage in order to sort, filter and analyse the data in Excel. In Excel 2016, you simply follow these steps

1. Data tab
2. New Query > From Azure > From Microsoft Azure Table Storage
3. Enter your Azure Storage Account Name 
4. Enter your Azure Storage Account Key
5. Select the data source that you want to view

This screen shot shows some of the test data stored in Table Storage being viewed in Excel.

![Azure Table data in Excel 2016]({{ site.baseurl }}/images/BlackRadley/Excel.jpg)

### Power BI for data analysis

Given the volume of data being collected, we needed something more visual to explain the potential business impact of the data. We discovered that very quickly and without data conversion from raw table storage, we could use the data stored in Azure Table Storage to produce a report using PowerBI Desktop.

To create the report, we first Imported the data using **Get Data**. After selecting, **Azure** we then selected **Azure Table Storage**. Once the data source was configured, it was then just a case of dragging visualizations on to the report canvas to visualize the data. 

![Azure Table data in PowerBI]({{ site.baseurl }}/images/BlackRadley/powerbi.png)

We were able to then share the dashboard by selecting **Publish**. The report example can be found [here](https://msit.powerbi.com/view?r=eyJrIjoiNTkxMzlmMmQtZmQ1Yy00MzBiLTkyM2YtYWE0ZDdmYjIxMGIxIiwidCI6IjcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI0NyIsImMiOjV9)

Whilst this example is very basic, it shows how very quickly we can convert data stored in a very cheap storage solution into an effective report that stakeholders can use to act upon.

## Conclusion & Next Steps

The challenge was to make museum exhibits more compelling for patrons and also to prodive museums with more data about how patrons use the museums.

The solution achieved the two primary goals of providing a more intelligent museum exhibit and tracking patrons data which allows museums to better understand how patrons interact with and traverse their museum.

The solution has involved a wide range of Microsoft technologies but pivotted around three main area:

* Windows UWP to capture patrons via an IoT device
* Cognitive Services to provide intelligence on patrons as they interact with exhibits
* Azure service for storage, hosting and analysis

The solution is entirely open source at https://github.com/blackradley/dinmore.

The next steps at a high level are to to further develop the solution to make it more robust and flexible (see details below) and take it to other museums with the view to gaining sponsorship to making it a production system. 

There are several next steps for the solution.

### Comparison Information
Before audio narration is added to exhibits it would be wise to determine how visitors interact with exhibits without narration.  To this end the initial solution will be deployed in a museum for a longer period of time to gather information without the narration running.  

The information gathered can then be used to provide a summary describing the types of visitors who viewed the painting and their dwell times.  This information can then be used to compare the effect of having the narration running. 

### Curator’s or Non-technical Interface
The device used off the shelf components and Open Source software. However, it does require some technical expertise to configure the device and to update the narration. If the devices are to see wider use it will need an interface that allows curators and other subject matter experts to update the sound files of the narrative.

### Multiple Devices
For speed and convenience the initial solution was constructed as a single stand-alone device.  However the low cost of the device would make it possible to have multiple devices, which either interact or track visitors through a museum or gallery.  In order for this to happen the software will need to be adjusted to use different narration for different devices.  Currently the design assumes that there is only one narration.

### Information Access and Sharing 
Some attempt was made during the project to gather information about how visitors were responding to the painting.  To be useful to museum and gallery curators, this information should be presented in a form that suits the museum or gallery professionals needs.  The museum and galleries community has a long history of sharing benchmarking and performance information so such an interface could reasonably include comparison information so that the performance of exhibits can be compared across different museums and galleries. 

> "The solution works, for one exhibit, in one museum.  It now needs to be generalised to it can be used with other exhibits and in other museums."

<cite>Joe Collins, CTO, Black Radley</cite>

### Further Resources and Documentation
- Product Overview: [Microsoft Windows 10 IoT Core](https://developer.microsoft.com/en-us/windows/iot)
- Product Overview: [Raspberry Pi 3](https://www.raspberrypi.org/products/raspberry-pi-3-model-b/) with a USB web cam and speaker
- Product Overview: [Microsoft Universal Windows Platform (UWP)](https://docs.microsoft.com/en-us/windows/uwp/get-started/universal-application-platform-guide)
- Product Overview: [Microsoft Cognitive Services Face API](https://www.microsoft.com/cognitive-services/en-us/face-api)
- Product Overview: [Microsoft ASP.NET Core Web API](https://www.asp.net/)
- Product Overview: [Microsoft Azure App Service](https://azure.microsoft.com/en-us/services/app-service/)
- Product Overview: [Microsoft Azure Table Storage](https://azure.microsoft.com/en-gb/services/storage/)
- Product Overview: [Microsoft Power BI](https://powerbi.microsoft.com/en-us/)
- Product Overview: [Microsoft Excel 2016](https://products.office.com/en-gb/excel)
- GitHub Repository: https://github.com/blackradley/dinmore

