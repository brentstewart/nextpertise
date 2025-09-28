---
title: "Tailscale"
description: "Using Tailscale with Linux"
author: "Brent Stewart"
date: "2022-10-04T14:48:18-04:00"
math: false
draft: false
Victor_Hugo: "true"
picture: "network"
Focus_Keyword: "Tailscale"
youtube: ""
github: ""
refs: ["https://tailscale.com","https://github.com/juanfont/headscale"]
tags: ["Zerotier","VPN","Review","Network"]
---
I'm interested in TailScale.  I've been hearing good things about it from my friend, Jared, and TailScale has a fair set of proponents on my favorite podcasts.  A couple years ago, I setup ZeroTier and built a dedicated Linux device to attach to the ZeroTier network and route into my local LAN.  I wrote a well-received set of articles about that experience ([Zerotier Basic Configuration](/posts/201027_zerotier/) and [ZeroTier Router](/posts/201027_zerotierrouter/)).  ZeroTier continues to work well, but I haven't been traveling as much and have left the VM off lately.  This investigation doesn't come from any frustration with Zerotier or urgent need, just from an interest in trying something new.

## Challenges
Both ZeroTier and TailScale are "overlay networks".  I have a Meraki stack at home with two Internet connections (WISP and Starlink).  Meraki has horrible VPN support and I'm not over-enthused about cutting holes in my firewall.  Plus, anything that requires an ISP failover would kill VPN, so these overlay-style connections fit my needs closely.

Both solutions use NAT traversal techniques and some of the same encryption suite.  Tailscale is an implementation of Wireguard (which is all the rage in Linux circles), but Zerotier predates wireguard and is a custom solution.  I'm not aware of any active security issues with either.  Obviously though, you're only as secure as who you trust.

Both ZeroTier and Tailscale operate in a "freemium" model, where the rendezvous server allows 20 connections.  Larger networks require a subscription, but both have self-hosted rendezvous servers as an option (presumably you'd set these up on something like EC2). I solved this with ZeroTier by configuring an Ubuntu server as a router from the ZT network into my home network.

## Experience with TailScale
The Tailscale experience starts with signing up on the [website](https://tailscale.com).  Instructions are provided for all the supported operating systems - Windows, Mac, Linux, iOS and Android.  Mobile operating systems send you to the respective App Stores to pick up a client.  My Pop! desktop is Ubuntu-based, so I was able to add a PPA and install from there.  TailScale doesn't have a Linux GUI client, it is invoked through the command line as shown below.

    sudo tailscale up 
    tailscale ip -4  #shows private TS IP, can also be seen using "ip a"

Once clients are instantiated, the VPN network is maintained at the [tailscale website](https://tailscale.com).  My machines were given addresses in the 100.64/10 range, but not in the same /24, which is a little different than Zerotier.  Clients _should_ be able to communicate after they are registered and visible on the dashboard.  Tailscale functions as expected - I was able to access internal TailScale-attached resources without having to provision access on the firewall and speeds were comporable to ZeroTier.  

With Zerotier, I had to build a router to access non-attached devices.  TailScale allows any device to be an "exit node" and to route traffic into the local network.  Here I ran into some minor issues.  Tailscale documentation is pretty good, but there are still some mental hurdles to getting this to work correctly.

First, the node has to be setup as an exit node.  To enable this, I re-enabled the tailscale client with the advertise flag.

    sudo tailscale down
    sudo tailscale up --advertise-exit-node 
    
The node will now show in the dashboard as an available exit, but it won't have any routes.  It turns out the node has to explicitly advertise local routes.  In ZT, this is controlled through the dashboard.  To enable this, I re-enabled the tailscale client with the routes.  For the record, I'm not sure that you have to take the service down everytime you make the change.  That might just be years of conditioning coming out on my part.  

    sudo tailscale down
    sudo tailscale up --advertise-routes=192.168.0.0/22 --advertise-exit-node --accept-routes=true  

By way of reference, I have four VLANs locally.  I could use seperate tailscale endpoints to attach into each of them, but I want to advertise them all as a block and thus have the /22 above.

At this point, routing onto the local network from Tailscale will still not work.  There are two issues left to deal with, one obvious and one bug.  Let's deal with the bug first.  When I review the Linux routing table, it does not show the tailscale network.  After beseeching the Great Google, I found references to a known bug in Ubuntu that doesn't add these routes.  Since the computer doesn't have a route in the tailscale network, it can't pass traffic back.

    >route
    Kernel IP routing table
    Destination     Gateway         Genmask http://192.168.26.53/ worked, but not to other devices in the same VLAN or to other VLANs.

    sudo route add -net 100.64.0.0/10 dev tailscale0

The remaining issue is that local devices have my firewall as their default gateway.  When they receive traffic from a tailscale-connected IP, they reply using their default route back to the firewall.  The firewall then uses it's default route to pass the traffic to the public Internet!  To fix this, I went into firewall (for those of you with Meraki, it's on the dashboard under _Security & SD-WAN > Addressing & VLAN_) and added a static route.  The route should target 10.64.0.0/10 and the next hop should be the IP of the tailscale exit node.  With this in place, everything works!

## Nix setup
Setup in Nix involves two steps and also varied slightly for me from the docs.  First, add tailscale to _configuration.nix_.

  environment.systemPackages = with pkgs; [
    . . .
    pkgs.tailscale
  ]
  services.tailscale.enable=true;
  # exit the text editor
  > sudo nixos-rebuild switch

Once tailscale is installed, run __sudo tailscale up__ as before.  This will provide a URL for authentication.  Finally, go into the tailscale dashboard and authorize the new machine (click the ellipsis to the right of the machine and choose authorize).  Nix runs on my travel laptop, so I didn't try to advertise it as an exit node.

![Dashboard](/221005_Tailscale.png#floatright)
## Tailscale Dashboard

My impression of the Tailscale dashboard is mixed.  There's a download link, and a place to add users.  Free accounts cannot have multiple users, so the main user would have to setup the client on each device (like my wifes' or kids' computers).

The documentation is pretty good, but I ran into several questions where it gave insufficient answers and I needed to just experiment to get things working.  The __Machines__ tab shows devices that are currently connected.  This also allows you to set tags and enable routing (assuming that the client is also configured to support routing).  The __Services__ tab collects a list of services so that you are aware of what you are sharing into the TailScale network.  This has the potential to be very useful, but you cannot "click to block" on this screen and it only shows services from the Tailscale-connected machine.  No services were shown from elsewhere on the connected network.  This could lead someone to a misunderstanding about their risk profile.

Other dashboard tabs allow you to setup access-lists and control DNS.  __Access control__ is configured through a JSON document.  The controls available are pretty good - they allow you to block access by user or group (both rendered useless on the free account), by host IP, or by service port.  The JSON ACL can be managed through Github using Github actions which is very exciting, but you'd have to make sure that repo is marked private.  The __DNS__ tab allows you to point Tailscale clients to an internal resolver or to use "MagicDNS".  MagicDNS, as near as I can tell, is basically a shared _hosts_ file, but it's nice for folks who don't have a private name server.

## Conclusions

What does this all boil down to?  I'm attracted to Tailscale because it uses wireguard and because it doesn't require a dedicated router-vm.  Zerotier seems to have better access controls.  In both cases, the free-tier accounts offer analogous features (20 devices, 1 user).  Setup complexity is different, but equal.  If one or the other is working for you already,  I don't think a change is necessary.  I've decided to keep Tailscale in place for at least a little while and I'm very interested in investigating the self-hosted option and seeing what additional capabilities that would provide.  Tailscale also offers a $5/month package of five users that would support a family and less work than spinning up an EC2 instance.