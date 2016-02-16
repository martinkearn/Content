
# Building with Text Analytics in ASP.NET Core 1.0
This demo will show how to create a simple web application that uses the Text Analytics API

### Pre Reqs
* Visual Studio 2015
* ASP.NET Core 1.0 RC1 or later
* Azure text Analytics API subscription: https://datamarket.azure.com/dataset/amla/text-analytics

## Show Azure Marketplace

Go to https://datamarket.azure.com/dataset/amla/text-analytics

Show interactive demo at https://text-analytics-demo.azurewebsites.net/

Get key from My Account > Account Keys

## Create new Web Application
Visual Studio > File > New > Visual C# > Web > Web Application > ASP.NET 5 > Web Application

Open `Home Controller` and look at the `About()` action

Update the action signature to match
```
public async Task<IActionResult> About(string input = "This is just a little test, all on its own")
```

Declare local variables 
```
//local variables
string serviceBaseUri = "https://api.datamarket.azure.com/";
string accountKey = "vAK12LdYcZ7mPBqeCuLTVkAPHAVz0l5fJWlVim5THBs=";
```

## Setup the HttpCLient

Add the HTTPClient 
```
using (var httpClient = new HttpClient())
{
}
```

Add `System.Net.HttpClient` package using the lightbulb menu

Add these usings using the lightbulb menu
* `Using System.Text`
* `using System.Net.Http.Headers;`


Copy the HttpClient setup code and resolve the folowng packages
```
//setup HttpClient with auth
httpClient.BaseAddress = new Uri(serviceBaseUri);
string creds = "AccountKey:" + accountKey;
string authorizationHeader = "Basic " + Convert.ToBase64String(Encoding.ASCII.GetBytes(creds));
httpClient.DefaultRequestHeaders.Add("Authorization", authorizationHeader);
httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
string fullRequestUri = "data.ashx/amla/text-analytics/v1/GetSentiment?Text=" + input;
var response = await httpClient.GetAsync(fullRequestUri);
```

Make the request with this code
```        
//make request
var responseContent = await response.Content.ReadAsStringAsync();
```

Parse the JSON repsonse and update view data
```
//parse response and write to view
dynamic d = JObject.Parse(responseContent);
ViewData["Message"] = "\"" +input + "\" scores " +d.Score;
```
  
Add `Newtonsoft.Json` package using the lightbulb menu

## Run the application
Go to the about page and show the default text

Add something via the query string, for example `http://localhost:1061/Home/About?Input=Glasgow is really a great city, i love it`

## Snippets

```
public async Task<IActionResult> About(string input = "This is just a little test, all on its own")
{
    //local variables
    string serviceBaseUri = "https://api.datamarket.azure.com/";
    string accountKey = "vAK12LdYcZ7mPBqeCuLTVkAPHAVz0l5fJWlVim5THBs=";

    using (var httpClient = new HttpClient())
    {
        //setup HttpClient with auth
        httpClient.BaseAddress = new Uri(serviceBaseUri);
        string creds = "AccountKey:" + accountKey;
        string authorizationHeader = "Basic " + Convert.ToBase64String(Encoding.ASCII.GetBytes(creds));
        httpClient.DefaultRequestHeaders.Add("Authorization", authorizationHeader);
        httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
        string fullRequestUri = "data.ashx/amla/text-analytics/v1/GetSentiment?Text=" + input;
        var response = await httpClient.GetAsync(fullRequestUri);

        //make request
        var responseContent = await response.Content.ReadAsStringAsync();

        //parse response and write to view
        dynamic d = JObject.Parse(responseContent);
        ViewData["Message"] = "\"" +input + "\" scores " +d.Score;
    }

    return View();
}
```