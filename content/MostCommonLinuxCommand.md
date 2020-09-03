---
title: "Most Commonly Used Linux Commands"
date: 2020-08-17T12:47:16-04:00
draft: false
author: "Brent Stewart"
github: ""
youtube: ""
refs: [""]
tags: ["Linux"]
---
I've seen some articles recently on "Linux commands frequently used by admins"  or "15 commonly used Linux commands", which got me thinking . . . what commands do I use most frequently?

__history__ shows us a list of recent commands starting with a sequence number (example below).  But I just want the command!  

> brent@MintyTwenty:__~$ history__  
    1  mkdir git  
    2  cd git  
    3  git clone https://github.com/brentstewart/Mint-install.git  
    . . .

Piping this output into __awk__ allows me to filter this down to the first word.  __Awk__ can pull a field out; in this case the sequence number is field 1 and the command is field 2.

>  brent@MintyTwenty:~$ __history | awk '{print $2}'__  
    mkdir  
    cd  
    git  
    . . .

Getting closer!  I want a frequence count of which commands are used, so let's pipe this to __uniq -c__.

> brent@MintyTwenty:~$ __history | awk '{print $2}' | uniq -c__  
      1 mkdir  
      1 cd  
      1 git  
      3 sudo  
      1 exit  
      1 git  
      1 ls  
      1 cd  
      1 git  
      1 cd   
      1 ls  
        . . .  

Notice that it's showing __cd__ counted in multiple groups.  I _think_ this is because of the way __uniq__ is grouping, so let's help it out by piping that output to sort before asking __uniq__ to group and count.

> brent@MintyTwenty:~$ __history | awk '{print $2}' | sort | uniq -c__  
      1 "  
      3 alias  
      4 awk  
      4 ./basic.sh  
      1 cat  
     31 cd  
      3 chmod  
      . . .  

At this point it is easy to find that my top commands are:
1. __history__ (probably from working through this example)  
2. __git__  
3. __cd__  
4. __ls__  
5. __sudo__   