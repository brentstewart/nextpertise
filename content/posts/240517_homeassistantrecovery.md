---
title: "Home Assistant Device Recovery"
description: "How to find device IPs"
author: "Brent Stewart"
date: "2024-05-18T15:09:11-04:00"

math: false
draft: false
Victor_Hugo: "true"
picture: "homeassistant"
Focus_Keyword: "Home Assistant"
youtube: ""
github: ""
refs: [""]
tags:
  - "home assistant"
---
## The problem
Spectrum was in my neighborhood, extending their cable network finally.  Bear in mind that Spectrum (Charter in those days) promised this "soon" when I first moved in.

So, anyway, 24 years later they were installing conduit, nicked a power line, and shut down the neighborhood.  As much as I'm griping about others, this story is really about personal stupidity.  I setup a bunch of Shelly plugs to work with Home Assistant and didn't lock down their IPs.

## Is this, really a problem?
Yes, it is.

Devices are added to Home Assistant under settings > devices and then choosing the Add Device button. The Device settings records the MAC, but the IP isn't shown anywhere in Home Assistant that I could find.  For completeness, yes, I googled it as well.  I guess I assumed that the discovery was via MAC, but after the Spectrum reboot my devices all got new addresses from DHCP and all my buttons were broken.

One way to recover would be to rediscover all the devices and subsequently rebuild all my dashboards and integrations.  I'd prefer not to do that.  I didn't label devices before, nor did I write down the original IPs, so that process sounds like work.  I had one device that I _did_ have an IP for so I tried to change the IP back and was able to verify that using the original IP will fix the integration.

## I need a map
I was able to find my original IPs in the Home Assistant container config files.  From the container host, I used docker to access the host as shown below.  __-i__ is for interactive and __-t__ is for a terminal, with homeassistant being the container name (found via "docker ps").

    sudo docker exec -i -t homeassitant bash

In the container, navigate to /config/.storage and view the _core.device_registry_ file.

    cd .storage
    cat core.device_registry

The registry is formatted in JSON, but a little searching will turn up the obvious sections that map IPs to MACs (snippet below).

  "configuration_url": "http://192.168.1.222",
  "connections": [
    [
      "mac",
      "d4:d4:da:01:02:03"
    ]
  ]

## Resetting IPs
To fix the situation, I did what I should have done the first time.  I locked down the IPs.  One way would be to set a static IP, but I want to manage this in DHCP.  

I'm using PFSense for DHCP, so after logging into PFSense I navigated to _Services>DHCP Server_.  At the bottom of the page, under "DHCP Static Mappings" I clicked the button and filled in the IP and MAC on the next page.  Saving the mapping is the obvious last step.
![PFSense DHCP](/240519_pfsensedhcp.png)
![Shelly Page](/240519_shellypage.png#floatsmallright)
Setting a DHCP record doesn't change the device. To get it to move to the new IP, you'll need to restart it.  One way would be to unplug the device (probably the easy way).  Another way - my method - was to go to Status > DHCP Leases in PFSense and query the MAC to get it's current IP.  Shelly devices have a web interface, so navigating to the device provides a reboot button under settings.  When the device reboots, the integration in Home Assistant will be working!

![PFSense DHCP Leases](/20240519_pfsensedhcpstatus.png)