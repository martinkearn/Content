
# Cognitive Services in Action
This demo shows some of the capabilities of Cognitive Services by using the live demo site and some of the spin-off sites created to show-case the APIs

### Pre-reqs
* Have easy access to the supporting files folder
* Ideally a machine with a webcam
* The camera app open

## Emotion API
Show the [Emotion.jpg](https://github.com/martinkearn/Content/blob/master/Demos/Project%20Oxford/Supporting%20Files/Emotion.jpg)

Go to https://www.microsoft.com/cognitive-services/en-us/emotion-api
* or just search for "Microsoft Cognitive" and go from there

Scroll down the Emotion API page and use the live demo section to analyse photo

The primary emotions here are:
* 61% contempt
* 20% disgust
* 9% anger

_This is what happens when you tell a three-year-old who has no respect for her father that she cannot have another piece of chocolate_

## MyMoustache.net (optional)
*Skip if time contsrained*

_Built on the Face API_

As the audience to a volunteer who wears a moustache.
* If someone comes forward, take a photo using the Windows camera app
* If no-one comes forward use [Emotion.jpg](https://github.com/martinkearn/Content/blob/master/Demos/Project%20Oxford/Supporting%20Files/Moustache1.jpg)

Go to <https://www.mymoustache.net>

Upload the photo and discuss results

## TwinsOrNot.net
_This demo only really works for Martin Kearn because he apparently looks like Jake Gyllenhaal_

Go to <https://www.twinsornot.net>

Upload
* [Twins_Martin.jpg](https://github.com/martinkearn/Content/blob/master/Demos/Project%20Oxford/Supporting%20Files/Twins_Martin.jpg)
* [Jake.jpg ](https://raw.githubusercontent.com/martinkearn/Content/master/Demos/Machine%20Learning/Supporting%20Files/Jake.jpg)
* Should get a result of around 90% - #besties

## HowOld.net
_It is not all upside for Martin, apparently he looks 6 years older than he is, despite looking like Jake Gyllenhall_

Go to <http://how-old.net>

Upload [HowOld_Martin.jpg](https://github.com/martinkearn/Content/blob/master/Demos/Project%20Oxford/Supporting%20Files/HowOld_Martin.jpg)
* Result should be 42 (actual age 36)

_It is not that bad though, at least Jake looks old too_

Upload [Jake.jpg](https://raw.githubusercontent.com/martinkearn/Content/master/Demos/Machine%20Learning/Supporting%20Files/Jake.jpg)
* Result should be 43 (actual age 35)

## Sentimental (optional)
*Do not show for web day, this is covered in the hybrid talk*

_To bring this all together I want to show an Office add-in that uses the Text Analytics API to do sentiment and key phrase analysis on text in Office documents. The add-in is called 'Sentimental' and you can get it from the [Office Store](https://store.office.com/sentimental-WA104379510.aspx?assetid=WA104379510&sourcecorrid=755ae580-2491-436f-8471-7888c38149d7&searchapppos=0)_

Open Excel

Install or activate Sentimental

Write `I love Office, it rocks` in a cell

Analyse

Insert score and key phrases
