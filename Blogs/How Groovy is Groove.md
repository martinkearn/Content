---
title: How Groovy is Groove
author: Martin Kearn
description: A look at Groove
image: https://dummyimage.com/800x600/000/fff&text=placeholder
thumbnail: https://dummyimage.com/200x200/000/fff&text=placeholder
type: article
status: published
published: 2007/01/23 09:40:00
categories: 
  - SharePoint 2007
  - Groove
---

# How Groovy is Groove
As some of you may have noticed from my lack of recent posts, I’m working on a fairly intense MOSS deployment project right now (apologies, will blog as much as I can). We are using Groove 2007 as our primary collaboration tool within the project and I thought I’d share my thoughts on exactly how Groovy we have found Groove to be......

**So what is Groove?**

OK, so I could spend a whole article explaining what Groove is (and is not), but that is not my intention here. Therefore if you have never ‘got Groovy’ before, you might want to find an article that gives you the high level view like this one from [Mark Wilson](http://www.markwilson.co.uk/blog/2006/11/office-groove-2007-overview.htm) or you could try [Mark Olson's blog](http://blogs.msdn.com/marco/default.aspx) (Groove product team) or grab the [trial version from here](http://ukireland.trymicrosoftoffice.com/product.aspx?family=officelivegroove&culture=en-GB).

For those that require a refresher, Groove is a peer-to-peer client-side collaboration application that enables network and organisation independent collaboration. In a nutshell, Groove allows you to share files and other bits with people from different organisations across whichever network you like (Inc public internet). Groove does have a server aspect, but you can work perfectly well just using the client.

**Project background**

We are deploying MOSS to a large customer and therefore, we have about 10 full-time team members working on the design and planning for the project. 5 of the team members are from Microsoft, 3 from 3 different partners and 2 from the customer (which makes 5 different organisations in total)

In terms of network, we were all connected by a very un-reliable wireless LAN (up and down throughout the day) or public internet via home, hotels or MS offices.

**What has worked well?**

This is what has worked really well:

· Using Groove has really cut down on all the inter-team emailing we did. Everyone knows that the latest copy of something is in Groove and that is all there is to it. We spent much less time chasing and waiting for documents that if we’d used a file system.

· The IM and presence capabilities were really useful. Although all the Microsoft guys have communicator, being able to IM the partner/customer team members and see if they are online or actively using the workspace has been very useful indeed.

· We’ve made use of the ‘notes’ tool to store contact information. It would have been nice to be able to sync this with Outlook, but the plain text notes served their purpose.

· By everyone having Groove, it serves as an unofficial backup repository because essentially every user has a copy of all document on their local machine

· The peer-to-peer nature of Groove means that physical location really is not important, so long as you have at least an internet connection (which is great for me because I live about 80 miles from the customer office J )

· We have had lots of ‘new starters’ on the project and it is very good to be able to say “everything is in Groove” in terms of giving them project documentation

· We were able to store remote desktop connection files to all our dev servers (13 of them). By having the RDP files in Groove with passwords embedded. No-one needs to worry about server names or passwords; they just double-click the RDP file.

· Other people from within Microsoft have wanted to take a look at our designs on several occasions. Being about 50Mb in total, email has been out of the question, but the capability to add them as a temporary Groove user and allow them to browse the workspace has been really useful.

**Not so good**

There have been a few areas where the experience has been slightly lacking. They are generally quite minor, but here goes...

· There is a feeling that the capabilities and Office integration could be better. Don't get me wrong .. there are lots of things in Groove that do integrate well with Office and lots of good collaboration tools, but there are areas that are lacking a bit. For example, it would have been great to have the calendar and contacts capabilities linked directly into Outlook. I have also missed the document check-out & version history features that SharePoint provides and the more advanced real-time collaboration features in Office Communicator or Vista Meeting Space.

· We use a WSS 2.0 site as our official deliverable repository and Groove has no direct integration which means we have to manually copy everything up there on a weekly basis. Groove 2007 does have very good integration with WSS 3.0, but not WSS 2.0 so for our puposes we had a gap here. Clearly, as WSS 3.0 becomes more popular, this problem will go away, but for now it is one extra task.

· If you are using Groove directly (i.e. opening and editing directly out of Groove), you have to confirm each time you save a document and everyone else gets a notification – this can get very annoying.

**What I’d do different next time**

Lessons learnt and things that I’d do differently on the next project:

· Out of the team, I think that I’ve been the only person brave enough to work directly from Groove. I.e. I have not maintained a local copy of my documents, but have opened and edited directly out of Groove. This ensures that the latest copy of all my content is always in Groove. Some of the other guys have not taken this approach and have tended to work locally and then do a manual upload once they have reached a certain milestone. The down side to this is that a) they have to remember to upload and b) The latest content is not always in Groove. Next time round, I’d probably try to enforce a practice where everyone works directly from Groove rather than maintaining local copies and uploading.

· By using Groove, we’ve put together a structure that probably reflects an average MOSS project. Next time round, I’d probably save this as a template and start the workspace based on that template.

· I work harder on encouraging all users to use Groove. In my case, this would include the staff from the customer who could not easily install Groove on their production machines (had to rely on using their home PCs and copying every night).

· I’d probably look to establish a collection of links to key documents for the purpose of on-boarding

**Conclusion**

Groove does feel like a young product and lacks some of the key features that you take for granted with other Office products and especially SharePoint. There are lots of great collaboration features and ideas in the product and good integration with WSS 3.0, but you are often left thinking “if only it did X, Y or Z”.

Having said that, the fact that Groove is peer-to-peer and has no network dependencies has really paid dividends on this project as it has enabled cross-organisation collaboration for the whole team. This feature alone more than makes-up for Groove’s short-comings around collaboration features and Office integration. I really feel that collaboration would not have been anywhere near as straight forward without it.

Added to the above, Groove has given the whole team collaboration tools that have been invaluable (such as chat, IM, presence, calendaring etc) and whilst they do not match the might of SharePoint & co, they have been useful on the project and we’d have been that little bit less-collaborative without them.

I would certainly look to use Groove again on future projects and if the “What I’d do different next time” learning’s are observed, it makes for a great tool to help run small project teams and I’d recommend it without hesitating.

I hope this was useful.
