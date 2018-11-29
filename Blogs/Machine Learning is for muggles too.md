---
title: Machine Learning is for Muggles too!
author: Martin Kearn
description: Machine Learning is one of those tech areas which has always seemed just slightly out of reach for me. In this article, I'll demystify machine learning and explain how acessible it is for regular developers
image: https://dummyimage.com/800x600/000/fff&text=placeholder
thumbnail: https://dummyimage.com/200x200/000/fff&text=placeholder
type: article
status: published
published: 2016/03/01 09:30:00
categories: 
  - Cognitive Services
  - Machine Learning
---

# Machine Learning is for Muggles too!
Machine Learning is one of those tech areas which has always seemed just slightly out of reach for me. I've always assumed you need a degree in extreme nerdiness from the Boffins university to understand the algorithms and data science that sits behind Machine Learning. Perhaps you need some kind of magic to 'get it' and those of us born without magic (i.e [muggles](https://en.wikipedia.org/wiki/Muggle)) should not attempt to enter this world for fear of smashing our face on [platform 9 3/4](https://en.wikipedia.org/wiki/London_King%27s_Cross_railway_station#Harry_Potter) at London Kings Cross station.

I've discovered of late that this is really not the case. Machine Learning is conceptually fairly simple and Microsoft has some great tools in the [Azure Machine Learning service](https://azure.microsoft.com/en-us/services/machine-learning/), [Project Oxford](https://www.projectoxford.ai/) and [Cortana Analytics](https://www.microsoft.com/en-gb/server-cloud/cortana-analytics-suite/overview.aspx) which makes Machine Learning accessible to us mere muggles. 

## So what is Machine Learning?
The principle of Machine Learning is actually very simple; Machine Learning is a process by which computers find patterns in data and makes those patterns available to applications. The application can then gain insights on new data based on conformity to the identified patterns.

Take a canonical example such as the product recommendations. If a Machine Learning process is given a set of historical order data and asked the question *"find out which products are commonly ordered together"*, the Machine Learning process can examine the historical data and work out which products are most commonly ordered together based on historical trends. 

Once these patterns are identified, the application can ask *"which products are most commonly ordered with this one"* when a user adds something to their shopping cart. The Machine Learning process will return a result set and the application can display its product recommendations.

Recommendations are just one example, but Machine Learning can help in any scenario where patterns can be usefully recognised in data, some other examples include:
* Fraud detection
* Revenue predictions
* Customer churn predictions
* Predictive maintenance
* Computer vision
* Sentiment analysis
* The list goes on, and on and on

## The Machine Learning process
Regardless of the platform being used, Machine Learning follows a relatively simple process.


![The high level Machine Learning process](https://raw.githubusercontent.com/martinkearn/Content/master/Blogs/Images/MLProcess.PNG)

The primary goal of the process is to identify a 'Model'. The Model is the main thing that applications can submit requests to in order to gain insight on new data. A person working as the role of a [Data Scientist](https://en.wikipedia.org/wiki/Data_science#Data_scientist) performs the Machine Learning process and will ultimately decide on the right model to use.

The process starts with a question; what are you trying to learn from your Machine Learning experiment? For example, in the case of recommendations, the question might be *"identify most commonly sold products for each product in the inventory"*

The next step is to provide 'prepared data'. Prepared data is one or more data sets that have been pre-processed (formatted, cleaned and sampled) in readiness to apply Machine Learning algorthms to. Preparing the data means that the data is in the best shape to draw scientific conclusions from and is not skewed in any way.

Once you have your prepared data, you apply one or more [Machine Learning algorithms](http://machinelearningmastery.com/a-tour-of-machine-learning-algorithms/) to it with a view to producing a Model. This is an iterative process and you may loop around testing various algorithms until you have a Model that sufficiently answers your question.

Once you have produced your chosen model, it will typically be exposed via some kind of API.

If you want to learn more about the Machine Learning process, I highly reocmmend [David Chappell's Introduction for Technical Professionals white paper]( http://download.microsoft.com/download/3/B/9/3B9FBA69-8AAD-4707-830F-6C70A545C389/Introducing_Azure_Machine_Learning.pdf)

## Azure Machine Learning
[Azure Machine Learning service](https://azure.microsoft.com/en-us/services/machine-learning/) is one of the main platforms for doing Machine Learning in a quick, easy, cloud-based way. 

The service contains a set of tools and modules that help the data scientist setup and run the Machine Learning process. It is designed for applied machine learning meaning and is designed to be used by real world applications and developers.

The Azure machine learning service offers 4 main components

* **ML Studio**: A web-based graphical user interface used to design experiments in a simple drag and drop style. Think of it as a web-based IDE for Data Scientists
* **Data pre-processing modules**: Azure offers a set of data pre-processing modules which can help clean, format and sample the raw data in order to get to the 'prepared data' stage of the process
* **Machine Learning Algorithms**: Azure offers a set of well-known and understood Machine Learning algorithms which can simply be imported into your experiment and applied to your prepared data to produce a model
* **REST API**: Once your chosen model is established, Azure can package it up as a published REST API which client applications can easily call in any language or platform

Learn more about the Azure Machine Learning service, including a free trial here: https://azure.microsoft.com/en-us/services/machine-learning/

## Project Oxford and Cortana Analytics
If all of the above still seems like it is the work of wizards and other magical folk, do not worry because the boffins at Microsoft have pre-packaged some of the more common Machine Learning scenarios and made them available as APIs which you can easily integrate into your application.

These APIs are available under two distinct brand names (not sure why, please [tweet me](https://twitter.com/MartinKearn/status/704558494238777345) if you know), they are [Project Oxford](https://www.projectoxford.ai/) and [Cortana Analytics](https://www.microsoft.com/en-gb/server-cloud/cortana-analytics-suite/overview.aspx). Both of these services offer a neatly packaged set of APIs which have been built by Microsoft using the Azure Machine Learning service. Both offer most APIs for free under a certain number of transactions per month (typically 10,000) and paid-for models beyond that. In both cases, the APIs are exposed as simple, well documented REST APIs which can be called by pretty much any programming language on any platform. Project Oxford has a set of [SDKs](https://www.projectoxford.ai/SDK) for Windows, Android, IOS and other platforms.

There are many, many APIs to choose from but some of the more popular ones are
* [Project Oxford Face API](https://www.projectoxford.ai/face): Face detection, verification, recognition, grouping, identification and matching in photos
* [Project Oxford Computer Vision API](https://www.projectoxford.ai/vision): Analyse an image and classify visual content, generate thumbnails and OCR
* [Project Oxford Emotion API](https://www.projectoxford.ai/emotion): Analyse a picture of a face and detection levels of emotion shown across anger, contempt, disgust, fear, happiness, neutral, sadness, and surprise
* [Project Oxford Speech API](https://www.projectoxford.ai/speech): Convert spoken audio to text, analyse intent in spoken audio, convert text to spoken audio
* [Cortana Analytics Text Analytics API](http://gallery.cortanaanalytics.com/MachineLearningAPI/Text-Analytics-2?share=1): Sentiment analysis, key phrase extraction, topic detection and language detection in text
* [Cortana Analytics Recommendations API](http://gallery.cortanaanalytics.com/MachineLearningAPI/Recommendations-2?share=1): Frequently bought together recommendations, item to item recommendations and customer to item recommendations
* [Cortana Analytics Content Moderator API](http://gallery.cortanaanalytics.com/MachineLearningAPI/Content-Moderator-1?share=1): Moderation and analysis of user generated images, text and video. Includes adult content detection
* [Cortana Analytics Translator API](http://gallery.cortanaanalytics.com/MachineLearningAPI/Translator-API-1?share=1): Lingual translation of text

The idea of these APIs is that as a developer (on any platform), you can integrate some of the intelligence of Machine Learning into your every day applications with little or no understanding of the underlying Machine Learning process.

I myself have written an app in the Office store called [Sentimental](https://store.office.com/sentimental-WA104379510.aspx?assetid=WA104379510&sourcecorrid=2dce0934-cd35-4ed2-824f-65352a512837&searchapppos=0) which uses the [Cortana Analytics Text Analytics API](http://gallery.cortanaanalytics.com/MachineLearningAPI/Text-Analytics-2?share=1) to do sentiment analysis and key phrase extraction directly from within Office; its pretty cool. There is also a website that does the same thing written in ASP.NET Core: http://sentimentalweb.azurewebsites.net.

## Just for fun - Machine Learning in action
If you want to ge a quick flavour of how these APIs work, Microsoft have created a set of websites that use combination of these APIs as well as some other Azure Machine Learning services.

* [TwinsOrNot.net](https://www.twinsornot.net/#) is built on the Face API to analyse two faces and give a level of similarity between them. So if people have ever said "you look like [insert famous person here]", you can find out for sure if the are right.
* [How-Old.net](http://how-old.net/#) is another website built on the Face API which examines a facial photo and gives an analysis of gender and estimated age. You can see if all those years writing code has made you look older than you really are.
* [What-Dog.net](https://www.what-dog.net/#) is built using the computer vision API and can determine what breed of dog is featured in a photo. It will also tell you what breed of dog you most resemble if you upload a photo of yourself!
* [MyMoustache.net](https://www.mymoustache.net/#) is another website built using the Face API combined with other Azure Machine Learning service which determines the quality of a person's moustache!

## In summary
Machine Learning is for boffins too. Especially is you include the ready-to-use APIs that Project Oxford and Cortana Analytics provide. Any developer can get started with these APIs in minutes and add some powerful intelligence to their applications.

I will be blogging in the coming months on use of specific APIs in both JavaScript and C# so watch this space. 

### Resources
* Azure Machine Learning Service: https://azure.microsoft.com/en-us/services/machine-learning/
* David Chappell's Introduction for Technical Professionals white paper: http://download.microsoft.com/download/3/B/9/3B9FBA69-8AAD-4707-830F-6C70A545C389/Introducing_Azure_Machine_Learning.pdf
* Project Oxford: https://www.projectoxford.ai/
* Cortana Analytics: https://www.microsoft.com/en-gb/server-cloud/cortana-analytics-suite/overview.aspx
* Amy Nicholson, a colleague of mine who specialises in Machine Learning: http://blogs.technet.com/b/amykatenicho/ or https://twitter.com/AmyKateNicho
* Andrew Fryer, another colleague of mine who has a penchant for data science: http://blogs.technet.com/b/andrew/ or https://twitter.com/DeepFat
