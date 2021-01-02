---
title: "SSHFS_Automount"
date: 2020-10-17T12:13:01-04:00
draft: false
author: "Brent Stewart"
Victor_Hugo: "true"
Focus_Keyword: "SSH"
github: ""
youtube: ""
refs: ["https://wiki.archlinux.org/index.php/SSHFS"]
tags: ["SSH", "Linux"]
---

I described using SSHFS as an alternative to NFS back in [Using SSH3](/using_ssh3). I've been using SSHFS as a standard way to mount since then, partly because I can use the same technique on a variety of OS and partly because it seems to work cleaner for me than straight NFS. However, I've been using a batch file to mount drives and that's getting old. I'd like to just add the SSHFS targets into _/etc/fstab_ and get them to automount.
As a general rule, the Arch Wiki is a great place to find all things Linux. Even though I'm running an Ubuntu variant, the Arch Wiki set me straight. For this to work there are a number of things that have to be set.
First, as described in [Using SSH2](using_ssh2) I need to make sure that logging into my target is done with keys so that an interactive password is not required. See the previous article for a more detailed walk through, but the basic process is:

> ssh-keygen  
> ssh-copy-id brent@192.168.1.1

# test

ssh brent@192.168.1.1

Second, edit _/etc/fuse.conf_ to allow non-root users to access drives when they're mounted with the _allowother_ option.

> sudo nano /etc/fuse.conf

# add or uncomment the following line

user_allow_other

Find out your user id and group id. This is easy: there's an **id** _username_ command. Note that the ellipsis below just indicate that I'm in other groups and I've edited those out.

> ➜ **id brent
> uid=1000(brent) gid=1000(brent)** ...

Finally, add the targets to your _etc/fstab_.

> **sudo nano /etc/fstab  
> sshfs#brent@192.168.1.1:/home /home/remote fuse user,\_netdev,reconnect,uid=UID,gid=GID,idmap=user,allow_other 0 0**

When you exit nano the remote directory should be mounted and active! It will also be there automatically each time you log back in. Hope this is helpful!
