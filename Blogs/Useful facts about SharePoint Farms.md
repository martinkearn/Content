---
title: Useful facts about SharePoint Farms
author: Martin Kearn
description: Useful facts about SharePoint Farms
image: https://dummyimage.com/800x600/000/fff&text=placeholder
thumbnail: https://dummyimage.com/200x200/000/fff&text=placeholder
type: article
status: published
published: 2008/03/13 09:40:00
categories: 
  - SharePoint 2007
---

# Useful facts about SharePoint Farms

I recently presented a talk at the Office Developer Conference in San Jose and I was surprised about the amount of questions we got around SharePoint server farms and topologies. Having a good understanding of what a farm is and some of the best practices around designing your farm is critical to your overall SharePoint deployment so I thought I'd put together some key facts about farms.

There are two articles on TechNet called [Plan for server farms](http://technet.microsoft.com/en-us/library/cc262936.aspx) and [Plan for availability](http://technet.microsoft.com/en-us/library/cc263044.aspx) which contain some useful information and I'd definitely recommend reading that in addition to this article.

**What is a Farm?**

_A place where lots of animals are kept and things are grown! J_ In the context of SharePoint, the term 'farm' is used to describe a collection of one or more SharePoint servers and one or more SQL servers that come together to provide a set of basic SharePoint services bound together by a single Configuration Database in SQL.

Farms can range in size from having everything (all SharePoint roles and SQL server) on one machine to scaling out every individual SharePoint serve role onto dedicated sets of servers. A farm the highest administrative boundary for SharePoint and everything that happens inside SharePoint happens in a farm.

**Server Services and Fault Tolerance**

Within a farm, there are several services that run on one or more servers. Some of these services are mandatory and some are optional. These services provide the underpinning functionality for SharePoint. The decision around which services run on which servers will have a huge impact on your overall farm architecture and performance.

It is worth noting that some services have built-in fault tolerance and some do not. In the TechNet articles listed above, the services that have fault-tolerance are described as "Roles that can be redundant".

This table describes each service:

<div>

<table border="0"><colgroup><col> <col> <col> <col> <col></colgroup>

<tbody>

<tr>

<td>

**Service** 

</td>

<td>

**Purpose** 

</td>

<td>

**Fault Tolerance** 

</td>

<td>

**Typical Server Placement** 

</td>

<td>

**Things to note / Rules**

</td>

</tr>

<tr>

<td>

Windows SharePoint Services Web Application (often referred to as 'Web Server' or 'WFE')

</td>

<td>

This service is responsible for serving the HTML to clients and routing requests to other services in the farm.

</td>

<td>

Yes, this service can exist on multiple servers.

</td>

<td>

Typically there are two or more servers running this service ion a farm. 

</td>

<td>

If you are using multiple servers, you must also implement a Network Load Balancing solution (NLB) to balance requests across the servers.

You must also use host headers for all web applications in this scenario. 

</td>

</tr>

<tr>

<td>

Office SharePoint Server Search in Query Mode (often referred to as 'Query Server') 

</td>

<td>

This service is responsible for executing search queries against a locally stored copy of the index

</td>

<td>

Yes, this role can exist on multiple servers 

</td>

<td>

This service is typically placed on each Web Application server in the farm. 

</td>

<td>

Since this service requires a local copy of the index, appropriate disk space is needed to store the index.

</td>

</tr>

<tr>

<td>

Office SharePoint Server Search in Index Mode (often referred to as 'Index Server') 

</td>

<td>

This service is responsible for indexing all of the configured content sources, creating an index and propagating it to every Query server in the farm

</td>

<td>

No, you can only have 1 Index server for each SSP in the farm. If you have multiple SSPs (not recommended – see SSP section), you can have multiple Index servers, but they have their own set of content sources. 

</td>

<td>

This service is typically placed on its own dedicated server.

</td>

<td>

It is possible to run both the Index and Query roles on the same server. However, in this configuration you cannot have multiple Query servers because when Index and Query are on the same server, propagation of the Index is disabled. 

</td>

</tr>

<tr>

<td>

Windows SharePoint Services Search

</td>

<td>

This service is basically a slimmed down version of the Office SharePoint Server Search service which combines both the Query and Index roles into a single service.

</td>

<td>

No. If you have more than one instance of this service in a farm, it will be simply do the same thing as the other servers.

</td>

<td>

This service is typically not used. If you do run this service in a MOSS farm, it is generally run on the same server as the Index server. 

</td>

<td>

If you have a MOSS farm, then the only reason for using this service would be to provide full text search of Help.

</td>

</tr>

<tr>

<td>

Excel Calculation Services

</td>

<td>

This service is responsible for performing calculations on Excel workbooks that are stored in the content databases.

</td>

<td>

Yes, many instances of this service can exist on the same farm.

</td>

<td>

This service is typically placed on each Web Application server in the farm initially.

However, this is a fairly resource intensive role; therefore, it is typically moved to a dedicated pair of servers if the load becomes too heavy.

</td>

<td>

Although this service does support fault tolerance, it does maintain session-state information, therefore users will stay on the same Excel Calculation Services server for the duration of their session.

</td>

</tr>

<tr>

<td>

Document Conversions Load Balancer Service 

</td>

<td>

This service balances document conversion requests from across the server farm.

</td>

<td>

No. An application server can only have a single Document Conversions Load Balancer Service enabled.

</td>

<td>

There is typically only one server running this service in a farm. This is typically placed on the Index server

</td>

<td>

For more information on the document conversion service click [here](http://technet.microsoft.com/en-us/library/cc263484.aspx).

</td>

</tr>

<tr>

<td>

Document Conversions Launcher Service 

</td>

<td>

This service schedules and initiates the document conversions on a server.

</td>

<td>

Yes. Multiple launchers can exist in the same farm.

</td>

<td>

This service is typically placed on each Web Application server in the farm.

</td>

<td>

If you are using multiple launcher services, they much each have the same set of document conversion applications installed.

</td>

</tr>

<tr>

<td>

Windows SharePoint Services Incoming E-Mail

</td>

<td>

This service is responsible for receiving incoming emails and placing them in email enabled lists. 

</td>

<td>

Yes, this service can run on multiple servers. However this does have additional configuration. See the 'Things to note' column for details.

</td>

<td>

This service is typically placed on each Web Application server in the farm.

</td>

<td>

There are lots of pre-requisite steps that must be configured before using this service. Read more [here](http://technet2.microsoft.com/windowsserver/WSS/en/library/445dd72e-a63b-46d0-b92d-bcf0aa9d8d061033.mspx).

</td>

</tr>

<tr>

<td>

Windows SharePoint Services Outgoing E-Mail 

</td>

<td>

This is not technically a SharePoint service but refers to an SMTP server which SharePoint send outbound email to

</td>

<td>

n/a 

</td>

<td>

n/a 

</td>

<td>

n/a 

</td>

</tr>

<tr>

<td>

Central Administration

</td>

<td>

This service enables the central administration interface that is required for farm-wide administration 

</td>

<td>

No, this service can only run on one server in the farm. 

**UPDATE: 17 May 2009**. This is an error. The CA service only runs on one server by default, however there are ways of enabling fault tolerance for this service and in actual fact you SHOULD enable fault tolerance for this service. Spence Harbar has a great article which talks all about it: [http://www.harbar.net/articles/spca.aspx](http://www.harbar.net/articles/spca.aspx) .The topic is also discussed on the 'From the field' blog from our PFE colleagues: [http://sharepoint.microsoft.com/blogs/fromthefield/Lists/Posts/Post.aspx?List=0ce77946%2D1e45%2D4b43%2D8c74%2D21963e64d4e1&ID=60](http://sharepoint.microsoft.com/blogs/fromthefield/Lists/Posts/Post.aspx?List=0ce77946%2D1e45%2D4b43%2D8c74%2D21963e64d4e1&ID=60)

</td>

<td>

Typically, this role is placed on either the Index server or one of the web servers. By default, the first server in the farm will run this service.

</td>

<td>

The Central Administration web application does run on every Web Application server in the farm; however requests are always routed to the server that is running this service.

</td>

</tr>

</tbody>

</table>

</div>

**So what is the difference between an 'Application Server' and a 'WFE Server'?**

When SharePoint is installed, you have to select one of three installation options, they are:

*   Application Server
*   Complete
*   Web Front End

There is a lot of confusion around what these options mean. An 'Application Server' is a server that is capable of running any of the services in the table above apart from the Windows SharePoint Services Web Application service. A 'Web Front End' (sometimes called a WFE) is the opposite in that it can only run the Windows SharePoint Services Web Application service. 'Complete' means that the server can run any SharePoint service.

The reason behind these options is that in some scenarios, very 'thin' web servers may be required which have a very small installation footprint. In this case, it is not desirable to install all of the DLLs etc that are required to run any service apart from the Windows SharePoint Services Web Application – these are true web servers. In my experience, this is a relatively rare scenario and only really relevant when SharePoint is being used to host high traffic internet sites.

The problem with selecting anything other than 'Complete' is that it means if you ever change your mind about what services a server can run, you'll need to re-install SharePoint on that server. For that reason alone, I would always recommend that you choose 'Complete' unless you have a very good reason to do otherwise.

This means that the terms 'Web Front End Server' and 'Application Server' are often used incorrectly. Unless your server is only running the Windows SharePoint Services Web Application service, it is an Application server. This means that the majority of servers in the majority of farms are actually Application servers.

**How should I design my web applications within my farm?**

I often get asked how to dedicate specific servers to specific web applications and the default answer is '"you can't"! J

One of the rules about SharePoint farms is that every server that is running the Windows SharePoint Services Web Application service has to serve every web application in the farm. It is not possible to say "Server X serves Web Application X". This means that when you create a new web application in Central Administration, the Windows SharePoint Services Timer on each server that is running the Windows SharePoint Services Web Application service creates the necessary sites and application pools in IIS to serve the web application.

_What about Central Administration? ....._

The Central Administration web application is no exception to this rule in that the web application does exist on every web server. However, the Central Administration service only runs on a single server and it is that server that responds to Central Administration requests. This is why the Central Administration site is always bound to a server name rather than an NLB-enabled host header.

**Cross geography farms**

One of the key questions is around how farms relate to different geographical data centres.

The quick answer is that you cannot run servers in the same farm across a WAN connection. For example you cannot have 1 web server in London and 1 in Reading if they are both in the same farm. This model will technically work, however it is unsupported because the web servers are very 'chatty' with the SQL server, therefore WAN connection are not seen as being good enough to support this.

_What happens if I have a super-fast WAN? ...._

There are some circumstances where you can span a farm over a WAN and they are as follows:

*   WFE has less than 1 millisecond(ms) latency to DB
*   WFE has less than 10 miles (16 kilometres) to DB
*   All servers on the same network segment
*   Same VLAN
*   No router switching
*   SSPs in the same datacenter
*   Servers cannot cross time zones

As a general rule of thumb you should plan to have a different farm at each data centre.

**A farm's relationship with the SSP**

One of the optional components in a farm is a Shared Service Provider (SSP). SSPs are optional applications that use a combination of web applications and server services to provide several shared services. The key thing to note here is that the SSP does not run on any single server within the farm, there is no such thing as an 'SSP server'. The SSP is actually an application that requires the following:

*   At least one server running Office SharePoint Server Search service in Index mode
*   An 'SSP Administration' Web Application.
*   2 databases in the farm's SQL server.
*   Optionally, a server running the Excel Calculation Service.

Once an SSP is configured, its shared services can be provided to all of the web applications within the local farm. An SSP can also provide services to separate farms; this is called 'cross-farm Shared Services' and is very common in large deployments. You can read more about this in my [MOSS Architecture and Shared Services article](http://blogs.msdn.com/martinkearn/archive/2006/06/06/MOSS-Architecture-_2600_-Shared-Services.aspx).

_How many SSPs should I have in my farm? ...._

Generally speaking it is best practice to have only one SSP per farm. It is possible to have multiple SSPs, but that configuration introduces a whole load of issues.

Each Web Application in your farm must get its Shared Services from a single SSP; it is not possible to pick and choose certain SSPs for certain shared services. Neither is it possible to say that a certain SSPs only provides certain services. The problem with this is that if you do have 2 SSPs, then you have 2 My Sites per user, 2 profiles per user, 2 sets of search indexes etc. Companies that do have this configuration, generally have to do a load of development work to keep the SSPs in sync with each other and make sure that user's are redirected to the correct SSP for their My Site.

There are only two common reasons for having multiple SSPs, they are:

*   Index Server scale. There is a recommended maximum of 50 million items per Index Server (more information click [here](http://technet.microsoft.com/en-us/library/cc262787.aspx)). Since you can only have 1 Index server per SSP. If you can more than 50 million items to index, you may need to split the load across multiple SSPs.
*   Privacy. Some parts of the organization may actually want to host their own My Sites, Profiles etc and the split is seen as a good thing.

**When to have multiple farms**

The final thing that I get asked a lot in relation to farms is when to make the decision to have multiple farms. The general answer is that wherever possible, you should have a single farm.

However, there are several scenarios where multiple farms make sense. I covered some of this in my [MOSS Architecture and Shared Services article](http://blogs.msdn.com/martinkearn/archive/2006/06/06/MOSS-Architecture-_2600_-Shared-Services.aspx), but here is a summary:

*   Physically separate data centres
*   Differing customisation approaches and polices
*   Differing support policies and SLAs
*   Development, staging and test environments

**That will do for now! ....**

I think that is quite enough for one article and I've covered most of the questions I hear a lot when chatting to people in the field.

I'm keen to hear about areas that I may have missed so please use the comments to log your own grey areas around SharePoint farms and maybe I'll do a part 2 covering the common themes.
