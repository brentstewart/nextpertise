---
title: "ZeroTier Basic Configuration"
date: 2020-10-25T16:29:57-04:00
draft: false
author: "Brent Stewart"
Victor_Hugo: "true"
Focus_Keyword: "zerotier"
picture: "zerotier"
github: ""
youtube: ""
refs: ["https://zerotier.com","http://adamierymenko.com/decentralization.html","https://www.zerotier.com/manual/#2_1_3", "https://github.com/zerotier"]
tags: ["Zerotier","VPN","Review"]
---
Zerotier is a virtualized network that runs "on top of" the Internet.

Traditional VPN solutions are built around a VPN server, which acts as a hub location with a stable IP.  Modern teams feature mobile workers and home connections with random IPs behind service-provider NATs.  Start-up teams and home users are left with few options, all of which involve some level of compromise.

Zerotier works around this by offering a stable point to connect end-points.  The connection strategy resembles VoIP connections - there's a registration to a central point, that tries a variety of ways to create a connection to end points.  It then allows the end-points to speak directly.  All traffic between end-points is encrypted peer-to-peer.

Zerotier allows the creation of a "virtual ethernet" that connects disparate endpoints.  I created a ZeroTier network and tested it with Fedora and Ubuntu Linux, as well as an Android phone.  I was able to connect to the ZeroTier network from a guest wifi and over a 4G connection.  Once connected, it behaved like a local network.  I was able to SSH, browse and download files, access a Calibre server, and use KDE Connect.

## Setting up a ZeroTier network
Go to [ZeroTier](https://www.zerotier.com) and scroll down to the bottom to __Sign Up__.  After signing up, you'll be taken to the ZeroTier Central page and be given a 16-digit hex network id and a made up name (like "gratious_donut").

Make sure under Access Control that you set your network to private.  This will not allow new connections without your permission.

Under advanced, choose a network range.  You can use one of the "easy" options or select an IP address range of your own.  For now, just choose a pre-defined range.

![Zero Tier Networking](/ZTnetworks.png#floatright)

There's an option to use IPv6.  The easy way is to click the ZeroTier 6PLANE option.  It's a great idea to be learning about IPv6, but most of us are still using v4 and if that's the case for you then just leave this turned off.

That's it for Central for now.  Copy your network ID (the 16 digit hex number).  We'll need to revisit Central later, but next we need to setup devices.

## Client installation
The instructions  for setting up clients can be found at [ZeroTier Downloads](https://www.zerotier.com/download).  There's a clicky MSI installer for Windows, and a pkg for Mac.  Smartphone users are directed to their stores.

On Linux, the software can be installed with this command:
> curl -s https://install.zerotier.com | sudo bash  

After installation, use __zerotier-cli__ to join the new virtual network.
> sudo zerotier-cli join 123456789ABCDEF  

![ZeroTier Client](/ZTclient.png#floatright)
Go back to Central and scroll down to clients.  Find the new client and check the Auth? box.  You should add a name and description here as well to help identify this client as you add more endpoints.

Back at Linux, confirm that you're on the network.

> sudo zerotier-cli status

Next, let's setup an Android endpoint to have something to talk to.  Grab the app from the Play store.  Click the + in the upper right and type in the network ID.  Slide the ON button over and go back to Central and Authorize the client.

You can continue to add clients in this manner, but I suggest you pause here.  My next article will be about routing between networks with Zerotier and that may be useful before you move further.