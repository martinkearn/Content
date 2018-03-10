# Start Cognitive and Bots
A demo/lab to get people started with cognitive service and bot service.

### Pre-reqs
* Have a Microsoft setup for the event (MartinKearnHack24@outlook.com / GP07ghzmini)

## Get an Azure code
* http://Aka.ms/azurepassmanager 
* Event name `aievent`
* Don't claim a code but use `WJY6GD6LOIDDYAV5L1` (pre-claimed)

## Create subscription
* Go to https://www.microsoftazurepass.com/
* Login
* Activate subscription 

## Get a Cognitive Text Analytics key
* Portal.azure.com
* New resource > Search "Text Analytics API" > Create
* North Europe
* S0
* New Resource Group (Hack24)
* Go to resource > keys > Copy Key 1 (3f748dc2104341d3a31671d854e931e2)

## Use Cognitive Text Analytics key in api console
* Bing search for "cognitive services text analytics api"
* API > Open API testing console > North Europe. NOTE: key is region specific. Must use the console (and endpoint) for wherever you created your key
* Go to POST sentiment
    - Add key to `Ocp-Apim-Subscription-Key`. NOTE: mandatory header with almost all cognitive services
	- Language en
	- Text whatever you want
	- Send and observe JSON result
* Go to POST key Phrases
	- Add key to `Ocp-Apim-Subscription-Key`
	- Language 'en'
	- Text "24 hours of code & creativity in the heart of Nottingham"
	- Send and observe JSON result has pull out "creativity" "hours of code" as key phrases
	
## Create Bot service with LUIS
* Portal.azure.com
* New resource > Search 'Web App Bot' > Create
* UK West
* S1
* Language Understanding template
* LUIS app location 'west europe'
* New service plan 'Ukwest'
* App insights off
* Create

## Explore and test bot
* Go to resource from previous step
* Test in web chat > Have a chat (like a child)
* Go to Build > Online code editor
* Look at controller
* Look at dialog and intent handlers
* Change "you said" to "you uttered"
* Console > deploy.cmd
* Go back and test in web chat again

## Explore LUIS
* Go to http://eu.luis.ai. 
    - NOTE: The 'location you created when setting up the bot service dictates which LUIS endpoint and portal you use). This is the European one. If you model does not show, you may be looking in the wrong portal
* Scroll to bottom and click 'create luis app'
* Pick out your model (identify byname)

## Add some Hack24 intents
* Add intent 'AgendaCheck'
* Add entity 'AgendaSlot'
* Go back to intent and train and tag with
    - "when is the evening meal served"
    - "when does shoulder massages start"
    - "when should i give your mum a call"
    - "what time does hacking begin"
    - "when is the welcome presentation"
* Train
* Publish
* Test and ask "what time should I give mum a call"
		○ Inpsect > show intent and entities
* Click production url and add "what time should I give mum a call" after 'q'
		○ Show JSON

## Update bot to support new LUIS
* Go back to bot service
* Go to Build > Online code editor > BasicLuisDialog
    - NOTE: In practice you would not edit code online like this. You'll need to hook up Visual Studio / VS code with continuous integrate to do this properly. You'll also need to install the emulator to do local debugging
* Add

```
[LuisIntent("AgendaCheck")]
public async Task AgendaCheckIntent(IDialogContext context, LuisResult result)
{
    var entity = result.Entities[0];
    
    if (entity.Entity == "give mum a call")
    {
        await context.PostAsync($"It's Mother's day on Sunday; give your Mum a call at about 12:01, right after you've finished hacking!");
    }
} 
```

* Console > deploy.cmd
* Go back and test in web chat again
* Ask "when should I give mum a call"
