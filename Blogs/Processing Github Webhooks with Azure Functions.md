# Processing Github Webhooks with Azure Functions

I recently had a requirement where I needed to react to a push event on a GitHub repository and do some data processing on what was pushed. 

GitHub expose webhooks for a wide range of events so I looked into using Azure Functions and webhooks to respond to the GitHub push event.

This turned out to be very easy and simple so I wanted to share what I learnt.

The details of my application are not relevant for this article so I've simplified it to just focus on the basics of Webhooks with Azure Functions.

You can get the function code (all 6 lines of it) here <https://github.com/martinkearn/GitHub-Webhook-Function>

## What is a Webhook?

I'll admit to not being 100% sure what a webhook was before doing this research, but it is actually a very simple concept.

A webhook (sometimes called "web callback" or "reverse api") is where a website, service or application issues a HTTP request to some consuming endpoint in response to an event, usually posting data about the event to the consuming endpoint.

Webhooks enable the consuming endpoint to react to data in near real time without having to poll or run on a timer.

The example that most people are likely to be familiar with is the Continuous Deployment capabilities between GitHub and Azure which means that code is automatically deployed to Azure when a commit happens on GitHub. This functionality is built on the GitHub web hooks infrastructure.

## Configuring GitHub webhook

GitHub offer webhooks for all sorts of events that could take place in a repository. The [GitHub Developers Webhooks documentation](https://developer.github.com/webhooks/) is a decent read if you want to get all the details, but the main points/steps are

* You configure a webhook by going to Github > Repository > Settings > Webhooks > Add Webhook
* The `Payload URL` is the endpoint where you want to receive the webhook's payload. For this example, enter `http://localhost:7071/api/PushCatcher` (we'll change this later but is fine for now)
* The `Content type` is the format that payload data comes in. `application/json` is probably the best choice for most scenarios
* You can optionally define a secret to secure the webhook but we don't need that for this scenario
* You can also choose from a wide range of events that will trigger the webhook. We'll use the default `Just the push event`. You can see all the available events at [GitHub Developers > Event Types & Payloads](https://developer.github.com/v3/activity/events/types/)



Once you save the webhook, it is active by default. Whenever a `push` occurs on the repository, a JSON payload will be sent to the URL you defined in real-time.

You can click into the webhook and look at Recent Deliveries to see detail of each event, the payload data and the response.

Right now, this will return an 404 error because the payload endpoint has not been created, but we'll look at that in the next section.

## Creating the Function

## NGrok for local debug

## 

