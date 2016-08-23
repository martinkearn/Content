
# Building with Text Analytics in ASP.NET Core 1.0
This demo will show how to create a simple web application that uses the Text Analytics API

### Pre Reqs
* Visual Studio 2015
* ASP.NET Core 1.0 RC1 or later
* Azure text Analytics API subscription: https://portal.azure.com/#create/Microsoft.CognitiveServices/apitype/TextAnalytics/pricingtier/S1

## Show Azure Marketplace

Go to https://portal.azure.com/#create/Microsoft.CognitiveServices/apitype/TextAnalytics/pricingtier/S1

Show interactive demo at https://text-analytics-demo.azurewebsites.net/

Get key from Cognitive Servcice Text Analytics > Keys > Key 1

## Create new Web Application
Visual Studio > File > New > Visual C# > Web > Asp.Net Core Web Application (.NET Core)

Open `Home Controller` and look at the `About()` action

Update the action signature to match
```
public async Task<IActionResult> About(string input = "This is just a little test, all on its own")
```

Declare local variables 
```
//local variables
string serviceBaseUri = "https://westus.api.cognitive.microsoft.com";
string accountKey = "48fa67ebf92e4d13abe6a4ddc0aa78d8";
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
httpClient.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", accountKey);
string requestString = "{\"documents\":[";
requestString += string.Format("{{\"id\":\"{0}\",\"text\":\"{1}\", \"language\":\"{2}\"}}", 1, input.Replace("\"", "'"), "en");
requestString += "]}";
byte[] byteData = Encoding.UTF8.GetBytes(requestString);
string fullRequestUri = "text/analytics/v2.0/sentiment";

```

Make the request with this code
```        
HttpResponseMessage response;
  using (var content = new ByteArrayContent(byteData))
  {
      content.Headers.ContentType = new MediaTypeHeaderValue("application/json");
      response = await httpClient.PostAsync(fullRequestUri, content);
  }
var responseContent = await response.Content.ReadAsStringAsync();
```

Parse the JSON repsonse and update view data
```
JObject d = JObject.Parse(responseContent);
var score = ((JValue)(d.SelectToken("documents[0].score"))).Value;
ViewData["Message"] = "\"" + input + "\" scores " + score.ToString(); 
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
    string serviceBaseUri = "https://westus.api.cognitive.microsoft.com";
    string accountKey = "48fa67ebf92e4d13abe6a4ddc0aa78d8";

    using (var httpClient = new HttpClient())
    {
        //setup HttpClient with auth
        httpClient.BaseAddress = new Uri(serviceBaseUri);
        httpClient.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", accountKey);
       
        // The content should be in the following format
        //                  {
        //                    "documents": [                  
        //                       {
        //                          "language": "en",                  
        //                          "id": "e",                  
        //                          "text": "You Text Here"
        //                      }
        //                     ]
        //                  }

        string requestString = "{\"documents\":[";
        requestString += string.Format("{{\"id\":\"{0}\",\"text\":\"{1}\", \"language\":\"{2}\"}}", 1, input.Replace("\"", "'"), "en");
        requestString += "]}";
        byte[] byteData = Encoding.UTF8.GetBytes(requestString);
        string fullRequestUri = "text/analytics/v2.0/sentiment";


        //make request
        HttpResponseMessage response;
        using (var content = new ByteArrayContent(byteData))
        {
            content.Headers.ContentType = new MediaTypeHeaderValue("application/json");
            response = await httpClient.PostAsync(fullRequestUri, content);
        }

        var responseContent = await response.Content.ReadAsStringAsync();
        //parse response and write to view
        JObject d = JObject.Parse(responseContent);
        var score = ((JValue)(d.SelectToken("documents[0].score"))).Value;
        ViewData["Message"] = "\"" + input + "\" scores " + score.ToString();              
      
    }

  return View();
}
```
