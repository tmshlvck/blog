---
date: 2010-12-12T12:12:00.000+01:00
description: ''
published: true
slug: 2010-12-zfs-pools-surveillance-using-nagios-and
tags:
- FreeBSD
- ZFS
- legacy-blogger
readtime: 5
title: ZFS pools surveillance using Nagios and NRPE
---

*This was originally posted on blogger [here](https://snarkybrill.blogspot.com/2010/12/zfs-pools-surveillance-using-nagios-and.html)*.

Just short post: I have found this [useful and almost out-of-box working HOWTO](http://s23.org/wiki/Nagios/checks/solaris_zpools). Thanks.

(One small issue was that path to zpool binary on FreeBSD is `/sbin/zpool`, not `/usr/sbin/zpool`. Trivial change.)
