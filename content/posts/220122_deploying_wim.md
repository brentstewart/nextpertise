---
title: "Deploying a WIM Image to VMWare"
date: 2022-01-22T12:54:46-05:00
draft: false
author: "Brent Stewart"
Victor_Hugo: "true"
Focus_Keyword: "deployment"
picture: "windows"
github: ""
youtube: ""
refs: ["https://www.microsoft.com/en-us/software-download/windows10ISO","https://github.com/FOGProject/fogproject","https://docs.microsoft.com/en-us/windows-hardware/manufacture/desktop/work-with-windows-images?view=windows-11","https://docs.microsoft.com/en-us/windows-hardware/manufacture/desktop/capture-and-apply-windows-using-a-single-wim?view=windows-11","https://docs.microsoft.com/en-us/windows-hardware/get-started/adk-install", "https://docs.microsoft.com/en-us/windows-hardware/manufacture/desktop/winpe-create-usb-bootable-drive?view=windows-11","https://docs.microsoft.com/en-us/windows-hardware/manufacture/desktop/oem-deployment-of-windows-desktop-editions-sample-scripts?preserve-view=true&view=windows-10#apply-image","https://docs.microsoft.com/en-us/windows-hardware/manufacture/desktop/compact-os?view=windows-11"]
tags: ["windows","vmware","virtualization"]
---
Work is Windows-based, but I need a Linux workstation with that set of tools.  I find WSL2 incomplete . . . partly because my personal workflow is Linux based.  I worked with the desktop expert and we agreed to use VMWare Workstation to deploy my Windows environment.  We also agreed that getting the standard image and figuring out how to deploy it to VMWare wasn't something he could support.  Fair, I'm the one trying to be a special case.  Based on our plan, I was passed a WIM file to deploy.

# What the heck is a WIM?
A WIM is a file-based Windows Image that is made to be easy to test and deploy.   It's kind of like a ZIP, to my understanding, in that it captures all the files and the directory structure of a partition in a file.  Being file-based makes it easy to modify (more on this later).  Because it's not a sector-by-sector image, you can deploy it to different sized drives.

## How don't you deploy it?
![Fog Project](/fogserver.png#floatsmallleft)
I tried several approaches to using the WIM file.  I'll mention them briefly here so that you can learn from my experience.

The first thing I tried was making a Windows VM using the Windows 10 downloadable [disk image](https://www.microsoft.com/en-us/software-download/windows10ISO) from Microsoft. Once booted, I added a drive and expanded the WIM to the new drive.  I deleted my drive with generic Windows and rebooted.  I think this didn't work because the new disk wasn't partitioned as a primary partition.  This approach may be doable, but I moved on pretty quickly.

The next thing I tried was to deploy it via netboot using a [Fog Server](https://github.com/FOGProject/fogproject).  That project is pretty stinking cool!  I was able to get a VM to reference the server for boot information.  The problem here was that I didn't know what it was looking for (first time with PXE).  When I decided it wanted an ISO, I thought "why not just boot the ISO directly in the VM?" and abandoned the Fog Server approach.  I may come back to this to learn more about PXE booting.

# How do you deploy WIM?

Microsoft has a series of articles that you'll need to read to understand how to do this.  I've referenced them, but be warned that they reference each other circlically and there's not a good starting point.  To help you, I'm going to attempt to draw a straight line through how _I_ accomplished this.

## 1 - build a WinPE DVD Image
You need to boot into a [WinPE](https://docs.microsoft.com/en-us/windows-hardware/manufacture/desktop/winpe-intro?view=windows-11) environment to deploy the WIM image.   Windows PE is a small OS made to facilitate installation, used by Windows as a "pre-boot" environment.  To do this, download the [Windows Assessment and Deployment Kit](https://docs.microsoft.com/en-us/windows-hardware/get-started/adk-install) on a Windows PC and install the ADK executable.

Start the __Deployment and Imaging Tools Environment__ as an administrator and create a working set of files using the __copype__ command.

    copype amd64 C:\WinPE_amd64

The documentation says you can build an ISO now.  __Don't!__  There are some batch files that will make this easier - download a zip from [here](https://docs.microsoft.com/en-us/windows-hardware/manufacture/desktop/oem-deployment-of-windows-desktop-editions-sample-scripts?preserve-view=true&view=windows-10#apply-image) and pull the scripts out and place them in the root of your WinPE directory.  Also, grab the WIM file that you're trying to deploy and stick that in the WinPE directory as well.  Now you can build the ISO using the __MakeWinMedia__ command.

    MakeWinPEMedia /ISO C:\WinPE_amd64 C:\WinPE_amd64\Acme_Installer.iso

That should create an ISO that's about a half-gig larger than the WIM file.
![TPM](/tpm.png#floatsmallright)
## 2 - Build a blank VM and enable Secure Boot

I created an empty VM with an empty hard drive.  The critical piece here is that my image expects to be deployed to a TPM environment.  TPM requires UEFI and that the VM be encrypted.

![VM Configuration](/vmsetup.png#floatsmallright)
Enable UEFI under Virtual Machine Settings: go to the Options tab, select Advanced, and set the Firmware type to UEFI and Enable Secure Boot.  This is shown in the picture to the right.  While at the Options tab, select Access Control, click the button to encrypt the virtual machine, and choose a password.

Next add the Trusted Platform Module.   Add it under the Hardware tab by clicking the Add button at teh bottom.

Finally, we need the new VM to boot from the ISO we created earlier.  Add the new WinPE ISO to the CD drive and make sure it's marked connected.

## 3 - Boot WinPE and deploy the image

Finally we're ready to install the WIM image!  Boot the new VM using the WinPE boot disk.  It will boot to a prompt.  The procedure here is laid out by [Microsoft](https://docs.microsoft.com/en-us/windows-hardware/manufacture/desktop/capture-and-apply-windows-using-a-single-wim?view=windows-11).  Use the scripts that you added to the boot disk to first help partition the drive and then to apply the image.  _CreatePartition-UEFI.txt_ and _ApplyImage.bat_ were included in those scripts.  Obviously image names will change.

    diskpart /s CreatePartitions-UEFI.txt
    D:\ApplyImage.bat D:\Images\ACME-Standard.wim

The ApplyImage batch file will ask a few questions you need to be prepared for.  You can safely answer "no" to all of them.
* You will be asked if you want to create a recovery partition.  Recovery Partitions are a great tool, but I want to keep this VM as small as possible and I'll setup recovery mechanisms at the VM level, so I answered no.
* Do you want a compact OS install?  This runs the OS from compressed files, saving a lot of disk space.  Of course, everything has to be uncompressed to be run, so it will slow things down and might take more memory.  Even though I want a small VM, I chose to not install it as a compressed OS because I want to have good performance.
* Does the WIM file have extended attributes? I'm not a Windows guy, but I chose "no" and everything was fine.

From this point, the VM will go through a preliminary setup, doing things like setting the keyboard type.  It will reboot, ask you to login, and then continue the higher level (Cortana led) part of setup.  From here, everything should install as you would expect!  