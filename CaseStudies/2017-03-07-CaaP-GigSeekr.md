---
layout: post
title:  "GigSeekr - live music discovery via a bot"
author: "Martin Kearn"
author-link: "http://martink.me" 
author-image: "http://martink.me/images/MartinKearnProfile1.jpg"
date:   2017-03-07
categories: [Conversations as a Platform, Cognitive Services]
color: "blue"
#image: "{{ site.baseurl }}/images/imagename.png" #should be ~350px tall
excerpt: GigSeekr used the Microsoft Bot Fraemwork and Cognitive services (LUIS and Recommendations) to build a live music discovery bot
language: "English"
verticals: "Retail"
---

During Feburary 2017, Microsoft worked with GigSeekr to produce a bot that would help users find out about artists, live music events and live music venues for a given location or date.

The Microsoft Bot Framework was used with the LUIS and Recommendation Cognitive Services. Users can use natural language utterances to find artists, where they are playing, when they are playing, recommended artists and other information. Users can also find live music venues near them or in a given location as well as live music events featuring given artists, gig styles, music styles or locations.

The Bot is written using the Bot Framework and published via Skype and Facebook Messenger. With Cortana bing a intended channel for the future.
 
- Core Team: Martin Kearn (@MartinKearn), Tomi Paananen (@tompaana), David Hamilton (@daveh101 / @gigseekr), Alec Cowton (@AlecTCowton), Lilian Kasem (@liliankasem)

## Customer profile ##
[GigSeeker](http://www.gigseekr.com/) is focussed on providing information about live music events & venues as well as artists performing within the UK and Ireland. They describe themselves as "Gigseekr is a new live music discovery service committed to helping you find and stay up to date with events across the UK and Ireland".

GigSeekr is a brand name of [Damm Good Media](http://www.damgoodmedia.com/) who are a UK-based company that provide data solutions for the live entertainment industry.

Damm Good Media are the creators of the popular [Pepper mobile app](http://www.pepper.so/) which focusses on social live music discovery.
 
## Problem statement ##
Using social media and conversational-UI was a natural fit for the sort of queries that GigSeekr want to allow users to make. Before the hack, GigSeekr planned to tackle the following scenarios.

Search for an artist
* Search for an artist > Artist details
* When are they next performing
* Where are they next performing
* When are they next performing near me
* Search for similar artists

Search for a location
* Locations near me > Location details with address etc
* Artists performing near me with a time frame i.e. "which bands are playing near me this weekend"

Proactive notifications of favourite artists
* Tell me when 'artist' are playing near me next > bot proactively alerts user

Bot Plumbing
* Implementation of a help dialog
* Implementation of a triage dialog and routing between dialogs

Cognitive Services
* LUIS for natural language processing, intent and entity extraction
* Recommendation API to make artist recommendations

## Solution and steps ##
The high level solution was a Microsoft Bot Framework bot that leans heavily on LUIS to understand the user's intent and help navigate them to either an artist, event or venue (referred to as an 'entity' by GigSeekr). From this point the user can invoke several other intents relating specifically to that Entity such as buy tickets or get recommended artists.

The recommendations were provided using the Microsoft Recommendations API.

The majority of the bot involved relatively standard use of dialogs and dialog stack management. However there were several areas where research was done and new patterns and practices were discovered.

### User interface considerations ###
GigSeekr have a long-term ambition to use the bot with digital assistants such as Cortana, Alexa etc and therefore there we took a design constraint to keep the user interface as text-based as possible without compromising the user interface.

Buttons and text have been used throughout the bot but richer interfaces such as carousels and cards have not been used as they are not likely to be compatible with Cortana and other text-based digital assistants.  

### LUIS Architecture and multiple models ### 
The nature of the Gigseekr bot mandated that users are likely to traverse from one logical location to another without following a linear flow. As an example, a user might follow a flow like this

1. "Tell me about Metallica" > Bot takes the user to the Metallica details dialog
2. "When are they next playing" > Bot lists upcoming events for Metallica
3. User clicks an event from the list > Bot takes the user to the event details page
4. "Tell me more about the venue" > Bot takes user to the venue details page for the venue where the event will take place
5. "Who else is playing at this venue" > Bot lists other upcoming events at this venue
6. User clicks another event (for example, a Korn gig) > Bot takes the user to the event details page
7. "Tell me more about Korn" > Bot take user to the artist details page for Korn
8. The flow going go on like this endlessly with no clear exit point

There was a requirement to support these endless flows between the 6 primary logical locations listed below:
* Artist
* List of Artists
* Event
* List of Events
* Venue
* List of Venues

Each of these logical locations has their own list of LUIS intents which should be supported; some of which are unique to a logical location (for example "show me related artists" only applies to the Artist location, "buy tickets" only applies to the Event location). Additionally, the primary entity is known by the time the user is at one of these locations, therefore knowledge of the Artists, Event and Venue LUIS entities is not required.

Whilst supporting these entity-based flows, the bot also had a requirement to respond to global commands such as "help", "abuse", "start again" or allowing the user to jump to a new search out of context of the existing path. In the Metallica example above, the user may at any point choose to ask about a completely different artist, venue or event and therefore start again.

This was a tricky conceptual challenge in terms of LUIS and during the hack we discussed the LUIS architecture in some detail.

The final solution saw a global LUIS model which supports various entities and intents relating to global commands and initial searches. We also implemented a LUIS model for Artists, Venues and Events seperately, each with their own unique intents and entities.

The problem then becomes the `LuisDialog` class in the c# Bot Builder SDK which only supports a single LUIS model at any one time. This meant that users could only use for example artists-specific intents or global one. This was a huge constraint on the flow of the bot and would have severely impacted its usefulness.

The solution was to implement a custom class which did the call to the LUIS api directly and allowed a cascade of multiple models. depending on the logical location, the class would call either the artist, venue or event specific models first, but would fall back to the global model, thus supporting all the required commands, both global ones and ones that are specific to the logical location.

### LUIS Phrase Lists ### 

### LUIS Geography ### 

### Proactive notifications from an external service ### 

### Recommendation API training ###

### Getting the user's location ###




 
## Conclusion ##
GigSeekr have been able to make a great start with their bot and hope to have it avaliable via Skype, Facebook and Cortana shortly.