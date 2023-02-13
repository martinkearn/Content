---
title: How to create a slipstream installation for MOSS with SP1
author: Martin Kearn
description: How to create a slipstream installation for MOSS with SP1
image: https://dummyimage.com/800x600/000/fff&text=placeholder
thumbnail: https://dummyimage.com/200x200/000/fff&text=placeholder
type: article
status: published
published: 2008/01/14 09:40:00
categories: 
  - SharePoint 2007
---

# How to create a slipstream installation for MOSS with SP1

_Update (8th March): The product team have now released a MOSS with SP1 download. Read all about it here:_ [_http://blogs.msdn.com/sharepoint/archive/2008/03/07/moss-2007-with-sp1-slipstream-officeserverwithsp1-exe-released.aspx_](http://blogs.msdn.com/sharepoint/archive/2008/03/07/moss-2007-with-sp1-slipstream-officeserverwithsp1-exe-released.aspx) 

In order to install MOSS 2007 on Windows Server 2008 you will need to install 'MOSS with Service Pack 1' (this includes WSS with Service Pack 1). This is known as a slipstream installation and it contains the original image for MOSS/WSS RTM and the MOSS/WSS SP1 files. The problem is that this 'all in one' slipstream for MOSS is not available for download just yet, so you have to create your own slipstream image. Fortunately, this is very easy to do, just follow these steps:

**1- Download MOSS and WSS Service Pack 1 files** You can download the MOSS and WSS SP1 files from here. You will need both service packs for a MOSS installation: WSS SP1: [http://www.microsoft.com/downloads/details.aspx?FamilyID=4191a531-a2e9-45e4-b71e-5b0b17108bd2&DisplayLang=en](http://www.microsoft.com/downloads/details.aspx?FamilyID=4191a531-a2e9-45e4-b71e-5b0b17108bd2&DisplayLang=en) MOSS SP1: [http://www.microsoft.com/downloads/details.aspx?FamilyID=ad59175c-ad6a-4027-8c2f-db25322f791b&DisplayLang=en](http://www.microsoft.com/downloads/details.aspx?FamilyID=ad59175c-ad6a-4027-8c2f-db25322f791b&DisplayLang=en) Once downloaded you will have two executable files: _Wssv3sp1-kb936988-x86-fullfile-en-us.exe_ and _Officeserver2007sp1-kb936984-x86-fullfile-en-us.exe_

**2 - Extract the exe's** You now need to extract the exe's, to do this enter this command (this assumes you've downloaded the exe's to your c:\, change if not) C:\Wssv3sp1-kb936988-x86-fullfile-en-us.exe /extract:c:\wsssp1extract C:\Officeserver2007sp1-kb936984-x86-fullfile-en-us.exe /extract:c:\mosssp1extract

**3 - Add the extracted files to your RTM MOSS installation** Once you've extracted the service pack exe's you need to copy the extracted contents to the /Updates folder on the MOSS CD. The easiest way to do this is take a copy of your MOSS RTM CD (or extract your ISO image) onto your file system and simply copy the contents of both c:\wsssp1extract and c:\mosssp1extract into the \Updates folder (both the WSS and MOSS service pack files go into the same folder, do not use sub-folders).

You're done; you will now be able to install MOSS as usual on your Windows Server 2008 machine. If you need a guide for doing this, check out my other post here: [http://blogs.msdn.com/martinkearn/archive/2007/03/28/how-to-install-sharepoint-server-2007-on-a-single-machine.aspx](http://blogs.msdn.com/martinkearn/archive/2007/03/28/how-to-install-sharepoint-server-2007-on-a-single-machine.aspx) . This guide was written for Windows Server 2003 so there may be some minor variations.
