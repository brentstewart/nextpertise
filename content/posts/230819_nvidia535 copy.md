---
title: "NVidia 5.35 Warning"
description: "Beware of NVidia 5.35"
author: "Brent Stewart"
date: "2023-08-19T08:32:41-04:00"
markup: 'mmark'
math: false
draft: false
Victor_Hugo: "true"
picture: "linux"
Focus_Keyword: "NVIdia driver"
youtube: ""
github: ""
refs: [""]
tags:
  - "linux"
---

# NVidia 5.35 may be buggy
I'm writing a quick note to share a word of warning.  I recently updated my NVidia driver to 5.35 and had issues.  I've seen people asking similar questions in the last few weeks in online threads, so I think this is happening to a few folks.

## My system

![Hyper](/hyper.png#floatright)
The affected computer is running Pop_OS 22.04.  One of the things I like about Pop! is that it keeps things like drivers and kernel current.  When they announced that they weren't going to produce releases every six months, I worried about it getting stale, but I've actually found that the update system keeps it fresh and it's been so stable I'm reluctant to move away.  For a distro-hopper, that's saying something.

I'm using a 1060 card as well.  I'm not a big gamer and this card is fine for work.  It drives two 4K displays, and I've had it running four displays using it's two HDMI and two DisplayPorts.  I haven't had many problems with it.  The lure of a newer card is there, but GPUs have been expensive for a long time.

So stable system, good hardware, been humming for a while.

## Symptoms

I maintain the system in two ways.  The Pop!_Shop application does a pretty good job with it's applications, but it doesn't pick up things outside of apt and flatpak.  I wrote about [Topgrade](/posts/221114_topgrade/) late last year - it is an application I've found that will catch apt and flatpak, plus things like Nix, Brew, Cargo, shell extensions, PIP, containers, and others.

i've been using NVidia 5.25 and Topgrade [presented the option](https://www.stickycomics.com/computer-update/) to go to 5.35.

My current configuration has a display on my standing desk and one on my sitting desk that are mirrored.  I was using HDMi for both.

After installation, both displays went blank.

## Troubleshooting
Troubleshooting involved rebooting where I saw the POST message, so I knew the card and displays _worked_.  Changing out cables and displays led me to realize that only DisplayPort #1 worked.

I found several posts on places like r/linux asking about similar problems without a solution.

The Pop! display configuration tool only showed the one display.  Somewhere around this time I ordered new DisplayPort cables, thinking it was maybe related to HDMI.  I eventually remembered the upgrade and backed it out.  _Viola!_ Everything has returned to working since.

## Conclusion
I haven't heard this discussed in the tech press, even on places like [Unplugged](https://www.jupiterbroadcasting.com/show/linux-unplugged/}, so I assume this isn't pervassive.  That said, approach any graphics driver upgrade with caution and specifically be wary of 5.35.

The Linux community benefits from a lot of cool people contributing to it and I appreciate that.  This type of issue is the tax we pay.  I appreciate NVidia's contributions and all the people who work to make it just pop in to my system and I'll definitely try the upgrade again when they bring out 5.4.