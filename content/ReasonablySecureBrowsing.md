---
title: "Reasonably Secure Browsing"
date: 2020-08-05T11:23:41-04:00
draft: false
author: "Brent Stewart"
github: ""
youtube: ""
refs: ["http://opendns.com", "https://1.1.1.1", "https://haveibeenpwned.com/"]
tags: ["Security","Non-Technical"]
---
Most of my articles are aimed at helping me remember how I did something _cool_ years later, and helping other people who share my interest and want to solve similar problems.  This one is a little different.  When I speak to non-technical folks - at church, other parents, or within my family - I'm disappointed with the lack of understanding about computer security.  I was going to say "surprised", but sadly I'm not.

I get that we can't all be aware of __everything__.  For instance, I'm not really interested in cars.  Still, if you are participating in an area (driving a car or browsing the web), then there's a basic level of awareness that is needed and that you should aim for.  So this post is written for the non-technical in order to provide that grounding.  I'll try to keep this up to date (pay attention to the posting date!) and I'll start pointing people who ask those questions to this resource.
 ![Tim McGee from NCIS](https://upload.wikimedia.org/wikipedia/en/0/09/Timmcgee.jpg#floatleft)
I need to define "reasonable" before I get started.  Internet paranoia comes in three flavors - fear of malicious actors, fear of giant companies assembling dossiers to feed into marketing, and Orwellian fears about nation-states.  All three are reasonable things to be concerned about but this post only addresses the first.  Hiding from the NSA is beyond the ability of most people - even if they unplug.  Hiding from creepy companies is possible, but requires foregoing a lot of services that most of us are loathe to do without.  However, there are small steps that you can take that will give you a good degree of protection against maliciousness.  Reasonable means simple steps picking up bad code or leaking sensitive personal information.

Finally, what makes me think I'm an expert?  Well, I have a Master's in Information Security and I've worked in IT operations and security for many, many years.  I'm no Tim McGee, but I get by.

## General safe-browsing advice
* _If possible, run Linux_
* Setup your home to use OpenDNS or CloudFlare Family DNS servers
* Check for browser updates regularly, and apply them.  Keep plugins up to date as well.
* Use Anti-Virus.
* Don’t __ever__ click on links in email.
* Use a secure password, and don’t re-use passwords between sites.
* Do not save passwords within the browser. Use a password safe and subscribe to __Have I been Pwned?__
* Use SSL – Sites that use “HTTPS:” instead of “HTTP” are encrypting your traffic.
* Have a solution to monitor your bank accounts and credit reports.

## General Browser checklist
1. Do not save passwords in the browser.  Use a password safe.
2. Do not use autofill.  Sites use hidden fields to send more than you think.
3. Have the browser ask you where to save files.  This alerts you to files being downloaded and allows you to cancel or put them in a folder for latter consideration.
4. Enable click-to-play for plugins.  This will speed up the browser and allow you to decide when to use useful but potentially dangerous plugins like JavaScript, Flash, and Silverlight.
5. Do not accept third-party cookies except by exception.  Clear all cookies after each session.
6. Use Do Not Track.  This is more aimed at commercial privacy, and depends on the server honoring the request, but why not?
7. Be cautious using extensions, however there are a few that are suggested:
   * AdBlock Plus
   * HTTPS Everywhere
8. Refer to the following sections for specific help with your browser.

## ![Chrome](https://www.mozilla.org/media/protocol/img/logos/firefox/browser/logo-lg-high-res.fbc7ffbb50fd.png#floatright) Chrome
1. Settings are found under the “stacked dots” icon on the right.
2. Under “set up sync”
   * Choose “encrypt all synced data with your sync password”.
   * Uncheck Autofill and Passwords.
3. Select advanced options
4. Under Privacy, check “protect you and your device from dangerous sites” and “send Do Not Track”.
5. Under Passwords and Forms, uncheck both options (do not autofill or remember passwords).
6. Under Downloads, check “Ask where to save each file before downloading”.
7. Under Plugins, select “Let me choose when to run plugin content”.
8. Under Cookies, block third-party cookies.
9. Add recommended Extensions and remove un-needed ones.
   * AdBlock Plus
   * HTTPS Everywhere

## ![Firefox](https://p1.hiclipart.com/preview/498/1015/635/mozilla-sleek-icons-firefox-256x256-mozilla-firefox-logo-png-clipart.jpg#floatright) Firefox
1. Options are found under the “hamburger” icon on the right.  In the drop down menu, select preferences.
2. Go to _Files and Applications_ and select “Always ask me where to save files.”
3. Under _Network Settings_ select the settings button.  Choose __enable DNS over HTTPS__ and set it to Cloudflare.
3. Under _Privacy & Security_
   * Select the Standard Tracking Protection option and “Always apply Do Not Track”
   * Set third-party cookies to delete when Firefox is closed.
   * Under Logins and Passwods, disable "ask to save"
   * Under Forms and Autofill, disable Autofill
   * Under Permissions, check "Warn me when sites try to install an add-on" and "Block pop-up windows"
   * Disable Firefox data collection.
   * Under Security, choose to Block dangerous and deceptive content, block dangerous downloads, and warn about unwanted software.
7. Back to the hamburger menu and select _Add Ons_.  Remove any extensions you don't need and add these two.
   * AdBlock Plus
   * HTTPS Everywhere

## Internet Explorer
1. Don’t use Internet Explorer.

## Other browsers . . .
There are some other browsers that are marketed as "secure".  Examples include Avast, AVG, and Comodo.  My experience is that these are just custom versions of Chrome.  It's difficult to keep up with all the customizations these different groups make, but generally I find that they are based on an older version of Chromium and aren't always transparent about what changes they are making.  They tend to be updated less often and sometimes behave in unexpected ways because of the changes.  I do not recommend these today, but I'm open to the idea.

Another set of browsers attempt to compete more directly with Firefox and Chrome.  These include names like Brave and Opera.  I have a good opinion of both these options, but they are more common with power users and not really in scope of this guide.  Safari is used on Macs and is quite good.  I'll try to add it in at a later date.