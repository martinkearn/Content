---
title: Blazor Form Submission and Navigation
author: Martin Kearn
description: Blazor has some specific behaviours with forms, form submission, validation and navigating away. In this article, we'll go through these behaviours and how to work with them.
image: https://github.com/martinkearn/Content/raw/master/Blogs/Images/forms.jpg
thumbnail: https://github.com/martinkearn/Content/raw/master/Blogs/Images/forms_thumb.jpg
type: article
status: draft
published: 2020/07/13 17:00:00
categories: 
  - Blazor
  - Forms
---

# Blazor Form Submission and Navigation

Blazor has some very rich capabilities with regard to the HTML `<form>`, how it validates data, handles submission and handles navigation. 

You can read about the general capabilities in [ASP.NET Core Blazor forms and validation](https://docs.microsoft.com/en-us/aspnet/core/blazor/forms-validation?view=aspnetcore-3.1).

There are few things that you need to be careful of when working with forms in Blazor, they are:

- In Blazor, when the `NavigationManager.NavigateTo` method is called, forms are submitted automatically. If you are not aware of this and have not setup your validation correctly, you could find half-submitted forms being sent to your back-end services.
- There is no default way to prevent the user from navigating before submission and asking if they are sure they want to discard the form's data. This could be a frustrating experience if the user has completed a very large form and navigates away expecting the data to be retained.

In this article, we'll explore solutions to the above issues as well as useful patterns for handling validation.

All the code in this article is from https://github.com/martinkearn/Blazor-Form-Submit-Navigation.

## EditContext and Form Validation

This is a recap of information that is covered elsewhere, specifically [ASP.NET Core Blazor forms and validation](https://docs.microsoft.com/en-us/aspnet/core/blazor/forms-validation?view=aspnetcore-3.1).

`EditContext` is an important feature of ASP.net forms which holds metadata related to a data editing process, such as flags to indicate which fields have been modified and the current set of validation messages. 



## Form Submission and Navigation



## In Summary

