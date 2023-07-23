---
title: "GNS3 Attached to ESXi"
date: 2021-04-21T20:11:14-04:00
draft: false
author: "Brent Stewart"
Victor_Hugo: "true"
Focus_Keyword: "GNS3 ESXi"
picture: "gns3"
github: ""
youtube: "https://github.com/brentstewart/gns3labs"
refs: ["/210417_Connecting_GNS3"]
tags: ["GNS3","Virtualization"]
---

![VLAN Setup](/210421_GNS3-30.png#floatsmallleft)

I got a little distracted.  I have been coming up to speed on Ansible, which got me started on Vagrant.  Vagrant got me building VMs in VMWare Workstation , which got me thinking how neat it would be to place those _automagically_ into my GNS3VM environment hosted on ESXi.  Didn't get that far - yet  - but the progress I made is pretty cool in it's own right.

Most GNS3 users are using a GNS3 VM to host their topologies.  Mine sits on an ESXi server.  I discussed a few days ago how to connect GNS3 into a network (see [Connecting GNS3](/posts/210417_Connecting_GNS3)). Here I want to do something more complex - I'd like to connect ESXi instances into arbitrary points in a GNS3 network.  The topology will still have a connection "out" to the home network and Internet, but I want to add an ESXi VM "inside" the network as well.

The approach I used was to attach the VMs into an ESXi VSwitch VLAN and then use additional cloud appliances to attach those VLAN into the GNS3 topology.  _This seems obvious in retrospect_.

## Setup on ESXi
![Alt](/210422_GNS3Shell.png#floatsmallright)
The first step is to go onto the VMWare ESXi server and create a new VLAN on the vSwitch.  From the ESXi management interface, select the networking tab and "add port group".  I created VLAN 30 and called it "GNS3-30" and assigned it to my default virtual switch (vSwitch0).



## Setup the GNS3VM

Next I went to the GNS3vm VMWare properties and added an interface.  The interface will attach to the VM "live", but you'll need to go into the GNS3vm to configure it before it can be used.

To setup the interface, login and choose "Shell" from the main menu.  The interface needs to be added to __netplan__.  I ended up adding two interfaces (more fun!) and also took the chance to set a static IP for my server.

```bash
cd /etc/netplan  
ls  
sudo nano 90_gns3vm_static_netcfg.yaml  
```

Here's the edited YAML file I'm using.

```yaml
network:
  version: 2  
  renderer: networkd  
  ethernets:  
    eth0:  
      dhcp4: no  
      addresses: [192.168.25.52/24]  
      gateway4: 192.168.25.1  
      nameservers:  
        addresses: [8.8.8.8,8.8.4.4]  
    eth1:  
      dhcp4: no  
    eth2:  
      dhcp4: no  
```

When I did this step, it replaced the existing eth0 on the GNS3VM and made my old interface eth1.  This disconnected the VM because the IP information was associated with eth0.  I diagnosed this by using the VMWare interface and the _ifconfig_ command on the GNS3VM to identify and associate names and MAC addresses, but it took a little time to understand what happened.  I'm still not sure why, but be alert for this issue if you add an interface.  My Internet GNS3 cloud appliance had to be disconnected (you cannot add interfaces to a cloud with existing connections), eth1 added, and reconnected to get it to work.

## Adding to GNS3 Topology
Recall from [Connecting GNS3](/210417_connecting_gns3/) that I've setup my home network to expect a GNS3 border router at 192.168.25.82 and it will be the route to 192.168.28.0/22.  In this simplest case, I'm attaching another interface on that same router to a different vSwitch VLAN and routing between them.  I _could_ have put the new VLAN deep in the GNS3 topology.

![Setup in GNS3](/210422_AddingACloud.png#center)

So pause before this next paragraph.  Recall that there are three contexts, one physical, one in terms of the ESXi vSwitch, and one inside GNS3.

Attaching a new cloud (in GNS3) that uses the GNS3VM interface (in the vSwitch context) attached to the new VLAN (in my case, eth0 -> VLAN30) will bring that new network into the virtual lab.

At this point I attached a Windows VM to the new VLAN and set it's interface to DHCP.  I connected the cloud to the virtual router (in GNS3), setup the interface, and added DHCP server capability.

```plaintext
int g0/2  
  ip add 192.168.30.1 255.255.255.224  
ip dhcp pool GNS3  
  network 192.168.30.0 /27  
```

I can verify that this works inside the Windows VM, and by verifying that an IP has been assigned from the router.

```plaintext
Router1# __sh ip dhcp bindings__  
Bindings from all pools not associated with VRF:  
IP address   Client-ID/        Lease expiration    Type
             Hardware address/  
             User name  
192.168.30.2 0100.0c29.e965.fd Apr 30 2021 12:05 AM  Automatic
```

## What's next?
I'd really like to be able to __Vagrant up__ straight into GNS3.  I'm not even sure why, except that it would be cool.  Right now I can build a VM on Workstation, transfer it to ESXi and place it in the VLAN and thus in the GNS3 topology.

I can easily extend the vSwitch VLAN to my home network, but for this to really work I'll need to implement a trunk to my desktop and be able to place Workstation VMs into the VLAN.  

One of the things I love about GNS3 is that it pushes me to understand things and learn new techniques.  I'll work on that as I have time in the days ahead.