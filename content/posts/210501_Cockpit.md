---
title: "Cockpit"
date: 2021-05-01T14:44:32-04:00
draft: false
author: "Brent Stewart"
Victor_Hugo: "true"
Focus_Keyword: "Linux"
picture: "linux"
github: ""
youtube: ""
refs: ["https://cockpit-project.org/"]
tags: ["linux"]
---

If "web-based graphical interface for administering Linux" makes you think _Webmin_,  then you  need to look at _Cockpit_.  Cockpit is a modern web-based management tool for all your Linux servers.  It's similar to webmin, but slicker.

![Login](/210506_Cockpit_Login.png#floatsmallleft)

Installation in Ubuntu is a breeze.  I installed this on all my linux machines and can manage all of the systems from one dashboard (again, Cockpit has to be installed on them all).  __Cockpit__ is the base install, with other packages to add functionality for storage, networking, pack manager, virtual machines, and containers.  After installation, browse to https://linux_ip:9090 to find the login screen for Cockpit.  Login using the same credentials you use to login to ssh.

    sudo apt install cockpit cockpit-storaged cockpit-networkmanager 
    cockpit-packagekit cockpit-machines cockpit-podman

![Dashboard](/210506_Cockpit_Dashboard.png#floatsmallright)

Once logged in, the Dashboard is the information hub.  By default it shows processor, memory, network, and storage graphs.  From this screen you can also add additional servers to this instance of cockpit (the + button).  I have setup several servers on cockpit on my server, and the graph shows all of them.

Click on the server name at the bottom of the Dashboard (or click the Host button on the side) to zoon in to instance specific information.  From here you can get hardware details or drill down into different areas.
* Logs makes it easy to look through all the logs on the system.  For instance, I can query all the logs from the current session for "Alert and above" messages.  I can even match to a text pattern!  This is one of the easiest ways to quickly comb through Linux logs.
* Storage lets you dig into the details of the attached drives, including NFS mounts
* Networking shows utilizaiton graphs and critical parameters, such as IP
* Accounts shows all the accounts on the box.  It also allows you to add, change or delete accounts.  Password reset?  It's a snap through here.
* Services shows all the running services, like Windows task manager.  You can rest stop or start services from here as well.

There is a software updates tab that makes it easy to keep everything up-to-date.  Click the update button and watch the progress bar.

![Terminal](/210510_Cockpit_Terminal.png#floatleft)
Finally, there's built-in access to the console from the web interface.  If you are managing a server and there's something that can't be done from the GUI, just click the terminal tab and do it from the command line.

If you have a group of devices, this is a great way to administer them from one console.  The log searching in particular is one of the best implementations I've seen.  More than that, you can quickly access the things you need in a "headless" environment such as adding accounts, confirming administrative details, or updating software.

I'll talk about automation in the future but there's a need for an interactive way to manage servers and this compliments ansible nicely.