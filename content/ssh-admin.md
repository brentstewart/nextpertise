---
title: "SSH Administrative Trivia"
date: 2020-12-23T18:47:30-05:00
draft: false
author: "Brent Stewart"
github: ""
youtube: ""
refs: [""]
tags: ["SSH"]
---

Moving keys
knownhosts file key match
server creds ref'd in /etc/ssh/sshd_config:
># HostKeys for protocol version 2
HostKey /etc/ssh/ssh_host_rsa_key  
HostKey /etc/ssh/ssh_host_dsa_key  
HostKey /etc/ssh/ssh_host_ecdsa_key  
HostKey /etc/ssh/ssh_host_ed25519_key  

/etc/ssh/ssh_host*

public keys /home/-account-/.ssh/authorized_keys


# who's on?
> who

>cat var/log/sshd.log | grep session

> jounralctl -u sshd

> netstat -tnpa | grep 'ESTABLISHED.*sshd'

> ps auxwww | grep sshd:
ps ax | grep sshd
ss | grep ssh
https://www.golinuxcloud.com/list-check-active-ssh-connections-linux/

# who am I
echo $SSH_CONNECTION
who (-a)


last | grep "still logged in"


# Enc
https://www.digitalocean.com/community/tutorials/understanding-the-ssh-encryption-and-connection-process

ssh -Q cipher       # List supported ciphers
ssh -Q mac          # List supported MACs
ssh -Q key          # List supported public key types
ssh -Q kex          # List supported key exchange algorithms

ssh -G user@pc query used
https://www.openssh.com/legacy.html


https://www.linux.com/training-tutorials/configuring-linux-sudoers-file/
