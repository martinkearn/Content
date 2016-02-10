
# Windows 10 Westminster Standalone
This script shows the capabilities available via Windows 10 Westminster. It has no pre-reqs on manifold JS

### Pre-reqs
* Have a browser open to http://sentimentalweb.azurewebsites.net/
* Have a browser open to http://codepen.io/seksenov/pen/wBbVyb/?editors=101
* Have the code snippets open
* Have the images folder open
* The [SentimentalWeb](https://github.com/martinkearn/SentimentalWeb) project open in VS code 

## Introduce Sentimental web
Look at [sentimentalweb.azurewebsites.net](http://sentimentalweb.azurewebsites.net/)

Do analysis on the title text ("Sentimental tells you about the sentiment and key phrases in text. It is really quite awesome.")

Show that it is repsonsive

## Create an initial Windows app
Create a new Windows JS app

Delete css, js and winjs folders and default.html

Copy images into app solution
* From this GitHub repo

Package manifest
* Display name > Sentimental
* Start page > http://sentimentalweb.azurewebsites.net
* Add Content URI http://codepen.io/seksenov/pen/wBbVyb/?editors=101 Include, None for WinRT access
* Visual Assets > background colour `#808080`
* Visual Assets > get rid of all errors

Run and explore app
* Function
* Responsive
* Icons
* Start

## Add Windows code to the Sentimental app
Explore the [SentimentalWeb](https://github.com/martinkearn/SentimentalWeb) project
* Look at the Windows JS at the bottom of Index.
* Look at the Windows.js file

Enable WinRT access in package manifest

Re-run the app
* Do an analysis
* Popup
* Notification
* Search and re-pin the tile > it is live

## Create a new Windows JS app based on Codepen
Explore http://codepen.io/seksenov/pen/wBbVyb/?editors=101 in browser

Create a new Windows JS app

Delete css, js and winjs folders and default.html

Package manifest
* Start page > http://codepen.io/seksenov/pen/wBbVyb/?editors=101
* Add Content URI http://codepen.io/seksenov/pen/wBbVyb/?editors=101 Include, All for WinRT access
* Add Content URI http://*.codepen.io/ Include, All for WinRT access
* Show toast notification

## Enable camera capture
Copy the `function cameraCapture()` code below `systemAlertCommandInvokedHandler()` in the JS section

Use the camera capture button
* Save is not implemented

## Defaut and Live Tiles
Open Package.appxmanifest > Visual assets

Copy images from C:\Users\marti\OneDrive\Work\NDC\HostedWebApp\images or https://github.com/martinkearn/WinDevHOLs/tree/master/04.%20Edge%20and%20Web%20Apps/04.%20Lab.%20Solution/HostedWebApp/images
Set images
* Square 71x71 - scale 200
* Square 150x150 - scale 200
* Square 44x33 - scale 200
* SplashScreen - scale 200

All Image Assets > Background colour to deepSkyBlue

Run

Find and pin the app to start

Add the `<br><br>` HTML snippet to add a tile button. Just below the camera button

Add the `updateTile()` JavaScript snippet just below the `cameraCapture()` function.

Click the 'Update tile' button and wait up to 20 second to see the tile update on your start menu

## Snippets

http://codepen.io/seksenov/pen/wBbVyb/?editors=101

http://*.codepen.io/

```
function cameraCapture() {
     if (typeof Windows != 'undefined')
     {
         var captureUI = new Windows.Media.Capture.CameraCaptureUI();
         //Set the format of the picture to be captured (.png, .jpg, ...) 
         captureUI.photoSettings.format =
             Windows.Media.Capture.CameraCaptureUIPhotoFormat.png;
         //Pop up the camera UI to take a picture 
         captureUI.captureFileAsync(
             Windows.Media.Capture.CameraCaptureUIMode.photo).then(
             function(capturedItem) {
             // Do something with the picture 
         });
   }
}
```
```
<br><br>
<button class="btn" onClick="updateTile() ">Update Tile</button>
```

```
function updateTile() {
     if (typeof Windows !== 'undefined' && typeof Windows.UI !== 'undefined' &&
         typeof Windows.UI.Notifications !== 'undefined')
     {
     console.log('Attempting to update the tile');
     var notifications = Windows.UI.Notifications,
         tile =    notifications.TileTemplateType.tileSquare150x150PeekImageAndText01,
         tileContent = notifications.TileUpdateManager.getTemplateContent(
             tile),
         tileText = tileContent.getElementsByTagName('text'),
         tileImage = tileContent.getElementsByTagName('image');
  
         tileText[0].appendChild(tileContent.createTextNode('Demo Message'));
         tileImage[0].setAttribute('src',
             'http://unsplash.it/150/150/?random');
         tileImage[0].setAttribute('alt', 'Random demo image');
  
         var tileNotification = new
             notifications.TileNotification(tileContent);
         var currentTime = new Date();
         tileNotification.expirationTime = new Date(currentTime.getTime() + 20
             * 1000);
  
         notifications.TileUpdateManager.createTileUpdaterForApplication(
             ).update( tileNotification);
  
   }
   else {
     //alternate behavior
   }
}
```