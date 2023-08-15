---
title: "Automaticly adding Hugo articles to Obsidian"
description: ""
author: "Brent Stewart"
date: "2023-03-13T18:08:17-04:00"
markup: 'mmark'
math: false
draft: false
Victor_Hugo: "true"
picture: "linux"
Focus_Keyword: "Linux cron Obsidian Hugo"
youtube: ""
github: ""
refs: [""]
tags:
  - "markdown"
  - "linux"
  - "shell"
  - "Hugo"
---
## Incorporating blog articles in Obsidian

Most of my writing occurs in two places, but I'd like to consolidate it into my "second brain".

My blog articles are written in Visual Studio Code, in markdown so that they can be compiled via [hugo](/posts/210102_hugoafter6).  I've discussed in this blog that Obsidian - also in Markdown -  is setup as my "second brain".  My blog posts represent an important part of that "second brain", so I'd like to make sure they're included in my vault.  Since both are markdown, this should be fairly simple.  This should just require that I copy my hugo content directory into my obsidian directory.  To test this, I made a quick script.

    cp ~/git/nextpertise/content/posts/*.md ~/2nd\ Brain/Nextpertise/
    echo "blog2obs.sh ran"

I also made this file executable.

    chmod +x blog2obs.sh

This works as expected.  The markdown files are copied from my hugo git directory into my Obsidian vault under the "Nextpertise" folder.  Obsidian actually updates dynamically as soon as the files are present.  There are some issues - my website has a directory for graphics that I'm not copying over, for instance, and the internal linking and tagging I expect in Obsidian wouldn't be present in these files.  External links in the posts seem to work as expected though.  All I need to do is remember to run this occassionally . . .

## Automatically
So the next step was to have this run as a _cron_ job.  On Linux, use __crontab -e__ to setup the job.  Below is the way I have this setup and working.

    MAILTO=MYEMAILADDRESS
    0 0 * * * /home/brent/blog2obs.sh
![It works!](/230314_Linux_Email.jpg#floatright)
The five variables before the job are minute, hour, day, month, and day of the week.  So my entry is to run at 00 minutes and 0 hour (midnight) every day.  Cron defaults to outputting to system mail, but I use the MAILTO entry and SSMTP to redirect this to a public email address.  For more information on how to set this up see my [last post](/posts/230313_command_line_email/).

The received email is on the right, and now it's clear why the _echo_ command is in the batch file.  The echo provides some text - without that text there's no output from the script and nothing to email.

There are some possible improvements that might make this worth revisiting in the future.  The simple script doesn't indicate if there was an error copying the files.  I could also imagine inserting links and tags that are used in my Obsidian vault as a header to the imported files.  However, this is a straight-forward process and it's meeting my immediate need.