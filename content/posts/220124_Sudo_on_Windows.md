---
title: "Sudo for Windows Powershell"
date: 2022-01-23T18:28:41-05:00
draft: false
author: "Brent Stewart"
Victor_Hugo: "true"
Focus_Keyword: "Windows Sudo"
picture: "windows"
github: ""
youtube: ""
refs: ["https://github.com/lukesampson/psutils/blob/master/sudo.ps1","https://scoop.sh/","https://github.com/ScoopInstaller/Scoop"]
tags: ["Windows","Shell"]
---
Some Windows Powershell commands must be run from Powershell running in an administrative context.  It's a little bit of a pain when you need to invoke Powershell this way (right click it in the menu).  The real problem is once there's a terminal up, do you limit this to just the command that requires it or do you just work out of the open (administrative) terminal window?  Choosing to remain in that admin context could lead to trouble.

![Sudo Logo](/sudologo.png#floatsmallleft)
## The need for sudo

Unix has a nice way of handling this.  The command prompt starts with your user priviledges.  It can be escalated for a single command with "sudo" - substitute user do.  Wouldn't it be cool (and more secure) if a similar command existed for Powershell on Windows?

Luke Sampson has a set of powershell scripts that appropximate Linux commands on [Github](https://github.com/lukesampson/psutils/blob/master/sudo.ps1).  These include sudo and are meant to be installed using _scoop_.  

## Using Scoop
Scoop is an installer, like winget or choco, but it's aimed more at simple installs.  Scoop doesn't require a developer to make a special installer - it can use a ZIP and instructions in a JSON.  These JSON files are stored in buckets - basically these are curated compilations of JSON files stored in a Git.

Scoop specializes in simple programs, like command-line tools such as sudo.  In fact, many linux-like tools can be easily installed by scoop such as sudo, git, and curl.  Scoop puts everything in your users directory, so it doesn't cause a lot of UAC pop-ups.

Scoop and sudo can be installed as shown below.  

    # allow powershell scripts
    Set-ExecutionPolicy unrestricted
    # install scoop
    iwr -useb get.scoop.sh |iex
    # install sudo 
    scoop install sudo

You can use __scoop search__ to see if a program is available through scoop.  Check out the github site to see other buckets that are available as well.

## Using Sudo in Powershell for Windows
Once installed, you can escalate priviledges on a command-by-command basis by prefixing them with "sudo", just like you would on Linux.

    sudo set-executionpolicy
