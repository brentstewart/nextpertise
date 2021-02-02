---
title: "Deskreen"
date: 2021-02-01T13:43:09-05:00
draft: false
author: "Brent Stewart"
Victor_Hugo: "true"
Focus_Keyword: "Deskreen"
picture: "review"
github: ""
youtube: ""
refs: ["https://github.com/pavlobu/deskreen","https://deskreen.com/"]
tags: ["Review","Linux","Windows"]
---

Deskreen is an open-source utility that allows you to send your Windows, Mac, or Linux desktop onto another screen, including another computer, a tablet, or even a phone.  It can duplicate an application, duplicate an entire display, or extend the desktop and treat the other computer as an additional monitor.

![HDMI Plug](https://deskreen.com/img/plugs/hdmi.jpg#floatsmallleft)
Deskreen outputs your display to a webpage with an embedded video.  Running the application prompts you to choose an application, existing display, or new display.  It then displays the produced webpage in text and with a 2D barcode.  The page, when opened on a separate device, allows you to play the video stream.

## Testing Deskreen
An easy scenario to imagine would be using a tablet as a second screen for a laptop sitting in a coffee shop.  I tested exactly this setup using PopOS! on a 3rd Gen i7 and a Fire HD 10 9th gen tablet.  I downloaded Deskreen from Github as a DEB and installed it.  In order to fool my laptop into thinking it should produce a second screen, I bought [HDMI dummy plugs](https://www.amazon.com/gp/product/B07C4TWZRM/ref=ppx_yo_dt_b_asin_title_o04_s00?ie=UTF8&psc=1).  I used Deskreen version 1.02 and 1.03 while testing.

Once running, Deskreen produced a barcode that I was able to scan from the tablet and use to connect to a webpage.  You can manually type in the link, but it's long and it's randomly generated each time the app start sharing.  I clicked a button on the web page to register.  Back to the PC, where I accepted the connection and chose the output.

![My Setup](/210201_Deskreen.jpg#floatsmallright)
The new display was initially set to the same as my main screen - 1920x1080.  The video was down-converted, but everything was too small to be usable.  I used the display options in PopOS! to adjust the size to 1280x720 and this created a very usable display.  The new screen is responsive, with maybe barely a touch of latency similar to using a low-refresh-rate monitor.  I imagine that the quality and usage of wifi will impact this, but I wasn't able to test that scenario.

![Phone](/210201_Deskreen2.jpg#floatsmallleft)
Just for fun, I sent the settings app to my phone at the same time.  Running two displays didn't seem to bother the PC.  I left the webpage running on the tablet for hours and battery life on the tablet seemed surprisingly good.  I think I could run it for a full day (or more) without having to charge the tablet.

## Conclusion
I could imagine a variety of cases where this could be useful.
* Extra screens
* sharing screens for troubleshooting
* connecting to smart TVs or displays

The developer, Pavlo Buidenkov, has an excellent set of directions about using Deskreen at deskreen.com.  He also mentions that he's hoping to find developers to collaborate with, so check it out!

For me, this utility works well and fills a niche in my toolset.  It will be a part of my standard build from here on out!