---
title: Introducing HowHappy
author: Martin Kearn
description: This week was the annual International Day of Happiness; so i built a website ot see hwo happy you are based on your face using the Cognitive Services Emotion API
image: https://raw.githubusercontent.com/martinkearn/Content/master/Blogs/Images/HowHappyScreenShot.PNG
thumbnail: https://raw.githubusercontent.com/martinkearn/Content/master/Blogs/Images/HowHappyScreenShot_thumb.png
type: article
status: published
published: 2016/03/22 09:40:00
categories: 
  - Cognitive Services
  - Emotion API
---

# Introducing HowHappy

This week was the annual [International Day of Happiness](http://www.dayofhappiness.net/); I never knew we had a specific day to be really happy but it turns out that we do. 

This inspired me to finish a pet project I've been working on recently which uses the Azure Machine Learning [Project Oxford Emotion API](https://www.projectoxford.ai/emotion) (which I have [blogged about before](https://blogs.msdn.microsoft.com/martinkearn/2016/03/07/using-the-project-oxford-emotion-api-in-c-and-javascript/)) to take a photo containing up to 64 human faces and order them by happiness. 

It is really fun to use when talking to groups of people [as I did at one of my recent events](https://blogs.msdn.microsoft.com/martinkearn/2016/03/07/using-the-project-oxford-emotion-api-in-c-and-javascript/). 

The website is available for anyone to use at [HowHappy.co.uk](http://howhappy.co.uk). 

![Some happy people at a recent MS Web Day event](https://raw.githubusercontent.com/martinkearn/Content/master/Blogs/Images/HowHappyScreenShot.PNG)

## How does it work?

The website builds on the [simple emotion API example](https://github.com/martinkearn/Project-Oxford-Emotion-API-sample) I published on GitHub a few weeks ago. 

It is written in ASP.NET Core 1.0 with a bit of JQuery and Bootstrap. 

You basically upload a photo and get a JSON response from the API. I then sort the results by the happiness score and use some JQuery and Bootstrap magic to plot the scores on the original picture.

You can see my source code here: [https://github.com/martinkearn/How-Happy](https://github.com/martinkearn/How-Happy)

## What's next?

This project is certainly not finished. Here are some of the things I want to do next:

* Scale the photo to fit on a screen rather than relying on the weird drag feature I have right now. If anyone can help with that, I do accept pull requests! :)
* Plot as a list as well as an image. I might steal from [Martin Beeby's HowHappyCordova](https://github.com/thebeebs/howhappycordova) project for this
* Do the same for all the emotion scores (how angry etc)

#### Resources

* [HowHappy.co.uk](http://howhappy.co.uk): The website this article is about
* [HowHappy GitHub repository](https://github.com/martinkearn/How-Happy): The source code for HowHappy.co.uk
* [Project Oxford Emotion API](https://www.projectoxford.ai/emotion): The underlying Azure Machine Learning API used for HowHappy.co.uk
