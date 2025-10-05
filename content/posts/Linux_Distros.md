---
title: "Distro run-down"
description: "Everything I think about Linux Distros"
author: "Brent Stewart"
date: "2025-10-01T20:01:46-04:00"

math: false
draft: false
Victor_Hugo: "true"
picture: "linux"
Focus_Keyword: "Distro"
youtube: ""
github: ""
refs: [""]
tags:
  - "linux"
---
# Distro run-down

This article summarizes my experiences and organizes my thoughts around Linux as of late 2025.  There are a _lot_ of Linux distributions and I haven't tried to survey the field completely.  In the past I've tried a wider sample, but for the most part I'm currently interested in either Debian-based or immutable ,modern distributions that support tiling.  It's also important that the distribution support my hardware (such as my multi-monitor setup and printer) and standard software.

## Ubuntu or Mint
Both Ubuntu and Mint are Debian-based, which is the environment I'm most familiar with.  They're both pretty standard, reliable, and support all my hardware.  They are boring in a good way and thus the yardstick to compare anything else against.

Why not run one of these choices all the time?  Mint uses Cinnamon as it's desktop, and Cinnamon doesn't (yet) run on Wayland and only supports the crudest tiling.  Wayland is the modern display environment and I want to make sure that I'm spending time learning the current technologies.  Tiling works really well with my workflow.  There are Ubuntu versions that support Gnome or KDE, both of which use Wayland and have tiling plugins.  My experience with those plugins is that they're not bad.

These are not immutable distributions, so they tend to accumulate cruft from past decisions and show age over time.  Both have become more opinionated about packaging and security, which creates situations where I have to work around the peculiarities of the environment (for instance, to load Python libraries).  The software I use is supported in native (DEB) packaging and works well.

## Pop-OS!
(Pop-OS!)[https://system76.com/pop/?srsltid=AfmBOoqbM-J1jVk7iEygYlQ5k4bHjmR0hJbYIQK43a5l4zblIEVkEH1c] 24.04 LTS utilizes an actively developed rust-based Wayland desktop called COSMIC.  COSMIC has a _lot_ of potential and recently moved from a seventh alpha to Beta.  I think the COSMIC version of tiling is the most developed and most thought out approach and works really well with my workstyle.  Pop is debian-based and supports the software and hardware I use well.

COSMIC is still a work-in-progress.  The Cosmic store install Enpass via Flatpak by default, and that version doesn't tie in to Firefox well.  If you're using Enpass with Cosmic, install the app version from [the website](https://enpass.io).  I use back-to-back desks and switch between them - it seems to only support one keyboard and mouse at a time.  The Cosmic Beta also has some rough spots and I had to reboot once where the desktop wouldn't accept focus.  That seems to be rare and I'm hoping that the splinters are sanded down quickly.  Where Ubuntu tends to freeze a system with each release, Pop-OS has some rolling sensibilities that keep it aggressively updated in between official releases.

## Bluefin and Aurora 
(Bluefin)[https://projectbluefin.io/] and (Aurora)[https://getaurora.dev/en] are immutable distros.  This flavor of immutability comes from booting an OCI image with only /home writable.  Software is installed in the user environment using Homebrew or Flatpak, and the operating system is upgraded semi-monolithically (changed bits are downloaded and overlaid on the existing image, but it's functionally an all-in image replacement).

I love the concept!  For purposes of functionality, the KDE (Aurora) and Gnome (Bluefin) versions are fungible.  I found the system to be rock-solid and I came to see the software installation process as practical.  For the most part, these images allow the user to focus on the work and not on the system.  They both check off boxes as modern, Wayland-based, and support tiling (as much as KDE and Gnome do).  I appreciate the isolation and organization provided by a reliance on Flatpak and an assumption of contianer usage.  If I was going to build a machine for Mom, I'd want it to be Chrome-OS like in terms of support and these images are the closest to that experience.  

That said, the cost is that it's difficult to get a customer Brother printer driver installed.  Python is supported via devcontainer, which I found a little bespoke but functional.  There's also a push to support AI in a very specific way - via Ramallama and podman desktop.  In both cases, these approaches worked but felt forced.  One of my primary goals is to use my home environment to learn things to apply at work.  Devcontainer wasn't a big bump, but it was a special case that I don't expect to use professionally.  In the case of AI, I bought a fancy graphics card specifically to play around with self-hosted models and most of the work there is being done in Ollama.  I spent a lot of time trying to understand how others used Ollama and translate that into the Bluefin approach.
I ended up taking one more stab at immutable in the form of [Bluefin](https://projectbluefin.io/).  Bluefin is an opinionated  "spin" of Silverblue, still using Gnome.  It's aimed more at the cloud-native developer crowd, but the operation of Bluefin is very much like Silverblue.  

Enpass as a Flatpak doesn't seem to integrate with the Firefox plug-in.  While giving Bluefin a shot, I tried installing Enpass within ostree.

    cd /etc/yum.repos.d/
    sudo wget https://yum.enpass.io/enpass-yum.repo
    sudo rpm-ostree  install enpass

That worked and fixed the Enpass problem!

## Nix
Nix offers a different vision of immutable computing.  [Nix](https://nixos.org/) has a great vision - you can specify the state of your computer  in one file, and basically compile that file into a finished computer.  This allows you to setup the OS and the installed applications.  I think there's a lot of good ideas there, especially in terms of cloud computing and CI/CD delivery.  

That said, a serious dive into Nix-os reveals some real gaps in the current implementation.  The core "nix" language has been extended through flakes.  Don't know what a flake is?  Good luck finding documentation.  If you have a programming background, you may make more headway on Nix.  I've found it to be slow going.

Without using flakes, there's not a good way to specify pulling in things like fonts from shared folders, dot (settings) files, and managing flatpaks.  I really buy into the vision of a fully specified environment, but Nix seems to be an incomplete implementation at this stage.

Ultimately, I came to see Nix-OS as almost but not fully baked.  There are also periodic stories about personality issues within the community that leave me wondering if I can rely on the Nix eco-system.  As a side-note - I also seriously looked at [Snowflake](https://snowflakeos.org/), which attempts to address some of the rough edges in Nix.  It's still Alpha today, but has promise.  I'll keep an eye on it. 

## Omarchy
Just to get this out of the way . . . btw, I use Arch.  [Omarchy](https://omarchy.org/) is a very personallized distribution of Arch using Hyprland and built along the ideas of David Heinemeier Hansson (dhh).  Hyprland is a Wayland-based update of i3 - a keyboard-oriented tiling user interface.

I'm concerned that Arch is going to be a big learning cul-de-sac, but I've been able to do a tremendous amount without having to dig into Arch-specific issues.  Software installation is setup to "just work" via the Omarchy menu system and has been golden.  Most of the system configuration is done in dot files using NeoVim.  Text files don't bother me.  I'm a "nano" guy, but I've been doing okay enough with NeoVim that I haven't tried to change it.

Omarchy defaults to Chrome, but I was able to install Firefox and switch it to be the default browser.  Other software works great - even Enpass which is becoming my litmus test.  Printing just worked.  Navigation requires a little commitment to learning the most-common keyboard commands, but they're starting to commit to muscle memory (in fact, I've updated Pop to use the core set for consistency).

Omarchy delivers on the concept of a workstation that's built for work.  The jury is out about Hyprland - mostly because I'm trying to give Cosmic some space to develop.  I've warmed a little more to the interface everytime I use it and don't have any specific tricks that I've needed to develop to adapt the system.

## Non-Linux Honorable Mentions
[Haiku](https://www.haiku-os.org/) isn't a Linux operating system at all, but an open-source descendant of BeOS.  It's a single-user systems and software support is hit and miss, however there's a dev version of Firefox (IceWeasel) available now.  More and more software is available via the web (self-hosted or in the cloud), so that gives a lot of room for Haiku to be used.  File sharing and network printeres aren't things that I've figured out, and it doesn't check some other boxes (tiling, wayland) and is hard to run on bare metal.  It's a beautiful system worth keeping an eye on.

[Redox](https://www.redox-os.org/) comes from a set of developers that overlap with Pop-OS!  Where COSMIC rustifies the desktop, Redox is an attempt to create a ground-up operating system using Rust.  In fact, COSMIC is used as the DE on top of the base OS.  It's an amazing vision with lots of promise, but the current system isn't ready for daily driving.  Web support is meh and there's not a lot of Redox software available.  Is this a successor to Linux or BSD?  There's good bones, but not a lot of flesh to judge from at this point.  

## Ventoy
![Ventoy Menu](https://www.ventoy.net/static/img/screen/screen_bios2.png#floatsmallleft)

Ventoy is an open source project that allows you to copy your ISOs onto a single drive and presents a menu that let's you choose which to boot.  Ventoy can even be used to boot a virtual disk and there's a network version that prompts after PXE-boot.  Ventoy is easy to setup on USB.  Download from the site and there is an included executable called __Ventoy2Disk__ that will setup your drive.