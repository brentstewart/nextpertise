---
title: "Obsidian Introduction (Part 1)"
description: ""
author: "Brent Stewart"
date: "2022-08-29T17:24:59-04:00"
markup: 'mmark'
math: false
draft: false
Victor_Hugo: "true"
picture: "markdown"
Focus_Keyword: "Obsidian"
youtube: ""
github: ""
refs:
  - https://fortelabs.co/blog/para/
  - https://help.obsidian.md/Obsidian/Index
  - https://www.youtube.com/watch?app=desktop&v=uqVx22lo9_4&themeRefresh=1
tags:
  - "markdown"
  - "obsidian"
---

I've kept notes using a lot of different tools over the years.  I've used Outlook, Simple Note, Visual Studio Code, and even just Notepad.  At the start of the pandemic, I switched back to writing on paper.  I like paper because the mechanical act of writing helps keep me present in a meeting, it engages my brain, and it's not distracting to the other folks in the meeting.  On the other hand, writing notes on my computer makes them easier to read (I have bad handwriting), easier to transport and easier to refer back to.

The balance has tilted toward the computer again.  In the past, my notes were passive and most of the value was the way that writing helped me remember.  I've recently started using Obsidian, which allows you to create a group of markdown documents (Simple Note or Code) and link them together (like a personal wiki).  It also has the ability to query fields across notes, and add dynamic linking that really helps surface the information you neeed.

![Obsidian](/220830_Obsidian.png#floatright)
Past systems I've used created lockin (Outlook) or involved making someone else the custodian of my private file.  Obsidian uses a local database that is simply a collection of directories and markdown text files - easy to migrate data in and out.  The project has a syncing service that helps support the developers.  You can also make a one-time [contribution](https://obsidian.md/pricing) if you find value in the program.

I'm blown away at what I can do with Obsidian and I'd like to share a part of that journey with you.  Obsidian is a blank slate and can adapt to any style you want to work in.  I'll be writing a series of articles to help you get concepts, demonstrate usage ideas, and share how I have my personal vault setup.

The application is available for Linux, Mac, Windows, IOS, and Android from [their site](https://obsidian.md).

## Obsidian Interface
Obsidian is a markdown editor that organizes notes into folders, collectively called _vaults_.  Those object map direclty to markdown files in directories.  The application is divided into three panes normally (you can open more) - the vault, the note editor, and (what I'll call) an action pane.  The vault (on the left) shows the file structure, similar to the way VSCode works.  The editor is in the middle, and the action pane presents additional features (many times from add-ins). 
![Obsidian Bar](/220830_Obsidian_Bar.png#floatleft)

The icon bar on the left (as shown in my screen shot and close up) includes several buttons.  In order, the first hides the vault pane.  Next is a table editor, present because I've added the Advanced Table Editor plugin.  Third is a button that will let you jump to a note by title, then a graph view that shows the linking relationships between notes.

The fifth icon will create a new note for today.  This is really useful if you routinely keep a file open for whatever happens in your day.  I extend this by specifying a template for my daily notes and specifying the directory they should all be kept in.  Next is a button to apply a template to a note, then a button to open the command palette.  ![Lower bar](/220830_Obsidian_LowBar.png#floatright)  The command palette allows you to enter a command instead of working through the GUI - I rarely use this, but it's useful in providing instructions.  Finally, the last icon is to publish this to an Obsidian hosted page.  I typically disable that option.

There are another three icons at the bottom of the bar - switching vaults, help, and settings.


## First note

Writing a note is easy - just click the "new note" button and start typing.  Markdown is pretty sparse - you can't specify things like fonts or headers.  This can be a plus - it allows you to focus on the content.  Refer to my previous [Markdown Cheatsheet](/posts/210424_hugo_markdown_cheatsheet) for a review of markdown syntax. 

Using the "new note" will put your file in the root of the vault.  Because I sort my notes into folders, I prefer to right-click the folder and choose new note so that it is created in the folder.

A third way to create a new note is wiki-style.  Just use double-square brackets to reference another file:

    [[new file]]

Clicking on the link will create the file and open it for editing.

## The end of the beginning

At this point you should have installed the app and completed your first file.  If this is the first time you have used Obsidian, you may be saying, "so what"?  Have patience.  The next post will get into some more advanced Obsidian usage ideas and later posts will cover dataview (!) and my organizing strategy.