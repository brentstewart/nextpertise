---
title: "Using SSH - Part 1 (Basics and Banners)"
date: 2020-08-11T12:36:12-04:00
draft: false
Victor_Hugo: "true"
Focus_Keyword: "SSH"
picture: "shell"
author: "Brent Stewart"
github: ""
youtube: ""
refs: ["https://docs.microsoft.com/en-us/windows-server/administration/openssh/openssh_install_firstuse","https://www.chiark.greenend.org.uk/~sgtatham/putty/","https://www.solarwinds.com/free-tools/solar-putty", "https://github.com/chubin/wttr.in"]
tags: ["How To","SSH","Linux"]
---

SSH is a pretty well known protocol that's used for a lot of different things.  Most of us are familiar with the basics and a trick or two.  This article is to try to consolidate a lot of the uses I have for SSH and share them.  The article is a quick review of basic terminal access and banners.  This is the first in a series, so more advanced topics are covered in succeeding posts.

## The Basics
SSH is included in modern operating systems.  Apparently it can now also be installed on Windows (I've included a link).  If you use Windows, the standard client suggested is PuTTY (I really like Solar-PuTTY as well). I have not used Windows as a client or server in my testing, so hopefully my comments will be helpful but I suspect server setup is going to be different.

My walk through assumes you are using a command-line client.  Note that the ssh _client_ is typically installed in the *nix world.  If you want your box to be the server then you'll need to add it via __sudo apt install openssh-server__ (Debian/Ubuntu).

Most of us encounter SSH as a secure replacement for telnet.  SSH allows us to connect to a remote terminal from the command line.  Assuming that I wanted to connect to my firewall by it's IP address and that there was an account named "brent" there, I can connect using __ssh _username_@_Destination__.  If this is the first time you've connected, you'll be asked to confirm the fingerprint.

```plaintext
brent@MintyTwenty:~$ ssh brent@192.168.24.230  
The authenticity of host '192.168.24.230 (192.168.24.230)' can't be established.  
ECDSA key fingerprint is SHA256:1XYZ12MBd5Sb345ABOBhoKx42D+STU56szGR/d3LkGs.  
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes  
Warning: Permanently added '192.168.24.230' (ECDSA) to the list of known hosts.  
brent@192.168.24.230's password:  
brent@inspiron:~$  
```

The fingerprint is to protect against a man-in-the-middle attack, where your traffic is being re-directed to a malicious third party.  Before you type in (and reveal) your password, best to make sure that this is a trusted server!  So, where do we find the fingerprint to match it to?  The easiest way to get it is to go to your server and use ssh to connect to itself: __ssh _username_@127.0.0.1__.  This will show the local fingerprint.  If someone has already used this trick and accepted the fingerprint, you can delete ~/.ssh/knownhosts (_not recommended_) or use ssh-keygen to examine the local public key.

```plaintext
brent@MintyTwenty:~$ __ssh-keygen -lf .ssh/id_rsa.pub  
4096 SHA256:cjyCsHXYZ12dESNo+12AB/oGGaxY1JHSTU%1p3Aeouw brent@X (RSA)
```

## Banners

SSH banners are specified in the ssh daemon configuration (_/etc/sshd\_config_),  To specify a banner, find the line reads "#banner none" and edit it to specify a file.  The contents of this file will be displayed _before_ the password prompt.

```plaintext
sudo nano /etc/sshd_config  
banner /etc/banner.txt
```
![neofetch](/neofetch.png#floatsmallright)

After authentication the prompt displays the server hostname.  You can display a banner _after_ authentication by editing _~/.bashrc_.  This has a side benefit - the terminal, when connected to locally or remotely, processes _~/.bashrc_ before it produces a prompt.  Go to the end of that file and add whatever you like - that output will be displayed before a prompt is produced.  I've listed some cool ideas to build a dynamic banner below.

* __neofetech__ is a popular script that summarizes system information.  There's a ppa available to add this from apt.

```bash
sudo add-apt-repository ppa:dawidd0811/neofetch  
sudo apt install neofetch  
echo "neofetch" >> /home/brent/.bashrc  # Add the command to the end of .bashrc  
```

![figlet](/figlet.png#floatright)

* __Figlet__ - draws letters in ASCII for a nice banner and any command can be piped through it (echo "for example" | figlet).  It's available in the standard Ubuntu repository.

* __Curl__ - pull in data from the web.  Try __curl v2.wttr.in/Hickory+NC__.  A more practical example might be:

```bash
curl wttr.in/Hickory+NC?format=2  # check out the github page for lots more options
```
* __Server stats__ - display information about the server such as IP (__hostname -I)__ or temperature (__sensors__).  This snippet will display just the main temperature.
```bash
data=$(sensors | grep "id 0:" | cut -c16-23)      #sensors displays a lot of data.
                            # Grep just grabs the one line, and cut pulls temp out.  
echo "CPU Temp:${data}"  
```

Part two of this series will cover secure authentication options.