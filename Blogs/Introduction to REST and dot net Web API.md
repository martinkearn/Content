---
title: Introduction to REST and .net Web API
author: Martin Kearn
description: An introduction to the concepts of REST and how to build REST APIs using net web api
image: https://dummyimage.com/800x600/000/fff&text=placeholder
thumbnail: https://dummyimage.com/200x200/000/fff&text=placeholder
type: article
status: published
published: 2015/01/05 09:30:00
categories: 
  - ASP.net
  - Web API
  - API
---

# Introduction to REST and .net Web API

If you are building apps or websites today, the chances are you’ll have heard of REST and .net Web API, but you may not be sure what these things are, whether you should use them or how to get started. If this sounds familiar, then this article for you.

## API; what and why?

Let’s start right at the start and figure out what an API is and why you should consider using one. The term API stands for ‘Application Programming Interface’. In the world of web development the term ‘API’ is synonymous with online web services which client apps can use to retrieve and update data. These online services have had several names/formats over the years such as SOAP, however the current popular choice is to create a REST (or RESTful) API. We’ll come onto REST and why REST is now preferred later, but first let’s examine why you would even bother with an API. Let’s consider a modern application which may include several mobile apps on various platforms and usually some kind of web application too. Without an API, a basic architecture may look like this where each client app has its own embedded business logic. 
[![](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/56/73/3225.NoAPIArchitecture.PNG)](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/56/73/3225.NoAPIArchitecture.PNG) 

Notice that each client app has its own embedded business logic (probably written in several different languages) which connects directly to the database to get, update and manipulate data. This local business logic means that the client apps can easily become complex and all need to be maintained in sync with each other. When a new feature is required, you’ll have to update each app accordingly. This can be a very expensive process which often leads to feature fragmentation, bugs and can really hold back innovation. Now let’s consider the same architecture with a central API which hold all the business logic. 

[![](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/56/73/2318.WithAPIArchitecture.PNG)](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/56/73/2318.WithAPIArchitecture.PNG) 

Each app uses the same API to get, update and manipulate data. All apps have feature parity and when you need to make a change you just make it in one place in line with the ‘Don’t Repeat Yourself’ (DRY) principle of software development. The apps themselves then become relatively lightweight UI layers. Happy Days

## What is REST?

REST stands for ‘Representational State Transfer’ and it is an architectural pattern for creating an API that uses HTTP as its underlying communication method. REST was originally conceived by Roy Fielding in his 2000 dissertation paper entitled ‘Architectural Styles and the Design of Network-based Software Architectures’, chapter 5 cover REST specifically: 
[http://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm](http://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm) 

Almost every device that is connected to the internet already uses HTTP; it is the base protocol that the internet is built on which is why it makes such a great platform for an API. HTTP is a request and response system; a calling client sends a request to an endpoint and the endpoint responds. The client and endpoint can be anything but a typical example is a browser accessing a web server or an app accessing and API. There are several key implementation details with HTTP that you should be aware of:

*   **Resources** – REST uses addressable resources to define the structure of the API. These are the URLs you use to get to pages on the web, for example ‘http://www.microsoft.com/Surface-Pro-3’ is a resource
*   **Request Verbs** – These describe what you want to do with the resource. A browser typically issues a GET verb to instruct the endpoint it wants to get data, however there are many other verbs available including things like POST, PUT and DELETE. See the full list at [http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html)
*   **Request Headers** – These are additional instructions that are sent with the request. These might define what type of response is required or authorisation details. See the full list at [http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html)
*   **Request Body** - Data that is sent with the request. For example a POST (creation of a new item) will required some data which is typically sent as the request body in the format of JSON or XML.
*   **Response Body** – This is the main body of the response. If the request was to a web server, this might be a full HTML page, if it was to an API, this might be a JSON or XML document.
*   **Response Status codes** – These codes are issues with the response and give the client details on the status of the request. See the full list at [www.w3.org/Protocols/rfc2616/rfc2616-sec10.html](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html)

In the context of a REST API, resources typically represent your data entities (i.e. ‘Product’, ‘Person’, ‘Order’ etc). The verb that is sent with the request informs the API what to do with the resource, for example a GET request gets data about an entity, POST requests create a new entity. There is a convention in place that GET requests to an entity url such as /Products returns a list of products, possibly matching some criteria that was sent with the request. However, to retrieve a specific product, you would use the product’s ID as part of the resource, for example /Products/81 would return product with the ID of 81\. It is also possible to use query string parameters with an API, for example you may have something like /Products?Colour=red which returns all red products. These are some typical requests you might expect to see in an ecommerce API:

<table>

<tbody>

<tr>

<td>**Resource**</td>

<td>**Verb**</td>

<td>**Expected Outcome**</td>

<td>**Response Code**</td>

</tr>

<tr>

<td>/Products</td>

<td>GET</td>

<td>A list of all products in the system</td>

<td>200/OK</td>

</tr>

<tr>

<td>/Products?Colour=red</td>

<td>GET</td>

<td>A list of all products in the system where the colour is red</td>

<td>200/OK</td>

</tr>

<tr>

<td>/Products</td>

<td>POST</td>

<td>Creation of a new product</td>

<td>201/Created</td>

</tr>

<tr>

<td>/Products/81</td>

<td>GET</td>

<td>Product with ID of 81</td>

<td>200/OK</td>

</tr>

<tr>

<td>/Products/881(a product ID which does not exist)</td>

<td>GET</td>

<td>Some error message</td>

<td>404/Not Found</td>

</tr>

<tr>

<td>/Products/81</td>

<td>PUT</td>

<td>An update to the product with an ID of 81</td>

<td>204/No Content</td>

</tr>

<tr>

<td>/Products/81</td>

<td>DELETE</td>

<td>Deletion of the product with an ID of 81</td>

<td>204/No Content</td>

</tr>

<tr>

<td>/Customers</td>

<td>GET</td>

<td>A list of all customers</td>

<td>200/OK</td>

</tr>

</tbody>

</table>

## What is .net Web API?

.Net’s Web API is an easy way to implement a RESTful web service using all of the goodness that the .net framework provides. Once you understand the basic principles of REST, then a .net Web API will be very easy to implement. Web API is built on .net’s modular, pluggable pipeline model. This means that when a server hosting a web API receives a request, it passes through .nets request pipeline first. This enables you to easily add your own modules if you find that the default capabilities are not enough for your needs. With the recent announcements on ASP.net vNext this also means you can potentially host your Web API outside of Windows Server which opens up a whole range of usage cases. See [http://www.asp.net/vnext](http://www.asp.net/vnext) for detail. Web API uses the Controller and Action concepts from MVC so if you already understand .net MVC you are in a good place. If you don’t, then Web API is a great way to learn MVC. Resources are mapped directly to controllers; you would typically have a different controller for each of your main data entities (Product, Person, Order etc). Web API uses the .net routing engine to map URLs to controllers. Typically, APIs are held within a ‘/api/’ route which helps to distinguish API controllers from other non-API in the same website. Actions are used to map to specific HTTP verbs, for example you would typically have a GET action which returns all of the entities. This action would respond to /api/Products (where ‘products’ is your controller) and would look something like this:

     public IEnumerable<string> Get()
     {
     return new string[] { "value1", "value2" };
     }

You may also have a GET action which accepts a specific ID and returns a specific entity. It would respond to /api/Products/81 and would look something like this:

     public string Get(int id)
     {
     return "value";
     }

There are many great hidden benefits to using Web API which you may not realise but actually save you a lot of work.

### Web API is part of ‘One ASP.net’

Web API is part of the ‘One ASP.net’ family which means that it natively supports all of the great shared features you may currently use with MVC or web forms, this includes (these are just a few examples):

*   Entity Framework
*   Authorisation and identity
*   Scaffolding
*   Routing

### Serialization and Model Binding

Web API is setup by default to provide responses in either XML or JSON (JSON is default). However, as a developer you do not need to do any conversion or parsing – you simply return a strongly typed object and Web API will convert it to XML or JSON and return it to the calling client, this is a process called Content Negotiation. This is an example of a GET action which returns a strongly typed Product object.

     public Product GetProduct(int id)
     {
     var product = _products.FirstOrDefault(p => p.ID == id);
     if (product == null)
     {
     throw new HttpResponseException(HttpStatusCode.NotFound);
     }
     return Request.CreateResponse(HttpStatusCode.OK, product);
     } 

This also works for incoming requests using a feature called Model Validation. With Model Validation, Web API is able to validate and parse incoming response body data to a strongly typed object for you to work with in your code. This is an example of model binding:

     public HttpResponseMessage Post(Product product)
     {
     if (ModelState.IsValid)
     {
     // Do something with the product (not shown).

return new HttpResponseMessage(HttpStatusCode.OK); } else { return Request.CreateErrorResponse(HttpStatusCode.BadRequest, ModelState); } }

### Dynamic API Help Pages

If you choose Web API as your project type, when you first create your project, you will get automated documentation with your API. This is a regular MVC project that sits alongside your API. The help pages project looks at your API code, models, attributes etc and constructs documentation automatically, which includes examples, model definitions and more. This documentation makes for a great resource to help developers of calling client understand the structure of your API. Help pages are contained within a route called ‘/help/’.

## In Summary

To summarise, use of an API will make the architecture of your application much cleaner, making it easier to add features and fix bugs as your project progresses. REST is an architectural pattern which is based on HTTP and uses HTTP requests, responses, verbs and status codes to communicate. The fact that REST services use HTTP means they can be consumed by almost any ‘online’ device or application (including IoT devices such as toasters, cars, pedometers etc) – no proprietary knowledge of the API is required. Web API in .net is the way you write REST APIs services in .net. Web API gives you all the benefits of the .net framework and deals with a lot of the complexities of content negotiation, model binding etc that you’d have to deal with yourself without Web API. To get started with Web API, I’d recommend you initially take a look at my 6 minute video which talks through the process of creating a very simple web API: [https://aka.ms/mk-filenewapivideo](https://aka.ms/mk-filenewapivideo) Then you should take a look on the official ASP.net sub site which has plenty of tutorials, examples and videos: [http://www.asp.net/web-api](http://www.asp.net/web-api)