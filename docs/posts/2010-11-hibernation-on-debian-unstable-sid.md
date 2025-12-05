---
date: 2010-11-17T02:37:00.000+01:00
description: ''
published: true
slug: 2010-11-hibernation-on-debian-unstable-sid
tags:
- Debian
- Linux
- Linux kernel
- legacy-blogger
readtime: 5
title: Hibernation on Debian unstable (sid) sucks badly
---

*This was originally posted on blogger [here](https://snarkybrill.blogspot.com/2010/11/hibernation-on-debian-unstable-sid.html)*.

As the header says... **It sucks completely.** Basically what you need to make hibernation working on pretty decent laptop like for instance ThinkPad X301 as in my case? First you need running Debian of course, SWAP partition of sufficient size (= greater than your RAM:-)) and install packages `hibernate` and `uswsusp`. Then try it... For instance run `s2disk` or click on some button on your Gnome/KDE/... desktop. It should hibernate (some percentage growing, disk working and then it is off), hopefully.

When you turn it on you may see message:

```
Invalidating stale software suspend images
```

and then the systems boot from scratch like it has beeing rebooted... What the **\*\*\*\***? Well in my case the

```
resume=swap:/dev/sda2
```

line was missing in `/boot/grub/grub.cfg`.

Well, you can add it there by hard. (Why it is not there by default? Well I am not a Debian guy, so I do not even know where to fill the bug actually, but I am providing a hack. Stone me.) Just add something like this line:

```
GRUB_CMDLINE_LINUX_DEFAULT="quiet resume=swap:/dev/sda2"
```

to file `/etc/default/grub`.

That't it. It worked for me.
