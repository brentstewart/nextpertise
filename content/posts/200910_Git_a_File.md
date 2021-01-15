---
title: "Git (and run) a File!"
date: 2020-09-10T11:03:09-04:00
draft: false
author: "Brent Stewart"
picture: "shell"
github: ""
youtube: ""
refs: ["https://docs.github.com/en/rest/reference/repos#get-repository-content", "https://docs.github.com/en/github/authenticating-to-github/creating-a-personal-access-token"]
tags: ["Git","Linux"]
---
![GitHub Repository Settings](/githubpriv1.png#floatright)

I maintain a private Github repository for my Linux install scripts.  My install scripts setup PPAs, install programs that I typically use, and setup my system the way I want it.  The repository has scripts for Ubuntu and Red Hat variants, plus secondary scripts that perform other admin tasks.

These scripts aren't supposed to have personal information in them, but things like IP addresses, paths, and security measures could sneak in.  I don't want to have to worry about revealing something that opens me to attack, so the repos are private. 
![GitHub Repository Settings](/githubpriv2.png#floatright)
## Creating a GitHub Private Repository
Github allows free accounts to host private repositories.  Initialize a repo on Github.  To make it private, go under _Settings_ and choose _options_.  Scroll down to the bottom to the section labeled "Danger Zone" and helpfully outlined in red.  The first option is to _Change repository visibility_.  Click here will give you the option to move this repository to private.

Private repositories operate just like public ones in my experience.  The only difference is that access is limited.

## Using a Linux install script 
Here's a snippet of the Ubuntu script to give you an idea of what I'm doing.  This section sets up fail2ban (an SSH security measure) and installs VSCodium.  

> __echo setup fail2ban__  
__systemctl start fail2ban__  
__systemctl enable fail2ban__  
__echo "/[sshd]__  
__enabled = true__  
__port = 22__  
__filter = sshd__  
__logpath = /var/log/auth.log__  
__maxretry = 3" >  /etc/fail2ban/jail.local__  
__##VSCodium__  
__wget -qO - https://gitlab.com/paulcarroty/vscodium-deb-rpm-repo/raw/master/pub.gpg | sudo apt-key add -__   
__echo 'deb https://gitlab.com/paulcarroty/vscodium-deb-rpm-repo/raw/repos/debs/ vscodium main' | sudo tee --append /etc/apt/sources.list.d/vscodium.list__  
__sudo apt update && sudo apt install codium__  

I build and tear down a lot of Linux machines (mostly VMs and EC2 instances).  The initial install lacks some of the tools I expect, and I don't want to go through a process to build the environment.  The script automates this setup, saves me time and makes sure that I don't forget anything!

Up until recently my process was to install Linux, grab git, then clone the repository.  From there, I could move into the repository and run the scripts I wanted.

> __sudo apt install git__  
__mkdir git/linuxinstall__  
__cd git/linuxinstall__  
__git clone__ https://github.com/brentstewart/Private_linuxinstall.git  
__chmod +x myscript.sh__  
__./myscript.sh__  

This works, but it's a bunch to type.  It also downloads more than the one file I need and leaves a repository on the drive.  None of these are problems, but Linux is for the lazy and it feels like there's a better way to do this.

## An easier way
I don't want a multi-step process and I don't want to download my whole repository.  The method I've developed to achieve this is to do all this in one line.
![Github Settings](/githubsettings.png#floatright)
> __curl -s --header 'Authorization: token aaabbbcccdddeeefff1112223334445556667778' --header 'Accept: application/vnd.github.v3.raw' --remote-name --location https://api.github.com/repos/brentstewart/Private_linuxinstall/contents/hw.sh && chmod +x hw.sh && bash hw.sh && rm hw.sh__  

That a lot, so let's break down each piece.
> __curl -s --header 'Authorization: token aaabbbcccdddeeefff1112223334445556667778' --header 'Accept: application/vnd.github.v3.raw' --remote-name --location__ https://api.github.com/repos/brentstewart/Private_linuxinstall/contents/hw.sh  

__Curl__ is used to transfer data via various protocols, including https.

__-s__ puts __curl__ in silent mode

![Github Token](/githubpat.png#floatright)
__- - header 'Authorization: token X'__ is used to authenticate to Github.  You'll need to create a token for your account, so choose settings under your account (upper right).  Choose _Developer Options_ and _Personal Access Tokens_.  Create a new token and copy it to your script.

__--header 'Accept: application/vnd.github.v3.raw' --remote-name__  are options that are documented by github as required.  Don't change these parts.

__- - location__ https://api.github.com/repos/brentstewart/Private_linuxinstall/contents/hw.sh is the part that took an _embarrassingly_ long time to get right.  I read a lot of blog posts and api docs and tried a lot of things that didn't work.  I gather that the working examples I found online were from a previous iteration of github and the methods have been deprecated.  This, however, works!  The parts you'll need to change here are your Github username and the name of repository.  _Contents_ stays on the path unchanged, but you'll need to add in the file you are looking for on the end.

I didn't want to run my install script over and over, so the referenced file just has __echo Hello World__ in it (thus hw.sh).

__&&__ is used to join multiple commands on a single line.

__chmod +x hw.sh__ makes the file that was just downloaded executable

__bash hw.sh__ runs the script

__rm hw.sh__ deletes the file so that the new environment is clean

## Conclusion
This approach has some interesting benefits.  First, the slug of a command is the kind of thing that can be cut and paste into new machines.

Second, this approach is open to automation because it removes the login to Github (replaced by the token) and allows the process to complete unsupervised.  As such, this would be the kind of thing that could be used to start up EC2 instances.  In that case, you could easily automate a standard EC2 startup to pull a script from Github.  Then you could maintain the current script and improve it over time without having to go into Cloudformation or EC2 to change processes.

A bonus conclusion as well - the result didn't end up looking like I expected.  I spent a lot of time trying to pipe commands together, and never developed that approach to a usable point.  It's important to have a good understanding of different approaches to a problem so that you can deal with the "expected unexpected" and keep moving.

I hope this discussion helps you!  If you have other approaches to this problem, I'd love to hear about them in the comments below.