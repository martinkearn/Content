---
title: How to install SharePoint 2007 on a single machine
author: Martin Kearn
description: An in-depth guide for installing Sharepoint 2007 on a single machine
image: https://dummyimage.com/800x600/000/fff&text=placeholder
thumbnail: https://dummyimage.com/200x200/000/fff&text=placeholder
type: article
status: published
published: 2007/03/28 09:40:00
categories: 
  - SharePoint 2007
---

# How to install SharePoint 2007 on a single machine

One of my first ever blog articles (and by far most popular to date) was a set of instructions on how to install Beta1 of SharePoint Server 2007 on a single machine. I removed this article because it was too much of an overhead updating it with the various Betas and the official guides were being developed. Now that SharePoint is RTM, I do still get a lot of questions from customers on how to do a simple installation of SharePoint (with SQL 2005) on a single machine to be used for a stand-alone development, demonstration or simple 'play-pen' server (normally on a virtual machine). This guide will outline all of the main steps to setup such an environment. Please bear in mind that this is just an unofficial guide to getting SharePoint 2007 installed quickly and easily in a demo / test environment. This guide will not necessarily observe best practices with regard to security etc. For production setups, you should seek guidance from the official documentation which is available on TechNet ([<span style="font-size: 10pt;">http://technet2.microsoft.com/Office/en-us/library/3e3b8737-c6a3-4e2c-a35f-f0095d952b781033.mspx?mfr=true</span>](http://technet2.microsoft.com/Office/en-us/library/3e3b8737-c6a3-4e2c-a35f-f0095d952b781033.mspx?mfr=true)). **Pre-Install** There are several things that you must do before you even insert the SharePoint 2007 CD they are:

*   Install Windows 2003 R2 with the latest service pack (2 at time of writing) and all of the latest Windows Updates.

_NOTE: Please do not use NewSID to change the SID of the machine if you are using a copy of another VM, this breaks things in SharePoint. My advice is to build Windows from fresh or to use Sysprep if you are using a copy of a VM._

*   Join your machine to a domain or create a domain by running DCPromo.exe from the Start > Run dialog.
*   Install the .net frameworks v3.0 and v2.0 from Windows Update. You can also download the full redistributable packages if your server is not online.
*   Install Windows 'Application Server' from Add/Remove Programs in Control Panel with default settings
*   Prepare a service account in your active directory domain to use for all Sharepoint services.

_NOTE: Do not use the main domain\administrator account. This causes a problem if ever you wish to install Project Server 2007 on the same machine._

*   Give your service account local administrator rights and logon as this account throughout the entire installation process.
*   Install SQL 2005 (and latest service pack) with typical settings.
*   <div>Assign your service account to the 'Security Administrators' and 'Database Creators' server roles in SQL server (You will need to use SQL Server Management Studio).</div>

**Base SharePoint Server Install** You are now ready to install SharePoint 2007 itself, follow these steps:<span style="text-decoration: underline;"></span>

*   Login as your service account
*   Insert your CD (or attach your ISO image) and run setup.exe if it does not autorun.

_NOTE: If you get an error about web service extensions here, ensure that 'ASP.net V2.0.50727' web service extension is allowed in IIS. If it is not in the list, perform a 'repair' on .net 3.0 framework using add/remove programs and then the web service extension will appear in the list. This is caused when IIS is installed after the .net framework_

*   Enter your CD key and accept the license agreement.
*   Choose 'Advanced' on the installation type dialog.

_NOTE: The definition of 'Advanced' means that you are using full SQL server (which may or may not be on the same machine). If you had selected 'Basic' then it would have installed the cut down version of SQL (MSDE)._

*   Select 'Complete' on the Server Type screen and click 'Install Now'. The setup will now commence and you'll get a blue progress bar.
*   Once installed you will get a screen with a check box that reads "Run the SharePoint products and Technologies Wizard now". Ensure this is ticked and click 'Close'.
*   After a short pause, you'll get a 'Welcome' screen. Click 'Next'.
*   You will get a warning that the wizard is about to reset several services, click 'Yes'.
*   You'll be asked about the farm configuration, select to 'No, I want to create a new server farm'.
*   Provide the database server (your server name) and your account details (account in the domain\user format). Leave the database name as the default. Click 'Next'.
*   Leave the authentication mode as 'NTLM', set a specific port number is desired (not required) and click 'Next'.

_NOTE: In a production environment, you would most likely use Kerberos where possible (if your infrastructure supports it)._

*   You'll get a summary screen; click 'Next' to kick-off the process.

_NOTE: If it fails here, it is most likely that you do not SQL setup correctly. Ensure your service account is in the right groups. Please also note that this section can take a very long time, especially step 2 (up to 45 minutes)._

*   You'll get a success screen at the end, click 'Finish'.
*   The wizard will attempt to load the central administration window. You may need to login here, use your service account. You may also get prompted to add the site to your trusted sites; go ahead and do that.

_NOTE: This authentication prompt is caused by the secure version of IE on Windows 2003 Server. You can turn if off by modifying the security settings in IE._ **Services on Server Configuration** The first bit of configuration to do is set your server to host all services. You do not strictly have to enable all of these services, but I find it helps if you are using the machine to test / investigate functionality.

*   When the Central Administration screen appears, go to 'Operations' tab, then 'Services on Server'.
*   Start the 'Document Conversions Load Balancer Service'.
*   Start the 'Document Conversions Launcher Service', you'll have to choose the 'Load Balancer Server'; there should only be one option. If there are no options, ensure that the 'Document Conversions Load Balancer Service' has been started.
*   Start the 'Excel Calculation Services'.
*   <div>Start the 'Office SharePoint Servers Search' service, observing the following guidelines:</div>

    *   Tick both Query and Indexing check boxes
    *   Specify a contact email address (this can be any address)
    *   Enter your service account in the 'Farm Search Service Account' section
    *   Accept all other defaults and click 'Start'
*   Leave all remaining services in their default configuration

**Web Application Setup** The next stage is to create the 3 web applications that will be required to host the basic set of sites for a typical deployment, these are:

*   Shared Service Provider Administration Site (Recommended to be called 'SSPAdmin')
*   My Site Host (Recommended to be called 'MySite')
*   <div>The Main Intranet (or 'Portal') Site (Recommended to be called 'Intranet')</div>

It is much simpler if all of these sites are on port 80 in IIS; this means that you do not have to remember to enter the ports all of the time. However having all three sites on port 80 means that each needs their own Host Header (required by IIS to differentiate between sites on the same port). The simplest way to do this is to create new 'Host (A)' records in DNS for each of your three sites. These should point to the IP address of your server; to do this follows these steps:

*   Open the DNS Management tool from Administration Tools on your domain controller
*   Navigate to your DNS zone
*   Create new 'Host (A)' record
*   Enter the Host header (i.e. 'SSPAdmin', 'MySite' or 'Intranet') for the site and the IP address of your server
*   <div>Click 'Add Host' and repeat for each of the three sites</div>

Now the DNS entries are configured, we can create the three web applications in SharePoint; follow these steps for all three of your web applications (i.e. 'SSPAdmin', 'MySite' or 'Intranet'):

*   In Central Administration, go to the 'Application Management' tab
*   Click 'Create or Extend Web Application' and then click 'Create a new Web Application'
*   <div>Fill out the new web application screen observing the following points:</div>

    *   Change the New IIS Site description to read something like 'SharePoint – 80 - <Host header name>' where <Host header name> is the name of the web application your are creating (i.e. 'SSPAdmin', 'MySite' or 'Intranet')
    *   Ensure the 'Port' is set to 80
    *   Set the 'Host Header' to match the DNS record you created (i.e. 'SSPAdmin', 'MySite' or 'Intranet')
    *   Change the 'Application Pool Name' to match the 'New IIS Site Description'
    *   Enter your service account for the Application Pool account settings
    *   Change the 'Database Name' to read something like 'WSS_Content_<Host header name>' where <Host header name> is the name of the web application your are creating (i.e. 'SSPAdmin', 'MySite' or 'Intranet')
    *   Leave all other settings on default and click 'OK'
*   <div>Repeat for all three web applications (i.e. 'SSPAdmin', 'MySite' or 'Intranet')</div>

**Shared Service Provider Setup** The next stage is to create the Shared Service Provider (SSP). The SSP is required in order to provide several key services such as Search or My Site. You can read more about SSP on my blog article about it [here](http://blogs.msdn.com/martinkearn/archive/2006/06/06/MOSS-Architecture-_2600_-Shared-Services.aspx). To configure the SSP, follow these steps:

*   In Central Administration, go to the 'Application Management' tab
*   In the 'Office SharePoint Server Shared Services' section, click 'Create or Configure This Farms' Shared Services'
*   Click 'New SSP'
*   <div>Fill out the 'New Shared Services Provider' screen observing the following guidelines:</div>

    *   For the 'SSP Administration Site' web application (the first one you get asked for), choose the web application that you created earlier (suggested name was 'SharePoint – 80 - SSPAdmin')
    *   For the 'My Site Location' web application (the second one you get asked for), choose the web application you created earlier (suggested name was 'SharePoint – 80 - MySite')
    *   Enter your service account for the 'SSP Service Credentials'
    *   Leave all other settings on default and click 'OK'
*   <div>The creation of an SSP can take some time (up to 1 hour on a virtual machine). When it is finished you will see a 'Success!' screen, Click OK.</div>

**Collaboration Portal Site Collection Setup** The next stage is to create a collaboration portal which is one of the more feature-filled site types and represents a typical intranet environment. To do this, follow these steps:

*   In Central Administration, go to the 'Application Management' tab
*   In the 'SharePoint Site Management' section, choose 'Create Site Collection'
*   <div>Fill out the 'Create Site Collection' observing the following guidelines:</div>

    *   Ensure you have selected the 'Intranet' web application you created earlier (suggested name was 'Intranet')
    *   Give your site a title ('Intranet' is suggested)
    *   In the 'Template Selection' section, choose 'Collaboration Portal' from 'Publishing' tab
    *   Enter you service account for the 'Primary Site Collection Administrator'
    *   Leave all other settings on default and click 'OK'
*   <div>When the 'Top-Level Site Successfully Created' message appears you have created the site, simply click the link that is provided (something like http://intranet)</div>

**Configure Indexing** The final step of the process is to configure indexing so that you have some search results. Though this step is optional, it is recommended as it will enable you to use the powerful search capabilities of SharePoint. To configure the index, follow these steps:

*   In Central Administration, click the 'SharedServices1' link on the left-side navigation (or whatever you name your SSP)
*   When the SSP Administration site appears, click on 'Search Settings' in the 'Search' section
*   On the 'Configure Search Settings' page, click 'Content Sources and Crawl Schedules'
*   Edit the 'Local office SharePoint Server Sites' content source by hovering your mouse over it and choosing 'Edit'
*   <div>Fill out the 'Edit Content Source' observing the following guidelines:</div>

    *   Set a full crawl schedule to be at least once a day
    *   Set a incremental crawl schedule for every 10 minutes
    *   Tick the 'Start Full Crawl of this Content Source' tick-box
    *   Click 'OK'
*   <div>A crawl will now start. Initial crawls normally take up to 10 minutes.</div>

The process is now complete. User should be able to access the main collaboration portal from http://intranet (or whatever you called the DNS record). I hope this was useful, please comment with any errors or amendments
