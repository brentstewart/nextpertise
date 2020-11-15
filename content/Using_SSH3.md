---
title: "Using SSH - Part 3 (File Shares)"
date: 2020-08-13T11:15:42-04:00
draft: false
Victor_Hugo: "true"
Focus_Keyword: "SSH"
author: "Brent Stewart"
github: ""
youtube: ""
refs: ["https://filezilla-project.org/","https://play.google.com/store/apps/details?id=com.cxinventor.file.explorer&hl=en_US", "https://github.com/billziss-gh/winfsp/releases/tag/v1.7", "https://github.com/billziss-gh/sshfs-win", "https://github.com/evsar3/sshfs-win-manager"]
tags: ["NFS","SSH", "Linux"]
---

One of the basic things you want to do on a network is share files.  At one point, everyone had a Windows PC and this involved shared directories and Network Neighborhood.  It had a lot of issues, but it worked.  However, today we have a variety of clients and CIFS isn't an easy (or appropriate) fit for all of them.  This article focuses on home users, but enterprise users face some of the same challenges.  There are a lot of ways you _could_ do this; I'm going to share how I'm currently doing it.  My environment includes several versions of Linux, Windows 10, a Mac, Chromebooks, and Android Phones.

I should start by saying that I'm _not_ using Microsoft sharing -- what has been variously termed SMB (Server Message Blocks) or CIFS (Common Internet File System).  My experience with SAMBA (SMB on Linux) has been uneven and I've never wanted to invest the time.  Your mileage may vary, but trying to sort out access and permissions and deal with the impact of software updates was a drag.

## SFTP ![Filezilla](/Filezilla.png#floatright)
SFTP is a Secure File Transfer Protocol built on top of SSH, and the two are usually bundled together since they are complementary.  One of the easiest ways to use SFTP to transfer files on all platforms is to use _Filezilla_.  Filezilla presents a left/right here/there file manager that allows easy drag and drop between locations.  It works most places SSH works.  Login using your SSH credentials and set the port to 22.  If you use Filezilla often, the first button on the left is the Site Manager and remembers common destinations.  Filezilla works, but there's no way to open a file in an application from the other disk.  It must be copied locally and this creates multiple file versions and is onerous to use.  So - Filezilla if nothing else works.

![Caja](/caja.png#floatleft)Linux file managers like _Caja_ and Finder on the Mac allow you to attach to an arbitrary destination in an ad-hoc fashion (Windows does not).  For Linux and Mac, just use existing SSH credentials.  This method also supports FTP, CIFS, and WebDav.  I don't recommend FTP because it's not secure and it's a very old protocol and can be difficult to handle on firewalls.  WebDav is slow and involves some Apache setup.  It can be secure, but most folks setting up a quick file share won't take the time to make it so.  I recommend SSH/SFTP.  File managers generally allow bookmarking, but don't automatically reconnect.  I'll walk through a technique that builds the connection at startup later in the article. 

On Android, I'm using _Cx File Explorer_.  This application allows me to connect to SFTP resources and bookmark them.  Cx integrates with the rest of Android, so I can do things like type an email and use Cx to attach a file from the server.  Cx has the same requirements SSH does - a network path to the server and credentials. For me, a common use is to grab a PDF from the server and transfer them to my Kindle.

## Aside - NFS

Network File System (NFS) is a dream for devices that support it.  It lacks the ad hoc browsing you might do on a Windows network, but at home I want all the files on the servers and if I have to do horizontal file sharing I can figure it out.  Setting up NFS on the server involves getting the NFS server, setting up the _/etc/fstab_ configuration file, and publishing the share using __exportfs__.  The example below publishes my user directory from the server.

> __sudo apt install nfs-kernel-server nfs-common__  
> __sudo nano /etc/exports__  
>> _add lines similar to this one_  
>> __/home/brent 192.168.1.0/255.255.255.0(rw,anonuid=1000,anongid=1000,sync)__  
>> _save file_  
>  
> __exportfs -avf__ 

On the client, I'll map this share to a folder so it sits in my directory tree.  In this case, I want my server user directory to fit under my local user directory as the _server_ sub-directory.

> __mkdir ~/server__  
> __sudo nano /etc/fstab__  
>> _add lines similar to this one_  
>> __192.168.1.1:/home/brent /home/brent/server nfs default 0 0__  
>> _save file_  
> __sudo mount ~/server__  

It should just work, but you may need to use __mount__ to kick it in the rear.  Because this is setup in your _fstab_ file, it will automatically reconnect when you restart.  My personal workflow is to save all my work products to the server because that's what is being backed up.  I use the local folders for scratch files, downloads, etc.  I like to try new things and end up re-installing my OS on my desktop about three times a year.  I can throw my Ventoy USB stick in the PC, pick a distro, and be back up with no lost data in minutes!

NFS works great for Linux to Linux filesharing.  I didn't have great success with Windows.  There is a process that includes using _Services for NFS_, but I won't even link to it.  It was difficult to get working and didn't "just work" in the way that I wanted for my wife's PC.  I haven't seen a way to use this with Android and haven't attempted with Chrome.  On the Mac, this works fine and is supported by Finder.  The procedure is just:

> __showmount -e 192.168.1.1__  _#view available shares_
> __sudo mkdir /server-files__ _ #depending on where you put it, you may not need sudo
> __sudo mount -o rw -t nfs 192.168.1.1:/home/brent /server-files__

NFS can be secure.  NFSv4 encrypts traffic in-transit and v2/3 allow you to limit promiscuous connections using a mask.  In the enterprise or if your traffic crosses a public network you _really_ need to use v4. 
![SSHFS Win](https://raw.githubusercontent.com/billziss-gh/sshfs-win/master/cap.gif#floatright)
##  SSHFS 

SSHFS is a file system using SFTP.  Since SFTP is built on top of SSH, SSHFS inherits all the goodness.  SSHFS  works for everything I've tested so far - I haven't gotten to the Chromebooks yet, but I _have_ used it in Haiku.  SSHFS requires installing the sshfs package and installing the SSH server daemon.  File permissions are communicated based on how you login.

On Linux, the command to mount a directory using SSHFS looks like this (the server is 192.168.1.1).
> __sudo apt install sshfs__
> __mkdir ~/server__  _#if it doesn't already exist_  
> __sudo sshfs -o allow_other,default_permissions brent@192.168.1.1:/home/brent /home/brent/server__  

You can add this to fstab if you want it to be automatic.
> __sudo nano /etc/fstab__  
>> _# add this line_   
>> __sshfs#brent@192.168.1.1:/home/brent /home/brent/server fuse.sshfs _netdev,idmap=user,uid=1001,gid=1002,allow_other,default_permissions 0 0__  


For Windows, I'm using a stack of WinFsp, SSHFS-Win, and SSHFS-Win-Manager (links in notes).  Here's the procedure:![SSHFS-WIn-Manager](/SSHFS-Win-Manager.png#floatleft)
1. Install _WinFsp_ from Github - there's an MSI attached to the latest release (I tested with winfsp-1.7.20172.msi)  
2. Install _SSHFS-Win_ from Github - again using an MSI (I tested with SSHFS-Win-3.5.20024-x64.msi).  At this point you can map drives using the UNC \\sshfs\user@server.  

![SSH-Win-Manager Adding a Conneciton](/SSHFS-Win-Manager-Add.png#floatright). This is aimed at the family members who _don't_ want to futz around with computers all day, so install _SSHFS-Win Manager_ from Github (I tested with sshfs-win-manager-setup-v1.0.1.exe).  Once installed, click "add connection".  The connection information is standard SSH information.  To attach my remote user directory to my local one as in the earlier example, I would specify a Remote path of _/home/brent_ and a Local Path of _/home/brent/server_.  