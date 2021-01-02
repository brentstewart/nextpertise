---
title: "GNS3_2.2.14"
date: 2020-09-15T09:21:45-04:00
draft: false
author: "Brent Stewart"
github: ""
youtube: ""
refs: ["https://gns3.com/community/blog/gns3-2-2-14-released", "https://github.com/GNS3/gns3-gui/releases"]
tags: ["GNS3"]
---

GNS3 2.2.14 came out today (9/15).  The GUI fixes a bug in the way new appliance versions are created.  The server side tweaks how Qemu runs and includes Beta 4 of the Web-UI.
![GNS3 WebUI](/GNS3WebUI.png#center)

## Should you upgrade?
This version doesn't include a security fix, and the bug fixes aren't issues I've run into.  The Web-UI is coming along nicely.  In my initial testing, I'm able to add a node to a topology, add links, and manage things from the webpage.  It looks like this would make it easy to collaborate with others on a shared topology.  I can also envision using this as my sole access to GNS3.  I  suggest you start evaluating it.  If you do find a bug, be sure to log an issue on the GNS3 Github repository.

I have run into issues before that affected my ability to run GNS3 and took me a day or so to sort out (at least once was because I upgraded my OS, not because of GNS3).  If you have ongoing work that depends on GNS3, hold off until you have a little slack in your schedule.  One way to evaluate the risk is to look at the GNS3 community forum to see if there are posts about the new version.

My personal experience with GNS3 has been that _most_ upgrades go without a hitch.  I usually just go for it, but I'm not typically dependent on GNS3 from day to day.  __Note__ that _gns3-gui_ and _gns3-server_ have to be the exact same version.  If for some reason you upgrade one, you either have to roll back or upgrade the other.

![GNS3 Server Upgrade](/GNS3ServerUpgrade.png#center)
## How do you upgrade?
On Windows, just download the executable and run it.  On Ubuntu, __sudo apt upgrade__.  If you have a server VM (and I recommend it), start by getting a snapshot of your current server.  I once had a server upgrade go poorly that resulted in rebuilding my VM, so this is a realistic risk.  After that, log into the server and you can kick-off the upgrade from the menu.  