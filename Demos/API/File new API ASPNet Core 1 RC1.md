
# File > New API (ASP.NET Core 1 RC1)
Create a new Web API project in ASP.NET Core 1 RC1 and shows some of the key capabilities and issues at this stage

## New Project
File > New project > Web Application > Web API
* Solution: 'ASPNet5'
* Project 'WebAPI'
* *NOTE: Authentication is not an option yet*

Examine the default Project

Look at default ValuesController.cs
* No model (it is created in line)
* Highlight 'API' route
* *NOTE: It is basically the same as 4.5*

## Add a Model and try to scaffold controller
Add Models folder

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

Right-click 'Controllers' 
* *NOTE: No scaffolding option*
	
Right-click 'Controllers' > 'Add' > 'New item' > 'Web API Controller Class'
* This is just the 'values' controller - no scaffolding

## Add a 'Web Application' project to the solution
Right-click solution > 'add' > 'new project'
* ASP.NET Web Application
* Web Application template
* Call it 'WebApplication'
* *NOTE: There is now a security option*
	
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

New > Scaffolded item > API Controller with actions using entity framework
* Choose Person (webapi.models)
* Use existing data context
* Async
* *NOTE: This is almost identical to the 4.5 scaffolding*

## Enable Entity Framework Migrations
*NOTE: The Entity Framework migrations experience is still work-in-progress. These steps will be removed for release but required right now*

Open a command prompt (Windows Key + R, type PowerShell, Run as admin)

Use the cd command to navigate to the project directory

Select the right version of .net
```
Run dnvm use 1.0.0-rc1-final
```

Scaffold a migration to create the initial set of tables for your model.
```
dnx ef migrations add MyFirstMigration
```

Apply the new migration to the database. 
```
dnx ef database update
```
*NOTE: Because your database doesn’t exist yet, it will be created for you before the migration is applied.*

Have a look in the migrations folder to see the migration

## Run and use the API
Run the app

POST in Postman
* POST
* http://localhost:2265/api/People
* Body: Raw
```
{
    "FirstName":"Bob",
    "Age":35 
}
```
	
GET in Postman
* GET
* http://localhost:2265/api/People

### Snippets

public class Person
{
    [Key]
    public int Id { get; set; }
    public string FirstName { get; set; }
    public int Age { get; set; }
}





Run dnvm use 1.0.0-rc1-final

dnx ef migrations add MyFirstMigration

dnx ef database update




{
    "FirstName":"Bob",
    "Age":35 
}