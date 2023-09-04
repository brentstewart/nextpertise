---
title: "Trayscale"
description: "Trayscale is an top-bar indicator for Tailscale"
author: "Brent Stewart"
date: "2023-03-18T15:46:32-04:00"
markup: 'mmark'
math: false
draft: false
Victor_Hugo: "true"
picture: "vpn"
Focus_Keyword: "tailscale trayscale"
youtube: ""
github: ""
refs: ["https://flathub.org/apps/details/dev.deedles.Trayscale", "https://github.com/DeedleFake/trayscale"]
tags:
  - "VPN"
---
I've covered a few items in recent months around my home network, including using [Tailscale](/posts/221004_tailscale) as a VPN overlay and setting up a home [DNS](/posts/230226_home) server.  This entry is an update on living with those elements.

As a point of comparison, I've used Zerotier in the recent past and really liked it.  With Zerotier, I had a dedicated Ubuntu server VM to route the local network onto the Zerotier VPN.  That worked beautifully.  Tailscale has been a little uneven, but it's becoming more comfortable.

![Tailscale DNS picture](/230318_tailscaledns.png#floatsmallright)
## Getting DNS straightened out
I've had issues accessing home devices by name.  I really can't believe I missed the setting, but logging into the tailscale admin portal showed that I didn't have a DNS setting.  Going to the DNS tab and poiting the Global nameserver setting to my local pi-hole fixed this issue nicely and now I'm able to ping into the home.arpa domain I use in the house.
![Trayscale](https://user-images.githubusercontent.com/326750/188052311-2267af08-82a1-422f-b6ad-bc2cd4de0ac5.png#floatsmallleft)
## Trayscale
Tailscale is typically invoked from the command line.  The command to turn on my desktop at home and to have it bridge the tailscale network into the home network looks like this.

    sudo tailscale up --advertise-routes=192.168.0.0/24 --advertise-exit-node --accept-routes=true 

Trayscale is an unofficial graphical client for Tailscale available from Flathub.  It is still described as "alpha", but aims to provide an easy way to invoke the VPN, change settings, and find other device on the tailnet.  It can be installed from the command line using the following command.

    flatpak install flathub dev.deedles.Trayscale

I've tested it on Pop! and Nix-OS.  In my experience it's reliable and much easier than the command line.  Plus, understanding the local node and getting a list of devices is well presented.  I like that it's available through Flathub, because that will make it easy to ensure that it's kept up to date.  Given the description, I assume that it's still at a stage where it's undergoing rapid change and that's born out by the release list shown on Github.  As of 3/18/22, Flathub gives me v0.8.1 which was released two days ago.  Github shows a v0.8.2 released today.

Cute little utility, and it makes it easier to support your tailscale network, so check it out!