---
title: "ZeroTier Router"
date: 2020-10-27T22:10:59-04:00
draft: false
author: "Brent Stewart"
Victor_Hugo: "true"
Focus_Keyword: "zerotier"
github: ""
youtube: ""
refs: ["https://zerotier.com"]
tags: ["ZeroTier","VPN","GNS3"]
---
This article continues the exploration of ZeroTier started in a previous [posting](/zerotier).  The first article described zerotier - an overlay virtual wire that hangs on the internet to connect disparate clients into a psuedo local network.  At the end of the discussion, we had a PC at home and a 4G mobile phone talking over Zerotier.

I'll continue the thought describing how to connect your local home network to your Zerotier virtual network.  For purposes of this article, let's consider a home network with a little complexity.
![Sample Network](/ZeroTier_Routing.png#center)

In this example, there is a base network of 192.168.100.0/24.  The 101 network is routed through the firewall and is used for IoT devices, while 102 is used for wireless.  104/22 has a next-hop in GNS3, so that a network can be establish and ennumberated using the network simulator and still route out to the "real" world.

We want to create a router that has one interface in the local 192.168.100.0/24 network and a virtual interface in the virtual Zerotier 103.0/24 network, able to route between them.  To do this, I built a new Linux server (an Ubuntu 20.10 VM, but any distro physical or virtual should work).  I named the router "ZTRouter".

A quick note - use care when routing 192.168.0.0/24 and 192.168.1.0/24.  A lot of home routers use these ranges and adding a ZeroTier route to the same destination might lead to confusion.  Select a space out of 10/8, 172.16/12, or 192.168/16 that won't conflict with other routes you need to support.

## ZeroTier Routing

I assigned ZTRouter 192.168.100.2/24 with a default route to the local router at 192.168.100.1.  Next I installed Zerotier and attached it to the SDN built in the last article.

> curl -s https://install.zerotier.com | sudo bash  
> sudo zerotier-cli join 0123456789ABCDEF
> sudo zerotier-cli listnetworks

![Zero Tier Routing Configuration](/ZTrouting.png#floatright)The router will be automatically assigned an address on the ZeroTier network - in this case I received 192.168.103.88.  __listnetworks__ is used to confirm the connection.

The routing that will need to be setup might not be obvious, so let's walk through each route.

* 192.168.103.0/24 is the Zerotier network.  A range from this network should be used in "auto-assign pools" in ZeroTier Central, such as 192.168.103.1 - 192.168.103.50.
* 192.168.100.0/22 is the summary route to the local network and it points to ZTRouter.  This tells the other ZeroTier clients that this range is available through ZTRouter.
* 192.168.104.0/22 is another summary route, this time for GNS3.  Again, this communicates the availability of the range to the ZeroTier network via ZTRouter.

## Routing between ZeroTier and the LAN

Next, ZTRouter needs to be enabled as a router.  Edit /etc/sysctl.conf and uncomment the line that says __net.ipv4.forward__.  This will enable the Linux machine to route when it reboots.  Since we want it to work _now_, well use this command as well:
> sudo sysctl -w net.ipv4.ip_forward=1

The local firewall also has to permit the traffic.  Depending on the distro, you may have nftables, iptables, or ufw.  Assuming the system uses iptables, start by getting the interface names.

> __ip link__

For purposes of the article, let's assume it shows you that the ethernet is enp1s0 and ZeroTier is zt1
> PHY_IFACE=enp1s0; ZT_IFACE=zt1 
sudo iptables -t nat -A POSTROUTING -o $PHY_IFACE -j MASQUERADE  
sudo iptables -A FORWARD -i $PHY_IFACE -o $ZT_IFACE -m state --state RELATED,ESTABLISHED -j ACCEPT  
sudo iptables -A FORWARD -i $ZT_IFACE -o $PHY_IFACE -j ACCEPT  

Make the iptables changes persistent.
>sudo apt install iptables-persistent
sudo bash -c iptables-save > /etc/iptables/rules.v4

## Reciprocal Routes

If testing is done at this point, it will show that ZT clients can ping the LAN interface of ZTRouter but can't reach other users on the LAN.  What gives?  The problem is that we've built a path from the ZT cloud into our LAN, but not the reciprocal path _back_.  The local users have a default gateway of the Internet router and _it_ doesn't have a route to 192.168.103.0/24.  The easy way to fix that is to give it a route.  Everyone's home router will be different, so in psuedo-code you just need to __route 192.168.103.0/24 via 192.168.100.2__.

Testing will now show that ZeroTier clients can ping devices in the "100" network!  But, they can't reach the other local VLANs.  The problem is that ZTRouter doesn't have a route.  To fix that, add a summary route going to the Internet router.

> ip route add 192.168.100.0/22 gw 192.168.100.1

Add this line to _/etc/rc.local_ so that it is persistent.

Note that this summary includes the ZeroTier network.  The routing table prefers the most specific path, so traffic to 103/24 will continue to route to ZeroTier and everything else will take the less specific route to the inter-vlan router.

At this point, ZeroTier clients will be able to reach all the local subnets (100/24, 101/24, and 102/24).  Traffic to GNS3 can also be pointed to the Internet router, or it can be directed to a GNS3 router.  Note that 100/22 and 104/22 can't be summarized into 100/21 because they fall across a bit-boundary, so they'll have to be configured as two routes.

One place that can cause trouble is route selection.  On ZTRouter, the ZeroTier summary route for 100/22 will be in the routing table.  The static route created _must_ be a lower metric so that it takes precedence.

## Summary
This is a slick setup.  ZeroTier makes a great VPN client, punches through NATs, and can create sophisticated routing.  The two places I expect folks to get hung up are getting the routes correctly configured in ZeroTier Central and making sure there is a reciprocal route back to the ZeroTier VPN range.  If you have problems, work your way out from the router one step at a time and make sure you understand how the routes are working going in _both_ directions (I've taught routing for twenty years and everyone always forgets to check the path back).

Of course, most networks won't be as sophisticated as the one shown here and will be very straightforward to setup.  