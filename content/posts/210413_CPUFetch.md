---
title: "CPUFetch"
date: 2021-04-13T17:50:03-04:00
draft: false
author: "Brent Stewart"
Victor_Hugo: "true"
Focus_Keyword: "Linux"
picture: "shell"
github: "https://github.com/Dr-Noob/cpufetch"
youtube: ""
refs: ["https://www.man7.org/linux/man-pages/man1/lscpu.1.html"]
tags: ["Linux"]
---
![LSCPU Output](/210413_lscpu.png#floatright)
I don't have to check CPU statistics very often, but occassionally I need to remember details like how many cores I have.  The traditional way to get CPU information in Linux is to use __lscpu__.  Here's the top of the output on my desktop.  I've truncated the picture - the output is a full page or two of details about your CPU and it's capabilities.  You can also __cat /proc/cpu__, which has similar info, or list hardware with __lshw__.  __lshw__ provides a _lot_ of output, so you can filter that down with __lshw -class CPU__.  All of these options work, but they vary from cluttered to hard-to-read.


![CPUFetch](/210413_cpufetch.png#floatleft)
I came across a fun utility to do the same thing, but prettier.  CPUfetch doesn't display the same level of detail, but it pulls the most interesting pieces.  It's actually a little clearer and easier to read because it doesn't have as much detail.  With the pretty logo to the left I assume the name is a nod to Neofetch, a utility I build in to my __.bashrc__ to show on startup and use all the time.

This doesn't solve a lot of problems, but it is kinda cool.  If you agree, check it out on Github.  To install, clone the repository and __make__ the executable.  I put all my repos in a single directory to organize them, a practice I suggest.

> cd ~/git  
git clone https://github.com/Dr-Noob/cpufetch  
cd cpufetch  
make  
./cpufetch  

It will also compile on Android, Windows and MacOS, if you're into that kinda thing.  The readme at github has some other sample pictures and some ways to modify the output.  Have fun!

