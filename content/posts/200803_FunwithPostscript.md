---
title: "Fun with Postscript"
date: 2020-08-03T16:59:30-04:00
draft: false
Victor_Hugo: "true"
Focus_Keyword: "PostScript"
picture: "201230_n"
author: "Brent Stewart"
github: "https://github.com/brentstewart/postscript_projects"
youtube: ""
refs: ["Postscript Langauge - Tutorial and Cookbook ISBN 0-201-10179-3"]
tags: ["Postscript","Coding"]
---

I really enjoy being a computer professional.  I like the  creativity, problem solving, and the sense that things can be understood.  Sometimes this is directly applicable, sometimes it's just _fun_.  One example of the latter is Postscript.

Most of you know Postscript only as a printer thing, but it's actually a programming language.  Postscript builds a mathematical model of a page and then converts that to a bitmap for printing as a final step.  Postscript files always print at the best resolution available on the output device without having to be reformatted.

Postscript uses a single stack and then pops off the required number of values to execute a command.  For instance, you might give it:
> 1 2 add

In college, I "discovered" postscript by printing a file for an Apple LaserWriter to an HP Laserjet. Since the Laserjet didn't speak Postscript (they use PCL) it just dumped raw text onto a stack of paper. I was fascinated because I could kinda read it, so I researched it and bought three books and just started playing:

>  Postscript Language: Tutorial and Cookbook by Adobe
>  Graphic Design with Postscript by Gerard Kunkel
>  Postscript by Example by Henry McGilton and Mary Campione

At first, I wrote text files and dumped them to the LaserWriter.

> copy myfile.ps > lpt1:

Later I started using early ghostscript to output to my dot matrix printer. I recently rediscoverd some of my files from the 1980's and found that I could display them in Linux with modern Ghostscript.

Here's a simple Postscript program I took from the Postscript Cookbook:
![Circle of Brent](/CircleofBrent.png#floatright)

    /Helvetica-Bold findfont  
            30 scalefont setfont  
            /oshow  %stack: {string}  
            {true charpath stroke} def  
        /circleofBrent  
            { 20 20 340  
                { gsave  
                    rotate 0 0 moveto  
                    (Brent) oshow  
                    grestore  
                } for  
            } def  
        % --Begin Program --  
        250 400 translate  
        .5 setlinewidth  
        circleofBrent  
        0 0 moveto  
        (Brent Stewart) true charpath  
        gsave 1 setgray fill grestore  
        stroke  
        showpage  



Without a lot of explanation, you get the sense of how postscript works.  There's a routine to print my name and rotate the coordinate system in increments of 20 degrees.  That routine is looped through and then the final full name is printed at the end.  Instead of printing this page, in Linux I just typed:

    gs CircleofBrent.ps

and the page is rendered in a window.  I discovered that you GIMP can directly load postscript files (!) and used that to create the logo and favicon for this site.  You can print the file to a postscript printer using lpr.  MFC9340CDW is a brother printer I use, and I opened system-config-printer to confirm the name.

    lpr -P MFC9340CDW rays.ps

I've built a repo with some examples.  I actually found some of my code from 30 years ago! PostScript is a simple enough language to quickly accomplish things with, but complicated enough to get some interesting results.  In fact, you can do things in the language that I've never seen translated to an app.  I hope this post will encourage you to give it a try and have some fun! 