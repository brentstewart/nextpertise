---
title: "VMWare 3D Acceleration"
date: 2020-07-28T15:44:02-04:00
draft: false
Victor_Hugo: "true"
Focus_Keyword: "VMWare"
author: "Brent Stewart"
github: ""
youtube: ""
refs: ["https://github.com/FydeOS/chromium_os-vm-vmware","https://askubuntu.com/questions/832755/no-3d-support-is-available-from-the-host-on-all-vmware-guests"]
tags: ["Virtualization"]
---

I tried to load Chrome OS in VMWare Workstation the other day and got the message "No 3D Support is available".  Apparently this is required for Chrome OS to boot.  I've gotten the message on other VMs, but never needed to worry about it.

I assumed this was an issue with Chrome OS at first and tried several versions including "CloudReady".  I was using the build put together by FydeOS and had a chance to talk with him on Telegram about the issue.  He recommended I look through a question on AskUbuntu.

I edited __.vmware/preferences__ and added the following line.

> mks.gl.allowBlacklistedDrivers = "TRUE"

Chrome OS booted without a problem!  I had VMs on ESXi that I was accessing via Workstation, and this also resolved the issue for them.  If you are interested in running a ChromeOS VM, I suggest checking out FydeOS.  My testing has been smooth and they have a Telegram channel that is super helpful.
