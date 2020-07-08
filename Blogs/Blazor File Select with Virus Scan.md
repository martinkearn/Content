---
title: Blazor File Select with Virus Scan
author: Martin Kearn
description: Uploading a file is a very popular thing to do on the web, but what if it has a virus on it? How do you tell!? Here is how to do it using popular packages for Blazor, including a virus scan with ClamAV.
image: https://github.com/martinkearn/Content/raw/master/Blogs/Images/virus.jpg
thumbnail: https://github.com/martinkearn/Content/raw/master/Blogs/Images/virus_thumb.jpg
type: article
status: published
published: 2020/07/07 17:10:00
categories: 
  - Blazor
  - ClamAV
  - nClam
---

# Blazor File Select with Virus Scan

This article describes how to do a file upload with an antivirus scan using popular packages for Blazor, including a virus scan with nClam and ClamAV.

![Gif showing the solution described in this article.](https://github.com/martinkearn/Content/raw/master/Blogs/Images/Blazor-File-Select-with-Virus-Sc.gif)

The full code that accompanies this article can be found at https://github.com/martinkearn/Blazor-FileUpload-ClamAV

This solution really just stitches together several awesome third party open source solutions.

The file selector component is provided by a package called **BlazorInputFile**

- Overview blog: http://blog.stevensanderson.com/2019/09/13/blazor-inputfile/
- NuGet: https://www.nuget.org/packages/BlazorInputFile
- GitHub: https://github.com/SteveSandersonMS/BlazorInputFile

For virus scanning, I'm using the popular **ClamAV** platform via the **filefrog/ClamAV** docker container image

- Website: https://www.clamav.net/
- Docker container: https://hub.docker.com/r/filefrog/clamav

To access ClamAV from .net, I'm using **nClam**

- NuGet: https://www.nuget.org/packages/nClam
- GitHub: https://github.com/tekmaven/nClam

## Setting up a ClamAV Container

The simplest way to setup a ClamAV server is to use a docker container. 

There are many prebuilt containers available but I used [filefrog/clamav](https://hub.docker.com/r/filefrog/clamav) because it includes all the ClamAV services and is self-updating.

You can host the container in any way you see fit. I find that the simplest approach is to host using Azure Container Instances (ACI). 

These are the high level steps on how to do that.

1. Create resource > search for "container instances" > Create
2. Choose a subscription, resource group, container name and location.
   - Take note of the Container Name, you'll need it later
3. Choose `Docker Hub or other registry`
4. Set the Image Type and `Public`
5. Set `filefrog/clamav` as the Image
6. Accept all other defaults and click `Next: Networking` (do not create yet - we need to set some other settings)
7. Set the `DNS Label Name` to be what you put as the Container name in step 2.
   - Take note of the full FQDN here. The FQDN will be the DNS label plus the `something.azurecontainer.io` url specified automatically (this will vary based on the location). In my case it is `kearnclamav.uksouth.azurecontainer.io`
8. Add port `3310` as `TCP` (Clam AV works on 3310 by default)
9. Now click `Review + Create` > `Create`

## Blazor Project Setup

In this section, we'll look at the setup of the basic project.

1. Create a Blazor Server project using the standard template. You can follow this tutorial if you have never created a Blazor project before: https://docs.microsoft.com/en-us/aspnet/core/tutorials/build-a-blazor-app?view=aspnetcore-3.1
   - I believe all this should also work for Blazor Web Assembly but I've not tried it
2. Install and configure the `BlazorInputFile` NuGet package. 
   - You can follow the steps outlined in this blog article: http://blog.stevensanderson.com/2019/09/13/blazor-inputfile/
3. Install the `nClam` NuGet package: `Install-Package nClam`

## Using BlazorInputFile

When installed, BlazorInputFile is very easy to use; it is basically just a component which you can add to the mark-up section of the page where you want to use it:

```html
<InputFile OnChange="HandleFileSelected" />
```

You then need to implement the `HandleFileSelected` method which will get the file stream when a file is selected. For now, lets just add a basic handler; add this code block to the bottom of your Blazor page.

```c#
@code {
    IFileListEntry file;

    private void HandleFileSelected(IFileListEntry[] files)
    {
        file = files.FirstOrDefault();
    }
}
```

Whenever a file is selected in the BlazorInputFile control, the `file` object will have all the details including the file stream.

If you want to see a visual indication that you have selected a file, you can add this code to the mark-up section of the razor page:

```html
@if (file != null)
{
    <p>Name: @file.Name</p>
    <p>Size in bytes: @file.Size</p>
    <p>Last modified date: @file.LastModified.ToShortDateString()</p>
    <p>Content type (not always supplied by the browser): @file.Type</p>
}
```

When you select a file, you should instantly see all the details of the file on the page.

## Using nClam

The nClam library is very well designed and makes it very easy to simply send the `stream` from the BlazorInputFile to a ClamAV server to be scanned.

Firstly, we need to define the address for a ClamAV server. This is the FQDN of the container you deployed in the 'Setting up a ClamAV Container' section. You can get this from the Overview page of your Azure Container Instance. You can use IP address if your forgot to setup a FQDN but be aware that Azure IP address do change.

For speed, you can just hard-code the name if you want to, but it is best practice to use `appsettings.json` and the `IConfiguration` to access the setting like this:

```c#
@inject IConfiguration Configuration //This goes at the top of the file
var serverName = Configuration["ClamAVServerName"]; //This goes in the Code section
```

In any case, lets assume you have a variable called `serverName` which contains the FQDN or IP of the ClamAV container you setup earlier.

You can scan the file in two simple lines with nClam:

```c#
var clam = new ClamClient(serverName, 3310); //3310 hard coded because that is default for ClamAV
var scanResult = await clam.SendAndScanFileAsync(file.Data);
```

To do this in the context of the existing `HandleFileSelected` method, you'll need to make it `async` as follows:

```c#
private async Task HandleFileSelected(IFileListEntry[] files)
```

You can now write code that works with `scanResult.Result` to do things based on the scan result of the file. There are 4 enumerations which you can use a switch statement to work with, for example:

```c#
var scanStatus;
switch (scanResult.Result)
{
    case ClamScanResults.Clean:
        scanStatus = $"File is clean according to {serverName}";
        break;
    case ClamScanResults.VirusDetected:
        scanStatus = $"File is infected according to {serverName}";
        break;
    case ClamScanResults.Error:
        scanStatus = $"File scan produced an error from {serverName}, {scanResult.RawResult}";
        break;
    case ClamScanResults.Unknown:
        scanStatus = $"File scan produced an unknown status from {serverName}, {scanResult.RawResult}";
        break;
}
```

You can see the full code at https://github.com/martinkearn/Blazor-FileUpload-ClamAV/blob/master/src/Pages/Index.razor

## Testing for infected files with EICAR test file

One of the challenging aspects of an antivirus scan is how to test it; most people don't just have a virus-infected file hanging around on their desktop!

Luckily, the [European Institute for Computer Antivirus Research](https://en.wikipedia.org/wiki/EICAR_(Research_institute)) (EICAR) and [Computer Antivirus Research Organization](https://en.wikipedia.org/wiki/CARO) (CARO) have created a file that most virus scanning software will flag as a virus, however the file is actually harmless. You can read about this here: https://en.wikipedia.org/wiki/EICAR_test_file.

To use the EICAR test file, simply create a file called `EICAR.txt` (the name does not matter) and put this as the contents:

```
X5O!P%@AP[4\PZX54(P^)7CC)7}$EICAR-STANDARD-ANTIVIRUS-TEST-FILE!$H+H*
```

This will will be flagged by most antivirus systems (ClamAV included) as a virus. Just be careful that you store in somewhere on you computer that is not being scanned by your antivirus software.

## Azure VNET

There is a known issue with nClam when running on an Azure App Service within a VNET.

nClam uses `TcpClient` to establish a connection to the ClamACV server and does not define the IP address range.

On .net core 3.0 and above, `TcpClient` will default to IPv6 if no parameters are defined. This causes compatibility issues with many services, including but not limited to Azure Functions and Azure VNET integration which expect IPv4.

There is an open pull request on nClam to address this by forcing use of IPv4 for the address range. You can see the PR here: https://github.com/tekmaven/nClam/pull/39

At the time of wiring the pull request had not been accepted so if you run into errors such as "*An attempt was made to access a socket in a way forbidden by its access permissions*", then you may want to use the forked version of nClam instead at https://github.com/martinkearn/nClam.

## In Summary

[Blazor](https://dotnet.microsoft.com/apps/aspnet/web-apps/blazor) is a great framework for building rich web applications in .net and the [BlazorInputFile](https://github.com/SteveSandersonMS/BlazorInputFile) package is a great tool for doing almost anything you might need to do with file selection.

[ClamAV](https://www.clamav.net/) is a very well respected and popular open source antivirus system and [NClam](https://github.com/tekmaven/nClam) is a great way to access it in .net.

Using all these technologies in combination, you can built a simple, effective file selector with antivirus features.