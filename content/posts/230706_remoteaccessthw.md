---
title: "Remote access - The Hard Way"
description: "Remote access via Cheese"
author: "Brent Stewart"
date: "2023-07-06T17:00:43-04:00"
markup: 'mmark'
math: false
draft: false
Victor_Hugo: "true"
picture: "linux"
Focus_Keyword: "HDMI capture"
youtube: ""
github: ""
refs: [""]
tags:
  - "linux"
---
A little bit of fun today.  There are many cases where we need to access different computers but can't setup something like RDP.  I had the idea to try this via video capture, so here's my experience.

## Remote access via video capture

I ordered the Extenuating Threads [HDMI Capture Dongle](https://www.amazon.com/dp/B0C2HG93TG) from Amazon for $14.  There are several options available - I went with Extenuating Threads because it was cheap and I wouldn't be out much if it didn't work.  I needed to capture 4k at least 30Hz and this model promises 4k at 60Hz.

The Dongle arrived and presented itself as a video source (like a camera) once plugged in.  In fact, I could use the second computer video and route it into any application, including Teams.  Identifying the dongle is pretty easy - I just flipped through all the available video sources in OBS or Cheese.  You can also do this from the command line:

    sudo apt-install v4l-utils  #install Video for Linux tools
    v4l2-ctl --list devices

My output looked like this:

    USB Video: USB Video (usb-0000:00:14.0-1):
        /dev/video0
        /dev/video1
        /dev/media0

## Issues
I tested this with a number of video players, including OBS, MPV, mplayer and VLC.  All three worked, but only OBS provided a decent frame rate and audio.  OBS seems like a lot to load, just to see a remote server.  My son suggested _Cheese_ and I was skeptical, but that was actually far and away the most responsive.  I used a Bluetooth mouse and keyboard to control the host and _Cheese_ presented the display at realtime speeds but didn't capture audio.  I didn't have any issues or artifacts with the display, even at 4k.  I ended up dialing the resolution down to 1920x1080 so that it fit into a quarter of my 4k display and this worked perfectly.

I made this a little less temporary by replacing the seperate mouse and keyboard with a USB switch that let me toggle my controls back and forth between my main machine and the captured device.

So my cheap $14 USB dongle works to allow me to access a machine here in my office.  I can treat the dongle as a video source and access it using most programs.  Speed was good, but only Cheese and OBS allowed interactive speed.  All told, a cute little experiment!