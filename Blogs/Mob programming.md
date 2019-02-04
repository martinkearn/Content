---
title: Mob Programming
author: Martin Kearn
description: Mob programming is like 'pair programming' but with a mob rather than just two folks. I did this recently at a hack and it was great. Here are some tips based on what i learnt
image: https://github.com/martinkearn/Content/raw/master/Blogs/Images/kids-excited-at-a-laptop.jpg
thumbnail: https://github.com/martinkearn/Content/raw/master/Blogs/Images/kids-excited-at-a-laptop-thumb.jpg
type: article
status: draft
published: 2019/02/04 14:00:00
categories: 
  - Productivity
  - Visual Studio
---

# Mob Programming

Last week I was at an [Open Hack](https://www.microsoftevents.com/profile/form/index.cfm?PKformID=0x5952150abcd) which involved working with four other folks to collaboratively solve coding challenges.

There were times where we used the 'mob programming' approach and I wanted to share my experience of what worked, and what I'd do differently next time.

## What is Mob Programming?

Mob programming is the same as [pair programming](https://en.wikipedia.org/wiki/Pair_programming) but with a mob of people rather than just two. In our case, there was five of us; [Jamie Dalton](https://twitter.com/daltskin), [Lynn Orrell](https://twitter.com/Lynn_Orrell), [Martin Simecek](https://twitter.com/deeedx) and [Maho Pacheco](https://twitter.com/mictlan).

The basic idea according to [wikipedia](https://en.wikipedia.org/wiki/Mob_programming) is that the 'mob' all work on the same code, at the same time, on the same computer with one person 'driving' and everyone else verbally contributing. 

The way we did it was a little different.

We all used [Visual Studio Live Share](https://code.visualstudio.com/blogs/2017/11/15/live-share) so rather than having one person driving, we could all dive in and modify code which was hosted on one person's machine (the 'host'). This worked across a variety of different setups, including:

* Visual Studio 2017 on Windows
* Visual Studio 2019 on Windows
* Visual Studio Code on Mac
* Visual Studio Code on Windows

We were all physically located around a table so could easily communicate verbally, but I guess in theory you could do this remotely using some kind of video conferencing service.

## What worked?

In general, I think everyone found the mob programming experience good, but here are some of the key benefits and tips.

**Easy Switching**: Whilst it was technically possible for everyone to code at the same time, we tended to switch the main person coding because it can get confusing if more than one person is making updates at the same time. Unlike pair programming where you only have one physical keyboard, VS Live Share means everyone uses their own machine and enables you to switch the 'driver' really easily and frequently as people come up with ideas or suggestions. We did this by simply saying "let me drive for a bit" whilst the others watched and verbally contributed.

**Learn by watching**: Mob programming allows you to watch as others code so you can secretly pick up tips and tricks from you team mates.

**The wisdom of crowds**: It is common knowledge that several minds will find a solution to a problem quicker than one. This is absolutely true of mob programming. We found that we were racing through the code much faster than we'd probably had done as individuals working on separate tasks and integrating.

**No tasks**: We were coding a bot project which does not always lend itself to being broken down into tasks. Mob programming allowed us to take on bigger tasks as a group rather than trying to distribute the work and integrate it.

**Avoid Git merge hell**: With VS Live Share, the code itself is actually hosted on one persons machine (the 'hoster') which reduces the potential for merge conflicts when the code is committed.

**Avoid leaving people behind**: It is quite common on a collaborate coding exercise for folks to get left behind as the more experienced folks push on with the solution. Mob programming can help with this because everyone is seeing the code as it is being written and has the option to contribute. Observing the the code being written really helps with understanding compared to pulling the last commit and trying to work out what changed.

## Downsides

As with any productivity technique, there are always downsides.

**Setup**: VS Live Share has to be installed and setup on every machine and there is a small learning curve on how to use it effectively. It does not take long but it is something to consider.

**Collaborate and learn vs JFDI**: Mob programming is only suitable for scenarios where you want group input and feel ownership for the code, such as a hack or open hack. Mob programming can potentially slow the more experienced folks down. So if the goal is to JFDI, you might be better to distribute the work and have the more experienced team mates do the more complex bits. (If you don't know what JFDI means, look it up! ;) )

**Debug**: VS Live Share lets you debug the code, but will not show you the application itself or in our case let us all see the Bot Emulator. We resolved this by having a monitor we could all see with this on.

## What we'd do differently next time

The experience worked very well as a whole but this is what we'd do different next time.

**Sync VS Versions**: Though VS Live Share technically works on every version of Visual Studio, one of our group had Visual Studio 2019 (preview) which we think may have caused some issues with it crashing etc

**Multiple mobs**: A mob of five was probably too many in practice for both VS Live Share and the verbal communication that is required. I think a group of three would be the ideal mob size.

## In Summary

Mob programming is a great way to work in a team and have everyone feel like they are part of the process. If you are at a hackathon or Open Hack, I'd strongly recommend you give it a try.

You don;t have to use [Visual Studio Live Share](https://code.visualstudio.com/blogs/2017/11/15/live-share) but I cannot imagine how practical mob programming is without a tool like this.
