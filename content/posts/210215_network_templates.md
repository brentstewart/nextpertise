---
title: "New Project - Testing *Every* GNS3 Network Appliance"
date: 2021-02-27T17:46:15-05:00
draft: false
author: "Brent Stewart"
Victor_Hugo: "true"
Focus_Keyword: "GNS3"
picture: "gns3"
github: "https://github.com/brentstewart/gns3labs"
youtube: ""
refs: [""]
tags: ["GNS3"]
---

I'm kicking off an exciting new project, and I'd appreciate your thoughts as I get started.  GNS3 supports a many different appliances and I'm fascinated by how they work and compare.  I've supported many of them in my work, but I'd like to build a lab for each one that demonstrated the core functions of the device. My hope would be to produce a reference that would allow folks to quickly gain traction with new equipment.  Part of the goal is to directly compare the setup between devices, so the network structure would be static.

I'm proposing three basic topologies to test switching, routing, and firewalling.  There will obviously be some limitations to my approach.  That list leaves off some appliances categories: endpoints, security tools, NAS, and load balancers come to mind.  If this is successful, I'll come back and explore those in their own environments.  I'm also not going to try to teach all the protocols and concepts as I go - this is two fold in that I have to scope this project to a reasonable size and also that 

## Switching
![Switching Topology](/Topo-Switching.svg#floatsmallright)

The goals of the switching lab would be to cover the following key switching capabilities:
* Spanning-tree and Rapid Spanning Tree
* VLANs
* Trunk
* Port channel
* Intervlan routing (possibly with a router on a stick)
* Port security
* Management - Telnet, SSH, HTTP, Syslog, TFTP
* Authentication - static or RADIUS/TACACS
The topology features three switches, but with the redundant connection provides some interesting cases for STP.  Spanning Tree, possibly alone on this list, is a requirement for switches.  It works so well that a lot of younger engineers don't even realize the problems that it prevents!  The redundant connections can later be used for a port-channel as well.  There's an external router (if the switch being considered is layer 2 only), plus three target devices to work through some VLAN examples.  There are some advanced cases that this doesn't cover, mostly because I want to make these labs accessible for those who don't have tons of memory.  Do you see anything I'm missing?

## Routing
![Routing Topology](/Topo-Routing.svg#floatsmallright)
The goals of the routing lab would cover the following capabilities: 
* IPv4 and v6 support
* Intervlan and WAN Routing
* Static Routing
* Dynamic routing - RIP, OSPF, EIGRP, BGP
* Access-lists
* Site-to-site VPN
* Management - Telnet, SSH, HTTP, Syslog, TFTP
* Authentication - static or RADIUS/TACACS

Three routers covers most simple cases.  It's important to point out that I'm not attempting to model _good designs_.  I'm trying to create a lab that exercises the most definitive features of each class of devices while using the least amount of memory and processor so that these labs are accessible.  One interesting note here - I don't have a lot of experience with IPv6.  But the reason for the challenge is to push ourselves.  There's enough complication here that we can implement a relatively static design with a lot of different tools to see how they differ. 
These labs will be fun to demonstrate and exercise the different routing protocols.  Of course it would be great to have a bigger lab to do things like iBGP and eBGP, but the goal here is to compare the different appliances.  

## Firewall
![Firewall Topology](/Topo-Firewall.svg#floatsmallright)
Firewalls are pretty straightforward to test.  The biggest question is whether we want to go full red-team or just demonstrate configuration options.  I chose the latter to be consistent at this stage, but I may come back to them when I'm ready to include Kali and Parrot.  Functions to be demonstrated include:
* Routing
* Bump on a wire
* Routing functions
* S2S VPN
* client VPN
* ACL / Policy
* Management - Telnet, SSH, HTTP, Syslog, TFTP
* Authentication - static, RADIUS or TACACS

I've created a repository to house all the lab files and each setup will be a separate directory with instructions.
