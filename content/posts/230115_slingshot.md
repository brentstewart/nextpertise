---
title: "Slingshot"
description: ""
author: "Brent Stewart"
date: "2023-01-15T15:03:12-05:00"
math: false
draft: false
Victor_Hugo: "true"
picture: "games"
Focus_Keyword: "Slingshot game"
youtube: ""
github: ""
refs: ["https://flathub.org/apps/details/com.github.ryanakca.slingshot"]
tags:
  - "Games"
  - "linux"
---

Thirty five years ago (!) I took my Amiga 1000 to college.  There used to be a copy of WordPerfect for _everything_ and that was what I did a lot of my term papers on.  I would print postscript files to a 3.5" disk, then take them to the library and print using __copy termpaper.ps > lpt1:__ to the LaserWriter.  

![Slingshot](/230115_Slingshot.png#floatsmallleft)

But then, like now, computers were used for more than just work.  There were a lot of cool Amiga games including Arctic Fox (a tank game), an "x and o" football game that we wore the controllers out on, and others.  But I also remember a really simple "Star Trek" game that we had a lot of fun with.

In retrospect, I'm guessing this game was early FOSS and it certainly wasn't licensed, but this was also that period where the last TV show was 20 years old and there had only been a few mediocre movies to keep the flame flickering.  

![Slingshot wide-angle](/230115_Slingshot2.png#floatright)

In the game, two ships were positioned on opposite sides of the 2D playing field - rudimentary Enterprise and Bird of Prey shapes.  In between were a set of planets of various sizes and densities that were shown as basic circles.  The opposing ship took turns shooting at each other by specifying an angle and a speed for their shot.  Once the shot was away, the gravity of the various planets pulled it into sweeping arcs.

This game had more in common with _Pong_ than modern space warfare simulators, but it was fun to sit around with my friends and give each other grief.  We'd imitate Star Trek characters and belittle each other's shots.

I've looked for that game over the years and finally found an updated version.  I recently re-installed NixOS on my laptop and was looking through Flathub when I saw __Slingshot__ by Ryan Kavanagh.  Slingshot can be installed from Flathub.

    flatpak install flathub com.github.ryanakca.slingshot

![Slingshot mid-game](/230115_Slingshot3.png#floatleft)

In the old Amiga game, players would type in an angle (0-360) and power (1-10).  In this version, a bar is shown emerging from the active ship.  Up and down arrows are used to control power (shown by the length of the bar) and left and right arrows are used to turn the ship.  You can see in the screenshot that the graphics look great but no longer mimic any particular sci-fi franchise.  

If a shot leaves the screen, there's a zoom-out (shown below).  Shots can orbit until they leave the wider area or timeout.

The gameplay is intuitive and very much in keeping with my memory of the "classic" version.  There's something about the simplicity that I enjoy, and the turn-based play makes it more social.  I played it with my son and had a ball, and anything that gives dads fifteen minutes with their teenager is awesome.