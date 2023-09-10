---
title: "Python on Pop"
description: "Pop! OS hasn't been updated in a while and python is getting long in the tooth."
author: "Brent Stewart"
date: "2023-09-09T21:58:42-04:00"
markup: 'mmark'
math: false
draft: false
Victor_Hugo: "true"
picture: "linux"
Focus_Keyword: "Python on Pop! OS"
youtube: ""
github: ""
refs: [""]
tags:
  - "linux"
---
In the course of building out a small program the other day, I realized that my version of Python was lagging behind.

    python -V
    Python 3.8.17

# My setup
I'm running Pop! OS 22.04, the latest version of Pop! for a while.  Pop! currently uses Gnome with add-ins to customize the interface and deliver the tiled experience that Pop! is known for.  Extensions have been a bugaboo for Gome for a long time (although recently - 2023 - they've gotten better, but I understand Gnome 45 breaks them _all_ again).  Word trickled out that System 76 had difficulties in working with the Gnome team as well.  In 2022, System 76 decided to pursue their own Rust-based desktop environment as a replacement for Gnome.  In order to free up resources, they decided to maintain 22.04 and to not build interim releases.

Despite the fact that I've been running the same release for a couple years, Pop! continues to be my favorite experience.  System 76 has pushed updates out and I've commented several times that the kernel is fresh and the components are kept current so Pop! still feels like a good place.  I've historically been a hopper and staying on one distro for a prolonged period of time is a change.  That said, finding out Python was several versions behind was a moment.

I'm very interested in Nix, as an aside.  I've got Nix installed on a laptop and loaded the System 76 tiling extension.  Nix has a lot going for it, but I'm still try to learn flakes and Home Manager and don't feel ready to put this on my main device.  The laptop is a place I can "burn and rebuild".  Still, if I start hopping again, it will be to go to Nix.

# Updating Python on Pop! OS 22.04

I used __python -V__ or __python --version__ to determine my current version.  To move to the latest Python, 3.10, I added the PPA for Python.

    sudo add-apt-repository ppa:deadsnakes/ppa
    sudo apt update
    sudo apt install python3.10

At this point, both 3.8 and 3.10 were installed on my system.  I used __sudo update-alternatives --config python3__ to select the version that I wanted to use by default.  Running the command will show you a list of python installs and allow you to select one.    Using __update-alternatives__ instead of deleting Python 3.8 maintains the old version for any dependencies. When I used it, I found I had a copy of _miniconda_ that I no longer needed and I ended up uninstalling it.

I also like to be able to type __python3__ or just __python__.  To set that up, I added an alias as well.

    alias python /usr/bin/python3.10

So now I'm up to date!

    python --version