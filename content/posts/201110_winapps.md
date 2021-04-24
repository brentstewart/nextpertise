---
title: "Winapps"
date: 2020-11-10T08:24:53-05:00
draft: false
author: "Brent Stewart"
Victor_Hugo: "true"
Focus_Keyword: "Linux"
picture: "linux"
github: "https://github.com/brentstewart/winapps"
youtube: ""
refs: ["https://github.com/Fmstrat/winapps"]
tags: ["Linux", "Windows", "Review"]
---

Winapps is a project that allows running Windows applications as if they were a part of a Linux Desktop. It's a sleight of hand - the apps run in a VM and an RDP window is created for just that application. However, it integrates with the Linux environment and even let's you use "open with" types of functionality.

![WinApps](https://raw.githubusercontent.com/brentstewart/winapps/develop/demo/demo.gif#floatright)

To give it a try, clone the code from [GitHub](https://github.com/Fmstrat/winapps). Remember that it's running programs that are installed on a VM. If you don't have a Windows VM, the project includes an empty KVM machine that Windows and applications can be installed within. You can use an existing Windows VM (or even physical machine) if you have one. Windows doesn't have to be a VM on your local machine - I set this up to run with a copy of Windows I'm running on my ESXi server downstairs. Theoretically, you could use your laptop or a remote computer to be the "Windows source".

Create a text file at \_~/.config/winapps/winapps.conf that looks like this.

```bash
RDP_USER="MyWindowsUser"  
RDP_PASS="MyWindowsPassword"  
#RDP_IP="192.168.123.111"  
#DEBUG="true"
```
The IP is only required if Windows is remote. The Debug command tells it to create a log and is optional.

Finally, run the **install.sh** script. This script will use the variables defined in the config file and login and scan Windows. If it finds a file it knows, it will setup the link, put an icon and entry in the local application menu, and link the appropriate mime-types.

When I tested this, I ran into a couple issues running the install script. The script would timeout. Looking at the script, I saw that it was trying to use xfreerdp to login to Windows. I ran that command from the command line and saw that it wasn't connecting. Troubleshooting on Windows revealed that I needed to enable remote desktop under settings>system>remote desktop. Doh! Re-testing with xfreerdp revealed that I needed to accept a certificate.

With the RDP part confirmed working, I re-ran the script and _voila!_.

A lot of folks will be interested in running Office and those apps are defined, but I use my Windows VM mostly for things that can't be done on Linux like running the Kindle application. Look in the repository under _apps_ and you'll see that various programs are defined, each to a directory. In the directory is a definition file that includes mime-types consumed and a path to the application in Windows. There's also an icon in the directory. This is what the install script references when it runs.

I was able to create a Kindle definition file. I grabbed an SVG icon from Google Images and created an _info_ file that contained the following.

```bash
# GNOME shortcut name  
NAME="Kindle"

# Used for descriptions and window class  
FULL_NAME="Amazon Kindle"

# The executable inside windows  
WIN_EXECUTABLE="c:\users\brent\appdata\local\amazon\kindle\application\kindle.exe"

# GNOME categories  
CATEGORIES="Education"

# GNOME mimetypes  
MIME_TYPES=";"
```

![Big Brains](https://microfilums.files.wordpress.com/2010/01/2260894625_ea1feecb2a.jpg#floatleft)
I wanted to add the work I'd done back to the project, so I forked the _develop_ branch, added the files, and submitted a Pull Request back to "fmstrat". I also added some troubleshooting suggestions for them based on my experience.

This was a neat adventure to write up because it combines a lot of the things we've been discussing in this blog. The implementation is a lot like running an X application on a remote machine, which we discussed in [Remotely Possible](/Using_SSH5). Of course we talk a lot about Linux and using Linux in a Windows world, and we talk about Git.

But this is another chance to hit on a favorite theme of mine. You don't need to be gods-gift-to-programming to make meaningful and appreciated contributions to the open source community. In the day I took to play with this and contribute the Kindle definition, several other people also submitted apps and the library is quickly growing. Similar to defining GNS3 appliances, there's a lot of ways to give back. There's a real satisfaction to contributing in this way that I hope you have a chance to experience.

As far as Winapps, as I mentioned at the beginning, it's interesting and definitely looks cool. I'm not complete sold that it's that much more useful than just pulling up a Windows VM, but it's close enough that it will fit the work styles of some folks really well. It's worth a look!
