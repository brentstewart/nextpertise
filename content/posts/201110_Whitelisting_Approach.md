---
title: "Whitelisting Approach to Security"
date: 2020-10-07T06:30:00-00:00
draft: false
Victor_Hugo: "true"
Focus_Keyword: "Whitelisting, AppSamvid"
picture: "security"
author: "Myron Estibeiro"
refs: ["https://cdac.in/index.aspx?id=cs_eps_appsamvid"]
tags: ["Security"]
---

Today, I am sure that you would be hearing a lot of buzz around ransomware and other malware outbreaks.  To combat some of these threats on a home consumer scale, we mostly use a traditional __Anti-Malware__ (Anti-Virus) software. As you may know, this technology is based on signatures, patterns and heuristics, which are extracted from the malware behaviour or type; thus, as the number of malware instances increase, becoming more sophisticated in the ever increasing threat landscape, it becomes very hard for the Anti-Virus companies to keep up with the pace. I would like to call this a “Cat and a Mouse Chase”.  Anti-Virus software may give you the best protection against known malware, but it may never give you adequate protection against unknown malware, whose signatures, patterns and heuristics are not known to anyone. By design, the traditional Anti-Virus fails miserably and is unable to keep up with todays sophisticated and evolving malware. This post is a follow-up to [reasonable secure browsing](/posts/200805_reasonablysecurebrowsing/).


Instead of focussing on blocking known malware through an Anti-Virus software, how about we only allow the applications and programs we trust and use on daily basis, and block everything else? Well, this is exactly what is called a __“Whitelist”__ approach, while the former that we discussed above is called a __“Blacklist”__ approach. This approach can be nearly 100% effective in blocking all malware if used correctly; however, most of the Whitelisting software out there is very expensive and only targeted towards enterprise use.


![WL](/whitelist.png)


I use something called as __AppSamvid__, which is a free utility that is freely available at: https://cdac.in/index.aspx?id=cs_eps_appsamvid
You can use this on any of your Windows based system and have it run as a Whitelisting software. Once installed, you would need to provide the list of applications and software utilities that you trust and use daily. Ideally, you should setup your system and create an effective “Baseline” that is a known ‘good’ or ‘trusted’ system state. Beyond this, any attempts to execute applications that are not listed in the Trusted list would be automatically blocked. You can unblock applications by providing the path or the file Hash. This is highly effective in blocking known and known malware.


Give this a try and get one step closer in perfecting your system security!