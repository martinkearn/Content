---
title: MOSS Architecture and Shared Services
author: Martin Kearn
description: An introduction to MOSS Architecture and Shared Services
image: https://dummyimage.com/800x600/000/fff&text=placeholder
thumbnail: https://dummyimage.com/200x200/000/fff&text=placeholder
type: article
status: published
published: 2006/06/06 09:40:00
categories: 
  - SharePoint 2007
---

# MOSS Architecture and Shared Services

I have been doing some work recently on MOSS architecture for large deployments and thought that I’d share some of my findings. MOSS architecture is very similar to SharePoint 2003 in some ways but different in a few crucial areas.

**So what is the same?**

MOSS adopts a typical 3-tier model with web servers at the front, application servers in the middle and a database server at the back where all the data and config is stored.

The front-end web servers are simple web servers that can be network load balanced to achieve additional performance and fault tolerance (like 2003). The backend database is a typical SQL 2005 database service and can be clustered as you’d expect.

**So what is different?**

The interesting bit is what goes on in the middle. With MOSS, it is mandatory to have a Shared Service Provider. This is a collection of application servers that provide shared services out to any portals or sites that need them. These services include:

*   <div>Search</div>

*   <div>Index</div>

*   <div>Audience compilation</div>

*   <div>User profiles database</div>

*   <div>My Sites</div>

*   <div>Business Data Catalogue</div>

*   <div>Excel Services</div>

Any of the above services can exist on any number of servers within the SSP. For example, for a small-ish deployment you could have them all on one box, or you could scale them out onto different boxes as you see fit. There are no rules that govern where these services reside within the SSP or how many servers you have servicing them (with the exception of the Index server - only one per SSP). For example if you want 10 search servers – got for it ... “fill your boots” as we’d say here in England!

This model is very similar to the 2003 approach with one crucial difference,  the SSP has no portal affiliation. This means that you are not forced to have a specific parent/child style portal topology in order to use Shared Services which was one of the big sticking points with Sharepoint 2003 shared services.

**So what does the ‘Portal’ actually do now?**

For starters, it is now called ‘Collaboration Portal', not ‘portal’ and it is just another type of site collection that is hosted on an IIS site (called web applications in MOSS). Portals no longer contain any application services such as search, my site etc – all these services now have to come from an SSP. This means that all you need to host a portal is a web server and a place to put the content database.

Different portals can live on their own hardware which is completely isolated from any other hardware other than the fact that it consumes services from a centrally managed SSP. Alternatively, you can put some of your portals on shared hardware and some on dedicated.

I always hear requirements around different business units wanting their own portal which has its own visual style, custom webparts, different SLAs etc . With this model, there does not have to be any affiliation between any portals or sites if it is not required.

This diagram outlines the logic of how SSP provide services that are consumed by several site collections:

![](http://static.flickr.com/104/262210767_49fbd1490c_o.png)

**So does this mean that ‘Supported Farm configurations’ have gone?**

In short, yes!

This model means that you can scale any element of the system out as you see fit. However, there are several recommended server layouts that will meet most scenarios. These broadly follow the small, medium and large models of Sharepoint 2003, but should be considered as starting points only. You can easily deviate to a different setup depending on your requirements.

**So how does all this map to physical servers?**

All three tiers (web, application, database) of the SharePoint model can be hosted on a single machine or scaled out to a huge collection of servers to meet the requirements.

Most organisations will want some level of fault tolerance and separation between server roles. Typically these kind of organisations will have at least 2 web servers running your portals and sites, at least one application server hosting all services (maybe a second for fault tolerance) and one database cluster. This diagram shows this model:

![](http://static.flickr.com/100/262210771_b4e5ff0ff2_o.png)

Larger organisations may want to have separate web servers for each of their portals, sites and my sites. They may also have multiple application servers as part of the SSP. This diagram describes this model:

![](http://static.flickr.com/100/262210768_5f023d7d07_o.png)

I hope this was usefull. There is an increasing amount of content on the public Microsoft sites now about this stuff, so be sure to have a dig around.
