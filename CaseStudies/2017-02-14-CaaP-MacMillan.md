---
date: "2017-03-27"
title: MacMillan Cancer Support
author: Martin Kearn
---

MacMillan are a charity that provide information and support for people affected
by cancer.

MacMillan wants to make the vast array of information on their website more
acessible both in terms of discovery and access at different times of day.

Their solution was to use the Microsoft Bot Fraemwork with the Microsoft
Cognitive Services QandA maker to create a FAQ bot.

Core Team

-   Martin Kearn (\@MartinKearn)

-   Mohammad Salman

-   Nick Palmer

Parter Profile
--------------

MacMillan are a charity that provide information and support for people affected
by cancer. You can see more about their services at
<https://www.macmillan.org.uk>

Problem Statement
-----------------

The MacMillan website has a wide range of information about cancer and the
services that Macmillan can offer. The website is backed up by a phone line
which operates 9:00 \> 20:00 Monday-Friday.

The website serves and average of 1.1 million unique users each month and an
average of 4.4 million page views.

There are problems associated with the current services:

-   The phone line is an expensive operation. There would be significant cost
    savings in offloading some of the phone line traffic to other, more cost
    effective interfaces

-   The website is vast and users often have difficuty finding the right
    information. This is down to several reasons:

    -   Users are often in a heightened emotional state when interacting with
        the website as they may have recently been diagnosed with cancer

    -   The typical age of users is older than most website and users may be
        less comfortable navigating a large website

-   Many of the calls to the help desk result in agents directing the users to
    the right place on the website.

-   The phone line is only open 9:00 \> 20:00 Monday-Friday

MacMillan want to make it easier and quicker for users to access information
when and how they need to. This includes making website information more
discoverable and providing a backfill for times when the phone line is either
closed or busy.

MacMillan also want to make it easy for user to request a call from a human.

When users are using a non-human interface, Macmillan would like to do a human
hand-off for scenarios that require it. For example if someone says "I really
want to talk to someone", the interface should broker a conversation with a real
human.

Solution and Steps
------------------

The solution was a bot focussed on answering frequently asked questions and
guiding people to the relevant part of the Macmillan website via a
conversational user interface.

The bot will help people find the information they are looking for by either
providing information directly via the bot or by linking to the relevant place
online.

TO DO, have Macmillan provide a quote as to why they are looking at bots

The focus area in terms of source information will be on the 'Information and
Support' section of the website which caters for people asking for information
about cancer as opposed to donating, volunteering or finding out about
Macmillan. Going forward the bot may explore these areas too, but MacMillan are
keen to execute well on a small, specific area first.

MacMillan would also like to capture responses to the conversation and feed it
through to CRM.

There are three main interactions within the bot flow, they are as follows

![MacMillan bot flow](media/c72b484105210a5a620f55af61832414.jpg)

MacMillan bot flow

MacMillan bot flow

MacMillan bot flow

### What type of information?

The first interaction the user has with the bot is around ascertaining what sort
of information the user is looking for. This is implemented by a
`PromptDialog.Choice` which presents the main types of information MacMillan
offer:

-   **Understanding**: what cancer is

-   **Diagnosing**: symptoms, causes and risk factors

-   **Organising**: the practical, work and financial side

-   **Treating**: cancer and what to expect

-   **Coping**: with and after cancer treatment

-   **Resources**: publications to order, download and print

Each of the above choices map to a specific section of the MacMillan website
beneath the ‘Information and support’ section:
<http://www.macmillan.org.uk/information-and-support/index.html>

The response to this choice is stored and kept for later in the conversation.

### Intent and Keywords

The second interaction is the bot asking the users for more information about
what they are looking for. This utterance is passed to a LUIS model which is
trained as follows:

Intents:

-   **Find Information**: This intent will pass information to the third stage
    of the conversation which is the Q&A maker

-   **Arrange a Call Back**: This intent will use
    [FormFlow](https://docs.botframework.com/en-us/csharp/builder/sdkreference/forms.html)
    to gather basic contact information and pass the information to the CRM
    system where it will be picked up for a call back

Entities

-   **Cancer Type**: the type of cancer that the user wants information about.
    For example, ‘Breast Cancer’, ‘Brain cancer’

-   **Treatment**: the types of treatment the user wants information about. For
    example, ‘surgery’, ‘radiotherapy’, ‘chemotherapy’. See
    <http://www.macmillan.org.uk/information-and-support/treating/index.html#162757>

-   **Symptoms**: the symptoms users may want to explore

-   **Support Type**: the type of support the user wants information about. For
    example: financial support, emotional support

### Q&A Maker

The original intentions was to direct the Q&AMaker service at the various parts
of the MacMillan website have it work out the questions and answers. However,
early experimentation informed us that this approach was not going to work well
because the website is not structured in question and answer format and is very
deep with lots of individual pages, therefore the service did not do a good job
of extracting the questions which resulted in poor results.

However, when questions and answers were manually added with links to the
relevant web page in the answer, the results become more accurate and usable.

To increase accuracy, MacMillan implemented questions in a keyword format based
on the entities and intent from LUIS and the choice from the initial ‘type of
information’ dialog. This approach worked well and gave a high level of
accuracy.

Here are some example inputs to the Q&AMaker service:

-   Breast Cancer Symptoms

-   Brain Tumour Understanding

-   Chemotherapy financial support

There is a question about what value the Q&A maker service adds here as it is
effectively being used as a knowledge base. There are several benefits:

-   Compared to web site search, the Q&A maker gives a single answer rather than
    a long list of results which must then be navigated

-   The portal and manual question authoring is a great feature which
    non-technical people can manage without affecting the way the bot works

Technical Delivery
------------------

The bot was written using the [Microsoft Bot
Fraemwork](https://dev.botframework.com/) with C\#. The [Microsoft Cognitive
Services QandA
Maker](https://www.microsoft.com/cognitive-services/en-us/qnamaker) was used to
generate a FAQ service based on website data.

The use of Microsoft technology was a natural and obvious choice for MacMillan
...

>   "We use a Microsoft stack at our core so the use of the Microsoft Bot
>   Fraemwork and Microsoft Cognitive Services was a natural extension to our IT
>   estate. As an architectural principle we are in favour of using
>   software/platform-as-a-service as a first choice rather thanbuilding our own
>   software. Both the Bot Framework and Cognitive Services gives us a great
>   heads start with a very low cost of entry." Nick Palmer, Senior Applications
>   Developer, MacMillan

[Microsoft Cognitive Services Language Understanding Intelligent Service
(LUIS)](https://www.microsoft.com/cognitive-services/en-us/luis-api/documentation/home)
was also used to triage enquiries and gather some background information before
handing off to the Q&A maker service.

To Do: architecture diagram

### LUIS Phrase List for Improved Recognition

A Phrase List feature was used to increase the accuracy for LUIS’s capability to
identify Cancer Types.

Conclusion
----------

Next Steps
----------

Additional Resources
--------------------
