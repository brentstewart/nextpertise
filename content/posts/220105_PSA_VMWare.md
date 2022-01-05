---
title: "PSA VMWare"
date: 2022-01-05T17:49:07-05:00
draft: false
author: "Brent Stewart"
Victor_Hugo: "true"
Focus_Keyword: "VMWare"
picture: "vmware"
github: ""
youtube: ""
refs: ["https://github.com/mkubecek/vmware-host-modules"]
tags: ["VMWare"]
---

# Public Service Annoucement on VMWare Workstation

That's a little dramatic, but I have had a fit with VMWare Workstation and want to pass along what I've learned.  I recently upgraded my desktop and VMWare performance went in the dump.

### Backstory

I'm running Pop!_OS 21.10 using kernel 5.15.  I'm using VMWare Workstation 16.2.1 (latest as of early January 2022).  Since I do a lot with virtualization, I have 64GB of RAM.  I was running a Gen 6 i7 and recently upgraded my processor and motherboard to get to gen 12.  Wow! What a difference!  Everything runs super-fast, but VMWare suddenly became anemic.

## What'd I do to fix it?

Workstation has trouble running on recent kernels.  Michal Kubeček maintains a [repository](https://github.com/mkubecek/vmware-host-modules) with kernel patches.  Download the patch from GitHub - instructions are included in the repository.  You'll know this worked because VMWare boots.

The ultimate fix was to tell VMware not to swap memory, since I have plenty.  To do this, run Workstation as root:

    sudo -i vmware

Go to __Edit__ > __Preferences__ and under "Memory" select "Fit all virtual machine memory into reserved host RAM".

![VMWare Workstation Settings](/220105_VMWS_Memory.png)

I searched all over the Internet and tried a dozen different things to fix this.  I know this is a short article, but I wanted to make sure that others had a less frustrating time!