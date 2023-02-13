---
title: Using OData query syntax with Web API
author: Martin Kearn
description: How to use OData query syntax with Web API
image: https://dummyimage.com/800x600/000/fff&text=placeholder
thumbnail: https://dummyimage.com/200x200/000/fff&text=placeholder
type: article
status: published
published: 2015/03/10 09:40:00
categories: 
  - Web API
  - ODATA Query Syntax
  - ASP.net
---

# Using OData query syntax with Web API

I was just messing around with a Web API and thought _"I had better add some kind of filtering support here"_. I was vaguely aware of OData and knew that OData was the way to do filtering, sorting etc with REST APIs (.net Web API in my case), but had never looked into it.

When I did look into it, I have to say ... it blew my mind! It is soo easy to do and gives you so much power over your data. 

In the spirit of trying to 'learn in public', I've captured the main details here.

### What is OData query syntax?

OData query syntax is a standard way of querying RESTful APIs using a pre-defined syntax that allows the calling client to define the sort order, filter parameters and pages of data that is returned.

The full syntax can be found in [section 5 of the OData Version 4.0 Part 2 specification](http://docs.oasis-open.org/odata/odata/v4.0/errata02/os/complete/part2-url-conventions/odata-v4.0-errata02-os-part2-url-conventions-complete.html#_Toc406398092) but here are some quick examples of how the syntax is used:

*   **http://host/service/Products?$filter=Name eq 'Milk'**: Returns all products where the name is equal to 'Milk'
*   **http://host/service/Products?$filter=Name eq 'Milk' and Price lt 2.55**: Returns all products where the name is equal to 'Milk' and the price is less than 2.55
*   **http://host/service/Products?$top=10&$skip10**: Returns items 11>21\. The $top system query option requests the number of items in the queried collection to be included in the result. The $skip query option requests the number of items in the queried collection that are to be skipped and not included in the result
*   **http://host/service/Products?$orderby=Name desc,Id**: Returns all products ordered by the Name in descending order, then ordered by Id
*   **http://host/service/Products?$filter=Enabled eq true**: Returns all products where the Enabled field (which is a boolean) is true
*   **http://host/service/Products?$filter=substringof('Martin', Name)**: Returns all products the Name field contains the text Martin

### How to use OData query syntax in ASP.net Web API?

This is where things get really awesome - it is super, super easy to change a regular Web API controller to support OData query syntax - once you know how to do this, you'll never not do it!

If we assume we are starting from a regular Web API project (File > New Project > ASP.net Web Application > Web API), you first need to add a scaffolded controller which you can do by following these steps:

1.  Add a model class (I'm using the typical 'Person' class with Id, First Name, Last Name, Age etc)
2.  Right-click the 'controllers' folder > Add > New Scaffolded item > Web API 2 Controller with actions, using Entity Framework > Model = Person

NOTE: You do not need to choose the 'Web API 2 OData Controller...' option, you can add the query syntax to any controller

Once you've setup your controller, you should end up with a simple controller which has a default GET action that looks a little like this:

    // GET: api/Peoplepublic IQueryable<Person> GetPeople(){return db.People;}

This action will simply return all the rows in the 'People' table of the database with no ability to filter, sort etc. To add full OData query support, you need to make a few changes:

1.  Add the Microsoft.Aspnet.OData package via nugget ... simply pop this into you 'Package Manager Console': `Install-Package Microsoft.AspNet.Odata`
2.  Add this using statement: `using System.Web.OData;`
3.  Add the`[EnableQueryAttribute]`attribute to your GetPeople action.
4.  Add `AsQueryable();` to your return line so it looks like this: `return db.People.AsQueryable();`

Your finished code will look something like this:

    // GET: api/People[EnableQueryAttribute]public IQueryable<Person> GetPeople(){return db.People.AsQueryable();}

That is all there is to it! .... now you have this setup, you can use the full OData syntax to query sort etc without any extra code.

Use the best online[HTML, CSS and JS tools](https://html-css-js.com/) at html-css-js.com: editors, code optimizers and more.
