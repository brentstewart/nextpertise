---
title: "SSH Crypto"
date: 2020-12-29T13:01:35-05:00
draft: false
author: "Brent Stewart"
Victor_Hugo: "true"
Focus_Keyword: "SSH"
github: "https://github.com/brentstewart/ssh-crypto"
youtube: ""
refs: ["https://www.cisecurity.org/", "https://www.digitalocean.com/community/tutorials/understanding-the-ssh-encryption-and-connection-process#negotiating-encryption-for-the-session"]
tags: ["SSH","Linux"]
---
# Cleaning up Crypto
A previous article - [SSH Admin](/ssh-admin) - went through understanding who was logging into a Linux server using SSH or SFTP.  To continue that thought, let's suppose that we are required to make sure that only cypher suites recommended in the CIS benchmarks are in use on a server.  Before we disable old options, we need to check and make sure that no one is using them!

## Understanding local crypto

From a client, we can see which cipher elements are supported.  Each of these commands outputs a range of protocols.  When connecting to a server, the client transmits protocols that it supports and the server reciprocates.  They then agree to use the first option from the client's list that is supported on the server (or the connection fails).  The table below lists commands used to see the protocols supported on a client.  The examples were chosen because they were well known and establish context, and not as a recommendation.

{{< bootstrap-table table_class="table table-responsive table-hover" thead_class="table-info" caption="Table: SSH options" >}}
| Element | Command | Example options  |
|------|-----|------|
| Cipher | ssh -Q cipher | 3des-cbc, aes256-cbc |
| MAC | ssh -Q mac |hmac-md5, hmac-sh2-256 |
| Key | ssh -Q key | ssh-rsa, ecdsa-sha2-nistp256 |
| Kex | ssh -Q kex | diffie-hellman-group1-sha1, curve25519-sha256 |
{{</bootstrap-table>}}

Setting up an SSH connection goes through some basic phases:
* Kex (key-exchange) is used to complete an asymmetrically encrypted initial key exchange.
* Keys are exchange.  The key list is types of keys supported.
* The body of the communication is encrypted symmetricly.
* MAC or "message authentication code" is a hash that verifies the integrity of transmissions.

## Understanding remote clients crypto

It's surprising that there isn't a command to show which cipher suites are in use by particular clients.  To build a tool, I went into /etc/ssh/sshd_config and set the logging level to grab everything.

> \# Logging  
SyslogFacility AUTH  
LogLevel DEBUG3  

This can then be reviewed using __journalctl -u ssh__ to display entries related to the sshd unit.  I noticed that the relevant lines were at DEBUG1 level and that each sequence completed with the "password accepted" line.  Based on this pattern, I wrote a utility in Python to create a report.

>  Dec 28 15:56:44 pop-os sshd[701591]: debug1: kex: algorithm: curve25519-sha256 [preauth]  
Dec 28 15:56:44 pop-os sshd[701591]: debug1: kex: host key algorithm: ecdsa-sha2-nistp256 [preauth]  
Dec 28 15:56:44 pop-os sshd[701591]: debug1: kex: client->server cipher: chacha20-poly1305@openssh.com MAC: <implicit> compression: none [preauth]  
Dec 28 15:56:44 pop-os sshd[701591]: debug1: kex: server->client cipher: chacha20-poly1305@openssh.com MAC: <implicit> compression: none [preauth]  
Dec 28 15:56:46 pop-os sshd[701591]: debug1: PAM: password authentication accepted for pop

[__ssh-crypto__](https://github.com/brentstewart/ssh-crypto) is a Python3 program to read ssh debugging and identify who has logged in and what settings were used.  It expects a file name, which is a text file that contains ssh logging output.  First make sure that sshd is logging at least at DEBUG1.  Restart the SSH service for the new logging setting to take effect.
> sudo service sshd restart

Keep in mind that the crypto fields won't be populated for logins before the loggin change takes effect.  To create the text file for analysis, export from journalctl.
> journalctl -u ssh > ~/ssh.txt

Again, __ssh-crypto assumes that the system has Python3, uses Systemd, has debugging setup.

### Usage
     pop  pop-os  ~  $  ~/git/ssh-crypto/ssh-crypto.py ~/ssh.txt
    -------------------------------------------------------------------------------------------------------------------
    | # |       User        |       IP       |     Algorithm      |        Host        |            Cipher            |
    -------------------------------------------------------------------------------------------------------------------
    |  0|pop                |192.168.25.2    |undefined           |undefined           |undefined                     |
    |  1|pop                |192.168.25.72   |undefined           |undefined           |undefined                     |
    |  2|pop                |192.168.25.81   |undefined           |undefined           |undefined                     |
    |  3|pop                |192.168.25.81   |undefined           |undefined           |undefined                     |
    |  4|pop                |192.168.25.81   |undefined           |undefined           |undefined                     |
    |  5|pop                |192.168.25.81   |curve25519-sha256   |ecdsa-sha2-nistp256 |chacha20-poly1305@openssh.com |
    |  6|pop                |192.168.25.81   |curve25519-sha256   |ecdsa-sha2-nistp256 |chacha20-poly1305@openssh.com |
    -------------------------------------------------------------------------------------------------------------------
## Removing Weak Ciphers
Per the CIS Ubuntu 20.04 Standard (5.2.12), FIPS compliant ciphers include aes256-ctr, aes192-ctr, aes128-ctr.  FIPS compliant MACs include hmac-sha2-256 and 512.  FIPS allows a pretty broad range of key exchange protocols, including ecdh-sha2-nistp256, ecdh-sha2-nistp384, ecdh-sha2-nistp521, diffie-hellman-group-exchange-sha256, diffie-hellman-group16-sha512, diffie-hellman-group18-sha512, and diffie-hellman-group14-sha256.
To limit the server to only accept these options, edit /etc/ssh/sshd_config.  Here are the ones I've chosen to support.
> Ciphers chacha20-poly1305\@openssh.com,aes256-gcm@openssh.com,aes256-ctr  
MACs hmac-sha2-512-etm\@openssh.com,hmac-sha2-256-etm@openssh.com,hmac-sha2-512,hmac-sha2-256  
KexAlgorithms curve25519-sha256,curve25519-sha256@libssh.org,diffie-hellman-group14-sha256,diffie-hellman-group16-sha512,diffie-hellman-group18-sha512,ecdh-sha2-nistp521,ecdh-sha2-nistp384,ecdh-sha2-nistp256,diffie-hellman-group-exchange-sha256  

Using ssh-crypto will allow review of recent client connections and unused ciphers can be weeded out.  After communicating the change to users, specific recalcitrant users can be identified for follow-up with the utility before ultimately removing the old protocols.