---
date: "2017-03-07"
title: GigSeekr - live music discovery via a bot
author: Martin Kearn
---

During Feburary 2017, Microsoft worked with GigSeekr to produce a bot that would
help users find out about artists, live music events and live music venues for a
given location or date.

The Microsoft Bot Framework was used with the LUIS and Recommendation Cognitive
Services. Users can use natural language utterances to find artists, where they
are playing, when they are playing, recommended artists and other information.
Users can also find live music venues near them or in a given location as well
as live music events featuring given artists, gig styles, music styles or
locations.

The Bot is written using the Bot Framework and published via Skype and Facebook
Messenger. With Cortana bing a intended channel for the future.

Core Team

-   Martin Kearn (\@MartinKearn)

-   Tomi Paananen (\@tompaana)

-   David Hamilton (\@daveh101 / \@gigseekr)

-   Alec Cowton (\@AlecTCowton)

-   Lilian Kasem (\@liliankasem)

Customer profile
----------------

[GigSeeker](http://www.gigseekr.com/) is focussed on providing information about
live music events & venues as well as artists performing within the UK and
Ireland. They describe themselves as "Gigseekr is a new live music discovery
service committed to helping you find and stay up to date with events across the
UK and Ireland".

GigSeekr is a brand name of [Damm Good Media](http://www.damgoodmedia.com/) who
are a UK-based company that provide data solutions for the live entertainment
industry.

Damm Good Media are the creators of the popular [Pepper mobile
app](http://www.pepper.so/) which focusses on social live music discovery.

Problem Statement
-----------------

Using social media and conversational-UI was a natural fit for the sort of
queries that GigSeekr want to allow users to make. Before the hack, GigSeekr
planned to tackle the following scenarios.

Search for an artist

-   Search for an artist \> Artist details

-   When are they next performing

-   Where are they next performing

-   When are they next performing near me

-   Search for similar artists

Search for a location

-   Locations near me \> Location details with address etc

-   Artists performing near me with a time frame i.e. "which bands are playing
    near me this weekend"

Proactive notifications of favourite artists

-   Tell me when 'artist' are playing near me next \> bot proactively alerts
    user

Bot Plumbing

-   Implementation of a help dialog

-   Implementation of a triage dialog and routing between dialogs

Cognitive Services

-   LUIS for natural language processing, intent and entity extraction

-   Recommendation API to make artist recommendations

Solution and Steps
------------------

The high level solution was a Microsoft Bot Framework bot that leans heavily on
LUIS to understand the user's intent and help navigate them to either an artist,
event or venue (referred to as an 'entity' by GigSeekr). From this point the
user can invoke several other intents relating specifically to that Entity such
as buy tickets or get recommended artists.

The recommendations were provided using the Microsoft Recommendations API.

The majority of the bot involved relatively standard use of dialogs and dialog
stack management. However there were several areas where research was done and
new patterns and practices were discovered.

### User Interface Considerations

GigSeekr have a long-term ambition to use the bot with digital assistants such
as Cortana, Alexa etc and therefore there we took a design constraint to keep
the user interface as text-based as possible without compromising the user
interface.

Buttons and text have been used throughout the bot but richer interfaces such as
carousels and cards have not been used as they are not likely to be compatible
with Cortana and other text-based digital assistants.

### LUIS Architecture: Using Multiple Models Concurrently

The nature of the Gigseekr bot mandated that users are likely to traverse from
one logical location to another without following a linear flow. As an example,
a user might follow a flow like this

1.  "Tell me about Metallica" \> Bot takes the user to the Metallica details
    dialog

2.  "When are they next playing" \> Bot lists upcoming events for Metallica

3.  User clicks an event from the list \> Bot takes the user to the event
    details page

4.  "Tell me more about the venue" \> Bot takes user to the venue details page
    for the venue where the event will take place

5.  "Who else is playing at this venue" \> Bot lists other upcoming events at
    this venue

6.  User clicks another event (for example, a Korn gig) \> Bot takes the user to
    the event details page

7.  "Tell me more about Korn" \> Bot take user to the artist details page for
    Korn

8.  The flow going go on like this endlessly with no clear exit point

There was a requirement to support these endless flows between the 6 primary
logical locations listed below: 

-   Artist

-   List of Artists

-   Event

-   List of Events

-   Venue

-   List of Venues

Each of these logical locations has their own list of LUIS intents which should
be supported; some of which are unique to a logical location (for example "show
me related artists" only applies to the Artist location, "buy tickets" only
applies to the Event location). Additionally, the primary entity is known by the
time the user is at one of these locations, therefore knowledge of the Artists,
Event and Venue LUIS entities is not required.

Whilst supporting these entity-based flows, the bot also had a requirement to
respond to base commands such as "help", "abuse", "start again" or allowing the
user to jump to a new search out of context of the existing path. In the
Metallica example above, the user may at any point choose to ask about a
completely different artist, venue or event and therefore start again.

This was a tricky conceptual challenge in terms of LUIS and during the hack we
discussed the LUIS architecture in some detail.

The final was to create a base LUIS model which supports various entities and
intents relating to base(global) commands and initial searches. We also
implemented a LUIS model for Artists, Venues and Events seperately, each with
their own unique intents and entities.

The problem then becomes the `LuisDialog` class in the c\# Bot Builder SDK which
only supports a single LUIS model at any one time. This meant that users could
only use artist/venue/event-specific intents or base one. This was a huge
constraint on the flow of the bot and would have severely impacted its
usefulness.

The solution was to implement a custom class which did the call to the LUIS api
directly and allowed a cascade of multiple models. Depending on the logical
location, the class would call either the Artist, Venue or Event specific models
first, but would fall back to the base model, thus supporting all the required
commands, both base ones and ones that are specific to the logical location.

### LUIS Phrase Lists

The search LUIS model has the following entities:

-   Artist: i.e. Metallica, Muse, Frank Turner

-   ArtistType: i.e. Band, Solo Artist, Singer/Songwriter

-   Genre: i.e Pop, Folk, Country

-   Location: i.e. London, Birmingham, Glasgow (see more on why this exists in
    the LUIS Geography section)

-   Venue: i.e. Cavern Club, The Live Lounge, The Deaf Institute

During the initial testing for the LUIS, we struggled to have LUIS differentiate
between entities with any usable accuracy, especially artist and venue as many
artist and venue names sound the same. For example, even humans may assume that
‘The Purple Turtle’ is a band name, but it is actually a venue in Reading, UK.

To resolve this, we started to use phrase lists to give LUIS training
indicators. Phrase lists can have up to 5000 comma-separated terms in them which
map to an entity with a maximum of 10 phrase lists per model.

We used the top 1500 artists, venues and locations (see more on why this exists
in the LUIS Geography section) from GigSeekr’s database as a phrase lists and in
doing so, we saw a huge jump in accuracy of entity identification.

To prepare the phrase lists, we found this tool very useful. It converts columns
of data into comma-separate text blocks:
https://convert.town/column-to-comma-separated-list

### LUIS Geography

LUIS has a range of built-in entities for common things such as DateTime,
Currency, Ordinals etc. One of the built-in entities which we wanted to use was
Geography which is supposed to be able to identify names of towns, cities,
postal code (zip codes) and lat//long coordinates.

Geography was key to GigSeekr’s requirement in order to support the
location-based queries such as

-   Who is playing in Worcester tonight

-   Are there any singer/songwritiers in Birmingham on Saturday

Upon testing the built-in Geography entity, we found that it worked well for USA
locations and major cities such as London, but failed to recognize even large UK
towns and cities such as Leeds, Oxford, Bristol. This meant that the built-in
Geography entity was useless for GigSeekr because we needed to be able to
identify even small towns and villages.

To address this problem, we created our own ‘Location’ entity which we trained
with a phrase list of the top 1000 towns and cities in the UK (sorted by
population).

You can download the CSV file we used for the phrase list which contains the top 1000 UK place names [here](https://raw.githubusercontent.com/martinkearn/Content/master/CaseStudies/media/Top1000UKCities.txt).

### Proactive notifications from an external service

### Recommendation API training

### Getting the user's location

Conclusion
----------

GigSeekr have been able to make a great start with their bot and hope to have it
avaliable via Skype, Facebook and Cortana shortly.
