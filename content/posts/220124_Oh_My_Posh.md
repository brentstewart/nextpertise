---
title: "Oh My Posh"
date: 2022-01-25T19:05:41-05:00
draft: false
author: "Brent Stewart"
Victor_Hugo: "true"
Focus_Keyword: "Oh My Posh"
picture: "shell"
github: ""
youtube: ""
refs: ["https://ohmyposh.dev/docs/","https://chrisant996.github.io/clink/","https://www.nerdfonts.com/"]
tags: ["Shell","Windows","Linux"]
---
I really like the powerline-style prompts that jazz up the command line and I'd like to be able to carry that experience through from Linux to Windows.  It seems like everytime I install a new systema and think about this, I find another slightly different way to do something similar.  Recently I found _Oh My Posh_, which is designed to support Windows, Linux, and MacOS.  The attraction here is that this gives me the prompt style I like from a consolidated source and with a defined way to set it up.

Here's an example of OMP installed in Linux running inside Tilix.  You can see that it's providing me collapsed directory information, git info, and the time of the last command.  OMP can be customized and the details of that are described extensively in the online docs.
![Oh My Posh in Tilix on Linux](/omp_tilix.png#center)
This has some marginal productivity - the Agnoster theme condenses directory structure in a very visible way and helps me understand the state of Git.  Regardless, it looks cool and a little terminal rice establishes some credibility.  If it looks cool to you too, I've put together some notes on how it's done.  Follow along!


## Linux
Installation on Linux involves grabbing the file from Github and making it executable.

    sudo wget https://github.com/JanDeDobbeleer/oh-my-posh/releases/latest/download/posh-linux-amd64 -O /usr/local/bin/oh-my-posh
    sudo chmod +x /usr/local/bin/oh-my-posh

Next, grab the themes JSON collection, uncompress them, and set the permissions appropriately.  With the themes locally stored, you can easily switch as the mode strikes.  Agnoster fits my needs, so that's what is used in the examples, but you can substitute anywhere you see it mentioned.

    mkdir ~/.poshthemes
    wget https://github.com/JanDeDobbeleer/oh-my-posh/releases/latest/download/themes.zip -O ~/.poshthemes/themes.zip
    unzip ~/.poshthemes/themes.zip -d ~/.poshthemes
    chmod u+rw ~/.poshthemes/*.json
    rm ~/.poshthemes/themes.zip

Finally, edit ~/.bashrc to run Oh My Posh as part of the shell.  Notice in the code below that I've specified the "agnosterplus" layout.  Substitute whichever theme you are interested in there.

    eval "$(oh-my-posh --init --shell bash --config ~/poshthemes/agnosterplus.omp.json)"

You'll need to specify the terminal font in your terminal application.  A lot of the styling is done through extra ligatures included in [nerd fonts](https://www.nerdfonts.com/) - font files that are re-compiled to include additional symbols.  Grab a font you like (I'm using Meslo in this example, but I'm also partial to JetBrains NF) and set it as the default in the terminal profile.

This setup is used by bash, regardless of the terminal application.  I have Tabby, Tilix, and the included Terminal from Pop! and all three "just work".  As mentioned, the only cavaet is getting a good font setup in the terminal profile.

## Windows PowerShell
As mentioned, Oh My Posh works swimmingly with PowerShell on Windows.  I have it working in the Powershell terminal and in the Windows Terminal (but recommend the later).

![Om My Posh in Windows Terminal](/omp_windows.png)

OMP can be installed on Windows using PowerShell, [Chocolatey](/posts/220118_Choco/), [Winget](/posts/211228_Winget/), or scoop.  I prefer Choco, so that's what is used in the examples below.

    choco install oh-my-posh

Then edit the profile.  You may get an error because there's not an existing profile.  If so, just create one.  Type __$profile__ in PowerShell to see what the filename and location should be.

    notepad $profile

add the following into the profile:

    Import-Module oh-my-posh
    oh-my-posh --init --shell pwsh --config ~/agnosterplus.omp.json | Invoke-Expression

Again, you'll need to use the [nerd font](https://www.nerdfonts.com/) of your choice.  Set this up in the PowerShell Terminal or Microsoft Terminal.  Both apps use the same $profile, so you just need to change the font in the terminal.

## Windows CMD

OMP is even available for the traditional command line.  For cmd, install [clink](https://chrisant996.github.io/clink/).  Clink adds some of the editing features of Bash to the traditional CMD.  Download clink and run the installer.  You can verify the installer by running __clink info__.

    clink info
    version  : 1.3.2.222baa
    session  : 8504
    binaries : C:\Program Files (x86)\clink\1.3.2.222baa
    state    : C:\Users\Brent\AppData\Local\clink
        < output trimmed>

Next create a file called oh-my-posh.lua in your clink directory.  Note that this directory is given from __clink info__.  The load string below starts OMP - note the theme is specified as well.  This section of text can be replaced if you disagree with me on the theme to be used. 

    notepad AppData\Local\clink\oh-my-posh.lua
        <add this text>
    load(io.popen('oh-my-posh --config="C:/Users/Brent/.oh-my-posh/themes/agnosterplus.omp.json" --init --shell cmd'):read("*a"))()

