
# Windows 10 Westminster Say Cheese
This script shows the advanced capabilities available via Windows 10 Westminster based on CodePen (say cheese edition)

### Pre-reqs
* Have a browser open to http://codepen.io/MartinKearn/pen/dMqdOo
* Have the code snippets open

## Create a new Windows JS app
Create a new Windows JS app
* Call it 'say cheese'

Delete css, js and winjs folders and default.html

Explore http://codepen.io/MartinKearn/pen/dMqdOo in browser

Package manifest
* Start page > http://codepen.io/MartinKearn/pen/dMqdOo
* Add Content URI http://codepen.io/MartinKearn/pen/dMqdOo Include, All for WinRT access
* Add Content URI http://*.codepen.io/ Include, All for WinRT access
* Show toast notification

## Enable camera capture
Copy the `function cameraCapture()` code snippet to the JS section

Use the camera capture button

Go to C:\Users\mkearn\AppData\Local\Packages and find the most recent package

Now go to the LocalCache folder and show the photo has been saved

## Default and Live Tiles
Open Package.appxmanifest > Visual assets

Copy images from C:\Users\mkearn\OneDrive\Work\WooWeb Worcester\AppImages

Set images

All Image Assets > Background colour to `yellow`

Run

Find and pin the app to start

Add the `updateTile()` JavaScript snippet just below the `cameraCapture()` function.

Click the 'Update tile' button and wait up to 20 second to see the photo update on your start menu

## Snippets

http://codepen.io/seksenov/pen/wBbVyb/?editors=101

http://*.codepen.io/

```
function cameraCapture() {
  if (typeof Windows != 'undefined') {
    var captureUI = new Windows.Media.Capture.CameraCaptureUI();

    //Set the format of the picture to be captured (.png, .jpg, ...) 
    captureUI.photoSettings.format =
      Windows.Media.Capture.CameraCaptureUIPhotoFormat.png;

    //Pop up the camera UI to take a picture 
    captureUI.captureFileAsync(
      Windows.Media.Capture.CameraCaptureUIMode.photo).then(
      function(capturedItem) {
        var applicationData = Windows.Storage.ApplicationData.current;
        var localFolder = applicationData.localFolder;

        //save the captured file into the local storage folder
        localFolder.createFileAsync("PhotoFromWestminster.jpg", Windows.Storage.CreationCollisionOption.replaceExisting)
          .then(function(file) {
            capturedItem.copyAndReplaceAsync(file);
          }).then(function() {
            updateTile();
          });

      });
  }
}
```

```
function updateTile() {
  if (typeof Windows !== 'undefined' && typeof Windows.UI !== 'undefined' &&
    typeof Windows.UI.Notifications !== 'undefined') {
    var notifications = Windows.UI.Notifications;
    var currentTime = new Date();
    var tileText = "Greetings from Windows Westminster";

    //150x150 Square tile
    console.log('Attempting to update the 150x150 tile');
    var tileSquareTemplate = notifications.TileTemplateType.tileSquare150x150PeekImageAndText04;
    var tileSquareContent = notifications.TileUpdateManager.getTemplateContent(tileSquareTemplate);
    var tileSquareText = tileSquareContent.getElementsByTagName('text');
    var tileSquareImage = tileSquareContent.getElementsByTagName('image');
    tileSquareText[0].appendChild(tileSquareContent.createTextNode('Greetings from Windows Westminster'));
    tileSquareImage[0].setAttribute('src', 'ms-appdata:///local/PhotoFromWestminster.jpg');
    var tileSquareNotification = new notifications.TileNotification(tileSquareContent);
    tileSquareNotification.expirationTime = new Date(currentTime.getTime() + 20 * 1000);
    notifications.TileUpdateManager.createTileUpdaterForApplication().update(tileSquareNotification);
    console.log('Finished updating the 150x150 tile');

  } else {
    //alternate behavior
  }
}
```