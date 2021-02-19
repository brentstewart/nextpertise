---
title: "VHEditor - Visual Studio Code for Android"
date: 2021-02-18T20:07:38-05:00
draft: false
author: "Brent Stewart"
Victor_Hugo: "true"
Focus_Keyword: "Visual Studio Code"
picture: "coding"
github: "https://github.com/vhqtvn/VHSCode-Android"
youtube: ""
refs: ["https://play.google.com/store/apps/details?id=vn.vhn.vsc&hl=en_US&gl=US","https://simplenote.com/","https://github.com/cdr/code-server","https://vscodium.com/"]
tags: ["Coding","Review"]
---
These days I live in VSCode.  I confess to having developed a hardened opinion of Microsoft in years past, but I have been impressed with their work in recent years.  O365 runs like a champ in a browser.  Teams is a really well done application, which I've come to _prefer_ over Zoom and (especially) Webex.  But VScode is in a league of it's own.  I didn't know how much I needed a really well done IDE, but it's become a must-have.

I use VSCode for writing this blog.  There are extensions that make working with MarkUp easy and that facilitate uploading to Github.  I use the built-in terminal to execute Hugo commands while I'm working (like _hugo server -D_ to preview articles in a browser).  The workspace allows me to move files around or quickly move between files for comparison or editing.

I also use VSCode to write Python (again, with some great extensions).  The biggest surprise is that I've moved my notes into VSCode.  I sync my notes to a private Github repo, so that I can make them available on whichever machine I'm working with.  I used to use Simplenote for this, and there's nothing wrong with Simplenote.  But by handling this in VSCode I can consolidate tools.

![VHEditor](/vheditor.png#floatright)
I am going to do some traveling and it occurred to me that I didn't want to lug a laptop.  Wouldn't it be _cool_ if I could use my Kindle HD for VSCode?  I already travel with the Kindle for books, and VSCode would fit in the 8" display.  It seemed possible, but looking through the Google Play store didn't turn up a Microsoft VSCode app.

That's when I stumbled on VHEditor.  But the story goes a little further back to two other projects I've been watching with interest: VSCodium and Code-Server.

VSCode is mostly open source, with some "special" parts added onto the FOSS bits.  Microsoft add telemetry, the gallery, a logo and other pieces.  VSCodium takes the open pieces and compiles a clean version with no telemetry.  The downside is that Microsoft prohibits clones from accessing the VS Code Marketplace, which means that some extensions you need aren't available.  An open Marketplace is available at [open-vsx.org](https://open-vsx.org), but not all extensions are published to that service yet.  For me, the Github extension I use is an issue, so I currently use the original MicroSoft version.  But I like VSCodium and have used it a lot.

Code-Server takes the same source code and produces a web version.  I don't have a use-case for Code-Server currently, but it seems like a fascinating idea.  All of this brings us back to VHEditor.

![VHEditor in action](/vheditorpic.jpeg#floatleft)

VHEditor takes Code-Server and Termux and creates a space in the Android OS for this to run.  There's a terminal that works and code-server is running locally.  Basically, when you run VHEditor, it starts Code-Server and accesses it via a loopback address.  VHEditor runs pretty well on my Kindle HD - paired with a small bluetooth keyboard, I can definitely use this to write notes and blog posts.  I have cloned multiple repositories down to the Kindle.

It feels like there are some kinks to work out.  You use "pkg install git" on the terminal to setup Git (there's a note about this on GitHub), and that format makes me wonder if it's running in a BSD container.  Also, the first repo I cloned worked straight from VHEditor.  The second required me to __git clone__ from the shell.  Still, the result is not only usable but useful.

I've expressed the opinion that the secret sauce in IT right now is Git + Python + WebAPIs.  VSCode has become this tool that fits into that mix of products perfectly.  VHEditor allows me to bring that all the way down to my tablet and is definitely worth a look.

If you are familiar with VSCode, you know there's a lot of things on the screen.  I have a large phone, but I just don't see this being useful on a display below 8".  You are also really going to need a bluetooth keyboard, especially to use any of the key combinations.  The app is currently sitting at 3 1/2 stars - I read through the reviews and the issues I see relate to unfamiliar or less technical users.  Be prepared to put a little thought into it, but I recommend checking this out.



