---
title: "Linking and Embedding in Obsidian Notes (Part 3)"
description: ""
author: "Brent Stewart"
date: "2022-09-01T06:24:59-04:00"
markup: 'mmark'
math: true
draft: false
Victor_Hugo: "true"
picture: "markdown"
Focus_Keyword: "Obsidian"
youtube: ""
github: ""
refs:
  - https://help.obsidian.md/Obsidian/Index
  - https://help.obsidian.md/How+to/Use+callouts

tags:
  - "markdown"
---

Obsidian has been a great way to organize my work and my personal activities.  This is the third article I've written in this series.  [Part 1](/220829_Obsidian_Intro) was an introduction and [Part 2](/220831_using_obsidian) focused on _tasks_ and used that as an introduction to plugins and tags.

This entry will focus on linking and embedding.  At this point we're focusing on using Obsidian to take notes as we go.  After this, the series will turn to more advanced and non-obvious uses.

## Links
Links are central to the way Obsidian works.  I described Obsidian as part note-taking and part personal knowledge-base wiki.  Being able to quickly link articles furthers both of those aspects.  Embedding links allows quick reference to existing material so that it doesn't need to be repeated.  The links also start to create a navigational structure that allows quickly finding referenced information.

I've created a sample daily entry below that demonstrates three basic types of linking that everyone needs to be able to do.

![Links in Obsidian](/220904_Links.png)

The simplest is a straightforward link to another note in the same vault.  I've demonstrated that below with the two notes to call Bob and Alice.  Notice that two formats are supported - the double square brackets used by twiki and the markdown style used for Alice.  Twiki-style is very easy to use - just type two opening square brackets and Obsidian will drop-down a selection list of notes to link.  You can start typing a name if you need to narrow down the list.  Selecting a note and pressing enter will automatically close the brackets.

One neat wrinkle on this style link is that it can reference a point within the document.  Add a hash and then a header within the target document to the link and it will point to that location in the document.  You can see this demonstrated below in the tasks section with a link that goes to the task section of the project.

Markdown links use a single set of square brackets for the link text and paranthesis for the link.  In the case of Alice (below), the link is referencing a document in this vault and just needs the "Obsidian path".  Note the line below Alice . . . the same format is used for an external link.  A markdown link is required for external references.

```
#  Sunday, Sep 04, 2022

## Tasks
- [ ] Update [[Take over the World Project#Tasks]]

## Notes
Call [[Bob]]
Call [Alice](People/Alice)
tutorial on [MongoDB](https://www.tutorialspoint.com/mongodb/index.htm)
```

## Links provide structure
Other search engines provided an index of web pages, but Google realized the power of links for ranking results.  If a lot of _other_ websites refenced a page, then it might be interesting.  The same logic is true in Obsidian and there are two tools to help you use this information.

__Backlinks__ refer to other Obsidian notes that have links back to the note currently being viewed.  This helps to create two-way traffic bewteen notes.  I've also found it helpful to track down related notes.  Backlinks are compiled automatically by Obsidian and shown in the "action" pane to the right by clicking the left-most tab.  This is pictured below.

![Backlinks](/220904_Backlinks.png)

![Graph View](/220904_Graphview.png#floatright)
__Graph view__ is another way that Obsidian tries to help surface relationships between notes.  Graph view shows you all the notes in the vault, with linking lines to show where links exist.  The graph view on the right is from my personal vault.  I turned off the note names in this view, both to consolidate the view and to protect my privacy.  Even without labels, two "hub" nodes are apparent.

Graph view is pretty, but I don't personally find it necessary.  Backlinks, on the other hand, I use fairly frequency to remember the name of the other file.

## Embeds
Embedding is a term I'm using to describe including non-Markdown objects in an Obsidian note.  We'll talk here about including code, math, pictures, and other files in a note.

![Blockquotes](/220904_Obsidian_Blockquote.png#floatleft)
__Block quotes__ are used to pull something out of the body of the text.  These are useful for highlighting an interesting author's quote or creating a sidebar.  Block quotes are created by preceeding lines with a greater-than sign.

![Callout](/220904_Obsidian_callout.png#floatsmallright)

__Callout boxes__ are a cool way to focus attention on a part of the page.  These extend the use of quote blocks with commonly used headings and colors to draw attention to a point.  Think about a cookbook that needed to say, "Danger - don't put metal in the microwave!".

Callout boxes are built like block quotes with a header. The header is constructed from square-brackets with an exclamation mark.  The box includes everything until an empty line return, so it can be long.

```
## Callout
> [!Danger]
> Here's a danger callout
```

Obsidian supports twelve types of callouts and will automatically apply a simbol and colored header appropriately.  Those types are listed below.
-   note
-   abstract, summary, tldr
-   info, todo
-   tip, hint, important
-   success, check, done
-   question, help, faq
-   warning, caution, attention
-   failure, fail, missing
-   danger, error
-   bug
-   example
-   quote, cite

Obsidian also supports __code snippets__.  Programming should be enclosed in three tic-marks as shown below.  The language should be identified after the first set of tics - Obsidian will automatically color the text similar to an IDE.  Obsidian supports close to a hundred languages, including HTML, CSS, and Python.

```
  ```python
	print("Hello World")
  ```  #tics to open and close
```

Finally, Obsidian supports __Latex__ code in-line or blocked out.  $Latex$ included in-line should be brackted by dollar signs ($).  If the equation needs to be blocked out and centered it should use double-dollar signss.  The code for this paragraph is shown below the equation.
$$ x = {-b \pm \sqrt{b^2-4ac} \over 2a} $$

```markdown
Obsidian supports __Latex__ code in-line or blocked out.  $Latex$ included 
in-line should be brackted by dollar signs ($).  If the equation needs to be 
blocked out and centered it should use double-dollar signs.
$$ x = {-b \pm \sqrt{b^2-4ac} \over 2a} $$
```

## Pictures and other files

Pictures and other files can be embedded in an Obsidian note.  Pictures are automatically displayed as part of the note.  There are two ways to do this.  The first is to use the markdown link format.  This does not copy the image into your vault, which is both good and bad.  Linking to an external image keeps your vault smaller and cleaner, but if that object changes or is deleted then it will be gone from your notes as well.  When I use this approach, I select a picture on a web page and right-click to choose "copy link".

A second option is to copy the file into the vault and use the twiki-style link.  This increases the size and clutter in the vault.  This tecnique allows specifying size with a | bar and a width.  It also protects your notes from random changes on the Internet.  When copying, right-click and choose "copy file".  Move into Obsidian and paste where you want it.  The file will default to the root of the vault, but I usually store images in an "Archive" folder.   

```markdown
![Obsidian](https://obsidian.md/images/crafting.svg)
![[Pasted image 20220904161541.png|200]]
```

In the code above, I've limited the width of the obsidian image to 200 pixels.  I can't control the size of the linked image.

## Wrap-up
I hope this has been a useful run-through of some of the Obsidian formatting.  I'm working on further posts to get useful plugins.  Dataview is a special treat - it really blew my mind - so look for that post soon.