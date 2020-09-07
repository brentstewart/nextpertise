---
title: "Using SSH - Part 2 (Authentication)"
date: 2020-08-12T11:36:12-04:00
draft: false
Victor_Hugo: "true"
Focus_Keyword: "SSH"
author: "Brent Stewart"
github: ""
youtube: ""
refs: ["https://www.fail2ban.org", "https://www.digitalocean.com/community/tutorials/how-to-set-up-multi-factor-authentication-for-ssh-on-ubuntu-16-04"]
tags: ["How To","SSH", "Linux"]
---
This aricle makes up part two of the series on SSH.  If you're interested in the basics or how to setup a banner, refer to the first article.  As with the first article, I've tested all of this on Ubuntu Linux and exact commands may vary as you get farther from there.

## Secure Authentication with Passwords

By default, SSH authenticates users via a password.  Passwords are transmitted in a secure manner, but can be prone to brute force guessing attacks.

One way to secure the ssh interface is to limit the devices allowed to access your server.  This can be done at different places - on your network firewall, in the OS firewall, or in the ssh process.   SSH uses TCP port 22, so blocking that at the firewall is one way to mitigate against maliciousness.  Since this article is about using SSH, we'll focus on the latter.  Go into sshd_config and add a line for AllowUsers.  The example below allows anyone to login from the 192.168.1.0/24 network. Remember to restart the ssh service after changing sshd_config: __sudo systemctl restart ssh__. 
> __sudo nano /etc/sshd_config__  
>> __AllowUsers \*@192.168.1.*__

Blocking source addresses only works up to a point.  Bad actors from within can still attack, and outside actors can use another host as a jump server (SSH to there, then start a new SSH session from the inside box).  Picking a good password helps make brute-force attacks take longer, but we need to prevent opportunities to work through every combination of letters.  Fail2ban is a service that blocks IP addresses that exhibit suspicious behavior.  Install it using __sudo apt install fail2ban__.  Below is a script that will setup fail2ban to block IPs that fail three consecutive login attempts.
> __echo setup fail2ban__  
> __systemctl start fail2ban__  
> __systemctl enable fail2ban__  
> __echo "/[sshd]__  
> __enabled = true__  
> __port = 22__  
> __filter = sshd__  
> __logpath = /var/log/auth.log__  
> __maxretry = 3" >  /etc/fail2ban/jail.local__  

## Authentication with keys
Another way to login is using keys.  A key pair - public and private - can be generated on a client and authenticates the client to the server.  Since the keys are stored in the _user_ account, they also in theory are associated with identity.  There are two advantages of using keys.  First, it can eliminate remembering and typing a knuckle-busting password and supports automation.  Second, keys are more secure than passwords _on the assumption that the key file is secure_.

To use public-key authentication, you first need to generate a key pair using the command __ssh-keygen__.  You can optionally enter a passphrase to use to unlock the key.  By default, the public key is saved as _~/.ssh/id\_rsa_ and the private key as _~/.ssh/id\_rsa.pub_.  This process is shown below.
> brent@inspiron:~$ __ssh-keygen__  
> Generating public/private rsa key pair.  
> Enter file in which to save the key (/home/brent/.ssh/id_rsa):   
> Enter passphrase (empty for no passphrase):   
> Enter same passphrase again:   
> Your identification has been saved in /home/brent/.ssh/id_rsa  
> Your public key has been saved in /home/brent/.ssh/id_rsa.pub  
> The key fingerprint is:  
> SHA256:A5RBWIxVGMCAQbzAfenno9hlwQAeafZgnCPJCylrnz8 brent@inspiron  
> The key's randomart image is:  
> +---[RSA 3072]----+  
> |*====OO*.        |  
> |**.@==+          |  
> |+.B.* +          |  
> |.+   o =         |  
> |. . . o S        |  
> |   o   = .       |  
> |    + + .        |  
> |   . G           |  
> |      .          |  
> +----[SHA256]-----+  
> brent@inspiron:~$   

I don't want to publish my keys to the world, so I just re-ran __ssh-keygen__ and accepted the prompt to overwrite the old set.

Once a key pair is generated, the public key needs to be copied to the host that you want to login to.  To do this, you need password access to the host and this process doesn't disable password access.  Unless you opt to turn that off, you still need to secure the password access using ACLs and fail2ban as previously discussed.  That said, ssh includes a utility to push your public key to a target device - __ssh-copy-id__.

> brent@MintyTwenty:~$ __ssh-copy-id brent@192.168.1.1__   
> /usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed  
> /usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys  
> brent@192.168.1.1's password:   
>  
> Number of key(s) added: 1  
>  
> Now try logging into the machine, with:   "ssh 'brent@192.168.1.1'"  
> and check to make sure that only the key(s) you wanted were added.  

Now I should be able to just type __ssh brent@192.168.1.1__ and be attached to the server without a password prompt!  Easier administration and easier to script.

## Authentication with TOTP
TOTP is for the _really_ paranoid and for those uber-geeks that want to impress their friends.  Digital Ocean has a really nice write up on this, and that was my primary source for learning.  I've referenced it in the notes.  Their procedure is written for Ubuntu 16.04 but I've personally used it up through 20.04 without a problem.

Ideally authentication involves something you _know_ and something you _have_.  Time-based One Time Passwords are six-digit codes that change periodically.  Hopefully, you already use this to secure critical online resources like your email.  TOTP utilities generate a 3D barcode that can be read by the camera on a phone, and use that to set a unique nugget that can be combined with the time to give random number strings.  Google authenticator is the "go-to" app on the phone for entering and holding these authenticators.  I use _Enpass_, which does a similarly good job.

Before you begin, you'll need the authenticator app loaded on your phone and you'll need to be physically in front of the server.  On the server, install the authenticator module and initiate the settings.
> __sudo apt install libpam-google-authenticator__  
> __google-authenticator -t -d -f -r 3 -R 30 -W__   # NOTE: using cmd w/o flags will walk you through prompts  

The __google-authenticator__ command will show you a 3D barcode and your first code.  Scan that in on your phone and verify the code.  The output will also include five "emergency scratch codes".  These would be used if you lose your phone.  Write them down somewhere for emergencies before continuing.

Next, add a line to _/etc/pam.d/sshd_ for authentication and edit a line in _sshd\_config_ for Challenges.  Restart the service and you'll be ready to test.
> __sudo nano /etc/pam.d/sshd__  
> #_add this line, then close the file_
>> __auth required pam_google_authenticator.so nullok__
>  
>__sudo nano /etc/.ssh/sshd_config__  
> #_find and change this line, then close the file_
>> __ChallengeResponseAuthentication yes__
>
> #_restart sshd_
> __sudo systemctl restart sshd.service__

At this point, try connecting to this server using ssh.  It should _either_ use a key or prompt you for your password and then for the current TOTP code.  If you want it to require TOTP when using a key, you'll need to edit sshd_config and restart the process again.

> __sudo nano /etc/.ssh/sshd_config__  
> #_add this line_
>> __AuthenticationMethods publickey,password publickey,keyboard-interactive__

## Recommendations
I've presented a lot of ideas here, so I want to conclude by giving you my recommendations for personal machines.
* Install SSH server by default
* Use a banner in .bashrc to make clear which device you are currently logged into
* Limit SSH to local IPs unless there's a specific requirement otherwise.  If you can't limit by IP, use TOTP.
* Use fail2ban
* Use keys.  Don't try to use the same keys on all devices, just generate new ones every time you re-install or get a new PC.  At least for me, this cuts down on the risk of keys falling into outside hands.