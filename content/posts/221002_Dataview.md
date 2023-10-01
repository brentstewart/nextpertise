---
title: "Obsidian Dataview (Part 3)"
description: "Obsidian Dataview"
author: "Brent Stewart"
date: "2022-11-01T9:38:21-04:00"
markup: 'mmark'
math: false
draft: false
Victor_Hugo: "true"
picture: "markdown"
Focus_Keyword: "Obsidian Dataview"
youtube: ""
github: ""
refs: ["https://github.com/blacksmithgu/obsidian-dataview"]
tags:
  - "Markdown"
---

This article is the third I've written on Obsidian.  [Part 1](/posts/220829_obsidian_intro) describes the basics and [Part 2](/posts/220831_using_obsidian/) covers tasks, this article will describe using Dataview.

Core Obsidian is "just" a markdown editor with some Wiki functionality and a pretty good understanding of tasks.  The magic comes through add-ons.  The process of adding plugins is described in the second article where I discussed "Checklist", which pulled all your open tasks together on one list in a sidebar.  The Dataview plugin is also a way to collect information across notes, but much more powerful.

Once the Dataview plugin is added, you can add a dataview query into any document.  Fields can be created in any document either as YAML front matter or within the document using double-colons.
![Example job](/221101_Opty.png#floatright)
![Obsidian](/obsidian.png) 
### Let's build out an example!
At the time of this writing, I'm looking for a new job.  I've gone into Obsidian and created an "Opportunities" folder.  Each document in that folder is based on a standard template that I fill out.  I update the document as I learn more and I make "active=False" if it's not a good fit.  I should note that each document starts with these standard fields, but I add all kinds of unstructured data underneath.  I have a diary of notes from calls and follow-up tasks that starts after the fields.
```
## Untitled

active::True
Title::
Pay::
Link::

contact:: 
initialContact::
lastContact::

Onsite::
Location::

group:: #opportunity
```

Outside the folder, I have a "master view" document where I track the entire job search.  This gives me a dynamic dashboard of the opportunities I am targeting.  The first table on this dashboard document is a summary of the various opportunities.  The first field in the table is the filename and it links back to the underlying details document.
> NOTE:   Obsidian denotes sections of code by encapsulating with three tics (`).  Noting the language after the top tics will highlight based on the language.  When the Dataview plugin is present, mark the code "dataview" and it will be interpretted and replaced with the query results.  I've added a space in the bottom three tics to prevent Hugo from interpretting (and then hiding) the tics.
```
```dataview
table title,stage, pay, onsite from "Projects/Opportunities" 
where active SORT file.name asc
` ``
```
![Dashboard](/221101_dashboard.png#floatright)
I have a similar set of documents for the people I've met to track their contact information.  This query pulls in contact information from documents in the People folder that have the tag #jobhunt.

```
```dataview
table mobile,email, Last_contact 
from "People" and #jobhunt 
` ``
```

Another table in the dashboard file is a list of open tasks.  Remember that Obsidian allows you to create a task using square brackets anywhere in a document.  In my employment search, many people have volunteered to help but are not necessarily associated with a particular job.  This means that I may have a follow-up reminder associated with a contact or with a job.  Here's the query that pulls all those to-dos together.  It's worth noting that I can check off a task either in the source document or in the dataview list and it will update the other.

```
```dataview
TASK from "projects/opportunities" 
or ("People" and #jobhunt) 
WHERE !completed 
GROUP BY file.name SORT file.ctime asc
` ``
```
The second screenshot shows that I also have a dataview list of people on this "dashboard" page.  Dataview is enormously powerful and really turns building a personal database into a load of fun.

Dataview provides a pretty good exposure to NoSQL.  I have a lot of SQL experience and struggled with the concepts of "NoSQL".  Dataview uses it's own query language (called DSL), but it's easy to pick up and starts to give you a vision of what's possible in NoSQL.  A simple example here - if you built a SQL database and started populating it, it could be problematic to add a field later.  With Dataview, I could just start adding a field for "PTO" and the query would just ignore pages that didn't have that field.  Although Dataview can handle schema changes adroitly, it doesn't have any concepts of data relationships, so it's going to be fit for flat data.

Those of you with sharp eyes may notice that I have buttons on my dashboard (for instance, "New Opportunity").  I may describe setting that up in a future post, but just to not leave the question hanging I'll mention those are created using the "Buttons" and "Quick Add" plugins.  You may also notice that my folders have icons - that's done using the "Icon Folder" plugin.