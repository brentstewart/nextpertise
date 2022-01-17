---
title: "Using WebDAV on Apache"
date: 2021-09-06T13:00:30-04:00
draft: false
author: "Brent Stewart"
Victor_Hugo: "true"
Focus_Keyword: "WebDAV"
picture: "apache"
github: ""
youtube: ""
refs: ["https://httpd.apache.org","https://www.isc.org/bind/"]
tags: ["Apache","How To","Linux"]
---
In recent articles, I walked through how to setup a home [webserver](/210830_apache) with [Apache](https://httpd.apache.org) on Linux and how to configure home [DNS server](/210831_dnsonubuntu) using [bind](https://www.isc.org/bind/) on Linux, complete with custom in-home domain for local name resolution.  This article revisits the webserver and creates a second virtual host to handle WebDav.

WebDAV is a file sharing protocol built on top of HTTP.  Many operating systems can attach to WebDAV folders to upload and download files, including Linux, Windows, Mac, IOS, and Android.  I have a password database that I want to keep sync'd between different computers and phones and I'm not comfortable hosting that "in the cloud", so this allows me to self-host.

I'm using Ubuntu Server 21.04 for this exercise.

## Setting Up DNS
My forward lookup zone file includes an A record for the server and a CNAME for the dav share, similar to the output below.

    www IN  A   192.168.26.53
    dav IN  CNAME   www.stewart.lan

Once the zone file is updated and __named__ restarted, this can be tested by pinging "www.stewart.lan" and "dav.stewart.lan".

## Setting up Apache
If you haven't already done so, the first thing to do is to install apache2.  Next, enable the webdav Apache modules.  Apache using __a2enmod__ and __a2dismod__ for handling modules.  Finally, create a folder to handle the WebDAV files and set the permissions up.  When complete, restart Apache to load the modules.

    sudo apt install apache2
    sudo a2enmod dav
    sudo a2enmod dav_fs
    sudo mkdir /var/www/dav
    sudo chown -R wwwroot:wwwroot /var/www/dav
    sudo systemctl restart apache2.service

## Setting up the DAV site
Apache is now ready to host a WebDAV site, but needs a configuration.  For this, create a text file under /etc/apache2/sites-available (I named mine _dav.conf_).  The serveralis parameter tells it to respond to requests to dav.stewart.lan and the alias directive tells Apache the root location is the _dav_ folder.


    brent@webnamer:/etc/apache2/sites-available$ cat dav.conf 
    DavLockDB /var/www/DavLock                      #database file Apache uses to lock files
    <VirtualHost *:80>
        ServerName stewart.lan
        ServerAlias dav.stewart.lan
        alias / /var/www/dav
            ServerAdmin brent@stewart.tc
            DocumentRoot /var/www/dav/
            <Directory /var/www/dav/>
                    Options Indexes MultiViews
                    AllowOverride None
                    Order allow,deny
                    allow from all
        </Directory>
        <Location /dav>
            DAV On
            AuthType Basic
            AuthName "webdav"
            AuthUserFile /var/www/passwd.dav
            Require valid-user
        </Location>
    </VirtualHost>

Enable the site and reload Apache

    a2ensite dav.conf
    sudo systemctl restart apache2.service

## Add Authentication
Go to the directory referenced by DavLockDB and create an empty file named _users.password_.  Set the file ownership to www-data.  Finally, add users to this file using htdigest (you'll be prompted for passwords).

    sudo touch /var/www/users.password
    sudo chown www-data:www-data /var/www/passwd.dav
    sudo htdigest /var/www/passwd.dav webdav newuser
    Adding user newuser in realm webdav
    New password:

## Confirming the setup
There are a _lot_ of ways to test.  You can browse to that URL, use an application, or attach to it from your file manager using the url webdav://dav.myserver.  Confirm that you are prompted for a password as expected!