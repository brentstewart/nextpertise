---
title: "Filezilla"
date: 2022-06-26T18:00:16-04:00
draft: false
author: "Brent Stewart"
Victor_Hugo: "true"
Focus_Keyword: "Filezilla"
picture: "ftp"
github: ""
youtube: ""
refs: ["https://filezilla-project.org/"]
tags: ["ftp"]
---

Filezilla produces a set of graphical file transfer tools and this has been a high-quality product for a very long time.  I've used Filezilla since the early 2000s, as have a lot of other folks, and it's something of a standard in industry where FTP/FTPS is used.  Part of the charm is that the Filezilla client is very accessible for non-technical folks - a little introduction and they can manage their own downloads.

The traditional Filezilla open-source client supports FTP and SFTP.  Filezilla makes an open-source FTP/FTPS server that makes it easy to host an FTP site.  Filezilla server used to be a great way to support FTP on a Windows server (and I used it that way 15 years ago), but researching this article I see that it is now offered for other OS as well.  They have also released a [Filezilla Pro](https://filezilla-project.org/filezilla_pro.php) client that includes support for things like WebDAV, S3, and various cloud file stores.

![Filezilla](/220626_Filezilla.png#floatright)
## Using Filezilla

I still occassionally have to explain Filezilla to new folks, so I thought I'd document a quick run through.

When you first start Filezilla, local files are on the left and remote files are on the right.  Because you haven't connected anywhere, the right pane will be blank to start.  Note the path for each side is shown above the files (marked (1)).

You can "quick connect" by typing in the particulars in the top row (marked (2) in the picture).  To attach to a remote FTP server, for instance, you might fill in host with an IP address (like 1.2.3.4) or a Fully Qualified Domain Name (an FQDN looks like "ftp.makingthisup.com").  Username and password are for the remote system and will be supplied by the administrator of that system.  Most FTP systems use port 21 and most FTPS systems use port 22, so it's safe to assume these port numbers unless you've been given an alternative.  When everything is ready, click the button!


Another way to connect is to use bookmarks.  This is particularly useful if you attach to the same remote systems repeatedly.  To create a bookmark, click the top left icon that looks like a file folder (marked (3) in the picture).  Once that screen opens, click New Site and fill in the same particulars that were used for the quick connect.  Bookmarks offer a range of logon types including:
* Anonymous - don't trust any site using this 🙂
* Normal - just fill in the user/pass below.
* Ask - it will pop a box for login when you connect.
* Interactive - not really sure, don't recall using that
* Key file - used if you have a cryptographic key supplied by an SFTP server owner.  If you are using FTP you don't need this.

Once you're done, click OK to save it or "Connect" to save and to start a session immediately.

Remote files will appear in the area marked (4).  Filezilla functions like a normal file manager with two side-by-side windows open.  Click ".." to go up a level, click a directory to open it.  Once you've found the files you need, either double-click the remote files to start a transfer or drag them to a local directory.

Filezilla has a lot of features that address more advanced cases, but this introduction covers the basics that are used 99% of the time. 