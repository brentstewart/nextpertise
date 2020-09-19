---
title: "CI for Docs with Pandoc (take 2)"
date: 2020-09-19T13:18:28-04:00
draft: false
Victor_Hugo: "true"
Focus_Keyword: "Pandoc"
author: "Brent Stewart"
github: "https://github.com/brentstewart/Pandoc-CI"
youtube: ""
refs:
  [
    "https://pandoc.org/",
    "https://github.com/pandoc/pandoc-action-example",
    "https://github.com/Wandmalfarbe/pandoc-latex-template/releases/tag/v1.5.0",
    "https://hub.docker.com/repository/docker/brentstewart/pandoc_texlive",
  ]
tags: ["Git", "Pandoc", "CI"]
---

In a previous [post](/Pandoc), I built out a Continuous Integration process for documentation. That process allowed the team to pull a Github repository and keep a local copy of documentation. As the markdown files were updated and the repository pushed, a clean PDF with table of contents was generated.
I identified a few problems with that and I've been working to improve the process. If you take a look at the associated github project you can follow my struggles. Here's where we are with the open issues from last time:

The generated document placed the cover _after_ the table of contents. It also include the github README.md in the generated file. The Github CI process uses _.github/workflows/test.yml_ to build the workflow and my file pulled in all the markdown files alphabetically.

> \- run: echo "::set-env name=FILELIST::\$(printf '%s ' \*.md)"
>
> with:  
>  args: --template eisvogel2.tex --o output/result.pdf \${{env.FILELIST}}

I fixed this by changing FILELIST to use **d\*.md**.

I was also able to improve the output a little by generating a default latex layout.

> pandoc -D latex > ~/next.latex

I also found that including YAML headers in the markdown transferred over to the output. Pandoc only takes the first headers it finds, so I placed my files in doc0.md. I was able to transfer all my customizations from the latex file to the YAML, which is much cleaner to maintain. Here's a version of that file.

> ---
>
> title: "My Sample Doc"  
> subtitle: ""  
> author: [Brent Stewart]  
> date: "2020-09-18"  
> abstract: |  
>  This is a sample document used to demonstrate documentation via pandoc and Github.  
> keywords: ""  
> institute: "Nextpertise.Net"  
> numbersections: true  
> toc: true  
> geometry: margin=2.5cm  
> header-include: \pagebreak  
> include-before:  
> include-after:  
> logo: "Feed-icon.png"  
> header-includes: |  
>  \usepackage{fancyhdr}  
>  \pagestyle{fancy}  
>  \lfoot{Prepared September 18th, 2020}  
>  \rfoot{Page \thepage}  
>  \cfoot{}

---

Notice that this also cleaned up the command line used to invoke pandoc,as things like the Table of Contents directive were moved to YML. You can also use this technique to change fonts, margins, headers, and such.

## What's **not** working

That the good news. Using the existing directions and my **mymeta.md** example, you can get a nice clean output. What's not working is having a cover page. That led me into customizing the latex file, which led me to find the eisvogeltemplate project. Pascal Wagler has developed a _very_ nice and usable latex template (it's currently in my github project). I was able to use it locally and output wonderful PDFs. It has some dependencies that required I grab the full latex distribution (apt install texlive-full), but otherwise no problems.

I wanted to update the pandoc Docker file to support the eisvogel template. I made a custom Dockerfile to pull pandoc/ubuntu-latex
and add in the full set of texlive files.

> from pandoc/ubuntu-latex  
> run apt update && apt install texlive-full texlive-full -y

This was then built and pushed to Docker hub.

> docker build --tag=pandoc_texlive:1.0
> docker push brentstewart/pandoc_texlive:1.0

I went back to github and updated the CI action to use the new docker file and to run **pandoc --template eisvogel.tex -o Output/result.pdf d\*.md** and . . . didn't work! I keep getting:

> ! LaTeX Error: File `adjustbox.sty' not found.

This works on my local system, but it doesn't work if I run docker locally with my image. The default pandoc images don't have the required files either.

I've tried rebuilding Docker with just the texlive-extras (a smaller set of Latex files). I tried editing the latex to remove the dependencies. So far nothing has worked.

## Conclusion

Good progress was made on the project. I improved the CI output and simplified the CI process by using the default docker file and by supplying instructions in YAML. I also was able to produce really nice PDFs locally using the eisvogel.tex template.

Trying to duplicate this last step on Github failed because I'm missing includes in the docker system, and trying to update that Docker system has been a source of frustration! I'll update this when I have some more time, but we're getting close.

If anyone out there want's to take a look at the github project and the docker image, I'd welcome your thoughts!
