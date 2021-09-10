---
title: Deploying .net to Azure App Service with Pulumi
author: Martin Kearn
description: Pulumi is an Infrastructure-as-code platform that facilitates deployment of cloud resource and application using standard coding languages such as c# or TypeScript. I used Pulumi to deploy a .net 6 Blazor app and Azure Function to Azure App Service; this is what I learnt.
image: https://github.com/martinkearn/Content/raw/master/Blogs/Images/Blocks.jpg
thumbnail: https://github.com/martinkearn/Content/raw/master/Blogs/Images/Blocks-Thumb.jpg
type: article
status: draft
published: 2021/09/10 14:00:00
categories: 
  - Blazor
  - Azure Functions
  - Pulumi
  - IaC
---

.[Pulumi](https://www.pulumi.com/) is an infrastructure-as-code (IaC) platform that facilitates deployment of cloud resources and applications using familiar programming languages such as C#, Typescript, Python and Go. Pulumi is probably the main current alternative to [Terraform](https://www.terraform.io/) for cloud deployment.

Having never touched Pulumi before, I spent a few days learning about it and seeing if I could deploy a .net 6 Blazor Server application and an Azure Function to Azure App Services (PaaS, not container) using C#. While the Pulumi examples/tutorials are usefull, I found that there was not anything that covered what I wanted to do, which is why I set about creating my own by patching various examples together.

The whole process was remarkably easy, but there were a few areas which I spent time researching so this article is designed to help anyone else who is new to Pulumi working with .net and Azure.

I'm not going to waste your time trying to explain how Pulumi works, the best way to get that information is simply to use the Pulumi "Getting Started" tutorials which you can find at [Get Started with Azure | Pulumi](https://www.pulumi.com/docs/get-started/azure/).

In addition to these simple tutorials, Pulumi provide a bunch of open-source examples on GitHub at (the C# Azure list) [examples/azure-cs-appservice at master · pulumi/examples (github.com)](https://github.com/pulumi/examples/tree/master/azure-cs-appservice).

## Deployment zip and WEBSITE_RUN_FROM_PACKAGE

## App Service Plan



## In summary

- [Pulumi's Azure Getting Started tutorial](https://www.pulumi.com/docs/get-started/azure/begin/)
- [Pulumi's Examples GitHub for Azure and C#](https://github.com/pulumi/examples#c-1)
- [martinkearn/Pulumi-Playpen: A playpen for experimenting and learning about Pulumi for Azure. (github.com)](https://github.com/martinkearn/Pulumi-Playpen)
-   More articles from me: http://martink.me/articles

