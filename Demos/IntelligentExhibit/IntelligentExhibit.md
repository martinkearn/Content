# Intelligent Exhibit

How to demonstrate the Black Radley Intelligent Exhibit solution

## Microsoft Museum

Use Windows camera app to show audience the devices

Use Device B (Windows 2000 professional) box

Have someone in 20’s walk up > get ‘Younger’ audio

Have someone 35+ walk up > get ‘older’ audio

Show photo of Jamie > get child's audio

Repeat with device A (Surface Pro 3)

## Visitor Data

Show TableStorage.xlsx in Excel

Sort by partition key (face ID)

Open Azure Storage Explorer

Martin Kearn MSFT Internal Consumption > Storage Accounts > intelligentexhibit > Tables > patronsightings

Show raw data

## Admin & Onboarding

Run app - show it is waiting to be onboarded

Show admin interface

Explore voice package

* Voice json

* WAVs

Create new

* Label = `SurfaceBook`

* Exhibit = `SurfaceBook`

* Venue = `MuseumOfMicrosoft`
* `Speak System Info`
* `Is Sound On`

* VoicePackage = https://intelligentexhibit.blob.core.windows.net/voicepackages/Surface Pro-B-20171025095620.zip (also in TXT file on desktop)
* `Create`

Run app

* make sure no faces are in view

Show code

* Open http://intelligentexhibit-webapp.azurewebsites.net/ on phone or ipad 
* Devices > SurfaceBook > Details
* Show QR code to webcam

Show app onboarding and working

### Reset

Delete everything in C:\Users\mkearn\AppData\Local\Packages\BlackRadleyIntelligentExhibit2_0z116gybmc3me\Settings

Delete everything in C:\Users\mkearn\AppData\Local\Packages\BlackRadleyIntelligentExhibit2_0z116gybmc3me\LocalState\Assets\Voice

Remove `SurfaceBook` device



