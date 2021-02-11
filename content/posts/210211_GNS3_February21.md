---
title: "GNS3 Update for February, 2021"
date: 2021-02-11T07:03:01-05:00
draft: false
author: "Brent Stewart"
Victor_Hugo: "true"
Focus_Keyword: "GNS3"
picture: "gns3"
github: ""
youtube: ""
refs: ["https://github.com/GNS3/", "https://vyos.net/", "http://openwrt.org","http://puppylinux.com/" ]
tags: ["GNS3","linux"]
---

The GNS3 project continues to see regular updates, even though we haven't see a release since December.  I check in on the project periodically and I'm going to make updating the status a regular feature of the blog.

## State of 2.2.17
The current release has been stable in my personal testing.  I use if for all kinds of things, including VMs, containers, and networking out to my physical topology.  A review of the issues reported in GitHub shows 15 bugs reported, with several related to Big Sur.  If you have a new M1 Mac, you might want to run the GNS3 VM and use the Web UI.  The M1 is new and I'm hearing reports of software being ported every week, so give this a little time. 

## New and updated appliances

In the appliance space, we've seen steady activity.

* Cisco IOS-V was updated to include 15.8.3
* EXOS was updated to 31.1
* OpenWrt was updated to 19.07.6
* Ubuntu Cloud was updated to 20.04
* Vyos was updated to support 1.3


![Puppy Linux](/fossadusk.jpg#floatsmallright)

### Puppy Linux
Puppy Linux was added.  Puppy is a little weird - it's not really a distribution, but a collection of installers from other distros that have been customized by the Puppy system ("Woof-CE") and that adhere to a philosophical consistency.  The GNS3 installer includes builds around three versions of Ubuntu - the latest version is "Focal".  Puppy Linux builds are generally known to be small but have the tools you need built in, and to run well on older hardware.  This makes it a good fit for GNS3 where we want to have VMs in the network to use as clients or servers and where we are sensitive to overhead (especially when topologies get complex).

## OpenMediaVault
OpenMediaVault is a NAS server that supports SSH, S/FTP, CIFS, and other types of file access.  NAS services are an important part of corporate network environments and this provides a great opportunity to explore those services.  Corporate data centers tend to focus on Exablock, Equallogic, or NetApp (or similar solutions from VMWare or Cisco), but those aren't represented in GNS3.  Build your own NAS has focused on FreeNAS for a long time.  If you are just wanting a stand-in to test with, I'd recommend using FreeNAS.  However, OMV is a newer option and it's nice to have a choice. One of the big differences is that FreeNAS is based on FreeBSD and OMV is based on Linux, which may make it more accessible for some users.  The current version of the appliance in GNS3 is 5.5.11.

