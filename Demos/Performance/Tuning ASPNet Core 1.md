
# Tuning ASP.Net Core 1.0.0
This demo takes a default ASP.Net Core 1.0.0 web application and tunes it for better performance.

### Pre Reqs
* ASP.NET Core 1.0.0
* Chrome
* YSlow Chrome extension (with localhost removed from CDNs)

## Create New & Test
File > New Project > Visual C# > Web > ASP.NET Core Web Application (.NET Core) > Web Application

Run in Chrome

Scan site in YSlow

Add `localhost` as CDN to increase score to 91
* That was easy! :)

_NOTE: Some issues are not showing up because we are on Localhost, so we'll scan an exact duplicate that is alreday hosted on Azure_

Open http://netcoreperformancebefore.azurewebsites.net/ in Chrome

Scan in YSlow
* Add CDN again
* Grade D on Configure entity tags (ETags)
* Grade C on Use cookie-free domains

## Expire Headers
Examine report in YSlow
* Grade F on Add Expires headers. There are 7 static components without a far-future expiration date.

Open Startup.cs

Change `app.UseStaticFiles();` to
```
app.UseStaticFiles(new StaticFileOptions()
{
    OnPrepareResponse = r => r.Context.Response.Headers.Add("Expires", DateTime.Now.AddDays(7).ToUniversalTime().ToString("r"))
});
```
