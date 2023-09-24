---
title: "Obsidian Tasks (Part 2)"
description: ""
author: "Brent Stewart"
date: "2022-08-31T06:24:59-04:00"
markup: 'mmark'
math: false
draft: false
Victor_Hugo: "true"
picture: "markdown"
Focus_Keyword: "Obsidian"
youtube: ""
github: ""
refs:
  - https://help.obsidian.md/Obsidian/Index
  - https://help.obsidian.md/How+to/Working+with+tags
tags:
  - "markdown"
---

In [Part 1](/posts/220829_obsidian_intro), I introduced you to Obsidian.  Obsidian is a markdown-based note taking application that is supported on most desktop and phone operating systems.  That article describes the basic interface and usage of Obsidian.  If you are not familiar with Obsidian, it's a good place to start.  

Part 2 will focus more on tasks within notes.  We'll use tasks to also introduce plugins and tags.   Future articles will discuss links, embeds, tags and some of the other plugins that add functionality, such as Dataview.

## Tasks
![Obsidian Tasks](/220831_Obsidian_Tasks.png#floatright)
In the last article, we discussed the basics of creating a note using Obsidian.  One of the first ways that Obsidian starts to differentiate itself from other text-based note systems is the way it incorporates tasks.  Creating a task simply involves starting a line with space seperated dash and square brackets (_the spaces are required_).  In the snippet below I've demonstrated how this might look.  Obsidian recognizes the pattern as a checkbox and presents it as a graphical box as shown in the screen capture to the right.

```
## Task List #todo
- [ ] Write article
- [ ] Post article
```

## Checklist Plugin
Notice that I've tagged this list with _#todo_.  That is necessitated by a plugin I use - __Checklist__.  This also presents a chance to introduce the usage of plugins and tags.  The tag will be used by the plugin to generate a consolidated list of open to-dos across all documents.

To configure plugins, select the gear button (bottom left).  Plugins are grouped as "Core" - the ones included with the application and supported by the developer - and "community" - those that are created by third parties and made available to other Obsidian users.  Checklist is a community plugin, so step one is to go to the Community Plugins page in options and Turn Off restricted mode.  This will allow you to browse and install community plugins.

![Configuring Plugins](/220831_Obsidian_Plugins.png#floatsmallright)

Browsing plugins gives you a _huge_ list.  There are 637 plugins listed in August of 2022!  There's relatively little danger in just scrolling through the list and trying some.  For this discussion, let's stick to __Checklist__ to understand how to install and configure a plugin.  You can find a specific plugin by scrolling or by typing the name or a keyword into search. Once you find it, click the install button and then the enable button.  Typing "Checklist" gives a single possibility.

> Using plugins requires that they be downloaded (installed) and then enabled.  If you forget to enable, just go back into options.  Scroll down to the bottom where Community Plugins are listed and they can be enabled from there.

![Configuring Plugins](/220831_Obsidian_Checklist.png#floatleft)
![Configuring Plugins](/220831_Obsidian_Tasklist.png#floatleft)
![Working with tags](/220831_Obsidian_Tags.png#floatleft)

Once the Checklist plugin is installed, you'll be able to configure it under options.  Select the gear icon (lower left) to get into options and scroll down to the bottom.  You'll find a header called "community plugins" and the Checklist plugin should be under that.  Notice that my screen shows several plugins, while you may only have Checklist to this point.

## Tags to build our Checklist

Check four settings while here.  First, multiple tags can be specified to pull in tasks.  You might have #honeydo and #worktasks for instance.  In this case, a generic #todo tag is specified.  Some other settings to check (to get this to work the way one might intuitively expect) are Show Completed - OFF, Group By - Tags, and Auto-Refresh ON.

Once the plugin is configured, create a couple notes with tasks.  Remember to tag the tasks using a tag you defined in the Checklist seutp.  In the example screenshot, I took our sample list above and named the note "Sample 1".  I created a second list and named it "Sample 2".

The vault pane shows that I'm editing "Sample 2".  You can see the new note defined in the editing pane.  Because the checklist plugin is installed, the "action" pane on the right should now have a "check" tab (I've circled it in the example). Selecting this tab will show a consolidated list of incomplete tasks.  Marking an item complete in the pane will remove it from the list and mark it complete in the original document.  Similarly, you can complete tasks in the document and the update will carry over to the checklist sidebar. 

Tags are used by Obsidian to group things - in this case checklist.  They can also be used to group notes.  To see where we have tags defined, click the hashmark tab in the right "action" pane.  My example vault, built to demonstrate these concepts, has three tags in use: #todo, #evil, and #work.
Choosing _todo_ changes the left pane to show us which notes contain that tag and allow us to quickly jump to related entries.

## Scratching the surface

At this point, we have [introduced](/posts/220829_obsidian_intro) Obsidian as a [markdown](/posts/210424_hugo_markdown_cheatsheet)-based note taking tool. Obsidian has great tools to help manage tasks, and we've demonstrated the basics of using tasks.  Finally, we've started to cover the concepts of plugins and tags.

There is a lot more to discuss!  Once I started using Obsidian, my use cases grew and the way I built notes started to fundamentally change.  I'm very excited by the possibilities.  I'm equally excited to share the journey.  