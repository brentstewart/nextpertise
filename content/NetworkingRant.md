---
title: "Technically Correct Verbage Rant"
date: 2020-12-20T02:00:15-05:00
draft: false
author: "Brent Stewart"
github: ""
youtube: ""
refs: [""]
tags: ["Basics"]
---

Part of our industry is the need to constantly keep up to date.  I've been going through some training and flipped the "bozo" bit on the instructor because he was struggling to explain some basic network concepts.  I'm in management these days and often have to quickly evaluate other technical people.  Whether it's hiring, considering contract work, or talking to a potential "partner", I form an opinion of you, your company, and your product based on your technical vocabulary.

Here are three terms that are commonly misused and make me suspecious.

## Bandwith

This one is so common, that even people who understand concepts will slip into a vernacular.  In untechnical slang, "bandwidth" has come to be synonymous with time and capacity.  A colleague might ask you "do you have the bandwidth to handle this?"  What they mean is "do you have the time?"  In this sense, it's trendy and hip and can be more easily forgiven.

When your ISP says that their bandwidth is 10Mbps they are describing _network capacity_ or how fully it can be _utilized_.  This usage is technically wrong and either indicates that the speaker doesn't understand the term or is pitching the discussion toward a non-technical audience.  Either of these uses is incorrect even in an analogous sense because they both speak to the amount of information over time.   
__Bandwidth__ properly describes the difference between the upper and lower frequencies being considered at an instant.

![TCP Acks](https://upload.wikimedia.org/wikipedia/commons/thumb/5/55/TCP_CLOSE.svg/260px-TCP_CLOSE.svg.png#floatright)
Further, even the adulterated version of the term is misused.  When a link is said to be "10Mbps", it doesn't mean that a 10MB file can be downloaded in one second!

Notice that ISP speeds are small "b" bits.  There are eight bits to a byte, so a 10 Megabyte file is 80 Megabits.  Further, files are transmitted a little bit at a time.  The client has to acknowledge receipt periodically.  That means that the server has to pause and wait for that acknowledgement.  Actual file transfer has as much to do with the round-trip time for this ack as with the size of the connection.

## IP is Classless

The thing that really set me off was an AWS networking discussion where the instructor mentioned that we would use the "10.10.0.0/16 Class B network".

A long time ago, IP addresses were just numbers.  I could be "1" and you could be "2".

At a certain point, the network was divided into smaller networks of different sizes and the idea of an implicit class was used.  In this _classful_ structure, one could tell what portion of the address was the network prefix based on  the first octet.

{{< bootstrap-table table_class="table table-responsive table-hover" thead_class="table-info" caption="Table: Classful addressing" >}}
| Class | | First Octet | | Assumed Mask  |
|:-----:|:--:|:-----|-|-----|
| A |  | 0-127   | | 255.0.0.0 |
| B |  | 128-191 | |255.255.0.0 |
| C |  | 192-223 | |255.255.255.0 |
{{</bootstrap-table>}}


Later (mid-90s) as the Internet started to grow it became obvious that this system lacked necessary flexibility.  IP addresses began to use _explicit masks_, so that they could specify network prefixes of any number of bits.  At that point (25 years ago!) class became a thing of the past.

## Slash notation

Finally, it's technically correct to describe a mask as "255.255.240.0" but the cool kids would say "/20".  That means the first 20 bits are used as the network prefix.  Not understanding this usage is a clear indication I'm dealing with a noobie.

## My point

Words are important.  The words we choose carry meaning, sometimes beyond their strict definition.  They identify us as knowledgeable and up-to-date in a given subject and inspire confidence in our audience.  I hope you take care to communicate clearly and accurately and avoid these all-too-common flags.

_What other mis-uses of technical language am I missing?_ I'd love your comments.  I'll try to update this discussion as we go forward together!