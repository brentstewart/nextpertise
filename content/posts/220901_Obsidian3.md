---
title: "Using Obsidian (Part 3)"
description: ""
author: "Brent Stewart"
date: "2022-09-01T06:24:59-04:00"
markup: 'mmark'
math: false
draft: true
Victor_Hugo: "true"
picture: "markdown"
Focus_Keyword: "Obsidian"
youtube: ""
github: ""
refs:
  - https://theproductiveengineer.net/the-beginners-guide-to-obsidian-notes-step-by-step/
  - https://notes.nicolevanderhoeven.com/Obsidian+Dataview
  - https://mermaid-js.github.io/mermaid/#/gantt?id=styling
  - https://fortelabs.co/blog/para/
  - https://www.sitepoint.com/obsidian-beginner-guide/
  - https://help.obsidian.md/Obsidian/Index
  - https://help.obsidian.md/How+to/Working+with+tags
  - https://www.zylstra.org/blog/2020/10/100-days-in-obsidian-pt-1/
  - https://www.youtube.com/watch?app=desktop&v=uqVx22lo9_4&themeRefresh=1
  - https://thesweetsetup.com/our-favorite-obsidian-plugins/
  - https://medium.com/produclivity/my-3-favourite-obsidian-themes-and-most-useful-plug-ins-e130aba1103a
  - https://medium.datadriveninvestor.com/the-only-10-obsidian-plugins-i-need-to-have-the-best-and-simplest-pkm-of-my-life-c3a3b9d1ebda
  - https://fortelabs.co/blog/para/
  - https://blacksmithgu.github.io/obsidian-dataview/query/queries/
tags:
  - "markdown"
---

In [Part 1](/220829_Obsidian_Intro), I introduced you to Obsidian.  Obsidian is a markdown-based note taking application that is supported on most desktop and phone operating systems.  That article describes the basic interface and usage of Obsidian.  If you are not familiar with Obsidian, it's a good place to start.  

Part 2 will focus more on some the ways that notes are constructed in Obsidian.  This article covers the basics of tasks, links, embeds, and tags.  Future articles will discuss some of the specific plugins that add functionality, such as Dataview.

## Tasks
![Obsidian Tasks](/220831_Obsidian_Tasks.png#floatright)
In the last article, we discussed the basics of creating a note using Obsidian.  One of the first ways that Obsidian starts to differentiate itself from other text-based note systems is the way it incorporates tasks.  Creating a task simply involves starting a line with space seperated dash and square brackets (_the spaces are required_).  In the snippet below I've demonstrated how this might look.  Obsidian recognizes the pattern as a checkbox and presents it as a graphical box as shown in the screen capture to the right.

```
## Task List #todo
- [ ] Write article
- [ ] Post article
```

Notice that I've tagged this list with _#todo_.  I recommend adding the __Checklist__ plugin, and this also serves to introduce the usage of plugins.

To configure plugins, select the gear button (bottom left).  Plugins are grouped as "Core" - the ones included with the application and supported by the developer - and "community" - those that are created by third parties and made available to other Obsidian users.  Checklist is a community plugin, so step one is to go to the Community Plugins page in options and Turn Off restricted mode.  This will allow you to browse and install community plugins.

![Configuring Plugins](/220831_Obsidian_Plugins.png#floatsmallright)

Browsing plugins gives you a _huge_ list.  There are 637 plugins listed in August of 2022!  There's relatively little danger in just scrolling through the list and trying some.  For this discussion, let's stick to __Checklist__ to understand how to install and configure a plugin.  You can find a specific plugin by scrolling or by typing the name or a keyword into search. Once you find it, click the install button and then the enable button.  Typing "Checklist" gives a single possibility.

> Using plugins requires that they be downloaded (installed) and then enabled.  If you forget to enable, just go back into options.  Scroll down to the bottom where Community Plugins are listed and they can be enabled from there.

![Configuring Plugins](/220831_Obsidian_Checklist.png#floatleft)

Once the Checklist plugin is installed, you'll be able to configure it under options.  Select the gear icon (lower left) to get into options and scroll down to the bottom.  You'll find a header called "community plugins" and the Checklist plugin should be under that.  Notice that my screen shows several plugins, while you may only have Checklist to this point.

Check four settings while here.  First, multiple tags can be specified to pull in tasks.  You might have #honeydo and #worktasks for instance.  In this case, a generic #todo tag is specified.  Some other settings to check (to get this to work the way one might intuitively expect) are Show Completed - OFF, Group By - Tags, and Auto-Refresh ON.






	Markdown
		checkboxes
    
## Links
		links to heading  
		external links
		backlinks

## Embeds
	Embeds to pics, audio/video, pdfs, etc.
		Graph
	Latex
$$ x = {-b \pm \sqrt{b^2-4ac} \over 2a} $$
	code blocks
```python
	print("Hello World")
```

## Tags
	Tags (#tag, tag pane)