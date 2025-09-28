---
title: "Self-Hosted Obsidian"
description: ""
author: "Brent Stewart"
date: "2022-12-10T17:17:04-05:00"
math: false
draft: false
Victor_Hugo: "true"
picture: "markdown"
Focus_Keyword: "obsidian"
youtube: ""
github: ""
refs: ["https://www.pcmag.com/reviews/obsidian","https://www.resilio.com/platforms/desktop/"]
tags:
  - "markdown"
  - "obsidian"
---
I've recently written about using Obsidian - an [intro](/posts/220829_obsidian_intro), a discussion of [tasks](/posts/220831_using_obsidian) and add-ins, and a discussion of [dataview](/posts/221002_dataview).  The number of add-ins and the ideas that are being implemented using Obsidian has exploded, and hopefully I've provided enough to get you started and help you discover how it could work for you.  

Justin Pot at PC Magazine published a [review](https://www.pcmag.com/reviews/obsidian) in November.  It's a good review and on the whole very positive.  One deficiency that he points out, though, is the fact that Obsidian is installed locally and there's no facility for accessing it on multiple computers. Justin goes on to suggest that one could use a seperate service to sync files between computers.  I'd argue that this is consistent with the [Unix philosophy](https://en.wikipedia.org/wiki/Unix_philosophy) of "making each program do one thing well".  If you need two functions (notes, file replication) then use two programs.

I'm aware of six ways that Obsidian could be kept up to date between computers, and I've tested several.  If you are interested in using Obsidian and being able to access notes from your PC on your phone is a requirement, here are my experiences and opinions.

You'll note that I'm syncing to my phone.  Obsidian runs - plug-ins, themes and all - identically on the phone.  I don't enjoy the swishy keyboard interface, but I use my daily notes for to-dos and bullet points.  I find the using Obsidian on mobile invaluable for checking things off or adding quick notes as I move around.  I also have added some key PDFs to my vault for reference and the Obsidian interface is an easy way to access that information.

## The ones I didn't try
Several solutions may be good, but I haven't personally tried them.  [Obsidian Sync](https://obsidian.md/pricing) is made by the developers to fund the project and I've looked at this closely.  Obsidian Sync also supports version history, something you don't get using other methods.  That said, it's $8 per month which strikes me as steep.  I just came through a period of unemployment and feel like there's something sinister about the way we're being pushed into more and more subscriptions.  The same link includes an option to donate to the project one-time, and I'm electing to support the devs that way.

Obsidian could also be sync'd through a cloud file system, such as Apple Cloud, Google or Box.  The PC Mag article references using [Resilio Sync](https://www.resilio.com/platforms/desktop/), which is a new one for me. There are comments on Reddit from folks who use various cloud services (Apple seems popular).  My issue here is that Obsidian has some of my most personal information.  I'm not entirely comfortable putting that in cloud storage.  Many cloud providers have some level of free storage, probably enough for an Obsidian vault.  If you need to pay, it's typically around $5-10 and could be used for a variety of things, so this technique could be free and in the worst case is on-par with Obsidian Sync.

{{< danger title="Danger!" >}}
Backup your Obsidian vault before experimenting with sync options.
{{< /danger >}}

## Shared Drive
The first idea is the easiest and most obvious: use a shared drive.  I put my Obsidian vault on a server and published that folder using NFS, then mounted it on my PC.  I used SSHFS to map to it from a Linux laptop and from Windows.  Read more about using SSHFS and NFS [here](/posts/200813_using_ssh3).  No problems.  All three devices were able to co-edit within the same vault - I even noticed open files being updated dynamically.  This method is free, but requires access to the devices (local LAN) and doesn't work for mobile devices.

The private network can be extended using VPN to get this to work remotely.  In fact, I used [Zerotier](/posts/201027_zerotierrouter) and [Tailscale](/posts/221004_tailscale) to test this and things worked perfectly for laptops.  There might be sync issues if someone was editing on both sides, but as long as it's being used as a personal vault, this shouldn't be an issue.  IOS and Android both support various VPN mechanisms, but Obsidian on those platforms expects a device-local vault. 
![Syncthing](/221210_Syncthing.png#floatright)
## Syncthing

[Syncthing](https://syncthing.net/) is a tool that many people use to keep files sync'd between computers.  It's free, and is supported on just about every OS short of Haiku (nope, I checked).  Once Syncthing is downloaded to a device, it needs to have a  relationship approved on a remote device.  The devices agree on local directories that are to be sync'd, then syncthing runs periodically to update files.

Like a shared drive, it's free and devices need to be local to each other.  As shown in the picture to the right, I setup my desktop, tablet and phone to keep an Obsidian vault in sync.  When I first setup Syncthing, there was an instance where it deleted a file.  I wasn't able to replicate that behavior later so it may have been something I did, but it left me with a bad taste.  Syncthing doesn't overwrite - it creates duplicate marked copies.  This results in having a proliferation of these dups that have to be cleaned up.  I had as many as four devices syncing and that may have added to the replication issues.

So my experience of Syncthing was that it worked, but required local software and had some rough edges.
![WebDav](/posts/221211_Obsidian_webdav.png#floatright)
## Remotely Save Plug-in
The [Remotely Save](https://github.com/remotely-save/remotely-save) plug-in is the option that I've personally settled on.  Remotely Save allows syncing using S3-compatible storage, Dropbox, OneDrive, or WebDav.  I already have a local WebDav server running on Apache (see [Using WebDAV on Apache](/posts/210906_apachedav)).  This required minimal self-hosted infrastructure, supported PC and mobile operating systems, and was free.  I've had no issues with it working or losing data.

The screenshot to the right shows the setup of Remotely Save.  The first selection is for the storage type and Webdav is chosen.  Next I specified the server address.  I use other applications that will just take the IP address of the server and Remotely Save required the protocol prefix "http://".  This brings up the one problem I had - there's very little in the way of error messages or debugging output if you have a problem.  If failed when I used a raw IP, but didn't provide clues as how to remedy the issue and it took a bit of trial and error.

I have this setup of several devices.  On each, I have them setup to run at startup and then periodically afterward.  I don't tend to have Obsidian open in multiple places, so this is sufficient to prevent replication bifurcations.


## Git Plug-in

The [Git Plug-in](https://github.com/denolehov/obsidian-git) has a lot of potential.  I tested with Github and couldn't get it to work with cert-based authentication (as is required by Github).  That was okay, because I'm not really interested in copying my vault to public cloud.  I also tested this with local Gitlab.  That worked very similarly to the Remotely Save plugin, with the added bonus of having a history.  If you are comfortable setting up a local Git server and using Git, this is a little more complicated than WebDav but a good option.

## Round-up

For my usage, Obsidian is superior to other options such as Evernote or OneNote.  It's plug-in architecture have also made it an exciting platform for experimentation. There are constantly new ideas about what can be done using this as a base.  The PC Mag review was right to call out multi-device usage as a shortcoming, but I've found this can be addressed in a variety of ways.  I've been using local WebDAV (using the Remotely Save plugin) for about a month and Tailscale to allow remote syncing and I've been very pleased.