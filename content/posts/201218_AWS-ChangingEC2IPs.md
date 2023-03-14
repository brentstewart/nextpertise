---
title: "AWS Changing EC2 IPs or Water Never Lies"
date: 2020-12-18T20:35:31-05:00
draft: false
author: "Brent Stewart"
Victor_Hugo: "true"
Focus_Keyword: "AWS EC2"
picture: "aws"
github: ""
youtube: ""
refs: [""]
tags: ["Cloud"]
---
I had an issue that required changing the IP address of an EC2 instance.  __The short version: you can't change the primary IP of an EC2 instance.__

## Water never lies ##

It seems intuitive to me that you can change an IP of a VM, so when asked I said "I think so".  Turns out I answered from ignorance, but then I did a smart thing and actually tested the process to understand it better.

My Dad was a builder.  One time when I was young he was questioned about grading, so he took me to the site and observed it in the rain.  Pointing out that the water ran away from the building, he said, "Water never lies".  It seems obvious, but the older I get the more I find it profound.

People _think_ a lot of things.  What someone _thinks_ will happen isn't nearly as interesting as what actually happens.  It's important to test our intuition against experience and continually validate and update our expectations.

## Back to the story ##

To test IP address mobility in EC2, I created two t2.micro instances running Amazon Linux 2 (which we'll call "A" and "B" for convenience). After choosing the AMI and instance type you are prompted to "Configure Instance".  In this screen, after selecting the subnet the Network Interface details appear at the bottom of the page.  You can assign a valid unused IP - if left blank an IP will be assigned for you.  I allowed both instances to auto-assign an IP and they were assigned to the "primary" (eth0) interface.  
![AWS IP configuration](/AWS_Conf_IP.png#floatcenter)

The first thing I tried to do was to change the IP at the prompt.
```bash
sudo ifconfig eth0 192.168.255.5 netmask 255.255.255.0  
```

That would work on a physical instance, but this left the instance unreachable.  After rebooting, I tried changing the IP address from the AWS console and then I tried to remove the interface.  Neither action was allowed.

![AWS Secondary IP](/AWS_Sec_IP.png#floatright)
Next I assigned a secondary IP.  To do this, go to EC2 and select the instance and then select the network interface.  Under the network interface, go to the Actions button in the top right corner and select "Manage IP addresses.  In the ensuing screen, expand the "eth0" selection and you'll see a button for "assign new IP address".  When you add another address, AWS will limit you to only valid and available addresses on the subnet.  If the IP is used by another instance - whether active or not - you will _not_ be able to assign it.

I tried removing the secondary IP and didn't have a problem.  I was able to take the secondary IP assigned to "A", unassign it, and put it on "B".  This works, but on AL2 you'll need to restart the network service before the secondary IP will be "seen".

```bash
sudo systemctl restart networking
```

### Elastic Network Interfaces
![Elastic Network Interface](/AWS_ENI_IP.png#floatright)
I also played with ENIs.  Originally, my idea was to create a new network interface, add it to the VM, and remove the old one in order to move the IP.  Again, you can't change or delete the primary interfaces of an EC2 instance once created.

However, you can create a stand-alone ENI and associate an IP with it.  _This_ can be attached to "A", unattached, and attached to "B" without a networking restart.  In practice, it works very similar to a secondary IP.

In the Instances page, look in the menu on the left under networking and find "Network Interfaces".  From here you can click the "create network interface" button to create an independent NIC and assign it an IP.

Interface IPs cannot be reassigned without deleting the instance.  Yes, you can take a snapshot before deleting, but your tolerance for that kind of risk may be limited.

The best option is to construct your VPC environment so that all references are done via Fully Qualified Domain Name (FQDN).  DNS can easily be updated to point the name "server" from 10.0.0.1 to 10.0.0.2.

There are places where we are forced to refernce by IP.  In such cases, I recommend using an ENI.  Disable eth0 if having two IPs bother you.  Upgrading or replacing a VM can be as easy as standing up the new version and transferring the ENI.

I've found several references on the Internet to the fact that you can't move EC2 IPs, but not a more detailed walk through of what you _can_ do.  I hope this discussion has been helpful!

