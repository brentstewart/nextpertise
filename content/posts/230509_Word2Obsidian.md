---
title: "Word to Obsidian with a DIY CI"
description: "A simple Ci-like process to send word files to Obsidian"
author: "Brent Stewart"
date: "2023-05-09T21:20:24-04:00"
math: false
draft: false
Victor_Hugo: "true"
picture: "pandoc"
Focus_Keyword: "linux obsidian"
youtube: ""
github: ""
refs: ["https://www.writage.com/", "https://pandoc.org/", "https://www.linuxjournal.com/content/linux-filesystem-events-inotify"]
tags:
  - "linux"
  - "markdown"
  - "pandoc"
  - "CI"
  - "shell"
  - "obsidian"
---

I use Obsidian as a note taking journal, but I get a lot of documents in other formats that I'd like to include in that journal.  One example is Word docs, such as my weekly reports.  I've copied some PDFs into my Obsidian vault, but for some reason I hit on the idea of converting DOCX to Markdown.

## What Didn't Work

Just to save you time, I'll mention a few ideas that I tried and discarded on the way.  There is a plugin to save files from Word in Markdown called [Writage](https://www.writage.com/).  It's $29, but a trial version is available.  I'm obstinately opposed to closed source and I'm feeling less and less comfortable about downloading and installing EXEs and MSIs from random websites, so I haven't tried it.

I also found an old github repo that purported to address this issue.  That project has pivoted to HTML and deprecated the markdown code.

## The beginning of an idea
Looking for a FOSS solution lead me back to Pandoc.  Long, _long_ time readers may recall one of my early [experiments](/posts/200919_pandoc_improved/) with Pandoc.  [Pandoc](https://pandoc.org/) is a file converter and will handle conversions between things like DOC, EPUB, PDF, and HTML.  I setup a continuous integration (CI) pipeline using Github actions so that I uploaded some markdown files and they were automatically assembled and formatted as chapters into a PDF book.  That was a cool project, and perfect for maintaining SOPs, but a cloud solution seems like a lot of steps to get this into my Obsidian vault.

I took a moment to confirm that pandoc will do the conversion I wanted.  After a little back and forth, here's the command I came up with.  I've tested this with business memos and it worked fine.  I haven't tried it with complex tables or graphs.  -f and -t are the from and to formats, -o is the output and the first file in quotes is the input.  The wrap command prevents pandoc from setting line length to 72 and adding a line return in every line.

    pandoc -wrap=none -f docx -t markdown "test.doc" -o "test.md"

## Automating markdown conversion and ingestion
Pandoc gets the conversion, but I really don't want to have to remember that command and then move files around.  I want something that is a DIY pipeline to go from DOCX to Markdown.  Here's how I did it - explanation to follow.

    #!/bin/bash
    TARGET=~/doc2obs/
    PROCESSED=~/Downloads

    inotifywait -m -e create -e moved_to --format "%f" $TARGET | while read FILENAME
    do
      echo Detected $FILENAME
      pandoc -wrap=none -f docx -t markdown "/home/brent/doc2obs/$FILENAME" -o "/home/brent/doc2obs/$FILENAME.md"
      echo converted to Markdown
      rm "/home/brent/doc2obs/$FILENAME"
      echo removed doc file
      mv "/home/brent/doc2obs/$FILENAME.md" "/home/brent/2nd Brain/Notes/$(date +%y%m%d)_$FILENAME.md"
    done

I created a directory (doc2obs) and created a watcher shell script.  It waits for a DOCX file to be copied into _doc2obs_.  When that occurs, it converts the file into markdown, deletes the DOCX, and renames and moves the markdown file.  Of course, the script needs to be executable.

    chmod +x watch-doc2obs.sh

Let's take that script step by step and explain a little more.

    TARGET=~/doc2obs/
    inotifywait -m -e create -e moved_to --format "%f" $TARGET | while read FILENAME
    do
      ...
    done

This batch of done defines the directory to be monitored.  If your Linux of choice doesn't have __inotify__, it can be loaded using yum or apt as inotify-tools.  -m tells it to monitor, -e defines the events to be monitored.  You can notify on a variety of events.

{{< bootstrap-table table_class="table table-responsive table-hover" thead_class="table-info" caption="Table of inotify events" >}}
|  |  |  |
| --- | --- | --- |
| access |	create |	move_self |
| attrib |	delete |	moved_to |
| close_write |	delete_self |	moved_from |
|close_nowrite |	modify |	open |
| close |	move |	unmount |
{{</bootstrap-table>}}

The __echo__ commands are present for debugging.  Note that the __mv__ moves the markdown file into my Obsidian vault and names it.  My daily notes all start with a date prefix like 230510 (two digits for year, month, and date), so the date command embedded in the move automatically prefixes the markdown file with the current date in the correct format.

## Automating the script

So the script is ready.  I can run it and it will monitor the _doc2obs_ directory until I stop it or reboot.  The next step is to make this into something that just runs automatically, so I don't have to open a shell and worry about restarting it.

Here I'll refer back to the process I used in [Automatically adding Hugo articles to Obsidian](/posts/230313_blog2obsidian/), which is to use __cron__.  That script ran periodically and this one runs continuously, so we'll modify the approach to ask __cron__ to run it once at startup.

    crontab -e  # gets us into edit mode
    # add below entry
    @reboot /home/brent/watch-doc2obs.sh

## Things to fix
This does what I need it to do, but I have a few ideas about how it could be improved.  I'm not sure if I'll ever get to them, but they're worth noting.

I build daily notes from a template.  The template is essentially some buttons, backlinks, tags, and such.  I may try to add those elements into the markdown output.  Right now my thought is just to append the tags at the end, which would be easy.

I could build a set of these CI actions.  Sometimes I get business documents and want to read them later on my tablet, so another idea is to setup a directory that converts to PDF or EPUB and emails it to my kindle email address.  This one I really think I'll do, and will probably blog about.

This version of the script generates an error when the markdown is created because it's created in the directory I'm monitoring.  I could maybe just create it straight into my vault, but I'd need to handle the date prepending.  That's not a big issue, but it's a bigger issue than just ignoring an error that doesn't really do anything.

## Toodles
So that's it.  This is a cool project for Obsidian obsessives (hand raised) because it makes it easy to ingest all the _other_ documents in our lives.  But the part I'm most excited about is that in a clumsy and hacky way, this is a really easy home delivery pipeline that could be adopted for _anything_ that you want to automate.