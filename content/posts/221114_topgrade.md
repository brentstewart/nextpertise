---
title: "Updated Ubuntu Upgrading"
description: ""
author: "Brent Stewart"
date: "2022-11-14T17:27:51-05:00"
math: false
draft: false
Victor_Hugo: "true"
picture: "linux"
Focus_Keyword: "Linux Upgrade"
youtube: ""
github: ""
refs: ["https://github.com/topgrade-rs/topgrade", "https://gitlab.com/volian/nala/-/wikis/Installation"]
tags:
  - "linux"
---
## OS Hopping update
I've confessed in this blog to being a distro hopper.  I love to see what is possible and to explore alternatives!  I haven't provided an update on where that journey has taken me, so let's start there.

I have a desktop, where the majority of work is done, and a laptop for traveling and returning emails from the couch.  

The desktop is running Pop! OS and I've considered changing that many times (most recently to Nix OS) but just haven't.  I've found Pop! really fits my workflow.  In particular, I think the Pop! implementation of tiling is the experience that suits me best.  I'm not crazy about the full-frontal keyboard experience of i3, and tiling in Gnome and KDE is "meh" in comparison.  I also like that Pop! builds on Ubuntu, the environment with which I'm most familiar.

Tiling on a big 4K display is just the obvious way to go, but it doesn't work as well on a smaller screen.  My laptop has been where I've done my current distro hopping.  It ran Nix-OS KDE for a long time and that was a mostly great experience.  The big miss there was software - I had issues getting my Brother printer driver to load and I wanted to update to KDE 5.26 and it isn't available yet.  I'll circle back to Nix at a later point because it sold me on the vision of reproducibility.  I briefly tried stock Ubuntu and got frustrated with Gnome extensions and . . . well, Gnome.  It's currently running Kubuntu.  I've added _backports_ and upgrade KDE to 5.26 as well.

All this sets the context that I've settled into Ubuntu family systems and developed some comfort there.  The package system is a core part of what differentiates distros and I'm pretty comfortable with _apt_.   Recently, two new programs have emerged to provide a better experience - _nala_ and _topgrade_.

## Apt
Package updates on an Ubuntu-derived system (Ubuntu, Kubuntu, Mint, Pop! and others) is done through _apt_.  Apt is pretty good, but isn't optimized for speed and really only covers packages distributed as debs.  Also, new package formats have emerged that build the application and all it's dependencies together (like flatpak, snap) and these aren't updated by apt.

To address part of this, I made a simple update script that includes flatpak and snap.  I just run "updateall.sh".

    sudo apt update
    sudo apt upgrade -y
    sudo snap refresh -y
    flatpak update -y

## Nala

[Nala](https://gitlab.com/volian/nala/-/wikis/Installation) is an apt replacement that has been generating some buzz recently.  It is able to pick the fastest mirrors, utilize multiple mirrors, and download in parrallel.  Installing nala via PPA is shown below.  You'll need to run _nala fetch_ to have it evaluate the latency to mirrors and pick the fastest.

    echo "deb [arch=amd64,arm64,armhf] http://deb.volian.org/volian/ scar main" | sudo tee /etc/apt/sources.list.d/volian-archive-scar-unstable.list
    wget -qO - https://deb.volian.org/volian/scar.key | sudo tee /etc/apt/trusted.gpg.d/volian-archive-scar-unstable.gpg > /dev/null
    sudo apt update
    sudo apt install nala

    nala fetch

Once nala is installed, it's used very similarly to apt.  A system upgrade is _nala update_ and _nala upgrade_.

In my testing, nala is slightly faster than apt and a little more visually appealing.  It didn't seem to result in a qualitatively better experience though and didn't address other packaging formats.  It's probably worth moving from apt to nala, but you're still going to need an _updateall.sh_ script!

![Topgrade](/topgrade.png#floatsmallright)

## Topgrade

[Topgrade](https://github.com/topgrade-rs/topgrade) is a rust application that detects the invoking OS and then steps through various appropriate update routines.  On my system, it not only recognized that Pop! is a deriviate of Debian but also caught that I had installed Nala and used it instead of apt!  I'm not a Rust programmer, but looking through the _linux.rs_ file gives a sense of the logic.

![Topgrade running](https://github.com/topgrade-rs/topgrade/raw/master/doc/screenshot.gif)

Topgrade uses every update mechanism it finds on your system.  On my PC, it used Nala, Brew, conda, pip, flatpak, snap, and fwupd.  It updated containers and Gnome shell extensions as well.  Wow!

Installing Topgrade on Ubuntu is done through cargo.  Conducting an upgrade simply involves running _topgrade_.

    cargo install topgrade

I haven't had any issues running this against Pop! OS.

## Do Both!
I started this article with an eye toward examing Nala against Topgrade (which I was already using).  My conclusion is that the combination is the best of both worlds.  Topgrade will incorporate Nala if installed.  Through Nala, it gains some speed over apt.  Topgrade also provides the upgrade mechanism for a variety of other sources.  