---
title: "Linux Home Cloud Backup"
date: 2020-08-04T09:12:52-04:00
draft: false
Victor_Hugo: "true"
Focus_Keyword: "Backup"
author: "Brent Stewart"
github: ""
youtube: ""
refs: ["https://www.duplicati.com/", "https://www.jupiterbroadcasting.com/", "https://www.backblaze.com/"]
tags: ["Review","Backblaze","Duplicati","Backup","Cloud","Linux"]
---

At one point, I was taught to divide tasks by priority A, B, C.  As I've gotten older, I've converted that scale into "things that will immediately get me fired or divorced", "things that will eventually get me fired or divorced", and "things I'd like to do if I have time".  One of the "A" tasks on this scale is making sure that we don't lose our family digital pictures!

Our home file server is an Ubuntu VM.  Over the years, I've used a variety of strategies to maintain personal backups.  Recently, I felt the time was right to move to cloud based backup - both for the convenience and the security of having things off-site.  I considered AWS S3, but Backblaze offers a similar and less-expensive service.  A former employer used Backblaze for laptop backups and I administered that system, and I always felt they did a good job and were reasonable to work with.

![Backblaze Dashboard](/BB_bucket_setup.png#floatright)  I settled on using Duplicati for the backkup software.  Duplicati runs on everything (Linux + various less secure OS), has a DEB, and is FOSS.  Duplicati has built in support for cloud backup, including Backblaze and S3.  I have a friend that uses Duplicati and it's discussed on Jupiter Broadcasting, so I wanted to give it a try.

Let's start with Backblaze.  I setup an account and configured it for 2FA.  Then I created an ID and application key at the Backblaze dashboard and setup a bucket.  You can specify the bucket policy, and I recommend keeping older copies to protect against crypto-locking malware.  I setup my bucket to retain older copies for 180 days.

Setting up Duplicati is as easy as installing the DEB and enabling the app to autostart.  My server runs Mate, so I opened the Control Center (alt--f2, "mate-control-center") and added Duplicati to the autoruns (at the bottom of the control center window).

![Mate Autorun](/Mate-autorun-Duplicati.png#floatleft) Once running, Duplicati shows a menu-bar applet. The application is administered from a web page on port 8200.  This webpage can be accessible from other machines and I usually manage the backups from my desktop.  Duplicati has excellent documentation on their website, but I was able to get it up and running quickly without investing a lot of time.

From the initial Duplicati page choose "Add backup" and a wizard will walk you through specifying the details.  Make sure you keep track of the passphrase used by the backup!  Here's a quick rundown on the selections I used:
* General - AES-256 Encryption.  Other options are no encryption and GNU Privacy Guard.  I don't protect nuclear secrets, but I get itchy not encrypting data at rest, so I definitely don't recommend that option.  I don't know much about GNU PG, but AES-256 is considered a solid and well researched encryption so I used it.
* Backup destination - this is where you'll plug in the ID and Key you generated at Backblaze earlier.
* Source data - gives you a file tree to select what you'd like to backup.  I'm cheap, so I separated out the non-private stuff into another directory (like installation ISOs) so I didn't pay to back them up.
* Schedule - We'll get into a discussion of RTO and RPO another time perhaps.  Basically, think about your cost to transfer files (with Backblaze, there is no incremental cost) and how much data you are willing to lose between backups.  I setup my schedule to run every night - with Backblaze there's not really a reason _not to_.
* Options - Duplicati allows you to set the remote volume size.  I kept this at the recommended 50Mb.  Basically, it chunks your data so that it's easier to restore and so that an adversary can't identify individual file sizes, which could be a way that you'd leak information.  I also chose to keep all backups, again to protect against crypto-lockers.

![Duplicati Wizard](/Duplicati_Wiz.png#floatright) After all that, just let it run!  My biggest knock on the combination of Duplicati and Backblaze is that there's not an easy way to confirm backups are happening.  Backblaze has a 10 day trial and I didn't initially put in a credit card.  To be clear, kudos to them for letting you try it and being easy to work with.  But . . . I forgot and the trial ran out and my backups stopped for several days.  Worse, I was clueless.

Duplicati has options to setup a confirmation email after backups, which I recommend.  You'll know there's a problem when you _don't_ get an email.  Unless you are more clever than me, that's suboptimal but it is something.  Backblaze doesn't have an alerting option for things that don't happen.  I'm thinking that I could setup a Lambda to check via API, then send an SNS, but that's for another day.

Overall, I'm please with the setup and the results I'm getting and would recommend either component to someone trying to solve a similar home problem.  I don't see a reason this wouldn't be good for a work environment, but I'll need to use it for a while before I feel comfortable making that a recommendation. 