---
title: "How to write an event to Windows and Linux logs"
date: 2020-10-25T16:29:57-04:00
draft: false
author: "Brent Stewart"
github: ""
youtube: ""
refs: ["https://scoop.sh/","https://github.com/lukesampson/scoop"]
tags: ["Syslog","Windows","Linux"]
---
Anyone who has trouble sleeping should discover the joys of reviewing Syslog data.  System logging contains a wealth of information that can assist in troubleshooting, security, and incident handling.  The hard part is wading through all the data to put together a useful and actionable story.  There are a wealth of tools to help us correlate and make sense these days, such as SIEM, but there are still times when we need to get into the data.

One of the first problems we encounter in understanding syslog is figuring out where to start in the stream of events.  It would be nice if there were a bookmark that we could reference.  This article is about inserting that bookmark into either Windows Event logs or Linux Journals.

## PowerShell sudo

A brief aside: The dichotomy of an admin PowerShell session and a regular PowerShell session is annoying.  One specific but near-to-my-heart example is the built-in terminal in Visual Studio Code (or VSCodium) for Windows, which uses a "non-admin" session.  

Linux systems have sudo.  Sudo allows a single command to run in an elevated state and sudo commands can be intermingled with un-priviledged commands.  The following script uses _scoop_ to grab a "sudo" application for Powershell.

> __iex (new-object net.webclient).downloadstring('https://get.scoop.sh')__  
__set-executionpolicy unrestricted -s cu -f__  
__scoop install sudo__  

## How to write an Event to Windows Event Log

This technique allows you to place comments into the Windows event logs, for instance to mark the beginning or completion of a change window.  We could also build this into certain scripts so that they left an entry when they ran.  It could even be automated into systems so that the markings took place without a person having to remember.

The general process is to create an information "source" to write into (if it doesn't already exist) and then to write the event.

### Creating a log source

Since we're creating log events for comments, let's create a log source that matches our username.  If there are several admins, it may be a good idea to use a format like "admin-username" so that we can later search logs for "admin*".  Creating a new source requires PowerShell to run with Administrator priviledges.  

> __New-eventlog -logname application -source "brent"__  

Note that you can use the environment variable _$env:username_.  You can build this into a script - if the source already exists, the command will return an error to that effect.

## Logging an entry 

To create a log entry use the "write-eventlog" cmdlet.  Specify the log (like Application or Security), the source that we defined, and the message.  The EventID isn't significant, so you could also use this numeric field if you had something suitable (like a ticket number).

> __Write-EventLog -LogName Application -Source "brent" -EntryType Information -Message "Sample text" -EventID 1__  

It is also possible to write an entry on a remote computer. The example below assumes that the source "bstewart" exists on the remote computer.

> __Write-EventLog -computername "Server" -LogName Application -Source "bstewart" -EntryType Information -Message "Sample text" -EventID 1__  

## Scripting
This can all be simplified in a script, saved as "log.ps1"
>  try  
{  
        sudo New-eventlog -logname application -source $env:username  
}  
Catch  
{  
    Write-Output "Log source already exists"  
}  
Write-eventlog -logname application -source $env:username -entrytype information -message $args[0] -eventid 1  
write-host "The following was written to the Application log using the source $env:username for that log."  
write-host $args[0]  

Then run the command from powershell to write a string.  It will try to create a source based on your username.  If one exists, it will use it and keep moving.  The argument is passed through as the message.  You could easily extend this script to have a second argument to pass the eventid as well.

> .\log.ps1 "Something happened"

# Linux (much easier)

Writing to the Linux journal is pretty straight-forward. 

> __echo 'Sample text' | Systemd-cat__  
 
To take a look at this, just use __journalctl -f__.

We'll talk about logging to syslog another time, or maybe I can talk Myron into delving in because he has great experience with pulling things out of SIEMs.  Whether you use this in the scope of a SIEM or just for local logging, I'm sure you'll find this idea useful.