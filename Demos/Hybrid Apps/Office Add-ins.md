
# Office add-ins
This script shows how a web application can also be extended into office

### Pre-reqs
* Complete the [ManifoldJS demo](ManifoldJS.md) 
* The [SentimentalWeb](https://github.com/martinkearn/SentimentalWeb) project open in VS code 
* VS 2015 installed with office tools

## Show some popular Office add ins
Word > Store

Word > Wikipedia
* Search Video
* Insert picture

Word > Translator
* Welsh
* Insert 

## Explore Sentimental web in VS code
Layout references (top)

Show what /wwwroot/js/OfficeApp.js does

## Create Office app
Create new Excel Add-in App
* File New > Visual C# > Office/SharePoint > Web Add-ins > Excel Add-in
* Add new functionalities to Excel

Update Manifest.xml
* Replace `~remoteAppUrl/Home.html` with `https://sentimentalweb.azurewebsites.net`

Launch and use in excel

Show selecting data from Office populates the app
