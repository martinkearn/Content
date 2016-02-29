
# Machine Learning is for Muggles too!
Machine Learning is one of those tech areas which has always seem just slightly out of reach for me. I've always assumes you need a degree in extreme nerdiness from the Boffins university to understand the algorithms and data science that sits behind machine learning. Perhaps you need some kind of magic to get it and those of us born without magic (i.e [muggles](https://en.wikipedia.org/wiki/Muggle)) should not attempt to enter this world for fear of smashing our face on [platform 9 3/4](https://en.wikipedia.org/wiki/London_King%27s_Cross_railway_station#Harry_Potter) at London Kings Cross station.

I've discovered of late that this is really not the case. Machine Learning is conceptually fairly simple and Microsoft has some great tools in the [Azure Machine Learning service](https://azure.microsoft.com/en-us/services/machine-learning/) and [Project Oxford](https://www.projectoxford.ai/) which makes it accessible to us mere muggles. 

## So what is Machine Learning?
The principle of Machine Learning is actually very simple. Machine Learning is a process by which computers find patterns in data and makes those patterns available to applications. The application can then gain insights on new data based on conformity to the identified patterns.

Take a canonical example such as the product recommendations. If a Machine Learning process is given a set of historical order data and asked the question *"find out which products are commonly ordered together"*, the Machine Learning process can examine the historical data and work out which products are most commonly ordered together. Once these patterns are identified, the application can ask *"which products are most commonly ordered with this one"* when a user adds something to their shopping cart. The Machine Learning process will return a result set and the application can display its product recommendations.

Recommendations are just one example, but machine Learning can help in any scenario where patterns can be usefully recognised in data, some other examples include:
* Fraud detection
* Revenue predictions
* Customer churn predictions
* Predictive maintenance
* Computer vision
* Sentiment analysis
* The list goes on, and on and on

## The Machine Learning process
Regardless of the platform being used, Machine Learning follows a relatively simple process, the primary goal being to identify a 'Model'. This is the main thing that applications can submit requests to in order to gain insight on new data. A person working as the role of a [Data Scientist](https://en.wikipedia.org/wiki/Data_science#Data_scientist) performs the machine learning process.

The process starts with a question; what are you trying to learn from your Machine Learning experiment? For example, in the case of recommendations, the question might be *"identify most commonly sold products for each product in the inventory"*

The next step is to provide 'prepared data'. Prepared data is one or more data sets that have been pre-processed (formatted, cleaned and sampled). Preparing the data means that the data is in the best shape to draw scientific conclusions from and is not skewed in any way.

Once you have your prepared data, you apply one or more [Machine Learning algorithms](http://machinelearningmastery.com/a-tour-of-machine-learning-algorithms/) to it with a view to producing a Model. This is an iterative process and you may loop around testing various algorithms until you have a Model that sufficiently answers your question.

Once you have produced your chosen model, it will typically be exposed via some kind of API.

![The high level Machine Learning process](/Images/MLProcess.PNG)

If you want to learn more about the Machine Learning process, I highly reocmmend [David Chappell's Introduction for Technical Professionals white paper]( http://download.microsoft.com/download/3/B/9/3B9FBA69-8AAD-4707-830F-6C70A545C389/Introducing_Azure_Machine_Learning.pdf)

## Azure Machine Learning
[Azure Machine Learning service](https://azure.microsoft.com/en-us/services/machine-learning/) is one of the main platforms for doing Machine Learning in a quick, easy, cloud-based way. 

The service contains a set of tools and modules that help the data scientist setup and run the Machine Learning process. It is designed for applied machine learning meaning that it is designed to be used by real world applications and developers.

## Project Oxford