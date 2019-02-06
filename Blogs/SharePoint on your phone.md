---
title: SharePoint on your phone
author: Martin Kearn
description: How to get SharePoint 2007 on your phone
image: https://dummyimage.com/800x600/000/fff&text=placeholder
thumbnail: https://dummyimage.com/200x200/000/fff&text=placeholder
type: article
status: published
published: 2006/04/28 09:30:00
categories: 
  - SharePoint 2007
  - Apps
---

# SharePoint on your phone

One of the interesting new features in MOSS 2007 is the support around mobile devices. In this article, I’ll aim to give you a quick overview of how you can get the next version of SharePoint on your phone.
What are Mobile Views?
Every list and library in MOSS 2007 or WSSv3 is capable of hosting ‘Mobile Views’. These are standard views of lists or libraries that an administrator has defined as being mobile enabled. You can also view individual list items in mobile form.

This is a picture of the mobile view for a standard task item.

![](http://static.flickr.com/87/262214211_f082d16194_o.png)
 
## How do you navigate to these Mobile Views?
Every site has a mobile home. You can get to it by simply appending an ‘m’ on the end of the URL. For example the mobile view for the following URL http://<server>/TestSite can be accessed by http://<server>/TestSite/m.  
From the mobile home you can navigate through the lists, libraries and list items in the site.

This picture shows the mobile home of a standard team site

![](http://static.flickr.com/98/262214210_a98e0b4022_o.png)

## How do I configure Mobile Views?
Each list has a default mobile view, but you can configure any normal view as being ‘mobile’. This is all done through the same interface that you’d use to modify view filters, sorts etc. 

This picture shows the UI for enabling a normal view as being ‘mobile’
![](http://static.flickr.com/119/262214212_a154ac5ff4_o.png)
 
When you access a list using your device, you can use a drop-down to choose the most appropriate view. However, generally speaking the most sensible views for a mobile device are pre-configured as the default mobile view. For example, the default for a tasks list is ‘my tasks’.

## How do I play with this more?
The easiest way to investigate this further is to get the Microsoft Windows Mobile emulator from http://msdn.microsoft.com/mobility/windowsmobile/downloads/emulatorpreview/default.aspx. Follow the instructions on the page for getting onto Betaplace and downloading the emulator.
Once you have the installation file, I suggest that you install it directly onto your SharePoint server. This way it is much easier to configure the networking between the server and the emulator.
The emulator will allow you to emulate a smart phone or PDA. I found that the PDA is easiest to work with. Follow these instructions to get a mobile view of SharePoint on your PDA emulator (you should be able to adapt them for SmartPhone if you are familiar with the SmartPhone interface)
* Install the emulator and start the ‘Emulate Pocket PC-WM 2003 SE(Cold Boot)’ from your start menu
* Map your real network card to the device by going to File > Configure > Network Tab > Enable ‘NE2000 PCMCIA adapter and bind to’ and click ‘OK’
Note: All of the following instructions are for the device interface itself, not the emulator application, unless otherwise stated.
* Go to ‘Settings’ from the start menu 
* Click on ‘Network Cards’
* Choose ‘The Internet’ on the ‘My network card connects to’ drop-down
* Click on ‘NE2000 Compatible Adapter’
* Set your IP and Name Server settings in accordance with your environment (it needs to be in the same subnet as your server). 
* Click ‘OK’ until you are back to your home screen
* On the emulator application, disable and re-enable the network card mapping from File > Configure > Network Tab > Enable ‘NE2000 PCMCIA adapter and bind to’ (this is the equivalent to unplugging the PCMCIA card, you’ll see the icon change on the device screen)
* On the device, wait for the network icon to show that it has re-connected.
* You will get a new icon at the top of the device screen. Click on it and choose ‘This network card connects me to the internet’
* Load Internet Explorer from the start menu and type your portal URL with an /m at the end
* Enter login credentials and the mobile home should appear
 
 I hope this was interesting.
