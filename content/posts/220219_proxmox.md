---
title: "Problems with Proxmox VE"
date: 2022-02-19T15:37:06-05:00
draft: false
author: "Brent Stewart"
Victor_Hugo: "true"
Focus_Keyword: "Proxmox"
picture: "proxmox"
github: ""
youtube: ""
refs: ["https://www.proxmox.com"]
tags: ["Proxmox","Virtualization"]
---
Look, I know you're reading this, but sometimes I write these posts for myself.  To remind myself of how I built or repaired something.  This is one of those posts.

My home server ran VMWare ESXi for a long time.  I had trouble upgrading it from 6.5 and I was intrigued by Proxmox VE and putting my home network more firmly in the open source camp.  I've been running Proxmox VE 7.1 for a while now.

## Quick review of PVE
My experience with PVE has been mixed.  VMWare hid a lot of it's Linux base, where PVE is like an opinionated distro aimed at virtualization.  With PVE, you are definitely administering a debian derivitive box.  PVE will host VMs and also does OS-level containers, which is an interesting take and seems to conserve processor.  I implemented my internal servers this way and it's indistinguishable from a full VM in terms of use    .

Probably the biggest "issue" is that I use VMWare Workstation to have a Windows VM on my Linux machine.  Workstation was a pretty good front-end for ESXi and you could migrate loads between the two.  Obviously, that use case is out the window now.

Generally, PVE runs as good as or better than ESXi.  However, on the occassion that something goes sideways you are combing through blog posts and support forums (assuming you don't have a subscription, which I don't for home).  I like that the things I learn in PVE are transferable to Linux and vice versa, but I wouldn't make PVE your first experience with Linux.

## Back to the show

Power went out at the house the other night and the VMs (actually OS-level containers, but that's arduous to say and if I abbreviate it OSLC no one will know what I'm nattering on about) storage had an issue.  When PVE tried to load those volumes it gave an error _"activating LV 'pve/data' failed: Activation of logical volume pve/data is prohibited while logical volume pve/data_tmeta is active. (500)"_

I tried several approaches to resolve this.  You can see the error from the PVE command line via __lsblk__.  What actually worked was to make the interfering volumes inactive. Seems obvious, but I needed the command __vgchange__.  As a note, after I deactivated _tmeta_ I got an error because _tdata_ was active so I had to deactivate that as well.

    #deactivate the offending volume
    lvchange -an pve/data_tmeta
    ##activate the expected volumes
    vgchange -ay

So this post is a note to myself for the next time.  Hopefully it's helpful to you as well.