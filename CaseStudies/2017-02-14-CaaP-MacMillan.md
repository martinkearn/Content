---
layout: post
title:  "MacMillan Cancer Support"
author: "Martin Kearn"
author-link: "http://martink.me"
author-image: "http://martink.me/images/MartinKearnProfile1.jpg"
date:   2017-03-27
categories: [Conversations as a Platform, Cognitive Services]
color: "blue"
image: "http://www.macmillan.org.uk/_images/16558-9-17322Logo.jpg"
excerpt: "MacMillan make cancer support information more acessible via Microsoft Bot Framework and Cognitive Services"
language: [English]
verticals: [Healthcare]
---

MacMillan are a charity that provide information and support for people affected by cancer.

MacMillan wants to make the vast array of information on their website more acessible both in terms of discovery and access at different times of day.

Their solution was to use the Microsoft Bot Fraemwork with the Microsoft Cognitive Services QandA maker to create a FAQ bot.
 
Core Team

-   Martin Kearn (\@MartinKearn)

## Parter Profile ##
MacMillan are a charity that provide information and support for people affected by cancer.

- https://www.macmillan.org.uk/

## Problem Statement ##

The MacMillan website has a wide range of information about cancer and the services that Macmillan can offer. The website is backed up by a phone line which operates 9:00 > 20:00 Monday-Friday. 

There are problems associated with the current services:

- The phone line is an expensive operation and there would be significant cost savings in offloading some of the phone line traffic to other, more cost effective interfaces

- The website is vast and users often have difficuty finding the right information. Many of the calls to the help desk result in agents directing the users to the right place on the website.

- The phone line is only open 9:00 > 20:00 Monday-Friday

MacMillan want to make it easier and quick for users to access information when and how they need to. This includes making website information more discoverable and providing a backfill for times when the phone line is either closed or busy.

When users are using a non-human uinterface, Macmillan would like to do a human hand-off for scenarios that require it. For example if someone says "I really want to talk to someone", the interface should broker a conversation with a real human.
 
## Solution and Steps ##

The solution was a bot focussed on answering frequently asked questions and guiding people to the relevant part of the Macmillan website via conversational UI. 

The bot will help people find the information they are looking for by either providing information directly via the bot or by linking to the relevant place online.

The focus area in terms of source information will be on the 'Information and Support' section of the website which caters for people asking for information about cancer as opposed to donating, volunteering or finding out about Macmillan

Macmillan would also like to capture responses to the conversation and feed it through to CRM - perhaps send to a service bus or mock API.

## Technical Delivery ##
The bot was written using the [Microsoft Bot Fraemwork](https://dev.botframework.com/) with C#. The [Microsoft Cognitive Services QandA Maker](https://www.microsoft.com/cognitive-services/en-us/qnamaker) was used to generate a FAQ service based on website data.


 
## Conclusion ##




## Additional resources ##
