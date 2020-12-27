---
title: "SSH Administrative Trivia"
date: 2020-12-23T18:47:30-05:00
draft: false
author: "Brent Stewart"
github: ""
youtube: ""
refs: [""]
tags: ["SSH","Linux"]
---

Let's consider an interesting case: we'd like to identify remote ssh users.  Remember that SFTP is a part of SSH, so these commands will also identify SSH users.  There are a variety of ways to do this and some are even fairly obviouis.

## Who or w
__Who__ is a utility to display logged in users.  The man page can walk you through the various switches, but the two I find most valuable are _-a_ to show all and _-H_ to show headings.  The _all_ option includes the time that the session has been active, how it's attached, and where it's coming from.

    > who -aH
    NAME       LINE         TIME             IDLE          PID COMMENT  EXIT  
               system boot  2020-12-02 05:47  
               run-level 5  2020-12-02 05:47  
    pop      ? :1           2020-12-02 08:08   ?          3663 (:1)  
    pop      + pts/2        2020-12-23 18:14  old       570072 (192.168.1.72)  
    pop      + pts/6        2020-12-27 15:36 00:01      665161 (192.168.1.81)  

The first column - Name - is the local name that these users are logged in as.  In this example, I'm logged in as "brent" on 192.168.1.81 but my ssh session to this computer uses the username "pop".  The LINE identifies connection - _pts_ stands for psuedo terminal slave, or a sub  process of _pty_ (psuedo-tty).  You may be more familiar with _tty_ connections - those are direct connections like a local terminal.  Notice that there's a local connection and two remote connections in this example.

If __who__ is too much typing for you, try __w__.  It provides very similar output, no switches required

    > w
     16:22:45 up 25 days, 10:35,  3 users,  load average: 1.90, 1.70, 1.53
    USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
    pop      :1       :1               02Dec20 ?xdm?  12days  0.00s /usr/libexec/gdm-x-session --run-sc
    pop      pts/2    192.168.1.72    Wed18    3days  0.09s  0.09s -bash
    pop      pts/6    192.168.1.81    15:36   46:35   0.02s  0.02s -bash

You can also derive this information from __ps__.  This command lists active processes and includes active ssh sessions.  Note that you can pipe to __grep__ to limit it to lines that include 'pts' or 'ssh'.
> ps  ax

## Last
__Last__ looks through /var/log/wtmp and shows login activity.  You can specify a username to see when that user logged in and out.  Note that a psuedo-user named _reboot_ logs in when the system reboots, so __last reboot__ will show a list of all reboots.

    > last
    pop      pts/6        192.168.1.81    Sun Dec 27 15:36   still logged in  
    pop      pts/2        192.168.1.72    Wed Dec 23 18:14   still logged in  
    pop      pts/2        192.168.1.2     Wed Dec 23 18:11 - 18:11  (00:00)  
    pop      :1           :1               Wed Dec  2 08:08   still logged in  
    reboot   system boot  5.8.0-7630-gener Wed Dec  2 05:47   still running  
    pop      :1           :1               Wed Nov 25 18:02 - down  (6+11:44)  
    reboot   system boot  5.8.0-7630-gener Wed Nov 25 17:56 - 05:46 (6+11:50)  
    pop      :1           :1               Mon Nov 23 08:31 - down  (2+09:24)  
    reboot   system boot  5.8.0-7630-gener Mon Nov 23 08:29 - 17:56 (2+09:26)  
    pop      :1           :1               Sat Nov 14 17:40 - down  (8+14:47)  
    reboot   system boot  5.8.0-7625-gener Sat Nov 14 17:39 - 08:28 (8+14:48) 

__Last__ shows similar information to __who__, but shows activity over time instead of just current activity.  On a busy server, __w__ might do a better job of concisely showing current users.  A related utility is __lastb__ which shows _bad_ login attempts.  In the previous example I mentioned that my account is "brent" on 192.168.1.81.  I forgot that there was a different user on this machine and you can see here the failed login attempts.  Notice that this command requires elevated priviledges.

> __sudo lastb__  
brent    ssh:notty    192.168.1.81    Sun Dec 27 15:35 - 15:35  (00:00)  
brent    ssh:notty    192.168.1.81    Sun Dec 27 15:35 - 15:35  (00:00)  
brent    ssh:notty    192.168.1.81    Sun Dec 27 15:35 - 15:35  (00:00)  

Paranoid users may want to review failed logins every time they open a terminal.  
> echo "sudo lastb" >> /home/user/.bashrc  

A similar command is __journalctl -u ssh__.  This shows the systemd journal, so obviously it's only of use on systemd-based systems.  Modern Fedora/RHEL and Ubuntu are on that list.  The switch _-u_ limits output to certain units, in this case _ssh_.  Note that some systems will require the unit to be listed as "sshd".   Notice that this shows socket information and failed attempts and is organized chronologically.  This might be useful if you're trying to match events in troubleshooting.

> __journalctl -u ssh__  
-- Logs begin at Sat 2020-11-14 17:39:49 EST, end at Sun 2020-12-27 18:10:11 EST. --  
Dec 23 18:11:37 pop-os sshd[569656]: Accepted password for pop from 192.168.1.2 port 52778 ssh2  
Dec 23 18:11:37 pop-os sshd[569656]: pam_unix(sshd:session): session opened for user pop by (uid=0)  
Dec 23 18:14:55 pop-os sshd[570072]: Accepted password for pop from 192.168.25.72 port 23639 ssh2  
Dec 23 18:14:55 pop-os sshd[570072]: pam_unix(sshd:session): session opened for user pop by (uid=0)  
Dec 27 15:35:39 pop-os sshd[665153]: Invalid user brent from 192.168.25.81 port 54850  
Dec 27 15:35:41 pop-os sshd[665153]: pam_unix(sshd:auth): check pass; user unknown  
Dec 27 15:35:41 pop-os sshd[665153]: pam_unix(sshd:auth): authentication failure; logname= uid=0 eu>  
Dec 27 15:35:43 pop-os sshd[665153]: Failed password for invalid user brent from 192.168.25.81 port>  
Dec 27 15:35:48 pop-os sshd[665153]: pam_unix(sshd:auth): check pass; user unknown  
Dec 27 15:35:51 pop-os sshd[665153]: Failed password for invalid user brent from 192.168.25.81 port>  
Dec 27 15:35:57 pop-os sshd[665153]: Connection closed by invalid user brent 192.168.25.81 port 548>  
Dec 27 15:35:57 pop-os sshd[665153]: PAM 1 more authentication failure; logname= uid=0 euid=0 tty=s>  
Dec 27 15:36:07 pop-os sshd[665161]: Accepted password for pop from 192.168.25.81 port 54862 ssh2  
Dec 27 15:36:07 pop-os sshd[665161]: pam_unix(sshd:session): session opened for user pop by (uid=0)  



## Network commands
Finally, there are also a few ways to look at this from a network perspective.  You can show socket statistics with __ss_.  This can be interesting for associating unknown traffic to a process id.  The following example is truncated to give a sense of the output, but the full dump is long.

    > ss | more
    Netid State Recv-Q  Send-Q  Local Address:Port         Peer Address:Port    Process
    u_seq ESTAB      0      0   @00031 4813785                 *                4813786        
    u_seq ESTAB      0      0   @00041 8426824                 *                8426825        

__Netstat__ provides another network perspective, this time organized as conversations.  The tabular form of netstat is a little easier to digest.  The version shown uses switches for numeric output, processes info, and all.  

> netstat -npa  
(Not all processes could be identified, non-owned process info  
 will not be shown, you would have to be root to see it all.)  
Active Internet connections (servers and established)  
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name      
tcp        0      0 192.168.25.2:22         192.168.25.81:54862     ESTABLISHED -  

## Conclusion
These networking commands give you a different view into what's happening on your server, but for our original purpose they're abstract.  I'd recommend trying all these techniques to gain wider familiarity with your server, but I find the most common commands I use are __w__, __lastb__, and __journalctl -u ssh__ (depending on what I'm trying to troubleshoot).

Future articles will continue to review some of the administrative issues with maintaining an SSH/SFTP server, such as understanding encryption in use and limiting it to "modern" protocols.