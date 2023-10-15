---
title: "Setting up a Basic FTP server on Linux"
date: 2022-01-17T15:49:07-05:00
draft: false
author: "Brent Stewart"
Victor_Hugo: "true"
Focus_Keyword: "FTP"
picture: "linux"
github: ""
youtube: ""
refs: [""]
tags: ["Linux","FTP"]
---

# Brother Printers work well with Linux

My last two printers have been Brother multi-function copiers (MFCs).  These printers work really well with everything in the house - Windows, Linux, even the school Chromebooks.  They have a Linux driver available on their website for all their printers, the print quality is excellent, and they've been very durable.

If you're looking for a printer that plays well with others, take a look.

My Brother MFC-L3770CDW printer can scan to SMB, FTP, or a flash drive you plug in.  I wanted to set it up to FTP scans into the personal directories of family members.

# Basic FTP with Linux

FTP is a very old file transfer protocol.  VSFTP ("Very Secure") is the default FTP server for Linux.  Regardless of the name, plain vanilla FTP transmits usernames and passwords in clear text and setting it up to be "Very Secure" takes a little extra effort.  I'm also using it because it's compatible with my printer, but securing it might interfere with that connection.  So this setup isn't security optimized and is really only appropriate for a home network.

_As a note, if secure file transfer is interesting to you, check out the articles on [SSHFS](/posts/200813_using_ssh3/)._

On Ubuntu, installation can be done through apt.  Configuration is done by editing the _/etc/vsftpd.conf_ file.  After editing, you'll need to restart the service.

    sudo apt install vsftpd
    sudo nano /etc/vsftpd.conf
    sudo systemctl restart vsftpd

The configuration file is mostly comments, with each setting preceded by documentation.  Remove the hash to "uncomment" a line.  Here's the relevant settings that I used in this case.

    listen=YES
    listen_ipv6=NO
    anonymous_enable=NO
    local_enable=YES
    write_enable=YES
    allow_writeable_chroot=YES

This configuration turns off IPv6 since I'm not using it.  It limits access to valid users on the server, allows those users to write files, and limits each user to their personal folder.  VSFTP will throw an error if a chroot'd user has both SSH and FTP write priviledges, which makes sense under more secure circumstances.  In this case, I turned off the security check.  Be very careful when setting this up, sentences like that last are usually followed by tragedy!  It's only appropriate because I'm limiting access to the server and it's not "mission critical".  

Testing is easy with Filezilla or from the command line.

    brent@server:~$ ftp 192.168.1.2
    Connected to 192.168.1.2.
    220 (vsFTPd 3.0.3)
    Name (192.168.1.2:brent): brent
    331 Please specify the password.
    Password:
    230 Login successful.
    Remote system type is UNIX.
    Using binary mode to transfer files.
    ftp> 

Once complete, this worked like a champ.  I created _~/scan_ directories under each user and setup a quick-connect button from the Brother printer.  Voila - scans directly to each users home directory!