---
title: "Github setup"
description: ""
author: "Brent Stewart"
date: "2024-02-25T14:47:54-05:00"
markup: 'mmark'
math: false
draft: false
Victor_Hugo: "true"
picture: "git"
Focus_Keyword: "Github setup"
youtube: ""
github: ""
refs: [""]
tags:
  - ""
---
![_Github screenshot_](/Githuboptions.png#floatsmallleft)
Recently I had to rebuild my desktop.  Backups were my friend, and recreating my rig and loading files went without a hitch.  My _git_ directory restored, but I needed to reconnect them to Github.  I decided to start with fresh pulls, so I renamed _git_ to _oldgit_ and pulled down my repos (including the one that's used for this website).

Once setup, Github is easy to use, but I always forget how to set it up and have to rediscover the process.  Hopefully, this quick step-by-step will help you avoid that difficulty (and will help me the next time I need to do it)!


## Cloning the repo
In this case, my repos already exist.  I started by cloning from github to have a clean current copy.  I'm using this website as an example.

    git clone https://github.com/brentstewart/nextpertise.git

## Setting 
Next I created a personal token.  I clicked my user icon (my smiling face) in the upper right hand corner of the [Github](https:github.com) site and chose _Settings_ (near the bottom).  In the settings menu, choose __\<Developer settings\>__ at the bottom.  This gets you to a menu that let's you chose __Personal access tokens__.  I use classic tokens - the "fine grained" option allows for permisssions and would be more appropriate if there was a team of people maintaining this site.

At this point, it's as simple as clicking __Generate new token__ and then copying the text string down for later use.
![_Github token setup_](/Githubtoken.png#center)

## Git Access to Github

You can test the new setup with an ssh to Github.  You should be prompted for your Github username and then a password.  Use the token from above for the password.

    ssh -T git@github.com
    Hi brentstewart! You've successfully authenticated, but GitHub does not provide shell access.

Once the token is setup, push and pull commands should work without forcing your to reauthenticate. A typical workflow of syncing to git might be:

    git add .
    git commit -m "Another commit"
    git push