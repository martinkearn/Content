---
title: Configuring Kerberos for SharePoint 2007 - Base Configuration for SharePoint
author: Martin Kearn
description: How to configure kerberos for SharePoint 2007
image: https://dummyimage.com/800x600/000/fff&text=placeholder
thumbnail: https://dummyimage.com/200x200/000/fff&text=placeholder
type: article
status: published
published: 2007/04/23 09:30:00
categories: 
  - SharePoint 2007
---

# Configuring Kerberos for SharePoint 2007 - Base Configuration for SharePoint

(UPDATED on 04/06/07 as per feedback from two different subscribers (thank-you). Updates in Italic) 
(UPDATED on 20/08/07: My colleauge James World has just published an excellent article which is a kind of follow-up to this one. It goes a bit deeper on Kerberos basics and covers some real-world tips that relate specifically to SharePoint. Check it out here: [http://blogs.msdn.com/james_world/archive/2007/08/20/essential-guide-to-kerberos-in-sharepoint.aspx](http://blogs.msdn.com/james_world/archive/2007/08/20/essential-guide-to-kerberos-in-sharepoint.aspx) ) 

At some point during a career working with SharePoint, everyone will be given the dubious task of configuring Kerberos authentication. I've done this a few times with SPS 2003 in the past, but despite my previous experience, it is a complex and difficult task to undertake if you don't know how. As with most things, if you have the right info, it is really quite easy. This is the first of a several-part series that outlines what you need to do to enable Kerberos in a MOSS 2007 environment. This article (part 1) will focus on how to get Kerberos working for just MOSS; the later articles will then expand on that to include Excel Services, Data Connections and SQL Server 2005 Analysis Services. 

**Why Kerberos?** There are many reasons why Kerberos authentication can be used rather than the default NTLM, the main reason should be because it is faster and more secure than NTLM. It should really be the default choice for any SharePoint deployment on this basis, however in the SharePoint world the main reason is normally to get around the 'double hop' authentication issue. I am no security expert and I am sure there are better explanations out on the web somewhere (try [here](http://blogs.msdn.com/knowledgecast/archive/2007/01/31/the-double-hop-problem.aspx)) but my simple understanding of a 'double hop' is where a user authenticates to a web server and that web server then needs to impersonate the user against another service. When this happens, the user's authentication ticket is 'hopping' across two services; this is not allowed in NTLM and you will have to user Kerberos to do this. In SharePoint, 'double hops' are most commonly seen when webparts need to access other web services or databases. In MOSS 2007, the most common scenarios are around Excel Services, Data Connections and connecting to SQL Server Analysis Services cubes. (I.e. 'hopping' from the Excel services webpart to the SQL analysis cube). 

**Step 1: SPNs** The first thing you need to do in order to enable Kerberos for SharePoint is configure Service Principle Names (SPNs) for your SharePoint service accounts in Active Directory. Here lies the first stumbling block... depending on your configuration it is very hard to figure out which SPNs need to be applied to which accounts. If you are installing SharePoint properly, you'll use the 'least privilege account principle'; this basically means that each distinct service inside the SharePoint farm will have its own domain user account. These accounts should have the minimum privileges that they need to perform their jobs. There is a great document which goes into detail on each different account (8+ accounts) [here](http://technet2.microsoft.com/Office/en-us/library/f07768d4-ca37-447a-a056-1a67d93ef5401033.mspx?mfr=true), however in summary, you should have the following accounts:

*   SQL Server Service Account: Account used by SQL to run all SQL services
*   Server Farm Account
*   SSP Service Account
*   Office SharePoint Server Search Account
*   Default Content Access Account
*   User Profile and Properties Content Access Account
*   Excel Services Unattended Account
*   One account per application pool: This is typically three accounts; SSPAdministration, MySite and your main 'Portal' or 'Intranet'.

SPNs are used by Kerberos to ensure that only certain accounts have permission to delegate a specific service on a user's behalf. An SPN needs to be configured for each service and address that the account needs to delegate for. SPNs are configured by using SetSPN.exe ([download here](http://support.microsoft.com/?kbid=832769)) which is a command line provided as part of the Windows 2003 resource kit. This table outlines the commands that are required to create the right SPNs for each of the relevant SharePoint service accounts, however please bear the following points in mind:

*   Some services require the fully qualified domain name (FQDN) even if your users only use the host name. For example if user type http://portal to get to you main portal and you Active Directory is called Domain.local then the FQDN is Portal.Domain.Local
*   Some SPNs require the host name which is the FQDN without the .domain.local bit on the end. In the example above, this would simply be portal
*   For all user accounts you must include the domain prefix.
*   To make the table easier to understand, I have included an example for a single server farm in a domain called 'Domain.local' where the MOSS server is called 'Server' and I have three host headers for web applications which are called 'My Site', 'Portal' and 'SSPAdmin'. The 'least privilege account principle' has been used in this example and the accounts are fairly descriptively named.

<table>

<tbody>

<tr>

  <td><b>Command</b></td>

<td>Notes</td>

</tr>

<tr>

<td>Setspn.exe -A HTTP/%SHAREPOINTSERVERFQDN% %SERVERFARMACCOUNT%</td>

<td>%SHAREPOINTSERVERFQDN% = the FQDN of your SharePoint server's NetBIOS name (local machine name) %SERVERFARMACCOUNT% = Server Farm Account Example: Setspn.exe -A HTTP/server.domain.local domain\serverfarm</td>

</tr>

<tr>

<td>Setspn.exe -A HTTP/%MYSITEURLFQDN% %MYSITEAPPPOOLACCOUNT%</td>

<td>%MYSITEURLFQDN% = the FQDN of the host header for the My Site Web Application %MYSITEAPPPOOLACCOUNT% = The application pool account that the My Site web application uses Example: Setspn.exe -A HTTP/mysite.domain.local domain\mysiteapppool or Setspn.exe -A HTTP/server.domain.local domain\mysiteapppool</td>

</tr>

<tr>

<td>Setspn.exe -A HTTP/%MYSITEURLHOST% %MYSITEAPPPOOLACCOUNT%</td>

<td>%MYSITEURLHOST% = the host name (i.e. without the .domain.local bit) for the My Site web application %MYSITEAPPPOOLACCOUNT% = The application pool account that the My Site web application uses Example: Setspn.exe -A HTTP/mysite domain\mysiteapppool or Setspn.exe -A HTTP/server domain\mysiteapppool</td>

</tr>

<tr>

<td>Setspn.exe -A HTTP/%PORTALURLFQDN% %PORTALAPPPOOLACCOUNT%</td>

<td>%PORTALURLFQDN% = the FQDN of the host header for the main portal or intranet Web Application %MYSITEAPPPOOLACCOUNT% = The application pool account that the Portal web application uses Example: Setspn.exe -A HTTP/portal.domain.local domain\portalapppool</td>

</tr>

<tr>

<td>Setspn.exe -A HTTP/%PORTALURLHOST% %PORTALAPPPOOLACCOUNT%</td>

<td>% PORTALURLHOST % = the host name (i.e. without the .domain.local bit) for the Portal web application %MYSITEAPPPOOLACCOUNT% = The application pool account that the Portal web application uses Example: Setspn.exe -A HTTP/portal domain\portalapppool</td>

</tr>

<tr>

<td>Setspn.exe -A HTTP/%SSPADMINURLFQDN% %SSPADMINAPPPOOLACCOUNT%</td>

<td>% SSPADMINURLFQDN % = the FQDN of the host header for the SSP Administration Web Application % SSPADMINAPPPOOLACCOUNT % = The application pool account that the SSP Administration web application uses Example: Setspn.exe -A HTTP/sspadmin.domain.local domain\sspadminapppool</td>

</tr>

<tr>

<td>Setspn.exe -A HTTP/%SSPADMINURLHOST% %SSPADMINAPPPOOLACCOUNT%</td>

<td>% SSPADMINURLHOST % = the host name (i.e. without the .domain.local bit) for the SSP Administration web application % SSPADMINAPPPOOLACCOUNT % = The application pool account that the SSP Administration web application uses Example: Setspn.exe -A HTTP/sspadmin domain\sspadminapppool</td>

</tr>

</tbody>

</table>

**Step 2: Trust for Delegation** In addition to setting the SPNs for each of your service accounts, you also need to trust each of the computer accounts and some of the service accounts for delegation. Trusting for delegation means that the accounts are allowed to delegate on a user's behalf. In order to trust for delegation you need to open Active Directory Users and Computers as a user with domain administration rights and follow these instructions

*   Repeat the process for each of the following
    *   MOSS Server (Computer Account)
    *   SQL Server (Computer Account)
    *   FarmService (User Account)
    *   MySiteAppPool (User Account)
    *   SSPAdminAppPool (User Account)
    *   PortalAppPool (User Account)
*   Locate the account and click 'properties'
*   Navigate to the 'Delegation' tab
*   Choose 'Trust this user/computer for delegation to any service (Kerberos)'

**Step 3: Enable Kerberos on your web applications** In SharePoint 2007, the switch between Kerberos and NTLM is very simple and is undertaken via Central Administration. If you are creating your farm from scratch, be sure to set Central Administration itself to use Kerberos which you can set as part of the 'SharePoint Products and Technologies Configuration Wizard', however if the farm is pre-created you can easily enable Kerberos by following these steps:

*   Open Central Administration
*   Navigation to Application Management > Authentication Providers
*   Choose the web application you wish to configure from the drop-down in the top right corner (this includes the Central Administration web application)
*   Click on 'Default'
*   Set the authentication to Negotiate (Kerberos)
*   IISRESET

**Step 4: Enable Kerberos on your SSP** In this step you enable Kerberos on your SSP. Follow these steps:

*   Open a Command Prompt and navigate to your '12\Bin' directory (normally c:\program files\common files\microsoft shared\web server extensions\12\bin)
*   Run 'STSADM.exe -o SetSharedWebServiceAuthn -negotiate'

**Step 5: Component Services Configuration** This is one of the lesser documented steps. You need to set various permissions in Component Services. Follow these steps:

*   Open Component Services on the MOSS server
*   Navigation to Component Services > Computers > My Computer
*   Click on Properties (for My Computer) > Default Properties > Default Impersonation Level = Delegate (see [http://support.microsoft.com/kb/917409](http://support.microsoft.com/kb/917409))
*   Navigate to Component Services > Computers > My Computer > DCOM Config > IIS WAMREG Admin Service
*   Click on Properties (for IIS WAMREG Admin Service) and navigate to the Security tab
*   Edit Launch and Activate Permissions
*   Grant all three of your application pool account 'Local Activation' permissions (see [http://support.microsoft.com/kb/920783](http://support.microsoft.com/kb/920783)). In our example, these accounts would be domain\MySiteAppPool, domain\SSPAdminAppPool, domain\PortalAppPool

**How do you know it has worked?** The thing with Kerberos is that it can be difficult to see if it is working properly. There are several things you can do to check:

*   Do your web applications work from a client computer? If they do, then this is a good sign
*   Keep an eye on your System event log on both your MOSS and SQL servers. Kerberos related errors are logged here.
*   Make sure all the servers in the loop (MOSS, SQL and Domain Controllers) have the same time set. Inconsistent time settings are one of the primary causes of Kerberos related issues.

If you have further difficulties, try these articles:

*   Troubleshooting Kerberos Errors: [http://www.microsoft.com/technet/prodtechnol/windowsserver2003/technologies/security/tkerberr.mspx](http://www.microsoft.com/technet/prodtechnol/windowsserver2003/technologies/security/tkerberr.mspx)
*   How to configure a Windows SharePoint Services virtual server to use Kerberos authentication and how to switch from Kerberos authentication back to NTLM authentication: [http://support.microsoft.com/?kbid=832769](http://support.microsoft.com/?kbid=832769)
*   How to use Kerberos authentication in SQL Server: [http://support.microsoft.com/kb/319723/](http://support.microsoft.com/kb/319723/)
