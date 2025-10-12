---
title: "Proxmox"
description: ""
author: "Brent Stewart"
date: "2025-10-06T21:24:58-04:00"

math: false
draft: false
Victor_Hugo: "true"
picture: "virtualization"
Focus_Keyword: "virtualization"
youtube: ""
github: ""
refs: [""]
tags:
  - "virtualization"
  - "linux"
---
## Proxmox Virtual Edition (PVE)
PVE is a hypervisor that supports LXC containers and full virtual machines.  It's comparable to VMWare ESXi.  The comparison fits across the board.  There's a Proxmox DataCenter Manager that's similar to VMWare Cloud Director (VCD), and it supports a similar set of features such as high availability failover.

The current version is 9 (as of 10/25), but my experience has been poor when attempting to upgrade.  As a result I recommend v8 for the time being.  I also recommend running Proxmox Backup - the closest VMWare comparable might be Zerto.

I've been using PVE for a several years and I've been pretty pleased with it.  I run a virtualization server at home for a few reasons.  VMs have been a way to break apps out into seperate environments, making self-hosting easier.  Second, I support virtual environments and wanted to use my home network to get hands-on time.

Originally, I used a free version of VMWare but VMWare was purchased by Broadcom.  They halted the free version for a while and haven't proven to be transparent in their approach to licensing, plus the server hardware I was using died, so it made sense to explore alternatives.  Proxmox VE had a lot of the capabilities, plus the Linux underpinnings were much more obvious (which supported another direction I was heading).  The PVE environment has been mostly good and I've never had an issue that I couldn't recover from (similar to what you'd expect running ESXi).
### Helper scripts
There's a [community](https://community-scripts.github.io/ProxmoxVE/) that creates scripts to build VMs and LXCs to enable easy setup of popular applications.  The scripts cover popular applications including Pi-Hole, Homeassistant, and Homarr.  At last check, there were almost 400 supported applications!

Use caution here because the scripts are presented as easy "pipe to bash" commands that should be run from PVE root.  The scripts can be downloaded and reviewed or just viewed on [Github](https://github.com/community-scripts/ProxmoxVE).  Before running a foreign script on your server, it would be advisable to review the script!


## Command line notes

Here are some basic command line notes for working with PVE.
Version
```pveversion```
Node Status
```pvesh get /nodes/<id>/status```

####  Cluster
Cluster Status
```pvecm status|nodes|quorum```
```corosync-cmapctl```
Join a cluster
```pvecm add|delnode <ip|hostname>```
Change the quorum number
```pvecm expected <count>```

#### Networking
Network specifics are configured in /etc/network/interfaces.  Details about the environment can be seen from the command line.
What the IP?
```ip a```
List bridges
```brctl show```
List FW rules
```iptables -L -n```

#### Manage VMs
Managing VMs from the command line can be a good deal easier than using the gui.  I've found it's convenient to have a default Ubuntu server ready and turned off.  This can then be cloned and configured.

List VMs
```qm list```
Interacting with VMs
```qm start|stop|shutdown|destroy <id>```
Create a VM
```qm create <id> --name <name> --memory <size> --net0 <virtio|bridge=vmbrX> --cores <#> --sockets <#> --virtio0 local:<storage>:<size>```
Clone a VM
```qm clone <source-id> <new-id> --name <name>```
Interacting with Snapshots
```qm snapshot|rollback <id> <snapshot-name>```
Interacting with Backup
```vzdump <id> --compress <type> --storage<id>```
```vzrestore <file> <id>```

#### Storage
List storage
```pvesh get/storage```
List Storage details
```pvesh get/storage/<id>```
Create
```pvesh create /storage --storage <id> --type <type> --content <type> --path <path>```
Delete
```pvesh delete /storage/<id>```

#### Lost password
Proxmox allows the administrator to attach to a VM using lxc-attach.  The session is logged in as root.

```lxc-attach -n <id>```

This is extremely useful - for instance, it could be used to reset a lost administrator password using the __passwd__ command!

## Proxmox Backup Server
PBS has saved my bacon several times.  It's a "must" when running PVE because it's an easy way to backup VMs.

Follow a 3-2-1 strategy for backups - at least 3 copies, at least two media types, at least one offline.  The original data is the first copy and PBS is the second _as long as the PBS VM sits on a seperate server_.  I recommend Backblaze as a third offsite copy. 

You should absolutely review the files that are being backed up to make sure that they include all of the correct files.  

It's also a rule, call it _Brent's Law of DR_, that untested disaster recovery never works.  You should do a test restore periodically.  This might be restoring just a sample file, or restoring an entire VM.  Testing the restore process ensures both that the backup is _really_ working and that you remember the steps necessary to invoke a restore.  

### PBS Setup
Setting up PBS is about as difficult as setting up PVE.  If you did the one, the other will not be a problem.  The documentation is good, but takes some time to read so here's a quick walkthrough of the actions you'll need to take.

1. Download and install PBS on a new machine.  I'm using an old NUC with a 5TB drive.  [Download](https://www.proxmox.com/en/downloads) the image and throw it on [Ventoy](/posts/210911_distrohoppingwventoy/)!  This is an easy and obvious setup, so I won't walk you through clicking "next".
{{< danger title="Danger!">}}
Don't install PBS on the PVE it's backing up!  If your Virtual enviroment fails, you don't want it to take backups with it!  If you don't have a suitable old PC, consider a 1L PC based on the modern Celeron. 
{{< /danger >}}
2. When the process is complete, you can administer PBS from https://BACKUPSERVER:8007.  Login as root and the password you chose during setup.
3. Setup a Datastore on PBS.  "Add Datastore" is found under Datastore on the Proxmox menu.  This will make a local path available for backup use.

![PBS Datastore Setup](/240204_pbsdatastore.png#center)

4. Setup Pruning to keep the latest backups.  This setup mostly comes down to the crossing point of your budget, paranoia, and available space.  You can specify the number of backups to keep over various timeframes, such as days, weeks, months, and years.  I backup weekly and want to keep a couple backups, but I also keep monthlys as well in case I realize later that I need a restore.

5. Get the "fingerprint" using the big "show Fingerprint" button on the dashboard.

### PVE Setup to use PBS
![PBS setup in PVE](/240204_pbssetup.png#floatright)
1. Back in the PVE environment, go to the server view and select DataCenter > Storage.

2. Click Add and choose Proxmox Backup Server from the drop-down menu.

3. Give the backup server an ID and enter in it's IP address.  Use _root@pam_ as the username, the password you set on PBS and then supply the datastore setup in Step 3 above.  Here's where you'll need to paste in the fingerprint from PBS.  This is also the place where you can fiddle with retention and encryption if needed.

### Setup Backups

![PBS Job setup](/240204_automated_pbs.png#floatsmallleft)

The simple way to backup a VM at this point is just to select a VM and choose "Backup".  No one remembers to do backups though, so the key is to have them happen automatically.

1. Still under Datacenter, choose backup and __Add__.

2. To backup all VMs from all PVE instances weekly, use the following settings:
  * Node __--All --__
  * Storage __pbs__
  * Schedule __sun 01:00__
  * Selection: __Include selected VMs__ or __All__
  * Send email: __Always__
  * Send email to:  _you@yourdomain.tld_
  * Mode: __Stop__

At this point, weekly backups should start flowing in.  PBS will deduplicate and make very efficient use of space, so even a 4TB backup drive has been fine for me.

### Restore a file
Restores are conducted from the PVE admin screen (http://your-pve-server:8006).

![PBS Restore Admin Screen](/240210_pbs_screen.png)

1. Under the  Server View Datacenter heading, go to the target server and select the Proxmox Backup Server storage.  Mine is called _pbs_ in the screenshot.  Because it's attached to the datacenter, that storage shows up under every server.  It's the same thing, so for file recovery just pick a server to work through.

2. Pick the backup that has the file  you'd like to restore.  Depending on the backup policy you specified, there will be several copies over a period of time.  If you're restoring because you borked the config last week, for instance, you may need to go back two weeks to find a working copy.

3. Once you've found your target, select File Restore.  I know, kind of obvious.

4. This opens a file viewer that lets you burrow into directories to find your file.  Once selected, choose the Download button to get the file.

### Restore a VM

1. Under the  Server View Datacenter heading, go to the target server and select the Proxmox Backup Server storage.  Mine is called _pbs_ in the screenshot.  Because it's attached to the datacenter, that storage shows up under every server and so I'm selecting the one under the server that I want to restore to.

2. Pick the backup you'd like to restore.  Again, there will probably be several copies.

3. Once you've found your target, select Restore.  

4. When you restore, you can select the target PVE instance, the storage on that PVE you want to use, and the VM ID.

You can restore it to the existing server, using the existing VM ID, and overwrite the current copy.  You can also choose to restore it to a different ID so that there's a second copy.  That could be useful if you're trying to clone a setup.

## Final Notes
The process of setting this up isn't too confusing.  I find the Proxmox interface takes a little time to develop an intuitive feel, but it's consistent and mades sense.