---
title: "Network_templates"
date: 2021-02-15T17:46:15-05:00
draft: true
author: "Brent Stewart"
Victor_Hugo: "true"
Focus_Keyword: "GNS3"
picture: "gns3"
github: ""
youtube: ""
refs: [""]
tags: [""]
---

I'm kicking off an exciting new project, and I'd appreciate your thoughts as I get started.  GNS3 supports a many different appliances and I'm fascinated by how they work and compare.  I've supported many of them in my work, but I'd like to build a lab for each one that demonstrated the core functions of the device. My hope would be to produce a reference that would allow folks to quickly gain traction with new equipment.  Part of the goal is to directly compare the setup between devices, so the network structure would be static.

I'm proposing three basic topologies to test switching, routing, and firewalling.  There will obviously be some limitations to my approach.  That list leaves off some appliances categories: endpoints, security tools, NAS, and load balancers come to mind.  If this is successful, I'll come back and explore those in their own environments.  I'm also not going to try to teach all the protocols and concepts as I go - this is two fold in that I have to scope this project to a reasonable size and also that 

## Switching
![Switching Topology](/Topo-Switching.svg#floatsmallright)

The goals of the switching lab would be to cover the following key switching capabilities:
* Spanning-tree and Rapid Spanning Tree
* VLAN
* Trunk
Port channel
* Port security
* Router on a stick
* Management - Telnet, SSH, HTTP, Syslog, TFTP
* Authentication - static, RADIUS, TACACS

## Routing
![Routing Topology](/Topo-Routing.svg#floatsmallright)
Router 
* Intervlan and WAN
* static
* dynamic - RIP, OSPF, EIGRP, BGP
* ACLs
* S2S VPN
* Management - Telnet, SSH, HTTP, Syslog, TFTP
* Authentication - static, RADIUS, TACACS

## Firewall

Firewall
* Routing
* Bump on a wire
* Routing functions
* S2S VPN
* client VPN
* ACL / Policy
* Management - Telnet, SSH, HTTP, Syslog, TFTP
* Authentication - static, RADIUS, TACACS