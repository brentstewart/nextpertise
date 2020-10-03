---
title: "Calibre 5.0"
date: 2020-10-01T20:27:16-04:00
draft: false
author: "Brent"
github: ""
youtube: ""
refs:
  [
    "http://calibre-ebook.com",
    "https://www.humblebundle.com/",
    "https://www.bookbub.com/",
    "https://centslessbooks.com/",
    "https://apprenticealf.wordpress.com/",
  ]
tags: ["Calibre"]
---

One of my favorite open-source programs is **Calibre**. Calibre is like iTunes for books, in the sense that it allows you to build a library and helps with indexing and completing metadata. It even fetches book art!
![Calibre](https://lh3.googleusercontent.com/rXiQJLi7-4RUi9MPoBBBHvNkI9GmuEnhhNlsdkDLavAhH0K5R_vINazBmxOR_kc1TAT0BGEG1iLlcBU2yLV2X9Cr7BZ0tF140P2AZZ_nuRyAmtqffTQdxlVrppjW0KDVI-D-7yKG=d#center)

I like e-readers. I have an Amazon Kindle Tablet and a Likebook Mars. Before that I used a Kobo and I've had a Nook and the old Sony Reader. I mostly read technical PDFs and sci-fi books in EPUB or MOBI. The Kindle is great for technical books because its easy to pinch in and out of diagrams. The Kindle is also harder on the eyes and can be distracting (it also has games and email, etc.). The Likebook is great for reading things like a good Science Fiction novel. It's built to be a straight-up reader, so it's not as distracting. I like the e-paper screen and it's easy on the eyes. The Likebook has a quad-core processor and runs Android, so it's reasonably fast (it's just that epaper as a medium has a low refresh rate).

I get books from all over the place. I buy books that are interesting, I purchase "humble bundles" of programming books, and I subscribe to lists of free books like Bookbub and Centsless Ebooks.

## How I Use Calibre

My first use-case is to build and maintain my "pleasure reading" library of ebooks.

Calibre can import all the different ebook formats and convert to any other. It supports TXT, AZW, MOBI, PRC, PDF, RTF, DOC, EPUB and others. In fact, you can tell Calibre the ebook model you are using and it will automatically convert books to the correct format for that device and use the screensize to format the book to work with your reader. Calibre can also look up missing metadata, like authors, descriptions, and art.

I use Apprentice Alf's de-DRM tools to allow converting and formatting. To be clear - I'm an author and piracy deprives me of a reason to write so _don't steal_! But it's annoying that ebooks are locked in to a particular reader (what if that store goes Zune?) and it's nice to be able to buy a book at the lowest price. DeDRM installs into Calibre as a plugin and just automatically strips DRM as a step in the conversion process.

The second place I use Calibre is for technical material. Open source projects and IT vendors put out a lot of their information as PDFs. I also put user manuals and other reference material into Calibre. The program allows me to curate those documents, apply meta-data, and easily search for "all docs by Cisco" or "recent books about Python" or "books by Brent Stewart".

Libraries are directories and you can easily use Calibre to curate a library that can be shared - for instance, important documentation with appropriate metadata. Multiple instances of Calibre can access the same library as well - a technique I used for a while to access books from my desk or my laptop.

Calibre (like iTunes) makes it easy to plug in an ebook and transfer files from your library. More and more I'm using a feature in Calibre that produces a web interface and allows you to grab books over wifi. Both my Kindle tablet and Likebook can open the OPDR-compatible web page and use it to search and download. I can still plug them into local USB, but wireless access is easier.

## Other Major Features

Calibre is written in Python, and has been packaged to run on pretty much any OS. Downloads are available for Linux, Windows and Mac, plus I've seen it ported to Haiku and AmiOS. If the different machines can reach the same share drive, the different Calibre implementations can all work out of the same library (see [SSH File Sharing](/Using_SSH3) article).

Calibre has a feature that will run a scheduled job to grab articles from the web. You can use this to build a virtual newspaper delivered fresh every morning. This feature is interesting, but I've found it less compelling as the world has become more "always connected".

Calibre also includes a reader that will open all the ebook formats it supports. This makes it easy to read on the desktop. If you are reading for work - for instance, a programming book where you want to work examples as you go - or using books for reference, this can be particularly helpful.

Calibre has a large set of plug-ins to help catalog ebooks, set the appropriate metadata, and clean up formatting.

## Calibre 5.0

The author of Calibre, Kovid Goyal, continues to develop the program and has released point versions pretty consistently for years. He recently released version 5 and for the most part this major version shows continued incremental improvement. There's now a dark mode (check that off the buzzword list!), better in-book searching, and the ability to highlight text in books.

The big news is that the code has been updated to Python 3. Python 2 has been deprecated, so this was an important step for the continuation of the project. It doesn't make a difference from the usability perspective, but it's nice to know that the project is in good shape.

## Recommendation

I maintain a "reinstall script" to quickly setup the programs that I depend on when I do a new install (for the record, I have versions of this for Windows, RHEL-based Linux,Debian-based Linux, and Mac). Calibre is on that august list of programs that I can't live without. Calibre has always been a great program, but Kovid continues to improve it bit by bit and maintained a high level of quality throughout. I depend on Calibre and _highly_ recommend it.
