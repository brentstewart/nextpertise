---
title: "Ubuntu 20.10"
date: 2020-10-24T18:40:56-04:00
draft: false
author: "Brent Stewart"
github: ""
youtube: ""
refs: ["https://www.ubuntu.com"]
tags: ["Linux","Gnome","Material Design","Ubuntu]
---

I had a chance to download Ubuntu 20.10 and install it on a gen 8 i5 laptop.  I typically use Cinnamon for a desktop environment, which I have long found to be mature, usable, and stable.  In the past, I've found Gnome to be laggy but I've heard good things about 3.38.

I've become enamored of tiling windows as well.  I've tried i3, Pop OS!, and others.  So another thing I wanted to look into was the Material Design extension.

## About Ubuntu 20.10

Ubuntu releases a Long Term Support (LTS) release biannually.  LTS versions, like 20.04,  are supported for five years and are appropriate for business servers.  Interim releases like 20.10 are supported for nine months.  These releases are kept current and represent the most up-to-date implementation of Ubuntu's vision for the Linux desktop.  Ubuntu supports a variety of DEs including KDE, Budgie, MATE and others. There are unsupported versions for Cinnamon and Deepin as well.  I've used the MATE version for VMs in the past and it's performed very well, but this time I wanted to check out Gnome.

The first two things that jumped out at me have to do with login.  First, there's now a built in choice to authenticate via Active Directory.  I have several workstations and servers and the idea of being able to easily centralize authentication is _very_ appealing.  I haven't had time to build that part out, but I will be looking into that process in the months ahead.  I'm hoping I can make it work with FreeIPA.

The other thing - and this sounds minor - is the responsiveness of the login screen.  It's been my past opinion that Gnome was slow, but logging in is breezy.  I use Windows 10 at work and the Ubuntu lock screen is especially quick comparted to that. 

The default Gnome experience has an _application grid_ like Android (all the apps in alphabetical order).  This was always annoying to me - partly because I'm a heirarchical organizer and partly because I could never remember the name of the app I wanted.  The new 3.38 allows you to organize your apps in whatever order makes sense to you and to pull icons into folders.  This is a big win for me.

Ubuntu is still running Gnome on X.  X Windows is stable and mature and has better support for NVidia video cards, but the next generation Wayland is coming and Ubuntu hasn't pushed that envelope yet.

Ubuntu 20.10 includes Linux kernel 5.8.0.25.  This was billed as the "biggest release of all time" by Linux Torvalds.  Most of the improvements are around drivers and the Spectre patch.  According to Phoronix, there's good benefit for ATI graphics.  Otherwise, from a desktop perspective, this won't be a big deal.

Applications, such as LibreOffice (7.0), Firefox (82), and Thunderbird (78.3.2), have been updated to latest versions.

To me, the biggest difference between Gnome and Mate or Cinnamon is app selection.  At the end of the day, despite the improved performance, it's not enough to make me want to switch.

## Tiling with Material Design
As I mentioned before, the window tiling paradigm really makes sense to me.  I've tried i3 and Regolith, which I like, but they suffer from the same problem as Gnome - having an easy way to browse available applications.  Pop OS! has a tiling extension for Gnome that's well thought out, but I abandoned it because it made Gnome _less_ responsive.  Still, the Pop OS! experience came the closest to what I imagined tiling could be.

Recently I was introduced to __Material Design__.  I decided to install it on my new Ubuntu 20.10 installation and try to "live in it" for a better test.  __Material Design__ is a gnome extension that provides a few really specific tiling options.

To install Material Design in Ubuntu 20.10, go to https://extensions.gnome.org and add the extension.  Unfortunately, Ubuntu doesn't support the Gnome add-in page out of the box.  At the Gnome page, load the firefox extension.  You'll also need to install some supporting pieces for it to run properly.  Note that the chrome-gnome-shell is used by both Firefox and Chrome.

> sudo apt-get install libproxy1-plugin-networkmanager gnome-shell-extension-system-monitor  
> sudo apt-get install chrome-gnome-shell  

Windows are on desktops, with the taskbar across the top showing you what's on the current desktop.  Desktops are shown on the side bar, so you can think of the two bars as showing a grid of apps X desktops. The version of Material design at Gnome supports four windowing modes. 
* Single Window - each application takes up the full window and you can use the taskbar to move easily between them.
* Split - each application takes up either the left or right half of the screen.  The app order is shown on the taskbar, and you can drag to rearrange.  Many times I'm typing in one window with another for reference, so this mode sets up exactly what I'm looking for.
* Half - the first application occupies the left half of the screen.  Subsequent apps stack to the right.  You can drag windows or rearrange the taskbar to change which applications go in each spot.
* Free - this is the "normal" Gnome experience, with tiling turned off.
![Material Design](https://raw.githubusercontent.com/material-shell/material-shell/master/documentation/tiling_showcase.gif)

In my testing, I found that I could set different modes for each desktop.  Right now, the desktop I'm typing on has Codium on one side and a browser on the other in "Split" mode.  My other desktop use "Half" with a big browser on the right and a terminal, Enpass, and settings stacked on the right.

I also installed the __Gnome menu__ so that I'd have the heirarchical app picking experience I like.  Material shell has a button for the gnome app grid but I want something different.  I'm unimpressed with __Gnome Menu__.  Anyone want to recommend a menu extension?  Gnome Menu hits the minimum reqs - it's an organized menu.  On the other hand, you can't type to search, it doesn't open sub-menus automatically, and it doesn't a place where frequently used apps or files can be quickly found.

In the end though, the beauty of Linux is that the machine is _yours_.  In a half hour I installed a new distro with a new DE, updated it and installed some critical apps, customized the desktop for tiling and changed the application selection process.

## Conclusion
I'm going to keep Ubuntu 20.10 and Material Design on this machine for a while.  It works well on the 1920x1080 17" display.  I'm also curious how the combination will work on my desktop with it's two 4k displays, but I'll take a little time before making that decision.

Ubuntu 20.10 has answered my objections to the laggy experience of Gnome and given me a "standard" desktop experience I can customize to my interests.  I'm curious about your impressions - write to me and let me know!