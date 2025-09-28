---
title: "Restoring from Proxmox Backup Server"
description: "An easy way to protect your data"
author: "Brent Stewart"
date: "2024-02-06T13:19:46-05:00"

math: false
draft: false
Victor_Hugo: "true"
picture: "backup"
Focus_Keyword: "Proxmox Backup"
youtube: ""
github: ""
refs: [""]
tags:
  - "backup"
  - "virtualization"
  - "linux"
---
Proxmox Backup Server (PBS) saved my bacon.  My previous [journal](/posts/240204_pbs/) described how to install PBS and how to set it up to backup virtual machines (VMs) from ProxMox Virtual Edition (PVE).  

I wanted to continue the story to tell you how I used it.

![Email Verifying Backup](/240210_pbs_email.png#floatright)
## Is it working?
Before we delve into how to restore a backup, it's worth asking whether there is something there to restore!

Proxmox backup asks for an e-mail address during setup.  Assuming you enter a valid e-mail, you should get a note each time the backup job runs.  A screenshot of this email appears to the right.  You can see that it's reporting successfully backing up four VMs (technically two LXD containers and two fully virtualized devices, but that difference isn't the focus of this article).

My home server has crown-jewel level data assets.  Things like baby pictures, that would be awful to lose.  It's important to note that the backup is one piece of a larger multi-level strategy to maintain those files.  That strategy includes using ZFS to protect against "data rot", having some form of RAID to protect against disk error, backing up, and backing up offsite.

When it comes to the backup piece, setting it up is good.  Having some form of monitoring is better (email in this case, although there's also a server screen you could go check out).  However there are a _lot_ of stories about backups that didn't work when they were needed.

You should absolutely review the files that are being backed up to make sure that they include all of the correct files.  

It's also a rule, call it _Brent's Law of DR_, that untested disaster recovery never works.  You should do a test restore periodically.  This might be restoring just a sample file, or restoring an entire VM.  Testing the restore process ensures both that the backup is _really_ working and that you remember the steps necessary to invoke a restore.  

Enough pontificating!  On to the good stuff . . .

## Restore a file
Restores are conducted from the PVE admin screen (http://your-pve-server:8006).

![PBS Restore Admin Screen](/240210_pbs_screen.png)

1. Under the  Server View Datacenter heading, go to the target server and select the Proxmox Backup Server storage.  Mine is called _pbs_ in the screenshot.  Because it's attached to the datacenter, that storage shows up under every server.  It's the same thing, so for file recovery just pick a server to work through.

2. Pick the backup that has the file  you'd like to restore.  Depending on the backup policy you specified, there will be several copies over a period of time.  If you're restoring because you borked the config last week, for instance, you may need to go back two weeks to find a working copy.

3. Once you've found your target, select File Restore.  I know, kind of obvious.

4. This opens a file viewer that lets you burrow into directories to find your file.  Once selected, choose the Download button to get the file.

## Restore a VM

1. Under the  Server View Datacenter heading, go to the target server and select the Proxmox Backup Server storage.  Mine is called _pbs_ in the screenshot.  Because it's attached to the datacenter, that storage shows up under every server and so I'm selecting the one under the server that I want to restore to.

2. Pick the backup you'd like to restore.  Again, there will probably be several copies.

3. Once you've found your target, select Restore.  

4. When you restore, you can select the target PVE instance, the storage on that PVE you want to use, and the VM ID.

You can restore it to the existing server, using the existing VM ID, and overwrite the current copy.  You can also choose to restore it to a different ID so that there's a second copy.  That could be useful if you're trying to clone a setup.