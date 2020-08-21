---
title: "Team Documentation Using GitHub and Pandoc"
date: 2020-08-18T13:56:30-04:00
draft: false
author: "Brent Stewart"
github: "https://github.com/brentstewart/Pandoc-CI"
youtube: ""
refs: ["https://pandoc.org/", "https://github.com/pandoc/pandoc-action-example"]
tags: ["Git","Pandoc", "CI"]
---
![Dean Wormer](https://thumbs.gfycat.com/GargantuanScaryAnura-max-1mb.gif#floatleft) 
Building team documentation is a critical part of IT.  After all, you can't manage what you don't know about.  You can't follow policies you don't know about.  It's common in IT that documentation is divided between shared files and updated copies on individual laptops.  The problem is that it's difficult for any one person to collect all the most recent files.  I've learned a lot about Git in the last few years and I wanted to explore whether it could be an answer.

Various attempts have been made to resolve this versioning issue.  One common approach is to ask everyone to contribute their individual documents to a shared folder.  This has the advantage that everyone can be a contributor and the files are accessible and easy to update.  However, different team members may update the file at different times and in different ways and there's no clear editorial process to bring everything back together.  A second approach is to have a strict editorial process -- maybe a dedicated person who "checks in" so the boss can "approve".  There's typically a drop box for proposed documents and then a locked-down directory with a PDF for the final version.  This process can take a while, can occupy a person, and is discouraging to contributors.

Using Git has a lot of advantages.  Everyone can have an up-to-date copy using __git clone__ and __git fetch__.  Team members can edit documents and submit them as Pull Requests (PRs).  There's a built in process for the repo owner to accept or reject those changes.  GitHub already has an issues process that would make it easy to note deficiencies and discrepancies.  Finally, Github repos can be marked private, are available even when internal systems are down, and maintain historical versions.

Having a repository of Word files would be useful, but formatting can be maddening.  All those files will use different fonts, sizes, margins, colors, headings . . . sigh.  Another problem is that you'll have a directory full of files with names like __ITPROC1.docx__.  As an administrator, I would like to have one place where I can easily browse through documentation and be confident that I'm up to date!

My proposal is to use Markdown files for documentation.  They're easy to create and reasonably readable in a text editor.  As pushes or PRs occur, these files can be combined into a PDF using __pandoc__.  I've built a demonstration of this in the referenced Github repository.  There's a Continuous Integration (CI) process built out of Github Actions.  Setting that up adds a _.github/workflows/name.yaml_ file to your repository.  

__Be careful!__  _I originally built my repository locally and pushed it to Github, then used the Github actions "wizard" to setup the CI process.  That builds an initial configuration file for you and puts it into your repository.  The next time I pushed, this directory and file were wiped out!  The result was that the CI process didn't run and it took me a while to understand what I had done._

Here's my YAML file to handle the CI process.

> name: CI  
> on:  
>> push:  
>>> branches: [ master ]  
>> pull_request:  
>>>branches: [ master ]  
> jobs:  
>> build:  
>>> runs-on: ubuntu-latest  
>>> steps:  
>>>> \- uses: actions/checkout@v2  
>>>> \- run: echo "::set-env name=FILELIST::$(printf '%s ' *.md)"  
>>>> \- uses: docker://pandoc/latex:2.9  
>>>> with:  
>>>>> args: --toc --output=output/result.pdf ${{env.FILELIST}}  
>>>>> \- uses: actions/upload-artifact@master  
>>>>> with:  
>>>>>> name: output  
>>>>>> path: output  

![Artifact](/githubartifact.png#floatright)
This process will pull all the md files into a PDF, ordering them alphabetically.  It will then add a table of contents to the front, based on headings found in the files.  I've cribbed this together using the pandoc example on github (referenced below).

The [result](/result.pdf) is a zip file named _output_ that will show up under Actions.  The latest run should be at the top of the screen, and clicking the link should show you the Artifacts produced.  Note that if there are problems with the CI process, you can review those by looking at the _build_ section.  You could add to the CI process to have the output file emailed to you or stored in a convenient place.  For instance, you could send the PDF directly to your Kindle!  I've chosen not to bother with that since this is a public repository.  Another idea would be to have this process output HTML files that could be placed on a local web server.  Pandoc can handle PDF, HTML, EPUB, and a lot of other formats.

My repository is public and you are welcome to clone it and play with this process.  The markdown files currently in place are filled with Lorem Ipsum nonsense but they give you a sense of how it might look as a finished PDF.  I'd like to build in an automatic way to add a cover page.  The Pandoc documentation also references using a CSS file to dictate formatting when outputting to EPUB, so I'd like to see if I could get that supported in PDF.  PRs are welcome if you have any ideas!

What do you think?  Would this be a good way for your IT group to maintain documentation?  I'd welcome your comments in the section below!