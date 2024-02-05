---
title: "VMWare PSA update"
date: 2022-07-27T13:00:36-04:00
draft: false
author: "Brent Stewart"
Victor_Hugo: "true"
Focus_Keyword: "VMWare"
picture: "virtualization"
github: ""
youtube: ""
refs: ["https://github.com/mkubecek/vmware-host-modules","https://github.com/LinuxEuphony/vmware-host-modules-builder-cli"]
tags: ["virtualization"]
---
I wrote back on [January 5th](/posts/220105_psa_vmware) about an issue with VMWare Workstation on the latest Linux kernels.  I'm using Pop! OS and it is fairly aggressive about keeping the system on fairly recent kernels.  VMWare doesn't support the newest kernels and Workstaion thus can't recompile vmmon and vmnet after a kernel upgrade.  This is probably a problem shared by anyone who keeps their kernel updated, but it's worth saying that - if your goal is stability - you don't have to upgrade the kernel when offered.  

I previously recommended a project from Michal Kubeček that maintained the [necessary patches](https://github.com/mkubecek/vmware-host-modules).  That project is still great, but you have to periodically download a new copy to stay current.  Today when I hit this issue again, I used a different project - the [VMWare host modules Builder CLI](https://github.com/LinuxEuphony/vmware-host-modules-builder-cli).  This project builds on and automates the Kubeček project.

__Builder CLI__ supports Debian (and thus Pop!), Arch, and is working on Fedora support.  When run, the script confirms that you are logged in as root and that there is internet connectivity.  It checks for unmet dependencies and cleans up.  As part of the script it installs ncat and wget, as well as open-vm-tools.  It detects your kernel and VMWare Workstation versions, downloads the updates, and guides you through installation.  It was quick and easy on my system.

To run the VMWare Host Modules Builders script, download and unzip the script, then make the script executable.  For Debian, the script is called _debian-vmware-host-modules-builder-cli.sh_.  Thus run the script as root (or sudo).

![Script prompt](/220727_vmwarescript.png)

Method 1 worked fine for me, although I had to reboot to get everthing working.  That's consistent with the behavior from the older method as well.  It's a little disruptive, since downloading a new kernel can cause VMWare Workstation to have errors until you reboot.  Once you reboot into the new kernel, you have to check vmware, and then run the script if needed and then reboot again with the patches loaded.  Simple enough to just only do updates after business hours and make sure everything is working before you walk away.   

