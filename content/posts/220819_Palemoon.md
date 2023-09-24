---
title: "Palemoon for legacy Flash support"
description: "Palemoon iu"
author: "Brent Stewart"
date: "2022-08-19T09:07:14-04:00"
markup: 'mmark'
math: false
draft: false
Victor_Hugo: "true"
picture: "cisco"
Focus_Keyword: "Legacy Flash support"
youtube: ""
github: ""
refs: ["https://linux.palemoon.org/download/mainline/","https://github.com/darktohka/clean-flash-builds/releases/tag/v1.7"]
tags: ["Cisco","Network","Linux"]
---
Flash was a technology to extend the functionality of websites past what was possible with HTML at the time.  It allowed for highly interactive experiences and was used for streaming, video games, and for "application like" experiences inside the browser.  Flash was implemented on the client-side via a browser "plug-in" and was notorious for security issues.  In my experience, a big part of the problem with Flash was the update process.  The plug-in was updated seperately from the Operating System and browser, leading to many cases to persistence of older versions.  Furthermore, the installer would commonly not clean up old versions, leaving an attack surface.

Why do we care?  For the most part we don't.  Flash was deprecated in 2020 and is no longer supported by the major browsers.  The functionality of Flash has been ably replaced by HTML5.  However, there was a range of IT products created in the mid-teens that used a Flash console for administrative access.  Cisco used Flash in equipment like ASA firewalls, SOHO switches, and UCS servers.  Much of that equipment is ageing out, but some of it is still in good shape and capable of delivering value.  One example is my home server, which is an old Cisco UCS M3.
## Accessing a Flash Console
ProxMox recently crashed - I'll write about that seperately - but I suspected a disk issue.  The best way to access the information I needed was through the CIMC (Cisco Integrated Management Console), an out-of-band server management interface other vendors refer to as Integrated Lights Out access (ILO) or DRAC (Cell Remote Access Controller).  
I hadn't accessed the CIMC in a long time.  Rebooting the server displays the assigned IP and let's you setup the interface.  I had assigned an IP and identified it as in a VLAN on a trunk port.  However, pinging the IP was unsuccessful.  I used the Meraki "clients" display to identify the switch port used and setup a continuous ping from my workstation.  I tried a variety of configurations on the CIMC and switch, but ultimately what worked was to set the port as access (turn off 802.1q) and let the speed and duplex auto-negotiate.  I originally had this set for trunk, then tried trunk and identified the VLAN as the "native" VLAN which should have removed the .1Q shim from the header.  I _suspect_ that the UCS wanted to run fast ethernet and had some compatibility issue with .1Q as spoken by the Meraki.
> Step 1 - set the port to auto/auto, define the VLAN and set the mode to access

With the CIMC port responding, I could browse to it using it's IP address.  The next problem is that the site presents a security warning.  Although the CIMC uses TLS 1.2 (which is still supported), it uses 128b keys (which are not).  Mozilla [phased out](https://wiki.mozilla.org/Security/Features/Certs_Disallow_Weak_Keys) key sizes smaller than 2048b at the end of 2013.  Even getting around this issue still leaves us with the Flash problem.

[Palemoon](https://linux.palemoon.org/download/mainline/) is a browser forked from Firefox years ago and developed in the years since.  It maintains compatibility with the older XUL-based plugins.  It is distributed as a tar-ball, so I just extracted it to my _apps_ directory and ran the palemoon executable.
> Step 2 - Download Palemoon, extract and run

The flash plugin was abandoned at version 34.0.0.137 and can be obtained from [Github](https://github.com/darktohka/clean-flash-builds/releases/tag/v1.7).  Again, it can be installed directly from github using the following command.


     mkdir -p ~/.mozilla/plugins && wget -q https://github.com/darktohka/clean-flash-builds/releases/download/v1.7/flash_player_patched_npapi_linux.$( (( $(getconf LONG_BIT) == 32 )) && echo "i386" || echo "x86_64").tar.gz -O - | tar -zxf - -C ~/.mozilla/plugins libflashplayer.so

After installation, Palemoon is able to access the Flash-based admin console for the UCS server.  The installation did not impact my current (104) version of Firefox.
> Step 3 - Install Flash from Github

## This is a bad idea
_Palemoon_ is an interesting browser and - to my knowledge - hasn't had security concerns associated with it specifically.  However, Flash was deprecated for a reason and this article walks through installing unpatched and unsupported legacy software into a browser.  I would limit Palemoon to internal trusted addresses as long as the Flash plug-in is present and active. This can be addressed to some extent by limiting when Flash is allowed to run, as shown below, but I would still be very cautious.

![Limiting Palemoon/Flash exposure](/220818_palemoon_always_activate.png)