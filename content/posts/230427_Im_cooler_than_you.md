---
title: "I'm cooler than you"
description: "Getting VSCode to work on Android"
author: "Brent Stewart"
date: "2023-04-27T17:38:24-04:00"
markup: 'mmark'
math: false
draft: false
Victor_Hugo: "true"
picture: "linux"
Focus_Keyword: "Linux Android Tablet"
youtube: ""
github: ""
refs: ["https://learntermux.tech"]
tags:
  - "Android"
  - "Linux"
---

Hah!  I don't really believe that thing about being cooler, but have you ever done a project and just felt _so_ pleased with yourself?  I turned my Samsung Galaxy Tab S6 Lite into a Linux tablet/laptop and I'm feeling pretty happy.  The actual process isn't that involved, so a little of my self-congratulation is probably undeserved, but the result is stinking cool!

## What is a Linux device?
Before we get into the work, we need to take a philosophical detour.  I wanted a Linux tablet, but what exactly does that mean?  Android is based on Linux, so isn't Android "Linux"?

In my opinion, a "Linux desktop" involves two components.  The first is a shell (in my case Bash) that support the expected set of tools, like __git__ and __hugo__.  I was able to create that environment by installing Termux.  A graphical x-environment is suppored with Termux, but you have to use VNC to access it and it seems like trying to use fingers to control an Xfce desktop would be frustrating.  And for what?  Certainly there is some coup to be counted, but I see a desktop as a way to run applications.  I see the Android shell like an alternative DE with it's own app environment.  As long as Android apps can access the files in the Termux environment, then why not use the native graphical environment?

Office is available for Android, as is Google Docs.  I can use __Markor__ for markdown files, and of course there is a native Firefox.  I can't find a native version of __VS Code__, so we'll have to think about that.  Otherwise, a traditional desktop really doesn't move things forward on this tablet.

## Installing the Bash Environment

![Termux](/230427_Termux.png#floatsmallright)

Prior to this effort, I had already installed [F-Droid](https://f-droid.org) as an alternative to the Play Store that focuses on FOSS (Free Open Source Software).  Pulling Termux from F-Droid instead of the Play Store is supposed to provide an updated version according to their docs.  I downloaded two apps: __Termux__ and __Termux:Styling__.  Termux provides a debian-based Bash environment.  Apt is supported for updates, although the Termux project suggests using __pkg__ for installs.  __Pkg__ automatically runs "apt update" and finds the right mirror, but is basically a wrapper for apt.

Termux creates a home directory at _/data/data/com.termux/files/home_.  I was able to install the command line tools I expect, such as __git__, __hugo__, __openssh__ and __python__.  Android apps are able to read and write into the home directory.  Running a Linux shell via Termux doesn't have any impact on speed (and you wouldn't expect it to).  I found a great article at [Learn Termux](https://www.learntermux.tech/2022/06/termux-lsd-install-file-folder-icons-in.html) that even walked through installing nerd fonts!

I was able to use __git__ to pull down a copy of this website from Github.  I could then use __hugo server -D__ to run the dev environment.  Going out to the local Firefox allowed me to connect to http://127.0.0.1:1313 and see the dev page!  I typically use VS Code to write for this site, but at this point I could use __nano__ on bash to edit markdown files or use the Android-native Markor editor.  

## Visual Studio Code

![VS Code running on Android](/230427_Code_on_Droid.png#floatsmallright)

There's no version of Visual Studio Code for Android.  [Code-server](https://github.com/coder/code-server) is an open-source version of VS Code that runs in a web page, and given my success with hugo I immediately thought of it as an alternative.  There are a _lot_ of instructions online for how to do this, some of them quite complicated, but I ultimately got it to work in a fairly straight-forward way.  In the end, what worked was:

    pkg install tur-repo
    pkg install code-server

With code-server installed, you need to edit ~/.config/code-server/config.yaml and set the password.  You can do this using _nano_ or from an Android text editor.  

Start the application using the __code-server__ command.  Once it was up and running, I was able to access the environment at http://127.0.0.1:8080.  One last note - I didn't know the path to my home directory and __code-server__ didn't know what to do with "~".  Clicking the "files" icon (right under the hamburger menu) doesn't have an interactive way to browse until you get into your home directory.  So here it is again: _/data/data/com.termux/files/home_.  

## Final notes

I _love_ this setup.  It's a lot of fun to use the device and it's easy to travel with it.  Although the pictures show an on-screen keyboard - which works pretty well if you are comfortable two-finger typing - my slim bluetooth keyboard packs up easily and works great in this environment.  I'm running [TailScale](/posts/221004_tailscale/) in Android to access my home and Termux can access the VPN addresses without an issue.  In fact, the first screenshot where I am sshing into my desktop was taken remotely.  On the whole, this setup takes advantage of Android for the part it does well but still exposes the power of a Linux shell and I'm really pleased with it.  It wasn't that complicated to setup, but I'm really happy with the result.
