
# A Look at ManifoldJS
This script shows the basic operation of ManifoldJS through both the website and command line. Focusing slightly on Windows 10 features.

### Pre-reqs
* Install ManifoldJS NPM
* Camera app open on right camera
* Android tablet with Sentimental app installed

## Introduce Sentimental web
Look at [sentimentalweb.azurewebsites.net](http://sentimentalweb.azurewebsites.net/)

Do a analysis on the title text ("Sentimental tells you about the sentiment and key phrases in text. It is really quite awesome.")

Show that it is repsonsive

Look at manifest file: http://sentimentalweb.azurewebsites.net/manifest.webmanifest

## Introduce ManifoldJS
Look at [manifoldjs.com](http://manifoldjs.com/)

Go through steps of generating ManifoldJS via website, but do not actually creat packages

Show manifold NPM install in command prompt 
* `npm install -g manifoldjs`

Create packages via command prompt 
* `manifoldjs http://sentimentalweb.azurewebsites.net -d C:\DemoDump\Manifold`

While the NPM command is runnig show the android app by projecting via the Windows camera app

Explore the zip file
* Android project
* IOS project
* Chrome
* Firefox

(a backup zip is stored in C:\Users\marti\Downloads\ManifoldJS Sentimental)

## Explore Windows 10 app
Look at images

Package Manifest
* Start page
* Content URIs
* WinRT Allow All

Run and demonstrate the app
* Splash screen
* Icons
* Do a analysis
* Responsive
* Search and pin it to start
* Start tile