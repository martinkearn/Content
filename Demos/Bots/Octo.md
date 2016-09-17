
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
* Response: *Hi Martin Kearn. I'm Octo, the Ocado bot, how can I help you today?*
* Story: Knows you the user is from Skype. Is able to look-up their account details in the background

Show architecture slides
1. A bot
2. Hosted on the web somewhere
3. Published and connected to Microsoft Bot Framework
4. Bot Framework connects to channels, in this case Facebook
5. Users interact with channels as usual

**"I'd like to book a delivery please"**
* Response: *Sure, I can book a delivery slot for you. I know you normally prefer Mondays, I have these slots available next Monday:*
* Story: 
    * Knows user's preferences (Mondays) and available slots so is able to provide personalised information back
    * Rich 'cards' with buttons

Show architecture slides
6. Bot is connected to Ocado API
7. Cognitive Services LUIS. 

Show Cognitive Portal > LUIS
* Weather
* How is the weather in London tomorrow?

**"No no no, that is no good"**
* Response: *Oh you don't seem happy about that suggestion. Would any other days work for you?*
* Story: Sentiment Analysis via Cognitive Services Text Analytics API

Show architecture slides
8. Cognitive Services text Analytics - Sentiment

Show Cognitive Portal > Text Analytics API
* Enter the same text into the sample app to see negative sentiment

**"How about Tuesday?"**
* Response: *Sure, I have these slots available next Tuesday 13th September:*
* Story: 
    * Is able to understand the change in the context of the activity of booing the delivery slot.
    * Chained Dialogs
    * Also able to understand natural language via LUIS api

**"10:30 sounds good"**
* Response: *Thanks, I've booked a delivery for 10:30 on Tuesday 13th September for you
We're ready to make an order, here are your saved shopping lists, which one would you like to use?*
* Story: 
    * Understand natural language via LUIS api
    * Able to move the conversation onto a different thread (the actual order) and pull up user data again

## Shopping lists

The previous section pulled up the user's shopping lists in response to booking a 10:30 slot

**"10:30 sounds good"**
* Response: *Thanks, I've booked a delivery for 10:30 on Tuesday 13th September for you
We're ready to make an order, here are your saved shopping lists, which one would you like to use?*
* Story: 
    * Understand natural language via LUIS api
    * Able to move the conversation onto a different thread (the actual order) and pull up user data again

**"the weekend one please"**
* Response: *Sure, I'll use the Weekend Drinks and Nibbles list. Are you interested in any additional recommended products?*
* Story: 
    * The text given does not exactly match the list name but LUIS is able to deal with that and correctly choose the right list

**"the weekend one please"**
* Response: *Sure, I'll use the Weekend Drinks and Nibbles list. Are you interested in any additional recommended products?*
* Story: 
    * The text given does not exactly match the list name but LUIS is able to deal with that and correctly choose the right list
    * The bot is able to offer recommendations

**"Sure, I'd like to try some new beers"**
* Response: *OK, we have an 'Any 3 for £5' offer on offer for bottled beers at the moment. You have 4 items that qualify already in your Weekend Drinks and Nibbles list, can I recommend any two of the following to complete the offer?*
* Story: 
    * LUIS again taking the natural language and converting that to a request to see beer recommendations
    * The Cognitive Recommendations API can look the beers that are already in the list and make 'Item to item recommendations' and 'Personalized user recommendations'. 
    * It is also able to check the Ocado API for offers and up-sell

Show architecture slides
9. Cognitive Service Recommendations API

Show Cognitive Portal > Recommendations

**"London pride"**
* Response: *OK, I'll add London Pride £1.79 please choose one more to complete the offer?*
* Story: 
    * Remembering context of the dialog

**"doom bar sounds nice"**
* Response: *OK, I'll add Doom Bar Bitter £1.79. Is there anything else I can do for you? If you are not sure what I can do, ask for help! :)*
* Story: 
    * Chained ongoing dialouge
    * Help

**"No that is everything thanks"**
* Response: *OK, thanks, we'll email you an order receipt and you can change the order anytime up to 23:59 the day before (Monday 12th September). Just ask me to change the order if you want to add anything, otherwise our driver Karl will be with you at 10:30 on Tuesday 13th September, I hope you enjoy those new beers.*
* Story: 
    * Summarise order with details and web link
    * Contextually usefull information
    * Web link   

**"OK, bye"**
* Response: *OK, thanks, we'll email you an order receipt and you can change the order anytime up to 23:59 the day before (Monday 12th September). Just ask me to change the order if you want to add anything, otherwise our driver Karl will be with you at 10:30 on Tuesday 13th September, I hope you enjoy those new beers.*
* Story: 
    * Code personality into the bot

Show architecture slides
10. Other channels > Skype

Add 'Octo' in skype and start a dialog
* Hi
* Show my lists
* Help