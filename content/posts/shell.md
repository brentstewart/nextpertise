---
title: "Shell Setup"
description: ""
author: "Brent Stewart"
date: "2025-10-07T10:43:06-04:00"

math: false
draft: true
Victor_Hugo: "true"
picture: ""
Focus_Keyword: ""
youtube: ""
github: ""
refs: [""]
tags:
  - ""
---
A shell is a tool used to execute commands in an operating systems.  We most commonly think about shells as graphical (GUI) or command-line (text user interface or "TUI"), although I suppose you could imagine a spoken interface or mixed reality interface.  In the context of "Linux", it's used mostly to refer to the specific TUI environment in use.  Most Linux distributions ship with the Bourne shell ("sh") and BASH (Bourne again Shell).  Here I'm describing my preferred setup for that environment.

## Fish
I prefer [fish](https://fishshell.com/) (friendly interactive shell), which does a better job of suggesting syntax.  The commands below show installation in a Debian distrobution.  __chsh__ is "change shell" and __which__ provides the full path to the executable.

> sudo apt install fish
> chsh -s $(which fish)

## Starship
I also enjoy a snazzier presentation of the command line, similar to the Powerline style.  For that I use [Starship](https://starship.rs/).  Starship uses a font that has symbols built in to give a more graphical presentation, so you'll need to download a [nerd font](https://www.nerdfonts.com/font-downloads).  I'm currently using Overpass.

Install Starship:
> curl -sS https://starship.rs/install.sh | sh
and add a line to ~/.bashrc to start it with bash.  Why bother, since we switched to fish above?  Mostly a mental hangup, but there are times you get dropped back to a bash shell and I like it to be pretty.
> eval "$(starship init bash)"

Setup Starship for Fish by adding a line to ~/.config/fish/config.fish
> starship init fish | source

Finally, the default Starship scheme is pretty pedestrian.  It can be customized to your hearts content, but a good set of examples are available and easily selectable.  I use the catppuccin-powerline preset.
> starship preset catppuccin-powerline -o ~/.config/starship.toml

