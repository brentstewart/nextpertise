---
title: "Using SSH Part 4 - Port Forwarding"
date: 2020-08-30T15:12:24-04:00
draft: false
Victor_Hugo: "true"
Focus_Keyword: "SSH"
picture: "shell"
author: "Brent Stewart"
github: ""
youtube: ""
refs: [""]
tags: ["SSH","Linux"]
---
![SSH Tunnel](/SSH-Tunnel.png#floatright)__Problem__: We want to access an internal web page that is behind a firewall.  We have SSH access to a server behind the firewall, and _that_ server can see the intranet.

SSH has a solution for this type of problem - tunneling.  Most people use SSH as a telnet replacement -- as just a way to get a terminal session.  SSH is capable of much more than mere terminal access.  There are several ways to accomplish out goal, so let's sort through them.

__NOTE:__ This article demonstrates an obscure and useful way to use a tool, but raises two important points.  First, don't take any part of this to be an example of good design.  I've constructed a case that allows demonstrating a technique.  Second, using the tool this way may short-circuit your organizations' security design and so security folks may want to mitigate against allowing this use. 

## Option 1 - SSH from the client
For this to work, the intermediate host (10.0.0.22 in this case) needs to allow itself to pass ports.  Open the __sshd_config__ file and set GatewayPorts to _yes_.

```bash
sudo nano /etc/ssh/sshd_config
# edit line to remove remark and change to yes
GatewayPorts yes
```
Next, ssh from the external device to the intermediate device and link a local port to an address and port reachable from the ssh target.  In the example below, we connect into 2.2.2.2 ("server") and then we map _local (on the external device)_ port 8080 to a target reachable from the server - webserver port 80.

```bash
ssh -L 8080:10.0.0.80:80 2.2.2.2
# -L maps a local port
# 8080:10.0.0.80:80 ties port 8080 to a remote destination of 10.0.0.80:80
# 2.2.2.2 is the ssh target
```

After running this command, you'll be asked to log into the ssh server normally.  Once logged in, open a web browser on the external client to http://localhost:8080 and the remote internal webpage will be visible.

## Option 2 - SSH from the inside (Reverse Tunnel)
Another option is to make a port available to a remote computer.  In this case, we start ssh from the server and connect to the remote client (which we'll imagine is me.myself.com).  Again, the command prompts us to login to the remote machine.  

```bash
ssh -R 8080:10.0.0.80:80 me.myself.com
```

At this point the remote user can open a browser to http://localhost:8080 and see the internal page.  In fact, the firewall may allow ssh traffic to originate from the webserver.  In that case the reverse tunnel could be established from the webserver without having to use an intermediate host.

```bash
ssh -R 8080:localhost:80 me.myself.com
```

## Option 3 - HTTP from the outside
The final scenario to consider is to allow the server to listen on a port and forward traffic to the intranet server.  Obviously this would require the firewall configuration to allow some port in addition to tcp/22 (SSH) into the server.  In this case, we'll ask the server to listen on port 8001 and forward that traffic to the internal web server.

```bash
ssh -R 8001:10.0.0.80:80 localhost
```

The client can now browse to http://2.2.2.2:8001 to see the webpage.

## Conclusion
SSH port forwarding and reverse SSH connections are powerful tools that can circumvent network policy.  Being familiar with this use may be helpful in troubleshooting, and may be important to you when considering how ssh servers are deployed.
