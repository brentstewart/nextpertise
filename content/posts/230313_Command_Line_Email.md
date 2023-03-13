---
title: "Sending Email through Google from the Command Line"
description: ""
author: "Brent Stewart"
date: "2023-03-12T18:22:33-04:00"
markup: 'mmark'
math: false
draft: false
Victor_Hugo: "true"
picture: "linux"
Focus_Keyword: "Bash Email"
youtube: ""
github: ""
refs: ["https://wiki.archlinux.org/title/SSMTP","https://linuxhandbook.com/linux-send-email-ssmtp/"]
tags:
  - "linux"
  - "shell"
---

In the course of another project, I recently worked out how to send email through Gmail from the Linux shell.  This is both a really cool and powerful tool as well as something I could see incorporating into a lot of future work.  Since it had such utility, I wanted to document the process for myself and share that with you.

Some Linux operations, such as cron, will send output to the local mail spooler.  Files sent this way end up in /var/mail/$USER or /var/spool/mail/$USER.  Sendmail can be configured as well so that the output goes to a public email address, however running Sendmail involves a more complication and overhead.  For instance, mail coming from an SMTP server has to be trusted by the receiver and a lot of places (O365, Gmail, etc) don't trust random SMTP servers that pop up - for good reason.

SSMTP is a program that takes this "local mail" and sends it to an external SMTP system.  It can be configured to work with any SMTP server, but I use Google Mail and so that's the example I'll walk through.

## Installation

SSMTP doesn't have a facility to handle two factor authentication, so before you begin you'll need to generate an app-password at Google.  Log into your Google account, use the menu icon (3x3 squares) to choose "account", and 2-step Verification.  App Password setup is at the bottom of the 2FA screen.  To create a new app password, specify the app (I used "Linux") and device and choose generate.  You have to copy the password shown - it will never be displayed for you again!  If you forget it, you'll need to follow this procedure to delete the forgotten app password and create a new one.

On Ubuntu, install ssmtp using apt (no PPA needed):

    sudo apt install ssmtp

Edit the SSMTP configuration file.

    sudo nano /etc/ssmtp/ssmtp.conf

My configuration file is shown below and is verified working.  You'll need to change the email address to your gmail address and change the AuthPass line based on the app password you generated earlier.

    AuthUser=YOU@YOURCOMPANY.COM
    AuthPass=YOURAPPPASSWORD
    FromLineOverride=YES
    mailhub=smtp.gmail.com:587
    UseSTARTTLS=YES
    FROM:YOU@YOURCOMPANY.COM

## Testing

Once this is complete, an easy way to test is to pipe something to ssmtp as shown below.  In the first example, I'm just sending some text.  The trailing email address will be used as the "to" address.  Leaving off the from address (-au option) will result in a bcc: to test@yourmachine, which will be bounced by Google and give you a successful message (the "to" line) and a bounce message (from the bcc).

    echo "Hello E-mail!" | ssmtp -au YOU@YOURCOMPANY.COM -s "Test" YOU@YOURCOMPANY.COM

Once you understand how this works, you can redirect or pipe any output this way.  Here's another simple example that sends a directory output to email.

    ls | ssmtp -au YOU@YOURCOMPANY.COM -s "Test" YOU@YOURCOMPANY.COM

I'll consider as I add devices and services into my home network and lab.  One of the immeadeate ideas that pops up is that I'd like my backup job to let me know it completed successfully.  It's a good basic tool to have in the admin tool bag!