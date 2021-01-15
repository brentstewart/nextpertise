---
title: "Fonts on Linux"
date: 2020-09-09T16:42:10-04:00
draft: false
author: "Brent Stewart"
picture: "201230_n"
github: ""
youtube: ""
refs: ["https://fonts.google.com", "https://www.google.com/get/noto/"]
tags: ["linux"]
---

I'm a little bit of a font nerd.  In the early 90s, someone who knew computers and had a laser printer had an easy opportunity to get into graphic design.  I knew computers and the college had a laser printer, so I got a copy of __Looking Good in Print__ and dabbled.  I've written about this earlier in describing my love of [Postscript](/FunwithPostscript).

If you'd like to have some font options on Linux, there are some great places to get high-quality fonts that work well with Linux applications.  This is a minor, but hopefully useful tip!

## Formats
Most Linux desktop environments support a variety of formats, but the most common are PostScript, OpenType, and TrueType.

_Postscript_ is the oldest of the three.  Developed by Adobe in the 1984, they found a home on the Apple LaserWriter and were _the_ way to print high quality fonts.  They were expensive to license though, so Microsoft and Apple developed TrueType which was a similar vector-based approach to font description.  _TrueType_ added in the idea of "hinting" which offered more control over how glyphs were presented.  _OpenType_ is the evolution of TrueType and allows more complex descriptions of shapes and larger character sets (up to 64K).

All three formats work in Linux.  Unless you are very detail oriented, I doubt you'll be able to tell which font is TrueType and which is OpenType.  There are a a lot of TrueType fonts available, but I use OpenType if I have a choice.

## Using Apt

One way to get fonts is to use the archive for your distribution.  For Ubuntu, there are a variety of fonts available via PPA.  To list and install them use:
> __apt search font__  
__sudo apt install fonts-noto__  

Noto is a Google font that is clean and available in a range of type faces.

This is an easy way to install new fonts, but there's no preview and this approach may require some effort.

## From the Web
![Google Fonts](/DownloadGoogleFont.png#center)
Option #2 is to search for fonts online.  One great place to get (free!) quality fonts is from [Google](https://fonts.google.com).  Fonts can be previewed online and are easy to download. Google passes these to you as TrueType (TTF) files in a Zip.  Unzip the file and copy the files to _/usr/share/fonts_.  The __fc-cache__ command rebuilds the font cache to include the new files and they should be available in programs afterward.

> __unzip _font_.zip__  
__sudo cp *ttf /usr/share/fonts/truetype__  
__sudo fc-cache -f -v__  

The _fonts_ directory has a set of sub-directories (including _postscript_, _truetype_, and _openscript_) so change the target directory appropriately.  If you downloaded a group of fonts that are sorted into sub-directories, you can grab all the ttf files recursively using __sudo cp *ttf /usr/share/fonts/truetype -r__.

This font directory (/usr/share/fonts) makes fonts available to all users.  If you want to install them for just one account, you can install them to _~/.fonts_.  I've never done it that way, since I control my system, but this would be particularly useful if you don't have root privileges.

The Google site is a great resource, but there are many places to purchase or get open-source fonts [online](https://lmgtfy.com/?q=fonts&pp=1&iie=1).  Hope this helps!





