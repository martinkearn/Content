
# File > New API (ASP.NET Core 1 RC2)
Create a new Web Application project in ASP.NET Core 1 RC2 and shows some of the key capabilities and issues at this stage. Compare to the Web API project template to show how that is even more lacking

## Create a 'Web Application' project
File > New Project > Web > ASP.NET Core Web Application (.NET Core)
* Web Application template
* Call it 'WebApplication'
* *NOTE: There is a security option which includes individual accounts*
	
## Add a Model and scaffold a controller
Add Person model
```
public class Person
{
    [Key]
    public int Id { get; set; }
    public string FirstName { get; set; }
    public int Age { get; set; }
}
```
	
Rebuild project

Right-click Controllers > New Scaffolded Item > API Controller with actions using entity framework
* Choose Person (webapi.models)
* Use existing data context
* *NOTE: This is almost identical to the 4.5 scaffolding but without the choices about Async*

Explore the 'PeopleController'
* `ApplicationDbContext` Dependancy injection for DB contexzt
* `[HttpGet("{id}")]` / `[FromRoute] int id`
* `[FromBody] Person person`
* Code inside action almost identical for 4.x

## Enable Entity Framework Migrations
*NOTE: The Entity Framework migrations experience is still work-in-progress. These steps will be removed for release but required right now*

Open a command prompt (Windows Key + R, type CMD, Run as admin)

Use the cd command to navigate to the project directory

Scaffold a migration to create the initial set of tables for your model.
```
dotnet ef migrations add Initial
```

Apply the new migration to the database. 
```
dotnet ef database update
```
*NOTE: Because your database doesn’t exist yet, it will be created for you before the migration is applied.*

Have a look in the migrations folder to see the migration

## Run and use the API
Run the app

POST in Postman
* POST
* http://localhost:13798/api/People
* Body: Raw
* JSON
```
{
    "FirstName":"Bob",
    "Age":35 
}
```
* Look for a 201 response
* Post another few people
	
GET in Postman
* GET
* http://localhost:2265/api/People

## Add a Web API project to show where it is lacking right now
Right-click Solution > Add > New Project > Web > ASP.NET Core Web Application (.NET Core)
* Web API template
* Call it 'WebAPI'
* *NOTE: Authentication is avaliable, but not for individual accounts*

Examine the default Project

Look at default ValuesController.cs
* No model (it is created in line)
* Highlight 'API' route
* *NOTE: It is basically the same as 4.5*

## Show there is no scaffolding
Right-click 'Controllers' 
* *NOTE: No scaffolding option*
	
Right-click 'Controllers' > 'Add' > 'New item' > 'Web API Controller Class'
* This is just the 'values' controller - no scaffolding

### Snippets

```
public class Person
{
    [Key]
    public int Id { get; set; }
    public string FirstName { get; set; }
    public int Age { get; set; }
}
```

`dotnet ef migrations add Initial`

`dotnet ef database update`

```
{
    "FirstName":"Bob",
    "Age":35 
}
```
