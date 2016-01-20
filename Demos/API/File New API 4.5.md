
# File > New API (4.5)
Create a new Web API project in ASP.net 4.5 and shows some ofteh key capabilities

### Pre reqs
* Visual Studio 2015
* Chrome with Postman extension

## New Project
File > New project > Web Application > Web API
* Can use Web api in any asp.net project but if you choose api as the project type it adds a help page

## Examine the default Project
Show Read me
* It is optimised for the type of content the project is for

Look at default ValuesController.cs
* No model (it is created in line)
* Show URL example
* Highlight 'API' route
	
App_starts\WebApiConfig.cs
* Show default route that gets anything that starts with 'api'

Run the API
* Point out that this is a combined app and includes MVC
* Go to API and show help page
* Show the various inputs / outputs / response types

## Add a model and scaffold controller
Add person model
```
public class Person
{
    [Key]
    public int Id { get; set; }
    public string FirstName { get; set; }
    Public int Age { get; set; }
}
```
	
Rebuild project

Right click controller > Add > New Scaffolded Item 
* Web API 2 controller with actions using entity framework
* PersonController
* Model: Person
* New data context 'personcontext'
* This will create the controller (no views)
* Show the controller code

## Play with API in PostMan
Run the site and show the new help pages for the PersonController
* Show Person help page
* (NOTE: Uses data annotations in CS to provide additional details to the automated documentation)
* Show POST Person and copy sample JSON

Open PostMan and POST people
* POST
* http://localhost:58463/api/people
* Header: Content-Type: text/json
* Body: Copy sample JSON from documentation to body but remove ID
* Look for 201/Created response
* POST a few more people (several required for odata demo later)

Change the verb to GET and show the people that were just added
* Show the JSON response
* Show that no XML was returns

Request XML response
* Add  header: ACCEPT: application/xml
* Show XML response

## Add Odata and show queries
Install the Odata nuget: `Install-Package Microsoft.AspNet.WebApi.OData`

Change the GET action to match the following
```
// GET: api/People
[EnableQueryAttribute]
public IQueryable<Person> GetPeople()
{
return db.People.AsQueryable();
}
```

Run the application again

Search for people whos name starts is Martin: 
`?$filter=Name eq 'Martin'`

Search for people who are less than 16 years old: 
`?$filter=Price lt 16`
		
## Secure the API
Add the Authorize attribute above the class `[Authorize]`

Run the api again

Do a GET in PostMan, note the `401` response

Create a user with the /api/account/register action
* Look at the docs to see syntax
* POST to `/api/account/register`
```
Body
{
    "Email": "martin.kearn@microsoft.com",
    "Password": "Password1!",
    "ConfirmPassword": "Password1!"
}
```
* Look for 200/OK

Do a POST to /Token
* Body: `form-urlencoded`
* Grant_type: `password`
* Username: `martn.kearn@microsoft.com`
* Password: `Password1!`

Copy `Access_Token`

Do a GET to /api/People
* Authorization header: Authorization: bearer {access token}