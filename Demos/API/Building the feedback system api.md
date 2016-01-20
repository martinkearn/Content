
# Building the feedback system API
A demo that show how the API for the MSWebDay feedback system was built.

### Pre-Reqs
* API published as debug
* Have two instances of VS open
    * API
    * Other projects
* Make sure all break-points are deleted in API project
* Cloud explorer installed and logged in
* Azure SDK installed

## Explore the API project
Look at Domain
* Explore all four entities and explain their relationships
* Key attribute
* ID properties
		
Look at MVC 4.5 project structure
* Controllers
* Areas for MVC docs project

Look at Migrations
* AutomaticMigrationsEnabled
* Seed data
	
## Debug the live API
Keep the api project open

View > Cloud explorer
*NOTE: can also find it in search*

Web Apps > api-MSWebDay > Right-click > Attach debugger
* Wait for the api website to load

## Debug the Angular App
	
Show public angular app on its own
* Open a different browser
* Show the public Angular app
* http://mswebday.net > feedback

Add break-points on the following paths in the API
* `Events.GetEvents` - to get the list of all events for drop down
* `CurrentEvent.GetCurrentEvent` - to get the current event details
* `Questions.GetQuestions` - to get the questions for this event
* `Evaluations.Post` - to post the users feedback

Ask the audience to start using the app

Talk through each break-point/request for initial load (all GET)
* `Events.GetEvents` - to get the list of all events for drop down
* `CurrentEvent.GetCurrentEvent` - to get the current event details
* `Questions.GetQuestions` - to get the questions for this event
	
Submit feedback and walk through the post
* `Evaluations.Post` - to post the users feedback

Take the time to explore the model binding and step through the code

## (optional) Debug the Windows 'Current event' app
Add break-points on the following paths in the API
* `Events.GetEvents` - gets the list of events for the list control
* `CurrentEvent.GetCurrentEvent` - gets the current event
* `CurrentEvent.PostCurrentEvent` - updates the current event

Use the app, catch the requests and talk through them