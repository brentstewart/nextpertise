---
title: "Command Line Browser Carbonyl"
description: ""
author: "Brent Stewart"
date: "2023-03-05T21:54:31-05:00"
markup: 'mmark'
math: false
draft: false
Victor_Hugo: "true"
picture: "shell"
Focus_Keyword: "Carbonyl"
youtube: ""
github: ""
refs: ["https://fathy.fr/carbonyl", "https://github.com/fathyb/carbonyl"]
tags:
  - "Linux"
  - "Windows"
  - "shell"
---

Not sure where to classify this discovery - Carbonyl is a shell-based brower that is available for Linux, Mac, and Windows.  Carbonyl is built on a Chromium engine and does not support plugins at this point or tie into an existing Chrome installation.

Carbonyl is fast and it produces a low-res but usable web page.  It is surprisingly responsive - there's a demo of someone playing Doom using it and I watched some Youtube using it.  That's a little hard to visualize, so you may just have to try it.

Here's Carbonyl producing a portion of this site: ![Carbonyl](/230305_Carbonyl.png)

Carbonyl is easy to install - just go to the Github assets and grab the version for your OS.  Here's the [current version](https://github.com/fathyb/carbonyl/releases/tag/v0.0.3) as of early March 2023.  It extracts to a single file and it can be executed from the command line similar to this example.

  ./carbonyl https://www.nextpertise.net

Use Control-C to exit the browser experience.  In the meantime, the mouse can be used to click through links and it interacts exactly like Chrome would.

### Why?

So, who cares?  Certainly, the Carbonyl experience doesn't match a full browser in terms of resolution or functionality.  I think there are two use cases that are worth considering.  The first is as a demonstration - if a full browser can be supported from the command line, what else is possible?  Are there modalities where web content could be used on the command line?  One possibility that occurs to me is _man_ pages.  Imagine if the result of typing "man ls" was a set of linked hypertext, formatted to present cleanly in the shell!

Carbonyl conceivably has some current advantages as well.  It's a single stand-alone binary, so shouldn't be subject to dependencies or system limitations.  It's small and easily downloaded, installed, and executed.

Carbonyl doesn't render exactly like a regular browser - I am currently using it to check the rendering of my page and you can't trust the layout and CSS seems to be mostly hit but some miss.  I'm unclear if a seperate Chrome binary is good or bad from a security standpoint.  Is it a new attack surface or is it a sand-box to play in?  My gut is to stick it in a container to isolate it from the system.

As a curiousity, it's interesting.  I can see where there are certain jobs where this could be the right tool.  I'm most bullish on how this could be coupled with pages designed for text-rendering to improve the command line experience.