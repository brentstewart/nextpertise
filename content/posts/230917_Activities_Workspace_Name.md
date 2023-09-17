---
title: "Gnome Activities Workspace Name Extension"
description: "A Gnome extension, compatible with Pop! that lables workspaces"
author: "Brent Stewart"
date: "2023-09-17T14:34:35-04:00"
markup: 'mmark'
math: false
draft: false
Victor_Hugo: "true"
picture: "linux"
Focus_Keyword: "Gnome Workspace Labels"
youtube: ""
github: ""
refs: ["https://extensions.gnome.org/extension/5311/activities-workspace-name/","https://github.com/ahmafi/gnome-activities-workspace-name"]
tags:
  - "Linux"
---

# Cool Extension
The __Activities Workspace Name__ allows you to change the "Activities" label in the top left of a Gnome desktop to reflect a name for each workspace.  I find it really useful.

My current setup is to use workspaces to seperate the different types of activities, so I have one for work, one for writing, one for development, and one for learning.  I have a 4K display, run Pop! tiling or use that tiling extension in Gnome, and use the to left corner to flip desktops. Maybe I'm a little too obsessive, but I've wanted a way to label those spaces to make them easy to identify.

I found this extension, available on the [Gnome extension site](https://extensions.gnome.org/extension/5311/activities-workspace-name) and at [Github](https://github.com/ahmafi/gnome-activities-workspace-nam), and it does a great job of filling this gap.  Below is a sample with the Workspace label applied (I zoomed in):

![Sample](/WorkspaceLabel.png#floatsmallright)

The easy way to add the extension is to go to the [gnome extension site](extensions.gnome.org) and enable it.  There are two ways to set workspace names: long clicking on the label or via command line.  Long clicking I eventually got to work, sometimes.  Sometimes a long click is results in showing all the workspaces, but this may be an artifact of runnign on Pop!.  Option #2, which is easy and always works for me, is to set it via the command line.  Here's the command I use to name my four workspaces.

    gsettings set org.gnome.desktop.wm.preferences workspace-names "['Work', 'Writing', 'Dev','Learning']"

# A lament
Extensions on Gnome have recently been in better shape.  There was a long time where I found them to be a problem.  My understanding is that Gnome changed pieces of the environment between versions which tended to break extensions.  It seems like the last year or so has been a good period, even with a couple versions of Gnome revving.  However, word is that Gnome 45 will break every extension.  Will this one be updated, or any of the other extensions I've built my workflow around?

I ran Cinnamon for a long time and really liked it.  It was very stable, easy, and attractive.  I spent a little time bouncing around between KDE and Gnome (and really liked KDE better), then discovered tiling in i3 and started trying to incorporate that into a richer DE.  Pop! came out about that time and settled the issue and has been a dream for years.  Pop! is creating their own DE for the next version, Gnome is back to breaking things, and KDE is undergoing a major version shift (to 6) in February 2024, so I may be in for another of those periods of flitting between desktops.  The winner, for me, will be the one that enables the workflow I've become used to - easy and automated tiling and easy access to multiple desktops.

None of that discussion takes anythig away from the Workspace Name extension, which has been a good fit and stable addition to my desktop.

