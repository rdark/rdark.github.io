---
layout: post
title:  "Ubuntu 10.04.3 LTS grub-install failure on software RAID"
date:   2012-03-25 01:00:00
categories: blog
---

Ubuntu 10.04.3 LTS has an apparent [bug][launchpad-bug] when running `grub-install` onto the MBR during the post-install phase. Neither grub2 nor grub legacy want to install. To get grub to install, you need to chroot into the installed environment (takes me back to my Gentoo days..) update, then retry installing legacy grub.
To summarise:

Press `CTRL-Alt-F2` to switch to PTS/2, then `[Enter]` to activate the console.

Mount virtual filesystems:

    mount --bind /proc /target/proc
    mount --bind /dev /target/dev</pre>

Chroot into the environment, update and install grub:

    chroot /target
    bash
    apt-get update
    apt-get -y dist-upgrade
    apt-get install grub
    CTRL-D
    CTRL-D

Now switch back to the ncurses installer on PTS/1 with `CTRL-Alt-F1` and re-run the grub install step, making sure to select legacy grub.

Apparently a fix has made it into 10.04.5..

[launchpad-bug]: https://bugs.launchpad.net/ubuntu/+source/grub2/+bug/527401
