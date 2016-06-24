# Get Started with Cognitive Services
This demo shows how to get started with Microsoft Cognitive Services

### Pre-reqs
* None

## Create a Microsoft account
This can be skipped if necessary but good to point out where to create a Microsoft account if they do no have one.

Search for "Microsoft account signup"
* This is the same as @Hotmail.co.uk, @Outlook.com, @Live.co.uk  any of those will do
* Also the same as your Azure login if you are already using Azure

Start the process of signing up
* Continue to sign up or login with an existing account

## Sign up to Cognitive
Search for "Microsoft cognitive sevrices"

Show that each API has a test app
* Show the Computer Vision > Analyse an image sample app

Sign-up to 'Text Analytics'
* Show the functions of the API
* 'Get started for free'
* Agree to OAUTH and license

Expose and copy 'Key 1'

## Explore API
Go back to the main 'Text Analytics' page

API reference

Go to POST Sentiment > Try It
* Set `ocp-apim-subscription-key` as the key your copied previously
* Set the request body as follows and execute the request

```
{
  "documents": [
    {
      "language": "en",
      "id": "1",
      "text": "I cannot beleive how easy this is - it is really awesome"
    }
  ]
}
```
