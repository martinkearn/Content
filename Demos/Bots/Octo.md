
# Octo
This demo demonstrates the Bot Framework and Cognitive Services via a bot called 'Octo' which is an online shopping bot for Ocado.

### Pre-reqs
* Have Octo as a contact in Facebook messenger
* Have any previous conversations cleared in both Skype and FB messenger
* Make sure the Octo app service has been warmed-up or is on 'always on' mode
* Make sure you've removed any dodgy messages from friends in FB messenger
* Have the browser zoomed in so people can see the text in et overall context (magnifier does not work so well for this demo)

## Add Octo to Facebook messenger
Open Messenger.com in a browser and log in (the Windows app is not that reliable)

Search for an add 'Octo' (look for the green Ocado icon)

## Book a delivery Slot

**"Hi Octo"**
* Knows you the user is from Skype. Is able to look-up their account details in the background

Show architecture slides
1. A bot
2. Hosted on the web somewhere
3. Published and connected to Microsoft Bot Framework
4. Bot Framework connects to channels, in this case Facebook
5. Users interact with channels as usual

**"I'd like to book a delivery please"**
* Knows user's preferences (Mondays) and available slots so is able to provide personalised information back
* Rich 'cards' with buttons

Show architecture slides
6. Bot is connected to Ocado API
7. Cognitive Services LUIS

Show Cognitive Portal > LUIS
* Weather
* How is the weather in London tomorrow?

**"No no no, that is no good"**
* Sentiment Analysis via Cognitive Services Text Analytics API

Show architecture slides
8. Cognitive Services text Analytics - Sentiment

Show Cognitive Portal > Text Analytics API
* Enter the same text into the sample app to see negative sentiment

**"How about Tuesday?"** 
* Is able to understand the change in the context of the activity of booing the delivery slot.
* Chained Dialogs
* Also able to understand natural language via LUIS api

**"10:30 sounds good"**
* Understand natural language via LUIS api
* Able to move the conversation onto a different thread (the actual order) and pull up user data again

## Shopping lists

The previous section pulled up the user's shopping lists in response to booking a 10:30 slot

**"10:30 sounds good"**
* Understand natural language via LUIS api
* Able to move the conversation onto a different thread (the actual order) and pull up user data again

**"the weekend one please"**
* The text given does not exactly match the list name but LUIS is able to deal with that and correctly choose the right list

**"the weekend one please"**
* The text given does not exactly match the list name but LUIS is able to deal with that and correctly choose the right list
* The bot is able to offer recommendations

**"Sure, I'd like to try some new beers"**
* LUIS again taking the natural language and converting that to a request to see beer recommendations
* The Cognitive Recommendations API can look the beers that are already in the list and make 'Item to item recommendations' and 'Personalized user recommendations'. 
* It is also able to check the Ocado API for offers and up-sell

Show architecture slides
9. Cognitive Service Recommendations API

Show Cognitive Portal > Recommendations

**"London pride"**
* Remembering context of the dialog

**"doom bar sounds nice"**
* Chained ongoing dialouge
* Help

**"No that is everything thanks"**
* Summarise order with details and web link
* Contextually useful information
* Web link

**"OK, bye"**
* Code personality into the bot

Show architecture slides
10. Other channels > Skype

Add 'Octo' in skype and start a dialog
* Hi
* Show my lists
* Help