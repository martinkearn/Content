---
title: Blazor Real-time Events
author: Martin Kearn
description: Blazor can subscribe to events in a .net service and refresh the UI in real-time ... without JavaScript or SignalR. This article covers how from both a .net service and Blazor app perspective. This is the future of the web.
image: https://github.com/martinkearn/Content/raw/master/Blogs/Images/RealtimeEvents.jpg
thumbnail: https://github.com/martinkearn/Content/raw/master/Blogs/Images/RealtimeEvents_thumb.jpg
type: article
status: published
published: 2020/07/10 15:00:00
categories: 
  - Blazor
  - SignalR
---

.net has had the ability to handle and raise events for several years now. Events allows a service to inform subscribing code when something happens in the service, allowing the subscribing code to take action.

You can read about the basic of [handling and raising events in .net](https://docs.microsoft.com/en-us/dotnet/standard/events/) if you are not familiar with the `event` keyword.

Events are very useful for applications which need to update in real-time. [SignalR](https://docs.microsoft.com/en-us/aspnet/core/signalr/introduction?view=aspnetcore-3.1) is often used to add real-time functionality to applications. Blazor Server is built on top of SignalR and takes it a step further by abstracting the need to create a SignalR hub and client code.

Blazor can subscribe and respond directly to .net service events through dependency injection with just C#, **no need for JavaScript or SignalR code** (though both technologies are being used under-the-hood). 

This article will talk through how simple this is. 

All the code for this article is here: https://github.com/martinkearn/Blazor-Realtime-Events

## The Counter Scenario

This article will use a very simple scenario of a .net service which maintains a number counter. Client code can add to the counter and get the current counter. It is a deliberately simple scenario to show how real-time in Blazor works.

The data is stored in server memory for simplicity, but could be easily backed by some kind of data store or cache.

The client application has two pages:

- Counter viewer: Shows the current counter
- Add to counter: Adds a random number to the current counter

This GIF shows each page in a separate browser; you can see the counter increasing in real-time in response to the button click events on the other browser. 

This is done with just C# and Blazor; no written JavaScript or SignalR code (both technologies are in play under-the-hood).

![A video showing the application working](https://github.com/martinkearn/Content/raw/master/Blogs/Images/Blazor-Realtime-Events.gif)

## The CounterService

The counter service is a very typical .net standard class library with the following interface:

```c#
public interface ICounterService
{
    int Get();

    int Add(int numberToAdd);

    event EventHandler<int> CounterChanged;
}
```

The concrete implementation of `ICounterService` is as follows:

```c#
public class CounterService : ICounterService
{
    private int counter = 0;

    public int Get()
    {
        return counter;
    }

    public int Add(int numberToAdd) 
    {
        counter += numberToAdd;
        CounterChanged?.Invoke(this, counter);
        return counter;
    }

    public event EventHandler<int> CounterChanged;
}
```

The interesting bit here is `CounterChanged?.Invoke(this, counter)`.

This line makes sure that whenever `counter` is updated, the `CounterChanged` event handler is invoked and the newly updated `counter` is passed as event data. Any code that subscribes to this event will receive the new `counter` whenever it is updated. 

In this example, `counter` is a simple `int` but could be any .net object, including custom types.

It is not shown in the [sample](https://github.com/martinkearn/Blazor-Realtime-Events), but in theory, the service could be fronted by an API and multiple applications could be calling into it and updating the counter.

The code for the counter service is here: https://github.com/martinkearn/Blazor-Realtime-Events/blob/master/src/BlazorServerRealtime.Services/CounterService.cs.

## Blazor & Dependency Injected Services

One of the really cool things about Blazor apps is that you can use standard .net dependency injection to inject services directly into your Blazor application. This means that front-end code can directly access the service.

To do this, we first need to add the service to the `startup.cs` for the Blazor application. 

Add this to the `ConfigureServices` method in `startup.cs` just like you would normally for .net DI.

```c#
services.AddSingleton<ICounterService, CounterService>();
```

> NOTE: In the [sample project](https://github.com/martinkearn/Blazor-Realtime-Events/tree/master/src), the .Services and .Application (Blazor) projects are separate projects in the same solution. It is not required, but I think this better illustrates that the service is just a .net standard class library and nothing to do with Blazor. In order for the .Application project to use the .Service for DI, there must be a project reference to the .Service project.

Now that you have the service dependency injected into your Blazor project, you can simply use it in any Blazor page or component by adding this to the top of the file:

```c#
@inject ICounterService CounterService
```

This means that in your Blazor code you can do things like this in the `code` section: 

```c#
var counter = CounterService.Get();
```

And this in the mark-up:

```html
<p>Counter: <strong>@counter</strong></p>
```

The full code for `startup.cs` is here (most of it from the default Blazor template): https://github.com/martinkearn/Blazor-Realtime-Events/blob/master/src/BlazorServerRealtime.Application/Startup.cs

## Subscribing to the event in Blazor

Now we have our service dependency injected into the Blazor, app we can simply wire up the event to the Blazor page.

The first step is to setup the event handler in the Blazor `OnInitialized` method. This is what runs when the page is first initialised by the server (see [ASP.NET Core Blazor lifecycle](https://docs.microsoft.com/en-us/aspnet/core/blazor/components/lifecycle?view=aspnetcore-3.1)).

Add this line to the `OnInitialized` method.

```c#
CounterService.CounterChanged += CounterChanged;
```

We now need to implement `CounterChanged`, which is as follows:

```c#
int counter; // This is a private var which would typically be at the top of the code section. Shown here for completeness.
private async void CounterChanged(object sender, int newCounter)
{
    await InvokeAsync(() =>
    {
        // Set the local counter variable
        counter = newCounter;

        // Tell Blazor to rewrite the DOM
        StateHasChanged();
    });
}
```

This is a fairly standard event handler. The key bit is that the event data is passed through as `newCounter`. We use this value to set a private variable in the Blazor page called `counter` which drives the user interface using the standard Blazor syntax (i.e.. `<p>Counter: @counter</p>`).

The last element is that you need to make sure `CounterChanged` calls `StateHasChanged()` because that tells the server to re-write the DOM which updates the screen for the user (see [State changes](https://docs.microsoft.com/en-us/aspnet/core/blazor/components/lifecycle?view=aspnetcore-3.1#state-changes)).

The page where this all happens is here: https://github.com/martinkearn/Blazor-Realtime-Events/blob/master/src/BlazorServerRealtime.Application/Pages/Index.razor

## In Summary

Due to ever increasing popularity of mobile apps, real-time web applications are increasingly becoming the 'norm'. 

It has traditionally been complex to build real-time systems requiring multiple technologies and programming languages. SignalR made this a bit easier for .net developers but there was still complexity and the need to learn about SignalR and how it works.

Using the approach of .net events, dependency injection and Blazor, it is possible to create powerful real-time applications with *just* C# and programming patterns you are already familiar with.

This is the future of the web.
