---
title: "Nautilus Tweaks"
date: 2022-07-17T17:04:02-04:00
draft: false
author: "Brent Stewart"
Victor_Hugo: "true"
Focus_Keyword: "Nautilus"
picture: "linux"
github: ""
youtube: ""
refs: [""]
tags: ["linux"]
---
Nautilus, the Gnome file manager, has a number of available plug-ins available that make it much easier to use.  It also supports a scripting function that you can use to develop your own extensions.  The extentions can be easily added using _apt_, but many of them will not be active until a reboot.  You can short-circuit that process by using __nautilus -q__ to quit all open instances of nautilus and then opening a new window.  I'm using all of the following extensions with _Pop! OS 22.04_, so I can verify they work in COSMIC. 

![Nautlus Image Resize](/220717_gnomeimageresize.png#floatsmallright)
## Easily Downsize an Image
The first plug-in, and the one I use the most, is the image converter.  I maintain a simple internal web page with the sites I use the most and I like to grab an image to use as a site icon.  There are a lot of ways to get a good icon - some sites allow you to right click and save as.  Some icons are available on [Google Images](https://images.google.com).  I also sometimes just use Flameshot to grab a screen portion.  The problem with all these methods is that they don't create a standard image size.  I can dynamically resize, but that slows down my page display.  I can also use GIMP to resize, but that's an involved process.

This is a problem that pops up fairly often.  There's never the exact right icon in Lucidchart, so I want to pull one in.  I create a new GNS3 device and want to put a cool image on it.  In all these cases, I want a standard size.  The solution is the Nautilus Image Converter.  This can be installed from the command line as shown.

    sudo apt install nautilus-image-converter

Right click an image and you'll be offered two new menu options - rotate and resize.  This let's you easily scale the image or set it to a desired size (96x96 works well for a lot of icons, I use 72x72 for GNS3 icons per their style guide).  You can change the current file or save it as a new file.

## Preview a File
Gnome Sushi is a way to quickly preview a file.  Sushi works with most text, music, image, and video formats.  Install this using apt:

    sudo apt install gnome-sushi

Once installed, choose a file and hit [space] to open a preview.  Arrow keys can be used to move through the Nautilus directory with the preview window updating to show each file.  [Esc] can be used to close the preview.

## Admin!
Windows has a cool right-click option to "open as Administrator".  This plug-in is similar - the context window will show "Edit as Administrator".  Choosing it will open a prompt to escalate priviledges (side note, this works really well with [Howdy](/posts/220501_howdy/).  This extension is trying to open whatever file as a text file, so it doesn't work with images (for instance).  Install this as shown below.

    sudo apt install nautilus-admin

![Nautlus Image Resize](/220717_foldercolors.png#floatsmallright)
## Folder Colors

Folder colors is an arguably less useful add-in.  This allows particular folders be be changed from the default color.  My experience is that this is most useful when applied sparingly.  I use this to highlight important folders that I want to be able to easily zoon in on in a list.  Install this plug in as shown below.

    sudo apt install folder-color

Once installed and Nautilus is restarted, there should be a "Folder's Color" option in the context menu.

![Hashing](/220717_hash.png#floatsmallright)
## Hash
Hashing is a mathematical technique for testifying that a file has not changed since it's originator created it.  A hash takes a set of numbers and converts them to a specific length code, or "hash".  It's important that the length is consistent - we don't want that to provide a clue to the length of the file!  The simplest hash would be just to add up all the binary ones in a file and return "0" if there are an even number and "1" to indicate odd.  Obviously, that's not a real hash, but it starts to convey the idea.  Modification to the file might change the hash and alert users.  A clever miscreant might recognize the problem and change the file in a way that keeps the same hash and file size, so we need more complicated math to make this more difficult.

If you download a file from the Internet, the originator may publish a hash that you can use to verify your download.  Typically you'd do this from the command line using an implementation of the hashing algorithym (examples follow).

    md5sum myfile
    sha1sum myfile
    sha256sum myfile

You can install a Nautilus add-in, as shown below, to do this as well.

    sudo apt install nautilus-gtkhash

Go to "properties" in the context menu and you'll see a window similar to the example.  Go to the Digest tab (1) and select the hashes you'd like to computer (2).  Press the Hash button(3) and it will compute your hashes.  Is this easier than the command line?  Probably not, but if you're in a graphical workflow sometimes this is easier.


## New Document
The last tip is around creating new blank documents.  Windows allows you to create a new text file, for instance, so this feature allows you to do the same in Gnome.  There's nothing to add-in.  Gnome has a templates folder in your home directory (~/Templates).  Any file you place here will be available under New Document.  To play with this, just create a document in that directory.

    touch ~/Templates/"New Document"

Next, in any directory, right click and you should see a "New Document" option with an indicator that you can create a variety of documents based on templates.  You could, for instance, create a markdown file with Hugo header information already defaulted and name it "New Markdown".  You could take a spreadsheet formatted for accounting and call it "New Expense Report".  That said, this is the feature I use the least.

