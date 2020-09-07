---
title: "AWS S3 Review"
date: 2020-07-28T15:44:02-04:00
draft: false
Victor_Hugo: "true"
Focus_Keyword: "AWS"
author: "Brent Stewart"
github: ""
youtube: ""
refs: ["https://aws.amazon.com/"]
tags: ["Review","AWS", "S3", "CloudFormation"]
---

In a previous post, I described hosting this website on Render.  I mentioned that I am coming up to speed on AWS and it was my intention to host the site on S3 as well.  This post documents my experience.

## Hugo

Render has a CI-step that builds the html from Hugo auto-magically.  AWS isn’t integrated with Github, so I needed to build the website.  This is pretty easy, just navigate to the directory and type “hugo”.  This produces a “public” directory that needs to be copied to your webserver.

AWS allows you to specify an error page.  In Hugo, I setup a _404.html_ page under _theme/layouts_ and used the S3 Properties page to specify that URL for the error page.

## AWS
The short version of hosting a site on S3 is:
1. Create an S3 bucket
2. The bucket needs to be public, so set "Block all public access" to OFF.
3. Navigate to S3, select the bucket and go to the Properties tab.  Under "Static Website Hosting" select "Use this bucket to host a website".  You can also grab the URL from this screen.  This will look something like  http://mybucket.s3-website-us-east-1.amazonaws.com.
4. In your DNS, setup a CNAME for www to the bucket URL.

The above is pretty well documented at various places and it's pretty easy.  So obviously, that's not the way I did it.

AWS has a feature called Cloud Formation that let's you specify an environment in JSON or YAML.  This approach is called _Infrastructure as Code_.  There are a lot of scenarios where IaC is useful.  It reduces the time and cost of setting up an environment, which could be useful if you wanted to quickly setup a dev environment or duplicate an environment for some other purpose.  This approach reduces errors because you can troubleshoot the setup script when you build it and then iteratively improve it.  It also allows for the environment to be specified and reviewed by security specialists, improving communication between operations and security and reducing risks.

Cloud Formation is free to use.  I built a JSON [file](/CloudFormation-Setup_Public_S3.json) that creates an S3 bucket, marks the bucket public, and then applies a security policy.  My template also outputs the URL back to you when it completes.  The Amazon online user guide has a lot of examples I used to understand the process, plus there is a template designer that let's you draw out your target environment a la Visio and builds the JSON for you.  I didn't use the designer to draw, but I pasted the file I developed into the designer and it was a good way to "debug".

Doing initial development of a Cloud Formation template meant running the process several times and fixing issues.  For me, most of these were formatting.  This took a little over an hour to iron out.  When everything was ready, I instantiated my S3 web bucket and I just needed to copy my Hugo public folder into the bucket.

AWS has a "free tier" that's offered during your first 12 months.  Five gigs of S3 space is included in this tier, so the initial cost isn't bad and S3 isn't expensive after that.  Whether you my example to use Cloud Formation or not, this is a cheap and effective way to get a static website setup.  Amazon provides a very durable and scalable environment, there's a ton of tools available, and it's easy to imagine growing from this initial setup to a dynamic site using K8s.

That said, updating the html feels a little clunky after using Render and it's integration with Github.  I'm going to leave the S3 version up for a while and try some improvements.  I'd like to build a command line script to run the Cloud Formation process, run Hugo to compile the site, and then transfer files.  That seems doable and it would make this a lot easier to maintain.  AWS also has a CodeCommit repository that looks like Github from a distance.  It would be interesting to explore using CodeCommit for the site as well.

For now, I'm very pleased with the Render workflow and I've decided to leave the "official" copy of the site there.

As always, I'm interested in your experiences and suggestions!