---
date: 2010-12-12T12:12:00.000+01:00
description: ''
published: true
slug: 2010-12-zfs-pools-surveillance-using-nagios-and
tags:
- FreeBSD
- ZFS
- legacy-blogger
time_to_read: 5
title: ZFS pools surveillance using Nagios and NRPE
---

*This was originally posted on blogger [here](https://snarkybrill.blogspot.com/2010/12/zfs-pools-surveillance-using-nagios-and.html)*.

Just short post: I have found this <a href="http://s23.org/wiki/Nagios/checks/solaris_zpools" target="_self" title="agios/checks/solaris zpools">useful and almost out-of-box working HOWTO</a>. Thanks.<br />
<br />
(One small issue was that path to zpool binary on FreeBSD is <strong>/sbin/zpool</strong>, not <strong>/usr/sbin/zpool</strong>. Trivial change.)