---
date: 2014-12-11T18:51:00.000+01:00
description: ''
published: true
slug: 2014-12-debians-driver-for-intel-gpus-is--
tags:
- Debian
- Linux
- legacy-blogger
time_to_read: 5
title: Debian's driver for Intel GPUs is s***
---

*This was originally posted on blogger [here](https://snarkybrill.blogspot.com/2014/12/debians-driver-for-intel-gpus-is-shit.html)*.

After a few years spent with Ubuntu and Linux Mint I decided to give Debian Jessie a try this week. And I have to admit that I also wanted to see systemd in action just to assess it myself and see whether it has some potential to do something good or otherwise.<br />
<br />
But the Debian Jessie with either Cinnamon or Mate desktop was unusable on my ThinkPad X301. The graphics was sluggish. Even video playback in VLC was apparently loosing one third of frames even in lower quality movies. The mouse wheel on external USB mouse was lagging. Keyboard has visible lag behind key press and the letter being inserted to terminal/text editor/whatever. The experience was really horrible. Wtf?<br />
<br />
Well, I suspected the Cinnamon desktop that I installed in the first place. I thought that it might eat up CPU and put too much strain on GPU. This assumption proved wrong. So the problem was apparently somwhere in XOrg. Sluggish video and lagging inputs worried me because how one problem could possibly cause that. Well, it is possible in the XOrg because one driver can slow down the whole thing and cause problems even in other subsystems.<br />
<br />
After a few hours tuning this and that I checked the version of the Intel driver and... Well, the driver is old deprecated ***. And it is present in all current Debian versions (stable... that's a joke:-) ), testing (which is freezed so it is going to be "stable", LOL), and unstable (at least the name suggest that it does not work properly, which is completely true).<br />
<br />
This blog post explains everything:&nbsp;<a href="http://blogs.fsfe.org/the_unconventional/2014/11/12/debian-x-drivers/">http://blogs.fsfe.org/the_unconventional/2014/11/12/debian-x-drivers/</a><br />
<br />
Just for reference: That's what I have.<br />
<br />
<pre>00:02.0 VGA compatible controller: Intel Corporation Mobile 4 Series Chipset Integrated Graphics Controller (rev 07) (prog-if 00 [VGA controller])
 Subsystem: Lenovo Device 20e4
 Flags: bus master, fast devsel, latency 0, IRQ 47
 Memory at f0000000 (64-bit, non-prefetchable) [size=4M]
 Memory at d0000000 (64-bit, prefetchable) [size=256M]
 I/O ports at 1800 [size=8]
 Expansion ROM at  [disabled]
 Capabilities: [90] MSI: Enable+ Count=1/1 Maskable- 64bit-
 Capabilities: [d0] Power Management version 3
 Kernel driver in use: i915
</pre>
<br />
And my solution was:
<br />
<pre>echo "deb http://ftp.debian.org/debian experimental main" &gt;&gt; /etc/apt/sources.list
apt-get update
apt-get -t experimental install xserver-xorg-video-intel
</pre>