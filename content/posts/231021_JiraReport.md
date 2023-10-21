---
title: "JiraReport"
description: "A Quick program to produce reports from Jira Cloud"
author: "Brent Stewart"
date: "2023-10-21T11:47:23-04:00"
markup: 'mmark'
math: false
draft: false
Victor_Hugo: "true"
picture: "python"
Focus_Keyword: "Python Jira"
youtube: ""
github: "https://github.com/brentstewart/jirareport"
refs: ["https://python-docx.readthedocs.io/en/latest/"]
tags:
  - "python"
---
I recently had a lot of fun building a quick program to pull data from Jira Cloud and produce Word reports.  I've shared [jirareport](https://github.com/brentstewart/jirareport) at Github if you are curious.  If you use Jira, this may be of interest (although most folks who use Jira are well beyond my meager abilities).  

Even if you don't use Jira, this was an interesting experience.  It was a fairly simple re-introduction to Python, something that I've tried to get started in on-and-off over the years.  Seeing it work though was a huge feeling of empowerment and encourgagement, something I hope you are able to experience (whatever cloud resource you need to use).

## Using my tools
My project contains two Python 3 programs - __jirareport__ and __lsFields__. To run these programs, you'll need the following information:

* Jira Cloud URL (like https://yourcompany.atlassian.com)
* Your Jira username (like me@mycompany.com)
* Your Jira API Key.

If you don't have an API key already
1. In Jira, click your picture in the upper right and choose manage account
2. In the profile page, choose the Security tab on the top.
3. Under Security, go to Create and manage API tokens

### lsFields

This program will take the above input and produce a list of fields and field IDs used in your instance of Jira Cloud. In our instance, we've added fields and the ID shows as customfield_00000. I'm positive that you'll want to change the fields I print in the report, since some of mine are custom. Use this tool to list the fields in your instance and replace the fields that I use.

### JiraReport

This is the one that does the work. JiraReport will take the above inputs plus a JQL query to produce a report. I use the Filters page in Jira Cloud to produce and test my queries as needed, then just paste JQL into the report. Finally, it will ask for an output filename. If you press enter, it will just use jira_report.docx.

The report includes story keys that are hyperlinks back to the story in Jira Cloud. The Assignee field also displays the persons name and is a hyperlink to their email address.

## Developing

This is my first really useful program in a few years, since I wrote a program to parse SSH logs a while back.  There are a few things I wanted to note about the experience that may be helpful for you.

First, ChatGPT was mostly useful.  General web searches came up with a lot of different snippets, but just asking ChatGPT to write the program gave me most of the bones.  I asked ChatGPT for a program to access Jira, for instance, then I asked it for a program to output Word files.  I relied on my hackery to stitch the pieces together.  As I encountered issues, I'd ask ChatGPT something like, "The above is great, but how would I do it in _yellow_?"

This is probably significant for this point in time (2023).  A few years ago, places like Stack Overflow were reliably the best places to work through coding issues.  Stack Overflow has become, over time, a less forgivning environment and some questions result in contradictory solutions.  

Vendor documentation used to be filled with examples as well, but I didn't find what I needed from Jira.  I used the open-source Python-docx library and it's great, but I really struggled with understanding how to apply what I found in their documentation.  I don't want that to sound like complaining - I am grateful for the volunteers at Python-docx and the great tool that their providing to the community.  As a beginner, I just found it hard to get started.  I found ChatGPT to be a useful way to bridge the gap in both cases.

Second, ChatGPT has it's limits.  At certain points, ChatGPT gave me code that just didn't work and I was off to Lemmy or Reddit.  I found that ChatGPT was a pretty good place to start, but you need to be prepared to drop it if you realize it's giving you junk.

The Python-docx library is a huge gift.  There are some places where I had to spend some time expirimenting to get it to do what I wanted, so be prepared to do some tinkering.  I've got some good examples in the code that you may want to look at for other projects.  For example, the process of building links took some time to put together, especially when those links are part of a paragraph.  

Finally, my users don't always fill out all fields and the program would fail if the linking information wasn't present which led to the try/except bracketing.  That was a really good lesson in graceful error handling.