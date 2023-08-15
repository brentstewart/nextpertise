---
title: "Writing a Blog - Reflections after a half year"
date: 2021-01-02T11:45:16-05:00
draft: false
author: "Brent Stewart"
Victor_Hugo: "true"
Focus_Keyword: "Blogging"
picture: "hugo"
github: ""
youtube: ""
refs: ["https://www.markdownguide.org/","https://jamstack.org/","https://vscodium.com/","https://render.com","https://mikedane.com","https://analytics.google.com/analytics/academy/"]
tags: ["Hugo","JAMStack","Cloud"]
---
I started writing this blog back in July of 2020.  There was a lot going on at that point, and yet a lot of nothing.  My employer was acquired in May and management was cut, so I was surprised to be out of job.  We were quarantining, which added a degree of difficulty to the job search, and summer is a slow time to find work anyway.  Against all this uncertainty, I wanted to do _something_ and decided to start writing.  I hoped that the blog might provide a way to establish my _bona fides_, and if nothing else it gave me a chance to share what I was working on (since I no longer had co-workers to share with).

It's now January of 2021.  I'm employed and the blog has been going for six months.  I thought I'd take a look back at the experience of starting a blog.  Has it accomplished what I hoped?  How hard was it and what things would I change?

Most posts here are technical and this article is intended to address technical things I've learned.  At the same time, writing is a personal expression and accomplishes personal goals, so I also want to share how this experience has worked for me.

I'll skip to the conclusion . . . I've found it valuable, technically approachable, and I think it's something most people should consider.  With that conclusion in mind, one of the biggest challenges is trying to figure out how to get started.  There's some good information online and the Hugo community is very supportive, but it's mostly organized into how to accomplish small discrete tasks and not around helping communicate the big picture.  Hopefully this will encourage you to give it a try and help you avoid some of the problems that might derail you.

## What I did well
![Hugo](https://d33wubrfki0l68.cloudfront.net/c38c7334cc3f23585738e40334284fddcaf03d5e/2e17c/images/hugo-logo-wide.svg#floatsmallright)
[Hugo](https://gohugo.io) is a winner.  I write articles in Markdown, which is lightly formatted plain text.  Hugo then automatically applies formatting and plugs it into the navigational structure.  Hugo takes my raw files and generates a set of set of finished static HTML files.  Because of the way Hugo works, I haven't had to think about server infrastructure or spend a lot of time "coding", I've been able to take the time I have and focus on writing.

I write articles in [VSCodium](https://vscodium.com/).  Microsoft's Visual Studio is open-source and VSCodium has stripped out the telemetry.  The two products are almost perfectly equivalent, so take your pick.  VSCodium is very easy to use, once you get the hang of it.  I open my local copy of the Nextpertise repo in the left pane and a terminal below.  I typically write with __hugo server -D__ running so I can check output on the fly.  I used to use different products for note taking, but I've switched over to VSCode for that, and I could really imagine writing a longer book using VSCode and then outputting via [pandoc](/posts/200919_pandoc_improved/).

![Visual Studio Code](https://code.visualstudio.com/assets/images/home-git.svg#floatsmallright)
Blog files are kept in a local copy of a Git repository.  After I finish updating, I sync up to GitHub which backs up my files and starts the process of moving them "live".  Like VSCodium, Git has become a central tool for me in a variety of settings.  I use it for this blog, for code that I write, for artifacts that I contribute to GNS3, and even for keeping notes (you can have a private Repo).  I've learned about Git, started contributing to more projects, published my first "app", and even expirimented with CI/CD.  One of the benefits of writing the blog is that I've grown technically and picked up these cool tools.

![Render](https://dka575ofm4ao0.cloudfront.net/pages-transactional_logos/retina/89884/render-logo-dark3.png#floatsmallleft)
[Render](https://render.com) hosts the site, and that has been a big win.  Render hosts static sites for free, so my only cost has been the domain.  Render integrates with GitHub via a CI process, so when I sync my repository it automatically generates web content with Hugo and posts it to the Render CDN network.  I've had friends in different parts of the world confirm that performance is uniformly great.  Like other decisions I made early on, this has worked out to allow me to focus my time on content.

So those are the big wins: Hugo, VSCodium, GitHub, and Render.  How did I decide on that workflow and set of vendors?  The truth is that I saw [Mike Dane's Giraffe Academy](https://www.mikedane.com/) Hugo videos on Youtube and thought "I can do that".  I was also starting to learn Python and had been fooling around with Visual Studio.  GitHub was something I'd been using for a while, especially to work with the GNS3 project, but I'd been using it more at work as well.  It really just all came together.  One of the things that I think I did right was I didn't over-analyze things.  I expirimented enough to make sure that I had a good plan and then I just went for it.

Would I revisit any of these decisions?  There are several static site generator alternatives (the other one I hear most about is Jekyll), but so far I've been able to do everything I want with Hugo.  I expirimented with hosting in [AWS S3](/posts/200728_s3review/) and AWS offers this free in the first year.  [Netlify](https://www.netlify.com/) is similar to Render in that it too focuses on the JAMStack space.  Netlify has a lot of good technical documentation and videos of lectures, and they sponsor and participate in the community.  I think all three would have been good options, but for me at this point: it's not broke.  I think that is really the magic of Render: the technical and financial barrier to doing this was significantly lowered, and that encouraged me to start and has allowed me to focus on the part I enjoy since.

Another of my early decisions that has really born fruit - making my own CSS and theme.  Hugo allows you to clone a theme from GitHub.  As I mentioned earlier, the Hugo community is very supportive and there are a lot of themes shared via their [gallery](https://themes.gohugo.io/).  I initially started with someone else's theme, but I quickly realized that it was going to take a lot of work to understand how it all fit together.  This was particularly true because I don't have a lot of background in web design.  The Giraffe Academy videos are really good and Mike gets into the early stages of developing a theme there.  I used some of the Giraffe Academy ideas as a basic theme and built up from there.  I've been able to leverage the blog to learn HTML and CSS.  The theme generally get's more complicated because I want to solve a new problem, so "rolling my own" has allowed me to grow with the site. 

## What I would change
![Google](https://www.google.com/images/branding/googlelogo/1x/googlelogo_color_272x92dp.png#floatsmallright)
My biggest issue has been building readership.  I still don't think I've got a great handle on how to do that.  Google and Bing make it easy to register your page with them so it shows up in search results.  Google also has free online classes in using their Analytics and Search portals.  I recommend you do those things, but I'm still seeing slow and steady growth in readership and not a big growth.  Because the site isn't widely read, it really didn't get my name out when I was job hunting.  I recommend spending some time understanding how to publicize your site.  If anyone has great ideas and could leave them in the comments, I would appreciate your thoughts!  

Another mistake has been setting __draft=false__ in the Archtype.  Turning off the "drafts" feature in Hugo has resulted in publishing pages that weren't finished.  Yes, you will be annoyed that you forget to "publish" by setting __draft=false__, but it was a dumb idea to just turn it off.

Finally, when I started I didn't really design the site for growth.  I put all the files in the "content" directory and let them pile up, making them hard to organize.  Over New Year's, I went back and prepended dates to file names and moved everything under a "posts" subdirectory.  This also allowed me to create the "Archives" link and made curation easier.

## Conclusions

I started the blog with two goals: sharing some cool things I've learned and marketing myself during a period of unemployment.  The blog  didn't really help get a job, but it kept me busy and focused on moving forward during a potentially depressing period.  However, I've found it cathartic to write and I hope that people have read and found value in this work.  

Creating this site has also given me a "real world" case to learn about HTML and CSS, cloud hosting, and online analytics.  It's also pushed me to get more involved in various online communities and to learn at a deeper level so I can share my conclusions here.

George Carlin used to have a bit about the difference in the quality of experience betweeen _riding_ and _driving_.  It was a funny routine, but it stuck with me (like several of his thoughts) because it was true.  Learning through necessity pushes you to go deeper and wider.  Although my day job is management, continuous detailed learning is an important part of the IT industry.

This has been easy enough that I've thought about creating sites for my church or the Trail Life group I lead.  If you have an expertise and an interest in sharing it, I would encourage you!  Let me know how I can help!  For those of you already on the path, I'm going to continue this conversation in a second column to detail some of the cool things I've learned how to do!