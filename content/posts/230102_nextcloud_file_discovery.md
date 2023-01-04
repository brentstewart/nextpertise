---
title: "Nextcloud File Discovery"
description: ""
author: "Brent Stewart"
date: "2023-01-02T14:47:26-05:00"
markup: 'mmark'
math: false
draft: true
Victor_Hugo: "true"
picture: "self-host"
Focus_Keyword: "Nextcloud"
youtube: ""
github: ""
refs: [""]
tags:
  - "self-host"
---

I've heard good things about Nextcloud for a long time and finally decided to take the plunge.  I have a home file server that is accessed via sshfs and nfs, but it's a little problematic to get to from Windows or mobile.  Nextcloud extends the file sharing through a web and dav interface to make it accessible from anywhere.  Nextcloud offers a lot of functionality, but my initial focus is just on straight-forward file-sharing.

## The Issue
Setting up Nextcloud went okay, but it defaults to sharing directories at _/var/www/nextcloud/data_.  I spent some time trying to change Nextcloud to access the directories that already had my files but didn't see a good way.  Option two would be to move the files and my first attempt was to access Nextcloud on my desktop PC and drag files from the NFS share.  That was  s l o w.

Rethinking my file movement, I accessed the server command line and moved the files.  Replace _Source_ and _user_ with the appropriate values.

    sudo mv /Source/ /var/www/nextcloud/data/user/files

This moves the files (as shown by ls) but doesn't show the files in Nextcloud.  

## The solution
There are two steps to finish this process.  First, change ownership of the files to www-data so that Nextcloud can access.  This command may take a moment to complete.

    sudo chown -R www-data:www-data /var/www/nextcloud/data/brent/files/

Once everything is in place, use Nextcloud's built-in __occ__ tool  tool to re-index user files.  Depending on the size of your file share, this may take more than a moment.  

    sudo -u www-data php /var/www/nextcloud/occ files:scan --all

Open Nextcloud and go to files - if already open, refresh.  Your files will now be visible.