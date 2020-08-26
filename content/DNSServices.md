---
title: "DNSServices"
date: 2020-08-26T12:01:51-04:00
draft: false
author: "Brent"
github: ""
youtube: ""
refs: ["https://www.opendns.com","https:/1.1.1.1","https://quad9.net"]
tags: ["Security","Non-technical"]
---
This post is to continue the conversation started in [Reasonably Secure Browsing](/ReasonablySecureBrowsing).  Like that post, this is intended for non-industry friends.  My goal is to have a set of references for the people I care about, but who don't share my love of technology.  For the gurus out there, understand that this leaves out a lot of details for the sake of clarity for the target audience.

The first post covered browser settings that balanced security, privacy concerns, and convenience.  Another way to improve your security is use an alternative DNS provider.  DNS (Domain Name System) is an under-the-hood service that connects a name like "amazon.com" with a number like 176.32.103.205.  Think about your mobile phone.  We rarely memorize peoples telephone numbers anymore, we just call "Brent" or choose the picture that matches the person we want to call.  The phone translates that into a telephone number.  DNS is a centralized process that translates a name to a number to make the internet more "human friendly".

Why should you care?  Three reasons - speed, security, and privacy.  DNS is typically set up by your Internet provider.  Sometimes these implementations are the sources of problems, so changing the default DNS will give you a more stable (and sometimes faster) experience.  Anyone "listening" to your traffic can easily make a list of where you visit by tracking DNS requests.  This could be someone snooping on a shared wifi at the coffee shop, or it could be your ISP (in fact, ISPs sometimes redirect your traffic to internet locations they control for their own benefit).

There are a set of alternative DNS providers that provide free services that are unfiltered and private.  I recommend that you consider one of these.  They're all good, but I've listed them in the order of my preference.

To enable, go into your router and change the DNS setting to the IP provided (the exact instructions vary by router).  At home, this is a reasonable step to protect you and your family.  If you travel, these providers also support a variety of options to encrypt your request, including DNS over HTTPS (DoH).  I recommend using DoH within Firefox for devices that leave the house.

Firefox can encrypt your DNS traffic from "snoopers".  Go to the menu button and choose _preferences_ and then _Network Settings_.  Click _Enable DNS over HTTPS_ and then choose a provider or select "Custom" and type in the IP address (included below).

## OpenDNS (208.67.222.222) - Best for home use
OpenDNS is a great choice.  By default, their free service is excellent.  It recognizes many cases where you've been directed to a malicious site, and keeps you out of trouble by blocking it.  OpenDNS is owned by Cisco, and it benefits from the huge investments they've made in Internet security.  It also blocks adult content by default.  You can create an account and customize your home's experience - for instance, do you want to block Gambling or Tobacco advertising?  These settings are tied to your home IP, so your laptop goes back to the "default" when not at home.  Especially for public places, like church wifi, or for a home with children this allows you to control what portion of the Internet is available to users.

## Cloudflare (unfiltered 1.1.1.1, malware blocking 1.1.1.2, malware and adult content 1.1.1.3) - best for travelers
Cloudflare is very easy to setup.  Use the IP address that matches your use case.  These settings can carry over when you are away from home if you change them on your device.  There's also an app that provides this service no matter where you are (just for mobile with Windows and Mac coming soon).  Cloudflare has good settings for most cases, is easy to setup, and has the mobile apps, but lacks the customization of OpenDNS.

## Quad9 (9.9.9.9)
Quad9 provides a service that is very similar to Cloudflare 1.1.1.2 - just set the DNS and forget it and it provides DNS with malicious sites blocked.  My experience with Quad9 has been largely indistinguishable from Cloudflare or OpenDNS with default settings, when testing blocking or response speed.  However, Quad9 doesn't have the customization of OpenDNS or the apps that Cloudflare has.

## Google (8.8.8.8)
This has been a popular alternative for the geek set for a long time.  It's easy to remember and it provides an "unfiltered" response.

## Summary
Using one of these services helps to protect your home.  I use OpenDNS at home and have customized it to filter out a range of categories.  It doesn't block everything on the Internet, but it addresses some of the obvious sites and it helps prevent "oops" experiences.  It seems to help with advertising too.  I use Cloudflare on my office network and it does a great job as well.

If you've benefitted from the "Reasonably Secure Browsing" discussion, this is another "reasonable" step that you can take to improve your families Internet experience.