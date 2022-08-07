---
title: "Distro Hopping in Style with Ventoy"
date: 2021-09-11T09:14:40-04:00
draft: false
author: "Brent Stewart"
Victor_Hugo: "true"
Focus_Keyword: "Ventoy"
picture: "linux"
github: ""
youtube: ""
refs: ["https://pop.system76.com/","https://kubuntu.org/","https://github.com/kwin-scripts/kwin-tiling","https://regolith-linux.org/","https://ubuntudde.com/","https://cutefish-ubuntu.github.io/","https://www.haiku-os.org/","https://reactos.org/","https://en.wikipedia.org/wiki/AROS_Research_Operating_System","https://www.ventoy.net/","https://www.amazon.com/gp/product/B082NRJ5MS/","https://www.amazon.com/gp/product/B079X7K6VP/"]
tags: ["linux"]
---
One of the many things I love about Linux is all the competing ideas about how computing should be done and how desktop environments should be organized.  I've pretty much settled in with Debian-derived systems.  This isn't a religious decision, but I've found that most of the things that I want to do are easily supported and I've grown comfortable with the commands and structure.  

Desktop Environments (DEs) are another matter.  Not only have I _not_ picked a favorite, I love to switch back and forth and try out the latest.  My current (9/11/21) thoughts are that tiled interfaces work best for the large 4K displays I like to use.  I liked [Regolith](https://regolith-linux.org/), which is an i3 on Ubuntu distro but I'm not committed to throwing away my mouse.  I'm currently split between [Pop! OS](https://pop.system76.com/) (which is a customized Gnome) and [Kubuntu](https://kubuntu.org/) with the [Tiling Extension](https://github.com/kwin-scripts/kwin-tiling) (KDE on Ubuntu).  On HD screens and smaller, I really like [Cinnamon](https://linuxmint.com/) although I'm a sucker for the eye-candy in [Deepin](https://ubuntudde.com/).  I've even recently started playing with [Cutefish](https://cutefish-ubuntu.github.io/) which is a Mac-like DE.

All this is _besides_ my fascination with alternative operating systems like [Haiku](https://www.haiku-os.org/), [ReactOS](https://reactos.org/), and [AROS](https://en.wikipedia.org/wiki/AROS_Research_Operating_System).

I think my dream would be the Pop!OS Tiling Extension on Cinnamon.  Sigh.

To try out all these operating systems, one has to download ISOs and write them to USBs.  That's not awful, but USB drives aren't easy to label or write but they are easy to lose. That's why I was stoked to discover Ventoy.

## Ventoy
![Ventoy Menu](https://www.ventoy.net/static/img/screen/screen_bios2.png#floatsmallleft)

Ventoy is an open source project that allows you to copy your ISOs onto a single drive and presents a menu that let's you choose which to boot.  Ventoy can even be used to boot a virtual disk (vhd or vdi).

Ventoy is easy to setup.  Download from the site and there is an included executable called __Ventoy2Disk__ that will setup your drive.  I recently wanted to upgrade ESXi and had trouble getting my aging server to boot from USB.  I put the ESXi image on Ventoy and it came up without a problem.  Then I decided to migrate to ProxMox and that worked too!
![UGREEN M.2 Enclosure](https://images-na.ssl-images-amazon.com/images/I/31rtVwnH1GS._SY90_.jpg#floatright)
![Silicon Power M.2](https://images-na.ssl-images-amazon.com/images/I/41sG6BDAbML._SX90_.jpg#floatright)

I initially put Ventoy on a large flash drive, but decided I wanted something more robust and speedy.  I bought a UGREEN M.2 Drive enclosure and a Silicon Power 128GB M.2 drive to put inside it.  With this setup, I put all the operating systems I use and all the ones I play with onto the drive and can easily boot into a live environment or install.

The UGREEN enclosure has a type A connection on one end and a type C on the other.  It's long and a little heavy, so it puts some torque on the connector if you plug it up directly.  I use an extension cable so that the drive can be out of the way and not damage the port.

Every once in a while, I get a wild hair and install a different OS on my main computer or more often on a play laptop I keep on my desk.  I can also plug in my Ventoy disk and use it to setup a VM.  I'm in this industry because it's fun and this is a way that I stay in touch with the magic, even when real IT work starts to look like a parade of spreadsheets.  Ventoy helps make it quick and easy to play!

If you really want to change out your main PC OS, I recommend keeping data off the PC (using [ssh](/posts/201017_sshfs_automount/) or [nfs](/posts/200813_using_ssh3/)) and having a setup file to quickly install critical components.

Have fun!