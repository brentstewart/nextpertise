---
title: "Adding a user to Ubuntu"
description: "How to add a user from BASH"
author: "Brent Stewart"
date: "2024-02-26T20:56:43-05:00"

math: false
draft: false
Victor_Hugo: "true"
picture: "linux"
Focus_Keyword: "linux user add"
youtube: ""
github: ""
refs: [""]
tags:
  - "linux"
---

I built an Ubuntu server for work and then had to add a group of co-workers.  I always need to lookup this up, so hopefully my notes will help you!

## Adding a user

Adding a user to Linux is simple enough.  The bash command is _useradd_.  For a user bstewart, the commend is shown below.

    sudo useradd bstewart

## Set password
One the user is created, they have to be assigned an initial password.  This is done with the _passwd_ command, which will then prompt for the new password twice (to confirm).

    sudo passwd bstewart

The passwd command can be used by the user later - without sudo - to change their initial password.

## Create a Home Directory
The last step to to instatiate a home directory.  

    sudo mkhomedir_helper bstewart

