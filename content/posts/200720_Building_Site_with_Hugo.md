---
title: "Building this site using Hugo"
description: "My first post!"
date: 2020-07-20T11:38:07-04:00
draft: false
Victor_Hugo: "true"
Focus_Keyword: "Hugo"
picture: "hugo"
author: "Brent Stewart"
github: "https://github.com/brentstewart/nextpertise"
youtube: ""
refs: ["https://www.youtube.com/watch?v=qtIqKaDlqXo&list=PLLAZ4kZ9dFpOnyRlyS-liKL5ReHDcj4G3","http://gohugo.io"]
tags: ["Review","Hugo","JAMStack"]
---

This site was built using Hugo, which is a static site generator.  Hugo allows me to create templates and then write my content in markdown.  This makes it easy to update the site without having to fiddle with HTML.  It also makes updating the look and feel easy, because I can update the template and regenerate the site.  

## ![Hugo](https://d33wubrfki0l68.cloudfront.net/c38c7334cc3f23585738e40334284fddcaf03d5e/2e17c/images/hugo-logo-wide.svg#floatleft)
Hugo is found in most distributions - for Ubuntu I installed it with "apt install hugo".  I've found that running the local Hugo dev server ("hugo server -D") and working with the files in VSCodium is a super easy way to develop.

Mike Dane at Giraffe Academy has done an excellent series of videos that walk through Hugo.  Rather than repeat his work, I will tell you a little about my site.

Hugo supports multiple taxonomies, but for now I've focused on using tags.  I've defined some parameters in my front matter for a github link, Youtube link, and other references.  If I populate those parameters, they automatically display on the single template.  I used an HTML Grid for the list pages and set it to scale based on window width to produce a nice responsive behavior.  Hugo supports using themes and there are some great options, but I've chosen to build my own theme ("next") because I wanted to understand the process.  You're welcome to clone the theme.  Better yet, tell me what I did wrong!

This website is maintained on GitHub.  If you like the theme, clone the submodule.