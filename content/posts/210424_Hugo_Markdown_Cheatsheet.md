---
title: "Hugo Markdown Cheatsheet"
date: 2021-04-24T12:42:56-04:00
draft: false
author: "Brent Stewart"
Victor_Hugo: "true"
Focus_Keyword: "Hugo"
picture: "hugo"
github: "https://github.com/brentstewart/nextpertise"
youtube: ""
refs: ["https://gohugo.io","https://codingnconcepts.com/hugo/","https://www.mybluelinux.com/how-create-bootstrap-tables-in-hugo/"]
tags: ["hugo","JAMStack","markdown"]
---
Markdown is a fantastic distraction-free way to write.  Using Markdown in VSCode is one of the things I like most about building this blog with Hugo.  It's important to realize that the site CSS and Hugo shortcodes that are present play a big role in the way you write for the Hugo platform.  Those elements are usually included in the theme.  My theme is called _next_ and you can clone it from Github.

This post is a reference for how to use Markdown with my CSS and shortcodes.  I built my own hugo theme and CSS because I wanted to understand how it all worked.  I don't think that my setup is the fanciest, but you are welcome to clone it or fork it.  If you are new to Hugo, CSS, or HTML in general then my efforts are cleaner and probably more understandable than one of the advanced themes.

I've been doing this for nine months - pretty much the entire pandemic - and I've started to realize that I've forgotten some of the things I used months ago.  This document is therefore a reference for __me__.  My career has never centered on HTML or CSS, so this has been an awesome learning opportunity.  [Mike Dane](https://mikedane.com) has some fantastic videos that helped me get started.

## Basic Markdown
The __quick__ _brown_ __*fox*__ jumped over the {{<strike "frog">}} {{<highlight "lazy dog">}}.  Use the backslash to display literal symbols instead of processing them \{}.

```markdown
The __quick__ _brown_ __*fox*__ jumped over the {{\<strike "frog">}} 
{{\<highlight "lazy dog">}}.    Use the backslash to display literal 
symbols instead of processing them \{}.
```
_Note that "\\" literals appear in the shortcodes to force shortcode syntax display and are not used in production.  Double tildes "~~" can be used before and after to strikethorugh as well._

I picked up the shortcodes  for highlighting and strikethrough  from Ashish Lahoti at [codingconcepts.com](https://codingnconcepts.com/hugo/), who has several interesting Hugo articles on his blog.

## Lists
Unnumbered and ordered list are easy and intuitive.
* Bullet
1. Numbered
2. Example

~~~markdown
* Bullet
1. Numbered
2. Example
~~~
## Headings
Headings are created by successive hash symbols.
# Heading One
## Heading Two
### Heading Three

```markdown
# H1  
## H2     
### H3
```

_I usually use Heading one for the title and heading two inside an article._

## Blockquotes
Block quotes are accomplished by tabs.  Consecutive lines are assumed to be a continuation.  You can also use the "greater than" sign to indicate a section is a blockquote.  When presenting this way add two spaces at the end of every line to indicate the following line is also a part of the block and a new line, otherwise it won't respect returns.  You can also use multiple greater thans to create an indented section.  This comes in using my CSS Blockquote style.

> This is a block quote.  “Two things are infinite: the universe and human stupidity; and I'm not sure about the universe.”
― Albert Einstein 
>> This is a indented block quote.  Because my CSS centers them, it looks a little wonky.  “Darkness cannot drive out darkness: only light can do that. Hate cannot drive out hate: only love can do that.”
― Martin Luther King Jr., A Testament of Hope: The Essential Writings and Speeches 

and here's the actual typed Markdown.
```markdown
> This is a block quote.  “Two things are infinite: the universe and human stupidity; and I'm not sure about the universe.”
― Albert Einstein 
>> This is a indented block quote.  Because my CSS centers them, it looks a little wonky.  “Darkness cannot drive out darkness: only light can do that. Hate cannot drive out hate: only love can do that.”
― Martin Luther King Jr., A Testament of Hope: The Essential Writings and Speeches 
```

## Code
Code can be accomplished a number of ways.  It can be surrounded by three ticks (`).  The easiest way to create a code block is to add a line to the block, then tab and add your code.  Leave a line after the block and the tabbed section will be treated as a code block.

### Tick block
```
Print ("tick block")
Print ("using three ticks before and after")
```
and here's what that looks like:

    ```
    Print ("tick block")
    Print ("using three ticks before and after")
    ```

## Tab block

    Print ("tick block")
    Print ("using three ticks before and after")

and here's what that looks like - line before and after, tabbed block.

     
              Print ("tick block")
              Print ("using three ticks before and after")
     

### Non-printing characters
You can also make this work with non-printing characters as demonstrated below.  Pretty weak approach, but sometimes I don't have a better way to make columns line up.

          non-printing characters were used to indent this line . . .

## Tables
Tables are created using a table shortcode as demonstrated below.
{{< bootstrap-table table_class="table table-responsive table-hover" thead_class="table-info" caption="Table: Demonstration" >}}
| Letters | | Numbers | | Symbols  |
|:-----:|:--:|:-----|-|-----|
| A |  | 0   | | \* |
| B |  | 1 | | $ |
| C |  | 2 | | % |
{{</bootstrap-table>}}

Ignore the backslashes below - the shortcut kicks in when displaying if I leave them out.
```markdown
\{\{\< bootstrap-table table_class="table table-responsive table-hover" thead_class="table-info" caption="Table: Demonstration" \>\}\}  
| Letters | | Numbers | | Symbols  |  
|:-----:|:--:|:-----|-|-----|  
| A |  | 0   | | \* |  
| B |  | 1 | | $ |  
| C |  | 2 | | % |   
\{\{\</bootstrap-table>\}\}
```

I copied the shortcode for bootstrap tables from [MyBlueLinux](https://mybluelinux.com).  I'm not sure who the author is, but they are interested in many of the same things I am - networking, Linux, and so on.  Readers of my blog would probably enjoy this site as well.
![GNS3](/gns3.png#floatsmallright)
## Links and Images
Links in Markdown are formatted as shown below for [GNS3](https://gns3.com).
```markdown
[GNS3](https://gns3.com)
```

Images in markdown are formatted similarly to links, but start with an exclamation.  The code to bring in the GNS3 symbols is shown below.

```markdown
\![GNS3]\(/gns3.png#floatsmallright)  
```

My CSS supports the following directives - these are all dynamic and change the image size based on the window size, which gives a much cleaner look.  
* floatsmallright
* floatsmallleft
* floatright
* floatleft
* center

I'm not sure where I copied this idea from (God bless open source and github!).  You can find this in the next theme style.css in my Github account, along with all the shortcodes.