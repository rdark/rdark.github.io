---
layout: post
title:  "Vim-Friendly Keybinding in X on Sun Keyboards"
date:   2011-04-24 01:00:00
categories: blog
---

If you're a half-decent sysadmin/coder/computer user, then the chances are that you spend half your life in vim (yes I know, a half-decent hacker may use vim, but only a fully-decent hacker would use emacs..). The escape key is a pretty important key that gets a lot of usage (as it's the key to exit insert mode and return to command mode, among other things). Yes, I know you can re-map this function to another key such as `CAPS-Lock` (if your keyboard has that key), or alternativly use `CTRL`+`]` to get an escape keycode; but there's something pretty satisfying about hitting that key - perhaps one of the reasons why PFU sell a big red escape key for the [HHKB2][hhkb2-elitekeyboards] for $4.50, and perhaps why I was suckered into actually buying it.

The second-favourite of my keyboards, after the [HHKB2][hhkb2-elitekeyboards], is the [Sun Type 6][sun-type6-img]. This keyboard has a giant help button on the top-left of the keyboard, right next the the escape key. It seems that it's only function is to annoy me by opening the Gnome help dialogue whenever I miss-hit escape. Luckily, X allows you to remap any key so I re-mapped this seemingly useless key to be another escape key:

To make the change system-wide, edit the file `/etc/X11/Xmodmap` (create a new one if it doesn't exist already).

    ! Bind SunHelp to escape key
    keysym Help = Escape

To make the change local to your own account only, place the above configuration in the dotfile: `~/.xmodmap`
Then either restart X, or type `xmodmap ~/.xmodmap` to load it immediately.

Useful testing commands:

* To get a full list of all available keys: `xmodmap -pk`
* To capture keyboard input using the X Event tester: `xev`

[hhkb2-elitekeyboards]: http://elitekeyboards.com/products.php?sub=pfu_keyboards,hhkbpro2
[sun-type6-img]: http://gem.win.co.nz/mb/misc/sun/ipx_pic/keyboard.jpeg
