---
title: "Running GNS3 on Proxmox VE"
description: ""
author: "Brent Stewart"
date: "2023-01-21T10:00:18-05:00"
math: false
draft: false
Victor_Hugo: "true"
picture: "gns3"
Focus_Keyword: "GNS3"
youtube: ""
github: ""
refs: ["https://www.gns3.com", "https://www.proxmox.com"]
tags:
  - "GNS3"
  - "Proxmox"
  - "Virtualization"
---
I used to run [GNS3 on ESXi](/posts/210421_gns3_attached_to_esxi/), by my really-old Cisco UCS server is not supported by current versions of VMWare and I wanted to move to a FOSS solution anyway so I've been using [Proxmox VE](/posts/220219_proxmox/) for about a year.  I have a generally positive impression of Proxmox - in fact, one of the most positive things I can say is that it's been pretty much a drop-in replacement.

## The Issue
Proxmox virtualization is Qemu-based and the [GNS3 VM](https://gns3.com/software/download-vm) is distributed for VirtualBox, VMWare Workstation, VMWare ESXi, and Hyper-V.  I've worked around that by running GNS3 in VMWare Workstation, but I've run into some issues with Workstation causing my machine to "freeze" so I got interested in moving the GNS3 VM to my virtualization host.  A little poking around found that other people have been successful with this and provided some ideas.

## How to run GNS3 VM on Proxmox VE

Nothing stands still, so before we get into instructions I'll date this article.  It's January, 2023, and I used this process to get the VMWare ESXi GNS3 VM 2.2.36 to run on Proxmox VE 7.3-4 using Linux pve kernel 5.13-19-15.  I think Proxmox is now shipping 5.15, but I ran into a boot issue on my server and haven't upgraded the kernel since I fixed it.

1. The first step is to [download](https://gns3.com/software/download-vm) the VMware ESXi GNS3 VM.
2. The downloaded file is a ZIP, which expands out to an OVA.  OVA is really just another ZIP, so expanding that out gives you two VMDK virtual disks, a manifest(MF) file and an OVF file.  The OVF provides information about the virtual hardware environment and the manifest has the hashes of all the files in the OVA.  We're only interested in the two VMDK files (GNS3_VM-disk1.vmdk and GNS3_VM-disk2.vmdk).  You can expand these out using most file managers of via the command line.
```
    tar -zzvf GNS3_VM.ova
```
3. Upload the VMDK files to Proxmox.  In the _root_ home directory, I created an _import_ subdirectory for the files.  This is very easy to do with Filezilla or from the command line.
```
    # sftp brent@server
    sftp> cd import
    sftp> put GNS3_VM-disk1.vmdk
    sftp> put GNS3_VM-disk2.vmdk
```

4. Create a new Qemu VM in Proxmox by clicking the "Create VM" button in the upper right of the management page. ![Create VM](/230121-createvm.png)

5. You can accept the defaults on the General tab.  Under OS, steup the VM without any media. 
 ![No Media](/230121-PVEnomedia.png#center)

6. Accept defaults on the System tab and Disks tab.  I set my VM to use 4 processors (2 sockets, 2 cores each) and set the default memory to 16GB.  I didn't see a lot of guidance on this, so that was a SWAG.  Note the ID (105 in my case).
![GNS3 VM](/230121-GNS3VM.png#center)

7. After the VM is created, select the VM, go to the Hardware section, and select the disk.  Click Detach and then Remove from the menu bar.
![Remove disk](/230121-PVErmove.png#center)

8. Import the VMDK disks into Proxmox.  SSH into the host and use the __qm__ command to convert the disk to QCOW2 format and import it for use.  Change the machine ID (105 is my GNS3 VM) and make sure the VMDK file name matches before running the command below.
```
    root@pve:~/import# qm importdisk 105 GNS3_VM-disk1.vmdk local-lvm -format qcow2
    importing disk 'GNS3_VM-disk1.vmdk' to VM 105 ...
    Logical volume "vm-105-disk-0" created.
    transferred 0.0 B of 19.5 GiB (0.00%)
    transferred 200.0 MiB of 19.5 GiB (1.00%)
    transferred 400.0 MiB of 19.5 GiB (2.00%)
    ...
```

9. Attach the QCOW2 images to your VM using the Add button.
10.  Go to the Proxmox admin page, select the VM, select options, and set the boot order.  Double-click the boot Order line and a window will appear to allow you to drag the disk into the correct order.  Disk1, the smaller of the two images, should be enabled and the boot drive.
![Boot order](/230121-PVEboot.png#center)

11. One other tweak that I used: I run multiple VLANs and so I went into Hardware and double-clicked the Network Device to put the device into my office VLAN by setting the tag.
![Networking](/230121-PVEnet.png#center)

12.  At this point, I was able to start the VM.  GNS3 started up in the correct VLAN and grabbed a DHCP address.  Opening a console, the boot screen of the VM shows the IP address assigned.  Accessing this address from a browser will provide the GNS3 web console.  You can also use this address in the GNS3 front-end to identify the back-end server.

I've been using the web version more this time and continue to be impressed by it and by the progression that the GNS3 team is making on it.  I was able to add appliances using either interface - I actually find the web client to be a little easier in that regard.  Early testing is great, although I can always go back and commit more cores or more memory if needed.