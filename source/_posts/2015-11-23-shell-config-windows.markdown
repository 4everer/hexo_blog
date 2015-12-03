---
layout: post
title: "Shell Config -- Part One: Windows"
modified:
categories: resources
description: console customization on Win
tags: [shell config]
image:
  feature:
  credit:
  creditlink:
comments: true
share: true
---

Shell is so powerful and quick. One could accomplish many tasks with pipeline thanks to the philosophy of \*nux system. Customizing shell environment will for sure increase efficiency, and maybe even make you happier while you are working.

This will be a series of articles about configuration on Win, mac and Linux platform. The goal is to have similar interface of the shell and similar good and informative experience. 

Part One: Windows
====

## Choose a console emulator
I recommend *[cmder](http://cmder.net/)* for win platform, which can be used not only for **cmd** and **powershell**, but also can be used for installed linux shell like minGW, CygWin, zsh or git bash.


### cmd and Powershell
It is said that **cmd** was not a proper shell at all, while **Powershell** looks like product from the future generation powerful (talking about calling office api and vba) but not really popular. 

Anyway, if just for cmd or powershell, it is fairly easy to quickly start with *cmder* and customize color, font and keys.

___

## use bash/zsh/minGW/CygWin
Another advantages for *cmder* is that it is super easy to use other shell environment in the emulator. In fact, it will auto collect all the installed shell and add to available option.

Among those shells, I chose zsh for customizations. Additionally, it is good to use the same shell and config on different platforms. 

___

## zsh
Asking google about zsh on Windows, [Babun](http://babun.github.io/) quickly stands out. 

### Battery included

Based on CygWin, it comes with useful bash command and python as well as a simple package distribute system `pact`. And of course, it is compatible with [oh-my-zsh](https://github.com/robbyrussell/oh-my-zsh).

### configuration for cmder

The configuration can be manually added to cmder as follows:
   
1. Go to Settings>Startup>Tasks ==> Create a new task
2. Task parameters: `/icon "%userprofile%\.babun\cygwin\bin\mintty.exe" /dir "%userprofile%"`
    
    Commands: `%userprofile%\.babun\cygwin\bin\mintty.exe`
3. Babun is available in the "Create new console" menu.

### Choose theme
edit .zshrc file: `vim ~/.zshrc`

then `ZSH_THEME="agnoster"`

### configure powerline font
get powerline compatible font from [git repo](https://github.com/powerline/fonts), install font as usually by opening .ttf file and clicking install

then choose the installed font in menu of babun shell to display proper powerline symbol.

### other config
- git config
- ruby: `pact install ruby`; use with .bat, e.g. `gem.bat install bundle`

   

