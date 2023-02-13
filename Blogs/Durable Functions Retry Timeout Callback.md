---
title: Durable Functions with Retry Timeout and Callback
author: Martin Kearn
description: How to use Durable Functions to call external systems with retry and timeout loops, including waiting for the external system to call back and rehydrate the function.
image: https://raw.githubusercontent.com/martinkearn/Content/master/Blogs/Images/Retry.jpg
thumbnail: https://raw.githubusercontent.com/martinkearn/Content/master/Blogs/Images/Retry_thumb.jpg
type: article
status: published
published: 2021/09/08 14:40:00
categories: 
  - Durable Functions
  - Azure Functions
  - Serverless
---

.Azure Durable Functions are a platform for building code-based workflows built on top of the Azure Functions runtime.

Like any workflow system, it is common for Durable Functions to need to call external systems and handle retry and timeout scenarios. It is also common for the external systems to carry out long running tasks which need to call back to the Durable Function when they are done.

In this article, we'll discuss patterns and examples for achieving all of these features with Durable Functions, based on a real-world customer scenario.

The article assume a working knowledge of Durable Functions; if you don't already have that, you may benefit from reading [Durable Functions Overview](https://docs.microsoft.com/en-us/azure/azure-functions/durable/durable-functions-overview?tabs=csharp) first.

There is an open-source companion repository that goes with this article available at GitHub:  [Durable-Function-Retry-Timeout-Callback](https://github.com/martinkearn/Durable-Function-Retry-Timeout-Callback)

## The scenario

The customer scenario that inspired this article was a Durable Function which needed to execute jobs on [Azure Databricks](https://docs.microsoft.com/en-us/azure/databricks/). Because of the long running nature of the Databricks jobs, the system was designed so that Databricks itself calls back to the Durable Function when the job has completed rather than the Durable Function polling Databricks for status.

The customer wanted the Durable Function system to work with a set of configuration values for each job that defined:

- **Timeout limit**: A setting in seconds that defines how long should be given between the job being sent to Databricks and the call-back arriving back at the Durable Function.
- **Retry limit**: A setting that defines how many times a job should be retried if it either timeout or has an error when initially requested

Durable Functions are very well suited to these kinds of requirements because Durable Function allows for long-running operations (see [Human interaction and timeouts in Durable Functions](https://docs.microsoft.com/en-us/azure/azure-functions/durable/durable-functions-phone-verification?tabs=csharp)). Durable Functions also allows complex retry logic to be coded using standard code conventions such as `do/while` loops and `try/catch` blocks.

Though the example given is based on Databricks, it can apply to any external system that has long-running operations.

## Call-back and timeout

Durable Functions has a great way to call external systems and wait for a call-back, this is called "wait for external event" and is explained in details at [Handling external events in Durable Functions](https://docs.microsoft.com/en-us/azure/azure-functions/durable/durable-functions-external-events?tabs=csharp).

The basic gist is that an orchestration function can use the `WaitForExternalEvent` method of the function context to tell the function to wait until an external request is received on a specific endpoint.

#### Getting the call-back endpoint

The endpoint itself is a combination of the function's host, the instance ID and the event name and is visible in the response body's `SendEventPostUri` when invoking an orchestration via HTTP, this is an example:

`http://localhost:7071/runtime/webhooks/durabletask/instances/3f733347b1cd44d1af699e8993a68b7f/raiseEvent/{eventName}?taskHub=TestHubName&connection=Storage&code=MahkwEx2o9sEZKMoDAG8dfWihFNm6Pa1DxdcNuQRlXr7PivLT/9rlA==`

It is possible to obtain this url, complete with the task hub, function code (auth key) and other details from any client function by using the `IDurableOrchestrationClient.CreateHttpManagementPayload` method. This method requires an instance Id but you can pass any value which can be swapped out later (clients will not have the orchestration's instance Id until they invoke the orchestration which is why a temporary value is required). 

The method will produce a url with `{eventname}` where the external event name should go, but you can easily swap this for your real event name using string manipulation.

You can then pass this value through to the orchestration as part of its payload. The orchestration can replace the temporary instance Id and event name and then you have the full call-back uri which can be passed to your external system.

See the [`HttpStartClient.cs`](https://github.com/martinkearn/Durable-Function-Retry-Timeout-Callback/blob/main/RetryTimeoutCallback/FunctionApp/Clients/HttpStartClient.cs) from this articles associated sample repo for an example of how this is done.

#### Wait for external event with timeout

Once the external system has been triggered (with the call back uri as a parameter), the function can wait for call-back by using this code

````c#
var callbackTimeSpan = new TimeSpan(0, 0, 180);
var callBackSuccess = await context.WaitForExternalEvent<bool>("callbackEventname", callbackTimeSpan);
````

This will make the function wait for a request to the call-back uri until the `callbackTimeSpan` has passed.

If the time span does pass (i.e. the external system does not call back in time), the function will throw a `TimeoutException` which you can handle using something like a `try/catch` block.

Notice that the result of the `WaitForExternalEvent` can be typed so that a specific object is expected with the call-back. In this simple example, we just look for a Boolean which is stored in the `callBackSuccess` variable.

## Durable Entities for recording status

While the approach outlined in the "Call-back and timeout" section works well for coordinating the call-back and timeout, it can be difficult to observe what is happening which makes debugging and testing difficult.

To resolve this, I used [Durable Entities](https://docs.microsoft.com/en-us/azure/azure-functions/durable/durable-functions-entities?tabs=csharp) to maintain the state of each attempt that was being made on the external system as well as the overall state for the workflow.

You can see the [AttemptsEntity.cs](https://github.com/martinkearn/Durable-Function-Retry-Timeout-Callback/blob/main/RetryTimeoutCallback/FunctionApp/Entities/AttemptsEntity.cs) to see exactly how the entity works, but the basic gist is that it maintains a `Dictionary<Guid, Attempt> Attempts { get; set; }` which can be used to record information and state about each "Attempt".

The orchestration can update this state as it goes along by using an entity proxy and something like the following code (in this case, adding a new `Attempt` and updating the status of it):

```c#
var entityId = new EntityId(nameof(AttemptsEntity), context.InstanceId);
var entity = context.CreateEntityProxy<IAttemptsEntity>(entityId);
var thisAttemptId = context.NewGuid();
entity.AddAttempt(thisAttemptId);
entity.UpdateAttemptState(KeyValuePair.Create(thisAttemptId, AttemptState.Executing));
```

#### Observing the attempt status with a client function

To make this Durable Entity information observable, we can use a http client function which can be used by tools like Postman to see the exact status of the attempts loop at any time, see [HttpAttemptCounterEntityClient.cs ](https://github.com/martinkearn/Durable-Function-Retry-Timeout-Callback/blob/main/RetryTimeoutCallback/FunctionApp/Clients/HttpAttemptCounterEntityClient.cs).

This allows us to see status at any time. In this example we see one attempt which timed out and a second which succeeded because it received a call-back from the external system.

```json
{
    "attempts": {
        "2f4e8f16-2f8f-5d50-88e4-5814ef4352ab": {
            "id": "2f4e8f16-2f8f-5d50-88e4-5814ef4352ab",
            "started": "2021-09-07T14:25:22.0514048Z",
            "stateSet": "2021-09-07T14:26:52.0984539Z",
            "timeoutDue": "2021-09-07T14:26:26.9336796Z",
            "state": "TimedOut",
            "message": "Event Callback not received in 00:01:00",
            "isSuccess": false
        },
        "c4d496cc-c4e4-5f9c-b513-85e8cce55eed": {
            "id": "c4d496cc-c4e4-5f9c-b513-85e8cce55eed",
            "started": "2021-09-07T14:26:52.6610989Z",
            "stateSet": "2021-09-07T14:26:59.3755181Z",
            "timeoutDue": "2021-09-07T14:27:56.6890218Z",
            "state": "CallbackSuccess",
            "message": "External system called back with CallbackSuccess",
            "isSuccess": true
        }
    },
    "overallstate": "Completed API request after 2 attempts. Final state CallbackSuccess, status text: External system called back with CallbackSuccess"
}
```

## In summary

Durable Functions are a very powerful platform which allow you to produce complex looping, retry and timeout logic using regular code constructs.

In this article and associated sample you can see how a Durable Function can call an external system, pass a call-back uri and wait for the external system to call-back, governed by timeout and retry limits.

- Wait for external event: https://docs.microsoft.com/en-us/azure/azure-functions/durable/durable-functions-external-events
- Durable Entities: https://docs.microsoft.com/en-us/azure/azure-functions/durable/durable-functions-entities
- GitHub repo: [github.com/martinkearn/Durable-Function-Retry-Timeout-Callback](https://github.com/martinkearn/Durable-Function-Retry-Timeout-Callback)
-   More articles from me: http://martink.me/articles

