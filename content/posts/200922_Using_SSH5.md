---
title: "Using SSH (Part 5) - Remotely Possible"
date: 2020-09-22T14:47:47-04:00
draft: false
author: "Brent Stewart"
Victor_Hugo: "true"
Focus_keyword: "SSH"
picture: "shell"
github: ""
youtube: ""
refs: [""]
tags: ["SSH", "Linux"]
---

I confess that I never meant for this to be an SSH blog. It was an easy topic to write about when we started, but friends keep suggesting new things I should cover. The takeaway then is that SSH is a heck of a tool and can be used to accomplish a lot of different things.

Today's topic is running a GUI program on a remote computer and displaying the output locally. Pretty cool, right? This is suprisingly easy to do.

Why would we want to do this? I already covered that under "pretty cool", but I suppose I could say something about using your netbook to run a big program that needs lots of RAM and CPU.

To demonstrate, I'm going to run the text editor from my server on my desktop.

```bash
ssh -X brent@192.168.1.1 # -X or -Y work  
pluma
```

![Displaying a remote program](/XRemote.png#center)

Simply SSH into a remote host using either the -X or -Y switch. From the remote prompt, enter the command to run the graphical program of your choosing and the window will be displayed locally. Using "-X" (as shown in the code) allows this to work but keeps the X11 Security extension restrictions. Using "-Y" like I did in the screen capture bypasses those restrictions. From an operative point of view, they act identically.

This has been the most straightforward of the series. If you're interested in SSH, check out:

- [SSH - Part 1 Basics and Banners](/posts/200811_using_ssh1/)
- [SSH - Part 2 Authentication](/posts/200812_using_ssh2/)
- [SSH - Part 3 File Shares](/posts/200813_using_ssh3/)
- [SSH - Part 1 Port Forwarding](/posts/200830_using_ssh4/)
