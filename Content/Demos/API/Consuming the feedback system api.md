
# Consuming the feedback system API
A demo that show how the API for the MSWebDay feedback system was built.

WARNING: When last attempted, Visual Studio crashed when trying to start debugging the remote Azure web app

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
* _Optional_ - Capture the first POSTed evaluation, look up the user id and award a prize. Post man has a saved query called 'Contacts GET by ID'

Take the time to explore the model binding and step through the code

## Debug the Windows 'Current event' app
Clear breakpoints (Debug > Delete All breakpoints)

Add break-points on the following paths in the API
* `Events.GetEvents` - gets the list of events for the list control
* `CurrentEvent.GetCurrentEvent` - gets the current event
* `CurrentEvent.PostCurrentEvent` - updates the current event

Run the app

Talk through these GET requests needed to load the initial app
* `Events.GetEvents` - gets the list of events for the list control
* `CurrentEvent.GetCurrentEvent` - gets the current event

Change the current event and talk through these requests
* `CurrentEvent.PostCurrentEvent` - updates the current event
* `CurrentEvent.GetCurrentEvent` - gets the current event again to update the ui

## Debug the Office 'Evaluations' app
Clear breakpoints (Debug > Delete All breakpoints)

Add break-points on the following paths in the API
* `Events.GetEvents` - gets the list of events for the list control
* `Evaluations.Get(int EventId)` - gets the evaluations of the event

Run app
Load the app and talk through
* `Events.GetEvents` - get the initial list for the drop down

Insert Summary
* `Evaluations.Get(int EventId)` - gets the evaluations of the event

Insert Evaluations
* `Evaluations.Get(int EventId)` - gets the evaluations of the event