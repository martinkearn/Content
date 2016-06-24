
# Face API with UWP
This demo shows a very quick, simple Windows 10 UWP app using the Face api.

### Pre Reqs
* Snippets (if using them)

## New Blank App
Visual Studio > File > New > Project > Visual C# > Windows > Blank App (Universal Windows)
* Accept default Target / Minimum versions

## XAML
Use `facexaml` snippet or manually add inside the `grid`

```
<StackPanel VerticalAlignment="Top" Margin="10,50,10,10">
    <Button x:Name="button" Content="Button" HorizontalAlignment="Left" VerticalAlignment="Top" Click="button_Click"/>
    <TextBlock x:Name="textBlock" HorizontalAlignment="Left" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top"/>
</StackPanel>
```

Show the controls in the designer

## Code behind
Double click the button in the XAML designer to add the on click event and open mainpage.xaml.cs

Set the `button_click` function as async: ```private async void button_Click(object sender, RoutedEventArgs e)```

Add and discuss the following resolving using statement as you go (Snippets `face1` through `face6`)

### Snippet `face1`
```
// Capture image from camera
CameraCaptureUI captureUI = new CameraCaptureUI();
captureUI.PhotoSettings.Format = CameraCaptureUIPhotoFormat.Jpeg;
captureUI.PhotoSettings.CroppedSizeInPixels = new Size(500, 500);
StorageFile photo = await captureUI.CaptureFileAsync(CameraCaptureUIMode.Photo);
```

### Snippet `face2`
```
// Setup http content using stream of captured photo
IRandomAccessStream stream = await photo.OpenAsync(FileAccessMode.Read);
HttpStreamContent streamContent = new HttpStreamContent(stream);
streamContent.Headers.ContentType = new Windows.Web.Http.Headers.HttpMediaTypeHeaderValue("application/octet-stream");
```

### Snippet `face3`
```
// Setup http request using content
string apiQuery = "https://api.projectoxford.ai/face/v1.0/detect"
    + "?returnFaceId=true"
    + "&returnFaceLandmarks=true"
    + "&returnFaceAttributes=age,gender,headpose,smile,facialHair,glasses";
Uri apiEndPoint = new Uri(apiQuery);
HttpRequestMessage request = new HttpRequestMessage(HttpMethod.Post, apiEndPoint);
request.Content = streamContent;
```

### Snippet `face4`
```
// Do an asynchronous POST.
HttpClient httpClient = new HttpClient();
string apiKey = "e0643cb1a6404b6bbfd7b6fb20f67963"; //Replace this with your own Microsoft Cognitive Services Face API key from https://www.microsoft.com/cognitive-services/en-us/face-api. Please do not use my key. I include it here so you can get up and running quickly
httpClient.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", apiKey);
HttpResponseMessage response = await httpClient.SendRequestAsync(request).AsTask();
```

### Snippet `face5`
```
// Read response
string responseContent = await response.Content.ReadAsStringAsync();
```

### Snippet `face6`
```
// Display response
textBlock.Text = responseContent;
```

## Run the app
Run the app, take a selfie and watch the json come in