---
title: "Render"
date: 2020-07-24T08:21:27-04:00
draft: false
Victor_Hugo: "true"
Focus_Keyword: "Render"
author: "Brent Stewart"
github: ""
youtube: ""
refs: ["https://aws.amazon.com/s3/?nc2=h_ql_prod_fs_s3", "https://render.com/"]
tags: ["Hugo","Review","Render"]
---
### TLDR: you should take a look at Render.com ###

I wrote in a previous post that I decided to build my site using Hugo, a decision I'm still really tickled with.  My initial draw to a Static Site Generator was to host my site in S3.  There's a lot of attraction there - creating a public S3 bucket is easy, it's low-cost, there's no server to maintain, and the data is replicated within region between Availability Zones.  From a security perspective, S3 is easy to secure and the bucket is isolated.

I have experience with the major cloud providers and my high-level opinion is that AWS is the most mature, has the most complete set of products, and is the easiest to deal with.  Plus, I'm working my way through the AWS certs.

![Render Logo](/render.png#floatright)

In coming up to speed on Hugo, I heard about a site called Render.  The salient points were that Render offered free static-site hosting and would pull your site from Git.  The Git integration was attractive - I had already decided to put the theme there and now I could just put the entire site there.  I decided to try Render.

At the time of this writing, I've had a Render account for two days.  Signup was easy and didn't require a credit card.  They support federation with Github, so I used that option and that may have made things easier later.  

Forcing me to give a card when I signup for something free always makes me feel like I'm being suckered into something.  In fact, I had an experience with Azure where I signed up for a "free" tier and ended up getting a big bill a couple months later so I have empirical reasons to be wary.

I was super-impressed with the Git integration.  I went to Github and created a new "Nextpertise" project, then went to my Hugo directory and made it a repository and sync'd it to Github.

> $ git init  
> $ git add .  
> $ git commit -m "Initial commit"  
> $ git remote add origin https://github.com/brentstewart/nextpertise.git  
> $ git push -f origin master  

Hugo takes your markdown content and compiles it against templates to generate a public directory of html files that can be copied to a web server.  When you are ready to deploy, just run "hugo" with no options.  The caveat here is that Hugo doesn't clear out old content first, and will just copy the new build on top of the old.  Best practice then is to delete the public directory before regenerating.  So before setting up Render, I generated the public directory and sync'd my repo to Github.

![Render Setup](/Render_setup.png#floatright) 

From Render, I selected New "Web Service" and selected the repository I wanted to use.  Render asked for the web content directory (the "Publish directory")  and the build command - here's where I realized I messed up.  I went back and removed my **public** directory and resync'd to Github, then used  **hugo** as my build command.   

By default, Render published my site to **nextpertise.onrender.com**, but adding a custom domain is super-easy.  The setup screen provides instructions on setting up your DNS and tests to confirm that this step is complete.  The Nextpertise DNS is at Network Solutions, so it was easy enough to add the required records and the changes replicated overnight and were working this morning.  Render automatically assigns certs and makes the site available via https (I literally did nothing to enable this feature, it _just worked_).

Render can redirect traffic to unknown pages.  I setup a rule to redirect this traffic to 404.html.  In Hugo, I created a 404.html file under _theme/layouts_.

When I finish this update, I'll commit my local changes and push to Github.  Then I need to go to Render and click Manual Deploy.  Render will pull the changes, build the site using Hugo, and the new site will be online!  Render supports a build api hook, so I may look into using Githubs CI to trigger a Render deploy.  For now, I'm focused on getting enough content onto the site to make it interesting and cleaning up the look.  

Here's a screenshot of the pull and build.
![Render build](/Render_deploy.png#floatright)

Render deployed my site to Oregon - I wasn't given an option, but that seems reasonable for a free service.  They mention that "lightning-fast CDN" is included and accessing the site from the eastern US does seem reasonably quick.  _If one of my friends in India reads this, could you provide some feedback on what it's like for you?_

I'm really impressed with Render and - based on two days of playing - definitely recommend you take a look.  I still intend to deploy to S3, for comparison and to get some experience with S3, so I'll write about that in the future.