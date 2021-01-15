---
title: "Alternate GNS3 Symbols"
date: 2020-08-21T13:18:28-04:00
draft: false
Victor_Hugo: "true"
Focus_Keyword: "GNS3"
picture: "gns3"
author: "Brent Stewart"
github: "https://github.com/brentstewart/GNS3Symbols"
youtube: ""
refs: ["https://gns3.com/","https://github.com/GNS3/gns3-registry.git"]
tags: ["GNS3"]
---
GNS3 is a graphical network simulation tool.  Imagine something like Visio that let you place network devices and draw connections, then boot them up and interact with them.  The screenshot above is a simulation I ran that used five Cisco CSR routers to demonstrate BGP for a class.  The devices typically run in KVM or Docker, but can use VMWare or Virtualbox.  This article assumes that you are familiar with the project.
![GNS3 Screen Capture](/GNS3Lab.png#center)

I've found GNS3 to be an invaluable tool.  I used it to do labs while writing books years ago.  I've used it to teach networking and security to the people who work with me, and to do labs to teach myself.  I've also used it to simulate an environment and walk through a change process to practice, verify steps, and perfect configuration updates.

GNS3 originally had one set of symbols that could be used for the various virtual machines.  The switches labeled "Home" and "Remote" in the screenshot are in this style.  The GNS3 team added a family of symbols labeled "Affinity" (the CSRs use this type of symbol) which is very clean and modern.

![Untangle](/untangle2.png#floatright)
![VyOS](/vyos.png#floatright)
The Affinity symbols work very well for most of my labs, but sometimes I need to differentiate between a _Cisco_ router and _VyOS_.  Sometimes you just need to call out a particular device.  In fact, one of the symbols in my library is a big red X that I used to denote a server in a special project.  For this reason, I created a library of symbols using vendor/project logos.  The link for that project is in the reference section.  Here are two sample icons I created.

Per the specifications posted in the GNS3 Registry project, symbols should be SVG less than 70 pixels tall.  I've found that PNG with transparent background also looks good.  I typically use the scaling feature in GIMP (_Image>Scale Image_) to adjust image size.

If you'd like to use these symbols, clone my repository.  In GNS3, you can update the device template (and thus all future devices) by right clicking in the device selection window, choosing _Configure Template_, and then browsing for the Symbol.  Alternatively, you can use a symbol for one instance by right clicking on the VM in the topology and choosing _Change Symbol_.

I add to the repository periodically and I'd love to have contributions from you.  You can also submit them to the GNS3 project as a pull request on the _GNS3 Registry_ project.