---
title: "Distro Hopping"
description: "Again"
author: "Brent Stewart"
date: "2024-02-17T18:08:32-05:00"
markup: 'mmark'
math: false
draft: false
Victor_Hugo: "true"
picture: "linux"
Focus_Keyword: ""
youtube: ""
github: ""
refs: [""]
tags:
  - "linux"
---

This article is the last thing I'm doing on this installation of Silverblue.

## What happened to Pop! OS
I've written in the past about my tendencies to distro-hop.  There's always been a greener grass kind of feel to checking out new  Linux distributions.  However, I found "home" with Pop! OS about three years ago and stopped hopping.  Pop! is a distribution that really hits a lot of sweet spots with me.  It's Debian based - moving to an RPM distribtion isn't the hardest thing, but I've had better luck for over a decade with Debian distros.  The Pop! desktop is the best compromise of windowing/tiling, allowing me to use either or both paradigms to allocate screen real estate without a lot of futzing.  Finally, Pop! had a great update policy.  I noticed updates, like new kernels, arrived aggressively but that (with the exception of the NVidia driver update) they were reliable.  Unfortunately, I got bit by a dying NVME drive.  

I had good backups, but I needed to reinstall. Pop! is in the middle of seperating it's desktop environment from Gnome and they've pretty much halted the development of the Pop! DE while that happens.  I'm very excited about the COSMIC environment, but it's not here today and Pop! still uses X.11.  One of the things I'm trying to do with my home environment is to learn about what's next, and maintaining X.11 doesn't seem like where I want to spend my time.  So there has opened a gap - I have to reinstall something and Pop! isn't ready.

## Stop #1 - Nix
I claimed to have stopped distro hopping, but I have a laptop that I use as a "casual" computer and I've tried a number of things with it.  I'm really interested in the possibilities of immutable distros and I've been playing with Nix.  What a great opportunity!

[Nix](https://nixos.org/) has a great vision - you can specify the state of your computer  in one file, and basically compile that file into a finished computer.  This allows you to setup the OS and the installed applications.  I think there's a lot of good ideas there, espeically in terms of cloud computing and CI/CD delivery.  

That said, a serious dive into Nix-os reveals some real gaps in the current implementation.  The core "nix" language has been extended through flakes.  Don't know what a flake is?  Good luck finding documentation.  If you have a programming background, you may make more headway on Nix.  I've found it to be slow going.

Without using flakes, there's not a good way to specify pulling in things like fonts from shared folders, dot (settings) files, and managing flatpaks.  I really buy into the vision of a fully specified environment, but Nix seems to be an incomplete implementation at this stage.

Ultimately, I came to see Nix-OS as almost but not fully baked.  I'm going to continue to run it on my "casual laptop", but the desktop needs to be in a more stable and predictable environment.

As a side-note - I also seriously looked at [Snowflake](https://snowflakeos.org/), which attempts to address some of the rough edges in Nix.  It's still Alpha as of February '24, but has promise.  I'll keep an eye on it. 

## Vanilla
I experimented with [Vanilla](https://vanillaos.org/download).  I didn't spend as much time here.  What I saw looked fine.  It uses ABRoot for the immutable part.  

The project is in the middle of a re-imagining.  The Orchid release will be build in a different way and use differnt tools.  That's great and I'm looking forward to where they end up, but I don't want to put a lot of energy into learning a system that will change completely later this year.

## RedHat Silverblue
I wasn't as crazy about [Fedora Silverblue](https://fedoraproject.org/atomic-desktops/silverblue/).  It's atomic through the use of OSTree.  OSTree requires a reboot when the config is changed.  I went all-in on Flatpaks to address this and that seemed to close the worst of the gap.  

One conclusion I made is that it is possible to run about 90% of the software I need strictly using Flatpak.  I was also able to create a script to setup my environment, pulling in the flatpaks I wanted and adding to the OStree image.

Silverblue ended up being the closest to usable of the immutable environments for me.  It is stable, and I learned a few tricks with BTRFS in the scope of setting it up.  It recognized hardware and performed really well.  

I learned about [Distrobox](https://github.com/89luca89/distrobox) on Silverblue.  Distrobox is a wrapper around a container system (it uses either podman or docker).  It allows you to install any Linux distribution in a container on another Linux box.  You can install things in the container and the application is tightly integrated with the host environment.  The normal container isolation is removed, allowing those applications to function like host applications.  I used a GUI called BoxBuddy to make setting this up easier.  Boxbuddy/Distrobox made setting up my Python environment a snap.  I used it to setup an Ubuntu environment, but could just as easily have setup Arch.

But I'm not staying.

Silverblue plays havoc with my password vault or Calibre.  The vault flatpak doesn't display a GUI and the integration with the browser doesn't work.  I'm back to typing in passwords like an animal!  Calibre also never starts up. Yes, I've thought of trying to reinstall apps at the ostree level, I've tried [flatseal](https://flathub.org/apps/com.github.tchx84.Flatseal) (which allows you to adjust flatpak permissions).  I've also thought about installing apps through Distrobox.

## Back to the Start
In the end I've decided to retreat from Immutable for the moment for my working desktop.  I can keep playing at the edges and chipping away at it, watching the progress.  I'm also really interested in what System76 puts forward with the new version of Pop! (hopefully 24.04).  In the meantime, I'll retreat to Ubuntu and run a modern (Wayland) but not immutable environment and get some work done.

## Addendum
I ended up taking one more stab at immutable in the form of [Bluefin](https://projectbluefin.io/).  Bluefin is an opinionated  "spin" of Silverblue, still using Gnome.  It's aimed more at the cloud-native developer crowd, but the operation of Bluefin is very much like Silverblue.  

I was very happy with Bluefin and think it's a better Silverblue than Silverblue.  That said, I still had some of the same issues.  I had screen freezes that required reboot, and issues running some programs.  While giving Bluefin a shot, I tried installing Enpass within ostree.

    cd /etc/yum.repos.d/
    sudo wget https://yum.enpass.io/enpass-yum.repo
    sudo rpm-ostree  install enpass

That worked and fixed the Enpass problem!
