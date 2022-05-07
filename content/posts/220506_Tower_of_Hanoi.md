---
title: "Tower of Hanoi Backup Strategy"
date: 2022-05-06T21:23:38-04:00
draft: false
author: "Brent Stewart"
Victor_Hugo: "true"
Focus_Keyword: "Algorithm"
picture: "n"
github: ""
youtube: ""
refs: [""]
tags: [""]
---

When I was getting started in IT in the late 80s, my mentor taught me the most effecient way to handle backup tapes.  He called it the "Tower of Hanoi" strategy.  This has come up in various contexts through the years, and I've had to explain it.  I thought it might be interesting, particularly to younger professionals, to understand the problem and how it was solved using this algorithm.  Who knows? Maybe there will be a new application you run across!

## Understanding the Game
![Tower of Hanoi](/220506_Tower_of_Hanoi.png#floatsmallright)
The game involves three pegs.  One peg has three rings of increasing size.  The objective is to move the stack from peg one to peg two.  The rules are that you can only move one ring at a time and smaller rings always have to sit on larger rings.

The solution is to move the red ring to peg 2, green to 3, and red to 2 (this puts the top two rings on peg 3).  Move the blue ring to 2, red to 1, green to two, and red to 2.  Bingo!

Analyzing the pattern though, you find that every other move is the smallest ring.  Every other other move is the mid-sized green ring.  Every other other other move is the largest ring.

## Understanding the Problem
In those days we backed up to tape.  In 2022, we still use tape sometimes.  I setup a ToH strategy using USB sticks at church.  These days it could be your cloud backup retention strategy.  Whatever the media, we want to use the fewest number (of tapes or sticks or whatever) to minimize costs, while being able to have the most up-to-date backup covering the longest period of time.

Say a file became corrupted after a week.  Ideally, you'd want to go back through your backups and find the last one before the corruption and restore the file to make sure you had the most recent version of the file.

A popular strategy in the 80s was to have one tape for each day of the week and one tape for each week of the month.  Thus at any time you used nine tapes and had backups that were something like 1 day, 2 days, 3, 4, 5, and 7 days, plus tapes that were two weeks, three weeks,and four weeks old.

## Understanding the Solution
JD taught me the Tower of Hanoi strategy as a better solution.  In this strategy, every other day you use tape 1.  Of the remaining days, every other day you use tape 2.  YOu continue this recursive pattern until you've used all your tapes.  With six tapes, you end up with ages 1 day, 2 days, 4 days, 8 days, 16 days, and 32 days.  This strategy consumes fewer media and covers an arbitrarily long period in a rational way.

In the illustration, the most current tape is on the left and the oldest on the right.  Notice the recursive pattern of tape rotation.  Tapes are reused on a regular pattern, so I've marked the most recent tape to illustrate the rage of dates covered by this method.

![Tower of Hanoi Tape Rotation](/220506_ToHTapes.png)

## Using the Algorithm in the real world
This is an efficient and rational way to commit the few resources to retain a long range of backups.  I like it a lot and it's a common approach I take.  However, I've found that trying to explain this to non-technical folks is casting pearls before swine.  My advice: tell them to buy more tapes.

All that said, throughout my career it's been true that concepts recycle on a predictable basis and what's old is new again.  Understanding some of these ideas has a way of paying off eventually.  

It's also true that I owe my long career to a mentor who took an interest in a younger me.  I'm blessed that I get to see him occassionally to this day.  I'll never be able to repay him, but I try to pay it forward a little bit at a time.