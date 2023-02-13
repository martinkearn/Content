---
title: Discarding forms in Blazor
author: Martin Kearn
description: This article will discuss how to check what a user wants to do with an unsubmitted form if they attempt to navigate away in Blazor.
image: https://github.com/martinkearn/Content/raw/master/Blogs/Images/forms.jpg
thumbnail: https://github.com/martinkearn/Content/raw/master/Blogs/Images/forms_thumb.jpg
type: article
status: draft
published: 2020/07/13 17:40:00
categories: 
  - Blazor
  - Forms
---

.Blazor has some very rich capabilities with regard to the HTML `<form>`, how it validates data and handles submission.

You can read about the general capabilities in [ASP.NET Core Blazor forms and validation](https://docs.microsoft.com/en-us/aspnet/core/blazor/forms-validation?view=aspnetcore-3.1).

Despite the goodness, there is no default way check what a user wants to do with unsubmitted forms before navigating away.

Typically, you'd expect to see an "are you sure" message giving the user the choice to opt-in to discarding the form or going back to submit it. This could be a frustrating experience if the user has completed a very large form and navigates away expecting the data to be retained.

In this article, we'll explore a solution to the above issue as well as useful patterns for handling validation.

All the code in this article is from https://github.com/martinkearn/Blazor-Form-Submit-Navigation.

## EditContext and Form Validation

This is a recap of information that is covered elsewhere, specifically [ASP.NET Core Blazor forms and validation](https://docs.microsoft.com/en-us/aspnet/core/blazor/forms-validation?view=aspnetcore-3.1).

`EditContext` is an important feature of ASP.net forms which holds metadata related to a data editing process, such as flags to indicate which fields have been modified and the current set of validation messages. 



## Form Submission and Navigation



## In Summary

