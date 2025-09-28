---
title: "Terminal Weather"
description: "Pulling weather from the command line"
author: "Brent Stewart"
date: "2023-09-10T19:45:03-04:00"
math: false
draft: false
Victor_Hugo: "true"
picture: "shell"
Focus_Keyword: "weather"
youtube: ""
github: ""
refs: ["https://wttr.in","https://github.com/chubin/wttr.in"]
tags:
  - "linux"
  - "shell"
---
I found a fun addition to my command line - wttr.in.  I live in Hickory, NC, and the following curl command will return the weather to the command line:

    curl 'https://wttr.in/Hickory%20NC'?0?A?u
    Weather report: Hickory NC

      _`/"".-.     Thunderstorm in vicinity, light rain
        ,\_(   ).   68 °F          
        /(___(__)  ↓ 2 mph        
          ⚡‘‘⚡‘‘  9 mi           
          ‘ ‘ ‘ ‘   0.0 in  

Seeing a three day forcast is even easier.
    curl 'https://wttr.in/Hickory%20NC'
    Weather report: Hickory NC

      _`/"".-.     Thunderstorm in vicinity, light rain
        ,\_(   ).   68 °F          
        /(___(__)  ↓ 2 mph        
          ⚡‘‘⚡‘‘  9 mi           
          ‘ ‘ ‘ ‘   0.0 in         
                                                          ┌─────────────┐                                                       
    ┌──────────────────────────────┬───────────────────────┤  Sun 10 Sep ├───────────────────────┬──────────────────────────────┐
    │            Morning           │             Noon      └──────┬──────┘     Evening           │             Night            │
    ├──────────────────────────────┼──────────────────────────────┼──────────────────────────────┼──────────────────────────────┤
    │               Fog            │               Overcast       │    \  /       Partly cloudy  │     \   /     Clear          │
    │  _ - _ - _ -  68 °F          │      .--.     71 °F          │  _ /"".-.     +77(80) °F     │      .-.      +73(77) °F     │
    │   _ - _ - _   ↓ 3-4 mph      │   .-(    ).   ↙ 3-4 mph      │    \_(   ).   ← 1-2 mph      │   ― (   ) ―   ↓ 1-4 mph      │
    │  _ - _ - _ -  6 mi           │  (___.__)__)  6 mi           │    /(___(__)  6 mi           │      `-’      6 mi           │
    │               0.0 in | 0%    │               0.0 in | 0%    │               0.0 in | 0%    │     /   \     0.0 in | 0%    │
    └──────────────────────────────┴──────────────────────────────┴──────────────────────────────┴──────────────────────────────┘
                                                          ┌─────────────┐                                                       
    ┌──────────────────────────────┬───────────────────────┤  Mon 11 Sep ├───────────────────────┬──────────────────────────────┐
    │            Morning           │             Noon      └──────┬──────┘     Evening           │             Night            │
    ├──────────────────────────────┼──────────────────────────────┼──────────────────────────────┼──────────────────────────────┤
    │    \  /       Partly cloudy  │     \   /     Sunny          │    \  /       Partly cloudy  │  _`/"".-.     Patchy rain po…│
    │  _ /"".-.     68 °F          │      .-.      +82(84) °F     │  _ /"".-.     +77(80) °F     │   ,\_(   ).   71 °F          │
    │    \_(   ).   ↘ 2-3 mph      │   ― (   ) ―   ↑ 3-4 mph      │    \_(   ).   ↓ 3-6 mph      │    /(___(__)  ↘ 3-7 mph      │
    │    /(___(__)  6 mi           │      `-’      6 mi           │    /(___(__)  6 mi           │      ‘ ‘ ‘ ‘  6 mi           │
    │               0.0 in | 0%    │     /   \     0.0 in | 0%    │               0.0 in | 0%    │     ‘ ‘ ‘ ‘   0.0 in | 82%   │
    └──────────────────────────────┴──────────────────────────────┴──────────────────────────────┴──────────────────────────────┘
                                                          ┌─────────────┐                                                       
    ┌──────────────────────────────┬───────────────────────┤  Tue 12 Sep ├───────────────────────┬──────────────────────────────┐
    │            Morning           │             Noon      └──────┬──────┘     Evening           │             Night            │
    ├──────────────────────────────┼──────────────────────────────┼──────────────────────────────┼──────────────────────────────┤
    │     \   /     Sunny          │     \   /     Sunny          │  _`/"".-.     Patchy rain po…│  _`/"".-.     Patchy rain po…│
    │      .-.      71 °F          │      .-.      +84(86) °F     │   ,\_(   ).   68 °F          │   ,\_(   ).   68 °F          │
    │   ― (   ) ―   → 2-3 mph      │   ― (   ) ―   ↗ 4-5 mph      │    /(___(__)  ↘ 4-9 mph      │    /(___(__)  ↘ 6-13 mph     │
    │      `-’      6 mi           │      `-’      6 mi           │      ‘ ‘ ‘ ‘  5 mi           │      ‘ ‘ ‘ ‘  5 mi           │
    │     /   \     0.0 in | 0%    │     /   \     0.0 in | 0%    │     ‘ ‘ ‘ ‘   0.0 in | 60%   │     ‘ ‘ ‘ ‘   0.0 in | 73%   │
    └──────────────────────────────┴──────────────────────────────┴──────────────────────────────┴──────────────────────────────┘
    Location: Hickory, Catawba County, North Carolina, United States [35.7333312,-81.3442915]

You can also go to this web page to see the forcast as a graphic file: https://wttr.in/hickory+nc.png
![Hickory Weather](/hickory+nc.png)

All well and good, but that's a complicated command to remember.  I know, that's what the up-arrow and _history_ are for, but still there needs to be an easier way.  I created two aliases to let me easily see the current and three day forcast.

    alias weather="curl 'https://wttr.in/Hickory%20NC'?m"
    alias forcast="curl 'https://wttr.in/Hickory%20NC'?0?A?u?m"

The __alias__ will remain in place for that session.  If you use BASH and want to make that permanently available, add the alias command into .bashrc.  You can also add one of those alias at the end of ,bashrc to have it automatically display your current conditionsa and forcast every time you start a session - I've found _forecast_ works pretty well.
