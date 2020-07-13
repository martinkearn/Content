---
title: Blazor Checkbox Group
author: Martin Kearn
description: Imagine you need to ensure at least one checkbox is checked from a group of checkboxes. It does not matter which one, but it must be one of them. This is how to do it in Blazor.
image: https://github.com/martinkearn/Content/raw/master/Blogs/Images/ChecboxGroup.png
thumbnail: https://github.com/martinkearn/Content/raw/master/Blogs/Images/CheckboxGroup_thumb.png
type: article
status: published
published: 2020/07/09 17:00:00
categories: 
  - Blazor
  - Forms
---

# Blazor Checkbox Group

Sometimes in a HTML form you need to present a choice of options and you need the user to select at least one. It may not matter which one or if more than one is selected, but at least one choice is mandatory.

Examples could include toppings on a pizza or permissions for a user.

In Blazor, you can easily validate against whether a checkbox is checked or not (see [Blazor Forms and Validation](https://docs.microsoft.com/en-us/aspnet/core/blazor/forms-validation?view=aspnetcore-3.1)) but validating that one of many is checked is a little more involved.

I had this requirement for a project recently and this is how I figured it out.

You can see the code and a working sample for this article at https://github.com/martinkearn/Blazor-Checkbox-Group, all the code featured in this article is in [Index.razor](https://github.com/martinkearn/Blazor-Checkbox-Group/blob/master/src/Pages/Index.razor).

![A Blazor Checkbox group](https://github.com/martinkearn/Content/raw/master/Blogs/Images/Blazor-Checkbox-Group.gif)

## The code

This is the full code for the solution. The only other thing you'll need to is add to a standard Blazor template is to add the following to `_Imports.razor`

```
@using System.ComponentModel
@using System.ComponentModel.DataAnnotations
```

This is all in a razor page. In my [GitHub sample](https://github.com/martinkearn/Blazor-Checkbox-Group/blob/master/src/Pages/Index.razor) it is in `Index.razor` but could be anywhere.	

```c#
@page "/"
<h1>Blazor-Checkbox-Group</h1>
<EditForm EditContext="editContext" OnSubmit="HandleSubmit">
    <p>Make at least one choice...</p>
    <p>
        <label>
            Choice A:
            <InputCheckbox type="checkbox" id="choiceA" @bind-Value="model.ChoiceA" />
        </label>
    </p>
    <p>
        <label>
            Choice B:
            <InputCheckbox type="checkbox" id="choiceA" @bind-Value="model.ChoiceB" />
        </label>
    </p>
    <p>
        <label>
            Choice C:
            <InputCheckbox type="checkbox" id="choiceA" @bind-Value="model.ChoiceC" />
        </label>
    </p>
    <button type="submit">Submit</button>
    <DataAnnotationsValidator />
    <ValidationSummary />
</EditForm>

@if (formIsValid)
{
    <p>The form is valid, well done for checking at least one of the checkboxes.</p>
}

@code{
    ModelForForm model;
    EditContext editContext;
    bool formIsValid;

    protected override void OnInitialized()
    {
        model = new ModelForForm();
        editContext = new EditContext(model);
    }

    private void HandleSubmit()
    {
        var isValid = editContext.Validate();
        if (isValid)
        {
            // Set form is valid for the purposes of the sample, just to show that the form is valid.
            formIsValid = true;
        }
        else
        {
            return;
        }
    }

    private class ModelForForm
    {
        public bool ChoiceA { get; set; }

        public bool ChoiceB { get; set; }

        public bool ChoiceC { get; set; }

        [Required]
        [Range(typeof(bool), "true", "true", ErrorMessage = "At least one choice is required.")]
        public bool OneChoiceSelected
        {
            get
            {
                return ((ChoiceA) || (ChoiceB) || (ChoiceC));
            }
            set { }
        }
    }
}
```

## How it works

The key feature here is the `OneChoiceSelected` property. 

`OneChoiceSelected` is a property that represents the full group of checkboxes and is `Required`.

As well as being required, you can see that I have set a `Range` attribute which ensures the property's value is `true`. If it is not true the `ErrorMessage` will be shown and the model will fail validation.

The clever bit (at least it is clever for me) is that the `get` for `OneChoiceSelected` is:

```
get
{
	return ((ChoiceA) || (ChoiceB) || (ChoiceC));
};
```

This means that if any of the `bool` properties are `true` then  `OneChoiceSelected` is also `true`. If they are all `false`, then  `OneChoiceSelected` is also `false`.

The rest of the form is standard Blazor model binding and validation where each choice has its own `<InputCheckBox>`. 

See [Built-in forms components](https://docs.microsoft.com/en-us/aspnet/core/blazor/forms-validation?view=aspnetcore-3.1#built-in-forms-components) for details of how this works.

## In Summary

This is an elegant Blazor solution for ensuring that at least one of many checkboxes are checked.

The solution uses built-in validation, conventions and data annotations.