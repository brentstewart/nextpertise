---
title: "Ahrefs update"
description: "Nextpertise is a journal of interesting ideas in technology, covering topics such as Linux use at home and in the cloud, self-hosting, and cybersecurity."
author: "Brent Stewart"
date: "2023-09-04T08:43:45-04:00"
math: false
draft: false
Victor_Hugo: "true"
picture: "review"
Focus_Keyword: "Ahrefs"
youtube: ""
github: ""
refs: ["https://ahrefs.com","https://css-tricks.com/essential-meta-tags-social-media/"]
tags:
  - "jamstack"
---
I wrote about [Ahrefs](/posts/230731_ahrefs) a month ago.  Ahrefs is a site inspector that provides feedback on errors on a website.  I contine to use it and be pleased with it.

## Ahrefs after a month
As this site has grown, I've encountered a number of issues.  In the original post, I mentioned that I changed the folder structure at one time and even after a self-audit to clean things up still found references using the old structure.  I've been continuing to work through the problems that Ahrefs found and try to clean up a few each week.  I haven't posted as much because of this change of focus, but I hope that you can see an improvement in website quality.

![Garden Tools](/gardentools.jpeg#floatright)
Ahrefs has also revealed problems of old age.  When I mention a product or a project and include an image, I have generally done that by referencing the image on the source page.  I think the reason I did that was philosophical, it's not like I have space issues working with [Render](/posts/200817_jamstack/).  That said, some of those sources have disappeared.  Others have updated their sites and used new images with new names.  It's not just images - URLs are being updated as well.  The result is broken references from my older pages.  It feels like this aspect of  maintaining a site is like gardening, and in that analogy I'm finding Ahrefs to be the right tools to maintain the garden because Ahrefs runs down all the links and lets me know of any that don't complete successfully.

Ahrefs is also telling me that I lack [social media tags](https://css-tricks.com/essential-meta-tags-social-media/).  My personal view is that Facebook is more of a security risk than a socialization tool.  About the only social media I use is LinkedIn.  That said, I'd like for folks to be able to find what I write and I'd like it to present cleanly in those platforms.  Ahrefs pointed out my backwardsness and provided directions about how to add these tags (as shown below, using Hugo variables to customize the output for each page).

    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Nextpertise - {{.Params.Title}}</title>
        <meta name="{{.Params.Description}}">
        <meta name="keywords" content="{{.Params.Focus_Keyword}},linux, VPN, Virtual Machine, cybersecurity, Virtualization, self-hosting">
        <meta name="twitter:card" content="{{.Params.description}}">
        <meta property="og:title" content="{{.Title}}" >
        <meta property="og:url" content="{{.Permalink}}" >
        <meta property="og:description" content="{{.Params.summary}}" >
        <meta property="og:image" content="https://nextpertise.net/non-technical.png" >
        <link rel="apple-touch-icon" href="https://nextpertise.net/non-technical.png" >
        <link rel="stylesheet" href="https://nextpertise.net/style.css" >
    </head>

## Thoughts
My original conclusion - that Ahrefs offered a valuable resource to web developers - continues to hold.  Using their feedback has helped me to understand some new wrinkles in the way pages are constructed.  Ahrefs has definitely helped me handle the technical debt on the site and keep it up to date with current practices.  After a month, I rely on it more and as I become a more sophisticated blogger and understand more, the tool continues to be accessible and valuable.