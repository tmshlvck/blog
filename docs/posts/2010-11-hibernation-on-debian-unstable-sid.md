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
time_to_read: 5
title: Hibernation on Debian unstable (sid) sucks badly
---

*This was originally posted on blogger [here](https://snarkybrill.blogspot.com/2010/11/hibernation-on-debian-unstable-sid.html)*.

As the header says... <strong>It sucks completely.</strong> Basically what you need to make hibernation working on pretty decent laptop like for instance ThinkPad X301 as in my case? First you need running Debian of course, <strong>SWAP partition</strong> of  <strong><em>sufficient size <span style="font-weight: normal;">(= greater than your RAM:-))</span></em></strong> and install packages <strong>hibernate</strong> and <strong>uswsusp</strong>. Then try it... For instance run <strong>s2disk</strong> or click on some button on your Gnome/KDE/... desktop. It should hibernate (some percentage growing, disk working and then it is off), hopefully.<br />
<br />
When you turn it on you may see message:<br />
<pre>﻿﻿﻿Invalidating stale software suspend images</pre>and then the systems boot from scratch like it has beeing rebooted... What the <strong>****</strong>? Well in my case the<br />
<br />
<pre>resume=swap:/dev/sda2</pre><br />
line was missing in <strong>/boot/grub/grub.cfg</strong>.<br />
<br />
Well, you can add it there by hard. (Why it is not there by default? Well I am not a Debian guy, so I do not even know where to fill the bug actually, but I am providing a hack. Stone me.) Just add something like this line:<br />
<br />
<pre>GRUB_CMDLINE_LINUX_DEFAULT="quiet resume=swap:/dev/sda2"</pre><br />
to file <strong>/etc/default/grub</strong>.<br />
<br />
That't it. It worked for me.