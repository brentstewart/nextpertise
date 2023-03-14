---
title: "230313_Blog2Obsidian"
description: ""
author: "Brent Stewart"
date: "2023-03-13T18:08:17-04:00"
markup: 'mmark'
math: false
draft: true
Victor_Hugo: "true"
picture: ""
Focus_Keyword: ""
youtube: ""
github: ""
refs: [""]
tags:
  - ""
---
Most of my writing occurs in two places.  My blog articles are written in Visual Studio Code, in markdown so that they can be compiled via [hugo](/210102_hugoafter6).  I've discussed in this blog that Obsidian - also in Markdown -  is setup as my "second brain".  My blog posts represent an important part of that "second brain", so I'd like to make sure they're included in my vault.  Since both are markdown, this should be fairly simple, just copy my hugo content directory into my obsidian directory.  To test this, I made a quick script.

    cp ~/git/nextpertise/content/posts/*.md ~/2nd\ Brain/Nextpertise/
    echo "blog2obs.sh ran"

This works as expected.  The markdown files are copied from my hugo git directory into my Obsidian vault under the "Nextpertise" folder.  Obsidian actually updates dynamically as soon as the files are present.  There are some issues - my website has a directory for graphics that I'm not copying over, for instance.  Links seem to work as expected though.  All I need to do is remember to run this occassionally . . .

So the next step was to have this run as a _cron_ job.  On Linux, use __crontab -e__ to setup the job.  Below is the way I have this setup and working.

    MAILTO=brent@stewart.tc
    0 0 * * * /home/brent/blog2obs.sh

The five variables before the job are minute, hour, day, month, and day of the week.  So my entry is to run at 00 minutes and 0 hour (midnight) every day.  Cron defaults to outputting to system mail, but the MAILTO entry redirects this to a public address.  For more information on how to set this up see my [last post](/230313_Command_Line_Email).