# Processing Github Webhooks with Azure Functions

I recently had a need to react to a push event on a GitHub repository and do some data processing on what was pushed. 

GitHub expose webhooks for a wide range of events so I looked into using Azure Functions and webhooks to respond to the GitHub push event.

This turned out to be very easy and simple so I wanted to share what I learnt.

The details of my application are not relevant for this article so I've simplified it to just focus on the basics of Webhooks with Azure Functions.

You can get the function code (all 6 lines of it) here <https://github.com/martinkearn/GitHub-Webhook-Function>

## What is a Webhook?

I'll admit to not being 100% sure what a webhook was before doing this research, but it is actually a very simple concept.

A webhook (sometimes called "web callback" or "reverse api") is where a website, service or application issues a HTTP request to a consuming endpoint in response to an event, usually posting data about the event to the endpoint.

Webhooks enable the endpoint to react to data in near real time without having to poll or run on a timer.

The example that most Azure users are likely to be familiar with is the continuous deployment capabilities between GitHub and Azure which means that code is automatically deployed to Azure when a commit happens on GitHub. This functionality is built on the GitHub webhooks infrastructure.

## Configuring GitHub webhook

GitHub offer webhooks for all sorts of events that could take place in a repository. The [GitHub Developers > Webhooks](https://developer.github.com/webhooks/) documentation is a decent read if you want to get all the details, but the main points/steps are

* You configure a webhook by going to Github > Repository > Settings > Webhooks > Add Webhook
* The `Payload URL` is the endpoint where you want to receive the webhook's payload. For this example, enter `http://localhost:7071/api/Function1` (we'll change this later but is fine for now)
* The `Content type` is the format that payload data comes in; `application/json` is probably the best choice for most scenarios
* You can optionally define a secret to secure the webhook but we don't need that for this scenario
* You can also choose from a wide range of events that will trigger the webhook. We'll use the default `Just the push event`. You can see all the available events at [GitHub Developers > Event Types & Payloads](https://developer.github.com/v3/activity/events/types/)

Once you save the webhook, it is active by default which means that whenever a `push` occurs on the repository, a JSON payload will be sent to the `Payload URL` you defined in real-time.

You can click into the webhook and look at Recent Deliveries to see detail of each event, the payload data and the response.

Right now, this will return an 404 error because the payload endpoint has not been created, but we'll look at that in the next section.

## Creating the Azure Function

An Azure Function is an obvious candidate for the `Payload URL`, although this could be any service that accepts a HTTP Post. For example, in Azure you could also use an Azure Logic App or Web API hosted as an App Service.

We'll stick with Azure Functions as this kind of task is a perfect match for the serverless architecture. 

> **Note**: Azure Functions Webhook mode is only available for version 1.x of the Functions runtime. In version 2.x, the base HTTP trigger still works and is the recommended approach for webhooks.

The simplest way to get started is to follow [Azure Docs > Create your first function using Visual Studio](https://docs.microsoft.com/en-us/azure/azure-functions/functions-create-your-first-function-visual-studio) right up until the 'Publish the project to Azure' section (don't publish to Azure just yet as there are some tips for local debugging I want to cover first).

Once you have a standard Function, we can make it respond to a GitHub push event webhook. To do this, we'll simply add some details from the payload in the 200 response message; this should be enough to get your started with receiving the payload and doing something with it.

Change the function code within the `Run` method as follows (or you can clone my [GitHub-Webhook-Function](https://github.com/martinkearn/GitHub-Webhook-Function) sample from GitHub):

```c#
log.LogInformation("C# HTTP trigger function processed a GitHub webhook request.");

// get payload
string requestBody = new StreamReader(req.Body).ReadToEnd();
dynamic payload = JsonConvert.DeserializeObject(requestBody);

// get commit details required to call API
var commitId = payload.after;
var owner = payload.head_commit.author.username;
var repo = payload.repository.name;

// respond
return (ActionResult)new OkObjectResult($"We got data for {commitId} by {owner} on {repo}.");
```

You can now run your Function from Visual Studio; it will open a console and start an endpoint which looks like `http://localhost:7071/api/Function1`. You can test your Function by sending a Http POST request to this endpoint using something [Postman](https://www.getpostman.com/) or [cURL](https://curl.haxx.se/). 

Make a note of the Function's URL, you will need it when setting up NGROK in the next section.

## NGrok for local debug

Now we have a Function running locally, we need to debug it against a live GitHub webhook.

We can use [NGROK](https://ngrok.com/) to help us do this. NGROK is a tool which exposes local servers (our Function is effectively a server in this context) to a public internet endpoint via a secure tunnel.

To setup NGROK, follow these steps (taken in part from [GitHub Developer > Webhooks > Configuring Your Server](https://developer.github.com/webhooks/configuring/))

* Download and unzip NGROK from <https://ngrok.com/download>
* Open a console and navigate to the directory with the unzipped `ngrok.exe` in it
* Enter `ngrok http 7071` into your console (assuming that `7071` is the port your Function runs on)
* Make a note of the `Forwarding url`, which will be something like <http://0a929546.ngrok.io>

We now have a public NGROK endpoint which will forward onto our local server. The next step is to update the GitHub webhook to use the NGROK endpoint. Follow these steps:

* Go to Github > Repository > Settings > Webhooks > Edit the webhook you created earlier
* Set the `Payload URL` to be your NGROK URL with `/api/Function1` on the end. For example, <http://0a929546.ngrok.io/api/Function1>
* Update webhook

Now we get to test it all out. Follow these steps:

* You could optionally add a break point in your code if you want to, but you don't have to
* Run your Function locally
* Make sure your NGROK console is still running
* Add any file to the GitHub repository. You should see some status messages happening in your Function and NGROK console windows
* Go back to the webhook settings > Recent Deliveries section. You should see a green tick followed by a GUID. Click this and go to the `Response` tab.

* You should see that the tab title is `Response 200` which indicates you receive a `200/OK` response from the Function
* You should also see that the `Body` has details from the GitHub push event in it. It will look something like `We got data for dcd123a0b6b227093c68f6a81336a69062f18f19 by martinkearn on GitHub-Webhook-Function.`

You now have a local Function which is responding to an GitHub webhook via NGROK

## Publishing to Azure

The final step to make this real is to publish your Function to Azure. 

You can pick up when you left the [Azure Docs > Create your first function using Visual Studio](https://docs.microsoft.com/en-us/azure/azure-functions/functions-create-your-first-function-visual-studio) documentation earlier and follow the steps from the `Publish the project to Azure` section onwards (be sure to make a note of your `Site URL` as you'll need it next.

You no longer need NGROK now that the Function is on Azure so replace the `Payload URL` in GitHub Webhook settings with your Function's public `Site URL` which will be something like `https://martinkmefunctions.azurewebsites.net/api/Function1`. 

## In Summary

Webhooks offer a very powerful way to respond to events in applications, websites and services such as GitHub in a near real-time fashion. Webhooks are advantageous because responding to an event is much more efficient than polling an API on a timer or schedule.

Webhooks only work if they have an endpoint to POST their payload to and Azure Functions are an ideal way to create a quick, lightweight endpoint for processing Webhook payloads.

Going forward you may want to look into how Azure Logic apps could be used to receive a webhook payload and kick off a broader workflow of tasks to process the data.

Further reading and resources:

* My sample GitHub Webhook Function: <https://github.com/martinkearn/GitHub-Webhook-Function>
* Creating an Azure Function in Visual Studio: https://docs.microsoft.com/en-us/azure/azure-functions/functions-create-your-first-function-visual-studio
* NGROK: https://ngrok.com/
* GitHub webhook documentation: https://developer.github.com/webhooks/



