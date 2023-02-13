---
title: Securing and securely calling Web API with Authorize
author: Martin Kearn
description: How to secure and securely calling Web API with .net Authorize header
image: https://dummyimage.com/800x600/000/fff&text=placeholder
thumbnail: https://dummyimage.com/200x200/000/fff&text=placeholder
type: article
status: published
published: 2015/03/25 09:40:00
categories: 
  - ASP.net
  - Web API
---

# Securing and securely calling Web API with Authorize

I recently had a requirement to secure a ASP.net Web API I was working on so that only authenticated users could make certain calls. It turns out this was fairly easy to do so I thought I'd share it. For the purposes of this article, I'm using Visual Studio 2013 with update 4\. I'm working with a project that has been created as a ASP.Net Web Application with the 'Web API' template and 'Individual User Accounts' enabled as the authentication option. I'm also using the [Chrome 'Postman REST Client' browser app](https://chrome.google.com/webstore/detail/postman-rest-client/fdmmgilgnpjigdojojpjoooidkmcomcm) to make calls into my API, but you could use any API testing tool such as Fiddler or even PowerShell.

## Securing your Web API with [Authorize]

Just like any ASP.net project, securing your Web API is very easy to do via the [[Authorize]](http://www.asp.net/web-api/overview/security/authentication-and-authorization-in-aspnet-web-api) attribute. By using [Authorize], you can restrict access to authenticated users, this means user that have usernames / passwords in .net's built-in identity system. If you are creating a new project, choose 'Web API' and have the authentication set to 'Individual User Accounts'. This means that a database will be used to store user details and you'll get an 'Account' controller which will facilitate logon, password change etc. You do not need to write any code to make this happen, it will all be created for you. You can secure an entire controller, or specific actions by adding the [Authorize] attribute above either the class (to secure the entire controller) or the specific action. This is an example showing the entire default 'Values' controller secured:

    namespace SecureWebAPI.Controllers
     {
     [Authorize]
     public class ValuesController : ApiController

This shows the default GET action secured with [Authorize]:

     // GET api/values
     [Authorize]
     public IEnumerable<string> Get()
     {
     return new string[] { "value1", "value2" };
     }

Simply using [Authorize] means that any authenticated user can call the controller/action, however if you want to specific named users or even roles you can do this:

    [Authorize(Users = "user@someemailaddress.co.uk","someuser","anyotheruser@email.com")]

    [Authorize(Roles = "Administrators")]

With the [Authorize] attribute in place, if an unauthenticated user attempts to call your API, they will receive a 401/Unauthorized response.

## Creating accounts via Web API

In order to access a secured API, you'll first need an account in the database. Fortunately, ASP.net has all the necessary API actions built-in so that you can register an account. If you created your project with the 'Web API' template you'll get the built in dynamic documentation pages at http://{your server name/domain name]/Help. These will show a set of calls available under the 'Account' controller for managing accounts, one of these is a POST to 'api/Account/Register' which accepts Email, Password and ConfirmPassword parameters. In JSON it looks like this:

    {
     "Email": "sample string 1",
     "Password": "sample string 2",
     "ConfirmPassword": "sample string 3"
     }

If you pass this data as a POST request to 'api/Account/Register', a new user account will be created. By default, the password requirements are quite strict, but these can be altered in \App_Start\IdentityConfig.cs. If the call succeeds, you'll receive a 200/OK response with no data attached. In PostMan, the request will look something like this: 

[![](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/56/73/0284.PostMan%20API%20Register%20Account.PNG)](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/56/73/0284.PostMan%20API%20Register%20Account.PNG)

## Securely calling your API: Step 1, Get an access token

Now that you have a secure API with at least one user account created, it is time to get to the meat of the article and that is securely accessing your API. To demonstrate this, I have scaffolded a 'Web API 2 Controller with actions, using Entity Framework''. I set a very simple 'Person' model class as the model (see below), created a new data context and accepted the default name of 'PeopleController'

     public class Person
     {
     [Key]
     public int Id { get; set; }

public string Name { get; set; }

public int Age { get; set; } }

The only change I've made to the default 'PeopleController' was to add the [Authorize] attribute above the class, this making all actions within the controller secure. To access secured API actions, you need to acquire a Token based on your username and password. This is very simple to do by sending a POST request to /Token (not '/api/Token', just '/Token') with the following parameters:

*   grant_type = password (just the word 'password')
*   userName = the username you registered before
*   password = the password you registered before

In PostMan, the request will look something like this (note, you'll need to send the params as 'x-www-form-urlencoded' for this to work): 

[![](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/56/73/6170.PostMan%20API%20Get%20access%20token.PNG)](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/56/73/6170.PostMan%20API%20Get%20access%20token.PNG) 

If the request is successful, you'll get a 200/OK response and some JSON data which includes a field called' access_token', this is the access token you'll need for the next step (see the screen shot above).

## Securely calling your API: Step 2, Make a call with an authorization header

Now you have your authorisation token, you can make a secure call into your API. You simply need to add an 'Authorization' header to your request and set the value to 'bearer {your authorisation token}'. In PostMan, it would look like this: [![](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/56/73/8780.PostMan%20API%20Secure%20call%20with%20bearer.PNG)](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/56/73/8780.PostMan%20API%20Secure%20call%20with%20bearer.PNG) 

If your call is successful, you should receive an appropriate 'success' status code with any data that the API returns. In the case of a GET request as shown above,this is typically 200/OK.

## How to make a secure API call in .net

If you are reading this, the chances are, you are a .net developer so you'll probably want to know how to do all of this in .net. I've set-up a simple console application that goes through the whole process. To make a call to any http API in a console application, you'll need 'Microsoft.Net.Http' which you can get by running this in the package manager console.

    Install-Package Microsoft.Net.Http 

In this example, I'm also using JSON .net which you can get by running this in the package manager console.

    Install-Package Newtonsoft.Json

If you have both of these packages installed, you should be able to resolve all missing references in the code. The full code looks like this, clearly the constant variables at the top should be changed to suit your environment. The code for this entire project (The API and test client is in my GIT repository here: [https://github.com/martinkearn/SecureWebAPIExample](https://github.com/martinkearn/SecureWebAPIExample))

```
    using Newtonsoft.Json.Linq;
    using System;
    using System.Collections.Generic;
    using System.Net.Http;
    using System.Net.Http.Headers;
    using System.Threading.Tasks;

    namespace Client { class Program { const string userName = "someuser@someemail.com"; const string password = "Password1!"; const string apiBaseUri = "http://localhost:18342"; const string apiGetPeoplePath = "/api/people";

    static void Main(string[] args) { //Get the token var token = GetAPIToken(userName, password, apiBaseUri).Result; Console.WriteLine("Token: {0}", token);

    //Make the call var response = GetRequest(token, apiBaseUri, apiGetPeoplePath).Result; Console.WriteLine("response: {0}", response);

    //wait for key press to exit Console.ReadKey(); }

    private static async Task<string> GetAPIToken(string userName, string password, string apiBaseUri) { using (var client = new HttpClient()) { //setup client client.BaseAddress = new Uri(apiBaseUri); client.DefaultRequestHeaders.Accept.Clear(); client.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));

    //setup login data var formContent = new FormUrlEncodedContent(new[] { new KeyValuePair<string, string>("grant_type", "password"), new KeyValuePair<string, string>("username", userName), new KeyValuePair<string, string>("password", password), });

    //send request HttpResponseMessage responseMessage = await client.PostAsync("/Token", formContent);

    //get access token from response body var responseJson = await responseMessage.Content.ReadAsStringAsync(); var jObject = JObject.Parse(responseJson); return jObject.GetValue("access_token").ToString(); } }

    static async Task<string> GetRequest(string token, string apiBaseUri, string requestPath) { using (var client = new HttpClient()) { //setup client client.BaseAddress = new Uri(apiBaseUri); client.DefaultRequestHeaders.Accept.Clear(); client.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json")); client.DefaultRequestHeaders.Add("Authorization", "Bearer " + token);

    //make request HttpResponseMessage response = await client.GetAsync(requestPath); var responseString = await response.Content.ReadAsStringAsync(); return responseString; } } } }
```

## In summary

It is actually very simple to secure your Web API and securely access it in .net code. The source code that goes with this article is in my GIT repository here: [https://github.com/martinkearn/SecureWebAPIExample](https://github.com/martinkearn/SecureWebAPIExample)
