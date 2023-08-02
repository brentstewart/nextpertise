---
title: "Save to Kindle with a DIY CI"
description: ""
author: "Brent Stewart"
date: "2023-05-09T21:20:24-04:00"
markup: 'mmark'
math: false
draft: false
Victor_Hugo: "true"
picture: "linux"
Focus_Keyword: "Linux Kindle"
youtube: ""
github: ""
refs: []
tags:
  - "linux"
  - "CI"
  - "shell"
---

My [previous post](/posts/230509_Word2Obsidian) dealt with building an easy way to convert Word files to Markdown and automatically incorporate them in Obsidian.  That was accomplished by copying the DOCX file into a directory and having automation to perform the actions to get the file into the right place with the right format.  I was pleased with the way that worked out and thought about other places where I'd like to use a similar approach.

# Print to Tablet

I've always wanted a way to "print to tablet".  I've had IPads and Galaxy Tabs and enjoy the form factor - tablets are an easy way to read.  But there's never been a great way to move something I create on my desktop over to the tablet.  I've resorted to saving it to PDF, emailing it to myself, and then opening it on the tablet.  But wait!  Because of the way tabllet break up storage, it's usually confusing to understand where the file is stored and which programs should be used.  Bah!

This really galls me because sometimes I'll print out a document and think, "This would save a lot of paper if I could just print it to my tablet".  The thought of saving printing costs, saving a fraction  of a tree, and having the file in a convenient form would really be nice.

## Ingredients

How did I get this to work?  In my mind, the basic building blocks would be __inotifywait__ (discussed in the previous post), kindle's email import function and ssmtp (discussed in [Command Line Email](/posts/230313_Command_Line_Email)).  Expiriments with ssmtp determined that it's hard to use with attachments, but researching that led me to __mpack__.

This project will use __inotifywait__ to monitor a directory.  When a file is put in that directory it will be copied out to the kindle app on my tablet.  There's a little longer discussion of inotifywait in the previous post.

![Amazon Devices](/230514_Amazon_Devices.png#floatsmallleft)

Amazon provides an email associated with every Kindle, physical or app, that can be used to import files.  Sending an email to that address will copy the attached file into the Kindle's local library (and convert it if needed).  You can find this email address two ways - either login to Amazon and navigate to "Accounts & List" and choose "Devices".  From here you can select either a Fire Tablet or a Kindle app and see it's assigned email address.  It will look like _name_ABCD@kindle.com.  You can also go into the kindle app and find it under "More", then "Settings" and it will be shown as __Send to Kindle Email Address__.

Amazon calls this function the "Kindle Personal Document Service" and claim that it can convert several types of files.  I tested PDF, DOCX, and EPUB and didn't have any issues.
{{< bootstrap-table table_class="table table-responsive table-hover" thead_class="table-info" caption="Table of supported import formats" >}}
|  |  |  |
| --- | --- | --- | --- |
| DOC |	DOCX |	RTF | TXT |
| HTM |	HTML |	ZIP |	x-zip |
|	MOBI | EPUB |	PDF |	JPEG |
| GIF |	BMP |	PNG | |
{{</bootstrap-table>}}

The last piece is __mpack__.  I used __ssmtp__ in my previous project and found some ideas on how to attach a file using ssmtp, but never got that to work.  In the process of researching that issue I found __mpack__, which uses ssmtp (at least the settings and a library) to send an email with attachment.  Install mpack on Ubuntu using __sudo apt install mpack__.  Once it's installed, here's a usage example to help you test.  The part being echo'd in is the body of the email - unnecessary when sending to kindle.  Email subject is set with "-s".  Attachment is defined with "-a", and then followed by the email address this should go to.

    echo "Sent from your linux desktop" | mpack -s "Subject Line" -a "File.TXT" destination_email@ddress

If there are issues, I suggest going back to the ssmtp setup and making sure that part is working.

## Mixing it all together.
I defined a directory _send2kindle_.  Anything copied in will be sent to my kindle email address and imported into that kindle library.  I created a batch file (watch_send2kindle.sh) and made it executable.  That script is shown below.

    #!/bin/bash
    TARGET=~/send2kindle/

    inotifywait -m -e create -e moved_to --format "%f" $TARGET | while read FILENAME
    do
      echo Detected $FILENAME
      echo "Sent from your linux desktop" \
      | mpack -s "$FILENAME" -a "send2kindle/$FILENAME" user_ABCD@kindle.com
    done

This follows the logic used in the previous post.  Note that the backslash (" \ ") shows line continuation, so the echo and pipe are one line.  I did it that way to make it easier to read here.  I've setup __innotify__ to trigger on something being copied in.  The previous discussion has a few more details on that command if you are interested.  Note that I leave the files in the directory, so you may need to occassionally clean up and delete them.  I could have added a __rm__ command, but I decided that it might be useful to have a copy.  Once they're copied in, they won't trigger the workflow again.


## Testing
With everything in place, all that's left is testing.  Run the script and then copy a file into that directory.  It should pop into the Kindle library of your choice in less than a minute.

Just like I did in the previous discussion, I recommend settting up the watcher script to start itself after reboot.

    crontab -e  # gets us into edit mode
    # add below entry
    @reboot /home/brent/watch-send2kindle.sh

## In closing
This really extends some of the recent things I've been doing in a very useful way.  For instance, I can Print from any file and choose "PDF" as the printer.  When prompted for a filename, directing that to ~/send2kindle/newfile.pdf will send it to my tablet.  It's not very complex to get setup and working and it "scratches an itch" I had.  Hope it is useful to you as well!