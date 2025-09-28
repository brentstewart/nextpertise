---
title: "Biz card"
description: "Creating a curl-able business card"
author: "Brent Stewart"
date: "2023-10-25T21:14:12-04:00"

math: false
draft: false
Victor_Hugo: "true"
picture: "shell"
Focus_Keyword: "curl business card"
youtube: ""
github: ""
refs: [""]
tags:
  - "shell"
---
One of the guys on Jupiter Broadcasting spoke about a curl-able business card and I thought, "Gotta try it".

A little searching led me to an example - the card of [Bryan Jenks](https://github.com/tallguyjenks/BusinessCard/blob/master/business_card).  I used Bryan's work as a template.  Here's what my finished version looks like.

![My curlable business card](/bizcard.png#floatleft)

The basic concept is simple enough - put a slug of text on a web server and anyone can curl it.  Following Bryan's example, I'm using console escape codes to provide color.  Here's a few I find useful.

{{<bootstrap-table table_class="table table-responsive table-hover" thead_class="table-info" caption="Table: Console escape codes" >}}
| Esc | Effect | | Esc | Color |
|:-----:|:-----:|-|:-----:|:-----:|
| [0m | Plain text | | [30m | Black |
| [1m | bold | | [31m | Red |
| [2m | dim  | | [32m | Green |
| [4m | underscore  | | [33m | Gold |
| [5m | blink  | | [34m | Blue |
| [7m | reverse  | | [35m | Magenta |
|  |   | | [36m | Cyan |
|  |   | | [37m | White |
{{</bootstrap-table>}}

You'll notice from the screen shot that I was able to test this with Hugo's built-in server.  I want to deploy this publicly though, so I went to __bit.ly__ and took the long URL _http://nextpertise.net/business\_card.text_ and shortened it to _bit.ly/brentbizcard_.

The hardest part of this short project was getting the spacing to work out.  There's no magic, just testing, adjusting, and retesting.  Not the most useful project, but it has a certain geek-cred.  To access the card, just use curl.

    curl -sL http://bit.ly/brentbizcard