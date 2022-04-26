---
title: "Webapps"
date: 2022-02-23T17:37:37-05:00
draft: false
author: "Brent Stewart"
Victor_Hugo: "true"
Focus_Keyword: "Webapp"
picture: "linux"
github: ""
youtube: ""
refs: ["https://github.com/nativefier/nativefier"]
tags: ["Linux"]
---

# Using Web Applications
I've found that the quality and capability of applications delivered over the web has been on-par with what is expected in a native application.  In my experience, Lucidchart was an early example of what was possible and today I actually prefer it to Visio.  I've also been very impressed by O365.  On Linux, I can use O365 to open an office file from OneDrive, edit and save it and get many (most?) of the features I expect in the office applications.  From a work compatibility mode, this really simplifies using a Linux machine.

These applications run in a tab in the browser.  Generally, I find this to be a good use of screen real estate.  However, this approach still ties up space on the browser button bar and status bar and adds a little cognitive load in starting the app and finding it on the screen.  Two applications that address these last issues are "Web App Manager" from the Linux Mint project and Nativefier.  Both wrap the application in a browser to make it look like a native program.

## Web App Manager
![Web](/Web_App_Manager.png#floatsmallright)

I really like Linux Mint.  I think there's a real clarity of vision to the project around making the power of Linux and open source accessible.  They develop utilities as part of that vision and Web App Manager is a great example.

Web App Manager makes it easy to manage and use Web Apps.  It can be [downloaded as a DEB](http://packages.linuxmint.com/pool/main/w/webapp-manager/) or installed from the Mint repository.  If you are running an Ubuntu flavor or derivative (I use Pop!) then this is shown below.

    sudo apt install ./Downloads/linuxmint-keyring*.deb
    sudo sh -c 'echo "deb https://packages.linuxmint.com ulyssa main" >> /etc/apt/sources.list.d/mint.list'

![New Web App](/Web_Apps_New.png#floatsmallleft)
Creating a new application is as as easy as entering in a name and address.  The program will let you select an icon or find one for you.  You can choose to retain the browser navigation bar - this ends up being just a new Firefox window, so why bother? - or to start in Private mode.  Web App Manaager will then create the entry in the desktop menu.

This ends up creating an application window, just as billed, that behaves like an application.  You can even register it as the default email client.  The big issue I have is that the resulting window doesn't have close, minimize, or maximize buttons.  This is a very solid option.

Setting up an Outlook application was as easy as specifying https://outlook.office365.com, selecting the icon, and putting it in a category for Gnome.  Web App Manager allows you to select the browser it runs in, but my system has Firefox, Ungoogled Chrome, and LibreWolf installed and it only offered Firefox.  My guess is it only looks for the major browsers.


## nodejs-nativefier

Setting up Nativifier is a little more complicated.  Nativifier uses  NPM to create an Electron app and this is the first issue with considering it.  This application requires a lot of  dependencies be downloaded for NPM and my security friends cast a sceptical eye toward nodejs.  That's not to say it's a no-go, just an item to consider.  Electron is something that folks like or don't.  It can produce an app using web technologies that can be easily versioned for different operating systems, but it doesn't carry a native look or feel.  Not a big deal for me, but your mileage may vary. 

Installing npm and then nativifier on Ubuntu derivitives looks like this.

    sudo apt install npm
    sudo apt install nativefier -g

Creating the web app is done from the command line.  The example below builds the same Outlook web app.  The options here specify the OS and  architecture, as well as setting the resulting electron to minimize to the system tray. 

    nativefier -p linux -a x64 https://outlook.office365.com  --tray
    
When I ran this it produced a directory called SignIntoOutlook containing an executable of the same name.  I renamed the directory and executable to _Outlook_ and gave the application execution permission.

    chmod +x Outlook

The resulting application is a window that has the web application inside - that part is the same in both apps or even in a browser.  A minor quibble is that it seems to start a little slower (Web App Manager is probably just faster because I already have a Firefox window open). However, this brings us to the next set of big issues for nativefier.

Nativefier builds an electron app but doesn't add it to the menu.  This is easily done in Cinnamon, which has menu editing built in.  You can download Menu Editor for Gnome to add it.  Since I run Pop-OS!, I created an _Outlook.desktop_ file in ~/.local/share/applications similar to the one shown below.

    [Desktop Entry]
    Version=1.1
    Type=Application
    Name=Outlook
    Comment=Outlook Web Electron app
    Icon=mail-send-receive
    Exec=/home/brent/Outlook/Outlook
    Actions=
    Categories=Office;  

## Closing thoughts
Both Web App Manager and Nativefier produce a working executable that to a good extent feels like a part of the desktop environment.  Since the actual functionality comes from the underlying software as a service, they both function in largely similar ways.

Web App Manager is definitely the choice if you have less experience.  It's easier to tweak, it automatically adds things to the menu, and it's only dependency is the browser. The only negative I have is that the app window doesn't have a close button, which sounds silly but is annoying in practice.  Alt-F4 or selecting it in the application dock allow you to close, FYI.

Nativefier actually puts an Electron wrap around, gives window controls, and has the option to minimize to the tray.  However, it required NPM, the only way to tweak the app is to rebuild it from the command line, and it didn't integrate with the application menu.  None of those are big deals if you are comfortable with the command line.