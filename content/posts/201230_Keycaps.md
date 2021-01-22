---
title: "Custom Keycaps - Updated"
date: 2021-01-12T17:30:38-05:00
draft: false
author: "Brent Stewart"
Victor_Hugo: "true"
Focus_Keyword: "Keycaps"
picture: "mechanical keyboard"
github: "https://github.com/brentstewart/postscript_projects"
youtube: ""
refs: ["https://www.maxkeyboard.com/custom-backlight-compatible-keycap-for-backlit-keyboard.html"]
tags: ["Mechanical Keyboard","Postscript"]
---
## From 12-30-20

My kids got me a Ducky mechanical keyboard for Christmas.  It's a wonderful keyboard, but the Windows keys are hurting my sensibilities.  I decided that I wanted to replace the default Windows logo with the "starburst N" from my Nextpertise logo.

I found that [Max Keyboard](https://www.maxkeyboard.com/custom-backlight-compatible-keycap-for-backlit-keyboard.html) will custom print keycaps.  At the ordering page there's a link to a chart showing the [size](https://www.maxkeyboard.com/mechanical-keycap-layout-and-size-chart.html) of the various keys on different keyboards.  I already new that I needed R1 1.25 keys from Ducky, but MaxKeyboard had the Ducky layout as well so I was able to confirm.

MaxKeyboard wanted a file to print the image from.  I used a postscript program called "rays.ps" (available in my postscript github repo) for the original graphic, but MK wanted at least 300x300 and the resolution on my existing picture was a quarter that.

I decided to update the postscript to just output the "N" and used a larger size.  Here's the code.
                                      
> /Times findfont 300 scalefont setfont  
![N](/201230_n.png#floatsmallright)      /rays  
    { 0 1.5 359  
            {gsave  
            rotate  
            0 0 moveto 1200 0 lineto  
            stroke  
            grestore  
            } for  
    } def  
    0 500 translate  
    1.5 setlinewidth  
    newpath  
    0 0 moveto  
    (N) true  
    charpath clip  
    newpath  
    98 0 translate  
    rays  
    showpage  



 Of course, you can easily show this onscreen using:
 > gs rays.ps

In the past, I've used GIMP to read the Postscript and produce other formats.  Yes, GIMP can do that!  But to simplify the operation, I had Ghostscript output directly to PNG.

> gs -r600 -sDEVICE=pngmonod -sOutputFile=n.png rays.ps

MaxKeyboards has an easy to follow ordering page where you can upload your graphic and specify how you want the image placed.  You can even have them print on the front of the keys.  The black part of the image will be translucent to fit with my backlit keyboard.  

I should get my new keys next week!  Looks like the total cost will be about $10 per key.  

![Nextpertise Keycaps](/210112_NewKeycaps.png#floatright)

## Update 1-12-21
I received the new keys today!  You can see the new keys in the picture here.

As a side story, after I sent in my purchase I noticed that the PNG file I created had a lot of white space.  I was a little worried that the logo would be small and printed to the side.  MaxKeyboard apparently recognized my error and fixed it.  I was also worried that the fine lines of the rays wouldn't translate well through the 3D printing process.  As you can tell from the picture, they came through perfect.

It's really fun when small things like this give you happiness.  I giggled a little taking off the Windows keys and replacing them!  The new keys fit perfectly and the size was dead on.

MaxKeyboard did a great job!