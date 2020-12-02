---
title: "SMAG (command line graphing)"
date: 2020-12-01T20:35:56-05:00
draft: false
author: "Brent Stewart"
github: ""
youtube: ""
refs: ["https://github.com/aantn/smag"]
tags: ["linux"]
---
Here's a neat tool to try out - "Show Me a Graph" or SMAG.  SMAG is available on Github and is a command line tool to produce live character-based graphs on the command line.

There are all kinds of situations with modern cloud environments where we are working via SSH and a desktop and graphical tools aren't available.  We still need to troubleshoot and a quick graph is a great way to get a sense of the situation.  Smag takes any command that produces a numeric output and turns that into a graph.

To use smag, either clone the repository or grab the latest release from Github.  The example from the site monitors the number of processes.
> ./smag "ps aux | grep ssh | wc -l" "ps aux | grep bash | wc -l"

The output needs to produce a number.  I accomplished that with other commands by using _grep_ to isolate a line in the output and _cut_ to pick out the number that I want.  I'm sure there are a lot of different ways to accomplish a similar result. Here are some sample scenarios that I tried.

# Graph ping response times
This example pings Google DNS and graphs the round trip time.  I need ping to exit and return a number, so I set the count to one.  That command is then looped thorugh to create the graph.  Awk is pulling the seventh field - I think I could skip this and cut from the raw row, but this works.
![Ping](/smag-ping.png#floatcenter)
> ./smag "ping -c 1 -R 8.8.8.8 | grep time= | awk '{print \$7}' | cut -c 6-7"

# Graph CPU temperatures
![Temp](/smag-sensor.png#floatright)

For this, you'll need to grab some packages to monitor system parameters.  Notice that I have enclosed quotes, so I use the single quote for grep and double quotes for smag.  The two types of quotes do the same thing, but we need to help bash recognize that we have one set inside another.
> sudo apt install lm-sensors  
sudo apt install hddtemp  

and then monitor CPU temperature using this command.  Note that sensors returns a lot of information, such as temperature per core.  I'm arbitrarily grabbing the summary.  You could certainly adjust this to pull the the value that's most relevant to you.
> ./smag "sensors | grep 'id 0:' | cut -c 17-20"  

# Graph Ethernet packets received
![RX Packets](/smag-eth.png#floatright)
You can graph just about anything, but here's a third example to demonstrate the case.
>./smag "ifconfig enp0s31f6 | grep 'RX packets'|cut -c 20-26"

SMAG is a valuable tool, but trying to remember the library of commands to draw out the values might be an issue over time.  If you have some particular values that you need to access consistently, this might be a good place for an _alias_.  Alias allows us to substitute a command for another (usually more complicated) one.  Let's say I want to call up this Ethernet graph pretty often.  I can make this easier to remember:
> alias smag-eth="./smag \"ifconfig enp0s31f6 | grep 'RX packets'|cut -c 20-26\""
  

Here we have enclosed quotes inside enclosed quotes!  Let's parse this out.  The inner-most quotes are used by grep.  I've used single quotes to distinguish this from the outer quotes.  The next layer of quotes are used by SMAG, but they are inside another layer of quotes.  I'm using the backslash to tell alias to treat these quotes as literal characters, which allows them to be enclosed.  Finally, there are double quotes around the aliased command.  Now I can call up this graph by just typing:
> smag-eth

## Conclusion

Thumbs-up.  If this is a problem you need to solve, it's a neat tool to have in your toolbag.  If I had a particular need for this, my recommendation would be to make Smag and a common alias part of my standard build.  A little prep work like that will make things easier if stuff hits the fan.

I want to mention that I had a question and posted an issue on Github.  "Aantn", the developer of Smag, reached out to me and pointed out the error in the command I was using.  I appreciate the work they've put in to Smag and the impetus to share it with the world, and I especially appreciate the time they took with me. 