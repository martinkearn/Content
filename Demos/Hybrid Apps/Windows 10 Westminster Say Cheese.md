
# Windows 10 Westminster Say Cheese
This script shows the advanced capabilities available via Windows 10 Westminster based on CodePen (say cheese edition)

### Pre-reqs
* Have a browser open to http://codepen.io/MartinKearn/pen/dMqdOo
* Have the code snippets open

## Explore Codepen in browser
Explore http://codepen.io/MartinKearn/pen/dMqdOo in browser

## Create a new Windows JS app
Create a new Windows JS app
* Templates > Other Languages > JavaScript > Windows > Universal > Blank App (Windows Universal)
* Call it "Say Cheese"
* Accept default target versions

Delete css, js folders and index.html

Package manifest
* Start page > http://codepen.io/MartinKearn/pen/dMqdOo
* Add Content URI http://codepen.io/MartinKearn/pen/dMqdOo Include, All for WinRT access
* Add Content URI http://*.codepen.io/ Include, All for WinRT access

Open Package.appxmanifest > Visual assets

Copy images from C:\Users\mkearn\OneDrive\Work\WooWeb Worcester\AppImages to the images folder
* Delete the default StoreLogo.png
* Set short name to "Say Cheese"
* Set Square 71x71 to `images\Square71x71Logo.png`
* Slash Screen background colour to `yellow`

## Enable camera capture
Run the app

Copy the `function cameraCapture()` code snippet to the JS section

Use the 'Say cheese' button and save the image

Go to C:\Users\mkearn\AppData\Local\Packages and find the most recent package

Now go to the `LocalState` folder and show the photo has been saved


## Enable live tile
Run

Find and pin the app to start
* Show static tile on start meu

Add the `updateTile()` JavaScript snippet just below the `cameraCapture()` function.
* Show how the `cameraCapture` function calls it

Click the 'Say cheese' button again
* Make sure it is a square
* Save the image

Wait up to 20 second to see the photo update on your start menu
* Picture and text

## Snippets

http://codepen.io/seksenov/pen/wBbVyb/?editors=101

http://*.codepen.io/

```
function cameraCapture() {
  if (typeof Windows != 'undefined') {
    var captureUI = new Windows.Media.Capture.CameraCaptureUI();

    //Set the format of the picture to be captured (.png, .jpg, ...) 
    captureUI.photoSettings.format = Windows.Media.Capture.CameraCaptureUIPhotoFormat.png;

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