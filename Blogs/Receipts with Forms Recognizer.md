---
title: Receipt Recognition with Form Recogniser
author: Martin Kearn
description: The new Cognitive Services Forms Recognizer API has a pre-built receipt recognition model. I investigated it for a customer and this is what I found out in terms of how it works, what works well, what is not so good.
image: https://github.com/martinkearn/Content/raw/master/Blogs/Images/till_receipts-260.jpg
thumbnail: https://github.com/martinkearn/Content/raw/master/Blogs/Images/till_receipts-1200.jpg
type: article
status: draft
published: 2019/09/04 13:00:00
categories: 
  - Cognitive Services
  - Document Recognition
  - OCR
---

# Receipt Recognition with Form Recogniser

One of the latest [Cognitive Services](https://azure.microsoft.com/en-gb/services/cognitive-services/) is [Form Recogniser](https://azure.microsoft.com/en-gb/services/cognitive-services/form-recognizer/) which is a preview API built around helping extract data from electronic forms. 

Specifically, Form Recogniser can extract text, key/value pairs and tables from documents and receipts.

There are two modes of operation for Forms Recogniser:

1. **[Analyze Form](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api/operations/AnalyzeWithCustomModel)**: A trainable mode which requires at least 5 sample documents of the same type. It uses transfer learning algorithms based on your 5 samples to build a recognition model. You can use this model to extract data from new forms of the same type.
2. [**Analyze Receipt**](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api/operations/AnalyzeReceipt): A pre-built model which cannot be trained (it is pre-trained by Microsoft). Analyze Receipt will extract multiple key/value pairs as well as line item data from typical retail receipts.

I've been working with a customer that uses general retail receipts for market research purposes and had a requirement to extract specific data points from photos of receipts. I did some research around the Analyze Receipt function and this article is what I learnt ....

## What gets extracted?

There are common fields found in most retail receipts such as retailer, amount, date etc. 

The receipts model looks to extract these common fields, specifically it can identify the following:

- MerchantName
- MerchantPhoneNumber
- MerchantAddress
- TransactionDate
- TransactionTime
- Total
- SubTotal
- Tax

Interestingly, the receipts model does not contain any special functionality around recognizing line items within a receipt. The raw OCR data for lines items is contained, but they are not explicitly extracted out as 'understood fields'. See the Missing Features section later in this article for details.

## How do I use it?

Because it is still in private preview, you must request access via the [Form Recognizer access request](https://aka.ms/FormRecognizerRequestAccess) form. When access is granted you should receive an email with a specific link to create the Form Recogniser resource in your Azure subscription. 

When I wrote this article (September 2019), the link was as follows: https://portal.azure.com/?microsoft_azure_marketplace_ItemHideKey=microsoft_azure_cognitiveservices_formUnderstandingPreview#create/Microsoft.CognitiveServicesFormRecognizer

> Note: You will not find it by searching or browsing in the 'Create' menu whilst it is still a preview service.

Once you have a provisioned the service, you'll have your own service endpoint which will be something like `https://whateveryoucalledit.cognitiveservices.azure.com`. There will also be a key that you'll need. You can get both of these by looking in the Azure Portal in the `Quick Start` section for the resource. 

The Receipts function uses a two part approach where you initially post the [Analyze Receipt](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api/operations/AnalyzeReceipt) request and then check [Get Receipt Result](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api/operations/GetReceiptResult) until you get a `200` response (I suspect that this may be tidied up into a single call before the service goes into general availability).

You can `POST` an image containing a receipt with your key in the `Ocp-Apim-Subscription-Key` header to [Analyze Receipt](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api/operations/AnalyzeReceipt). The response will contain a header called `Operation-Location` which contains a URL which you will need to get the result.

When you have `Operation-Location` you can send a `GET` request to whatever the value of `Operation-Location`  was (see [Get Receipt Result](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api/operations/GetReceiptResult)). The response will be a JSON payload containing all the recognized data.

The response is split into two sections:

- **recognitionResults**: This is the raw optical character recognition output which contains the same output that you would get from the standard [Computer Vision OCR API](https://azure.microsoft.com/en-gb/services/cognitive-services/computer-vision/#detect-text). This is a series of lines containing identified words with their 'bounding box' which can be used to determine their position within the image. This can be used to find fields that the receipt model does not automatically extract, providing you know where they will appear within the image.
- **understandingResults**: These are the specifically recognized key/value pairs that the receipt model looks for (merchantName, total etc). This is the `understandingResults` section from [this image](https://raw.githubusercontent.com/martinkearn/Content/master/Demos/Machine%20Learning%20and%20Cognitive/ML%20Supporting%20Files/Receipts/TheCurator-3140.jpg)

![](https://raw.githubusercontent.com/martinkearn/Content/master/Demos/Machine%20Learning%20and%20Cognitive/ML%20Supporting%20Files/Receipts/TheCurator-3140-Thumb.jpg)

```json
"understandingResults": [
        {
            "pages": [
                1
            ],
            "fields": {
                "Subtotal": null,
                "Total": {
                    "valueType": "numberValue",
                    "value": 28.4,
                    "text": "28.40",
                    "elements": [
                        {
                            "$ref": "#/recognitionResults/0/lines/23/words/0"
                        },
                        {
                            "$ref": "#/recognitionResults/0/lines/23/words/1"
                        },
                        {
                            "$ref": "#/recognitionResults/0/lines/23/words/2"
                        }
                    ]
                },
                "Tax": null,
                "MerchantAddress": null,
                "MerchantName": {
                    "valueType": "stringValue",
                    "value": "The Curator",
                    "text": "The Curator",
                    "elements": [
                        {
                            "$ref": "#/recognitionResults/0/lines/0/words/0"
                        },
                        {
                            "$ref": "#/recognitionResults/0/lines/0/words/1"
                        }
                    ]
                },
                "MerchantPhoneNumber": null,
                "TransactionDate": {
                    "valueType": "stringValue",
                    "value": "2019-02-19",
                    "text": "19 Feb 2019",
                    "elements": [
                        {
                            "$ref": "#/recognitionResults/0/lines/7/words/0"
                        },
                        {
                            "$ref": "#/recognitionResults/0/lines/7/words/1"
                        },
                        {
                            "$ref": "#/recognitionResults/0/lines/7/words/2"
                        }
                    ]
                },
                "TransactionTime": {
                    "valueType": "stringValue",
                    "value": "14:46:00",
                    "text": "14:46",
                    "elements": [
                        {
                            "$ref": "#/recognitionResults/0/lines/7/words/3"
                        }
                    ]
                }
            }
        }
    ]
```

You can see the official quick-start steps for testing the service out here: https://docs.microsoft.com/en-us/azure/cognitive-services/form-recognizer/quickstarts/curl-receipts

## Common Errors & pit-falls

## Missing Features

Line item recognition

Tips

Resolutions for multiple matches

## In Summary

To do

## Further Reading

You may find these resources useful.

- [Cognitive Services Form Recognizer Product Page](https://azure.microsoft.com/en-us/services/cognitive-services/form-recognizer/)
- [Microsoft Azure Docs > Forms Recognizer Overview](https://docs.microsoft.com/en-us/azure/cognitive-services/form-recognizer/overview)
- [Microsoft Azure Docs > Quickstart: Extract receipt data using the Form Recognizer REST API with cURL](https://docs.microsoft.com/en-us/azure/cognitive-services/form-recognizer/quickstarts/curl-receipts)
- [Microsoft Azure Docs > Quickstart: Extract receipt data using the Form Recognizer REST API with Python](https://docs.microsoft.com/en-us/azure/cognitive-services/form-recognizer/quickstarts/python-receipts)
