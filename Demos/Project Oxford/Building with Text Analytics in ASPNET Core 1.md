
# Building with Text Analytics in ASP.NET Core 1.0


File > New > Project > Visual C# > Web > ASP.NET Web Application > 'Web Application' project

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