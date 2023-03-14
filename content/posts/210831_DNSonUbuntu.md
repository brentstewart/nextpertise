---
title: "DNS on Ubuntu"
date: 2021-08-31T19:08:11-04:00
draft: false
author: "Brent Stewart"
Victor_Hugo: "true"
Focus_Keyword: "DNS"
picture: "linux"
github: ""
youtube: ""
refs: [""]
tags: ["Linux","DNS"]
---
Continuing from my [previous post](/posts/210830_apache/), I have recently rebuilt my server infrastructure at home, migrating from VMWare to Proxmox VE.  I'm still getting the hang of Proxmox, although I'm feeling favorable towards it so far.  In the meantime, I wanted to document some of the little pieces to setting up a home network.  This time I'll provide a walk through of a simple local DNS server.  My goal at home is to create a "stewart.lan" network that I can use to reference local resources.

Like the Apache server I built last time, this server is running Ubuntu 21.04 Server and my instructions are written from that perspective.  Some commands may change as you move to non-Debian distributions or with different versions.  Installation of DNS services is done with __bind9__.

    sudo apt update
    sudo apt install bind9
    sudo ufw allow 53
    sudo ufw allow 53/udp

Once the service is installed, __bind__ configuration files are found in _/etc/bind_.  In my configuration there are five files that I modified or created: the service configuration file _named.conf_ which loads in _named.conf.local_ and _named.conf.options_.

    root@webnamer:/etc/bind# cat named.conf
    include "/etc/bind/named.conf.options";
    include "/etc/bind/named.conf.local";

    ###

    root@webnamer:/etc/bind# cat named.conf.local
    //
    // Do any local configuration here
    //
    zone "stewart.lan" {
	    type master;
	    file "/etc/bind/forward.stewart.lan";
	    allow-query { any; };
    };

    ###
    
    root@webnamer:/etc/bind# cat named.conf.options 
    acl internal-network {
	    192.168.24.0/22;
	    localhost;
	    localnets;
    };
    options {
	    directory "/var/cache/bind";
	    forwarders {
		    192.168.26.53;  //this server
		    208.67.222.222; //OpenDNS1
		    208.67.220.220; //OpenDNS2
		    8.8.8.8;        //Google DNS1
		    8.8.4.4;        //Google DNS2
	    };
        allow-query { internal-network; };
	    allow-query-cache { internal-network; };
        allow-recursion { internal-network; };
        allow-transfer { none; };
        allow-update { none; };
        dnssec-validation yes;
        auth-nxdomain no;
        recursion yes;
        notify no;
        listen-on { any; };
        listen-on-v6 { none; };
    };
Anytime DNS config files are changed the system will need to be restarted.

    systemctl restart named


With these files in place recursive lookups (like _google.com_) should be working.  This can be tested by changing DNS on a machine, or by using __dig__ or __nslookup__.  __Dig__ accepts arguments for the DNS server to use, the domain to be queried, and the type of record (by default A) to be returned.  In the example below, the server and the domain to be returned are specified.   __NSLookup__ is a similar command that queries DNS servers.  In the example below, the server command tells it to connect to the new server and then the "www" is a query for a record (haven't gotten to the local zone setup yet).  

    root@webnamer:/etc/bind# dig 192.168.26.3 stewart.lan

    ; <<>> DiG 9.16.8-Ubuntu <<>> stewart.lan
    ;; global options: +cmd
    ;; Got answer:
    ;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 35138
    ;; flags: qr aa rd ra; QUERY: 1, ANSWER: 0, AUTHORITY: 1, ADDITIONAL: 1

    ;; OPT PSEUDOSECTION:
    ; EDNS: version: 0, flags:; udp: 1232
    ; COOKIE: 1b490288965657d7010000006131687c06bba43e1f56fdbc (good)
    ;; QUESTION SECTION:
    ;stewart.lan.			IN	A

    ;; AUTHORITY SECTION:
    stewart.lan.		604600	IN	SOA	localhost. root.localhost. 6 604820 86600 2419600 604600

    ;; Query time: 0 msec
    ;; SERVER: 192.168.26.53#53(192.168.26.53)
    ;; WHEN: Thu Sep 02 20:12:44 EDT 2021
    ;; MSG SIZE  rcvd: 118

    root@webnamer:/etc/bind# nslookup
    > server 192.168.26.53
    Default server: 192.168.26.53
    Address: 192.168.26.53#53
    > www
    Server:		192.168.26.53
    Address:	192.168.26.53#53

    > 

If there are problems at this point this point, use __named-checkconf__ to review the configuration files for errors.  By default, _named_ logs can be reviewed with __tail /var/log/syslog__ as well.

A forward lookup zone (which matches names to numbers) needs to be created if we want a local zone.  My house is named _stewart.lan_, but any name is fine with the caveat that collisions with valid public name spaces should be avoided.  A forward lookup zone is a text file similar to the one below.  Note that this file was referenced in the _named.conf_ setup.  A records link names to IPs.  CNAMEs are alias records.

    brent@webnamer:/etc/bind$ cat forward.stewart.lan
    $TTL    604800
    @       IN      SOA     localhost. root.localhost. (
                                6         ; Serial
                            604820         ; Refresh
                            86600         ; Retry
                            2419600         ; Expire
                            604600 )       ; Negative Cache TTL
    ;
    ;Name Server Information
    @       IN      NS      ns.stewart.lan.

    ;IP address of Your Domain Name Server(DNS)
    ns IN       A      192.168.26.53

    ;A Record for Host names
    gw     IN       A       192.168.26.1
    ns	IN	A	192.168.26.53
    pop	IN	A	192.168.25.7
    print	IN	A	192.168.24.17
    oldprint	IN	A	192.168.24.11
    server	IN	A	192.168.26.9
    proxmox	IN	A	192.168.26.10
    library	IN	A	192.168.26.11


    ;CNAME Record
    www	IN	CNAME	ns.stewart.lan.
    dav	IN	CNAME	ns.stewart.lan.
    newprint	IN	CNAME	print.stewart.lan.
    pve	IN	CNAME	proxmox.stewart.lan.
    webnamer	IN	CNAME	ns.stewart.lan


This setup doesn't show the reverse lookup zone (24.168.192.in-addr.arpa), but that can be built easily and added if needed.  Reverse zones link numbers to names and are used for authentication usually.  With DNS setup and the forward zone in place, we should be able to ping by name (link _printer.stewart.lan_).  If there are problems, use __named-checkzone__ to confirm that the format of your zone file is correct.