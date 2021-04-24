---
title: "A Love Letter to Flameshot"
date: 2020-12-10T14:28:39-05:00
draft: false
author: "Brent Stewart"
Victor_Hugo: "true"
Focus_Keyword: "Flameshot"
picture: "linux"
github: ""
youtube: ""
refs: ["https://en.wikipedia.org/wiki/Unix_philosophy","https://flameshot.org/"]
tags: ["Linux","Review"]
---
![Flameshot](https://flameshot.org/media/animatedUsage.gif#floatright) My friend Jared always extols the Unix philosophy that programs should "do one thing and do it well".  One program that fits that description is _flameshot_.  Flameshot makes great screenshots and allows you to easily annotate them.

A few years ago I was put in a position where I had to produce monthly reports that included screenshots from our tools.  The default gnome-screenshot application is difficult to work with.  It has to be re-invoked for each capture and it doesn't allow you to easily adjust the parameters or annotate the picture.  As a result, I'd take a bunch of screenshots and each one involved a try or two to capture the right section, then opening in Gimp to edit.  It took _forever_.

Flameshot made this an easy process.  Since then, I very commonly want to take a screenshot for this blog or for an email.  Usually I'm trying to demonstrate a function or show what a screen looks like.  This program allows me to quickly grab the section of the screen I need, highlight, draw, and even obfuscate sensitive portions (like credentials).

Flameshot can be downloaded from Github, but most distros have it in their archives.  It's available as a flatpak, snap, AppImage, rpm, or deb.  It's even available for Windows.  On Ubuntu, it can be pulled down using:
```bash
sudo apt install flameshot  
```
For snap:
```bash
sudo install snap
```

For flatpak:
```bash
flatpak install https://github.com/flameshot-org/flameshot/releases/download/v0.8.0/org.flameshot.flameshot_0.8.0_x86_64.flatpak  
```

or grab the AppImage version from the website.

There's a lot of gnashing of teeth about which of these formats is better.  Personally, grabbing the DEB means that I update it with the rest of my applications and I haven't had issues with DEB.  I've used all three of the portable formats and found them to be fungible.  Appimage is both the most portable and the easiest to forget updates with.
 
## Invoking Flameshot
Flameshot can be launched from the menu.  It will also install an icon on the bar - on Gnome you need the [TopIcons](https://extensions.gnome.org/extension/1031/topicons/) extension to see it.  
It also works well to map a keybinding to it, such as PrintScreen.  Link the keypress to the command: __flameshot gui__

## Using Flameshot
Once you start Flameshot, the screen immediately dims and you are prompted to drag a box around the area of the screen you want to snip.  Once the box is defined, a series of circular option buttons surround the selection to allow editing.

The animated graphic above does a good job showing this action.  Some of the features I most commonly use are arrows, lines and boxes,and the "smear" tool to hide sensative parts.  Once done, click the disk icon and save your file.

## Conclusion
As I said, Flameshot takes great screen captures.  Since I titled this a "love letter", it's obviously a program that I find valuable.  It makes it easy to annotate them.  It's easy to adjust the capture and it's easy to re-invoke if needed.

Flameshot has become a part of my standard install - frankly I'm not clear why it's not included standard with most distributions.  It does one thing really well.  I encourage you to give it a try!