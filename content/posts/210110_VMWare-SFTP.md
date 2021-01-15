---
title: "VMWare File Transfer Options including SFTP "
date: 2021-01-10T16:51:18-05:00
draft: false
author: "Brent Stewart"
Victor_Hugo: "true"
Focus_Keyword: "VMWare"
picture: "vmware"
github: ""
youtube: ""
refs: [""]
tags: ["VMWare","SSH"]
---

![VMWare SFTP](/210110_VMWare-SFTP.png#floatright)

I'm going to be upgrading my home VMware server and need to backup the VMs.  My server uses ESXi 6.5 and I'll need to backup the files before upgrading.  Longtime readers may recall that I'm using [Backblaze](/posts/200804_homebackup) to backup the _data_ on my server.  That is going swimmingly so far.  I want to backup the images so I don't have to rebuild the VMs after this is done.  

## Admin interface
One way to accomplish this is to login to the admin web portal and export the VMs.  Under each VM, go to Actions > Export and this queues downloading the component files.  Exporting over HTTP is slow though.

## VMWare Workstation
A second option is to backup from VMWare Workstation.  I prefer using this to manage server VMs anyway.  First, connect to the server under _File>Connect to Server_.  Once the server is attached to workstation and you can see the VMs, right click a machine and choose _Manage > Download_. This is also fairly slow.

## Using SSH with VMWare

![Filezilla to VMWare](/210110_Filezilla.png#floatright)

To speed up the action, I wanted to grab the VMDKs directly from the server using SFTP.  To set this up, login to the administrative interface of ESXi and look under "Host".  Choose "Actions" and "Enable SSH". SFTP is a part of SSH, so this also enables SFTP.  This isn't the best way to grab the backup, since it will take a little work to stand these up again, but it is faster.



To make this super easy, I used Filezilla.  Under the site manager, I selected SFTP, entered the IP address, and username.  When connecting, you'll need to accept the host key and navigate to /vmfs/volumes/YOUR_VOLUME_NAME/ and each of the VMs will have a directory.  You can also easily upload images this way - ISOs can be saved to to VMWare easily this way.

