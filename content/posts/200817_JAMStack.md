---
title: "JAMStack"
date: 2020-08-17T08:25:02-04:00
draft: false
Victor_Hugo: "true"
Focus_Keyword: "JAMStack"
picture: "201230_n"
author: "Brent Stewart"
github: "https://github.com/brentstewart/nextpertise"
youtube: ""
refs: ["https://jamstack.org", "https://guides.github.com/pdfs/markdown-cheatsheet-online.pdf", "https://www.netlify.com/","https://disqus.com/","https://utteranc.es/"]
tags: ["JAMStack","Hugo"]
---
I wrote a few weeks ago about setting up this site using Hugo and [Render]({{< ref "render.md">}}).  It's become clear since then that what I "discovered" was a developed concept aimed at solving big problems in web development and that my use case was the simplest use.  Some of you were no doubt well ahead of me, but couldn't tell me because I didn't have a comment system on the blog until this week.
Hugo and Render are one iteration of a concept called the JAMStack.

### You should consider JAMStack

LAMP is a particular implementation of a web server.  By comparison, JAMStack is a loose collection of ideas about how to assemble the pieces needed to serve a webpage.  I have a diatribe on the "loose but enlightening concept" to "marketecture babble" cycle, but I won't bore you with it.  Suffice to say that JAMStack is still early in the process and thus still valuable.

With JAMStack we are removing the ediface of a database/content management system/webserver stack.  The old model stored content in a database and built HTML on the fly.  It was difficult on many levels - building, updating, and securing the pieces, maintaining capacity and availability, and difficult for content creators to view their finished page.  

JAMStack, as originally defined, is JavaScript, APIs, and Markup.  For Nextpertise, this is Hugo+Markup, 3rd Party APIs, and GitHub+Render.  Content is easily created and edited, then pushed straight to a Content Distribution Network (CDN) which provides fast response everywhere in the world.  I don't have to build, license, or grow servers.  I have a local copy of Nextpertise in a local Git repository and can build as much as I want with very little effort.  
![JAMStack](/JAMstack.png#floatright)

HTML is a markup language, but it's complicated and it entangles site design into the content.  Markdown is a simplified markup language that is human-readable.  I write content using VSCodium as a markdown file.  Hugo then compiles this markdown file against a template (in the case of this page, the _single.html_ file in the _themes/layouts/\_default_ directory).  In the VSCodium terminal, I'm running __Hugo server -D__ and a browser is automatically updating a view of this page as I save.  When complete, I can run __hugo server__ and it will output my entire site as a set of html files in the _public_ directory.  Hugo and VSCodium are open source and well supported by their communities, but if you want to use something different there are too many choices for me to list.  I hear good things about Jekyll, Gatsby, and Eleventy, for instance.

Once the site is updated, I push the local copy to Github.  Git provides a backup and handles version control.  It also handles permissions and tracks who makes changes, so I can invite collaborators over time.  Finally, Git provides the Continuous Integration piece.

I don't actually compile HTML locally.  Hugo shows it on the fly for development, but keeping a local public directory and dragging file to a host can create a problem with old file versions still present.  Better to compile a clean copy with each push, and GitHub handles that for me using a continuous integration (CI) process.  When I push a change to Github, a process automatically kicks off to compile the HTML and pass it to Render for distribution.

Speaking of CDNs - I love working with Render.  I chose them after inadequate research and thought they were entirely unique.  It turns out that there are a number of ways to host static sites.  I wrote about S3: good, but doesn't have a way to auto-deploy from Github.  I've heard good things about CloudFlare, Firebase, and GCP as well, but none of them have the CI integration.  If that's a major factor for you, also look at GitHub pages and Netlify.  Of those options, I want to call out Netlify as providing a lot of support and documentation to the larger community.

Everything I've discussed so far is around static content.  Great for a blog, but serious sites require user feedback for things like purchases and comments.  In the case of Nextpertise, adding a comment option looked like it was going to require standing up an EC2 instance and deploying a service that I could embed in the page.  But I really don't want to build and secure a server and I especially don't want to _pay_ for one.  This is where JAMStack gets into the API part.

There are services to which I can subscribe that will provide a tenanted commenting capability for my site.  The biggest of these is Disqus.  Disqus appears to be a great choice and reasonable plans are available.  In the end, I used _utteranc.es_, which is a bit of code that leverages Github APIs to store comments in Github issues.  I'm not building, I'm consuming.  The JAMStack model is to use APIs (like _utteranc.es_) instead of incorporating that logic and - when you reflect on it - it's a Unix-like philosophy of _doing one thing well_ and coupling those things at a higher level.  The philosophy I'll take is to use third party APIs when possible, then to develop a Lambda if needed, then to stand up a server if I have to.  I've seen third party apis for maps, weather, jokes, and even shopping carts.

We're working our way backward thorough the JAM.  Javascript would be added to help with interactivity, but at this point I don't have a use case on my site.  So, mid-August Nextpertise is no javascript, utteranc.es API for commenting, and Hugo>Git>Render for the static site.

I'm continuing to read on this and I want to be careful not to present myself as an expert.  That said, I'm so enthusiastic about what I see that I wanted to share what I've learned with you.  I'll check back on this topic as I have a more developed picture.  I'd also welcome your comments and suggestions!