---
title: "Using the SSH client config file"
date: 2021-04-02T15:40:47-04:00
draft: false
author: "Brent Stewart"
Victor_Hugo: "true"
Focus_Keyword: "SSH"
picture: "shell"
github: ""
youtube: ""
refs: ["https://www.openssh.com/manual.html"]
tags: ["ssh"]
---
This article continues a series of articles I've done on SSH.  We've used the server configuration file (sshd_config) to set parameters, but many folks do not realize that there is a client configuration file as well.  In fact, ssh uses command line options, then the _client_ file, then the server file when building the capabilities of a new connection.

## Simple Example

The file is a plain text file found in __~/.ssh__.  It doesn't exist by default, so a new file with that name needs to be created.  The simplest version of an ssh config file looks something like this.

> host server  
  hostname 10.1.1.1  
  user brent  

Even at this stage, this is beneficial.  We can use the __host__ to resolve a name, so even without DNS we don't have to remember IPs.  Because the user is specified we can now simplify our ssh command to: __ssh server__.

I am often moving between locations and don't have access to internal DNS.  I can share this file between my desktop and laptop and ease some of the memorization required to move through the environment.

## What Else Can We do?

Building on the previous example, we can also specify a non-standard port or a specific key.  We can add additional hosts as well.

> host server  
  hostname 10.1.1.1  
  user root  
  port 2222  

>  host server2  
  hostname 10.2.2.2  
  user vagrant  
  localforward 2222 10.1.1.1:2222

>  host home    
  hostname 192.168.1.10  
  user brent 

This example includes three servers.  The first now uses a non-standard port.  The second is setup to forward tcp traffic on 2222 to the first server.  Each has a different username specified.  

Other examples of additional commands can be found in the OpenSSH documentation (referenced below).

## Wildcards
The last complication I'll add is to add the following to the config file above.  Wildcards allow us to specify things that apply to multiple hosts.

>  host serv*  
  identityfile server_id_rsa  

> host *  
  ForwardX11Trusted yes

Connecting to "server" will now pull in the the key file and X11 command.  For "home" only the X11 forwarding would be added.

These days we're connecting to local servers with one set of credentials and cloud hosts with a different set.  Often there's some specific options that have to be used when connecting to the cloud host - to specify a keyfile for instance.  Particularly given the complication of managing cloud assets, the client config file can be an important tool.

I'll close by discussing security.  Building an ssh client config file can make life easier and there's a natural desire to share that work in a team.  This is probably safe, assuming that servers are locked down with something more than passwords.  Reference my article on [PAM changes](/200812_using_ssh2) if you are interested in that.  If you are managing a large environment, it's probably good to think about some centralized authentication (like sssd) so that you can quickly update credentials.  The config file - by itself - doesn't compromise security except for allowing an outsider to "case the joint".  Still, I'd suggest handling the config file conservatively and limiting distribution.
